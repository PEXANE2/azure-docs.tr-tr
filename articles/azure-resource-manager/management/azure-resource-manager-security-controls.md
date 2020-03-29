---
title: Güvenlik denetimleri
description: Azure Kaynak Yöneticisi hizmetini değerlendirmek için yerleşik güvenlik denetimlerinin bir denetim listesi.
ms.topic: conceptual
ms.date: 09/04/2019
ms.openlocfilehash: d0a0625153e428a0d261e52d40b31ef5142eddfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75485630"
---
# <a name="security-controls-for-azure-resource-manager"></a>Azure Kaynak Yöneticisi için güvenlik denetimleri

Bu makalede, Azure Kaynak Yöneticisi'nde yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet |  |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | HTTPS/TLS. |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Yok | Azure Kaynak Yöneticisi müşteri içeriği depolar, yalnızca verileri kontrol edin. |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Evet | |
| API şifreli aramalar| Evet | |

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Hayır | |
| VNet enjeksiyon desteği| Evet | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |
| Zorunlu tünel desteği| Hayır |  |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Hayır | |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | Etkinlik günlükleri, kaynaklarınızda gerçekleştirilen tüm yazma işlemlerini (PUT, POST, DELETE) ortaya çıkarır; kaynaklardaki [eylemleri denetlemek için etkinlik günlüklerini görüntüleyin.](view-activity-logs.md) |
| Veri düzlemi günlüğü ve denetimi| Yok | |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Evet | [Azure Etkin Dizini](/azure/active-directory) tabanlı.|
| Yetkilendirme| Evet | |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.