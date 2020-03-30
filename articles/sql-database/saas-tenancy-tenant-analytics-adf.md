---
title: Kiracı veritabanlarına karşı analitik sorguları çalıştırma
description: Azure SQL Veritabanı, SQL Veri Ambarı, Azure Veri Fabrikası veya Power BI'den çıkarılan verileri kullanarak kiracılar arası analiz sorguları.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: anumjs
ms.author: anjangsh
ms.reviewer: MightyPen, sstein
ms.date: 12/18/2018
ms.openlocfilehash: 4791cd3a6b6f72c5d9ee4ca828d66b0d361f356c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73816777"
---
# <a name="explore-saas-analytics-with-azure-sql-database-sql-data-warehouse-data-factory-and-power-bi"></a>Azure SQL Veritabanı, SQL Veri Ambarı, Veri Fabrikası ve Power BI ile SaaS analitiğini keşfedin

Bu öğreticide, uçtan uca bir analiz senaryosunda yürürsiniz. Senaryo, kiracı verileri üzerindeki analizlerin yazılım satıcılarını akıllı kararlar verme konusunda nasıl güçlendirebileceğini gösterir. Her kiracı veritabanından çıkarılan verileri kullanarak, örnek Wingtip Tickets SaaS uygulamasını kullanmaları da dahil olmak üzere kiracı davranışı hakkında bilgi edinmek için analitiği kullanırsınız. Bu senaryo üç adım içerir: 

1.  Her kiracı veritabanından verileri bir analiz deposuna, bu durumda bir SQL Veri Ambarı'na **ayıklayın.**
2.  Analiz işleme için **çıkarılan verileri optimize edin.**
3.  Karar verme de yönlendirebilecek yararlı öngörüler ortaya çıkarmak için **İş Zekası** araçlarını kullanın. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Yükleme için kiracı analiz mağazasını oluşturun.
> - Her kiracı veritabanından analiz veri ambarına veri ayıklamak için Azure Veri Fabrikası'nı (ADF) kullanın.
> - Çıkarılan verileri optimize edin (yıldız şemasını yeniden düzenleyin).
> - Analitik veri ambarını sorgulayın.
> - Kiracı verilerindeki eğilimleri vurgulamak ve iyileştirmeler için önerilerde bulunmak için veri görselleştirme için Power BI'yi kullanın.

![architectureOverView](media/saas-tenancy-tenant-analytics/adf_overview.png)

## <a name="analytics-over-extracted-tenant-data"></a>Çıkarılan kiracı verileri üzerinde analitik

SaaS uygulamaları bulutta büyük miktarda kiracı verisi barındırıyor. Bu veriler, uygulamanızın çalışması ve kullanımı ve kiracılarınızın davranışları hakkında zengin bir öngörü kaynağı sağlayabilir. Bu öngörüler özellik geliştirme, kullanılabilirlik geliştirme ve uygulamalara ve platformdaki diğer yatırımlara rehberlik edebilir.

Tüm veriler tek bir çok kiracıveritabanında olduğunda tüm kiracılar Için verilere erişmek kolaydır. Ancak binlerce veritabanına ölçekte dağıtıldığında erişim daha karmaşıktır. Karmaşıklığı evcilleştirmenin bir yolu, verileri bir analişe veritabanına veya sorgu için bir veri ambarına ayıklamaktır.

Bu öğretici, Wingtip Tickets uygulaması için uçlardan uca bir analiz senaryosu sunar. İlk olarak, [Azure Veri Fabrikası (ADF),](../data-factory/introduction.md) her kiracı veritabanından bilet satışlarını ve ilgili verileri ayıklamak için düzenleme aracı olarak kullanılır. Bu veriler bir analiz deposunda hazırlama tablolarına yüklenir. Analiz deposu bir SQL Veritabanı veya SQL Veri Ambarı olabilir. Bu öğretici, analiz deposu olarak [SQL Veri Ambarı'nı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) kullanır.

Daha sonra, çıkarılan veriler dönüştürülür ve [yıldız şema](https://www.wikipedia.org/wiki/Star_schema) tabloları kümesine yüklenir. Tablolar merkezi bir olgu tablosu artı ilgili boyut tablolarından oluşur:

- Yıldız şemasındaki merkezi gerçek tablosu bilet verilerini içerir.
- Boyut tabloları mekanlar, etkinlikler, müşteriler ve satın alma tarihleri hakkında veriler içerir.

Merkezi ve boyut tabloları birlikte verimli analitik işleme sağlar. Bu öğreticide kullanılan yıldız şema aşağıdaki resimde görüntülenir:
 
![architectureOverView](media/saas-tenancy-tenant-analytics/starschematables.JPG)

Son olarak, yıldız şema tabloları sorgulanır. Sorgu sonuçları, kiracı davranışı ve uygulamanın kullanımı yla ilgili öngörüleri vurgulamak için Power BI kullanılarak görsel olarak görüntülenir. Bu yıldız şeması yla, şu sorguları ortaya çıkaran sorgular çalıştırın:

- Kim bilet alıyor ve hangi mekandan.
- Bilet satışındaki desenler ve trendler.
- Her mekanın göreceli popülaritesi.

Bu öğretici, Wingtip Tickets verilerinden elde edilebilen öngörülerin temel örneklerini sağlar. Her mekanın hizmeti nasıl kullandığını anlamak, Wingtip Biletleri satıcısının, örneğin az çok etkin mekanları hedefleyen farklı hizmet planları hakkında düşünmesine neden olabilir. 

## <a name="setup"></a>Kurulum

### <a name="prerequisites"></a>Ön koşullar

> [!NOTE]
> Bu öğretici, Azure Veri Fabrikası'nın şu anda sınırlı bir önizlemede (bağlantılı hizmet parametreleştirmesi) olan özelliklerini kullanır. Bu öğretici yapmak istiyorsanız, abonelik kimliğinizi [burada](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxrVywox1_tHk9wgd5P8SVJUNlFINjNEOElTVFdMUEREMjVVUlJCUDdIRyQlQCN0PWcu)sağlayın. Aboneliğiniz etkinleştirilir etkinleştirilmez size bir onay göndereceğiz.

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:
- Wingtip Tickets SaaS Veritabanı Kiracı Başına uygulaması dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için [Bkz.](saas-dbpertenant-get-started-deploy.md)
- Wingtip Biletleri SaaS Veritabanı Kiracı başına komut dosyaları ve uygulama [kaynak kodu](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/) GitHub indirilir. İndirme yönergelerine bakın. İçeriğini ayıklamadan önce *zip dosyasının engelini kaldırdıktan* emin olun.
- Power BI Desktop yüklendi. [Download Power BI Masaüstü](https://powerbi.microsoft.com/downloads/).
- Ek kiracılar toplu sağlanmıştır, [**Provision kiracı öğretici**](saas-dbpertenant-provision-and-catalog.md)bakın.

### <a name="create-data-for-the-demo"></a>Demo için veri oluşturma

Bu öğretici, bilet satış verileri üzerinden analizleri inceler. Bu adımda, tüm kiracılar için bilet verileri oluşturursunuz. Daha sonraki bir adımda, bu veriler çözümleme için ayıklanır. Bir dizi farklı bilet satın alma modelini ortaya çıkarmak için yeterli veriye sahip olmak için kiracı toplu (daha önce açıklandığı gibi) _sağladığından emin olun._

1. PowerShell ISE'de açık *...\Öğrenme Modülleri\Operasyonel Analitik\Kiracı Analytics DW\Demo-TenantAnalyticsDW.ps1*, ve aşağıdaki değeri ayarlayın:
    - **$DemoScenario** = **1** Tüm mekanlardaki etkinlikler için bilet satın alın
2. Komut dosyasını çalıştırmak ve tüm mekanlar için bilet satın alma geçmişi oluşturmak için **F5** tuşuna basın. 20 kiracı ile, script bilet on binlerce oluşturur ve 10 dakika veya daha fazla sürebilir.

### <a name="deploy-sql-data-warehouse-data-factory-and-blob-storage"></a>SQL Veri Ambarı, Veri Fabrikası ve Blob Depolama dağıtma 
Wingtip Tickets uygulamasında, kiracıların işlem verileri birçok veritabanına dağıtılır. Azure Veri Fabrikası (ADF), bu verilerin Ayıklanması, Yüklenve Dönüşümü (ELT) için veri ambarına kullanılır. Verileri EN verimli şekilde SQL Veri Ambarı'na yüklemek için ADF verileri ara blob dosyalarına ayıklar ve verileri veri ambarına yüklemek için [PolyBase'i](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading) kullanır.   

Bu adımda, öğreticide kullanılan ek kaynakları dağıtırsınız: _tenantanalytics_adlı bir SQL Veri Ambarı, _\<dbtodwload kullanıcısı\>_ olarak adlandırılan bir Azure Veri Fabrikası ve _wingtipstaging\<kullanıcısı\>_ olarak adlandırılan bir Azure depolama hesabı. Depolama hesabı, çıkarılan veri dosyalarını veri ambarına yüklenmeden önce geçici olarak blob olarak tutmak için kullanılır. Bu adım ayrıca veri ambarı şemasını dağır ve ELT işlemini düzenleyen ADF ardışık hatlarını tanımlar.
1. PowerShell ISE'de açık *...\Öğrenme Modülleri\Operasyonel Analitik\Kiracı Analitiği DW\Demo-TenantAnalyticsDW.ps1* ve set:
    - **$DemoScenario** = **2** Kiracı analitik veri ambarı, blob depolama ve veri fabrikası dağıtma 
1. Demo komut dosyasını çalıştırmak ve Azure kaynaklarını dağıtmak için **F5** tuşuna basın. 

Şimdi dağıttığınız Azure kaynaklarını inceleyin:
#### <a name="tenant-databases-and-analytics-store"></a>Kiracı veritabanları ve analiz mağazası
Kiracılara bağlanmak için [SQL Server Management Studio'yu (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) **kullanın1-dpt-&lt;kullanıcı&gt; ** ve **katalog-dpt-&lt;kullanıcı&gt; ** sunucularına. Uygulamayı &lt;&gt; dağıtırken kullanıcıyı kullanılan değerle değiştirin. Giriş Kullan = *geliştirici* ve Şifre = *P\@ssword1*. Daha fazla rehberlik için [giriş öğreticisi](saas-dbpertenant-wingtip-app-overview.md) bakın.

![SSMS'ten SQL Veritabanı sunucusuna bağlanın](media/saas-tenancy-tenant-analytics/ssmsSignIn.JPG)

Nesne Gezgini'nde:

1. *Kiracı1-dpt-&lt;kullanıcı&gt; * sunucusunu genişletin.
1. Veritabanları düğümlerini genişletin ve kiracı veritabanları listesine bakın.
1. *&lt;Katalog-dpt- kullanıcı&gt; * sunucusunu genişletin.
1. Aşağıdaki nesneleri içeren analiz deposunu gördüğünüzden doğrulayın:
    1. Tablolar **raw_Tickets**, **raw_Customers,** **raw_Events** ve **raw_Venues** kiracı veritabanlarından ham çıkarılan verileri tutun.
    1. Yıldız-şema tabloları **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, ve **dim_Dates**.
    1. Saklanan yordam, **sp_transformExtractedData** verileri dönüştürmek ve yıldız şema tablolarına yüklemek için kullanılır.

![DWtables](media/saas-tenancy-tenant-analytics/DWtables.JPG)

#### <a name="blob-storage"></a>Blob depolama
1. Azure [Portalı'nda,](https://ms.portal.azure.com)uygulamayı dağıtmak için kullandığınız kaynak grubuna gidin. **Wingtipstaging\<kullanıcısı\> ** adlı bir depolama hesabının eklendiğini doğrulayın.

   ![DWtables](media/saas-tenancy-tenant-analytics/adf-staging-storage.PNG)

1. Mevcut nesneleri keşfetmek için **wingtipstaging\<kullanıcı\> ** depolama hesabı'nı tıklatın.
1. **Blobs döşemesi** tıklayın
1. Kapsayıcı **configfile'yi** tıklatın
1. **Configfile** **TableConfig.json**adlı bir JSON dosyası içerdiğini doğrulayın. Bu dosya kaynak ve hedef tablo adları, sütun adları ve izci sütun adı içerir.

#### <a name="azure-data-factory-adf"></a>Azure Veri Fabrikası (ADF)
Kaynak grubundaki Azure _\<Portalı'nda, dbtodwload kullanıcısı\> _ olarak adlandırılan bir Azure Veri Fabrikası eklendiğini doğrulayın. [Azure Portal](https://ms.portal.azure.com) 

 ![adf_portal](media/saas-tenancy-tenant-analytics/adf-data-factory-portal.png)

Bu bölümde oluşturulan veri fabrikası inceletir. Veri fabrikasını başlatmak için aşağıdaki adımları izleyin:
1. Portalda, **dbtodwload-\<kullanıcı\>** adlı veri fabrikası tıklayın.
2. Veri Fabrikası tasarımcısını ayrı bir sekmede başlatmak için **Yazar & Monitör'ü** tıklatın. 

## <a name="extract-load-and-transform-data"></a>Verileri Ayıkla, Yükle ve Dönüştür
Azure Veri Fabrikası, verilerin ayıklanması, yüklenmesi ve dönüştürülmesini düzenlemek için kullanılır. Bu öğreticide, kiracı veritabanlarının her birinden dört farklı SQL görünümünden veri ayıklarsınız: **rawTickets,** **rawCustomers,** **rawEvents**, ve **rawVenues**. Bu görünümler mekan Kimliği'ni içerir, böylece veri ambarındaki her mekandaki verileri ayırt edebilirsiniz. Veriler veri ambarındaki ilgili evreleme tablolarına yüklenir: **raw_Tickets,** **raw_customers,** **raw_Events** ve **raw_Venue.** Depolanan bir yordam daha sonra ham verileri dönüştürür ve yıldız şema tabloları doldurulur: **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, ve **dim_Dates**.

Önceki bölümde, veri fabrikası da dahil olmak üzere gerekli Azure kaynaklarını dağıttınız ve başlattınız. Dağıtılan veri fabrikası, kiracı verilerini ayıklamak, yüklemek ve dönüştürmek için gereken ardışık hatları, veri kümelerini, bağlantılı hizmetleri vb. içerir. Bu nesneleri daha fazla inceleyelim ve sonra verileri kiracı veritabanlarından veri ambarına taşımak için ardışık hattı tetikleyelim.

### <a name="data-factory-pipeline-overview"></a>Veri fabrikası boru hattına genel bakış
Bu bölümde veri fabrikasında oluşturulan nesneler inceletir. Aşağıdaki şekil, bu öğreticide kullanılan ADF ardışık hattının genel iş akışını açıklar. Ardışık ardışık hattı daha sonra keşfetmek ve önce sonuçları görmek isterseniz, bir sonraki bölüme atlayın **Boru hattı çalışmasını tetikle.**

![adf_overview](media/saas-tenancy-tenant-analytics/adf-data-factory.PNG)

Genel bakış sayfasında, sol paneldeki **Yazar** sekmesine geçin ve üç [ardışık hat lar](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) ve üç veri [kümesi](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) oluşturulduğunu gözlemleyin.
![adf_author](media/saas-tenancy-tenant-analytics/adf_author_tab.JPG)

İç içe üç ardışık hat: SQLDBToDW, DBCopy ve TableCopy.

**Pipeline 1 - SQLDBToDW** Katalog veritabanında depolanan kiracı veritabanlarının adlarını arar (tablo adı: [__ShardManagement].[ ShardsGlobal]) ve her kiracı veritabanı için **DBCopy** ardışık çalışır. Tamamlandıktan sonra, sağlanan **sp_TransformExtractedData** depolanan yordam şeması, yürütülür. Bu depolanan yordam, evreleme tablolarında yüklenen verileri dönüştürür ve yıldız şema tablolarını doldurur.

**Pipeline 2 - DBCopy,** blob depolamada depolanan bir yapılandırma dosyasından kaynak tabloların ve sütunların adlarını arar.  **TableCopy** ardışık hattı dört tablonun her biri için çalıştırılır: TicketFacts, CustomerFacts, EventFacts ve VenueFacts. **[Foreach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)** etkinliği, 20 veritabanının tümü için paralel olarak yürütülür. ADF, en fazla 20 döngü yinelemesinin paralel olarak çalıştırılmasına izin verir. Daha fazla veritabanları için birden çok ardışık ardışık ardışık ardışık ardışık    

**Pipeline 3 - TableCopy,** değiştirilen veya güncelleştirilen satırları tanımlamak için SQL Veritabanı'ndaki _(rowversion)_ satır sürüm numaralarını kullanır. Bu etkinlik, kaynak tablolardan satır ayıklamak için başlangıç ve bitiş satırı sürümünü arar. Her kiracı veritabanında depolanan **CopyTracker** tablosu, her çalıştırmadaki her kaynak tablodan çıkarılan son satırı izler. Yeni veya değiştirilen satırlar veri ambarındaki ilgili evreleme tablolarına kopyalanır: **raw_Tickets,** **raw_Customers,** **raw_Venues**ve **raw_Events.** Son olarak son satır sürümü **copytracker** tablosunda bir sonraki çıkarma için ilk satır sürümü olarak kullanılmak üzere kaydedilir. 

Ayrıca, veri fabrikasını kaynak SQL Veritabanlarına, hedef SQL Veri Ambarı'na ve ara Blob depolamasına bağlayan üç parametreli bağlantılı hizmet de vardır. **Yazar** sekmesinde, aşağıdaki resimde gösterildiği gibi bağlantılı hizmetleri keşfetmek için **Bağlantılar'ı** tıklatın:

![adf_linkedservices](media/saas-tenancy-tenant-analytics/linkedservices.JPG)

Üç bağlantılı hizmete karşılık gelen üç veri kümesi, ardışık hatlar etkinliklerinde kullandığınız verileri giriş veya çıktı olarak ifade eder. Kullanılan bağlantıları ve parametreleri gözlemlemek için veri kümelerinin her birini keşfedin. _AzureBlob,_ kaynak ve hedef tablolar ve sütunların yanı sıra her kaynaktaki izci sütununu içeren yapılandırma dosyasını işaret eder.
  
### <a name="data-warehouse-pattern-overview"></a>Veri ambarı deseni genel bakış
SQL Veri Ambarı, kiracı verilerinde toplama gerçekleştirmek için analitik deposu olarak kullanılır. Bu örnekte, PolyBase SQL Veri ambarına veri yüklemek için kullanılır. Ham veriler, yıldız şeması tablolarına dönüştürülmüş satırları izlemek için kimlik sütununa sahip hazırlama tablolarına yüklenir. Aşağıdaki resimde yükleme deseni ![gösterilmektedir: yükleme deseni](media/saas-tenancy-tenant-analytics/loadingpattern.JPG)

Bu örnekte Yavaş Değişen Boyut (SCD) türü 1 boyut tabloları kullanılmıştır. Her boyutun bir kimlik sütunu kullanılarak tanımlanan bir vekil anahtarı vardır. En iyi yöntem olarak, tarih boyutu tablosu zaman kazanmak için önceden doldurulur. Diğer boyut tabloları için SELECT olarak BIR CREATE TABLE... (CTAS) deyimi, vekil anahtarlarıyla birlikte varolan değiştirilmiş ve değiştirilmemiş satırları içeren geçici bir tablo oluşturmak için kullanılır. Bu IDENTITY_INSERT=ON ile yapılır. Yeni satırlar daha sonra IDENTITY_INSERT=OFF ile tabloya eklenir. Kolay geri dönüş için, varolan boyut tablosu yeniden adlandırılır ve geçici tablo yeni boyut tablosu olarak yeniden adlandırılır. Her çalıştırmadan önce, eski boyut tablosu silinir.

Boyut tabloları olgu tablosundan önce yüklenir. Bu sıralama, gelen her gerçek için başvurulan tüm boyutların zaten var olmasını sağlar. Gerçekler yüklendikçe, ilgili her boyut için iş anahtarı eşleşir ve ilgili vekil anahtarları her gerçeğe eklenir.

Dönüşümün son adımı, evreleme verilerini ardışık düzeneçlerinin bir sonraki yürütülmesi için hazır olarak siler.
   
### <a name="trigger-the-pipeline-run"></a>Boru hattı çalışmasını tetikle
Tüm kiracı veritabanları için tam ayıklama, yükleme ve dönüştürme ardışık hattı çalıştırmak için aşağıdaki adımları izleyin:
1. ADF kullanıcı arabiriminin **Yazar** sekmesinde, sol bölmeden **SQLDBToDW** ardışık hattını seçin.
1. **Tetikle'yi** tıklatın ve aşağı çekilen menüden **Şimdi Tetikle'yi**tıklatın. Bu eylem boru hattını hemen çalıştırın. Üretim senaryosunda, verileri zamanlamada yenilemek için ardışık hattı çalıştırmak için bir zaman çizelgesi tanımlarsınız.
  ![adf_trigger](media/saas-tenancy-tenant-analytics/adf_trigger.JPG)
1. **Pipeline Run** **sayfasında, Finish'i**tıklatın.
 
### <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme
1. ADF kullanıcı arabiriminde, soldaki menüden **Monitör** sekmesine geçin.
1. SQLDBToDW ardışık durum **başarılı**olana kadar **Yenile'yi** tıklatın.
  ![adf_monitoring](media/saas-tenancy-tenant-analytics/adf_monitoring.JPG)
1. SSMS ile veri ambarına bağlanın ve verilerin bu tablolara yüklendiğini doğrulamak için yıldız şema tablolarını sorgulayın.

Boru hattı tamamlandıktan sonra, olgu tablosu tüm mekanların bilet satış verilerini tutar ve boyut tabloları ilgili mekanlar, etkinlikler ve müşterilerle doldurulur.

## <a name="data-exploration"></a>Veri Arama

### <a name="visualize-tenant-data"></a>Kiracı verilerini görselleştirin

Yıldız şemasındaki veriler, analiziniz için gereken tüm bilet satış verilerini sağlar. Verileri grafik olarak görselleştirmek, büyük veri kümelerinde eğilimleri görmeyi kolaylaştırır. Bu bölümde, veri ambarındaki kiracı verilerini işlemek ve görselleştirmek için **Power BI'yi** kullanırsınız.

Power BI'ye bağlanmak ve daha önce oluşturduğunuz görünümleri almak için aşağıdaki adımları kullanın:

1. Power BI masaüstünü başlatın.
2. Ana sayfa şeridinden **Veri Al'ı**seçin ve **Daha Fazla...** menüden.
3. Veri **Al** penceresinde **Azure SQL Veritabanı'nı**seçin.
4. Veritabanı giriş penceresinde, sunucu adınızı girin **(katalog-dpt-&lt;Kullanıcı&gt;.database.windows.net).** **Veri Bağlantısı Modu**için **İçe Aktar'ı** seçin ve ardından **Tamam'ı**tıklatın. 

    ![oturum açma-ve güç-bi](./media/saas-tenancy-tenant-analytics/powerBISignIn.PNG)

5. Sol bölmede **Veritabanı'nı** seçin, ardından kullanıcı adı = *geliştirici*girin ve parola = *P\@ssword1*girin. **Bağlan**'a tıklayın.  

    ![veritabanı oturum açma](./media/saas-tenancy-tenant-analytics/databaseSignIn.PNG)

6. **Navigator** bölmesinde, analitik veritabanı altında, yıldız şema tablolarını seçin: **fact_Tickets**, **dim_Events,** **dim_Venues,** **dim_Customers** ve **dim_Dates.** Ardından **Yükle'yi**seçin. 

Tebrikler! Verileri Power BI'ye başarıyla yükledin. Şimdi kiracılarınız hakkında bilgi edinmek için ilginç görselleri keşfedin. Analizin Wingtip Tickets iş ekibine nasıl veri odaklı öneriler sunabileceğini inceleyelim. Öneriler, iş modelini ve müşteri deneyimini optimize etmeye yardımcı olabilir.

Mekanlardaki kullanım değişimini görmek için bilet satış verilerini analiz ederek başlayın. Her mekan tarafından satılan toplam bilet sayısının bir çubuk grafiğini çizmek için Power BI'de gösterilen seçenekleri seçin. (Bilet jeneratöründeki rastgele değişim nedeniyle, sonuçlarınız farklı olabilir.)
 
![Toplam BiletByVenues](./media/saas-tenancy-tenant-analytics/TotalTicketsByVenues-DW.PNG)

Yukarıdaki arsa her mekan tarafından satılan bilet sayısı değişir onaylar. Daha fazla bilet satan mekanlar, daha az bilet satan mekanlara göre hizmetinizi daha yoğun bir şekilde kullanır. Burada kaynak tahsisini farklı kiracı ihtiyaçlarına göre uyarlama fırsatı olabilir.

Bilet satışlarının zaman içinde nasıl değiştiğini görmek için verileri daha da analiz edebilirsiniz. Her gün 60 günlük bir süre için satılan toplam bilet sayısını çizmek için Power BI'de aşağıdaki resimde gösterilen seçenekleri seçin.
 
![SaleVersusDate](./media/saas-tenancy-tenant-analytics/SaleVersusDate-DW.PNG)

Önceki grafik, bazı mekanlar için bilet satışlarının arttığını göstermektedir. Bu ani artışlar, bazı mekanların sistem kaynaklarını orantısız bir şekilde tüketiyor olabileceği fikrini güçlendiriyor. Şimdiye kadar ani artışlar meydana geldiğinde belirgin bir desen yoktur.

Sonraki bu pik satış gün önemini araştıralım. Biletler satışa çıktıktan sonra bu zirveler ne zaman gerçekleşir? Günde satılan biletleri çizmek için Power BI'de aşağıdaki resimde gösterilen seçenekleri seçin.

![SaleDayDistribution](./media/saas-tenancy-tenant-analytics/SaleDistributionPerDay-DW.PNG)

Bu arsa bazı mekanları satış ın ilk gününde bilet çok sayıda satmak gösterir. Bu mekanlarda biletler satışa gider gitmez, çılgın bir koşuşturma var gibi görünüyor. Birkaç mekanın bu etkinlik patlaması diğer kiracıların hizmetini etkileyebilir.

Bu çılgın koşuşturmanın bu mekanların barındırılan tüm etkinlikleri için doğru olup olmadığını görmek için verileri tekrar delebilirsiniz. Önceki arsalarda, Contoso Konser Salonu'nun birçok bilet sattığını ve Contoso'nun da belirli günlerde bilet satışlarında ani bir artış olduğunu gördün. Contoso Konser Salonu için kümülatif bilet satışlarını planlamak için Power BI seçenekleriyle oynayın ve her bir etkinlik için satış trendlerine odaklanın. Tüm etkinlikler aynı satış modelini takip eder mi? Aşağıdaki gibi bir arsa üretmeye çalışın.

![ContosoSatış](media/saas-tenancy-tenant-analytics/EventSaleTrends.PNG)

Her etkinlik için Contoso Konser Salonu için zaman içinde kümülatif bilet satış bu arsa deli acele tüm etkinlikler için olmaz gösterir. Diğer mekanlar için satış eğilimlerini keşfetmek için filtre seçenekleriyle oynayın.

Bilet satış alışkanlıklarına ilişkin öngörüler Wingtip Tickets'in iş modellerini optimize etmesine yol açabilir. Tüm kiracıları eşit ücretlendirmek yerine, belki wingtip farklı işlem boyutlarına sahip hizmet katmanları sunmalıdır. Günde daha fazla bilet satması gereken daha büyük mekanlara daha yüksek bir hizmet düzeyi anlaşması (SLA) ile daha yüksek bir katman sunulabilir. Bu mekanların veritabanları veritabanı başına daha yüksek kaynak sınırları ile havuza yerleştirilmiş olabilir. Her hizmet katmanında, tahsisatı aşmak için ek ücretler tahsil edilen saatlik satış tahsisi olabilir. Periyodik satış patlamaları olan daha büyük mekanlar daha yüksek katmanlardan yararlanacak ve Wingtip Tickets hizmetlerinden daha verimli bir şekilde para kazanabilir.

Bu arada, bazı Wingtip Biletleri müşterileri, hizmet maliyetini haklı çıkarmak için yeterli bilet satmak için mücadele şikayet. Belki de bu anlayışlar orada düşük performanslı mekanlar için bilet satışlarını artırmak için bir fırsattır. Daha yüksek satışlar, hizmetin algılanan değerini artırır. Sağ tıklayın fact_Tickets ve **Yeni ölçü seçin.** **AverageTicketsSold**adlı yeni ölçü için aşağıdaki ifadeyi girin:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Göreceli başarılarını belirlemek için her mekan tarafından satılan yüzde biletleri çizmek için aşağıdaki görselleştirme seçeneklerini seçin.

![AvgTicketsByVenues](media/saas-tenancy-tenant-analytics/AvgTicketsByVenues-DW.PNG)

Yukarıdaki arsa çoğu mekan biletlerinin% 80'den fazlasını satmak olsa da, bazı koltuklarının yarısından fazlasını doldurmak için mücadele olduğunu göstermektedir. Her mekan için satılan biletlerin maksimum veya minimum yüzdesini seçmek için Values Well ile oynayın.

## <a name="embedding-analytics-in-your-apps"></a>Uygulamalarınıza analitik yerleştirme 
Bu öğretici, yazılım satıcısının kiracılarını anlamasını iyileştirmek için kullanılan kiracılar arası analize odaklanmıştır. Analytics, işlerini daha etkin bir şekilde yönetmelerine yardımcı olmak için _kiracılara_öngörüler de sağlayabilir. 

Wingtip Biletleri örneğinde, bilet satışlarının öngörülebilir desenleri izleme eğiliminde olduğunu daha önce keşfettiniz. Bu öngörü, düşük performans gösteren mekanların bilet satışlarını artırmasına yardımcı olmak için kullanılabilir. Belki de etkinlikler için bilet satışlarını tahmin etmek için makine öğrenimi tekniklerini kullanma fırsatı vardır. Fiyat değişikliklerinin etkileri de modellenebilir, teklif indirimlerinin etkisinin tahmin edilmesine izin vermek için. Power BI Embedded, indirimlerin satılan toplam koltuklar ve gelirin düşük satış etkinlikleri üzerindeki etkisi de dahil olmak üzere, tahmin edilen satışları görselleştirmek için bir etkinlik yönetimi uygulamasına entegre edilebilir. Power BI Embedded ile, hatta aslında bilet fiyatlarına indirim uygulayarak entegre edebilirsiniz, sağ görselleştirme deneyimi.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kiracı analitiği için yıldız şemasıyla doldurulan bir SQL Veri Ambarı dağıtın.
> * Her kiracı veritabanından analitik veri ambarına veri ayıklamak için Azure Veri Fabrikası'nı kullanın.
> * Çıkarılan verileri optimize edin (yıldız şemasını yeniden düzenleyin).
> * Analitik veri ambarını sorgulayın. 
> * Tüm kiracılardaki verilerdeki eğilimleri görselleştirmek için Power BI'yi kullanın.

Tebrikler!

## <a name="additional-resources"></a>Ek kaynaklar

- [Wingtip SaaS uygulaması üzerine inşa](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)ek öğreticiler .
