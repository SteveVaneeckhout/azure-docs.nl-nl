---
title: Voorbeeld Azure Policy - Factureringstagbeleidinitiatief
description: Voor deze voorbeeldbeleidsset zijn gespecificeerde tagwaarden voor kostenplaats en productnaam vereist.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 936a794826b8923512deb4c3def2ee41499683e9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984618"
---
# <a name="billing-tags-policy-initiative"></a>Factureringstagbeleidinitiatief

Voor deze beleidsset zijn gespecificeerde tagwaarden voor kostenplaats en productnaam vereist. Er wordt gebruikgemaakt van ingebouwde beleidsregels om vereiste tags toe te passen en af te dwingen. U geeft de vereiste waarden voor de tags op.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Voorbeeldsjabloon

[!code-json[main](../../../../policy-templates/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.json "Billing Tags Policy Initiative")]

U kunt deze sjabloon implementeren met behulp van de [Azure-portal](#deploy-with-the-portal) of met [PowerShell](#deploy-with-powershell).

## <a name="deploy-with-the-portal"></a>Implementeren met portal

[![Implementeren in Azure](http://azuredeploy.net/deploybutton.png)](https://aka.ms/getpolicy)

## <a name="deploy-with-powershell"></a>Implementeren met PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

```azurepowershell-interactive
$policydefinitions = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.definitions.json"
$policysetparameters = "https://raw.githubusercontent.com/Azure/azure-policy/master/samples/PolicyInitiatives/multiple-billing-tags/azurepolicyset.parameters.json"

$policyset= New-AzureRmPolicySetDefinition -Name "multiple-billing-tags" -DisplayName "Billing Tags Policy Initiative" -Description "Specify cost Center tag and product name tag" -PolicyDefinition $policydefinitions -Parameter $policysetparameters

New-AzureRmPolicyAssignment -PolicySetDefinition $policyset -Name <assignmentname> -Scope <scope>  -costCenterValue <required value for Cost Center tag> -productNameValue <required value for product Name tag>
```

### <a name="clean-up-powershell-deployment"></a>PowerShell-implementatie opschonen

Gebruik de volgende opdracht om de resourcegroep, VM, en alle gerelateerde resources te verwijderen.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="apply-tags-to-existing-resources"></a>Tags toepassen op bestaande resources

Nadat u het beleid hebt toegewezen, kunt u een update van alle bestaande resources activeren om het tagbeleid dat u hebt toegevoegd, af te dwingen. Het volgende script behoudt andere tags die op de resources aanwezig waren:

```azurepowershell-interactive
$resources = Get-AzureRmResource -ResourceGroupName 'ExampleGroup'

foreach ($r in $resources) {
    try {
        Set-AzureRmResource -Tags ($a = if ($r.Tags -eq $NULL) { @{} } else {$r.Tags}) -ResourceId $r.ResourceId -Force -UsePatchSemantics
    }
    catch {
        Write-Host $r.ResourceId + "can't be updated"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

- Bekijk meer voorbeelden op [Voorbeelden van Azure Policy](index.md)