---
title: Güvenlik denetimleri
description: Azure SQL veritabanı 'nı değerlendirmek için güvenlik denetimlerinin denetim listesi
services: sql-database
author: msmbaldwin
manager: rkarlin
ms.service: load-balancer
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 97d2cd8e9ba51e4fc6ebab8459b04f4f37e876d3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84668432"
---
# <a name="security-controls-for-azure-sql-database-and-sql-managed-instance"></a>Azure SQL veritabanı ve SQL yönetilen örneği için güvenlik denetimleri
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Bu makale, Azure SQL veritabanı ve Azure SQL yönetilen örneği 'nde yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]



## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Yalnızca [SQL veritabanı](../index.yml) için geçerlidir. |
| Azure sanal ağ ekleme desteği| Evet | Yalnızca [SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) için geçerlidir. |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Hem veritabanı düzeyinde hem de sunucu düzeyinde güvenlik duvarı. Ağ yalıtımı yalnızca [SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) içindir. |
| Zorlamalı tünel desteği| Evet | [ExpressRoute](../expressroute/../index.yml) VPN aracılığıyla [SQL yönetilen örneği](../managed-instance/sql-managed-instance-paas-overview.md) . |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Log Analytics veya Application Insights gibi Azure izleme desteği| Evet | EMVA 'dan SıEM çözümü olan SecureSphere, [SQL denetimi](../../azure-sql/database/auditing-overview.md)aracılığıyla [Azure Event Hubs](../event-hubs/../index.yml) tümleştirme aracılığıyla da desteklenir. |
| Denetim düzlemi ve yönetimi-düzlem günlüğü ve denetimi| Evet | Yalnızca bazı olaylar için Evet |
| Veri düzlemi günlüğü ve denetimi | Evet | [SQL denetimi](../../azure-sql/database/auditing-overview.md) aracılığıyla |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory (Azure AD) |
| Yetkilendirme| Evet | Hiçbiri |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Evet | [Always Encrypted](always-encrypted-certificate-store-configure.md)makalesinde açıklandığı gibi "kullanımda olan şifreleme" olarak adlandırılır. Sunucu tarafı şifreleme, [Saydam veri şifrelemesi](transparent-data-encryption-tde-overview.md)kullanır.|
| Aktarım sırasında şifreleme:<ul><li>Azure ExpressRoute şifrelemesi</li><li>Bir sanal ağda şifreleme</li><li>Sanal ağlar arasında şifreleme</ul>| Evet | HTTPS kullanma. |
| CMK veya BYOK gibi şifreleme anahtarı işleme| Evet | Hem hizmet tarafından yönetilen hem de müşteri tarafından yönetilen anahtar işleme sunulur. İkincisi [Azure Key Vault](../key-vault/../index.yml)aracılığıyla sunulur. |
| Azure veri Hizmetleri tarafından sunulan sütun düzeyinde şifreleme| Evet | [Always Encrypted](always-encrypted-certificate-store-configure.md)üzerinden. |
| Şifrelenmiş API çağrıları| Evet | HTTPS/TLS kullanma. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma sürümü oluşturma gibi yapılandırma yönetimi desteği| Hayır  | Hiçbiri |

## <a name="additional-security-controls-for-sql-database"></a>SQL veritabanı için ek güvenlik denetimleri

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Koruyucu: güvenlik açığı değerlendirmesi | Evet | Bkz. [SQL güvenlik açığı değerlendirmesi hizmeti, veritabanı güvenlik açıklarını belirlemenize yardımcı olur](sql-vulnerability-assessment.md). |
| Koruyucu: veri bulma ve sınıflandırma  | Evet | Bkz. [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırması](data-discovery-and-classification-overview.md). |
| Algılama: tehdit algılama | Evet | Bkz. [Azure SQL veritabanı Için Gelişmiş tehdit koruması](threat-detection-overview.md). |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
