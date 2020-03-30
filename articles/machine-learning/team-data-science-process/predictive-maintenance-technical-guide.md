---
title: Havacılık için tahmine dayalı bakım kılavuzu - Ekip Veri Bilimi Süreci
description: Havacılık, kamu hizmetleri ve taşımacılıkta öngörülebilir bakım için Çözüm Şablonu için teknik bir kılavuz.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: previous-author=fboylu, previous-ms.author=fboylu
ms.openlocfilehash: 0542106f70e96b6c2f63e8ca03d2532de191d365
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477179"
---
# <a name="technical-guide-to-the-solution-template-for-predictive-maintenance-in-aerospace"></a>Havacılık alanında öngörülebilir bakım için Çözüm Şablonu için teknik kılavuz

> [!Important]
> Bu makale amortismana alınmıştır. Havacılıkda Tahmine Dayalı Bakım hakkındaki tartışma hala geçerlidir, ancak güncel bilgiler için [İş Hedef Kitleleri için Çözüme Genel Bakış'a](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace)bakın.


Çözüm şablonları, bir E2E demosu oluşturma işlemini hızlandırmak için tasarlanmıştır. Dağıtılan şablon, aboneliğinizi gerekli bileşenlerle karşılar ve bunlar arasındaki ilişkileri oluşturur. Ayrıca, çözüm şablonunu dağıttıktan sonra yerel makinenize indirip yüklediğiniz bir veri üreteci uygulamasından örnek verilerle veri dizine tohumlar. Jeneratörden gelen veriler veri boru hattını nemlendirir ve daha sonra Power BI panosunda görüntülenebilir makine öğrenme tahminleri oluşturmaya başlar.

Dağıtım işlemi, çözüm kimlik bilgilerinizi ayarlamak için çeşitli adımlarda size yol verir. Dağıtım sırasında sağladığınız çözüm adı, kullanıcı adı ve parola gibi kimlik bilgilerini kaydettiğinizden emin olun. 


Bu makalenin amaçları şunlardır:
- Aboneliğinizde sağlanan başvuru mimarisini ve bileşenleri açıklayın.
- Örnek verileri kendi verilerinizle nasıl değiştireniz gerektiğini gösterin. 
- Çözüm şablonu nasıl değiştirilen gösterin.  

## <a name="overview"></a>Genel Bakış
![Öngörülebilir bakım mimarisi](./media/predictive-maintenance-technical-guide/predictive-maintenance-architecture.png)

Çözümü dağıttığınızda, Event Hub, Stream Analytics, HDInsight, Data Factory ve Machine Learning gibi Azure hizmetlerini etkinleştirir. Mimari diyagram, Havacılık ve Uzay Çözümleri Için Öngörülü Bakım Şablonu'nun nasıl oluşturuldurulunu gösterir. Bu hizmetleri Azure portalında, çözüm dağıtımıyla oluşturulan çözüm şablonu diyagramına tıklayarak (ilgili boru hattı faaliyetlerinin çalıştırılması gerektiğinde isteğe bağlı olarak sağlanan HDInsight hariç) sonradan silinir).
[Diyagramın tam boyutlu bir sürümünü indirin.](https://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png)

Aşağıdaki bölümlerde çözüm bölümleri açıklayınız.

## <a name="data-source-and-ingestion"></a>Veri kaynağı ve alımı
### <a name="synthetic-data-source"></a>Sentetik veri kaynağı
Bu şablon için, kullanılan veri kaynağı, başarılı dağıtımdan sonra yerel olarak çalıştırdığınız karşıdan yüklenmiş bir masaüstü uygulamasından oluşturulur.

Bu uygulamayı indirmek ve yüklemek için yönergeleri bulmak için, çözüm şablonu diyagramındaki ilk düğüm olan Predictive Maintenance Data Generator'u seçin. Talimatlar Properties çubuğunda bulunur. Bu uygulama, Azure [Olay Hub](#azure-event-hub) hizmetini çözüm akışının geri kalanında kullanılan veri noktaları veya olaylarla besler. Bu veri [kaynağı, Turbofan Motor Bozunması Simülasyon Veri Seti](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan)kullanılarak [NASA veri deposundan](https://c3.nasa.gov/dashlink/resources/139/) kamuya açık verilerden türetilmiştir.

Olay oluşturma uygulaması, Azure Etkinlik Hub'ını yalnızca bilgisayarınızda yürütülerken dolduruyor.  

### <a name="azure-event-hub"></a>Azure Event Hub  
[Azure Olay Hub](https://azure.microsoft.com/services/event-hubs/) hizmeti, Sentetik Veri Kaynağı tarafından sağlanan girdinin alıcısıdır.

## <a name="data-preparation-and-analysis"></a>Veri hazırlama ve analiz  
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
[Azure Event Hub](#azure-event-hub) hizmetinden giriş akışında neredeyse gerçek zamanlı analiz sağlamak için Azure Akış [Analizi'ni](https://azure.microsoft.com/services/stream-analytics/) kullanın. Daha sonra sonuçları bir [Power BI](https://powerbi.microsoft.com) panosunda yayımlar ve azure veri [fabrikası](https://azure.microsoft.com/documentation/services/data-factory/) hizmeti tarafından daha sonra işlenmek üzere tüm ham gelen olayları Azure [Depolama](https://azure.microsoft.com/services/storage/) hizmetine arşivlersiniz.

### <a name="hdinsight-custom-aggregation"></a>HDInsight özel toplama
Azure Akışı Analizi kaynağı kullanılarak arşivlenmiş ham olaylarla ilgili toplamalar sağlamak için HDInsight'ı kullanarak [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyalarını (Azure Veri Fabrikası tarafından düzenlendi) çalıştırın.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning Service](https://azure.microsoft.com/services/machine-learning/) (Azure Veri Fabrikası tarafından yönetilen) ile alınan girişleri kullanarak belirli bir uçak motorunun kalan yararlı ömrü (RUL) hakkında tahminlerde bulunun. 

## <a name="data-publishing"></a>Veri yayımlama
### <a name="azure-sql-database"></a>Azure SQL Veritabanı
Azure Machine Learning tarafından alınan ve daha sonra [Power BI](https://powerbi.microsoft.com) panosunda tüketilen tahminleri depolamak için Azure [SQL Veritabanı'nı](https://azure.microsoft.com/services/sql-database/) kullanın.

## <a name="data-consumption"></a>Veri tüketimi
### <a name="power-bi"></a>Power BI
[Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/)tarafından sağlanan toplamave uyarıların yanı sıra Azure Machine [Learning](https://azure.microsoft.com/services/machine-learning/)kullanılarak üretilen Azure [SQL Veritabanı'nda](https://azure.microsoft.com/services/sql-database/) depolanan RUL tahminlerini içeren bir pano göstermek için [Power BI'yi](https://powerbi.microsoft.com) kullanın.

## <a name="how-to-bring-in-your-own-data"></a>Kendi verilerinizi nasıl getirin?
Bu bölümde, kendi verilerinizi Azure'a nasıl getireceğiniz ve bu mimariye getirdiğiniz veriler için hangi alanlarda değişiklik gerektirdiği açıklanmaktadır.

Veri setinizin bu çözüm şablonu için kullanılan [Turbofan Motor Bozunma Simülasyon Veri Seti](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan) tarafından kullanılan veri kümesiyle eşleşmesi olası değildir. Verilerinizi ve gereksinimlerinizi anlamak, bu şablonu kendi verilerinizle çalışacak şekilde nasıl değiştirişinizde çok önemlidir. 

Aşağıdaki bölümlerde, yeni bir veri kümesi tanıtıldığında şablonun değişiklik gerektiren bölümleri tartışılır.

### <a name="azure-event-hub"></a>Azure Event Hub
Azure Etkinlik Hub'ı geneldir; veriler hub'a CSV veya JSON biçiminde nakledilebilir. Azure Etkinlik Hub'ında özel bir işlem yapılmaz, ancak bu işle beslenen verileri anlamanız önemlidir.

Bu belge, verilerinizi nasıl yutabileceğinizi açıklamaz, ancak Etkinlik Hub API'lerini kullanarak etkinlikleri veya verileri bir Azure Etkinlik Hub'ına kolayca gönderebilirsiniz.

### <a name="azure-stream-analytics"></a><a name="azure-stream-analytics-1"></a>Azure Akış Analizi
Veri akışlarından ve veri çıkışlarından herhangi bir sayıda kaynağa bakarak neredeyse gerçek zamanlı analiz sağlamak için Azure Akış Analizi kaynağını kullanın.

Havacılık ve Uzay Çözümü Için Öngörülü Bakım Şablonu için Azure Akış Analizi sorgusu, her biri Azure Event Hub hizmetinden gelen olayları tüketen ve çıkışları dört farklı konuma sahip dört alt sorgudan oluşur. Bu çıktılar üç Power BI veri kümesi ve bir Azure Depolama konumundan oluşur.

Azure Akışı Analizi sorgusu şu şekilde bulunabilir:

* Azure portalına bağlanın
* Çözüm dağıtıldığında oluşturulan Stream ![](./media/predictive-maintenance-technical-guide/icon-stream-analytics.png) Analytics iş lerini bulma *(örneğin,* tahmine dayalı bakım çözümü için **maintenancesa02asapbi** ve **maintenancesa02asablob)**
* Seçme
  
  * Sorgu girişini görüntülemek için ***GIRIŞLER***
  * ***Sorgunun*** kendisini görüntülemek için SORGU
  * FARKLI ÇıKTıLARI GÖRÜNTÜLEMEK IÇIN ***ÇıKTıLAR***

Azure Akış Analizi sorgu yapısı hakkındaki bilgiler MSDN'deki [Akış Analizi Sorgu Başvurusu'nda](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) bulunabilir.

Bu çözümde, sorgular, bu çözüm şablonunun bir parçası olarak sağlanan bir Power BI panosuna gelen veri akışı hakkında neredeyse gerçek zamanlı analiz bilgileriyle birlikte üç veri kümesi çıkar. Gelen veri biçimi hakkında örtük bilgi olduğundan, bu sorguların veri biçiminize göre değiştirilmesi gerekir.

İkinci Stream Analytics iş **bakımındaki** sorgu yalnızca tüm [Event Hub](https://azure.microsoft.com/services/event-hubs/) olaylarını [Azure Depolama'ya](https://azure.microsoft.com/services/storage/) aktarıyor ve bu nedenle tüm etkinlik bilgileri depolamaya aktarıldıkça veri biçiminiz ne olursa olsun hiçbir değişiklik gerektirmez.

### <a name="azure-data-factory"></a>Azure Data Factory
[Azure Veri Fabrikası](https://azure.microsoft.com/documentation/services/data-factory/) hizmeti, verilerin hareketini ve işlenmesini yönetir. Havacılık ve Uzay Çözümü Şablonu için Öngörülü Bakım'da, veri fabrikası çeşitli teknolojileri kullanarak verileri hareket ettiren ve işleyen üç [boru hattından](../../data-factory/concepts-pipelines-activities.md) oluşur.  Çözümün dağıtımıyla oluşturulan çözüm şablonu diyagramının altındaki Veri Fabrikası düğümunu açarak veri fabrikanıza erişin. Veri kümelerinizin altındaki hatalar, veri üreteci başlatılmadan önce veri fabrikasının dağıtılmasından kaynaklanmaktadır. Bu hatalar yoksayılabilir ve veri fabrikanızın çalışmasını engellemez.

![Veri Fabrikası veri kümesi hataları](./media/predictive-maintenance-technical-guide/data-factory-dataset-error.png)

Bu [bölümde, Azure Veri Fabrikası'nda](https://azure.microsoft.com/documentation/services/data-factory/)bulunan gerekli [ardışık hatlar ve etkinlikler](../../data-factory/concepts-pipelines-activities.md) anlatılmaktadır. Çözümün diyagram görünümü aşağıda verilmiştir.

![Azure Data Factory](./media/predictive-maintenance-technical-guide/azure-data-factory.png)

Bu fabrikanın iki boru hattı, verileri bölmek ve toplamak için kullanılan [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyalarını içerir. Not edildiğinde, komut dosyaları kurulum sırasında oluşturulan [Azure Depolama](https://azure.microsoft.com/services/storage/) hesabında bulunur. Konumları: maintenancesascript\\\\\\\\script\\ \\ kovan (veya https://[Çözüm adınız].blob.core.windows.net/maintenancesascript).

Azure [Akışı Analytics](#azure-stream-analytics-1) sorgularına benzer şekilde, [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyaları da gelen veri biçimi hakkında örtük bilgiye sahiptir ve veri biçiminize göre değiştirilmelidir.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*
Bu [ardışık işlem,](../../data-factory/concepts-pipelines-activities.md) [Azure Akış Analizi](https://azure.microsoft.com/services/stream-analytics/) işi sırasında [Azure Depolama'ya](https://azure.microsoft.com/services/storage/) konulan verileri bölmek için [hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyası çalıştıran bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) kullanan tek bir etkinlik içerir. [HDInsightHive](../../data-factory/transform-data-using-hadoop-hive.md)

Bu bölümleme görevi için [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyası ***AggregateFlightInfo.hql*** olduğunu

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
Bu [ardışık işlem,](../../data-factory/concepts-pipelines-activities.md) bu çözüm şablonuyla ilişkili [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) deneyinden alınan puanlı tahminler olan birkaç etkinlik içerir.

Dahil etkinlikler şunlardır:

* [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) denemesi için gerekli toplamaları ve özellik mühendisliğini gerçekleştirmek için [hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyası çalıştıran [bir HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) kullanarak [HDInsightHive](../../data-factory/transform-data-using-hadoop-hive.md) etkinliği.
  Bu bölümleme görevi için [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komut dosyası ***PrepareMLInput.hql***olduğunu.
* [HDInsightHive](../../data-factory/transform-data-using-hadoop-hive.md) etkinliğinden sonuçları [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) etkinliği tarafından erişilen tek bir [Azure Depolama](https://azure.microsoft.com/services/storage/) blob'una taşıyan etkinliği [kopyalayın.](https://msdn.microsoft.com/library/azure/dn835035.aspx)
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) etkinliği, tek bir [Azure Depolama](https://azure.microsoft.com/services/storage/) blob'una konulan sonuçlarla [Birlikte Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) denemesini çağırır.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
Bu [ardışık işlem](../../data-factory/concepts-pipelines-activities.md) tek bir etkinlik içerir - [Azure Machine Learning](#azure-machine-learning) denemesinin sonuçlarını ***MLScoringPipeline'dan*** çözüm şablonu yüklemesinin bir parçası olarak sağlanan [Azure SQL Veritabanı'na](https://azure.microsoft.com/services/sql-database/) aktaran bir [Kopyalama](https://msdn.microsoft.com/library/azure/dn835035.aspx) etkinliği.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Bu çözüm şablonu için kullanılan [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) denemesi, bir uçak motorunun Kalan Yararlı Ömrünü (RUL) sağlar. Deneme, tüketilen veri kümesine özgüdür ve getirilen verilere özgü modifikasyon veya değiştirme gerektirir.

## <a name="monitor-progress"></a>İlerlemeyi İzle
Veri Jeneratörü başlatıldıktan sonra, boru hattı susuz kalmaya başlar ve çözümünüzün farklı bileşenleri veri fabrikası tarafından verilen komutları izleyerek harekete geçerek başlar. Boru hattını izlemenin iki yolu vardır.

* Stream Analytics işlerinden biri, gelen ham verileri blob depolamasına yazar. Çözümü başarıyla dağıttığınız ekrandan çözümünüzün Blob Depolama bileşenine tıklarsanız ve doğru panelde Aç'ı tıklatırsanız, sizi [Azure portalına](https://portal.azure.com/)götürür. Bir kez orada, Blobs tıklayın. Bir sonraki panelde Kapsayıcıların listesini görürsünüz. **maintenancesadata'ya**tıklayın. Bir sonraki panelde **rawdata** klasörü yer alan bir panel de bulunmaktadır. Rawdata klasöründe saat=17 ve saat=18 gibi adlara sahip klasörler bulunur. Bu klasörlerin varlığı, ham verilerin bilgisayarınızda oluşturulduğunu ve blob depolama alanında depolandığını gösterir. Bu klasörlerde MB sonlu boyutlarda csv dosyalarını görmelisiniz.
* Ardışık hattın son adımı, SQL Veritabanına veri (örneğin makine öğreniminden öngörüler) yazmaktır. Verilerin SQL Veritabanı'nda görünmesi için en fazla üç saat beklemeniz gerekebilir. SQL Veritabanınızda ne kadar veri kullanılabileni izlemenin bir yolu [Azure portalı](https://portal.azure.com/)üzerindendir. Sol panelde SQL DATABASES ![SQL](./media/predictive-maintenance-technical-guide/icon-SQL-databases.png) simgesini bulun ve tıklatın. Daha sonra veritabanınızı **bulun** ve üzerine tıklayın. Alttaki sonraki sayfada YÖNET'e tıklayın.
   
    ![Simgeyi yönetme](./media/predictive-maintenance-technical-guide/icon-manage.png)
   
    Burada, Satır sayısı için Yeni Sorgu ve sorgu 'a tıklayabilirsiniz (örneğin PMResult'dan count(*) seçin). Veritabanınız büyüdükçe, tablodaki satır sayısı artacaktır.

## <a name="power-bi-dashboard"></a>Power BI Panosu

Azure Akış Analizi verilerinizi (sıcak yol) ve toplu tahmin sonuçlarınızı Azure makine öğreniminden (soğuk yol) görselleştirmek için bir Power BI panosu ayarlayın.

### <a name="set-up-the-cold-path-dashboard"></a>Soğuk yol panosunu ayarlama
Soğuk yol veri boru hattında amaç, bir uçuşu (döngü) tamamladıktan sonra her uçak motorunun tahmine dayalı RUL'unu (kalan yararlı ömrü) elde etmektir. Tahmin sonucu, son 3 saat içinde uçuşu tamamlayan uçak motorlarını tahmin etmek için her 3 saatte bir güncellenir.

Power BI, tahmin sonuçlarının depolandığı veri kaynağı olarak Azure SQL Veritabanına bağlanır. 

Not: 
1.    Çözümünüzü dağıtılırken, 3 saat içinde veritabanında bir tahmin görüntülenir. Generator indirme ile birlikte gelen pbix dosyası, power bi panosunu hemen oluşturabilmeniz için bazı tohum verileri içerir. 
2.    Bu adımda, ön koşul indirmek ve ücretsiz yazılım [Power BI masaüstü](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)yüklemektir.

Aşağıdaki adımlar, pbix dosyasını, görselleştirme için veri içeren çözüm dağıtımı sırasında (örneğin, tahmin sonuçları) döndürülen SQL Veritabanına nasıl bağladığınız konusunda size yol göstermektedir.

1. Veritabanı kimlik bilgilerini alın.
   
   Sonraki adımlara geçmeden önce **veritabanı sunucusu adı, veritabanı adı, kullanıcı adı ve parola** gerekir. Bunları nasıl bulacağınız konusunda size rehberlik edecek adımlar aşağıda veda edebilirsiniz.
   
   * Çözüm şablon diyagramınızdaki **'Azure SQL Veritabanı'** yeşile döndüğünde, 'Aç'ı tıklatın ve **'Aç'ı**tıklatın.
   * Azure portal sayfasını görüntüleyen yeni bir tarayıcı sekmesi/penceresi görürsünüz. Sol panelde **'Kaynak grupları'nı** tıklatın.
   * Çözümü dağıtmak için kullandığınız aboneliği seçin ve ardından **'YourSolutionName\_ResourceGroup'u**seçin.
   * Yeni açılan panelde, veritabanınıza erişmek için ![SQL simgesini](./media/predictive-maintenance-technical-guide/icon-sql.png) tıklatın. Veritabanı adınız bu simgenin yanındadır (örneğin, **'pmaintenancedb'**) ve **veritabanı sunucusu adı** Sunucu adı özelliği altında listelenir ve **YourSolutionName.database.windows.net**benzer görünmelidir.
   * Veritabanı **kullanıcı adınız** ve **parolanız,** çözümün dağıtımı sırasında daha önce kaydedilmiş olan kullanıcı adı ve parolayla aynıdır.
2. Power BI Desktop ile soğuk yol raporu dosyasının veri kaynağını güncelleştirin.
   
   * Generator dosyasını indirip fermuarını açtığınızda, **\\PowerBI PredictiveMaintenanceAerospace.pbix** dosyasını çift tıklatın. Dosyayı açtığınızda herhangi bir uyarı iletisi görürseniz, bunları yoksayın. Dosyanın üst kısmında **'Sorguları Edit'** seçeneğini tıklatın.
     
     ![Sorguları Düzenle](./media/predictive-maintenance-technical-guide/edit-queries.png)
   * İki tablo, **RemainingUsefulLife** ve **PMResult**görürsünüz. Sağdaki **'Sorgu Ayarları'** ](./media/predictive-maintenance-technical-guide/icon-query-settings.png) panelinde **'APPLIED STEPS'** altında **'Kaynak'** simgesinin yanındaki ilk tabloyu seçin ve Sorgu ayarları simgesini tıklatın. ![ Görünen uyarı iletilerini yoksayın.
   * Açılan pencerede, **'Sunucu'** ve **'Veritabanı'nı** kendi sunucu ve veritabanı adlarla değiştirin ve **ardından 'Tamam'ı**tıklatın. Sunucu adı için, bağlantı noktası 1433 **(YourSolutionName.database.windows.net, 1433)** belirttiğinden emin olun. Veritabanı alanını **pmaintenancedb**olarak bırakın. Ekranda görünen uyarı iletilerini yoksay.
   * Sonraki açılan pencerede, sol bölmede iki seçenek **(Windows** ve **Veritabanı)** görürsünüz. **'Veritabanı'yı**tıklatın, **'Kullanıcı Adınızı'** ve **'Parolanızı'** (çözümü ilk dağıttığınızda ve bir Azure SQL Veritabanı oluşturduğunuzda girdiğiniz kullanıcı adı ve parola) doldurun. ***Bu ayarların hangi düzeye uygulanacağı***seç'te veritabanı düzeyi seçeneğini işaretleyin. Ardından **'Bağlan'** seçeneğini tıklatın.
   * İkinci tablo **PMResult'a** ![tıklayın](./media/predictive-maintenance-technical-guide/icon-navigation.png) ve **sağdaki 'Sorgu Ayarları'** panelinde **'APPLIED STEPS'** altında **'Kaynak'** simgesinin yanına tıklayın ve yukarıdaki adımlardaki gibi sunucu ve veritabanı adlarını güncelleyin ve Tamam'ı tıklatın.
   * Önceki sayfaya yönlendirildikten sonra pencereyi kapatın. İleti görüntülenir - **Uygula'yı**tıklatın. Son olarak, değişiklikleri kaydetmek için **Kaydet** düğmesini tıklatın. Power BI dosyanız artık sunucuya bağlantı kurdu. Görselleriniz boşsa, göstergelerin sağ üst köşesindeki silgi simgesine tıklayarak tüm verileri görselleştirmek için görselleştirmeler üzerindeki seçimleri temizlediğinizden emin olun. Görselleştirmelere yeni verileri yansıtmak için yenileme düğmesini kullanın. Başlangıçta, veri fabrikası her 3 saatte bir yenilenmesi zamanlanırken, yalnızca görselleştirmelerinizdeki tohum verilerini görürsünüz. 3 saat sonra, verileri yenilediğinizde görsellerinize yansıyan yeni öngörüler görürsünüz.
3. (İsteğe bağlı) [Power BI online](https://www.powerbi.com/)soğuk yol panosu yayınlayın. Bu adımda bir Power BI hesabı (veya Office 365 hesabı) gerekir.
   
   * **'Yayımla'yı** tıklatın ve birkaç saniye sonra "Power BI Success'e yayımlama" gösteren bir pencere görüntülenir. yeşil bir onay işareti ile. "Power BI'de PredictiveMaintenanceAerospace.pbix'i aç" linkine tıklayın. Ayrıntılı yönergeleri bulmak için Power [BI Desktop'dan Yayımla'ya](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop)bakın.
   * Yeni bir pano oluşturmak **+** için: sol bölmedeki **Panolar** bölümünün yanındaki işareti tıklatın. Bu yeni pano için "Predictive Maintenance Demo" adını girin.
   * Raporu açtıktan sonra, ![panonuzdaki tüm görselöğeleri sabitlemek için PIN simgesini](./media/predictive-maintenance-technical-guide/icon-pin.png) tıklatın. Ayrıntılı yönergeleri bulmak için bkz: [Rapordan Power BI panosuna bir döşemeyi sabitleyin.](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report)
     Pano sayfasına gidin ve görsellerinizin boyutunu ve konumunu ayarlayın ve başlıklarını değiştirin. Kutucuklarınızı nasıl düzenleyeceğinize ilişkin ayrıntılı yönergeleri bulmak için [bkz.](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename) Burada bazı soğuk yol görselleştirmeler ona sabitlenmiş bir örnek pano.  Veri jeneratörünüzü ne kadar süre çalıştırdığınıza bağlı olarak, görselleştirmeler üzerindeki numaralarınız farklı olabilir.
     <br/>
     ![Son görünüm](./media/predictive-maintenance-technical-guide/final-view.png)
     <br/>
   * Verilerin yenilenmesini zamanlamak için farenizi **PredictiveMaintenanceAerospace** veri kümesinin üzerine](./media/predictive-maintenance-technical-guide/icon-elipsis.png) taşırık, Elipsis simgesini tıklatın ![ve ardından **Yenile'yi Zamanla'yı**seçin.
     <br/>
     > [!NOTE]
     > Bir uyarı iletisi görüyorsanız, **Kimlik Bilgilerini Edit'i** tıklatın ve veritabanı kimlik bilgilerinizin adım 1'de açıklananlarla aynı olduğundan emin olun.
     <br/>
     ![Yenileme zamanlama](./media/predictive-maintenance-technical-guide/schedule-refresh.png)
     <br/>
   * Zamanlama **Yenile** bölümünü genişletin. "Verilerinizi güncel tutun" diye açın.
     <br/>
   * Yenilemeyi ihtiyaçlarınıza göre zamanlayın. Daha fazla bilgi edinmek için [Power BI'de Veri yenilemesi](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi)ne rendeleyin.

### <a name="setup-hot-path-dashboard"></a>Kurulum sıcak yol panosu
Aşağıdaki adımlar, çözüm dağıtımı sırasında oluşturulan Stream Analytics işlerinden gelen veri çıktısını nasıl görselleştirebilirsiniz size yol göstermektedir. Aşağıdaki adımları gerçekleştirmek için [Power BI çevrimiçi](https://www.powerbi.com/) hesabı gereklidir. Hesabınız yoksa, bir hesap [oluşturabilirsiniz.](https://powerbi.microsoft.com/pricing)

1. Azure Akış Analizi'nde (ASA) Güç BI çıkışı ekleyin.
   
   * Azure Akış Analizi & Power BI'deki yönergeleri izlemeniz gerekir: Azure Akış Analizi işinizin çıktısını Power BI panonuz olarak ayarlamak [için akış verilerinin gerçek zamanlı görünürlüğü için](../../stream-analytics/stream-analytics-power-bi-dashboard.md) bir analiz panosu.
   * ASA **sorgusunda aircraftmonitor**, **aircraftalert**ve **flightsbyhour**olan üç çıkış vardır. Sorgu sekmesine tıklayarak sorguyu görüntüleyebilirsiniz. İlk çıktıyı **(aircraftmonitor)** eklediğinizde **Çıktı Alias**, **Dataset Adı** ve **Tablo Adı** aynı olduğundan emin olun (**aircraftmonitor**). **Uçak uyarısı**ve **flightsbyhour**için çıkışlar eklemek için adımları tekrarlayın. Üç çıktı tablolarını da ekledikten ve ASA işini başlattıktan sonra bir onay iletisi almalısınız ("Başlangıç Akışı Analytics iş maintenancesa02asapbi başarılı oldu").
2. [Power BI'ye çevrimiçi](https://www.powerbi.com) giriş yapın
   
   * Çalışma Alanım'daki sol panel Datasets bölümünde ***DATASET*** **aircraftmonitor,** **aircraftalert**ve **flightsbyhour** adlarını ve uçuş saatlerini adlandırır. Bu, önceki adımda Azure Akış Analitiği'nden ittiğinizlerin akış verileridir. Veri kümesi **flightsbyhour,** arkasındaki SQL sorgusunun doğası nedeniyle diğer iki veri kümesiyle aynı anda görünmeyebilir. Ancak, bir saat sonra görünmelidir.
   * ***Görseller*** bölmesinin açık olduğundan ve ekranın sağ tarafında gösterildiğinden emin olun.
3. Verileri Power BI'ye akttİlke kadar, akış verilerini görselleştirmeye başlayabilirsiniz. Aşağıda bazı sıcak yol görselleştirmeler ona sabitlenmiş bir örnek pano. Uygun veri kümelerini temel alan diğer pano döşemeleri oluşturabilirsiniz. Veri jeneratörünüzü ne kadar süre çalıştırdığınıza bağlı olarak, görselleştirmeler üzerindeki numaralarınız farklı olabilir.

    ![Pano görünümü](media/predictive-maintenance-technical-guide/dashboard-view.png)

1. Yukarıdaki karolardan birini oluşturmak için bazı adımlar şunlardır – "Sensor 11 ile Eşik 48,26'nın Filo Görünümü" döşemesi:
   
   * Sol panel Datasets bölümünde ki dataset **aircraftmonitor'u** tıklatın.
   * Çizgi **Grafiği** simgesini tıklatın.
   * **Görseller** bölmesinde "Eksen" altında gösterilebilmek için **Alanlar** bölmesinde **İşlenmiş'i** tıklatın.
   * "S11" ve "s11\_uyarısı"nı tıklayarak her ikisinin de "Değerler" altında görünmesini sağlar. **s11** ve **\_s11 uyarısının**yanındaki küçük oku tıklatın, "Toplam"ı "Ortalama" olarak değiştirin.
   * Üstte **KAYDET'i** tıklatın ve raporu "uçak monitörü" olarak adlandırın. "Uçak monitörü" adlı rapor soldaki **Navigator** bölmesinde **Raporlar** bölümünde gösterilir.
   * Bu çizgi grafiğinin sağ üst köşesindeki **Görseli İkile'yi İkile** simgesini tıklatın. Bir pano seçmeniz için bir "Panoya Pin" penceresi görünebilir. "Tahmine Dayalı Bakım Demosu"nı seçin ve ardından "Pin"i tıklatın.
   * Fareyi panodaki bu döşemenin üzerine tleyin, başlığını "Sensor 11 vs. Threshold 48.26"' olarak değiştirmek için sağ üst köşedeki "düzelt" simgesine tıklayın ve altyazı "Zaman içinde filo genelinde ortalama" başlığını kullanın.

## <a name="delete-your-solution"></a>Çözümünüzü silme
Veri jeneratörü çalıştırmak gibi aktif olarak çözüm kullanmadığınız zaman veri üreteci durdurmak emin olun daha yüksek maliyetlere neden olacaktır. Kullanmıyorsanız çözümü silin. Çözümünüzü silmek, çözümü dağıttığınızda aboneliğinizde sağlanan tüm bileşenleri siler. Çözümü silmek için, çözüm şablonunun sol panelinde çözüm adınızı tıklatın ve sonra **Sil'i**tıklatın.

## <a name="cost-estimation-tools"></a>Maliyet tahmin araçları
Aşağıdaki iki araç, aboneliğinizde Havacılık ve Uzay Çözümü Şablonu için Öngörülü Bakım'ın çalıştırılamasıyla ilgili toplam maliyetleri daha iyi anlamanıza yardımcı olmak için kullanılabilir:

* [Microsoft Azure Maliyet Tahmin Aracı (çevrimiçi)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure Maliyet Tahmin Aracı (masaüstü)](https://www.microsoft.com/download/details.aspx?id=43376)

