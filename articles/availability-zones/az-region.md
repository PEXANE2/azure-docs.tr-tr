---
title: Kullanılabilirlik Alanlarını destekleyen Azure Hizmetleri
description: Azure 'da yüksek oranda kullanılabilir ve dayanıklı uygulamalar oluşturmak için, kaynaklarınızı çalıştırmak için kullanabileceğiniz fiziksel olarak ayrı konumlar sağlayın Kullanılabilirlik Alanları.
author: prsandhu
ms.service: azure
ms.topic: conceptual
ms.date: 03/16/2021
ms.author: prsandhu
ms.reviewer: cynthn
ms.custom: fasttrack-edit, mvc, references_regions
ms.openlocfilehash: 5152b0ecf6eebe87031a2165d5d8ba11d99209cf
ms.sourcegitcommit: 27cd3e515fee7821807c03e64ce8ac2dd2dd82d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103601194"
---
# <a name="azure-services-that-support-availability-zones"></a>Kullanılabilirlik Alanlarını destekleyen Azure Hizmetleri

Microsoft Azure küresel altyapı, müşterilerine en yüksek düzeyde artıklık ve dayanıklılık sağlamak için her katmanda tasarlanır ve oluşturulur. Azure altyapısı, bir başarısızlık yarıçapını sınırlayan ve bu nedenle müşteri uygulamalarına ve verilerine potansiyel etkiyi sınırlayan coğrafi bölgeler, bölgeler ve Kullanılabilirlik Alanları oluşur. Azure Kullanılabilirlik Alanları yapısı, veri merkezi hatalarıyla korunmaya yönelik bir yazılım ve ağ çözümü sağlamak ve müşterilerimiz için yüksek kullanılabilirlik (HA) sağlamak üzere geliştirilmiştir.

Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her bölge, bağımsız güç, soğutma ve ağ ile bir veya daha fazla veri merkezinden oluşur. Bir bölgedeki kullanılabilirlik alanları fiziksel ayrımı, büyük ölçekli taşımalar, büyük fırtınalarını 'ler ve süper fırtınalarını gibi bölge hatalarından ve site erişimini, güvenli akışını, genişletilmiş yardımcı programları ve kaynakların kullanılabilirliğini kesintiye uğratabilecek diğer olayları kısıtlar. Kullanılabilirlik Alanları ve ilişkili veri merkezleri, bir bölgenin güvenliği tehlikeye girerse, hizmetler, kapasite ve kullanılabilirlik bölgesindeki diğer Kullanılabilirlik Alanları tarafından desteklenir.

Tüm Azure Yönetim Hizmetleri bölge düzeyindeki hatalardan dayanıklı olacak şekilde tasarlanmıştır. Başarısızlık kapsamında, bir bölgedeki bir veya daha fazla kullanılabilirlik bölgesi hatası, tüm bölge arızasına kıyasla daha küçük bir hata Radius içermelidir. Azure, bir bölgedeki yönetim hizmetlerinin bölge düzeyindeki başarısızlığından kurtudan kurtarır. Azure, bir bölgedeki Kullanılabilirlik Alanları arasında dağıtılan müşteri kaynaklarını etkileyen tüm sorunları engellemek için bir bölge içinde tek seferde kritik bakım gerçekleştirir.


![3 bölge içeren bir Azure bölgesinin kavramsal görünümü](./media/az-region/azure-region-availability-zones.png)


Kullanılabilirlik alanları destekleyen Azure hizmetleri üç kategoride yer almalıdır: **Bölgesel**, bölgesel olarak **yedekli** ve **bölgesel olmayan** hizmetler. Müşteri iş yükleri, uygulama performansını ve dayanıklılığı karşılamak üzere bu mimari senaryolarından herhangi birini kullanacak şekilde kategorilere ayrılmıştır.

- **ZGen Hizmetleri** – daha sıkı gecikme veya performans gereksinimlerine ulaşmak için bir kaynak belirli, otomatik olarak seçilen bir kullanılabilirlik bölgesine dağıtılabilir.  Dayanıklılık, uygulamalar ve veriler bölge içindeki bir veya daha fazla bölgeye çoğaltılarak kendi kendine tasarlanmıştır.  Kaynaklar belirli bir bölgeye sabitlenebilir. Örneğin, sanal makineler, yönetilen diskler veya standart IP adresleri belirli bir bölgeye sabitlenebilir ve bu sayede bir veya daha fazla kaynak örneği bölgelere yayılarak artan esnekliği.

- Bölgesel olarak **yedekli hizmetler** – Azure platformu, kaynakları ve verileri bölgeler arasında çoğaltır.  Azure, bölge içindeki örnekleri otomatik olarak çoğalttığından ve dağıttığından yüksek kullanılabilirlik teslimini yönetir.  ZRS, örneğin, bir bölge hatası verilerin HA 'sini etkilemeyecek şekilde verileri üç bölge arasında çoğaltır. 

- **Bölgesel olmayan hizmetler** – hizmetler Azure coğrafi bölgeler tarafından her zaman kullanılabilir ve bölge genelinde kesintiler ve bölge genelinde kesintilere karşı dayanıklıdır. 


Azure 'da kapsamlı iş sürekliliği elde etmek için Azure bölge çiftleriyle Kullanılabilirlik Alanları birleşimini kullanarak uygulama mimarinizi geliştirin. Uygulamalarınızı ve verilerinizi Azure bölgesindeki Kullanılabilirlik Alanları kullanarak yüksek kullanılabilirlik ve olağanüstü durum kurtarma koruması için Azure bölgelerinde zaman uyumsuz olarak çoğaltma yapabilirsiniz. Daha fazla bilgi edinmek için [kullanılabilirlik alanları kullanarak yüksek kullanılabilirlik için çözüm oluşturma](/azure/architecture/high-availability/building-solutions-for-high-availability)makalesini okuyun. 

## <a name="azure-services-supporting-availability-zones"></a>Kullanılabilirlik Alanları destekleyen Azure hizmetleri

 - Eski nesil sanal makineler listelenmez. Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../virtual-machines/sizes-previous-gen.md).
 - [Azure 'Daki bölgelerde ve kullanılabilirlik alanları](az-overview.md)bölümünde belirtildiği gibi, bazı hizmetler bölgesel değildir. Bu hizmetlerin belirli bir Azure bölgesine bağımlılığı yoktur, bu nedenle bölge genelinde kesintiler ve bölge genelinde kesintiler vardır.  Bölgesel olmayan hizmetlerin listesi, [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/)' de bulunabilir.


## <a name="azure-regions-with-availability-zones"></a>Kullanılabilirlik Alanları olan Azure bölgeleri


| Kuzey ve Güney Amerika           | Avrupa               | Afrika              | Asya Pasifik   |
|--------------------|----------------------|---------------------|----------------|
|                    |                      |                     |                |
| Güney Brezilya       | Orta Fransa       | Güney Afrika Kuzey * | Doğu Japonya     |
| Orta Kanada     | Almanya Orta Batı |                     | Güneydoğu Asya |
| Central US         | Kuzey Avrupa         |                     | Doğu Avustralya |
| Doğu ABD            | Güney Birleşik Krallık             |                     |                |
| Doğu ABD 2          | West Europe          |                     |                |
| Orta Güney ABD |                      |                     |                |
| US Gov Virginia     |                      |                     |                |
| Batı ABD 2        |                      |                     |                |


\* Bu bölgelerde Kullanılabilirlik Alanları ve kullanılabilir Hizmetler desteği hakkında daha fazla bilgi edinmek için Microsoft Sales veya müşteri temsilcinizle iletişime geçin. Kullanılabilirlik Alanları destekleyecek yaklaşan bölgeler için bkz. [Azure geographiler](https://azure.microsoft.com/en-us/global-infrastructure/geographies/).


## <a name="azure-services-supporting-availability-zones"></a>Kullanılabilirlik Alanları destekleyen Azure hizmetleri

- Daha eski nesil sanal makineler aşağıda listelenmez. Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutları](../virtual-machines/sizes-previous-gen.md).

- Bazı hizmetler bölgesel değildir; daha fazla bilgi için bkz. [Azure 'Da bölgeler ve kullanılabilirlik alanları](az-overview.md) . Bu hizmetlerin, belirli bir Azure bölgesine bağımlılığı yoktur, bu da bölge genelinde kesintiler ve bölge genelinde kesintiler için esnek hale getirir.  Bölgesel olmayan hizmetlerin listesi, [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/)' de bulunabilir.


### <a name="zone-resilient-services"></a>Bölge dayanıklı Hizmetleri 

: globe_with_meridians: bölgesel olmayan hizmetler-hizmetler Azure coğrafi bölgeler tarafından her zaman kullanılabilir ve bölge genelinde kesintiler ve bölge genelinde kesintiler sağlar.

: large_blue_diamond: bölge genelinde kesintiler için esnek 

**Temel hizmetler**

|     Ürünler                                                    | Dayanıklılık             |
|-----------------------------------------------------------------|:----------------------------:|
|     Depolama hesabı                                           | : large_blue_diamond:  |
|     Application Gateway (v2)                                  | : large_blue_diamond:  |
|     Azure Backup                                                | : large_blue_diamond:  |
|     Azure Cosmos DB                                           | : large_blue_diamond:  |
|     Azure Data Lake Storage Gen 2                             | : large_blue_diamond:  |
|     Azure Express Route                                       | : large_blue_diamond:  |
|     Azure genel IP                                           | : large_blue_diamond:  |
|     Azure SQL veritabanı (Genel Amaçlı katmanı)                 | : large_blue_diamond:  |
|     Azure SQL veritabanı (Premium & İş Açısından Kritik katmanı)     | : large_blue_diamond:  |
|     Disk Depolama                                                | : large_blue_diamond:  |
|     Event Hubs                                                  | : large_blue_diamond:  |
|     Key Vault                                                   | : large_blue_diamond:  |
|     Load Balancer                                               | : large_blue_diamond:  |
|     Service Bus                                                 | : large_blue_diamond:  |
|     Service Fabric                                            | : large_blue_diamond:  |
|     Depolama: sık/seyrek erişimli BLOB depolama katmanları                      | : large_blue_diamond:  |
|     Depolama: yönetilen diskler                                    | : large_blue_diamond:  |
|     Sanal Makine Ölçek Kümeleri                               | : large_blue_diamond:  |
|     Sanal makineler                                          | : large_blue_diamond:  |
|     Sanal makineler: Av2-Series                              | : large_blue_diamond:  |
|     Sanal makineler: Bs-Series                               | : large_blue_diamond:  |
|     Sanal makineler: DSv2-Series                             | : large_blue_diamond:  |
|     Sanal makineler: DSv3-Series                             | : large_blue_diamond:  |
|     Sanal makineler: Dv2-Series                              | : large_blue_diamond:  |
|     Sanal makineler: Dv3-Series                              | : large_blue_diamond:  |
|     Sanal makineler: ESv3-Series                             | : large_blue_diamond:  |
|     Sanal makineler: Ev3-Series                              | : large_blue_diamond:  |
|     Sanal makineler: F serisi                                | : large_blue_diamond:  |
|     Sanal makineler: FS-Series                               | : large_blue_diamond:  |
|     Sanal ağ                                           | : large_blue_diamond:  |
|     VPN Gateway                                                 | : large_blue_diamond:  |


**Temel hizmetler**

| Ürünler                                        | Dayanıklılık |
|-------------------------------------------------|:------------:|
| App Service ortamları                        |      : large_blue_diamond:  |
| Azure Active Directory Domain Services          |      : large_blue_diamond:  |
| Azure Bastion                                   |      : large_blue_diamond:  |
| Redis için Azure Önbelleği                           |      : large_blue_diamond:  |
| Azure Bilişsel Hizmetler: Metin Analizi        |      : large_blue_diamond:  |
| Azure Veri Gezgini                             |      : large_blue_diamond:  |
| MySQL için Azure veritabanı – esnek sunucu      |      : large_blue_diamond:  |
| PostgreSQL için Azure veritabanı – esnek sunucu |      : large_blue_diamond:  |
| Azure DDoS Koruması                           |      : large_blue_diamond:  |
| Azure Disk Şifrelemesi                           |      : large_blue_diamond:  |
| Azure Güvenlik Duvarı                                  |      : large_blue_diamond:  |
| Azure Güvenlik Duvarı Yöneticisi                          |      : large_blue_diamond:  |
| Azure Kubernetes Service (AKS)                  |      : large_blue_diamond:  |
| Azure Özel Bağlantı                              |      : large_blue_diamond:  |
| Azure Red Hat OpenShift                         |      : large_blue_diamond:  |
| Azure Site Recovery                             |      : large_blue_diamond:  |
| Azure SQL: sanal makine                      |      : large_blue_diamond:  |
| Azure Search                                    |      : large_blue_diamond:  |
| Azure Web Uygulaması Güvenlik Duvarı                  |      : large_blue_diamond:  |
| Container Registry                              |      : large_blue_diamond:  |
| Event Grid                                      |      : large_blue_diamond:  |
| Ağ İzleyicisi                                 |      : large_blue_diamond:  |
| Ağ Izleyicisi: Trafik Analizi              |      : large_blue_diamond:  |
| Power BI Embedded                               |      : large_blue_diamond:  |
| Premium blob depolaması                            |      : large_blue_diamond:  |
| Depolama: Azure Premium dosyaları                    |      : large_blue_diamond:  |
| Sanal makineler: Azure adanmış ana bilgisayar          |      : large_blue_diamond:  |
| Sanal makineler: Ddsv4-Series                  |      : large_blue_diamond:  |
| Sanal makineler: Ddv4-Series                   |      : large_blue_diamond:  |
| Sanal makineler: Dsv4-Series                   |      : large_blue_diamond:  |
| Sanal makineler: Dv4-Series                    |      : large_blue_diamond:  |
| Sanal makineler: Edsv4-Series                  |      : large_blue_diamond:  |
| Sanal makineler: Edv4-Series                   |      : large_blue_diamond:  |
| Sanal makineler: Esv4-Series                   |      : large_blue_diamond:  |
| Sanal makineler: Ev4-Series                    |      : large_blue_diamond:  |
| Sanal makineler: Fsv2-Series                   |      : large_blue_diamond:  |
| Sanal makineler: a serisi                      |      : large_blue_diamond:  |
| Sanal WAN                                     |      : large_blue_diamond:  |
| Sanal WAN: ExpressRoute                       |      : large_blue_diamond:  |
| Sanal WAN: Noktadan siteye VPN Gateway          |      : large_blue_diamond:  |
| Sanal WAN: siteden siteye VPN Gateway           |      : large_blue_diamond:  |


**Bölgesel olmayan**

|     Ürünler                                  |     Dayanıklılık    |
|-----------------------------------------------|:-------------------:|
|     Azure DNS                                 |     : globe_with_meridians:             |
|     Azure Active Directory                  |     : globe_with_meridians:             |
|     Azure Gelişmiş Tehdit Koruması          |     : globe_with_meridians:             |
|     Azure Danışmanı                             |     : globe_with_meridians:             |
|     Azure Blueprints                          |     : globe_with_meridians:             |
|     Azure Bot Hizmetleri                        |     : globe_with_meridians:             |
|     Azure Front Door                          |     : globe_with_meridians:             |
|     IoT için Azure Defender                  |     : globe_with_meridians:             |
|     Azure Front Door                           |     : globe_with_meridians:             |
|     Azure Information Protection            |     : globe_with_meridians:             |
|     Azure hafif kullanım                        |     : globe_with_meridians:             |
|     Azure yönetilen uygulamalar              |     : globe_with_meridians:             |
|     Azure Haritalar                                |     : globe_with_meridians:             |
|     Azure İlkesi                              |     : globe_with_meridians:             |
|     Azure Kaynak Grafiği                    |     : globe_with_meridians:             |
|     Azure Sentinel                            |     : globe_with_meridians:             |
|     Azure Stack                               |     : globe_with_meridians:             |
|     Azure Stack uç                        |     : globe_with_meridians:             |
|     Cloud Shell                               |     : globe_with_meridians:             |
|     Content Delivery Network                  |     : globe_with_meridians:             |
|     Maliyet Yönetimi                           |     : globe_with_meridians:             |
|     Microsoft Azure için Müşteri Kasası    |     : globe_with_meridians:             |
|     Intune                                    |     : globe_with_meridians:             |
|     Microsoft Azure eşleme hizmeti         |     : globe_with_meridians:             |
|     Microsoft Azure portal                  |     : globe_with_meridians:             |
|     Microsoft Cloud App Security              |     : globe_with_meridians:             |
|     Microsoft Graph                           |     : globe_with_meridians:             |
|     Güvenlik Merkezi                         |     : globe_with_meridians:             |
|     Traffic Manager                         |     : globe_with_meridians:             |


## <a name="pricing-for-vms-in-availability-zones"></a>Kullanılabilirlik Alanları VM 'Leri için fiyatlandırma

Bir kullanılabilirlik bölgesinde dağıtılan sanal makineler için ek ücret yoktur. Daha fazla bilgi için [bant genişliği fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/bandwidth/)gözden geçirin.


## <a name="get-started-with-availability-zones"></a>Kullanılabilirlik Alanları kullanmaya başlayın

- [Sanal makine oluşturma](../virtual-machines/windows/create-portal-availability-zone.md)
- [PowerShell kullanarak yönetilen disk ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Bölge yedekli sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Bölgesel olarak yedekli bir ön uç ile bir Standart Load Balancer kullanarak VM 'lerde Yük Dengeleme](../load-balancer/quickstart-load-balancer-standard-public-cli.md?tabs=option-1-create-load-balancer-standard)
- [Bir bölge içindeki VM 'leri, bölgesel ön ucu ile standart Load Balancer kullanarak yük dengeleme](../load-balancer/quickstart-load-balancer-standard-public-cli.md?tabs=option-1-create-load-balancer-standard)
- [Alanlar arası yedekli depolama](../storage/common/storage-redundancy.md)
- [SQL veritabanı genel amaçlı katmanı](../azure-sql/database/high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)
- [Event Hubs coğrafi olağanüstü durum kurtarma](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Alanlar arası yedekli sanal ağ geçidi oluşturma](../vpn-gateway/create-zone-redundant-vnet-gateway.md)
- [Azure Cosmos DB için bölge yedekli bölgesi ekleme](../cosmos-db/high-availability.md#availability-zone-support)
- [Redsıs için Azure önbelleği Başlarken Kullanılabilirlik Alanları](https://gist.github.com/JonCole/92c669ea482bbb7996f6428fb6c3eb97#file-redisazgettingstarted-md)
- [Azure Active Directory Domain Services örneği oluşturma](../active-directory-domain-services/tutorial-create-instance.md)
- [Kullanılabilirlik Alanları kullanan bir Azure Kubernetes hizmeti (AKS) kümesi oluşturma](../aks/availability-zones.md)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure’daki Bölgeler ve Kullanılabilirlik Alanları](az-overview.md)
