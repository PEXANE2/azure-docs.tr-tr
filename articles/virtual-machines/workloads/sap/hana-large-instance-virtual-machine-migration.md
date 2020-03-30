---
title: SAP HANA'yı Azure'da (Büyük Örnekler) Azure sanal makinelerine geçirme| Microsoft Dokümanlar
description: SAP HANA'yı Azure'da (Büyük Örnekler) Azure sanal makinelerine geçirme
services: virtual-machines-linux
documentationcenter: ''
author: bentrin
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/11/2020
ms.author: bentrin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fd1267711871b3e55f1a6229e46ae27b360322f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617032"
---
# <a name="sap-hana-on-azure-large-instance-migration-to-azure-virtual-machines"></a>Azure Sanal Makinelere Azure Büyük Örnek geçişinde SAP HANA
Bu makalede, olası Azure Büyük Örnek dağıtım senaryoları açıklanır ve en aza indirgenen geçiş kapalı kalma süresiyle planlama ve geçiş yaklaşımı sunar

## <a name="overview"></a>Genel Bakış
Eylül 2016'da SAP HANA için Azure Büyük Örnekleri'nin (HLI) duyurulup duyurulup, birçok müşteri bu donanımı bellek içi hesaplama platformları için bir hizmet teklifi olarak benimsemiştir.  Son yıllarda, Azure VM boyut uzantısı, HANA ölçekli dağıtım desteği ile birleştiğinde çoğu kurumsal müşterinin ERP veritabanı kapasite talebini aştı.  Müşterilerin SAP HANA iş yüklerini fiziksel sunuculardan Azure VM'lerine geçirme ilgilerini ifade ettiğini görmeye başlarız.
Bu kılavuz adım adım yapılandırma belgesi değildir. Ortak dağıtım modellerini açıklar ve planlama ve geçiş önerir.  Amaç geçiş kesintisi en aza indirmek için hazırlık için gerekli hususlar ı çağırmaktır.

## <a name="assumptions"></a>Varsayımlar
Bu makalede, aşağıdaki varsayımlar yapar:
- Dikkate alınan tek ilgi, önemli yazılım yükseltmesi veya düzeltme olmadan Hana Büyük Örnek'ten (HLI) Azure VM'ye homojen bir HANA veritabanı hesaplama hizmeti geçişidir. Bu küçük güncelleştirmeler, daha yeni bir işletim sistemi sürümünün veya HANA sürümünün kullanımını açıkça ilgili SAP notları tarafından desteklenmiş olarak ifade eder. 
- Tüm güncelleştirmeler/yükseltme etkinlikleri geçişten önce veya sonra yapılmalıdır.  Örneğin, SAP HANA MCOS MDC dağıtımına dönüştürme. 
- En az kapalı kalma süresi sunacak geçiş yaklaşımı SAP HANA Sistem Çoğaltma'dır. Diğer geçiş yöntemleri bu belgenin kapsamının bir parçası değildir.
- Bu kılavuz HLI rev3 ve Rev4 SUS hem de geçerlidir.
- HANA dağıtım mimarisi öncelikle geçiş sırasında değişmeden kalır.  Yani, tek örnek DR ile bir sistem hedef aynı şekilde kalacaktır.
- Müşteriler, hedef (olacak) mimarisinin Hizmet Düzeyi Sözleşmesi'ni (SLA) gözden geçirip anlamıştır. 
- HLI'ler ve VM'ler arasındaki ticari terimler farklıdır. Müşteriler maliyet yönetimi için VM'lerinin kullanımını izlemelidir.
- Müşteriler, VM'ler paylaşılan ancak yalıtılmış altyapıda çalışırken HLI'nın özel bir işlem platformu olduğunu anlar.
- Müşteriler, hedef VM'lerin amaçlanan mimarinizi desteklediğini doğrulamışlardır. SAP HANA dağıtımı için onaylı tüm desteklenen VM SK'leri görmek için [SAP HANA donanım dizinine](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)bakın.
- Müşteriler tasarım ve geçiş planını doğruladı.
- Birincil site ile birlikte olağanüstü durum kurtarma VM planı.  Müşteriler geçişten sonra VM'lerde çalışan birincil site için HLI düğüm adını kullanamaz.
- Müşteriler, iş kurtarılabilirliği ve uyumluluk gereksinimlerine bağlı olarak VM'leri hedeflemek için gerekli yedekleme dosyalarını kopyaladı. VM erişilebilir yedeklemeleri ile geçiş döneminde zaman içinde kurtarma sağlar.
- HSR HA için müşterilerin SLES ve [RHEL](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker)için SAP HANA HA kılavuzları başına STONITH cihazını kurmaları ve yapılandırmaları [gerekmektedir.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)  HLI davası gibi önceden yapılandırılmamış.
- Bu geçiş yaklaşımı, Optane yapılandırması ile HLI SK'leri kapsamaz.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları
HLI müşterileri ile ortak dağıtım modelleri aşağıdaki tabloda özetlenmiştir.  Tüm HLI senaryoları için Azure VM'lerine geçiş mümkündür.  Kullanılabilir tamamlayıcı Azure hizmetlerinden yararlanmak için küçük mimari değişiklikler gerekebilir.

| Senaryo Kimliği | HLI Senaryosu | VM'ye harfi harfine mi geçirin? | Açıklama |
| --- | --- | --- | --- |
| 1 | [Bir SID ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-one-sid) | Evet | - |
| 2 | [MCOS ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-mcos) | Evet | - |
| 3 | [Depolama çoğaltma kullanarak DR ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-storage-replication) | Hayır | Azure sanal platformunda depolama çoğaltma kullanılamıyor, geçerli DR çözümünü HSR veya yedekleme/geri yükleme ile değiştirin |
| 4 | [Depolama çoğaltma kullanarak DR (çok amaçlı) ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-multipurpose-using-storage-replication) | Hayır | Azure sanal platformunda depolama çoğaltma kullanılamıyor, geçerli DR çözümünü HSR veya yedekleme/geri yükleme ile değiştirin |
| 5 | [Yüksek kullanılabilirlik için STONITH ile HSR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#hsr-with-stonith-for-high-availability) | Evet | Hedef VM'ler için önceden yapılandırılmış SBD yok.  Bir STONITH çözümünü seçin ve dağıtın.  Olası seçenekler: Azure Eskrim Aracısı (hem [RHEL,](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker) [SLES, SBD](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)için desteklenir |
| 6 | [HSR ile HA, depolama çoğaltma ile DR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-with-hsr-and-dr-with-storage-replication) | Hayır | DR ihtiyaçları için depolama çoğaltmaişlemini HSR veya yedekleme/geri yükleme ile değiştirme |
| 7 | [Ana bilgisayar otomatik arıza (1+1)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#host-auto-failover-11) | Evet | Azure VM'lerle paylaşılan depolama için ANF'yi kullanma |
| 8 | [Bekleme ile ölçeklendirme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-standby) | Evet | M128s, M416s, M416ms VMs ile BW/4HANA sadece depolama için ANF kullanarak |
| 9 | [Bekleme olmadan ölçeklendirme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-without-standby) | Evet | M128s, M416s, M416ms VMs ile BW/4HANA (depolama için ANF ile veya kullanmadan) |
| 10 | [Depolama çoğaltma kullanarak DR ile ölçeklendirme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-storage-replication) | Hayır | DR ihtiyaçları için depolama çoğaltmaişlemini HSR veya yedekleme/geri yükleme ile değiştirme |
| 11 | [HSR kullanarak DR ile tek düğüm](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-with-dr-using-hsr) | Evet | - |
| 12 | [Dr tek düğüm HSR (maliyet optimize)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#single-node-hsr-to-dr-cost-optimized) | Evet | - |
| 13 | [HSR ile HA ve DR](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr) | Evet | - |
| 14 | [HSR ile HA ve DR (maliyet optimize edilmiş)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#high-availability-and-disaster-recovery-with-hsr-cost-optimized) | Evet | - |
| 15 | [HSR kullanarak DR ile ölçeklendirme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-supported-scenario#scale-out-with-dr-using-hsr) | Evet | M128s ile BW/4HANA. M416s, M416ms VM 'ler (DEPOLAMA İçİn ANF ile veya kullanmadan) |


## <a name="source-hli-planning"></a>Kaynak (HLI) planlaması
Bir HLI sunucusuna binerken, hem Microsoft Hizmet Yönetimi hem de müşteriler SAP HANA veritabanını çalıştırmak için bilgi işlem, ağ, depolama ve işletim sistemi tabanlı ayarların planlanmasından geçer.  Azure VM'ye geçiş için de benzer planlamaların gerçekleşmesi gerekir.

### <a name="sap-hana-housekeeping"></a>SAP HANA temizlik 
İstenmeyen, güncel olmayan verileri veya eski günlükleri yeni veritabanına geçirilmemiş kadar veritabanı içeriğini toparlamak için iyi bir operasyonel uygulamadır.  Temizlik genellikle eski, süresi dolmuş veya etkin olmayan verilerin silmesini veya arşivleilmesini içerir.  Bu 'veri hijyeni' eylemleri, üretim kullanımından önce veri kırpma geçerliliklerini doğrulamak için üretim dışı sistemlerde test edilmelidir.

### <a name="allow-network-connectivity-for-new-vms-and-or-virtual-network"></a>Yeni VM'ler ve veya sanal ağ için ağ bağlantısına izin ver 
Bir müşterinin HLI dağıtımında, ağ [SAP HANA (Büyük Örnekler) ağ mimarisinde](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture)açıklanan bilgilere dayalı olarak kurulmuştur. Ayrıca, ağ trafiği yönlendirmesi 'Azure'da Yönlendirme' bölümünde belirtildiği şekilde yapılır.
- Geçiş hedefi olarak yeni bir VM ayarlarken, Varolan sanal ağa IP adresi aralıkları ile yerleştirilmişse, HLI'ya bağlanmasına izin verilmişse, başka bağlantı güncelleştirmesi gerekmez.
- Yeni Azure VM'si yeni bir Microsoft Azure Sanal Ağına yerleştirilmişse, başka bir bölgede olabilir ve varolan sanal ağa bakıyorsa, orijinal HLI sağlamadan ExpressRoute hizmet anahtarı ve Kaynak Kimliği bu yeni sanal ağa erişim sağlamak için kullanılabilir ağ IP aralığı.  Sanal ağın HLI bağlantısına olanak sağlamak için Microsoft Service Management ile işbirliği kurun.  Not: Uygulama ve veritabanı katmanları arasındaki ağ gecikmesini en aza indirmek için hem uygulama hem de veritabanı katmanlarının aynı sanal ağda olması gerekir.  

### <a name="existing-app-layer-availability-set-availability-zones-and-proximity-placement-group-ppg"></a>Mevcut uygulama katmanı Kullanılabilirlik Kümesi, Kullanılabilirlik Bölgeleri ve Yakınlık Yerleşim Grubu (PPG)
Geçerli dağıtım modeli, belirli hizmet düzeyi hedeflerini karşılamak için yapılır.  Bu hamlede, hedef altyapının belirlenen hedefleri karşılamasını veya aştığından emin olun.  
Büyük olasılıkla, müşteriler SAP uygulama sunucuları bir kullanılabilirlik kümesine yerleştirilir.  Geçerli dağıtım hizmet düzeyi tatmin ediciyse ve 
- Hedef VM, HLI mantıksal adının ana adını varsayarsa, Etki alanı adı hizmetini (DNS) VM'nin IP'sini işaret eden adres çözünürlüğünü güncelleştirmek herhangi bir SAP profilini güncelleştirmeden çalışır
- PPG kullanmıyorsanız, ağ gecikmesini en aza indirmek için tüm uygulamayı ve DB sunucularını aynı bölgeye yerleştirdiğinden emin olun.
- PPG kullanıyorsanız, bu belgenin 'Hedef Planlama, Kullanılabilirlik Kümesi, Kullanılabilirlik Bölgeleri ve Yakınlık Yerleşim Grubu (PPG)' bölümüne bakın.

### <a name="storage-replication-discontinuance-process-if-used"></a>Depolama çoğaltma durdurma işlemi (kullanılırsa)
Depolama çoğaltma DR çözüm olarak kullanılırsa, SAP uygulaması kapatıldıktan sonra sonlandırılmalıdır (planlanmamış).  Buna ek olarak, son SAP HANA kataloğu, günlük dosyası ve veri Yedekleri uzak DR HLI depolama birimleri üzerine çoğaltılmıştır.  Bunu, fiziksel sunucudan Azure VM geçişisırasında bir felaket olması durumunda önlem olarak bunu yapmak.

### <a name="data-backups-preservation-consideration"></a>Veri yedeklemeleri koruma değerlendirmesi
Azure VM'de SAP HANA'ya kesildikten sonra, HLI'daki tüm anlık görüntü tabanlı veriler veya günlük yedeklemeleri gerekirse VM'ye kolayca erişilemez veya geri yüklenebilir. Erken geçiş döneminde, Azure tabanlı yedekleme, Zaman Noktası kurtarma gereksinimlerini karşılamak için yeterli geçmiş oluşturmadan önce, kesmeden günler veya haftalar önce HLI'daki anlık görüntülere ek olarak dosya düzeyinde yedeklemeler almanızı öneririz.  Bu yedeklemelerin yeni SAP HANA VM tarafından erişilebilen bir Azure Depolama hesabına kopyalanmasını isteyin.
HLI içeriğini yedeklemenin yanı sıra, geri alma gerektiğinde SAP ortamının tam yedeklemelerine kolayca erişilebiliyor olmak da ihtiyatlı bir davranıştır.

### <a name="adjusting-system-monitoring"></a>Sistem izlemenin ayarlanması 
Müşteriler, SAP ortamındaki sistemler için uyarı bildirimlerini izlemek ve göndermek için birçok farklı araç kullanır.  Bu öğe, izleme için değişiklikleri birleştirmek ve gerekirse uyarı bildirimi alıcılarını güncelleştirmek için uygun eylem için yalnızca bir çağrıdır.

### <a name="microsoft-operations-team-involvement"></a>Microsoft Operations ekibinin katılımı 
Azure portalından mevcut HLI örneğine göre bir bilet açın.  Destek bileti oluşturulduktan sonra, bir destek mühendisi sizinle e-posta yoluyla iletişime geçecektir.  

### <a name="engage-microsoft-account-team"></a>Microsoft hesap ekibini meşgul edin
Bilgi işlem kaynağındaki gereksiz giderleri en aza indirmek için HLI sözleşmesinin yıldönümü yenileme süresine yakın geçiş planı yapın. HLI bıçağının devre dışı bırakılması için, sözleşmenin feshi ve ünitenin fiilen kapatılmasını koordine etmek gerekir.

## <a name="destination-planning"></a>Hedef planlama
Mevcut bir yerine yeni bir altyapı ayakta yeni ek şeylerin büyük düzeni uyacak sağlamak için bazı düşünce hak ediyor.  Aşağıda tefekkür için bazı önemli noktalar vardır.

### <a name="resource-availability-in-the-target-region"></a>Hedef bölgede kaynak kullanılabilirliği 
Geçerli SAP uygulama sunucularının dağıtım bölgesi genellikle ilişkili HLI'lara yakındır.  Ancak, HLI'ler kullanılabilir Azure bölgelerinden daha az konumda sunulur.  Fiziksel HLI'yi Azure VM'ye geçirdiğinizde, performans optimizasyonu için ilgili tüm hizmetlerin yakınlık mesafesini 'hassas ayarlama' yapmak için de iyi bir zaman.  Bunu yaparken, önemli bir husus seçilen bölgenin gerekli tüm kaynaklara sahip olduğundan emin olmaktır.  Örneğin, belirli VM ailesinin kullanılabilirliği veya yüksek kullanılabilirlik kurulumu için Azure Bölgeleri teklifi.

### <a name="virtual-network"></a>Sanal ağ 
Müşterilerin yeni HANA veritabanını varolan bir sanal ağda çalıştırıp çalıştırmayacağını veya yeni bir ağ oluşturup oluşturmayacağını seçmeleri gerekir.  Birincil belirleyici faktör SAP manzara için geçerli ağ düzenidir.  Ayrıca altyapı tek bölgeden iki bölgeye dağıtım alaerdiğinde ve PPG kullandığında, mimari değişim uygular. Daha fazla bilgi [için, SAP uygulamasıyla en iyi ağ gecikmesi için Azure PPG makalesine](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios)bakın.   

### <a name="security"></a>Güvenlik
Yeni SAP HANA VM yeni veya mevcut vnet/subnet üzerine iniş olsun, koruma gerektiren yeni bir iş kritik hizmeti temsil eder.  Şirket bilgi güvenliği ilkesiyle uyumlu erişim denetimi, bu yeni hizmet sınıfı için değerlendirilmeli ve dağıtılmalıdır.

### <a name="vm-sizing-recommendation"></a>VM boyutlandırma önerisi
Bu geçiş aynı zamanda HANA işlem motorunuzu doğru boyutlandırmak için de bir fırsattır.  Hana Studio ile birlikte hana [sistem görünümlerini](https://help.sap.com/viewer/7c78579ce9b14a669c1f3295b0d8ca16/Cloud/3859e48180bb4cf8a207e15cf25a7e57.html) kullanarak sistem kaynak tüketimini anlayabilir, bu da harcama verimliliğini artırmak için doğru boyutlandırmaya olanak sağlar.

### <a name="storage"></a>Depolama 
Depolama performansı, SAP uygulaması kullanıcı deneyimini etkileyen faktörlerden biridir.  Belirli bir VM SKU temel, minimum depolama düzeni sap [HANA Azure sanal makine depolama yapılandırmaları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)yayınlandı. Bu minimum özellikleri gözden geçirmenizi ve yeni HANA VM için yeterli IO kapasitesi ve performansı sağlamak için mevcut HLI sistem istatistikleriyle karşılaştırmanızı öneririz.

PPG'yi yeni HANA VM ve ilişkili severlikleri için yapılandırırsanız, depolama alanının ve VM'nin birlikte konumunu incelemek ve sağlamak için bir destek bileti gönderin. Yedekleme çözümünüzün değişmesi gerekebileceğinden, operasyonel harcama sürprizlerini önlemek için depolama maliyeti nin de yeniden gözden geçirilmesi gerekir.

### <a name="storage-replication-for-disaster-recovery"></a>Olağanüstü durum kurtarma için depolama çoğaltma
HLI ile, olağanüstü durum kurtarma için varsayılan seçenek olarak depolama çoğaltma sunuldu. Bu özellik, Azure VM'deki SAP HANA için varsayılan seçenek değildir. HSR, yedekleme/geri yükleme veya iş ihtiyaçlarınızı karşılayan diğer desteklenen çözümleri göz önünde bulundurun.

### <a name="availability-sets-availability-zones-and-proximity-placement-groups"></a>Kullanılabilirlik Setleri, Kullanılabilirlik Bölgeleri ve Yakınlık Yerleşim Grupları 
Ağ gecikmesini minimumda tutmak için uygulama katmanı ile SAP HANA arasındaki mesafeyi kısaltmak için, yeni veritabanı VM ve geçerli SAP uygulama sunucuları ppg'ye yerleştirilmelidir. Azure Kullanılabilirlik Kümesi ve Kullanılabilirlik Bölgeleri'nin SAP dağıtımları için PPG ile nasıl çalıştığını öğrenmek için [Yakınlık Yerleşim Grubu'na](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-proximity-placement-scenarios) bakın.
Hedef HANA sisteminin üyeleri birden fazla Azure Bölgesinde dağıtılırsa, müşterilerin seçilen bölgelerin gecikme profilini net bir şekilde görmeleri gerekir. SAP sistem bileşenlerinin yerleştirilmesi, SAP uygulaması ile veritabanı arasındaki proksimal mesafe açısından en uygun şekildedir.  Kamu malı [Kullanılabilirlik bölgesi gecikme testi aracı](https://github.com/Azure/SAP-on-Azure-Scripts-and-Utilities/tree/master/AvZone-Latency-Test) ölçümü kolaylaştırıyor.  


### <a name="backup-strategy"></a>Yedekleme stratejisi
Birçok müşteri zaten HLI SAP HANA için üçüncü taraf yedekleme çözümleri kullanıyor.  Bu durumda yalnızca ek bir korumalı VM ve HANA veritabanları yapılandırılması gerekir.  Makine geçişten sonra devre dışı bırakıldıysa, devam eden HLI yedekleme işleri artık zamanlanmamış olabilir.
VM'deki SAP HANA için Azure Yedekleme artık genel olarak kullanılabilir.  Hakkında ayrıntılı bilgi için bu bağlantılara bakın: [Yedekleme](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database), [Geri Yükleme](https://docs.microsoft.com/azure/backup/sap-hana-db-restore), Azure VM'lerde SAP HANA yedeklemeyi [yönet.](https://docs.microsoft.com/azure/backup/sap-hana-db-manage)

### <a name="dr-strategy"></a>DR stratejisi
Hizmet düzeyi hedefleriniz daha uzun bir kurtarma süresini barındırıyorsa, depolamayı blob etmek ve yerinde geri yüklemek veya yeni bir VM'ye geri yüklemek için basit bir yedekleme en basit ve en ucuz DR stratejisidir.  
HANA DR genellikle HSR ile yapılır büyük örnek platformu gibi; Azure VM'de HSR aynı zamanda en doğal ve yerel SAP HANA DR çözümüdür.  Kaynak dağıtımın tek örnekli veya kümelenmiş olup olmadığına bakılmaksızın, KAYNAK altyapısının bir kopyası DR bölgesinde gereklidir.
Bu DR yinelemesi, birincil HLI-VM geçişi tamamlandıktan sonra yapılandırılacaktır.  DR HANA DB ikincil çoğaltma sitesi olarak VM örneğinde birincil SAP HANA'ya kaydolur.  

### <a name="sap-application-server-connectivity-destination-change"></a>SAP uygulama sunucusu bağlantı hedef değişikliği
HSR geçiş yeni bir HANA DB ana bilgisayar ve dolayısıyla uygulama katmanı için yeni bir DB ana bilgisayar adı sonuçları, SAP profilleri yeni ana bilgisayar adını yansıtacak şekilde değiştirilmesi gerekir.  Anahtarlama, ana bilgisayar adını koruyarak ad kararıyla yapılırsa, profil değişikliği gerekmez.

### <a name="operating-system"></a>İşletim sistemi
HLI ve VM için işletim sistemi görüntüleri, aynı sürüm düzeyinde olmasına rağmen, Örneğin, SLES 12 SP4, aynı değildir. Müşteriler, hedefe aynı paketleri yüklemek için HLI'de gerekli paketleri, sıcak düzeltmeleri, düzeltmeleri, çekirdekleri ve güvenlik düzeltmelerini doğrulamalıdır.  Daha eski bir işletim sistemi nden vm'ye yeni bir işletim sistemi sürümüyle çoğaltmak için HSR'yi kullanmak desteklenir.  [SAP note 2763388'i](https://launchpad.support.sap.com/#/notes/2763388)inceleyerek desteklenen belirli sürümleri doğrulayın.

### <a name="new-sap-license-request"></a>Yeni SAP lisans isteği
VM'lere geçirildiğini gösteren yeni HANA sistemi için yeni bir SAP lisansı istemek için basit bir çağrı.

### <a name="service-level-agreement-sla-differences"></a>Hizmet düzeyi sözleşmesi (SLA) farkları
Yazarlar, HLI ve Azure VM arasındaki kullanılabilirlik SLA farkı dikkat istiyorum.  Örneğin, kümelenmiş HLIs HA çiftleri %99,99 kullanılabilirlik sunar. Aynı SLA'yı elde etmek için kullanılabilirlik bölgelerinde VM'ler dağıtılması gerekir. Bu [makalede,](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_9/) müşterilerin hedef altyapılarını buna göre planlayabilmeleri için ilişkili dağıtım mimarileriyle kullanılabilirliği açıklanmaktadır.


## <a name="migration-strategy"></a>Geçiş stratejisi
Bu belgede, HLI'dan Azure VM'ye geçiş için yalnızca HANA Sistem Çoğaltma yaklaşımını kapsamaktayız.  Dağıtılan hedef depolama çözümüne bağlıdır, işlem biraz farklıdır. Üst düzey adımlar aşağıda açıklanmıştır.

### <a name="vm-with-premiumultra-disks-for-data"></a>Veri için premium/ultra diskli VM
Premium veya ultra disklerle dağıtılan VM'ler için, Standart SAP HANA sistem çoğaltma yapılandırması HSR'yi ayarlamak için geçerlidir.  [SAP yardım makalesi,](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/099caa1959ce4b3fa1144562fa09e163.html) sistem çoğaltmayı ayarlama, ikincil bir sistemi devralma, birincil sisteme geri dönme ve sistem çoğaltmaişlemini devre dışı bırakma adımlarına genel bir bakış sağlar.  Geçiş amacıyla, yalnızca kuruluma, devralmaya ve çoğaltma adımlarını devre dışı bırakmaya ihtiyacımız olacaktır.  

### <a name="vm-with-anf-for-data-and-log-volumes"></a>Veri ve günlük hacimleri için ANF ile VM
Yüksek düzeyde, tam veri ve günlük hacimlerinin en son HLI depolama anlık görüntülerini, hedef HANA VM tarafından erişilebilir ve kurtarılabilir oldukları Azure Depolama'ya kopyalanması gerekir.  Kopyalama işlemi herhangi bir yerli Linux kopyalama araçları ile yapılabilir.  

> [!IMPORTANT]
> Kopyalama ve veri aktarımı hana veritabanı boyutuna ve ağ bant genişliğine bağlıdır.  Kopyalama işleminin büyük kısmı birincil HANA DB kesinti süresinden önce yapılmalıdır.

### <a name="mcos-to-mdc-conversion"></a>MCOS'dan MDC Dönüşümüne
Birden Çok Bileşen Bir Sistemde (MCOS) dağıtım modeli bazı HLI müşterilerimiz tarafından kullanılmıştır.  Motivasyon, önceki SAP HANA sürümlerinin Çoklu Veritabanları Kapsayıcısı (MDC) depolama anlık görüntüsünü atlatmaktı.  MCOS modelinde, birkaç bağımsız SAP HANA örneği tek bir HLI bıçakta üst üste dizilir.  Geçiş için HSR'nin kullanılması iyi çalışır, ancak her biri bir kiracı DB ile birden fazla HANA VM'si ile sonuçlanır.  Bu son durum, müşterinin alışık olabileceğinden daha yoğun bir manzara sağlar.  SAP HANA 2.0 varsayılan dağıtımı MDC olduğundan, Geçerli bir alternatif HSR geçişinden sonra [HANA kiracı hareketini](https://launchpad.support.sap.com/#/notes/2472478) gerçekleştirmektir.  Bu işlem, bu bağımsız HANA veritabanlarını tek bir HANA kapsayıcısında cotenants içine 'konsolide' eder.  

### <a name="application-layer-consideration"></a>Uygulama katmanı dikkate
DB sunucusu bir SAP sisteminin merkezi olarak görüntülenir.  Tüm uygulama sunucuları SAP HANA DB yakınında bulunmalıdır.  PpG'nin yeni kullanımının istendiği bazı durumlarda, mevcut uygulama sunucularının HANA VM'nin bulunduğu PPG'ye taşınması gerekebilir.  Dağıtım şablonlarınız zaten kullanışlıysa, yeni uygulama sunucuları oluşturmak daha kolay kabul edilebilir.  
Varolan uygulama sunucuları ve yeni HANA VM en iyi şekilde konumlanmışsa, ek kapasite gerekmedikçe yeni uygulama sunucuları oluşturulmamalıdır.  
Hizmet kullanılabilirliğini artırmak için yeni bir altyapı oluşturulursa, varolan uygulama sunucuları gereksiz hale gelebilir ve kapatıp silinmelidir.
Hedef VM ana bilgisayar adı değiştiyse ve HLI ana bilgisayar adından farklıysa, SAP uygulama sunucusu profillerinin yeni ana bilgisayara işaret edecek şekilde ayarlanması gerekir.  Yalnızca HANA DB IP adresi değişmişse, yeni HANA VM'ye gelen bağlantıları yönlendirmek için bir DNS kayıt güncelleştirmesi gerekir.

### <a name="acceptance-test"></a>Kabul testi
HLI'dan VM'ye geçiş, heterojen bir geçişle karşılaştırıldığında veritabanı içeriğinde önemli bir değişiklik yapmasa da, yeni kurulumun temel işlevlerini ve performans yönünü doğrulamanızı öneririz.  

### <a name="cutover-plan"></a>Kesme planı
Bu geçiş doğrudan olsa da, ancak varolan bir DB'nin devre dışı bırakılması içerir.  Kaynak sistemi ilişkili içeriği ve yedekleme görüntüleriyle korumak için dikkatli bir planlama yapılması, geri dönüş gerektiğinde kritik öneme sahip.  İyi planlama daha hızlı bir geri dönüş sunar.   


## <a name="post-migration"></a>Geçiş sonrası
Veri bütünlüğünün korunmasını sağlamak için HLI'ya bağımlı hizmetleri veya bağlantıyı güvenli bir şekilde ayırmadan geçiş işi yapılmaz.  Ayrıca, gereksiz hizmetleri kapatın.  Bu bölümde birkaç top-of-mind öğeleri çağırır.

### <a name="decommissioning-the-hli"></a>HLI'nın kapatılması
HANA DB'nin Azure VM'ye başarılı bir şekilde geçişinden sonra, HLI DB'de verimli iş işlemlerinin çalışmamasını sağlayın.  Ancak, HLI'nin yerel yedekleme saklama penceresine eşit bir süre boyunca çalışmasını sağlamak, gerekirse daha hızlı kurtarma sağlayan güvenli bir uygulamadır.  Ancak o zaman HLI bıçağı devre dışı kalmalıdır.  Müşteriler, Microsoft temsilcileriyle iletişime geçerek HLI taahhütlerini sözleşmeyle yerine getirmelidir.

### <a name="remove-any-proxy-ex-iptables-bigip-configured-for-hli"></a>HLI için yapılandırılan proxy'yi (ör. Iptable, BIGIP) kaldırın 
IPTable'lar gibi bir proxy hizmeti, şirket içi trafiği HLI'ye yönlendirmek için kullanılırsa, VM'ye başarılı geçişten sonra artık buna gerek kalmaz.  Ancak, bu bağlantı hizmeti HLI bıçağı hala beklemede olduğu sürece tutulmalıdır.  Yalnızca HLI bıçağı tamamen kullanımdan kaldırıldıktan sonra hizmeti kapatın.

### <a name="remove-global-reach-for-hli"></a>HLI için Global Reach'i kaldırın 
Global Reach, müşterilerin ExpressRoute ağ geçidini HLI ExpressRoute ağ geçidine bağlamak için kullanılır.  Müşterilerin şirket içi trafiğinin, proxy hizmeti kullanmadan doğrudan HLI kiracısına ulaşmasını sağlar.  Geçişten sonra HLI biriminin yokluğunda bu bağlantıya artık gerek yoktur.  IPTables proxy hizmeti durumunda olduğu gibi, GlobalReach de HLI bıçak tamamen devre dışı bırakılana kadar tutulmalıdır.

### <a name="operating-system-subscription--movereuse"></a>İşletim sistemi aboneliği – taşıma/yeniden kullanma
VM sunucuları bekletildikçe ve HLI bıçakları kullanımdan kaldırıldığından, işletim sistemi lisanslarının çift ödemesini önlemek için işletim sistemi abonelikleri değiştirilebilir veya yeniden kullanılabilir.



## <a name="next-steps"></a>Sonraki adımlar
Şu makalelere bakın:
- [Azure'da SAP HANA altyapı yapılandırmaları ve işlemleri.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [Azure'daki SAP iş yükleri: planlama ve dağıtım denetim listesi.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist)
