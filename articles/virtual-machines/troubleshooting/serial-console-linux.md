---
title: Seriële Console van virtuele Azure-Machine | Microsoft Docs
description: Bidirectionele seriële console voor virtuele machines van Azure.
services: virtual-machines-linux
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/11/2018
ms.author: harijay
ms.openlocfilehash: 57abb01d70929144a8457a04ebc0caf9aecaa61c
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212400"
---
# <a name="virtual-machine-serial-console"></a>Seriële Console van virtuele Machine


De seriële Console van de virtuele Machine op Azure biedt toegang tot een op tekst gebaseerde console voor virtuele Linux-machines. Deze seriële verbinding is met de seriële poort COM1 van de virtuele machine, toegang tot de virtuele machine die onafhankelijk is van het netwerk of de staat van besturingssysteem van een virtuele machine. Toegang tot de seriële console voor een virtuele machine op dit moment kunnen alleen worden uitgevoerd via Azure portal en alleen voor gebruikers die VM-Inzender hebben of hoger toegang tot de virtuele machine is toegestaan. 

Voor de seriële console-documentatie voor Windows-VM's, [Klik hier](../windows/serial-console.md).

> [!Note] 
> Seriële Console voor virtuele machines is algemeen beschikbaar in de globale Azure-regio's. Op dit moment is seriële Console nog niet beschikbaar in Azure Government or Azure China-clouds.


## <a name="prerequisites"></a>Vereisten 

* U moet de resource Manager-implementatiemodel gebruiken. Klassieke implementaties worden niet ondersteund. 
* Uw virtuele machine moet hebben [diagnostische gegevens over opstarten](boot-diagnostics.md) ingeschakeld - Zie de onderstaande schermafbeelding.

    ![](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* Het Azure-account met behulp van de seriële console moet hebben [rol van Inzender](../../role-based-access-control/built-in-roles.md) voor de virtuele machine en de [diagnostische gegevens over opstarten](boot-diagnostics.md) storage-account. 
* De virtuele machine waarvoor u de seriële console acessing zijn moet ook een account op basis van wachtwoorden hebben. U kunt maken met de [wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) functionaliteit van de toegang tot VM-extensie - Zie de onderstaande schermafbeelding.

    ![](./media/virtual-machines-serial-console/virtual-machine-serial-console-reset-password.png)

* Zie voor specifieke instellingen voor Linux-distributies, [seriële Console-Linux-distributie beschikbaarheid](#serial-console-linux-distro-availability)



## <a name="get-started-with-serial-console"></a>Aan de slag met de seriële Console
Seriële console voor virtuele machines is alleen toegankelijk via [Azure-portal](https://portal.azure.com). Zorg ervoor dat u hebt voldaan aan de [vereisten](#prerequisites) hierboven. Hieronder volgen de stappen voor toegang tot de seriële console voor virtuele machines via de portal:

  1. De Azure-portal openen
  1. (Deze stap overslaan als de virtuele machine heeft een gebruiker die gebruikmaakt van wachtwoordverificatie) Toevoegen van een gebruiker met verificatie voor gebruikersnaam en wachtwoord door te klikken op de blade 'Wachtwoord opnieuw instellen'
  1. Selecteer de virtuele machines in het menu links.
  1. Klik op de virtuele machine in de lijst. De overzichtspagina voor de virtuele machine wordt geopend.
  1. Schuif omlaag naar de ondersteuning en probleemoplossing sectie en klik op de optie 'Seriële console'. Een nieuw deelvenster met de seriële console opent en start de verbinding.

![](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)

### 

> [!NOTE] 
> Seriële console vereist een lokale gebruiker met een wachtwoord dat is geconfigureerd. Op dit moment zich VM's alleen worden geconfigureerd met een openbare SSH-sleutel niet aanmelden bij de seriële console. Gebruik voor het maken van een lokale gebruiker met wachtwoord de [VM-extensie voor toegang](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension), beschikbaar in de portal door te klikken op 'Wachtwoord opnieuw instellen' in de portal en maak een lokale gebruiker met een wachtwoord.
> U kunt het administrator-wachtwoord ook opnieuw instellen in uw account door [WORMGATEN gebruiken om te verwijderen in de modus voor één gebruiker](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distro-availability"></a>Seriële Console Linux-distributie-beschikbaarheid
In de volgorde voor de seriële console te laten functioneren, moet het gastbesturingssysteem worden geconfigureerd om te lezen en schrijven van consoleberichten naar de seriële poort. De meeste [Azure Linux-distributies die zijn goedgekeurd](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) hebben van de seriële console standaard geconfigureerd. Eenvoudig te klikken op de seriële Console-sectie in Azure portal biedt toegang tot de console. 

Distributie      | Toegang tot de seriële Console
:-----------|:---------------------
Red Hat Enterprise Linux    | Red Hat Enterprise Linux-installatiekopieën die beschikbaar op Azure hebt toegang tot de console standaard ingeschakeld. 
CentOS      | CentOS-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld. 
Ubuntu      | Ubuntu-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld.
CoreOS      | CoreOS-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld.
SUSE        | Nieuwere SLES-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld. Als u oudere versies (10 of lager) van SLES op Azure gebruikt, volgt u de [KB-artikel](https://www.novell.com/support/kb/doc.php?id=3456486) seriële console inschakelen. 
Oracle Linux        | Oracle Linux-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld.
Aangepaste Linux-installatiekopieën     | Om in te schakelen voor uw aangepaste Linux-VM-installatiekopie de seriële console, schakel toegang tot de console in `/etc/inittab` om uit te voeren van een terminal op `ttyS0`. Hier volgt een voorbeeld om toe te voegen deze in het bestand inittab: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Zie voor meer informatie over het maken van aangepaste installatiekopieën correct [een Linux-VHD in Azure maken en uploaden](https://aka.ms/createuploadvhd). Als u een aangepaste kernel bouwt, sommige kernel vlaggen Overweeg in te schakelen worden `CONFIG_SERIAL_8250=y` en `CONFIG_MAGIC_SYSRQ_SERIAL=y`. Het configuratiebestand dat bevindt vaak zich onder /boot/ voor verder onderzoek.

## <a name="common-scenarios-for-accessing-serial-console"></a>Algemene scenario's voor toegang tot de seriële console 
Scenario          | Acties in de seriële console                
:------------------|:-----------------------------------------
Verbroken FSTAB-bestand | `Enter` de sleutel te gaan en op te lossen van fstab-bestand met een teksteditor. U moet mogelijk worden in de modus voor één gebruiker voor deze. Zie [voor het oplossen van problemen met fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) en [met behulp van de seriële Console van toegang tot WORMGATEN en de modus voor één gebruiker](serial-console-grub-single-user-mode.md) aan de slag.
Onjuiste firewall-regels | Toegang tot de seriële console en op te lossen iptables. 
Bestandssysteem beschadigd/selectievakje | Toegang tot de seriële console van maken en herstellen van bestandssysteem. 
Problemen met SSH/RDP-configuratie | Toegang tot de seriële console en instellingen wijzigen. 
Systeem voor het vergrendelen van netwerk| Toegang tot de seriële console via de portal voor het beheren van systeem. 
Interactie met de bootloader | Toegang WORMGATEN via de seriële console. Ga naar [met behulp van de seriële Console van toegang tot WORMGATEN en de modus voor één gebruiker](serial-console-grub-single-user-mode.md) aan de slag. 

## <a name="disable-serial-console"></a>Seriële Console uitschakelen
Standaard hebben alle abonnementen seriële console-toegang ingeschakeld voor alle virtuele machines. U kunt de seriële console van het abonnement of de VM-niveau uitschakelen.

> [!Note] 
> Als u wilt in- of uitschakelen voor een abonnement op de seriële console, moet u hebt schrijfmachtigingen voor het abonnement. Dit omvat, maar is niet beperkt tot de beheerder of eigenaar van rollen. Aangepaste rollen kunnen ook schrijfmachtigingen hebben.

### <a name="subscription-level-disable"></a>Abonnementsniveau uitschakelen
Seriële console kan worden uitgeschakeld voor een volledige abonnement door via de [uitschakelen Console REST API-aanroep](https://aka.ms/disableserialconsoleapi). U kunt het 'Try It' functionaliteit die beschikbaar is op de pagina van de API-documentatie uitschakelen en inschakelen van de seriële Console voor een abonnement. Voer uw `subscriptionId`, 'standaard' in de `default` veld en klik op uitvoeren. Azure CLI-opdrachten zijn nog niet beschikbaar en wordt bezorgd moeten worden op een later tijdstip. [Probeer de REST-API-aanroep hier](https://aka.ms/disableserialconsoleapi).

![](./media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

U kunt ook de reeks opdrachten hieronder gebruiken in Cloud Shell (bash-opdrachten die worden weergegeven) als wilt uitschakelen, inschakelen en weergeven van de met uitgeschakelde status van de seriële console voor een abonnement. 

* De uitgeschakelde status van de seriële console voor een abonnement ophalen:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* Om uit te schakelen voor een abonnement op de seriële console:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Om in te schakelen voor een abonnement op de seriële console:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/enableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```

### <a name="vm-level-disable"></a>VM-niveau uitschakelen
Seriële console kan worden uitgeschakeld voor specifieke virtuele machines door het uitschakelen van deze virtuele machine opstarten diagnostische instelling. Gewoon opstarten diagnostische gegevens van de Azure portal en de seriële console zal worden uitgeschakeld voor de virtuele machine uitschakelen.

## <a name="serial-console-security"></a>Seriële Console-beveiliging 

### <a name="access-security"></a>Beveiliging voor toegang 
Toegang tot de seriële console is beperkt tot gebruikers die hebben [VM inzenders](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) of hoger dan toegang tot de virtuele machine. Als uw AAD-tenant meervoudige verificatie vereist en toegang tot de seriële console ook de MFA moet omdat de toegang [Azure-portal](https://portal.azure.com).

### <a name="channel-security"></a>Beveiliging van het kanaal
Alle gegevens die worden verzonden heen en weer worden versleuteld op de kabel.

### <a name="audit-logs"></a>Controlelogboeken
Alle toegang tot de seriële console momenteel is aangemeld de [diagnostische gegevens over opstarten](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) logboeken van de virtuele machine. Toegang tot deze logboeken zijn eigendom van en beheerd door de beheerder van de virtuele machine van Azure.  

>[!CAUTION] 
Terwijl er geen wachtwoorden voor toegang voor de console bent aangemeld als opdrachten worden uitgevoerd binnen de console bevat of uitvoer van wachtwoorden, geheimen, gebruikersnamen of enige andere vorm van persoonlijk identificeerbare informatie (PII), worden die geschreven naar de diagnostische gegevens over opstarten van virtuele machine registreert, samen met alle andere zichtbare tekst, als onderdeel van de implementatie van de seriële console scrollback functionaliteit. Deze logboeken zijn circulaire en alleen personen met leesmachtigingen voor het opslagaccount voor diagnostische gegevens hebben de toegang tot, maar we raden aan na de aanbevolen procedure van het gebruik van de SSH-console voor alles wat die hebben mogelijk betrekking op geheimen en/of PII. 

### <a name="concurrent-usage"></a>Gelijktijdig gebruik
Als een gebruiker is verbonden met de seriële console en een andere gebruiker is toegang tot deze virtuele machine met dezelfde aanvraagt, wordt de eerste gebruiker verbroken en wordt de tweede gebruiker op een manier die lijkt op de eerste gebruiker permanent van en het verlaten van de fysieke-console en een nieuwe verbinding de gebruiker zich omlaag.

>[!CAUTION] 
Dit betekent dat de gebruiker die wordt verbroken wordt niet afgemeld. De mogelijkheid om af te dwingen een afmelden bij het verbreken van de verbinding (via SIGHUP of een vergelijkbaar mechanisme) is nog steeds in de roadmap. Voor Windows er een automatische time-out in SAC ingeschakeld is, maar voor Linux kunt u configureren terminal time-outinstelling. Doet dit gewoon toevoegen `export TMOUT=600` in uw .bash_profile of .profile voor de gebruiker die u in de console, time-out voor de sessie 10 minuten na de aanmelding.

## <a name="accessibility"></a>Toegankelijkheid
Toegankelijkheid is een belangrijke focus voor de seriële console van Azure. Wat dat betreft, hebben we ervoor gezorgd dat dat de seriële console is toegankelijk voor mensen met het visuele element en gehoorproblemen, evenals de mensen die misschien niet mogelijk een muis te gebruiken.

### <a name="keyboard-navigation"></a>Toetsenbordnavigatie
Gebruik de `tab` sleutel op het toetsenbord om te navigeren in de interface van de seriële console binnen de Azure-portal. Uw locatie wordt gemarkeerd op het scherm worden weergegeven. Als u wilt de focus van de seriële console-blade laten, drukt u op `Ctrl + F6` op het toetsenbord.

### <a name="use-serial-console-with-a-screen-reader"></a>Seriële console gebruiken met een schermlezer
Seriële console wordt geleverd met ingebouwde ondersteuning voor schermlezers. Navigeren om met een schermlezer ingeschakeld, kunnen de alt-tekst voor de geselecteerde knop om te worden door de schermlezer voorgelezen.

## <a name="errors"></a>Fouten
De meeste fouten van tijdelijke aard zijn en deze opnieuw wordt geprobeerd de verbinding van de seriële console van vaak adressen. De onderstaande tabel geeft een lijst met fouten en oplossingen

Fout                            |   Oplossing 
:---------------------------------|:--------------------------------------------|
Kan niet worden opgehaald van de instellingen voor diagnostische gegevens over opstarten voor '<VMNAME>'. Voor het gebruik van de seriële console of dat de diagnostische gegevens over die opstarten is ingeschakeld voor deze virtuele machine. | Zorg ervoor dat de virtuele machine heeft [diagnostische gegevens over opstarten](boot-diagnostics.md) ingeschakeld. 
De virtuele machine is gestopt toewijzing ongedaan gemaakt. Start de virtuele machine en probeer de seriële console-verbinding. | Virtuele machine moet zich in een status gestart voor toegang tot de seriële console
U hebt niet de vereiste machtigingen voor deze virtuele machine de seriële console gebruiken. Zorg ervoor dat u ten minste beschikken over machtigingen van de rol Inzender voor virtuele machine.| Toegang tot de seriële console moet bepaalde machtiging voor toegang tot. Zie [vereisten](#prerequisites) voor meer informatie
Kan niet bepalen van de resourcegroep voor het opslagaccount van de diagnostische gegevens over opstarten '<STORAGEACCOUNTNAME>'. Controleer of u diagnostische gegevens over opstarten is ingeschakeld voor deze virtuele machine en u toegang hebt tot dit opslagaccount wordt gebruikt. | Toegang tot de seriële console moet bepaalde machtiging voor toegang tot. Zie [vereisten](#prerequisites) voor meer informatie
Web socket is gesloten of kan niet worden geopend. | U moet mogelijk aan lijst met geaccepteerde `*.console.azure.com`. Een meer gedetailleerde maar langer aanpak is het whitelist de [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/en-us/download/details.aspx?id=41653), waardoor vrij regelmatig worden gewijzigd.
Een antwoord 'Verboden' is opgetreden bij het openen van deze virtuele machine opstarten diagnostische storage-account. | Zorg ervoor dat diagnostische gegevens over de opstarten beschikt niet over een account-firewall. Een toegankelijke opstarten diagnostische storage-account is nodig voor de seriële console van functie.

## <a name="known-issues"></a>Bekende problemen 
We zijn ons bewust van enkele problemen met de seriële console. Hier volgt een lijst van deze problemen beschreven en stappen voor risicobeperking.

Probleem                           |   Oplossing 
:---------------------------------|:--------------------------------------------|
Nadat de banner van de verbinding wordt niet weergegeven voor een logboek in de prompt te maken met invoeren | Raadpleeg deze pagina: [Hitting invoeren, gebeurt er niets](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Dit kan gebeuren als u een aangepaste VM, beperkte toestel of GRUB-configuratie die ervoor zorgt Linux dat mislukken correct verbinding maken met de seriële poort.
Tekst van de seriële console wordt alleen een gedeelte van de schermgrootte (vaak na met een teksteditor) | Seriële consoles bieden geen ondersteuning voor het onderhandelen over venstergrootte ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)), wat betekent dat er geen signaal SIGWINCH worden verzonden naar het bijwerken van de grootte van het scherm en de virtuele machine heeft geen kennis van de grootte van de terminal. U wordt aangeraden instaling xterm of enige andere vergelijkbaar hulpprogramma waarmee u de opdracht 'grootte'. Uitvoeren van 'grootte' wordt dit opgelost.
Het plakken van zeer lange tekenreeksen werkt niet | Seriële console beperkt de lengte van tekenreeksen in de terminal naar 2048 tekens geplakt. Zo wordt voorkomen dat de bandbreedte van de seriële poort wordt belast.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen 
**Q. Hoe kan ik feedback verzenden?**

A. Feedback geven als het probleem door te gaan naar https://aka.ms/serialconsolefeedback. U kunt ook feedback via (minder bij voorkeur), verzenden azserialhelp@microsoft.com of in de categorie van de virtuele machine van de http://feedback.azure.com

**Q. Biedt ondersteuning voor de seriële console kopiëren/plakken?**

A. Ja doet. Gebruik Ctrl + Shift + C en Ctrl + Shift + V om te kopiëren en plakken in de terminal.

**Q. Kan ik de seriële console in plaats van een SSH-verbinding gebruiken?**

A. Terwijl dit technisch mogelijk lijkt, is de seriële console moet worden gebruikt als een hulpprogramma voor probleemoplossing in situaties waar connectiviteit via SSH niet mogelijk is voornamelijk bedoeld. We raden het af met de seriële console als een SSH-vervanging voor twee redenen:

1. Seriële console hoeft niet zo veel bandbreedte in zoals SSH - dit is een verbinding alleen tekst, zodat meer GUI-zware interacties lastig om in de seriële console is.
1. Toegang tot de seriële console is momenteel alleen door de gebruikersnaam en wachtwoord. SSH-sleutels zijn veel veiliger dan combinaties van gebruikersnaam en wachtwoord, dus vanuit beveiligingsoogpunt login SSH via de seriële console beste.

**Q. Wie kunt inschakelen of uitschakelen van de seriële console voor mijn abonnement?**

A. Als u wilt in- of uitschakelen van de seriële console op het niveau van een brede, door het abonnement, moet u hebt schrijfmachtigingen voor het abonnement. Rollen die gemachtigd te schrijven bevatten, maar niet beperkt zijn tot de beheerder of eigenaar van rollen. Aangepaste rollen kunnen ook schrijfmachtigingen hebben.

**Q. Wie toegang heeft tot de seriële console voor mijn VM?**

A. Hebt u toegang van Inzender-niveau of hoger met een virtuele machine voor toegang tot de seriële console van de virtuele machine. 

**Q. Mijn seriële console niet wordt weergegeven van alles zijn, wat moet ik doen?**

A. Uw installatiekopie is waarschijnlijk niet goed is geconfigureerd voor toegang tot de seriële console. Zie [seriële Console-Linux-distributie beschikbaarheid](#serial-console-linux-distro-availability) voor meer informatie over het configureren van de afbeelding om in te schakelen van de seriële console.

**Q. Seriële console beschikbaar is voor Virtual Machine Scale Sets?**

A. Toegang tot de seriële console voor schaalsetinstanties virtuele machine wordt niet ondersteund op dit moment.

**Q. Stel ik mijn virtuele machine met behulp van alleen het sleutelverificatie SSH, kan ik daar nog steeds de seriële console verbinding met mijn virtuele machine te gebruiken?** A. Ja. Seriële console vereist niet de SSH-sleutels, dus hoeft u een combinatie van gebruikersnaam en wachtwoord is ingesteld. U kunt dit doen met behulp van de blade 'Wachtwoord opnieuw instellen' in de portal en deze referenties gebruikt voor aanmelding bij de seriële console.

## <a name="next-steps"></a>Volgende stappen
* Seriële Console te gebruiken [WORMGATEN opstart en geef de modus voor één gebruiker](serial-console-grub-single-user-mode.md)
* Gebruik voor de seriële Console [NMI en SysRq aanroepen](serial-console-nmi-sysrq.md)
* Informatie over het gebruik van de seriële Console [WORMGATEN in verschillende distributies inschakelen](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/)
* Seriële Console van het is ook beschikbaar voor [Windows](../windows/serial-console.md) VM's
* Meer informatie over [diagnostische gegevens over opstarten](boot-diagnostics.md)