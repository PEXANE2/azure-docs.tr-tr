---
title: Havacılık - Team Data Science Process için Tahmine dayalı bakım için kılavuz
description: Microsoft Cortana Intelligence çözüm şablonuyla teknik Kılavuzu Havacılık, yardımcı programlar ve nakliye Tahmine dayalı bakım için.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=fboylu, previous-ms.author=fboylu
ms.openlocfilehash: 9871e1402336f5ad282c12f959d45fda85512a84
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721855"
---
# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace"></a>Havacılıkta Tahmine dayalı bakım için Cortana Intelligence çözüm şablonları için teknik kılavuz

> [!Important]
> Bu makalede kullanım dışıdır. Aerospace 'de tahmine dayalı bakım hakkında tartışma hala geçerlidir, ancak geçerli bilgiler için [Iş kitlelerine yönelik çözüme genel bakış](https://github.com/Azure/cortana-intelligence-predictive-maintenance-aerospace)bölümüne bakın.


Çözüm şablonları, Cortana Intelligence Suite üzerine bir E2E tanıtım oluşturma sürecini hızlandırmak için tasarlanmıştır. Dağıtılan şablon, gerekli Cortana Intelligence bileşenleri aboneliğinizle sağlar ve ardından aralarındaki ilişkilerin oluşturur. Ayrıca, veri işlem hattı örnek verilerle indirin ve yerel makinenize yükleyin, çözüm şablonu dağıttıktan sonra bir veri Oluşturucu uygulamasını sağlar. Oluşturucu verileri, veri işlem hattı ve ardından Power BI panosunda görselleştirilebilir machine learning öngörüleri, oluşturma başlangıç hydrates.

Dağıtım işlemi, çözüm kimlik bilgilerini ayarlamak için birkaç adım size kılavuzluk eder. Çözüm adı, kullanıcı adını ve dağıtım sırasında sağladığınız parola gibi kimlik bilgilerini kaydetmek emin olun. 


İçin bu makalenin amaçları şunlardır:
- Başvuru mimarisi ve bileşenleri, aboneliğinizde sağlanan açıklanmaktadır.
- Örnek verileri kendi verilerinizle nasıl değiştirileceğini göstermektedir. 
- Çözüm şablonu değiştirme işlemini göstermektedir.  

> [!TIP]
> [Bu makalenin bir PDF sürümünü](https://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf)indirebilir ve yazdırabilirsiniz.
> 
> 

## <a name="overview"></a>Genel Bakış
![Tahmine dayalı bakım mimarisi](./media/cortana-analytics-technical-guide-predictive-maintenance/predictive-maintenance-architecture.png)

Çözümünü dağıttığınızda, Cortana Analytics paketi (Event Hub, Stream Analytics, HDInsight, Data Factory ve Machine Learning dahil olmak üzere) içindeki Azure hizmetlerini etkinleştirir. Mimari diyagramı, Tahmine dayalı bakım için Havacılık çözüm şablonu nasıl oluşturulur gösterir. Çözüm dağıtımı (dışında ilgili işlem hattı etkinliklerinin çalıştırmak için gereklidir ve isteğe bağlı olarak sağlanan, HDInsight ile oluşturulan çözüm şablonu diyagramı tıklayarak bu hizmetleri Azure portalında araştırabilirsiniz. ardından silinir).
[Diyagramın tam boyutlu bir sürümünü](https://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png)indirin.

Aşağıdaki bölümlerde, çözüm parçalar açıklanmaktadır.

## <a name="data-source-and-ingestion"></a>Veri kaynağı ve alma
### <a name="synthetic-data-source"></a>Yapay veri kaynağı
Bu şablon için, kullanılan veri kaynağı, başarılı dağıtımdan sonra yerel olarak çalıştırdığınız indirilen bir masaüstü uygulamasından oluşturulur.

Bu uygulamayı indirmek ve yüklemek için yönergeleri bulmak için ilk düğümünde, Tahmine dayalı bakım veri Oluşturucu, çözüm şablonu diyagramı seçin. Yönergeler, Özellikler araç çubuğunda bulunur. Bu uygulama, çözüm akışının geri kalanında kullanılan veri noktaları veya olaylar ile [Azure Event hub](#azure-event-hub) hizmetini besler. Bu veri kaynağı, [turbofan altyapı performansında azalma benzetim veri kümesi](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan)kullanılarak [NASA veri deposundaki](https://c3.nasa.gov/dashlink/resources/139/) genel kullanıma açık verilerden türetilir.

Yalnızca, bilgisayarınızda yürütülürken olay oluşturma uygulamasını Azure olay hub'ı doldurur.  

### <a name="azure-event-hub"></a>Azure Olay Hub'ı  
[Azure Olay Hub 'ı](https://azure.microsoft.com/services/event-hubs/) hizmeti, yapay veri kaynağı tarafından belirtilen girişin alıcıdır.

## <a name="data-preparation-and-analysis"></a>Veri hazırlama ve analiz  
### <a name="azure-stream-analytics"></a>Azure Stream Analytics
[Azure Olay Hub](#azure-event-hub) 'ı hizmetinden gelen giriş akışında neredeyse gerçek zamanlı analiz sağlamak için [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) kullanın. Daha sonra, [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) hizmeti tarafından işlenmek üzere tüm ham gelen olayları [Azure depolama](https://azure.microsoft.com/services/storage/) hizmeti 'ne arşivleyerek [Power BI](https://powerbi.microsoft.com) panosu üzerinde yayımlayın.

### <a name="hdinsight-custom-aggregation"></a>HDInsight özel toplama
Azure Stream Analytics kaynağı kullanılarak arşivlenen ham olaylara toplamalar sağlamak için HDInsight kullanarak [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betikleri (Azure Data Factory tarafından düzenlenmiş) çalıştırın.

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning hizmeti](https://azure.microsoft.com/services/machine-learning/) ile alınan girişleri kullanarak belirli bir uçak altyapısının kalan kullanım ömrü (rul) için tahminleri yapın (Azure Data Factory tarafından düzenlenmiş). 

## <a name="data-publishing"></a>Veri yayımlama
### <a name="azure-sql-database"></a>Azure SQL Veritabanı
[Power BI](https://powerbi.microsoft.com) panosunda tüketilen Azure Machine Learning tarafından alınan tahminleri depolamak IÇIN [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) 'nı kullanın.

## <a name="data-consumption"></a>Veri tüketimi
### <a name="power-bi"></a>Power BI
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)tarafından sunulan toplamalar ve uyarıların yanı sıra [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/)kullanılarak üretilmiş [Azure SQL veritabanında](https://azure.microsoft.com/services/sql-database/) depolanan rul öngörülerini içeren bir panoyu göstermek için [Power BI](https://powerbi.microsoft.com) kullanın.

## <a name="how-to-bring-in-your-own-data"></a>Nasıl kendi verilerinizi getirin
Bu bölümde, kendi verilerinizi Azure'a taşımalarına olanak açıklar ve bu mimariye Getir veriler için hangi alanlarda değişiklikler gerektirir.

Veri kümeniz, bu çözüm şablonu için kullanılan [turbofan motor düşme simülasyonu veri kümesi](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan) tarafından kullanılan veri kümesiyle eşleşiyor olabilir. Verilerinizi ve gereksinimleri anlama kendi verilerle çalışmak için bu şablonu nasıl değiştirileceği içinde çok önemlidir. 

Aşağıdaki bölümlerde, değişiklikler yeni bir veri kümesi eklendiğinde gerektiren şablonunun bölümlerine açıklanmaktadır.

### <a name="azure-event-hub"></a>Azure Olay Hub'ı
Azure olay hub'ı genel; Veri, CSV veya JSON biçiminde hub'ına gönderilebilir. Azure olay Hub'ında hiçbir özel işlem gerçekleşir, ancak içine beslenme veri anlamak önemlidir.

Bu belge, verilerinizi nasıl açıklamak değildir, ancak kolayca olayları ya da veri Azure olay Hub'ına olay hub'ı API'lerini kullanarak gönderebilirsiniz.

### <a name="azure-stream-analytics-1"></a>Azure Stream Analytics
Veri akışlarından okuyup verileri istediğiniz sayıda kaynağa aktararak neredeyse gerçek zamanlı analiz sağlamak için Azure Stream Analytics kaynağını kullanın.

Çözüm şablonu Havacılık için Tahmine dayalı bakım için Azure Stream Analytics sorgu dört alt sorgular her sorgu olayları çıkışlar dört farklı konumlara ile Azure olay hub'ı hizmetinden oluşur. Bu çıktı, üç Power BI veri kümeleri ve bir Azure depolama konumunda oluşur.

Azure Stream Analytics sorgu tarafından bulunabilir:

* Azure Portalı'na bağlanmak
* Çözüm dağıtıldığında oluşturulan Stream Analytics Icon](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-stream-analytics.png) ![Stream Analytics işleri bulma (*Örneğin*, tahmine dayalı bakım çözümü için **maintenancesa02asapbi** ve **maintenancesa02asablob** )
* Seçme
  
  * Sorgu girişini görüntülemek için ***girişler***
  * Sorgunun kendisini görüntülemek için ***sorgu***
  * Farklı çıkışları görüntülemek için ***çıkışlar***

Azure Stream Analytics sorgu oluşturma hakkındaki bilgiler, MSDN 'deki [Stream Analytics sorgu başvurusunda](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) bulunabilir.

Bu çözümde, bu çözüm şablonunun bir parçası olarak sunulan bir Power BI panosuna gelen veri akışını gerçek zamanlı analiz bilgilerini neredeyse üç kümeleriyle sorguları çıktı. Gelen veri biçimi hakkında örtük bilgiye olduğundan, bu sorgular, veri biçimi göre değiştirilmesi gerekir.

İkinci Stream Analytics iş **maintenancesa02asablob** sorgusu, tüm [Olay Hub](https://azure.microsoft.com/services/event-hubs/) olaylarını [Azure Storage](https://azure.microsoft.com/services/storage/) 'a çıkarır ve bu nedenle, tam olay bilgileri depoya akışa alınarak veri biçiminizden bağımsız olarak hiçbir değişiklik gerektirmez.

### <a name="azure-data-factory"></a>Azure Data Factory
[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) hizmeti verilerin hareketini ve işlenmesini düzenler. Aerospace için tahmine dayalı bakım çözüm şablonunda Veri Fabrikası, çeşitli teknolojiler kullanarak verileri taşımak ve işlemek için üç işlem [hattı](../../data-factory/concepts-pipelines-activities.md) oluşur.  Data factory'nizi Data Factory düğümü altındaki çözümün dağıtımı ile oluşturulan çözüm şablonu diyagramı açarak erişin. Veri oluşturucuyu başlatılmadan önce veri kümeleriniz altında hatalar nedeniyle veri fabrikası dağıtılır. Bu hata yoksayılabilir ve veri fabrikanıza çalışmasını engellemez

![Data Factory veri kümesi hataları](./media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Bu bölümde [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)bulunan gerekli işlem [hatları ve Etkinlikler](../../data-factory/concepts-pipelines-activities.md) açıklanmaktadır. Çözümün bir diyagram görünümü aşağıda verilmiştir.

![Azure Data Factory](./media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Bu fabrikaya ait işlem hatlarından ikisi, verileri bölümlemek ve toplamak için kullanılan [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betikleri içerir. Not edildiğinde, betikler kurulum sırasında oluşturulan [Azure Storage](https://azure.microsoft.com/services/storage/) hesabında bulunur. Bu konum şunlardır: maintenancesascript\\\\betik\\\\Hive\\\\ (veya https://[çözümünüz adınız]. blob. Core. Windows. net/maintenancesascript).

[Azure Stream Analytics](#azure-stream-analytics-1) sorgulara benzer şekilde, [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betiklerinin gelen veri biçimi hakkında örtülü bilgileri vardır ve veri biçiminizdeki olarak değiştirilmesi gerekir.

#### <a name="aggregateflightinfopipeline"></a>*Aggregateflightınfopipeline*
Bu [işlem hattı](../../data-factory/concepts-pipelines-activities.md) , [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) işi sırasında [Azure depolama](https://azure.microsoft.com/services/storage/) 'ya yerleştirilen verileri bölümlemek için [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betiği çalıştıran bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) kullanan tek bir Activity- [hdınsighthive](../../data-factory/transform-data-using-hadoop-hive.md) etkinliği içerir.

Bu bölümlendirme görevinin [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betiği, ***Aggregateflightınfo. HQL*** ' dir

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*
Bu işlem [hattı](../../data-factory/concepts-pipelines-activities.md) , son sonucu bu çözüm şablonuyla ilişkili [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) denemenizin puanlanması olan çeşitli etkinlikleri içerir.

Etkinlikler dahil şunlardır:

* [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) denemesi için gereken toplamaları ve özellik mühendisliğini gerçekleştirmek üzere [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betiği çalıştıran bir [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) kullanarak etkinlik [hdınsighthive](../../data-factory/transform-data-using-hadoop-hive.md) .
  Bu bölümlendirme görevinin [Hive](https://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) betiği, ***hazırlık emlinput. HQL***' dir.
* [Hdınsighthive](../../data-factory/transform-data-using-hadoop-hive.md) etkinlikten sonuçları [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) etkinliğinin eriştiği tek bir [Azure Storage](https://azure.microsoft.com/services/storage/) blobuna taşınan [kopyalama](https://msdn.microsoft.com/library/azure/dn835035.aspx) etkinliği.
* [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) etkinliği, sonuçları tek bir [Azure Storage](https://azure.microsoft.com/services/storage/) blobuna yerleştirerek [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) denemeyi çağırır.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*
Bu işlem [hattı](../../data-factory/concepts-pipelines-activities.md) , ***MLScoringPipeline*** [Azure Machine Learning](#azure-machine-learning) denemesinin sonuçlarını çözüm şablonu YÜKLEMESININ bir parçası olarak sağlanan [Azure SQL veritabanına](https://azure.microsoft.com/services/sql-database/) kopyalayan tek bir etkinlik ( [kopyalama](https://msdn.microsoft.com/library/azure/dn835035.aspx) etkinliği) içerir.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Bu çözüm şablonu için kullanılan [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) deneme, uçak altyapısının kalan yararlı ömrünü (rul) sağlar. Denemeyi tüketilen veri kümesi için özeldir ve değişikliği yapılması gerektiğinden veya değiştirilen verileri getirildi.

Azure Machine Learning denemenizin nasıl oluşturulduğu hakkında bilgi için bkz. tahmine [dayalı bakım: adım 1/3, veri hazırlama ve özellik Mühendisliği](https://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>İlerlemeyi İzle
Veri oluşturucuyu başlatıldıktan sonra işlem hattı dehydrate başlar ve eylem aşağıdaki komutlar veri fabrikası tarafından verilen içine başlatılmadan çözümünüzü farklı bileşenlerini başlatın. İşlem hattını izlemek için iki yolu vardır.

* Bir Stream Analytics işlerinin ham gelen verileri blob depolamaya yazar. Çözümü başarıyla dağıttığınızdan çözümünüzün BLOB depolama bileşeni ' ne tıklayın ve ardından sağ bölmede aç ' a tıkladığınızda, sizi [Azure Portal](https://portal.azure.com/)götürür. Bir kez Bloblarda, tıklayın. Sonraki panelinde, bir kapsayıcı listesi görürsünüz. **Maintenancesadata**' ye tıklayın. Sonraki panelde **rawData** klasörüdür. Rawdata klasörü klasörlerdir saat gibi adlara sahip 17 ve saat = = 18. Ham veriler bu klasörleri varlığını gösterir, bilgisayarınızda oluşturulur ve blob depolama alanında depolanır. Csv dosyaları bu klasörlerdeki MB sınırlı boyutlarıyla görmeniz gerekir.
* İşlem hattının son adım, SQL veritabanı'na veri (örneğin tahminler makine öğreniminin) yazmaktır. En fazla üç saat verilerin SQL veritabanı'nda görünmesi için beklemeniz gerekebilir. SQL veritabanınızda kullanılabilir veri miktarını izlemenin bir yolu [Azure Portal](https://portal.azure.com/). Sol bölmede SQL VERITABANı ![SQL simge](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-SQL-databases.png) bulun ve tıklayın. Daha sonra veritabanınızın **pmaintenancedb** bulun ve üzerine tıklayın. Altındaki sonraki sayfada, Yönet'e tıklayın.
   
    ![Yönet simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-manage.png)
   
    Burada, yeni sorgu ve sorgu (örneğin seçin count(*) PMResult gelen) satır sayısı için tıklayabilirsiniz. Veritabanınızı büyüdükçe, tablodaki satır sayısını artırmalısınız.

## <a name="power-bi-dashboard"></a>Power BI Panosu

Azure Stream Analytics verilerinizi (etkin yol) ve Azure machine learning (Durgun yol) batch tahmin sonuçlarını görselleştirmek için bir Power BI Pano ayarlayın.

### <a name="set-up-the-cold-path-dashboard"></a>Durgun yoldaki Pano Ayarla
Durgun yoldaki veri işlem hattı, hedefin bir uçuştaki (döngüsü) tamamlandıktan sonra (kalan faydalı ömrü) Tahmine dayalı RUL her uçak motorunun almaktır. Tahmin sonuç 3 bir uçuş sırasında son 3 saat tamamladınız uçak motorları tahmin etmeye yönelik saatte bir güncelleştirilir.

Power BI, tahmin sonuçlarının depolandığı, veri kaynağı olarak bir Azure SQL veritabanına bağlanır. 

Not: 
1.    Çözümünüzü dağıtmaya yönelik olarak, veritabanında 3 saat içinde bir tahmin görüntülenecektir. Power BI Panosu hemen oluşturabilir, böylece bazı temel veri Oluşturucu yüklemeyle birlikte gelen pbıx dosyasını içerir. 
2.    Bu adımda önkoşul, ücretsiz yazılım [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)'ı indirip yüklemektir.

Aşağıdaki adımları pbıx dosyasını (örneğin, tahmin sonuçlarını) verileri görselleştirme içeren bir çözüm dağıtımı sırasındaki küme çalışmaya başladıktan SQL veritabanına bağlanma konusunda rehberlik.

1. Veritabanı kimlik bilgileri alın.
   
   Sonraki adımlara geçmeden önce **veritabanı sunucu adı, veritabanı adı, Kullanıcı adı ve parola** gerekir. Bunları nasıl bulacağınıza ilişkin kılavuz için adımlar aşağıda verilmiştir.
   
   * Çözüm şablonu diyagramınızdaki **' Azure SQL veritabanı '** yeşil ' i etkinleştirdikten sonra, tıklayın ve sonra **' Aç '** düğmesine tıklayın.
   * Azure portal sayfasını görüntüleyen yeni bir tarayıcı sekmesi/penceresi görürsünüz. Sol panelde **' kaynak grupları '** seçeneğine tıklayın.
   * Çözümü dağıtmak için kullanmakta olduğunuz aboneliği seçin ve ardından **' YourSolutionName\_ResourceGroup '** öğesini seçin.
   * Yeni açılan panelde, veritabanınıza erişmek için ![SQL simgesi](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-sql.png) simgesine tıklayın. Veritabanınızın adı bu simgenin yanında (örneğin, **' pmaintenancedb '** ) ve **veritabanı sunucu adı** sunucu adı özelliği altında listelenir ve **YourSolutionName.Database.Windows.net**şuna benzer görünmelidir.
   * Veritabanınızın **Kullanıcı adı** ve **parolası** , çözümün dağıtımı sırasında daha önce kaydedilen Kullanıcı adı ve parola ile aynıdır.
2. Durgun yoldaki rapor dosyası veri kaynağı, Power BI Desktop ile güncelleştirin.
   
   * Oluşturucu dosyasını indirdiğiniz ve sıkıştırmışın klasörde, **PowerBI\\PredictiveMaintenanceAerospace. pbix** dosyasına çift tıklayın. Dosyayı açtığınızda herhangi bir uyarı iletisi görürseniz, bunları yoksayar. Dosyanın üst kısmında **' sorguları Düzenle '** seçeneğine tıklayın.
     
     ![Sorguları Düzenle](./media/cortana-analytics-technical-guide-predictive-maintenance/edit-queries.png)
   * **RemainingUsefulLife** ve **pmresult**olmak üzere iki tablo görürsünüz. İlk tabloyu seçin ve sağ **' sorgu ayarları '** panelinde **' uygulanan adımlar '** altında bulunan ' **kaynak '** seçeneğinin yanındaki ![sorgu ayarları simgesine](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-query-settings.png) tıklayın. Görünen uyarı iletileri yoksayın.
   * Açılan pencerede, **' sunucu '** ve **' veritabanı '** ' nı kendi sunucu ve veritabanı adlarınızla değiştirin ve ardından **' Tamam 'a**tıklayın. Sunucu adı için 1433 numaralı bağlantı noktasını (**YourSolutionName.Database.Windows.net, 1433**) belirttiğinizden emin olun. Veritabanı alanını **pmaintenancedb**olarak bırakın. Ekranda görüntülenen uyarı iletilerini yoksayın.
   * Sonraki açılan pencerede, sol bölmede (**Windows** ve **veritabanı**) iki seçenek görürsünüz. ' **Veritabanı '** seçeneğine tıklayın, **' Kullanıcı adı '** ve **' parola '** (çözümü ilk dağıtırken girdiğiniz Kullanıcı adı ve parola) ile bir Azure SQL veritabanı oluşturduğunuzda girin. ***Bu ayarların hangi düzeye uygulanacağını seçin***bölümünde, veritabanı düzeyi seçeneğini işaretleyin. Ardından **' Bağlan**'a tıklayın.
   * İkinci **tabloya tıklayın** . daha sonra sağ **' sorgu ayarları '** panelinde **' uygulanan adımlar '** altında bulunan ' **kaynak '** öğesinin yanındaki ![gezinti simgesine](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-navigation.png) tıklayın ve sunucu ve veritabanı adlarını yukarıdaki adımlarda olduğu gibi güncelleştirin ve Tamam ' a tıklayın.
   * Önceki sayfaya geri destekli sonra penceresini kapatın. Bir ileti görüntülenir ve **Uygula**' ya tıklayın. Son olarak, değişiklikleri kaydetmek için **Kaydet** düğmesine tıklayın. Power BI dosyanızı, şimdi sunucuyla bağlantı kurmuştur. Görselleştirmelerinizi boş ise göstergeleri sağ üst köşesinde Silgi simgesini tıklatarak tüm verileri görselleştirmek için görselleştirmelere seçimleri Temizle emin olun. Bir görselleştirmeyi yeni verileri yansıtması için Yenile düğmesini kullanın. Data factory 3 saatte bir yenileme zamanlandığında başlangıçta yalnızca çekirdek veri görselleştirmelerinizi üzerinde görürsünüz. Verileri yenilediğinizde görselleştirmeleriniz yansıtılan yeni Öngörüler 3 saat sonra görürsünüz.
3. Seçim [Power BI çevrimiçi](https://www.powerbi.com/)olarak soğuk yol panosunu yayımlayın. Bu adım bir Power BI hesabına (veya Office 365 hesabına) ihtiyaç duyuyor.
   
   * Daha sonra "Power BI başarılı yayımlama!" ile görüntülenen bir pencere görüntülenirken " **Yayımla** " ve birkaç saniye ' ne tıklayın. Yeşil bir onay işareti ile. Aşağıdaki "Açık PredictiveMaintenanceAerospace.pbix Power BI'da" bağlantısına tıklayın. Ayrıntılı yönergeleri bulmak için bkz. [Power BI Desktop yayımlama](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).
   * Yeni bir pano oluşturmak için: sol bölmedeki **panolar** bölümünün yanındaki **+** işaretine tıklayın. Bu yeni bir Pano için "Tahmine dayalı bakım tanıtımını" adını girin.
   * Raporu açtığınızda, tüm görselleştirmeleri panonuza sabitlemek için ![SABITLEME simgesine](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-pin.png) tıklayın. Ayrıntılı yönergeleri bulmak için bkz. bir [panodaki kutucuğu bir Power BI panoya sabitleme](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
     Pano sayfasına gidin, görselleştirmelerinizi konumunu ve boyutunu ayarlayın ve ünvanları düzenleyin. Kutucuklarınızın nasıl düzenleneceği hakkında ayrıntılı yönergeler için bkz. [kutucuğu düzenleme--yeniden boyutlandırma, taşıma, yeniden adlandırma, sabitleme, silme, köprü ekleme](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Burada, sabitlenmiş öğelere bazı soğuk yolunu görselleştirmeler içeren bir örnek Pano verilmiştir.  Veri oluşturucuyu ne kadar süreyle çalıştırdığınız bağlı olarak numaralarınızı görselleştirmelere farklı olabilir.
     <br/>
     ![son görünüm](./media/cortana-analytics-technical-guide-predictive-maintenance/final-view.png)
     <br/>
   * Verilerin yenilenmesini zamanlamak için, farenizi **PredictiveMaintenanceAerospace** veri kümesinin üzerine getirin, ![üç nokta simgesine](./media/cortana-analytics-technical-guide-predictive-maintenance/icon-elipsis.png) ve sonra **yenilemeyi zamanla**' yı seçin.
     <br/>
     **Note:** Bir uyarı açıklaması görürseniz, **kimlik bilgilerini düzenle** ' ye tıklayın ve veritabanı kimlik bilgilerinizin adım 1 ' de açıklananlarla aynı olduğundan emin olun.
     <br/>
     ![yenilemeyi zamanla](./media/cortana-analytics-technical-guide-predictive-maintenance/schedule-refresh.png)
     <br/>
   * **Yenilemeyi zamanla** bölümünü genişletin. "Verilerinizi güncel tutun" etkinleştirin.
     <br/>
   * Gereksinimlerinize göre yenileme zamanlayabilirsiniz. Daha fazla bilgi edinmek için bkz. [Power BI veri yenileme](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Kurulum etkin yolu Panosu
Aşağıdaki adımlar, Çözüm dağıtımı sırasında oluşturulan Stream Analytics işlerini veri çıkışı görselleştirmek nasıl size rehberlik eder. Aşağıdaki adımları gerçekleştirmek için [Power BI çevrimiçi](https://www.powerbi.com/) bir hesap gereklidir. Hesabınız yoksa, [bir hesap oluşturabilirsiniz](https://powerbi.microsoft.com/pricing).

1. Power BI çıkışına Azure Stream Analytics (ASA) ekleyin.
   
   * Azure Stream Analytics işinizin çıkışını Power BI panonuz olarak ayarlamak için [Azure Stream Analytics & Power BI: veri akışı verilerinin gerçek zamanlı görünürlüğü için analiz panosu](../../stream-analytics/stream-analytics-power-bi-dashboard.md) ' nda bulunan yönergeleri izlemeniz gerekir.
   * ASA sorgusunda **aircraftmonitor**, **aircraftalert**ve **flightsbyhour**'un üç çıkışı vardır. Sorgu sekmesine tıklayarak sorguyu görüntüleyebilirsiniz. bu tabloların her birine karşılık gelen, ASA 'ye bir çıktı eklemeniz gerekir. İlk çıktıyı eklediğinizde (**aircraftmonitor**) **Çıkış diğer**adının, **veri kümesi adının** ve **tablo adının** aynı (**aircraftmonitor**) olduğundan emin olun. **Uçak uyarısı**ve **flightsbyhour**için çıkış eklemek için adımları tekrarlayın. Üç tablo çıktı ve ASA işi başlatıldı ekledikten sonra bir onay iletisi ("başarılı başlangıç Stream Analytics işi maintenancesa02asapbi") almanız gerekir.
2. [Power BI çevrimiçi](https://www.powerbi.com) olarak oturum açın
   
   * Çalışma alanım 'daki sol panel veri kümeleri bölümünde **aircraftmonitor**, **aircraftalert**ve **Flightsbyhour** ***veri kümesi*** adları görüntülenmelidir. Azure Stream Analytics'ten önceki adımda gönderilen veri akışı budur. Veri kümesi **flightsbyhour** , arkasındaki SQL sorgusunun doğası nedeniyle diğer iki veri kümesiyle aynı anda görüntülenmeyebilir. Ancak, bir saat sonra gösterilmesi gerekir.
   * ***Görsel öğeler*** bölmesinin açık olduğundan ve ekranın sağ tarafında gösterildiğinden emin olun.
3. Veri akışını Power BI'da oturum açtıktan sonra akış verilerini görselleştirme başlatabilirsiniz. Bazı sık erişimli yolunu görselleştirmeler içeren bir örnek Pano altındaki kendisine sabitlenir. Uygun veri kümeleri üzerinde bağlı diğer Pano kutucukları oluşturabilirsiniz. Veri oluşturucuyu ne kadar süreyle çalıştırdığınız bağlı olarak numaralarınızı görselleştirmelere farklı olabilir.

    ![Pano görünümü](media/cortana-analytics-technical-guide-predictive-maintenance/dashboard-view.png)

1. Yukarıdaki kutucuktan birini oluşturmak için bazı adımlar vardır: "algılayıcı 11 ' in Fleet görünümü-eşik 48,26" kutucuğu:
   
   * Sol panel veri kümeleri bölümünde veri kümesi **aircraftmonitor** öğesine tıklayın.
   * **Çizgi grafiği** simgesine tıklayın.
   * **Görsel öğeler** bölmesinde "eksen" altında görünmesi için **alanlar** bölmesinde **işlenen** ' e tıklayın.
   * "S11" ve "S11\_Alert" öğesine tıklayarak her ikisi de "Values" altında görünmesini sağlayın. **S11** ve **S11\_uyarısı**' nın yanındaki küçük oka tıklayın, "Sum" ı "Average" olarak değiştirin.
   * Üstteki **Kaydet** ' e tıklayın ve raporu "aircraftmonitor" olarak adlandırın. "Aircraftmonitor" adlı rapor, sol taraftaki **Gezgin** bölmesinde bulunan **raporlar** bölümünde gösterilir.
   * Bu çizgi grafiğin sağ üst köşesinde bulunan **görsel sabitle** simgesine tıklayın. Bir "Panoya Sabitle" penceresi sizin için bir Pano seçmek görünebilir. "Tahmine dayalı bakım tanıtımını" seçin, ardından "Sabitle" seçeneğini tıklatın
   * Fare işaretçisini panoda bu kutucuğun üzerine getirin, başlığını "en sağ üst köşedeki" Düzenle "simgesine tıklayarak başlığını" bir süre içinde ortalama olarak "ESET, algılayıcı 11 ile 48,26 arasında"

## <a name="delete-your-solution"></a>Çözümünüzü Sil
Veri oluşturucuyu çalışan, daha yüksek maliyetlere neden şekilde aktif çözümü kullanarak veri oluşturucuyu durdurduğunuzdan emin olun. Bunu kullanmıyorsanız çözümü silin. Çözümünüzü silme çözümü dağıttığınızda, aboneliğinizde sağlanan tüm bileşenleri siler. Çözümü silmek için, çözüm şablonunun sol panelinde çözümünüz adına tıklayın ve ardından **Sil**' e tıklayın.

## <a name="cost-estimation-tools"></a>Maliyet tahmini araçları
Tahmine dayalı bakım için Havacılık çözüm şablonu aboneliğinizde çalıştırılan ilgili toplam maliyetleri daha iyi anlamanıza yardımcı olması aşağıdaki iki araçlar mevcuttur:

* [Microsoft Azure Cost Estimator Aracı (çevrimiçi)](https://azure.microsoft.com/pricing/calculator/)
* [Microsoft Azure Cost Estimator Aracı (Masaüstü)](https://www.microsoft.com/download/details.aspx?id=43376)

