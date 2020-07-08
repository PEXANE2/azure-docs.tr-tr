---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: dc018b5d09c9b33c10cd2d54ac6572537e05ed25
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188258"
---
Aşağıdaki tabloda PolicyBased ve RouteBased VPN Gateway gereksinimleri listelenmektedir. Bu tablo hem Resource Manager, hem de klasik dağıtım modellerine uygulanır. Klasik modelde, PolicyBased VPN ağ geçitleri statik ağ geçitleriyle aynıdır ve rota tabanlı ağ geçitleri, dinamik ağ geçitleriyle aynıdır.

|  | **PolicyBased temel VPN Gateway** | **RouteBased temel VPN Gateway** | **RouteBased standart VPN Gateway** | **RouteBased yüksek performans VPN Gateway** |
| --- | --- | --- | --- | --- |
| **Siteden Siteye bağlantı   (S2S)** |PolicyBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |RouteBased VPN yapılandırması |
| **Noktadan Siteye bağlantı (P2S**) |Desteklenmiyor |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |Destekleniyor (S2S ile birlikte var olabilir) |
| **Kimlik doğrulama yöntemi** |Önceden paylaşılan anahtar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |S2S bağlantısı için önceden paylaşılan anahtar, P2S bağlantısı için sertifikalar |
| **S2S bağlantılarının en yüksek sayısı** |1 |10 |10 |30 |
| **P2S bağlantılarının en yüksek sayısı** |Desteklenmiyor |128 |128 |128 |
| **Etkin yönlendirme desteği (BGP)** (*) |Desteklenmiyor |Desteklenmiyor |Destekleniyor |Destekleniyor |

  (*) BGP, klasik dağıtım modeli için desteklenmiyor.
