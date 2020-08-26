---
title: dosya dahil etme
description: dosya dahil etme
services: networking
author: anzaman
ms.service: VPN Gateway
ms.topic: include
ms.date: 08/25/2020
ms.author: alzam
ms.custom: include file
ms.openlocfilehash: 01796985a05f88b59786be7bbe0b06907cf0bc25
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88854077"
---
| Kaynak                                | Sınır        |
|-----------------------------------------|------------------------------|
| VNet adresi önekleri                   | VPN Gateway başına 600          |
| Toplam BGP yolları                    | VPN Gateway başına 4.000        |
| Yerel ağ geçidi adresi önekleri  | Yerel ağ geçidi başına 1000               |
| S2S bağlantıları                         | [Ağ Geçidi SKU 'suna bağımlıdır](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku)|
| P2S bağlantıları                         | [Ağ Geçidi SKU 'suna bağımlıdır](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways#gwsku) |
| P2S rota sınırı-Ikev2                 | Windows için Windows 25 için 256 **/**           |
| P2S yol sınırı-OpenVPN               | 1000                         |
| En çok, flows                              | 100  **/**  -4/az Için VpnGw1/az 512K için 100K|