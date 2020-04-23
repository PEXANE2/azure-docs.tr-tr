---
title: Azure Kullanılabilirlik Bölgeleri nelerdir?
description: Kullanılabilirlik Bölgeleri, Azure'da yüksek oranda kullanılabilir ve esnek uygulamalar oluşturmak için kaynaklarınızı çalıştırmak için kullanabileceğiniz fiziksel olarak ayrı konumlar sağlar.
author: cynthn
ms.service: azure
ms.topic: article
ms.date: 10/17/2019
ms.author: cynthn
ms.custom: fasttrack-edit, mvc
ms.openlocfilehash: e7ba62a96a6fc76ad63960cfe57f5b1f85589c32
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085051"
---
# <a name="what-are-availability-zones-in-azure"></a>Azure'daki Kullanılabilirlik Bölgeleri nelerdir?
Kullanılabilirlik Bölgeleri, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirlik teklifidir. Kullanılabilirlik Alanları, Azure bölgesi içinde fiziksel olarak benzersiz konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılığı güvence altına almak için etkinleştirilmiş tüm bölgelerde en az üç ayrı alan vardır. Kullanılabilirlik Bölgelerinin bir bölge içindeki fiziksel olarak ayrılması, uygulamaları ve verileri veri merkezi hatalarından korur. Bölge yedekli hizmetler, tek hata noktalarından korunmak için uygulamalarınızı ve verilerinizi Kullanılabilirlik Bölgeleri arasında çoğaltır. Kullanılabilirlik Bölgeleri ile Azure, endüstrinin en iyi %99,99'luk VM çalışma süresi SLA'sını sunar. [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) şartları, Azure’un tamamının kullanılabilirlik garantisini açıklamaktadır.

Azure bölgesindeki Kullanılabilirlik Bölgesi, hata etki alanı ve güncelleştirme etki alanının birleşimidir. Örneğin, bir Azure bölgesinde üç bölgede üç veya daha fazla VM oluşturursanız, VM'leriniz üç hata etki alanına ve üç güncelleştirme etki alanına etkili bir şekilde dağıtılır. Azure platformu, farklı bölgelerdeki VM'lerin aynı anda güncelleştirilemediğinden emin olmak için bu dağıtımı güncelleştirme etki alanlarında tanır.

İşleminizi, depolamanızı, ağınızı ve veri kaynaklarınızı bir bölge içinde birlikte bularak ve diğer bölgelerde çoğaltarak uygulama mimarinizde yüksek kullanılabilirlik oluşturun. Kullanılabilirlik Alanlarını destekleyen Azure hizmetleri iki kategoriye ayrılır:

- **Zonal hizmetler** – kaynağı belirli bir bölgeye (örneğin, sanal makineler, yönetilen diskler, Standart IP adresleri) sabitlersiniz veya
- **Alanlar arası yedekli hizmetler**: Platform alanlar arasında otomatik olarak çoğaltılır (alanlar arası yedekli depolama, SQL Veritabanı gibi).

Azure'da kapsamlı iş sürekliliği elde etmek için, Kullanılabilirlik Bölgeleri ile Azure bölge çiftleri birleşimini kullanarak uygulama mimarinizi oluşturun. Yüksek kullanılabilirlik için bir Azure bölgesindeki Kullanılabilirlik Bölgeleri'ni kullanarak uygulamalarınızı ve verilerinizi senkronize olarak çoğaltabilir ve olağanüstü durum kurtarma koruması için Azure bölgelerinde eşzamanlı olarak çoğaltabilirsiniz.
 
![bir bölgede aşağı inen bir bölgenin kavramsal görünümü](./media/az-overview/az-graphic-two.png)

> [!IMPORTANT]
> Kullanılabilirlik Bölgesi tanımlayıcıları (yukarıdaki resimdeki 1, 2 ve 3 sayıları) mantıksal olarak her abonelik için gerçek fiziksel bölgelere bağımsız olarak eşlenir. Bu, belirli bir abonelikteki Kullanılabilirlik Bölgesi 1'in, farklı bir abonelikteki Kullanılabilirlik Bölgesi 1'den farklı bir fiziksel bölgeye atıfta bulunabileceği anlamına gelir. Sonuç olarak, sanal makine yerleşimi için farklı abonelikler arasında Kullanılabilirlik Bölgesi adlarına güvenmemeönerilir.

## <a name="services-support-by-region"></a>Bölgeye göre hizmet desteği

Kullanılabilirlik Bölgelerini destekleyen Azure hizmetlerinin ve bölgelerinbirleşimleri şunlardır:


|                                 |Kuzey ve Güney Amerika |              |           |           | Avrupa |              |          |              | Asya Pasifik |                 |                |
|----------------------------|----------|----------|---------|---------|--------------|------------|--------|----------|----------|-------------|-------------|
|          |Orta ABD|Doğu ABD|Doğu ABD 2|Batı ABD 2|Orta Fransa|Kuzey Avrupa|Güney Birleşik Krallık|Batı Avrupa|Doğu Japonya|Güneydoğu Asya|Doğu Avustralya|
| **İşlem**                         |            |              |           |           |                |              |          |             |            |                |                |
| Linux Sanal Makineleri          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Windows Sanal Makineleri        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Sanal Makine Ölçek Kümeleri      | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure Uygulama Hizmet Ortamları ILB | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Azure Kubernetes Service        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Depolama**   |            |              |           |           |                |              |          |             |            |                |                |
| Yönetilen Diskler                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Bölge yedekli Depolama          | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| **Ağ**                     |            |              |           |           |                |              |          |             |            |                |                |
| Standart IP Adresi        | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| Standart Yük Dengeleyici     | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;   | &#10003;       |                |
| VPN Gateway            | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| ExpressRoute Ağ Geçidi   | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| Uygulama Ağ Geçidi(V2)    | &#10003;   |  &#10003;    | &#10003;  | &#10003;  | &#10003;       | &#10003;     |  &#10003;  | &#10003;    |  &#10003;   | &#10003;       |                |
| Azure Güvenlik Duvarı           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| **Veritabanları**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure Veri Gezgini                   | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    | &#10003;        | &#10003;       |                |
| SQL Veritabanı                    | &#10003;   | &#10003;     | &#10003;  | &#10003;(Önizleme) | &#10003;       | &#10003;(Önizleme)     | &#10003; | &#10003;    | &#10003;       | &#10003;       |&#10003;        |
| Redis için Azure Önbelleği           | &#10003;   | &#10003;     | &#10003;  | &#10003;  | &#10003;       | &#10003;     | &#10003; | &#10003;    |  &#10003;       | &#10003;       |                |
| Azure Cosmos DB                    | &#10003;   |  &#10003;  |  &#10003; | &#10003; |       |     | &#10003; |  &#10003;   |            | &#10003;       |                |
| **Analiz**                       |            |              |           |           |                |              |          |             |            |                |                |
| Event Hubs                      | &#10003;   |   &#10003; | &#10003;  | &#10003;  | &#10003; | &#10003; | &#10003; | &#10003; | &#10003; | &#10003;       |                |
| **Tümleştirme**                     |            |              |           |           |                |              |          |             |            |                |                |
| Servis Otobüsü (Sadece Premium Tier) | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| Event Grid | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |
| **Kimlik**                     |            |              |           |           |                |              |          |             |            |                |                |
| Azure AD Domain Services | &#10003;   |  &#10003;  | &#10003;  | &#10003;  | &#10003;  | &#10003;     |&#10003;   | &#10003;    |&#10003;      | &#10003;       |                |

## <a name="services-resiliency"></a>Hizmet esnekliği
Tüm Azure yönetim hizmetleri, bölge düzeyindeki hatalardan esnek olacak şekilde tasarlanır. Hata spektrumunda, bir bölge içindeki bir veya daha fazla Kullanılabilirlik Bölgesi hatası, tüm bölge hatasıyla karşılaştırıldığında daha küçük bir hata yarıçapına sahiptir. Azure, bölge içindeki veya başka bir Azure bölgesinde yönetim hizmetlerinin bölge düzeyinde ki başarısızlığından kurtarabilir. Azure, bir bölge içindeki Kullanılabilirlik Bölgeleri arasında dağıtılan müşteri kaynaklarını etkileyen hataları önlemek için, bir bölge içinde her seferinde kritik bakım gerçekleştirir.

## <a name="pricing"></a>Fiyatlandırma
Kullanılabilirlik Bölgesinde dağıtılan sanal makineler için ek bir ücret yoktur. Bir Azure bölgesinde iki veya daha fazla Kullanılabilirlik Bölgesi'nde iki veya daha fazla VM dağıtıldığında %99,99 VM çalışma süresi SLA sunulur. İlave Stoklar Arası Bölge VM-vM veri aktarım ücretleri olacaktır. Daha fazla bilgi için [Bant Genişliği fiyatlandırma](https://azure.microsoft.com/pricing/details/bandwidth/) sayfasını inceleyin.


## <a name="get-started-with-availability-zones"></a>Kullanılabilirlik Bölgeleri ile başlayın
- [Sanal bir makine oluşturma](../virtual-machines/windows/create-portal-availability-zone.md)
- [PowerShell kullanarak Yönetilen Disk Ekleme](../virtual-machines/windows/attach-disk-ps.md#add-an-empty-data-disk-to-a-virtual-machine)
- [Bölge gereksiz sanal makine ölçek kümesi oluşturma](../virtual-machine-scale-sets/virtual-machine-scale-sets-use-availability-zones.md)
- [Bölge yedekli ön uça sahip standart yük dengeleyicisi kullanarak bölgeler arasında yük dengesi VM'leri](../load-balancer/load-balancer-standard-public-zone-redundant-cli.md)
- [Bölge ön ucuna sahip standart yük dengeleyicisi kullanarak bölge içindeki yük dengesi VM'leri](../load-balancer/load-balancer-standard-public-zonal-cli.md)
- [Alanlar arası yedekli depolama](../storage/common/storage-redundancy-zrs.md)
- [SQL Veritabanı](../sql-database/sql-database-high-availability.md#zone-redundant-configuration)
- [Event Hubs coğrafi olağanüstü durum kurtarma](../event-hubs/event-hubs-geo-dr.md#availability-zones)
- [Service Bus coğrafi olağanüstü durum kurtarma](../service-bus-messaging/service-bus-geo-dr.md#availability-zones)
- [Alanlar arası yedekli sanal ağ geçidi oluşturma](../vpn-gateway/create-zone-redundant-vnet-gateway.md)
- [Azure Cosmos DB için bölge gereksiz bölge ekleme](../cosmos-db/high-availability.md#availability-zone-support)
- [Redis Kullanılabilirlik Bölgeleri için Azure Önbelleğine Başlarken](https://aka.ms/redis/az/getstarted)
- [Azure Active Directory Domain Services örneği oluşturma](../active-directory-domain-services/tutorial-create-instance.md)
- [Kullanılabilirlik Bölgeleri kullanan bir Azure Kubernetes Hizmeti (AKS) kümesi oluşturma](../aks/availability-zones.md)

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı başlangıç şablonları](https://aka.ms/azqs)
