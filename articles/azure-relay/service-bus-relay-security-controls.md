---
title: Azure Relay için güvenlik denetimleri
description: Bu makaleler, Azure Relay değerlendirmek için yerleşik güvenlik denetimlerinin bir listesini sağlar.
services: service-bus-relay
ms.service: service-bus-relay
author: spelluru
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: f8165d994e998af4f15cd6aa2fd08b75191b8b64
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83211467"
---
# <a name="security-controls-for-azure-relay"></a>Azure Relay için güvenlik denetimleri

Bu makalede, Azure Relay yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet uç noktası desteği| Hayır |  |   |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |   |
| Zorlamalı tünel desteği| Yok | Geçiş, TLS tünelidir  |   |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Yes | |   |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Yes | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden. |   |
| Veri düzlemi günlüğü ve denetimi| Yes | Bağlantı başarısı/hatası ve hataları ve günlüğe kaydedildi.  |   |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik Doğrulaması| Yes | SAS aracılığıyla. | [Azure Relay kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md) |
| Yetkilendirme|  Yes | SAS aracılığıyla. | [Azure Relay kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar |  Yok | Geçiş bir Web soketi ve verileri kalıcı yapmaz. |   |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır | Yalnızca Microsoft TLS sertifikalarını kullanır.  |   |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |   |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yes | Hizmet için TLS gerekiyor. |   |
| Şifrelenmiş API çağrıları| Yes | 'Dir. |


## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yes | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden.|   |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.