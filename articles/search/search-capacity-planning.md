---
title: Sorgu ve dizin iş yükleri için kapasite ayarlama
titleSuffix: Azure Cognitive Search
description: Her kaynağın faturalandırılabilir arama birimlerinde fiyatlandırıldığı Azure Bilişsel Arama'da bölümleme ve çoğaltma bilgisayar kaynaklarını ayarlayın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: 270ff3c3e8e4cffbb1f4b1987ee497530d0c0982
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546256"
---
# <a name="adjust-capacity-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da kapasite ayarlama

[Bir arama hizmetini sağlamadan](search-create-service-portal.md) ve belirli bir fiyatlandırma katmanına kilitlemeden önce, bir hizmetteki yinelemelerin ve bölümlerin rolünü ve bir hizmeti kaynak talebindeki ani artışlara ve düşüşlere uyum sağlayacak şekilde nasıl ayarlayabileceğinizi anlamak birkaç dakikanızı ayırın.

Kapasite seçtiğiniz [katmanın](search-sku-tier.md) bir işlevidir (katmanlar donanım özelliklerini belirler) ve öngörülen iş yükleri için gerekli çoğaltma ve bölüm birleşimidir. Ayarlamanın katmanına ve boyutuna bağlı olarak, kapasite ekleme veya azaltma kapasitesi 15 dakikaile birkaç saat arasında sürebilir. 

Yinelemelerin ve bölümlerin tahsisini değiştirirken Azure portalını kullanmanızı öneririz. Portal, bir kademenin maksimum sınırlarının altında kalan izin verilen kombinasyonlara sınırlamalar uygular. Ancak, komut dosyası tabanlı veya kod tabanlı bir sağlama yaklaşımına ihtiyaç duyarsanız, [Azure PowerShell](search-manage-powershell.md) veya [Management REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/services) alternatif çözümlerdir.

## <a name="terminology-replicas-and-partitions"></a>Terminoloji: kopyaları ve bölümleri

|||
|-|-|
|*Bölümler* | Okuma/yazma işlemleri için dizin depolama ve G sağlar (örneğin, bir dizini yeniden oluşturma veya yenileme). Her bölüm toplam dizin payına sahiptir. Üç bölüm ayırırsanız, dizininiz üçte bire bölünür. |
|*Çoğaltmalar* | Öncelikle bakiye sorgusu işlemlerini yüklemek için kullanılan arama hizmeti örnekleri. Her yineleme bir dizin bir kopyasıdır. Üç yineleme ayırırsanız, sorgu isteklerine hizmet vermek için kullanılabilir bir dizinin üç kopyasına sahip olursunuz.|

## <a name="when-to-add-nodes"></a>Düğümler ne zaman eklenir?

Başlangıçta, bir hizmet, bir bölüm ve bir yinelemeden oluşan en az düzeyde kaynak tahsis edilir. 

Tek bir hizmetin tüm iş yüklerini (dizin oluşturma ve sorguları) işlemek için yeterli kaynağa sahip olması gerekir. Arka planda ne iş yükü çalışır. Dizin oluşturmayı sorgu isteklerinin doğal olarak daha az sıklıkta olduğu zamanlar için zamanlayabilirsiniz, ancak hizmet bir göreve diğerine göre öncelik vermez. Ayrıca, hizmetler veya düğümler dahili olarak güncelleştirildiğinde, belirli bir miktar fazlalık sorgu performansını düzelter.

Genel bir kural olarak, arama uygulamaları, özellikle hizmet işlemleri sorgu iş yüklerine karşı önyargılı olduğunda, bölümlerden daha fazla yinelemeye ihtiyaç duyma eğilimindedir. [Yüksek kullanılabilirlik](#HA) bölümü nedenini açıklar.

> [!NOTE]
> Daha fazla yineleme veya bölüm eklemek, hizmeti çalıştırma maliyetini artırır ve sonuçların nasıl sıralandırılına benzer küçük farklılıklara neden olabilir. Daha fazla düğüm eklemenin faturalandırma etkilerini anlamak için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kontrol ettiğinizden emin olun. [Aşağıdaki grafik,](#chart) belirli bir yapılandırma için gereken arama birimi sayısını çapraz referans olarak göndermenize yardımcı olabilir. Ek yinelemelerin sorgu işlemeyi nasıl etkilediği hakkında daha fazla bilgi için [bkz.](search-pagination-page-layout.md#ordering-results)

## <a name="how-to-allocate-replicas-and-partitions"></a>Yinelemeler ve bölümler nasıl tahsis edilir?

1. [Azure portalında](https://portal.azure.com/) oturum açın ve arama hizmetini seçin.

1. **Ayarlar'da,** yinelemeleri ve bölümleri değiştirmek için **Ölçek** sayfasını açın. 

   Aşağıdaki ekran görüntüsü, tek bir yineleme ve bölümle birlikte sağlanan standart bir hizmeti gösterir. Alttaki formül kaç arama birimi kullanıldığını gösterir (1). Birim fiyat 100 TL (gerçek bir fiyat değil) ise, bu hizmeti çalıştırmanın aylık maliyeti ortalama 100 TL olacaktır.

   ![Geçerli değerleri gösteren ölçek sayfası](media/search-capacity-planning/1-initial-values.png "Geçerli değerleri gösteren ölçek sayfası")

1. Bölüm sayısını artırmak veya azaltmak için kaydırıcıyı kullanın. Alttaki formül kaç arama birimi kullanıldığını gösterir.

   Bu örnek, her biri iki yineleme ve bölümle kapasiteyi ikikatına çıkar. Arama birimi sayısına dikkat edin; faturalandırma formülü bölmelerle çarpıldığından (2 x 2) artık dört olur. Kapasiteyi iki katına çıkarmak, hizmeti çalıştırma nın maliyetini iki katına çıkar. Arama birimi maliyeti 100 $ olsaydı, yeni aylık fatura şimdi 400 $ olacaktır.

   Her kademenin birim başına geçerli maliyetleri için [Fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/search/)ziyaret edin.

   ![Yinelemeler ve bölümler ekleme](media/search-capacity-planning/2-add-2-each.png "Yinelemeler ve bölümler ekleme")

1. Değişiklikleri onaylamak için **Kaydet'i** tıklatın.

   ![Ölçek ve faturalandırmadaki değişiklikleri onaylama](media/search-capacity-planning/3-save-confirm.png "Ölçek ve faturalandırmadaki değişiklikleri onaylama")

   Kapasite değişikliklerinin tamamlanması birkaç saat sürer. İşlem başladıktan sonra iptal edemezsiniz ve yineleme ve bölüm ayarlamaları için gerçek zamanlı izleme yoktur. Ancak, değişiklikler devam ederken aşağıdaki ileti görünür kalır.

   ![Portaldaki durum iletisi](media/search-capacity-planning/4-updating.png "Portaldaki durum iletisi")

> [!NOTE]
> Bir hizmet sağlandıktan sonra daha yüksek bir katmana yükseltilemez. Yeni katmanda bir arama hizmeti oluşturmanız ve dizinlerinizi yeniden yüklemeniz gerekir. Bkz. Hizmet sağlama konusunda yardım için [portalda bir Azure Bilişsel Arama hizmeti oluşturun.](search-create-service-portal.md)
>
> Ayrıca, bölümler ve yinelemeler hizmet tarafından yalnızca ve dahili olarak yönetilir. İşlemci yakınlığı kavramı veya belirli bir düğüme iş yükü atama yoktur.
>

<a id="chart"></a>

## <a name="partition-and-replica-combinations"></a>Bölme ve çoğaltma kombinasyonları

Temel hizmet, en fazla üç SUs sınırı için tam olarak bir bölüme ve en fazla üç yinelemeye sahip olabilir. Düzeltilebilir tek kaynak yinelemedir. Sorgularda yüksek kullanılabilirlik için en az iki yineleme gerekir.

Tüm Standart ve Depolama Optimize edilmiş arama hizmetleri, 36-SU sınırına tabi olarak aşağıdaki yineleme ve bölüm kombinasyonlarını üstlenebilir. 

|   | **1 bölüm** | **2 bölüm** | **3 bölüm** | **4 bölüm** | **6 bölüm** | **12 bölüm** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 çoğaltma** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 yineleme** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 yineleme** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 yineleme** |4 SU |8 SU |12 SU |16 SU |24 SU |Yok |
| **5 çoğaltma** |5 SU |10 SU |15 SU |20 SU |30 SU |Yok |
| **6 çoğaltma** |6 SU |12 SU |18 SU |24 SU |36 SU |Yok |
| **12 kopya** |12 SU |24 SU |36 SU |Yok |Yok |Yok |

SU'lar, fiyatlandırma ve kapasite Azure web sitesinde ayrıntılı olarak açıklanır. Daha fazla bilgi için [Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/search/)bakın.

> [!NOTE]
> Yineleme ve bölüm sayısı eşit olarak 12'ye bölünür (özellikle 1, 2, 3, 4, 6, 12). Bunun nedeni, Azure Bilişsel Arama'nın her dizini 12 parçaya önceden bölmesi ve böylece tüm bölümlere eşit bölümlerhalinde yayılmasıdır. Örneğin, hizmetinizin üç bölümü varsa ve bir dizin oluşturursanız, her bölüm dizin dört parça içerir. Azure Bilişsel Arama'nın bir dizini nasıl parçalayıştırdığını, gelecekteki sürümlerde değişiklik olacak bir uygulama ayrıntısi dir. Bugün sayı 12 olmasına rağmen, bu sayının gelecekte her zaman 12 olmasını beklememalısınız.
>

<a id="HA"></a>

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Ölçeklendirmek kolay ve nispeten hızlı olduğundan, genellikle bir bölüm ve bir veya iki yinelemeyle başlamanızı ve sorgu birimleri oluştururken ölçeklendirmenizi öneririz. Sorgu iş yükleri öncelikle yinelemelerde çalışır. Daha fazla iş elde veya yüksek kullanılabilirlik gerekiyorsa, büyük olasılıkla ek yinelemeler gerekir.

Yüksek kullanılabilirlik için genel öneriler şunlardır:

* Salt okunur iş yüklerinin (sorgular) yüksek kullanılabilirliği için iki yineleme

* Okuma/yazma iş yüklerinin yüksek kullanılabilirliği için üç veya daha fazla yineleme (tek tek belgeler eklendikçe, güncelleştirildikçe veya silindikçe sorgular artı dizin oluşturma)

Azure Bilişsel Arama için hizmet düzeyi anlaşmaları (SLA), belge ekleme, güncelleştirme veya silme den oluşan sorgu işlemlerine ve dizin güncelleştirmelerine hedeflenir.

Temel katman, bir bölüm ve üç yinelemede en üst düzeydedir. Hem dizin oluşturma hem de sorgu iş bölümü için talepteki dalgalanmalara anında yanıt verme esnekliği istiyorsanız, Standart katmanlardan birini düşünün.  Depolama gereksinimlerinizin sorgu iş akışınızdan çok daha hızlı büyüdüğünü düşünüyorsanız, Depolama Optimize Edilmiş katmanlardan birini düşünün.

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Şu anda, olağanüstü durum kurtarma için yerleşik bir mekanizma yoktur. Bölüm veya yineleme eklemek, olağanüstü durum kurtarma hedeflerini karşılamak için yanlış bir strateji olacaktır. En yaygın yaklaşım, başka bir bölgede ikinci bir arama hizmeti ayarlayarak hizmet düzeyinde artıklık eklemektir. Dizin yeniden oluşturma sırasında kullanılabilirlik olduğu gibi, yeniden yönlendirme veya başarısız mantığı kodunuzdan gelmelidir.

## <a name="estimate-replicas"></a>Tahmin yinelemeleri

Bir üretim hizmetinde, SLA amaçları için üç yineleme ayırmanız gerekir. Yavaş sorgu performansı yla karşılaşırsanız, daha büyük sorgu iş yüklerini desteklemek ve istekleri birden çok yineleme üzerinde yüklemek için dizin ek kopyalarının çevrimiçi duruma getirilmesi için yinelemeler ekleyebilirsiniz.

Sorgu yüklerini karşılamak için kaç yinelemenin gerekli olduğuna ilişkin yönergeler sağlamayız. Sorgu performansı, sorgunun karmaşıklığına ve rakip iş yüklerine bağlıdır. Yinelemelerin eklenmesi açıkça daha iyi performansla sonuçlanmasına rağmen, sonuç kesinlikle doğrusal değildir: üç yineleme eklemek üçlü iş elde etme garantisi vermez.

Çözümünüz için QPS tahmininde kılavuz için [performans için Ölçek](search-performance-optimization.md)ve [Sorguları İzle](search-monitor-queries.md)

## <a name="estimate-partitions"></a>Bölümleri tahmin edin

[Seçtiğiniz katman](search-sku-tier.md) bölüm boyutunu ve hızını belirler ve her katman çeşitli senaryolara uyan bir dizi özellik etrafında optimize edilir. Daha yüksek uç bir katman seçerseniz, S1 ile giderseniz daha az bölüm gerekebilir. Kendi kendini yöneten sınama yoluyla yanıtlamanız gereken sorulardan biri, daha büyük ve daha pahalı bir bölümün daha düşük bir katmanda sağlanan bir hizmetteki iki ucuz bölümden daha iyi performans verip vermediğidir.

Yakın gerçek zamanlı veri yenileme gerektiren arama uygulamaları, yinelemelerden orantılı olarak daha fazla bölüme ihtiyaç duyar. Bölümler ekleme, okuma/yazma işlemlerini daha fazla sayıda bilgi işlem kaynağına dağıtır. Ayrıca ek dizinler ve belgeler depolamak için daha fazla disk alanı sağlar.

Daha büyük dizinlerin sorgulanması daha uzun sürer. Bu nedenle, bölümlerdeki her artımlı artışın yinelemelerde daha küçük ama orantılı bir artış gerektirdiğini görebilirsiniz. Sorgularınızın ve sorgu biriminizin karmaşıklığı, sorgu yürütmenin ne kadar hızlı döndürüldünüzü hesaba katacaktır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için bir fiyatlandırma katmanı seçin](search-sku-tier.md)