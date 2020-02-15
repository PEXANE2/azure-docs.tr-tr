---
title: Sorgu ve Dizin iş yükleri için kapasiteyi ayarla
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de, her kaynağın faturalandırılabilir arama birimlerinde fiyatlandırıldıkları bölüm ve çoğaltma bilgisayar kaynaklarını ayarlayın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: bc90ecb029afe70ed61e94a727c67c53bb968b96
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77212555"
---
# <a name="adjust-capacity-in-azure-cognitive-search"></a>Azure Bilişsel Arama kapasiteyi ayarlama

Belirli bir fiyatlandırma katmanında [bir arama hizmetini](search-create-service-portal.md) ve kilitlemeyi sağlamadan önce, bir hizmette bulunan kopyaların ve bölümlerin rolünü anlamak için birkaç dakikanızı alın, bu durumda orantılı veya daha hızlı bölümler gerekip gerekmediğini ve hizmeti beklenen yük için nasıl yapılandırabileceğinizi öğrenin.

Kapasite, [seçtiğiniz katmanın](search-sku-tier.md) bir işlevidir (Katmanlar, donanım özelliklerini belirleme) ve tasarlanan iş yükleri için gereken çoğaltma ve bölüm birleşimini. Bu makale, çoğaltma ve bölüm birleşimlerine ve etkileşimlere odaklanır.

## <a name="terminology-replicas-and-partitions"></a>Terminoloji: çoğaltmalar ve bölümler

|||
|-|-|
|*Bölümler* | Okuma/yazma işlemleri için dizin depolama ve g/ç sağlar (örneğin, bir dizini yeniden oluşturma veya yenileme). Her bölümde toplam dizinin bir payı vardır. Üç bölüm ayırırsanız, dizininiz üçe bölünür. |
|*Çoğaltmaları* | Arama hizmeti örnekleri, öncelikli olarak sorgu işlemlerinin yükünü dengelemek için kullanılır. Her çoğaltma bir dizinin bir kopyasıdır. Üç çoğaltma ayırırsanız, sorgu isteklerine hizmet vermek için kullanılabilir bir dizinin üç kopyasına sahip olursunuz.|

## <a name="how-to-allocate-replicas-and-partitions"></a>Çoğaltmaları ve bölümleri ayırma

Başlangıçta, bir hizmete bir bölümden ve bir çoğaltmadan oluşan en az düzeyde kaynak ayrılır. 

Tek bir hizmetin tüm iş yüklerini (Dizin oluşturma ve sorgular) işlemek için yeterli kaynak olması gerekir. Hiçbir iş yükü arka planda çalışır. Sorgu isteklerinin doğal olarak daha az sıklıkla olduğu durumlarda Dizin oluşturmayı zamanlayabilirsiniz, ancak hizmet diğer bir görevi başka bir görev için önceliklendirmez.

Kopyaların ve bölümlerin tahsisini değiştirirken Azure portal kullanmanızı öneririz. Portal, bir katmanın en fazla limitlerini takip eden izin verilen birleşimler üzerinde sınırlar uygular. Ancak, komut dosyası tabanlı veya kod tabanlı bir sağlama yaklaşımına ihtiyacınız varsa [Azure PowerShell](search-manage-powershell.md) veya [Yönetim REST API](https://docs.microsoft.com/rest/api/searchmanagement/services) alternatif çözümlerdir.

Genel bir kural olarak, arama uygulamalarında, özellikle de hizmet işlemleri sorgu iş yüklerine yaklaşmanız durumunda bölümden daha fazla çoğaltma olması eğilimindedir. [Yüksek kullanılabilirlik](#HA) bölümünde bunun neden olduğu açıklanmaktadır.

1. [Azure Portal](https://portal.azure.com/) oturum açın ve arama hizmetini seçin.

1. **Ayarlar**' da, çoğaltmaları ve bölümleri değiştirmek için **Ölçek** sayfasını açın. 

   Aşağıdaki ekran görüntüsünde, bir çoğaltma ve bölüm ile sağlanan standart bir hizmet gösterilmektedir. Alttaki formül, kaç arama birimi kullanıldığını gösterir (1). Birim fiyatı $100 (gerçek fiyat değil) ise, bu hizmeti çalıştırmanın aylık maliyeti, ortalama olarak $100 ' dir.

   ![Geçerli değerleri gösteren sayfayı Ölçeklendir](media/search-capacity-planning/1-initial-values.png "Geçerli değerleri gösteren sayfayı Ölçeklendir")

1. Bölüm sayısını artırmak veya azaltmak için kaydırıcıyı kullanın. Alttaki formül, kaç tane arama birimi kullanıldığını gösterir.

   Bu örnek, iki çoğaltmalarla ve her bir bölümden oluşan kapasiteyi iki katına çıkarır. Arama Birimi sayısına dikkat edin; Fatura formülü bölümler (2 x 2) ile çarpıldığı için artık dördü vardır. Katlama kapasitesi, hizmeti çalıştırmanın maliyetini iki katına çıkarır. Arama birimi maliyeti $100 ise, yeni aylık fatura artık $400 olacaktır.

   Her katmanın birim maliyetlerine göre geçerli olan [fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/search/)ziyaret edin.

   ![Çoğaltmalar ve bölümler ekleme](media/search-capacity-planning/2-add-2-each.png "Çoğaltmalar ve bölümler ekleme")

1. Değişiklikleri onaylamak için **Kaydet** ' e tıklayın.

   ![Ölçek ve faturalandırma Değişikliklerini Onayla](media/search-capacity-planning/3-save-confirm.png "Ölçek ve faturalandırma Değişikliklerini Onayla")

   Kapasite değişikliklerinin tamamlanması birkaç saat sürer. İşlem başladıktan sonra iptal edemezsiniz ve çoğaltma ve bölüm ayarlamaları için gerçek zamanlı izleme yoktur. Ancak, değişiklikler devam ederken aşağıdaki ileti görünür kalır.

   ![Portalda durum iletisi](media/search-capacity-planning/4-updating.png "Portalda durum iletisi")

> [!NOTE]
> Bir hizmet sağlandıktan sonra, daha yüksek bir katmana yükseltilemez. Yeni katmanda bir arama hizmeti oluşturmanız ve dizinlerinizi yeniden yüklemeniz gerekir. Hizmet sağlama konusunda yardım için bkz. [portalda Azure bilişsel arama hizmeti oluşturma](search-create-service-portal.md) .
>
> Ayrıca, bölümler ve çoğaltmalar, hizmet tarafından özel olarak ve dahili olarak yönetilir. İşlemci benzeşimi kavramı veya belirli bir düğüme iş yükü atama yoktur.
>

<a id="chart"></a>

## <a name="partition-and-replica-combinations"></a>Bölüm ve çoğaltma birleşimleri

Temel bir hizmet, en fazla üç SUs sınırı için tam olarak bir bölüme ve en fazla üç çoğaltmaya sahip olabilir. Yalnızca ayarlanabilir kaynak çoğaltmadır. Sorgularda yüksek kullanılabilirlik için en az iki çoğaltma gerekir.

Tüm standart ve depolama için Iyileştirilmiş arama hizmetleri, 36-SU sınırına tabi olan aşağıdaki çoğaltma ve bölüm birleşimlerini kabul edebilir. 

|   | **1 Bölüm** | **2 Bölüm** | **3 Bölüm** | **4 Bölüm** | **6 Bölüm** | **12 Bölüm** |
| --- | --- | --- | --- | --- | --- | --- |
| **1 çoğaltma** |1 SU |2 SU |3 SU |4 SU |6 SU |12 SU |
| **2 çoğaltma** |2 SU |4 SU |6 SU |8 SU |12 SU |24 SU |
| **3 çoğaltma** |3 SU |6 SU |9 SU |12 SU |18 SU |36 SU |
| **4 çoğaltma** |4 SU |8 SU |12 SU |16 SU |24 SU |Yok |
| **5 çoğaltma** |5 SU |10 SU |15 SU |20 SU |30 SU |Yok |
| **6 çoğaltma** |6 SU |12 SU |18 SU |24 SU |36 SU |Yok |
| **12 çoğaltma** |12 SU |24 SU |36 SU |Yok |Yok |Yok |

SUs, fiyatlandırma ve kapasite Azure Web sitesinde ayrıntılı olarak açıklanmıştır. Daha fazla bilgi için bkz. [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/search/).

> [!NOTE]
> Çoğaltma ve bölüm sayısı eşit olarak 12 ' ye bölünür (özellikle, 1, 2, 3, 4, 6, 12). Bunun nedeni, Azure Bilişsel Arama her bir dizini tüm bölümler arasında eşit kısımlara yayılabilecek şekilde 12 parçalara böler. Örneğin, hizmetinizin üç bölümü varsa ve bir dizin oluşturursanız, her bölüm dizin dört bölümden oluşur. Azure Bilişsel Arama Dizin oluşturma işlemi, sonraki sürümlerde değişikliğe tabi olan bir uygulama ayrıntısı. Sayı bugün 12 olsa da, bu sayının gelecekte her zaman 12 olması beklenmemelidir.
>

<a id="HA"></a>

## <a name="high-availability"></a>Yüksek kullanılabilirlik

Ölçeği hızla ve nispeten daha hızlı olduğundan, genellikle bir bölüm ve bir veya iki çoğaltma ile başlayıp daha sonra, sorgu birimleri oluşturma olarak ölçeklendirmeniz önerilir. Sorgu iş yükleri birincil olarak çoğaltmalar üzerinde çalışır. Daha fazla üretilen iş veya yüksek kullanılabilirliğe ihtiyacınız varsa, muhtemelen ek çoğaltmalar yapmanız gerekir.

Yüksek kullanılabilirlik için genel öneriler şunlardır:

* Salt okuma iş yüklerinin yüksek kullanılabilirliği için iki çoğaltma (sorgular)

* Okuma/yazma iş yüklerinin yüksek kullanılabilirliği için üç veya daha fazla çoğaltma (tek belgeler için sorgular ve dizin oluşturma eklenir, güncelleştirilir veya silinir)

Azure Bilişsel Arama için hizmet düzeyi sözleşmeleri (SLA), sorgu işlemlerine ve belge ekleme, güncelleştirme veya silme işlemlerinden oluşan Dizin güncelleştirmelerine yöneliktir.

Temel katman, bir bölümde ve üç çoğaltmadan çıkar. Hem dizin oluşturma hem de sorgu işleme için talepteki dalgalanmaların hemen yanıt vermesini istiyorsanız Standart katmanlardan birini göz önünde bulundurun.  Depolama gereksinimlerinizin sorgu aktarım süresinden çok daha hızlı bir şekilde büyümesini fark ederseniz, depolama için Iyileştirilmiş katmanlardan birini düşünün.

## <a name="disaster-recovery"></a>Olağanüstü durum kurtarma

Şu anda olağanüstü durum kurtarma için yerleşik bir mekanizma yoktur. Bölüm veya çoğaltmaları eklemek, olağanüstü durum kurtarma hedeflerini karşılamak için yanlış stratejidir. En yaygın yaklaşım, başka bir bölgede ikinci bir arama hizmeti ayarlayarak hizmet düzeyinde artıklık eklemektir. Dizin yeniden oluşturma sırasında kullanılabilirliğinde olduğu gibi, yeniden yönlendirme veya yük devretme mantığı koddan gelmelidir.

## <a name="estimate-replicas"></a>Çoğaltmaları tahmin etme

Bir üretim hizmetinde, SLA amaçları için üç çoğaltma ayırmanız gerekir. Yavaş sorgu performansına karşılaşıyorsanız, çoğaltma eklemek için bir çözüm daha büyük sorgu iş yüklerini destekleyecek ve isteklerin birden çok çoğaltma üzerinden yük dengelenmesi için çevrimiçi hale getirilir.

Sorgu yüklerini karşılamak için kaç yinelemenin gerekli olduğu konusunda yönergeler sağlamayız. Sorgu performansı, sorgunun karmaşıklığına ve rekabet iş yüklerine bağlıdır. Çoğaltmaları ekleme işlemi daha iyi performansa neden olsa da, sonuç kesinlikle doğrusal değildir: üç çoğaltma eklemek Üçlü üretilen iş garantisi vermez.

Çözümünüz için QPS tahmini konusunda rehberlik için bkz. performans ve [izleme sorguları](search-monitor-queries.md) [için ölçeklendirme](search-performance-optimization.md)

## <a name="estimate-partitions"></a>Tahmin bölümleri

[Seçtiğiniz katman](search-sku-tier.md) bölüm boyutunu ve hızını belirler ve her katman çeşitli senaryolara uyan bir dizi özellik etrafında en iyi duruma getirilir. Daha yüksek bir katman seçerseniz S1 ile devam ediyorsanız daha az bölüm gerekebilir.

Neredeyse gerçek zamanlı veri yenileme gerektiren uygulamalarda, çoğaltmalara göre orantılı daha fazla bölüm gerekecektir. Bölüm ekleme okuma/yazma işlemlerini çok sayıda bilgi işlem kaynağına yayar. Ayrıca ek dizinleri ve belgeleri depolamak için daha fazla disk alanı sağlar.

Daha büyük dizinler sorgu için daha uzun sürer. Bu nedenle, bölümlerdeki her artımlı artışın çoğaltmalarda daha küçük ancak orantılı bir artış gerektirdiğini fark edebilirsiniz. Sorgularınızın ve sorgu biriminizin karmaşıklığı, sorgu yürütmenin ne kadar hızlı bir şekilde bir şekilde çalıştığını çarpanlara katacaktır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Bilişsel Arama için bir fiyatlandırma katmanı seçin](search-sku-tier.md)