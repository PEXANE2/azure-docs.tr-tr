---
title: Azure SQL veritabanı için güvenlik öznitelikleri
description: Azure SQL veritabanı 'nı değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 1318b3e433224b009b76458b12e82c9bcf94bb7a
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444386"
---
# <a name="security-attributes-for-azure-sql-database"></a>Azure SQL veritabanı için güvenlik öznitelikleri

Bu makale, Azure SQL veritabanı 'nda yerleşik olarak bulunan güvenlik özniteliklerini belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

SQL veritabanı hem [tek veritabanı](sql-database-single-index.yml) hem de [yönetilen örnek](sql-database-managed-instance.md)içerir. Aşağıdaki girişler, aksi belirtilmedikçe her iki teklife de uygulanır.

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme</li><li>İstemci tarafı veya Always Encrypted gibi diğer şifreleme özellikleri</ul>| Evet | [Always Encrypted](sql-database-always-encrypted.md)makalesinde açıklandığı gibi "kullanımda olan şifreleme" olarak adlandırılır. Sunucu tarafı şifreleme, [Saydam veri şifrelemesi](transparent-data-encryption-azure-sql.md)kullanır.|
| Aktarım sırasında şifreleme:<ul><li>Azure ExpressRoute şifrelemesi</li><li>Bir sanal ağda şifreleme</li><li>Sanal ağlar arasında şifreleme</ul>| Evet | HTTPS kullanma. |
| CMK veya BYOK gibi şifreleme anahtarı işleme| Evet | Hem hizmet tarafından yönetilen hem de müşteri tarafından yönetilen anahtar işleme sunulur. İkincisi [Azure Key Vault](../key-vault/index.yml)aracılığıyla sunulur. |
| Azure veri Hizmetleri tarafından sunulan sütun düzeyinde şifreleme| Evet | [Always Encrypted](sql-database-always-encrypted.md)üzerinden. |
| Şifrelenmiş API çağrıları| Evet | HTTPS/SSL kullanma. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Yalnızca [tek veritabanı](sql-database-single-index.yml) için geçerlidir. |
| Azure sanal ağ ekleme desteği| Evet | Yalnızca [yönetilen örnek](sql-database-managed-instance.md) için geçerlidir. |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Hem veritabanı düzeyinde hem de sunucu düzeyinde güvenlik duvarı. Ağ yalıtımı yalnızca [yönetilen örnek](sql-database-managed-instance.md) içindir. |
| Zorlamalı tünel desteği| Evet | [ExpressRoute](../expressroute/index.yml) VPN aracılığıyla [yönetilen örnek](sql-database-managed-instance.md) . |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Log Analytics veya Application Insights gibi Azure izleme desteği| Evet | EMVA 'dan SıEM çözümü olan SecureSphere, [SQL denetimi](sql-database-auditing.md)aracılığıyla [Azure Event Hubs](../event-hubs/index.yml) tümleştirme aracılığıyla da desteklenir. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Azure Active Directory (Azure AD) |
| Authorization| Evet | Yok. |

## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim düzlemi ve yönetimi-düzlem günlüğü ve denetimi| Evet | Yalnızca bazı olaylar için Evet |
| Veri düzlemi günlüğü ve denetimi | Evet | [SQL denetimi](sql-database-auditing.md) aracılığıyla |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma sürümü oluşturma gibi yapılandırma yönetimi desteği| Hayır  | Yok. |

## <a name="additional-security-attributes-for-sql-database"></a>SQL veritabanı için ek güvenlik öznitelikleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Koruyucu: güvenlik açığı değerlendirmesi | Evet | Bkz. [SQL güvenlik açığı değerlendirmesi hizmeti, veritabanı güvenlik açıklarını belirlemenize yardımcı olur](sql-vulnerability-assessment.md). |
| Koruyucu: veri bulma ve sınıflandırma  | Evet | Bkz. [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırması](sql-database-data-discovery-and-classification.md). |
| Algılama: tehdit algılama | Evet | Bkz. [Azure SQL veritabanı Için Gelişmiş tehdit koruması](sql-database-threat-detection-overview.md). |
