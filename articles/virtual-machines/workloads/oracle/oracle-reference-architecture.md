---
title: Azure 'da Oracle veritabanları için başvuru mimarileri | Microsoft Docs
description: Microsoft Azure Sanal Makineler üzerinde Oracle Database Enterprise Edition veritabanlarını çalıştırmaya yönelik mimarilere başvurur.
services: virtual-machines-linux
author: romitgirdhar
manager: gwallace
tags: ''
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 12/13/2019
ms.author: rogirdh
ms.custom: ''
ms.openlocfilehash: 235482f5d44877e5c4e47aed4d7eaf2baea5c3fd
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75560343"
---
# <a name="reference-architectures-for-oracle-database-enterprise-edition-on-azure"></a>Azure 'da Oracle Database Enterprise Edition için başvuru mimarileri

Bu kılavuzda, Azure 'da yüksek kullanılabilirliğe sahip bir Oracle veritabanını dağıtmaya ilişkin bilgiler ayrıntılı olarak sunulmaktadır. Ayrıca, bu kılavuz olağanüstü durum kurtarma ile ilgili hususlar ele alır. Bu mimariler, müşteri dağıtımları temel alınarak oluşturulmuştur. Bu kılavuz yalnızca Oracle Database Enterprise Edition için geçerlidir.

Oracle veritabanınızın performansını en üst düzeye çıkarmak hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Oracle DB mimarisi](oracle-design.md).

## <a name="assumptions"></a>Varsayımlar

- Azure 'un [kullanılabilirlik alanları](../../../availability-zones/az-overview.md) gibi farklı kavramlarını kavramış olursunuz
- Oracle Database Enterprise Edition 12c veya üstünü çalıştırıyorsunuz
- Bu makaledeki çözümleri kullanırken lisanslama etkilerine ilişkin farkındayız ve bunları onaylamış olursunuz

## <a name="high-availability-for-oracle-databases"></a>Oracle veritabanları için yüksek kullanılabilirlik

Bulutta yüksek kullanılabilirlik elde etmek, her kuruluşun planlama ve tasarımının önemli bir parçasıdır. Microsoft Azure kullanılabilirlik [alanları](../../../availability-zones/az-overview.md) ve kullanılabilirlik kümeleri sunar (kullanılabilirlik bölgelerinin kullanılamadığı bölgelerde kullanılmak üzere). Bulut için tasarlamak üzere [sanal makinelerinizin kullanılabilirliğini yönetme](../../../virtual-machines/linux/manage-availability.md) hakkında daha fazla bilgi edinin.

Oracle Native araçlara ve sunumlarına ek olarak, Oracle [Data Guard](https://docs.oracle.com/en/database/oracle/oracle-database/18/sbydb/introduction-to-oracle-data-guard-concepts.html#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7), Azure 'da AYARLANABILECEĞI fsfo, [parça](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/admin/sharding-overview.html)ve [altın başak](https://www.oracle.com/middleware/technologies/goldengate.html) [ile Data Guard](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/index.html)gibi yüksek kullanılabilirliğe yönelik çözümler sağlar. Bu kılavuz, bu çözümlerin her biri için başvuru mimarilerini içerir.

Son olarak, bulut için uygulamaları geçirirken veya oluştururken, [yeniden deneme deseni](https://docs.microsoft.com/azure/architecture/patterns/retry) ve [devre kesici deseni](https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker)gibi bulut Yerel desenleri eklemek için uygulama kodunuzu ince ayar önemlidir. [Bulut tasarım desenleri kılavuzunda](https://docs.microsoft.com/azure/architecture/patterns/) tanımlanan ek desenler uygulamanızın daha dayanıklı olmasını sağlayabilir.

### <a name="oracle-rac-in-the-cloud"></a>Bulutta Oracle RAC

Oracle gerçek uygulama kümesi (RAC), bir veritabanı depolamasına (paylaşılan-tüm mimari desenler) erişen çok sayıda örneğe sahip olacak şekilde, müşterilerin yüksek bir geçiş yapmasına yardımcı olmak için Oracle tarafından sağlanan bir çözümdür. Oracle RAC, şirket içi yüksek kullanılabilirlik için de kullanılabilir olsa da, yalnızca örnek düzeyindeki hatalara karşı koruma ve raf düzeyinde ya da veri merkezi düzeyindeki hatalara karşı değil, bulutta yüksek kullanılabilirlik için yalnızca Oracle RAC kullanılamaz. Bu nedenle, Oracle, yüksek kullanılabilirlik için veritabanınızda Oracle Data Guard (tek örnekli veya RAC) kullanmanızı önerir. Müşteriler, iş açısından kritik uygulamalarını çalıştırmak için genellikle yüksek bir SLA gerektirir. Oracle RAC Şu anda Azure 'da Oracle tarafından sertifikalı veya desteklenmiyor. Ancak Azure, örnek düzeyindeki hatalara karşı korumaya yardımcı olmak için Azure gibi Kullanılabilirlik Alanları ve planlı bakım pencereleri sunmaktadır. Bunlara ek olarak, müşteriler, veritabanlarını raf düzeyi ve coğrafi politik arızalardan farklı şekilde koruyarak yüksek performans ve resiliancy için Oracle Data Guard, Oracle GoldenGate ve Oracle parçaları gibi teknolojileri de kullanabilir.

Oracle veritabanlarını Oracle Data Guard veya GoldenGate ile birlikte birden çok [kullanılabilirlik](https://docs.microsoft.com/azure/availability-zones/az-overview) alanında çalıştırırken, müşteriler% 99,99 ' lik bir çalışma süresi SLA 'sı alabilir. Kullanılabilirlik bölgelerinin henüz mevcut olmadığı Azure bölgelerinde, müşteriler [kullanılabilirlik kümelerini](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) kullanabilir ve% 99,95 ' lik bir çalışma süresi SLA 'sı elde edebilir.

>NOTE: Microsoft tarafından sunulan çalışma süresi SLA 'sına göre çok daha yüksek bir çalışma süresi hedefi olabilir.

## <a name="disaster-recovery-for-oracle-databases"></a>Oracle veritabanları için olağanüstü durum kurtarma

Görev açısından kritik uygulamalarınızı bulutta barındırırken, yüksek kullanılabilirlik ve olağanüstü durum kurtarma için tasarlamak önemlidir.

Oracle Database Enterprise Edition için Oracle Data Guard olağanüstü durum kurtarma için yararlı bir özelliktir. [Eşlenmiş bir Azure bölgesinde](/azure/best-practices-availability-paired-regions) bir bekleme veritabanı örneği ayarlayabilir ve olağanüstü durum kurtarma Için Data Guard yük devretmesini ayarlayabilirsiniz. Sıfır veri kaybı için, etkin Data Guard 'a ek olarak bir Oracle Data Guard 'a yönelik eşitleme örneği dağıtmanız önerilir. 

Uygulamanızın gecikmeye izin vermesi durumunda Data Guard 'ın en çok eşitleme örneğini, Oracle birincil veritabanınızın farklı bir kullanılabilirlik alanında ayarlamayı düşünün (tam test gerekir). Yineleme dosyalarınızın eşzamanlı aktarımını en fazla eşitleme örneğine ayarlamak için **maksimum kullanılabilirlik** modunu kullanın. Daha sonra bu dosyalar, bekleyen veritabanına zaman uyumsuz olarak aktarılır. 

Uygulamanız, **en yüksek kullanılabilirlik** modundaki (zaman uyumlu) farklı bir kullanılabilirlik alanında bir çok eşitleme örneği ayarlarken performans kaybına izin vermezse, birincil veritabanınızla aynı Kullanılabilirlik bölgesinde bir çok eşitleme örneği ayarlayabilirsiniz. Ek kullanılabilirlik için, birincil veritabanınıza ve en az bir örneğe yakın olan birden çok sayıda eşitleme örneğini, bekleyen veritabanınıza (rol geçişleri) yakın bir şekilde ayarlamayı düşünün. Bu [Oracle Active Data Guard 'ın en çok eşitleme teknik incelemesi](https://www.oracle.com/technetwork/database/availability/farsync-2267608.pdf)Içindeki Oracle Data Guard 'ın en fazla eşitlemesi hakkında daha fazla

Oracle Standard Edition veritabanlarını kullanırken, yüksek kullanılabilirlik ve olağanüstü durum kurtarma ayarlamanıza olanak tanıyan, Dbziyaret bekleme gibi ISV çözümleri vardır.

## <a name="reference-architectures"></a>Başvuru mimarileri

### <a name="oracle-data-guard"></a>Oracle Data Guard

Oracle Data Guard, Kurumsal veriler için yüksek kullanılabilirlik, veri koruma ve olağanüstü durum kurtarma sağlar. Data Guard, bekleyen veritabanlarını birincil veritabanının işlemsel olarak tutarlı kopyaları olarak korur. Birincil ve ikincil veritabanları ile gecikme süresi için uygulama toleransı arasındaki uzaklığa bağlı olarak, zaman uyumlu veya zaman uyumsuz çoğaltma ayarlayabilirsiniz. Daha sonra, birincil veritabanı planlanmış veya planlanmamış bir kesinti nedeniyle kullanılamıyorsa, veri koruma, bekleyen bir veritabanını birincil role değiştirebilir ve kapalı kalma süresini en aza indirir. 

Oracle Data Guard kullanırken, ikincil veritabanınızı salt okuma amacıyla de açabilirsiniz. Bu yapılandırmaya Active Data Guard adı verilir. Oracle Database 12c, Data Guard 'ın en çok eşitleme örneği olarak adlandırılan bir özellik sunmuştur. Bu örnek, performansı tehlikeye duymadan Oracle veritabanınızın sıfır veri kaybı yapılandırmasını ayarlamanıza olanak sağlar.

> [!NOTE]
> Etkin veri koruma ek lisanslama gerektirir. Bu lisans, en çok eşitleme özelliğini kullanmak için de gereklidir. Lisanslama etkilerini tartışmak için lütfen Oracle temsilcinizle bağlantı sağlayın.

#### <a name="oracle-data-guard-with-fsfo"></a>FSFO ile Oracle Data Guard
Hızlı başlangıç yük devretmesi (FSFO) ile Oracle Data Guard, aracıyı ayrı bir makineye ayarlayarak ek dayanıklılık sağlayabilir. Data Guard Broker ve ikincil veritabanı, gözlemci çalıştırır ve kapalı kalma süresi için birincil veritabanını gözlemleyin. Bu, Data Guard gözlemci kurulumunda de artıklık sağlar. 

Oracle Database sürüm 12,2 ve üzeri ile, tek bir Oracle Data Guard Broker yapılandırması ile birden çok Observer yapılandırmak da mümkündür. Bu kurulum, bir gözlemci ve ikincil veritabanı deneyimine kapalı kalma olasılığına karşı ek kullanılabilirlik sağlar. Data Guard Aracısı hafif ve görece küçük bir sanal makinede barındırılabilir. Data Guard Aracısı ve avantajları hakkında daha fazla bilgi edinmek için bu konudaki [Oracle belgelerini](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/oracle-data-guard-broker-concepts.html) ziyaret edin.

Aşağıdaki diyagram, Azure 'da kullanılabilirlik alanları ile Oracle Data Guard 'ın kullanılması için önerilen bir mimaridir. Bu mimari% 99,99 VM çalışma süresi SLA 'sını almanızı sağlar.

![Veri koruma Aracısı ile kullanılabilirlik alanlarını kullanmak Oracle Database-FSFO](./media/oracle-reference-architecture/oracledb_dg_fsfo_az.png)

Önceki diyagramda istemci sistemi, Web aracılığıyla Oracle arka ucu ile özel bir uygulamaya erişir. Web ön ucu, bir yük dengeleyicide yapılandırılır. Web ön ucu, çalışmayı işlemek için uygun uygulama sunucusuna bir çağrı yapar. Uygulama sunucusu birincil Oracle veritabanını sorgular. Oracle veritabanı, lisans maliyetlerine kaydetmek ve performansı en üst düzeye çıkarmak için [Kısıtlanmış çekirdek vCPU 'ları](../../../virtual-machines/windows/constrained-vcpu.md) olan hiper iş parçacıklı [bellek için iyileştirilmiş bir sanal makine](../../../virtual-machines/windows/sizes-memory.md) kullanılarak yapılandırılmıştır. Performans ve yüksek kullanılabilirlik için birden fazla Premium veya ultra disk (yönetilen diskler) kullanılır.

Oracle veritabanları, yüksek kullanılabilirlik için birden fazla kullanılabilirlik bölgesine yerleştirilir. Her bölge, bağımsız güç, soğutma ve ağ ile donatılmış bir veya daha fazla veri merkezinden oluşur. Dayanıklılık sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge ayarlanır. Bölge içindeki kullanılabilirlik bölgelerinin fiziksel ayrımı, verileri veri merkezi hatalarından korur. Ayrıca, bir kesinti oluştuğunda veritabanını ikincil üzerinde başlatmak ve yükünü devretmek için iki kullanılabilirlik bölgesinde iki fsfo gözlemcilerin ayarlanır. 

Diğer gözlemcilerin ve/veya standby veritabanlarını, önceki mimaride gösterilen bölgeden farklı bir kullanılabilirlik bölgesinde (Bu durumda az 1) ayarlayabilirsiniz. Son olarak, Oracle veritabanları, Oracle Enterprise Manager (OEM) tarafından çalışma süresi ve performans için izlenir. OEM, çeşitli performans ve kullanım raporlarını oluşturmanıza olanak tanır.

Kullanılabilirlik bölgelerinin desteklenmeyen bölgelerde, Oracle Database yüksek oranda kullanılabilir bir şekilde dağıtmak için kullanılabilirlik kümelerini kullanabilirsiniz. Kullanılabilirlik kümeleri% 99,95 ' lik bir VM çalışma süresi elde etmenizi sağlar. Aşağıdaki diyagramda bu kullanım için bir başvuru mimarisi verilmiştir:

![Veri koruma Aracısı ile kullanılabilirlik kümelerini kullanmak Oracle Database-FSFO](./media/oracle-reference-architecture/oracledb_dg_fsfo_as.png)

> [!NOTE]
> * Yalnızca bir OEM 'nin dağıtılmakta olan bir örneği olduğundan, Oracle Enterprise Manager VM 'sinin bir kullanılabilirlik kümesine yerleştirilmesi gerekmez.
> * Ultra diskler, bir kullanılabilirlik kümesi yapılandırmasında Şu anda desteklenmiyor.

#### <a name="oracle-data-guard-far-sync"></a>Oracle Data Guard en çok eşitleme

Oracle Data Guard en çok eşitleme, Oracle veritabanları için sıfır veri kaybı koruma yeteneği sağlar. Bu özellik, veritabanı makineniz başarısız olursa veri kaybına karşı korumanızı sağlar. Oracle Data Guard 'ın en çok eşitlemesini ayrı bir sanal makineye yüklenmesi gerekir. En çok eşitleme, yalnızca bir denetim dosyası, parola dosyası, SPFile ve bekleme günlüğü olan basit bir Oracle örneğidir. Veri dosyası veya rego günlük dosyası yok. 

Sıfır veri kaybı koruması için, birincil veritabanınız ile en çok eşitleme örneği arasında zaman uyumlu iletişim olmalıdır. En çok eşitleme örneği, birincili olarak zaman uyumlu bir şekilde geri alır ve bunu tüm bekleme veritabanlarına anında zaman uyumsuz bir şekilde iletir. Bu kurulum, yalnızca tüm bekleme veritabanları yerine yalnızca en fazla eşitleme örneğine bir tane gönderilmesi gerektiğinden birincil veritabanındaki yükü azaltır. Bir çok eşitleme örneği başarısız olursa Data Guard, neredeyse sıfır veri kaybı korumasını sağlamak için birincil veritabanından ikincil veritabanına zaman uyumsuz aktarım kullanır. Müşteriler, ek dayanıklılık için her veritabanı örneği (birincil ve ikincil) başına birden çok eşitleme örneği dağıtabilir.

Aşağıdaki diyagram, Oracle Data Guard 'ın en çok eşitlemesini kullanan yüksek oranda kullanılabilir bir mimaridir:

![Veri koruma ile kullanılabilirlik alanlarını kullanan Oracle veritabanı & Broker-FSFO](./media/oracle-reference-architecture/oracledb_dg_fs_az.png)

Önceki mimaride, iki arasındaki gecikmeyi azaltmak için veritabanı örneğiyle aynı Kullanılabilirlik bölgesinde dağıtılan bir çok eşitleme örneği vardır. Uygulamanın gecikme süresine duyarlı olduğu durumlarda, veritabanınızı ve en çok eşitleme örneğini veya örneklerini bir [yakınlık yerleşimi grubuna](../../../virtual-machines/linux/proximity-placement-groups.md)dağıtmanız önerilir.

Aşağıdaki diyagram, yüksek kullanılabilirlik ve olağanüstü durum kurtarma sağlamak için Oracle Data Guard FSıNFO ve en çok eşitleme kullanan bir mimaridir:

![Veri koruma & Aracısı-FSFO ile olağanüstü durum kurtarma için kullanılabilirlik bölgelerini kullanma Oracle Database](./media/oracle-reference-architecture/oracledb_dg_fs_az_dr.png)

### <a name="oracle-goldengate"></a>Oracle GoldenGate

GoldenGate, kuruluş genelinde birden çok farklı platform arasında işlem düzeyindeki verilerin değişimini ve işlemesini mümkün değildir. İşlem bütünlüğü ve mevcut altyapınızdaki en az ek yük ile kaydedilmiş işlemleri hareket ettirir. Modüler mimarisi, seçili veri kayıtlarını, işlemsel değişiklikleri ve DDL 'de yapılan değişiklikleri (veri tanımlama dili) çeşitli topolojilerde ayıklama ve çoğaltma esnekliği sağlar.

Oracle GoldenGate, çift yönlü çoğaltma sağlayarak veritabanınızı yüksek kullanılabilirlik için yapılandırmanızı sağlar. Bu, **çok yöneticili** veya **etkin-etkin bir yapılandırma**ayarlamanıza olanak sağlar. Aşağıdaki diyagram, Azure 'da Oracle GoldenGate etkin-etkin kurulumu için önerilen bir mimaridir. Aşağıdaki mimaride, Oracle veritabanı, lisans maliyetlerine kaydetmek ve performansı en üst düzeye çıkarmak için [Kısıtlanmış çekirdek vCPU 'ları](../../../virtual-machines/windows/constrained-vcpu.md) olan hiper iş parçacıklı [bellek için iyileştirilmiş bir sanal makine](../../../virtual-machines/windows/sizes-memory.md) kullanılarak yapılandırılmıştır. Performans ve kullanılabilirlik için birden fazla Premium veya ultra disk (yönetilen diskler) kullanılır.

![Veri koruma Aracısı ile kullanılabilirlik alanlarını kullanmak Oracle Database-FSFO](./media/oracle-reference-architecture/oracledb_gg_az.png)

> [!NOTE]
> Benzer bir mimari, kullanılabilirlik bölgelerinin Şu anda kullanılamadığı bölgelerde kullanılabilirlik kümeleri kullanılarak ayarlanabilir.

Oracle GoldenGate, verilerinizi bir Oracle veritabanı sunucusundan diğerine zaman uyumsuz olarak çoğaltmanıza yardımcı olan ayıklama, pompa ve Replicat gibi süreçler içerir. Bu süreçler, kullanılabilirlik bölge düzeyi kapalı kalma süresi varsa veritabanınızın yüksek oranda kullanılabilirliğini sağlamak için çift yönlü bir çoğaltma ayarlamanıza olanak sağlar. Önceki diyagramda, ayıklama işlemi Oracle veritabanınızla aynı sunucuda çalışır, ancak veri göndericisi ve Replicat işlemleri aynı Kullanılabilirlik bölgesindeki ayrı bir sunucuda çalışır. Replicat işlemi, diğer kullanılabilirlik bölgesindeki veritabanından veri almak ve verileri kullanılabilirlik bölgesinde Oracle veritabanına uygulamak için kullanılır. Benzer şekilde, veri göndericisi işlemi, ayıklama işlemi tarafından ayıklanan verileri diğer kullanılabilirlik bölgesindeki Replicat işlemine gönderir. 

Yukarıdaki mimari diyagramda, ayrı bir sunucuda yapılandırılmış veri göndericisi ve Replicat işlemi gösterilirken, sunucunuzun kapasitesine ve kullanımına bağlı olarak, tüm Oracle GoldenGate işlemlerini aynı sunucuda ayarlayabilirsiniz. Sunucunuzun kullanım modelini anlamak için her zaman AWR raporunuzu ve Azure 'daki ölçümleri inceleyin.

Farklı kullanılabilirlik bölgelerinde veya farklı bölgelerde Oracle GoldenGate çift yönlü çoğaltmasını ayarlarken, farklı bileşenler arasındaki gecikmede uygulamanız için kabul edilebilir olduğundan emin olmanız önemlidir. Kullanılabilirlik alanları ve bölgeler arasındaki gecikme farklılık gösterebilir ve birden çok etkene bağlıdır. Uygulama katmanınız ve/veya bölgelerindeki uygulama katmanınız ile veritabanı katmanınız arasında performans testlerini ayarlamanız önerilir.

Uygulama katmanı kendi alt ağında ayarlanabilir ve veritabanı katmanı kendi alt ağına ayrılabilir. Mümkün olduğunda, uygulama sunucularınız arasındaki trafiğin yükünü dengelemek için [Azure Application Gateway](../../../application-gateway/overview.md) kullanmayı düşünün. Azure Application Gateway sağlam bir Web trafiği yük dengeleyicidir. Bu, bir kullanıcı oturumunu aynı sunucuda tutan ve böylece veritabanındaki çakışmaları en aza indiren tanımlama bilgisine dayalı oturum benzeşimi sağlar. Application Gateway alternatifleri [Azure Load Balancer](../../../load-balancer/load-balancer-overview.md) ve [Azure Traffic Manager](../../../traffic-manager/traffic-manager-overview.md).

### <a name="oracle-sharding"></a>Oracle parçalama

Parçalama, Oracle 12,2 ' de tanıtılan bir veri katmanı düzeniydi. Bağımsız veritabanlarında verilerinizi yatay olarak bölümleyip ölçeklendirmenize olanak tanır. Her bir veritabanının ayrılmış bir sanal makinede barındırıldığı, dayanıklılık ve daha fazla kullanılabilirlik özelliklerine ek olarak yüksek okuma ve yazma verimi sağlayan bir Share-Nothing mimarisidir. Bu model tek hata noktalarını ortadan kaldırır, hata yalıtımı sağlar ve kapalı kalma süresi olmadan çalışırken yükseltmelere izin verir. Bir parça veya veri merkezi düzeyi hatasının kapalı kalma süresi, diğer veri merkezlerindeki diğer parçaların performansını veya kullanılabilirliğini etkilemez. 

Parçalama, herhangi bir kapalı kalma süresine neden olmayan yüksek verimlilik OLTP uygulamaları için uygundur. Aynı parçalı anahtar içeren tüm satırlar her zaman aynı parça üzerinde olmak üzere garanti edilir ve bu sayede yüksek tutarlılık sağlayan performansı artırır. Parçalama kullanan uygulamaların, birincil olarak parçalı anahtar (örneğin, *CustomerID* veya *accountNum*) kullanarak verilere eriştiği iyi tanımlanmış bir veri modeli ve veri dağıtım stratejisi (tutarlı karma, Aralık, liste veya bileşik) olması gerekir. Parçalama Ayrıca son müşterilere daha yakın olan belirli veri kümelerini depolamanıza olanak tanır ve böylece performans ve uyumluluk gereksinimlerinizi karşılamanıza yardımcı olur.

Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için parçaları çoğaltmanız önerilir. Bu kurulum, Oracle Data Guard veya Oracle GoldenGate gibi Oracle teknolojileri kullanılarak yapılabilir. Çoğaltma birimi bir parça, parçanın bir parçası veya parça grubu olabilir. Parçalı bir veritabanının kullanılabilirliği, bir veya daha fazla parçalama kesintiye veya yavaşlamasının etkilenmemektedir. Yüksek kullanılabilirlik için, bekleme parçaları birincil parçaların yerleştirildiği aynı Kullanılabilirlik bölgesine yerleştirilebilecek. Olağanüstü durum kurtarma için, bekleme parçaları başka bir bölgede yer alabilir. Ayrıca, bu bölgelerdeki trafiğe sunmaları için parçaları birden çok bölgeye dağıtabilirsiniz. [Oracle parçalama belgelerinde](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-high-availability.html)parçalı veritabanınızın yüksek kullanılabilirliğini ve çoğaltmasını yapılandırma hakkında daha fazla bilgi edinin.

Oracle parçalama öncelikle aşağıdaki bileşenlerden oluşur. [Oracle parçalama belgelerinde](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-overview.html)bu bileşenler hakkında daha fazla bilgi bulabilirsiniz:

- Parça **kataloğu** -tüm parça veritabanı yapılandırma verileri için kalıcı bir mağaza olan özel amaçlı Oracle veritabanı. Parça kataloğunda parçaları ekleme veya kaldırma, veri eşleme ve DDLs gibi tüm yapılandırma değişiklikleri parça kataloğunda başlatılır. Parça kataloğu, bir SDB içindeki tüm yinelenen tabloların ana kopyasını da içerir. 

  Parça kataloğu, tüm parçalar içindeki yinelenen tablolarda yapılan değişiklikleri otomatik olarak çoğaltmak için gerçekleştirilmiş görünümleri kullanır. Parça kataloğu veritabanı aynı zamanda, parçalı anahtar belirtmeyen çok parçalı sorguları ve sorguları işlemek için kullanılan bir sorgu Düzenleyicisi görevi görür. 
  
  Parçalı Katalog için kullanılabilirlik alanları veya kullanılabilirlik kümeleri ile birlikte Oracle Data Guard 'ın kullanılması, yüksek kullanılabilirlik için önerilen en iyi uygulamadır. Parça kataloğunun kullanılabilirliğinin, parçalı veritabanının kullanılabilirliği üzerinde hiçbir etkisi yoktur. Parça kataloğundaki bir kesinti, Data Guard yük devretmesinin tamamlandığı kısa bir süre boyunca yalnızca bakım işlemlerini ve çok parçalı sorguları etkiler. Çevrimiçi işlemler, SDB tarafından yönlendirilmeye ve yürütülmeye devam eder ve bir katalog kesintisinden etkilenmez.

- Parça **yöneticileri** -parçalarınızın bulunduğu her bölge/kullanılabilirlik bölgesinde dağıtılması gereken hafif hizmetler. Parça doğrulayıcılar, Oracle parçaları 'nın bağlamında dağıtılan küresel hizmet yöneticilerinden oluşur. Yüksek kullanılabilirlik için, parçalarınızın bulunduğu her bir kullanılabilirlik alanına en az bir parça Direktörü dağıtmanız önerilir. 

  Başlangıçta veritabanına bağlanırken, yönlendirme bilgileri bir parça Direktörü tarafından ayarlanır ve sonraki istekler için önbelleğe alınır ve bu da parça Direktörü atlar. Oturum bir parça ile kurulduktan sonra, tüm SQL sorguları ve DMLs 'ler, belirtilen parça kapsamında desteklenir ve yürütülür. Bu yönlendirme hızlıdır ve çoklu site işlemleri gerçekleştiren tüm OLTP iş yükleri için kullanılır. En yüksek performans ve kullanılabilirlik gerektiren tüm OLTP iş yükleri için doğrudan yönlendirmeyi kullanmanız önerilir. Bir parça kullanılamaz hale geldiğinde veya parçalara ayırma topolojisinde değişiklik yapıldığında yönlendirme önbelleği otomatik olarak yenilenir. 
  
  Yüksek performanslı, verilere bağımlı yönlendirme için, Oracle, parçalı veritabanındaki verilere erişirken bir bağlantı havuzunun kullanılmasını önerir. Oracle bağlantı havuzları, dile özgü kitaplıklar ve sürücüler Oracle parçalama 'yi destekler. Daha fazla ayrıntı için [Oracle parçalama belgelerine](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-overview.html#GUID-3D41F762-BE04-486D-8018-C7A210D809F9) bakın.

- **Küresel hizmet** -genel hizmet normal veritabanı hizmetine benzerdir. Bir veritabanı hizmetinin tüm özelliklerine ek olarak, küresel bir hizmet, istemciler ile parça ve çoğaltma gecikmesi toleransı arasındaki bölge benzeşimi gibi parçalı veritabanları için özelliklere sahiptir. Parçalı bir veritabanına veri okumak/buradan veri yazmak için yalnızca bir genel hizmetin oluşturulması gerekir. Etkin Data Guard 'ı kullanırken ve parçaların salt okuma çoğaltmalarını ayarlarken, salt okuma iş yükleri için başka bir gGobal hizmeti oluşturabilirsiniz. İstemci bu genel Hizmetleri veritabanına bağlanmak için kullanabilir.

- Parça **veritabanları-parça veritabanları Oracle** veritabanlarlarlardır. Her veritabanı, hızlı başlatma yük devretmesi (FSFO) etkin olan bir aracı yapılandırmasında Oracle Data Guard kullanılarak çoğaltılır. Her parçada Data Guard yük devretmesini ve çoğaltmasını ayarlamanız gerekmez. Bu, paylaşılan veritabanı oluşturulduğunda otomatik olarak yapılandırılır ve dağıtılır. Belirli bir parça başarısız olursa, Oracle paylaşımı birincil sunucudan bekleme moduna otomatik olarak yük devreder.

Oracle parçalı veritabanlarını iki arabirim ile dağıtabilir ve yönetebilirsiniz: Oracle Enterprise Manager Cloud Control GUI ve/veya `GDSCTL` komut satırı yardımcı programı. Bulut denetimini kullanarak kullanılabilirlik ve performans için farklı parçaları da izleyebilirsiniz. `GDSCTL DEPLOY` komutu, parçaları ve ilgili dinleyicileri otomatik olarak oluşturur. Ayrıca, bu komut, yönetici tarafından belirtilen parça düzeyinde yüksek kullanılabilirlik için kullanılan çoğaltma yapılandırmasını otomatik olarak dağıtır.

Bir veritabanını parçalara almanın farklı yolları vardır:

* Sistem tarafından yönetilen parçalar bölümlendirme kullanılarak parçalar arasında otomatik olarak dağıtılır
* Kullanıcı tanımlı parçalama-verileri parçalara eşlemeyi, yasal düzenlemeler veya veri yerelleştirme gereksinimleri olduğunda iyi şekilde çalışacak şekilde belirtmenize olanak tanır.
* Birleşik parçalara ayırma-farklı parçalama _alanları_ için sistem tarafından yönetilen ve Kullanıcı tanımlı parçaların birleşimi
* Tablo alt bölümleri-normal bölümlenmiş tabloya benzer.

Oracle belgelerindeki farklı [parça yöntemleri](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-methods.html) hakkında daha fazla bilgi edinin.

Parçalı bir veritabanı uygulamalar ve geliştiriciler için tek bir veritabanı gibi görünebilmesine karşın, parçalı olmayan bir veritabanından parçalı bir veritabanına geçiş yaparken, hangi tabloların çoğaltılana karşı bir şekilde olduğunu belirlemesi için dikkatli bir planlama gerekir. 

Yinelenen tablolar tüm parçalar üzerinde depolanır, ancak parçalı tablolar farklı parçalar arasında dağıtılır. Küçük ve boyutlu tabloları yinelemek ve olgu tablolarını dağıtmak/parçalara eklemek tavsiye edilir. Veriler, merkezi düzenleyici olarak ya da her parça üzerinde veri göndericisinin çalıştırılarak parçalı veritabanınıza yüklenebilir. Oracle belgelerindeki [verileri parçalı bir veritabanına geçirme](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-loading-data.html) hakkında daha fazla bilgi edinin.

#### <a name="oracle-sharding-with-data-guard"></a>Data Guard ile Oracle parçalama

Oracle Data Guard, sistem tarafından yönetilen, Kullanıcı tanımlı ve bileşik parçalama yöntemleriyle parçalama için kullanılabilir.

Aşağıdaki diyagramda, her parça için yüksek kullanılabilirlik için kullanılan Oracle veri koruyucusu ile Oracle parçalara yönelik bir başvuru mimarisi bulunur. Mimari diyagramı, bileşik bir parçalama _yöntemi_gösterir. Mimari diyagramı, büyük olasılıkla veri konumu, Yük Dengeleme, yüksek kullanılabilirlik, olağanüstü durum kurtarma vb. için farklı gereksinimlere sahip uygulamalar için farklılık gösterir ve parçalama için farklı bir yöntem kullanabilir. Oracle parçalama, bu seçenekleri sağlayarak bu gereksinimleri karşılamanıza ve yatay ve verimli bir şekilde ölçeklendirmenize olanak tanır. Benzer bir mimari, Oracle GoldenGate kullanılarak da dağıtılabilir.

![Data Guard broker ile kullanılabilirlik bölgelerini kullanarak parçalama Oracle Database-FSFO](./media/oracle-reference-architecture/oracledb_dg_sh_az.png)

Sistem tarafından yönetilen parça yapılandırma ve yönetme kolayıdır, Kullanıcı tanımlı parça veya bileşik parçalar, veri ve uygulamanızın coğrafi olarak dağıtıldığı ve çoğaltma üzerinde denetim sahibi olmanız gereken senaryolarda oldukça uygundur . 

Yukarıdaki mimaride bileşik parçalar, verileri coğrafi olarak dağıtmak ve uygulama katmanlarınızın yatay olarak ölçeğini genişletmek için kullanılır. Bileşik parçalama, sistem tarafından yönetilen ve Kullanıcı tanımlı parçaların bir birleşimidir ve bu nedenle her iki yöntemin de avantajını sağlar. Önceki senaryoda, veriler ilk olarak bölgeye göre ayrılmış birden çok parçalanmış boşlukların arasına eklenir. Daha sonra, veriler, parçalama alanında birden çok parça arasında tutarlı karma ile daha fazla bölümlenmiştir. Her bir parça alanı birden çok kıardgrup içerir. Her bir shardgroup birden çok parçaya sahiptir ve bu durumda çoğaltma için bir "birim" olur. Her bir shardgroup, shardspace 'teki tüm verileri içerir. A1 ve B1 'ler birincil kıarda grupları, a2 ve B2 grupları her bir Tek tek parçaları, bir parçalama grubu yerine çoğaltma birimi olmasını seçebilirsiniz.

Önceki mimaride, yüksek kullanılabilirlik için her kullanılabilirlik alanına bir GSM/parça Direktörü dağıtılır. Öneri, veri merkezi/bölge başına en az bir GSM/parça Direktörü dağıtmaktır. Ayrıca, bir shardgroup içeren her bir kullanılabilirlik alanında uygulama sunucusunun bir örneği dağıtılır. Bu kurulum, uygulamanın uygulama sunucusu ve veritabanı/shardgroup arasındaki gecikmeyi düşük olarak tutmasını sağlar. Bir veritabanı başarısız olursa, bekleyen veritabanıyla aynı bölgedeki uygulama sunucusu, veritabanı rolü geçişi gerçekleştiğinde istekleri işleyebilir. Azure Application Gateway ve parça Direktörü, istek ve yanıt gecikmesini ve istek isteklerini uygun şekilde izler.

İstemci sistemi, bir uygulama açısından, isteği istemciye en yakın bölgeye yönlendiren Azure Application Gateway (veya Azure 'daki diğer yük dengeleme teknolojilerine) bir istek yapar. Azure Application Gateway Ayrıca yapışkan oturumları destekler, bu nedenle aynı istemciden gelen tüm istekler aynı uygulama sunucusuna yönlendirilir. Uygulama sunucusu, veri erişim sürücülerinde bağlantı havuzu kullanır. Bu özellik JDBC, ODP.NET, OCı vb. gibi sürücülerde kullanılabilir. Sürücüler, isteğin bir parçası olarak belirtilen parça anahtarlarını algılayabilir. JDBC istemcileri için [Oracle Universal bağlantı havuzu (UCP)](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/jjucp/ucp-database-sharding-overview.html) , Apache Tomcat ve IIS gibi Oracle olmayan uygulama istemcilerinin Oracle parçalaması ile çalışmasını sağlayabilir. 

İlk istek sırasında, uygulama sunucusu, isteğin yönlendirilmesi gereken parça için yönlendirme bilgilerini almak amacıyla bölgesindeki parça Direktörü bağlanır. Bir parçalama anahtarına bağlı olarak, yönetmen uygulama sunucusunu ilgili parçaya yönlendirir. Uygulama sunucusu bu bilgileri bir harita oluşturarak önbelleğe alır ve sonraki istekler için parça Direktörü atlar ve istekleri doğrudan parçalamaya yönlendirir.

#### <a name="oracle-sharding-with-goldengate"></a>GoldenGate ile Oracle parçalama

Aşağıdaki diyagramda, her parça için bölge içi yüksek kullanılabilirlik için Oracle GoldenGate ile Oracle parçalama için bir başvuru mimarisi bulunur. Önceki mimarinin aksine, Bu mimaride yalnızca tek bir Azure bölgesinde (birden fazla kullanılabilirlik bölgesi) yüksek kullanılabilirlik noktası yoktur. Bunlardan biri, Oracle GoldenGate kullanarak çok bölgeli yüksek kullanılabilirliğe sahip bir veritabanını (önceki örneğe benzer şekilde) dağıtabilir.

![GoldenGate ile kullanılabilirlik bölgelerini kullanarak parçalama Oracle Database](./media/oracle-reference-architecture/oracledb_gg_sh_az.png)

Önceki başvuru mimarisi, verileri parçalara ayırma için _sistem tarafından yönetilen_ parça yöntemini kullanır. Oracle GoldenGate çoğaltma bir öbek düzeyinde yapıldığından, bir parçaya çoğaltılan veriler başka bir parça için çoğaltılabilir. Diğer yarısı farklı bir parça için çoğaltılabilir. 

Verilerin nasıl çoğaltılacağı, çoğaltma faktörüne bağlıdır. 2 çoğaltma faktörü ile, her bir veri öbeğinin, shardgroup içindeki üç parçalama üzerinde iki kopyasına sahip olursunuz. Benzer şekilde, shardgroup 'inizdeki bir çoğaltma faktörü 3 ve üç parçalama ile, her bir parçadaki tüm veriler, shardgroup 'taki her parçaya çoğaltılır. Shardgroup 'taki her parçanın farklı bir çoğaltma faktörü olabilir. Bu kurulum, yüksek kullanılabilirlik ve olağanüstü durum kurtarma tasarımınızı bir parçalıya grubu içinde ve birden çok kıardgroup 'ta verimli bir şekilde tanımlamanıza yardımcı olur.

Önceki mimaride, shardgroup A ve shardgroup B her ikisi de aynı verileri içerir, ancak farklı kullanılabilirlik bölgelerinde bulunur. Hem shardgroup A hem de shardgroup B aynı çoğaltma faktörüyle 3 ' ü içeriyorsa, parçalı tablonuzun her satırı/öbeği iki parça grubunda 6 kez çoğaltılır. Shardgroup A 'nın yineleme faktörü 3 ve shardgroup B çoğaltma faktörü 2 ise, her satır/öbek iki kıardı grubunda 5 kez çoğaltılır.

Bu kurulum, örnek düzeyinde veya kullanılabilirlik bölgesi düzeyinde bir hata oluşursa veri kaybını önler. Uygulama katmanı, her parçaya okuyup yazabilir. Çakışmaları en aza indirmek için, Oracle parçalama her bir karma değer aralığı için bir "ana öbek" belirler. Bu özellik, belirli bir öbek için yazma isteklerinin karşılık gelen öbekte yönlendirilmesini sağlar. Oracle GoldenGate Ayrıca, oluşabilecek çakışmaları işlemek için otomatik çakışma algılama ve çözümleme sağlar. Oracle parçalama ile GoldenGate uygulama hakkında daha fazla bilgi ve sınırlamalar için bkz. Oracle [GoldenGate 'i parçalı bir veritabanıyla](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-high-availability.html#GUID-4FC0AC46-0B8B-4670-BBE4-052228492C72)kullanma hakkında Oracle belgeleri.

Önceki mimaride, yüksek kullanılabilirlik için her kullanılabilirlik alanına bir GSM/parça Direktörü dağıtılır. Öneri, veri merkezi veya bölge başına en az bir GSM/parça Direktörü dağıtmaktır. Ayrıca, bir shardgroup içeren her bir kullanılabilirlik alanında uygulama sunucusunun bir örneği dağıtılır. Bu kurulum, uygulamanın uygulama sunucusu ve veritabanı/shardgroup arasındaki gecikmeyi düşük olarak tutmasını sağlar. Bir veritabanı başarısız olursa, bekleme veritabanıyla aynı bölgedeki uygulama sunucusu, istekleri veritabanı rolü geçişleri bir kez işleyebilir. Azure Application Gateway ve parça Direktörü, istek ve yanıt gecikmesini ve istek isteklerini uygun şekilde izler.

İstemci sistemi, bir uygulama açısından, isteği istemciye en yakın bölgeye yönlendiren Azure Application Gateway (veya Azure 'daki diğer yük dengeleme teknolojilerine) bir istek yapar. Azure Application Gateway Ayrıca yapışkan oturumları destekler, bu nedenle aynı istemciden gelen tüm istekler aynı uygulama sunucusuna yönlendirilir. Uygulama sunucusu, veri erişim sürücülerinde bağlantı havuzu kullanır. Bu özellik JDBC, ODP.NET, OCı vb. gibi sürücülerde kullanılabilir. Sürücüler, isteğin bir parçası olarak belirtilen parça anahtarlarını algılayabilir. JDBC istemcileri için [Oracle Universal bağlantı havuzu (UCP)](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/jjucp/ucp-database-sharding-overview.html) , Apache Tomcat ve IIS gibi Oracle olmayan uygulama istemcilerinin Oracle parçalaması ile çalışmasını sağlayabilir. 

İlk istek sırasında, uygulama sunucusu, isteğin yönlendirilmesi gereken parça için yönlendirme bilgilerini almak amacıyla bölgesindeki parça Direktörü bağlanır. Bir parçalama anahtarına bağlı olarak, yönetmen uygulama sunucusunu ilgili parçaya yönlendirir. Uygulama sunucusu bu bilgileri bir harita oluşturarak önbelleğe alır ve sonraki istekler için parça Direktörü atlar ve istekleri doğrudan parçalamaya yönlendirir.

## <a name="patching-and-maintenance"></a>Düzeltme eki uygulama ve bakım

Oracle iş yüklerinizi Azure 'a dağıttığınızda, Microsoft tüm ana bilgisayar işletim sistemi düzeyinde düzeltme eki uygulamayı üstlenir. Planlı tüm işletim sistemi düzeyinde bakım, müşterilere bu planlı bakım için izin vermek üzere önceden gönderilir. İki farklı Kullanılabilirlik Alanları iki sunucu aynı anda hiçbir şekilde Düzeltme Eki. VM bakımı ve düzeltme eki uygulama hakkında daha fazla bilgi için bkz. [sanal makinelerin kullanılabilirliğini yönetme](../../../virtual-machines/linux/manage-availability.md) . 

Sanal makine işletim sisteminizin düzeltme eki uygulama, [Azure Otomasyonu](../../../automation/automation-tutorial-update-management.md)kullanılarak otomatikleştirilebilir. Oracle veritabanınızın düzeltme eki uygulama ve sürdürme, kapalı kalma süresini en aza indirmek için [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops) veya [Azure Otomasyonu](../../../automation/automation-tutorial-update-management.md) kullanılarak otomatik ve zamanlanabilir. Oracle veritabanlarınızın bağlamında nasıl kullanılabileceğini anlamak için [sürekli teslim ve mavi/yeşil dağıtımlar](/azure/devops/learn/what-is-continuous-delivery) konusuna bakın.

## <a name="architecture-and-design-considerations"></a>Mimari ve tasarım konuları

- Lisanslama maliyetlerine kaydetmek ve performansı en üst düzeye çıkarmak için Oracle Database sanal makinenizin [kısıtlı çekirdek vCPU 'ları](../../../virtual-machines/windows/constrained-vcpu.md) ile hiper iş parçacıklı [bellek için iyileştirilmiş sanal makine](../../../virtual-machines/windows/sizes-memory.md) kullanmayı düşünün. Performans ve kullanılabilirlik için birden fazla Premium veya ultra disk (yönetilen diskler) kullanın.
- Yönetilen diskler kullanılırken, yeniden başlatmalar üzerinde disk/cihaz adı değişebilir. Takmaların yeniden başlatmalar arasında kalıcı olmasını sağlamak için, ad yerine cihaz UUID 'sini kullanmanız önerilir. Ayrıntılı bilgiye [buradan](../../../virtual-machines/linux/configure-raid.md#add-the-new-file-system-to-etcfstab) ulaşabilirsiniz.
- Bölge içi yüksek kullanılabilirlik elde etmek için kullanılabilirlik alanlarını kullanın.
- Oracle veritabanınız için Ultra diskler (kullanılabilir olduğunda) veya Premium diskler kullanmayı düşünün.
- Oracle Data Guard kullanarak başka bir Azure bölgesinde bir bekleme Oracle veritabanı ayarlamayı düşünün.
- Uygulamanız ve veritabanı katmanınız arasındaki gecikmeyi azaltmak için [yakınlık yerleştirme gruplarını](../../../virtual-machines/linux/co-location.md#proximity-placement-groups) kullanmayı düşünün.
- Yönetim, izleme ve günlüğe kaydetme için [Oracle Enterprise Manager](https://docs.oracle.com/en/enterprise-manager/) 'ı ayarlayın.
- Veritabanınız için kolaylaştırılmış depolama yönetimi için Oracle otomatik depolama yönetimi (ASM) kullanmayı düşünün.
- Herhangi bir kesinti olmadan veritabanınıza düzeltme eki uygulamayı ve güncelleştirme yönetimini yönetmek için [Azure Pipelines](/azure/devops/pipelines/get-started/what-is-azure-pipelines) kullanın.
- [Yeniden deneme deseni](/azure/architecture/patterns/retry), [devre kesici deseni](/azure/architecture/patterns/circuit-breaker)ve uygulamanızın daha dayanıklı olmasını sağlamaya yardımcı olabilecek [bulut tasarım desenleri kılavuzunda](/azure/architecture/patterns/) tanımlanan diğer desenler gibi bulut Yerel desenleri eklemek için uygulama kodunuzu ince ayar.

## <a name="next-steps"></a>Sonraki adımlar

Senaryonuza uygulanan aşağıdaki Oracle başvuru makalelerini gözden geçirin.

- [Oracle Data Guard 'a giriş](https://docs.oracle.com/en/database/oracle/oracle-database/18/sbydb/introduction-to-oracle-data-guard-concepts.html#GUID-5E73667D-4A56-445E-911F-1E99092DD8D7)
- [Oracle Data Guard aracı kavramları](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/dgbkr/oracle-data-guard-broker-concepts.html)
- [Oracle GoldenGate 'i etkin-etkin yüksek kullanılabilirlik için yapılandırma](https://docs.oracle.com/goldengate/1212/gg-winux/GWUAD/wu_bidirectional.htm#GWUAD282)
- [Oracle parçalara genel bakış](https://docs.oracle.com/en/database/oracle/oracle-database/19/shard/sharding-overview.html)
- [Oracle etkin Data Guard her mesafede sıfır veri kaybı](https://www.oracle.com/technetwork/database/availability/farsync-2267608.pdf)
