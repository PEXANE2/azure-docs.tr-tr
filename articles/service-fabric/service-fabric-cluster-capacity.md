---
title: Hizmet Kumaşı küme kapasitesinin planlanması
description: Hizmet Kumaş küme kapasite planlama hususlar. Düğüm tipleri, Operasyonlar, Dayanıklılık ve Güvenilirlik katmanları
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: pepogors
ms.openlocfilehash: 6e60fc10dd7e0eec24de4a089d09d914624dcfbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258921"
---
# <a name="service-fabric-cluster-capacity-planning-considerations"></a>Hizmet Kumaş küme kapasite planlama hususları
Herhangi bir üretim dağıtımı için kapsite planlaması önemli bir adımdır. Bu işlemin bir parçası olarak göz önünde bulundurmanız gereken öğelerden bazıları şunlardır.

* Kümenizin başlaması gereken düğüm türlerinin sayısı
* Her düğüm türünün özellikleri (boyut, birincil, internet eki, VM sayısı, vb.)
* Kümenin güvenilirlik ve dayanıklılık özellikleri

> [!NOTE]
> Planlama sırasında tüm **İzin Verilmeyen** yükseltme ilkesi değerlerini en az şekilde gözden geçirmelisiniz. Bu, değerleri uygun şekilde ayarlamanızı sağlamak ve değiştirilemez sistem yapılandırma ayarları nedeniyle daha sonra kümenizin yanmasını azaltmak içindir. 
> 

Bu öğelerin her birini kısaca gözden geçirelim.

## <a name="the-number-of-node-types-your-cluster-needs-to-start-out-with"></a>Kümenizin başlaması gereken düğüm türlerinin sayısı
İlk olarak, oluşturduğunuz kümenin ne için kullanılacağını bulmanız gerekir.  Bu kümeye ne tür uygulamalar dağıtmayı planlıyorsunuz? Kümenin amacı hakkında net değilseniz, büyük olasılıkla kapasite planlama işlemine girmeye henüz hazır değilsiniz.

Kümenizin başlaması gereken düğüm türlerinin sayısını belirleyin.  Her düğüm türü sanal makine ölçeği kümesine eşlenir. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Yani düğüm türlerinin sayısının kararı aslında aşağıdaki hususlaraşağı gelir:

* Uygulamanızın birden çok hizmeti var mı ve bunlardan herhangi birinin herkese açık veya internetle karşı karşıya olması gerekiyor mu? Tipik uygulamalar, istemciden giriş alan bir ön uç ağ geçidi hizmeti ve ön uç hizmetleriyle iletişim sağlayan bir veya daha fazla arka uç hizmeti içerir. Yani bu durumda, en az iki düğüm türüne sahip olma sonunda.
* Hizmetleriniz (uygulamanızı oluşturan) daha fazla RAM veya daha yüksek CPU döngüleri gibi farklı altyapı gereksinimlerine sahip mi? Örneğin, dağıtmak istediğiniz uygulamanın bir ön uç hizmeti ve bir arka uç hizmeti içerdiğini varsayalım. Ön uç hizmeti, bağlantı noktaları internete açık olan daha küçük VM'lerde (D2 gibi VM boyutları) çalıştırılabilir.  Ancak arka uç hizmeti, hesaplama yoğundur ve internete dönük olmayan daha büyük VM'lerde (D4, D6, D15 gibi VM boyutlarında) çalışması gerekir.
  
  Bu örnekte, tüm hizmetleri tek bir düğüm türüne koymaya karar verebilirsiniz, ancak bunları iki düğüm türüne sahip bir kümeye yerleştirmenizi tavsiye ettik.  Bu, her düğüm türünün internet bağlantısı veya VM boyutu gibi farklı özelliklere sahip olmasını sağlar. VM sayısı da bağımsız olarak ölçeklendirilebilir.  
* Çünkü geleceği tahmin edemezsiniz, bildiğiniz gerçeklerle gidin ve uygulamalarınızın başlaması gereken düğüm türlerinin sayısını seçin. Düğüm türlerini daha sonra her zaman ekleyebilir veya kaldırabilirsiniz. Hizmet Kumaşı kümesinin en az bir düğüm türü olmalıdır.

## <a name="the-properties-of-each-node-type"></a>Her düğüm türünün özellikleri
**Düğüm türü** Bulut Hizmetleri'ndeki rollere eşdeğer olarak görülebilir. Düğüm türleri VM boyutlarını, VM sayısını ve özelliklerini tanımlar. Hizmet Kumaşı küme eşlemlerinde tanımlanan her düğüm türü [sanal makine ölçeği kümesine ayarlanır.](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)  
Her düğüm türü ayrı bir ölçek kümesidir ve bağımsız olarak yukarı veya aşağı ölçeklenebilir, farklı bağlantı noktaları kümeleri açık olabilir ve farklı kapasite ölçütlerine sahiptir. Düğüm türleri ve sanal makine ölçek kümeleri arasındaki ilişkiler hakkında daha fazla bilgi için, rdp'nin örneklerden birine nasıl girileceği, yeni bağlantı noktalarının nasıl açılacağını ve benzeri bilgiler için [Service Fabric küme düğümü türlerine](service-fabric-cluster-nodetypes.md)bakın.

Service Fabric kümesi birden fazla düğüm türünden oluşabilir. Bu durumda, küme bir birincil düğüm türü ve bir veya daha fazla birincil olmayan düğüm türünden oluşur.

Tek bir düğüm türü, SF uygulamaları için ayarlanan sanal makine ölçeği başına 100 düğümün ötesine güvenilir bir şekilde ölçeklenemez; güvenilir bir şekilde 100'den fazla düğüm elde etmek, ek sanal makine ölçek setleri eklemenizi gerektirir.

### <a name="primary-node-type"></a>Birincil düğüm türü

Servis Kumaşı sistem hizmetleri (örneğin, Cluster Manager hizmeti veya Resim Mağazası hizmeti) birincil düğüm türüne yerleştirilir. 

![İki Düğüm Türü olan bir kümenin ekran görüntüsü][SystemServices]

* Birincil düğüm türü için **minimum VM boyutu** seçtiğiniz **dayanıklılık katmanına** göre belirlenir. Varsayılan dayanıklılık katmanı Bronz'dur. Daha fazla ayrıntı için [kümenin dayanıklılık özelliklerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) bakın.  
* Birincil düğüm türü için **minimum VM sayısı** seçtiğiniz **güvenilirlik katmanına** göre belirlenir. Varsayılan güvenilirlik katmanı Gümüş'tir. Daha fazla ayrıntı için [kümenin güvenilirlik özelliklerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) bakın.  

Azure Kaynak Yöneticisi şablonundan, birincil düğüm türü `isPrimary` düğüm türü [tanımı](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/clusters#nodetypedescription-object)altında öznitelik ile yapılandırılır.

### <a name="non-primary-node-type"></a>Birincil olmayan düğüm türü

Birden çok düğüm türü olan bir kümede bir birincil düğüm türü vardır ve geri kalanı birincil değildir.

* Birincil olmayan düğüm türleri için **minimum VM boyutu** seçtiğiniz **dayanıklılık katmanına** göre belirlenir. Varsayılan dayanıklılık katmanı Bronz'dur. Daha fazla bilgi için [kümenin dayanıklılık özelliklerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)bakın.  
* Birincil olmayan düğüm türleri için **en az VM sayısı** biridir. Ancak, bu düğüm türünde çalıştırmak istediğiniz uygulama/hizmet yinelemelerinin sayısına göre bu sayıyı seçmeniz gerekir. Kümeyi dağıttıktan sonra düğüm türündeki VM sayısı artırılabilir.

## <a name="the-durability-characteristics-of-the-cluster"></a>Kümenin dayanıklılık özellikleri
Dayanıklılık katmanı, vm'lerinizin temel Azure altyapısıyla sahip olduğu ayrıcalıkları sisteme göstermek için kullanılır. Birincil düğüm türünde, bu ayrıcalık Service Fabric'in sistem hizmetleri ve devlet hizmetleri için yeterli çoğunluk gereksinimlerini etkileyen vm düzeyindeki altyapı isteğini (VM yeniden başlatma, VM yeniden başlatma veya VM geçişi gibi) duraklatmasına olanak tanır. Birincil olmayan düğüm türlerinde bu ayrıcalık, Service Fabric'in, devlet hizmetlerinizin yeterli liğini etkileyen VM düzeyindeki altyapı isteklerini (VM yeniden başlatma, VM yeniden görüntü ve VM geçişi gibi) duraklatmasına olanak tanır.

| Dayanıklılık katmanı  | Gerekli minimum VM sayısı | Desteklenen VM SUs'lar                                                                  | Sanal makine ölçek setinizde yaptığınız güncellemeler                               | Azure tarafından başlatılan güncelleştirmeler ve bakım                                                              | 
| ---------------- |  ----------------------------  | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Gold             | 5                              | Tek bir müşteriye adanmış tam düğümLÜ SK'ler (örneğin, L32s, GS5, G5, DS15_v2, D15_v2) | Service Fabric kümesi tarafından onaylanana kadar gecikebilir | Yinelemelerin önceki hatalardan kurtarılması için ek süre tanımak için UD başına 2 saat duraklatılabilir |
| Silver           | 5                              | En az 50 GB yerel SSD içeren tek çekirdekli veya üstü VM'ler                      | Service Fabric kümesi tarafından onaylanana kadar gecikebilir | Önemli bir süre geciktirilemez                                                    |
| Bronz           | 1                              | En az 50 GB yerel SSD içeren VM'ler                                              | Service Fabric kümesi tarafından geciktirilmeyecek           | Önemli bir süre geciktirilemez                                                    |

> [!WARNING]
> Bronz dayanıklılık ile çalışan düğüm türleri _hiçbir ayrıcalık_elde. Bu, durum daki iş yüklerinizi etkileyen altyapı işlerinin durdurulmayacağı veya geciktirilmeyeceğini ve bu nedenle iş yüklerinizi etkileyebileceği anlamına gelir. Yalnızca durum disyük lerini çalıştıran düğüm türleri için yalnızca Bronz kullanın. Üretim iş yükleri için Gümüş veya üzeri çalıştırma önerilir. 
> 
> Herhangi bir dayanıklılık düzeyi ne olursa olsun, VM Ölçeği Kümesi'ndeki [deallocation](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/deallocate) işlemi kümeyi yok eder

**Gümüş veya Altın dayanıklılık düzeylerini kullanmanın avantajları**
 
- Bir ölçek lendirme işleminde gerekli adımların sayısını azaltır (diğer bir deyişle, düğüm devre dışı bırakma ve Remove-ServiceFabricNodeState otomatik olarak adlandırılır).
- Müşteri tarafından başlatılan yerinde VM SKU değiştirme işlemi veya Azure altyapı işlemleri nedeniyle veri kaybı riskini azaltır.

**Gümüş veya Altın dayanıklılık düzeylerini kullanmanın dezavantajları**
 
- Sanal makine ölçeği kümenize ve diğer ilgili Azure kaynaklarına yapılan dağıtımlar gecikebilir, zaman dolabilir veya kümenizdeki veya altyapı düzeyindeki sorunlar nedeniyle tamamen engellenebilir. 
- Azure altyapı işlemleri sırasında otomatik düğüm devre dışı bırakmalar nedeniyle [yineleme yaşam döngüsü olaylarının](service-fabric-reliable-services-lifecycle.md) (örneğin, birincil değiştirmeler) sayısını artırır.
- Azure platformu yazılım güncelleştirmeleri veya donanım bakım etkinlikleri gerçekleşirken düğümleri uzun süre hizmet dışı alır. Bu etkinlikler sırasında durum Devre Dışı/Devre Dışı bırakılan düğümler görebilirsiniz. Bu, kümenizin kapasitesini geçici olarak azaltır, ancak kümenizin veya uygulamalarınızın kullanılabilirliğini etkilememelidir.

### <a name="recommendations-for-when-to-use-silver-or-gold-durability-levels"></a>Gümüş veya Altın dayanıklılık seviyelerinin ne zaman kullanılacağına ilişkin öneriler

Sık sık ölçeklendirmeyi beklediğiniz devlet hizmetlerini barındıran tüm düğüm türleri için Gümüş veya Altın dayanıklılığını kullanın (VM örnek sayısını azaltın) ve dağıtım işlemlerinin geciktirilmesini ve bu ölçeklendirmenin basitleştirilmesi lehine kapasitenin azaltılmasını tercih edersiniz Işlem. Ölçeklendirme senaryoları (VM örnekleri ekleme) dayanıklılık katmanı seçtiğiniz içine oynamaz, yalnızca ölçeklendirme yapar.

### <a name="changing-durability-levels"></a>Dayanıklılık düzeylerini değiştirme
- Dayanıklılık düzeyi Gümüş veya Altın olan düğüm tipleri Bronza indirgenemez.
- Bronzdan Gümüş veya Altın'a yükseltme birkaç saat sürebilir.
- Dayanıklılık düzeyini değiştirirken, hem sanal makine ölçeği küme kaynağınızdaki Service Fabric uzantı yapılandırmasında hem de Service Fabric küme kaynağınızdaki düğüm türü tanımında güncelleştirdiğinizden emin olun. Bu değerler eşleşmelidir.

### <a name="operational-recommendations-for-the-node-type-that-you-have-set-to-silver-or-gold-durability-level"></a>Gümüş veya altın dayanıklılık düzeyine ayarladığınız düğüm türü için operasyonel öneriler.

- Kümenizi ve uygulamalarınızı her zaman sağlıklı tutun ve uygulamaların tüm [Hizmet çoğaltma yaşam döngüsü olaylarına](service-fabric-reliable-services-lifecycle.md) (yapıdaki çoğaltma gibi) zamanında yanıt verdiğinden emin olun.
- VM SKU değişikliği yapmak için daha güvenli yollar benimsemek (Yukarı/aşağı ölçeklendirin): Sanal makine ölçeği kümesinin VM SKU'yu değiştirmek için birkaç adım ve dikkat edilmesi gereken ler gerektirir. Sık karşılaşılan sorunları önlemek için izleyebileceğiniz işlem aşağıda veda edebilirsiniz.
    - **Birincil olmayan düğüm türleri için:** Yeni sanal makine ölçeği kümesi oluşturmanız, hizmet yerleşimi kısıtlamasını yeni sanal makine ölçeği kümesi/düğüm türünü içerecek şekilde değiştirmeniz ve ardından eski sanal makine ölçeği küme örnek sayısını sıfıra, bir düğüme düşürmeniz önerilir (bu, düğümlerin kaldırılmasının kümenin güvenilirliğini etkilemediğinden emin olmak içindir).
    - **Birincil düğüm türü için:** Seçtiğiniz VM SKU kapasitedeyse ve daha büyük bir VM SKU'ya geçiş yapmak istiyorsanız, [birincil düğüm türü için dikey ölçekleme](https://docs.microsoft.com/azure/service-fabric/service-fabric-scale-up-node-type)kılavuzumuzu izleyin. 

- Altın veya Gümüş etkin dayanıklılık düzeyine sahip herhangi bir sanal makine ölçeği kümesi için en az beş düğüm sayısını koruyun.
- Dayanıklılık düzeyine sahip her sanal makine ölçeği Gümüş veya Altın, Service Fabric kümesinde kendi düğüm türüne eşlenmelidir. Birden çok sanal makine ölçeği kümesini tek bir düğüm türüyle eşleme, Hizmet Dokusu kümesi ile Azure altyapısı arasındaki koordinasyonun düzgün çalışmasını engeller.
- Rastgele VM örneklerini silmeyin, her zaman sanal makine ölçeği küçültme ölçeği ni kullanın. Rasgele VM örneklerinin silinmesi, UD ve FD'ye yayılmış VM örneğinde dengesizlikoluşturma potansiyeline sahiptir. Bu dengesizlik, hizmet örnekleri/Hizmet yinelemeleri arasında dengeyi düzgün bir şekilde yükleme sistemlerinin yeteneğini olumsuz etkileyebilir.
- Otomatik Ölçek kullanıyorsanız, bu ölçekteki (VM örneklerinin kaldırılması) kuralları ayarlayın, aynı anda yalnızca bir düğüm yapılır. Aynı anda birden fazla örneği ölçekleme güvenli değildir.
- Birincil düğüm türünde VM'leri silerseniz veya ayırıyorsanız, ayrılan VM'lerin sayısını hiçbir zaman güvenilirlik katmanının gerektirdiğinin altına düşürmemelisiniz. Bu işlemler, dayanıklılık düzeyi Gümüş veya Altın olan bir ölçekte süresiz olarak engellenir.

## <a name="the-reliability-characteristics-of-the-cluster"></a>Kümenin güvenilirlik özellikleri
Güvenilirlik katmanı, bu kümede çalıştırmak istediğiniz sistem hizmetlerinin yineleme sayısını birincil düğüm türünde ayarlamak için kullanılır. Çoğaltma sayısı ne kadar çoksa, sistem hizmetleri kümenizde o kadar güvenilir dir.  

Güvenilirlik katmanı aşağıdaki değerleri alabilir:

* Platin - Sistem hizmetlerini dokuz hedef çoğaltma kümesi sayısıyla çalıştırın
* Altın - Sistem hizmetlerini yedi hedef çoğaltma set sayısıyla çalıştırın
* Gümüş - Beş bir hedef çoğaltma kümesi sayısı ile Sistem hizmetleri çalıştırın 
* Bronz - Üç bir hedef çoğaltma kümesi sayısı ile Sistem hizmetleri çalıştırın

> [!NOTE]
> Seçtiğiniz güvenilirlik katmanı, birincil düğüm türünizin sahip olması gereken en az düğüm sayısını belirler. 
> 
> 

### <a name="recommendations-for-the-reliability-tier"></a>Güvenilirlik katmanı için öneriler

Kümenizin boyutunu artırdığınızda veya azalttığınızda (tüm düğüm türlerindeki VM örneklerinin toplamı), kümenizin güvenilirliğini bir katmandan diğerine güncelleştirmeniz gerekir. Bunu yapmak, sistem hizmetleri yineleme kümesi sayısını değiştirmek için gereken küme yükseltmelerini tetikler. Düğüm ekleme gibi kümede başka değişiklikler yapmadan önce yükseltmenin tamamlanmasını bekleyin.  Service Fabric Explorer'da yükseltmenin ilerlemesini izleyebilir veya [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps) çalıştırabilirsiniz

Güvenilirlik katmanını seçme önerisi aşağıda veda edinebilirsiniz.  Tohum düğümlerinin sayısı da bir güvenilirlik katmanı için en az düğüm sayısına ayarlanır.  Örneğin, Gold güvenilirliği olan bir küme için 7 tohum düğümü vardır.

| **Küme düğümlerinin sayısı** | **Güvenilirlik Katmanı** |
| --- | --- |
| 1 |Güvenilirlik Katmanı parametresini belirtmeyin, sistem bunu hesaplar |
| 3 |Bronz |
| 5 veya 6|Silver |
| 7 veya 8 |Gold |
| 9 ve yukarı |Platin |

## <a name="primary-node-type---capacity-guidance"></a>Birincil düğüm türü - kapasite kılavuzu

Birincil düğüm türü kapasitesini planlama kılavuzu aşağıda veda edin:

- **Azure'da herhangi bir üretim iş yükünü çalıştırmak için VM örneklerinin sayısı:** En az Birincil Düğüm türü boyutu 5 ve Gümüş Güvenilirlik Katmanı belirtmeniz gerekir.  
- **Azure'da test iş yüklerini çalıştırmak için VM örneklerinin sayısı** En az birincil düğüm türü boyutu 1 veya 3 belirtebilirsiniz. Tek düğüm kümesi, özel bir yapılandırma ile çalışır ve böylece, bu kümenin dışına ölçek desteklenmez. Tek düğüm kümesi, hiçbir güvenilirlik ve böylece Kaynak Yöneticisi şablonu vardır, bu yapılandırma (yapılandırma değeri ayarlama yeterli değildir) belirtmeniz gerekir. Portal üzerinden ayarlanan tek düğüm kümesini ayarlarsanız, yapılandırma otomatik olarak halledilir. Üretim iş yüklerini çalıştırmak için bir ve üç düğüm kümeleri desteklenmez. 
- **VM SKU:** Birincil düğüm türü sistem hizmetlerinin çalıştığı yerdir, bu nedenle seçtiğiniz VM SKU kümeye yerleştirmeyi planladığınız genel tepe yükünü dikkate almalıdır. Burada ne demek istediğimi göstermek için bir benzetme - Sizin "Akciğerler" olarak birincil düğüm türü düşünün, bu ne beyninize oksijen sağlar, ve böylece beyin yeterli oksijen almaz, vücudunuzun acı çekiyor. 

Kümede çalıştırmayı planladığınız iş yüküne göre bir kümenin kapasite gereksinimleri belirlendiğinden, size belirli iş yükünüz için nitel kılavuz sağlayamayız, ancak burada başlamanıza yardımcı olacak geniş kılavuz

Üretim iş yükleri için: 

- Kümelerinizi birincil NodeType'ınızı sistem hizmetlerine ayırmanız ve uygulamanızı ikincil Düğüm Türlerine dağıtmak için yerleşim kısıtlamaları kullanmanız önerilir.
- Önerilen VM SKU standart D2_V2 veya eşdeğeri dir ve en az 50 GB yerel SSD ile eşdeğerdir.
- Desteklenen minimum kullanım VM SKU, en az 50 GB yerel SSD ile Standard_D2_V3 veya Standart D1_V2 veya eşdeğeridir. 
- Tavsiyemiz en az 50 GB'dır. İş yükleriniz için, özellikle Windows kapsayıcılarını çalıştırırken daha büyük diskler gereklidir. 
- Standart A0 gibi kısmi çekirdekli VM STU'lar üretim iş yükleri için desteklenmez.
- VM STU serisi performans nedenleriyle üretim iş yükleri için desteklenmez.
- Düşük öncelikli VM'ler desteklenmez.

> [!WARNING]
> Çalışan kümedeki birincil düğüm VM SKU boyutunu değiştirmek bir ölçeklendirme işlemidir ve Sanal Makine Ölçeği'nde belgelenmiş belgeleri [ölçeklendirin.](virtual-machine-scale-set-scale-node-type-scale-out.md)

## <a name="non-primary-node-type---capacity-guidance-for-stateful-workloads"></a>Birincil olmayan düğüm türü - durumlu iş yükleri için kapasite kılavuzu

Bu kılavuz, birincil olmayan düğüm türünde çalıştırdığınız Hizmet [kumaşı güvenilir koleksiyonları veya güvenilir Aktörleri](service-fabric-choose-framework.md) kullanarak devlete uygun İş Yükleri içindir.

**VM örneklerinin sayısı:** Durum dolu üretim iş yükleri için, bunları en az 5 ve hedef yineleme sayısıyla çalıştırmanız önerilir. Bu, sabit durumda her hata etki alanında bir yineleme (çoğaltma kümesinden) ve yükseltme etki alanı ile sona erer anlamına gelir. Birincil düğüm türü için tüm güvenilirlik katmanı kavramı, sistem hizmetleri için bu ayarı belirtmenin bir yoludur. Yani aynı husus sizin devlet hizmetleri için de geçerlidir.

Bu nedenle, üretim iş yükleri için önerilen en az Birincil Düğüm olmayan tür boyutu, içinde durum sallandırıcı iş yükleri çalıştırıyorsanız 5'tir.

**VM SKU:** Bu, uygulama hizmetlerinizin çalıştığı düğüm türüdür, bu nedenle seçtiğiniz VM SKU, her düğüme yerleştirmeyi planladığınız en yüksek yükü dikkate almalıdır. Düğüm türünün kapasite gereksinimleri, kümede çalıştırmayı planladığınız iş yüküne göre belirlenir, bu nedenle size belirli iş yükünüz için nitel kılavuz sağlayamayız, ancak burada başlamanıza yardımcı olacak geniş kılavuz

Üretim iş yükleri için 

- Önerilen VM SKU standart D2_V2 veya eşdeğeri dir ve en az 50 GB yerel SSD ile eşdeğerdir.
- Desteklenen minimum kullanım VM SKU, en az 50 GB yerel SSD ile Standard_D2_V3 veya Standart D1_V2 veya eşdeğeridir. 
- Standart A0 gibi kısmi çekirdekli VM STU'lar üretim iş yükleri için desteklenmez.
- VM STU serisi performans nedenleriyle üretim iş yükleri için desteklenmez.

## <a name="non-primary-node-type---capacity-guidance-for-stateless-workloads"></a>Birincil olmayan düğüm türü - durumsuz iş yükleri için kapasite kılavuzu

Birincil olmayan düğüm türünde çalıştırdığınız durumsuz İş Yükleri bu kılavuz.

**VM örneklerinin sayısı:** Durum dolmadan üretim iş yükleri için desteklenen en az Birincil Düğüm türü boyutu 2'dir. Bu, uygulamanızın iki stateless örneğini çalıştırmanızı ve hizmetinizin bir VM örneğinin kaybından kurtulmasına olanak tanır. 

**VM SKU:** Bu, uygulama hizmetlerinizin çalıştığı düğüm türüdür, bu nedenle seçtiğiniz VM SKU, her düğüme yerleştirmeyi planladığınız en yüksek yükü dikkate almalıdır. Düğüm türünün kapasite gereksinimleri, kümede çalıştırmayı planladığınız iş yüküne göre belirlenir. Size özel iş yükünüz için nitel rehberlik sağlayamayız.  Ancak, burada başlamak yardımcı olmak için geniş bir kılavuzdur.

Üretim iş yükleri için 

- Önerilen VM SKU Standart D2_V2 veya eşdeğeridir. 
- Desteklenen minimum kullanım VM SKU Standart D1 veya Standart D1_V2 veya eşdeğeridir. 
- Standart A0 gibi kısmi çekirdekli VM STU'lar üretim iş yükleri için desteklenmez.
- VM STU serisi performans nedenleriyle üretim iş yükleri için desteklenmez.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Sonraki adımlar
Kapasite planlamanızı bitirip bir küme kurduktan sonra aşağıdakileri okuyun:

* [Servis Kumaş küme güvenliği](service-fabric-cluster-security.md)
* [Servis Kumaş küme ölçekleme](service-fabric-cluster-scaling.md)
* [Olağanüstü durum kurtarma planlaması](service-fabric-disaster-recovery.md)
* [Düğüm tiplerinin sanal makine ölçeği kümesiyle ilişkisi](service-fabric-cluster-nodetypes.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png
