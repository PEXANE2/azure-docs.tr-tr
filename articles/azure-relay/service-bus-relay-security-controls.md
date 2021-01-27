---
title: Azure Relay için güvenlik denetimleri
description: Bu makaleler, Azure Relay değerlendirmek için yerleşik güvenlik denetimlerinin bir listesini sağlar.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: ce5053366ac1d3536a152610d8ed7f76fad62b84
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98919587"
---
# <a name="security-controls-for-azure-relay"></a>Azure Relay için güvenlik denetimleri

Bu makalede, Azure Relay yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Özel uç nokta desteği| Yes |  |   |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes |  |   |
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
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifrelemesi ve VNet-VNet şifreleme gibi)| Yes | Hizmet için TLS gerekiyor. |   |
| Şifrelenmiş API çağrıları| Yes | 'Dir. |


## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yes | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden.|   |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.