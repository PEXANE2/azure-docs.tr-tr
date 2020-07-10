---
title: Güvenlik denetimleri
description: Azure Backup hizmetinde kullanılan güvenlik denetimleri hakkında bilgi edinin. Bu denetimler hizmetin güvenlik açıklarını engellemesine, algılamasına ve yanıt vermesine yardımcı olur.
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 7d8fd98467e975c6151d09779ab450810ea32c6e
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86165575"
---
# <a name="security-controls-for-azure-backup"></a>Azure Backup için güvenlik denetimleri

Bu makalede, Azure Backup yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|--|
| Hizmet uç noktası desteği| Hayır |  |  |
| VNet ekleme desteği| Hayır |  |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Zorlamalı tünel VM yedeklemesi için desteklenir. Zorlamalı tünel, VM 'Ler içinde çalışan iş yükleri için desteklenmez. |  |
| Zorlamalı tünel desteği| Hayır |  |  |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler
|---|---|--|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Log Analytics, kaynak günlükleri aracılığıyla desteklenir. Daha fazla bilgi için bkz. [Log Analytics kullanarak Azure Backup korunan iş yüklerini izleme](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/). |  |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Azure portal müşterinin tetiklediği tüm eylemler etkinlik günlüklerine kaydedilir. |  |
| Veri düzlemi günlüğü ve denetimi| Hayır | Azure Backup veri düzlemine doğrudan ulaşılamıyor.  |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler
|---|---|--|--|
| Kimlik doğrulaması| Evet | Kimlik doğrulaması Azure Active Directory. |  |
| Yetkilendirme| Evet | Oluşturulan müşteri ve yerleşik RBAC rolleri kullanılır. Daha fazla bilgi için bkz. [Azure Backup kurtarma noktalarını yönetmek Için rol tabanlı Access Control kullanma](/azure/backup/backup-rbac-rs-vault). |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Evet | Depolama hesapları için depolama hizmeti şifrelemesini kullanma. |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır |  |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Hayır |  |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Hayır | HTTPS kullanma. |  |
| Şifrelenmiş API çağrıları| Evet |  |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet|  |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
