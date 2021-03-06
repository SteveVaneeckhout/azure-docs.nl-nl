---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/29/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 03af5efcd4a37203a82db503f8bc602b33de734d
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226644"
---
|**SKU**   | **S2S-/VNet-naar-VNet-<br>tunnels** | **P2S<br> SSTP-verbindingen** | **P2S<br> IKEv2-verbindingen** | **Benchmark cumulatieve<br>doorvoer** |
|---       | ---        | ---       | ---            | ---       |
|**VpnGw1**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 250       | 650 Mbps  |
|**VpnGw2**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 500       | 1 Gbps    |
|**VpnGw3**| Met maximaal 30*   | Met maximaal 128  | Met maximaal 1000      | 1,25 Gbps |
|**Basic** | Met maximaal 10    | Met maximaal 128  | Niet ondersteund  | 100 Mbps  | 

(\*) Gebruik [Virtual WAN](../articles/virtual-wan/virtual-wan-about.md) als u meer dan 30 S2S VPN-tunnels nodig hebt.

* De Benchmark cumulatieve doorvoer voor een VPN-gateway is gebaseerd op de combinatie S2S + P2S. **Als u veel P2S-verbindingen gebruikt, kan dit in verband met doorvoerbeperkingen een negatieve invloed hebben op S2S-verbindingen.** De Benchmark cumulatieve doorvoer is gebaseerd op de metingen van meerdere tunnels die via één gateway worden gecombineerd. Het is geen gegarandeerde doorvoer vanwege de omstandigheden van internetverkeer en het gedrag van uw toepassing.

* Er gelden afzonderlijke verbindingslimieten. U kunt bijvoorbeeld 128 SSTP-verbindingen en ook 250 IKEv2-verbindingen hebben in een SKU van het type VpnGw1.

* Prijsinformatie vindt u op de pagina [Prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway).

* Gegevens over de SLA (Service Level Agreement) vindt u op de pagina [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* VpnGw1, VpnGw2 en VpnGw3 worden alleen ondersteund voor VPN-gateways die gebruikmaken van het Resource Manager-implementatiemodel.
