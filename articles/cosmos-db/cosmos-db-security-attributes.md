---
title: Azure Cosmos DB için güvenlik öznitelikleri
description: Azure Cosmos DB değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: cosmos-db
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: f1b578ff70e8a2795ba886e6d0c75eff521f77c3
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69615544"
---
# <a name="security-attributes-for-azure-cosmos-db"></a>Azure Cosmos DB için güvenlik öznitelikleri

Bu makale, Azure Cosmos DB yerleşik güvenlik özniteliklerini belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet | Tüm Cosmos veritabanları ve yedeklemeleri varsayılan olarak şifrelenir; [Azure Cosmos DB veri şifrelemeyi](database-encryption-at-rest.md)inceleyin. Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme desteklenmez. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Tüm Azure Cosmos DB verileri aktarım sırasında şifrelenir. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Evet | Yalnızca API Premium tablolarında. Tüm API 'Ler bu özelliği desteklemez. Bkz [. Azure Cosmos DB giriş: Tablo API'si](table-introduction.md). |
| Şifrelenmiş API çağrıları| Evet | Azure Cosmos DB tüm bağlantıları HTTPS 'yi destekler. Azure Cosmos DB TLS 1,2 bağlantılarını da destekler, ancak bu henüz zorlanmaz. Müşteriler, son ucunda daha düşük düzey TLS 'yi devre dışı bırakır. Cosmos DB bağlandıklarından emin olabilirler.  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Evet | VNet hizmet uç noktası ile, bir Azure Cosmos DB hesabını yalnızca bir sanal ağın (VNet) belirli bir alt ağından erişime izin verecek şekilde yapılandırabilirsiniz. Ayrıca, VNet erişimini güvenlik duvarı kurallarıyla birleştirebilirsiniz.  Bkz. [sanal ağlardan erişim Azure Cosmos DB](VNet-service-endpoint.md). |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Güvenlik Duvarı desteğiyle, Azure Cosmos hesabınızı yalnızca onaylanan bir IP adresi kümesinden, IP adresi aralığına ve/veya bulut hizmetlerinden erişime izin verecek şekilde yapılandırabilirsiniz. Bkz. [Azure Cosmos DB IP güvenlik duvarını yapılandırma](how-to-configure-firewall.md).|
| Zorlamalı tünel desteği| Evet | , Sanal makinelerin bulunduğu VNet üzerinde istemci tarafında yapılandırılabilir.   |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure Cosmos DB gönderilen tüm istekler günlüğe kaydedilir. Azure [izleme](../azure-monitor/overview.md), Azure ölçümleri, Azure denetim günlüğü desteklenir.  Veri düzlemi isteklerine karşılık gelen bilgileri, sorgu çalışma zamanı istatistiklerini, sorgu metnini, MongoDB isteklerini günlüğe kaydedebilirsiniz. Ayrıca, uyarıları da ayarlayabilirsiniz. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Evet, veritabanı hesabı düzeyinde; veri düzlemi düzeyinde Cosmos DB, kaynak belirteçlerini ve anahtar erişimini kullanır. |
| Authorization| Evet | Azure Cosmos hesabında ana anahtarlar (birincil ve ikincil) ve kaynak belirteçleri ile desteklenir. Ana anahtarlarla verilere okuma/yazma veya salt okuma erişimi alabilirsiniz. Kaynak belirteçleri, belgeler ve kapsayıcılar gibi kaynaklara sınırlı zamanlı erişime izin verir. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Güvenlik duvarları, VNET 'ler, anahtar erişimi ve ıAM gibi hesap düzeyindeki işlemler için Azure etkinlik günlüğü. |
| Veri düzlemi günlüğü ve denetimi | Evet | Kapsayıcı oluşturma, sağlama işleme, dizin oluşturma ilkeleri ve belgelerde CRUD işlemleri gibi kapsayıcı düzeyindeki işlemler için Tanılama izleme günlüğü. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Hayır  | | 

## <a name="additional-security-attributes-for-cosmos-db"></a>Cosmos DB için ek güvenlik öznitelikleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Çapraz kaynak kaynak paylaşımı (CORS) | Evet | Bkz. [çıkış noktaları arası kaynak paylaşımı 'nı (CORS) yapılandırma](how-to-configure-cross-origin-resource-sharing.md). |
