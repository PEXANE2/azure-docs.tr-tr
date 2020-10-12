---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/07/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e84a77629026bb8885a48b8ed928699825f07115
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "77111200"
---
| **Satıcı** | **Cihaz ailesi** | **Üretici yazılımı sürümü** |
| --- | --- | --- |
|Cisco | ISR| IOS 15,1 (Önizleme)|
|Cisco | ASA | ASA (*) RouteBased (Ikev2-No BGP) for ASA 9,8 |
|Cisco | ASA | Asa RouteBased (Ikev2-No BGP) ASA 9.8 + |
|Juniper | SRX_GA | 12. x|
|Juniper | SSG_GA | ScreenOS 6.2. x|
|Juniper | JSeries_GA | JunOS 12. x|
|Juniper | SRX | JunOS 12. x RouteBased BGP |
|Ubiquiti| EdgeRouter| EdgeOS v 1,10 x RouteBased VTı|
|Ubiquiti| EdgeRouter| EdgeOS v 1,10 x RouteBased BGP|

> [!NOTE]
> ( * ) Gerekli: NarrowAzureTrafficSelectors (UsePolicyBasedTrafficSelectors seçeneğini etkinleştir) ve CustomAzurePolicies (ıKE/IPSec)
>
