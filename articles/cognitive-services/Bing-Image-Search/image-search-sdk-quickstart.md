---
title: 'Snelstart: Zoeken naar afbeeldingen met de Bing Afbeeldingen zoeken-SDK voor C#'
description: Gebruik deze snelstart om voor de eerste keer afbeeldingen te zoeken met behulp van de Bing Afbeeldingen zoeken-SDK, wat een wrapper is voor de API en die dus dezelfde functies bevat. Deze eenvoudige C#-toepassing verzendt een zoekquery voor afbeeldingen, parseert het JSON-antwoord en geeft de URL weer van de eerst geretourneerde afbeelding.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: edebd1361e39a338672b4249dd159e5c1d4078ce
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294149"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-c"></a>Snelstart: Zoeken naar afbeeldingen met de Bing Afbeeldingen zoeken-SDK en C#

Gebruik deze snelstart om voor de eerste keer afbeeldingen te zoeken met behulp van de Bing Afbeeldingen zoeken-SDK, wat een wrapper is voor de API en die dus dezelfde functies bevat. Deze eenvoudige C#-toepassing verzendt een zoekquery voor afbeeldingen, parseert het JSON-antwoord en geeft de URL weer van de eerst geretourneerde afbeelding.

De broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) met extra foutafhandeling en aantekeningen.

## <a name="prerequisites"></a>Vereisten

* Elke versie van [Visual Studio 2017](https://visualstudio.microsoft.com/vs/whatsnew/).
* Het [Cognitieve Afbeeldingen zoeken NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/1.2.0).

Als u de Bing Afbeeldingen zoeken-SDK in Visual Studio wilt installeren, gebruikt u de `Manage NuGet Packages`-optie uit de Solution Explorer in Visual Studio.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>De toepassing maken en initialiseren

Maak eerst in Visual Studio een nieuwe C#-consoletoepassing. Voeg vervolgens de volgende pakketten toe aan uw project.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
```

Maak in de Main-methode van uw project variabelen voor uw geldige abonnementssleutel, de afbeeldingsresultaten die moeten worden geretourneerd door Bing en een zoekterm. Maak vervolgens met behulp van de sleutel een instantie van de client voor het zoeken van afbeeldingen.

```csharp
//IMPORTANT: replace this variable with your Cognitive Services subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";
//initialize the client
var client = new ImageSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));
```

## <a name="send-a-search-query-using-the-client"></a>Een zoekquery verzenden met behulp van de client

Gebruik de client om te zoeken met een querytekst:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Het eerste afbeeldingsresultaat

Parseer de afbeeldingsresultaten die in het antwoord zijn geretourneerd.
Als het antwoord zoekresultaten bevat, wordt het eerste resultaat opgeslagen en worden de bijbehorende gegevens weergegeven, zoals een miniatuur-URL en de oorspronkelijke URL, samen met het totale aantal geretourneerde afbeeldingen.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor app met één pagina voor Bing Image Search](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Zie ook

* [Wat is Bing Image Search?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Online interactieve demo proberen](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [.NET-voorbeelden voor de Azure Cognitive Services-SDK](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Documentatie van Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Naslag voor Bing Afbeeldingen zoeken-API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
