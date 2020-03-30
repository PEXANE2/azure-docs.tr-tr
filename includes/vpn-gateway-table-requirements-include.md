---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: dc018b5d09c9b33c10cd2d54ac6572537e05ed25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188258"
---
Aşağıdaki tabloda, Politika Tabanlı ve RouteBased VPN ağ geçitleri için gereksinimleri listelenizdir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır. Klasik model için, PolicyBased VPN ağ geçitleri Statik ağ geçitleri ile aynıdır ve Rota tabanlı ağ geçitleri Dinamik ağ geçitleri ile aynıdır.

|  | **Politika Tabanlı Temel VPN Ağ Geçidi** | **RouteBased Temel VPN Ağ Geçidi** | **RouteBased Standart VPN Ağ Geçidi** | **RouteBased Yüksek Performanslı VPN Ağ Geçidi** |
| --- | --- | --- | --- | --- |
| **Siteden Siteye bağlantı   (S2S)** |PolicyBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |
| **Noktadan Siteye bağlantı (P2S**) |Desteklenmiyor |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |
| **Kimlik doğrulama Yöntemi** |Önceden paylaşılan anahtar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |
| **S2S bağlantılarının en yüksek sayısı** |1 |10 |10 |30 |
| **P2S bağlantılarının en yüksek sayısı** |Desteklenmiyor |128 |128 |128 |
| **Etkin yönlendirme desteği (BGP)** (*) |Desteklenmiyor |Desteklenmiyor |Destekleniyor |Destekleniyor |

  (*) BGP klasik dağıtım modeli için desteklenmez.
