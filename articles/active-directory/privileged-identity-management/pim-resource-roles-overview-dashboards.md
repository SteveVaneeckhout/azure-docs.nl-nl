---
title: Een resource-dashboard gebruiken voor het uitvoeren van een toegangscontrole - Azure | Microsoft Docs
description: Hierin wordt beschreven hoe u een resource-dashboard met een toegangscontrole uitvoeren in Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 03/30/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 20172cf7413397aedc4b3c32d0f1419531a2588a
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188494"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review"></a>Een resource-dashboard gebruiken voor een toegangscontrole uitvoeren

Een resource-dashboard kunt u een toegangscontrole uitvoeren in Privileged Identity Management (PIM) voor Azure-resources. Het dashboard Beheerdersweergave heeft drie primaire onderdelen:

- Een grafische weergave van rolactiveringen resource.
- Twee grafieken die de distributie van roltoewijzingen door toewijzingstype weergeven.
- Een data-gebied met betrekking tot nieuwe roltoewijzingen.

![Schermopname van het dashboard-Beheerweergave van grafieken en diagrammen](media/azure-pim-resource-rbac/rbac-overview-top.png)

![Schermafbeelding van het dashboard weergeven van de beheerder een lijst met gegevens](media/azure-pim-resource-rbac/role-settings.png)

De grafische weergave van rolactiveringen resource bevat informatie over de afgelopen zeven dagen. Deze gegevens is afgestemd op de geselecteerde resource en activeringen voor de meest voorkomende rollen (eigenaar, Inzender, beheerder van gebruikerstoegang), en voor alle rollen gecombineerd weergegeven.

Aan de rechterkant van de grafiek activeringen weergeven twee grafieken de distributie van roltoewijzingen door toewijzingstype, voor zowel gebruikers en groepen. U kunt de waarde in een percentage (of omgekeerd), wijzigen door het selecteren van een segment van de grafiek.

Onder de grafieken ziet u het aantal gebruikers en groepen met nieuwe roltoewijzingen gedurende de afgelopen 30 dagen en een lijst met functies die zijn gesorteerd op basis van totaal aantal toewijzingen (in aflopende volgorde).

## <a name="next-steps"></a>Volgende stappen

- [Een toegangscontrole voor Azure resource-rollen in PIM starten](pim-resource-roles-start-access-review.md) 
