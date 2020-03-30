---
title: Güvenlik denetimleri
description: Azure Yedekleme hizmetinde kullanılan güvenlik denetimleri hakkında bilgi edinin. Bu denetimler, hizmetin güvenlik açıklarını önlemesine, algılamasına ve yanıtlatanlarına yardımcı olur.
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 0e3f5ce942ea8aef9bf5eb98883ae1e72a7ab239
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74172131"
---
# <a name="security-controls-for-azure-backup"></a>Azure Yedekleme için güvenlik denetimleri

Bu makalede, Azure Yedekleme'de yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Hizmet bitiş noktası desteği| Hayır |  |  |
| VNet enjeksiyon desteği| Hayır |  |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | VM yedeklemesi için zorunlu tünelleme desteklenir. Zorunlu tünelleme, VM'lerin içinde çalışan iş yükleri için desteklenmez. |  |
| Zorunlu tünel desteği| Hayır |  |  |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar| | Belgeler
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | Log Analytics kaynak günlükleri aracılığıyla desteklenir. Daha fazla bilgi için Bkz. [Log Analytics'i kullanarak Azure Yedekleme korumalı iş yüklerini izleyin.](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) |  |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | Azure portalından müşteri tarafından tetiklenen tüm eylemler etkinlik günlüklerine kaydedilir. |  |
| Veri düzlemi günlüğü ve denetimi| Hayır | Azure Yedekleme veri düzlemine doğrudan erişimez.  |  |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar| | Belgeler
|---|---|--|
| Kimlik doğrulaması| Evet | Kimlik doğrulama, Azure Etkin Dizini aracılığıyla yapılır. |  |
| Yetkilendirme| Evet | Müşteri oluşturulan ve yerleşik RBAC rolleri kullanılır. Daha fazla bilgi için Azure [Yedekleme kurtarma noktalarını yönetmek için Rol Tabanlı Erişim Denetimini Kullan'a](/azure/backup/backup-rbac-rs-vault)bakın. |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar | | Belgeler
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | Depolama hesapları için depolama hizmeti şifrelemesi kullanma. |  |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır |  |  |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Hayır |  |  |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Hayır | HTTPS'yi kullanma. |  |
| API şifreli aramalar| Evet |  |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar| | Belgeler
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet|  |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
