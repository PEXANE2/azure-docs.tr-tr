---
title: Azure 'da bölgeler ve Kullanılabilirlik Alanları
description: Azure 'da teknik ve yasal gereksinimlerinizi karşılayacak bölgeler ve Kullanılabilirlik Alanları hakkında bilgi edinin.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 08/27/2020
ms.author: cynthn
ms.custom: fasttrack-edit, mvc
ms.openlocfilehash: b9a400862280cbbc219e87695e363c0ec2e75ee6
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89018680"
---
# <a name="regions-and-availability-zones-in-azure"></a>Azure 'da bölgeler ve Kullanılabilirlik Alanları

Microsoft Azure Hizmetleri, bulut işlemlerinizi en uygun düzeyde olacak şekilde küresel olarak kullanılabilir. Gereksinimlerinize göre en iyi bölgeyi teknik ve yasal noktalara göre seçebilirsiniz: hizmet özellikleri, veri yerleşimi, uyumluluk gereksinimleri ve gecikme.

## <a name="terminology"></a>Terminoloji

Azure 'da bölgeleri ve Kullanılabilirlik Alanları daha iyi anlamak için, temel hüküm veya kavramların anlaşılmasına yardımcı olur.

| Terim veya kavram | Açıklama |
| --- | --- |
| region | Gecikme süresi tanımlı bir çevre içinde dağıtılan ve ayrılmış bölgesel düşük gecikmeli bir ağ ile bağlanmış bir veri merkezleri kümesi. |
| Coğrafya | En az bir Azure bölgesi içeren bir dünya alanı. Geographiler, veri uygunluğunu ve uyumluluk sınırlarını koruyan ayrı bir pazar tanımlar. Coğrafyalar, özel veri yerleşikliği ve uyumluluk gereksinimleri olan müşterilerin verileri ile uygulamalarını yakın tutmasına olanak tanır. Geographiler, adanmış yüksek kapasiteli ağ altyapımıza yönelik bağlantı yoluyla, tam bölge arızalarına karşı dayanıklı bir şekilde hataya dayanıklıdır. |
| Kullanılabilirlik Alanı | Bölge içindeki benzersiz fiziksel konumlar. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. |
| Önerilen bölge | En geniş hizmet özelliklerini sağlayan ve şimdi Kullanılabilirlik Alanları destekleyecek şekilde tasarlanan bölge. Bunlar, Azure portal **Önerilen**şekilde belirtilmiştir. |
| Alternatif (diğer) bölge | Azure 'un bir veri yerleşimi içindeki parmak izini, önerilen bir bölgenin de bulunduğu bölge için genişleten bölge. Alternatif bölgeler, gecikme süresini en iyi duruma getirmeye ve olağanüstü durum kurtarma gereksinimlerine ikinci bir bölge sağlamaya yardımcı olur. Bunlar, Kullanılabilirlik Alanları desteklemek için tasarlanmamıştır (Azure, bu bölgelerin önerilen bölgeler olup olmadığını belirlemesi için düzenli olarak değerlendirmelerine rağmen). Bunlar, Azure portal **başka**bir şekilde belirtilmiştir. |
| temel hizmet | Bölge Genel kullanıma sunulduğunda tüm bölgelerde kullanılabilen bir çekirdek Azure hizmeti. |
| temel hizmet | Bölge/hizmet genel kullanılabilirliği veya diğer bölgelerde isteğe bağlı kullanılabilirlik için 12 ay içinde tüm önerilen bölgelerde kullanılabilen bir Azure hizmeti. |
| özelleştirilmiş hizmet | Özelleştirilmiş/özelleştirilmiş donanımla desteklenen bölgeler arasında isteğe bağlı kullanılabilirlik sağlayan bir Azure hizmetidir. |
| bölgesel hizmet | Bölgeye göre dağıtılan ve müşterinin hizmetin dağıtılacağı bölgeyi belirtmesini sağlayan bir Azure hizmeti. Tüm liste için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=all). |
| bölgesel olmayan hizmet | Belirli bir Azure bölgesine bağımlılığı olmayan bir Azure hizmeti. Bölgesel olmayan hizmetler iki veya daha fazla bölgeye dağıtılır ve bölgesel bir hata varsa, başka bir bölgedeki hizmetin örneği müşterilere hizmet vermeye devam eder. Tüm liste için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=all). |

## <a name="regions"></a>Bölgeler

Bölge, gecikme tarafından tanımlanan bir çevre içinde dağıtılan ve adanmış bölgesel düşük gecikmeli bir ağ ile bağlanan bir veri merkezleri kümesidir. Azure, siteler arası dayanıklılık sağlamak için birden çok bölgeye dahil olmak üzere, ihtiyacınız olan uygulamaları dağıtma esnekliği sunar. Daha fazla bilgi için bkz. [dayanıklılık bilgilerine genel bakış](https://docs.microsoft.com/azure/architecture/framework/resiliency/overview).

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Kullanılabilirlik alanı, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılığı güvence altına almak için etkinleştirilmiş tüm bölgelerde en az üç ayrı alan vardır. Bir bölgedeki Kullanılabilirlik Alanları fiziksel ayrımı, uygulamaları ve verileri veri merkezi hatalarından korur. Bölgesel olarak yedekli hizmetler, uygulamalarınızı ve verilerinizi Kullanılabilirlik Alanları arasında çoğaltarak hata noktalarından koruyun. Azure, Kullanılabilirlik Alanları sayesinde sektörün en iyi% 99,99 VM çalışma süresi SLA 'sını sunmaktadır. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

Bir Azure bölgesindeki kullanılabilirlik bölgesi bir hata etki alanının ve bir güncelleştirme etki alanının birleşimidir. Örneğin, bir Azure bölgesindeki üç bölgede üç veya daha fazla VM oluşturursanız, VM 'niz üç hata etki alanına ve üç güncelleştirme etki alanına etkili bir şekilde dağıtılır. Azure platformu, farklı bölgelerdeki VM 'Lerin aynı anda güncellenmek üzere zamanlanmadığından emin olmak için bu dağıtımı güncelleştirme etki alanları genelinde tanır.

Bir bölgedeki işlem, depolama, ağ ve veri kaynaklarınızı birlikte bularak ve diğer bölgelerde çoğaltarak uygulama mimarinizde yüksek kullanılabilirlik oluşturun. Kullanılabilirlik Alanlarını destekleyen Azure hizmetleri iki kategoriye ayrılır:

- **Zonal services** Bir kaynağın belirli bir bölgeye sabitlendiği (örneğin, sanal makineler, yönetilen diskler, standart IP adresleri) veya
- Bölgesel olarak **yedekli hizmetler** – Azure platformu bölgeler arasında otomatik olarak çoğaltılır (örneğin, bölgesel olarak yedekli depolama, SQL veritabanı).

Azure 'da kapsamlı iş sürekliliği elde etmek için Azure bölge çiftleriyle Kullanılabilirlik Alanları birleşimini kullanarak uygulama mimarinizi geliştirin. Uygulamalarınızı ve verilerinizi Azure bölgesindeki Kullanılabilirlik Alanları kullanarak yüksek kullanılabilirlik ve olağanüstü durum kurtarma koruması için Azure bölgelerinde zaman uyumsuz olarak çoğaltma yapabilirsiniz.
 
![bölgede aşağı doğru bir bölgenin kavramsal görünümü](./media/az-overview/az-graphic-two.png)

> [!IMPORTANT]
> Kullanılabilirlik alanı tanımlayıcıları (yukarıdaki resimde 1, 2 ve 3 sayıları), her aboneliğin bağımsız olarak gerçek fiziksel bölgelere mantıksal olarak eşlenir. Bu, belirli bir abonelikteki kullanılabilirlik Bölge 1 farklı bir abonelikte kullanılabilirlik Bölge 1 farklı bir fiziksel bölgeye başvurabileceği anlamına gelir. Sonuç olarak, sanal makine yerleştirmesi için farklı aboneliklerde kullanılabilirlik bölgesi kimliklerinin olmaması önerilir.

## <a name="region-and-service-categories"></a>Bölge ve hizmet kategorileri

Azure 'un bölgeler genelinde Azure hizmetlerinin kullanılabilirliğine yönelik yaklaşımı, önerilen bölgelerde ve alternatif bölgelerde sunulan hizmetleri ifade ederek en iyi şekilde açıklanmıştır.

- **Önerilen bölge** -en geniş hizmet özelliklerini sağlayan ve şimdi kullanılabilirlik alanları veya gelecekte destekleyecek şekilde tasarlanan bölge. Bunlar, Azure portal **Önerilen**şekilde belirtilmiştir.
- **Alternatif (diğer) bölge** -Azure 'un bir veri yerleşimi içindeki parmak izini, önerilen bir bölgenin de bulunduğu bölge için genişleten bir bölgedir. Alternatif bölgeler, gecikme süresini en iyi duruma getirmeye ve olağanüstü durum kurtarma gereksinimlerine ikinci bir bölge sağlamaya yardımcı olur. Bunlar, Kullanılabilirlik Alanları desteklemek için tasarlanmamıştır (Azure, bu bölgelerin önerilen bölgeler olup olmadığını belirlemesi için düzenli olarak değerlendirmelerine rağmen). Bunlar, Azure portal **başka**bir şekilde belirtilmiştir.

### <a name="comparing-region-types"></a>Bölge türlerini karşılaştırma

Azure hizmetleri üç kategoride gruplandırılır: temel, genel ve özel hizmetler. Azure 'un belirli bir bölgeye hizmet dağıtmaya yönelik genel ilkesi, birincil olarak bölge türü, hizmet kategorileri ve müşteri talebi tarafından çalıştırılır:

- Temel **: bölge** genel kullanıma sunulunca veya yeni bir temel hizmetin genel kullanıma sunulmasına ilişkin 12 ay içinde tüm önerilen ve alternatif bölgelerde kullanılabilir.
- **Temel** : bölge/hizmet genel kullanılabilirliği 12 ay içinde önerilen tüm bölgelerde kullanılabilir; diğer bölgelerde talep odaklı (birçoğu, diğer bölgelere ait büyük bir alt kümeye zaten dağıtılır).
- **Özelleştirilmiş-hedeflenen** hizmet sunumları, genellikle sektör odaklı veya özelleştirilmiş/özelleştirilmiş donanımlar tarafından desteklenir. Bölgeler arasında isteğe bağlı kullanılabilirlik (birçoğu, önerilen bölgelerin büyük bir alt kümesine zaten dağıtılır).

Belirli bir bölgede hangi hizmetlerin dağıtıldığını görmek ve bir bölgedeki hizmetlerin önizleme veya genel kullanılabilirliği için gelecekteki yol haritasını görmek üzere bkz. [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=all).

Belirli bir bölgede bir hizmet teklifi yoksa, Microsoft satış temsilcinizle iletişim kurarak ilgi alanınızı paylaşabilirsiniz.

| Bölge türü | Bölgesel olmayan | Temel | Temel | Özelleştirilmiş | Kullanılabilirlik Alanları | Veri yerleşimi |
| --- | --- | --- | --- | --- | --- | --- |
| Önerilen | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Talep odaklı | :heavy_check_mark: | :heavy_check_mark: |
| Diğer | :heavy_check_mark: | :heavy_check_mark: | Talep odaklı | Talep odaklı | Yok | :heavy_check_mark: |

### <a name="services-by-category"></a>Kategoriye göre hizmetler

Daha önce belirtildiği gibi, Azure hizmetleri üç kategoride sınıflandırır: temel, genel ve özel. Hizmet kategorileri genel kullanıma sunulduğuna göre atanır. Genellikle, hizmetler yaşam döngüsünü özelleştirilmiş bir hizmet olarak başlatır ve talep ve kullanım artışları, temel veya temel olarak yükseltilebilir. Aşağıdaki tabloda, hizmetler için temel, genel veya özel olarak kategori listelenmektedir. Tabloyla ilgili olarak aşağıdakileri göz önünde belirtmelisiniz:

- Bazı hizmetler bölgesel değildir. Bölgesel olmayan hizmetlerin bilgileri ve bir listesi için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/).
- Daha eski nesil sanal makineler listelenmez. Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutlarında](../virtual-machines/sizes-previous-gen.md)belgeler.

> [!div class="mx-tableFixed"]
> | Temel | Temel | Özelleştirilmiş |
> | --- | --- | --- |
> | Hesap depolama alanı | API Management | FHIR için Azure API'si |
> | Application Gateway | Uygulama Yapılandırması | Azure Analysis Services |
> | Azure Backup | App Service | Azure Blok Zinciri Hizmeti |
> | Azure Cosmos DB | Otomasyon | Azure Blueprints |
> | Azure Data Lake Storage Gen2 | Azure Active Directory Domain Services | MariaDB için Azure Veritabanı |
> | Azure ExpressRoute | Azure Bastion | Azure Ayrılmış HSM |
> | Azure SQL Veritabanı | Redis için Azure Önbelleği | Azure Dev Spaces |
> | Cloud Services | Azure Bilişsel Arama | Azure Digital Twins |
> | Cloud Services: AV2-Series | Azure Veri Gezgini | Azure Lab Services |
> | Cloud Services: dv2-Series | Azure Veri Paylaşımı | Azure NetApp Files |
> | Cloud Services: Dv3-Series | MySQL için Azure Veritabanı | Azure hisse |
> | Cloud Services: Ev3-Series | PostgreSQL için Azure Veritabanı | Azure yay bulut hizmeti |
> | Cloud Services: örnek düzeyi IP 'Leri | Azure Veritabanı Geçiş Hizmeti | Azure Zaman Serisi Görüşleri |
> | Cloud Services: Ayrılmış IP | Azure Databricks | CloudSimple tarafından sunulan Azure VMware Çözümü |
> | Disk Depolama | Azure DDoS Koruması | Cloud Services: G serisi |
> | Event Hubs | Azure DevTest Labs | Cloud Services: H serisi |
> | Key Vault | Azure Güvenlik Duvarı | Bilişsel hizmetler: Özel Görüntü İşleme |
> | Yük dengeleyici | Azure Güvenlik Duvarı Yöneticisi | Bilişsel hizmetler: konuşmacı tanıma |
> | Service Bus | Azure İşlevleri | Data Box Heavy |
> | Service Fabric | Azure HPC Önbelleği | Veri Kataloğu |
> | Sanal Makine Ölçek Kümeleri | Azure IoT Hub | Data Factory: Data Factory v1 |
> | Sanal Makineler | Azure Kubernetes Service (AKS) | Data Lake Analytics |
> | Sanal makineler: AV2-Series | Azure Machine Learning | Machine Learning Studio |
> | Sanal makineler: BS serisi | Azure Özel Bağlantı | Microsoft Genomiks |
> | Sanal makineler: DSv2-Series | Azure Red Hat OpenShift | Remote Rendering |
> | Sanal makineler: DSv3-Series | Azure SignalR Hizmeti | Spatial Anchors |
> | Sanal makineler: dv2-Series | Azure Site Recovery | StorSimple |
> | Sanal makineler: Dv3-Series | Azure Stack Hub | Video Indexer |
> | Sanal makineler: ESv3-Series | Azure Stream Analytics | Sanal makineler: DASv4-Series |
> | Sanal makineler: Ev3-Series | Azure Synapse Analytics | Sanal makineler: DAv4-Series |
> | Sanal makineler: F serisi | Batch | Sanal makineler: DCsv2-Series |
> | Sanal makineler: FS Serisi | Cloud Services: a serisi | Sanal makineler: EASv4-Series |
> | Sanal makineler: örnek düzeyi IP 'Leri | Bilişsel Hizmetler | Sanal makineler: EAv4-Series |
> | Sanal makineler: Ayrılmış IP | Bilişsel hizmetler: Görüntü İşleme | Sanal makineler: G serisi |
> | Sanal Ağ | Bilişsel hizmetler: Content Moderator | Sanal makineler: GS serisi |
> | VPN Ağ Geçidi | Bilişsel hizmetler: yüz | Sanal makineler: HBv1-Series |
> |  | Bilişsel hizmetler: form tanıyıcı | Sanal makineler: HBv2-Series |
> |  | Bilişsel hizmetler: Language Understanding | Sanal makineler: HCv1-Series |
> |  | Bilişsel hizmetler: Soru-Cevap Oluşturma | Sanal makineler: H serisi |
> |  | Bilişsel hizmetler: konuşma Hizmetleri | Sanal makineler: LS Serisi |
> |  | Container Instances | Sanal makineler: LSv2-Series |
> |  | Container Registry | Sanal makineler: Mv2-Series |
> |  | Data Factory | Sanal makineler: NC serisi |
> |  | Event Grid | Sanal makineler: NCv2-Series |
> |  | HDInsight | Sanal makineler: NCv3-Series |
> |  | Logic Apps | Sanal makineler: NDs serisi |
> |  | Media Services | Sanal makineler: NDv2-Series |
> |  | Ağ İzleyicisi | Sanal makineler: NV serisi |
> |  | Notification Hubs | Sanal makineler: NVv3-Series |
> |  | Power BI Embedded | Sanal makineler: NVv4-Series |
> |  | Premium blob depolaması | Sanal makineler: Azure üzerinde SAP HANA Büyük Örnekleri |
> |  | Premium dosya depolaması | Visual Studio App Center |
> |  | Depolama alanı: Arşiv Depolama |  |
> |  | Ultra Disk Depolama |  |
> |  | Sanal makineler: Ddsv4-Series |  |
> |  | Sanal makineler: Ddv4-Series |  |
> |  | Sanal makineler: Dsv4-Series |  |
> |  | Sanal makineler: dv4-Series |  |
> |  | Sanal makineler: Edsv4-Series |  |
> |  | Sanal makineler: Edv4-Series |  |
> |  | Sanal makineler: Esv4-Series |  |
> |  | Sanal makineler: Ev4-Series |  |
> |  | Sanal makineler: Fsv2-Series |  |
> |  | Sanal makineler: a serisi |  |
> |  | Sanal WAN |  |

###  <a name="services-resiliency"></a>Hizmet dayanıklılığı

Tüm Azure Yönetim Hizmetleri bölge düzeyindeki hatalardan dayanıklı olacak şekilde tasarlanmıştır. Başarısızlık kapsamında, bir bölgedeki bir veya daha fazla kullanılabilirlik bölgesi hatası, tüm bölge arızasına kıyasla daha küçük bir hata Radius içermelidir. Azure, bölgedeki veya başka bir Azure bölgesindeki yönetim hizmetlerinden oluşan bölge düzeyindeki bir hatadan kurtuya ait olabilir. Azure, bir bölgedeki Kullanılabilirlik Alanları arasında dağıtılan müşteri kaynaklarını etkileyen tüm sorunları engellemek için bir bölge içinde tek seferde kritik bakım gerçekleştirir.

### <a name="pricing-for-vms-in-availability-zones"></a>Kullanılabilirlik Alanları VM 'Leri için fiyatlandırma

Bir kullanılabilirlik bölgesinde dağıtılan sanal makineler için ek ücret yoktur. % 99,99 VM çalışma süresi SLA 'Sı, iki veya daha fazla VM bir Azure bölgesi içinde iki veya daha fazla Kullanılabilirlik Alanları dağıtıldığında sunulur. Ek kullanılabilirlik alanı VM 'leri-VM veri aktarımı ücretleri olacaktır. Daha fazla bilgi için [bant genişliği fiyatlandırma](https://azure.microsoft.com/pricing/details/bandwidth/) sayfasını gözden geçirin.

### <a name="get-started-with-availability-zones"></a>Kullanılabilirlik Alanları kullanmaya başlayın

- [Sanal makine oluşturma](../virtual-machines/windows/create-portal-availability-zone.md)
- [PowerShell kullanarak yönetilen disk ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Bölge yedekli sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Bölgesel olarak yedekli bir ön uç ile bir Standart Load Balancer kullanarak VM 'lerde Yük Dengeleme](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Bir bölge içindeki VM 'leri, bölgesel ön ucu ile standart Load Balancer kullanarak yük dengeleme](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Alanlar arası yedekli depolama](../storage/common/storage-redundancy-zrs.md)
- [SQL Veritabanı](../azure-sql/database/high-availability-sla.md#zone-redundant-configuration)
- [Event Hubs coğrafi olağanüstü durum kurtarma](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Alanlar arası yedekli sanal ağ geçidi oluşturma](../vpn-gateway/create-zone-redundant-vnet-gateway.md)
- [Azure Cosmos DB için bölge yedekli bölgesi ekleme](../cosmos-db/high-availability.md#availability-zone-support)
- [Redsıs için Azure önbelleği Başlarken Kullanılabilirlik Alanları](https://aka.ms/redis/az/getstarted)
- [Azure Active Directory Domain Services örneği oluşturma](../active-directory-domain-services/tutorial-create-instance.md)
- [Kullanılabilirlik Alanları kullanan bir Azure Kubernetes hizmeti (AKS) kümesi oluşturma](../aks/availability-zones.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da Kullanılabilirlik Alanları destekleyen bölgeler](az-region.md)
- [Hızlı başlangıç şablonları](https://aka.ms/azqs)
