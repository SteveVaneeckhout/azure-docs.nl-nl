---
title: Een CI/CD-pijplijn voor uw bestaande code maken met het Azure DevOps-project | Azure DevOps Services-zelfstudie
description: Met het DevOps-project kunt u eenvoudig aan de slag in Azure. Hiermee kunt u uw eigen code en GitHub-opslagplaats gebruiken om slechts in enkele stappen een app op een Azure-service van uw keuze te starten.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 02b6823a46c94edb0ba28c7a2a8b9ae0efc44ae8
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406089"
---
# <a name="tutorial--create-a-cicd-pipeline-for-your-existing-code-with-the-azure-devops-project"></a>Zelfstudie: Een CI/CD-pijplijn voor uw bestaande code maken met het Azure DevOps-project

Het Azure DevOps-project geeft een vereenvoudigde ervaring waar u uw bestaande code en Git-opslagplaats gebruikt of een van de voorbeeldtoepassingen kiest voor het maken van een continue integratie- (CI) en continue leveringspijplijn (CD) naar Azure.

U gaat het volgende doen:

> [!div class="checklist"]
> * Een Azure DevOps-project maken
> * De toegang tot uw GitHub-opslagplaats configureren en een framework kiezen
> * Azure DevOps Services en een Azure-abonnement configureren 
> * Wijzigingen doorvoeren in GitHub en automatisch implementeren in Azure
> * De Azure DevOps Services CI/CD-pijplijn onderzoeken
> * Azure Application Insights-bewaking configureren

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. U kunt er een gratis krijgen via [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).
* Toegang tot een GitHub- of externe Git-opslagplaats met .NET, Java, PHP, Node, Python of statische webcode.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Het Azure DevOps-project maakt een CI-/CD-pijplijn in Azure DevOps Services.  U kunt een **nieuwe Azure DevOps Services**-organisatie maken of een **bestaande organisatie** gebruiken.  Het Azure DevOps-project maakt ook **Azure-resources** in het **Azure-abonnement** van uw keuze.

1. Meld u aan bij [Microsoft Azure Portal](https://portal.azure.com).

1. Kies het pictogram **+ Nieuw** in de linkernavigatiebalk en zoek naar **DevOps-project**.  Kies **Maken**.

    ![Continue levering starten](_img/azure-devops-project-github/fullbrowser.png)

1. Selecteer **Uw eigen code gebruiken**.  Als u klaar bent, kiest u **Volgende**.

## <a name="configure-access-to-your-github-repository-and-choose-a-framework"></a>De toegang tot uw GitHub-opslagplaats configureren en een framework kiezen

1. Selecteer **GitHub**.  Desgewenst kunt u een **externe Git-opslagplaats** kiezen.  Kies de **opslagplaats** en **vertakking** met uw toepassing.

1. Selecteer uw **webframework**.  Als u klaar bent, kiest u **Volgende**.

    ![.NET Framework](_img/azure-devops-project-github/webframework.png)

1. Het toepassingsframework dat u in de vorige stappen hebt gekozen bepaalt welk type implementatiedoel hier beschikbaar is voor de Azure-service.  Selecteer de **doelservice** van uw keuze.  Als u klaar bent, kiest u **Volgende**.

## <a name="configure-azure-devops-services-and-an-azure-subscription"></a>Azure DevOps Services en een Azure-abonnement configureren 

1. Maak een **nieuwe** Azure DevOps Services-organisatie of kies een **bestaande** organisatie.  Kies een **naam** voor uw Azure DevOps-project.  Selecteer uw **Azure-abonnement** en **locatie** en kies een **naam** voor uw toepassing.  Als u klaar bent, kiest u **Gereed**.

1. In slechts enkele minuten wordt het **Azure DevOps-projectdashboard** in de Azure Portal geladen.  Er wordt een voorbeeldtoepassing in een opslagplaats in uw Azure DevOps Services-organisatie ingesteld, er wordt een build uitgevoerd en uw toepassing wordt geïmplementeerd in Azure.  Dit dashboard biedt inzicht in uw GitHub-**codeopslagplaats**, **Azure DevOps Services CI/CD-pijplijn** en uw **toepassing in Azure**.  Aan de rechterkant van het dashboard selecteert u **Bladeren** om de lopende toepassing weer te geven.

    ![Dashboardweergave](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Het Azure DevOps-project configureert automatisch een CI-build en een versietrigger.  Uw code blijft in uw GitHub- of andere externe opslagplaats.  

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Wijzigingen doorvoeren in GitHub en automatisch implementeren in Azure 

U bent nu klaar om met een team samen te werken aan uw app met een CI/CD-proces dat automatisch uw meest recente werk aan uw website implementeert.  Elke wijziging in de GitHub-opslagplaats start een build in Azure DevOps, en met een Azure DevOps-CD-pijplijn wordt een implementatie uitgevoerd naar Azure.

1.  Breng een wijziging aan in uw toepassing en **voer deze door** in uw GitHub-opslagplaats.
2.  Na enkele ogenblikken wordt een build in Azure DevOps Services gestart.  U kunt de buildstatus bewaken met het Azure DevOps-projectdashboard of in de browser met uw Azure DevOps Services-organisatie.
3.  Wanneer de build is voltooid, **vernieuwt u de toepassing** in de browser om te controleren of u uw wijzigingen kunt zien.

## <a name="examine-the-azure-devops-services-cicd-pipeline"></a>De Azure DevOps Services CI/CD-pijplijn onderzoeken

Het Azure DevOps-project heeft automatisch een Azure DevOps Services-CI/CD-pijplijn geconfigureerd in uw Azure DevOps Services-organisatie.  U kunt de pijplijn verkennen en zo nodig aanpassen.  Volg de onderstaande stappen om vertrouwd te raken met de Azure DevOps Services-build- en -release-pipelines.

1. Selecteer **Build-pipelines** **bovenin** het Azure DevOps-projectdashboard.  Met deze koppeling opent u een tabblad in de browser met de Azure DevOps Services-build-pipeline voor het nieuwe project.

1. Beweeg de muisaanwijzer naar de rechterkant van de build-pijplijn naast het veld **Status**. Selecteer het **weglatingsteken** dat wordt weergegeven.  Met deze actie wordt een menu geopend waarin u diverse acties kunt starten, zoals een nieuwe build in de wachtrij plaatsen, een build onderbreken en de build-pipeline bewerken.

1. Selecteer **Bewerken**.

1. In deze weergave **onderzoekt u de verschillende taken** voor uw build-pijplijn.  De build voert verschillende taken uit, zoals het ophalen van bronnen uit de Git-opslagplaats, het herstellen van afhankelijkheden en het publiceren van uitvoer die wordt gebruikt voor implementaties.

1. Selecteer bovenaan de build-pijplijn de **naam van de build-pijplijn**.

1. Wijzig de **naam** van de build-pijplijn in een gebruiksvriendelijkere naam.  Selecteer **Opslaan en wachtrij** en selecteer vervolgens **Opslaan**.

1. Selecteer onder de naam van de build-pijplijn de optie **Geschiedenis**.  U ziet een audittrail van recente wijzigingen voor de build.  Azure DevOps Services houdt alle wijzigingen in de build-pipeline bij en biedt de mogelijkheid om versies te vergelijken.

1. Selecteer **Triggers**.  Het Azure DevOps-project maakt automatisch een CI-trigger en met elke doorvoering naar de opslagplaats wordt een nieuwe build gestart.  U kunt desgewenst kiezen of u vertakkingen van het CI-proces wilt opnemen of uitsluiten.

1. Selecteer **Retentie**.  Op basis van uw scenario kunt u beleidsregels opgeven om een bepaald aantal builds te behouden of te verwijderen.

1. Selecteer **Build en release** en kies **Releases**.  Het Azure DevOps-project heeft een Azure DevOps Services-release-pipeline gemaakt om implementaties naar Azure te beheren.

1. Selecteer links van de browser het **beletselteken** naast uw release-pipeline en kies **Bewerken**.

1. De release-pijplijn bevat een **pijplijn** die het releaseproces definieert.  Onder **Artefacten** selecteert u **Neerzetten**.  De build-pijplijn die u in de vorige stappen hebt onderzocht, produceert de uitvoer die wordt gebruikt voor het artefact. 

1. Selecteer rechts van het pictogram **Neerzetten** het pictogram **Continue implementatietrigger**.  Deze release-pipeline heeft een ingeschakelde CD-trigger die een implementatie uitvoert telkens wanneer er een nieuw build-artefact beschikbaar is.  U kunt de trigger desgewenst uitschakelen, zodat uw implementaties handmatig moeten worden uitgevoerd. 

1. Selecteer aan de linkerkant van de browser de optie **Taken**.  De taken zijn de acties die in het implementatieproces worden uitgevoerd.  In dit voorbeeld is een taak gemaakt om te implementeren naar **Azure App Service**.

1. Selecteer aan de rechterkant van de browser de optie **Releases weergeven**.  In deze weergave wordt een versiesgeschiedenis weergegeven.

1. Selecteer het **weglatingsteken** naast een van uw versies en kies **Openen**.  Er zijn verschillende menu's die u in deze weergave kunt verkennen, zoals een versieoverzicht, gekoppelde werkitems en tests.

1. Selecteer **Doorvoeringen**.  In deze weergave worden de codedoorvoeringen getoond die zijn gekoppeld aan de specifieke implementatie. 

1. Selecteer **Logboeken**.  De logboeken bevatten nuttige informatie over het implementatieproces.  Ze kunnen worden weergegeven tijdens en na de implementaties.

## <a name="configure-azure-application-insights-monitoring"></a>Azure Application Insights-bewaking configureren

Met Azure Application Insights kunt u eenvoudig de prestaties en het gebruik van een webtoepassing controleren.  Het Azure DevOps-project heeft automatisch een Application Insights-resource voor uw toepassing geconfigureerd.  U kunt verder diverse waarschuwingen en bewakingsopties configureren, indien nodig.

1. Navigeer naar het **Azure DevOps-project**dashboard in Azure Portal.  Kies rechtsonder in het dashboard de koppeling **Application Insights** voor uw app.

1. De blade **Application Insights** wordt geopend in Azure Portal.  Deze weergave bevat controlegegevens over gebruik, prestaties en beschikbaarheid voor uw app.

    ![Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Selecteer **Tijdsbereik** en kies **Afgelopen uur**.  Selecteer **Bijwerken** om de resultaten te filteren.  U ziet nu alle activiteiten van de afgelopen 60 minuten.  Selecteer de **x** om het tijdsbereik af te sluiten.

1. Selecteer **Waarschuwingen** en vervolgens **+ Metrische waarschuwing toevoegen**.  

1. Voer een **naam** in voor de waarschuwing.

1. Selecteer de vervolgkeuzelijst voor **Bron wijzigen voor**.  Kies uw **App Service-resource.**
<!-- Could you please confirm if this should be "Source Alter on" instead of "Source Alert on"? -->
1. De standaardwaarschuwing is voor een **serverreactietijd langer dan 1 seconde**.  Selecteer de vervolgkeuzelijst **Metrische gegevens** om de verschillende metrische waarschuwingsgegevens te verkennen.  U kunt eenvoudig tal van waarschuwingen configureren om de controlemogelijkheden van uw app te verbeteren.

1. Schakel het selectievakje in voor **Eigenaars, bijdragers en lezers op de hoogte brengen via e-mail**.  Desgewenst kunt u aanvullende acties uitvoeren wanneer een waarschuwing wordt geactiveerd, door een logische app van Azure uit te voeren.

1. Kies **OK** om de waarschuwing te maken.  Binnen enkele ogenblikken wordt de waarschuwing als actief weergegeven op het dashboard.  **Verlaat** het gebied Waarschuwingen en ga terug naar de blade **Application Insights**.

1. Selecteer **Beschikbaarheid** en selecteer **+ Test toevoegen**. 

1. Voer een **testnaam** in en kies **Maken**.  Hiermee maakt u eenvoudige ping-test om de beschikbaarheid van uw toepassing te controleren.  Na een paar minuten zijn de resultaten beschikbaar en toont het Application Insights-dashboard een beschikbaarheidsstatus.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de Azure-app-service en verwante resources die in deze snelstart zijn gemaakt niet meer nodig hebt, kunt u ze verwijderen met de functionaliteit **Verwijderen** in het Azure DevOps-projectdashboard.

## <a name="next-steps"></a>Volgende stappen

Wanneer u het CI/CD-proces in deze snelstart hebt geconfigureerd, zijn er automatisch een build- en release-pipeline in uw Azure DevOps-project gemaakt. U kunt deze build- en release-pipelines desgewenst wijzigen in overeenstemming met de behoeften van uw team. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een Azure DevOps-project maken
> * De toegang tot uw GitHub-opslagplaats configureren en een framework kiezen
> * Azure DevOps Services en een Azure-abonnement configureren 
> * Wijzigingen doorvoeren in GitHub en automatisch implementeren in Azure
> * De Azure DevOps Services CI/CD-pijplijn onderzoeken
> * Azure Application Insights-bewaking configureren

In de volgende zelfstudie vindt u meer informatie over de Azure DevOps Services CI/CD-pijplijn:

> [!div class="nextstepaction"]
> [CD-proces aanpassen](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
