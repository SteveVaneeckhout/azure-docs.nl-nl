---
title: 'Snelstart: Video Search SDK, Python'
titleSuffix: Azure Cognitive Services
description: De Video Search SDK-consoletoepassing instellen.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: quickstart
ms.date: 02/15/2018
ms.author: rosh
ms.openlocfilehash: 9784894ea9a9deb350171e0d19042eb65644093b
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225647"
---
# <a name="quickstart-bing-video-search-sdk-with-python"></a>Snelstart: Bing Video Search SDK met Python

De Bing Image Search SDK bevat de functionaliteit van de REST API voor webaanvragen en parseerresultaten.

De [broncode voor voorbeelden van de Bing Video Search SDK voor Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) is beschikbaar op Git Hub.


## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing
Als u Python nog niet hebt, moet u dit eerst installeren. De SDK is compatibel met Python 2.7, 3.3, 3.4, 3.5 en 3.6.

De algemene aanbeveling voor het ontwikkelen met Python is om een [virtuele omgeving](https://docs.python.org/3/tutorial/venv.html) te gebruiken. Installeer en initialiseer de virtuele omgeving met de [venv-module](https://pypi.python.org/pypi/virtualenv). Installeer Virtualenv voor Python 2.7.
```
python -m venv mytestenv
```
Installeer Bing Video Search SDK-afhankelijkheden:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```
## <a name="video-search-client"></a>Video Search-client
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder *Zoeken*. Voeg imports toe:
```
subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Maak een instantie van de `CognitiveServicesCredentials` en instantieer de client:
```
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Zoek naar video's voor (SwiftKey) en controleer het aantal resultaten. Druk `ID`, `name` en `URL` van het eerste videoresultaat af.
```
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

try:
    video_result = client.videos.search(query="SwiftKey")
    print("Search videos for query \"SwiftKey\"")

    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")

except Exception as err:
    print("Encountered exception. {}".format(err))

```
Zoek naar video's voor (Bellevue Trailer) die gratis en kort zijn en een resolutie van 1080p hebben. Controleer het aantal resultaten en druk de `ID`, `name` en `URL` van het eerste videoresultaat af.
```
def video_search_with_filtering(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        video_result = client.videos.search(
            query="Bellevue Trailer",
            pricing=VideoPricing.free,  # Can use the str "free" too
            length=VideoLength.short,   # Can use the str "short" too
            resolution=VideoResolution.hd1080p  # Can use the str "hd1080p" too
        )
        print("Search videos for query \"Bellevue Trailer\" that is free, short and 1080p resolution")

        if video_result.value:
            first_video_result = video_result.value[0]
            print("Video result count: {}".format(len(video_result.value)))
            print("First video id: {}".format(first_video_result.video_id))
            print("First video name: {}".format(first_video_result.name))
            print("First video url: {}".format(first_video_result.content_url))
        else:
            print("Didn't see any video result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```

Haal trending resultaten op. Controleer de bannertegels en categorieën:
```
def video_trending(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        trending_result = client.videos.trending()
        print("Search trending video")

        # Banner tiles
        if trending_result.banner_tiles:
            first_banner_tile = trending_result.banner_tiles[0]
            print("Banner tile count: {}".format(len(trending_result.banner_tiles)))
            print("First banner tile text: {}".format(first_banner_tile.query.text))
            print("First banner tile url: {}".format(first_banner_tile.query.web_search_url))
        else:
            print("Couldn't find banner tiles!")

        # Categorires
        if trending_result.categories:
            first_category = trending_result.categories[0]
            print("Category count: {}".format(len(trending_result.categories)))
            print("First category title: {}".format(first_category.title))
            if first_category.subcategories:
                first_subcategory = first_category.subcategories[0]
                print("Subcategory count: {}".format(len(first_category.subcategories)))
                print("First subcategory title: {}".format(first_subcategory.title))
                if first_subcategory.tiles:
                    first_tile = first_subcategory.tiles[0]
                    print("Subcategory tile count: {}".format(len(first_subcategory.tiles)))
                    print("First tile text: {}".format(first_tile.query.text))
                    print("First tile url: {}".format(first_tile.query.web_search_url))
                else:
                    print("Couldn't find subcategory tiles!")
            else:
                print("Couldn't find subcategories!")
        else:
            print("Couldn't find categories!")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Zoek naar video's voor (Bellevue Trailer) en zoek vervolgens naar gedetailleerde informatie over de eerste video.
```
def video_detail(subscription_key):

    client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        video_result = client.videos.search(query="Bellevue Trailer")
        first_video_result = video_result.value[0]

        video_details = client.videos.details(
            query="Bellevue Trailer",
            id=first_video_result.video_id,
            modules=[VideoInsightModule.all]  # Can use ["all"] too
        )
        print("Search detail for video id={}, name={}".format(
            first_video_result.video_id,
            first_video_result.name
        ))

        if video_details.video_result:
            print("Expected Video id: {}".format(video_details.video_result.video_id))
            print("Expected Video name: {}".format(video_details.video_result.name))
            print("Expected Video url: {}".format(video_details.video_result.content_url))
        else:
            print("Couldn't find expected video")

        if video_details.related_videos.value:
            first_related_video = video_details.related_videos.value[0]
            print("Related video count: {}".format(len(video_details.related_videos.value)))
            print("First related video id: {}".format(first_related_video.video_id))
            print("First related video name: {}".format(first_related_video.name))
            print("First related video content url: {}".format(first_related_video.content_url))
        else:
            print("Couldn't find any related video!")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
## <a name="next-steps"></a>Volgende stappen

[Voorbeelden voor Cognitive Services Python SDK](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

