---
title: Modules ontwikkelen voor Azure IoT Edge | Microsoft Docs
description: Informatie over het maken van aangepaste modules voor Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 10/05/2017
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 761485de4bf52b7261ac8f1f8c3d937486c66546
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50247997"
---
# <a name="understand-the-requirements-and-tools-for-developing-iot-edge-modules"></a>Informatie over de vereisten en hulpprogramma's voor het ontwikkelen van IoT Edge-modules

In dit artikel wordt uitgelegd welke functies beschikbaar zijn bij het schrijven van toepassingen die worden uitgevoerd als IoT Edge-module en hoe u kunt profiteren van deze.

## <a name="iot-edge-runtime-environment"></a>IoT Edge-runtime-omgeving
IoT Edge-runtime biedt de infrastructuur voor het integreren van de functionaliteit van meerdere IoT Edge-modules en deze te implementeren op IoT Edge-apparaten. Op hoog niveau, kan elk programma worden geleverd als een IoT Edge-module. Echter, als u wilt profiteren van IoT Edge communicatie- en beheerfuncties, een programma wordt uitgevoerd in een module kunt verbinding maken met lokale IoT Edge hub, geïntegreerd in de IoT Edge-runtime.

## <a name="using-the-iot-edge-hub"></a>Met behulp van de IoT Edge hub
De IoT Edge hub biedt twee belangrijke functies: proxy naar IoT Hub en lokale communicatie.

### <a name="iot-hub-primitives"></a>IoT Hub primitieven
IoT Hub ziet een module-exemplaar analogously aan een apparaat, in de zin dat deze:

* Er is een moduledubbel, die is uniek en geïsoleerd van de [apparaatdubbel](../iot-hub/iot-hub-devguide-device-twins.md) en de andere moduledubbels van het apparaat;
* Er kunnen worden verzonden [apparaat-naar-cloud-berichten](../iot-hub/iot-hub-devguide-messaging.md);
* kan de ontvangen [directe methoden](../iot-hub/iot-hub-devguide-direct-methods.md) speciaal bedoeld voor de identiteit.

Een module kan niet op dit moment ontvangt berichten van cloud-naar-apparaat en de functionaliteit voor het uploaden niet gebruiken.

Wanneer een module schrijven, kunt u gewoon gebruiken de [Azure IoT Device SDK](../iot-hub/iot-hub-devguide-sdks.md) verbinding maken met de IoT Edge hub en de bovenstaande functionaliteit gebruiken net als bij het gebruik van IoT-Hub met de apparaattoepassing van een, het enige verschil dat afkomstig is van uw back-endtoepassing, die u hebt om te verwijzen naar de module-id in plaats van de apparaat-id.

Zie [ontwikkelen en implementeren van een IoT Edge-module op een gesimuleerd apparaat](tutorial-csharp-module.md) voor een voorbeeld van een module-toepassing die apparaat-naar-cloud-berichten verzendt en maakt gebruik van de moduledubbel.

### <a name="device-to-cloud-messages"></a>Apparaat-naar-cloud-berichten
Om in te schakelen complexe verwerking van apparaat-naar-cloud-berichten, IoT Edge hub biedt declaratieve routering van berichten tussen modules en tussen modules en IoT-Hub.
Hierdoor kunnen modules worden onderschept en verwerken van berichten verzonden door andere modules en het doorgeven ervan in complexe pijplijnen.
Het artikel [modulesamenstelling](module-composition.md) wordt uitgelegd hoe u voor het opstellen van modules in complexe pijplijnen met behulp van routes.

Een IoT Edge-module, kan anders dan een normale IoT Hub device-toepassing, apparaat-naar-cloud-berichten die via proxy door de lokale IoT Edge hub, worden om te worden verwerkt door ze ontvangen.

IoT Edge hub doorgeven van berichten naar de module op basis van declaratieve routes die worden beschreven in de [modulesamenstelling](module-composition.md) artikel. Bij het ontwikkelen van een IoT Edge-module, kunt u deze berichten ontvangen door in te stellen bericht handlers, zoals wordt weergegeven in de zelfstudie [ontwikkelen en implementeren van een IoT Edge-module op een gesimuleerd apparaat] [lnk-zelfstudie2].

Vereenvoudigt het maken van routes, IoT Edge wordt het concept van module *invoer* en *uitvoer* eindpunten. Een module kan ontvangen van alle apparaat-naar-cloud-berichten doorgestuurd naar het zonder invoer op te geven en apparaat-naar-cloud-berichten kunt verzenden zonder eventuele uitvoer op te geven.
Met behulp van expliciete invoer en uitvoer, maar dan wel maakt regels voor doorsturen eenvoudiger om te begrijpen. Zie [modulesamenstelling](module-composition.md) voor meer informatie over regels voor doorsturen en invoer- en eindpunten voor modules.

Ten slotte worden verwerkt door de Edge hub apparaat-naar-cloud-berichten factureringslabel voor de volgende eigenschappen:

| Eigenschap | Beschrijving |
| -------- | ----------- |
| $connectionDeviceId | De apparaat-ID van de client die het bericht heeft verzonden |
| $connectionModuleId | De module-ID van de module die het bericht heeft verzonden |
| $inputName | De invoer die dit bericht ontvangen. Kan niet leeg zijn. |
| $outputName | De uitvoer die wordt gebruikt voor het verzenden van het bericht. Kan niet leeg zijn. |

### <a name="connecting-to-iot-edge-hub-from-a-module"></a>Verbinding maken met IoT Edge hub vanuit een module
Verbinding maken met de lokale IoT Edge hub vanuit een module bestaat uit twee stappen: de verbindingsreeks gebruiken op voorwaarde dat door de IoT Edge-runtime wanneer de module wordt gestart, en zorg ervoor dat uw toepassing accepteert het certificaat dat door de IoT Edge hub op het apparaat.

Het maken van verbinding tekenreeks die moet worden gebruikt door de IoT Edge-runtime in de omgevingsvariabele wordt geïnjecteerd `EdgeHubConnectionString`. Dit maakt ze beschikbaar voor alle programma's die wil gebruiken.

Analogously, het certificaat moet worden gebruikt voor het valideren van de IoT Edge hub-verbinding is opgenomen door de IoT Edge-runtime in een bestand waarvan het pad beschikbaar in de omgevingsvariabele is `EdgeModuleCACertificateFile`.

De zelfstudie [ontwikkelen en implementeren van een IoT Edge-module op een gesimuleerd apparaat] [lnk-zelfstudie2] laat zien hoe u om ervoor te zorgen dat het certificaat in het archief van de computer in uw toepassing module is. Moge duidelijk zijn dat een andere methode verbindingen met behulp van die werkzaamheden certificaat vertrouwen.

## <a name="packaging-as-an-image"></a>Verpakken als een installatiekopie
IoT Edge-modules worden geleverd als Docker-installatiekopieën.
U kunt Docker hulpprogrammaketen rechtstreeks of Visual Studio Code gebruiken zoals wordt weergegeven in de zelfstudie [ontwikkelen en implementeren van een IoT Edge-module op een gesimuleerd apparaat] [lnk-zelfstudie2].

## <a name="next-steps"></a>Volgende stappen

Nadat u een module ontwikkelen, informatie over hoe u [implementeren en controleren van IoT Edge-modules op schaal](how-to-deploy-monitor.md).

