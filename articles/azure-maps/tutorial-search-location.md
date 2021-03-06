---
title: Zoeken met Azure Maps | Microsoft Docs
description: Zoeken naar nuttige plaatsen in de buurt met behulp van Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 10/22/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 10fb30b77cc3cd18cbb6b3def9682349474fba71
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49645811"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Zoeken naar nuttige plaatsen in de buurt met behulp van Azure Maps

Deze zelfstudie laat zien hoe u een account van Azure Maps instelt en vervolgens de API's van Maps gebruikt om te zoeken naar een nuttige plaats. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Maps-account maken
> * De primaire sleutel voor uw Maps-account ophalen
> * Een nieuwe webpagina maken met de kaartbesturingselement-API
> * De zoekservice van Maps gebruiken om een nuttige plaats in de buurt te vinden

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Een account van Azure Maps maken

Voer de volgende stappen uit om een nieuw Maps-account te maken:

1. Klik in de linkerbovenhoek van [Azure Portal](https://portal.azure.com) op **Een resource maken**.
2. Typ **Maps** in het vak *Marketplace doorzoeken*.
3. Selecteer **Toewijzingen** in de *Resultaten*. Klik op de knop **Maken** die onder de kaart wordt weergegeven.
4. Voer de volgende waarden in op de pagina **Azure Kaarten-account maken**:
    * De *Naam* van uw nieuwe account.
    * Het *Abonnement* dat u wilt gebruiken voor dit account.
    * De naam van de *Resourcegroep* voor dit account. U kunt kiezen om een *Nieuwe* of *Bestaande* resourcegroep te gebruiken.
    * Selecteer een *Loacatie voor de resourcegroep*.
    * Lees de *licentie* en de *privacyverklaring*, en schakel het selectievakje in om de voorwaarden te accepteren.
    * Klik op de knop **Maken**.

    ![Maps-account maken in de portal](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>De primaire sleutel voor uw account ophalen

Als het Azure Kaarten-account is gemaakt, haalt u de sleutel op waarmee u query's kunt uitvoeren op de API's van kaarten.

1. Open uw Maps-account in de portal.
2. Selecteer **Sleutels** in de sectie Instellingen.
3. Kopieer de **Primaire Sleutel** naar het Klembord. Sla de sleutel lokaal op voor gebruik verderop in deze zelfstudie.

    ![Primaire sleutel in de portal ophalen](./media/tutorial-search-location/get-key.png)

<a id="createmap"></a>

## <a name="create-a-new-map"></a>Een nieuwe kaart maken

De Map Control-API is een handige clientbibliotheek waarmee u Maps eenvoudig kunt integreren in uw webtoepassing. Het verbergt de complexiteit van het aanroepen van de REST-service en verhoogt de productiviteit met stijlbare en aanpasbare onderdelen. Gebruik de volgende stappen voor het maken van een statische HTML-pagina, ingesloten met de Map Control-API.

1. Maak een nieuw bestand op uw lokale computer en noem dit **MapSearch.html**.
2. Voeg de volgende HTML-onderdelen toe aan het bestand:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.min.js?api-version=1"></script>

        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #map {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>

    <body>
        <div id="map"></div>
        <script>
            // Embed Map Control JavaScript code here
        </script>
    </body>

    </html>
    ```
    U ziet dat de HTML-header de CSS- en JavaScript-bronbestanden bevat, gehost door de Azure Map Control-bibliotheek. Let op het *script*-segment dat is toegevoegd aan de *hoofdtekst* van het HTML-bestand. Dit segment bevat de inline JavaScript-code voor toegang tot de API's van Azure Maps.

3. Voeg de volgende JavaScript-code toe aan het blok *script* van het HTML-bestand. Vervang de tekenreeks **\<your account key\>** door de primaire sleutel die u hebt gekopieerd uit uw Maps-account.

    ```JavaScript
    // Instantiate map to the div with id "map"
    atlas.setSubscriptionKey("<your account key>");
    var map = new atlas.Map("map");
    ```

    Dit segment initialiseert de Map Control-API voor de sleutel van uw Azure Maps-account. **Atlas** is de naamruimte die de API en gerelateerde visuele onderdelen bevat. **Atlas.Map** biedt het besturingselement voor een visuele en interactieve webkaart.

4. Sla de wijzigingen op in het bestand en open de HTML-pagina in een browser. Dit is de eenvoudigste kaart die u kunt maken door **atlas.map** aan te roepen en de sleutel van uw account op te geven.

   ![De kaart weergeven](./media/tutorial-search-location/basic-map.png)

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Zoekmogelijkheden toevoegen

In dit gedeelte ziet u hoe u met de Maps Search-API een nuttige plaats vindt op de kaart. Het is een RESTful-API die is bedoeld voor ontwikkelaars om te zoeken naar adressen, nuttige plaatsen en andere geografische informatie. De Search-service wijst een breedtegraad en lengtegraad toe aan een opgegeven adres. Met de **servicemodule** die hieronder wordt beschreven, kunt u zoeken naar een locatie met de Kaarten zoeken-API.

### <a name="service-module"></a>Servicemodule

1. Voeg een nieuwe laag toe aan uw kaart om de zoekresultaten weer te geven. Voeg de volgende Javascript-code toe aan het scriptblok, na de code waarmee de kaart wordt geïnitialiseerd.

    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    ```

2. Als u de clientservice wilt instantiëren, voegt u de volgende Javascript-code toe aan het scriptblok, na de code waarmee de kaart wordt geïnitialiseerd.

    ```JavaScript
    var client = new atlas.service.Client(MapsAccountKey);
    ```

3. Alle functies op de kaart moeten zijn geladen nadat de kaart is geladen. U kunt hiervoor zorgen door alle kaartfuncties op te nemen in het blok met de eventListener voor de kaart. Voeg de volgende regels met code toe om een [eventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) toe te voegen aan de kaart, om ervoor te zorgen dat de kaart volledig wordt geladen voordat u functies toevoegt.
    
    ```JavaScript
         map.events.add("load", function() {
         });
    ```

4. Voeg het volgende scriptblok toe **binnen de gebeurtenissen voor het laden van de kaart** om de query te bouwen. Hierin wordt de service Fuzzy zoeken gebruikt, een basiszoek-API van de Search Service. Via de service Fuzzy zoeken wordt de meeste fuzzy invoer verwerkt, zoals een combinatie van tokens voor een adres en een nuttige plaats. Er wordt gezocht naar benzinestations binnen de opgegeven radius. Het antwoord wordt vervolgens geparseerd in GeoJSON-indeling en geconverteerd naar punten, die aan de kaart worden toegevoegd als punaises. Met het laatste deel van het script worden cameragrenzen aan de kaart toegevoegd met de eigenschap [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) van de kaart.

    ```JavaScript

            // Execute a POI search query then add pins to the map for each result once a response is received
            client.search.getSearchFuzzy("gasoline station", {
            lat: 47.6292,
            lon: -122.2337,
            radius: 100000
            }).then(response => {
       
            // Parse the response into GeoJSON 
            var geojsonResponse = new atlas.service.geojson.GeoJsonSearchResponse(response);

            // Create the point features that will be added to the map as pins
            var searchPins = geojsonResponse.getGeoJsonResults().features.map(poiResult => {
               var poiPosition = [poiResult.properties.position.lon, poiResult.properties.position.lat];
               return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                name: poiResult.properties.poi.name,
                address: poiResult.properties.address.freeformAddress,
                position: poiPosition[1] + ", " + poiPosition[0]
               });
            });

            // Add pins to the map for each POI
            map.addPins(searchPins, {
               name: searchLayerName,
               cluster: false, 
               icon: "pin-round-darkblue" 
            });

            // Set the camera bounds
            map.setCameraBounds({
               bounds: geojsonResponse.getGeoJsonResults().bbox,
               padding: 50
            });
    });
    ```
5. Sla het bestand **MapSearch.html** op en vernieuw de browser. U ziet nu dat de kaart is gecentreerd rond Seattle en dat blauwe spelden de locaties van tankstations in het gebied aangeven.

   ![De kaart met zoekresultaten weergeven](./media/tutorial-search-location/pins-map.png)

6. U kunt de onbewerkte gegevens die op de kaart worden weergegeven, zien door de volgende HTTPRequest in uw browser te typen. Vervang \<your account key\> door de primaire sleutel.

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<your account key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Op dit moment kunnen op de pagina MapSearch de locaties worden weergegeven van nuttige plaatsen die worden geretourneerd door een fuzzy zoekquery. Laten we enkele interactieve mogelijkheden en meer informatie over de locaties toevoegen.

## <a name="add-interactive-data"></a>Interactieve gegevens toevoegen

De kaart die we tot nu toe hebben gemaakt, is uitsluitend gebaseerd op de gegevens voor breedtegraad en lengtegraad voor de zoekresultaten. Als u kijkt naar de onbewerkte JSON die door de service Maps Search wordt geretourneerd, ziet u echter dat deze aanvullende informatie over elk tankstation bevat, inclusief de naam en het adres. U kunt die gegevens opnemen in de kaart met behulp van interactieve pop-upvakken.

1. Voeg de volgende regels toe aan het blok *script*, om pop-ups te maken voor de nuttige plaatsen die zijn geretourneerd door de Search Service:

    ```JavaScript
    // Add a popup to the map which will display some basic information about a search result on hover over a pin
    var popup = new atlas.Popup();
    map.addEventListener("mouseover", searchLayerName, (e) => {
        var popupContentElement = document.createElement("div");
        popupContentElement.style.padding = "5px";

        var popupNameElement = document.createElement("div");
        popupNameElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupNameElement);

        var popupAddressElement = document.createElement("div");
        popupAddressElement.innerText = e.features[0].properties.address;
        popupContentElement.appendChild(popupAddressElement);

        var popupPositionElement = document.createElement("div");
        popupPositionElement.innerText = e.features[0].properties.position;
        popupContentElement.appendChild(popupPositionElement);

        popup.setPopupOptions({
            position: e.features[0].geometry.coordinates,
            content: popupContentElement
        });

        popup.open(map);
    });
    ```
    De API **atlas.Popup** voorziet in een informatievenster dat is verankerd op de juiste positie op de kaart. Met dit codefragment worden de inhoud en positie van het pop-upvenster ingesteld. Ook wordt er een gebeurtenislistener toegevoegd aan het besturingselement `map`, dat wacht totdat de _mouse_ over de pop-up beweegt.

2. Sla het bestand op en vernieuw de browser. De kaart in de browser toont nu informatie in pop-ups wanneer u de muisaanwijzer over een van de spelden beweegt.

    ![Azure Map Control en Search Service](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een account van Azure Maps maken
> * De primaire sleutel voor uw account ophalen
> * Nieuwe webpagina met Map Control API maken
> * Search Service gebruiken om nuttige plaatsen in de buurt te vinden

De voorbeeldcode voor deze zelfstudie vindt u hier:

> [Locatie zoeken met Azure Maps](https://github.com/Azure-Samples/azure-maps-samples/blob/master/src/search.html)

De volgende zelfstudie laat zien hoe u een route tussen twee locaties weergeeft.

> [!div class="nextstepaction"]
> [Route naar een bestemming](./tutorial-route-location.md)
