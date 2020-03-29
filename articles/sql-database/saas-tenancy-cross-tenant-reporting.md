---
title: Birden çok veritabanında sorguları raporlama
description: Dağıtılmış sorguları kullanarak kiracı lar arası raporlama.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewers: billgib,ayolubek
ms.date: 01/25/2019
ms.openlocfilehash: c863946934df9990c14e49ef1a0a82bbc55b27c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822074"
---
# <a name="cross-tenant-reporting-using-distributed-queries"></a>Dağıtılmış sorguları kullanarak kiracı arası raporlama

Bu öğreticide, raporlama için tüm kiracı veritabanları kümesiboyunca dağıtılmış sorgular çalıştırın. Bu sorgular Wingtip Tickets SaaS kiracılarının günlük operasyonel verilerine gömülü öngörüler ayıklayabilir. Bunu yapmak için, katalog sunucusuna ek bir raporlama veritabanı dağıtın ve dağıtılmış sorguları etkinleştirmek için Elastik Sorgu'yu kullanın.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> 
> * Raporlama veritabanı nasıl dağıtılır?
> * Tüm kiracı veritabanlarında dağıtılmış sorgular nasıl çalıştırılır?
> * Her veritabanındaki genel görünümler kiracılar arasında verimli sorgulamayı nasıl etkinleştirebilir?


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:


* Wingtip Tickets SaaS Database Per Tenant uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için, [Wingtip Tickets SaaS Veritabanı'nı](saas-dbpertenant-get-started-deploy.md) Kiracı Başına dağıtın ve keşfedin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) yüklenir. SSMS'i indirmek ve yüklemek için sql [server management studio (SSMS) indir'e](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)bakın.


## <a name="cross-tenant-reporting-pattern"></a>Kiracı çapraz raporlama deseni

![kiracı dağıtılmış sorgu deseni](media/saas-tenancy-cross-tenant-reporting/cross-tenant-distributed-query.png)

SaaS uygulamalarıyla ilgili bir fırsat, uygulamanızın işleyişi ve kullanımı hakkında bilgi edinmek için bulutta depolanan büyük miktardaki kiracı verilerini kullanmaktır. Bu öngörüler özellik geliştirme, kullanılabilirlik geliştirme ve uygulamalarınıza ve hizmetlerinize yapılan diğer yatırımlara rehberlik edebilir.

Bu verilere tek bir çok kiracılı veritabanında erişim kolaydır, ancak binlerce veritabanına ölçekli olarak dağıtıldığında çok kolay değildir. Bir yaklaşım, ortak şema ile veritabanları dağıtılmış bir dizi arasında sorgu sağlayan [Elastik Sorgu](sql-database-elastic-query-overview.md), kullanmaktır. Bu veritabanları farklı kaynak grupları ve abonelikler arasında dağıtılabilir, ancak ortak bir giriş paylaşmak gerekir. Elastik Sorgu, dağıtılmış (kiracı) veritabanlarındaki tabloları veya görünümleri yansıtan dış tabloların tanımlandığı tek bir *kafa* veritabanı kullanır. Bu baş veritabanına gönderilen sorgular, gerektiğinde kiracı veritabanlarına gönderilen sorgu kısımlarıyla birlikte dağıtılmış bir sorgu planı oluşturmak üzere derlenir. Elastik Sorgu, tüm kiracı veritabanlarının konumunu belirlemek için katalog veritabanındaki parça eşlemi kullanır. Kafa veritabanının kurulumu ve sorgusu standart [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)kullanılarak ve Power BI ve Excel gibi araçlardan sorgulamayı desteklemek kolaydır.

Elastik Sorgu, sorguları kiracı veritabanları arasında dağıtarak canlı üretim verilerine anında bakış sağlar. Elastik Sorgu potansiyel olarak birçok veritabanından veri çektiğinden, sorgu gecikmesi tek bir çok kiracılı veritabanına gönderilen eşdeğer sorgulardan daha yüksek olabilir. Baş veritabanına döndürülen verileri en aza indirmek için sorgular tasarla. Elastik Sorgu genellikle, sık kullanılan veya karmaşık analitik sorguları veya raporları oluşturmak yerine, küçük miktarlarda gerçek zamanlı verileri sorgulamak için en uygun olandır. Sorgular iyi performans göstermezse, sorgunun hangi bölümünün uzak veritabanına itildiğini ve ne kadar veri döndürüldedildiğini görmek için [yürütme planına](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) bakın. Karmaşık toplama veya analitik işleme gerektiren sorgular, kiracı verilerini analitik sorgular için optimize edilmiş bir veritabanına veya veri ambarına ayıklayarak daha iyi işlenebilir. Bu desen kiracı [analitik öğretici](saas-tenancy-tenant-analytics.md)açıklanmıştır. 

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip Biletleri SaaS Veritabanı Kiracı başına uygulama komut alın

Wingtip Biletleri SaaS Çok kiracıveritabanı komut dosyaları ve uygulama kaynak kodu [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub repo mevcuttur. Wingtip Tickets SaaS komut dosyalarını indirmek ve engelini kaldırmak için gereken adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) göz atın.

## <a name="create-ticket-sales-data"></a>Bilet satış verileri oluşturma

Sorguları daha ilginç bir veri kümesine karşı çalıştırmak için, bilet jeneratörü çalıştırarak bilet satış verileri oluşturun.

1. *PowerShell ISE*, açık ... \\\\Öğrenme Modülleri Operasyonel\\Analitik Adhoc\\*Raporlama Demo-AdhocReporting.ps1* komut dosyası ve aşağıdaki değeri ayarlayın:
   * **$DemoScenario** = 1, **Tüm mekanlarda etkinlikler için bilet satın alın.**
2. Komut dosyasını çalıştırmak ve bilet satışları oluşturmak için **F5** tuşuna basın. Komut dosyası çalışırken, bu öğreticideki adımları devam edin. Bilet verileri Run ad *hoc dağıtılmış sorgular* bölümünde sorgulanır, bu nedenle bilet jeneratörünün tamamlanmasını bekleyin.

## <a name="explore-the-global-views"></a>Küresel görünümleri keşfedin

Wingtip Tickets SaaS Veritabanı Kiracı Başına uygulamasında, her kiracıya bir veritabanı verilir. Böylece, veritabanı tablolarında bulunan veriler tek bir kiracının perspektifine göre kapsamlıdır. Ancak, tüm veritabanları arasında sorgu yaparken, Elastik Sorgu'nun verileri kiracı tarafından parçalanan tek bir mantıksal veritabanının parçası gibi işleme sunması önemlidir. 

Bu deseni simüle etmek için, genel olarak sorgulanan tabloların her birine kiracı kimliği yansıtan kiracı veritabanına bir dizi 'genel' görünüm eklenir. Örneğin, *VenueEvents* *görünümü, Etkinlikler* tablosundan yansıtılan sütunlara hesaplanmış bir *VenueId* ekler. Benzer şekilde, *VenueTicketPurchases* ve *VenueTickets* görünümleri, ilgili tablolarından yansıtılan bir hesaplanmış *VenueId* sütunu ekler. Bu görünümler, sorguları paralelleştirmek ve *VenueId* sütunu bulunduğunda bunları uygun uzak kiracı veritabanına itmek için Elastik Sorgu tarafından kullanılır. Bu, döndürülen veri miktarını önemli ölçüde azaltır ve birçok sorgu için performansta önemli bir artışa neden olabilir. Bu genel görünümler tüm kiracı veritabanlarında önceden oluşturulmuştur.

1. SSMS'i açın ve [kiracılara&lt;&gt; bağlanın1- USER sunucusu.](saas-tenancy-wingtip-app-guidance-tips.md#explore-database-schema-and-execute-sql-queries-using-ssms)
1. **Veritabanlarını**genişletin, _contosoconcerthall'a_sağ tıklayın ve **Yeni Sorgu'yu**seçin.
1. Tek kiracıtablolar ve genel görünümler arasındaki farkı keşfetmek için aşağıdaki sorguları çalıştırın:

   ```T-SQL
   -- The base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice the plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- The base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects the VenueId retrieved from the Venues table.
   SELECT * FROM VenueEvents
   ```

Bu görünümlerde, *VenueId* Mekan adının bir karma olarak hesaplanmıştır, ancak herhangi bir yaklaşım benzersiz bir değer tanıtmak için kullanılabilir. Bu yaklaşım, kiracı anahtarının katalogda kullanılmak üzere hesaplanmış şekilde benzer.

*Mekanlar* görünümünün tanımını incelemek için:

1. **Object Explorer'da** **contosoconcerthall** > **Görünümlerini**genişletin:

   ![görünümler](media/saas-tenancy-cross-tenant-reporting/views.png)

2. Sağ tıklayın **dbo. Mekanları**.
3. Yeni Sorgu Düzenleyicisi**Penceresine** > CREATE **olarak** > Komut Dosyası Görünümü'nü seçin**New Query Editor Window**

Script diğer *Mekan* görünümleri herhangi nasıl *VenueId*eklemek görmek için .

## <a name="deploy-the-database-used-for-distributed-queries"></a>Dağıtılmış sorgular için kullanılan veritabanını dağıtma

Bu _alıştırma, adhocreporting_ veritabanını dağıtır. Bu, tüm kiracı veritabanlarıarasında sorgulama için kullanılan şema içeren baş veritabanıdır. Veritabanı, örnek uygulamadaki yönetimle ilgili tüm veritabanları için kullanılan sunucu olan varolan katalog sunucusuna dağıtılır.

1. *içinde PowerShell ISE*, açık ... \\\\Öğrenme Modülleri Operasyonel\\Analitik Adhoc\\*Raporlama Demo-AdhocReporting.ps1*. 

1. set **$DemoScenario = 2**, Ad _hoc raporlama veritabanı dağıtın._

1. Komut dosyasını çalıştırmak ve *adhocreporting* veritabanını oluşturmak için **F5** tuşuna basın.

Sonraki bölümde, dağıtılmış sorguları çalıştırmak için kullanılabilen şema veritabanına eklersiniz.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>Dağıtılmış sorguları çalıştırmak için 'kafa' veritabanını yapılandırma

Bu alıştırma, tüm kiracı veritabanları arasında sorgulamayı etkinleştirmek için _adhocreporting_ veritabanına şema (dış veri kaynağı ve dış tablo tanımları) ekler.

1. SQL Server Management Studio'yu açın ve önceki adımda oluşturduğunuz Adhoc Reporting veritabanına bağlanın. Veritabanının adı *adhocreporting*olduğunu.
2. SSMS'te ...\Öğrenme Modülleri\Operasyonel Analitik\Adhoc Raporlama\ _Initialize-AdhocReportingDB.sql'i_ açın.
3. SQL komut dosyasını gözden geçirin ve not alın:

   Elastik Sorgu, kiracı veritabanlarının her birine erişmek için veritabanı kapsamı içinde bir kimlik bilgisi kullanır. Bu kimlik bilgisinin tüm veritabanlarında bulunması ve normalde bu sorguları etkinleştirmek için gereken minimum haklara verilmesi gerekir.

    ![kimlik bilgisi oluşturma](media/saas-tenancy-cross-tenant-reporting/create-credential.png)

   Dış veri kaynağı olarak katalog veritabanı ile, sorgular sorgu çalışır anda katalogda kayıtlı tüm veritabanlarına dağıtılır. Sunucu adları her dağıtım için farklı olduğundan, bu komut dosyası, komut@servernamedosyasının yürütüldüğü geçerli sunucudan (@ ) katalog veritabanının konumunu alır.

    ![dış veri kaynağı oluşturma](media/saas-tenancy-cross-tenant-reporting/create-external-data-source.png)

   Önceki bölümde açıklanan ve **DISTRIBUTION = SHARDED(VenueId)** ile tanımlanan genel görünümlere başvuran dış tablolar. Her *VenueId* ayrı bir veritabanıyla eşleştirin, bu durum bir sonraki bölümde gösterildiği gibi birçok senaryonun performansını artırır.

    ![dış tablolar oluşturma](media/saas-tenancy-cross-tenant-reporting/external-tables.png)

   Oluşturulan ve doldurulan yerel tablo _VenueTypes._ Bu başvuru veri tablosu tüm kiracı veritabanlarında yaygındır, bu nedenle burada yerel bir tablo olarak temsil edilebilir ve ortak verilerle doldurulabilir. Bazı sorgular için, bu tablonun kafa veritabanında tanımlanması, kafa veritabanına taşınması gereken veri miktarını azaltabilir.

    ![tablo oluşturma](media/saas-tenancy-cross-tenant-reporting/create-table.png)

   Başvuru tablolarını bu şekilde eklerseniz, kiracı veritabanlarını her güncelleştirmeniz de tablo şemasını ve verilerini güncelleştirdiğinizden emin olun.

4. Komut dosyasını çalıştırmak ve *adhocreporting* veritabanını başlatmak için **F5** tuşuna basın. 

Artık dağıtılmış sorgular çalıştırabilir ve tüm kiracılar arasında öngörüler toplayabilirsiniz!

## <a name="run-distributed-queries"></a>Dağıtılmış sorguları çalıştırma

*Adhocreporting* veritabanı ayarlı olduğundan, devam edin ve bazı dağıtılmış sorguları çalıştırın. Sorgu işleminin nerede gerçekleştiğini daha iyi anlamak için yürütme planını ekleyin. 

Yürütme planını incelerken, ayrıntılar için plan simgelerinin üzerine tıklayın. 

Dikkat edilmesi gereken nokta, dış veri kaynağı tanımlandığında **DISTRIBUTION = SHARDED(VenueId)** ayarı birçok senaryo için performansı artırır. Her *VenueId* tek bir veritabanına eş ler olarak, filtreleme kolayca uzaktan yapılır ve yalnızca gerekli verileri döndürür.

1. Açık... \\\\Öğrenme Modülleri Operasyonel\\Analitik Adhoc Raporlama\\*Demo-AdhocReportingQueries.sql* SSMS içinde.
2. **Adhocreporting** veritabanına bağlı olduğundan emin olun.
3. **Sorgu** menüsünü seçin ve **Fiili Yürütme Planı Ekle'yi** tıklatın
4. Hangi *mekanların şu anda kayıtlı olduğunu vurgulayın?* sorgu ve **F5 tuşuna**basın.

   Sorgu, tüm kiracılar arasında sorgu lamanın ve her kiracıdan gelen verileri döndürmenin ne kadar hızlı ve kolay olduğunu gösteren tüm mekan listesini döndürür.

   Planı inceleyin ve tüm maliyetin uzak sorguda olduğunu görün. Her kiracı veritabanı sorguyu uzaktan yürütür ve yer bilgilerini baş veritabanına döndürür.

   ![SELECT * FROM dbo. Mekanlar](media/saas-tenancy-cross-tenant-reporting/query1-plan.png)

5. Bir sonraki sorguyu seçin ve **F5 tuşuna**basın.

   Bu sorgu, kiracı veritabanlarından ve yerel *VenueTypes* tablosundaki verileri *(yerel, adhocreporting* veritabanındaki bir tablo olduğu için) birleştirir.

   Planı inceleyin ve maliyetin çoğunluğunun uzaktan sorgu olduğunu görün. Her kiracı veritabanı mekan bilgilerini döndürür ve dostu adı görüntülemek için yerel *VenueTypes* tablosuile yerel bir birleştirme gerçekleştirir.

   ![Uzak ve yerel verilere katılma](media/saas-tenancy-cross-tenant-reporting/query2-plan.png)

6. Şimdi *en çok satılan bilet hangi gün seçin?* **F5**

   Bu sorgu biraz daha karmaşık birleştirme ve toplama yok. İşlemin çoğu uzaktan oluşur.  Her mekanın günlük bilet satış sayısını içeren tek satırlar baş veritabanına döndürülür.

   ![sorgu](media/saas-tenancy-cross-tenant-reporting/query3-plan.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> 
> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma
> * Bir raporlama veritabanı dağıtın ve dağıtılmış sorguları çalıştırmak için gereken şemayı tanımlayın.


Şimdi daha karmaşık analitik işleme için ayrı bir analitik veritabanına veri ayıklama keşfetmek için [Tenant Analytics öğretici](saas-tenancy-tenant-analytics.md) deneyin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip Tickets SaaS Veritabanı'nın Kiracı Başına uygulaması üzerine inşa ek öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Esnek Sorgu](sql-database-elastic-query-overview.md)
