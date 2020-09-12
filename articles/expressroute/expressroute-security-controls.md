---
title: 'Azure ExpressRoute: güvenlik denetimleri'
description: Azure ExpressRoute 'daki güvenlik denetimleri hakkında bilgi edinin. bu özellikler, güvenlik açıklarını önlemeye, algılamaya ve bunlara yanıt vermeye yardımcı olan kalitelerdir veya özelliklerdir.
services: expressroute
ms.service: expressroute
author: duongau
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: duau
ms.openlocfilehash: 24057de44f3d28df96bcb93e89af9c3afa6fa3c6
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89394916"
---
# <a name="security-controls-for-azure-expressroute"></a>Azure ExpressRoute için güvenlik denetimleri

Bu makale, Azure ExpressRoute 'da yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok |  |
| VNet ekleme desteği| Yok | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes | Her müşteri kendi yönlendirme etki alanında bulunur ve kendi VNet 'ine tünel |
| Zorlamalı tünel desteği| Yok | Sınır Ağ Geçidi Protokolü (BGP) üzerinden. |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Yes | Bkz. [ExpressRoute izleme, ölçümler ve uyarılar](expressroute-monitoring-metrics-alerts.md).|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Yes |  |
| Veri düzlemi günlüğü ve denetimi| No |   |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Yes | Microsoft için ağ geçidi için hizmet hesabı (GWM) (denetleyici); Geliştirme ve OP için tam zamanında (JıT) erişim. |
| Yetkilendirme|  Yes |Microsoft için ağ geçidi için hizmet hesabı (GWM) (denetleyici); Geliştirme ve OP için tam zamanında (JıT) erişim. |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar |  Yok | ExpressRoute müşteri verilerini depolamaz. |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yok |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| No | |
| Şifrelenmiş API çağrıları| Yes | [Azure Resource Manager](../azure-resource-manager/index.yml) ve https üzerinden. |


## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yes | Ağ kaynak sağlayıcısı (NRP) aracılığıyla. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.