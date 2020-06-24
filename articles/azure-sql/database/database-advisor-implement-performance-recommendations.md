---
title: Azure SQL veritabanı için veritabanı Danışmanı performans önerileri
description: Azure SQL veritabanı, Azure SQL veritabanı 'nda sorgu performansını iyileştirebilecek veritabanları için öneriler sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: fasttrack-edit, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: 5a81ceea151b937b63544cbe51cc22de11d25230
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85254948"
---
# <a name="database-advisor-performance-recommendations-for-azure-sql-database"></a>Azure SQL veritabanı için Veritabanı Danışmanı performans önerileri
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure SQL veritabanı öğrenir ve uygulamanızla uyum sağlar. Azure SQL veritabanı, performansı en üst düzeye çıkarmanızı sağlayan özelleştirilmiş öneriler sağlayan çeşitli veritabanı danışmanları sunmaktadır. Bu veritabanı danışmanları sürekli olarak kullanım geçmişini değerlendirir ve analiz eder ve performansı artırmaya yardımcı olan iş yükü desenlerine göre öneriler sağlar.

## <a name="performance-overview"></a>Performansa genel bakış

Performansa genel bakış, veritabanı Performanslarınızın bir özetini sağlar ve performans ayarlama ve sorun giderme konusunda size yardımcı olur.

![Azure SQL veritabanı için performansa genel bakış](./media/database-advisor-implement-performance-recommendations/performance-overview-annotated.png)

- **Öneriler** kutucuğu, veritabanınız için ayarlama önerilerinin dökümünü sağlar (daha fazla varsa ilk üç öneri gösterilir). Bu Kutucuğa tıkladığınızda **[performans önerisi seçeneklerine](database-advisor-find-recommendations-portal.md#viewing-recommendations)** gidersiniz.
- **Ayarlama etkinliği** kutucuğu, veritabanınız için devam eden ve tamamlanmış ayarlama eylemlerinin bir özetini sağlar ve bu sayede ayarlama etkinliğinin geçmişine hızlı bir görünüm sağlar. Bu Kutucuğa tıkladığınızda veritabanınız için tam ayarlama geçmişi görünümüne gidersiniz.
- **Otomatik ayarlama** kutucuğu veritabanınız için **[otomatik ayarlama yapılandırmasını](automatic-tuning-enable.md)** gösterir (veritabanınıza otomatik olarak uygulanan ayarlama seçenekleri). Bu Kutucuğa tıkladığınızda Otomasyon yapılandırma iletişim kutusu açılır.
- **Veritabanı sorguları** kutucuğu, veritabanınızın sorgu performansının özetini gösterir (genel DTU kullanımı ve en üstteki kaynak kullanan sorgular). Bu Kutucuğa tıkladığınızda **[sorgu performansı içgörüleri](query-performance-insight-use.md)** için gidersiniz.

## <a name="performance-recommendation-options"></a>Performans önerisi seçenekleri

Azure SQL veritabanı 'nda kullanılabilen performans önerisi seçenekleri şunlardır:

| Performans önerisi | Tek veritabanı ve havuza alınmış veritabanı desteği | Örnek veritabanı desteği |
| :----------------------------- | ----- | ----- |
| **Dizin önerileri oluşturma** -iş yükünüzün performansını iyileştirebilecek dizinlerin oluşturulmasını önerir. | Yes | Hayır |
| **Dizin önerilerini bırakma** -benzersiz dizinler ve uzun süredir kullanılmamış dizinler dışında, yedekli ve yinelenen dizinlerin her gün kaldırılmasını önerir (>90 gün). Bu seçeneğin bölüm değiştirme ve Dizin ipuçlarını kullanan uygulamalarla uyumlu olmadığına lütfen unutmayın. Kullanılmayan dizinleri bırakma, Premium ve İş Açısından Kritik hizmet katmanlarında desteklenmez. | Yes | Hayır |
| **Sorgu önerilerini Parametreleştirme (Önizleme)** -sürekli olarak yeniden derlenen ancak aynı sorgu yürütme planına sahip bir veya daha fazla sorgunuz varsa, bu durumlarda zorunlu Parametreleştirme önerilir. | Yes | Hayır |
| **Şema sorunları önerilerini düzeltme (Önizleme)** -Azure SQL veritabanı, veritabanınızda gerçekleşen şemaya ilişkin SQL hatalarının sayısında bir anomali fark edildiğinde şema düzeltme için öneriler görüntülenir. Microsoft şu anda "şema sorununu çözme" önerilerini kullanımdan kaldıramamaktadır. | Yes | Hayır |

![Azure SQL veritabanı için performans önerileri](./media/database-advisor-implement-performance-recommendations/performance-recommendations-annotated.png)

Performans önerilerini uygulamak için bkz. [öneri uygulama](database-advisor-find-recommendations-portal.md#applying-recommendations). Önerilerin durumunu görüntülemek için bkz. [izleme işlemleri](database-advisor-find-recommendations-portal.md#monitoring-operations).

Geçmişte uygulanan ayarlama eylemlerinin tamamlanma geçmişini de bulabilirsiniz.

## <a name="create-index-recommendations"></a>Dizin önerileri oluşturma

Azure SQL veritabanı, çalıştıran sorguları sürekli izler ve performansı iyileştirebilecek dizinleri tanımlar. Belirli bir dizinin eksik olduğu güvenden sonra yeni bir **Dizin oluşturma** önerisi oluşturulur.

Azure SQL veritabanı, dizinin bir süre içinde dolaştıracağından emin olarak güven oluşturur. Tahmini performans kazanmasına bağlı olarak, öneriler yüksek, orta veya düşük olarak kategorize edilir.

Öneriler kullanılarak oluşturulan dizinler her zaman otomatik olarak oluşturulan dizinler olarak işaretlenir. [Sys. Indexes görünümüne](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-indexes-transact-sql)bakarak hangi dizinlerin otomatik olarak oluşturulduğunu görebilirsiniz. Otomatik oluşturulan dizinler DEĞIŞTIRME/yeniden adlandırma komutlarını engellemez.

Otomatik olarak oluşturulmuş bir dizine sahip sütunu bırakmaya çalışırsanız, komut geçer. Otomatik oluşturulan dizin, komutla birlikte bırakılır. Normal dizinler, dizini oluşturulmuş sütunlarda ALTER/RENAME komutunu engeller.

Dizin oluşturma önerisi uygulandıktan sonra Azure SQL veritabanı, sorguların performansını taban çizgisi performansına göre karşılaştırır. Yeni Dizin performansı artırdıysa, öneri başarılı olarak işaretlenir ve etki raporu kullanılabilir. Dizin performansı iyileştirmediyse, otomatik olarak geri döndürülür. Azure SQL veritabanı, önerilerin veritabanı performansını iyileştirdiğinden emin olmak için bu işlemi kullanır.

Herhangi bir **Dizin oluşturma** önerisi, bir veritabanının veya havuzun kaynak kullanımı yüksek olduğunda öneriyi uygulamaya izin veren bir geri dönüş ilkesine sahiptir. Geri ödeme ilkesi, hesap CPU 'SU, veri GÇ, günlük GÇ ve kullanılabilir depolama alanına girer.

CPU, veri GÇ veya günlük GÇ, önceki 30 dakika içinde %80 ' den yüksekse, dizin oluşturma önerisi ertelenir. Kullanılabilir depolama alanı dizin oluşturulduktan sonra %10 ' un altında olursa öneri bir hata durumuna geçer. Birkaç gün sonra otomatik ayarlama, dizinin faydalı olacağını düşünmeye devam ediyorsa, işlem yeniden başlatılır.

Bu işlem, dizin oluşturmak için yeterli kullanılabilir depolama alanı olana kadar veya dizin artık yararlı olarak görülene kadar yinelenir.

## <a name="drop-index-recommendations"></a>Dizin önerilerini bırak

Eksik dizinlerin algılanmasının yanı sıra Azure SQL veritabanı, mevcut dizinlerin performansını sürekli olarak analiz eder. Bir dizin kullanılmıyorsa, Azure SQL veritabanı bunu bırakmayı önerir. İki durumda da dizin bırakma önerilir:

- Dizin, başka bir dizinin yinelemesi (aynı dizinli ve dahil edilen sütun, Bölüm şeması ve filtreler).
- Dizin, uzun süreli bir süre (93 gün) için kullanılmadı.

Bırakma dizini önerileri, Ayrıca, uygulamadan sonra doğrulama aracılığıyla da gider. Performans artdığı takdirde, etki raporu kullanılabilir. Performans düşer ise, öneri geri döndürülür.

## <a name="parameterize-queries-recommendations-preview"></a>Sorgu önerilerini Parametreleştirme (Önizleme)

*Parametreli* sorgu önerileri, sürekli olarak yeniden derlenmesi gereken ancak aynı sorgu yürütme planıyla biten bir veya daha fazla sorgunuz olduğunda görünür. Bu koşul zorunlu Parametreleştirme uygulamak için bir fırsat oluşturur. Zorunlu Parametreleştirme, daha sonra sorgu planlarının önbelleğe alınıp daha sonra yeniden kullanılmasını sağlar ve bu da performansı artırır ve kaynak kullanımını azaltır.

Bir yürütme planı oluşturmak için başlangıçta her sorgunun derlenmesi gerekir. Oluşturulan her plan plan önbelleğine eklenir. Aynı sorgunun sonraki yürütmeleri bu planı önbellekten yeniden kullanabilir ve bu da ek derleme ihtiyacını ortadan kaldırır.

Parametreli olmayan değerlere sahip sorgular, parametreli olmayan değerler farklı olan her seferinde yürütme planı yeniden derlendiğinden performans yüküne neden olabilir. Çoğu durumda, farklı parametre değerlerine sahip aynı sorgular aynı yürütme planlarını oluşturur. Ancak, bu planlar hala plan önbelleğine ayrı olarak eklenir.

Yürütme planlarını yeniden derleme işlemi veritabanı kaynaklarını kullanır, sorgu süresi süresini artırır ve plan önbelleğinin dışına taşıyor. Bu olaylar, sırayla önbellekten çıkarılmasına neden olur. Bu davranış, veritabanında zorlanan Parametreleştirme seçeneği ayarlanarak değiştirilebilir.

Bu önerinin etkisini tahmin etmenize yardımcı olmak için, gerçek CPU kullanımı ve öngörülen CPU kullanımı (öneri uygulanmış gibi) arasında bir karşılaştırma sağlanır. Bu öneri, CPU tasarrufu elde etmenize yardımcı olabilir. Ayrıca, plan önbelleğinin sorgu süresini ve ek yükünü azaltmanıza yardımcı olabilir. Bu, planlardan daha fazla planın önbellekte kalabileceği ve yeniden kullanılması anlamına gelir. **Uygula** komutunu seçerek bu öneriyi hızlıca uygulayabilirsiniz.

Bu öneriyi uyguladıktan sonra, veritabanınızda dakika içinde zorunlu Parametreleştirme etkinleştirilir. Yaklaşık 24 saat süren izleme işlemini başlatır. Bu dönemden sonra doğrulama raporunu görebilirsiniz. Bu rapor, önerinin uygulandıktan önce ve sonra veritabanınızın CPU kullanımını 24 saat gösterir. Azure SQL Veritabanı Danışmanı, performans gerileme algılanırsa uygulanan öneriyi otomatik olarak geri alan bir güvenlik mekanizmasına sahiptir.

## <a name="fix-schema-issues-recommendations-preview"></a>Şema sorunları önerilerini çözme (Önizleme)

> [!IMPORTANT]
> Microsoft şu anda "şema sorununu çözme" önerilerini kullanımdan kaldıramamaktadır. Daha önce bahsedilen "şema sorununu çözme" önerilerini içeren şema sorunları da dahil olmak üzere, veritabanı performans sorunlarınızı izlemek için [akıllı içgörüler](intelligent-insights-overview.md) kullanmanızı öneririz.

**Şema sorunlarını çözme** önerileri, Azure SQL veritabanı, veritabanınızda gerçekleşen şemaya ilişkin SQL hatalarının sayısında bir anomali fark edildiğinde görüntülenir. Bu öneri genellikle veritabanınız bir saat içinde birden fazla şemaya ilişkin hatayla (geçersiz sütun adı, geçersiz nesne adı vb.) karşılaştığında görüntülenir.

"Şema sorunları" bir sözdizimi hataları sınıfıdır. SQL sorgusunun tanımı ve veritabanı şemasının tanımı hizalanmazsa bunlar oluşur. Örneğin, sorgu tarafından beklenen sütunlardan biri hedef tabloda eksik olabilir veya bunun tersi de geçerlidir.

"Şema sorununu çözme" önerisi, Azure SQL veritabanı, veritabanınızda oluşan şemaya ilişkin SQL hatalarının sayısında bir anomali fark edildiğinde görüntülenir. Aşağıdaki tabloda, şema sorunlarıyla ilgili hatalar gösterilmektedir:

| SQL hata kodu | İleti |
| --- | --- |
| 201 |' ' Yordamı veya işlevi, sağlanmamış *' ' parametresini bekliyor*. |
| 207 |Geçersiz sütun adı ' * '. |
| 208 |Geçersiz nesne adı ' * '. |
| 213 |Girilen değerlerin sütun adı veya numarası tablo tanımıyla eşleşmiyor. |
| 2812 |' * ' Saklı yordamı bulunamadı. |
| 8144 |Yordam veya işlev *, çok fazla sayıda bağımsız değişken belirtti. |

## <a name="custom-applications"></a>Özel uygulamalar

Geliştiriciler, Azure SQL veritabanı için performans önerilerini kullanarak özel uygulamalar geliştirmeyi düşünebilirler. Bir veritabanı için Portalda listelenen tüm önerilere [Get-AzSqlDatabaseRecommendedAction](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction) API aracılığıyla erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı dizinlerinin ve sorgu yürütme planlarının otomatik olarak ayarlanması hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı otomatik ayarlama](automatic-tuning-overview.md).
- Performans sorunlarının otomatik tanılama ve kök neden analizine sahip veritabanı performansını otomatik olarak izleme hakkında daha fazla bilgi için bkz. [Azure SQL akıllı içgörüler](intelligent-insights-overview.md).
- En iyi sorguların performans etkisini öğrenmek ve görüntülemek için bkz. [sorgu Performans öngörüleri](query-performance-insight-use.md) .
