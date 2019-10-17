---
title: Azure Resource Manager için güvenlik denetimleri
description: Azure Resource Manager hizmetini değerlendirmek için yerleşik güvenlik denetimlerinin denetim listesi.
services: azure-resource-manager
author: msmbaldwin
manager: rkarlin
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 78f3f41ca2f0aa45d295fd9846cf3aa96cccea72
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72390457"
---
# <a name="security-controls-for-azure-resource-manager"></a>Azure Resource Manager için güvenlik denetimleri

Bu makalede, Azure Resource Manager yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Veri koruması

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yes | HTTPS/TLS. |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yok | Azure Resource Manager hiçbir müşteri içeriği depolar, yalnızca denetim verileri. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yes | |
| Şifrelenmiş API çağrıları| Yes | |

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Yes | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |
| Zorlamalı tünel desteği| Hayır |  |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Hayır | |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Yes | Etkinlik günlükleri, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, SILME) kullanıma sunar; bkz. [kaynaklardaki eylemleri denetlemek için etkinlik günlüklerini görüntüleme](resource-group-audit.md). |
| Veri düzlemi günlüğü ve denetimi| Yok | |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Yes | [Azure Active Directory](/azure/active-directory) tabanlı.|
| Yetkilendirme| Yes | |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yes |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.