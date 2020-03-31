---
title: Spark'taki Python kitaplıklarıyla web sitesi günlüklerini analiz et - Azure
description: Bu not defteri, Azure HDInsight'ta Spark ile özel bir kitaplık kullanarak günlük verilerinin nasıl analiz edilebildiğini gösterir.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/27/2019
ms.openlocfilehash: c6bf26d8f3a73db6ee69b2aa0de73872911893bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552721"
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-apache-spark-cluster-on-hdinsight"></a>HDInsight'ta Apache Spark kümesine sahip özel bir Python kitaplığı kullanarak web sitesi günlüklerini analiz edin

Bu not defteri, HDInsight'ta Apache Spark ile özel bir kitaplık kullanarak günlük verilerinin nasıl analiz edilebildiğini gösterir. Kullandığımız özel kitaplık **iislogparser.py**adlı bir Python kitaplığıdır.

## <a name="prerequisites"></a>Ön koşullar

HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Ham verileri RDD olarak kaydetme

Bu bölümde, ham örnek verilerinizi işleyen işleri çalıştırmak ve kovan tablosu olarak kaydetmek için HDInsight'ta Apache Spark kümesiyle ilişkili [Jupyter](https://jupyter.org) dizüstü bilgisayarını kullanıyoruz. Örnek veriler varsayılan olarak tüm kümelerde kullanılabilen bir .csv dosyasıdır (hvac.csv).

Verileriniz Apache Hive tablosu olarak kaydedildikten sonra, bir sonraki bölümde Power BI ve Tableau gibi BI araçlarını kullanarak Kovan tablosuna bağlanacağız.

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/jupyter`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. Yeni bir not defteri oluşturun. **Yeni'yi**ve ardından **PySpark'ı**seçin.

    ![Yeni bir Apache Jupyter dizüstü bilgisayar oluşturun](./media/apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Yeni bir Jupyter not defteri oluşturma")

1. Yeni bir not defteri oluşturulur ve Untitled.pynb adı ile açılır. En üstteki not defteri adını seçin ve samimi bir ad girin.

    ![Not defteri adını belirtme](./media/apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "Not defteri adını belirtme")

1. PySpark çekirdeğini kullanarak bir not defteri oluşturduğunuziçin, herhangi bir bağlamı açıkça oluşturmanız gerekmez. Birinci kod hücresini çalıştırdığınızda Spark ve Hive bağlamları sizin için otomatik olarak oluşturulur. Bu senaryo için gerekli türleri içe aktararak başlayabilirsiniz. Boş bir hücreye aşağıdaki snippet'i yapıştırın ve **sonra Shift + Enter**tuşuna basın.

    ```pyspark
    from pyspark.sql import Row
    from pyspark.sql.types import *
    ```

1. Kümede zaten bulunan örnek günlük verilerini kullanarak bir RDD oluşturun. Kümeyle ilişkili varsayılan depolama hesabındaki verilere `\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log`erişebilirsiniz. Şu kodu yürütün:

    ```pyspark
    logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')
    ```

1. Önceki adımın başarıyla tamamlandığını doğrulamak için örnek günlük kümesi ni alın.

    ```pyspark
    logs.take(5)
    ```

    Aşağıdaki metne benzer bir çıktı görmeniz gerekir:

    ```output
    [u'#Software: Microsoft Internet Information Services 8.0',
    u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
    u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
    u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
    u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']
    ```

## <a name="analyze-log-data-using-a-custom-python-library"></a>Özel bir Python kitaplığı kullanarak günlük verilerini çözümleme

1. Yukarıdaki çıktıda, ilk çift satır üstbilgi bilgilerini içerir ve kalan her satır bu üstbilgide açıklanan şemaile eşleşir. Bu tür günlükleri ayrıştMa karmaşık olabilir. Bu nedenle, bu tür günlükleri ayrıştırma çok daha kolay kılan özel bir Python kitaplığı **(iislogparser.py)** kullanırız. Varsayılan olarak, bu kitaplık HDInsight'taki Spark `/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py`kümenize 'de dahil edilir.

    Ancak, bu kitaplık gibi `PYTHONPATH` bir alma deyimi kullanarak kullanamazsınız bu `import iislogparser`yüzden değil. Bu kitaplığı kullanmak için, tüm işçi düğümlerine dağıtmamız gerekir. Aşağıdaki parçacığı çalıştırın.

    ```pyspark
    sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')
    ```

1. `iislogparser``parse_log_line` bir log line `None` üstbilgi satırı ise döndürür ve bir `LogLine` günlük satırı karşılaşırsa sınıfın bir örneği döndürür. YALNıZCA `LogLine` RDD'den günlük satırlarını ayıklamak için sınıfı kullanın:

    ```pyspark
    def parse_line(l):
        import iislogparser
        return iislogparser.parse_log_line(l)
    logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
    ```

1. Adımın başarıyla tamamlandığını doğrulamak için birkaç ayıklanmış günlük satırı alın.

    ```pyspark
    logLines.take(2)
    ```

   Çıktı aşağıdaki metne benzer olmalıdır:

    ```output
    [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
    ```

1. Sınıf, `LogLine` sırayla, bir günlük girişi `is_error()`bir hata kodu olup olmadığını döndürür gibi bazı yararlı yöntemler vardır. Ayıklanan günlük satırlarındaki hata sayısını hesaplamak ve ardından tüm hataları farklı bir dosyaya günlüğe kaydetmek için bu sınıfı kullanın.

    ```pyspark
    errors = logLines.filter(lambda p: p.is_error())
    numLines = logLines.count()
    numErrors = errors.count()
    print 'There are', numErrors, 'errors and', numLines, 'log entries'
    errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')
    ```

    Çıktı belirtilmelidir. `There are 30 errors and 646 log entries`

1. Ayrıca verilerin bir görselleştirme oluşturmak için **Matplotlib** kullanabilirsiniz. Örneğin, uzun süre çalışan isteklerin nedenini yalıtmak istiyorsanız, ortalama olarak hizmet vermek için en çok zaman alan dosyaları bulmak isteyebilirsiniz. Aşağıdaki parçacık, bir isteği sunmak için en çok zaman alan en iyi 25 kaynağı alır.

    ```pyspark
    def avgTimeTakenByKey(rdd):
        return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                    .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

    avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])
    ```

   Aşağıdaki metin gibi bir çıktı görmeniz gerekir:

    ```output
    [(u'/blogposts/mvc4/step13.png', 197.5),
    (u'/blogposts/mvc2/step10.jpg', 179.5),
    (u'/blogposts/extractusercontrol/step5.png', 170.0),
    (u'/blogposts/mvc4/step8.png', 159.0),
    (u'/blogposts/mvcrouting/step22.jpg', 155.0),
    (u'/blogposts/mvcrouting/step3.jpg', 152.0),
    (u'/blogposts/linqsproc1/step16.jpg', 138.75),
    (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
    (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
    (u'/blogposts/nested/step2.jpg', 126.0),
    (u'/blogposts/adminpack/step1.png', 124.0),
    (u'/BlogPosts/datalistpaging/step2.png', 118.0),
    (u'/blogposts/mvc4/step35.png', 117.0),
    (u'/blogposts/mvcrouting/step2.jpg', 116.5),
    (u'/blogposts/aboutme/basketball.jpg', 109.0),
    (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
    (u'/blogposts/mvc4/step12.png', 106.0),
    (u'/blogposts/linq8/step0.jpg', 105.5),
    (u'/blogposts/mvc2/step18.jpg', 104.0),
    (u'/blogposts/mvc2/step11.jpg', 104.0),
    (u'/blogposts/mvcrouting/step1.jpg', 104.0),
    (u'/blogposts/extractusercontrol/step1.png', 103.0),
    (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
    (u'/blogposts/mvcrouting/step21.jpg', 101.0),
    (u'/blogposts/mvc4/step1.png', 98.0)]
    ```

1. Ayrıca bu bilgileri arsa şeklinde sunabilirsiniz. Bir çizim oluşturmak için ilk adım olarak, önce geçici bir tablo **Ortalama Zaman**oluşturalım. Tablo, belirli bir zamanda olağandışı gecikme artışları olup olmadığını görmek için günlükleri zamana göre gruplandırıyor.

    ```pyspark
    avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
    schema = StructType([StructField('Minutes', IntegerType(), True),
                        StructField('Time', FloatType(), True)])

    avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
    avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
    ```

1. Daha sonra **Ortalama Zaman** tablosundaki tüm kayıtları almak için aşağıdaki SQL sorgusunu çalıştırabilirsiniz.

    ```pyspark
    %%sql -o averagetime
    SELECT * FROM AverageTime
    ```

   Ardından `%%sql` gelen `-o averagetime` büyü, sorguçıktısının Jupyter sunucusunda (genellikle kümenin baş düğümü) yerel olarak kalıcı olmasını sağlar. Çıktı, belirtilen ad **ortalamasına**sahip bir [Pandas](https://pandas.pydata.org/) veri çerçevesi olarak devam eder.

   Aşağıdaki resim gibi bir çıktı görmeniz gerekir:

   ![hdinsight jupyter sql sorgu çıktısı](./media/apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL sorgu çıktısı")

   Büyü hakkında daha fazla bilgi için % [sql magic ile desteklenen Parametrelere](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic)bakın. `%%sql`

1. Artık bir çizim oluşturmak için verilerin görselleştirilmesini oluşturmak için kullanılan bir kitaplık olan Matplotlib'i kullanabilirsiniz. Çizim yerel olarak kalıcı **ortalama zaman** veri çerçevesinden oluşturulması gerektiğinden, kod parçacığı `%%local` sihirle başlamalıdır. Bu, kodun Jupyter sunucusunda yerel olarak çalıştırılmasını sağlar.

    ```pyspark
    %%local
    %matplotlib inline
    import matplotlib.pyplot as plt

    plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
    plt.xlabel('Time (min)')
    plt.ylabel('Average time taken for request (ms)')
    ```

   Aşağıdaki resim gibi bir çıktı görmeniz gerekir:

   ![apache kıvılcım web günlüğü analizi arsa](./media/apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib çıkışı")

1. Uygulamayı çalıştırmayı bitirdikten sonra, kaynakları serbest bırakmak için not defterini kapatmanız gerekir. Bunu yapmak için not defterindeki **Dosya** menüsünde **Kapat ve Durdur**’u seçin. Bu eylem defteri kapatır ve kapatır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri keşfedin:

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)
* [Jupyter not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)