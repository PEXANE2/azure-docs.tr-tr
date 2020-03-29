---
title: Bir fiyatlandırma katmanı veya SKU seçin
titleSuffix: Azure Cognitive Search
description: "Azure Bilişsel Arama bu SDO'larda kullanılabilir: Ücretsiz, Temel ve Standart ve Standart çeşitli kaynak yapılandırmalarında ve kapasite düzeylerinde kullanılabilir."
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/30/2020
ms.openlocfilehash: 35dbd064a09a96dae58e1b15a6d8889bda45ee0d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76899835"
---
# <a name="choose-a-pricing-tier-for-azure-cognitive-search"></a>Azure Bilişsel Arama için bir fiyatlandırma katmanı seçin

Bir Azure Bilişsel Arama hizmeti oluşturduğunuzda, bir kaynak hizmetin ömrü boyunca sabitlenmiş bir fiyatlandırma katmanında (veya SKU)'da [oluşturulur.](search-create-service-portal.md) Katmanlar, Ücretsiz, Temel, Standart ve Depolama Optimize Edilmiş'yi içerir. Standart ve Depolama Optimize edilmiş çeşitli yapılandırmalar ve kapasiteler ile kullanılabilir.

Çoğu müşteri hizmeti değerlendirebilmek için Ücretsiz katmanla başlar. Değerlendirme sonrası, geliştirme ve üretim dağıtımları için daha yüksek katmanlardan birinde ikinci bir hizmet oluşturmak yaygındır.

## <a name="feature-availability-by-tier"></a>Katmana göre özellik kullanılabilirliği

Ücretsiz dahil olmak üzere hemen hemen her özellik her katmanda kullanılabilir, ancak yeterli kapasite vermediğiniz sürece kaynak yoğun bir özellik veya iş akışı iyi çalışmayabilir. Örneğin, veri kümesi küçük olmadığı [sürece, AI zenginleştirme,](cognitive-search-concept-intro.md) ücretsiz bir hizmette zaman kazandıran uzun soluklu becerilere sahiptir.

Aşağıdaki tabloda katmanla ilgili özellik kısıtlamaları açıklanmaktadır.

| Özellik | Sınırlamalar |
|---------|-------------|
| [Dizinleyicileri](search-indexer-overview.md) | Dizin leyiciler S3 HD'de kullanılamaz. |
| [Müşteri tarafından yönetilen şifreleme anahtarları](search-security-manage-encryption-keys.md) | Ücretsiz katmanda kullanılamaz. |

## <a name="tiers-skus"></a>Katmanlar (SKUs)

Katmanlar şu şekilde farklılaştırılır:

+ Oluşturabileceğiniz dizin ve dizin leyici lerin miktarı
+ Bölümlerin boyutu ve hızı (fiziksel depolama)

Seçtiğiniz katman faturalandırılabilir oranı belirler. Azure portalından aşağıdaki ekran görüntüsü kullanılabilir katmanları, eksi fiyatlandırmayı (portalda ve [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulabileceğiniz) gösterir. **Ücretsiz,** **Temel**ve **Standart** en yaygın katmanlardır.

**Ücretsiz,** diğer abonelerle paylaşılan bir kümeüzerinde sınırlı bir arama hizmeti oluşturur. Hızlı başlatmalar ve öğreticiler de dahil olmak üzere küçük projeleri tamamlayabilirsiniz, ancak hizmeti ölçeklendiremez veya önemli iş yüklerini çalıştıramazsınız. **Temel** ve **Standart,** **varsayılan** standart olan en yaygın olarak kullanılan faturalandırılabilir katmanlardır.

![Azure Bilişsel Arama'nın fiyatlandırma katmanları](media/search-sku-tier/tiers.png "Azure Bilişsel Arama'nın fiyatlandırma katmanları")

Bazı katmanlar belirli çalışma türleri için optimize edilebiyi optimize eder. Örneğin, **Standart 3 Yüksek Yoğunluklu (S3 HD),** temel donanımın çok sayıda küçük dizin için optimize edildiği ve çok sayıda senaryo için tasarlandığı S3 için bir *barındırma modudur.* S3 HD, S3 ile aynı birim başına ücrete sahiptir, ancak donanım çok sayıda küçük dizinüzerinde hızlı dosya okumaları için optimize edilmiştir.

**Depolama Optimize edilmiş** katmanlar, TB başına Standart katmanlardan daha düşük bir fiyata daha büyük depolama kapasitesi sunar. Birincil tradeoff, belirli uygulama gereksinimleriniz için doğrulamanız gereken daha yüksek sorgu gecikmesidir.  Bu katmanDaki performans konuları hakkında daha fazla bilgi edinmek için Performans [ve optimizasyon konularına](search-performance-optimization.md)bakın.

[Fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/search/)çeşitli katmanlar hakkında daha fazla bilgiyi Azure [Bilişsel Arama makalesindeki Hizmet sınırları](search-limits-quotas-capacity.md) ve bir hizmeti sunarken portal sayfasında bulabilirsiniz.

## <a name="billable-events"></a>Faturalandırılabilir olaylar

Azure Bilişsel Arama'da oluşturulmuş bir çözüm aşağıdaki yollarla maliyete neden olabilir:

+ Minimum yapılandırmada temel hizmet maliyeti (hizmet oluşturma)
+ Ölçekleme yaparken artımlı maliyet (yineleme veya bölüm ekleme)
+ Bant genişliği ücretleri (giden veri aktarımı) 
+ Bilişsel arama (AI zenginleştirme için Bilişsel Hizmetler eklemek, bilgi deposu için Azure depolama)

### <a name="service-costs"></a>Servis maliyetleri

Ücretleri önlemek için "duraklatılabilir" sanal makineler veya diğer kaynakların aksine, azure bilişsel arama hizmeti her zaman özel kullanımınız için özel donanımda kullanılabilir. Bu nedenle, hizmet oluşturmak, hizmeti oluşturduğunuzda başlayan ve hizmeti sildiğinizde sona eren faturalandırılabilir bir olaydır. 

Minimum ücret faturalandırılabilir oranda ilk arama birimi (bir yineleme x bir bölüm) olduğunu. Hizmet bu yapılandırmadan daha az bir şey üzerinde çalıştırılamadığından, bu minimum hizmetin ömrü boyunca sabitlenir. Minimumun ötesinde, yinelemeleri ve bölümleri birbirinden bağımsız olarak ekleyebilirsiniz. Yinelemeler ve bölümler aracılığıyla kapasitedeki artımlı artışlar aşağıdaki formüle göre faturanızı artırır: [(çoğaltmalar x bölümleri x oranı)](#search-units), ücretlendirildiğiniz ücretlendirme oranı seçtiğiniz fiyatlandırma katmanına bağlıdır.

Bir arama çözümünün maliyetini tahmin ederken, fiyatlandırma nın ve kapasitenin doğrusal olmadığını unutmayın. (Kapasiteyi iki katına çıkarmak maliyeti iki katına çıkar.) Formülün nasıl çalıştığına bir örnek olarak, [yinelemeleri ve bölümleri nasıl ayırılabilirsiniz.](search-capacity-planning.md#how-to-allocate-replicas-and-partitions)

### <a name="bandwidth-charges"></a>Bant genişliği ücretleri

[Azure Bilişsel Arama dizin leyicilerinin](search-indexer-overview.md) kullanılması, hizmetlerinizin konumuna bağlı olarak faturalandırmayı etkileyebilir. Azure Bilişsel Arama hizmetini verilerinizle aynı bölgede oluşturursanız, veri çıkış ücretlerini tamamen ortadan kaldırabilirsiniz. İşte [bant genişliği fiyatlandırma sayfasından](https://azure.microsoft.com/pricing/details/bandwidth/)bazı bilgiler:

+ Microsoft, Azure'daki herhangi bir hizmete gelen veriler veya Azure Bilişsel Arama'dan giden veriler için ücret almaz.
+ Çok hizmetli çözümlerde, tüm hizmetler aynı bölgedeyken, kabloyu geçen veriler için ücret alınmaz.

Hizmetler farklı bölgelerdeyse, giden veriler için ücretler uygulanır. Bu ücretler aslında Azure Bilişsel Arama faturanızın bir parçası değildir. Burada adı geçildiği için, farklı bölgelerden veri çekmek için veri veya AI ile zenginleştirilmiş dizin leyiciler kullanıyorsanız, maliyetlerin genel faturanıza yansıdığını görürsünüz.

### <a name="ai-enrichment-with-cognitive-services"></a>Bilişsel Hizmetler ile AI zenginleştirme

[AI zenginleştirme için,](cognitive-search-concept-intro.md)Azure Bilişsel Arama ile aynı bölgede, istediğiniz kadar öde işleme için S0 fiyatlandırma katmanına [faturalandırılabilir bir Azure Bilişsel Hizmetler kaynağı eklemeyi](cognitive-search-attach-cognitive-services.md)planlamanız gerekir. Bilişsel Hizmetler ekleme ile ilgili hiçbir sabit maliyet var. Yalnızca ihtiyacınız olan işlem için ödeme yapıyorsun.

| İşlem | Faturalama etkisi |
|-----------|----------------|
| Belge kırma, metin çıkarma | Ücretsiz |
| Belge çatlama, görüntü çıkarma | Belgelerinizden çıkarılan görüntü sayısına göre faturalandırılır. [Dizinleyici](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-parameters) **yapılandırmasında, imageAction** görüntü ayıklamatetikleyen parametredir. **imageAction** "none" (varsayılan) olarak ayarlanmışsa, görüntü ayıklama için ücretlendirilmezsiniz. Görüntü ayıklama oranı, Azure Bilişsel Arama'nın [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/) sayfasında belgelenmiştir.|
| [Yerleşik bilişsel beceriler](cognitive-search-predefined-skills.md) | Görevi doğrudan Bilişsel Hizmetler'i kullanarak gerçekleştirmişsiniz gibi aynı oranda faturalandırılır. |
| Özel beceriler | Özel bir beceri sağladığınız işlevselliktir. Özel bir beceri kullanmanın maliyeti tamamen özel kodun diğer tarifeli hizmetleri çağırıp çağırmadığına bağlıdır. |

<a name="search-units"></a>

## <a name="billing-formula-r-x-p--su"></a>Faturalama formülü (R x P = SU)

Azure Bilişsel Arama işlemleri için anlaşılması gereken en önemli faturalandırma konsepti *arama birimidir* (SU). Azure Bilişsel Arama, dizin oluşturma ve sorgular için hem yinelemelere hem de bölümlere bağlı olduğundan, faturalandırmanın sadece bir veya diğerini faturalandırması mantıklı değildir. Bunun yerine, faturalandırma her ikisinin de bir bileşimini temel alar.

SU, bir hizmet tarafından kullanılan *yinelemeve* *bölümlerin* ürünüdür: **(R x P = SU)**.

Her hizmet minimum olarak bir SU (bir yineleme bir bölümle çarpılır) ile başlar. Herhangi bir hizmet için maksimum 36 SUs'dur. Bu maksimum birden çok şekilde ulaşılabilir: 6 bölüm x 6 çoğaltmalar, ya da 3 bölüm x 12 çoğaltmalar, örneğin. Toplam kapasiteden daha az kullanmak yaygındır (örneğin, 9 SUs olarak faturalanan 3 çoğaltma, 3 bölümlü bir hizmet). Geçerli [kombinasyonlar](search-capacity-planning.md#chart) için Bölüm ve yineleme kombinasyonları grafiğine bakın.

Faturalandırma oranı SU başına saatliktir. Her katman giderek daha yüksek bir orana sahiptir. Daha yüksek katmanlar daha büyük ve daha hızlı bölümlerle gelir ve bu da o katman için genel olarak daha yüksek bir saatlik hıza katkıda bulunur. [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/) sayfasında her kademeiçin fiyatları görüntüleyebilirsiniz.

Çoğu müşteri toplam kapasitenin sadece bir kısmını çevrimiçi olarak getirsin, geri kalanını yedekte tutar. Faturalandırma için, SU formülü yle hesaplanan çevrimiçi getirdiğiniz bölüm ve yineleme lerin sayısı, saatlik olarak ne kadar ödeyeceğinizi belirler.

## <a name="how-to-manage-costs"></a>Maliyetleri yönetme

Aşağıdaki öneriler maliyetleri minimumda tutmanıza yardımcı olabilir:

- Bant genişliği ücretlerini en aza indirmek veya ortadan kaldırmak için aynı bölgedeki veya mümkün olduğunca az bölgede tüm kaynakları oluşturun.

- Azure Bilişsel Arama, Bilişsel Hizmetler ve çözümünüzde kullanılan diğer Azure hizmetleri gibi tüm hizmetleri tek bir kaynak grubunda birleştirin. Azure portalında kaynak grubunu bulun ve gerçek ve öngörülen harcamaları anlamak için **Maliyet Yönetimi** komutlarını kullanın.

- İsteklerin ve yanıtların veri merkezi sınırı içinde kalması için ön uç uygulamanız için Azure Web Uygulamasını düşünün.

- Dizin oluşturma gibi kaynak yoğun işlemler için ölçeklendirin ve ardından normal sorgu iş yükleri için aşağı doğru yeniden yeniden ayarlayın. Azure Bilişsel Arama için minimum yapılandırmayla başlayın (bir SU bir bölüm ve bir yinelemeden oluşur) ve ardından daha fazla kapasite gereksinimini gösteren kullanım modellerini belirlemek için kullanıcı etkinliğini izleyin. Öngörülebilir bir desen varsa, ölçeği etkinlikle eşitleyebilir (bunu otomatikleştirmek için kod yazmanız gerekir).

Ayrıca, harcamalarla ilgili yerleşik araçlar ve özellikler için [Faturalandırma ve maliyet yönetimini](https://docs.microsoft.com/azure/billing/billing-getting-started) ziyaret edin.

Bir arama hizmetini geçici olarak kapatmak mümkün değildir. Özel kaynaklar her zaman çalışır durumdadır ve hizmetinizin kullanım ömrü boyunca özel kullanımınız için ayrılmıştır. Bir hizmeti silmek kalıcıdır ve ilişkili verilerini de siler.

Hizmetin kendisi açısından, faturanızı düşürmenin tek yolu yinelemeleri ve bölümleri hala kabul edilebilir performans ve [SLA uyumluluğu](https://azure.microsoft.com/support/legal/sla/search/v1_0/)sağlayan bir düzeye düşürmek veya daha düşük bir katmanda bir hizmet oluşturmaktır (S1 saatlik ücretler S2 veya S3 oranlarından daha düşüktür). Hizmetinizi yük projeksiyonlarınızın alt ucunda gördüğünüzü varsayarsak, hizmeti büyütürseniz, ikinci bir daha büyük katmanlı hizmet oluşturabilir, dizinlerinizi ikinci hizmette yeniden oluşturabilir ve ilkini silebilirsiniz.

## <a name="how-to-evaluate-capacity-requirements"></a>Kapasite gereksinimleri nasıl değerlendirilir?

Azure Bilişsel Arama'da kapasite *çoğaltma* ve *bölüm*olarak yapılandırılır.

+ Yinelemeler arama hizmetiörnekleridir. Her yineleme, bir dizinin yük dengeli bir kopyasını barındırr. Örneğin, altı yinelemeli bir hizmet, hizmette yüklenen her dizinin altı kopyasına sahiptir.

+ Bölümler dizinleri depolar ve aranabilir verileri otomatik olarak böler. Dizininizi ikiye, üç bölüm üçte bire böler ve böyle devam eder. Kapasite açısından, *bölüm boyutu* katmanlar arasında birincil ayırt edici özelliktir.

> [!NOTE]
> Tüm Standart ve Depolama Optimize edilmiş katmanlar, bakiyeyi değiştirerek [sisteminizi hız veya depolama için optimize](search-performance-optimization.md) edebilmeniz için esnek çoğaltma ve bölüm [kombinasyonlarını](search-capacity-planning.md#chart) destekler. Temel katman, yüksek kullanılabilirlik için en fazla üç yineleme sunar, ancak yalnızca bir bölümü vardır. Ücretsiz katmanlar özel kaynaklar sağlamaz: bilgi işlem kaynakları birden çok abone tarafından paylaşılır.


### <a name="evaluating-capacity"></a>Kapasitenin değerlendirilmesi

Kapasite ve hizmet çalıştırma maliyetleri el ele gider. Katmanlar iki düzeyde sınırlar uygular: depolama ve kaynaklar. Her ikisini de düşünmelisiniz, çünkü ilk hangi sınıra ulaşırsanız ulaşın etkili sınırdır.

İş gereksinimleri genellikle gereksinim duyduğunuz dizin sayısını belirler. Örneğin, büyük bir belge deposu için genel bir dizin gerekebilir. Veya bölge, uygulama veya iş niş dayalı birden çok dizin gerekebilir.

Bir dizin boyutunu belirlemek için, [bir tane oluşturmanız](search-create-index-portal.md)gerekir. Boyutu, öneriyi etkinleştirip etkinleştirmediğiniz, filtreleme ve sıralama gibi içe aktarılan verilere ve dizin yapılandırmasına dayanır. Yapılandırmanın boyut üzerindeki etkisi hakkında daha fazla bilgi için [bkz. ](search-what-is-an-index.md)

Tam metin aramaiçin birincil veri yapısı, kaynak verilerden farklı özelliklere sahip ters bir [dizin](https://en.wikipedia.org/wiki/Inverted_index) yapısıdır. Ters bir dizin için boyut ve karmaşıklık, içine beslediğiniz veri miktarına göre değil, içerik tarafından belirlenir. Yüksek artıklığa sahip büyük bir veri kaynağı, yüksek değişken içerik içeren daha küçük bir veri kümesinden daha küçük bir dizinle sonuçlanabilir. Bu nedenle, orijinal veri kümesinin boyutuna bağlı olarak dizin boyutunu çıkarmak nadiren mümkündür.

> [!NOTE] 
> Even though estimating future needs for indexes and storage can feel like guesswork, it's worth doing. Bir katmanın kapasitesi çok düşük çıkarsa, daha yüksek bir katmanda yeni bir hizmet sağlamanız ve [ardından dizinlerinizi yeniden yüklemeniz](search-howto-reindex.md)gerekir. Bir SKU'dan diğerine bir hizmetin yerinde yükseltmesi yoktur.
>

### <a name="estimate-with-the-free-tier"></a>Serbest katmanla tahmin edin

Kapasiteyi tahmin etmek için bir yaklaşım, Serbest katmanla başlamaktır. Ücretsiz hizmetin üç dizin, 50 MB depolama alanı ve 2 dakikalık dizin oluşturma süresi sunduğunu unutmayın. Bu kısıtlamalarla öngörülen dizin boyutunu tahmin etmek zor olabilir, ancak aşağıdaki adımlar şunlardır:

+ [Ücretsiz bir hizmet oluşturun.](search-create-service-portal.md)
+ Küçük, temsili bir veri kümesi hazırlayın.
+ [Portalda bir başlangıç dizini oluşturun](search-create-index-portal.md) ve boyutunu not edin. Özellikler ve özniteliklerin depolama üzerinde etkisi vardır. Örneğin, öneriyi ler (typeahead) ekleme depolama gereksinimlerini artırır. Aynı veri kümesini kullanarak, depolama gereksinimlerinin nasıl değiştiğini görmek için her alanda farklı özniteliklere sahip bir dizinin birden çok sürümü oluşturmayı deneyebilirsiniz. Daha fazla bilgi için [temel dizin oluştur'daki "Depolama etkileri"](search-what-is-an-index.md#index-size)başlıklı ana bakın.

Elinde kaba bir tahminle, bu tutarı iki dizin (geliştirme ve üretim) için bütçeye iki katına çıkarabilir ve katmanınızı buna göre seçebilirsiniz.

### <a name="estimate-with-a-billable-tier"></a>Faturalandırılabilir bir katmanla tahmin

Ayrılmış kaynaklar, geliştirme sırasında dizin miktarı, boyutu ve sorgu hacimlerinin daha gerçekçi tahminleri için daha büyük örnekleme ve işlem sürelerini barındırabilir. Bazı müşteriler faturalandırılabilir bir katmanla hemen atlar ve geliştirme projesi olgunlaştıkça yeniden değerlendirir.

1. Alt katmanların ihtiyacınız olan dizin sayısını destekleyip desteklemeyeceğini belirlemek için [her katmandaki hizmet sınırlarını gözden geçirin.](https://docs.microsoft.com/azure/search/search-limits-quotas-capacity#index-limits) Temel, S1 ve S2 katmanlarında dizin sınırları sırasıyla 15, 50 ve 200'dir. Depolama Optimize Edilen katman, çok büyük dizin sayısını desteklemek üzere tasarlandığından 10 dizin sınırına sahiptir.

1. [Faturalandırılabilir bir katmanda hizmet oluşturma:](search-create-service-portal.md)

    + Öngörülen yükten emin değilseniz, Basic veya S1'de düşük başlayın.
    + Büyük ölçekli dizin oluşturma ve sorgu yükleri olacağını biliyorsanız, S2 ve hatta S3'te yüksek başlayın.
    + Büyük miktarda veri dizine ekiyorsanız ve iç iş uygulamasında olduğu gibi sorgu yükü nispeten düşükse, Depolama Optimize Optimize Edile başlayın.

1. Kaynak verilerin bir dizine nasıl çevrilmesini belirlemek için [bir başlangıç dizini oluşturun.](search-create-index-portal.md) Dizin boyutunu tahmin etmenin tek yolu budur.

1. Portalda [depolamayı, hizmet sınırlarını, sorgu hacmini ve gecikme süresini izleyin.](search-monitor-usage.md) Portal, saniyede sorguları, daraltılmış sorguları ve arama gecikmesini gösterir. Tüm bu değerler, doğru katmanı seçip seçmediğinize karar vermenize yardımcı olabilir. 

Dizin sayısı ve boyutu analiziçin eşit derecede önemlidir. Bunun nedeni, hangisi önce gerçekleşirse, depolamanın (bölümlerin) tam kullanımı veya kaynaklardaki maksimum sınırlar (dizinler, dizinleyiciler vb.) yoluyla maksimum sınırlara ulaşılmasıdır. Portal, Genel Bakış sayfasında geçerli kullanımı ve maksimum sınırları yan yana göstererek her ikisini de izlemenize yardımcı olur.

> [!NOTE]
> Belgeler gereksiz veriler içeriyorsa depolama gereksinimleri şişirilebilir. İdeal olarak, belgeler yalnızca arama deneyimi için gereken verileri içerir. İkili veriler aranabilir değildir ve ayrı olarak depolanmalıdır (belki bir Azure tablosunda veya blob depolama alanında). Daha sonra dış verilere URL başvurusu tutmak için dizine bir alan eklenmelidir. Tek bir belgenin maksimum boyutu 16 MB'dır (veya tek bir istekte birden fazla belge yüklüyorsanız daha az). Daha fazla bilgi için [Azure Bilişsel Arama'da Hizmet sınırlarına](search-limits-quotas-capacity.md)bakın.
>

**Birim degistlerini sorgula**

Saniyedeki sorgular (QPS) performans ayarlaması sırasında önemli bir ölçümdür, ancak başlangıçta yüksek sorgu hacmi bekliyorsanız genellikle yalnızca bir katman göz önünde bulundurulması gerekir.

Standart katmanlar yinelemeler ve bölümlerin bir dengesini sağlayabilir. Yük dengeleme için yinelemeler ekleyerek sorgu geri dönüşünü artırabilir veya paralel işleme için bölümler ekleyebilirsiniz. Daha sonra hizmet sağlandıktan sonra performans için ayarlayabilirsiniz.

En başından itibaren yüksek sürekli sorgu birimleri bekliyorsanız, daha güçlü donanımlarla desteklenen daha yüksek Standart katmanları göz önünde bulundurmanız gerekir. Daha sonra, bu sorgu birimleri oluşmuyorsa, bölümleri ve yinelemeleri çevrimdışı alabilir veya hatta daha düşük katmanlı bir hizmete geçebilirsiniz. Sorgu iş ortasını nasıl hesaplayıp hesaplanacağınız hakkında daha fazla bilgi için Azure [Bilişsel Arama performansı ve optimizasyonuna](search-performance-optimization.md)bakın.

Depolama Optimize Edilen katmanlar, sorgu gecikmesi gereksinimlerinin daha az önemli olduğu zamanlar için daha fazla genel kullanılabilir dizin depolamasını destekleyerek büyük veri iş yükleri için yararlıdır. Yine de yük dengeleme ve paralel işleme için ek bölümler için ek yinelemeler kullanmanız gerekir. Daha sonra hizmet sağlandıktan sonra performans için ayarlayabilirsiniz.

**Hizmet düzeyi anlaşmaları**

Ücretsiz katman ve önizleme özellikleri [hizmet düzeyi anlaşmaları (SLA'lar)](https://azure.microsoft.com/support/legal/sla/search/v1_0/)sağlamaz. Tüm faturalandırılabilir katmanlar için, hizmetiniz için yeterli fazlalık sağdığınızda SLA'lar etkili olur. Sorgu (okuma) SLA'ları için iki veya daha fazla yinelemeye sahip olmanız gerekir. Sorgu ve dizin oluşturma (okuma-yazma) SLA'ları için üç veya daha fazla yinelemeye sahip olmanız gerekir. Bölüm sayısı SLA'ları etkilemez.

## <a name="tips-for-tier-evaluation"></a>Katman değerlendirmesi için ipuçları

+ Ölçümlerin sorgular etrafında oluşturmasına ve kullanım desenleri (iş saatlerinde sorgular, yoğun olmayan saatlerde dizini oluşturma) etrafında veri toplamasına izin verin. Hizmet sağlama kararlarını bildirmek için bu verileri kullanın. Saatlik veya günlük olarak pratik olmasa da, sorgu birimlerinde planlanan değişiklikleri karşılamak için bölümleri ve kaynakları dinamik olarak ayarlayabilirsiniz. Ayrıca, düzeyler eyleme geçmeyi gerektirecek kadar uzun tutarsa, planlanmamış ancak sürekli değişiklikleri de barındırabilirsiniz.

+ Yetersiz sağlamanın tek dezavantajının, gerçek gereksinimler öngörülerinizden daha büyükse bir hizmeti yıkmak zorunda kalmak zorunda kalmak olması olduğunu unutmayın. Hizmet kesintisini önlemek için, daha yüksek bir katmanda yeni bir hizmet oluşturur ve tüm uygulamalar ve istekler yeni bitiş noktasını hedefleyene kadar yan yana çalıştırırsınız.

## <a name="next-steps"></a>Sonraki adımlar

Serbest katmanla başlayın ve özelliklerini anlamak için verilerinizin bir alt kümesini kullanarak bir başlangıç dizini oluşturun. Azure Bilişsel Arama'daki veri yapısı ters bir dizin yapısıdır. Ters bir dizinin boyutu ve karmaşıklığı içerik tarafından belirlenir. Son derece gereksiz içeriğin, son derece düzensiz içeriğe göre daha küçük bir dizin oluşturma eğiliminde olduğunu unutmayın. Bu nedenle, veri kümesinin boyutundan çok içerik özellikleri dizin depolama gereksinimlerini belirler.

Dizin boyutunuzla ilgili ilk tahmininizi yaptıktan sonra, bu makalede tartışılan katmanlardan birine [faturalandırılabilir bir hizmet sağlama:](search-create-service-portal.md) Temel, Standart veya Depolama Optimize Edildi. Veri boyutlandırmayla ilgili yapay kısıtlamaları gevşetin ve aranabilir olmasını istediğiniz tüm verileri içerecek şekilde [dizininizi yeniden oluşturun.](search-howto-reindex.md)

Gerek duyduğunuz performansı ve ölçeği elde etmek için [gerektiğinde bölümleri ve yinelemeleri ayırın.](search-capacity-planning.md)

Performans ve kapasite iyiyse, bittiniz. Aksi takdirde, gereksinimlerinize daha yakından uyumlu farklı bir katmanda bir arama hizmeti oluşturun.

> [!NOTE]
> Sorularınız varsa [StackOverflow'a](https://stackoverflow.com/questions/tagged/azure-search) gönderin veya [Azure desteğine başvurun.](https://azure.microsoft.com/support/options/)
