---
title: Wat is toegangsbeheer op basis van rollen in Azure? | Microsoft Docs
description: Bekijk een overzicht van toegangsbeheer op basis van rollen in Azure. Gebruik roltoewijzingen om toegang tot resources in Azure te beheren.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: role-based-access-control
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 37498394bc163852d397337cf5728b4941ae45a7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956503"
---
# <a name="what-is-role-based-access-control-rbac"></a>Wat is toegangsbeheer op basis van rollen?

Toegangsbeheer voor cloudresources is een uiterst belangrijke functie voor elke organisatie die van de cloud gebruikmaakt. Het gebruik van op rollen gebaseerd toegangsbeheer helpt u bij het beheren van wie er toegang heeft tot Azure-resources, wat ze kunnen doen met die resources en tot welke gebieden ze toegang hebben.

Op rollen gebaseerd toegangsbeheer is een machtigingssysteem dat is gebouwd op [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) dat een geavanceerd toegangsbeheer van resources in Azure biedt.

## <a name="what-can-i-do-with-rbac"></a>Wat kan ik doen met op rollen gebaseerd toegangsbeheer?

Hier volgen enkele voorbeelden van wat u met op rollen gebaseerd toegangsbeheer kunt doen:

- Toestaan dat één gebruiker de virtuele machines in een abonnement kan beheren en een andere gebruiker de virtuele netwerken kan beheren.
- Een DBA-groep toestaan de SQL-databases in een abonnement te beheren.
- Toestaan dat een gebruiker alle resources in een resourcegroep kan beheren, zoals virtuele machines, websites en subnetten.
- Toestaan dat een toepassing toegang heeft tot alle resources in een resourcegroep.

## <a name="best-practice-for-using-rbac"></a>Aanbevolen procedures voor het gebruik van RBAC

Met op rollen gebaseerd toegangsbeheer kunt u taken scheiden binnen uw team en alleen de mate van toegang verlenen aan gebruikers die nodig is om de taken uit te voeren. In plaats van iedereen onbeperkte machtigingen te verlenen in uw Azure-abonnement of -resources, kunt u zelf bepalen welke acties er met een bepaalde bereik zijn toegestaan.

Het is een aanbevolen procedure om tijdens het plannen van een strategie voor toegangsbeheer gebruikers minimale bevoegdheden te verlenen om hun werk gedaan te krijgen. In het volgende diagram wordt een voorgesteld patroon weergegeven voor het gebruik van RBAC.

![RBAC en minimale bevoegdheden](./media/overview/rbac-least-privilege.png)

## <a name="how-rbac-works"></a>Hoe op rollen gebaseerd toegangsbeheer werkt

De manier waarop u de toegang tot resources beheert met behulp van op rollen gebaseerd toegangsbeheer is door roltoewijzingen te maken. Dit is een belangrijke concept om te weten: het is de manier waarop machtigingen worden afgedwongen. Een roltoewijzing bestaat uit drie elementen: beveiligings-principal, roldefinitie en bereik (ook wel scope of niveau genoemd).

### <a name="security-principal"></a>Beveiligings-principal

Een *beveiligings-principal* is een object dat een gebruiker, groep of service-principal vertegenwoordigt die toegang tot Azure-resources aanvraagt.

![Beveiligings-principal voor een roltoewijzing](./media/overview/rbac-security-principal.png)

- Gebruiker: een persoon een profiel heeft in Azure Active Directory. U kunt ook rollen toewijzen aan gebruikers in andere tenants. Zie [Azure Active Directory B2B](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) voor meer informatie over gebruikers in andere organisaties.
- Groep: een aantal gebruikers dat in Azure Active Directory is gemaakt. Wanneer u een rol aan een groep toewijst, hebben alle gebruikers binnen die groep die rol. 
- Service-principal: een beveiligings-id die wordt gebruikt door toepassingen of services om toegang tot specifieke Azure-resources te krijgen. U kunt het zien als een *gebruikersidentiteit* (gebruikersnaam en wachtwoord of certificaat) voor een toepassing.

### <a name="role-definition"></a>Roldefinitie

Een *roldefinitie* is een verzameling machtigingen. Dit wordt soms gewoon een *rol* genoemd. Een roldefinitie beschijft de bewerkingen die kunnen worden uitgevoerd, zoals lezen, schrijven en verwijderen. Rollen kunnen op algemeen zijn, zoals eigenaar, of specifiek, zoals de lezer van de virtuele machine.

![Roldefinitie voor een roltoewijzing](./media/overview/rbac-role-definition.png)

Azure bevat diverse [ingebouwde rollen](built-in-roles.md) die u kunt gebruiken. Hier volgen vier fundamentele ingebouwde rollen. De eerste drie zijn op alle resourcetypen van toepassing.

- [Eigenaar](built-in-roles.md#owner): heeft volledige toegang tot alle resources, waaronder het recht om toegang aan anderen te delegeren.
- [Inzender](built-in-roles.md#contributor): kan alle typen Azure-resources maken en beheren, maar kan anderen geen toegang verlenen.
- [Lezer](built-in-roles.md#reader): kan bestaande Azure-resources bekijken.
- [Beheerder gebruikerstoegang](built-in-roles.md#user-access-administrator): kan gebruikerstoegang tot Azure-resources beheren.

Met de overige ingebouwde rollen kunnen specifieke Azure-resources worden beheerd. Met de rol [Inzender voor virtuele machines](built-in-roles.md#virtual-machine-contributor) kan een gebruiker bijvoorbeeld virtuele machines maken en beheren. Als de ingebouwde rollen niet voldoen aan de specifieke behoeften van uw organisatie, kunt u uw eigen [aangepaste rollen](custom-roles.md) maken.

Azure heeft gegevensbewerkingen (momenteel in Preview-versie) geïntroduceerd waarmee u toegang tot gegevens in een object kunt verlenen. Als een gebruiker bijvoorbeeld toegang heeft tot gegevens in een opslagaccount, kan deze de blobs of berichten in dat opslagaccount lezen. Zie [Roldefinities begrijpen](role-definitions.md) voor meer informatie.

### <a name="scope"></a>Bereik

*Bereik* is de grens die waarop de toegang van toepassing is. Wanneer u een rol toewijst, kunt u de acties die zijn toegestaan verder beperken door een bereik te definiëren. Dit is handig als u van iemand een [Inzender voor websites](built-in-roles.md#website-contributor) wilt maken, maar slechts voor één resourcegroep.

In Azure kunt u een bereik op meerdere niveaus opgeven: [beheergroep](../azure-resource-manager/management-groups-overview.md), abonnement, resourcegroep of resource. Bereiken zijn gestructureerd in een bovenliggende/onderliggende relatie.

![Bereik voor een roltoewijzing](./media/overview/rbac-scope.png)

Wanneer u toegang op een bovenliggend bereik verleent, worden deze machtigingen overgenomen door de onderliggende bereiken. Bijvoorbeeld:

- Als u de [Eigenaar](built-in-roles.md#owner)-rol aan een gebruiker bij het bereik van de beheergroep toewijst, kan die gebruiker alles in alle abonnementen in de beheergroep beheren.
- Als u de rol van [lezer](built-in-roles.md#reader) toewijst aan een groep in het bereik van het abonnement, kunnen de leden van die groep elke resourcegroep en de resource in het abonnement bekijken.
- Als u de rol van [inzender](built-in-roles.md#contributor) toewijst aan een toepassing in het bereik van de resource, kunnen hiermee resources van alle typen in die resourcegroep worden beheerd, maar geen andere resourcegroepen in het abonnement.

### <a name="role-assignments"></a>Roltoewijzingen

Een *roltoewijzing* is het proces waarmee voor een bepaald bereik een roldefinitie aan een gebruiker, groep of service-principal wordt gekoppeld, met het doel om toegang te verlenen. Toegang wordt verleend door een roltoewijzing te maken en toegang kan worden ingetrokken door een roltoewijzing te verwijderen.

Het volgende diagram toont een voorbeeld van een roltoewijzing. In dit voorbeeld wordt aan de groep Marketing de rol van [inzender](built-in-roles.md#contributor) toegewezen voor de resourcegroep pharma-sales. Dit betekent dat gebruikers in de groep Marketing een Azure-resource in de resourcegroep pharma-sales kunnen maken of beheren. Gebruikers in de groep Marketing hebben geen toegang tot resources buiten de resourcegroep pharma-sales, tenzij ze deel uitmaken van een andere roltoewijzing.

![Roltoewijzing om toegang te beheren](./media/overview/rbac-overview.png)

U kunt roltoewijzingen maken met behulp van Azure Portal, Azure CLI, Azure PowerShell, Azure-SDK's of REST-API's. U kunt maximaal 2000 roltoewijzingen in elk abonnement hebben. Om roltoewijzingen te maken en te verwijderen, moet u een `Microsoft.Authorization/roleAssignments/*`-machtiging hebben. Deze machtiging wordt verleend via de rol van [eigenaar](built-in-roles.md#owner) of [administrator](built-in-roles.md#user-access-administrator).

## <a name="deny-assignments"></a>Weigeringstoewijzingen

Voorheen was RBAC een model op basis van alleen-toestaan zonder de mogelijkheid tot weigeren, maar nu ondersteunt RBAC in beperkte mate weigeringstoewijzingen. Ongeveer op dezelfde manier als een roltoewijzing verbindt een *weigeringstoewijzing* een reeks acties aan een gebruiker, groep of service-principal met een bepaald bereik met het doel toegang te weigeren. Een roltoewijzing definieert een set acties die zijn *toegestaan*, terwijl een weigeringstoewijzing een set acties definieert die *niet zijn toegestaan*. Met andere woorden, weigeringstoewijzingen voorkomen dat gebruikers opgegeven acties uitvoeren, zelfs als een roltoewijzing hen deze toegang verleent. Weigeringstoewijzingen hebben voorrang op roltoewijzingen.

Op dit moment zijn weigeringstoewijzingen **alleen-lezen** en kunnen deze alleen worden ingesteld door Azure. U kunt geen eigen weigeringstoewijzingen maken, maar u kunt wel een lijst met weigeringstoewijzingen weergeven omdat deze invloed kunnen hebben op uw effectieve machtigingen. Om informatie te verkrijgen over een weigeringstoewijzing moet u de machtiging `Microsoft.Authorization/denyAssignments/read` hebben, die is opgenomen in de meeste [ingebouwde rollen](built-in-roles.md#owner). Zie [Inzicht in weigeringstoewijzingen](deny-assignments.md) voor meer informatie.

## <a name="how-rbac-determines-if-a-user-has-access-to-a-resource"></a>Hoe RBAC bepaalt of een gebruiker toegang tot een resource heeft

Hier volgen de stappen op hoog niveau die RBAC gebruikt om te bepalen of u toegang tot een resource hebt op het beheervlak. Het is handig om dit te begrijpen als u probeert een toegangsprobleem op te lossen.

1. Een gebruiker (of service-principal) krijgt een token voor Azure Resource Manager.

    Het token bevat groepslidmaatschappen van de gebruiker (met inbegrip van overdraagbare groepslidmaatschappen).

1. De gebruiker voert een REST API-aanroep uit naar Azure Resource Manager met het gekoppelde token.

1. Azure Resource Manager haalt alle roltoewijzingen en weigeringstoewijzingen op die betrekking hebben op de resource waarop de actie wordt ondernomen.

1. Azure Resource Manager beperkt de roltoewijzingen die van toepassing zijn op deze gebruiker of hun groep en bepaalt welke rollen de gebruiker heeft voor deze resource.

1. Azure Resource Manager bepaalt of de actie in de API-aanroep is opgenomen in de rollen die de gebruiker voor deze resource heeft.

1. Als de gebruiker geen rol met de actie heeft in het aangevraagde bereik, wordt er geen toegang verleend. Anders controleert Azure Resource Manager of er een weigeringstoewijzing van toepassing is.

1. Als de weigeringstoewijzing van toepassing is, wordt toegang geblokkeerd. Anders wordt toegang verleend.

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: Grant access for a user using RBAC and the Azure portal](quickstart-assign-role-user-portal.md) (Snelstart: Toegang verlenen aan een gebruiker met op rollen gebaseerd toegangsbeheer en Azure Portal)
- [Toegang beheren met op rollen gebaseerd toegangsbeheer en Azure Portal](role-assignments-portal.md)
- [Inzicht in de verschillende rollen](rbac-and-directory-admin-roles.md)
