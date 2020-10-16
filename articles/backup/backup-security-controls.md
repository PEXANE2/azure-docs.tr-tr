---
title: Güvenlik denetimleri
description: Azure Backup hizmetinde kullanılan güvenlik denetimleri hakkında bilgi edinin. Bu denetimler hizmetin güvenlik açıklarını engellemesine, algılamasına ve yanıt vermesine yardımcı olur.
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 96899c0669f3063232c36ad3ae1fec76a90e0a5c
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92090871"
---
# <a name="security-controls-for-azure-backup"></a>Azure Backup için güvenlik denetimleri

Bu makalede, Azure Backup yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|--|
| Hizmet uç noktası desteği| No |  |  |
| VNet ekleme desteği| No |  |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes | |  |
| Azure VM 'Leri için Zorlamalı tünel desteği | Yes  |  |  |
| Azure VM 'Leri içinde çalışan uygulamalar için Zorlamalı tünel desteği| No  |  |  |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler
|---|---|--|--|
| Azure izleme desteği (Log Analytics, App Insights gibi)| Yes | Log Analytics, kaynak günlükleri aracılığıyla desteklenir. Daha fazla bilgi için bkz. [Log Analytics kullanarak Azure Backup korunan iş yüklerini izleme](backup-azure-diagnostics-mode-data-model.md). |  |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Yes | Azure portal müşterinin tetiklediği tüm eylemler etkinlik günlüklerine kaydedilir. |  |
| Veri düzlemi günlüğü ve denetimi| No | Azure Backup veri düzlemine doğrudan ulaşılamıyor.  |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler
|---|---|--|--|
| Kimlik Doğrulaması| Yes | Kimlik doğrulaması Azure Active Directory. |  |
| Yetkilendirme| Yes | Oluşturulan müşteri ve Azure yerleşik rolleri kullanılır. Daha fazla bilgi için bkz. [Azure Backup kurtarma noktalarını yönetmek Için Azure rol tabanlı erişim denetimi kullanma](./backup-rbac-rs-vault.md). |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes | Depolama hesapları için depolama hizmeti şifrelemesini kullanma. |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | No |  |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| No |  |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifrelemesi ve VNet-VNet şifreleme gibi)| No | HTTPS kullanma. |  |
| Şifrelenmiş API çağrıları| Yes |  |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma vb.)| Evet|  |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
