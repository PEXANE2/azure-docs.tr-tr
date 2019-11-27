---
title: Azure Load Balancer için güvenlik denetimleri
description: Load Balancer değerlendirmek için güvenlik denetimlerinin denetim listesi
services: load-balancer
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: allensu
ms.openlocfilehash: 6043e574697489b6566641c352bc21a2b6d87f51
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74214894"
---
# <a name="security-controls-for-azure-load-balancer"></a>Azure Load Balancer için güvenlik denetimleri

Bu makalede, Azure Load Balancer yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok | |
| VNet ekleme desteği| Yok | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yok |  |
| Zorlamalı tünel desteği| Yok | |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [Genel temel Load Balancer Için Azure izleyici günlükleri](load-balancer-monitor-log.md). |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Bkz. [Genel temel Load Balancer Için Azure izleyici günlükleri](load-balancer-monitor-log.md). |
| Veri düzlemi günlüğü ve denetimi | Yok |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Yok |  |
| Yetkilendirme| Yok |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yok | |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yok | |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yok | |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yok |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.