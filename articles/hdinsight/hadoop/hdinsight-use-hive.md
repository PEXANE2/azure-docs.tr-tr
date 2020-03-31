---
title: Apache Hive ve HiveQL nedir - Azure HDInsight
description: Apache Hive, Apache Hadoop için bir veri ambarı sistemidir. Hive'da depolanan verileri Transact-SQL'e benzer HiveQL kullanarak sorgulayabilirsiniz. Bu belgede, Azure HDInsight ile Hive ve HiveQL'yi nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 02/28/2020
ms.openlocfilehash: f7dc7b520cba2bbf2351d93795a1a26b3b5124be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471362"
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Azure HDInsight'ta Apache Hive ve HiveQL nedir?

[Apache Hive,](https://hive.apache.org/) Apache Hadoop için bir veri ambarı sistemidir. Kovan veri özetleme, sorgulama ve veri analizi sağlar. Kovan sorguları, SQL'e benzer bir sorgu dili olan HiveQL'de yazılır.

Kovan, yapıyı büyük ölçüde yapılandırılmamış veriler üzerinde yansıtmanızı sağlar. Yapıyı tanımladıktan sonra, Java veya MapReduce bilgisi olmadan verileri sorgulamak için HiveQL'yi kullanabilirsiniz.

HDInsight, belirli iş yükleri için ayarlanmış birkaç küme türü sağlar. Aşağıdaki küme türleri en sık Kovan sorguları için kullanılır:

|Küme türü |Açıklama|
|---|---|
|Interactive Query|Etkileşimli sorgular için yanıt sürelerini iyileştirmek için [Düşük GecikmeLi Analitik İşleme (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) işlevselliği sağlayan bir Hadoop kümesi. Daha fazla bilgi için [HDInsight belgesinde Etkileşimli Sorguyla Başlat'a](../interactive-query/apache-interactive-query-get-started.md) bakın.|
|Hadoop|Toplu işleme iş yükleri için ayarlanmış bir Hadoop kümesi. Daha fazla bilgi için [HDInsight belgesinde Apache Hadoop ile Başlat'a](../hadoop/apache-hadoop-linux-tutorial-get-started.md) bakın.|
|Spark|Apache Spark, Hive ile çalışmak için yerleşik işlevsellik sağlar. Daha fazla bilgi için [HDInsight belgesinde Apache Spark ile Başlat'a](../spark/apache-spark-jupyter-spark-sql.md) bakın.|
|HBase|HiveQL, Apache HBase'de depolanan verileri sorgulamak için kullanılabilir. Daha fazla bilgi için [HDInsight belgesinde Apache HBase ile Başlat'a](../hbase/apache-hbase-tutorial-get-started-linux.md) bakın.|

## <a name="how-to-use-hive"></a>Hive nasıl kullanılır

HDInsight ile Kovan'ı kullanmanın farklı yollarını keşfetmek için aşağıdaki tabloyu kullanın:

| İstersenbu **yöntemi kullanın...** | ... **etkileşimli** sorgular | ... **toplu** işleme | ... bu **istemci işletim sisteminden** |
|:--- |:---:|:---:|:--- |:--- |
| [Visual Studio Code için HDInsight araçları](../hdinsight-for-vscode.md) |✔ |✔ | Linux, Unix, Mac OS X veya Windows |
| [Visual Studio için HDInsight araçları](../hadoop/apache-hadoop-use-hive-visual-studio.md) |✔ |✔ |Windows |
| [Hive Görünümü](../hadoop/apache-hadoop-use-hive-ambari-view.md) |✔ |✔ |Any (tarayıcı tabanlı) |
| [Beeline istemcisi](../hadoop/apache-hadoop-use-hive-beeline.md) |✔ |✔ |Linux, Unix, Mac OS X veya Windows |
| [REST API](../hadoop/apache-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux, Unix, Mac OS X veya Windows |
| [Windows PowerShell](../hadoop/apache-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Windows |

## <a name="hiveql-language-reference"></a>HiveQL dil başvurusu

HiveQL dil referans [ı dil kılavuzunda](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)mevcuttur.

## <a name="hive-and-data-structure"></a>Kovan ve veri yapısı

Kovan yapılandırılmış ve yarı yapılandırılmış verilerle nasıl çalışılacağını anlar. Örneğin, alanların belirli karakterlerle sınırlandırıldığı metin dosyaları. Aşağıdaki HiveQL deyimi, alanı sınırlandıran veriler üzerinde bir tablo oluşturur:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Kovan, karmaşık veya düzensiz yapılandırılmış veriler için özel **serializer/deserializers (SerDe)** destekler. Daha fazla bilgi için [HDInsight belgesine sahip özel bir JSON SerDe nasıl kullanılır bölümüne](https://web.archive.org/web/20190217104719/https://blogs.msdn.microsoft.com/bigdatasupport/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight/) bakın.

Hive tarafından desteklenen dosya biçimleri hakkında daha fazla bilgi için [Dil kılavuzunahttps://cwiki.apache.org/confluence/display/Hive/LanguageManual) ](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) bakın (

### <a name="hive-internal-tables-vs-external-tables"></a>Kovan iç tablolar vs dış tablolar

Hive ile oluşturabileceğiniz iki tür tablo vardır:

* __Internal__: Veriler Kovan veri ambarında depolanır. Veri ambarı küme `/hive/warehouse/` için varsayılan depolama yerinde bulunur.

    Aşağıdaki koşullardan biri geçerli olduğunda dahili tabloları kullanın:

    * Veriler geçicidir.
    * Hive'ın tablo nun ve verilerin yaşam döngüsünü yönetmesini istiyorsunuz.

* __Harici__: Veriler veri ambarı dışında depolanır. Veriler küme tarafından erişilebilen herhangi bir depolama da depolanabilir.

    Aşağıdaki koşullardan biri geçerli olduğunda harici tabloları kullanın:

    * Veriler Hive dışında da kullanılır. Örneğin, veri dosyaları başka bir işlem tarafından güncelleştirilir (dosyaları kilitlemez.)
    * Tablodüştükten sonra bile verilerin altta yatan konumda kalması gerekir.
    * Varsayılan olmayan bir depolama hesabı gibi özel bir konuma ihtiyacınız vardır.
    * Kovan dışındaki bir program veri biçimini, konumunu ve benzeri bilgileri yönetir.

Daha fazla bilgi [için, Kovan İç ve Dış Tablolar Intro](https://blogs.msdn.microsoft.com/cindygross/2013/02/05/hdinsight-hive-internal-and-external-tables-intro/) blog yazısına bakın.

## <a name="user-defined-functions-udf"></a>Kullanıcı tanımlı fonksiyonlar (UDF)

Kovan da **kullanıcı tanımlı fonksiyonlar (UDF)** ile uzatılabilir. UDF, HiveQL'de kolayca modellenmemiş işlevsellik veya mantığı uygulamanızı sağlar. Hive ile UDF'ler kullanma örneği için aşağıdaki belgelere bakın:

* [Apache Hive ile Java kullanıcı tanımlı bir işlev kullanma](../hadoop/apache-hadoop-hive-java-udf.md)

* [Apache Hive ile Python kullanıcı tanımlı bir işlev kullanma](../hadoop/python-udf-hdinsight.md)

* [Apache Hive ile C# kullanıcı tanımlı bir işlev kullanma](../hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [HDInsight'a özel bir Apache Hive kullanıcı tanımlı işlev ekleme](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Tarih/saat biçimlerini Hive zaman damgasına dönüştürmek için örnek Apache Hive kullanıcı tanımlı işlev](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a name="example-data"></a>Örnek veriler

HDInsight'ta Kovan, ". `hivesampletable` HDInsight ayrıca Hive ile kullanılabilecek örnek veri kümeleri de sağlar. Bu veri kümeleri `/example/data` ve `/HdiSamples` dizinlerde depolanır. Bu dizinler kümeniz için varsayılan depolama bulunmaktadır.

## <a name="example-hive-query"></a>Örnek Kovan sorgusu

Aşağıdaki HiveQL deyimleri `/example/data/sample.log` dosya üzerine sütunlar proje:

```hiveql
DROP TABLE log4jLogs;
CREATE EXTERNAL TABLE log4jLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs
    WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log'
    GROUP BY t4;
```

Önceki örnekte, HiveQL deyimleri aşağıdaki eylemleri gerçekleştirir:

|Deyim |Açıklama |
|---|---|
|DAMLA TABLOSU|Tablo zaten varsa, silin.|
|DıŞ TABLO OLUŞTURMA|Hive'da yeni bir **dış** tablo oluşturur. Dış tablolar yalnızca Hive tablo tanımını depolar. Veriler özgün konumda ve özgün biçimde bırakılır.|
|SATıR BIÇIMI|Hive'a verilerin nasıl biçimlendiğini söyler. Bu durumda, her günlükteki alanlar bir boşlukla ayrılır.|
|TEXTFILE KONUMU OLARAK DEPOLANAN|Hive'a verilerin nerede depolandığı `example/data` (dizin) ve metin olarak depolandığı söyler. Veriler tek bir dosyada olabilir veya dizin içindeki birden çok dosyaya yayılabilir.|
|SELECT|**T4** sütununun **[HATA]** değerini içerdiği tüm satırların sayısını seçer. Bu değeri içeren üç satır olduğundan, bu ifade **3'ün** değerini döndürür.|
|INPUT__FILE__NAME LIKE '%.log'|Hive şemayı dizindeki tüm dosyalara uygulamaya çalışır. Bu durumda, dizin şema eşleşmiyor dosyaları içerir. Sonuçlardaki çöp verilerini önlemek için bu ifade Hive'a yalnızca .log ile biten dosyalardan veri döndürmemiz gerektiğini söyler.|

> [!NOTE]  
> Temel verilerin harici bir kaynak tarafından güncelleştirilmesini beklerken dış tablolar kullanılmalıdır. Örneğin, otomatik bir veri yükleme işlemi veya MapReduce işlemi.
>
> Harici bir tablonun düşmesi verileri **silmez,** yalnızca tablo tanımını siler.

Harici yerine **dahili** bir tablo oluşturmak için aşağıdaki HiveQL'yi kullanın:

```hiveql
CREATE TABLE IF NOT EXISTS errorLogs (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
STORED AS ORC;
INSERT OVERWRITE TABLE errorLogs
SELECT t1, t2, t3, t4, t5, t6, t7 
    FROM log4jLogs WHERE t4 = '[ERROR]';
```

Bu ifadeler aşağıdaki eylemleri gerçekleştirir:

|Deyim |Açıklama |
|---|---|
|VARSA TABLO OLUŞTUR|Tablo yoksa, oluşturun. **EXTERNAL** anahtar sözcüğü kullanılmadığından, bu deyim bir iç tablo oluşturur. Tablo Hive veri ambarında depolanır ve tamamen Hive tarafından yönetilir.|
|ORK OLARAK DEPOLANAN|Verileri Optimize Edilmiş Satır Sütunu (ORC) biçiminde depolar. ORC, Kovan verilerini depolamak için son derece optimize edilmiş ve verimli bir biçimdir.|
|ÜZERINE EKLEME ... Seçin|**[ERROR]** içeren **log4jLogs** tablosundan satırları seçer ve sonra **verileri errorLogs** tablosuna ekler.|

> [!NOTE]  
> Dış tabloların aksine, dahili tablonun düşmesi de temel verileri siler.

## <a name="improve-hive-query-performance"></a>Hive sorgu performansını geliştirme

### <a name="apache-tez"></a>Apaçi Tez

[Apache Tez,](https://tez.apache.org) Hive gibi veri yoğun uygulamaların ölçekte çok daha verimli çalışmasını sağlayan bir çerçevedir. Tez varsayılan olarak etkinleştirilir.  [Tez tasarım belgelerindeki Apache Hive,](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) uygulama seçenekleri ve ayar yapılandırmaları hakkında ayrıntılar içerir.

### <a name="low-latency-analytical-processing-llap"></a>Düşük GecikmeLi Analitik İşleme (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (bazen Live Long and Process olarak da bilinir), Hive 2.0'da sorguların bellekte önbelleğe aldamasına izin veren yeni bir özelliktir. LLAP hive sorguları çok daha hızlı yapar, [bazı durumlarda Hive 1.x daha hızlı 26x](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

HDInsight, Etkileşimli Sorgu küme türünde LLAP sağlar. Daha fazla bilgi için [Etkileşimli Sorgu yla Başlat](../interactive-query/apache-interactive-query-get-started.md) belgesine bakın.

## <a name="scheduling-hive-queries"></a>Hive sorgularını zamanlama

Zamanlanmış veya isteğe bağlı iş akışının bir parçası olarak Kovan sorgularını çalıştırmak için kullanılabilecek çeşitli hizmetler vardır.

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Veri Fabrikası, HDInsight'ı Veri Fabrikası ardışık hattının bir parçası olarak kullanmanıza olanak tanır. Bir ardışık kaynaktan Hive kullanma hakkında daha fazla bilgi için [Azure Veri Fabrikası belgesinde Kovan etkinliğini kullanarak Verileri Dönüştür'e](../../data-factory/transform-data-using-hadoop-hive.md) bakın.

### <a name="hive-jobs-and-sql-server-integration-services"></a>Kovan işleri ve SQL Server Entegrasyon Hizmetleri

Bir Hive işini çalıştırmak için SQL Server Integration Services 'ı (SSIS) kullanabilirsiniz. SSIS için Azure Özellik Paketi, HDInsight'ta Hive işleri ile çalışan aşağıdaki bileşenleri sağlar.

* [Azure HDInsight Kovan Görevi](https://docs.microsoft.com/sql/integration-services/control-flow/azure-hdinsight-hive-task)

* [Azure Abonelik Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-subscription-connection-manager)

Daha fazla bilgi için [Azure Özellik Paketi](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis) belgelerine bakın.

### <a name="apache-oozie"></a>Apaçi Oozie

Apache Oozie Hadoop işleri yöneten bir iş akışı ve koordinasyon sistemidir. Hive ile Oozie kullanma hakkında daha fazla bilgi [için, bir iş akışı belgesini tanımlamak ve çalıştırmak için Apache Oozie'yi kullanın'a](../hdinsight-use-oozie-linux-mac.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Hive'ın ne olduğunu ve HDInsight'ta Hadoop ile nasıl kullanılacağını öğrendiğiniz için, Azure HDInsight ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın.

* [HDInsight'a veri yükleme](../hdinsight-upload-data.md)
* [HDInsight'ta Apache Hive ve Apache Pig ile Python Kullanıcı Tanımlı Fonksiyonlar (UDF) kullanın](./python-udf-hdinsight.md)
* [HDInsight ile MapReduce işleri kullanma](hdinsight-use-mapreduce.md)
