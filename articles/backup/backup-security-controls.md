---
title: Azure Backup için güvenlik denetimleri
description: Azure Backup değerlendirmek için güvenlik denetimlerinin denetim listesi
ms.reviewer: mbaldwin
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: dacurwin
ms.openlocfilehash: f44be7556b6d741df93faeeab1dbdfc15bc8ebfd
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71211807"
---
# <a name="security-controls-for-azure-backup"></a>Azure Backup için güvenlik denetimleri

Bu makalede, Azure Backup yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir. 

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |  |
| VNet ekleme desteği| Hayır |  |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Zorlamalı tünel VM yedeklemesi için desteklenir. Zorlamalı tünel, VM 'Ler içinde çalışan iş yükleri için desteklenmez. |  |
| Zorlamalı tünel desteği| Hayır |  |  |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| | Belgeler
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Log Analytics tanılama günlükleri aracılığıyla desteklenir. Daha fazla bilgi için bkz. [Log Analytics kullanarak korumalı iş yüklerini izleme Azure Backup](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) . |  |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Azure portal müşterinin tetiklediği tüm eylemler etkinlik günlüklerine kaydedilir. |  |
| Veri düzlemi günlüğü ve denetimi| Hayır | Azure Backup veri düzlemine doğrudan ulaşılamıyor.  |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| | Belgeler
|---|---|--|
| Authentication| Evet | Kimlik doğrulaması Azure Active Directory. |  |
| Authorization| Evet | Oluşturulan müşteri ve yerleşik RBAC rolleri kullanılır. Daha fazla bilgi için [Azure Backup kurtarma noktalarını yönetmek üzere rol tabanlı Access Control kullanma](/azure/backup/backup-rbac-rs-vault) konusuna bakın. |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar | | Belgeler
|---|---|--|
| Bekleyen sunucu tarafı şifrelemesi: Microsoft tarafından yönetilen anahtarlar | Evet | Depolama hesapları için depolama hizmeti şifrelemesini kullanma. |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır |  |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Hayır |  |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Hayır | HTTPS kullanma. |  |
| Şifrelenmiş API çağrıları| Evet |  |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| | Belgeler
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet|  |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.