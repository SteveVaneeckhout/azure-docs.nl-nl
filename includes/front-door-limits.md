---
title: bestand opnemen
description: bestand opnemen
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 9/17/2018
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: f0c2d1501b9aa19dec8c4ad157e004a57e0e5070
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006484"
---
| Resource | Standaardlimiet |
| --- | --- |
| Voordeur resources per abonnement | 100 |
| Front-end-hosts geldt ook voor aangepaste domeinen per resource | 100 |
| Regels voor doorsturen per resource | 100 |
| Back-endpools per resource | 50 |
| Back-ends per back-endpool | 100 |
| Padpatronen voor een regel voor doorsturen | 25 |
| Aangepaste web application firewall-regels per beleid | 10 |
| Web application firewall-beleid per resource | 100 |

### <a name="timeout-values"></a>Time-outwaarden
#### <a name="client-to-front-door"></a>Client naar de voordeur
- Voordeur heeft een niet-actieve TCP-verbindingstime-out van 61 seconden.
#### <a name="front-door-to-application-backend"></a>Voordeur back-end van toepassing
- Als het antwoord een gesegmenteerde antwoord is, wordt een 200 wordt geretourneerd als / wanneer de eerste chunk wordt ontvangen.
- Nadat de HTTP-aanvraag wordt doorgestuurd naar de back-end, wacht voordeur 30 seconden voor het eerste pakket van back-end, voordat u een 503-fout terugkeert naar de client.
- Nadat het eerste pakket wordt ontvangen van de back-end, wacht de voordeur gedurende 30 seconden (time-out voor inactiviteit) voordat u een 503-fout terugkeert naar de client.
- Voordeur op back-end TCP sessietime-out is 30 minuten.

### <a name="upload--download-data-limit"></a>Uploaden / downloaden van gegevens beperken

|  | Met gesegmenteerde overdrachtscodering (CTE) | Zonder HTTP logische groepen te verdelen |
| ---- | ------- | ------- |
| **Downloaden** | Er is geen limiet voor de downloadgrootte | Er is geen limiet voor de downloadgrootte |
| **Uploaden** |  Er is geen limiet is, zolang elke CTE uploaden minder dan 28.6 MB | De grootte mag niet groter zijn dan 28.6 MB. |
