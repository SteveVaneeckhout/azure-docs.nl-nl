---
title: Azure Cloud Shell-beperkingen | Microsoft Docs
description: Overzicht van de beperkingen van Azure Cloud Shell
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: juluk
ms.openlocfilehash: 135496e17ae884db580922aa31f6824b2e7fd934
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855981"
---
# <a name="limitations-of-azure-cloud-shell"></a>Beperkingen van Azure Cloudshell

Azure Cloud Shell bevat de volgende beperkingen:

## <a name="general-limitations"></a>Algemene beperkingen

### <a name="system-state-and-persistence"></a>Systeemstatus- en persistentie

De computer waarmee u uw Cloud Shell-sessie is tijdelijk en deze wordt gerecycled nadat uw sessie niet actief voor de 20 minuten is. Cloudshell is vereist voor een Azure-bestandsshare te koppelen. Uw abonnement moet als gevolg hiervan kunnen opslagresources instellen voor toegang tot Cloud Shell. Andere overwegingen zijn onder andere:

* Met gekoppelde opslag, alleen de wijzigingen in de `$Home` directory zijn opgeslagen.
* Azure-bestandsshares kunnen worden gekoppeld alleen vanuit uw [regio toegewezen](persisting-shell-storage.md#mount-a-new-clouddrive).
  * Voer in Bash, `env` te vinden van uw regio instellen als `ACC_LOCATION`.

### <a name="browser-support"></a>Browserondersteuning

Cloudshell biedt ondersteuning voor de nieuwste versies van Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox en Apple Safari. Safari in de privémodus wordt niet ondersteund.

### <a name="copy-and-paste"></a>Kopiëren en plakken

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Voor een bepaalde gebruiker kan slechts één shell actief zijn

Gebruikers kunnen alleen starten één type shell op een tijdstip, ofwel **Bash** of **PowerShell**. U mogelijk echter meerdere exemplaren van de Bash- of PowerShell in één keer uitgevoerd. Wisselen tussen Bash of PowerShell oorzaken Cloud Shell te starten, waarmee bestaande sessies beëindigd.

### <a name="usage-limits"></a>Gebruiksbeperkingen

Cloudshell is bedoeld voor interactieve gebruiksvoorbeelden. Als gevolg hiervan, geen niet-interactieve langlopende-sessies worden beëindigd zonder waarschuwing.

## <a name="bash-limitations"></a>Bash-beperkingen

### <a name="user-permissions"></a>Gebruikersmachtigingen

Machtigingen zijn ingesteld als normale gebruikers zonder toegang tot sudo. Elke installatie buiten uw `$Home` directory is niet persistent.

### <a name="editing-bashrc"></a>.Bashrc bewerken

Nemen voorzichtig bij het bewerken van .bashrc, in dat geval kan onverwachte fouten veroorzaken in Cloud Shell.

## <a name="powershell-limitations"></a>PowerShell-beperkingen

### <a name="azuread-module-name"></a>`AzureAD` modulenaam

De `AzureAD` modulenaam is momenteel `AzureAD.Standard.Preview`, de module biedt dezelfde functionaliteit.

### <a name="sqlserver-module-functionality"></a>`SqlServer` modulefunctionaliteit

De `SqlServer` opgenomen in de Cloud Shell-module bevat alleen prerelease ondersteuning voor PowerShell Core. In het bijzonder `Invoke-SqlCmd` is nog niet beschikbaar.

### <a name="default-file-location-when-created-from-azure-drive"></a>Standaardlocatie wanneer gemaakt op basis van Azure-station:

Met behulp van PowerShell-cmdlets, kunnen gebruikers niet maken in de Azure-station bestanden. Wanneer gebruikers nieuwe bestanden met andere hulpprogramma's, zoals vim of nano, maakt de bestanden worden opgeslagen in de `$HOME` standaard. 

### <a name="gui-applications-are-not-supported"></a>GUI-toepassingen worden niet ondersteund

Als de gebruiker wordt uitgevoerd een opdracht die u een Windows-dialoogvenster, zoals maakt `Connect-AzureAD` of `Connect-AzureRmAccount`, een foutmelding zoals ziet: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-crashes-psreadline"></a>Tab-Aanvulling PSReadline loopt vast

Als de gebruiker EditMode in PSReadline is ingesteld op Emacs, de gebruiker probeert om weer te geven van alle mogelijkheden via de tab-aanvulling, en de-venstergrootte is te klein om weer te geven van alle mogelijkheden, PSReadline loopt vast.

### <a name="large-gap-after-displaying-progress-bar"></a>Wijd nadat de voortgangsbalk weergegeven

Als de gebruiker een actie uitvoert die wordt weergegeven een voortgangsbalk, zoals een tabblad voltooien tijdens het in de `Azure:` station, dan is het mogelijk dat de cursor is niet juist ingesteld en een onderbreking wordt weergegeven wanneer de voortgangsbalk is eerder.

### <a name="random-characters-appear-inline"></a>Willekeurige tekens inline worden weergegeven

De volgorde van de positie van cursor codes, bijvoorbeeld `5;13R`, kan worden weergegeven in de invoer van de gebruiker.  De tekens kunnen handmatig worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

[Oplossen van problemen met Cloudshell](troubleshooting.md) <br>
[Snelstartgids voor Bash](quickstart.md) <br>
[Snelstartgids voor PowerShell](quickstart-powershell.md)
