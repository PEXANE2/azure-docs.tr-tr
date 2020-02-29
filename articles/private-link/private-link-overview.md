---
title: Azure Özel Bağlantı nedir?
description: Azure özel bağlantı özelliklerine, mimarisine ve uygulamasına genel bakış. Azure özel uç noktaları ve Azure özel bağlantı hizmeti 'nin nasıl çalıştığını ve nasıl kullanılacağını öğrenin.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: overview
ms.date: 02/27/2020
ms.author: allensu
ms.custom: fasttrack-edit
ms.openlocfilehash: 710c5a780841135344d92e93a02f97963b36b09e
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77921399"
---
# <a name="what-is-azure-private-link"></a>Azure Özel Bağlantı nedir? 
Azure özel bağlantısı, şu gibi Azure PaaS hizmetlerine erişmenizi sağlar:
 
 - **Azure depolama alanı**
 - **Azure Cosmos DB**
 - **Azure SQL Veritabanı**

Özel bağlantı, sanal ağınızdaki [özel bir uç nokta](private-endpoint-overview.md) üzerinden barındırılan müşteri ve iş ortağı hizmetlerine erişim sağlar.

Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağını de dolaşır. Hizmetinizi genel İnternet 'e sunma artık gerekli değildir. Kendi [özel bağlantı hizmetinizi](private-link-service-overview.md) sanal ağınızda oluşturabilir ve müşterilerinize iletebilirsiniz. Azure özel bağlantısını kullanarak kurulum ve tüketim, Azure PaaS, müşteriye ait ve paylaşılan iş ortağı hizmetleri arasında tutarlıdır.

> [!IMPORTANT]
> Azure özel bağlantısı artık genel kullanıma sunulmuştur. Hem özel uç nokta hem de özel bağlantı hizmeti (Standart yük dengeleyicinin arkasındaki hizmet) genel kullanıma sunulmuştur. Farklı zamanlamalarda Azure özel bağlantısına farklı Azure PaaS da eklenecektir. Özel bağlantıdaki Azure PaaS 'nin doğru durumu için aşağıdaki [kullanılabilirlik](https://docs.microsoft.com/azure/private-link/private-link-overview#availability) bölümünü denetleyin. Bilinen sınırlamalar için bkz. [Özel uç nokta](private-endpoint-overview.md#limitations) ve [özel bağlantı hizmeti](private-link-service-overview.md#limitations). 

![Özel uç noktaya genel bakış](media/private-link-overview/private-endpoint.png)

## <a name="key-benefits"></a>Önemli avantajlar
Azure özel bağlantısı aşağıdaki avantajları sağlar:  
- **Azure platformunda özel olarak erişim Hizmetleri**: Sanal ağınızı, kaynak veya hedef üzerinde genel bir IP adresi olmadan Azure 'daki hizmetlere bağlayın. Hizmet sağlayıcıları kendi sanal ağındaki hizmetlerini işleyebilir ve tüketiciler kendi yerel sanal ağında bu hizmetlere erişebilir. Özel bağlantı platformu, Azure omurga ağı üzerinden tüketici ve hizmetler arasındaki bağlantıyı işleymeyecektir. 
 
- Şirket **içi ve eşlenmiş ağlar**: Azure 'Da, ExpressRoute özel EŞLEMESI, VPN tünelleri ve eşlenmiş sanal ağlar üzerinden şirket içinden çalışan hizmetlere özel uç noktalar kullanarak erişin. Hizmete ulaşmak için genel eşlemeyi ayarlamanız veya internet 'e geçiş yapmanız gerekmez. Özel bağlantı, iş yüklerini Azure 'a geçirmek için güvenli bir yol sağlar.
 
- **Veri sızıntılarına karşı koruma**: özel bir uç nokta, tüm hizmet yerine bir PaaS kaynağı örneğine eşlenir. Tüketiciler yalnızca belirli bir kaynağa bağlanabilir. Hizmette başka bir kaynağa erişim engellenir. Bu mekanizma, veri sızıntısı risklerine karşı koruma sağlar. 
 
- **Küresel erişim**: diğer bölgelerde çalışan hizmetlere özel olarak bağlanın. Tüketicinin sanal ağı A bölgesinde olabilir ve B bölgesinde özel bağlantı arkasındaki hizmetlere bağlanabilir.  
 
- **Kendi hizmetlerinizi genişletin**: hizmetinizi Azure 'daki tüketicilere özel olarak işlemek için aynı deneyimi ve işlevselliği etkinleştirin. Hizmetinizi standart bir Azure Load Balancer arkasına yerleştirerek özel bağlantı için etkinleştirebilirsiniz. Tüketici daha sonra kendi sanal ağında özel bir uç nokta kullanarak doğrudan hizmetinize bağlanabilir. Bağlantı isteklerini bir onay çağrı akışı kullanarak yönetebilirsiniz. Azure özel bağlantısı, farklı Azure Active Directory kiracılarına ait tüketiciler ve hizmetler için geçerlidir. 

## <a name="availability"></a>Kullanılabilirlik 
 Aşağıdaki tabloda özel bağlantı Hizmetleri ve kullanılabilir oldukları bölgeler listelenmektedir. 

|Senaryo  |Desteklenen hizmetler  |Kullanılabilir bölgeler | Durum  |
|:---------|:-------------------|:-----------------|:--------|
|Müşterinin sahip olduğu hizmetler için özel bağlantı|Standart Azure Load Balancer arkasındaki özel bağlantı Hizmetleri | Tüm ortak bölgeler  | GA <br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/private-link/private-link-service-overview) |
|Azure PaaS hizmetleri için özel bağlantı   | Azure Storage        |  Tüm ortak bölgeler      | Önizleme <br/> [Daha fazla bilgi](/azure/storage/common/storage-private-endpoints)  |
|  | Azure Data Lake Storage Gen2        |  Tüm ortak bölgeler      | Önizleme <br/> [Daha fazla bilgi](/azure/storage/common/storage-private-endpoints)  |
|  |  Azure SQL Veritabanı         | Tüm ortak bölgeler      |   Önizleme <br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)      |
||Azure SQL Veri Ambarı| Tüm ortak bölgeler |Önizleme <br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)|
||Azure Cosmos DB| Orta Batı ABD, WestUS, Orta Kuzey ABD |Önizleme <br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints)|
|  |  PostgreSQL için Azure veritabanı-tek sunucu         | Tüm ortak bölgeler      |   Önizleme <br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-private-link)      |
|  |  MySQL için Azure Veritabanı         | Tüm ortak bölgeler      |   Önizleme <br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)     |
|  |  MariaDB için Azure Veritabanı         | Tüm ortak bölgeler      |   Önizleme <br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link)      |
|  |  Azure Key Vault         | Tüm ortak bölgeler      |   Önizleme   <br/> [Daha fazla bilgi](https://docs.microsoft.com/azure/key-vault/private-link-service)   |

En güncel bildirimler için, [Azure sanal ağ güncelleştirmeleri sayfasını](https://azure.microsoft.com/updates/?product=virtual-network)inceleyin.

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

Azure özel bağlantısı, Azure Izleyici ile tümleştirilmesine sahiptir. Bu bileşim şunları sağlar:

 - Günlükleri bir depolama hesabına arşivleme.
 - Olay Hub 'ınıza olay akışı.
 - Azure Izleyici günlüğü.

Azure Izleyici 'de aşağıdaki bilgilere erişebilirsiniz: 
- **Özel uç nokta**: 
    - Özel uç nokta tarafından işlenen veriler (ın/OUT)
 
- **Özel bağlantı hizmeti**:
    - Özel bağlantı hizmeti tarafından işlenen veriler (ın/OUT)
    - NAT bağlantı noktası kullanılabilirliği  
 
## <a name="pricing"></a>Fiyatlandırma   
Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link/).
 
## <a name="faqs"></a>SSS  
SSS için bkz. [Azure özel bağlantı SSS](private-link-faq.md).
 
## <a name="limits"></a>Sınırlar  
Sınırlar için bkz. [Azure özel bağlantı limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#private-link-limits).

## <a name="service-level-agreement"></a>Hizmet Düzeyi Sözleşmesi
SLA için bkz. [Azure özel bağlantısı Için SLA](https://azure.microsoft.com/support/legal/sla/private-link/v1_0/).

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure portal kullanarak özel uç nokta oluşturma](create-private-endpoint-portal.md)
- [Hızlı başlangıç: Azure portal kullanarak özel bir bağlantı hizmeti oluşturma](create-private-link-service-portal.md)


 
