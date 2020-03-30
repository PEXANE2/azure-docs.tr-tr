---
title: Güvenlik denetimleri
description: Azure SQL Veritabanı'nı değerlendirmek için güvenlik denetimleri denetim listesi
services: sql-database
author: msmbaldwin
manager: rkalrin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: ce7f3eafa57cbd993be98f4a2da3d89cb312f9b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190684"
---
# <a name="security-controls-for-azure-sql-database"></a>Azure SQL Veritabanı için güvenlik denetimleri

Bu makalede, Azure SQL Veritabanı'nda yerleşik güvenlik denetimleri belgelenir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

SQL Veritabanı hem [tek veritabanı](sql-database-single-index.yml) hem de [yönetilen örneği](sql-database-managed-instance.md)içerir. Aksi belirtilmedikçe, aşağıdaki girişler her iki teklif için de geçerlidir.

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Evet | Yalnızca [tek bir veritabanı](sql-database-single-index.yml) için geçerlidir. |
| Azure Sanal Ağ enjeksiyon desteği| Evet | Yalnızca [yönetilen örnek](sql-database-managed-instance.md) için geçerlidir. |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Hem veritabanı düzeyinde hem de sunucu düzeyinde güvenlik duvarı. Ağ yalıtımı yalnızca [yönetilen örnektir.](sql-database-managed-instance.md) |
| Zorunlu tünel desteği| Evet | [ExpressRoute](../expressroute/index.yml) VPN üzerinden [yönetilen örnek.](sql-database-managed-instance.md) |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Günlük Analizi veya Uygulama Öngörüleri gibi Azure izleme desteği| Evet | Imperva'nın SIEM çözümü SecureSphere, [SQL denetimi](sql-database-auditing.md)yoluyla Azure [Event Hubs](../event-hubs/index.yml) entegrasyonu ile de desteklenir. |
| Kontrol-düzlem ve yönetim-düzlem günlüğü ve denetimi| Evet | Yalnızca bazı etkinlikler için evet |
| Veri düzlemi günlüğü ve denetimi | Evet | [VIA SQL denetimi](sql-database-auditing.md) |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Evet | Azure Active Directory (Azure AD) |
| Yetkilendirme| Evet | None |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | "Şifreleme-in-use" olarak adlandırılan makalede açıklandığı gibi [Her Zaman Şifrelenmiş](sql-database-always-encrypted.md). Sunucu tarafı şifreleme [saydam veri şifreleme](transparent-data-encryption-azure-sql.md)kullanır.|
| Aktarımda şifreleme:<ul><li>Azure ExpressRoute şifrelemesi</li><li>Sanal ağda şifreleme</li><li>Sanal ağlar arasında şifreleme</ul>| Evet | HTTPS'yi kullanma. |
| CMK veya BYOK gibi şifreleme anahtarı işleme| Evet | Hem servis yönetimi hem de müşteri tarafından yönetilen anahtar işleme sunulmaktadır. İkincisi [Azure Key Vault](../key-vault/index.yml)üzerinden sunulmaktadır. |
| Azure veri hizmetleri tarafından sağlanan sütun düzeyinde şifreleme| Evet | [Her zaman şifreli](sql-database-always-encrypted.md)üzerinden . |
| Şifreli API aramaları| Evet | HTTPS/TLS kullanarak. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırmanın sürümü gibi yapılandırma yönetimi desteği| Hayır  | None |

## <a name="additional-security-controls-for-sql-database"></a>SQL Veritabanı için ek güvenlik denetimleri

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Önleyici: güvenlik açığı değerlendirmesi | Evet | Bkz. [SQL Güvenlik Açığı Değerlendirmesi hizmeti veritabanı güvenlik açıklarını belirlemenize yardımcı olur.](sql-vulnerability-assessment.md) |
| Önleyici: veri bulma ve sınıflandırma  | Evet | Bkz. [Azure SQL Veritabanı ve SQL Veri Ambarı veri bulma & sınıflandırması.](sql-database-data-discovery-and-classification.md) |
| Algılama: tehdit algılama | Evet | [Azure SQL Veritabanı için Gelişmiş Tehdit Koruması'na](sql-database-threat-detection-overview.md)bakın. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
