---
title: Azure'daki Oracle veritabanları için başvuru mimarileri | Microsoft Dokümanlar
description: Microsoft Azure Sanal Makineler'de Oracle Database Enterprise Edition veritabanlarını çalıştırmak için mimarilere başvurur.
services: virtual-machines-linux
author: BorisB2015
manager: gwallace
tags: ''
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2019
ms.author: borisb
ms.custom: ''
ms.openlocfilehash: bbb6665299ce9b6521eeb8801d8621dfbdc17f4a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683480"
---
# <a name="reference-architectures-for-oracle-database-enterprise-edition-on-azure"></a>Azure'da Oracle Database Enterprise Edition için başvuru mimarileri

Bu kılavuz, Azure'da yüksek oranda kullanılabilir bir Oracle veritabanı dağıtma yla ilgili ayrıntıları niçin ayrıntılarına dayalıdır. Buna ek olarak, bu kılavuz felaket kurtarma hususlar içine dalış. Bu mimariler müşteri dağıtımlarına dayalı olarak oluşturulmuştur. Bu kılavuz yalnızca Oracle Database Enterprise Edition için geçerlidir.

Oracle veritabanınızın performansını en üst düzeye çıkarma hakkında daha fazla bilgi edinmek istiyorsanız, [Bkz.](oracle-design.md)

## <a name="assumptions"></a>Varsayımlar

- [Kullanılabilirlik bölgeleri](../../../availability-zones/az-overview.md) gibi Azure'un farklı kavramlarını anlayabilirsiniz
- Oracle Database Enterprise Edition 12c veya sonraki sürümleri çalıştırıyorsanız
- Bu makaledeki çözümleri kullanırken lisanslama nın sonuçlarını fark eder ve kabul emzebilirsiniz

## <a name="high-availability-for-oracle-databases"></a>Oracle veritabanları için yüksek kullanılabilirlik

Bulutta yüksek kullanılabilirlik elde etmek, her kuruluşun planlama ve tasarımının önemli bir parçasıdır. Microsoft Azure [kullanılabilirlik bölgeleri](../../../availability-zones/az-overview.md) ve kullanılabilirlik kümeleri sunar (kullanılabilirlik bölgelerinin kullanılamadığı bölgelerde kullanılabilir). Bulut için tasarım yapmak üzere [sanal makinelerinizin kullanılabilirliğini yönetme](../../../virtual-machines/linux/manage-availability.md) hakkında daha fazla bilgi edinin.

Oracle, buluta özgü araçlara ve tekliflere ek olarak, [Oracle Data Guard,](https://docs.oracle.com/en/database/oracle/oracle-database/18/sbydb/introduction-to-oracle-data-guard-concepts.html#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7) [FSFO ile Data Guard](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/index.html), [Sharding](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/admin/sharding-overview.html)ve Azure'da kurulabilen [GoldenGate](https://www.oracle.com/middleware/technologies/goldengate.html) gibi yüksek kullanılabilirlik için çözümler sunar. Bu kılavuz, bu çözümlerin her biri için başvuru mimarilerini kapsar.

Son olarak, bulut için uygulama geçirirken veya oluştururken, [yeniden deneme deseni](https://docs.microsoft.com/azure/architecture/patterns/retry) ve devre kesici [deseni](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker)gibi bulut açi kalıpları eklemek için uygulama kodunuzu değiştirmeniz önemlidir. [Bulut Tasarım Desenleri kılavuzunda](https://docs.microsoft.com/azure/architecture/patterns/) tanımlanan ek desenler, uygulamanızın daha esnek olmasını sağlayabilir.

### <a name="oracle-rac-in-the-cloud"></a>Bulutta Oracle RAC

Oracle Real Application Cluster (RAC), müşterilerin tek bir veritabanı depolamasına (Paylaşılan her şey mimarisi deseni) erişen birçok örneğine sahip olarak müşterilerin yüksek iş hacmine ulaşmasına yardımcı olan bir çözümdür. Oracle RAC şirket içinde yüksek kullanılabilirlik için de kullanılabilse de, Oracle RAC tek başına bulutta yüksek kullanılabilirlik için kullanılamaz, çünkü yalnızca raf düzeyindeveya Veri merkezi düzeyindeki hatalara karşı değil, yalnızca örnek düzey hatalarına karşı korur. Bu nedenle Oracle, yüksek kullanılabilirlik için veritabanınızla (tek örnek veya RAC) Oracle Data Guard kullanmanızı önerir. Müşteriler genellikle görev kritik uygulamaları çalıştırmak için yüksek bir SLA gerektirir. Oracle RAC şu anda Azure'da Oracle tarafından onaylanmadı veya desteklenmedi. Ancak Azure, örnek düzeyindeki hatalara karşı korunmaya yardımcı olmak için Kullanılabilirlik Bölgeleri ve planlı bakım pencereleri sunar. Buna ek olarak, müşteriler veri tabanlarını raf düzeyinde ve veri merkezi düzeyinde ve jeo-politik hatalardan koruyarak yüksek performans ve resilik için Oracle Data Guard, Oracle GoldenGate ve Oracle Sharding gibi teknolojileri kullanabilirler.

Oracle Data Guard veya GoldenGate ile birlikte birden çok [kullanılabilirlik bölgesinde](https://docs.microsoft.com/azure/availability-zones/az-overview) Oracle Veritabanlarıçalıştırırken, müşteriler %99,99'luk bir çalışma süresi SLA'sı alabilir. Kullanılabilirlik bölgelerinin henüz mevcut olmadığı Azure bölgelerinde, müşteriler [Kullanılabilirlik Kümeleri'ni](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) kullanabilir ve %99,95'lik çalışma süresi SLA'sı elde edebilir.

>NOT: Microsoft tarafından sağlanan çalışma süresi SLA'dan çok daha yüksek bir çalışma süresi hedefiniz olabilir.

## <a name="disaster-recovery-for-oracle-databases"></a>Oracle veritabanları için olağanüstü durum kurtarma

Görev açısından kritik uygulamalarınızı bulutta barındırırken, yüksek kullanılabilirlik ve olağanüstü durum kurtarma için tasarım yapmak önemlidir.

Oracle Database Enterprise Edition için Oracle Data Guard, olağanüstü durum kurtarma için kullanışlı bir özelliktir. Eşleştirilmiş bir [Azure bölgesinde](/azure/best-practices-availability-paired-regions) bekleme veritabanı örneği ayarlayabilir ve olağanüstü durum kurtarma için Veri Koruması başarısızlığı ayarlayabilirsiniz. Sıfır veri kaybı için, Active Data Guard'a ek olarak bir Oracle Data Guard Far Sync örneği dağıtmanız önerilir. 

Uygulamanız gecikmesüresine izin verirse (kapsamlı test gereklidir) Oracle birincil veritabanınızdan farklı bir kullanılabilirlik bölgesinde Veri Koruma Far Eşitleme örneğini ayarlamayı düşünün. Redo dosyalarınızın Far Sync örneğine eşzamanlı aktarımını ayarlamak için **Maksimum Kullanılabilirlik** modunu kullanın. Bu dosyalar daha sonra eş senkronize bir şekilde bekleme veritabanına aktarılır. 

Uygulamanız, Far Sync örneğini **Maksimum Kullanılabilirlik** modunda (senkron) başka bir kullanılabilirlik bölgesinde ayarlarken performans kaybına izin vermiyorsa, birincil veritabanınızla aynı kullanılabilirlik bölgesinde bir Far Eşitleme örneği ayarlayabilirsiniz. Daha fazla kullanılabilirlik için, birincil veritabanınıza yakın birden çok Far Sync örneği ve bekleme veritabanınıza en az bir örnek ayarlamayı düşünün (rol geçiş yaparsa). Bu Oracle Active Data Guard [Far Sync teknik](https://www.oracle.com/technetwork/database/availability/farsync-2267608.pdf)incelemesinde Oracle Data Guard Far Sync hakkında daha fazla bilgi edinin.

Oracle Standard Edition veritabanlarını kullanırken, yüksek kullanılabilirlik ve olağanüstü durum kurtarma ayarlamanızı sağlayan DBVisit Standby gibi ISV çözümleri vardır.

## <a name="reference-architectures"></a>Başvuru mimarileri

### <a name="oracle-data-guard"></a>Oracle Data Guard

Oracle Data Guard, kurumsal veriler için yüksek kullanılabilirlik, veri koruması ve olağanüstü durum kurtarma sağlar. Data Guard, bekleme veritabanlarını birincil veritabanının işlem açısından tutarlı kopyaları olarak tutar. Birincil ve ikincil veritabanları arasındaki uzaklığa ve gecikme için uygulama toleransına bağlı olarak, senkron veya eşzamanlı çoğaltma ayarlayabilirsiniz. Ardından, birincil veritabanı planlanmış veya planlanmamış bir kesinti nedeniyle kullanılamıyorsa, Veri Koruması herhangi bir bekleme veritabanını birincil role geçiş yapabilir ve kapalı kalma süresini en aza indirebilir. 

Oracle Data Guard'ı kullanırken, ikincil veritabanınızı salt okunur amaçlarla da açabilirsiniz. Bu yapılandırmaya Etkin Veri Koruması adı verilir. Oracle Database 12c, Data Guard Far Sync Instance adlı bir özelliği tanıttı. Bu örnek, performanstan ödün vermek zorunda kalmadan Oracle veritabanınızın sıfır veri kaybı yapılandırmasını ayarlamanızı sağlar.

> [!NOTE]
> Etkin Veri Koruması ek lisans gerektirir. Bu lisansın Far Sync özelliğini kullanması da gereklidir. Lisanslama nın sonuçlarını tartışmak için lütfen Oracle temsilcinizle bağlantı kurun.

#### <a name="oracle-data-guard-with-fsfo"></a>FSFO ile Oracle Data Guard
Hızlı Başlatma Failover 'ı (FSFO) ile Oracle Data Guard, aracıyı ayrı bir makinede kurarak ek esneklik sağlayabilir. Data Guard aracısı ve ikincil veritabanı hem gözlemciyi çalıştırAr hem de kapalı kalma süresi için birincil veritabanını gözlemler. Bu, Veri Koruma gözlemci kurulumunuzda da fazlalık sağlar. 

Oracle Database sürüm 12.2 ve üzeri ile, birden fazla gözlemciyi tek bir Oracle Data Guard broker yapılandırması ile yapılandırmak da mümkündür. Bu kurulum, bir gözlemci ve ikincil veritabanı deneyimi kapalı kalma süresi durumunda ek kullanılabilirlik sağlar. Data Guard Broker hafif ve nispeten küçük bir sanal makine üzerinde barındırılabilir. Data Guard Broker ve avantajları hakkında daha fazla bilgi edinmek için bu konuyla ilgili [Oracle belgelerini](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/oracle-data-guard-broker-concepts.html) ziyaret edin.

Aşağıdaki diyagram, kullanılabilirlik bölgeleriyle Azure'da Oracle Data Guard'ı kullanmak için önerilen bir mimaridir. Bu mimari% 99,99 bir VM çalışma süresi SLA almanızı sağlar.

![Data Guard Broker ile kullanılabilirlik bölgelerini kullanan Oracle Veritabanı - FSFO](./media/oracle-reference-architecture/oracledb_dg_fsfo_az.png)

Önceki diyagramda, istemci sistemi web üzerinden Oracle arka uç ile özel bir uygulama erişer. Web frontend bir yük dengeleyici sonlanır. Web frontend işi işlemek için uygun uygulama sunucusuna bir çağrı yapar. Uygulama sunucusu birincil Oracle veritabanını sorgular. Oracle veritabanı, lisans maliyetlerinden tasarruf etmek ve performansı en üst düzeye çıkarmak için [kısıtlı çekirdek vCPUs'lara](../../../virtual-machines/windows/constrained-vcpu.md) sahip hiper iş parçacığı [bellek optimize edilmiş sanal makine](../../../virtual-machines/windows/sizes-memory.md) kullanılarak yapılandırılmıştır. Performans ve yüksek kullanılabilirlik için birden çok premium veya ultra disk (Yönetilen Diskler) kullanılır.

Oracle veritabanları, yüksek kullanılabilirlik için birden çok kullanılabilirlik bölgesine yerleştirilir. Her bölge, bağımsız güç, soğutma ve ağ ile donatılmış bir veya daha fazla veri merkezinden oluşur. Esnekliği sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge kurulur. Bir bölgedeki kullanılabilirlik bölgelerinin fiziksel olarak ayrılması verileri veri merkezi hatalarından korur. Ayrıca, iki FSFO gözlemcisi, bir kesinti gerçekleştiğinde veritabanı üzerinde ikincil olarak başlatılması ve başarısız olması için iki kullanılabilirlik bölgesi ne zaman ayarlanır. 

Önceki mimaride gösterilen bölgeden farklı bir kullanılabilirlik bölgesinde (BU durumda AZ 1) ek gözlemciler ve/veya bekleme veritabanları ayarlayabilirsiniz. Son olarak, Oracle veritabanları çalışma süresi ve performans için Oracle Enterprise Manager (OEM) tarafından izlenir. OEM ayrıca çeşitli performans ve kullanım raporları oluşturmanıza olanak tanır.

Kullanılabilirlik bölgelerinin desteklenmediği bölgelerde, Oracle Veritabanınızı son derece kullanılabilir bir şekilde dağıtmak için kullanılabilirlik kümelerini kullanabilirsiniz. Kullanılabilirlik kümeleri% 99,95 vm çalışma süresi elde etmek için izin verir. Aşağıdaki diyagram, bu kullanımın bir başvuru mimarisidir:

![Data Guard Broker ile kullanılabilirlik kümelerini kullanan Oracle Veritabanı - FSFO](./media/oracle-reference-architecture/oracledb_dg_fsfo_as.png)

> [!NOTE]
> * Oracle Enterprise Manager VM'nin dağıtılan OEM'in yalnızca bir örneği olduğundan, kullanılabilirlik kümesine yerleştirilmesi gerekmez.
> * Ultra diskler şu anda kullanılabilirlik kümesi yapılandırmasında desteklenmez.

#### <a name="oracle-data-guard-far-sync"></a>Oracle Data Guard Far Sync

Oracle Data Guard Far Sync, Oracle Veritabanları için sıfır veri kaybı koruma özelliği sağlar. Bu özellik, veritabanı makineniz başarısız olursa veri kaybına karşı koruma sağlar. Oracle Data Guard Far Sync'in ayrı bir VM'ye yüklenmesi gerekir. Far Sync, yalnızca bir denetim dosyası, parola dosyası, spfile ve bekleme günlükleri olan hafif bir Oracle örneğidir. Veri dosyaları veya rego günlük dosyaları yok. 

Sıfır veri kaybı koruması için birincil veritabanınız ile Far Eşitleme örneği arasında eşzamanlı iletişim olmalıdır. Far Sync örneği birincil den eşzamanlı bir şekilde redo alır ve hemen tüm bekleme veritabanlarına asynchronous bir şekilde iletir. Bu kurulum, tüm bekleme veritabanları yerine yalnızca redo'yu Far Sync örneğine göndermesi olduğundan, birincil veritabanındaki yükü de azaltır. Far Eşitleme örneği başarısız olursa, Veri Koruması sıfıra yakın veri kaybı koruması sağlamak için birincil veritabanından ikincil veritabanına otomatik olarak asenkron aktarım kullanır. Daha fazla esneklik için, müşteriler her veritabanı örneği (birincil ve ikinci basamak) başına birden çok Far Eşitleme örneği dağıtabilir.

Aşağıdaki diyagram, Oracle Data Guard Far Sync kullanarak yüksek kullanılabilirlik mimarisidir:

![Data Guard Far Sync & Broker ile kullanılabilirlik bölgelerini kullanan Oracle veritabanı - FSFO](./media/oracle-reference-architecture/oracledb_dg_fs_az.png)

Önceki mimaride, ikisi arasındaki gecikme süresini azaltmak için veritabanı örneğiyle aynı kullanılabilirlik bölgesinde dağıtılan bir Far Eşitleme örneği vardır. Uygulamanın gecikme süresine duyarlı olduğu durumlarda, veritabanınızı ve Uzak Eşitleme örneğini veya örneklerini bir [yakınlık yerleşim grubunda](../../../virtual-machines/linux/proximity-placement-groups.md)dağıtmayı düşünün.

Aşağıdaki diyagram, yüksek kullanılabilirlik ve olağanüstü durum kurtarma elde etmek için Oracle Data Guard FSFO ve Far Sync kullanan bir mimaridir:

![Oracle Veritabanı Data Guard Far Sync & Broker ile olağanüstü durum kurtarma için kullanılabilirlik bölgeleri kullanarak - FSFO](./media/oracle-reference-architecture/oracledb_dg_fs_az_dr.png)

### <a name="oracle-goldengate"></a>Oracle GoldenGate

GoldenGate, kuruluş genelinde ki birden fazla heterojen platform arasında işlem düzeyinde veri alışverişini ve manipülasyonunu sağlar. İşlem bütünlüğü ve varolan altyapınızda minimum genel merkeze sahip taahhüt edilen hareketleri taşır. Modüler mimarisi, seçili veri kayıtlarını, işlem değişikliklerini ve ddl (veri tanım dili dili) değişikliklerini çeşitli topolojilerde ayıklama ve çoğaltma esnekliği sağlar.

Oracle GoldenGate, çift yönlü çoğaltma sağlayarak veritabanınızı yüksek kullanılabilirlik için yapılandırmanıza olanak tanır. Bu, **çok büyük** veya etkin bir **yapılandırma**ayarlamanızı sağlar. Aşağıdaki diyagram, Azure'da Oracle GoldenGate etkin kurulumu için önerilen bir mimaridir. Aşağıdaki mimaride, Oracle veritabanı lisans maliyetlerinden tasarruf etmek ve performansı en üst düzeye çıkarmak için [kısıtlı çekirdek vCPUs'lara](../../../virtual-machines/windows/constrained-vcpu.md) sahip hiperiş parçacıklı [bellek optimize edilmiş sanal makine](../../../virtual-machines/windows/sizes-memory.md) kullanılarak yapılandırılmıştır. Performans ve kullanılabilirlik için birden çok premium veya ultra disk (yönetilen diskler) kullanılır.

![Data Guard Broker ile kullanılabilirlik bölgelerini kullanan Oracle Veritabanı - FSFO](./media/oracle-reference-architecture/oracledb_gg_az.png)

> [!NOTE]
> Benzer bir mimari, kullanılabilirlik bölgelerinin şu anda kullanılamadığı bölgelerdeki kullanılabilirlik kümeleri kullanılarak ayarlanabilir.

Oracle GoldenGate,Extract, Pump ve Replicat gibi verilerinizi bir Oracle veritabanı sunucusundan diğerine eşzamanlı olarak kopyalamanıza yardımcı olan işlemlere sahiptir. Bu işlemler, kullanılabilirlik bölgesi düzeyinde kapalı kalma süresi varsa veritabanınızın yüksek kullanılabilirliğini sağlamak için çift yönlü bir çoğaltma ayarlamanızı sağlar. Önceki diyagramda, Ayıklama işlemi Oracle veritabanınızla aynı sunucuda çalışırken, Veri Pompası ve Çoğaltma işlemleri aynı kullanılabilirlik bölgesinde ayrı bir sunucuda çalışır. Replicat işlemi, diğer kullanılabilirlik bölgesindeki veritabanından veri almak ve verileri kullanılabilirlik bölgesindeki Oracle veritabanına işlemek için kullanılır. Benzer şekilde, Veri Pompası işlemi ayıklama işlemi tarafından çıkarılan verileri diğer kullanılabilirlik bölgesindeki Çoğaltma işlemine gönderir. 

Önceki mimari diyagram, ayrı bir sunucuda yapılandırılan Veri Pompası ve Çoğaltma işlemini gösterirken, sunucunuzun kapasitesine ve kullanımına bağlı olarak tüm Oracle GoldenGate işlemlerini aynı sunucuda ayarlayabilirsiniz. Sunucunuzun kullanım modelini anlamak için her zaman AWR raporunuza ve Azure'daki ölçümlere başvurun.

Oracle GoldenGate çift yönlü çoğaltmayı farklı kullanılabilirlik bölgelerinde veya farklı bölgelerde ayarlarken, farklı bileşenler arasındaki gecikmenin uygulamanız için kabul edilebilir olduğundan emin olmak önemlidir. Kullanılabilirlik bölgeleri ve bölgeler arasındaki gecikme değişebilir ve birden çok etkene bağlı olabilir. Uygulama performans gereksinimlerinizi karşıladığını doğrulamak için uygulama katmanınızla veritabanı katmanınız arasında farklı kullanılabilirlik bölgeleri ve/veya bölgelerde performans testleri ayarlamanız önerilir.

Uygulama katmanı kendi alt ağına ayarlanabilir ve veritabanı katmanı kendi alt ağına ayrılabilir. Mümkün olduğunda, uygulama sunucularınız arasındaki trafiği dengelemek için [Azure Uygulama Ağ Geçidi'ni](../../../application-gateway/overview.md) kullanmayı düşünün. Azure Application Gateway sağlam bir web trafiği yük dengeleyicisidir. Kullanıcı oturumunu aynı sunucuda tutan çerez tabanlı oturum afiyeti sağlar ve böylece veritabanındaki çakışmaları en aza indirir. Uygulama Ağ Geçidi'ne alternatifler [Azure Yük Dengeleyici](../../../load-balancer/load-balancer-overview.md) ve [Azure Trafik Yöneticisi'dir.](../../../traffic-manager/traffic-manager-overview.md)

### <a name="oracle-sharding"></a>Oracle Sharding

Sharding, Oracle 12.2'de tanıtılan bir veri katmanı desenidir. Verilerinizi bağımsız veritabanları arasında yatay olarak bölmenize ve ölçeklendirmenize olanak tanır. Her veritabanının, esneklik ve artırılmış kullanılabilirlik ek olarak yüksek okuma ve yazma iş başına olanak tanıyan özel bir sanal makinede barındırıldığı bir paylaşım-yok mimarisidir. Bu desen, tek hata noktalarını ortadan kaldırır, hata yalıtımı sağlar ve kapalı kalma süresi olmadan yükseltmeleri yuvarlama sağlar. Bir parçanın veya veri merkezi düzeyindeki bir hatanın kapalı kalma süresi, diğer veri merkezlerindeki diğer parçaların performansını veya kullanılabilirliğini etkilemez. 

Sharding, herhangi bir kesinti yiyemez yüksek iş gücü OLTP uygulamaları için uygundur. Aynı parçalama tuşuna sahip tüm satırların her zaman aynı parçaüzerinde olması garanti edilir, böylece yüksek tutarlılık sağlayan performans artar. Parçalama kullanan uygulamaların, öncelikle bir parçalama anahtarı (örneğin *customerId* veya *accountNum)* kullanarak verilere erişen iyi tanımlanmış bir veri modeli ve veri dağıtım stratejisi (tutarlı karma, aralık, liste veya bileşik) olmalıdır. Parçalama ayrıca belirli veri kümelerini son müşterilere daha yakın depolamanıza olanak sağlayarak performans ve uyumluluk gereksinimlerinizi karşılamanıza yardımcı olur.

Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için parçaları çoğaltmak önerilir. Bu kurulum Oracle Data Guard veya Oracle GoldenGate gibi Oracle teknolojileri kullanılarak yapılabilir. Çoğaltma birimi bir parça, bir parçanın parçası veya bir grup parça olabilir. Parçalanmış bir veritabanının kullanılabilirliği, bir veya daha fazla parçanın kesintisi veya yavaşlaması nedeniyle etkilenmez. Yüksek kullanılabilirlik için bekleme parçaları birincil parçaların yerleştirildiği aynı kullanılabilirlik bölgesine yerleştirilebilir. Olağanüstü durum kurtarma için bekleme parçaları başka bir bölgede bulunabilir. Ayrıca, bu bölgelerdeki trafiğe hizmet etmek için birden çok bölgeye parça dağıtabilirsiniz. [Oracle Sharding belgelerinde](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-high-availability.html)yüksek kullanılabilirlik ve sıkıştırılmış veritabanıçoğaltma yapılandırma hakkında daha fazla bilgi edinin.

Oracle Sharding öncelikle aşağıdaki bileşenlerden oluşur. Bu bileşenler hakkında daha fazla bilgi [Oracle Sharding belgelerinde](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-overview.html)bulunabilir:

- **Shard kataloğu** - Tüm Shard veritabanı yapılandırma verileri için kalıcı bir mağaza olan özel amaçlı Oracle veritabanı. Parça ekleme veya kaldırma, verilerin eşleneme ve parça veritabanındaki DDL'ler gibi tüm yapılandırma değişiklikleri parça kataloğunda başlatılır. Parça kataloğu, bir SDB'deki tüm yinelenen tabloların ana kopyasını da içerir. 

  Parça kataloğu, tüm kırıklarda yinelenen tablolardaki değişiklikleri otomatik olarak çoğaltmak için somutlaştırılmış görünümler kullanır. Shard catalog veritabanı, çok parçalı sorguları ve parçalama anahtarı belirtmeyin sorguları işlemek için kullanılan bir sorgu koordinatörü olarak da davranır. 
  
  Sabit katalog yüksek kullanılabilirlik için kullanılabilirlik bölgeleri veya kullanılabilirlik kümeleri ile birlikte Oracle Data Guard kullanarak önerilen en iyi uygulamadır. Parça kataloğunun kullanılabilirliği, parçalanan veritabanının kullanılabilirliği üzerinde hiçbir etkiye sahip değildir. Parça kataloğundaki bir kapalı kalma süresi, Yalnızca Veri Koruması'nın başarısız olduğu kısa süre boyunca bakım işlemlerini ve çok çeşitli sorguları etkiler. Çevrimiçi işlemler SDB tarafından yönlendirilmeye ve yürütülmeye devam eder ve katalog kesintisinden etkilenmez.

- **Shard yöneticileri** - Parçalarınızın bulunduğu her bölgede/kullanılabilirlik bölgesinde dağıtılması gereken hafif hizmetler. Shard Directors, Oracle Sharding bağlamında dağıtılan Global Service Managers'dır. Yüksek kullanılabilirlik için, parçalarınızın bulunduğu her kullanılabilirlik bölgesinde en az bir parça yönetmeni dağıtmanız önerilir. 

  Veritabanına ilk olarak bağlanırken, yönlendirme bilgileri bir parça yöneticisi tarafından ayarlanır ve sonraki istekler için önbelleğe alınmış, parça yönetmeni atlanır. Oturum bir parça ile kurulduktan sonra, tüm SQL sorguları ve DML'ler desteklenen ve verilen parça kapsamında yürütülür. Bu yönlendirme hızlıdır ve parça içi işlemleri gerçekleştiren tüm OLTP iş yükleri için kullanılır. En yüksek performans ve kullanılabilirlik gerektiren tüm OLTP iş yükleri için doğrudan yönlendirme kullanılması önerilir. Bir parça kullanılamadığında veya parçalama topolojisinde değişiklikler oluştuğunda yönlendirme önbelleği otomatik olarak yenilenir. 
  
  Yüksek performanslı, veriye bağımlı yönlendirme için Oracle, parçalanmış veritabanındaki verilere erişirken bir bağlantı havuzu kullanmanızı önerir. Oracle bağlantı havuzları, dile özel kitaplıklar ve sürücüler Oracle Sharding'i destekler. Daha fazla ayrıntı için [Oracle Sharding belgelerine](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-overview.html#GUID-3D41F762-BE04-486D-8018-C7A210D809F9) bakın.

- **Global hizmet** - Global hizmet, normal veritabanı hizmetine benzer. Bir veritabanı hizmetinin tüm özelliklerine ek olarak, genel bir hizmet istemciler ve parça ve çoğaltma gecikme toleransı arasındaki bölge yakınlığı gibi kırık veritabanları için özelliklere sahiptir. Parçalanmış bir veritabanına/veri tabanından veri okumak/yazmak için yalnızca bir Global hizmetioluşturulması gerekir. Etkin Veri Koruması'nı kullanırken ve parçaların salt okunur yinelemelerini ayarlarken, salt okunur iş yükleri için başka bir gGobal hizmeti oluşturabilirsiniz. İstemci veritabanına bağlanmak için bu Global hizmetleri kullanabilirsiniz.

- **Shard veritabanları** - Shard veritabanları Oracle veritabanlarınızdır. Her veritabanı, Fast-Start Failover (FSFO) etkinleştirilmiş bir Broker yapılandırmasında Oracle Data Guard kullanılarak çoğaltılır. Her parçaüzerinde Veri Koruma failover ve çoğaltma kurmak gerekmez. Paylaşılan veritabanı oluşturulduğunda bu otomatik olarak yapılandırılır ve dağıtılır. Belirli bir parça başarısız olursa, Oracle Sharing birincilden beklemeye veritabanı bağlantıları üzerinde otomatik olarak başarısız olur.

Oracle'ın parçalanmış veritabanlarını iki arabirimle dağıtabilir ve yönetebilirsiniz: Oracle `GDSCTL` Enterprise Manager Cloud Control GUI ve/veya komut satırı yardımcı programı. Bulut denetimini kullanarak kullanılabilirlik ve performans için farklı parçaları bile izleyebilirsiniz. Komut, `GDSCTL DEPLOY` parçaları ve ilgili dinleyicileri otomatik olarak oluşturur. Ayrıca, bu komut, yönetici tarafından belirtilen sabit düzey yüksek kullanılabilirlik için kullanılan çoğaltma yapılandırmasını otomatik olarak dağır.

Veritabanını parçalamanın farklı yolları vardır:

* Sistem tarafından yönetilen parçalama - Bölümleme kullanarak parçaları otomatik olarak dağıtır
* Kullanıcı tanımlı parçalama - Düzenleyici veya veri yerelleştirme gereksinimleri olduğunda iyi çalışan parçaların verilerin eşlemesi belirtmenizi sağlar)
* Kompozit parçalama - Farklı _kırıklar_ için sistem tarafından yönetilen ve kullanıcı tarafından tanımlanan parçalamanın birleşimi
* Tablo alt bölümleri - Normal bölümlenmiş tabloya benzer.

Oracle'ın belgelerinde farklı [parçalama yöntemleri](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-methods.html) hakkında daha fazla bilgi edinin.

Parçalanmış bir veritabanı uygulamalar ve geliştiriciler için tek bir veritabanı gibi görünse de, parçalanmış olmayan bir veritabanından kırık bir veritabanına geçiş yaparken, hangi tabloların çoğaltılıp parçalanacağını belirlemek için dikkatli planlama gerekir. 

Yinelenen tablolar tüm kırıklarda depolanırken, kırık tablolar farklı kırıklar arasında dağıtılır. Öneri, küçük ve boyutlu tabloları çoğaltmak ve olgu tablolarını dağıtmak/parçalamaktır. Veriler, merkezi koordinatör olarak parça kataloğu nu kullanarak veya her parçada Veri Pompası çalıştırılarak parçalanmış veritabanınıza yüklenebilir. Oracle'ın belgelerinde [verileri parçalanmış bir veritabanına geçirme](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-loading-data.html) hakkında daha fazla bilgi edinin.

#### <a name="oracle-sharding-with-data-guard"></a>Veri Koruma ile Oracle Sharding

Oracle Data Guard, sistem tarafından yönetilen, kullanıcı tanımlı ve bileşik parçalama yöntemleriyle parçalama için kullanılabilir.

Aşağıdaki diyagram, Oracle Sharding ve Oracle Data Guard için her parçanın yüksek kullanılabilirliği için kullanılan bir başvuru mimarisidir. Mimari diyagramı _bileşik bir parçalama yöntemini_gösterir. Mimari diyagramı büyük olasılıkla veri yerelliği, yük dengeleme, yüksek kullanılabilirlik, olağanüstü durum kurtarma, vb için farklı gereksinimleri olan uygulamalar için farklı olacaktır ve parçalama için farklı bir yöntem kullanabilirsiniz. Oracle Sharding, bu gereksinimleri karşılamanızı ve bu seçenekleri sunarak yatay ve verimli bir şekilde ölçeklendirmenizi sağlar. Benzer bir mimari bile Oracle GoldenGate kullanılarak dağıtılabilir.

![Data Guard Broker ile kullanılabilirlik bölgelerini kullanarak Oracle Database Sharding - FSFO](./media/oracle-reference-architecture/oracledb_dg_sh_az.png)

Sistem tarafından yönetilen parçalama yapılandırmak ve yönetmek en kolay olsa da, kullanıcı tanımlı parçalama veya bileşik parçalama, verilerinizin ve uygulamanızın coğrafi olarak dağıtıldığı senaryolar veya her parçanın çoğaltılması üzerinde denetime sahip olmanız gereken senaryolar için uygundur. 

Önceki mimaride, bileşik parçalama verileri coğrafi olarak dağıtmak ve uygulama katmanlarınızı yatay olarak ölçeklendirmek için kullanılır. Kompozit parçalama, sistem tarafından yönetilen ve kullanıcı tarafından tanımlanan parçalamanın bir kombinasyonudur ve böylece her iki yöntemin de yararını sağlar. Önceki senaryoda, veriler ilk olarak bölgeye göre ayrılmış birden çok parça alanı arasında bölünür. Daha sonra, veriler parça alanındaki birden çok parça arasında tutarlı karma ile daha da bölümlere ayrılmıştır. Her parça alanı birden çok parça grubu içerir. Her parça grubunun birden çok parçası vardır ve bu durumda çoğaltmanın bir "birimi"dir. Her parça grubu, parça alanındaki tüm verileri içerir. A1 ve B1 grupları birincil parça grupları, A2 ve B2 grupları ise bekleme gruplarıdır. Tek tek parçaların bir parça grubu yerine çoğaltma birimi olmasını seçebilirsiniz.

Önceki mimaride, yüksek kullanılabilirlik için her kullanılabilirlik bölgesinde bir GSM/shard yöneticisi dağıtılır. Öneri, veri merkezi/bölge başına en az bir GSM/parça yöneticisi dağıtmaktır. Ayrıca, uygulama sunucusunun bir örneği, bir parça grubu içeren her kullanılabilirlik bölgesinde dağıtılır. Bu kurulum, uygulama sunucusu ile veritabanı/parça grubu arasındaki gecikme yitirme süresinin düşük tutulmasını sağlar. Bir veritabanı başarısız olursa, bekleme veritabanıyla aynı bölgede bulunan uygulama sunucusu, veritabanı rolü geçişi gerçekleştiğinde istekleri işleyebilir. Azure Application Gateway ve parça yöneticisi, istek ve yanıt gecikmesini ve rota isteklerini buna göre izler.

Uygulama açısından, istemci sistemi, isteği istemciye en yakın bölgeye yönlendiren Azure Uygulama Ağ Geçidi'ne (veya Azure'daki diğer yük dengeleme teknolojileri) bir istekte bulunuyor. Azure Application Gateway, aynı istemciden gelen tüm isteklerin aynı uygulama sunucusuna yönlendirilen yapışkan oturumları da destekler. Uygulama sunucusu, veri erişim sürücülerinde bağlantı havuzu kullanır. Bu özellik JDBC, ODP.NET, OCI, vb. gibi sürücülerde kullanılabilir. Sürücüler isteğin bir parçası olarak belirtilen parçalama tuşlarını tanıyabilir. JDBC istemcileri için [Oracle Evrensel Bağlantı Havuzu (UCP),](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/jjucp/ucp-database-sharding-overview.html) Apache Tomcat ve IIS gibi Oracle dışı uygulama istemcilerinin Oracle Sharding ile çalışmasını sağlayabilir. 

İlk istek sırasında, uygulama sunucusu, isteğin yönlendirilmesi gereken parça için yönlendirme bilgileri almak için kendi bölgesindeki parça yöneticisine bağlanır. Geçirilen parçalama anahtarına bağlı olarak, yönetmen uygulama sunucusunu ilgili parçaya yönlendirir. Uygulama sunucusu bu bilgileri bir harita oluşturarak önbelleğe alınır ve sonraki istekler için, parça yönetmenini atlar ve istekleri doğrudan parçaya yönlendirir.

#### <a name="oracle-sharding-with-goldengate"></a>GoldenGate ile Oracle Sharding

Aşağıdaki diyagram, Oracle GoldenGate ile Oracle Sharding için her parçanın bölge içi yüksek kullanılabilirliği için bir başvuru mimarisidir. Önceki mimarinin aksine, bu mimari yalnızca tek bir Azure bölgesinde (birden çok kullanılabilirlik bölgesi) yüksek kullanılabilirliği betimlemektedir. Oracle GoldenGate'i kullanarak çok bölgeli yüksek kullanılabilirlik eleştirilmiş bir veritabanı (önceki örneğe benzer) dağıtılabilir.

![GoldenGate ile kullanılabilirlik bölgelerini kullanarak Oracle Database Sharding](./media/oracle-reference-architecture/oracledb_gg_sh_az.png)

Önceki başvuru mimarisi, verileri parçalamak için _sistem tarafından yönetilen_ parçalama yöntemini kullanır. Oracle GoldenGate çoğaltma bir yığın düzeyinde yapıldığından, bir parçaya çoğaltılan verilerin yarısı başka bir parçaya çoğaltılabilir. Diğer yarısı farklı bir parçaya çoğaltılabilir. 

Verilerin çoğaltılış şekli çoğaltma faktörüne bağlıdır. 2'lik bir çoğaltma faktörüyle, parça grubundaki üç parçanızda her veri parçasının iki kopyasına sahip olursunuz. Benzer şekilde, parça grubunuzun 3 ve üç parçalık bir çoğaltma faktörüyle, her parçadaki tüm veriler parça grubundaki diğer tüm parçalara çoğaltılır. Parça grubundaki her parçanın farklı bir çoğaltma faktörü olabilir. Bu kurulum, yüksek kullanılabilirlik ve olağanüstü durum kurtarma tasarımınızı bir parça grubu içinde ve birden çok parça grubu arasında verimli bir şekilde tanımlamanıza yardımcı olur.

Önceki mimaride, A ve parça grubu B grubu her ikisi de aynı verileri içerir, ancak farklı kullanılabilirlik bölgelerinde bulunur. Hem parça grubu A hem de parça grubu B aynı çoğaltma faktörüne sahipse 3, parçalanmış tablonuzun her satırı/parçası iki parça grubunda 6 kez çoğaltılır. A grubu 3'lük bir çoğaltma faktörüne sahipse ve B grubu 2 çoğaltma faktörüne sahipse, her satır/yığın iki parça grubunda 5 kez çoğaltılır.

Bu kurulum, örnek düzeyinde veya kullanılabilirlik bölgesi düzeyinde bir hata oluşursa veri kaybını önler. Uygulama katmanı her parçadan okuyup yazabilir. Çakışmaları en aza indirmek için, Oracle Sharding her karma değer aralığı için bir "ana yığın" belirler. Bu özellik, belirli bir yığın için yazma isteklerinin ilgili yığına yönlendirilmesini sağlar. Buna ek olarak, Oracle GoldenGate ortaya çıkabilecek çakışmaları işlemek için otomatik çakışma algılama ve çözüm sağlar. Oracle Sharding ile GoldenGate uygulama daha fazla bilgi ve sınırlamalar için, [bir kırık veritabanı ile Oracle GoldenGate](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-high-availability.html#GUID-4FC0AC46-0B8B-4670-BBE4-052228492C72)kullanarak Oracle'ın belgeleri bakın.

Önceki mimaride, yüksek kullanılabilirlik için her kullanılabilirlik bölgesinde bir GSM/shard yöneticisi dağıtılır. Öneri, veri merkezi veya bölge başına en az bir GSM/parça yöneticisi dağıtmaktır. Ayrıca, uygulama sunucusunun bir örneği, bir parça grubu içeren her kullanılabilirlik bölgesinde dağıtılır. Bu kurulum, uygulama sunucusu ile veritabanı/parça grubu arasındaki gecikme yitirme süresinin düşük tutulmasını sağlar. Bir veritabanı başarısız olursa, bekleme veritabanıyla aynı bölgede bulunan uygulama sunucusu, veritabanı rolü geçiş yaptıktan sonra istekleri işleyebilir. Azure Application Gateway ve parça yöneticisi, istek ve yanıt gecikmesini ve rota isteklerini buna göre izler.

Uygulama açısından, istemci sistemi, isteği istemciye en yakın bölgeye yönlendiren Azure Uygulama Ağ Geçidi'ne (veya Azure'daki diğer yük dengeleme teknolojileri) bir istekte bulunuyor. Azure Application Gateway, aynı istemciden gelen tüm isteklerin aynı uygulama sunucusuna yönlendirilen yapışkan oturumları da destekler. Uygulama sunucusu, veri erişim sürücülerinde bağlantı havuzu kullanır. Bu özellik JDBC, ODP.NET, OCI, vb. gibi sürücülerde kullanılabilir. Sürücüler isteğin bir parçası olarak belirtilen parçalama tuşlarını tanıyabilir. JDBC istemcileri için [Oracle Evrensel Bağlantı Havuzu (UCP),](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/jjucp/ucp-database-sharding-overview.html) Apache Tomcat ve IIS gibi Oracle dışı uygulama istemcilerinin Oracle Sharding ile çalışmasını sağlayabilir. 

İlk istek sırasında, uygulama sunucusu, isteğin yönlendirilmesi gereken parça için yönlendirme bilgileri almak için kendi bölgesindeki parça yöneticisine bağlanır. Geçirilen parçalama anahtarına bağlı olarak, yönetmen uygulama sunucusunu ilgili parçaya yönlendirir. Uygulama sunucusu bu bilgileri bir harita oluşturarak önbelleğe alınır ve sonraki istekler için, parça yönetmenini atlar ve istekleri doğrudan parçaya yönlendirir.

## <a name="patching-and-maintenance"></a>Yama ve bakım

Oracle iş yüklerinizi Azure'a dağıtırken, Microsoft tüm ana bilgisayar işletim sistemi düzeyinde düzeltme ekiyle ilgilenir. Planlanan işletim sistemi düzeyindeki herhangi bir bakım, müşteriye bu planlı bakım için izin vermek üzere önceden müşterilere bildirilir. İki farklı Kullanılabilirlik Bölgesi'nden iki sunucu asla aynı anda yamultmuldü. VM bakımı ve yama hakkında daha fazla bilgi için [sanal makinelerin kullanılabilirliğini yönet](../../../virtual-machines/linux/manage-availability.md) ini görün. 

Sanal makine işletim sisteminizi yamamak [Azure Otomasyonu](../../../automation/automation-tutorial-update-management.md)kullanılarak otomatikhale getirilebilir. Oracle veritabanınızı yamalamak ve korumak, kapalı kalma süresini en aza indirmek için [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops) veya [Azure Automation](../../../automation/automation-tutorial-update-management.md) kullanılarak otomatikleştirilmiş ve zamanlanabilir. Oracle veritabanlarınız bağlamında nasıl kullanılabileceğini anlamak için [Sürekli Teslimat ve Mavi/Yeşil](/azure/devops/learn/what-is-continuous-delivery) Dağıtımlar'a bakın.

## <a name="architecture-and-design-considerations"></a>Mimarlık ve tasarım konuları

- Lisans maliyetlerinde tasarruf sağlamak ve performansı en üst düzeye çıkarmak için Oracle Database VM'niz için [kısıtlı çekirdek vCPUs'lara](../../../virtual-machines/windows/constrained-vcpu.md) sahip hiper iş parçacığı [bellek optimize edilmiş sanal makine](../../../virtual-machines/windows/sizes-memory.md) kullanmayı düşünün. Performans ve kullanılabilirlik için birden çok premium veya ultra disk (yönetilen disk) kullanın.
- Yönetilen diskler kullanırken, yeniden başlatıldığında disk/aygıt adı değişebilir. Montajlarınızın yeniden başlatmalarda kalıcı olmasını sağlamak için ad yerine UUID cihazını kullanmanız önerilir. Daha fazla bilgiyi [burada](../../../virtual-machines/linux/configure-raid.md#add-the-new-file-system-to-etcfstab)bulabilirsiniz.
- Bölge içinde yüksek kullanılabilirlik elde etmek için kullanılabilirlik bölgelerini kullanın.
- Oracle veritabanınız için ultra diskler (varsa) veya premium diskler kullanmayı düşünün.
- Oracle Data Guard'ı kullanarak başka bir Azure bölgesinde bekleme Oracle veritabanı ayarlamayı düşünün.
- Uygulamanız ve veritabanı katmanınız arasındaki gecikme süresini azaltmak için [yakınlık yerleşimi gruplarını](../../../virtual-machines/linux/co-location.md#proximity-placement-groups) kullanmayı düşünün.
- [Yönetim,](https://docs.oracle.com/en/enterprise-manager/) izleme ve günlük için Oracle Enterprise Manager'ı ayarlayın.
- Veritabanınız için kolaylaştırılmış depolama yönetimi için Oracle Otomatik Depolama Yönetimi'ni (ASM) kullanmayı düşünün.
- Herhangi bir kesinti olmaksızın veritabanınızda düzeltme ve güncelleştirmeleri yönetmek için [Azure Ardışık Hatları'nı](/azure/devops/pipelines/get-started/what-is-azure-pipelines) kullanın.
- Uygulama kodunuzu [yeniden deneme deseni,](/azure/architecture/patterns/retry) [devre kesici deseni](/azure/architecture/patterns/circuit-breaker)ve uygulamanızın daha esnek olması için [Bulut Tasarım Desenleri kılavuzunda](/azure/architecture/patterns/) tanımlanan diğer desenler gibi bulut yerel desenler eklemek için değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Senaryonuz için geçerli olan aşağıdaki Oracle başvuru makalelerini gözden geçirin.

- [Oracle Data Guard'a Giriş](https://docs.oracle.com/en/database/oracle/oracle-database/18/sbydb/introduction-to-oracle-data-guard-concepts.html#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7)
- [Oracle Data Guard Broker Kavramları](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/oracle-data-guard-broker-concepts.html)
- [Aktif-Etkin Yüksek Kullanılabilirlik için Oracle GoldenGate'i yapılandırma](https://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_bidirectional.htm#GWUAD282)
- [Oracle Sharding'e Genel Bakış](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-overview.html)
- [Oracle Active Data Guard Far Sync Sıfır Veri Kaybı Herhangi Bir Mesafede](https://www.oracle.com/technetwork/database/availability/farsync-2267608.pdf)
