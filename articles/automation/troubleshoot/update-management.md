---
title: Problemen oplossen met updatebeheer
description: Meer informatie over het oplossen van problemen met updatebeheer
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: f52767058ef69d29465f1274109b6d3ffe58296c
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092624"
---
# <a name="troubleshooting-issues-with-update-management"></a>Oplossen van problemen met updatebeheer

Dit artikel worden oplossingen voor het oplossen van problemen die worden uitgevoerd op bij het gebruik van updatebeheer.

Er is een agent probleemoplosser voor Hybrid Worker-agent om te bepalen van het onderliggende probleem. Zie voor meer informatie over de probleemoplosser voor [problemen oplossen update-agent](update-agent-issues.md). Voor alle andere problemen, Zie de gedetailleerde informatie hieronder over mogelijke problemen.

## <a name="general"></a>Algemeen

### <a name="components-enabled-not-working"></a>Scenario: De onderdelen voor de oplossing voor beheer van updates zijn ingeschakeld en nu deze virtuele machine wordt geconfigureerd

#### <a name="issue"></a>Probleem

U blijven ziet het volgende bericht op een virtuele machine 15 minuten na de onboarding:

```
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Oorzaak

Deze fout kan worden veroorzaakt door de volgende redenen:

1. Communicatie terug naar het Automation-Account wordt geblokkeerd.
2. De virtuele machine onboarding wordt uitgevoerd kan afkomstig zijn uit een gekloonde machine die niet Sysprep met de Microsoft Monitoring Agent is geïnstalleerd is.

#### <a name="resolution"></a>Oplossing

1. Ga naar, [netwerkplanning](../automation-hybrid-runbook-worker.md#network-planning) voor meer informatie over welke adressen en poorten moeten worden toegestaan voor het beheer van updates om te werken.
2. Als u met behulp van een gekloonde installatiekopie, sysprep de installatiekopie van het eerste en de MMA-agent installeren na de gebeurtenis.

## <a name="windows"></a>Windows

Als u problemen ondervindt tijdens een poging voor de Onboarding van de oplossing op een virtuele machine, controleert u de **Operations Manager** logboek voor systeemgebeurtenissen onder **logboeken toepassingen en Services** op de lokale computer voor gebeurtenissen met de gebeurtenis-ID **4502** en gebeurtenis-bericht met **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.

De volgende sectie worden specifieke foutberichten en een mogelijke oplossing voor elk gemarkeerd. Zie voor andere onboarding problemen, [oplossen onboarding van oplossingen](onboarding.md).

### <a name="machine-already-registered"></a>Scenario: Machine is al geregistreerd bij een ander account

#### <a name="issue"></a>Probleem

U ontvangt de volgende strekking weergegeven:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Oorzaak

De machine is al vrijgegeven aan een andere werkruimte voor updatebeheer.

#### <a name="resolution"></a>Oplossing

Voer het opruimen van oude artefacten op de computer door [verwijderen van de hybride runbookgroep](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) en probeer het opnieuw.

### <a name="machine-unable-to-communicate"></a>Scenario: Machine kan niet communiceren met de service is

#### <a name="issue"></a>Probleem

U ontvangt een van de volgende foutberichten:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Oorzaak

Er zijn mogelijk een proxy, de gateway of de firewall blokkeert de netwerkcommunicatie.

#### <a name="resolution"></a>Oplossing

Controleer uw netwerken en zorg ervoor dat de juiste poorten en -adressen zijn toegestaan. Zie [vereisten voor de](../automation-hybrid-runbook-worker.md#network-planning), voor een lijst met poorten en -adressen die zijn vereist voor het beheer van updates en Hybrid Runbook Workers.

### <a name="unable-to-create-selfsigned-cert"></a>Scenario: Kan geen zelfondertekend certificaat maken

#### <a name="issue"></a>Probleem

U ontvangt een van de volgende foutberichten:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Oorzaak

De Hybrid Runbook Worker is niet een zelfondertekend certificaat genereren

#### <a name="resolution"></a>Oplossing

Controleer of systeemaccount leestoegang heeft tot map **C:\ProgramData\Microsoft\Crypto\RSA** en probeer het opnieuw.

### <a name="nologs"></a>Scenario: Updatebeheer gegevens niet worden weergegeven in Log Analytics voor een virtuele machine

#### <a name="issue"></a>Probleem

U machines hebt die als **niet beoordeeld** onder **naleving**, maar ziet u gegevens van heartbeat in Log Analytics voor de Hybrid Runbook Worker, maar niet voor updatebeheer.

#### <a name="cause"></a>Oorzaak

De Hybrid Runbook Worker moet mogelijk opnieuw worden geregistreerd en opnieuw geïnstalleerd.

#### <a name="resolution"></a>Oplossing

Volg de stappen in [Windows Hybrid Runbook Worker implementeren](../automation-windows-hrw-install.md) opnieuw installeren van de Hybrid Worker.

### <a name="hresult"></a>Scenario: Machine wordt niet beoordeeld en ziet u een uitzondering van HResult

#### <a name="issue"></a>Probleem

U machines hebt die als **niet beoordeeld** onder **naleving**, en u ziet een bericht van uitzondering eronder.

#### <a name="cause"></a>Oorzaak

Windows update is niet juist geconfigureerd op de machine.

#### <a name="resolution"></a>Oplossing

Dubbelklik op de uitzondering in rood om te zien van de gehele uitzonderingsbericht weergegeven. Bekijk de volgende tabel voor mogelijke oplossingen of acties te ondernemen:

|Uitzondering  |Oplossing of actie  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Zoeken naar de relevante foutcode in [Windows update code foutenlijst](https://support.microsoft.com/help/938205/windows-update-error-code-list) naar aanvullende informatie vinden over de oorzaak van de uitzondering.        |
|`0x8024402C` of `0x8024401C`     | Deze fouten zijn problemen met de netwerkverbinding. Zorg ervoor dat uw computer de juiste netwerkverbinding met updatebeheer heeft. Zie de sectie over [netwerkplanning](../automation-update-management.md#ports) voor een lijst met poorten en -adressen die vereist zijn.        |
|`0x8024402C`     | Als u van een WSUS-server gebruikmaakt, controleert u of de registerwaarden voor `WUServer` en `WUStatusServer` onder de registersleutel `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` hebt u de juiste WSUS-server.        |
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Zorg ervoor dat de Windows Update-service (wuauserv) wordt uitgevoerd en niet is uitgeschakeld.        |
|Andere algemene uitzondering     | Voer een zoekopdracht uit op het internet voor de mogelijke oplossingen en werken met uw lokale IT-ondersteuning.         |

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Scenario: De Update-uitvoering niet kan worden gestart

#### <a name="issue"></a>Probleem

Een update wordt uitgevoerd niet starten op een Linux-machine.

#### <a name="cause"></a>Oorzaak

De Hybrid Worker in Linux is beschadigd.

#### <a name="resolution"></a>Oplossing

Maak een kopie van het volgende logboekbestand en handhaven voor het oplossen van problemen:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Scenario: De Update-uitvoering wordt gestart, maar er een fout optreedt

#### <a name="issue"></a>Probleem

Een update-uitvoering wordt gestart, maar er een fout optreedt tijdens de uitvoering.

#### <a name="cause"></a>Oorzaak

Mogelijke oorzaken zijn:

* Pakketbeheer is niet in orde
* Specifieke pakketten kunnen leiden tot problemen met cloud op basis van patches
* Andere redenen

#### <a name="resolution"></a>Oplossing

Als er fouten optreden tijdens het bijwerken uitvoeren nadat deze is in Linux is gestart, controleert u de taak uitvoer van de geïnfecteerde computer in de uitvoering. Mogelijk vindt u specifieke foutberichten van Pakketbeheer van de computer die u kunt onderzoeken en actie ondernemen. Updatebeheer is de package manager in orde voor geslaagde update-implementaties zijn vereist.

In sommige gevallen pakketupdates kunnen leiden tot problemen met Update Management te voorkomen dat een update-implementatie niet worden voltooid. Als u dat ziet, hebt u deze handmatig installeren of deze pakketten uitsluiten van toekomstige update-uitvoeringen zelf.

Als u een probleem met een patch niet kunt oplossen, maakt u een kopie van het volgende logboekbestand en handhaven **voordat** de volgende update-implementatie is gestart voor het oplossen van problemen:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Volgende stappen

Als u uw probleem niet zien of u niet kunt oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

* Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
* Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
* Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.