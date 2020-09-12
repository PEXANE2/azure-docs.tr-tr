---
title: Fiyatlandırma Katmanı veya SKU seçin
titleSuffix: Azure Cognitive Search
description: "Azure Bilişsel Arama, bu SKU 'Larda sağlanabilir: ücretsiz, temel ve standart ve standart, çeşitli kaynak yapılandırmalarında ve kapasite düzeylerinde kullanılabilir."
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: c09c5b33f7960ffebfdfc8005537e75a249b8d99
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89661094"
---
# <a name="choose-a-pricing-tier-for-azure-cognitive-search"></a>Azure Bilişsel Arama için bir fiyatlandırma katmanı seçin

Bir Azure Bilişsel Arama hizmeti oluşturduğunuzda, hizmet ömrü boyunca düzeltilen bir fiyatlandırma katmanında (veya SKU) bir [kaynak oluşturulur](search-create-service-portal.md) . Katmanlar ücretsiz, temel, standart ve depolama için Iyileştirilmiş. Standart ve depolama için Iyileştirilmiş çeşitli yapılandırmalarda ve kapasitelerde kullanılabilir.

Çoğu müşteri, hizmeti değerlendirebilmeleri için ücretsiz katmanla başlar. Değerlendirme sonrası, geliştirme ve üretim dağıtımları için daha yüksek katmanlardan birinde ikinci bir hizmet oluşturmak yaygındır.

## <a name="feature-availability-by-tier"></a>Katmana göre özellik kullanılabilirliği

Aşağıdaki tabloda katman ile ilgili özellik kısıtlamaları açıklanmaktadır.

| Öne çıkan özelliği | Sınırlamalar |
|---------|-------------|
| [Dizinleyiciler](search-indexer-overview.md) | Dizin oluşturucular S3 HD üzerinde kullanılamaz. |
| [Yapay zeka zenginleştirme](search-security-manage-encryption-keys.md) | Ücretsiz katmanda çalışır, ancak önerilmez. |
| [Müşteri tarafından yönetilen şifreleme anahtarları](search-security-manage-encryption-keys.md) | Ücretsiz katmanda kullanılamaz. |
| [IP güvenlik duvarı erişimi](service-configure-firewall.md) | Ücretsiz katmanda kullanılamaz. |
| [Azure özel bağlantısı ile tümleştirme](service-create-private-endpoint.md) | Ücretsiz katmanda kullanılamaz. |

Birçok özellik, ücretsiz olarak da dahil olmak üzere her katmanda mevcuttur, ancak yeterli kapasiteye sahip olmadığınız takdirde Kaynak yoğunluklu Özellikler iyi çalışmayabilir. Örneğin, [AI zenginleştirme](cognitive-search-concept-intro.md) , veri kümesi küçük olmadığı sürece ücretsiz bir hizmette zaman aşımına uğrar uzun süreli yetenekler içerir.

## <a name="tiers-skus"></a>Katmanlar (SKU 'Lar)

Katmanlar şu şekilde farklılaştırılabilir:

+ Dizin ve dizin oluşturucularının miktarı (en fazla limit)
+ Bölümlerin boyutu ve hızı (fiziksel depolama)

Seçtiğiniz katman faturalandırılabilir ücreti belirler. Azure portal aşağıdaki ekran görüntüsünde, kullanılabilir katmanlar, eksi fiyatlandırma (portalda ve [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulabilirsiniz) gösterilmektedir. **Ücretsiz**, **temel**ve **Standart** en yaygın katmanlardır.

**Ücretsiz** , hızlı başlangıç ve öğreticiler dahil daha küçük projeler için sınırlı bir arama hizmeti oluşturur. Dahili olarak, çoğaltmalar ve bölümler birden çok abone arasında paylaşılır. Ücretsiz bir hizmeti ölçeklendiremez veya önemli iş yüklerini çalıştıramazsınız.

**Temel** ve **Standart** , **Standart** olarak varsayılan değer olan en yaygın olarak kullanılan faturalanabilir katmanlardır. Denetiminizin altında adanmış kaynaklar sayesinde, daha büyük projeler dağıtabilir, performansı iyileştirir ve kapasiteyi ayarlayabilirsiniz.

![Azure Bilişsel Arama fiyatlandırma katmanları](media/search-sku-tier/tiers.png "Azure Bilişsel Arama fiyatlandırma katmanları")

Bazı katmanlar belirli iş türleri için iyileştirilmiştir. Örneğin, **Standart 3 yüksek yoğunluklu (S3 HD)** , S3 için bir *barındırma modudur* ; burada temeldeki donanım çok sayıda daha küçük dizinler için optimize edilmiştir ve çok kiracılı senaryolar için tasarlanmıştır. S3 HD, S3 ile aynı birim başına ücretine sahiptir, ancak donanım çok sayıda daha küçük dizin üzerinde hızlı dosya okuma için en iyi duruma getirilmiştir.

**Depolama Için iyileştirilmiş** katmanlar, standart katmanlardan TB başına daha büyük depolama kapasitesi sunar. Birincil zorunluluğunu getirir, belirli uygulama gereksinimleriniz için doğrulamanız gereken daha yüksek sorgu gecikmedir.  Bu katmanın performans konuları hakkında daha fazla bilgi için bkz. [performans ve iyileştirme konuları](search-performance-optimization.md).

[Fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/search/)çeşitli katmanlar hakkında daha fazla bilgi edinmek için bkz. [Azure bilişsel arama 'deki hizmet sınırları](search-limits-quotas-capacity.md) ve hizmet sağlanırken Portal sayfası.

## <a name="billable-events"></a>Faturalandırılabilir olaylar

Azure Bilişsel Arama 'de oluşturulan bir çözüm, aşağıdaki yollarla maliyette ücret alabilir:

+ Hizmetin, en az yapılandırmada (bir bölüm ve çoğaltma) 7/24 çalışan maliyeti

+ Kapasite (çoğaltmalar veya bölümler) ekleme

+ Bant genişliği ücretleri (giden veri aktarımı)

+ Belirli yetenekler veya özellikler için gereken eklenti hizmetleri:

  + AI zenginleştirme (bilişsel [Hizmetler](https://azure.microsoft.com/pricing/details/cognitive-services/)gerektirir)
  + bilgi deposu ( [Azure Storage](https://azure.microsoft.com/pricing/details/storage/)gerektirir)
  + Artımlı zenginleştirme ( [Azure Storage](https://azure.microsoft.com/pricing/details/storage/)gerektırır, AI zenginleştirme için geçerlidir)
  + Müşteri tarafından yönetilen anahtarlar ve çift şifreleme ( [Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)gerekir)
  + İnternet erişimi olmayan bir model için özel uç noktalar ( [Azure özel bağlantısı](https://azure.microsoft.com/pricing/details/private-link/)gerekir)

### <a name="service-costs"></a>Hizmet maliyetleri

Ücretlendirmeden kaçınmak için sanal makinelerden veya "duraklatılmış" olabilecek diğer kaynaklardan farklı olarak Azure Bilişsel Arama hizmeti, özel kullanım için ayrılmış donanımlar üzerinde her zaman kullanılabilir. Bu nedenle, hizmet oluşturma hizmeti oluştururken başlayan faturalandırılabilir bir olaydır ve hizmeti sildiğinizde sona erer. 

Minimum ücret, faturalandırılabilir fiyata ilk arama birimidir (bir çoğaltma x bir bölüm). Hizmet bu yapılandırmadan daha küçük bir süre içinde çalışmadığından hizmetin kullanım ömrü için bu minimum değer düzeltildi. En düşük değerin ötesinde, çoğaltmaları ve bölümleri birbirinden bağımsız olarak ekleyebilirsiniz. Çoğaltmalar ve bölümler aracılığıyla kapasitenin artımlı artışları, faturanızı aşağıdaki formüle göre artırır: [(çoğaltmalar x Partitions x oranı)](#search-units), burada ücretlendirilebilecek ücret seçtiğiniz fiyatlandırma katmanına bağlıdır.

Bir arama çözümünün maliyetini tahmin ediyorsanız, fiyatlandırma ve kapasitenin doğrusal değil olduğunu aklınızda bulundurun. (Katından fazla kapasite maliyeti iki katına çıkarır.) Formülün nasıl çalıştığına ilişkin bir örnek için bkz. [çoğaltmaları ve bölümleri ayırma](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).

### <a name="bandwidth-charges"></a>Bant genişliği ücretleri

[Dizin oluşturucularının](search-indexer-overview.md) kullanılması, hizmetlerinizin konumuna bağlı olarak faturalandırmayı etkileyebilir. Azure Bilişsel Arama hizmetini verileriniz ile aynı bölgede oluşturursanız, veri çıkış ücretlerini tamamen ortadan kaldırabilirsiniz. [Bant genişliği fiyatlandırma sayfasından](https://azure.microsoft.com/pricing/details/bandwidth/)bazı bilgiler aşağıda verilmiştir:

+ Microsoft, tüm gelen verileri Azure 'daki herhangi bir hizmete veya Azure Bilişsel Arama giden veriler için ücretlendirmez.
+ Çoklu hizmet çözümlerinde, tüm hizmetler aynı bölgedeyse, bu, kablo ile kesişen veriler için ücret alınmaz.

Hizmetler farklı bölgelerde olduğunda, giden veriler için ücretler uygulanır. Bu ücretler aslında Azure Bilişsel Arama faturanızın bir parçası değildir. Farklı bölgelerdeki verileri çekmek için veri veya AI zenginleştirilmiş dizinleyiciler kullanıyorsanız, maliyetleri genel faturanızda görürsünüz.

### <a name="ai-enrichment-with-cognitive-services"></a>Bilişsel hizmetler ile AI zenginleştirme

[AI zenginleştirme](cognitive-search-concept-intro.md)için, Kullandıkça Öde Işleme için S0 fiyatlandırma katmanında Azure bilişsel arama ile aynı bölgede [faturalandırılabilir bir Azure bilişsel hizmetler kaynağı eklemeyi](cognitive-search-attach-cognitive-services.md)planlamalısınız. Bilişsel hizmetler ekleme ile ilişkili sabit bir maliyet yoktur. Yalnızca ihtiyacınız olan işleme için ödeme yaparsınız.

| İşlem | Faturalama etkisi |
|-----------|----------------|
| Belge çözme, metin ayıklama | Ücretsiz |
| Belge çözme, görüntü ayıklama | Belgelerinizden ayıklanan görüntü sayısına göre faturalandırılır. Bir [Dizin Oluşturucu yapılandırmasında](/rest/api/searchservice/create-indexer#indexer-parameters) **ımageaction** , görüntü ayıklamayı tetikleyen parametredir. **Imageaction** "none" (varsayılan) olarak ayarlandıysa, görüntü ayıklama için ücretlendirilmezsiniz. Görüntü ayıklama oranı, Azure Bilişsel Arama için [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/) sayfasında belgelenmiştir.|
| [Yerleşik bilişsel beceriler](cognitive-search-predefined-skills.md) | Bilişsel hizmetler 'i doğrudan kullanarak görevi gerçekleştirdiyseniz aynı hızda faturalandırılır. |
| Özel beceriler | Özel bir beceri sağladığınız işlevsellikdir. Özel bir beceri kullanmanın maliyeti, tamamen özel kodun diğer ölçülen Hizmetleri çağırarak çağrılmayacağı konusunda farklılık gösterir. |

[Artımlı zenginleştirme (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) özelliği, dizin oluşturucunun yalnızca beceri ' yi daha sonra değiştirmeniz gereken bilişsel becerileri çalıştırmaya ve zamandan ve paradan tasarruf etmenize olanak sağlayan bir önbellek sağlamanıza olanak tanır.

<a name="search-units"></a>

## <a name="billing-formula-r-x-p--su"></a>Faturalandırma formülü (R x P = SU)

Azure Bilişsel Arama işlemlerinde anlaşılması için en önemli faturalandırma kavramı, *arama birimidir* (su). Azure Bilişsel Arama Dizin oluşturma ve sorgular için hem çoğaltmalara hem de bölümlere bağlı olduğundan, yalnızca bir veya başka bir fatura tarafından faturalandırılmıyor. Bunun yerine, faturalandırma her ikisinin de bir bileşimini temel alır.

SU, bir hizmet tarafından kullanılan *kopyaların* ve *bölümlerin* ürünüdür: **(R x P = su)**.

Her hizmet bir SU (bir çoğaltma bir bölümden çarpılarak) en düşük düzeyde başlar. Her hizmet için en fazla 36 SUs olur. Bu en fazla iki şekilde erişilebilir: 6 Bölüm x 6 çoğaltma veya 3 bölümden oluşan x 12 çoğaltma. Toplam kapasitenin (örneğin, 3 çoğaltma, 3 bölümlü bir hizmetin 9 ' da faturalandırılan) kullanılması yaygındır. Geçerli birleşimler için [bölüm ve çoğaltma birleşimleri](search-capacity-planning.md#chart) grafiğine bakın.

Fatura ücreti SU başına saatlik olarak belirlenir. Her katmanda aşamalı olarak daha yüksek bir hız vardır. Daha yüksek katmanlar daha büyük ve speedileyici bölümleri ile gelir ve bu, bu katman için genel olarak saatlik bir fiyat sağlar. [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/) sayfasında her bir katmanın oranlarını görüntüleyebilirsiniz.

Çoğu müşteri, kalanı ayrılmış olarak tutarak toplam kapasitenin yalnızca bir kısmını çevrimiçi duruma getirir. Faturalandırma için, çevrimiçi duruma getirdiğiniz bölüm ve çoğaltmaların sayısı, SU formülü tarafından hesaplandığınız, saatlik olarak ne ödediklerinizi belirler.

## <a name="how-to-manage-costs"></a>Maliyetleri yönetme

Aşağıdaki öneriler, maliyetleri düşürmenize veya maliyetleri daha etkin bir şekilde yönetmenize yardımcı olabilir:

+ Bant genişliği ücretlerini en aza indirmek veya ortadan kaldırmak için, aynı bölgedeki veya mümkün olduğunca az sayıda bölgede tüm kaynakları oluşturun.

+ Tüm hizmetleri Azure Bilişsel Arama, bilişsel hizmetler ve çözümünüzde kullanılan diğer Azure hizmetleri gibi tek bir kaynak grubunda birleştirin. Azure portal, kaynak grubunu bulun ve gerçek ve tahmini harcama hakkında bilgi edinmek için **maliyet yönetimi** komutlarını kullanın.

+ İstek ve yanıtların veri merkezi sınırının içinde kalması için, ön uç uygulamanız için Azure Web App 'i göz önünde bulundurun.

+ Dizin oluşturma gibi yoğun kaynak gerektiren işlemlere göre ölçeklendirin ve normal sorgu iş yükleri için hemen aşağı doğru. Azure Bilişsel Arama için en düşük yapılandırma (bir bölümden ve bir çoğaltmayla bir SU) ile başlayın ve ardından daha fazla kapasiteye ihtiyacı olan kullanım düzenlerini belirlemek için Kullanıcı etkinliğini izleyin. Tahmin edilebilir bir model varsa, ölçeklendirmeyi etkinlikle eşitlenebilir (Bunu otomatikleştirmek için kod yazmanız gerekir).

Ayrıca, harcama ile ilgili yerleşik araçlar ve özellikler için [faturalandırma ve maliyet yönetimi](../cost-management-billing/manage/getting-started.md) ' ni ziyaret edin.

Arama hizmetini geçici olarak kapatmak mümkün değildir. Adanmış kaynaklar her zaman çalışır ve hizmetinizin kullanım ömrü boyunca özel kullanım için ayrılır. Bir hizmetin silinmesi kalıcıdır ve ayrıca ilişkili verileri de silinir.

Hizmetin kendisi açısından, faturanızı düşürmenin tek yolu, çoğaltmaları ve bölümleri, kabul edilebilir performans ve [SLA uyumluluğu](https://azure.microsoft.com/support/legal/sla/search/v1_0/)sağlayan bir düzeye düşürmeniz veya daha düşük bir katmanda bir hizmet oluşturmanız (S1 saatlik fiyatlar S2 veya S3 oranlarından daha düşüktür). Hizmetinizi yük projeksiyonlarınızın alt ucunda sağladığınızda, hizmeti büyüyerek ikinci bir daha büyük katmanlı hizmet oluşturabilir, dizinlerinizi ikinci hizmette yeniden oluşturabilir ve sonra birincisini silebilirsiniz.

## <a name="how-to-evaluate-capacity-requirements"></a>Kapasite gereksinimlerini değerlendirme

Azure Bilişsel Arama, kapasite *çoğaltmalar* ve *bölümler*olarak yapılandırılır.

+ Çoğaltmalar, arama hizmetinin örnekleridir. Her çoğaltma, bir dizinin yük dengeli bir kopyasını barındırır. Örneğin, altı çoğaltmaya sahip bir hizmetin, hizmete yüklenen her dizinin altı kopyası vardır.

+ Bölümler depolar ve aranabilir verileri otomatik olarak böler. İki bölüm, dizininizi yarı bir şekilde böler, üç bölüm onu üçe ayırır ve bu şekilde devam eder. Kapasite açısından *bölüm boyutu* , katmanlar arasında birincil farklılaştırıcı özelliktir.

> [!NOTE]
> Tüm standart ve depolama için Iyileştirilmiş katmanlar, [çoğaltma ve bölümlerin esnek birleşimlerini](search-capacity-planning.md#chart) destekler, böylece dengeyi değiştirerek [sisteminizi hız veya depolama için iyileştirebilirsiniz](search-performance-optimization.md) . Temel katman, yüksek kullanılabilirlik için en fazla üç çoğaltma sunar, ancak yalnızca bir bölüme sahiptir. Ücretsiz katmanlar adanmış kaynaklar sağlamıyor: bilgi işlem kaynakları birden çok abone tarafından paylaşılır.

### <a name="evaluating-capacity"></a>Kapasiteyi değerlendirme

Hizmetin kapasitesi ve maliyetleri, el ile çalışmaya devam ediyor. Katmanlar iki düzeyde sınırlar uygulayabilir: depolama ve kaynaklar. Her ikisi de düşünmeniz gerekir, çünkü ilk ulaşılan sınır etkin sınırdır.

İş gereksinimleri genellikle ihtiyaç duyacağınız dizin sayısını belirler. Örneğin, büyük bir belge deposu için genel bir dizine ihtiyacınız bulunabilir. Ya da bölge, uygulama veya iş kolu tabanlı birden çok dizine ihtiyacınız bulunabilir.

Bir dizinin boyutunu öğrenmek için [bir tane oluşturmanız](search-what-is-an-index.md)gerekir. Boyutu, içeri aktarılan verileri ve öneri araçları, filtrelemesini ve sıralamayı etkinleştirip etkinleştirmeyeceğinizi, dizin yapılandırmasını temel alır.

Tam metin arama için, birincil veri yapısı, kaynak verilerden farklı özelliklere sahip olan [ters bir dizin](https://en.wikipedia.org/wiki/Inverted_index) yapısıdır. Ters bir dizin için boyut ve karmaşıklık, içeriğe göre belirlenir, bu, içinde yer alan veri miktarına göre değildir. Yüksek artıklığa sahip büyük bir veri kaynağı, yüksek oranda değişken içerik içeren küçük bir veri kümesinden daha küçük bir dizin oluşmasına neden olabilir. Bu nedenle, özgün veri kümesinin boyutuna bağlı olarak dizin boyutunu çıkarsmak nadiren mümkündür.

> [!NOTE] 
> Dizinler ve depolama için gelecekteki ihtiyaçları tahmin etmek de tahmin etmek gibi görünse de bunun yapılması gerekir. Bir katmanın kapasitesi çok düşük olursa, daha yüksek bir katmanda yeni bir hizmet sağlamanız ve ardından [dizinlerinizi yeniden yüklemeniz](search-howto-reindex.md)gerekir. Bir hizmetin bir SKU 'dan diğerine yerinde yükseltilmesi gerekmez.
>

### <a name="estimate-with-the-free-tier"></a>Ücretsiz katman ile tahmin edin

Kapasiteyi tahmin etmek için bir yaklaşım ücretsiz katmanla başlamadır. Ücretsiz hizmetin üç Dizin, 50 MB depolama alanı ve 2 dakikalık dizin oluşturma süresi sunduğunu unutmayın. Tahmini bir dizin boyutunu bu kısıtlamalarla tahmin etmek zor olabilir, ancak bunlar şu adımlardan biri:

+ [Ücretsiz bir hizmet oluşturun](search-create-service-portal.md).
+ Küçük, temsili bir veri kümesi hazırlayın.
+ [Portalda bir başlangıç dizini oluşturun](search-get-started-portal.md) ve boyutunu aklınızda edin. Özellikler ve özniteliklerin depolama üzerinde bir etkisi vardır. Örneğin, öneri araçları (arama-yazma sorguları) ekleme, depolama gereksinimlerini artıracaktır. Aynı veri kümesini kullanarak, Depolama gereksinimlerinin nasıl değişeceğini görmek için, her bir alanda farklı özniteliklere sahip bir dizinin birden çok sürümünü oluşturmayı deneyebilirsiniz. Daha fazla bilgi için [temel dizin oluşturma içindeki "depolama etkileri"](search-what-is-an-index.md#index-size)başlığına bakın.

El ile kabaca bir tahmin sayesinde, bu miktarı iki dizin (geliştirme ve üretim) için bütçeye katmanızı ve ardından katmanınızı uygun şekilde seçmenizi sağlayabilirsiniz.

### <a name="estimate-with-a-billable-tier"></a>Faturalandırılabilir katman ile tahmin

Adanmış kaynaklar, geliştirme sırasında dizin miktarının, boyutunun ve sorgu birimlerinin daha gerçekçi tahminleri için daha büyük örnekleme ve işleme sürelerine uyum sağlayabilir. Bazı müşteriler, faturalandırılabilir bir katmanda doğrudan geçiş yaparken geliştirme projesi olarak yeniden değerlendirilir.

1. Alt katmanların ihtiyacınız olan dizin sayısını destekleyip desteklemediğini öğrenmek için [Her katmandaki hizmet sınırlarını gözden geçirin](./search-limits-quotas-capacity.md#index-limits) . Temel, S1 ve S2 katmanlarında Dizin sınırları sırasıyla 15, 50 ve 200 ' dir. Depolama için Iyileştirilmiş katmanın, az sayıda çok büyük dizini destekleyecek şekilde tasarlandığından 10 Dizin sınırı vardır.

1. [Faturalanabilir katmanda bir hizmet oluşturun](search-create-service-portal.md):

    + Tahmini yük hakkında emin değilseniz, temel veya S1 ' te düşük ' ı başlatın.
    + Büyük ölçekli dizin oluşturma ve sorgu yüklerine sahip olduğunuzu biliyorsanız, S2 veya hatta S3 ' da yüksek bir başlangıç yapın.
    + Depolama ile en Iyileştirilmiş, L1 veya L2 ile başlayın; büyük miktarda veri dizinleniyor ve sorgu yükü görece düşükse, dahili bir iş uygulamasında olduğu gibi.

1. Kaynak verilerin bir dizine nasıl çevrilip dönüştürmediğine yönelik [bir başlangıç dizini oluşturun](search-what-is-an-index.md) . Bu, dizin boyutunu tahmin etmenin tek yoludur.

1. Portalda [depolama, hizmet limitleri, sorgu hacmi ve gecikme süresini izleyin](search-monitor-usage.md) . Portal, saniye başına sorgular, kısıtlanmış sorgular ve arama gecikme süresi gösterir. Bu değerlerin tümü, doğru katmanı seçtiğinizden karar vermenize yardımcı olabilir. 

Dizin numarası ve boyutu, analizinizi eşit derecede önemlidir. Bunun nedeni, en fazla depolama (bölüm) kullanımı veya kaynakların (dizinler, Dizin oluşturucular, vb.) en üst limitlerinin (hangisi önce geldiğini) tam olarak ulaşılmasını içerir. Portal, Genel Bakış sayfasında geçerli kullanımı ve en fazla sınırları yan yana göstererek her ikisini de izlemenize yardımcı olur.

> [!NOTE]
> Belgeler, fazlalık veriler içeriyorsa depolama gereksinimleri daraltılabilir. İdeal olarak, belgeler yalnızca arama deneyimi için gereken verileri içerir. İkili veriler aranabilir değildir ve ayrı ayrı depolanmalıdır (Belki de bir Azure tablosu veya blob depolamada olabilir). Bir alan, dış verilere bir URL başvurusu tutmak için dizine eklenmelidir. Tek bir belgenin en büyük boyutu 16 MB 'tır (veya bir istekte birden çok belgeyi toplu olarak karşıya yüklüyorsanız, daha az). Daha fazla bilgi için bkz. [Azure bilişsel arama hizmet limitleri](search-limits-quotas-capacity.md).
>

**Sorgu hacmi konuları**

Saniyedeki sorgu sayısı (QPS), performans ayarlama sırasında önemli bir ölçümdür, ancak genellikle bir katman, mantıksal olarak yüksek sorgu hacmi beklemeniz durumunda yalnızca bir katman önem taşımaktadır.

Standart katmanlar, çoğaltmaları ve bölümleri dengeleyebilir. Yük Dengeleme için çoğaltmalar ekleyerek veya paralel işleme için bölüm eklemek yoluyla sorgu kapatma işlemini artırabilirsiniz. Daha sonra hizmet sağlandıktan sonra performansı ayarlayabilirsiniz.

Outset 'ten yüksek hacimli sorgu birimleri düşünüyorsanız, daha güçlü donanımlar tarafından desteklenen, daha yüksek standart katmanları göz önünde bulundurmanız gerekir. Daha sonra, bu sorgu birimleri gerçekleşmese de bölümleri ve çoğaltmaları çevrimdışı duruma geçirebilir veya alt katman bir hizmete geçebilirsiniz. Sorgu verimini hesaplama hakkında daha fazla bilgi için bkz. [Azure bilişsel arama performansı ve iyileştirmesi](search-performance-optimization.md).

Depolama için Iyileştirilmiş katmanlar, büyük veri iş yükleri için yararlıdır ve sorgu gecikme süresi gereksinimleri daha az önemli olduğunda için daha fazla genel kullanılabilir dizin depolaması destekler. Yük Dengeleme için ek çoğaltmalar ve paralel işleme için ek bölümler kullanmanız gerekir. Daha sonra hizmet sağlandıktan sonra performansı ayarlayabilirsiniz.

**Hizmet düzeyi sözleşmeleri**

Ücretsiz katman ve Önizleme özellikleri [hizmet düzeyi sözleşmeleri (SLA 'lar)](https://azure.microsoft.com/support/legal/sla/search/v1_0/)sağlamaz. Tüm faturalanabilir katmanlar için, hizmet için yeterli artıklık sağladığınızda SLA 'Lar devreye girer. Sorgu (okuma) SLA 'Ları için iki veya daha fazla kopyaya sahip olmanız gerekir. Sorgu ve dizin oluşturma (okuma-yazma) SLA 'Ları için üç veya daha fazla kopyaya sahip olmanız gerekir. Bölüm sayısı SLA 'Ları etkilemez.

## <a name="tips-for-tier-evaluation"></a>Katman değerlendirmesi için ipuçları

+ Ölçümlerin sorguları etrafında derleme yapmasına ve kullanım desenlerinde (iş saatleri sırasında sorgular, yoğun olmayan saatlerde dizin oluşturma) veri toplamasına izin verin. Hizmet sağlama kararlarını bilgilendirmek için bu verileri kullanın. Saatlik veya günlük temposunda pratik olmasa da, sorgu birimlerindeki planlı değişikliklere uyum sağlamak için bölümleri ve kaynakları dinamik olarak ayarlayabilirsiniz. Ayrıca, düzeylerin işlem yapmak için yeterince uzun sürmesi durumunda planlanmamış ancak sürekli değişikliklere de uyum sağlayabilirsiniz.

+ Sağlama altındaki tek alt tarafının, gerçek gereksinimler tahmine göre daha büyükse bir hizmeti bölmek zorunda kalmamanız gerektiğini unutmayın. Hizmet kesintisi yaşamamak için, daha yüksek bir katmanda yeni bir hizmet oluşturun ve tüm uygulamalar ve istekler yeni uç noktayı hedefleyerek yan yana çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Ücretsiz bir katmanla başlayın ve özelliklerini anlamak için verilerinizin bir alt kümesini kullanarak bir başlangıç dizini oluşturun. Azure Bilişsel Arama veri yapısı ters bir dizin yapısıdır. Ters çevrilen bir dizinin boyutu ve karmaşıklığı içeriğe göre belirlenir. Yüksek oranda yedekli içeriğin çok sayıda düzensiz içerikten daha küçük bir dizine neden olduğunu unutmayın. Bu nedenle, veri kümesinin boyutu yerine içerik özellikleri dizin depolama gereksinimlerini belirlemektir.

Dizin boyutunuzu ilk tahmine sahip olduktan sonra, bu makalede ele alınan katmanlardan birinde [faturalandırılabilir bir hizmet sağlayın](search-create-service-portal.md) : temel, standart veya depolama için iyileştirilmiş. Veri boyutlandırmasına ilişkin yapay kısıtlamaları rahatlaın ve dizinizi, aranabilir olmasını istediğiniz tüm verileri içerecek şekilde [yeniden derleyin](search-howto-reindex.md) .

İhtiyaç duyduğunuz performans ve ölçeği almak için gereken [bölümleri ve çoğaltmaları ayırın](search-capacity-planning.md) .

Performans ve kapasite iyi durumdaysa, işiniz bitti demektir. Aksi takdirde, gereksinimlerinize göre daha yakından hizalanan bir arama hizmetini farklı bir katmanda yeniden oluşturun.

> [!NOTE]
> Sorularınız varsa, [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) ' e gönderin veya [Azure desteği ile iletişime geçin](https://azure.microsoft.com/support/options/).