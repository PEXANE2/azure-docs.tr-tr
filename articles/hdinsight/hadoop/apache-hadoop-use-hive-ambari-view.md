---
title: Azure HDInsight'ta Apache Hadoop ile Apache Ambari Hive Görünümü'ni kullanma
description: Hive sorguları göndermek için web tarayıcınızdan Hive Görünümü'nün nasıl kullanılacağını öğrenin. Hive View, Linux tabanlı HDInsight kümenizle sağlanan Ambari Web UI'sinin bir parçasıdır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/06/2020
ms.openlocfilehash: 787d88d336abcf3b0ba9b14c3d3798850b665eca
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80745101"
---
# <a name="use-apache-ambari-hive-view-with-apache-hadoop-in-hdinsight"></a>HDInsight'ta Apache Hadoop ile Apache Ambari Hive Görünümünü Kullanma

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Apache Ambari Hive View'ı kullanarak Hive sorgularını nasıl çalıştıracağınızhakkında bilgi edinin. Hive View, web tarayıcınızdan Hive sorgularını yazmanızı, optimize etmenizi ve çalıştırmanızı sağlar.

## <a name="prerequisites"></a>Ön koşullar

HDInsight'ta hadoop kümesi. [Linux'ta HDInsight ile başlayın](./apache-hadoop-linux-tutorial-get-started.md)bakın.

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

1. Azure [portalından](https://portal.azure.com/)kümenizi seçin.  Bkz. Yönergeler için [liste ve gösteri kümeleri.](../hdinsight-administer-use-portal-linux.md#showClusters) Küme yeni bir portal görünümünde açılır.

1. **Küme panolarından** **Ambari görünümlerini**seçin. Kimlik doğrulaması istendiğinde, kümeyi oluşturduğunuzda `admin`sağladığınız küme giriş (varsayılan) hesap adını ve parolasını kullanın. Ayrıca, kümenizin adının `https://CLUSTERNAME.azurehdinsight.net/#/main/views` `CLUSTERNAME` bulunduğu tarayıcınızda da gidebilirsiniz.

1. Görünümler listesinden __Kovan Görünümü'nü__seçin.

    ![Apache Ambari Apache Hive görünümünü seçin](./media/apache-hadoop-use-hive-ambari-view/select-apache-hive-view.png)

    Kovan görünüm sayfası aşağıdaki resme benzer:

    ![Hive görünümü için sorgu çalışma sayfasının görüntüsü](./media/apache-hadoop-use-hive-ambari-view/ambari-worksheet-view.png)

1. __Sorgu__ sekmesinden, aşağıdaki HiveQL deyimlerini çalışma sayfasına yapıştırın:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs(
        t1 string,
        t2 string,
        t3 string,
        t4 string,
        t5 string,
        t6 string,
        t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS loglevel, COUNT(*) AS count FROM log4jLogs
        WHERE t4 = '[ERROR]'
        GROUP BY t4;
    ```

    Bu ifadeler aşağıdaki eylemleri yapmak:

    |Deyim | Açıklama |
    |---|---|
    |DAMLA TABLOSU|Tablo zaten varsa tabloyu ve veri dosyasını siler.|
    |DıŞ TABLO OLUŞTURMA|Hive'da yeni bir "harici" tablo oluşturur. Dış tablolar yalnızca Hive'da tablo tanımını depolar. Veriler özgün konumda bırakılır.|
    |SATıR BIÇIMI|Verilerin nasıl biçimlendirilir olduğunu gösterir. Bu durumda, her günlükteki alanlar bir boşlukla ayrılır.|
    |TEXTFILE KONUMU OLARAK DEPOLANAN|Verilerin nerede depolandığı ve metin olarak depolandığı gösterir.|
    |SELECT|T4 sütununun [HATA] değerini içerdiği tüm satırların sayısını seçer.|

   > [!IMPORTANT]  
   > __Veritabanı__ seçimini __varsayılan olarak__bırakın. Bu belgedeki örnekler, HDInsight ile birlikte varsayılan veritabanını kullanır.

1. Sorguyu başlatmak için **Execute** çalışma sayfasının altında Yürüt''ün'u seçin. Düğme turuncuya döner ve metin **Dur'a**dönüşür.

1. Sorgu tamamlandıktan sonra, **Sonuçlar** sekmesi işlemin sonuçlarını görüntüler. Aşağıdaki metin sorgunun sonucudur:

        loglevel       count
        [ERROR]        3

    İşin oluşturduğu günlük bilgilerini görüntülemek için **LOG** sekmesini kullanabilirsiniz.

   > [!TIP]  
   > **Sonuçlar** sekmesinin altındaki **Eylemler** açılır iletişim kutusundan sonuçları indirin veya kaydedin.

### <a name="visual-explain"></a>Görsel açıklama

Sorgu planının görselleştirilmesini görüntülemek için çalışma sayfasının altındaki **Görsel Açıklama** sekmesini seçin.

Sorgunun **Görsel Açıkla** görünümü karmaşık sorguların akışını anlamada yararlı olabilir.

### <a name="tez-ui"></a>Tez UI

Sorgu için Tez UI'sini görüntülemek için çalışma sayfasının altındaki **Tez UI** sekmesini seçin.

> [!IMPORTANT]  
> Tez tüm sorguları çözmek için kullanılmaz. Tez'i kullanmadan birçok sorguçözebilirsiniz.

## <a name="view-job-history"></a>İş geçmişini görüntüleme

__İşler__ sekmesi Hive sorgularının geçmişini görüntüler.

![Apache Hive görünüm işleri sekmesi geçmişi](./media/apache-hadoop-use-hive-ambari-view/apache-hive-job-history.png)

## <a name="database-tables"></a>Veritabanı tabloları

Hive veritabanındaki tablolarla çalışmak için __Tablolar__ sekmesini kullanabilirsiniz.

![Apache Hive tabloları sekmesinin görüntüsü](./media/apache-hadoop-use-hive-ambari-view/hdinsight-tables-tab.png)

## <a name="saved-queries"></a>Kaydedilen sorgular

**Sorgu** sekmesinden, isteğe bağlı olarak sorguları kaydedebilirsiniz. Bir sorguyu kaydettikten sonra, Kaydedilen __Sorgular__ sekmesinden yeniden kullanabilirsiniz.

![Apache Hive görünümleri sorguları sekmesini kaydetti](./media/apache-hadoop-use-hive-ambari-view/ambari-saved-queries.png)

> [!TIP]  
> Kaydedilen sorgular varsayılan küme depolama sında depolanır. Kaydedilen sorguları yolun `/user/<username>/hive/scripts`altında bulabilirsiniz. Bunlar düz metin `.hql` dosyaları olarak depolanır.
>
> Kümeyi siler, ancak depolama alanını korursanız, sorguları almak için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) veya Veri Gölü Depolama Gezgini [(Azure Portalından)](https://portal.azure.com)gibi bir yardımcı programı kullanabilirsiniz.

## <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Hive'ı kullanıcı tanımlı işlevler (UDF) aracılığıyla genişletebilirsiniz. HiveQL'de kolayca modellenmemiş işlevsellik veya mantığı uygulamak için bir UDF kullanın.

Kovan Görünümü'nün üst kısmındaki **UDF** sekmesini kullanarak bir UDF kümesini bildirin ve kaydedin. Bu UDF'ler Sorgu **Düzenleyicisi**ile kullanılabilir.

![Apache Hive görünüm UDFs sekmesi ekran](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

**Sorgu**Düzenleyicisi'nin alt kısmında **bir Ekle udfs** düğmesi görünür. Bu giriş, Kovan Görünümü'nde tanımlanan UDF'lerin açılır listesini görüntüler. UDF seçmek, UDF'yi etkinleştirmek için sorgunuza HiveQL deyimleri ekler.

Örneğin, aşağıdaki özelliklere sahip bir UDF tanımladıysanız:

* Kaynak adı: myudfs

* Kaynak yolu: /myudfs.jar

* UDF adı: myawesomeudf

* UDF sınıf adı: com.myudfs.Awesome

**Insert udfs** düğmesini kullanarak, bu kaynak için tanımlanan her UDF için başka bir açılır liste ile **myudfs**adlı bir giriş görüntüler. Bu durumda, bu **myawesomeudf**' s . Bu girişi seçmek, sorgunun başına aşağıdakileri ekler:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Daha sonra sorgunuzda UDF'yi kullanabilirsiniz. Örneğin, `SELECT myawesomeudf(name) FROM people;`.

HDInsight'ta Hive ile UDF kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight'ta Apache Hive ve Apache Pig ile Python kullanma](python-udf-hdinsight.md)
* [HDInsight'ta Apache Hive ile Java UDF kullanın](./apache-hadoop-hive-java-udf.md)

## <a name="hive-settings"></a>Kovan ayarları

Hive için yürütme motorlarını Tez'den (varsayılan) MapReduce olarak değiştirmek gibi çeşitli Hive ayarlarını değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight'ta Hive hakkında genel bilgi için:

* [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hdinsight-use-hive.md)

HDInsight'ta Hadoop ile çalışabileceğiniz diğer yollar hakkında bilgi için:

* [HDInsight'ta Apache Hadoop ile Apache Pig kullanın](hdinsight-use-pig.md)
* [HDInsight'ta Apache Hadoop ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)
