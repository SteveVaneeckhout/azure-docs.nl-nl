---
title: Het configureren van een toepassing toepassingsproxy | Microsoft Docs
description: Informatie over het maken van een configureren een APplication Proxy-toepassing in een paar eenvoudige stappen
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: cf3e367dad528017a98e103962c57cb758da55cb
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356492"
---
# <a name="how-to-configure-an-application-proxy-application"></a>Een toepassing Application Proxy configureren

In dit artikel kunt u meer informatie over het configureren van een Application Proxy-toepassing in Azure AD om beschikbaar te stellen van uw on-premises toepassingen naar de cloud.

## <a name="recommended-documents"></a>Aanbevolen documenten 

Voor meer informatie over de initiële configuraties en het maken van een Application Proxy-toepassing via de beheerportal, volgt u de [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md).

Zie voor meer informatie over het configureren van Connectors [toepassingsproxy inschakelen in Azure portal](application-proxy-enable.md).

Zie voor meer informatie over certificaten uploaden en gebruiken van aangepaste domeinen [werken met aangepaste domeinen in Azure AD-toepassingsproxy](application-proxy-configure-custom-domain.md).

## <a name="create-the-applicationsetting-the-urls"></a>De URL's voor de toepassingsinstelling maken

Als u de stappen in de [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md) documentatie en zijn er een fout optreedt bij het maken van de toepassing, Zie de foutdetails voor meer informatie en suggesties voor het oplossen van de de toepassing. De meeste foutberichten bevatten een voorgestelde oplossing. Algemene om fouten te voorkomen, controleert u of:

-   U bent een beheerder met de machtiging voor het maken van een toepassing Application Proxy

-   De interne URL is uniek

-   De externe URL is uniek

-   De URL's met http of https beginnen en eindigen met een '/'

-   De URL moet de domeinnaam van een, niet een IP-adres

Het foutbericht moet worden weergegeven in de rechterbovenhoek bij het maken van de toepassing. U kunt ook het meldingspictogram om te zien van de foutberichten selecteren.

   ![Melding prompt](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Configureren van connectors/connectorgroepen

Als u problemen bij het configureren van uw toepassing vanwege de waarschuwing over connectors en connectorgroepen ondervindt, er instructies weergegeven voor de toepassingsproxy wordt ingeschakeld voor meer informatie over het downloaden van connectors. Als u meer informatie over connectors wilt, raadpleegt u de [connectors documentatie](application-proxy-connectors.md).

Als uw connectors niet actief zijn, betekent dit dat deze kan niet worden bereikt van de service zijn. Dit is vaak omdat niet alle vereiste poorten geopend zijn. Een lijst met vereiste poorten, Zie de sectie vereisten van de documentatie van de toepassingsproxy inschakelen.

## <a name="upload-certificates-for-custom-domains"></a>Certificaten voor aangepaste domeinen uploaden

Aangepaste domeinen kunnen u het domein van de externe URL's opgeven. Voor het gebruik van aangepaste domeinen, moet u het certificaat voor dat domein uploaden. Zie voor meer informatie over het gebruik van aangepaste domeinen en certificaten [werken met aangepaste domeinen in Azure AD-toepassingsproxy](application-proxy-configure-custom-domain.md). 

Als u hebt met het uploaden van uw certificaat problemen, zoekt u de foutberichten in de portal voor meer informatie over het probleem met het certificaat. Veelvoorkomende problemen met de certificaten zijn onder andere:

-   Een verlopen certificaat

-   Certificaat is zelfondertekend

-   Certificaat ontbreekt de persoonlijke sleutel

Het foutbericht weergegeven in de rechterbovenhoek als u probeert om het certificaat te uploaden. U kunt ook het meldingspictogram om te zien van de foutberichten selecteren.

   ![Melding prompt](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a>Volgende stappen
[Toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md)
