---
title: 'Azure ExpressRoute: Güvenlik denetimleri'
description: Azure ExpressRoute'u değerlendirmek için güvenlik denetimleri listesi
services: expressroute
ms.service: expressroute
author: msmbaldwin
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: a288b44c07bc2df8529f07264dcee648f3af379a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74079923"
---
# <a name="security-controls-for-azure-expressroute"></a>Azure ExpressRoute için güvenlik denetimleri

Bu makalede, Azure ExpressRoute'da yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Yok |  |
| VNet enjeksiyon desteği| Yok | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Her müşteri kendi yönlendirme etki alanında bulunur ve kendi VNet tünel |
| Zorunlu tünel desteği| Yok | Sınır Ağ Geçidi Protokolü (BGP) üzerinden. |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | Bkz. [ExpressRoute izleme, ölçümler ve uyarılar.](expressroute-monitoring-metrics-alerts.md)|
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi| Hayır |   |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Evet | Microsoft için Ağ Geçidi (GWM) (denetleyici) için hizmet hesabı; Dev ve OP için Tam Zamanında (JIT) erişim. |
| Yetkilendirme|  Evet |Microsoft için Ağ Geçidi (GWM) (denetleyici) için hizmet hesabı; Dev ve OP için Tam Zamanında (JIT) erişim. |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar |  Yok | ExpressRoute müşteri verilerini depolamaz. |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Yok |  |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Hayır | |
| API şifreli aramalar| Evet | [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml) ve HTTPS üzerinden. |


## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | Ağ Kaynak Sağlayıcısı (NRP) üzerinden. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.