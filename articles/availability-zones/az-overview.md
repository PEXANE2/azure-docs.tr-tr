---
title: Azure 'da bölgeler ve Kullanılabilirlik Alanları
description: Azure 'da teknik ve yasal gereksinimlerinizi karşılayacak bölgeler ve Kullanılabilirlik Alanları hakkında bilgi edinin.
author: prsandhu
ms.service: azure
ms.topic: conceptual
ms.date: 01/26/2021
ms.author: prsandhu
ms.reviewer: cynthn
ms.custom: fasttrack-edit, mvc
ms.openlocfilehash: dae5319e6c8b87d6a9eef98875ad7e8da623e65c
ms.sourcegitcommit: 4e70fd4028ff44a676f698229cb6a3d555439014
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98955809"
---
# <a name="regions-and-availability-zones-in-azure"></a>Azure 'da bölgeler ve Kullanılabilirlik Alanları

Microsoft Azure Hizmetleri, bulut işlemlerinizi en uygun düzeyde olacak şekilde küresel olarak kullanılabilir. Gereksinimlerinize göre en iyi bölgeyi teknik ve yasal noktalara göre seçebilirsiniz: hizmet özellikleri, veri yerleşimi, uyumluluk gereksinimleri ve gecikme.

## <a name="terminology"></a>Terminoloji

Azure 'da bölgeleri ve Kullanılabilirlik Alanları daha iyi anlamak için, temel hüküm veya kavramların anlaşılmasına yardımcı olur.

| Terim veya kavram | Description |
| --- | --- |
| region | Gecikme süresi tanımlı bir çevre içinde dağıtılan ve ayrılmış bölgesel düşük gecikmeli bir ağ ile bağlanmış bir veri merkezleri kümesi. |
| Coğrafya | En az bir Azure bölgesi içeren bir dünya alanı. Geographiler, veri uygunluğunu ve uyumluluk sınırlarını koruyan ayrı bir pazar tanımlar. Coğrafyalar, özel veri yerleşikliği ve uyumluluk gereksinimleri olan müşterilerin verileri ile uygulamalarını yakın tutmasına olanak tanır. Geographiler, adanmış yüksek kapasiteli ağ altyapımıza yönelik bağlantı yoluyla, tam bölge arızalarına karşı dayanıklı bir şekilde hataya dayanıklıdır. |
| Kullanılabilirlik Alanı | Bölge içindeki benzersiz fiziksel konumlar. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. |
| Önerilen bölge | En geniş hizmet özelliklerini sağlayan ve şimdi Kullanılabilirlik Alanları destekleyecek şekilde tasarlanan bölge. Bunlar, Azure portal **Önerilen** şekilde belirtilmiştir. |
| Alternatif (diğer) bölge | Azure 'un bir veri yerleşimi içindeki parmak izini, önerilen bir bölgenin de bulunduğu bölge için genişleten bölge. Alternatif bölgeler, gecikme süresini en iyi duruma getirmeye ve olağanüstü durum kurtarma gereksinimlerine ikinci bir bölge sağlamaya yardımcı olur. Bunlar, Kullanılabilirlik Alanları desteklemek için tasarlanmamıştır (Azure, bu bölgelerin önerilen bölgeler olup olmadığını belirlemesi için düzenli olarak değerlendirmelerine rağmen). Bunlar, Azure portal **başka** bir şekilde belirtilmiştir. |
| temel hizmet | Bölge Genel kullanıma sunulduğunda tüm bölgelerde kullanılabilen bir çekirdek Azure hizmeti. |
| temel hizmet | Bölge/hizmet genel kullanılabilirliği veya diğer bölgelerde isteğe bağlı kullanılabilirlik için 12 ay içinde tüm önerilen bölgelerde kullanılabilen bir Azure hizmeti. |
| özelleştirilmiş hizmet | Özelleştirilmiş/özelleştirilmiş donanımla desteklenen bölgeler arasında isteğe bağlı kullanılabilirlik sağlayan bir Azure hizmetidir. |
| bölgesel hizmet | Bölgeye göre dağıtılan ve müşterinin hizmetin dağıtılacağı bölgeyi belirtmesini sağlayan bir Azure hizmeti. Tüm liste için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=all). |
| bölgesel olmayan hizmet | Belirli bir Azure bölgesine bağımlılığı olmayan bir Azure hizmeti. Bölgesel olmayan hizmetler iki veya daha fazla bölgeye dağıtılır ve bölgesel bir hata varsa, başka bir bölgedeki hizmetin örneği müşterilere hizmet vermeye devam eder. Tüm liste için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=all). |

## <a name="regions"></a>Bölgeler

Bölge, gecikme tarafından tanımlanan bir çevre içinde dağıtılan ve adanmış bölgesel düşük gecikmeli bir ağ ile bağlanan bir veri merkezleri kümesidir. Azure, siteler arası dayanıklılık sağlamak için birden çok bölgeye dahil olmak üzere, ihtiyacınız olan uygulamaları dağıtma esnekliği sunar. Daha fazla bilgi için bkz. [dayanıklılık bilgilerine genel bakış](/azure/architecture/framework/resiliency/overview).

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Kullanılabilirlik alanı, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılığı güvence altına almak için etkinleştirilmiş tüm bölgelerde en az üç ayrı alan vardır. Bir bölgedeki Kullanılabilirlik Alanları fiziksel ayrımı, uygulamaları ve verileri veri merkezi hatalarından korur. Bölgesel olarak yedekli hizmetler, uygulamalarınızı ve verilerinizi Kullanılabilirlik Alanları arasında çoğaltarak hata noktalarından koruyun. Azure, Kullanılabilirlik Alanları sayesinde sektörün en iyi% 99,99 VM çalışma süresi SLA 'sını sunmaktadır. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

Bir Azure bölgesindeki kullanılabilirlik bölgesi bir hata etki alanının ve bir güncelleştirme etki alanının birleşimidir. Örneğin, bir Azure bölgesindeki üç bölgede üç veya daha fazla VM oluşturursanız, VM 'niz üç hata etki alanına ve üç güncelleştirme etki alanına etkili bir şekilde dağıtılır. Azure platformu, farklı bölgelerdeki VM 'Lerin aynı anda güncellenmek üzere zamanlanmadığından emin olmak için bu dağıtımı güncelleştirme etki alanları genelinde tanır.

Bir bölgedeki işlem, depolama, ağ ve veri kaynaklarınızı birlikte bularak ve diğer bölgelerde çoğaltarak uygulama mimarinizde yüksek kullanılabilirlik oluşturun. Kullanılabilirlik Alanlarını destekleyen Azure hizmetleri iki kategoriye ayrılır:

-  Bir kaynağın belirli bir bölgeye sabitlendiği (örneğin, sanal makineler, yönetilen diskler, standart IP adresleri) veya
- Bölgesel olarak **yedekli hizmetler** – Azure platformu bölgeler arasında otomatik olarak çoğaltılır (örneğin, bölgesel olarak yedekli depolama, SQL veritabanı).

Azure 'da kapsamlı iş sürekliliği elde etmek için Azure bölge çiftleriyle Kullanılabilirlik Alanları birleşimini kullanarak uygulama mimarinizi geliştirin. Uygulamalarınızı ve verilerinizi Azure bölgesindeki Kullanılabilirlik Alanları kullanarak yüksek kullanılabilirlik ve olağanüstü durum kurtarma koruması için Azure bölgelerinde zaman uyumsuz olarak çoğaltma yapabilirsiniz.
 
![bölgede aşağı doğru bir bölgenin kavramsal görünümü](./media/az-overview/az-graphic-two.png)

> [!IMPORTANT]
> Kullanılabilirlik alanı tanımlayıcıları (yukarıdaki resimde 1, 2 ve 3 sayıları), her aboneliğin bağımsız olarak gerçek fiziksel bölgelere mantıksal olarak eşlenir. Bu, belirli bir abonelikteki kullanılabilirlik Bölge 1 farklı bir abonelikte kullanılabilirlik Bölge 1 farklı bir fiziksel bölgeye başvurabileceği anlamına gelir. Sonuç olarak, sanal makine yerleştirmesi için farklı aboneliklerde kullanılabilirlik bölgesi kimliklerinin olmaması önerilir.

## <a name="region-and-service-categories"></a>Bölge ve hizmet kategorileri

Azure 'un bölgeler genelinde Azure hizmetlerinin kullanılabilirliğine yönelik yaklaşımı, önerilen bölgelerde ve alternatif bölgelerde sunulan hizmetleri ifade ederek en iyi şekilde açıklanmıştır.

- **Önerilen bölge** -en geniş hizmet özelliklerini sağlayan ve şimdi kullanılabilirlik alanları veya gelecekte destekleyecek şekilde tasarlanan bölge. Bunlar, Azure portal **Önerilen** şekilde belirtilmiştir.
- **Alternatif (diğer) bölge** -Azure 'un bir veri yerleşimi içindeki parmak izini, önerilen bir bölgenin de bulunduğu bölge için genişleten bir bölgedir. Alternatif bölgeler, gecikme süresini en iyi duruma getirmeye ve olağanüstü durum kurtarma gereksinimlerine ikinci bir bölge sağlamaya yardımcı olur. Bunlar, Kullanılabilirlik Alanları desteklemek için tasarlanmamıştır (Azure, bu bölgelerin önerilen bölgeler olup olmadığını belirlemesi için düzenli olarak değerlendirmelerine rağmen). Bunlar, Azure portal **başka** bir şekilde belirtilmiştir.

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
- Daha eski nesil sanal makineler listelenmez. Daha fazla bilgi için bkz. [önceki nesil sanal makine boyutlarında](../virtual-machines/sizes-previous-gen.md) belgeler
- . Hizmetlere genel kullanıma (GA) kadar bir kategori atanmaz. Bilgi ve önizleme hizmetlerinin bir listesi için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/). 

> [!div class="mx-tableFixed"]
> | Temel                          | Temel                                        | Özelleştirilmiş                                          |
> |---------------------------------------|---------------------------------------------------|------------------------------------------------------|
> | Depolama Hesapları                      | API Management                                    | FHIR için Azure API'si                                   |
> | Application Gateway                   | Uygulama Yapılandırması                                 | Azure Analysis Services                              |
> | Azure Backup                          | App Service                                       | Azure bilişsel hizmetler: anomali algılayıcısı           |
> | Azure Cosmos DB                       | Otomasyon                                        | Azure bilişsel hizmetler: Özel Görüntü İşleme              |
> | Azure Data Lake Storage Gen2          | Azure Active Directory Domain Services            | Azure bilişsel hizmetler: form tanıyıcı            |
> | Azure ExpressRoute                    | Azure Bastion                                     | Azure bilişsel hizmetler: kişiselleştirici               |
> | Azure Genel IP                       | Redis için Azure Önbelleği                             | Azure bilişsel hizmetler: Soru-Cevap Oluşturma                  |
> | Azure SQL Veritabanı                    | Azure Bilişsel Arama                            | MariaDB için Azure Veritabanı                           |
> | Azure SQL: yönetilen örnek          | Azure Bilişsel Hizmetler                          | Azure Veritabanı Geçiş Hizmeti                     |
> | Cloud Services                        | Azure Bilişsel Hizmetler: Görüntü İşleme         | Azure Ayrılmış HSM                                  |
> | Cloud Services: Av2-Series            | Azure bilişsel hizmetler: Content Moderator       | Azure Digital Twins                                  |
> | Cloud Services: Dv2-Series            | Azure bilişsel hizmetler: yüz                    | Azure sistem durumu bot                                     |
> | Cloud Services: Dv3-Series            | Azure bilişsel hizmetler: modern okuyucu        | Azure HPC Önbelleği                                      |
> | Cloud Services: Ev3-Series            | Azure bilişsel hizmetler: Language Understanding  | Azure Lab Services                                   |
> | Cloud Services: örnek düzeyi IP 'Leri    | Azure bilişsel hizmetler: konuşma Hizmetleri         | Azure NetApp Files                                   |
> | Cloud Services: Ayrılmış IP           | Azure Bilişsel Hizmetler: Metin Analizi          | Azure SignalR Hizmeti                                |
> | Disk Depolama                          | Azure bilişsel hizmetler: çevirmen              | Azure yay bulut hizmeti                           |
> | Event Hubs                            | Azure Veri Gezgini                               | Azure Zaman Serisi Görüşleri                           |
> | Key Vault                             | Azure Veri Paylaşımı                                  | Azure VMware Çözümü                                |
> | Yük dengeleyici                         | MySQL için Azure Veritabanı                          | CloudSimple tarafından sunulan Azure VMware Çözümü                 |
> | Service Bus                           | PostgreSQL için Azure Veritabanı                     | Cloud Services: H serisi                             |
> | Service Fabric                        | Azure Databricks                                  | Veri Kataloğu                                         |
> | Depolama: sık/seyrek erişimli BLOB depolama katmanları  | Azure DDoS Koruması                             | Data Lake Analytics                                  |
> | Depolama: yönetilen diskler                | Azure DevTest Labs                                | Azure Machine Learning Studio (klasik)              |
> | Sanal Makine Ölçek Kümeleri            | Azure Güvenlik Duvarı                                    | Spatial Anchors                                      |
> | Sanal Makineler                      | Azure Güvenlik Duvarı Yöneticisi                            | Depolama alanı: Arşiv Depolama                             |
> | Sanal makineler: Av2-Series          | Azure İşlevleri                                   | StorSimple                                           |
> | Sanal makineler: Bs-Series           | Azure IoT Hub                                     | Ultra Disk Depolama                                   |
> | Sanal makineler: DSv2-Series         | Azure Kubernetes Service (AKS)                    | Video Indexer                                        |
> | Sanal makineler: DSv3-Series         | Azure Machine Learning                            | Sanal makineler: DASv4-Series                       |
> | Sanal makineler: Dv2-Series          | Azure Izleyici: Application Insights               | Sanal makineler: DAv4-Series                        |
> | Sanal makineler: Dv3-Series          | Azure Izleyici: Log Analytics                      | Sanal makineler: DCsv2-Series                       |
> | Sanal makineler: ESv3-Series         | Azure Özel Bağlantı                                | Sanal makineler: EASv4-Series                       |
> | Sanal makineler: Ev3-Series          | Azure Red Hat OpenShift                           | Sanal makineler: EAv4-Series                        |
> | Sanal makineler: örnek düzeyi IP 'Leri  | Azure Site Recovery                               | Sanal makineler: HBv1-Series                        |
> | Sanal makineler: Ayrılmış IP         | Azure Stream Analytics                            | Sanal makineler: HBv2-Series                        |
> | Sanal Ağ                       | Azure Synapse Analytics                           | Sanal makineler: HCv1-Series                        |
> | VPN Gateway                           | Batch                                             | Sanal makineler: H serisi                           |
> |                                       | Cloud Services: a serisi                          | Sanal makineler: LSv2-Series                        |
> |                                       | Container Instances                               | Sanal makineler: Mv2-Series                         |
> |                                       | Container Registry                                | Sanal makineler: NCv3-Series                        |
> |                                       | Data Factory                                      | Sanal makineler: NDv2-Series                        |
> |                                       | Event Grid                                        | Sanal makineler: NVv3-Series                        |
> |                                       | HDInsight                                         | Sanal makineler: NVv4-Series                        |> 
> |                                       | Logic Apps                                        | Sanal makineler: Azure üzerinde SAP HANA Büyük Örnekleri  |
> |                                       | Media Services                                    |                                                      |
> |                                       | Ağ İzleyicisi                                   |                                                      |
> |                                       | Notification Hubs                                 |                                                      |
> |                                       | Premium blob depolaması                              |                                                      |
> |                                       | Premium dosya depolaması                             |                                                      |
> |                                       | Sanal makineler: Ddsv4-Series                    |                                                      |
> |                                       | Sanal makineler: Ddv4-Series                     |                                                      |
> |                                       | Sanal makineler: Dsv4-Series                     |                                                      |
> |                                       | Sanal makineler: Dv4-Series                      |                                                      |
> |                                       | Sanal makineler: Edsv4-Series                    |                                                      |
> |                                       | Sanal makineler: Edv4-Series                     |                                                      |
> |                                       | Sanal makineler: Esv4-Series                     |                                                      |
> |                                       | Sanal makineler: Ev4-Series                      |                                                      |
> |                                       | Sanal makineler: Fsv2-Series                     |                                                      |
> |                                       | Sanal makineler: a serisi                        |                                                      |
> |                                       | Sanal WAN                                       |                                                      |


## <a name="next-steps"></a>Sonraki adımlar

- [Azure 'da Kullanılabilirlik Alanları destekleyen bölgeler](az-region.md)
- [Hızlı başlangıç şablonları](https://aka.ms/azqs)
