---
title: Azure Kullanılabilirlik Bölgeleri ile SAP iş yükü yapılandırmaları | Microsoft Dokümanlar
description: Azure Kullanılabilirlik Bölgelerini kullanarak SAP NetWeaver için yüksek kullanılabilirlik mimarisi ve senaryoları
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 887caaec-02ba-4711-bd4d-204a7d16b32b
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/05/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7a92bef85cd4ee7530940a065135e88c7530781
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78675600"
---
# <a name="sap-workload-configurations-with-azure-availability-zones"></a>Azure Kullanılabilirlik Alanlarıyla SAP iş yükü yapılandırmaları
[Azure Kullanılabilirlik Bölgeleri,](https://docs.microsoft.com/azure/availability-zones/az-overview) Azure'un sağladığı yüksek kullanılabilirlik özelliklerinden biridir. Kullanılabilirlik Bölgelerini kullanmak, Azure'daki SAP iş yüklerinin genel kullanılabilirliğini artırır. Bu özellik zaten bazı [Azure bölgelerinde](https://azure.microsoft.com/global-infrastructure/regions/)kullanılabilir. Gelecekte, daha fazla bölgede satışa sunulacak.

Bu grafik SAP yüksek kullanılabilirliğinin temel mimarisini gösterir:

![Standart yüksek kullanılabilirlik yapılandırması](./media/sap-ha-availability-zones/standard-ha-config.png)

SAP uygulama katmanı bir Azure [kullanılabilirlik kümesine](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)dağıtılır. SAP Central Hizmetlerinin yüksek kullanılabilirliği için, iki VM'yi ayrı bir kullanılabilirlik kümesine dağıtabilirsiniz. Windows Server Failover Clustering veya Pacemaker 'ı (Linux) bir altyapı veya yazılım sorunu durumunda otomatik olarak başarısız olan yüksek kullanılabilirlik çerçevesi olarak kullanın. Bu dağıtımlar hakkında daha fazla bilgi edinmek için bkz:

- [Bir KÜME paylaşılan diskkullanarak windows failover kümesinde sap ASCS/SCS örneğini kümele](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [Dosya paylaşımını kullanarak Bir Windows failover kümesinde sap ASCS/SCS örneğini kümeleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)
- [SAP uygulamaları için SUSE Linux Enterprise Server'da Azure VM'lerde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)
- [Red Hat Enterprise Linux'ta SAP NetWeaver için Azure Sanal Makineler yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel)

Benzer bir mimari SAP NetWeaver, S/4HANA veya Hybris sistemlerinin DBMS katmanı için de geçerlidir. Altyapı veya yazılım hatalarından korumak için DBMS katmanını etkin/pasif modda, başarısız küme çözümüyle dağıtırsınız. Failover cluster çözümü DBMS'ye özgü bir failover çerçevesi, Windows Server Failover Clustering veya Pacemaker olabilir.

Azure Kullanılabilirlik Bölgeleri'ni kullanarak aynı mimariyi dağıtmak için, daha önce özetlenen mimaride bazı değişiklikler yapmanız gerekir. Bu makalede, bu değişiklikler açıklanır.

## <a name="considerations-for-deploying-across-availability-zones"></a>Kullanılabilirlik Bölgeleri arasında dağıtım için dikkat edilecek hususlar


Kullanılabilirlik Bölgeleri'ni kullanırken aşağıdakileri göz önünde bulundurun:

- Bir Azure bölgesindeki çeşitli Kullanılabilirlik Bölgeleri arasındaki mesafelerle ilgili hiçbir garanti yoktur.
- Kullanılabilirlik Bölgeleri ideal bir DR çözümü değildir. Doğal afetler, enerji altyapılarına ağır hasar lar da dahil olmak üzere dünya bölgelerinde yaygın hasara yol açabilir. Çeşitli bölgeler arasındaki mesafeler uygun bir DR çözümü oluşturacak kadar büyük olmayabilir.
- Kullanılabilirlik Bölgeleri arasında ağ gecikmesi tüm Azure bölgelerinde aynı değildir. Bazı durumlarda, ağ bir bölgeden etkin DBMS VM'ye kadar olan ağ gecikmesi kabul edilebilir olduğundan SAP uygulama katmanını farklı bölgeler arasında dağıtabilir ve çalıştırabilirsiniz. Ancak bazı Azure bölgelerinde, etkin DBMS VM ile farklı bölgelerde dağıtıldığında SAP uygulama örneği arasındaki gecikme SAP iş süreçleri için kabul edilebilir olmayabilir. Bu gibi durumlarda, uygulama için etkin/etkin bir mimari veya bölge arası ağ gecikmesinin çok yüksek olduğu etkin/pasif mimariile dağıtım mimarisinin farklı olması gerekir.
- Kullanılabilirlik Bölgelerini nerede kullanacağınıza karar verirken, kararınızı bölgeler arasındaki ağ gecikmesine temel alasınız. Ağ gecikmesi iki alanda önemli bir rol oynar:
    - Senkron çoğaltma olması gereken iki DBMS örneği arasındaki gecikme. Ağ gecikmesi ne kadar yüksekse, iş yükünüzün ölçeklenebilirliğini etkileme olasılığı da o kadar yüksek olur.
    - Etkin DBMS örneği yle sap iletişim örneğini bölge içinde çalıştıran bir VM ile başka bir bölgede benzer bir VM arasındaki ağ gecikmesi farkı. Bu fark arttıkça, dbms ile veya farklı bir bölgede bölgede çalışıp çalışmadıklarına bağlı olarak, iş süreçlerinin ve toplu iş işlerinin çalışma süresi üzerindeki etkisi de artar.

Azure VM'lerini Kullanılabilirlik Bölgeleri arasında dağıtırken ve aynı Azure bölgesinde başarısız çözümler oluşturduğunuzda, bazı kısıtlamalar geçerlidir:

- Azure Kullanılabilirlik Bölgeleri'ne dağıtırken [Azure Yönetilen Diskler](https://azure.microsoft.com/services/managed-disks/) kullanmanız gerekir. 
- Bölge yıllarının fiziksel bölgelere eşlenemeleri Azure abonelik temelinde sabitlenir. SAP sistemlerinizi dağıtmak için farklı abonelikler kullanıyorsanız, her abonelik için ideal bölgeleri tanımlamanız gerekir.
- [Azure Yakınlık Yerleşim Grubu'ni](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)kullanmadığınız sürece Azure kullanılabilirlik kümelerini Azure Kullanılabilirlik Bölgesi'nde dağıtamazsınız. SAP DBMS katmanını ve merkezi hizmetleri bölgeler arasında dağıtma ve aynı zamanda kullanılabilirlik kümelerini kullanarak SAP uygulama katmanını dağıtma ve vm'lere yakın yakınlık elde etme şekli, [SAP uygulamalarıyla en iyi ağ gecikmesi için Azure Yakınlık Yerleşim Grupları](sap-proximity-placement-scenarios.md)makalesinde belgelenmiştir. Azure yakınlık yerleşim gruplarından yararlanmıyorsanız, sanal makineler için dağıtım çerçevesi olarak birini veya diğerini seçmeniz gerekir.
- Windows Server Failover Clustering veya Linux Pacemaker tabanlı başarısız küme çözümleri oluşturmak için [Azure Temel Yük Dengeleyicisi](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) kullanamazsınız. Bunun yerine, Azure [Standart Yük DengeleyiciSKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)kullanmanız gerekir.



## <a name="the-ideal-availability-zones-combination"></a>İdeal Kullanılabilirlik Bölgeleri kombinasyonu
Kullanılabilirlik Bölgelerini nasıl kullanacağınıza karar vermeden önce şunları belirlemeniz gerekir:

- Azure bölgesinin üç bölgesi arasında ağ gecikmesi. Bu, bölgeler arası ağ trafiğinde en az ağ gecikmesine sahip bölgeleri seçmenize olanak tanır.
- Seçtiğiniz bölgelerden birinde VM-vm gecikmesi ile seçtiğiniz iki bölge arasındaki ağ gecikmesi arasındaki fark.
- Dağıtmanız gereken VM türlerinin seçtiğiniz iki bölgede kullanılabilir olup olmadığınıbelirleme. Bazı VM'lerde, özellikle M-Serisi VM'lerde, bazı SNU'ların üç bölgeden yalnızca ikisinde kullanılabildiği durumlarla karşılaşabilirsiniz.

## <a name="network-latency-between-and-within-zones"></a>Bölgeler arasında ve bölgeler içinde ağ gecikmesi
Farklı bölgeler arasındaki gecikmeyi belirlemek için şunları yapmanız gerekir:

- DBMS örneğiniz için kullanmak istediğiniz VM SKU'yu üç bölgeye de dağıtın. Bu ölçümü yaparken [Azure Hızlandırılmış Ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) Özelliğinin etkin olduğundan emin olun.
- En az ağ gecikmesine sahip iki bölge bulduğunuzda, üç Kullanılabilirlik Bölgesi boyunca uygulama katmanı VM olarak kullanmak istediğiniz VM SKU'nun üç VM'sini daha dağıtın. Seçtiğiniz iki DBMS bölgesinde ağ gecikmesini iki DBMS VM'ye göre ölçün. 
- **Niping'i** bir ölçme aracı olarak kullanın. Sap'den gelen bu araç, SAP destek notlarında [#500235](https://launchpad.support.sap.com/#/notes/500235) ve [#1100926](https://launchpad.support.sap.com/#/notes/1100926/E)açıklanmıştır. Gecikme ölçümleri için belgelenen komutlara odaklanın. **Ping** Azure Hızlandırılmış Ağ kodu yollarında çalışmadığından, bunu kullanmanızı önermiyoruz.

Bu testleri el ile yapmanız gerekmez. Açıklanan gecikme testlerini otomatikleştiren bir PowerShell yordam [Kullanılabilirlik Bölgesi Gecikme Testi](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/master/AvZone-Latency-Test) bulabilirsiniz. 

Ölçümlerinize ve VM SCO'larınızın Kullanılabilirlik Bölgelerindeki kullanılabilirliğine bağlı olarak bazı kararlar almanız gerekir:

- DBMS katmanı için ideal bölgeleri tanımlayın.
- Etkin SAP uygulama katmanınızı bölgeler arasında ağ gecikmesi farklılıklarına bağlı olarak bir, iki veya üç bölgeye dağıtmak isteyip istemediğinizi belirleyin.
- Uygulama açısından etkin/pasif yapılandırma mı yoksa etkin/etkin yapılandırma mı dağıtmak istediğinizi belirleyin. (Bu yapılandırmalar daha sonra bu makalede açıklanmıştır.)

Bu kararları verirken, SAP [notunda](https://launchpad.support.sap.com/#/notes/1100926/E)#1100926 belgelenen SAP'nin ağ gecikmesi önerilerini de dikkate alın.

> [!IMPORTANT]
> Yaptığınız ölçümler ve kararlar, ölçümleri aldığınızda kullandığınız Azure aboneliği için geçerlidir. Başka bir Azure aboneliği kullanıyorsanız, ölçümleri yinelemeniz gerekir. Numaralandırılmış bölgelerin eşlenemesi başka bir Azure aboneliği için farklı olabilir.


> [!IMPORTANT]
> Daha önce açıklanan [ölçümlerin, Kullanılabilirlik Bölgelerini](https://docs.microsoft.com/azure/availability-zones/az-overview)destekleyen her Azure bölgesinde farklı sonuçlar sağlaması beklenmektedir. Ağ gecikme gereksinimleriniz aynı olsa bile, bölgeler arasındaki ağ gecikmesi farklı olabileceğinden, farklı Azure bölgelerinde farklı dağıtım stratejileri benimsemeniz gerekebilir. Bazı Azure bölgelerinde, üç farklı bölge arasındaki ağ gecikmesi çok farklı olabilir. Diğer bölgelerde, üç farklı bölge arasındaki ağ gecikmesi daha düzgün olabilir. Her zaman 1 ile 2 milisaniye arasında bir ağ gecikmesi olduğu iddiası doğru değildir. Azure bölgelerindeki Kullanılabilirlik Bölgeleri arasında ağ gecikmesi genelleştirilemiyor.

## <a name="activeactive-deployment"></a>Etkin/Etkin dağıtım
Etkin SAP uygulama sunucularınızı iki veya üç bölgeye dağıttığınızdan, bu dağıtım mimarisi etkin/etkin olarak adlandırılır. Enqueue çoğaltma kullanan SAP Merkezi Hizmetler örneği iki bölge arasında dağıtılır. Aynı durum, SAP Merkezi Hizmeti ile aynı bölgelere dağıtılacak olan DBMS katmanı için de geçerlidir.

Bu yapılandırmayı göz önünde bulundurarak, bölgenizde iş yükünüz ve eşzamanlı DBMS çoğaltmanız için kabul edilebilir bölgeler arası ağ gecikmesi sunan iki Kullanılabilirlik Bölgesi ni bulmanız gerekir. Ayrıca, seçtiğiniz bölgeler içindeki ağ gecikmesi ile bölgeler arası ağ gecikmesi arasındaki deltanın çok büyük olmadığından da emin olmak istersiniz. Bunun nedeni, bir işin DBMS sunucusuyla veya bölgeler arasında, iş süreçlerinizin veya toplu işlerinizin çalışma saatlerinde bölgede veya bölgeler arasında çalışıp çalışmadığına bağlı olarak büyük varyasyonlar istememenizdir. Bazı varyasyonlar kabul edilebilir, ancak fark faktörleri değildir.

İki bölgede etkin/etkin bir dağıtımın basitleştirilmiş şeması şu şekilde görünebilir:

![Etkin/Etkin bölge dağıtımı](./media/sap-ha-availability-zones/active_active_zones_deployment.png)

Bu yapılandırma için aşağıdaki hususlar geçerlidir:

- Azure [Yakınlık Yerleşim Grubu'nu](https://docs.microsoft.com/azure/virtual-machines/linux/co-location)kullanmadığınızda, kullanılabilirlik kümeleri Azure Kullanılabilirlik Bölgelerinde dağıtılamayacağından, Azure Kullanılabilirlik Bölgeleri'ni tüm VM'ler için hata olarak kabul eder ve etki alanlarını güncellersiniz.
- DBMS katmanı ve merkezi hizmetler için yersel dağıtımları birleştirmek, ancak uygulama katmanı için Azure kullanılabilirlik kümelerini kullanmak istiyorsanız, [SAP uygulamalarıyla en iyi ağ gecikmesi için Azure Yakınlık Yerleşim Grupları](sap-proximity-placement-scenarios.md)makalesinde açıklandığı gibi Azure yakınlık gruplarını kullanmanız gerekir.
- SAP Merkezi Hizmetleri ve DBMS katmanının arıza kümelerinin yük dengeleyicileri için [Standart SKU Azure Yük Dengeleyicisini](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)kullanmanız gerekir. Temel Yük Dengeleyicisi bölgeler arasında çalışmaz.
- SAP sistemini barındırmak için dağıttığınız Azure sanal ağı, alt ağlarıyla birlikte bölgelere doğru genişletilir. Her bölge için ayrı sanal ağlara ihtiyacınız yoktur.
- Dağıttığınız tüm sanal makineler de [Azure Yönetilen Diskler'i](https://azure.microsoft.com/services/managed-disks/)kullanmanız gerekir. Yönetilmeyen diskler, yerdeğiştirmeler için desteklenmez.
- Azure Premium Depolama ve [Ultra SSD depolama alanı,](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bölgeler arasında herhangi bir depolama çoğaltma türünü desteklemez. Uygulama (DBMS veya SAP Merkezi Hizmetleri) önemli verileri çoğaltmalıdır.
- Aynı durum paylaşılan bir disk (Windows), CIFS paylaşımı (Windows) veya NFS paylaşımı (Linux) olan paylaşılan sapmnt dizini için de geçerlidir. Bu paylaşılan diskleri veya bölgeler arasındaki paylaşımları çoğaltan bir teknoloji kullanmanız gerekir. Bu teknolojiler desteklenir:
  - Windows için, Azure'da bir küme paylaşılan disk kullanarak Windows [failover kümesinde sap ASCS/SCS örneğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)belgelenen SIOS DataKeeper kullanan bir küme çözümü.
  - SUSE Linux için, [SUSE Linux Enterprise Server'daki Azure VM'lerde NFS'nin Yüksek kullanılabilirlikte](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)belgelenmiş olarak oluşturulmuş bir NFS payı.
    
    Şu anda, Microsoft Scale-Out File Server'ı kullanan çözüm, Sap [ascs/SCS örnekleri için Windows failover kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirliği için Azure altyapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)hazırlayın'da belgelenen çözüm, bölgeler arasında desteklenmez.
- Üçüncü bölge, bir [SUSE Linux Pacemaker kümesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri oluşturmanız durumunda SBD aygıtını barındırmak için kullanılır.
- Kritik iş süreçleri için çalışma süresi tutarlılığı elde etmek için, SAP toplu iş sunucu grupları, SAP oturum açma grupları veya RFC gruplarını kullanarak belirli toplu iş ve kullanıcıları etkin DBMS örneğiyle birlikte bölge içinde olan uygulama örneklerine yönlendirmeyi deneyebilirsiniz. Ancak, bir öten aletme durumunda, bu grupları etkin DB VM ile bölgede çalışan VM'lerde çalışan örneklere el ile taşımanız gerekir.  
- Her bölgeye atıl iletişim örnekleri dağıtmak isteyebilirsiniz. Bu, uygulama örneklerinizin bir bölümü tarafından kullanılan bir bölge hizmet dışıysa, eski kaynak kapasitesine anında geri dönüş sağlamaktır.

> [!IMPORTANT]
> Bu etkin/etkin senaryoda bant genişliği için ek ücretler Microsoft tarafından 04/01/2020 tarihinden itibaren duyurulur. Belge [bant genişliği Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/)kontrol edin. SAP uygulama katmanı ile SAP DBMS katmanı arasındaki veri aktarımı oldukça yoğundur. Bu nedenle etkin/etkin senaryo maliyetlere biraz katkıda bulunabilir. Tam maliyetleri almak için bu makaleyi kontrol etmeye devam edin 


## <a name="activepassive-deployment"></a>Etkin/Pasif dağıtım
Bir bölge içindeki ağ gecikmesi ile çapraz bölge ağ trafiğinin gecikmesi arasında kabul edilebilir bir delta bulamıyorsanız, SAP uygulama katmanı açısından etkin/pasif karaktere sahip bir mimari dağıtabilirsiniz. Tüm uygulama katmanını dağıttığınız ve hem etkin DBMS'yi hem de SAP Merkezi Hizmetleri örneğini çalıştırmaya çalıştığınız bölge olan *etkin* bir bölge tanımlarsınız. Böyle bir yapılandırmayla, bir işin etkin DBMS örneğinde ve toplu işişlerinde bölgede çalışıp çalışmadığına bağlı olarak, aşırı çalışma süresi varyasyonları olmadığından emin olmanız gerekir.

Mimarinin temel düzeni aşağıdaki gibi görünür:

![Etkin/Pasif bölge dağıtımı](./media/sap-ha-availability-zones/active_passive_zones_deployment.png)

Bu yapılandırma için aşağıdaki hususlar geçerlidir:

- Kullanılabilirlik kümeleri Azure Kullanılabilirlik Bölgelerinde dağıtılamaz. Bunu telafi etmek için, [SAP uygulamalarında en iyi ağ gecikmesi için Azure Yakınlık Yerleşim Grupları](sap-proximity-placement-scenarios.md)makalesinde belgelenen Azure yakınlık yerleşim gruplarını kullanabilirsiniz.
- Bu mimariyi kullandığınızda, durumu yakından izlemeniz ve etkin DBMS ve SAP Merkezi Hizmetleri örneklerini dağıtılan uygulama katmanınızla aynı bölgede tutmaya çalışmanız gerekir. SAP Merkezi Hizmetinin veya DBMS örneğinin başarısız olması durumunda, SAP uygulama katmanı mümkün olan en kısa sürede dağıtılırken bölgeye el ile geri dönebileceğinizden emin olmak istersiniz.
- SAP Merkezi Hizmetleri ve DBMS katmanının arıza kümelerinin yük dengeleyicileri için [Standart SKU Azure Yük Dengeleyicisini](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)kullanmanız gerekir. Temel Yük Dengeleyicisi bölgeler arasında çalışmaz.
- SAP sistemini barındırmak için dağıttığınız Azure sanal ağı, alt ağlarıyla birlikte bölgelere doğru genişletilir. Her bölge için ayrı sanal ağlara ihtiyacınız yoktur.
- Dağıttığınız tüm sanal makineler de [Azure Yönetilen Diskler'i](https://azure.microsoft.com/services/managed-disks/)kullanmanız gerekir. Yönetilmeyen diskler, yerdeğiştirmeler için desteklenmez.
- Azure Premium Depolama ve [Ultra SSD depolama alanı,](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bölgeler arasında herhangi bir depolama çoğaltma türünü desteklemez. Uygulama (DBMS veya SAP Merkezi Hizmetleri) önemli verileri çoğaltmalıdır.
- Aynı durum paylaşılan bir disk (Windows), CIFS paylaşımı (Windows) veya NFS paylaşımı (Linux) olan paylaşılan sapmnt dizini için de geçerlidir. Bu paylaşılan diskleri veya bölgeler arasındaki paylaşımları çoğaltan bir teknoloji kullanmanız gerekir. Bu teknolojiler desteklenir:
    - Windows için, Azure'da bir küme paylaşılan disk kullanarak Windows [failover kümesinde sap ASCS/SCS örneğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)belgelenen SIOS DataKeeper kullanan bir küme çözümü.
    - SUSE Linux için, [SUSE Linux Enterprise Server'daki Azure VM'lerde NFS'nin Yüksek kullanılabilirlikte](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)belgelenmiş olarak oluşturulmuş bir NFS payı.
    
  Şu anda, Microsoft Scale-Out File Server'ı kullanan çözüm, Sap [ascs/SCS örnekleri için Windows failover kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirliği için Azure altyapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)hazırlayın'da belgelenen çözüm, bölgeler arasında desteklenmez.
- Üçüncü bölge, bir [SUSE Linux Pacemaker kümesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri oluşturmanız durumunda SBD aygıtını barındırmak için kullanılır.
- Bir bölge hatası durumunda uygulama kaynaklarını başlatabilmek için pasif bölgeye (DBMS bakış açısından) atıl durumdaki VM'leri dağıtmanız gerekir.
    - [Azure Site Kurtarma](https://azure.microsoft.com/services/site-recovery/) şu anda bölgeler arasında devre dışı kakacak aktif VM'leri çoğaltamıyor. 
- Bölge arızası durumunda SAP uygulama katmanını ikinci bölgede otomatik olarak başlatmanızı sağlayan otomasyona yatırım yapmalısınız.

## <a name="combined-high-availability-and-disaster-recovery-configuration"></a>Kombine yüksek kullanılabilirlik ve olağanüstü durum kurtarma yapılandırması
Microsoft, azure bölgesinde farklı Azure Kullanılabilirlik Bölgeleri barındıran tesisler arasındaki coğrafi uzaklıklar hakkında hiçbir bilgi paylaşmaz. Yine de, bazı müşteriler sıfır bir kurtarma noktası hedefi (RPO) vaat eden bir kombine HA ve DR yapılandırmaiçin bölgeleri kullanıyor. Bu, olağanüstü durum kurtarma durumunda bile taahhüt edilen veritabanı hareketlerini kaybetmemeniz gerektiği anlamına gelir. 

> [!NOTE]
> Böyle bir yapılandırmayı yalnızca belirli durumlarda kullanmanızı öneririz. Örneğin, veriler Güvenlik veya uyumluluk nedenleriyle Azure bölgesinden ayrıladığında bunu kullanabilirsiniz. 

Böyle bir yapılandırmanın nasıl görünebileceğine ilişkin bir örnek aşağıda verilmiştir:

![Bölgelerde kombine yüksek kullanılabilirlikLI DR](./media/sap-ha-availability-zones/combined_ha_dr_in_zones.png)

Bu yapılandırma için aşağıdaki hususlar geçerlidir:

- Kullanılabilirlik Bölgesi'ne ev sahipliği yapan tesisler arasında önemli bir mesafe olduğunu veya belirli bir Azure bölgesinde kalmak zorunda kaldığınızı varsayıyorsanız. Kullanılabilirlik kümeleri Azure Kullanılabilirlik Bölgelerinde dağıtılamaz. Bunu telafi etmek için, [SAP uygulamalarında en iyi ağ gecikmesi için Azure Yakınlık Yerleşim Grupları](sap-proximity-placement-scenarios.md)makalesinde belgelenen Azure yakınlık yerleşim gruplarını kullanabilirsiniz.
- Bu mimariyi kullandığınızda, durumu yakından izlemeniz ve etkin DBMS ve SAP Merkezi Hizmetleri örneklerini dağıtılan uygulama katmanınızla aynı bölgede tutmaya çalışmanız gerekir. SAP Merkezi Hizmetinin veya DBMS örneğinin başarısız olması durumunda, SAP uygulama katmanı mümkün olan en kısa sürede dağıtılırken bölgeye el ile geri dönebileceğinizden emin olmak istersiniz.
- Etkin QA uygulama örneklerini çalıştıran VM'lerde önceden yüklenmiş üretim uygulaması örnekleri olmalıdır.
- Bölge hatası durumunda, QA uygulama örneklerini kapatın ve bunun yerine üretim örneklerini başlatın. Bu işi yapmak için uygulama örnekleri için sanal adlar kullanmanız gerektiğini unutmayın.
- SAP Merkezi Hizmetleri ve DBMS katmanının arıza kümelerinin yük dengeleyicileri için [Standart SKU Azure Yük Dengeleyicisini](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)kullanmanız gerekir. Temel Yük Dengeleyicisi bölgeler arasında çalışmaz.
- SAP sistemini barındırmak için dağıttığınız Azure sanal ağı, alt ağlarıyla birlikte bölgelere doğru genişletilir. Her bölge için ayrı sanal ağlara ihtiyacınız yoktur.
- Dağıttığınız tüm sanal makineler de [Azure Yönetilen Diskler'i](https://azure.microsoft.com/services/managed-disks/)kullanmanız gerekir. Yönetilmeyen diskler, yerdeğiştirmeler için desteklenmez.
- Azure Premium Depolama ve [Ultra SSD depolama alanı,](https://docs.microsoft.com/azure/virtual-machines/windows/disks-ultra-ssd) bölgeler arasında herhangi bir depolama çoğaltma türünü desteklemez. Uygulama (DBMS veya SAP Merkezi Hizmetleri) önemli verileri çoğaltmalıdır.
- Aynı durum paylaşılan bir disk (Windows), CIFS paylaşımı (Windows) veya NFS paylaşımı (Linux) olan paylaşılan sapmnt dizini için de geçerlidir. Bu paylaşılan diskleri veya bölgeler arasındaki paylaşımları çoğaltan bir teknoloji kullanmanız gerekir. Bu teknolojiler desteklenir:
    - Windows için, Azure'da bir küme paylaşılan disk kullanarak Windows [failover kümesinde sap ASCS/SCS örneğinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)belgelenen SIOS DataKeeper kullanan bir küme çözümü.
    - SUSE Linux için, [SUSE Linux Enterprise Server'daki Azure VM'lerde NFS'nin Yüksek kullanılabilirlikte](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)belgelenmiş olarak oluşturulmuş bir NFS payı.

  Şu anda, Microsoft Scale-Out File Server'ı kullanan çözüm, Sap [ascs/SCS örnekleri için Windows failover kümesi ve dosya paylaşımı kullanarak SAP yüksek kullanılabilirliği için Azure altyapısı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)hazırlayın'da belgelenen çözüm, bölgeler arasında desteklenmez.
- Üçüncü bölge, bir [SUSE Linux Pacemaker kümesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#create-azure-fence-agent-stonith-device) veya ek uygulama örnekleri oluşturmanız durumunda SBD aygıtını barındırmak için kullanılır.





## <a name="next-steps"></a>Sonraki adımlar
Azure Kullanılabilirlik Bölgeleri arasında dağıtmaya yönelik sonraki adımlar şunlardır:

- [Azure'da bir küme paylaşılan disk kullanarak bir Windows failover kümesinde SAP ASCS/SCS örneğini kümeleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-shared-disk)
- [SAP ASCS/SCS örnekleri için Windows failover kümesi ve dosya paylaşımı kullanarak Azure altyapılarını SAP yüksek kullanılabilirliği için hazırlama](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-infrastructure-wsfc-file-share)






