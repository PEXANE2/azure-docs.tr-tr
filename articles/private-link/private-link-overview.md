---
title: Azure Özel Bağlantı nedir?
description: Azure Özel Bağlantı özelliklerine, mimarisine ve uygulamasına genel bakış. Azure Özel Uç Noktaları ve Azure Özel Bağlantı hizmetinin nasıl çalıştığını ve bunları nasıl kullanacağınızı öğrenin.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: overview
ms.date: 02/27/2020
ms.author: allensu
ms.custom: fasttrack-edit
ms.openlocfilehash: 1bef4e5f4129ddc8300d61d609392ce0b07b74b8
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656246"
---
# <a name="what-is-azure-private-link"></a>Azure Özel Bağlantı nedir? 
Azure Özel Bağlantı, Azure PaaS Hizmetlerine (örneğin, Azure Depolama ve SQL Veritabanı) erişmenizi ve Azure'un sanal [ağınızdaki özel](private-endpoint-overview.md) bir bitiş noktası üzerinden müşteriye ait/iş ortağı hizmetlerine erişmenizi sağlar.

Sanal ağınızla hizmet arasındaki trafik Microsoft omurga ağı arasında seyahat eder. Hizmetinizi genel internete maruz bırakmak artık gerekli değildir. Sanal ağınızda kendi [özel bağlantı hizmetinizi](private-link-service-overview.md) oluşturabilir ve müşterilerinize teslim edebilirsiniz. Azure Private Link'i kullanarak kurulum ve tüketim, Azure PaaS, müşteriye ait ve paylaşılan iş ortağı hizmetleri arasında tutarlıdır.

> [!IMPORTANT]
> Azure Private Link artık genel olarak kullanılabilir. Hem Özel Uç Nokta hem de Özel Bağlantı hizmeti (standart yük dengeleyicisi arkasında hizmet) genellikle mevcuttur. Farklı Azure PaaS, farklı zamanlamalarda Azure Özel Bağlantı'ya binecektir. Azure PaaS'ın Özel Bağlantı'daki doğru durumu için aşağıdaki [kullanılabilirlik](https://docs.microsoft.com/azure/private-link/private-link-overview#availability) bölümünü kontrol edin. Bilinen sınırlamalar için Bkz. [Özel Bitiş Noktası](private-endpoint-overview.md#limitations) ve Özel Bağlantı [Hizmeti.](private-link-service-overview.md#limitations) 

![Özel bitiş noktasına genel bakış](media/private-link-overview/private-endpoint.png)

## <a name="key-benefits"></a>Önemli avantajlar
Azure Private Link aşağıdaki avantajları sağlar:  
- **Azure platformundaki özel erişim hizmetleri**: Sanal ağınızı kaynakta veya hedefte genel bir IP adresi olmadan Azure'daki hizmetlere bağlayın. Hizmet sağlayıcılar hizmetlerini kendi sanal ağlarında verebilir ve tüketiciler bu hizmetlere yerel sanal ağlarında erişebilir. Özel Bağlantı platformu, Azure omurga ağı üzerinden tüketici ve hizmetler arasındaki bağlantıyı ele alacaktır. 
 
- **Şirket içi ve eşlenen ağlar**: Azure'da ExpressRoute özel bakış, VPN tünelleri ve özel uç noktaları kullanarak bakan sanal ağlar üzerinden şirket içinde çalışan hizmetlere erişim. Hizmete ulaşmak için herkese açık bir şekilde bakmanız veya internetten geçiş yapmanıza gerek yoktur. Özel Bağlantı, iş yüklerini Azure'a geçirmek için güvenli bir yol sağlar.
 
- **Veri sızıntısına karşı koruma**: Özel bir bitiş noktası, tüm hizmet yerine bir PaaS kaynağıörneğine eşlenir. Tüketiciler yalnızca belirli kaynağa bağlanabilir. Hizmetteki diğer kaynaklara erişim engellenir. Bu mekanizma veri sızıntısı risklerine karşı koruma sağlar. 
 
- **Genel erişim**: Diğer bölgelerde çalışan hizmetlere özel bağlanın. Tüketicinin sanal ağı A bölgesinde olabilir ve B bölgesindeki Private Link'in arkasındaki hizmetlere bağlanabilir.  
 
- **Kendi hizmetlerinize genişletin**: Hizmetinizi Azure'daki tüketicilere özel olarak vermek için aynı deneyimi ve işlevselliği etkinleştirin. Hizmetinizi standart bir Azure Yük Dengeleyicisi'nin arkasına yerleştirerek, Özel Bağlantı için etkinleştirebilirsiniz. Tüketici daha sonra kendi sanal ağında özel bir bitiş noktası kullanarak doğrudan hizmetinize bağlanabilir. Bir onay çağrısı akışını kullanarak bağlantı isteklerini yönetebilirsiniz. Azure Private Link, farklı Azure Active Directory kiracılarına ait tüketiciler ve hizmetler için çalışır. 

## <a name="availability"></a>Kullanılabilirlik 
 Aşağıdaki tabloda Özel Bağlantı hizmetleri ve kullanılabilir oldukları bölgeler listelenmektedir. 

|Senaryo  |Desteklenen hizmetler  |Kullanılabilir bölgeler | Durum  |
|:---------|:-------------------|:-----------------|:--------|
|Müşteriye ait hizmetler için Özel Bağlantı |Standart Azure Yük Dengeleyicisi'nin arkasındaki Özel Bağlantı hizmetleri | Tüm genel bölgeler  | GA <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/private-link/private-link-service-overview) |
|Azure PaaS hizmetleri için Özel Bağlantı   | Azure Storage        |  Tüm genel bölgeler      | GA <br/> [Daha fazlasını öğrenin](/azure/storage/common/storage-private-endpoints)  |
|  | Azure Data Lake Storage Gen2        |  Tüm genel bölgeler      | GA <br/> [Daha fazlasını öğrenin](/azure/storage/common/storage-private-endpoints)  |
|  |  Azure SQL Veritabanı         | Tüm genel bölgeler      |   GA <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)      |
|  |Azure Synapse Analytics (SQL Veri Ambarı)| Tüm genel bölgeler |GA <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/sql-database/sql-database-private-endpoint-overview)|
|  |Azure Cosmos DB|  Tüm genel bölgeler |GA <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/cosmos-db/how-to-configure-private-endpoints)|
|  |  PostgreSQL için Azure Veritabanı - Tek sunucu         | Tüm genel bölgeler      |   GA <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/postgresql/concepts-data-access-and-security-private-link)      |
|  |  MySQL için Azure Veritabanı         | Tüm genel bölgeler      |   GA <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/mysql/concepts-data-access-security-private-link)     |
|  |  MariaDB için Azure Veritabanı         | Tüm genel bölgeler      |   GA <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-private-link)      |
|  |  Azure Key Vault         | Tüm genel bölgeler      |   GA   <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/key-vault/private-link-service)   |
|  |Azure Kubernetes Servisi - Kubernetes API | Tüm genel bölgeler      |   GA   <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/aks/private-clusters)   |
|  |Azure Search | DOĞU ABD, BATı ABD 2, GÜNEY ORTA ABD |   Önizleme    |
|  |Azure Container Kayıt Defteri | Tüm genel bölgeler      |   Önizleme   |
|  |Azure Uygulama Yapılandırması | Tüm genel bölgeler      |   Önizleme   |
|  |Azure Backup | DOĞU ABD, BATı ABD 2, GÜNEY ORTA ABD     |   Önizleme   |
|  |Azure Event Hub | Tüm genel bölgeler      |   Önizleme    |
|  |Azure Service Bus | Tüm genel bölgeler      |   Önizleme   |
|  |Azure Geçişi | Tüm genel bölgeler      |   Önizleme   |
|  |Azure Event Grid| DOĞU ABD, BATı ABD 2, GÜNEY ORTA ABD      |   Önizleme   <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/event-grid/network-security)   |
|  |Azure Web Apps | DOĞU ABD, BATı ABD 2, GÜNEY ORTA ABD      |   Önizleme   <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/app-service/networking/private-endpoint)   |
|  |Azure Machine Learning | DOĞU ABD, BATı ABD 2, GÜNEY ORTA ABD      |   Önizleme   <br/> [Daha fazlasını öğrenin](https://docs.microsoft.com/azure/machine-learning/how-to-configure-private-link)   |

En güncel bildirimler için [Azure Sanal Ağ güncelleştirmeleri sayfasını](https://azure.microsoft.com/updates/?product=virtual-network)kontrol edin.

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

Azure Private Link'in Azure Monitor ile tümleştirmesi vardır. Bu kombinasyon şunları sağlar:

 - Günlüklerin bir depolama hesabına arşivlemesi.
 - Olayların Etkinlik Merkezinize aktarılaması.
 - Azure Monitör günlük.

Azure Monitor'da aşağıdaki bilgilere erişebilirsiniz: 
- **Özel bitiş noktası**: 
    - Özel Bitiş Noktası (IN/OUT) tarafından işlenen veriler
 
- **Özel Link hizmeti**:
    - Özel Bağlantı hizmeti (IN/OUT) tarafından işlenen veriler
    - NAT bağlantı noktası kullanılabilirliği  
 
## <a name="pricing"></a>Fiyatlandırma   
Fiyatlandırma ayrıntıları için Azure [Özel Bağlantı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/private-link/)bakın.
 
## <a name="faqs"></a>SSS  
SSS'ler için [Azure Özel Bağlantı SSS'lerine](private-link-faq.md)bakın.
 
## <a name="limits"></a>Sınırlar  
Sınırlar için Azure [Özel Bağlantı sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#private-link-limits)bakın.

## <a name="service-level-agreement"></a>Hizmet Düzeyi Sözleşmesi
SLA için [Azure Özel Bağlantısı için SLA'ya](https://azure.microsoft.com/support/legal/sla/private-link/v1_0/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Hızlı başlangıç: Azure portalLarını kullanarak Özel Bitiş Noktası Oluşturma](create-private-endpoint-portal.md)
- [Hızlı başlangıç: Azure portalını kullanarak Özel Bağlantı hizmeti oluşturma](create-private-link-service-portal.md)


 
