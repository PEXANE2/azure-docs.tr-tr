---
title: Azure Relay için güvenlik denetimleri
description: Bu makaleler, Azure Relay değerlendirmek için yerleşik güvenlik denetimlerinin bir listesini sağlar.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: 5d55026bfb6e3d6fe955a540b7596a85707398d6
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98133353"
---
# <a name="security-controls-for-azure-relay"></a>Azure Relay için güvenlik denetimleri

Bu makalede, Azure Relay yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Özel uç nokta desteği| No |  |   |
| Ağ yalıtımı ve güvenlik duvarı desteği| No |  |   |
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
| Kimlik doğrulaması| Yes | SAS aracılığıyla. | [Azure Relay kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md) |
| Yetkilendirme|  Yes | SAS aracılığıyla. | [Azure Relay kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar |  Yok | Geçiş bir Web soketi ve verileri kalıcı yapmaz. |   |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | No | Yalnızca Microsoft TLS sertifikalarını kullanır.  |   |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |   |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifrelemesi ve VNet-VNet şifreleme gibi)| Yes | Hizmet için TLS gerekiyor. |   |
| Şifrelenmiş API çağrıları| Yes | 'Dir. |


## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yes | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden.|   |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.