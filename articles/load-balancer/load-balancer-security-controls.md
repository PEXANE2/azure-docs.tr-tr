---
title: Azure Yük Dengeleyicisi için güvenlik denetimleri
description: Yük Dengeleyicisini değerlendirmek için güvenlik denetimleri denetim listesi
services: load-balancer
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: allensu
ms.openlocfilehash: 6043e574697489b6566641c352bc21a2b6d87f51
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74214894"
---
# <a name="security-controls-for-azure-load-balancer"></a>Azure Yük Dengeleyicisi için güvenlik denetimleri

Bu makalede, Azure Yük Dengeleyicisi'nde yerleşik güvenlik denetimleri belgelenir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Yok | |
| VNet enjeksiyon desteği| Yok | |
| Ağ Yalıtımı ve Güvenlik Duvarı desteği| Yok |  |
| Zorunlu tünel desteği| Yok | |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | [Genel Temel Yük Dengeleyicisi için Azure Monitör günlüklerine](load-balancer-monitor-log.md)bakın. |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | [Genel Temel Yük Dengeleyicisi için Azure Monitör günlüklerine](load-balancer-monitor-log.md)bakın. |
| Veri düzlemi günlüğü ve denetimi | Yok |  |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Yok |  |
| Yetkilendirme| Yok |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Yok | |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yok | |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Yok | |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |
| API şifreli aramalar| Evet | Azure [Kaynak Yöneticisi](../azure-resource-manager/index.yml)aracılığıyla. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Yok |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.