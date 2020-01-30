---
title: Azure Cosmos DB için güvenlik denetimleri
description: Azure Cosmos DB değerlendirmek için ağ, izleme, kimlik ve veri koruması gibi güvenlik denetimlerinin denetim listesini alın
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: sngun
ms.openlocfilehash: d06afb3670b1c3af3f1acf0bd690c1e3e266a11c
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76771801"
---
# <a name="security-controls-for-azure-cosmos-db"></a>Azure Cosmos DB için güvenlik denetimleri

Bu makalede, Azure Cosmos DB yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Evet | VNet hizmet uç noktası ile, bir Azure Cosmos DB hesabını yalnızca bir sanal ağın (VNet) belirli bir alt ağından erişime izin verecek şekilde yapılandırabilirsiniz. Ayrıca, VNet erişimini güvenlik duvarı kurallarıyla birleştirebilirsiniz. Daha fazla bilgi için bkz. [sanal ağlardan erişim Azure Cosmos DB](VNet-service-endpoint.md). |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Güvenlik Duvarı desteğiyle, Azure Cosmos hesabınızı yalnızca onaylanan bir IP adresi kümesinden, IP adresi aralığına ve/veya bulut hizmetlerinden erişime izin verecek şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure Cosmos DB IP güvenlik duvarını yapılandırma](how-to-configure-firewall.md).|
| Zorlamalı tünel desteği| Evet | , Sanal makinelerin bulunduğu VNet üzerinde istemci tarafında yapılandırılabilir.   |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure Cosmos DB gönderilen tüm istekler günlüğe kaydedilir. Azure [izleme](../azure-monitor/overview.md), Azure ölçümleri, Azure denetim günlüğü desteklenir.  Veri düzlemi isteklerine karşılık gelen bilgileri, sorgu çalışma zamanı istatistiklerini, sorgu metnini, MongoDB isteklerini günlüğe kaydedebilirsiniz. Ayrıca, uyarıları da ayarlayabilirsiniz. |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Güvenlik duvarları, VNET 'ler, anahtar erişimi ve ıAM gibi hesap düzeyindeki işlemler için Azure etkinlik günlüğü. |
| Veri düzlemi günlüğü ve denetimi | Evet | Kapsayıcı oluşturma, sağlama işleme, dizin oluşturma ilkeleri ve belgelerde CRUD işlemleri gibi kapsayıcı düzeyindeki işlemler için Tanılama izleme günlüğü. |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Evet, veritabanı hesabı düzeyinde; veri düzlemi düzeyinde Cosmos DB, kaynak belirteçlerini ve anahtar erişimini kullanır. |
| Yetkilendirme| Evet | Azure Cosmos hesabında ana anahtarlar (birincil ve ikincil) ve kaynak belirteçleri ile desteklenir. Ana anahtarlarla verilere okuma/yazma veya salt okuma erişimi alabilirsiniz. Kaynak belirteçleri, belgeler ve kapsayıcılar gibi kaynaklara sınırlı zamanlı erişime izin verir. |

## <a name="data-protection"></a>Veri koruması

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Evet | Tüm Azure Cosmos veritabanları ve yedeklemeleri varsayılan olarak şifrelenir; [Azure Cosmos DB veri şifrelemeyi](database-encryption-at-rest.md)inceleyin. |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Bkz. [Azure Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandırma](how-to-setup-cmk.md)  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Evet | Yalnızca API Premium tablolarında. Tüm API 'Ler bu özelliği desteklemez. Bkz. [Azure Cosmos DB giriş: tablo API'si](table-introduction.md). |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Tüm Azure Cosmos DB verileri aktarım sırasında şifrelenir. |
| Şifrelenmiş API çağrıları| Evet | Azure Cosmos DB tüm bağlantıları HTTPS 'yi destekler. Azure Cosmos DB ayrıca TLS 1,2 ' i destekler.<br>En düşük TLS sürümü sunucu tarafını zorlamak mümkündür. Bunu yapmak için lütfen [azurecosmosdbtls@service.microsoft.com](mailto:azurecosmosdbtls@service.microsoft.com)başvurun. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Hayır  | | 

## <a name="additional-security-controls-for-cosmos-db"></a>Cosmos DB için ek güvenlik denetimleri

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Çapraz kaynak kaynak paylaşımı (CORS) | Evet | Bkz. [çıkış noktaları arası kaynak paylaşımı 'nı (CORS) yapılandırma](how-to-configure-cross-origin-resource-sharing.md). |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.