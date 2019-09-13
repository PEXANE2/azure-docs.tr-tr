---
title: Azure Resource Manager için güvenlik denetimleri
description: Azure Resource Manager değerlendirmek için güvenlik denetimlerinin denetim listesi
services: azure-resource-manager
author: msmbaldwin
manager: rkarlin
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 97fd6611d7e2a2787b865365c4c7579f89f17d01
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886584"
---
# <a name="security-controls-for-azure-resource-manager"></a>Azure Resource Manager için güvenlik denetimleri

Bu makalede, Azure Resource Manager yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen sunucu tarafı şifrelemesi: Microsoft tarafından yönetilen anahtarlar | Evet |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | HTTPS/TLS. |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yok | Azure Resource Manager hiçbir müşteri içeriği depolar, yalnızca denetim verileri. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Evet | |
| Şifrelenmiş API çağrıları| Evet | |

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |
| Zorlamalı tünel desteği| Hayır |  |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Hayır | |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Etkinlik günlükleri, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, SILME) kullanıma sunar; bkz. [kaynaklardaki eylemleri denetlemek için etkinlik günlüklerini görüntüleme](resource-group-audit.md). |
| Veri düzlemi günlüğü ve denetimi| Yok | |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | [Azure Active Directory](/azure/active-directory) tabanlı.|
| Authorization| Evet | |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.