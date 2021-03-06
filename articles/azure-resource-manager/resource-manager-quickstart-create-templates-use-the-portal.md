---
title: Een Azure Resource Manager-sjabloon maken en implementeren via Azure Portal | Microsoft Docs
description: Leer hoe u uw eerste Azure Resource Manager-sjabloon maakt via Azure Portal en hoe u deze implementeert.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/17/2018
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: 62bc29495bab3446b6131223110c694e53412db9
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49407636"
---
# <a name="quickstart-create-and-deploy-azure-resource-manager-templates-by-using-the-azure-portal"></a>Snelstart: Azure Resource Manager-sjablonen maken en implementeren via Azure Portal

Leer hoe u uw eerste Azure Resource Manager-sjabloon maakt door er een via de Azure-portal te maken en hoe u de sjabloon vanuit de Azure-portal bewerkt en implementeert. Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren. Met de instructies in deze zelfstudie wordt een Azure Storage-account gemaakt. U kunt dezelfde procedure gebruiken om andere Azure-resources te maken.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="generate-a-template-using-the-portal"></a>Een sjabloon maken via de portal

In deze sectie maakt u een Storage-account via Azure Portal. Voordat u het Storage-account implementeert, hebt u de optie de sjabloon te bekijken die op basis van uw configuraties in de portal is gemaakt. U kunt de sjabloon opslaan voor later gebruik.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **Een resource maken** > **Opslag** > **Opslagaccount - blob, bestand, tabel, wachtrij**.

    ![Een Azure Storage-account in Azure Portal maken](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-portal.png)
3. Voer de volgende informatie in. 

    - **Resourcegroep**: maak een nieuwe Azure-resourcegroep met een naam naar keuze. In de schermafbeelding is de naam van de resourcegroep *mystorage1016rg*.
    - **Naam**: geef het opslagaccount een unieke naam. In de schermafbeelding is de naam *mystorage1016*.

    Voor de overige eigenschappen kunt u de standaardwaarden gebruiken.

    ![Een Azure Storage-accountconfiguratie in Azure Portal maken](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account.png)

    > [!NOTE]
    > Sommige geëxporteerde sjablonen moeten worden bewerkt voordat u ze implementeren.

4. Selecteer **Controleren + maken** onderaan in het scherm. 
5. Selecteer **Een sjabloon voor automatisering downloaden** onderaan in het scherm. In de portal wordt de gegenereerde sjabloon weergegeven:

    ![Een sjabloon genereren via de portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template.png)

    In het hoofdvenster wordt de sjabloon getoond. Het is een JSON-bestand met vier elementen van het hoogste niveau: `schema`, `contentVersion`, `parameters` en `resources`. Zie [Informatie over de structuur en de syntaxis van Azure Resource Manager-sjablonen](./resource-group-authoring-templates.md) voor meer informatie

    Er zijn zes parameters gedefinieerd. Een van deze parameters heet **storageAccountName**. In het tweede gemarkeerde gedeelte ziet u hoe u deze parameter in de sjabloon gebruikt. In het volgende gedeelte bewerkt u de sjabloon zodat er voor het opslagaccount gebruik wordt gemaakt van een gegenereerde naam.

    In de sjabloon is er één Azure-resource gedefinieerd. Deze is van het type [Microsoft.Storage/storageAccounts]. Bekijk hoe de resource wordt gedefinieerd en wat de definitiestructuur is.
6. Selecteer **Download**. Sla **template.json** uit het gedownloade pakket op op uw computer. In het volgende gedeelte gebruikt u een implementatiehulpprogramma voor sjablonen om de sjabloon te bewerken.
7. Selecteer het tabblad **Parameter** om de waarden te bekijken die u voor de parameters hebt opgegeven. Schrijf deze waarden op. U hebt ze in het volgende gedeelte (bij het implementeren van de sjabloon) weer nodig.

    ![Een sjabloon genereren via de portal](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-create-storage-account-template-parameters.png)

    Met behulp van zowel de sjabloon- als de parameterbestanden kunt u een Azure Storage-account maken.

## <a name="edit-and-deploy-the-template"></a>De sjabloon bewerken en implementeren

U kunt de Azure-portal gebruiken om eenvoudige wijzigingen door te voeren in sjablonen. In deze snelstart gebruikt u het portalhulpprogramma *Sjabloonimplementatie*. Als u een complexere sjabloon wilt bewerken, kunt u gebruikmaken van [Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md). Dit biedt meer functionaliteit.

Azure vereist dat elke Azure-service een unieke naam heeft. De implementatie mislukt als u de naam van een opslagaccount invoert dat al bestaat. Om dit probleem te vermijden, kunt u een sjabloon voor functieaanroepen, `uniquestring()`, gebruiken voor het genereren van een unieke opslagaccountnaam.

1. Selecteer in Azure Portal **Een resource maken**.
2. In **Marketplace doorzoeken** typt u **sjabloonimplementatie**. Druk vervolgens op **ENTER**.
3. Selecteer **Sjabloonimplementatie**.

    ![Azure Resource Manager-sjabloonbibliotheek](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-library.png)
4. Selecteer **Maken**.
5. Selecteer **Bouw uw eigen sjabloon in de editor**.
6. Selecteer **Bestand laden** en volg de instructies voor het laden van het bestand template.json dat u in het vorige gedeelte hebt gedownload.
7. Voeg één variabele toe zoals wordt weergegeven in de volgende schermafbeelding:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
    ```
    ![Azure Resource Manager-sjablonen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-edit-storage-account-template-revised.png)

    Er worden hier twee functies gebruikt: `concat()` en `uniqueString()`.

8. Verwijder de parameter **storageAccountName** die in de vorige schermafbeelding is gemarkeerd.
9. Werk het naamelement bij van de resource **Microsoft.Storage/storageAccounts** voor gebruik van de nieuw gedefinieerde variabele in plaats van de parameter:

    ```json
    "name": "[variables('storageAccountName')]",
    ```

    De uiteindelijke sjabloon moet er als volgt uitzien:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "location": {
                "type": "string"
            },
            "accountType": {
                "type": "string"
            },
            "kind": {
                "type": "string"
            },
            "accessTier": {
                "type": "string"
            },
            "supportsHttpsTrafficOnly": {
                "type": "bool"
            }
        },
        "variables": {
            "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
            {
                "name": "[variables('storageAccountName')]",
                "type": "Microsoft.Storage/storageAccounts",
                "apiVersion": "2018-07-01",
                "location": "[parameters('location')]",
                "properties": {
                    "accessTier": "[parameters('accessTier')]",
                    "supportsHttpsTrafficOnly": "[parameters('supportsHttpsTrafficOnly')]"
                },
                "dependsOn": [],
                "sku": {
                    "name": "[parameters('accountType')]"
                },
                "kind": "[parameters('kind')]"
            }
        ],
        "outputs": {}
    }
    ```
7. Selecteer **Opslaan**.
8. Voer de volgende waarden in:

    - **Resourcegroep**: geef de resourcegroep een unieke naam.
    - **Locatie**: selecteer een locatie voor de resourcegroep.
    - **Locatie**: selecteer een locatie voor het opslagaccount.  U kunt dezelfde locatie gebruiken als voor de resourcegroep.
    - **Accounttype**: voer voor deze snelstart **Standard_LRS** in.
    - **Soort**: voer voor deze snelstart **StorageV2** in.
    - **Toegangslaag**: voer voor deze snelstart **Dynamisch** in.
    - **Alleen HTTPS-verkeer ingeschakeld**.  Selecteer voor deze snelstart **true**.
    - **Ik ga akkoord met de bovenstaande voorwaarden**: (selecteren)

    Dit is een schermafbeelding van een voorbeeldimplementatie:

    ![Implementatie van Azure Resource Manager-sjablonen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-deploy.png)

10. Selecteer **Aankoop**.
11. Selecteer het belpictogram (meldingen) boven in het scherm om de implementatiestatus te zien. Wacht tot de implementatie is voltooid.

    ![Notificatie van implementatie van Azure Resource Manager-sjablonen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-notification.png)

12. Selecteer **Ga naar de resourcegroep** in het deelvenster meldingen. U ziet een scherm dat vergelijkbaar is met:

    ![Resourcegroep voor implementatie van Azure Resource Manager-sjablonen](./media/resource-manager-quickstart-create-templates-use-the-portal/azure-resource-manager-template-tutorial-portal-deployment-resource-group.png)

    U ziet dat de status van de implementatie is voltooid en er slechts één opslagaccount in de resourcegroep is. De naam van het opslagaccount is een unieke tekenreeks gegenereerd door de sjabloon. Zie voor meer informatie over het gebruik van Azure storage-accounts [Snelstart: blobs uploaden, downloaden, en noteren met behulp van de Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet het opslagaccount in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een sjabloon genereert in Azure Portal en hoe u de sjabloon via de portal implementeert. De gebruikte sjabloon in deze snelstart is een eenvoudige sjabloon met één Azure-resource. Als de sjabloon complex is, kunt u beter Visual Studio Code of Visual Studio gebruiken om de sjabloon te ontwikkelen. In de volgende snelstart ziet u hoe u sjablonen ook kunt implementeren met Azure PowerShell en de Azure-opdrachtregelinterface (CLI).

> [!div class="nextstepaction"]
> [Create templates by using Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md) (Sjablonen maken met Visual Studio Code)
