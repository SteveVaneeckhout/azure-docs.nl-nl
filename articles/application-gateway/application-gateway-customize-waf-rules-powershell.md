---
title: Regels voor web application firewall in Azure Application Gateway - PowerShell aanpassen | Microsoft Docs
description: In dit artikel bevat informatie over het aanpassen van regels voor web application firewall in Application Gateway met PowerShell.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: victorh
ms.openlocfilehash: b341bdbe6611187b158f353d00077d33f317f374
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365421"
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>Regels voor web application firewall via PowerShell aanpassen

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure-CLI](application-gateway-customize-waf-rules-cli.md)

De Azure Application Gateway web application firewall (WAF) biedt beveiliging voor webtoepassingen. Deze beveiligingen worden geleverd door de Open Web Application Security Project (OWASP) Core regel ingesteld (CRS). Sommige regels kunnen fout-positieven en echte verkeer blokkeert. Om deze reden biedt Application Gateway de mogelijkheid om aan te passen regelgroepen en -regels. Zie voor meer informatie over de specifieke regelgroepen en regels [lijst met web application firewall CRS-regelgroepen en -regels](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>Weergave-regelgroepen en -regels

De volgende codevoorbeelden laten zien hoe om regels en regelgroepen die kunnen worden geconfigureerd in een toepassingsgateway met WAF-functionaliteit weer te geven.

### <a name="view-rule-groups"></a>Groepen van de regel weergeven

Het volgende voorbeeld laat zien hoe om regelgroepen weer te geven:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

De volgende uitvoer is een afgekapte reactie van het vorige voorbeeld:

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a>Uitschakelen van regels

Het volgende voorbeeld wordt uitgeschakeld regels `910018` en `910017` in een toepassingsgateway:

```powershell
$disabledrules=New-AzureRmApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-910-IP-REPUTATION -Rules 910018,910017
Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
Set-AzureRmApplicationGateway -ApplicationGateway $gw
```

## <a name="next-steps"></a>Volgende stappen

Nadat u uw uitgeschakelde regels hebt geconfigureerd, leert u hoe u om uw WAF-logboeken weer te geven. Zie voor meer informatie, [Application Gateway Diagnostics](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
