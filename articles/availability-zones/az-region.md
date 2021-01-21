---
title: Kullanılabilirlik Alanlarını destekleyen Azure Hizmetleri
description: Azure 'da yüksek oranda kullanılabilir ve dayanıklı uygulamalar oluşturmak için, kaynaklarınızı çalıştırmak için kullanabileceğiniz fiziksel olarak ayrı konumlar sağlayın Kullanılabilirlik Alanları.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 12/17/2020
ms.author: cynthn
ms.custom: fasttrack-edit, mvc, references_regions
ms.openlocfilehash: ea21a56249c8eaa7c6b329b755e96b21b91ebaa1
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632317"
---
# <a name="azure-services-that-support-availability-zones"></a>Kullanılabilirlik Alanlarını destekleyen Azure Hizmetleri

Kullanılabilirlik Alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Kullanılabilirlik Alanları destekleyen mevcut ve yaklaşan bölgelerin listesi için bkz. [Azure 'Da bölgeler ve kullanılabilirlik alanları](az-overview.md).  

Bu bölümde Kullanılabilirlik Alanları destekleyen Azure hizmetleri listelenmektedir. 

Her bölgede kullanılabilir olan hizmetler, kullanılabilirlik için yaklaşan yol haritası ile birlikte [bölge tarafından kullanılabilen ürünlerde](https://azure.microsoft.com/global-infrastructure/services/)bulunabilir.

Tüm Azure Yönetim Hizmetleri bölge düzeyindeki hatalardan dayanıklı olacak şekilde tasarlanmıştır. Başarısızlık kapsamında, bir bölgedeki bir veya daha fazla kullanılabilirlik bölgesi hatası, tüm bölge arızasına kıyasla daha küçük bir hata Radius içermelidir. Azure, bir bölgedeki yönetim hizmetlerinin bölge düzeyindeki başarısızlığından kurtudan kurtarır. Azure, bir bölgedeki Kullanılabilirlik Alanları arasında dağıtılan müşteri kaynaklarını etkileyen tüm sorunları engellemek için bir bölge içinde tek seferde kritik bakım gerçekleştirir.


![3 bölge içeren bir Azure bölgesinin kavramsal görünümü](./media/az-region/azure-region-availability-zones.png)


Kullanılabilirlik alanları destekleyen Azure hizmetleri üç kategoride yer almalıdır: **Bölgesel**, bölgesel olarak **yedekli** ve **bölgesel olmayan** hizmetler. Müşteri iş yükleri, uygulama performansını ve dayanıklılığı karşılamak üzere bu mimari senaryolarından herhangi birini kullanacak şekilde kategorilere ayrılmıştır.

- **ZGen Hizmetleri** – daha sıkı gecikme veya performans gereksinimlerine ulaşmak için bir kaynak belirli, otomatik olarak seçilen bir kullanılabilirlik bölgesine dağıtılabilir.  Dayanıklılık, uygulamalar ve veriler bölge içindeki bir veya daha fazla bölgeye çoğaltılarak kendi kendine tasarlanmıştır.  Kaynaklar belirli bir bölgeye sabitlenebilir. Örneğin, sanal makineler, yönetilen diskler veya standart IP adresleri belirli bir bölgeye sabitlenebilir ve bu sayede bir veya daha fazla kaynak örneği bölgelere yayılarak artan esnekliği.

- Bölgesel olarak **yedekli hizmetler** – Azure platformu, kaynakları ve verileri bölgeler arasında çoğaltır.  Azure, bölge içindeki örnekleri otomatik olarak çoğalttığından ve dağıttığından yüksek kullanılabilirlik teslimini yönetir.  ZRS, örneğin, bir bölge hatası verilerin HA 'sini etkilemeyecek şekilde verileri üç bölge arasında çoğaltır. 

- **Bölgesel olmayan hizmetler** – belirli bir Azure bölgesine bağımlılığı olmayan, bölge genelinde kesintiler ve bölge genelinde kesintiler için esnek hale getiren hizmetler.


Azure 'da kapsamlı iş sürekliliği elde etmek için Azure bölge çiftleriyle Kullanılabilirlik Alanları birleşimini kullanarak uygulama mimarinizi geliştirin. Uygulamalarınızı ve verilerinizi Azure bölgesindeki Kullanılabilirlik Alanları kullanarak yüksek kullanılabilirlik ve olağanüstü durum kurtarma koruması için Azure bölgelerinde zaman uyumsuz olarak çoğaltma yapabilirsiniz. Daha fazla bilgi edinmek için [kullanılabilirlik alanları kullanarak yüksek kullanılabilirlik için çözüm oluşturma](https://docs.microsoft.com/azure/architecture/high-availability/building-solutions-for-high-availability)makalesini okuyun. 


### <a name="azure-services-supporting-availability-zones"></a>Kullanılabilirlik Alanları destekleyen Azure hizmetleri

- Daha eski nesil sanal makineler aşağıda listelenmez. Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../virtual-machines/sizes-previous-gen.md).

- Bazı hizmetler bölgesel değildir; daha fazla bilgi için bkz. [Azure 'Da bölgeler ve kullanılabilirlik alanları](az-overview.md) . Bu hizmetlerin, belirli bir Azure bölgesine bağımlılığı yoktur, bu da bölge genelinde kesintiler ve bölge genelinde kesintiler için esnek hale getirir.  Bölgesel olmayan hizmetlerin listesi, [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/)' de bulunabilir.



## <a name="americas"></a>Kuzey ve Güney Amerika

| **Ürünler** | **Central US** | **Doğu ABD** | **Doğu ABD 2** | **Batı ABD 2** | **Orta Kanada** |
|--|--|--|--|--|--|
| **İşlem** |  |  |  |  |  |
| [App Service ortamları (ıLB)](../app-service/environment/zone-redundancy.md#how-to-deploy-an-app-service-environment-in-an-availability-zone) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Service Fabric](../service-fabric/service-fabric-cross-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Makine Ölçek Kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Makineler](https://azure.microsoft.com/services/virtual-machines/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | 
| **Kapsayıcılar** |  |  |  |
| [Azure Kubernetes Service (AKS)](../aks/availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Container Registry](../container-registry/zone-redundancy.md) |  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  | 
| **Depolama** |  |  |  |  |  |
| [Azure Data Lake Storage 2. Nesil](../storage/common/storage-account-create.md?tabs=azure-portal)  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Premium dosya depolaması](../storage/files/storage-files-planning.md) |  | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |
| [Blob Depolama](../storage/blobs/storage-blobs-introduction.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Yönetilen Diskler](https://azure.microsoft.com/en-gb/updates/azure-managed-snapshots-images-ga/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Ağ** |  |  |  |  |  |
| [Application Gateway v2](../application-gateway/application-gateway-autoscaling-zone-redundant.md)  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Express Route](../expressroute/designing-for-high-availability-with-expressroute.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Güvenlik Duvarı](../firewall/deploy-availability-zone-powershell.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Standart IP adresi](../virtual-network/public-ip-addresses.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Load Balancer](../load-balancer/load-balancer-standard-availability-zones.md#concepts) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal ağ](../vpn-gateway/create-zone-redundant-vnet-gateway.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal ağ NAT](../virtual-network/nat-gateway-resource.md#availability-zones) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal WAN](../virtual-wan/virtual-wan-faq.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [VPN Gateway](../vpn-gateway/about-zone-redundant-vnet-gateways.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Veritabanları** |  |  |  |  |  |
| [Redis için Azure Önbelleği](../azure-cache-for-redis/cache-overview.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Cosmos DB](../cosmos-db/high-availability.md#availability-zone-support) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Veri Gezgini](/azure/data-explorer/create-cluster-database-portal) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [MySQL için Azure veritabanı-esnek sunucu](../mysql/flexible-server/concepts-high-availability.md) | sayı | sayı | :heavy_check_mark: | :heavy_check_mark: | sayı |
| [PostgreSQL için Azure veritabanı-esnek sunucu](../postgresql/flexible-server/overview.md) | sayı | sayı | :heavy_check_mark: | :heavy_check_mark: | sayı |
| [Azure SQL veritabanı (Genel Amaçlı katmanı)](../azure-sql/database/high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview) | sayı | : heavy_check_mark: (Önizleme) | : heavy_check_mark: (Önizleme) | : heavy_check_mark: (Önizleme) | sayı |
| [Azure SQL veritabanı (Premium & İş Açısından Kritik katmanları)](../azure-sql/database/high-availability-sla.md#premium-and-business-critical-service-tier-zone-redundant-availability) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analiz** |  |  |  |  |  |
| [Event Hubs](../event-hubs/index.yml) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Tümleştirme** |  |  |  |  |  |
| [Event Grid](../event-grid/index.yml) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Service Bus](../service-fabric/service-fabric-cross-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Yönetim ve Idare** |  |  |  |  |  |
| [Ağ İzleyicisi](../network-watcher/frequently-asked-questions.md) | sayı | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | sayı |
| **Güvenlik** |  |  |  |  |  |
| [Azure Active Directory Domain Services](../active-directory-domain-services/overview.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |

## <a name="europe"></a>Avrupa

| **Ürünler** | **Orta Fransa** | **Kuzey Avrupa** | **Güney Birleşik Krallık** | **West Europe** |
|--|--|--|--|--|
| **İşlem** |  |  |  |  |
| [App Service ortamları (ıLB)](../app-service/environment/zone-redundancy.md#how-to-deploy-an-app-service-environment-in-an-availability-zone) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Kubernetes Service (AKS)](../aks/availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Service Fabric](../service-fabric/service-fabric-cross-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Makine Ölçek Kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Makineler](https://azure.microsoft.com/services/virtual-machines/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Depolama** |  |  |  |  |
| [Azure Data Lake Storage 2. Nesil](../storage/common/storage-account-create.md?tabs=azure-portal)  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Premium dosya depolaması](../storage/files/storage-files-planning.md) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Blob Depolama](../storage/blobs/storage-blobs-introduction.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Yönetilen Diskler](https://azure.microsoft.com/en-gb/updates/azure-managed-snapshots-images-ga/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Ağ** |  |  |  |  |
| [Application Gateway v2](../application-gateway/application-gateway-autoscaling-zone-redundant.md)  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Express Route](../expressroute/designing-for-high-availability-with-expressroute.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Güvenlik Duvarı](../firewall/deploy-availability-zone-powershell.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Standart IP adresi](../virtual-network/public-ip-addresses.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Load Balancer](../load-balancer/load-balancer-standard-availability-zones.md#concepts) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal ağ](../vpn-gateway/create-zone-redundant-vnet-gateway.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal ağ NAT](../virtual-network/nat-gateway-resource.md#availability-zones) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal WAN](../virtual-wan/virtual-wan-faq.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [VPN Gateway](../vpn-gateway/about-zone-redundant-vnet-gateways.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Veritabanları** |  |  |  |  |
| [Redis için Azure Önbelleği](../azure-cache-for-redis/cache-overview.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Cosmos DB](../cosmos-db/high-availability.md#availability-zone-support) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Veri Gezgini](/azure/data-explorer/create-cluster-database-portal) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [MySQL için Azure veritabanı-esnek sunucu](../mysql/flexible-server/concepts-high-availability.md) | sayı | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [PostgreSQL için Azure veritabanı-esnek sunucu](../postgresql/flexible-server/overview.md) | sayı | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure SQL veritabanı (Genel Amaçlı katmanı)](../azure-sql/database/high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview) | : heavy_check_mark: (Önizleme) | : heavy_check_mark: (Önizleme) | sayı | : heavy_check_mark: (Önizleme) |
| [Azure SQL veritabanı (Premium & İş Açısından Kritik katmanları)](../azure-sql/database/high-availability-sla.md#premium-and-business-critical-service-tier-zone-redundant-availability) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analiz** |  |  |  |  |
| [Event Hubs](../event-hubs/index.yml) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Tümleştirme** |  |  |  |  |
| [Event Grid](../event-grid/index.yml) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Service Bus](../service-fabric/service-fabric-cross-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Yönetim ve Idare** |  |  |  |  |
| [Ağ İzleyicisi](../network-watcher/frequently-asked-questions.md) | :heavy_check_mark: | :heavy_check_mark: | sayı | :heavy_check_mark: |
| **Güvenlik** |  |  |  |  |
| [Azure Active Directory Domain Services](../active-directory-domain-services/overview.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |

## <a name="asia-pacific"></a>Asya Pasifik



| **Ürünler** | **Doğu Japonya** | **Güneydoğu Asya** | **Doğu Avustralya** |
|--|--|--|--|
| **İşlem** |  |  |  |
| [App Service ortamları (ıLB)](../app-service/environment/zone-redundancy.md#how-to-deploy-an-app-service-environment-in-an-availability-zone) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Kubernetes Service (AKS)](../aks/availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Service Fabric](../service-fabric/service-fabric-cross-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Makine Ölçek Kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Makineler](https://azure.microsoft.com/services/virtual-machines/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Depolama** |  |  |  |
| [Azure Data Lake Storage 2. Nesil](../storage/common/storage-account-create.md?tabs=azure-portal)  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Premium dosya depolaması](../storage/files/storage-files-planning.md) |  | :heavy_check_mark: | :heavy_check_mark: |
| [Blob Depolama](../storage/blobs/storage-blobs-introduction.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Yönetilen Diskler](https://azure.microsoft.com/en-gb/updates/azure-managed-snapshots-images-ga/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Ağ** |  |  |  |
| [Application Gateway v2](../application-gateway/application-gateway-autoscaling-zone-redundant.md)  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Express Route](../expressroute/designing-for-high-availability-with-expressroute.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Güvenlik Duvarı](../firewall/deploy-availability-zone-powershell.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Standart IP adresi](../virtual-network/public-ip-addresses.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Load Balancer](../load-balancer/load-balancer-standard-availability-zones.md#concepts) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal ağ](../vpn-gateway/create-zone-redundant-vnet-gateway.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal ağ NAT](../virtual-network/nat-gateway-resource.md#availability-zones) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal WAN](../virtual-wan/virtual-wan-faq.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [VPN Gateway](../vpn-gateway/about-zone-redundant-vnet-gateways.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Veritabanları** |  |  |  |
| [Redis için Azure Önbelleği](../azure-cache-for-redis/cache-overview.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Cosmos DB](../cosmos-db/high-availability.md#availability-zone-support) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Veri Gezgini](/azure/data-explorer/create-cluster-database-portal) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [MySQL için Azure veritabanı-esnek sunucu](../mysql/flexible-server/concepts-high-availability.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [PostgreSQL için Azure veritabanı-esnek sunucu](../postgresql/flexible-server/overview.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure SQL veritabanı (Genel Amaçlı katmanı)](../azure-sql/database/high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview) | : heavy_check_mark: (Önizleme) | : heavy_check_mark: (Önizleme) | : heavy_check_mark: (Önizleme) |
| [Azure SQL veritabanı (Premium & İş Açısından Kritik katmanları)](../azure-sql/database/high-availability-sla.md#premium-and-business-critical-service-tier-zone-redundant-availability) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Analiz** |  |  |  |
| [Event Hubs](../event-hubs/index.yml) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Tümleştirme** |  |  |  |
| [Event Grid](../event-grid/index.yml) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Service Bus](../service-fabric/service-fabric-cross-availability-zones.md) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| **Yönetim ve Idare** |  |  |  |
| [Ağ İzleyicisi](../network-watcher/frequently-asked-questions.md) | :heavy_check_mark: | sayı | sayı |
| **Güvenlik** |  |  |  |
| [Azure Active Directory Domain Services](../active-directory-domain-services/overview.md) | :heavy_check_mark: | :heavy_check_mark: |  |


## <a name="upcoming-availability-zones"></a>Yaklaşan Kullanılabilirlik Alanları 

Azure, aşağıdaki bölgelerde Kullanılabilirlik Alanları destek sunar:
- US Gov Virginia
- Güney Afrika - Kuzey
- Orta Güney ABD
- Almanya Orta Batı

Kullanılabilirlik Alanları destekleyen mevcut ve yaklaşan bölgelerin listesi [burada](https://azure.microsoft.com/global-infrastructure/geographies/)bulunabilir.    

Bu bölgelerde Kullanılabilirlik Alanları destek hakkında daha fazla bilgi edinmek için Microsoft Sales veya müşteri temsilcinizle iletişime geçin.


## <a name="pricing-for-vms-in-availability-zones"></a>Kullanılabilirlik Alanları VM 'Leri için fiyatlandırma

Bir kullanılabilirlik bölgesinde dağıtılan sanal makineler için ek ücret yoktur. Daha fazla bilgi için [bant genişliği fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/bandwidth/)gözden geçirin.


## <a name="get-started-with-availability-zones"></a>Kullanılabilirlik Alanları kullanmaya başlayın

- [Sanal makine oluşturma](../virtual-machines/windows/create-portal-availability-zone.md)
- [PowerShell kullanarak yönetilen disk ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Bölge yedekli sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Bölgesel olarak yedekli bir ön uç ile bir Standart Load Balancer kullanarak VM 'lerde Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-cli?tabs=option-1-create-load-balancer-standard)
- [Bir bölge içindeki VM 'leri, bölgesel ön ucu ile standart Load Balancer kullanarak yük dengeleme](https://docs.microsoft.com/azure/load-balancer/quickstart-load-balancer-standard-public-cli?tabs=option-1-create-load-balancer-standard)
- [Alanlar arası yedekli depolama](../storage/common/storage-redundancy.md)
- [SQL veritabanı genel amaçlı katmanı](../azure-sql/database/high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)
- [Event Hubs coğrafi olağanüstü durum kurtarma](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Alanlar arası yedekli sanal ağ geçidi oluşturma](../vpn-gateway/create-zone-redundant-vnet-gateway.md)
- [Azure Cosmos DB için bölge yedekli bölgesi ekleme](../cosmos-db/high-availability.md#availability-zone-support)
- [Redsıs için Azure önbelleği Başlarken Kullanılabilirlik Alanları](https://gist.github.com/JonCole/92c669ea482bbb7996f6428fb6c3eb97#file-redisazgettingstarted-md)
- [Azure Active Directory Domain Services örneği oluşturma](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance)
- [Kullanılabilirlik Alanları kullanan bir Azure Kubernetes hizmeti (AKS) kümesi oluşturma](../aks/availability-zones.md)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure’daki Bölgeler ve Kullanılabilirlik Alanları](az-overview.md)
