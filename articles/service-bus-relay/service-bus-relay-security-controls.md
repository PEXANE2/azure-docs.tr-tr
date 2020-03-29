---
title: Azure Servis Veri Veri Günü için güvenlik denetimleri
description: Bu makaleler, Azure Hizmet Veri Veri Günü'nün değerlendirilmesi için yerleşik güvenlik denetimlerinin bir denetim listesini sağlar.
services: service-bus-relay
ms.service: service-bus-relay
author: spelluru
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 28d3ba14aa7769ac4f3fc22bd2b5bd7acd30557c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76514026"
---
# <a name="security-controls-for-azure-service-bus-relay"></a>Azure Servis Veri Veri Günü için güvenlik denetimleri

Bu makalede, Azure Hizmet Veri Günü'nde yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet bitiş noktası desteği| Hayır |  |   |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |   |
| Zorunlu tünel desteği| Yok | Röle TLS tünelidir  |   |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | |   |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml)aracılığıyla. |   |
| Veri düzlemi günlüğü ve denetimi| Evet | Bağlantı başarısı / hata ve hatalar ve günlüğe kaydedilir.  |   |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik doğrulaması| Evet | SAS üzerinden. | [Azure Röle kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md) |
| Yetkilendirme|  Evet | SAS üzerinden. | [Azure Röle kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar |  Yok | Röle bir web soketidir ve verileri devam ettirmez. |   |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır | Yalnızca Microsoft TLS sertifikalarını kullanır.  |   |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |   |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Hizmet TLS gerektirir. |   |
| API şifreli aramalar| Evet | HTTPS adresinden edinebilirsiniz. |


## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml)aracılığıyla.|   |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.