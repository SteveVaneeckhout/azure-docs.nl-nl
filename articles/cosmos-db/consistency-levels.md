---
title: Consistentieniveaus in Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB biedt vijf consistentieniveaus te verdelen over uiteindelijke consistentie, beschikbaarheid en latentie-en nadelen.
keywords: uiteindelijke consistentie, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5cb439f7fe6461fcef0d010535179e16e28c294a
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50239166"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Consistentieniveaus in Azure Cosmos DB

Gedistribueerde databases die afhankelijk zijn van replicatie voor hoge beschikbaarheid, lage latentie of beide, moet het fundamentele verschil tussen het lezen van consistentie en beschikbaarheid, latentie en doorvoer. De meeste commercieel verkrijgbaar gedistribueerde databases stellen ontwikkelaars om te kiezen tussen de twee extreme consistentiemodellen: sterke consistentie en uiteindelijke consistentie. Terwijl de [verwerkingen](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) of het model sterke consistentie is de standaard gold van gegevens programmeerbaarheid, wordt geleverd tegen een prijs zonder een straffe van hogere latentie (in onveranderlijke) en tot verminderde beschikbaarheid (bij storingen). Aan de andere kant uiteindelijke consistentie biedt een hogere beschikbaarheid en betere prestaties, maar is zeer moeilijk is voor het programmeren in.

Cosmos DB nadert de consistentie van gegevens als een spectrum van de opties in plaats van de twee extreme. Zijn de twee uiteinden van het spectrum sterke consistentie en uiteindelijke consistentie, maar er zijn veel keuzes in consistentie langs de spectrum van de consistentie. Deze opties voor consistentiecontrole kunnen ontwikkelaars nauwkeurige keuzes en weer specifieker compromissen met betrekking tot hoge beschikbaarheid of prestaties. Cosmos DB kan ontwikkelaars kiezen tussen vijf duidelijk gedefinieerde consistentiemodellen uit het spectrum consistentie (sterk naar zwak) – **sterke**, **gebonden veroudering**, **sessie** , **consistent voorvoegsel**, en **uiteindelijke**. Elk van deze consistentiemodellen is goed gedefinieerde, intuïtieve en kan worden gebruikt voor specifieke praktijkscenario's. Elk van de vijf consistentiemodellen wissen optimalisatie van de beschikbaarheid en prestaties leveren en met uitgebreide Sla's worden ondersteund.

![Consistentie is een spectrum](./media/consistency-levels/five-consistency-levels.png)
**consistentie is een spectrum**

Houd er rekening mee dat de consistentieniveaus regioneutrale zijn. Consistentie niveau (en de bijbehorende garanties voor consistentie) van uw Cosmos DB-account kan worden gegarandeerd voor alle leesbewerkingen, ongeacht het volgende:

- De regio van waaruit de lees- en schrijfbewerkingen worden behandeld
- Het aantal regio's die zijn gekoppeld aan uw Cosmos-account
- Of uw account is geconfigureerd met één of meerdere regio's voor schrijven

## <a name="scope-of-the-read-consistency"></a>Bereik van de consistentie van de lezen

De lezen-consistentie is van toepassing op één leesbewerking binnen het bereik van binnen het bereik van de partitiesleutel (ook een logische partitie). De leesbewerking kan worden uitgegeven door een externe client of een opgeslagen procedure.

## <a name="configuring-the-default-consistency-level"></a>Het standaardconsistentieniveau configureren

U kunt configureren dat de **consistentieniveau standaard** op uw Cosmos DB-account op elk gewenst moment. Het standaardconsistentieniveau geconfigureerd in uw account is van toepassing op alle Cosmos-databases (en containers) onder dat account. Alle leesbewerkingen en query's die zijn uitgegeven voor een container of een database die consistentieniveau gebruikt standaard. Hier ziet voor informatie over het [het standaardconsistentieniveau configureren](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garanties die zijn gekoppeld aan consistentieniveaus

De uitgebreide Sla's geleverd door Azure Cosmos DB garandeert dat 100% van de leesaanvragen voldoet aan de consistentiegarantie voor elk consistentieniveau die u kiest. Een aanvraag wordt beschouwd als om te voldoen aan de consistentie SLA, als alle de consistentiegarantie die zijn gekoppeld aan het consistentieniveau is voldaan. De precieze definities van de vijf consistentieniveaus in Cosmos DB met behulp van de [TLA + specificatietaal (TLA +)](http://lamport.azurewebsites.net/tla/tla.html) vindt u in de [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla) GitHub-opslagplaats. De semantiek van de vijf consistentieniveaus worden hieronder beschreven:

- **Consistentieniveau = 'sterk'**: sterke consistentie biedt een [verwerkingen](https://aphyr.com/posts/313-strong-consistency-models) garanderen, met de leesbewerkingen gegarandeerd de meest recente doorgevoerde versie van een item. Een client kan nooit zien voor het terugschrijven van een niet-doorgevoerde of gedeeltelijke en is altijd gegarandeerd de meest recente toegezegde schrijven.
- **Consistentieniveau = "gebonden veroudering"**: de leesbewerkingen gegarandeerd de garantie consistent voorvoegsel in acht neemt. De leesbewerkingen kunnen volgen op schrijfbewerkingen door maximaal K versies of voorvoegsels (dat wil zeggen, updates) van een artikel of 't'-tijdsinterval. Daarom bij het kiezen van gebonden veroudering, de 'veroudering' kan worden geconfigureerd op twee manieren: aantal versies (kB) van het item of het tijdsinterval (t) waarmee de leesbewerkingen schrijfbewerkingen kunnen volgen. Gebonden veroudering aanbiedingen totale globale volgorde, behalve binnen de "veroudering venster." De monotone lezen garanties bestaan binnen een regio binnen en buiten de "veroudering venster." Sterke consistentie is dezelfde semantiek als die van de gebonden veroudering, maar met de "veroudering venster" gelijk is aan nul. Gebonden veroudering wordt ook wel 'verwerkingen tijd uitgesteld' genoemd. Wanneer een client leesbewerkingen binnen een regio voor het accepteren van de schrijfbewerkingen uitvoert, zijn de garanties geboden door consistentie voor gebonden veroudering identiek zijn aan die met de krachtige consistentie.
- **Consistentieniveau = "sessie"**: de leesbewerkingen gegarandeerd te voldoen aan het consistent voorvoegsel, monotone leesbewerkingen, monotone schrijfbewerkingen, read-your-writes, write-follows-reads garandeert. Sessieconsistentie is afgestemd op een clientsessie.
- **Consistentieniveau = 'consistent prefix'**: geretourneerde Updates zijn een prefix van alle updates, zonder hiaten. Consistent voorvoegsel wordt gegarandeerd dat leesbewerkingen nooit niet-geordende schrijfbewerkingen te zien.
- **Consistentieniveau = "uiteindelijke"**: Er is geen garantie volgorde voor leesbewerkingen. In de afwezigheid van geen schrijfbewerkingen meer, worden de replica's uiteindelijk geconvergeerd.

## <a name="consistency-levels-explained-through-baseball"></a>Consistentieniveaus uitgelegd honkbal

Als de [consistentie van gegevens via Baseball gerepliceerd](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) document ziet u, een reeks schrijfbewerkingen imagine voor de score van een game baseball met de regel inning door inning score. Dit spel hypothetische baseball is momenteel in het midden van de zevende inning (de proverbial zevende--inning stretch), en de home-team is het verschil maken 2-5.

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Wordt uitgevoerd** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Bezoekers** | 0 | 0 | 1 | 0 | 5 | 0 | 0 |  |  | 2 |
| **startpagina** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Een container van Cosmos DB bevat de bezoekers en thuis team uitvoeren totalen. Terwijl het spel uitgevoerd wordt, lezen verschillende garanties kunnen leiden tot clients verschillende scores lezen. De volgende tabel bevat de volledige set van scores die kunnen worden geretourneerd door de bezoekers en thuis scores met elk van de vijf consistentiegarantie lezen. Houd er rekening mee dat de bezoekers score eerst weergegeven wordt en verschillende mogelijke geretourneerde waarden worden gescheiden door komma's.

| **Consistentieniveau** | **Scores** |
| - | - |
| **Sterke** | 2-5 |
| **Gebonden veroudering** | scores die zijn op de meeste één inning verouderde"2-3, 2-4, 2-5 |
| **Sessie** | <ul><li>voor de schrijver"2-5</li><li> voor iedereen behalve de writer: 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2 en 3, 2-4, 2-5</li><li>na het lezen van 1-3: 1-3, 1-4, 1-5, 2 en 3, 2-4, 2-5</li> |
| **Consistent Prefix** | 0-0, 0-1, 1-1, 1-2, 1-3, 2 en 3, 2-4, 2-5 |
| **Uiteindelijke** | 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2 en 3, 2-4, 2-5 |

## <a name="additional-reading"></a>Meer lezen

Lees voor meer informatie over concepten van de consistentie van de volgende artikelen:

- [Op hoog niveau TLA + specificaties voor de vijf consistentieniveaus die worden aangeboden door Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla)
- [Gerepliceerde gegevens consistentie uitgelegd honkbal (video) door Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Gerepliceerde gegevens consistentie uitgelegd honkbal (technisch document) door Doug Terry](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Sessie-garanties voor zwak consistente gerepliceerde gegevens](https://dl.acm.org/citation.cfm?id=383631)
- [Optimalisatie van de consistentie in het ontwerp van moderne systemen voor gedistribueerde Database: LIMIET is slechts een deel van het artikel](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
- [Probabilistic gebonden veroudering (PBS) voor praktische gedeeltelijke quorum](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Uiteindelijk Consistent - herzien](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Volgende stappen

Lees de volgende artikelen voor meer informatie over consistentieniveaus in Cosmos DB:

* [De juiste consistentieniveau voor uw toepassing kiezen](consistency-levels-choosing.md)
* [Consistentieniveaus voor Cosmos DB-API 's](consistency-levels-across-apis.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Het standaardconsistentieniveau configureren](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Het onderdrukken van het standaardconsistentieniveau](how-to-manage-consistency.md#override-the-default-consistency-level)

