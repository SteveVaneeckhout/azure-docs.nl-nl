---
title: Opmerkingen bij de release - Custom Vision Service
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: anroth
ms.openlocfilehash: 8b4ba1601a7c5a9f523f7f8145a032861aac5d8c
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901318"
---
# <a name="custom-vision-service-release-notes"></a>Opmerkingen bij de Release van de Custom Vision Service

## <a name="october-9-2018"></a>9 oktober 2018
- Objectdetectie van krijgt de betaalde Preview-versie. U kunt nu Objectdetectie projecten maken met een Azure-resource.
- 'Verplaatsen naar Azure'-functie toegevoegd aan, zodat u gemakkelijk om te upgraden van een beperkte proefversie project koppelen aan een Azure-website. resource in de gekoppelde project (F0 of S0). U kunt dit vinden op de pagina instellingen voor uw product.  
- Toegevoegde exporteren naar de ONNX-1.2, ter ondersteuning van de Update van oktober 2018 Windows-versie van Windows-ML.
Bugfixes, met inbegrip van ONNX exporteren met speciale tekens. 


## <a name="august-14-2018"></a>14 augustus 2018
- Toegevoegde 'Aan de slag' widget naar site customvision.ai gebruikers begeleidt bij project training. 
- Verdere verbeteringen aan de machine learning-pijplijn om te profiteren van multilabel projecten (nieuwe verlies laag).

## <a name="june-28-2018"></a>28 juni 2018
- Verbeteringen in bugfixes & back-end.
- Multiklassen indeling, voor projecten waar afbeeldingen precies één label hebt ingeschakeld. In voorspellingen voor multiklassen modus kansen zal oplopen tot een (alle installatiekopieën worden geclassificeerd onder de opgegeven labels).

## <a name="june-13-2018"></a>13 juni 2018
- UX vernieuwen, gericht op gebruiksgemak en toegankelijkheid. 
- Verbeteringen aan de machine learning-pijplijn om te profiteren van multilabel projecten met een groot aantal tags.
- Bug verholpen in TensorFlow exporteren. Geëxporteerde model versiebeheer, ingeschakeld zodat iteraties kunnen meer dan één keer worden geëxporteerd. 

## <a name="may-7-2018"></a>7 mei 2018
- Preview-functie voor objectdetectie geïntroduceerd voor beperkte proefprojecten.
- Upgrade naar 2.0 API’s
- S0-laag is uitgebreid naar 250 labels en 50.000 afbeeldingen. 
- Belangrijke back-endverbeteringen aan de machine learning-pijplijn voor classificatieprojecten voor afbeeldingen. Projecten die zijn getraind na 27 april 2018, profiteren van deze updates.
- Modelexport naar ONNX toegevoegd voor gebruik met Windows ML.
- Modelexport naar DockerFile toegevoegd. Dit stelt u in staat om de artefacten te downloaden om uw eigen Windows- en Linux-containers te bouwen, inclusief een DockerFile, TensorFlow-model en servicecode. 
- Voor nieuwe getrainde modellen die zijn geëxporteerd naar TensorFlow in het algemeen (cd) en oriëntatiepunt (cd) domeinen, [betekenen waarden zijn nu (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample), voor consistentie in alle projecten. 

## <a name="march-1-2018"></a>1 maart 2018
- Start van betaalde preview-versie en onboarding bij Azure Portal. Projecten kunnen nu worden gekoppeld aan Azure-resources met een F0-laag (gratis) of een S0-laag (Standard). Introductie van S0-laagprojecten die ruimte bieden voor maximaal 100 labels en 25.000 afbeeldingen. 
- Back-endwijzigingen in de machine learning-pijplijn/-normalisatieparameter. Hierdoor hebben klanten meer controle bij afwegingen over de precisie van de resultaten bij het aanpassen van de waarschijnlijkheidsdrempelwaarde. Als onderdeel van deze wijzigingen is de standaardwaarde van de waarschijnlijkheidsdrempel in de CustomVision.ai-portal ingesteld op 50%.

## <a name="december-19-2017"></a>19 december 2017

- Export naar Android (TensorFlow) is toegevoegd, naast de eerder uitgebrachte export naar iOS (CoreML). Hierdoor kan het exporteren van een getraind compact-model offline worden uitgevoerd in een toepassing.
- Domeinen Detailhandel en Oriëntatiepunt (compact) zijn toegevoegd om modelexports voor deze domeinen in te schakelen.
- Uitgebrachte versie [1.2 Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) en [1.1 Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Bijgewerkte API’s bieden ondersteuning voor modelexports, nieuwe voorspellingsbewerking waarmee afbeeldingen niet worden opgeslagen in Voorspellingen, en introductie van batchbewerkingen in de Training API.
- UX-aanpassingen, waaronder de mogelijkheid om te zien welk domein is gebruikt om een iteratie te trainen.
- Bijgewerkte [C# SDK en voorbeeld](https://github.com/Microsoft/Cognitive-CustomVision-Windows).

