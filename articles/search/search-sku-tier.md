---
title: Fiyatlandırma katmanı seçin
titleSuffix: Azure Cognitive Search
description: 'Azure Bilişsel Arama şu katmanlarda sağlanabilir: ücretsiz, temel ve standart ve standart, çeşitli kaynak yapılandırmalarında ve kapasite düzeylerinde kullanılabilir.'
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/15/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: 4c58968cb6a38a10433915ec8fa00336ccad301e
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98216419"
---
# <a name="choose-a-pricing-tier-for-azure-cognitive-search"></a>Azure Bilişsel Arama için bir fiyatlandırma katmanı seçin

[Bir arama hizmeti oluşturduğunuzda](search-create-service-portal.md), hizmetin kullanım ömrü boyunca düzeltilen bir fiyatlandırma katmanı seçersiniz. Seçtiğiniz katman şunları belirler:

+ Oluşturabileceğiniz dizinlerin ve diğer nesnelerin miktarı (en fazla limit)
+ Bölümlerin boyutu ve hızı (fiziksel depolama)
+ Faturalanabilir ücret, sabit bir aylık maliyet, ancak aynı zamanda bölümler veya çoğaltmalar eklerseniz artımlı maliyet

Ayrıca, bazı [Premium özellikler](#premium-features) katman gereksinimleriyle birlikte gelir.

## <a name="tier-descriptions"></a>Katman açıklamaları

Katmanlar **ücretsiz**, **temel**, **Standart** ve depolama için **iyileştirilmiş**. Standart ve depolama için Iyileştirilmiş çeşitli yapılandırmalarda ve kapasitelerde kullanılabilir.

Azure portal aşağıdaki ekran görüntüsünde, kullanılabilir katmanlar, eksi fiyatlandırma (portalda ve [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/)bulabilirsiniz) gösterilmektedir. 

![Azure Bilişsel Arama fiyatlandırma katmanları](media/search-sku-tier/tiers.png "Azure Bilişsel Arama fiyatlandırma katmanları")

**Ücretsiz** , öğreticileri ve kod örneklerini çalıştırma gibi daha küçük projeler için sınırlı bir arama hizmeti oluşturur. Dahili olarak, çoğaltmalar ve bölümler birden çok abone arasında paylaşılır. Ücretsiz bir hizmeti ölçeklendiremez veya önemli iş yüklerini çalıştıramazsınız.

**Temel** ve **Standart** , **Standart** olarak varsayılan değer olan en yaygın olarak kullanılan faturalanabilir katmanlardır. Denetiminizin altında adanmış kaynaklar sayesinde, daha büyük projeler dağıtabilir, performansı iyileştirir ve kapasiteyi artırabilirsiniz.

Bazı katmanlar belirli iş türleri için iyileştirilmiştir. Örneğin, **Standart 3 yüksek yoğunluklu (S3 HD)** , S3 için bir *barındırma modudur* ; burada temeldeki donanım çok sayıda daha küçük dizinler için optimize edilmiştir ve çok kiracılı senaryolar için tasarlanmıştır. S3 HD, S3 ile aynı birim başına ücretine sahiptir, ancak donanım çok sayıda daha küçük dizin üzerinde hızlı dosya okuma için en iyi duruma getirilmiştir.

**Depolama Için iyileştirilmiş** katmanlar, standart katmanlardan TB başına daha büyük depolama kapasitesi sunar. Birincil zorunluluğunu getirir, belirli uygulama gereksinimleriniz için doğrulamanız gereken daha yüksek sorgu gecikmedir. Bu katmanın performans konuları hakkında daha fazla bilgi için bkz. [performans ve iyileştirme konuları](search-performance-optimization.md).

[Fiyatlandırma sayfasındaki](https://azure.microsoft.com/pricing/details/search/)çeşitli katmanlar hakkında daha fazla bilgi edinmek için bkz. [Azure bilişsel arama 'deki hizmet sınırları](search-limits-quotas-capacity.md) ve hizmet sağlanırken Portal sayfası.

<a name="premium-features"></a>

## <a name="feature-availability-by-tier"></a>Katmana göre özellik kullanılabilirliği

Birçok özellik, ücretsiz katman dahil olmak üzere tüm katmanlarda kullanılabilir. Birkaç durumda, seçtiğiniz katman bir özelliği uygulama yeteneğinizi etkiler. Aşağıdaki tabloda hizmet katmanıyla ilgili özellik kısıtlamaları açıklanmaktadır.

| Öne çıkan özelliği | Sınırlamalar |
|---------|-------------|
| [Dizinleyiciler](search-indexer-overview.md) | Dizin oluşturucular S3 HD üzerinde kullanılamaz.  |
| [Yapay zeka zenginleştirme](search-security-manage-encryption-keys.md) | Ücretsiz katmanda çalışır, ancak önerilmez. |
| [Müşteri tarafından yönetilen şifreleme anahtarları](search-security-manage-encryption-keys.md) | Ücretsiz katmanda kullanılamaz. |
| [IP güvenlik duvarı erişimi](service-configure-firewall.md) | Ücretsiz katmanda kullanılamaz. |
| [Özel uç nokta (Azure özel bağlantısı ile tümleştirme)](service-create-private-endpoint.md) | Bir arama hizmetine gelen bağlantılar için ücretsiz katmanda kullanılamaz. Diğer Azure kaynaklarına yönelik Dizin oluşturucular tarafından giden bağlantılar için ücretsiz veya S3 HD 'de kullanılamaz. Becerileri kullanan Dizin oluşturucular için, ücretsiz, temel, S1 veya S3 HD ' de kullanılamaz.|

Yeterli kapasiteye sahip değilseniz, Kaynak yoğunluklu Özellikler iyi çalışmayabilir. Örneğin, [AI zenginleştirme](cognitive-search-concept-intro.md) , veri kümesi küçük olmadığı sürece ücretsiz bir hizmette zaman aşımına uğrar uzun süreli yetenekler içerir.

## <a name="billable-events"></a>Faturalandırılabilir olaylar

Azure Bilişsel Arama 'de oluşturulan bir çözüm, aşağıdaki yollarla maliyette ücret alabilir:

+ [Hizmetin](#service-costs) , en düşük düzeyde (bir bölüm ve çoğaltma), temel fiyata 7/24 çalışan maliyeti. Bunu, hizmeti çalıştırmanın sabit maliyeti olarak düşünebilirsiniz.

+ Maliyetlerin faturalanabilir ücret artışlarına göre artması kapasitesi (çoğaltmalar veya bölümler) ekleme

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

Azure veri kaynağı Azure Bilişsel Arama farklı bir bölgedeyse, [Dizin oluşturucular](search-indexer-overview.md) kullanmak faturalandırmayı etkileyebilir. Bu senaryoda, giden verileri Azure veri kaynağından Azure Bilişsel Arama taşıma maliyeti olabilir. Ayrıntılar için, söz konusu Azure veri platformunun fiyatlandırma sayfalarına bakın.

Azure Bilişsel Arama hizmetini verileriniz ile aynı bölgede oluşturursanız, veri çıkış ücretlerini tamamen ortadan kaldırabilirsiniz. [Bant genişliği fiyatlandırma sayfasından](https://azure.microsoft.com/pricing/details/bandwidth/)bazı bilgiler aşağıda verilmiştir:

+ Gelen veriler: Microsoft, Azure 'daki herhangi bir hizmete gelen veriler için ücret ödemez. 

+ Giden veriler: giden veriler sorgu sonuçlarına başvurur. Bilişsel Arama giden veriler için ücret alınmaz, ancak hizmetler farklı bölgelerde olduğunda Azure 'daki giden ücretler mümkündür. Bu ücretler aslında Azure Bilişsel Arama faturanızın bir parçası değildir. Bunlar burada bahsedildikleri için, diğer bölgelere veya Azure olmayan uygulamalara sonuç gönderiyorsanız, bu maliyetlerin genel faturanızda yansıtıldığını görebilirsiniz.

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

## <a name="next-steps"></a>Sonraki adımlar

Maliyet yönetimi, kapasite planlamasının ayrılmaz bir parçasıdır. Bir sonraki adım olarak, kapasiteyi tahmin etme ve maliyetleri yönetme hakkında rehberlik için aşağıdaki makaleye devam edin.

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama maliyetleri ve tahmini kapasiteyi yönetme](search-sku-manage-costs.md)