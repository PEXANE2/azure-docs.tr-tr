---
title: Azure Özel Bağlantı nedir?
description: Azure özel bağlantı özelliklerine, mimarisine ve uygulamasına genel bakış. Azure özel uç noktaları ve Azure özel bağlantı hizmeti 'nin nasıl çalıştığını ve nasıl kullanılacağını öğrenin.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: overview
ms.date: 09/03/2020
ms.author: allensu
ms.custom: fasttrack-edit, references_regions
ms.openlocfilehash: 974631f9cba66404a3881caed1382be7ff3c00d1
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98881342"
---
# <a name="what-is-azure-private-link"></a>Azure Özel Bağlantı nedir? 
Azure özel bağlantısı, Azure PaaS hizmetlerine (örneğin, Azure depolama ve SQL veritabanı) ve Azure 'da barındırılan, müşteriye ait/iş ortağı hizmetlerine sanal ağınızdaki özel bir [uç nokta](private-endpoint-overview.md) üzerinden erişmenizi sağlar.

Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağını de dolaşır. Hizmetinizi genel İnternet 'e sunma artık gerekli değildir. Kendi [özel bağlantı hizmetinizi](private-link-service-overview.md) sanal ağınızda oluşturabilir ve müşterilerinize iletebilirsiniz. Azure özel bağlantısını kullanarak kurulum ve tüketim, Azure PaaS, müşteriye ait ve paylaşılan iş ortağı hizmetleri arasında tutarlıdır.

> [!IMPORTANT]
> Azure özel bağlantısı artık genel kullanıma sunulmuştur. Hem özel uç nokta hem de özel bağlantı hizmeti (Standart yük dengeleyicinin arkasındaki hizmet) genel kullanıma sunulmuştur. Farklı zamanlamalarda Azure özel bağlantısına farklı Azure PaaS da eklenecektir. Özel bağlantıdaki Azure PaaS 'nin doğru durumu için bu makaledeki [kullanılabilirliği](#availability) denetle bölümüne bakın. Bilinen sınırlamalar için bkz. [Özel uç nokta](private-endpoint-overview.md#limitations) ve [özel bağlantı hizmeti](private-link-service-overview.md#limitations). 

:::image type="content" source="./media/private-link-overview/private-link-center.png" alt-text="Azure özel bağlantı merkezi Azure portal" border="false":::

## <a name="key-benefits"></a>Önemli avantajlar
Azure özel bağlantısı aşağıdaki avantajları sağlar:  
- **Azure platformunda özel olarak erişim Hizmetleri**: Sanal ağınızı, kaynak veya hedef üzerinde genel bir IP adresi olmadan Azure 'daki hizmetlere bağlayın. Hizmet sağlayıcıları kendi sanal ağındaki hizmetlerini işleyebilir ve tüketiciler kendi yerel sanal ağında bu hizmetlere erişebilir. Özel bağlantı platformu, Azure omurga ağı üzerinden tüketici ve hizmetler arasındaki bağlantıyı işleymeyecektir. 
 
- Şirket **içi ve eşlenmiş ağlar**: Azure 'Da, ExpressRoute özel EŞLEMESI, VPN tünelleri ve eşlenmiş sanal ağlar üzerinden şirket içinden çalışan hizmetlere özel uç noktalar kullanarak erişin. ExpressRoute Microsoft eşlemesi 'ni yapılandırmaya veya internet 'e ulaşmaya gerek yoktur. Özel bağlantı, iş yüklerini Azure 'a geçirmek için güvenli bir yol sağlar.
 
- **Veri sızıntılarına karşı koruma**: özel bir uç nokta, tüm hizmet yerine bir PaaS kaynağı örneğine eşlenir. Tüketiciler yalnızca belirli bir kaynağa bağlanabilir. Hizmette başka bir kaynağa erişim engellenir. Bu mekanizma, veri sızıntısı risklerine karşı koruma sağlar. 
 
- **Küresel erişim**: diğer bölgelerde çalışan hizmetlere özel olarak bağlanın. Tüketicinin sanal ağı A bölgesinde olabilir ve B bölgesinde özel bağlantı arkasındaki hizmetlere bağlanabilir.  
 
- **Kendi hizmetlerinizi genişletin**: hizmetinizi Azure 'daki tüketicilere özel olarak işlemek için aynı deneyimi ve işlevselliği etkinleştirin. Hizmetinizi standart bir Azure Load Balancer arkasına yerleştirerek özel bağlantı için etkinleştirebilirsiniz. Tüketici daha sonra kendi sanal ağında özel bir uç nokta kullanarak doğrudan hizmetinize bağlanabilir. Bağlantı isteklerini bir onay çağrı akışı kullanarak yönetebilirsiniz. Azure özel bağlantısı, farklı Azure Active Directory kiracılarına ait tüketiciler ve hizmetler için geçerlidir. 

## <a name="availability"></a>Kullanılabilirlik 
 Aşağıdaki tabloda özel bağlantı Hizmetleri ve kullanılabilir oldukları bölgeler listelenmektedir. 

|Desteklenen hizmetler  |Kullanılabilir bölgeler | Diğer konular | Durum  |
|:-------------------|:-----------------|:----------------|:--------|
|Standart Azure Load Balancer arkasındaki özel bağlantı Hizmetleri | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri<br/>Tüm Çin bölgeleri  | Standart Load Balancer destekleniyor | GA <br/> [Özel bağlantı hizmeti oluşturmayı öğrenin.](create-private-link-service-portal.md) |
| Azure Blob depolama (Data Lake Storage 2. dahil)       |  Tüm ortak bölgeler<br/> Tüm kamu bölgeleri       |  Hesap türü Genel Amaçlı v2 'de desteklenir | GA <br/> [BLOB depolama için özel bir uç nokta oluşturmayı öğrenin.](tutorial-private-endpoint-storage-portal.md)  |
| Azure Dosyaları | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri      | |   GA <br/> [Azure dosyaları ağ uç noktaları oluşturmayı öğrenin.](../storage/files/storage-files-networking-endpoints.md)   |
| Azure Dosya Eşitleme | Tüm ortak bölgeler      | |   GA <br/> [Azure dosyaları ağ uç noktaları oluşturmayı öğrenin.](../storage/files/storage-sync-files-networking-endpoints.md)   |
| Azure Kuyruk Depolama       |  Tüm ortak bölgeler<br/> Tüm kamu bölgeleri       |  Hesap türü Genel Amaçlı v2 'de desteklenir | GA <br/> [Kuyruk depolaması için özel bir uç nokta oluşturmayı öğrenin.](tutorial-private-endpoint-storage-portal.md) |
| Azure Tablo depolama       |  Tüm ortak bölgeler<br/> Tüm kamu bölgeleri       |  Hesap türü Genel Amaçlı v2 'de desteklenir | GA <br/> [Tablo depolaması için özel bir uç nokta oluşturmayı öğrenin.](tutorial-private-endpoint-storage-portal.md)  |
|  Azure SQL Veritabanı         | Tüm ortak bölgeler <br/> Tüm kamu bölgeleri<br/>Tüm Çin bölgeleri      |  Ara sunucu [bağlantı ilkesi](../azure-sql/database/connectivity-architecture.md#connection-policy) için desteklenir | GA <br/> [Azure SQL için özel uç nokta oluşturmayı öğrenin](create-private-endpoint-portal.md)      |
|Azure Synapse Analytics| Tüm ortak bölgeler <br/> Tüm kamu bölgeleri |  Ara sunucu [bağlantı ilkesi](../azure-sql/database/connectivity-architecture.md#connection-policy) için desteklenir |GA <br/> [Azure SYNAPSE Analytics için özel uç nokta oluşturmayı öğrenin.](../azure-sql/database/private-endpoint-overview.md)|
|Azure Cosmos DB|  Tüm ortak bölgeler<br/> Tüm kamu bölgeleri</br> Tüm Çin bölgeleri | |GA <br/> [Cosmos DB için özel uç nokta oluşturmayı öğrenin.](./tutorial-private-endpoint-cosmosdb-portal.md)|
|  PostgreSQL için Azure veritabanı-tek sunucu         | Tüm ortak bölgeler <br/> Tüm kamu bölgeleri<br/>Tüm Çin bölgeleri     | Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarında desteklenir | GA <br/> [PostgreSQL için Azure veritabanı için özel uç nokta oluşturmayı öğrenin.](../postgresql/concepts-data-access-and-security-private-link.md)      |
|  MySQL için Azure Veritabanı         | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri<br/>Tüm Çin bölgeleri      |  | GA <br/> [MySQL için Azure veritabanı için özel bir uç nokta oluşturmayı öğrenin.](../mysql/concepts-data-access-security-private-link.md)     |
|  MariaDB için Azure Veritabanı         | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri<br/>Tüm Çin bölgeleri     |  | GA <br/> [MariaDB için Azure veritabanı için özel bir uç nokta oluşturmayı öğrenin.](../mariadb/concepts-data-access-security-private-link.md)      |
|  Azure Key Vault         | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri      |  | GA   <br/> [Azure Key Vault için özel uç nokta oluşturmayı öğrenin.](../key-vault/general/private-link-service.md)   |
|Azure Kubernetes hizmeti-Kubernetes API 'SI | Tüm ortak bölgeler      |  | GA   <br/> [Azure Kubernetes hizmeti için özel bir uç nokta oluşturmayı öğrenin.](../aks/private-clusters.md)   |
|Azure Search | Tüm ortak bölgeler <br/> Tüm kamu bölgeleri | Özel modda hizmet ile desteklenir | GA   <br/> [Azure Search için özel uç nokta oluşturmayı öğrenin.](../search/service-create-private-endpoint.md)    |
|Azure Container Registry | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri    | Kapsayıcı kayıt defterinin Premium katmanıyla desteklenir. [Katmanlar için seçin](../container-registry/container-registry-skus.md)| GA   <br/> [Azure Container Registry için özel uç nokta oluşturmayı öğrenin.](../container-registry/container-registry-private-link.md)   |
|Azure Uygulama Yapılandırması | Tüm ortak bölgeler      |  | Önizleme  </br> [Azure Uygulama yapılandırması için özel bir uç nokta oluşturmayı öğrenin](../azure-app-configuration/concept-private-endpoint.md) |
|Azure Backup | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri   |  | GA   <br/> [Azure Backup için özel uç nokta oluşturmayı öğrenin.](../backup/private-endpoints.md)   |
|Azure Event Hub | Tüm ortak bölgeler<br/>Tüm kamu bölgeleri      |   | GA   <br/> [Azure Olay Hub 'ı için özel bir uç nokta oluşturmayı öğrenin.](../event-hubs/private-link-service.md)  |
|Azure Service Bus | Tüm ortak bölgeler<br/>Tüm kamu bölgeleri  | Azure Service Bus Premium katmanıyla desteklenir. [Katmanlar için seçin](../service-bus-messaging/service-bus-premium-messaging.md) | GA   <br/> [Azure Service Bus için özel uç nokta oluşturmayı öğrenin.](../service-bus-messaging/private-link-service.md)    |
|Azure Geçişi | Tüm ortak bölgeler      |  | Önizleme <br/> [Azure Relay için özel uç nokta oluşturmayı öğrenin.](../azure-relay/private-link-service.md)  |
|Azure Event Grid| Tüm ortak bölgeler<br/> Tüm kamu bölgeleri       |  | GA   <br/> [Azure Event Grid için özel uç nokta oluşturmayı öğrenin.](../event-grid/network-security.md) |
|Azure Web Apps | Tüm ortak bölgeler      | PremiumV2, PremiumV3 veya Function Premium planıyla desteklenir  | GA   <br/> [Azure Web Apps için özel bir uç nokta oluşturmayı öğrenin.](./tutorial-private-endpoint-webapp-portal.md)   |
|Azure Machine Learning | Tüm ortak bölgeler    |  | GA   <br/> [Azure Machine Learning için özel uç nokta oluşturmayı öğrenin.](../machine-learning/how-to-configure-private-link.md)   |
| Azure Otomasyonu  | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri |  | Önizleme </br> [Azure Otomasyonu için özel bir uç nokta oluşturmayı öğrenin.](../automation/how-to/private-link-security.md)| |
| Azure IoT Hub | Tüm ortak bölgeler    |  | GA   <br/> [Azure IoT Hub için özel bir uç nokta oluşturmayı öğrenin.](../iot-hub/virtual-network-support.md) |
| Azure SignalR | DOĞU ABD, GÜNEY ORTA ABD,<br/>Batı ABD 2, tüm Çin bölgeleri      |  | Önizleme   <br/> [Azure SignalR için özel uç nokta oluşturmayı öğrenin.](../azure-signalr/howto-private-endpoints.md)   |
| Azure İzleyici <br/>(Log Analytics & Application Insights) | Tüm ortak bölgeler      |  | GA   <br/> [Azure Izleyici için özel bir uç nokta oluşturmayı öğrenin.](../azure-monitor/platform/private-link-security.md)   | 
| Azure Batch | Dışındaki tüm ortak bölgeler: Almanya Orta, Almanya kuzeydoğu <br/> Tüm kamu bölgeleri  | | GA <br/> [Azure Batch için özel uç nokta oluşturmayı öğrenin.](../batch/private-connectivity.md) |
|Azure Data Factory | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri<br/>Tüm Çin bölgeleri    | Kimlik bilgilerinin bir Azure Anahtar Kasası 'nda depolanması gerekir| GA   <br/> [Azure Data Factory için özel uç nokta oluşturmayı öğrenin.](../data-factory/data-factory-private-link.md)   |
|Azure Yönetilen Diskleri | Tüm ortak bölgeler<br/> Tüm kamu bölgeleri<br/>Tüm Çin bölgeleri    | [Bilinen sınırlamalar için buraya tıklayın](../virtual-machines/disks-enable-private-links-for-import-export-portal.md#limitations) | GA   <br/> [Azure yönetilen diskler için özel bir uç nokta oluşturmayı öğrenin.](../virtual-machines/disks-enable-private-links-for-import-export-portal.md)   |



En güncel bildirimler için, [Azure özel bağlantı güncelleştirmeleri sayfasına](https://azure.microsoft.com/updates/?product=private-link)bakın.

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

Azure özel bağlantısı, Azure Izleyici ile tümleştirilmesine sahiptir. Bu bileşim şunları sağlar:

 - Günlükleri bir depolama hesabına arşivleme.
 - Olay Hub 'ınıza olay akışı.
 - Azure İzleyici ile günlüğe kaydetme.

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