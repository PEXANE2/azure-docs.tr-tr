---
title: Azure Service Fabric olağanüstü kurtarma
description: Azure Service Fabric, felaketlerle başa çıkabilmek için özellikler sunar. Bu makalede, oluşabilecek felaket türleri ve bunlarla nasıl başa çıkılabilmek açıklanmaktadır.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b29985d40ae3a1bf582099e998e000fed83460f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371656"
---
# <a name="disaster-recovery-in-azure-service-fabric"></a>Azure Hizmet Kumaşında olağanüstü durum kurtarma
Yüksek kullanılabilirlik sağlamanın önemli bir parçası, hizmetlerin farklı türde hatalardan kurtulabilmesini sağlamaktır. Bu, özellikle planlanmamış ve denetiminiz dışında olan hatalar için önemlidir. 

Bu makalede, doğru modellenmedi ve yönetilmezse felaket olabilecek bazı yaygın hata modları açıklanır. Ayrıca, bir felaket zaten olursa yapılacak hafifletmeleri ve yapılacak eylemleri de tartışır. Amaç, planlanmış veya başka bir şekilde hatalar oluştuğunda kesinti veya veri kaybı riskini sınırlamak veya ortadan kaldırmaktır.

## <a name="avoiding-disaster"></a>Felaketten kaçınma
Azure Hizmet Kumaşı'nın temel amacı, hem çevrenizi hem de hizmetlerinizi yaygın hata türlerinin felaket olmayacak şekilde modellemesine yardımcı olmaktır. 

Genel olarak, iki tür büyük felaket/hata senaryosu vardır:
- Donanım ve yazılım hataları
- Operasyonel hatalar

### <a name="hardware-and-software-faults"></a>Donanım ve yazılım hataları
Donanım ve yazılım hataları tahmin edilemez. Hatalardan kurtulmanın en kolay yolu, hizmetin daha fazla kopyasını donanım veya yazılım hata sınırları arasında çalıştırmaktır. 

Örneğin, hizmetiniz yalnızca bir makinede çalışıyorsa, bu makinenin arızalanması bu hizmet için bir felakettir. Bu felaketi önlemenin en basit yolu, hizmetin birden çok makinede çalışmasını sağlamaktır. Bir makinenin arızalanmasının çalışan hizmeti aksatmadığından emin olmak için test de gereklidir. Kapasite planlaması, başka bir yerde değiştirme örneğinin oluşturulabilmesini ve kapasitedeki azalmanın kalan hizmetleri aşırı yüklememesini sağlar. 

Aynı desen ne olursa olsun ne başarısızlığı önlemek için çalışıyoruz çalışır. Örneğin, bir SAN'ın başarısızlığından endişe ediyorsanız, birden çok SN'ye rastlarsınız. Bir sunucu rafının kaybolmasından endişe ediyorsanız, birden çok rafta çalışırsınız. Veri merkezlerinin kaybolmasından endişe ediyorsanız, hizmetiniz birden çok Azure bölgesinde, birden çok Azure Kullanılabilirlik Bölgesinde veya kendi veri merkezlerinizde çalışmalıdır. 

Bir hizmet birden çok fiziksel örneğe (makineler, raflar, veri merkezleri, bölgeler) yayıldığında, yine de bazı eşzamanlı hata türlerine maruz kalırsınız. Ancak belirli bir türdeki tek ve hatta birden fazla hata (örneğin, tek bir sanal makine veya ağ bağlantısı nın arızalanması) otomatik olarak işlenir ve bu nedenle artık bir "felaket" değildir. 

Service Fabric kümeyi genişletmek için mekanizmalar sağlar ve başarısız düğümleri ve hizmetleri geri getirerek işler. Service Fabric, planlanmamış hataların gerçek felaketlere dönüşmesini önlemek için hizmetlerinizin birçok örneğini çalıştırmaya da olanak tanır.

Hataları yayılacak kadar büyük bir dağıtım çalıştırmanın mümkün olmamasının nedenleri olabilir. Örneğin, hata olasılığına göre ödemeye hazır olduğunuzdan daha fazla donanım kaynağı gerekebilir. Dağıtılmış uygulamalarla uğraşırken, coğrafi mesafeler arasında ek iletişim atlamaları veya durum çoğaltma maliyetleri kabul edilemez gecikmeye neden olabilir. Bu çizginin çizildiği her uygulama için farklıdır. 

Özellikle yazılım hataları için, hata ölçeklendirmeye çalıştığınız hizmette olabilir. Bu durumda, hata durumu tüm örnekler arasında ilişkili olduğundan, daha fazla kopya felaketi önlemez.

### <a name="operational-faults"></a>Operasyonel hatalar
Hizmetiniz birçok işten çıkarmayla dünya çapında yayılmış olsa bile, yine de felaket lerle karşılaşabilir. Örneğin, birisi yanlışlıkla hizmet için DNS adını yeniden yapılandırabilir veya tamamen silebilir. 

Örnek olarak, bir devlet Hizmeti Kumaş hizmeti vardı ve birisi yanlışlıkla bu hizmeti sildi varsayalım. Başka bir hafifletme olmadıkça, o hizmet ve sahip olduğu tüm devlet artık yok. Bu tür operasyonel felaketler ("oops"), düzenli planlanmamış hatalardan farklı azaltmalar ve kurtarma adımları gerektirir. 

Bu tür operasyonel hatalardan kaçınmanın en iyi yolu şunlardır:
- Ortama operasyonel erişimi kısıtlayın.
- Kesinlikle tehlikeli operasyonları denetle.
- Otomasyon uyguluyor, manuel veya bant dışı değişiklikleri önleyin ve bunları yürürlüğe koymadan önce ortama karşı belirli değişiklikleri doğrulayın.
- Yıkıcı operasyonların "yumuşak" olduğundan emin olun. Yumuşak işlemler hemen etkili olmaz veya bir zaman dilimi içinde geri alınabilir.

Service Fabric, küme işlemleri için [rol tabanlı](service-fabric-cluster-security-roles.md) erişim denetimi sağlamak gibi operasyonel hataları önlemek için mekanizmalar sağlar. Ancak, bu operasyonel hataların çoğu organizasyonel çabalar ve diğer sistemler gerektirir. Service Fabric, en önemlisi [yedekleme ve devlet hizmetleri için geri yükleme](service-fabric-backuprestoreservice-quickstart-azurecluster.md)olmak üzere operasyonel hatalardan kurtulma mekanizmaları sağlar.

## <a name="managing-failures"></a>Hataları yönetme
Service Fabric'in amacı hataların otomatik olarak yönetilmesidir. Ancak bazı hata türlerini işlemek için hizmetlerin ek kodu olması gerekir. Diğer hata türleri, _not_ güvenlik ve iş sürekliliği nedenleriyle otomatik olarak ele alınmamalıdır. 

### <a name="handling-single-failures"></a>Tek hataları işleme
Tek makineler her türlü nedenden dolayı başarısız olabilir. Bazen güç kaynakları ve ağ donanımı arızaları gibi donanım nedenolur. Diğer hatalar yazılımda vardır. Bunlar işletim sisteminin ve hizmetin kendisinin hatalarını içerir. Service Fabric, ağ sorunları nedeniyle makinenin diğer makinelerden izole edildiği durumlar da dahil olmak üzere bu tür hataları otomatik olarak algılar.

Hizmet türünden bağımsız olarak, kodun tek bir kopyası herhangi bir nedenle başarısız olursa, tek bir örnek çalıştırarak bu hizmet için kapalı kalma süresi ne olur. 

Tek bir hatayla başa çıkmak için yapabileceğiniz en basit şey, hizmetlerinizin varsayılan olarak birden fazla düğümüzerinde çalışmasını sağlamaktır. Devletsiz hizmetler için, `InstanceCount` bunun 1'den büyük olduğundan emin olun. Devlet hizmetleri için minimum öneri `TargetReplicaSetSize` şudur ve `MinReplicaSetSize` her ikisi de 3 olarak ayarlanır. Hizmet kodunuzu daha fazla kopya çalıştırmak, hizmetinizin herhangi bir tek arızayı otomatik olarak kaldırabilmesini sağlar. 

### <a name="handling-coordinated-failures"></a>Eşgüdümlü hataları işleme
Kümedeki eşgüdümlü hatalar, planlanmış veya planlanmamış altyapı hatalarından ve değişikliklerden veya planlanmış yazılım değişikliklerinden kaynaklanabilir. Service Fabric, hata etki alanı olarak eşgüdümlü hatalar yaşayan altyapı *bölgelerini*modeller. Eşgüdümlü yazılım değişikliklerinin karşılaşacağı alanlar *yükseltme etki alanları*olarak modellenir. Hata etki alanları, yükseltme etki alanları ve küme topolojisi hakkında daha fazla bilgi için [bkz.](service-fabric-cluster-resource-manager-cluster-description.md)

Varsayılan olarak, Service Fabric, hizmetlerinizin çalışması gereken yeri planlarken hataları ve yükseltme etki alanlarını dikkate alır. Varsayılan olarak, Service Fabric, hizmetlerinizin birkaç hata ve yükseltme etki alanlarında çalışmasını sağlamaya çalışır, böylece planlanmış veya planlanmamış değişiklikler olursa, hizmetlerinizin kullanılabilir durumda kalması durumunda. 

Örneğin, bir güç kaynağının arızalanmasının raftaki tüm makinelerin aynı anda arızalanmasına neden olduğunu varsayalım. Hizmetin birden çok kopyası çalışırken, hata etki alanı hatasındaki birçok makinenin kaybı, bir hizmet için tek bir hatanın başka bir örneğine dönüşür. Bu nedenle, hata ve yükseltme etki alanlarını yönetmek, hizmetlerinizin yüksek kullanılabilirliğini sağlamak için çok önemlidir. 

Azure'da Hizmet Kumaşı çalıştırırken, hata etki alanları ve yükseltme etki alanları otomatik olarak yönetilir. Diğer ortamlarda, olmayabilir. Kendi kümelerinizi şirket içinde oluşturuyorsanız, hata etki alanı düzeninizi doğru şekilde eşlediğinizden ve planladığınızdan emin olun.

Yükseltme etki alanları, yazılımın aynı anda yükseltilecek alanları modellemek için yararlıdır. Bu nedenle, yükseltme etki alanları da genellikle planlanan yükseltmeleri sırasında yazılımın aşağı alındığı sınırları tanımlar. Hem Service Fabric'in hem de hizmetlerinizin yükseltmeleri aynı modeli izler. Yuvarlanma yükseltmeleri, yükseltme etki alanları ve istenmeyen değişikliklerin kümeyi ve hizmetinizi etkilemesini önlemeye yardımcı olan Service Fabric sistem durumu modeli hakkında daha fazla bilgi için bkz:

 - [Uygulama yükseltme](service-fabric-application-upgrade.md)
 - [Uygulama yükseltme eğitimi](service-fabric-application-upgrade-tutorial.md)
 - [Servis Kumaş sağlık modeli](service-fabric-health-introduction.md)

[Service Fabric Explorer'da](service-fabric-visualizing-your-cluster.md)sağlanan küme haritasını kullanarak kümenizin düzenini görselleştirebilirsiniz:

<center>

![Hizmet Kumaş Explorer'da hata etki adlarına yayılan düğümler][sfx-cluster-map]
</center>

> [!NOTE]
> Hata alanlarını modelleme, yükseltmeleri yuvarlama, hizmet kodunuz ve durumun birçok örneğini çalıştırma, hizmetlerinizin hata ve yükseltme etki alanları arasında çalışmasını sağlamak için yerleşim kuralları ve yerleşik sistem durumu izleme, Service Fabric'in normal operasyonel sorunları ve arızaların felakete dönüşmesini sağlamak için sağladığı özelliklerden yalnızca *bazılarıdır.* 
>

### <a name="handling-simultaneous-hardware-or-software-failures"></a>Eşzamanlı donanım veya yazılım hatalarını işleme
Tek başarısızlıklardan bahsediyorduk. Gördüğünüz gibi, yalnızca kodun (ve durum) daha fazla kopyasını hata ve yükseltme etki alanları arasında çalışan tutarak hem devletsiz hem de devlet hizmetleri için işlemek kolaydır. 

Birden çok eşzamanlı rasgele hatalar da olabilir. Bunlar daha fazla kapalı kalma süresi veya gerçek bir felakete yol açma olasılığı vardır.


#### <a name="stateless-services"></a>Devletsiz hizmetler
Bir devletsiz hizmet için örnek sayısı, çalıştırılması gereken istenen örnek sayısını gösterir. Örneklerden herhangi biri (veya tümü) başarısız olduğunda, Service Fabric diğer düğümlerde otomatik olarak değiştirme örnekleri oluşturarak yanıt verir. Service Fabric, hizmet istenen örnek sayısına geri dönene kadar değiştirmeler oluşturmaya devam eder.

Örneğin, devletsiz hizmetin -1 `InstanceCount` değeri olduğunu varsayalım. Bu değer, kümedeki her düğümüzerinde bir örneğin çalıştırılması gerektiği anlamına gelir. Bu örneklerden bazıları başarısız olursa, Hizmet Kumaşı hizmetin istenilen durumda olmadığını algılar ve eksik oldukları düğümlerde örnekler oluşturmaya çalışır.

#### <a name="stateful-services"></a>Devlet hizmetleri
İki tür devlet hizmeti vardır:
- Israrlı devlet ile stateful.
- Kalıcı olmayan devlet ile stateful. (Durum bellekte depolanır.)

Durumlu bir hizmetin başarısızlığından kurtarma, devlet hizmetinin türüne, hizmetin kaç yinelemeye sahip olduğu ve kaç yinelemenin başarısız olduğu bağlıdır.

Durumlu bir hizmette, gelen veriler yinelemeler (birincil ve etkin ikinciler) arasında çoğaltılır. Yinelemelerin çoğunluğu verileri alırsa, veriler *çoğunluk* taahhüt olarak kabul edilir. (Beş yineleme için, üç bir çoğunluk olacaktır.) Bu, herhangi bir noktada, en son verileri içeren çoğaltmaların en az bir sayı olacağı anlamına gelir. Yinelemeler başarısız olursa (örneğin beşte iki), kurtarıp kurtaramayacağımızı hesaplamak için çoğunluk değerini kullanabiliriz. (Kalan beş yinelemeden üçü hala ayakta olduğundan, en az bir yinelemenin tam veriye sahip olacağı garanti edilir.)

Çoğaltma sayısı başarısız olduğunda, bölüm *çoğunluk kaybı* durumunda olarak ilan edilir. Bir bölümün beş yinelemesi olduğunu varsa, bu da en az üç bölümün tam veriye sahip olduğu anlamına gelir. Çoğaltmaların bir çoğunluk (beşüzerinden üç) başarısız olursa, Hizmet Kumaş kalan yinelemeler (iki beş) bölümü geri yüklemek için yeterli veri olup olmadığını belirleyemez. Service Fabric'in çoğunluk kaybını algıladığı durumlarda, varsayılan davranışı bölüme ek yazmaları önlemek, çoğunluk kaybını bildirmek ve bir çok yinelemenin geri yüklenmesini beklemektir.

Devlet hizmeti için bir felaketin meydana gelip gelmediğini belirlemek ve ardından yönetmek üç aşamadan oluşur:

1. Çoğunluk kaybı olup olmadığını belirlemek.
   
   Bir devlet hizmetinin yinelemelerinin çoğunluğu aynı anda düştüğünde çoğunluk kaybı beyan edilir.
2. Çoğunluk kaybının kalıcı olup olmadığının belirlenmesi.
   
   Çoğu zaman, hatalar geçicidir. İşlemler yeniden başlatılır, düğümler yeniden başlatılır, sanal makineler yeniden başlatılır ve ağ bölümleri iyileşir. Ancak bazen başarısızlıklar kalıcıdır. Hataların kalıcı olup olmaması, devlet hizmetinin durumunu devam ettirmesine veya yalnızca bellekte tutup tutmayacağına bağlıdır: 
   
   - Kalıcı durumu olmayan hizmetler için, bir çoğunluk veya daha fazla yinelemenin başarısızlığı _hemen_ kalıcı çoğunluk kaybına neden olur. Service Fabric, kalıcı olmayan bir hizmette çoğunluk kaybını algıladığında, (potansiyel) veri kaybını beyan ederek hemen 3. Veri kaybına devam etmek mantıklıdır, çünkü Service Fabric kopyaların geri gelmesini beklemenin bir anlamı olmadığını bilir. İyileşseler bile, hizmetin kalıcı olmayan yapısı nedeniyle veriler kaybolur.
   - Devlet kalıcı hizmetleri için, bir çoğunluk veya daha fazla yinelemenin başarısız olması, Service Fabric'in yinelemelerin geri gelmesini beklemesine ve yeterliliği geri getirmesine neden olur. Bu, hizmetin etkilenen bölümüne (veya "çoğaltma kümesine") _yapılan tüm yazılar_ için bir hizmet kesintisine neden olur. Ancak, okumalar yine de azaltılmış tutarlılık garantileri ile mümkün olabilir. Devam (potansiyel) bir veri kaybı olayı olduğundan ve diğer riskleri taşıdığından, Hizmet Kumaşı'nın çoğunluğun geri gelmesini beklediği varsayılan süre *sonsuzdur.* Bu, bir yönetici veri kaybını bildirmek için harekete geçmediği sürece Hizmet Kumaşı'nın bir sonraki adıma geçmeyeceğimanlamına gelir.
3. Verilerin kaybolup kaybolmadığını belirleme ve yedeklemelerden geri alma.

   Çoğunluk kaybı beyan edilirse (otomatik olarak veya idari işlem yoluyla), Service Fabric ve hizmetler verilerin gerçekten kaybolup olmadığını belirlemeye geçer. Bu noktada, Service Fabric diğer kopyaların geri gelmeyeceklerini de bilir. Çoğunluğun kaybının kendi kendine çözülmesini beklemeyi bıraktığımız da alınan karar buydu. Hizmet için en iyi eylem genellikle dondurma küçlükve belirli idari müdahale beklemektir.
   
   Service Fabric `OnDataLossAsync` yöntemi aradığında, bunun nedeni her zaman _şüpheli_ veri kaybıdır. Service Fabric, bu çağrının kalan _en iyi_ yinelemeye teslim edilmesini sağlar. Bu, en fazla ilerlemeyi sağlayan çoğaltmadır. 
   
   Her zaman _şüpheli_ veri kaybı dediğimiz in nedeni, kalan yinelemenin çoğunluk kaybolduğunda birincil durumla aynı duruma sahip olması mümkündür. Ancak, karşılaştırmak için bu durum olmadan, Hizmet Kumaş veya operatörler için emin bilmek için iyi bir yol yoktur.     
   
   Peki yöntemin `OnDataLossAsync` tipik bir uygulama ne yapar?
   1. Tetiklenen uygulama `OnDataLossAsync` günlükleri ve gerekli yönetim uyarılarını kapatır.
   1. Genellikle, uygulama duraklar ve daha fazla karar ve el ile yapılacak eylemleri bekler. Bunun nedeni, yedeklemeler kullanılabilir olsa bile, hazırlanmaları gerekebileceğidir. 
   
      Örneğin, iki farklı hizmet bilgileri koordine ederse, geri yükleme gerçekleştiğinde bu iki hizmetin önem verdiği bilgilerin tutarlı olduğundan emin olmak için bu yedeklemelerin değiştirilmesi gerekebilir. 
   1. Genellikle başka bir telemetri veya hizmet egzoz var. Bu meta veriler diğer hizmetlerde veya günlüklerde bulunabilir. Bu bilgiler, birincil olarak alınan ve işlenen yedeklemede bulunmayan veya bu çoğaltmada çoğaltılan çağrıların olup olmadığını belirlemek için gerektiği gibi kullanılabilir. Bu çağrıların geri yükleme mümkün olabilmesi için yeniden oynan veya yedeklemeye eklenmesi gerekebilir.  
   1. Uygulama, kalan yinelemenin durumunu kullanılabilir yedeklemelerde bulunan durumla karşılaştırır. Service Fabric güvenilir koleksiyonları kullanıyorsanız, bunu yapmak için [araçlar ve süreçler](service-fabric-reliable-services-backup-restore.md) vardır. Amaç, yineleme içindeki durumunun yeterli olup olmadığını görmek ve yedeklemenin ne eksik olabileceğini görmektir.
   1. Karşılaştırma yapıldıktan ve geri yükleme tamamlandıktan sonra (gerekirse), herhangi bir durum değişikliği yapıldıysa hizmet kodu **doğru** döndürülmelidir. Yineleme, bunun eyaletin mevcut en iyi kopyası olduğunu belirlemişse ve hiçbir değişiklik yapmadıysa, kod **yanlış**döndürür. 
   
      **True** değeri, kalan _diğer_ yinelemelerin artık bununla tutarsız olabileceğini gösterir. Bu kopyadan bırakılacak ve yeniden inşa edilecekler. **False** değeri, hiçbir durum değişikliği yapılmadığını gösterir, böylece diğer yinelemeler sahip olduklarını saklayabilir. 

Hizmet yazarlarının, hizmetler üretimde dağıtılmadan önce olası veri kaybı ve hata senaryoları uygulaması kritik öneme sahip. Veri kaybı olasılığına karşı korunmak için, devlet hizmetlerinizin durumunu düzenli aralıklarla coğrafi olarak yedekli bir mağazaya [yedeklemeniz](service-fabric-reliable-services-backup-restore.md) önemlidir. 

Ayrıca, devleti geri yükleme yeteneğine sahip olduğundan da emin olmalısınız. Birçok farklı hizmetin yedekleri farklı zamanlarda alındığından, geri yüklemeden sonra hizmetlerinizin birbirini tutarlı bir şekilde gözden geçirdiğinden emin olmanız gerekir. 

Örneğin, bir hizmetin bir sayı oluşturup depoladığı ve sonra da depolayan başka bir hizmete gönderdiği bir durumu düşünün. Geri yüklemeden sonra, yedeklemesi bu işlemi içermedığından, ikinci hizmetin numarasının olduğunu ancak ilkinin olmadığını keşfedebilirsiniz.

Kalan yinelemelerin bir veri kaybı senaryosunda devam etmek için yetersiz olduğunu ve telemetri veya egzozdan hizmet durumunu yeniden oluşturamadığınızı fark ederseniz, yedeklemelerinizin sıklığı mümkün olan en iyi kurtarma noktası hedefinizi (RPO) belirler. Service Fabric, kalıcı çoğunluk ve yedeklemeden geri yükleme gerektiren veri kaybı da dahil olmak üzere çeşitli hata senaryolarını sınamak için birçok araç sağlar. Bu senaryolar, Hata Analizi Hizmeti tarafından yönetilen Hizmet Kumaşı'ndaki sınanabilirlik araçlarının bir parçası olarak dahildir. Bu araçlar ve desenler hakkında daha fazla bilgi için, [Bkz. Hata Analizi Hizmetine Giriş.](service-fabric-testability-overview.md) 

> [!NOTE]
> Sistem hizmetleri de çoğunluk kaybına uğrayabilir. Etkisi söz konusu hizmete özgüdür. Örneğin, adlandırma hizmetindeki çoğunluk kaybı ad çözümlemesi etkilerken, Failover Manager hizmetindeki çoğunluk kaybı yeni hizmet oluşturmayı ve başarısızları engeller. 
> 
> Service Fabric sistem hizmetleri, devlet yönetimi hizmetleriniz ile aynı modeli izler, ancak bunları çoğunluk kaybından ve olası veri kaybına taşımayı denemenizi önermiyoruz. Bunun yerine, durumunuzu hedefleyen bir çözüm bulmak için [destek aramanızı](service-fabric-support.md) öneririz. Genellikle aşağı kopyaları dönene kadar beklemek tercih edilir.
>

#### <a name="troubleshooting-quorum-loss"></a>Sorun giderme çoğunluk kaybı

Geçici bir hata nedeniyle yinelemeler aralıklı olarak kapanabilir. Service Fabric onları getirmek için çalışır gibi bir süre bekleyin. Yinelemeler beklenenden daha uzun bir süre boyunca kapatıldıysa, aşağıdaki sorun giderme eylemlerini izleyin:
- Kopyalar çöküyor olabilir. Yineleme düzeyindeki sistem durumu raporlarını ve uygulama günlüklerinizi denetleyin. Kilitlenme dökümleri toplamak ve kurtarmak için gerekli eylemleri almak.
- Çoğaltma işlemi yanıt vermemiş olabilir. Bunu doğrulamak için uygulama günlüklerinizi inceleyin. İşlem dökümlerini toplayın ve yanıt vermeyen işlemi durdurun. Service Fabric değiştirme işlemi oluşturur ve çoğaltmayı geri getirmeye çalışır.
- Kopyaları barındıran düğümler aşağı olabilir. Düğümleri yukarı getirmek için altta yatan sanal makineyi yeniden başlatın.

Bazen, yinelemeleri kurtarmak mümkün olmayabilir. Örneğin, sürücüler başarısız oldu veya makineler fiziksel olarak yanıt vermiyor. Bu gibi durumlarda, Service Fabric çoğaltma kurtarma beklemek değil söylenmesi gerekir.

Hizmeti *not* çevrimiçi duruma getirmek için olası veri kaybı kabul edilemezse bu yöntemleri kullanmayın. Bu durumda, tüm çabalar fiziksel makineleri kurtarma doğru yapılmalıdır.

Aşağıdaki eylemler veri kaybına neden olabilir. Onları takip etmeden önce kontrol edin.
   
> [!NOTE]
> Belirli bölümlere karşı hedeflenen bir şekilde dışında bu yöntemleri kullanmak hiçbir zaman güvenli _değildir._ 
>

- Veya `Repair-ServiceFabricPartition -PartitionId` `System.Fabric.FabricClient.ClusterManagementClient.RecoverPartitionAsync(Guid partitionId)` API'yi kullanın. Bu API, bölümün kimliğinin çoğunluk kaybından çıkıp olası veri kaybına taşınmasını sağlar.
- Kümeniz, hizmetlerin çoğunluk kaybı durumuna geçmesine neden olan sık sık hatalarla _karşılaşırsa_ve olası veri kaybı kabul edilebilirse, uygun bir [QuorumLossWaitDuration](https://docs.microsoft.com/powershell/module/servicefabric/update-servicefabricservice?view=azureservicefabricps) değeri belirtmek hizmetinizin otomatik olarak kurtarılmasına yardımcı olabilir. Hizmet Kumaşı kurtarma `QuorumLossWaitDuration` gerçekleştirmeden önce sağlanan değeri (varsayılan sonsuzdur) bekler. Beklenmeyen veri kayıplarına neden olabileceğinden bu yöntemi *önermiyoruz.*

## <a name="availability-of-the-service-fabric-cluster"></a>Service Fabric kümesinin kullanılabilirliği
Genel olarak, Service Fabric kümesi tek bir hata noktası olmayan yüksek oranda dağıtılmış bir ortamdır. Herhangi bir düğümün arızası küme için kullanılabilirlik veya güvenilirlik sorunlarına neden olmaz, çünkü Service Fabric sistem hizmetleri daha önce sağlanan aynı yönergeleri izleyin. Diğer bir süre, varsayılan olarak üç veya daha fazla yinelemeyle ve tüm düğümlerde devletsiz çalışan sistem hizmetleriyle çalışırlar. 

Altta yatan Service Fabric ağ ve hata algılama katmanları tamamen dağıtılır. Çoğu sistem hizmeti kümedeki meta verilerden yeniden oluşturulabilir veya durumlarını başka yerlerden nasıl yeniden eşitleeceğini bilir. Sistem hizmetleri daha önce açıklananlar gibi çoğunluk kaybı durumlarına girerse kümenin kullanılabilirliği tehlikeye girebilirsiniz. Bu gibi durumlarda, küme üzerinde belirli işlemleri gerçekleştiremeyebilirsiniz (yükseltme başlatma veya yeni hizmetler dağıtma gibi), ancak kümenin kendisi hala açık. 

Çalışan kümedeki hizmetler, sistem hizmetlerine yazma gerektirmedikçe bu koşullarda çalışmaya devam eder. Örneğin, Failover Manager çoğunluk kaybı durumunda, tüm hizmetler çalışmaya devam edecektir. Ancak başarısız olan tüm hizmetler otomatik olarak yeniden başlatılamaz, çünkü bu Failover Manager'ın katılımını gerektirir. 

### <a name="failures-of-a-datacenter-or-an-azure-region"></a>Veri merkezinin veya Azure bölgesinin hataları
Nadir durumlarda, fiziksel bir veri merkezi güç veya ağ bağlantısı kaybından geçici olarak kullanılamaz hale gelebilir. Bu gibi durumlarda, hizmet kumaş kümeleriniz ve bu veri merkezi veya Azure bölgesindeki hizmetler kullanılamaz. Ancak, _verileriniz korunur._ 

Azure'da çalışan kümeler için, Azure durum [sayfasındaki][azure-status-dashboard]kesintilerle ilgili güncelleştirmeleri görüntüleyebilirsiniz. Fiziksel bir veri merkezinin kısmen veya tamamen yok edilmesi olasılığı oldukça düşük bir durumda, orada barındırılan Herhangi bir Hizmet Kumaşı kümeleri veya içlerindeki hizmetler kaybolabilir. Bu kayıp, veri merkezi veya bölge dışında yedeklenmemiş herhangi bir durumu içerir.

Tek bir veri merkezinin veya bölgenin kalıcı veya sürekli başarısızlığından kurtulmanın iki farklı stratejileri vardır: 

- Birden çok bu tür bölgelerde ayrı Hizmet Kumaş kümeleri çalıştırın ve bu ortamlar arasında başarısız olmak ve geri tepme için bazı mekanizmalar kullanın. Çoklu küme etkin/etkin veya etkin/pasif model bu tür ek yönetim ve işlem kodu gerektirir. Bu model, bir veri merkezinde veya bölgedeki hizmetlerden gelen yedeklemelerin koordinasyonunu da gerektirir, böylece bir veri merkezi başarısız olduğunda diğer veri merkezlerinde veya bölgelerde kullanılabilir olurlar. 
- Birden çok veri merkezine veya bölgeye yayılan tek bir Hizmet Dokusu kümesi çalıştırın. Bu strateji için desteklenen minimum yapılandırma üç veri merkezi veya bölgedir. Önerilen bölge veya veri merkezi sayısı beştir. 
  
  Bu model daha karmaşık bir küme topolojisi gerektirir. Ancak, yararı, bir veri merkezi veya bölgenin başarısızlığı normal bir hata içine bir felaket dönüştürülür olmasıdır. Bu hatalar, tek bir bölge içindeki kümeler için çalışan mekanizmalar tarafından işlenebilir. Hata etki alanları, yükseltme etki alanları ve Hizmet Dokusu yerleşim kuralları, iş yüklerinin normal hataları tolere edecek şekilde dağıtılmasını sağlar. 
  
  Bu tür kümedeki hizmetlerin çalıştırılabilen ilkeler hakkında daha fazla bilgi [için, Hizmet Kumaşı hizmetleri için Yerleşim ilkelerine](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)bakın.

### <a name="random-failures-that-lead-to-cluster-failures"></a>Küme hatalarına yol açan rasgele hatalar
Hizmet Kumaş tohum *düğümleri*kavramına sahiptir. Bunlar, alttaki kümenin kullanılabilirliğini koruyan düğümlerdir. 

Tohum düğümleri, kümenin diğer düğümlerle kiralama lar kurarak ve belirli türde hatalar sırasında tiebreaker olarak hizmet vererek ayakta kalmasını sağlamaya yardımcı olur. Rasgele hatalar kümedeki tohum düğümlerinin çoğunluğunu kaldırırsa ve hızlı bir şekilde geri getirilmezse, kümeniz otomatik olarak kapanır. Küme daha sonra başarısız olur. 

Azure'da, Service Fabric Kaynak Sağlayıcısı Service Fabric küme yapılandırmalarını yönetir. Varsayılan olarak, Kaynak Sağlayıcı *ana düğüm türü*için hata ve yükseltme etki alanları arasında tohum düğümleri dağıtır. Birincil düğüm türü Gümüş veya Altın dayanıklılık olarak işaretlenmişse, bir tohum düğümünüzü kaldırdığınızda (birincil düğüm türünü ölçeklendirerek veya el ile kaldırarak), küme birincil düğüm türünün kullanılabilir kapasitesinden başka bir tohum dışı düğümü tanıtmaya çalışır. Küme güvenilirlik düzeyiniz birincil düğüm türünüz için gerekenden daha az kullanılabilir kapasiteye sahipseniz, bu girişim başarısız olur.

Hem bağımsız Hizmet Kumaş kümelerinde hem de Azure'da, birincil düğüm türü tohumları çalıştıran dır. Birincil düğüm türünü tanımlarken, Service Fabric, her sistem hizmetinin en fazla dokuz tohum düğümü ve yedi kopyasını oluşturarak sağlanan düğüm sayısından otomatik olarak yararlanır. Rasgele hatalar kümesi bu yinelemelerin çoğunluğunu aynı anda alırsa, sistem hizmetleri çoğunluk kaybına girer. Tohum düğümlerinin çoğunluğu kaybolursa, küme kısa bir süre sonra kapanır.

## <a name="next-steps"></a>Sonraki adımlar
- [Test edilebilirlik çerçevesini](service-fabric-testability-overview.md)kullanarak çeşitli hataları nasıl simüle edebilirsiniz öğrenin.
- Diğer olağanüstü durum kurtarma ve yüksek kullanılabilirlik kaynaklarını okuyun. Microsoft bu konularda büyük miktarda kılavuz yayımlamıştır. Bu kaynaklardan bazıları diğer ürünlerde kullanılmak üzere belirli tekniklere atıfta bulunsa da, Hizmet Kumaşı bağlamında uygulayabileceğiniz birçok genel en iyi uygulama içerir:
  - [Kullanılabilirlik denetim listesi](/azure/architecture/checklist/resiliency-per-service)
  - [Olağanüstü durum kurtarma tatbikatı gerçekleştirme](../sql-database/sql-database-disaster-recovery-drills.md)
  - [Azure uygulamaları için olağanüstü durum kurtarma ve yüksek kullanılabilirlik][dr-ha-guide]
- Service [Fabric destek seçenekleri](service-fabric-support.md)hakkında bilgi edinin.


<!-- External links -->

[repair-partition-ps]: https://msdn.microsoft.com/library/mt163522.aspx
[azure-status-dashboard]:https://azure.microsoft.com/status/
[azure-regions]: https://azure.microsoft.com/regions/
[dr-ha-guide]: https://msdn.microsoft.com/library/azure/dn251004.aspx


<!-- Images -->

[sfx-cluster-map]: ./media/service-fabric-disaster-recovery/sfx-clustermap.png
