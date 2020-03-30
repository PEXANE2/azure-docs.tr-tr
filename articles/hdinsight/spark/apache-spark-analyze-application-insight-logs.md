---
title: Spark ile Uygulama Öngörügünlüklerini Analiz Et - Azure HDInsight
description: Application Insight günlüklerini blob depolamasına nasıl dışa aktarır ve ardından HDInsight'ta Spark ile günlükleri nasıl analiz edebilirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: 6fd7682f56fbe446904a4acdb39e78525f2523a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435237"
---
# <a name="analyze-application-insights-telemetry-logs-with-apache-spark-on-hdinsight"></a>HDInsight'ta Apache Spark ile Uygulama Öngörülerini telemetri günlüklerini analiz edin

Application Insight telemetri verilerini analiz etmek için HDInsight'ta [Apache Spark'ı](https://spark.apache.org/) nasıl kullanacağınızı öğrenin.

[Visual Studio Application Insights,](../../azure-monitor/app/app-insights-overview.md) web uygulamalarınızı izleyen bir analiz hizmetidir. Application Insights tarafından oluşturulan telemetri verileri Azure Depolama'ya aktarılabilir. Veriler Azure Depolama alanına girdiğinde, HDInsight verileri analiz etmek için kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

* Uygulama Öngörüleri'ni kullanmak üzere yapılandırılan bir uygulama.

* Linux tabanlı bir HDInsight kümesi oluşturma konusunda aşinalık. Daha fazla bilgi için [HDInsight'ta Apache Spark Oluştur'a](apache-spark-jupyter-spark-sql.md)bakın.

* Bir web tarayıcısı.

Bu belgenin geliştirilmesi nde ve test edilmesinde aşağıdaki kaynaklar kullanılmıştır:

* Application Insights telemetri [verileri, Application Insights'ı kullanmak üzere yapılandırılan bir Düğüm.js web uygulaması](../../azure-monitor/app/nodejs.md)kullanılarak oluşturuldu.

* Verileri analiz etmek için HDInsight küme sürümü 3.5'te Linux tabanlı bir Kıvılcım kullanıldı.

## <a name="architecture-and-planning"></a>Mimarlık ve planlama

Aşağıdaki diyagram, bu örneğin hizmet mimarisini göstermektedir:

![Uygulama Öngörüleri'nden blob depolamasına akan veriler, ardından Spark](./media/apache-spark-analyze-application-insight-logs/application-insights.png)

### <a name="azure-storage"></a>Azure Storage

Uygulama Öngörüleri, telemetri bilgilerini sürekli olarak blobs'a dışa aktaracak şekilde yapılandırılabilir. HDInsight daha sonra lekelerde depolanan verileri okuyabilir. Ancak, izlemeniz gereken bazı gereksinimler vardır:

* **Konum**: Depolama Hesabı ve HDInsight farklı konumlardaysa gecikme yi artırabilir. Bölgeler arasında hareket eden verilere çıkış ücretleri uygulandığından, maliyeti de artırır.

    > [!WARNING]  
    > HDInsight'tan farklı bir konumda depolama hesabı kullanmak desteklenmez.

* **Blob türü**: HDInsight yalnızca blok blobları destekler. Application Insights varsayılan olarak blok blobs kullanmakiçin, bu nedenle varsayılan olarak HDInsight ile çalışması gerekir.

Varolan bir kümeye depolama alanı ekleme hakkında bilgi için [ek depolama hesapları](../hdinsight-hadoop-add-storage.md) ekle belgesine bakın.

### <a name="data-schema"></a>Veri şeması

Application Insights, blobs'a dışa aktarılan telemetri veri biçimi için [dışa](../../azure-monitor/app/export-data-model.md) aktarma veri modeli bilgileri sağlar. Bu belgedeki adımlar, verilerle çalışmak için Spark SQL'i kullanır. Spark SQL, Application Insights tarafından günlüğe kaydedilen JSON veri yapısı için otomatik olarak bir şema oluşturabilir.

## <a name="export-telemetry-data"></a>Telemetri verilerini dışa aktarma

Uygulama Öngörülerinizi azure depolama blob'una telemetri bilgilerini dışa aktarmak için yapılandırmak için [Sürekli Dışaklığı Yapılandırma](../../azure-monitor/app/export-telemetry.md) adımlarını izleyin.

## <a name="configure-hdinsight-to-access-the-data"></a>Verilere erişmek için HDInsight'ı yapılandırın

Bir HDInsight kümesi oluşturuyorsanız, küme oluşturma sırasında depolama hesabını ekleyin.

Azure Depolama Hesabı'nı varolan bir kümeye eklemek için [ek Depolama Hesapları Ekle](../hdinsight-hadoop-add-storage.md) belgesindeki bilgileri kullanın.

## <a name="analyze-the-data-pyspark"></a>Verileri analiz edin: PySpark

1. Bir web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net/jupyter` CLUSTERNAME'nin kümenizin adı olduğu yere gidin.

2. Jupyter sayfasının sağ üst köşesinde, **Yeni**ve ardından **PySpark'ı**seçin. Python tabanlı Jupyter Notebook içeren yeni bir tarayıcı sekmesi açılır.

3. Sayfadaki ilk alana **(hücre**olarak adlandırılır) aşağıdaki metni girin:

   ```python
   sc._jsc.hadoopConfiguration().set('mapreduce.input.fileinputformat.input.dir.recursive', 'true')
   ```

    Bu kod, giriş verilerinin dizin yapısına özyinelemeli olarak erişmek üzere Spark'ı yapılandırır. Uygulama Öngörüleri telemetrisi benzer bir dizin `/{telemetry type}/YYYY-MM-DD/{##}/`yapısına kaydedilir.

4. Kodu çalıştırmak için **SHIFT+ENTER'ı** kullanın. Hücrenin sol tarafında, bu\*hücredeki kodun yürütülmekte olduğunu belirtmek için parantez arasında bir ' ' ' görünür. Tamamlandıktan sonra ,\*' ' ' bir sayıya dönüşür ve aşağıdaki metne benzer çıktı hücrenin altında görüntülenir:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    pyspark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...

5. İlkinin altında yeni bir hücre oluşturulur. Yeni hücreye aşağıdaki metni girin. Uygulama Öngörüleri verilerini içeren Azure Depolama hesap adı ve blob kapsayıcı adı ile değiştirin. `CONTAINER` `STORAGEACCOUNT`

   ```python
   %%bash
   hdfs dfs -ls wasbs://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Bu hücreyi çalıştırmak için **SHIFT+ENTER'ı** kullanın. Aşağıdaki metne benzer bir sonuç görürsünüz:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasbs://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Döndürülen wasbs yolu, Application Insights telemetri verilerinin konumudur. Döndürülen `hdfs dfs -ls` wasbs yolunu kullanmak için hücredeki satırı değiştirin ve hücreyi yeniden çalıştırmak için **SHIFT+ENTER'ı** kullanın. Bu kez, sonuçlar telemetri verileri içeren dizinleri görüntülemelidir.

   > [!NOTE]  
   > Bu bölümdeki adımların geri kalanı `wasbs://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` için dizin kullanılmıştır. Dizin yapınız farklı olabilir.

6. Sonraki hücrede, aşağıdaki kodu girin: Önceki adımdan yol ile değiştirin. `WASB_PATH`

   ```python
   jsonFiles = sc.textFile('WASB_PATH')
   jsonData = sqlContext.read.json(jsonFiles)
   ```

    Bu kod, sürekli dışa aktarma işlemi tarafından dışa aktarılan JSON dosyalarından bir veri çerçevesi oluşturur. Bu hücreyi çalıştırmak için **SHIFT+ENTER'ı** kullanın.
7. Sonraki hücrede, Spark'ın JSON dosyaları için oluşturduğu şemayı görüntülemek için aşağıdakileri girin ve çalıştırın:

   ```python
   jsonData.printSchema()
   ```

    Her telemetri türü için şema farklıdır. Aşağıdaki örnek, web istekleri `Requests` (alt dizinde depolanan veriler) için oluşturulan şemadır:

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)

8. Veri çerçevesini geçici bir tablo olarak kaydetmek ve verileri karşı bir sorgu çalıştırmak için aşağıdakileri kullanın:

   ```python
   jsonData.registerTempTable("requests")
   df = sqlContext.sql("select context.location.city from requests where context.location.city is not null")
   df.show()
   ```

    Bu sorgu, context.location.city'nin geçersiz olmadığı en iyi 20 kaydın şehir bilgilerini döndürür.

   > [!NOTE]  
   > Bağlam yapısı, Application Insights tarafından günlüğe kaydedilen tüm telemetrilerde bulunur. Şehir öğesi günlüklerinizde doldurulmayabilir. Günlükleriniz için veri içerebilecek sorgulayabileceğiniz diğer öğeleri tanımlamak için şemayı kullanın.

    Bu sorgu, aşağıdaki metne benzer bilgileri döndürür:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="analyze-the-data-scala"></a>Verileri analiz edin: Scala

1. Bir web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net/jupyter` CLUSTERNAME'nin kümenizin adı olduğu yere gidin.

2. Jupyter sayfasının sağ üst köşesinde, **Yeni**seçin ve sonra **Scala**. Scala tabanlı Jupyter Notebook içeren yeni bir tarayıcı sekmesi görüntülenir.

3. Sayfadaki ilk alana **(hücre**olarak adlandırılır) aşağıdaki metni girin:

   ```scala
   sc.hadoopConfiguration.set("mapreduce.input.fileinputformat.input.dir.recursive", "true")
   ```

    Bu kod, giriş verilerinin dizin yapısına özyinelemeli olarak erişmek üzere Spark'ı yapılandırır. Application Insights telemetrisi benzer bir dizin yapısına `/{telemetry type}/YYYY-MM-DD/{##}/`kaydedilir.

4. Kodu çalıştırmak için **SHIFT+ENTER'ı** kullanın. Hücrenin sol tarafında, bu\*hücredeki kodun yürütülmekte olduğunu belirtmek için parantez arasında bir ' ' ' görünür. Tamamlandıktan sonra ,\*' ' ' bir sayıya dönüşür ve aşağıdaki metne benzer çıktı hücrenin altında görüntülenir:

        Creating SparkContext as 'sc'

        ID    YARN Application ID    Kind    State    Spark UI    Driver log    Current session?
        3    application_1468969497124_0001    spark    idle    Link    Link    ✔

        Creating HiveContext as 'sqlContext'
        SparkContext and HiveContext created. Executing user code ...

5. İlkinin altında yeni bir hücre oluşturulur. Yeni hücreye aşağıdaki metni girin. Uygulama Öngörüleri günlüklerini içeren Azure Depolama hesap adı ve blob kapsayıcı adı ile değiştirin. `CONTAINER` `STORAGEACCOUNT`

   ```scala
   %%bash
   hdfs dfs -ls wasbs://CONTAINER@STORAGEACCOUNT.blob.core.windows.net/
   ```

    Bu hücreyi çalıştırmak için **SHIFT+ENTER'ı** kullanın. Aşağıdaki metne benzer bir sonuç görürsünüz:

        Found 1 items
        drwxrwxrwx   -          0 1970-01-01 00:00 wasbs://appinsights@contosostore.blob.core.windows.net/contosoappinsights_2bededa61bc741fbdee6b556571a4831

    Döndürülen wasbs yolu, Application Insights telemetri verilerinin konumudur. Döndürülen `hdfs dfs -ls` wasbs yolunu kullanmak için hücredeki satırı değiştirin ve hücreyi yeniden çalıştırmak için **SHIFT+ENTER'ı** kullanın. Bu kez, sonuçlar telemetri verileri içeren dizinleri görüntülemelidir.

   > [!NOTE]  
   > Bu bölümdeki adımların geri kalanı `wasbs://appinsights@contosostore.blob.core.windows.net/contosoappinsights_{ID}/Requests` için dizin kullanılmıştır. Telemetri verileriniz bir web uygulaması için olmadığı sürece bu dizin mevcut olmayabilir.

6. Sonraki hücrede, aşağıdaki kodu girin: Önceki adımdan yol ile değiştirin. `WASB\_PATH`

   ```scala
   var jsonFiles = sc.textFile('WASB_PATH')
   val sqlContext = new org.apache.spark.sql.SQLContext(sc)
   var jsonData = sqlContext.read.json(jsonFiles)
   ```

    Bu kod, sürekli dışa aktarma işlemi tarafından dışa aktarılan JSON dosyalarından bir veri çerçevesi oluşturur. Bu hücreyi çalıştırmak için **SHIFT+ENTER'ı** kullanın.

7. Sonraki hücrede, Spark'ın JSON dosyaları için oluşturduğu şemayı görüntülemek için aşağıdakileri girin ve çalıştırın:

   ```scala
   jsonData.printSchema
   ```

    Her telemetri türü için şema farklıdır. Aşağıdaki örnek, web istekleri `Requests` (alt dizinde depolanan veriler) için oluşturulan şemadır:

        root
        |-- context: struct (nullable = true)
        |    |-- application: struct (nullable = true)
        |    |    |-- version: string (nullable = true)
        |    |-- custom: struct (nullable = true)
        |    |    |-- dimensions: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |    |-- metrics: array (nullable = true)
        |    |    |    |-- element: string (containsNull = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- eventTime: string (nullable = true)
        |    |    |-- isSynthetic: boolean (nullable = true)
        |    |    |-- samplingRate: double (nullable = true)
        |    |    |-- syntheticSource: string (nullable = true)
        |    |-- device: struct (nullable = true)
        |    |    |-- browser: string (nullable = true)
        |    |    |-- browserVersion: string (nullable = true)
        |    |    |-- deviceModel: string (nullable = true)
        |    |    |-- deviceName: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- osVersion: string (nullable = true)
        |    |    |-- type: string (nullable = true)
        |    |-- location: struct (nullable = true)
        |    |    |-- city: string (nullable = true)
        |    |    |-- clientip: string (nullable = true)
        |    |    |-- continent: string (nullable = true)
        |    |    |-- country: string (nullable = true)
        |    |    |-- province: string (nullable = true)
        |    |-- operation: struct (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |-- session: struct (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- isFirst: boolean (nullable = true)
        |    |-- user: struct (nullable = true)
        |    |    |-- anonId: string (nullable = true)
        |    |    |-- isAuthenticated: boolean (nullable = true)
        |-- internal: struct (nullable = true)
        |    |-- data: struct (nullable = true)
        |    |    |-- documentVersion: string (nullable = true)
        |    |    |-- id: string (nullable = true)
        |-- request: array (nullable = true)
        |    |-- element: struct (containsNull = true)
        |    |    |-- count: long (nullable = true)
        |    |    |-- durationMetric: struct (nullable = true)
        |    |    |    |-- count: double (nullable = true)
        |    |    |    |-- max: double (nullable = true)
        |    |    |    |-- min: double (nullable = true)
        |    |    |    |-- sampledValue: double (nullable = true)
        |    |    |    |-- stdDev: double (nullable = true)
        |    |    |    |-- value: double (nullable = true)
        |    |    |-- id: string (nullable = true)
        |    |    |-- name: string (nullable = true)
        |    |    |-- responseCode: long (nullable = true)
        |    |    |-- success: boolean (nullable = true)
        |    |    |-- url: string (nullable = true)
        |    |    |-- urlData: struct (nullable = true)
        |    |    |    |-- base: string (nullable = true)
        |    |    |    |-- hashTag: string (nullable = true)
        |    |    |    |-- host: string (nullable = true)
        |    |    |    |-- protocol: string (nullable = true)

8. Veri çerçevesini geçici bir tablo olarak kaydetmek ve verileri karşı bir sorgu çalıştırmak için aşağıdakileri kullanın:

   ```scala
   jsonData.registerTempTable("requests")
   var city = sqlContext.sql("select context.location.city from requests where context.location.city isn't null limit 10").show()
   ```

    Bu sorgu, context.location.city'nin geçersiz olmadığı en iyi 20 kaydın şehir bilgilerini döndürür.

   > [!NOTE]  
   > Bağlam yapısı, Application Insights tarafından günlüğe kaydedilen tüm telemetrilerde bulunur. Şehir öğesi günlüklerinizde doldurulmayabilir. Günlükleriniz için veri içerebilecek sorgulayabileceğiniz diğer öğeleri tanımlamak için şemayı kullanın.

    Bu sorgu, aşağıdaki metne benzer bilgileri döndürür:

        +---------+
        |     city|
        +---------+
        | Bellevue|
        |  Redmond|
        |  Seattle|
        |Charlotte|
        ...
        +---------+

## <a name="next-steps"></a>Sonraki adımlar

Azure'daki veri ve hizmetlerle çalışmak için Apache Spark'ı kullanmaya daha fazla örnek için aşağıdaki belgelere bakın:

* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning ile Apache Spark: Gıda denetimi sonuçlarını tahmin etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight'ta Apache Spark kullanarak web sitesi günlük analizi](apache-spark-custom-library-website-log-analysis.md)

Spark uygulamaları oluşturma ve çalıştırma hakkında bilgi için aşağıdaki belgelere bakın:

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Livy'yi kullanarak apache Spark kümesinde işleri uzaktan çalıştırın](apache-spark-livy-rest-interface.md)
