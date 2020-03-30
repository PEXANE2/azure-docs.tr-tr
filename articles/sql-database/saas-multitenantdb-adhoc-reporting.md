---
title: Birden çok veritabanında geçici raporlama sorguları
description: Çok kiracılı bir uygulama örneğinde birden çok SQL veritabanında geçici raporlama sorguları çalıştırın.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: AyoOlubeko
ms.author: craigg
ms.reviewer: sstein
ms.date: 10/30/2018
ms.openlocfilehash: c0d1829c52041446b4feb43d8af262265e2680fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822182"
---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Birden çok Azure SQL veritabanında geçici analitik sorguları çalıştırma

Bu öğreticide, geçici etkileşimli raporlamayı etkinleştirmek için tüm kiracı veritabanları kümesinde dağıtılmış sorgular çalıştırın. Bu sorgular Wingtip Tickets SaaS uygulamasının günlük operasyonel verilerine gömülü öngörüler ayıklayabilir. Bu çıkarmaları yapmak için katalog sunucusuna ek bir analiz veritabanı dağıtın ve dağıtılmış sorguları etkinleştirmek için Elastik Sorgu'yu kullanırsınız.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> 
> * Geçici raporlama veritabanı nasıl dağıtılır?
> * Tüm kiracı veritabanlarında dağıtılmış sorgular nasıl çalıştırılır?


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip Tickets SaaS Çok kiracılı Veritabanı uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için [Wingtip Tickets SaaS Çok kiracılı Veritabanı uygulamasını dağıtın ve keşfedin](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) yüklenir. SSMS'i indirmek ve yüklemek için sql [server management studio (SSMS) indir'e](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)bakın.


## <a name="ad-hoc-reporting-pattern"></a>Özel raporlama deseni

![adhoc raporlama deseni](media/saas-multitenantdb-adhoc-reporting/adhocreportingpattern_shardedmultitenantDB.png)

SaaS uygulamaları, bulutta merkezi olarak depolanan büyük miktardaki kiracı verilerini analiz edebilir. Analizler, uygulamanızın işleyişi ve kullanımı yla ilgili öngörüleri ortaya çıkarır. Bu öngörüler özellik geliştirme, kullanılabilirlik geliştirme ve uygulamalarınıza ve hizmetlerinize yapılan diğer yatırımlara rehberlik edebilir.

Bu verilere tek bir çok kiracılı veritabanında erişim kolaydır, ancak binlerce veritabanına ölçekli olarak dağıtıldığında çok kolay değildir. Bir yaklaşım, ortak şema ile veritabanları dağıtılmış bir dizi arasında sorgu sağlayan [Elastik Sorgu](sql-database-elastic-query-overview.md), kullanmaktır. Bu veritabanları farklı kaynak grupları ve abonelikler arasında dağıtılabilir. Ancak ortak bir giriş tüm veritabanlarından veri ayıklamak için erişim olmalıdır. Elastik Sorgu, dağıtılmış (kiracı) veritabanlarındaki tabloları veya görünümleri yansıtan dış tabloların tanımlandığı tek bir *kafa* veritabanı kullanır. Bu baş veritabanına gönderilen sorgular, gerektiğinde kiracı veritabanlarına gönderilen sorgu kısımlarıyla birlikte dağıtılmış bir sorgu planı oluşturmak üzere derlenir. Elastik Sorgu, tüm kiracı veritabanlarının konumunu belirlemek için katalog veritabanındaki parça eşlemi kullanır. Kurulum ve sorgu standart [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)kullanarak basittir ve Power BI ve Excel gibi araçlardan geçici sorgu desteği.

Elastik Sorgu, sorguları kiracı veritabanları arasında dağıtarak canlı üretim verilerine anında bakış sağlar. Ancak, Elastik Sorgu potansiyel olarak birçok veritabanından veri çektiğinden, sorgu gecikmesi bazen tek bir çok kiracılı veritabanına gönderilen eşdeğer sorgulardan daha yüksek olabilir. Döndürülen verileri en aza indirmek için sorgular tasarladığınızdan emin olun. Elastik Sorgu genellikle, sık kullanılan veya karmaşık analitik sorguları veya raporları oluşturmak yerine, küçük miktarlarda gerçek zamanlı verileri sorgulamak için en uygun olandır. Sorgular iyi performans göstermiyorsa, sorgunun hangi bölümünün uzak veritabanına itildiğini görmek için [yürütme planına](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) bakın. Ve ne kadar verinin döndürüldedildiğini değerlendirin. Karmaşık analitik işleme gerektiren sorgular, ayıklanan kiracı verilerini analitik sorgular için en iyi duruma getirilmiş bir veritabanına kaydederek daha iyi sunulabilir. SQL Veritabanı ve SQL Veri Ambarı bu tür analiz veritabanını barındırabilir.

Analitik için bu desen [kiracı analitik öğretici](saas-multitenantdb-tenant-analytics.md)açıklanmıştır.

## <a name="get-the-wingtip-tickets-saas-multi-tenant-database-application-source-code-and-scripts"></a>Wingtip Biletleri SaaS Çok Kiracı Veritabanı uygulama kaynak kodu ve komut alın

Wingtip Biletleri SaaS Çok kiracıveritabanı komut dosyaları ve uygulama kaynak kodu [WingtipTicketsSaaS-MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub repo mevcuttur. Wingtip Tickets SaaS komut dosyalarını indirmek ve engelini kaldırmak için gereken adımlar için [genel kılavuza](saas-tenancy-wingtip-app-guidance-tips.md) göz atın.

## <a name="create-ticket-sales-data"></a>Bilet satış verileri oluşturma

Sorguları daha ilginç bir veri kümesine karşı çalıştırmak için, bilet jeneratörü çalıştırarak bilet satış verileri oluşturun.

1. *PowerShell ISE*, açık ... \\Öğrenme\\Modülleri Operasyonel\\Analitik\\*Adhoc Raporlama Demo-AdhocReporting.ps1* komut dosyası ve aşağıdaki değerleri ayarlayın:
   * **$DemoScenario** = 1, **Tüm mekanlarda etkinlikler için bilet satın alın.**
2. Komut dosyasını çalıştırmak ve bilet satışları oluşturmak için **F5** tuşuna basın. Komut dosyası çalışırken, bu öğreticideki adımları devam edin. Bilet verileri Run ad *hoc dağıtılmış sorgular* bölümünde sorgulanır, bu nedenle bilet jeneratörünün tamamlanmasını bekleyin.

## <a name="explore-the-tenant-tables"></a>Kiracı tablolarını keşfedin 

Wingtip Tickets SaaS Çok kiracıveritabanı uygulamasında, kiracılar, kiracı verilerinin çok kiracılı bir veritabanında veya tek bir kiracı veritabanında depolandığı ve ikisi arasında taşınabildiği karma kiracı yönetim modelinde depolanır. Tüm kiracı veritabanları arasında sorgu yaparken, Elastik Sorgu'nun verileri kiracı tarafından parçalanan tek bir mantıksal veritabanının parçası gibi işleme sunması önemlidir. 

Bu deseni elde etmek için, tüm kiracı tabloları, verilerin hangi kiracıya ait olduğunu tanımlayan bir *VenueId* sütunu içerir. *VenueId,* Venue adının bir karma olarak hesaplanmıştır, ancak herhangi bir yaklaşım bu sütun için benzersiz bir değer tanıtmak için kullanılabilir. Bu yaklaşım, kiracı anahtarının katalogda kullanılmak üzere hesaplanmış şekilde benzer. *VenueId* içeren tablolar, sorguları paralelleştirmek ve uygun uzak kiracı veritabanına itmek için Elastik Sorgu tarafından kullanılır. Bu, döndürülen veri miktarını önemli ölçüde azaltır ve özellikle verileri tek kiracı veritabanlarında depolanan birden çok kiracı olduğunda performans artışıyla sonuçlanır.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Ad hoc dağıtılmış sorgular için kullanılan veritabanını dağıtma

Bu *alıştırma, adhocreporting* veritabanını dağıtır. Bu, tüm kiracı veritabanlarıarasında sorgulama için kullanılan şema içeren baş veritabanıdır. Veritabanı, örnek uygulamadaki yönetimle ilgili tüm veritabanları için kullanılan sunucu olan varolan katalog sunucusuna dağıtılır.

1. Açık... \\\\ *PowerShell ISE'de* Öğrenme Modülleri Operasyonel Analitik\\*Adhoc Reporting Demo-AdhocReporting.ps1'i* \\bildirin ve aşağıdaki değerleri ayarlayın:
   * **$DemoScenario** = 2, **Dağıtım Ad hoc analitik veritabanı**.

2. Komut dosyasını çalıştırmak ve *adhocreporting* veritabanını oluşturmak için **F5** tuşuna basın.

Sonraki bölümde, dağıtılmış sorguları çalıştırmak için kullanılabilen şema veritabanına eklersiniz.

## <a name="configure-the-head-database-for-running-distributed-queries"></a>Dağıtılmış sorguları çalıştırmak için 'kafa' veritabanını yapılandırma

Bu alıştırma, tüm kiracı veritabanları arasında sorgulama yapılmasını sağlayan geçici raporlama veritabanına şema (dış veri kaynağı ve dış tablo tanımları) ekler.

1. SQL Server Management Studio'yu açın ve önceki adımda oluşturduğunuz Adhoc raporlama veritabanına bağlanın. Veritabanının adı *adhocreporting*olduğunu.
2. SSMS'te ...\Öğrenme Modülleri\Operasyonel Analitik\Adhoc Raporlama\ *Initialize-AdhocReportingDB.sql'i* açın.
3. SQL komut dosyasını gözden geçirin ve aşağıdakileri not edin:

   Elastik Sorgu, kiracı veritabanlarının her birine erişmek için veritabanı kapsamı içinde bir kimlik bilgisi kullanır. Bu kimlik bilgisinin tüm veritabanlarında bulunması ve normalde bu geçici sorguları etkinleştirmek için gereken minimum haklara verilmesi gerekir.

    ![kimlik bilgisi oluşturma](media/saas-multitenantdb-adhoc-reporting/create-credential.png)

   Dış veri kaynağı olarak katalog veritabanı kullanılarak, sorgu çalıştırıldığında sorgular katalogda kayıtlı tüm veritabanlarına dağıtılır. Sunucu adları her dağıtım için farklı olduğundan, bu başlatma komut dosyası, komut dosyasının yürütüldüğü geçerli sunucuyu (@)@servernamealarak katalog veritabanının konumunu alır.

    ![dış veri kaynağı oluşturma](media/saas-multitenantdb-adhoc-reporting/create-external-data-source.png)

   Kiracı tablolarına başvuran dış tablolar **DISTRIBUTION = SHARDED(VenueId)** ile tanımlanır. Bu, belirli bir *VenueId* için bir sorguyu uygun veritabanına yönlendirir ve sonraki bölümde gösterildiği gibi birçok senaryoiçin performansı artırır.

    ![dış tablolar oluşturma](media/saas-multitenantdb-adhoc-reporting/external-tables.png)

   Oluşturulan ve doldurulan yerel tablo *VenueTypes.* Bu başvuru veri tablosu tüm kiracı veritabanlarında yaygındır, bu nedenle burada yerel bir tablo olarak temsil edilebilir ve ortak verilerle doldurulabilir. Bazı sorgular için bu, kiracı veritabanları ve *adhocreporting* veritabanı arasında taşınan veri miktarını azaltabilir.

    ![tablo oluşturma](media/saas-multitenantdb-adhoc-reporting/create-table.png)

   Başvuru tablolarını bu şekilde eklerseniz, kiracı veritabanlarını her güncelleştirmeniz de tablo şemasını ve verilerini güncelleştirdiğinizden emin olun.

4. Komut dosyasını çalıştırmak ve *adhocreporting* veritabanını başlatmak için **F5** tuşuna basın. 

Artık dağıtılmış sorgular çalıştırabilir ve tüm kiracılar arasında öngörüler toplayabilirsiniz!

## <a name="run-ad-hoc-distributed-queries"></a>Ad hoc dağıtılmış sorguları çalıştırma

*Adhocreporting* veritabanı ayarlı olduğundan, devam edin ve bazı dağıtılmış sorguları çalıştırın. Sorgu işleminin nerede gerçekleştiğini daha iyi anlamak için yürütme planını ekleyin. 

Yürütme planını incelerken, ayrıntılar için plan simgelerinin üzerine tıklayın. 

1. *SSMS*olarak , açık ... \\\\Öğrenme Modülleri Operasyonel\\Analitik Adhoc\\*Raporlama Demo-AdhocReportingQueries.sql*.
2. **Adhocreporting** veritabanına bağlı olduğundan emin olun.
3. **Sorgu** menüsünü seçin ve **Fiili Yürütme Planı Ekle'yi** tıklatın
4. Hangi *mekanların şu anda kayıtlı olduğunu vurgulayın?* sorgu ve **F5 tuşuna**basın.

   Sorgu, tüm kiracılar arasında sorgu lamanın ve her kiracıdan gelen verileri döndürmenin ne kadar hızlı ve kolay olduğunu gösteren tüm mekan listesini döndürür.

   Planı inceleyin ve tüm maliyetin uzak sorgu olduğunu görün çünkü her kiracı veritabanına gidiyoruz ve mekan bilgilerini seçiyoruz.

   ![SELECT * FROM dbo. Mekanlar](media/saas-multitenantdb-adhoc-reporting/query1-plan.png)

5. Bir sonraki sorguyu seçin ve **F5 tuşuna**basın.

   Bu sorgu, kiracı veritabanlarından ve yerel *VenueTypes* tablosundaki verileri *(yerel, adhocreporting* veritabanındaki bir tablo olduğu için) birleştirir.

   Planı inceleyin ve her kiracının yer bilgilerini sorguladığımız için maliyetin büyük kısmının uzak sorgu olduğunu görün (dbo). Mekanlar) ve ardından dost adı görüntülemek için yerel *VenueTypes* tablosuile hızlı bir yerel birleştirme yapın.

   ![Uzak ve yerel verilere katılma](media/saas-multitenantdb-adhoc-reporting/query2-plan.png)

6. Şimdi *en çok satılan bilet hangi gün seçin?* **F5**

   Bu sorgu biraz daha karmaşık birleştirme ve toplama yok. Unutulmaması gereken şey, işlemin çoğunun uzaktan yapılması ve bir kez daha, her mekanın günlük toplam bilet satış sayısı için tek bir satır döndürerek yalnızca ihtiyacımız olan satırları geri getirdiğimizdir.

   ![sorgu](media/saas-multitenantdb-adhoc-reporting/query3-plan.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> 
> * Tüm kiracı veritabanlarında dağıtılmış sorguları çalıştırma
> * Geçici bir raporlama veritabanı dağıtın ve dağıtılmış sorguları çalıştırmak için şema ekleyin.

Şimdi daha karmaşık analitik işleme için ayrı bir analitik veritabanına veri ayıklama keşfetmek için [Tenant Analytics öğretici](saas-multitenantdb-tenant-analytics.md) deneyin.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- ??
* Additional [tutorials that build upon the Wingtip Tickets SaaS Multi-tenant Database application](saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
-->

* [Esnek Sorgu](sql-database-elastic-query-overview.md)
