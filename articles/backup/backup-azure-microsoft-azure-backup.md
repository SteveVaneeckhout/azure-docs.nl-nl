---
title: Azure Backup Server gebruiken voor back-up van workloads naar Azure
description: Azure Backup Server gebruiken om te beveiligen of back-up van workloads naar Azure portal.
services: backup
author: PVRK
manager: shivamg
keywords: Azure Backup-server; bescherming van workloads; back-up van workloads
ms.service: backup
ms.topic: conceptual
ms.date: 7/10/2018
ms.author: adigan
ms.openlocfilehash: 67243aca9f5f578402ff79422783148af53798c6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38546014"
---
# <a name="install-and-configure-azure-backup-server"></a>Installeren en configureren van Azure Backup Server
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

In dit artikel wordt uitgelegd hoe u uw omgeving voorbereiden op back-up van workloads met Azure Backup Server. Met Azure Backup Server, kunt u werkbelastingen van toepassingen zoals Hyper-V-machines, Microsoft SQL Server, SharePoint Server, Microsoft Exchange, en Windows-clients beveiligen vanaf één console.

> [!NOTE]
> Azure Backup Server kunnen nu virtuele VMware-machines beveiligen en biedt mogelijkheden voor verbeterde beveiliging. Het product installeert, zoals wordt beschreven in de secties hieronder; Update 1 en de meest recente Azure Backup-Agent toepassen. Zie het artikel voor meer informatie over back-ups van VMware-servers met Azure Backup Server, [Azure Backup Server gebruiken voor back-up van een VMware-server](backup-azure-backup-server-vmware.md). Raadpleeg voor meer informatie over mogelijkheden voor beveiliging, [Azure back-beveiligingsfuncties documentatie](backup-azure-security-feature.md).
>
>

U kunt ook infrastructuur beveiligen als een Service (IaaS)-werkbelastingen, zoals virtuele machines in Azure.

> [!NOTE]
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel bevat de informatie en procedures voor het herstellen van virtuele machines die zijn geïmplementeerd met behulp van de Resource Manager-model.
>
>

Azure Backup Server neemt veel van de back-functionaliteit van workload van Data Protection Manager (DPM). In dit artikel bevat koppelingen naar de DPM-documentatie met een verklaring van enkele van de gedeelde functionaliteit. Azure Backup Server deelt Hoewel veel van dezelfde functionaliteit als DPM. Azure Backup-Server geen back-up naar tape, noch is deze dan geïntegreerd met System Center.

## <a name="choose-an-installation-platform"></a>Kies vervolgens een installatieplatform voor
De eerste stap naar de Azure Backup-Server om aan de slag en actief is voor het instellen van een Windows-Server. Uw server worden in Azure of on-premises.

### <a name="using-a-server-in-azure"></a>Met behulp van een server in Azure
Bij het kiezen van een server voor het uitvoeren van Azure Backup Server, verdient het aanbeveling dat u beginnen met een afbeelding van Windows Server 2012 R2 Datacenter of Windows Server 2016 Datacenter. Het artikel [uw eerste Windows-machine maken in Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), biedt een zelfstudie voor aan de slag met de aanbevolen virtuele machine in Azure, zelfs als u Azure al eerder nog nooit hebt gebruikt. De aanbevolen minimale vereisten voor de server-machine (VM) moeten zijn: Standard A2 met twee kernen en 3,5 GB RAM-geheugen.

Werklasten beveiligen met Azure Backup Server heeft veel aspecten. Het artikel [DPM installeren als een virtuele machine van Azure](https://technet.microsoft.com/library/jj852163.aspx), helpt deze aspecten uitgelegd. Lees dit artikel volledig voordat het implementeren van de machine.

### <a name="using-an-on-premises-server"></a>Met behulp van een on-premises server
Als u niet uitvoeren van de basis-server in Azure wilt, kunt u de server uitvoeren op een Hyper-V-virtuele machine, een VMware-VM of een fysieke host. De aanbevolen minimale vereisten voor de serverhardware zijn twee kernen en 4 GB RAM-geheugen. De ondersteunde besturingssystemen worden vermeld in de volgende tabel:

| Besturingssysteem | Platform | SKU |
|:--- | --- |:--- |
| Windows Server 2016 en de meest recente SP 's |64 bits |Standard, Datacenter, Essentials (en hoger MABS-v2) |
| Windows Server 2012 R2 en de meest recente SP's |64 bits |Standard, Datacenter, Foundation |
| Windows Server 2012 en de meest recente SP's |64 bits |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 en de meest recente SP's |64 bits |Standard, Workgroup |
| Windows Storage Server 2012 en de meest recente SP's |64 bits |Standard, Workgroup |

U kunt de DPM-opslag met behulp van Windows Server-Ontdubbeling ontdubbelen. Meer informatie over hoe u [DPM en Ontdubbeling](https://technet.microsoft.com/library/dn891438.aspx) samenwerken wanneer geïmplementeerd in Hyper-V-machines.

> [!NOTE]
> Azure Backup Server is ontworpen om uit te voeren op een toegewezen server met één doel. U kunt Azure Backup Server niet installeren op:
> - Een computer die wordt uitgevoerd als een domeincontroller
> - Een computer waarop de toepassingsserverfunctie is geïnstalleerd
> - Een computer die een System Center Operations Manager-beheerserver is
> - Een computer waarop Exchange Server wordt uitgevoerd
> - Een computer die een knooppunt van een cluster is

Altijd Azure Backup Server toevoegen aan een domein. Als u van plan bent om te verplaatsen van de server naar een ander domein, installeert u eerst Azure Backup Server, wordt de server toevoegen aan het nieuwe domein. Verplaatsen van een bestaande Azure Backup Server-machine naar een nieuw domein na implementatie is *niet ondersteund*.

Of u het verzenden van back-upgegevens naar Azure of lokaal houden, moet u Azure Backup-Server geregistreerd met een Recovery Services-kluis.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Opslagreplicatie instellen
U kunt met de optie voor opslagreplicatie kiezen tussen geografisch redundante opslag en lokaal redundante opslag. Recovery Services-kluizen gebruiken standaard geografisch redundante opslag. Als deze kluis uw primaire kluis, laat u de opslagoptie die is ingesteld op geografisch redundante opslag. Kies lokaal redundante opslag als u een goedkopere optie wilt die niet zo duurzaam is. U vindt meer informatie over de opties voor [geografisch redundante](../storage/common/storage-redundancy-grs.md) en [lokaal redundante](../storage/common/storage-redundancy-lrs.md) opslag in het [overzicht van Azure Storage-replicatie](../storage/common/storage-redundancy.md).

De instelling voor opslagreplicatie bewerken:

1. Selecteer uw kluis om het dashboard van de kluis en het menu instellingen te openen. Als de **instellingen** menu niet wordt geopend, klikt u op **alle instellingen** in het dashboard van de kluis.
2. Op de **instellingen** menu, klikt u op **back-upinfrastructuur** > **back-upconfiguratie** openen de **back-upconfiguratie**blade. Op de **back-upconfiguratie** menu, kiest u de optie voor opslagreplicatie voor uw kluis.

    ![Lijst met back-upkluizen](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Nadat u de opslagoptie voor uw kluis hebt gekozen, bent u klaar om de VM aan de kluis te koppelen. Voordat u de VM aan de kluis koppelt, moet u eerst de virtuele Azure-machines detecteren en registreren.

## <a name="software-package"></a>Softwarepakket
### <a name="downloading-the-software-package"></a>Het softwarepakket downloaden
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Als u al een Recovery Services-kluis is geopend, gaat u verder met stap 3. Als u nog geen een Recovery Services-kluis is geopend, maar in Azure portal, in het hoofdmenu zijn, klikt u op **Bladeren**.

   * Typ in de lijst met resources **Recovery Services**.
   * Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Wanneer **Recovery Services-kluizen** wordt weergegeven, klikt u erop.

     ![Een Recovery Services-kluis maken, stap 1](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     De lijst met Recovery Services-kluizen wordt weergegeven.
   * Selecteer in de lijst met Recovery Services-kluizen een kluis.

     Het geselecteerde kluisdashboard wordt geopend.

     ![Blade Kluis openen](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. De **instellingen** blade wordt geopend standaard. Als dit is gesloten, klikt u op **instellingen** om de instellingenblade te openen.

    ![Blade Kluis openen](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Klik op **back-up** om de wizard aan de slag te openen.

    ![Back-up aan de slag](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    In de **aan de slag met back-up** blade die wordt geopend, **back-doelen** worden automatisch geselecteerd.

    ![Back-up-doelen-standaard-geopend](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. In de **back-updoel** blade van de **waar wordt uw werkbelasting uitgevoerd** in het menu **On-premises**.

    ![on-premises en werkbelastingen als doelen](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    Uit de **waarvan wilt u back-up maken?** vervolgkeuzelijst, selecteer de werkbelastingen die u wilt beveiligen met behulp van Azure Backup-Server en klik vervolgens op **OK**.

    De **aan de slag met back-up** wizard switches de **infrastructuur voorbereiden** optie voor het back-up van workloads naar Azure.

   > [!NOTE]
   > Als u alleen back-up van bestanden en mappen wilt, raden wij aan met behulp van de Azure backup-agent en de informatie in het artikel te volgen [eerste kennismaking: Maak een back-up van bestanden en mappen](backup-try-azure-backup-in-10-mins.md). Als u wilt meer dan de bestanden en mappen beveiligen, of u van plan bent om uit te breiden de behoeften voor beveiliging in de toekomst, selecteert u deze werkbelastingen.
   >
   >

    ![Wijzigen van de wizard aan de slag](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. In de **infrastructuur voorbereiden** blade die wordt geopend, klikt u op de **downloaden** koppelingen voor Azure Backup-Server installeren en de kluisreferenties downloaden. U kunt de kluisreferenties gebruiken tijdens de registratie van Azure Backup Server naar de recovery services-kluis. De koppelingen gaat u naar het Downloadcentrum waar het softwarepakket dat kan worden gedownload.

    ![Infrastructuur voorbereiden voor Azure Backup Server](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Selecteer alle bestanden en klik op **volgende**. Download alle bestanden die binnenkomen vanaf de downloadpagina van Microsoft Azure Backup en plaatst alle bestanden in dezelfde map.

    ![1 center downloaden](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Aangezien het van de downloadgrootte van alle bestanden samen > 3G, klikt u op een 10 Mbps downloaden koppeling die het duurt maximaal 60 minuten voor het downloaden is voltooid.

### <a name="extracting-the-software-package"></a>De software-updatepakket uitpakken
Nadat u alle bestanden hebt gedownload, klikt u op **MicrosoftAzureBackupInstaller.exe**. Hiermee start u de **installatiewizard voor Microsoft Azure-back-up** om op te halen van de setup-bestanden naar een door u opgegeven locatie. Ga door met de wizard en klikt u op de **extraheren** knop om te beginnen met de extractie.

> [!WARNING]
> Ten minste 4GB aan vrije ruimte is vereist voor het uitpakken van de setup-bestanden.
>
>

![Back-installatiewizard voor Microsoft Azure](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Zodra de extractie proces is voltooid, schakel het selectievakje in om te starten vers uitgepakte *setup.exe* om te beginnen met de installatie van Microsoft Azure Backup Server en klik op de **voltooien** knop.

### <a name="installing-the-software-package"></a>De software-updatepakket installeren
1. Klik op **Microsoft Azure Backup** om te starten van de wizard setup.

    ![Back-installatiewizard voor Microsoft Azure](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Klik op het welkomstscherm wordt weergegeven op de **volgende** knop. Hiermee gaat u naar de *Vereistencontroles* sectie. In dit scherm, klikt u op **controleren** om te bepalen als de hardware- en vereisten voor Azure Backup Server wordt voldaan. Als alle vereisten is voldaan, ziet u een bericht weergegeven dat aangeeft dat de machine voldoet aan de vereisten. Klik op de **volgende** knop.

    ![Azure Backup Server - welkomstpagina en de vereisten controleren](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup Server vereist SQL Server Standard. Het installatiepakket voor de Azure Backup Server wordt verder meegeleverd met de juiste SQL Server-binaire bestanden nodig als u niet wilt gebruiken van uw eigen SQL. Bij het starten met een nieuwe installatie van de Azure Backup Server, moet u de optie kiezen **nieuw exemplaar van SQL Server installeren met deze instelling** en klikt u op de **controleren en installeren** knop. Nadat de vereiste onderdelen zijn geïnstalleerd, klikt u op **volgende**.

    ![Azure Backup Server - controle van SQL](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Als er een fout optreedt met een aanbeveling klikken om de computer opnieuw hebt opgestart, doen en klikt u op **nogmaals controleren**.

   > [!NOTE]
   > Azure Backup Server werkt niet met een extern SQL Server-exemplaar. Het wordt gebruikt door Azure Backup Server-exemplaar moet lokaal zijn.
   >
   >
4. Geef een locatie voor de installatie van Microsoft Azure Backup server-bestanden en klikt u op **volgende**.

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    De nieuwe locatie is een vereiste voor back-up naar Azure. Zorg ervoor dat de nieuwe locatie is ten minste 5% van de gegevens die zijn gepland om te worden back-ups op de cloud. Voor bescherming van de schijf moeten afzonderlijke schijven worden geconfigureerd nadat de installatie is voltooid. Zie voor meer informatie over opslaggroepen [configureert u opslaggroepen en schijfopslag](https://technet.microsoft.com/library/hh758075.aspx).
5. Geef een sterk wachtwoord voor de beperkte lokale gebruikersaccounts en klik op **volgende**.

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Selecteer of u wilt gebruiken *Microsoft Update* om te controleren op updates en klikt u op **volgende**.

   > [!NOTE]
   > We raden u aan Windows Update wordt omgeleid naar de Microsoft Update beveiligingsupdates en belangrijke updates voor Windows en andere producten, zoals Microsoft Azure Backup Server biedt.
   >
   >

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Controleer de *samenvatting van instellingen* en klikt u op **installeren**.

    ![Back-up PreReq2 van Microsoft Azure](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. De installatie gebeurt in fasen. In de eerste fase wordt de Microsoft Azure Recovery Services-Agent geïnstalleerd op de server. De wizard controleert ook of verbinding met Internet. Als er verbinding met Internet beschikbaar is kunt u doorgaan met installatie, zo niet, moet u opgeven de proxygegevens verbinding maken met Internet.

    De volgende stap is het configureren van de Microsoft Azure Recovery Services-Agent. Als onderdeel van de configuratie moet u uw kluisreferenties opgeven voor het registreren van de machine naar de recovery services-kluis. U biedt ook een wachtwoordzin op om te versleutelen/ontsleutelen van de gegevens die worden verzonden tussen Azure en uw locatie. U kunt automatisch genereren van een wachtwoordzin of uw eigen minimaal 16 tekens wachtwoordzin opgeven. Ga verder met de wizard totdat de agent is geconfigureerd.

    ![Azure Backup Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Zodra de registratie van de Microsoft Azure Backup-server is voltooid, wordt de wizard setup van algemene gaat deze naar de installatie en configuratie van SQL Server en de Azure Backup Server-onderdelen. Nadat de installatie van de SQL Server-onderdelen is voltooid, wordt de Azure Backup Server-onderdelen zijn geïnstalleerd.

    ![Azure Backup-server](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Wanneer de installatiestap is voltooid, wordt pictogrammen op het bureaublad van het product zijn gemaakt ook. Dubbelklik op het pictogram voor het starten van het product.

### <a name="add-backup-storage"></a>Backup storage toevoegen
De eerste back-up wordt opgeslagen op opslag die is gekoppeld aan de Azure Backup-Server-machine. Zie voor meer informatie over het toevoegen van schijven [configureert u opslaggroepen en schijfopslag](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> U moet back-upopslag toevoegen, zelfs als u van plan bent om gegevens te verzenden naar Azure. In de huidige architectuur van Azure Backup-Server, de Azure Backup-kluis bevat de *tweede* kopie van de gegevens tijdens de lokale opslag bevat de eerste (en verplichte) back-up.
>
>

## <a name="network-connectivity"></a>Verbinding met het netwerk
Azure Backup Server is de verbinding met de Azure Backup-service voor het product om te werken is vereist. Als u wilt valideren of de machine de verbinding met Azure heeft, gebruikt u de ```Get-DPMCloudConnection``` cmdlet in de Azure Backup Server PowerShell-console. Als de uitvoer van de cmdlet waar is en vervolgens verbinding bestaat, anders is er geen met.

Op hetzelfde moment moet de Azure-abonnement in een foutloze toestand bevindt. Om erachter te komen de status van uw abonnement en te beheren, meld u aan bij de [-abonnementsportal](https://account.windowsazure.com/Subscriptions).

Zodra u weet de status van de Azure-connectiviteit en van het Azure-abonnement dat, kunt u de onderstaande tabel om de impact op de functionaliteit van back-up/herstel erachter te komen.

| Status connectiviteit | Azure-abonnement | Back-up naar Azure | Back-up naar schijf | Herstellen van Azure | Terugzetten vanaf schijf |
| --- | --- | --- | --- | --- | --- |
| Verbonden |Actief |Toegestaan |Toegestaan |Toegestaan |Toegestaan |
| Verbonden |Verlopen |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verbonden |De inrichting ongedaan gemaakt |Gestopt |Gestopt |Gestopt en Azure herstelpunten verwijderd |Gestopt |
| Verbinding is verbroken > 15 dagen |Actief |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verbinding is verbroken > 15 dagen |Verlopen |Gestopt |Gestopt |Toegestaan |Toegestaan |
| Verbinding is verbroken > 15 dagen |De inrichting ongedaan gemaakt |Gestopt |Gestopt |Gestopt en Azure herstelpunten verwijderd |Gestopt |

### <a name="recovering-from-loss-of-connectivity"></a>Herstellen uit het verlies van connectiviteit
Als u een firewall of een proxy die is verhinderen van toegang tot Azure hebt, moet u aan lijst met geaccepteerde de volgende adressen domein in de firewall/proxy-profiel:

* `http://www.msftncsi.com/ncsi.txt`
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Zodra de verbinding met Azure is hersteld naar de Azure Backup Server-machine, worden de bewerkingen die kunnen worden uitgevoerd worden bepaald door de status van de Azure-abonnement. De bovenstaande tabel bevat informatie over de bewerkingen die zijn toegestaan wanneer de computer 'verbonden'.

### <a name="handling-subscription-states"></a>Afhandeling van abonnementsstatussen
Is het mogelijk te houden van een Azure-abonnement van een *verlopen* of *inrichting ongedaan gemaakt* status naar de *Active* staat. Maar dit sommige gevolgen heeft op het gedrag van het product terwijl de status niet is *Active*:

* Een *inrichting ongedaan gemaakt* abonnement verliest functionaliteit voor de periode waarin het is de inrichting ongedaan gemaakt. Op het draaien *Active*, de functionaliteit van back-up/herstel van het product is verwijderd. De back-upgegevens op de lokale schijf kunnen ook worden opgehaald als deze werd gehouden met een groot genoeg bewaarperiode. De back-upgegevens in Azure is echter onherstelbaar verloren zodra het abonnement voert de *inrichting ongedaan gemaakt* staat.
* Een *verlopen* functionaliteit voor het abonnement alleen verliest totdat deze is gemaakt *Active* opnieuw. Een back-ups voor de periode dat het abonnement is gepland *verlopen* kan niet worden uitgevoerd.

## <a name="troubleshooting"></a>Problemen oplossen
Als Microsoft Azure Backup server is mislukt met fouten tijdens de setup-fase (of back-up of herstellen), verwijzen naar dit [fout codes document](https://support.microsoft.com/kb/3041338) voor meer informatie.
U kunt ook verwijzen naar [Azure back-up die betrekking hebben Veelgestelde vragen](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Volgende stappen
U kunt gedetailleerde informatie ophalen over [uw omgeving voorbereiden voor DPM](https://technet.microsoft.com/library/hh758176.aspx) op de website Microsoft TechNet. Het bevat ook informatie over ondersteunde configuraties waarop Azure Backup Server kan worden geïmplementeerd en gebruikt.

Deze artikelen kunt u een dieper begrip van werkbelastingsbescherming met Microsoft Azure Backup-server.

* [Back-up van SQL Server](backup-azure-backup-sql.md)
* [Back-up van SharePoint server](backup-azure-backup-sharepoint.md)
* [Alternatieve server back-up](backup-azure-alternate-dpm-server.md)
