---
title: Tek ve havuzlu veritabanları için veritabanı danışmanı performans önerileri
description: Azure SQL Veritabanı, Azure SQL veritabanında sorgu performansını artırabilecek tek ve birleştirilmiş veritabanları için öneriler sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 03/10/2020
ms.openlocfilehash: bd7473813722fd413947535413b98d493058634a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214148"
---
# <a name="database-advisor-performance-recommendations-for-single-and-pooled-databases"></a>Tek ve havuzlu veritabanları için Veritabanı Danışmanı performans önerileri

Azure SQL Veritabanı, uygulamanızı öğrenir ve uyum sağlar. Sql Veritabanı, tek ve birleştirilmiş veritabanları için performansı en üst düzeye çıkarmanızı sağlayan özelleştirilmiş öneriler sağlayan bir dizi veritabanı danışmanına sahiptir. Bu veritabanı danışmanları, kullanım geçmişini sürekli olarak değerlendirir ve analiz eder ve performansı artırmaya yardımcı olan iş yükü desenlerine dayalı öneriler sağlar.

## <a name="performance-overview"></a>Performansa genel bakış

Performansa genel bakış veritabanı performansınızın bir özetini sağlar ve performans ayarlama ve sorun giderme konusunda size yardımcı olur.

![Azure SQL Veritabanı için performansa genel bakış](./media/sql-database-performance/performance-overview-annotated.png)

- **Öneriler** döşemesi veritabanınız için atoklama önerilerinin dökümünü sağlar (daha fazla varsa en iyi üç öneri gösterilir). Bu döşemeyi tıklattığınızda Performans **[önerisi seçeneklerine](sql-database-advisor-portal.md#viewing-recommendations)** geçersiniz.
- **Tuning etkinlik** döşemesi, veritabanınız için devam eden ve tamamlanmış atonlama eylemlerinin bir özetini sağlayarak, atoklama etkinliğinin geçmişine hızlı bir görünüm sağlar. Bu döşemeyi tıklattığınızda veritabanınızın tam tuning geçmişi görünümüne götürür.
- **Otomatik ayar** döşemesi veritabanınızın otomatik **[ayar yapılandırmasını](sql-database-automatic-tuning-enable.md)** (veritabanınıza otomatik olarak uygulanan ayar seçenekleri) gösterir. Bu döşemeyi tıklattığınızda otomasyon yapılandırma iletişim kutusu açılır.
- **Veritabanı sorguları döşemesi** veritabanınız için sorgu performansının özetini (genel DTU kullanımı ve en iyi kaynak tüketen sorgular) gösterir. Bu döşemeyi tıklattığınızda, **[Sorgu Performans Öngörüsüne](sql-database-query-performance.md)** geçersiniz.

## <a name="performance-recommendation-options"></a>Performans önerisi seçenekleri

Azure SQL Veritabanı'nda tek ve havuza toplanan veritabanları için kullanılabilen performans önerisi seçenekleri şunlardır:

| Performans önerisi | Tek veritabanı ve havuzlu veritabanı desteği | Örnek veritabanı desteği |
| :----------------------------- | ----- | ----- |
| **Dizin önerileri oluşturma** - İş yükünüzün performansını artırabilecek dizinler oluşturulmasını önerir. | Evet | Hayır |
| **Bırakma dizini önerileri** - Benzersiz dizinler ve uzun süre kullanılmayan dizinler (>90 gün) dışında, gereksiz ve yinelenen dizinlerin günlük olarak kaldırılmasını önerir. Bu seçeneğin bölüm değiştirme ve dizin ipuçlarını kullanan uygulamalarla uyumlu olmadığını lütfen unutmayın. Kullanılmayan dizinlerin düşmesi Premium ve İş Açısından Kritik hizmet katmanları için desteklenmez. | Evet | Hayır |
| **Sorgu önerilerini (önizleme) parametrenize ayırma** - Sürekli olarak yeniden derlenen ancak aynı sorgu yürütme planıyla sona erecek bir veya daha fazla sorgunuz olduğunda zorunlu parametreizasyon önerir. | Evet | Hayır |
| **Şema sorunları önerilerini düzeltme (önizleme)** - SQL Veritabanı hizmeti, SQL veritabanınızda meydana gelen şema ile ilgili SQL hatalarının sayısında bir anormallik fark ettiğinde şema düzeltme önerileri görüntülenir. Microsoft şu anda "Şema sorununu düzelt" önerilerini küçümsüyor. | Evet | Hayır |

![Azure SQL Veritabanı için performans önerileri](./media/sql-database-performance/performance-recommendations-annotated.png)

Performans önerilerini uygulamak [için, bkz.](sql-database-advisor-portal.md#applying-recommendations) Önerilerin durumunu görüntülemek için izleme [işlemleri](sql-database-advisor-portal.md#monitoring-operations)bölümüne bakın.

Ayrıca, geçmişte uygulanan atokalma eylemlerinin tam geçmişini de bulabilirsiniz.

## <a name="create-index-recommendations"></a>Dizin önerileri oluşturma

SQL Veritabanı sürekli olarak çalışan sorguları izler ve performansı artırabilecek dizinleri tanımlar. Belirli bir dizinin eksik olduğuna dair yeterli güven olduktan sonra, yeni bir **Create dizin** önerisi oluşturulur.

Azure SQL Veritabanı, dizinin zaman içinde getireceği performans kazancını tahmin ederek güven oluşturur. Tahmini performans kazancına bağlı olarak, öneriler yüksek, orta veya düşük olarak sınıflandırılır.

Öneriler kullanılarak oluşturulan dizinler her zaman otomatik oluşturulan dizinler olarak işaretlenir. [Sys.indexs görünümüne](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-indexes-transact-sql)bakarak hangi dizinlerin otomatik olarak oluşturulduğunu görebilirsiniz. Otomatik oluşturulan dizinler ALTER/RENAME komutlarını engellemez.

Üzerinde otomatik olarak oluşturulmuş dizin olan sütunu düşürmeye çalışırsanız, komut geçer. Otomatik oluşturulan dizin komutu ile de bırakılır. Düzenli dizinler dizine eklenmiş sütunlarda ALTER/RENAME komutunu engeller.

Create index önerisi uygulandıktan sonra, Azure SQL Veritabanı sorguların performansını temel performansla karşılaştırır. Yeni dizin performansı artırdıysa, öneri başarılı olarak işaretlenir ve etki raporu kullanılabilir. Dizin performansı artırmadıysa, otomatik olarak döndürülebilir. SQL Veritabanı, önerilerin veritabanı performansını artırmasını sağlamak için bu işlemi kullanır.

Herhangi bir **create index** önerisi, bir veritabanı veya havuzun kaynak kullanımı yüksekse, önerinin uygulanmasına izin vermeyen bir geri leme ilkesine sahiptir. Geri çekme ilkesi CPU, Veri IO, Log IO ve kullanılabilir depolamayı dikkate alır.

CPU, veri IO veya günlük IO önceki 30 dakika içinde% 80'den yüksekse, oluşturma dizin önerisi ertelenir. Dizin oluşturulduktan sonra kullanılabilir depolama alanı %10'un altında olacaksa, öneri bir hata durumuna geçer. Birkaç gün sonra otomatik ayar hala dizinin yararlı olacağına inanıyorsa, işlem yeniden başlar.

Bu işlem, bir dizin oluşturmak için yeterli kullanılabilir depolama alanı olana veya dizin artık yararlı olarak görülmeyene kadar yinelenir.

## <a name="drop-index-recommendations"></a>Drop index önerileri

SQL Veritabanı, eksik dizinleri algılamanın yanı sıra varolan dizinlerin performansını sürekli olarak analiz eder. Bir dizin kullanılmazsa, Azure SQL Veritabanı dizin bırakmanızı önerir. Bir dizin bırakarak iki durumda önerilir:

- Dizin başka bir dizin (aynı dizinlenmiş ve sütun, bölüm şeması ve filtreler) bir kopyasıdır.
- Dizin uzun bir süre (93 gün) kullanılmamıştır.

Drop index önerileri de uygulamadan sonra doğrulama geçer. Performans iyileşirse, etki raporu kullanılabilir. Performans bozulursa, öneri geri döndürülür.

## <a name="parameterize-queries-recommendations-preview"></a>Sorgu önerilerini parametreize et (önizleme)

*Parameterize sorguları* önerileri, sürekli olarak yeniden derlenen ancak aynı sorgu yürütme planıyla sona erecek bir veya daha fazla sorgunuz olduğunda görüntülenir. Bu durum, zorunlu parametrelendirme uygulamak için bir fırsat oluşturur. Zorunlu parametreleştirme, sırayla, sorgu planlarının gelecekte önbelleğe alınmasına ve yeniden kullanılmasına izin verir, bu da performansı artırır ve kaynak kullanımını azaltır.

Bir yürütme planı oluşturmak için başlangıçta SQL Server'a karşı verilen her sorgunun derlenmesi gerekir. Oluşturulan her plan, plan önbelleğine eklenir. Aynı sorgunun sonraki yürütmeleri, ek derleme gereksinimini ortadan kaldıran önbellekten bu planı yeniden kullanabilir.

Yürütme planı parametresiz değerler her farklı olduğunda yeniden derlenişolduğundan, parametrelendirilmeyen değerlere sahip sorgular performans ek yüküne neden olabilir. Çoğu durumda, farklı parametre değerlerine sahip aynı sorgular aynı yürütme planlarını oluşturur. Ancak bu planlar yine de plan önbelleğine ayrı olarak eklenir.

Yürütme planlarını yeniden derleme işlemi veritabanı kaynaklarını kullanır, sorgu süresini artırır ve plan önbelleğini taşar. Bu olaylar da planların önbellekten tahliye edilmesine neden olur. Bu SQL Server davranışı veritabanında zorunlu parametreleme seçeneği ayarlayarak değiştirilebilir.

Bu önerinin etkisini tahmin etmek için, gerçek CPU kullanımı ile öngörülen CPU kullanımı (öneri uygulanmış gibi) arasında bir karşılaştırma sağlanır. Bu öneri, CPU tasarrufları elde yardımcı olabilir. Ayrıca, plan önbelleği için sorgu süresini ve ek yükü azaltmanıza da yardımcı olabilir, bu da planların daha fazlasının önbellekte kalıp yeniden kullanılabildiği anlamına gelir. Bu öneriyi **Uygula** komutunu seçerek hızlı bir şekilde uygulayabilirsiniz.

Bu öneriyi uyguladıktan sonra, veritabanınızda birkaç dakika içinde zorunlu parametreizasyon sağlar. Yaklaşık 24 saat süren izleme işlemini başlatır. Bu dönemden sonra doğrulama raporunu görebilirsiniz. Bu rapor, veritabanınızın cpu kullanımını önerinin uygulanmasından 24 saat önce ve sonra gösterir. SQL Database Advisor, performans regresyonu algılanırsa uygulanan öneriyi otomatik olarak geri alan bir güvenlik mekanizmasına sahiptir.

## <a name="fix-schema-issues-recommendations-preview"></a>Şema sorunları önerilerini düzeltme (önizleme)

> [!IMPORTANT]
> Microsoft şu anda "Şema sorununu düzelt" önerilerini küçümsüyor. Veritabanı performans sorunlarınızı izlemek için [Intelligent Insights'ı](sql-database-intelligent-insights.md) kullanmanızı öneririz, buna daha önce "Şema sorununu düzelt" önerilerinin kapsadığı şema sorunları da dahil.

SQL Veritabanı hizmeti, SQL veritabanınızda meydana gelen şema ile ilgili SQL hatalarının sayısında bir anormallik fark ettiğinde **şema sorunlarını düzeltme** önerileri görüntülenir. Bu öneri genellikle veritabanınız şema ile ilgili birden çok hatayla (geçersiz sütun adı, geçersiz nesne adı vb.) bir saat içinde karşılaştığında görünür.

"Şema sorunları" SQL Server'daki sözdizimi hataları sınıfıdır. Bunlar, SQL sorgusunun tanımı ve veritabanı şemasının tanımı hizalanmadığında oluşur. Örneğin, sorgu tarafından beklenen sütunlardan biri hedef tabloda eksik veya tam tersi olabilir.

"Şema sorununu düzelt" önerisi, Azure SQL Veritabanı hizmeti, SQL veritabanınızda meydana gelen şema ile ilgili SQL hatalarının sayısında bir anormallik fark ettiğinde görüntülenir. Aşağıdaki tabloşema sorunları ile ilgili hataları gösterir:

| SQL hata kodu | İleti |
| --- | --- |
| 201 |Yordam veya işlev '*' ' parametre bekliyor '*', hangi verilmedi. |
| 207 |Geçersiz sütun adı '*'. |
| 208 |Geçersiz nesne adı '*'. |
| 213 |Sütun adı veya verilen değerlerin sayısı tablo tanımıyla eşleşmiyor. |
| 2812 |Depolanan yordam '*' bulamadı. |
| 8144 |Yordam veya işlev * çok fazla bağımsız değişken belirtilmiştir. |

## <a name="custom-applications"></a>Özel uygulamalar

Geliştiriciler, Azure SQL Veritabanı için performans önerilerini kullanarak özel uygulamalar geliştirmeyi düşünebilir. Bir veritabanı için portalda listelenen tüm [önerilere Get-AzSqlDatabaseRecommendedAction](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction) API üzerinden erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı dizinlerinin ve sorgu yürütme planlarının otomatik olarak ayarlanması hakkında daha fazla bilgi için [Azure SQL Veritabanı otomatik ayarını](sql-database-automatic-tuning.md)görün.
- Otomatik tanılama ve performans sorunlarının kök neden analiziyle veritabanı performansını otomatik olarak izleme hakkında daha fazla bilgi için [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md)bölümüne bakın.
- En iyi sorgularınızın performans etkisini öğrenmek ve görüntülemek için [Sorgu Performansı Öngörüleri'ne](sql-database-query-performance.md) bakın.
