---
title: Azure Cosmos DB, Azure Analiz Hizmetleri ve Power BI kullanarak gerçek zamanlı bir pano oluşturun
description: Azure Cosmos DB ve Azure Analiz Hizmetlerini kullanarak Power BI'de canlı bir hava durumu panosu oluşturmayı öğrenin.
author: bharathsreenivas
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: bharathb
ms.reviewer: sngun
ms.openlocfilehash: d225a14edddcad58c08094dbc758d67df8f834e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70376599"
---
# <a name="create-a-real-time-dashboard-using-azure-cosmos-db-and-power-bi"></a>Azure Cosmos DB ve Power BI kullanarak gerçek zamanlı bir pano oluşturun

Bu makalede, Azure Cosmos DB ve Azure Analiz Hizmetleri kullanarak Power BI'de canlı bir hava durumu panosu oluşturmak için gereken adımlar açıklanmaktadır. Power BI panosu, bir bölgedeki sıcaklık ve yağış miktarı hakkında gerçek zamanlı bilgileri göstermek için grafikler görüntüler.

## <a name="reporting-scenarios"></a>Raporlama senaryoları

Azure Cosmos DB'de depolanan verilerde raporlama panoları ayarlamanın birden çok yolu vardır. Bayatlık gereksinimlerine ve verilerin boyutuna bağlı olarak, aşağıdaki tablo her senaryo için raporlama kurulumlarını açıklar:


|Senaryo |Kurulum |
|---------|---------|
|1. Özel raporlar oluşturma (yenileme yok)    |  [Alma moduna sahip POWER BI Azure Cosmos DB konektörü](powerbi-visualize.md)       |
|2. Periyodik yenileme ile geçici raporlar oluşturma   |  [Alma moduna sahip POWER BI Azure Cosmos DB konektörü (Zamanlanmış periyodik yenileme)](powerbi-visualize.md)       |
|3. Büyük veri kümelerinde raporlama (10 GB <)     |  Artımlı yenileme ile güç BI Azure Cosmos DB konektörü       |
|4. Büyük veri kümelerinde gerçek zamanlı raporlama    |  Doğrudan sorgu + Azure Analiz Hizmetleri (Azure Cosmos DB konektörü) ile Power BI Azure Analiz Hizmetleri bağlayıcısı       |
|5. Toplu verilerle canlı veri raporlama     |  [Doğrudan sorgu + Azure Databricks + Cosmos DB Spark konektörlü BI Spark konektörü ne şrıt.](https://github.com/Azure/azure-cosmosdb-spark/wiki/Connecting-Cosmos-DB-with-PowerBI-using-spark-and-databricks-premium)       |
|6. Büyük veri kümelerinde toplamlarla canlı veriler üzerinde raporlama   |  Doğrudan sorgu + Azure Analiz Hizmetleri + Azure Databricks + Cosmos DB Spark konektörlü Power BI Azure Analiz Hizmetleri bağlayıcısı.       |

Senaryo 1 ve 2, Azure Cosmos DB Power BI konektörü kullanılarak kolayca ayarlanabilir. Bu makalede, senaryo3 ve 4 için kurulumları açıklanmaktadır.

### <a name="power-bi-with-incremental-refresh"></a>Artımlı yenileme ile Güç BI

Power BI, artımlı yenilemenin yapılandırılabildiği bir modu vardır. Bu mod, Azure Analiz Hizmetleri bölümleri oluşturma ve yönetme gereksinimini ortadan kaldırır. Artımlı yenileme, yalnızca büyük veri kümelerinde yalnızca en son güncelleştirmeleri filtrelemek için ayarlanabilir. Ancak, bu mod yalnızca 10 GB veri kümesi sınırlaması olan Power BI Premium hizmetiyle çalışır.

### <a name="power-bi-azure-analysis-connector--azure-analysis-services"></a>Power BI Azure Analiz bağlayıcısı + Azure Analiz Hizmetleri

Azure Analiz Hizmetleri, bulutta kurumsal sınıf veri modellerini barındıran bir hizmet olarak tam olarak yönetilen bir platform sağlar. Azure Cosmos DB'den Azure Analiz Hizmetleri'ne büyük veri setleri yüklenebilir. Tüm veri kümesini sürekli sorgulamaktan kaçınmak için, veri kümeleri farklı frekanslarda bağımsız olarak yenilenebilecek Azure Çözümhizmetleri bölümlerine ayrılabilir.

## <a name="power-bi-incremental-refresh"></a>Güç BI artımlı yenileme

### <a name="ingest-weather-data-into-azure-cosmos-db"></a>Hava durumu verilerini Azure Cosmos DB'ye alma

[Hava durumu verilerini](https://catalog.data.gov/dataset/local-weather-archive) Azure Cosmos DB'ye yüklemek için bir yutma kanalı ayarlayın. HTTP Kaynak ve Cosmos DB lavabosu kullanarak en son hava durumu verilerini Azure Cosmos DB'ye düzenli olarak yüklemek için bir [Azure Veri Fabrikası (ADF)](../data-factory/connector-azure-cosmos-db.md) işi ayarlayabilirsiniz.


### <a name="connect-power-bi-to-azure-cosmos-db"></a>Power BI'yi Azure Cosmos DB'ye bağlayın

1. **Azure Cosmos hesabını Power BI** ' ye bağlayın - Power BI Desktop' ı açın ve doğru veritabanını ve kapsayıcıyı seçmek için Azure Cosmos DB konektörünü kullanın.

   ![Azure Cosmos DB Power BI bağlayıcısı](./media/create-real-time-weather-dashboard-powerbi/cosmosdb-powerbi-connector.png)

1. **Artımlı yenilemeyi yapılandırın** - Veri kümesi için artımlı yenilemeyi yapılandırmak için Power BI makalesi [ile artımlı yenileme](/power-bi/service-premium-incremental-refresh) adımlarını izleyin. **RangeStart** ve **RangeEnd** parametrelerini aşağıdaki ekran görüntüsünde gösterildiği gibi ekleyin:

   ![Aralık parametrelerini yapılandırma](./media/create-real-time-weather-dashboard-powerbi/configure-range-parameters.png)

   Veri kümesinde metin biçiminde bir Tarih sütunu olduğundan, **RangeStart** ve **RangeEnd** parametreleri aşağıdaki filtreyi kullanacak şekilde dönüştürülmelidir. Advanced **Editor** bölmesinde, RangeStart ve RangeEnd parametrelerini temel alan satırları filtrelemek için sorgunuzu aşağıdaki metni ekleyin:

   ```
   #"Filtered Rows" = Table.SelectRows(#"Expanded Document", each [Document.date] > DateTime.ToText(RangeStart,"yyyy-MM-dd") and [Document.date] < DateTime.ToText(RangeEnd,"yyyy-MM-dd"))
   ```
   
   Kaynak veri kümesinde hangi sütun ve veri türünün bulunduğuna bağlı olarak, RangeStart ve RangeEnd alanlarını buna göre değiştirebilirsiniz

   
   |Özellik  |Veri türü  |Filtre  |
   |---------|---------|---------|
   |_ts     |   Sayısal      |  [_ts] > Duration.TotalSeconds(RangeStart - #datetime(1970, 1, 1, 0, 0,0)) ve [_ts] < Süre.TotalSaniye(RangeEnd - #datetime(1970, 1, 1, 0, 0))       |
   |Tarih (örneğin:- 2019-08-19)     |   Dize      | [Document.date]> DateTime.ToText(RangeStart,"yyyy-MM-dd") ve [Document.date] < DateTime.ToText(RangeEnd,"yyyy-MM-dd")        |
   |Tarih (örneğin:- 2019-08-11 12:00:00)   |  Dize       |  [Document.date]> DateTime.ToText(RangeStart," yyyy-mm-dd HH:mm:ss" ve [Document.date] < DateTime.ToText(RangeEnd,"yyyy-mm-dd HH:mm:ss")       |


1. **Yenileme ilkesini tanımlayın** - Tablonun **bağlam** menüsündeki **Artımlı yenileme** sekmesine yön vererek yenileme ilkesini tanımlayın. Yenileme ilkesini **her gün** yenilemek ve son ay verilerini depolamak için ayarlayın.

   ![Yenileme ilkesini tanımlama](./media/create-real-time-weather-dashboard-powerbi/define-refresh-policy.png)

   *M sorgusunun katlanır olarak onaylanamayacağını*belirten uyarıyı yoksayın. Azure Cosmos DB bağlayıcısı sorguları filtreler.

1. **Verileri yükleyin ve raporları oluşturun** - Daha önce yüklediğiniz verileri kullanarak, sıcaklık ve yağış hakkında rapor vermek için grafikler oluşturun.

   ![Verileri yükleyin ve rapor oluşturun](./media/create-real-time-weather-dashboard-powerbi/load-data-generate-report.png)

1. **Raporu Power BI premium'da yayımlayın** - Artımlı yenileme yalnızca Premium özelliği olduğundan, yayımlama iletişim kutusu yalnızca Premium kapasitesi yle ilgili bir çalışma alanı nın seçilmesine izin verir. İlk yenilemede geçmiş verilerin içeri aktarılması uzun sürebilir. Sonraki veri yenilemeleri, artımlı yenileme kullandıklarından çok daha hızlıdır.


## <a name="power-bi-azure-analysis-connector--azure-analysis-services"></a>Power BI Azure Analiz bağlayıcısı + Azure Analiz Hizmetleri 

### <a name="ingest-weather-data-into-azure-cosmos-db"></a>Hava durumu verilerini Azure Cosmos DB'ye alma 

[Hava durumu verilerini](https://catalog.data.gov/dataset/local-weather-archive) Azure Cosmos DB'ye yüklemek için bir yutma kanalı ayarlayın. HTTP Kaynak ve Cosmos DB Lavabosu'nu kullanarak en son hava durumu verilerini Azure Cosmos DB'ye düzenli olarak yüklemek için bir Azure Veri Fabrikası (ADF) işi ayarlayabilirsiniz.

### <a name="connect-azure-analysis-services-to-azure-cosmos-account"></a>Azure Analiz Hizmetlerini Azure Cosmos hesabına bağlayın

1. **Yeni bir Azure Analiz Hizmetleri kümesi** - Oluşturun Azure Cosmos hesabı ve Databricks kümesiyle aynı bölgede[Azure Analizi hizmetlerinin bir örneğini oluşturun.](../analysis-services/analysis-services-create-server.md)

1. **Visual Studio'da** -  yeni bir Analiz Hizmetleri Tabular Projesi oluşturun[SQL Server Veri Araçlarını (SSDT) yükleyin](/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017) ve Visual Studio'da bir Analiz Hizmetleri Tabular projesi oluşturun.

   ![Azure Analiz Hizmetleri projesi oluşturun](./media/create-real-time-weather-dashboard-powerbi/create-analysis-services-project.png)

   **Tümleşik Çalışma Alanı** örneğini seçin ve **SQL Server 2017 / Azure Analiz Hizmetleri Uyumluluk Düzeyi (1400)** kümesini seçin

   ![Azure Analiz Hizmetleri tabular model tasarımcısı](./media/create-real-time-weather-dashboard-powerbi/tabular-model-designer.png)

1. **Azure Cosmos DB veri kaynağını ekleyin** - **Modellere**> Gidin**Veri Kaynakları** > **Yeni Veri Kaynağı'na** gidin ve aşağıdaki ekran görüntüsünde gösterildiği gibi Azure Cosmos DB veri kaynağını ekleyin:

   ![Cosmos DB veri kaynağı ekle](./media/create-real-time-weather-dashboard-powerbi/add-data-source.png)

   **HESAP URI,** **veritabanı adı**ve kapsayıcı **adını**sağlayarak Azure Cosmos DB'ye bağlanın. Azure Cosmos kapsayıcısından gelen verilerin Power BI'ye aktarılabileceğini artık görebilirsiniz.

   ![Azure Cosmos DB verilerini önizleme](./media/create-real-time-weather-dashboard-powerbi/preview-cosmosdb-data.png)

1. **Analiz Hizmetleri modelini oluşturma** - Sorgu düzenleyicisini açın, yüklenen veri kümesini en iyi duruma getirmek için gerekli işlemleri gerçekleştirin:

   * Yalnızca hava durumuyla ilgili sütunları ayıklayın (sıcaklık ve yağış)

   * Ay bilgilerini tablodan ayıklayın. Bu veriler, sonraki bölümde açıklandığı gibi bölümler oluştururken yararlıdır.

   * Sıcaklık sütunlarını sayıya dönüştürme

   Ortaya çıkan M ifadesi aşağıdaki gibidir:

   ```
    let
        Source=#"DocumentDB/https://[ACCOUNTNAME].documents.azure.com:443/",
        #"Expanded Document" = Table.ExpandRecordColumn(Source, "Document", {"id", "_rid", "_self", "_etag", "fogground", "snowfall", "dust", "snowdepth", "mist", "drizzle", "hail", "fastest2minwindspeed", "thunder", "glaze", "snow", "ice", "fog", "temperaturemin", "fastest5secwindspeed", "freezingfog", "temperaturemax", "blowingsnow", "freezingrain", "rain", "highwind", "date", "precipitation", "fogheavy", "smokehaze", "avgwindspeed", "fastest2minwinddir", "fastest5secwinddir", "_attachments", "_ts"}, {"Document.id", "Document._rid", "Document._self", "Document._etag", "Document.fogground", "Document.snowfall", "Document.dust", "Document.snowdepth", "Document.mist", "Document.drizzle", "Document.hail", "Document.fastest2minwindspeed", "Document.thunder", "Document.glaze", "Document.snow", "Document.ice", "Document.fog", "Document.temperaturemin", "Document.fastest5secwindspeed", "Document.freezingfog", "Document.temperaturemax", "Document.blowingsnow", "Document.freezingrain", "Document.rain", "Document.highwind", "Document.date", "Document.precipitation", "Document.fogheavy", "Document.smokehaze", "Document.avgwindspeed", "Document.fastest2minwinddir", "Document.fastest5secwinddir", "Document._attachments", "Document._ts"}),
        #"Select Columns" = Table.SelectColumns(#"Expanded Document",{"Document.temperaturemin", "Document.temperaturemax", "Document.rain", "Document.date"}),
        #"Duplicated Column" = Table.DuplicateColumn(#"Select Columns", "Document.date", "Document.month"),
        #"Extracted First Characters" = Table.TransformColumns(#"Duplicated Column", {{"Document.month", each Text.Start(_, 7), type text}}),
        #"Sorted Rows" = Table.Sort(#"Extracted First Characters",{{"Document.date", Order.Ascending}}),
        #"Changed Type" = Table.TransformColumnTypes(#"Sorted Rows",{{"Document.temperaturemin", type number}, {"Document.temperaturemax", type number}}),
        #"Filtered Rows" = Table.SelectRows(#"Changed Type", each [Document.month] = "2019-07")
    in
        #"Filtered Rows"
   ```

   Ayrıca, bu değerlerin Power BI'de çizilediğinden emin olmak için sıcaklık sütunlarının veri türünü Ondalık olarak değiştirin.

1. **Azure Analizi bölümleri oluşturma** - Veri kümesini bağımsız olarak ve farklı frekanslarda yenilenebilen mantıksal bölümlere bölmek için Azure Çözümleme Hizmetleri'nde bölümler oluşturun. Bu örnekte, veri kümesini en son ayın verilerine ve diğer her şeye bölecek iki bölüm oluşturursunuz.

   ![Analiz hizmetleri bölümleri oluşturma](./media/create-real-time-weather-dashboard-powerbi/create-analysis-services-partitions.png)

   Azure Çözümleme Hizmetleri'nde aşağıdaki iki bölümü oluşturun:

   * **Son Ay** - `#"Filtered Rows" = Table.SelectRows(#"Sorted Rows", each [Document.month] = "2019-07")`
   * **Tarihi** -  `#"Filtered Rows" = Table.SelectRows(#"Sorted Rows", each [Document.month] <> "2019-07")`

1. **Modeli Azure Analiz Sunucusuna dağıtın** - Azure Analiz Hizmetleri projesine sağ tıklayın ve **Dağıt'ı**seçin. **Deployment Server özellikleri** bölmesine sunucu adını ekleyin.

   ![Azure Analiz Hizmetleri modelini dağıtma](./media/create-real-time-weather-dashboard-powerbi/analysis-services-deploy-model.png)

1. **Yapılandırma bölümü yeniler ve birleştirmeler** - Azure Analiz Hizmetleri bölümlerin bağımsız olarak işlenmesine olanak tanır. **Son Ay** bölümünün en son verilerle sürekli olarak güncellenmesini istediğimizden, yenileme aralığını 5 dakikaya ayarlayın. Verileri geçmiş bölmede yenilemek gerekmez. Ayrıca, son ay bölümü geçmiş bölüme birleştirmek ve yeni bir son ay bölümü oluşturmak için bazı kod yazmanız gerekir.


## <a name="connect-power-bi-to-analysis-services"></a>Power BI'yi Analiz Hizmetlerine Bağlayın

1. **Azure Analiz Hizmetleri veritabanı Bağlayıcısı'nı kullanarak Azure Analiz Sunucusuna bağlanın** - **Canlı modunu** seçin ve aşağıdaki ekran görüntüsünde gösterildiği gibi Azure Analiz Hizmetleri örneğine bağlanın:

   ![Azure Analiz Hizmetleri'nden veri alma](./media/create-real-time-weather-dashboard-powerbi/analysis-services-get-data.png)

1. **Verileri yükleyin ve raporlar oluşturun** - Daha önce yüklediğiniz verileri kullanarak sıcaklık ve yağış hakkında rapor vermek için grafikler oluşturun. Canlı bağlantı oluşturduğunuzdan, sorguların önceki adımda dağıttığınız Azure Çözümleme Hizmetleri modelindeki verilerüzerinde yürütülmesi gerekir. Sıcaklık grafikleri, yeni veriler Azure Cosmos DB'ye yüklendikten sonra beş dakika içinde güncelleştirilir.

   ![Verileri yükleyin ve raporlar oluşturun](./media/create-real-time-weather-dashboard-powerbi/load-data-generate-report.png)

## <a name="next-steps"></a>Sonraki adımlar

* Power BI hakkında daha fazla bilgi edinmek için Power [BI ile başlayın.](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/)

* [Qlik Sense'i Azure Cosmos DB'ye bağlayın ve verilerinizi görselleştirin](visualize-qlik-sense.md)