---
title: Azure Load Balancer için güvenlik denetimleri
description: Load Balancer değerlendirmek için güvenlik denetimlerinin denetim listesi
services: load-balancer
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: allensu
ms.openlocfilehash: 6043e574697489b6566641c352bc21a2b6d87f51
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74214894"
---
# <a name="security-controls-for-azure-load-balancer"></a>Azure Load Balancer için güvenlik denetimleri

Bu makalede, Azure Load Balancer yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| YOK | |
| VNet ekleme desteği| YOK | |
| Ağ yalıtımı ve güvenlik duvarı desteği| YOK |  |
| Zorlamalı tünel desteği| YOK | |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [Genel temel Load Balancer Için Azure izleyici günlükleri](load-balancer-monitor-log.md). |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Bkz. [Genel temel Load Balancer Için Azure izleyici günlükleri](load-balancer-monitor-log.md). |
| Veri düzlemi günlüğü ve denetimi | YOK |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| YOK |  |
| Yetkilendirme| YOK |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | YOK | |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| YOK | |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | YOK | |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| YOK | |
| Şifrelenmiş API çağrıları| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| YOK |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.