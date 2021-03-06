---
title: Snelstartgids - Azure SignalR-service REST-API | Microsoft Docs
description: Een snelstartgids voor het gebruik van de Azure SignalR Service REST-API.
services: signalr
documentationcenter: ''
author: sffamily
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET
ms.workload: tbd
ms.date: 06/13/2018
ms.author: zhshang
ms.openlocfilehash: 36fb87d3255149c041c4288d13c54eaff8425e06
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024337"
---
# <a name="quickstart-broadcast-real-time-messages-from-console-app"></a>Snelstartgids: Realtimeberichten verzenden via de console-app

Azure SignalR-service biedt [REST-API](https://github.com/Azure/azure-signalr/blob/dev/docs/rest-api.md) ter ondersteuning van server-naar-client-communicatie, zoals broadcasting. U kunt elke programmeertaal kiezen die een REST-API kan aanroepen. U kunt berichten verzenden naar alle verbonden clients, een specifieke client op basis van naam of een groep clients.

In deze snelstartgids leert u hoe u in C# berichten via een opdrachtregel-app kunt verzenden naar verbonden client-apps.

## <a name="prerequisites"></a>Vereisten

Deze quickstart kan worden uitgevoerd op macOS, Windows of Linux.
* [.NET Core-SDK](https://www.microsoft.com/net/download/core)
* Een teksteditor of code-editor naar keuze.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij Azure Portal op <https://portal.azure.com/>.


[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Laten wij de code klaarmaken terwijl de service wordt geïmplementeerd. Kloon de [voorbeeld-app vanuit GitHub](https://github.com/aspnet/AzureSignalR-samples.git), stel de SignalR Service-verbindingsreeks in en voer de toepassing lokaal uit.

1. Open een nieuw git-terminalvenster. Ga naar een map waarin u het voorbeeldproject wilt klonen.

1. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

Dit voorbeeld is een console-app die het gebruik van de Azure SignalR-service weergeeft. Deze biedt twee modi:

- Servermodus: gebruik eenvoudige opdrachten om de REST-API voor Azure SignalR-service aan te roepen.
- Clientmodus: maak verbinding met de Azure SignalR-service en ontvang berichten van de server.

U kunt ook zien hoe u een toegangstoken kunt genereren voor verificatie met Azure SignalR-service.

### <a name="build-the-executable-file"></a>Het uitvoerbare bestand maken

We gebruiken macOS osx.10.13-x64 als voorbeeld. U kunt [naslaginformatie](https://docs.microsoft.com/dotnet/core/rid-catalog) vinden over het maken op andere platforms.
```bash
cd AzureSignalR-samples/samples/Serverless/

dotnet publish -c Release -r osx.10.13-x64
```

### <a name="start-a-client"></a>Een client starten

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="start-a-server"></a>Een server starten

```bash
cd bin/Release/netcoreapp2.1/osx.10.13-x64/

Serverless server -c "<ConnectionString>" -h <HubName>
```

## <a name="run-the-sample-without-publishing"></a>Het voorbeeld uitvoeren zonder te publiceren

U kunt ook de onderstaande opdracht uitvoeren om een server of client te starten

```bash
# Start a server
dotnet run -- server -c "<ConnectionString>" -h <HubName>

# Start a client
dotnet run -- client <ClientName> -c "<ConnectionString>" -h <HubName>
```

### <a name="use-user-secrets-to-specify-connection-string"></a>Gebruikersgeheimen gebruiken om een verbindingsreeks te specificeren

U kunt `dotnet user-secrets set Azure:SignalR:ConnectionString "<ConnectionString>"` uitvoeren in de hoofdmap van het voorbeeld. Daarna hebt u de optie `-c "<ConnectionString>"` niet meer nodig.

## <a name="usage"></a>Gebruik

Gebruik de opdracht om het bericht te verzenden nadat de server is gestart

```
send user <User Id>
send users <User List>
send group <Group Name>
send groups <Group List>
broadcast
```

U kunt meerdere clients met verschillende clientnamen starten.

## <a name="usage"> </a> Integratie met services van derden
Met behulp van de Azure SignalR-service kunt services van derden integreren met het systeem.
### <a name="usage"> </a> Definitie van technische specificaties
In de volgende tabel worden alle versies van de ondersteunde REST-API's weergegeven op datum. U kunt er ook het definitiebestand voor elke specifieke versie vinden

Versie | API-status | Door | Specifiek
--- | --- | --- | ---
`1.0-preview` | Beschikbaar | 5002 | [Swagger](https://github.com/Azure/azure-signalr/tree/dev/docs/swagger/v1-preview.json)
`1.0` | Beschikbaar | Standard | [Swagger](https://github.com/Azure/azure-signalr/tree/dev/docs/swagger/v1.json)

De lijst met beschikbare API's voor elke specifieke versie is beschikbaar in de volgende lijst.

API | `1.0-preview` | `1.0`
--- | --- | ---
[Uitzenden naar alle](#broadcast) | :heavy_check_mark: | :heavy_check_mark:
[Uitzenden naar een groep](#broadcast-group) | :heavy_check_mark: | :heavy_check_mark:
Uitzenden naar bepaalde groepen | :heavy_check_mark: (afgeschaft) | `N / A`
[Verzenden naar specifieke gebruikers](#send-user) | :heavy_check_mark: | :heavy_check_mark:
Verzenden naar bepaalde gebruikers | :heavy_check_mark: (afgeschaft) | `N / A`
[Een gebruiker aan een groep toevoegen](#add-user-to-group) | `N / A` | :heavy_check_mark:
[Een gebruiker uit een groep verwijderen](#remove-user-from-group) | `N / A` | :heavy_check_mark:

<a name="broadcast"> </a>
### <a name="broadcast-to-everyone"></a>Uitzenden naar iedereen
Versie | API HTTP-methode | Aanvraag-URL | Aanvraagbody
--- | --- | --- | ---
`1.0-preview` | `POST` | `https://<instance-name>.service.signalr.net:5002/api/v1-preview/hub/<hub-name>` | `{"target": "<method-name>", "arguments": [...]}`
`1.0` | `POST` | `https://<instance-name>.service.signalr.net/api/v1/hubs/<hub-name>` | Hetzelfde als hierboven

<a name="broadcast-group"> </a>
### <a name="broadcast-to-a-group"></a>Uitzenden naar een groep
Versie | API HTTP-methode | Aanvraag-URL | Aanvraagbody
--- | --- | --- | ---
`1.0-preview` | `POST` | `https://<instance-name>.service.signalr.net:5002/api/v1-preview/hub/<hub-name>/group/<group-name>` | `{"target": "<method-name>", "arguments": [...]}`
`1.0` | `POST` | `https://<instance-name>.service.signalr.net/api/v1/hubs/<hub-name>/groups/<group-name>` | Hetzelfde als hierboven

<a name="send-user"> </a>
### <a name="sending-to-specific-users"></a>Verzenden naar specifieke gebruikers
Versie | API HTTP-methode | Aanvraag-URL | Aanvraagbody
--- | --- | --- | ---
`1.0-preview` | `POST` | `https://<instance-name>.service.signalr.net:5002/api/v1-preview/hub/<hub-name>/user/<user-id>` | `{"target": "<method-name>", "arguments": [...]}`
`1.0` | `POST` | `https://<instance-name>.service.signalr.net/api/v1/hubs/<hub-name>/users/<user-id>` | Hetzelfde als hierboven

<a name="add-user-to-group"> </a>
### <a name="adding-a-user-to-a-group"></a>Een gebruiker aan een groep toevoegen
Versie | API HTTP-methode | Aanvraag-URL
--- | --- | ---
`1.0` | `PUT` | `https://<instance-name>.service.signalr.net/api/v1/hubs/<hub-name>/groups/<group-name>/users/<userid>`

<a name="remove-user-from-group"> </a>
### <a name="removing-a-user-from-a-group"></a>Een gebruiker uit een groep verwijderen
Versie | API HTTP-methode | Aanvraag-URL
--- | --- | ---
`1.0` | `DELETE` | `https://<instance-name>.service.signalr.net/api/v1/hubs/<hub-name>/groups/<group-name>/users/<userid>`

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]
