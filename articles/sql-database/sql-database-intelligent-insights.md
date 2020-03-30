---
title: Intelligent Insights ile veritabanı performansını izleme
description: Azure SQL Database Intelligent Insights, yapay zeka aracılığıyla veritabanı kullanımını sürekli olarak izlemek ve düşük performansa neden olan yıkıcı olayları tespit etmek için yerleşik zekayı kullanır.
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
ms.openlocfilehash: d7b9ada17871dc7882209b7a8a449a8edcd61a94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214067"
---
# <a name="intelligent-insights-using-ai-to-monitor-and-troubleshoot-database-performance-preview"></a>Veritabanı performansını izlemek ve sorun gidermek için AI kullanan Akıllı Öngörüler (Önizleme)

Azure SQL Database Intelligent Insights, veritabanı performansınızda neler olduğunu bilmenizi sağlar.

Intelligent Insights, yapay zeka aracılığıyla veritabanı kullanımını sürekli olarak izlemek ve düşük performansa neden olan yıkıcı olayları tespit etmek için yerleşik zekayı kullanır. Algılandıktan sonra, sorunun akıllı bir değerlendirmesiyle Intelligent Insights kaynak günlüğü (SQLInsights olarak adlandırılır) oluşturan ayrıntılı bir analiz gerçekleştirilir. Bu değerlendirme, veritabanı performans sorununun temel neden çözümlemesi ve mümkünse performans iyileştirmeleri için önerilerden oluşur.

## <a name="what-can-intelligent-insights-do-for-you"></a>Akıllı Öngörüler sizin için ne yapabilir?

Intelligent Insights, Azure yerleşik zekasının aşağıdaki değeri sağlayan benzersiz bir yeteneğidir:

- Öngörülebilir izleme
- Özel performans öngörüleri
- Veritabanı performans bozulmasının erken saptanması
- Algılanan sorunların kök neden analizi
- Performans geliştirme önerileri
- Yüz binlerce veritabanındaki yeteneği ölçeklendirin
- DevOps kaynaklarına olumlu etki ve toplam sahip olma maliyeti

## <a name="how-does-intelligent-insights-work"></a>Intelligent Insights nasıl çalışır?

Intelligent Insights, veritabanı nın son saatteki iş yükünü son yedi günlük temel iş yüküyle karşılaştırarak veritabanı performansını analiz eder. Veritabanı iş yükü, veritabanı performansı için en önemli olarak belirlenen sorgulardan oluşur( örneğin, en çok yinelenen ve en büyük sorgular). Her veritabanı yapısına, verilerine, kullanımına ve uygulamasına göre benzersiz olduğundan, oluşturulan her iş yükü taban çizgisi belirlidir ve bu iş yüküne özgüdür. İş yükü taban çizgisinden bağımsız olarak Akıllı Öngörüler, mutlak çalışma eşiklerini de izler ve aşırı bekleme süreleri, kritik özel durumlar ve performansı etkileyebilecek sorgu parametreleri olan sorunları algılar.

Bir performans bozulması sorunu yapay zeka kullanılarak birden fazla gözlenen ölçümlerden tespit edildikten sonra analiz yapılır. Veritabanınızda neler olduğuna dair akıllı bir içgörüyle bir tanılama günlüğü oluşturulur. Intelligent Insights, veritabanı performansı sorununu ilk görünümünden çözüme kadar izlemeyi kolaylaştırır. Algılanan her sorun, ilk sorun algılama ve tamamlanmasına kadar performans iyileştirme doğrulaması kendi yaşam döngüsü ile izlenir.

![Veritabanı performans analizi iş akışı](./media/sql-database-intelligent-insights/intelligent-insights-concept.png)

Veritabanı performans sorunlarını ölçmek ve algılamak için kullanılan ölçümler sorgu süresine, zaman aşım isteklerine, aşırı bekleme sürelerine ve hatalı istekleri temel almaktadır. Ölçümler hakkında daha fazla bilgi için [bkz.](#detection-metrics)

Tanımlanan SQL Veritabanı performans bozulmaları aşağıdaki özelliklerden oluşan akıllı girişlerle SQLInsights günlüğüne kaydedilir:

| Özellik | Ayrıntılar |
| :------------------- | ------------------- |
| Veritabanı bilgileri | Kaynak URI gibi bir öngörüalgılanan bir veritabanı hakkında meta veriler. |
| Gözlenen zaman aralığı | Algılanan öngörü dönemi için başlangıç ve bitiş saati. |
| Etkilenen ölçümler | Bir öngörüoluşturulmasına neden olan ölçümler: <ul><li>Sorgu süresi artışı [saniye].</li><li>Aşırı bekleme [saniye].</li><li>Zamanlanmış istekleri [yüzdesi].</li><li>Hatalı çıkış istekleri [yüzde].</li></ul>|
| Etki değeri | Ölçülen bir metnin değeri. |
| Etkilenen sorgular ve hata kodları | Sorgu karma veya hata kodu. Bunlar, etkilenen sorgularla kolayca ilişkilendirilebilmek için kullanılabilir. Sorgu süresi artışı, bekleme süresi, zaman atlama sayıları veya hata kodlarından oluşan ölçümler sağlanır. |
| Algılamalar | Bir olay sırasında veritabanında tanımlanan algılama. 15 tespit düzeni var. Daha fazla bilgi için [Intelligent Insights ile sorun giderme veritabanı performansı sorunlarına](sql-database-intelligent-insights-troubleshoot-performance.md)bakın. |
| Kök neden analizi | İnsan tarafından okunabilir bir biçimde tanımlanan sorunun kök neden analizi. Bazı öngörüler mümkünse bir performans geliştirme önerisi içerebilir. |
|||

Azure SQL Analytics ile Intelligent Insights'ı kullanma ve tipik kullanım senaryoları hakkında uygulamalı bir bakış için aşağıdaki videoya bakın:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

Intelligent Insights, SQL Veritabanı performans sorunlarını keşfetme ve sorun gidermede parlar. Veritabanı performans sorunlarını gidermek için Intelligent Insights'ı kullanmak için, [Intelligent Insights ile Sorun Giderme Azure SQL Veritabanı performans sorunlarına](sql-database-intelligent-insights-troubleshoot-performance.md)bakın.

## <a name="intelligent-insights-options"></a>Akıllı Öngörüler seçenekleri

Azure SQL Veritabanı'nda bulunan Akıllı Öngörüler seçenekleri şunlardır:

| Akıllı Öngörüler seçeneği | Tek veritabanı ve havuzlu veritabanı desteği | Örnek veritabanı desteği |
| :----------------------------- | ----- | ----- |
| **Akıllı Öngörüleri Yapılandırın** - Veritabanlarınız için Akıllı Öngörüler analizini yapılandırın. | Evet | Evet |
| **Azure SQL Analytics'e akış öngörüleri** -- Azure SQL Veritabanı için Azure SQL Analytics izleme çözümüne akış öngörüleri. | Evet | Evet |
| Daha fazla özel entegrasyon için **Etkinlik Hub'ına akış öngörüleri** - Daha fazla özel entegrasyon için Etkinlik Hub'larına akış öngörüleri. | Evet | Evet |
| **Öngörüleri Azure Depolama'ya aktarın** - Daha fazla analiz ve uzun vadeli arşivleme için öngörüleri Azure Depolama'ya aktarın. | Evet | Evet |

## <a name="configure-the-export-of-the-intelligent-insights-log"></a>Intelligent Insights günlüğünün dışa kaydını yapılandırma

Intelligent Insights'ın çıktısı analiz için birkaç varış noktasından birine aktarılabilir:

- Log Analytics çalışma alanına aktarılan çıktı, Azure portalının kullanıcı arabirimi aracılığıyla öngörüleri görüntülemek için [Azure SQL Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql) ile kullanılabilir. Bu, tümleşik Azure çözümüdür ve öngörüleri görüntülemenin en tipik yoludur.
- Azure Etkinlik Hub'larına aktarılan çıktılar, özel izleme ve uyarı senaryolarının geliştirilmesi için kullanılabilir
- Azure Depolama'ya aktarılan çıktı, örneğin özel raporlama, uzun vadeli veri arşivleme ve benzeri özel uygulama geliştirme için kullanılabilir.

Azure SQL Analytics, Azure Event Hub, Azure Depolama veya üçüncü taraf ürünlerin tüketimi için entegrasyonu, önce bir veritabanının Tanı ayarları bıçaklarında Intelligent Insights günlüğe kaydetmeyi ("SQLInsights" günlüğü) etkinleştirerek gerçekleştirilir ve ardından Intelligent Insights günlük verilerini bu hedeflerden birine aktarılacak şekilde yapılandırır.

Intelligent Insights günlüğe kaydetmeyi etkinleştirme ve metrik ve kaynak günlüğü verilerini tüketen bir ürüne aktaracak şekilde yapılandırmak hakkında daha fazla bilgi için [Azure SQL Veritabanı ölçümleri ve tanılama günlüğe kaydetme'ye](sql-database-metrics-diag-logging.md)bakın.

### <a name="set-up-with-azure-sql-analytics"></a>Azure SQL Analytics ile ayarlama

Azure SQL Analytics çözümü, Intelligent Insights kaynak günlüğü verilerini kullanarak veritabanı performansı hakkında grafiksel kullanıcı arabirimi, raporlama ve uyarı özellikleri sağlar.

Azure portal panonuza pazardan Azure SQL Analytics ekleyin ve bir çalışma alanı oluşturmak için [Azure SQL Analytics'i yapılandırmaya](../azure-monitor/insights/azure-sql.md#configuration) bakın

Azure SQL Analytics ile Akıllı Öngörüler'i kullanmak için, önceki adımda oluşturduğunuz Azure SQL Analytics çalışma alanına aktalanacak Intelligent Insights günlük verilerini yapılandırın, [Bkz. Azure SQL Veritabanı ölçümleri ve tanılama günlüğe kaydetme.](sql-database-metrics-diag-logging.md)

Aşağıdaki örnekte, Azure SQL Analytics tarafından görüntülenen Bir Akıllı Öngörüler gösterilmektedir:

![Akıllı Öngörüler raporu](./media/sql-database-intelligent-insights/intelligent-insights-azure-sql-analytics.png)

### <a name="set-up-with-event-hubs"></a>Etkinlik Hub'ları ile ayarlama

Olay Hub'larıyla Akıllı Öngörüler kullanmak için, Olay Hub'larına aktasılanacak Akıllı Öngörüler günlük verilerini yapılandırmak için Azure [SQL Veritabanı ölçümlerine ve tanılama günlüğe bakın](sql-database-metrics-diag-logging.md) ve Azure [tanılama günleme günleme günleme günleme günleme lerini Olay Hub'larına aktarın.](../azure-monitor/platform/resource-logs-stream-event-hubs.md)

Özel izleme ve uyarı kurmak için Olay Hub'larını kullanmak için, [Olay Hub'larında ölçümler ve tanılama günlükleriyle ne yapacağınız](sql-database-metrics-diag-logging.md#what-to-do-with-metrics-and-resource-logs-in-event-hubs)konusuna bakın.

### <a name="set-up-with-azure-storage"></a>Azure Depolama ile ayarlama

Akıllı Bilgiler'i Depolama ile kullanmak için, Akıllı Öngörüler günlük verilerini Depolama'ya aktarılacak şekilde yapılandırın, [Azure SQL Veritabanı ölçümlerine bakın ve tanılama günlüğe kaydedin](sql-database-metrics-diag-logging.md) ve [Azure Depolama'ya aktarın.](sql-database-metrics-diag-logging.md#stream-into-azure-storage)

### <a name="custom-integrations-of-intelligent-insights-log"></a>Intelligent Insights günlüğünün özel entegrasyonları

Intelligent Insights'ı üçüncü taraf araçlarıyla kullanmak veya özel uyarı ve izleme geliştirme için [bkz.](sql-database-intelligent-insights-use-diagnostics-log.md)

## <a name="detection-metrics"></a>Algılama ölçümleri

Akıllı Öngörüler oluşturan algılama modelleri için kullanılan ölçümler izlemeyi temel adamaktadır:

- Sorgu süresi
- Zaman ara istekleri
- Aşırı bekleme süresi
- Hatalı gelen istekler

Sorgu süresi ve zaman aşım istekleri, veritabanı iş yükü performansıyla ilgili sorunları algılamada birincil modeller olarak kullanılır. Onlar doğrudan iş yükü ile neler olduğunu ölçmek için kullanılır. İş yükü performansının düşmesi olası tüm durumlarda algılamak için, aşırı bekleme süresi ve hata-out istekleri iş yükü performansını etkileyen sorunları belirtmek için ek modeller olarak kullanılır.

Sistem, normal ve olağan dışı veritabanı performans eşiklerini dinamik olarak belirlemek için iş yükündeki değişiklikleri ve veritabanına yapılan sorgu istekleri sayısındaki değişiklikleri otomatik olarak dikkate alır.

Tüm ölçümler, algılanan her performans sorununu kategorilere ayıran bilimsel olarak türetilmiş bir veri modeli aracılığıyla çeşitli ilişkilerde birlikte değerlendirilir. Akıllı bir içgörü aracılığıyla sağlanan bilgiler şunları içerir:

- Performans sorununun ayrıntıları algılandı.
- Sorunun kök neden çözümlemesi algılandı.
- İzlenen SQL veritabanının performansını niçin iyileştireceğiniz konusunda öneriler, mümkünse.

## <a name="query-duration"></a>Sorgu süresi

Sorgu süresi bozulması modeli tek tek sorguları analiz eder ve performans taban çizgisiyle karşılaştırıldığında bir sorguyu derlemek ve yürütmek için gereken süredeki artışı algılar.

SQL Veritabanı yerleşik zekası, sorgu derleme veya sorgu yürütme süresinde iş yükü performansını etkileyen önemli bir artış algılarsa, bu sorgular sorgu süresi performansı bozulması sorunları olarak işaretlenir.

Intelligent Insights tanılama günlüğü, performansta azalan sorgunun sorgu karmasını çıktırıyor. Sorgu karma, performans bozulmasının sorgu süresi süresini artıran sorgu derleme seçimi veya yürütme zamanı artışıyla ilgili olup olmadığını belirtir.

## <a name="timeout-requests"></a>Zaman ara istekleri

Zaman arası istemeleri bozulma modeli tek tek sorguları analiz eder ve sorgu yürütme düzeyindeki zaman eklerinde ve performans temeli dönemine kıyasla veritabanı düzeyindeki genel istek zaman ekmelerinde herhangi bir artış algılar.

Bazı sorgular yürütme aşamasına ulaşmadan önce bile zaman alabilir. Sql Database yerleşik istihbarat, iptal edilen çalışanlar ve yapılan istekler aracılığıyla, yürütme aşamasına gelip gelmeseler de veritabanına ulaşan tüm sorguları ölçer ve analiz eder.

Çalıştırılan sorgular için zaman aşımlarının sayısı veya iptal edilen istek çalışanlarının sayısı sistem tarafından yönetilen eşikten geçtikten sonra, akıllı öngörülerle bir tanılama günlüğü doldurulur.

Oluşturulan öngörüler, zamanlanmış isteklerin sayısını ve zamanlanmış sorguların sayısını içerir. Performans bozulmasıgöstergesi yürütme aşamasında zaman adedi artışı ile ilgilidir veya genel veritabanı düzeyi sağlanır. Zaman ayırıklarındaartış veritabanı performansı için önemli kabul edildiğinde, bu sorgular zaman arası performans bozulması sorunları olarak işaretlenir.

## <a name="excessive-wait-times"></a>Aşırı bekleme süreleri

Aşırı bekleme süresi modeli tek tek veritabanı sorgularını izler. Sistem tarafından yönetilen mutlak eşikleri geçen olağandışı yüksek sorgu bekleme istatistiklerialgılar. Aşağıdaki sorgu aşırı bekleme süresi ölçümleri yeni SQL Server özelliği, Query Store Bekleme İstatistikleri (sys.query_store_wait_stats) kullanılarak gözlenir:

- Kaynak sınırlarına ulaşma
- Esnek havuz kaynak sınırlarına ulaşma
- Aşırı sayıda çalışan veya oturum iş parçacığı
- Aşırı veritabanı kilitleme
- Bellek baskısı
- Diğer bekleme istatistikleri

Kaynak sınırlarına veya esnek havuz kaynak sınırlarına ulaşmak, kullanılabilir kaynakların bir abonelikte veya esnek havuzda tüketiminin mutlak eşikleri aştığını gösterir. Bu istatistikler iş yükü performansının düşmesini gösterir. Aşırı sayıda alt veya oturum iş parçacığı, başlatılan alt iş parçacığı veya oturum sayısının mutlak eşikleri geçtiği bir durumu gösterir. Bu istatistikler iş yükü performansının düşmesini gösterir.

Aşırı veritabanı kilitleme, veritabanındaki kilit sayısının mutlak eşikleri geçtiği bir durumu gösterir. Bu istatistik, iş yükü performansı nın düşmesini gösterir. Bellek basıncı, bellek hibeleri isteyen iş parçacığı sayısının mutlak bir eşiği geçtiği bir durumdur. Bu istatistik, iş yükü performansı nın düşmesini gösterir.

Diğer bekleme istatistikleri algılama, Sorgu Deposu Bekleme İstatistikleri ile ölçülen çeşitli ölçümlerin mutlak bir eşikten geçtiği bir durumu gösterir. Bu istatistikler iş yükü performansının düşmesini gösterir.

Aşırı bekleme süreleri algılandıktan sonra, mevcut verilere bağlı olarak, Akıllı Öngörüler, performansta bozulan etkileyen ve etkilenen sorguların çıktılarını, sorguların yürütülmesinde beklemesine neden olan ölçümlerin ayrıntılarını ve ölçülen bekleme süresi.

## <a name="errored-requests"></a>Hatalı istekler

Hatalı istekler bozunma modeli tek tek sorguları izler ve temel döneme göre hata yapan sorgu sayısında bir artış algılar. Bu model, SQL Veritabanı yerleşik zeka tarafından yönetilen mutlak eşikleri geçen kritik özel durumları da izler. Sistem, veritabanına yapılan sorgu isteklerinin sayısını otomatik olarak dikkate alır ve izlenen dönemdeki iş yükü değişiklikleri için hesaplar.

Yapılan toplam istek sayısına göre hata yapılan isteklerde ölçülen artış iş yükü performansı açısından önemli kabul edildiğinde, etkilenen sorgular hata istekleri performans bozunma sorunları olarak işaretlenir.

Intelligent Insights günlüğü, hatalı istek sayısını verir. Performans bozulmasının hatalı istekteki artışla mı yoksa izlenen kritik özel durum eşiğini ve performans bozulmasının ölçülen süresini aşmakla mı ilişkili olduğunu gösterir.

İzlenen kritik özel durumlardan herhangi biri sistem tarafından yönetilen mutlak eşikleri geçerse, kritik özel durum ayrıntılarıyla birlikte akıllı bir öngörü oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Analytics'i kullanarak SQL Veritabanı'nı](../azure-monitor/insights/azure-sql.md)nasıl izleyeceğizi öğrenin.
- [Intelligent Insights ile SQL Veritabanı performans sorunlarını](sql-database-intelligent-insights-troubleshoot-performance.md)nasıl gidereceklerini öğrenin.
