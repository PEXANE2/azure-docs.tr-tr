---
title: Azure Servis Veri Servisi Mesajlaşması için güvenlik denetimleri
description: Azure Hizmet Veri Aracı İletisini değerlendirmek için güvenlik denetimleri listesi
services: service-bus-messaging
ms.service: service-bus-messaging
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: af119ef026b70fcb4a56b4f823d20c0e9eddddc8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75903258"
---
# <a name="security-controls-for-azure-service-bus-messaging"></a>Azure Servis Veri Servisi Mesajlaşması için güvenlik denetimleri

Bu makalede, Azure Hizmet Veri Servisi İletisi'nde yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet bitiş noktası desteği| Evet (Yalnızca Premium katman) | VNet hizmet bitiş noktaları yalnızca [Service Bus Premium katmanı](service-bus-premium-messaging.md) için desteklenir. |  |
| VNet enjeksiyon desteği| Hayır | |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet (Yalnızca Premium katman) |  |  |
| Zorunlu tünel desteği| Hayır |  |  |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | [Azure Monitör ve Uyarılar](service-bus-metrics-azure-monitor.md)aracılığıyla desteklenir. |  |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | İşlem günlükleri mevcuttur.  | [Servis Veri Aracı tanı günlükleri](service-bus-diagnostic-logs.md) |
| Veri düzlemi günlüğü ve denetimi| Hayır |  |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik doğrulaması| Evet | Azure [Etkin Dizin Yönetilen Hizmet Kimliği](service-bus-managed-service-identity.md)ile yönetilir.| [Servis Veri Servisi kimlik doğrulaması ve yetkilendirme.](service-bus-authentication-and-authorization.md) |
| Yetkilendirme| Evet | [RBAC](authenticate-application.md) ve SAS belirteci aracılığıyla yetkilendirmeyi destekler. | [Servis Veri Servisi kimlik doğrulaması ve yetkilendirme.](service-bus-authentication-and-authorization.md) |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar |  Varsayılan olarak sunucu tarafındaki şifreleme-at-rest için evet. |  |  |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet. | Azure KeyVault'ta müşteri tarafından yönetilen bir anahtar, Hizmet Veri Kurumu Ad alanındaki verileri tam olarak şifrelemek için kullanılabilir. | [Azure Hizmet Veri Servisi verilerini şifrelemek için müşteri tarafından yönetilen anahtarları Azure portalını kullanarak yapılandırma](configure-customer-managed-key.md)  |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |   |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Standart HTTPS/TLS mekanizmasını destekler. |   |
| API şifreli aramalar| Evet | API aramaları [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml) ve HTTPS üzerinden yapılır. |   |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | [Azure Kaynak Yöneticisi API](/rest/api/resources/)aracılığıyla kaynak sağlayıcı sürümünü destekler.|   |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
