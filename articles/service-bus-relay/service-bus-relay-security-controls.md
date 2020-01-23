---
title: Azure Service Bus geçişi için güvenlik denetimleri
description: Bu makaleler Azure Service Bus geçişini değerlendirmek için yerleşik güvenlik denetimlerinin bir listesini sağlar.
services: service-bus-relay
ms.service: service-bus-relay
author: spelluru
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 28d3ba14aa7769ac4f3fc22bd2b5bd7acd30557c
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76514026"
---
# <a name="security-controls-for-azure-service-bus-relay"></a>Azure Service Bus geçişi için güvenlik denetimleri

Bu makale Azure Service Bus geçişine yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet uç noktası desteği| Hayır |  |   |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |   |
| Zorlamalı tünel desteği| Yok | Geçiş, TLS tünelidir  |   |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | |   |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden. |   |
| Veri düzlemi günlüğü ve denetimi| Evet | Bağlantı başarısı/hatası ve hataları ve günlüğe kaydedildi.  |   |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik Doğrulaması| Evet | SAS aracılığıyla. | [Azure Relay kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md) |
| Yetkilendirme|  Evet | SAS aracılığıyla. | [Azure Relay kimlik doğrulaması ve yetkilendirme](relay-authentication-and-authorization.md) |

## <a name="data-protection"></a>Veri koruması

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar |  Yok | Geçiş bir Web soketi ve verileri kalıcı yapmaz. |   |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır | Yalnızca Microsoft TLS sertifikalarını kullanır.  |   |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |   |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Hizmet için TLS gerekiyor. |   |
| Şifrelenmiş API çağrıları| Evet | 'Dir. |


## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden.|   |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.