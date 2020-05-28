---
title: Güvenlik denetimleri
description: Azure SQL veritabanı 'nı değerlendirmek için güvenlik denetimlerinin denetim listesi
services: sql-database
author: msmbaldwin
manager: rkalrin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 09045e01ad4d40ab770dd6203f2dd4b299317a55
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84050012"
---
# <a name="security-controls-for-azure-sql-database-and-sql-managed-instance"></a>Azure SQL veritabanı ve SQL yönetilen örneği için güvenlik denetimleri
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Bu makale, Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]



## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yes | Yalnızca [SQL veritabanı](../index.yml) için geçerlidir. |
| Azure sanal ağ ekleme desteği| Yes | Yalnızca [SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) için geçerlidir. |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes | Hem veritabanı düzeyinde hem de sunucu düzeyinde güvenlik duvarı. Ağ yalıtımı yalnızca [SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) içindir. |
| Zorlamalı tünel desteği| Yes | [ExpressRoute](../expressroute/../index.yml) VPN aracılığıyla [SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) . |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Log Analytics veya Application Insights gibi Azure izleme desteği| Yes | EMVA 'dan SıEM çözümü olan SecureSphere, [SQL denetimi](../../azure-sql/database/auditing-overview.md)aracılığıyla [Azure Event Hubs](../event-hubs/../index.yml) tümleştirme aracılığıyla da desteklenir. |
| Denetim düzlemi ve yönetimi-düzlem günlüğü ve denetimi| Yes | Yalnızca bazı olaylar için Evet |
| Veri düzlemi günlüğü ve denetimi | Yes | [SQL denetimi](../../azure-sql/database/auditing-overview.md) aracılığıyla |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Yes | Azure Active Directory (Azure AD) |
| Yetkilendirme| Yes | Yok |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes | [Always Encrypted](always-encrypted-certificate-store-configure.md)makalesinde açıklandığı gibi "kullanımda olan şifreleme" olarak adlandırılır. Sunucu tarafı şifreleme, [Saydam veri şifrelemesi](transparent-data-encryption-tde-overview.md)kullanır.|
| Aktarım sırasında şifreleme:<ul><li>Azure ExpressRoute şifrelemesi</li><li>Bir sanal ağda şifreleme</li><li>Sanal ağlar arasında şifreleme</ul>| Yes | HTTPS kullanma. |
| CMK veya BYOK gibi şifreleme anahtarı işleme| Yes | Hem hizmet tarafından yönetilen hem de müşteri tarafından yönetilen anahtar işleme sunulur. İkincisi [Azure Key Vault](../key-vault/../index.yml)aracılığıyla sunulur. |
| Azure veri Hizmetleri tarafından sunulan sütun düzeyinde şifreleme| Yes | [Always Encrypted](always-encrypted-certificate-store-configure.md)üzerinden. |
| Şifrelenmiş API çağrıları| Yes | HTTPS/TLS kullanma. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma sürümü oluşturma gibi yapılandırma yönetimi desteği| No  | Yok |

## <a name="additional-security-controls-for-sql-database"></a>SQL veritabanı için ek güvenlik denetimleri

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Koruyucu: güvenlik açığı değerlendirmesi | Yes | Bkz. [SQL güvenlik açığı değerlendirmesi hizmeti, veritabanı güvenlik açıklarını belirlemenize yardımcı olur](sql-vulnerability-assessment.md). |
| Koruyucu: veri bulma ve sınıflandırma  | Yes | Bkz. [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırması](data-discovery-and-classification-overview.md). |
| Algılama: tehdit algılama | Yes | Bkz. [Azure SQL veritabanı Için Gelişmiş tehdit koruması](threat-detection-overview.md). |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
