---
title: Metrische gegevens en waarschuwingen in Azure Traffic Manager | Microsoft Docs
description: Dit artikel beschrijft metrische gegevens beschikbaar zijn voor Traffic Manager in Azure.
services: traffic-manager
documentationcenter: ''
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: 2be5718dd95eff837c5a016b5254be0992eb8f9a
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138375"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Traffic Manager-statistieken en -waarschuwingen

Traffic Manager kunt u DNS-gebaseerde taakverdeling waaronder meerdere Routering methoden en controle-opties-eindpunt. In dit artikel beschrijft de metrische gegevens en de bijbehorende waarschuwingen die beschikbaar voor klanten zijn. 

## <a name="metrics-available-in-traffic-manager"></a>Metrische gegevens beschikbaar zijn in Traffic Manager 

Traffic Manager biedt de volgende metrische gegevens op basis van per profiel die kan worden gebruikt door klanten om hun gebruik van Traffic manager en de status van de eindpunten in dit profiel te begrijpen.  

### <a name="queries-by-endpoint-returned"></a>Query's op eindpunt geretourneerd
Gebruik [met deze metriek](../monitoring-and-diagnostics/monitoring-supported-metrics.md) om het aantal query's dat is verwerkt door een Traffic Manager-profiel gedurende een bepaalde periode weer te geven. U kunt ook de dezelfde informatie weergeven op een eindpunt niveau granulariteit die geeft u inzicht in hoe vaak een eindpunt is geretourneerd in de reacties op query's vanuit Traffic Manager.

In het volgende voorbeeld wordt weergegeven in afbeelding 1 alle reacties van query's die zijn geretourneerd door de Traffic Manager-profiel. 

  
![Metrische gegevens van Traffic Manager - samengevoegde weergave van alle query 's](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Afbeelding 1: Weergeven met alle query's samenvoegen*
  
Afbeelding 2 ziet u de dezelfde gegevens, maar deze wordt gesplitst door eindpunten. Als gevolg hiervan, ziet u het volume van de reacties op query's waarin een bepaald eindpunt is geretourneerd.

![Metrische gegevens de Traffic Manager - weergave van queryvolume per eindpunt splitsen](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Afbeelding 2: Weergave splitsen met queryvolume weergegeven per eindpunt geretourneerd*

## <a name="endpoint-status-by-endpoint"></a>Eindpuntstatus door eindpunt
Gebruik [met deze metriek](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) om te begrijpen van de status van de eindpunten in het profiel. Het duurt twee waarden:
 - Gebruik **1** , als het eindpunt actief is.
 - Gebruik **0** als het eindpunt niet actief is.

Deze metrische gegevens kan worden weergegeven als een cumulatieve waarde vertegenwoordigt de status van alle metrische (afbeelding 3), of deze kan worden gesplitst (Zie afbeelding 4) om de status van specifieke eindpunten weer te geven. In het geval van de vorige, als het aggregatieniveau van de is geselecteerd als **Avg**, de waarde van deze metriek wordt het rekenkundige gemiddelde van de status van alle eindpunten. Bijvoorbeeld, als een profiel twee eindpunten heeft en slechts één in orde is, met deze metriek heeft een waarde van **0,50** zoals wordt weergegeven in afbeelding 3. 


![Metrische gegevens van Traffic Manager - samengestelde weergave van de status van endpoint](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Afbeelding 3: Samengestelde weergave van metrische gegevens eindpunt status: "Avg" aggregatie geselecteerd*


![Metrische gegevens de Traffic Manager - weergave van de status van endpoint splitsen](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Afbeelding 4: Gesplitste weergave van metrische gegevens over endpoint-status*

U kunt deze metrische gegevens via verbruiken [Azure Monitor service](../monitoring-and-diagnostics/monitoring-supported-metrics.md)van portal [REST-API](https://docs.microsoft.com/rest/api/monitor/), [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), en [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights), of via het gedeelte van de metrische gegevens van Traffic Manager van portal-ervaring.

## <a name="alerts-on-traffic-manager-metrics"></a>Waarschuwingen over metrische gegevens voor Traffic Manager
Naast de verwerking en weergeven van metrische gegevens van Traffic Manager, kunnen Azure Monitor klanten configureren en waarschuwingen die zijn gekoppeld aan deze metrische gegevens ontvangen. U kunt voorwaarden moeten worden voldaan in deze metrische gegevens voor een waarschuwing plaatsvindt, hoe vaak deze voorwaarden hoeven te worden bewaakt en hoe u de waarschuwingen moeten worden verzonden. Zie voor meer informatie, [documentatie van Azure Monitor-waarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure Monitor-service](../monitoring-and-diagnostics/monitoring-supported-metrics.md)
- Meer informatie over het [een nieuwe grafiek maken met Azure Monitor](../monitoring-and-diagnostics/monitoring-metric-charts.md#how-do-i-create-a-new-chart)
