---
title: Een overzicht voor de ontwikkelaars voor Azure-Database voor MySQL database
description: Overwegingen bij het ontwerpen die een ontwikkelaar volgen moet bij het schrijven van code van de toepassing verbinding maken met Azure-Database voor MySQL introduceert
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: b733468d41afacb616c95f0628e7bad6b0c837f0
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264160"
---
# <a name="application-development-overview-for-azure-database-for-mysql"></a>Overzicht van toepassing voor ontwikkelaars voor Azure-Database voor MySQL 
Dit artikel wordt beschreven ontwerpoverwegingen die een ontwikkelaar bij het schrijven van code van de toepassing verbinding maken met Azure-Database voor MySQL moet volgen. 

> [!TIP]
> Zie voor een zelfstudie waarin u het maken van een server, maken van een firewall op de server, servereigenschappen weergeven, database maken en verbinding maken en query uitvoeren met behulp van workbench en mysql.exe [ontwerpen van uw eerste Azure-Database voor de MySQL-database](tutorial-design-database-using-portal.md)

## <a name="language-and-platform"></a>Taal en platform
Er zijn codevoorbeelden beschikbaar voor verschillende programmeertalen en platforms. Vindt u koppelingen naar de codevoorbeelden op: [bibliotheken voor databaseconnectiviteit verbinding maken met Azure Database MySQL gebruikt](concepts-connection-libraries.md)

## <a name="tools"></a>Hulpprogramma's
Azure MySQL-Database maakt gebruik van de MySQL-community-versie, compatibel zijn met algemene beheerprogramma's zoals Workbench of MySQL hulpprogramma's zoals mysql.exe, MySQL [phpMyAdmin](https://www.phpmyadmin.net/), [Navicat](https://www.navicat.com/products/navicat-for-mysql), en andere. U kunt ook de Azure-portal, Azure CLI en REST-API's gebruiken om te communiceren met de database-service.

## <a name="resource-limitations"></a>Resourcebeperkingen
Azure MySQL-Database beheert de beschikbare bronnen voor een server met behulp van twee verschillende mechanismen: 
- Beheer van bronnen.
- Afdwinging van limieten.

## <a name="security"></a>Beveiliging
Azure MySQL-Database bevat bronnen voor beperkende toegang, beschermen gegevens configureren gebruikers en rollen en bewakingsactiviteiten op een MySQL-database.

## <a name="authentication"></a>Verificatie
Azure MySQL-Database biedt ondersteuning voor server-verificatie van gebruikers en aanmeldingen.

## <a name="resiliency"></a>Flexibiliteit
Wanneer een tijdelijke fout optreedt bij het verbinden met een MySQL-database, moet de aanroep opnieuw proberen uw code. Wordt aangeraden de Pogingslogica gebruiken uit logica zodat deze heeft niet de SQL-database overbelast met meerdere clients tegelijkertijd opnieuw uit te voeren.

- Codevoorbeelden: codevoorbeelden die aangeven Pogingslogica, vindt u voorbeelden voor de taal van uw keuze op: [bibliotheken voor databaseconnectiviteit verbinding maken met Azure Database MySQL gebruikt](concepts-connection-libraries.md)

## <a name="managing-connections"></a>Verbindingen beheren
Databaseverbindingen zijn een beperkte resource daarom logisch verbindingen worden gebruikt aangeraden bij het openen van uw MySQL-database voor betere prestaties bereiken.
- Toegang tot de database met behulp van verbindingsgroepering of permanente verbindingen.
- Toegang tot de database met behulp van de levensduur van een korte verbinding. 
- Pogingslogica voor gebruik in uw toepassing op het moment van de verbindingspoging om af te vangen fouten ten gevolge van gelijktijdige verbindingen het maximaal toegestane aantal is bereikt. In de Pogingslogica een korte vertraging instellen en vervolgens wachten op een willekeurige tijd voordat de aanvullende verbindingspogingen.