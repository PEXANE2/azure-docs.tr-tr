---
title: Azure Event Hubs için güvenlik denetimleri
description: Azure Event Hubs değerlendirmek için güvenlik denetimlerinin denetim listesi
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 3f02315012840b20dc4aa8639ade954f23a5526e
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886838"
---
# <a name="security-controls-for-azure-event-hubs"></a>Azure Event Hubs için güvenlik denetimleri

Bu makale, Azure Event Hubs yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Hayır | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet |  |
| Zorlamalı tünel desteği| Hayır |  |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi| Evet |   |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | |
| Authorization|  Evet | |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen sunucu tarafı şifrelemesi: Microsoft tarafından yönetilen anahtarlar |  Evet | |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
