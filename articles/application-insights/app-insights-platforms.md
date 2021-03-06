---
title: 'Application Insights: programmeertalen, platforms en integraties | Microsoft Docs'
description: De programmeertalen, platforms en integraties die beschikbaar zijn voor Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 09/01/2016
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 4f474ad234c80a0dcb5a9f704a263a97e7df0cc1
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174365"
---
# <a name="developer-analytics-languages-platforms-and-integrations"></a>Analyses voor ontwikkelaars: programmeertalen, platforms en integraties
Deze items zijn implementaties van [Application Insights](app-insights-overview.md) die tot nu toe bij ons bekend zijn (inclusief implementaties door derden).

## <a name="languages---officially-supported-by-application-insights-team"></a>Programmeertalen - officieel ondersteund door het team van Application Insights
* [C#|VB (.NET)](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [JavaScript-webpagina's](app-insights-javascript.md)
* [Node.JS](app-insights-nodejs.md)

## <a name="languages---community-supported"></a>Programmeertalen - ondersteund door de community
* [F#](https://safe-stack.github.io/docs/template-azure-ai/)
* [PHP](https://github.com/Microsoft/ApplicationInsights-PHP)
* [Python](https://pypi.python.org/pypi/applicationinsights/0.1.0)
* [Ruby](https://rubygems.org/gems/application_insights)
* [Overige](#projects)

## <a name="platforms-and-frameworks"></a>Platforms en frameworks
* [ASP.NET](app-insights-asp-net.md)
* [ASP.NET - voor apps die al live zijn](app-insights-monitor-performance-live-website-now.md)
* [ASP.NET Core](app-insights-asp-net-core.md)
* [Android](app-insights-mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [Angular](https://github.com/MarkPieszak/angular-application-insights)
* [Azure Web Apps](app-insights-azure-web-apps.md)
* [Azure Cloud Services](app-insights-cloudservices.md) met inbegrip van web- en werkrollen
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample)
* [Docker](app-insights-docker.md)
* [Glimpse](https://azure.microsoft.com/blog/glimpse-application-insights/)
* [iOS](app-insights-mobile-center-quickstart.md) (App Center)
* [Ionic](https://github.com/SoftwarePioniere/ionic-application-insights)
* [iOS](https://github.com/Microsoft/ApplicationInsights-iOS) (App Center)
* [J2EE](app-insights-java-get-started.md)
* [J2EE - voor apps die al live zijn](app-insights-java-live.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [OSX](https://github.com/Microsoft/ApplicationInsights-OSX)
* [SAFE Stack](https://safe-stack.github.io/docs/template-azure-ai/)
* [Spring](http://joe.blog.freemansoft.com/2015/12/enabling-microsoft-application-insight.html)
* [Universele Windows-app](app-insights-mobile-center-quickstart.md) (App Center)
* [WCF](https://github.com/Microsoft/ApplicationInsights-SDK-Labs/blob/master/WCF/readme.md)
* [Windows-bureaubladtoepassingen, -services en -werkrollen](app-insights-windows-desktop.md)
* [Overige](#projects)

## <a name="logging-frameworks"></a>Frameworks voor logboekregistratie
* [Log4Net, NLog of System.Diagnostics.Trace](app-insights-asp-net-trace-logs.md)
* [Java, Log4J of Logback](app-insights-java-trace-logs.md)
* [Semantische logboekregistratie (SLAB)](https://github.com/fidmor89/SLAB_AppInsights): kan worden geïntegreerd met het [Semantic Logging Application Block](https://msdn.microsoft.com/library/dn440729.aspx)
* [Belastingstests in de cloud](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/30/getting-application-insights-counters-with-cloud-based-load-testing.aspx)
* [LogStash-invoegtoepassing](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)
* [Logary](https://www.nuget.org/packages/Logary.Targets.AppInsights/)
* [Logrus](https://github.com/jjcollinge/logrus-appinsights)

## <a name="content-management-systems"></a>Content Management Systems (CMS)
* [Concrete](https://github.com/fidmor89/appInsights-Concrete)
* [Drupal](https://github.com/fidmor89/AppInsights-Drupal)
* [Joomla](https://github.com/fidmor89/AppInsights-Joomla)
* [Orchard](https://azure.microsoft.com/blog/integrating-application-insights-into-a-modular-cms-and-a-multi-tenant-public-saas/preview/)
* [SharePoint](app-insights-sharepoint.md)
* [WordPress](https://wordpress.org/plugins/application-insights/)

## <a name="export-and-data-analysis"></a>Exporteren en gegevensanalyse
* [Alooma](https://www.alooma.com/blog/application-insights-amazon-redshift)
* [Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](app-insights-export-power-bi.md)

## <a name="projects"></a> Uw eigen SDK bouwen
Als er nog geen SDK beschikbaar is voor uw programmeertaal of platform, kunt u er ook zelf een bouwen. Bekijk de code van de bestaande SDK's die worden beschreven in het [Application Insights SDK-project op GitHub](https://github.com/Microsoft/AppInsights-Home).
