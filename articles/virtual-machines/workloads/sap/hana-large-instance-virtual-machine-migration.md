---
title: Azure 'da SAP HANA (büyük örnekler) Azure sanal makinelerine geçirme | Microsoft Docs
description: Azure 'da (büyük örnekler) SAP HANA Azure sanal makinelerine geçirme
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/11/2020
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2dd01bf60104939fcf1f9bd1ccec0e7d72710041
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77154926"
---
# <a name="sap-hana-on-azure-large-instance-migration-to-azure-virtual-machines"></a>Azure sanal makinelerine Azure büyük örnek geçişi SAP HANA
Bu makalede olası Azure büyük örnek dağıtım senaryoları açıklanmakta ve küçültülmüş geçiş kapalı kalma süresi ile planlama ve geçiş yaklaşımı sunulmaktadır

## <a name="overview"></a>Genel Bakış
Eylül 2016 ' deki Azure büyük örneklerinin SAP HANA (HLI) duyurusu nedeniyle birçok müşteri bu donanımı bellek içi işlem platformu için bir hizmet olarak benimsemiştir.  Son yıllarda, Azure VM ölçeklenebilirlik uzantısı HANA genişleme dağıtım modelinin desteğiyle birlikte, çoğu kurumsal müşterinin ERP veritabanı kapasitesi talebini aşmış.  SAP HANA iş yüklerini fiziksel sunuculardan Azure VM 'lerine geçirmek istediğinizi ifade eden müşterileri görbaşlayacağız.
Bu kılavuz, adım adım bir yapılandırma belgesi değildir, ancak bunun yerine, geçiş kapalı kalma süresini en aza indirmek için gerekli konuları çağırma konusunda, genel dağıtım modellerini açıklar ve planlama, geçiş önerileri sağlar.

## <a name="assumptions"></a>Varsayımlar
Bu makalede aşağıdaki varsayımların yapar:
- Dikkate alınan tek ilgi, Hana büyük örneğinden (HLI) önemli yazılım yükseltme veya düzeltme eki olmadan Azure VM 'ye yönelik homojen bir HANA veritabanı işlem hizmeti geçişidir. Bu küçük güncelleştirmeler, ilgili SAP notları tarafından desteklenen şekilde daha yeni bir işletim sistemi sürümü veya HANA sürümünün kullanımını içerir. 
- Geçiş işleminden önce veya sonra gerçekleştirilmesi gerekirse tüm güncelleştirmeler/yükseltmeler etkinlikleri.  Örneğin, MCOS SAP HANA MDC dağıtımına dönüştürülüyor. 
- En az kapalı kalma süresini sunan geçiş yaklaşımı sistem çoğaltmasına SAP HANA. Diğer geçiş yöntemleri bu belgenin kapsamının bir parçası değildir.
- Bu, HLI 'nin hem Rev3 hem de Rev4 SKU 'Ları için geçerlidir.
- HANA dağıtım mimarisi, geçiş sırasında öncelikle değiştirilmeden kalır.  Diğer bir deyişle, tek örnekli bir sisteme sahip bir sistem hedefte aynı şekilde kalır.
- Müşteriler, hedef (e) mimarinin Hizmet Düzeyi Sözleşmesi (SLA) ' ni gözden geçirdiğini ve anlamıştır. 
- Müşteriler, HLIs ve VM 'Ler arasındaki ticari koşullara göre farklılık yüzünden, maliyet yönetimi için VM 'lerinin kullanımını izlemelidir.
- Müşteriler, HLı 'in özel bir işlem platformu olduğunu anlayıp kabul etmiş olur. Ancak, VM 'Ler paylaşılan altyapıda çalışır, bu da güvenli ve diğer kiracılardan yalıtılmıştır.
- Müşteriler hedeflenen mimarinizi destekleyen hedef VM 'Leri doğruladı. SAP HANA dağıtımı için sertifikalı tüm desteklenen VM SKU 'Larını görmek için lütfen [SAP HANA donanım dizinini](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)kontrol edin.
- Müşteriler tasarım ve geçiş planını doğruladı.
- Birincil siteyle birlikte olağanüstü durum kurtarma düğümünü planlayın.  Müşteriler, geçişten sonra VM 'lerde çalışan birincil site için HLI DR düğümünü kullanamaz.
- Müşteriler gerekli yedekleme dosyalarını, iş kurtarılabilirlik ve uyumluluk gereksinimlerine bağlı olarak hedef VM 'lere kopyaladı. Bu, geçiş dönemi boyunca zaman içinde kurtarma gereksinimini kapsamalıdır.
- HSR HA için müşterilerin, [SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker) ve [rhel](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker)için SAP HANA ha Kılavuzu başına stonith cihazı ayarlaması ve yapılandırması gerekir.  HLI durumu gibi önceden yapılandırılmış değildir.
- Bu geçiş yaklaşımı, Optane yapılandırmasına sahip HLI SKU 'Larını kapsamaz.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları
HLI müşterilerine sahip ortak dağıtım modelleri aşağıdaki tabloda özetlenmiştir.  Tüm HLI senaryolar için Azure VM 'lerine geçiş mümkündür.  Mevcut Azure hizmet tekliflerinden yararlanmak için birkaç senaryo küçük mimari değişikliği gerektirebilir.

| Senaryo KIMLIĞI | HLI senaryosu | VM 'ye geçiş mı? | Görüyorum |
| --- | --- | --- | --- |
| 1 | [Tek bir SID içeren tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-one-sid) | Yes | - |
| 2 | [MCOS ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-mcos) | Yes | - |
| 3 | [Depolama çoğaltması kullanan DR ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-storage-replication) | Hayır | Depolama çoğaltması Azure sanal platformunda kullanılamaz, geçerli DR çözümünü HSR veya yedekleme/geri yükleme olarak değiştirin |
| 4 | [Depolama çoğaltması kullanarak DR (çok amaçlı) ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-multipurpose-using-storage-replication) | Hayır | Depolama çoğaltması Azure sanal platformunda kullanılamaz, geçerli DR çözümünü HSR veya yedekleme/geri yükleme olarak değiştirin |
| 5 | [Yüksek kullanılabilirlik için STONITH ile HSR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#hsr-with-stonith-for-high-availability) | Yes | Hedef VM 'Ler için önceden yapılandırılmış bir SBD yok.  Bir STONITH çözümü seçin ve dağıtın.  Olası seçenekler: Azure Uçuşlama Aracısı (hem [RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker), [SLES](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)Için desteklenir), SBD |
| 6 | [HSR ile, depolama çoğaltması ile DR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-with-hsr-and-dr-with-storage-replication) | Hayır | DR için depolama çoğaltmasını, HSR veya yedekleme/geri yükleme ile değiştirin |
| 7 | [Konak otomatik yük devretme (1 + 1)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#host-auto-failover-11) | Yes | Azure VM 'leriyle paylaşılan depolama için ANF kullanma |
| 8 | [Bekleme ile genişleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-standby) | Yes | Yalnızca depolama için ANF kullanan M128s, M416s, M416ms VM 'Ler ile siyah beyaz/4HANA |
| 9 | [Bekleme olmadan genişleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-without-standby) | Yes | M128s, M416s, M416ms VM 'Leri ile siyah beyaz/4HANA (depolama için ANF kullanma ile veya olmadan) |
| 10 | [Depolama çoğaltması kullanarak DR ile genişleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-storage-replication) | Hayır | DR için depolama çoğaltmasını, HSR veya yedekleme/geri yükleme ile değiştirin |
| 11 | [HSR kullanarak DR ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-hsr) | Yes | - |
| 12 | [Tek düğümlü HSR-DR (maliyet için iyileştirilmiş)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-hsr-to-dr-cost-optimized) | Yes | - |
| 13 | [HSR ile HA ve DR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr) | Yes | - |
| 14 | [HSR ile HA ve DR (maliyet için iyileştirilmiş)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr-cost-optimized) | Yes | - |
| 15 | [HSR kullanarak DR ile ölçeklendirme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-hsr) | Yes | M128s ile siyah beyaz/4HANA. M416s, M416ms VM 'Leri (depolama için ANF kullanma ile veya olmadan) |


## <a name="source-hli-planning"></a>Kaynak (HLI) planlaması
Bir HLI sunucusu eklerken, hem Microsoft hizmet yönetimi hem de müşterileri, SAP HANA veritabanını çalıştırmaya yönelik işlem, ağ, depolama ve işletim sistemine özgü ayarların planlanmasından geçer.  Azure VM 'ye geçiş için benzer planlama yapılması gerekir.

### <a name="sap-hana-housekeeping"></a>SAP HANA temizlik 
HANA veritabanını geçirmeden önce, istenmeyen, süresi geçmiş veriler veya eski günlüklerin yeni veritabanına geçirilmemesi için veritabanı içeriğini bir yere taşımak iyi bir uygulamadır.  Temizlik genellikle eski, son kullanma veya etkin olmayan verileri silme veya arşivlemeyi içerir.  Bu nedenle, bu ' veri hygiene ' eylemleri üretim kullanımından önce veri kırpma geçerliliğini doğrulamak için üretim dışı sistemlerde test edilmelidir.

### <a name="allow-network-connectivity-for-new-vms-and-or-virtual-network"></a>Yeni VM 'Ler ve sanal ağ için ağ bağlantısına izin ver 
Müşterinin HLI dağıtımında, Azure sanal ağları 'nın ağ bağlantısı, makale [SAP HANA (büyük örnekler) ağ mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture)bölümünde açıklanan bilgilere göre oluşturulmuştur. Ayrıca, ağ trafiği yönlendirme, ' Azure 'da yönlendirme ' bölümünde özetlenen şekilde yapılır.
1. Yeni bir VM 'yi geçiş için HSR hedefi olarak ayarlama amacıyla, yeni Azure VM, IP adresi aralıklarına sahip var olan sanal ağa yerleştirilirse, zaten HLI düğümüne bağlanma iznine sahip olan başka bir HLI bağlantı güncelleştirmesi gerekmez
2. Yeni Azure VM yeni bir sanal ağa yerleştirilmişse (başka bir bölgede olabilir) ve var olan sanal ağla eşlenirse, bu yeni sanal ağ IP aralığına erişime izin vermek için özgün HLı sağlama alanındaki ExpressRoute hizmet anahtarı ve kaynak KIMLIĞI kullanılabilir.  Sanal ağın HLI bağlantı kurmasını sağlamak için Microsoft hizmet yönetimiyle koordine edin.  Note: uygulama ve veritabanı katmanları arasındaki ağ gecikmesini en aza indirmek Için hem uygulama hem de veritabanı katmanları aynı sanal ağda olmalıdır.  

### <a name="existing-app-layer-availability-set-availability-zones-and-proximity-placement-group"></a>Mevcut uygulama katmanı kullanılabilirlik kümesi, Kullanılabilirlik Alanları ve yakınlık yerleşimi grubu
Geçerli dağıtım modeli, belirli hizmet düzeyi hedeflerini karşılamak için yapılır.  Bu taşımanın hedef altyapının ayarlanan hedefleri karşıladığından veya aştığından emin olun.  
Daha büyük olasılıkla, müşteriler SAP uygulama sunucuları bir kullanılabilirlik kümesine yerleştirilir.  Geçerli dağıtım hizmeti düzeyi tatmin edici ise ve 
- SAP HANA sunucusunun değiştirilmesi, DB ana bilgisayar adına ait IP 'Leri değiştirerek ad çözümlemesi aracılığıyla yapılabiliyorsanız, başka hiçbir şeyin gerçekleştirilmesi gerekmez.  
- PPG kullanmıyorsanız, ağ gecikmesini en aza indirmek için tüm uygulama ve DB sunucularını aynı bölgeye yerleştirdiğinizden emin olun.
- PPG kullanıyorsanız, bu belge hedef planlama, kullanılabilirlik kümesi, Kullanılabilirlik Alanları ve yakınlık yerleşimi grubunun (PPG) bölümüne başvurun.

### <a name="storage-replication-discontinuance-process-if-used"></a>Depolama çoğaltması süreksizlik işlemi (kullanılıyorsa)
Depolama çoğaltması DR çözümü olarak kullanılıyorsa, SAP uygulaması kapatıldıktan ve son SAP HANA Katalog, günlük dosyası, veri yedeklemeleri uzak depolama birimlerine çoğaltıldıktan sonra çoğaltmanın sonlandırılması (zamanlanmamış) gerekir.  Bu eylem, fiziksel ve Azure VM geçişi sırasında olağanüstü bir durum oluşması durumunda DR HLI 'da geçerli bir veritabanı olan bir önlem amaçlı tik.

### <a name="data-backups-preservation-consideration"></a>Veri yedeklemeleri saklama değerlendirmesi
Tam geçişi 'in Azure VM 'de SAP HANA ' den sonra, hLi üzerindeki tüm anlık görüntü tabanlı veriler veya günlük yedeklemeleri, gerektiğinde bir VM 'ye kolayca erişilemez veya geri yüklenebilen bir sanal makineye geri aktarılabilir. Erken geçiş süresi boyunca, Azure tabanlı yedekleme, zaman içinde kurtarma gereksinimlerini karşılamak için yeterli geçmiş oluşturmadan önce dosya düzeyinde yedeklemeleri, cutover 'e göre HLI, gün/hafta anlık görüntülerine ek olarak yapmanızı öneririz.  Bu yedeklemelerin yeni SAP HANA VM tarafından erişilebilen bir Azure depolama hesabına kopyalanmasını sağlayabilirsiniz. HLi içeriğini yedeklemeye ek olarak, bir geri alma işlemi gerektiğinde SAP 'nin tam yedeklerinin erişilebilir olması için de akıllıca yöntemi de vardır.

### <a name="adjusting-system-monitoring"></a>Sistem Izlemeyi ayarlama 
Müşteriler, SAP yatay içindeki sistemlere yönelik uyarı bildirimlerini izlemek ve göndermek için birçok farklı araç kullanır.  Bu öğe, gerektiğinde uyarı bildirim alıcılarını izlemek ve güncelleştirmek için herhangi bir ayarlamayı dahil eden yeni VM 'leri eklerken uygun bir eylem için yalnızca basit bir çağrı çıkış işlemi olur.

### <a name="microsoft-operations-team-involvement"></a>Microsoft operasyon ekibi katılımı 
Mevcut HLI örneğindeki Azure portal tabanlarından bir bilet açın.  Destek bileti oluşturulduktan sonra bir destek mühendisi e-postayla sizinle iletişim kuracaktır.  

### <a name="engage-microsoft-account-team"></a>Microsoft hesabı ekibine katılın
İşlem kaynağında masrafın gereksiz kullanımını en aza indirmek için, geçiş, HLI sözleşmesinin yıldönümü yenileme zamanına yakın bir süre sonra planlayın. HLı dikey pencerenin yetkisini almak için, sözleşmenin sonlandırmasını ve birimin gerçek kapatılmasını koordine etmek gerekir.

## <a name="destination-planning"></a>Hedef planlama
Var olan bir altyapının yerini almak için yeni bir altyapının çıkarılması, yeni eklemenin büyük şeyler şemasına sığması için biraz düşünmeye devam eder.  Aşağıda, contemplation için bazı önemli noktaları verilmiştir.

### <a name="resource-availability-in-the-target-region"></a>Hedef bölgede kaynak kullanılabilirliği 
Geçerli SAP uygulama sunucusunun dağıtım bölgesi genellikle ilişkili HLIs ile yakınlardır.  Ancak, HLIs kullanılabilir Azure bölgelerinden daha az sayıda konumda sunulur.  Fiziksel HLI 'dan Azure VM 'ye geçiş yaparken, performans iyileştirmesi için ilgili tüm hizmetlerin yakınlık uzaklığını "hassas ayarlama" fırsatı sunar.  Bunu yaparken, seçili bölgenin ilgilendiğiniz kaynaklara sahip olduğundan emin olmak için önemli bir dikkat edilmelidir.  Örneğin, belirli VM ailesinin kullanılabilirliği veya yüksek kullanılabilirlik için Azure bölgelerinin sunumu.

### <a name="virtual-network"></a>Sanal ağ 
Altyapı mimarı, yeni HANA veritabanının mevcut SAP uygulama sanal ağı içinde mi yoksa yeni bir tane mi oluşturulacağı bir seçeneğe göre yapılır.  Birincil karar verme faktörü, SAP yatay için geçerli ağ yerleşimidir.  Dağıtım altyapısı değişikliği göz önünde bulundurularak, örneğin, bir bölgeden iki bölgeler arası dağıtıma ve PPG 'nin avantajlarından yararlanmaya devam edilir. Bu kullanılabilirlik geliştirmesi mimari değişikliği uygular. Daha fazla bilgi için [SAP uygulamasıyla en iyi ağ gecikmesi Için Azure PPG](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios)makalesine bakın.   

### <a name="security"></a>Güvenlik
Yeni SAP HANA VM 'nin yeni veya mevcut bir VNET/alt ağ üzerinde giriş yapılıp yapılmayacağını belirtir. Bu, güvenli koruma gerektiren yeni bir iş açısından kritik hizmeti temsil eder.  Bu yeni hizmet sınıfının, değerlendirilmek ve dağıtılması için şirket bilgileri güvenlik ilkesiyle uyumlu doğru erişim denetimi. 

### <a name="vm-sizing-recommendation"></a>VM boyutlandırma önerisi
Bu geçiş Ayrıca, HANA işlem motorunuzu doğru boyuta geçirmek için de bir fırsattır.  Bir diğeri, harcama verimliliğini artırmak için doğru boyutlandırmayı sağlayan sistem kaynak tüketimini anlamak için Hana Studio ile birlikte hana [sistem görünümlerinden](https://help.sap.com/viewer/7c78579ce9b14a669c1f3295b0d8ca16/Cloud/3859e48180bb4cf8a207e15cf25a7e57.html) yararlanabilir.

### <a name="storage"></a>Depolama 
Depolama performansı, SAP uygulama kullanıcı deneyimini etkileyen faktörlerden biridir.  Belirli bir VM SKU 'SU temel alınarak, [Azure sanal makine depolama yapılandırmalarının SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)yayınlanan en düşük depolama düzeni önerisi vardır. Yeni HANA VM için yeterli GÇ kapasitesi ve performans sağlamak üzere, bu en düşük özellikleri gözden geçirdikten ve mevcut HLI sistem istatistiklerine karşı karşılaştırılmasını öneririz.

Yeni HANA sanal makinesi ve ilişkili sanal makine için PPG 'yi yapılandırırsanız, depolama ve HANA VM 'nin birlikte konumunu incelemek ve sağlamak için bir destek bileti gönderebilirsiniz.  
Yedekleme çözümünüzün değiştirilmesi gerektiğinden, işlemsel harcama sürprizleri önlemek için depolama maliyetinin de yeniden ziyaret edilmelidir. 

### <a name="storage-replication-for-disaster-recovery"></a>Olağanüstü durum kurtarma için depolama çoğaltma
HLI 'da, depolama çoğaltması HANA veritabanı için olağanüstü durum kurtarma çoğaltması için varsayılan seçenek olarak sunulur. Bu özellik, Azure VM 'nin varsayılan seçeneği değildir. HSR, yedekleme/geri yükleme veya iş gereksinimlerinizi karşılayan diğer desteklenen çözümleri göz önünde bulundurun.

### <a name="availability-sets-availability-zones-and-proximity-placement-groups"></a>Kullanılabilirlik kümeleri, Kullanılabilirlik Alanları ve yakınlık yerleştirme grupları 
Uygulama katmanı ile SAP HANA arasındaki mesafeyi en aza indirmesinin yanı sıra ağ gecikmesini en düşük düzeyde tutmak için yeni veritabanı VM 'si ve geçerli SAP uygulama sunucuları bir yakınlık yerleşimi grubuna (PPG) yerleştirilmelidir. Azure kullanılabilirlik kümesi 'nin ve Kullanılabilirlik Alanları SAP dağıtımları için PPG ile nasıl çalıştığı hakkında bilgi edinmek için [yakınlık yerleşimi grubuna](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) bakın.
Hedef HANA sisteminin üyeleri birden fazla Azure bölgesinde dağıtılmışsa, müşteriler seçili bölgelerin gecikme süresi profilinin net bir görünümüne sahip olmalıdır. SAP sistem bileşenlerinin yerleştirilmesi, SAP uygulaması ve veritabanı arasındaki uzaklığı en iyi şekilde elde etmek için idealdir.  Genel etki alanı [kullanılabilirlik bölgesi gecikme testi aracı](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/master/AvZone-Latency-Test) ölçüyü daha kolay hale getirmeye yardımcı olur.  


### <a name="backup-strategy"></a>Yedekleme stratejisi
Birçok müşteri, HLI SAP HANA için üçüncü taraf yedekleme çözümlerini zaten kullanıyor.  Bu durumda yalnızca ek korumalı bir VM ve HANA veritabanlarının yapılandırılması gerekir.  Artık, makinenin geçiş sonrasında kullanımdan kalkmakta olup olmadığı için sürekli olarak devam eden yedekleme işleri zamanlanmamış.
VM 'de SAP HANA için Azure Backup genel kullanıma sunulmuştur.  Azure VM 'lerinde [yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database), [geri yükleme](https://docs.microsoft.com/azure/backup/sap-hana-db-restore), SAP HANA yedeklemeyi [yönetme](https://docs.microsoft.com/azure/backup/sap-hana-db-manage) hakkında ayrıntılı bilgi için şu bağlantılara bakın:

### <a name="dr-strategy"></a>DR stratejisi
Hizmet düzeyi hedefleriniz, BLOB depolama ve geri yükleme için basit bir yedeklemenin daha uzun bir kurtarma zamanına ve yerinde veya yeni bir VM 'ye sahip olursa, bu en basit ve en ucuz DR stratejisidir.  
HANA DR 'nin genellikle HSR ile yapıldığı büyük örnek platformu gibi Azure VM 'de HSR Ayrıca en doğal ve yerel SAP HANA DR çözümüdür.  Kaynak dağıtımının tek bir örnek olup olmamasına bakılmaksızın, otomatik yük devretme veya çok düğümlü genişleme HANA ile kümelenmiş, kaynak altyapısının hedef HSR çoğaltması DR bölgesinde gereklidir. Bu HSR hedefi DR çoğaltması, birincil HLI 'den VM 'ye geçiş tamamlandıktan sonra oluşturulacak.  DR HANA örneği, ikincil çoğaltma sitesi olarak sanal makine örneğindeki birincil SAP HANA kaydedilecek.  

### <a name="sap-application-server-connectivity-destination-change"></a>SAP uygulama sunucusu bağlantısı hedef değişikliği
HSR geçiş yaklaşımı, yeni bir HANA DB konağına neden olur ve bu nedenle uygulama katmanının yeni bir DB ana bilgisayar adına, SAP profillerinin yeni ana bilgisayar adını yansıtacak şekilde değiştirilmesi gerekir.  Anahtar, ana bilgisayar adını koruyan ad çözümlemesi tarafından yapıldıysa, profil değişikliği gerekli değildir.

### <a name="operating-system"></a>İşletim sistemi
Aynı yayın düzeyinde olsalar bile, HLI ve VM için işletim sistemi görüntüleri, örneğin, SLES 11 SP4 aynı değildir. Müşteriler, hedef VM 'nin işletim sistemine yüklenebilmeleri için gerekli paketleri, sık düzeltmeler, düzeltme ekleri, çekirdekten ve güvenlik düzeltmelerinin HLI 'de doğrulanması gerekir.  Ayrıca, müşterilerin SAP HSR için hedef VM 'de daha yeni bir işletim sistemi sürümünü yapılandırması yaygın bir sürümüdür.  Bu, [SAP note 2763388](https://launchpad.support.sap.com/#/notes/2763388)başına desteklenir.

### <a name="new-sap-license-request"></a>Yeni SAP lisans isteği
Yeni HANA sistemi için yeni bir SAP lisansı istemek üzere VM 'lere geçirilme için basit bir çağrı çıkışı.

### <a name="service-level-agreement-sla-differences"></a>Hizmet düzeyi sözleşmesi (SLA) farkları
HLI ve Azure VM arasında kullanılabilirlik SLA 'Sı farkını çağırmak gibi yazarlar.  Örneğin, kümelenmiş HLIs HA çiftleri% 99,99 kullanılabilirlik sağlar. Aynı SLA 'yı başarmak için, birinin kullanılabilirlik bölgelerinde VM 'Leri dağıtması gerekir. Bu [makalede](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_9/) , müşterilerin bu hedef altyapıyı uygun şekilde planlayabilmeleri için ilgili dağıtım mimarilerinden kullanılabilirliği açıklanır.


## <a name="migration-strategy"></a>Geçiş stratejisi
Bu belgede, HLI 'dan Azure VM 'ye geçiş için yalnızca HANA sistem çoğaltma yaklaşımını ele alınmaktadır.  Dağıtılan hedef depolama çözümüne bağlıdır, işlem biraz farklılık gösterir. Üst düzey adımlar aşağıda açıklanmıştır.

### <a name="vm-with-premiumultra-disks-for-data"></a>Veriler için Premium/Ultra disklere sahip VM
Premium veya ultra disklerle dağıtılan VM 'Ler için standart SAP HANA sistem çoğaltması yapılandırması uygulanır ve HSR 'yi kurmak için kullanılabilir.  Başvurulan [SAP yardım makalesi](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/099caa1959ce4b3fa1144562fa09e163.html) , iki sistem arasında sistem çoğaltmasını ayarlama, ikincil bir sisteme yük devretme, birincil sisteme geri dönme ve sistem çoğaltmasını devre dışı bırakma hakkındaki adımlara genel bir bakış sağlar.  Geçiş amacına yönelik olarak, ara kaynak adımlarında, sistem çoğaltmasını yalnızca kuruluma, almaya ve devre dışı bırakmaya ihtiyacımız olacak.  

### <a name="vm-with-anf-for-data-and-log-volumes"></a>Veri ve günlük birimleri için ANF ile VM
Yüksek düzeyde, tam verilerin ve günlük birimlerinin en son HLI depolama anlık görüntülerinin, hedef HANA VM tarafından erişilebilir ve kurtarılabilir oldukları Azure depolama 'ya kopyalanması gerekir.  Kopyalama işlemi, herhangi bir yerel Linux kopyalama araçlarıyla yapılabilir.  

>[!Important]
> Kopyalama ve veri aktarımı, HANA veritabanı boyutuna ve ağ bant genişliğine bağlı olarak saat alabilir.  Kopyalama işleminin toplu işlemi, birincil HANA DB kapalı kalma süresinin sonunda yapılmalıdır.

### <a name="mcos-to-mdc-conversion"></a>MCOS-MDC dönüştürmesi
Bir sistem (MCOS) dağıtım modelindeki birden çok bileşen, bazı HLI müşterilerimizden seçilmiş.  Mosyon, önceki SAP HANA sürümlerinin birden çok veritabanı kapsayıcısı (MDC) depolama anlık görüntü sınırlamasını atlamamıştı.  MCOS modelinde, bazı bağımsız SAP HANA örnekleri tek bir HLI dikey pencerede yığılır.  Geçiş için HSR kullanmak, her biri bir kiracı DB ile birden çok HANA VM 'ye neden olur.  Bu son durum, bir müşterinin acözel hale gelmesini sağlayan bir busier Yatayı kolaylaştırır.  SAP HANA 2,0 varsayılan dağıtımının MDC olması sayesinde, HSR geçişinden sonra [Hana kiracı taşıma](https://launchpad.support.sap.com/#/notes/2472478) işlemini gerçekleştirmede uygun bir alternatiftir.  Bu işlem, bu bağımsız HANA veritabanlarını tek bir HANA kapsayıcısında cokiracılar halinde birleştirir.  

### <a name="application-layer-consideration"></a>Uygulama katmanı değerlendirmesi
DB sunucusu bir SAP sisteminin merkezi olarak görüntülenir.  Tüm uygulama sunucuları SAP HANA DB 'nin yakınında bulunmalıdır.  Bazı durumlarda PPG 'nin yeni kullanımı istendiğinde, mevcut uygulama sunucularının HANA VM 'nin gerekli olabileceği PPG üzerine yerini değiştirme.  Dağıtım şablonlarına zaten sahipseniz yeni uygulama sunucularının oluşturulması daha kolay olabilir.  
Mevcut uygulama sunucuları ve yeni HANA VM en iyi şekilde bulunursa, ek kapasite istenmediği takdirde yeni uygulama sunucularının oluşturulması gerekmez.  
Hizmet kullanılabilirliğini iyileştirmek için yeni bir altyapı oluşturulmazsa, mevcut uygulama sunucuları gereksiz hale gelebilir ve kapatılması ve silinmesi gerekir.
Hedef VM ana bilgisayar adı değiştiyse ve HLI ana bilgisayar adından farklıysa, SAP uygulama sunucusu profillerinin yeni ana bilgisayarı işaret etmek üzere ayarlanması gerekir.  Yalnızca HANA DB IP adresi değiştiyse, yeni HANA VM 'sine gelen bağlantıları sağlamak için bir DNS kaydı güncelleştirmesi gerekir.

### <a name="acceptance-test"></a>Kabul testi
HLI 'dan VM 'ye geçiş, heterojen bir geçişe kıyasla veritabanı içeriğine hiçbir malzeme değişikliği yapmaz, ancak yeni kurulumun önemli işlevlerini ve performans oranını doğrulamanızı öneririz.  

### <a name="cutover-plan"></a>Cutover planı
Bu geçiş doğrudan ileri doğru olsa da, var olan bir veritabanının yetkisini almayı içerir.  Kaynak sistemi ilişkili içeriğiyle korumak için dikkatli planlama ve geri dönüş gerekli olduğunda yedekleme görüntüleri kritik öneme sahiptir.  İyi planlama, bir spedıma ters değişikliği sağlar.   


## <a name="post-migration"></a>Geçiş sonrası
Veri bütünlüğünün korunduğundan emin olmak için, herhangi bir HLI bağımlı hizmeti veya bağlantıyı güvenli bir şekilde ayırdığımızda geçiş işi yapılmaz.  Ayrıca, gereksiz hizmetleri kapatır.  Bu bölüm, birkaç önemli öğeyi çağırır.

### <a name="decommissioning-the-hli"></a>HLI 'nın yetkisini alma
HANA DB 'nin Azure VM 'ye başarılı bir şekilde geçişten sonra, HLI DB 'de üretim iş işlemlerinin gerçekleştirilmesi gerekmez.  Ancak, HLI 'nın yerel yedek veri saklama penceresine eşit bir dönemde tutulması, gerektiğinde veri kurtarmayı sağlayan güvenli bir uygulamadır.  Yalnızca, HLı dikey pencerenin kullanımdan alınması gerekir.  Teknik kullanım açısından, en iyi ilgilendikleri müşterilerin Microsoft ile sonuçlandırma taahhütlerini sözleşmeye dayalı gerekir.  Müşteriler, HLı yetki alma sürecinde çalışmak için Microsoft temsilcileriyle bağlantı etmelidir.

### <a name="remove-any-proxy-ex-iptables-bigip-configured-for-hli"></a>HLI için yapılandırılmış tüm proxy 'leri (örn. Iptables, BIGIP) kaldırın 
Şirket içi trafiği HLI 'ya yönlendirmek için bir Iptables gibi bir proxy hizmeti kullanılıyorsa, sanal makineye başarılı geçişten sonra bu hizmet artık gerekli değildir.  Ancak, bu bağlantı hizmeti için, yetki alma konusunda anlatıldığı gibi, HLı dikey pencere hala devam ettiği sürece bekletilecektir.  Bu hizmet, HLı dikey pencerenin tamamen kullanımdan kaldırıldıktan sonra kapatılabilir. 

### <a name="remove-global-reach-for-hli"></a>HLI için Global Reach kaldır 
Global Reach genellikle müşterinin ExpressRoute ağ geçidini HLI ExpressRoute ağ geçidi ile bağlamak için kullanılır. Bu, müşterinin Şirket içi trafiğinin, bir proxy hizmeti gerekmeden HLI kiracıya doğrudan ulaşmasını sağlar.  Iptables ara hizmetinde olduğu gibi, HLı dikey pencere tamamen kullanımdan kaldırılana kadar GlobalReach tutulmalıdır.

### <a name="operating-system-subscription--movereuse"></a>İşletim sistemi aboneliği – taşı/yeniden kullan
VM sunucuları Stood ve HLI Blade 'ler kullanımdan kaldırılarak, işletim sistemi abonelikleri, iki farklı işletim sistemi lisansı ödemesinden kaçınmak için değiştirilebilir veya yeniden kullanılabilir.



## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:
- [Azure 'da altyapı yapılandırma ve işlemlerini SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
- [Azure 'Da SAP iş yükleri: planlama ve dağıtım denetim listesi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist).
