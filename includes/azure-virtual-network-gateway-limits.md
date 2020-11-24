---
title: include dosyası
description: include dosyası
services: networking
author: anzaman
ms.service: VPN Gateway
ms.topic: include
ms.date: 08/25/2020
ms.author: alzam
ms.custom: include file
ms.openlocfilehash: 9fe9ef5549ced3b73d18d553fa0b62ec019684fe
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95561135"
---
| Kaynak                                | Sınır        |
|-----------------------------------------|------------------------------|
| VNet adresi önekleri                   | VPN Gateway başına 600          |
| Toplam BGP yolları                    | VPN Gateway başına 4.000        |
| Yerel ağ geçidi adresi önekleri  | Yerel ağ geçidi başına 1000               |
| S2S bağlantıları                         | [Ağ Geçidi SKU 'suna bağımlıdır](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku)|
| P2S bağlantıları                         | [Ağ Geçidi SKU 'suna bağımlıdır](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) |
| P2S rota sınırı-Ikev2                 | Windows için Windows 25 için 256 **/**           |
| P2S yol sınırı-OpenVPN               | 1000                         |
| En çok, flows                              | 100  **/**  -4/az Için VpnGw1/az 512K için 100K|