---
title: Ondersteuning voor Azure IoT Edge-Platform | Microsoft Docs
description: Platforms die worden ondersteund door Azure IoT Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b8b4664540d07b76d0cebe5df99ba2cc67dd47e6
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50248552"
---
# <a name="azure-iot-edge-support"></a>Ondersteuning van Azure IoT Edge
Er zijn tal van manieren om te zoeken naar ondersteuning voor de Azure IoT Edge-product.

**Rapportage van fouten** – het merendeel van ontwikkeling die in de Azure IoT Edge-product gaat er gebeurt in de IoT Edge open source-project. Fouten worden gerapporteerd in de [kwesties](https://github.com/azure/iotedge/issues) van het project. Oplossingen maken snel mogelijk naar productupdates van het project in.

**Microsoft Customer Support team** : gebruikers die beschikken over een [ondersteuningsplan](https://azure.microsoft.com/support/plans/) het team Microsoft Customer Support kunnen verrichten door het maken van een ondersteuningsticket rechtstreeks vanuit de [Azure-portal]( https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Functie aanvragen** : de Azure IoT Edge-product functieverzoeken bijgehouden via van het product [Uservoice-pagina](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="operating-systems"></a>Besturingssystemen
Azure IoT Edge wordt uitgevoerd op de meeste besturingssystemen die containers; kunnen worden uitgevoerd. al deze waarden worden echter niet gelijkmatig ondersteund. Besturingssystemen zijn gegroepeerd in lagen, waarbij het niveau van ondersteuning voor gebruikers kunnen verwachten.

### <a name="tier-1"></a>Categorie 1
Laag 1-systemen kunnen worden beschouwd als officieel ondersteund. Dit betekent dat Microsoft:
* Deze besturingssysteem heeft in geautomatiseerde tests uit
* biedt de installatiepakketten van deze

Algemeen beschikbaar
| Besturingssysteem | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Raspbian stretch | Nee | Ja|
| Ubuntu Server 16.04 | Ja | Nee |
| Ubuntu-Server 18.04 | Ja | Nee |

Openbare Preview
| Besturingssysteem | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Windows 10 IoT Core (April 2018 update) | Ja | Nee |
| Windows 10 IoT Enterprise (April 2018 update) | Ja | Nee |
| Windows 10-Server 1803 | Ja | Nee |

### <a name="tier-2"></a>Categorie 2
Laag 2-systemen kunnen worden beschouwd als compatibel met Azure IoT Edge en relatief eenvoudig kunnen worden gebruikt. Dit betekent dat:
* Microsoft heeft gedaan ad-hoc testen op een platform of kent van een partner die Azure IoT Edge is uitgevoerd op het platform
* Installatiepakketten voor andere platforms werken op deze platforms

| Besturingssysteem | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | Ja | Ja |
| Debian 8 | Ja | Ja |
| Debian 9 | Ja | Ja |
| RHEL 7.5 | Ja | Ja |
| Ubuntu 18.04 | Ja | Ja |
| Ubuntu 16.04 | Ja | Ja |
| Wind rivier 8 | Ja | Nee |
| Yocto | Ja | Nee |

## <a name="container-engines"></a>Container-engines
Azure IoT Edge moet een container-engine starten modules, ongeacht het besturingssysteem waarop deze wordt uitgevoerd. Microsoft biedt een container-engine, moby-engine, om te voldoen aan deze vereiste. Deze is gebaseerd op de Moby open source-project. Docker CE- en Docker EE zijn andere populaire container-engines. Ze zijn ook afhankelijk van het Moby open-source-project en compatibel zijn met Azure IoT Edge. Microsoft biedt best inspanning ondersteuning voor systemen met behulp van de container motoren; Microsoft heeft echter niet de mogelijkheid om oplossingen voor problemen in deze af te leveren. Microsoft adviseert om deze reden moby-engine op productiesystemen.

