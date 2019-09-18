---
title: HDInsight 'ta Hive ile çalışmak için Apache ambarı Görünümlerini kullanma (Apache Hadoop)-Azure
description: Hive sorguları göndermek için Web tarayıcınızdan Hive görünümünü nasıl kullanacağınızı öğrenin. Hive görünümü, Linux tabanlı HDInsight kümemenizle birlikte sunulan ambarı Web Kullanıcı arabiriminin bir parçasıdır.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: hrasheed
ms.openlocfilehash: da4d1ed7dec8b3b0bc61dd2959a868d03875039c
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71077006"
---
# <a name="use-apache-ambari-hive-view-with-apache-hadoop-in-hdinsight"></a>HDInsight 'ta Apache Hadoop Apache ambarı Hive görünümünü kullanma

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Apache ambarı Hive görünümünü kullanarak Hive sorgularını çalıştırmayı öğrenin. Hive görünümü, Web tarayıcınızdan Hive sorgularını yazmanıza, iyileştirmenize ve çalıştırmanıza olanak tanır.

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Hadoop kümesi. Bkz. [Linux 'Ta HDInsight kullanmaya başlama](./apache-hadoop-linux-tutorial-get-started.md).
* Bir Web tarayıcısı

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

1. [Azure Portal](https://portal.azure.com/), kümenizi seçin.  Yönergeler için bkz. [liste ve kümeleri gösterme](../hdinsight-administer-use-portal-linux.md#showClusters) . Küme, yeni bir portal dikey penceresinde açılır.

2. **Küme panolarında**, **ambarı görünümleri**' ni seçin. Kimlik doğrulaması istendiğinde, kümeyi oluştururken belirttiğiniz küme oturum açma ( `admin`varsayılan) hesap adını ve parolasını kullanın.

3. Görünümler listesinden __Hive görünümü__' nü seçin.

    ![Apache ambarı Apache Hive görünüm seç](./media/apache-hadoop-use-hive-ambari-view/select-apache-hive-view.png)

    Hive görünümü sayfası aşağıdaki görüntüye benzer:

    ![Hive görünümü için sorgu çalışma sayfasının görüntüsü](./media/apache-hadoop-use-hive-ambari-view/ambari-worksheet-view.png)

4. __Sorgu__ sekmesinden aşağıdaki HiveQL deyimlerini çalışma sayfasına yapıştırın:

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

    Bu deyimler aşağıdaki eylemleri gerçekleştirir:

   * `DROP TABLE`: Tablonun zaten mevcut olması durumunda tabloyu ve veri dosyasını siler.

   * `CREATE EXTERNAL TABLE`: Hive içinde yeni bir "External" tablosu oluşturur.
     Dış tablolar yalnızca Hive içindeki tablo tanımını depolar. Veriler özgün konumda bırakılır.

   * `ROW FORMAT`: Verilerin nasıl biçimlendirileceğini gösterir. Bu durumda, her günlükteki alanlar boşlukla ayrılır.

   * `STORED AS TEXTFILE LOCATION`: Verilerin nerede depolandığını ve metin olarak depolandığını gösterir.

   * `SELECT`: T4 sütununun [ERROR] değerini içerdiği tüm satırların sayısını seçer.

   > [!IMPORTANT]  
   > __Veritabanı__ seçimini __varsayılan__olarak bırakın. Bu belgedeki örnekler, HDInsight 'ta bulunan varsayılan veritabanını kullanır.

5. Sorguyu başlatmak için çalışma sayfasının altında **Yürüt** ' ü seçin. Düğme turuncu döner ve metin **durur**.

6. Sorgu bittikten sonra **sonuçlar** sekmesi işlemin sonuçlarını görüntüler. Aşağıdaki metin sorgunun sonucudur:

        loglevel       count
        [ERROR]        3

    İşin oluşturduğu günlük bilgilerini görüntülemek için **günlük** sekmesini kullanabilirsiniz.

   > [!TIP]  
   > **Sonuçlar sekmesinin altındaki** **Eylemler** açılan iletişim kutusundan sonuçları indirin veya kaydedin.

### <a name="visual-explain"></a>Görsel açıklama

Sorgu planının bir görselleştirmesini göstermek için, çalışma sayfasının altındaki **görsel açıklama** sekmesini seçin.

Sorgunun **görsel açıklama** görünümü karmaşık sorguların akışını anlamak için yararlı olabilir.

### <a name="tez-ui"></a>Tez Kullanıcı arabirimi

Sorgu için tez Kullanıcı arabirimini göstermek için, çalışma sayfasının altındaki **tez Kullanıcı arabirimi** sekmesini seçin.

> [!IMPORTANT]  
> Tez tüm sorguları çözümlemek için kullanılmaz. Tez kullanmadan birçok sorguyu çözebilirsiniz.

## <a name="view-job-history"></a>İş geçmişini görüntüleme

__İşler__ sekmesi Hive sorgularının geçmişini görüntüler.

![Apache Hive işleri sekme geçmişini görüntüleme](./media/apache-hadoop-use-hive-ambari-view/apache-hive-job-history.png)

## <a name="database-tables"></a>Veritabanı tabloları

Hive veritabanı içindeki tablolarla çalışmak için __Tablolar__ sekmesini kullanabilirsiniz.

![Apache Hive tabloları sekmesinin görüntüsü](./media/apache-hadoop-use-hive-ambari-view/hdinsight-tables-tab.png)

## <a name="saved-queries"></a>Kaydedilmiş sorgular

**Sorgu** sekmesinden, isteğe bağlı olarak sorguları kaydedebilirsiniz. Bir sorguyu kaydettikten sonra, __kaydedilmiş sorgular__ sekmesinden onu yeniden kullanabilirsiniz.

![Apache Hive kaydedilmiş sorgular sekmesini görüntüleme](./media/apache-hadoop-use-hive-ambari-view/ambari-saved-queries.png)

> [!TIP]  
> Kayıtlı sorgular varsayılan küme depolama alanında depolanır. Kaydedilen sorguları yolun `/user/<username>/hive/scripts`altında bulabilirsiniz. Bunlar düz metin `.hql` dosyaları olarak depolanır.
>
> Kümeyi siler, ancak depolamayı tutarsanız, sorguları almak için [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) veya Data Lake Storage Explorer ( [Azure portalından](https://portal.azure.com)) gibi bir yardımcı programı kullanabilirsiniz.

## <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Kullanıcı tanımlı işlevler (UDF) aracılığıyla Hive 'yi genişletebilirsiniz. HiveQL içinde kolayca Modellenmemiş işlevselliği veya mantığı uygulamak için UDF kullanın.

Hive görünümünün en üstündeki **udf** sekmesini kullanarak bir UDF kümesi bildirin ve kaydedin. Bu UDF 'ler **sorgu Düzenleyicisi**ile kullanılabilir.

![Apache Hive görünüm UDF sekmesi görüntüleme](./media/apache-hadoop-use-hive-ambari-view/user-defined-functions.png)

Hive görünümüne bir UDF ekledikten sonra **sorgu Düzenleyicisi**'nin altında bir UDF **Ekle** düğmesi görünür. Bu girdiyi seçtiğinizde, Hive görünümünde tanımlanan UDF 'ler açılır listesi görüntülenir. UDF 'nin seçilmesi, UDF 'yi etkinleştirmek için sorgulamanızı HiveQL deyimleri ekler.

Örneğin, aşağıdaki özelliklerle bir UDF tanımladıysanız:

* Kaynak adı: myudfs

* Kaynak yolu:/MyUDF.exe

* UDF adı: myawesomeudf

* UDF sınıf adı: com. myudfs. başar

**Udf 'Leri Ekle** düğmesinin kullanılması, söz konusu kaynak için tanımlanan her UDF için başka bir açılan liste ile **myudfs**adlı bir giriş görüntüler. Bu durumda, **myawesomeudf**'dir. Bu girdiyi seçtiğinizde sorgunun başına aşağıdakiler eklenir:

```hiveql
add jar /myudfs.jar;
create temporary function myawesomeudf as 'com.myudfs.Awesome';
```

Daha sonra sorginizdeki UDF 'i kullanabilirsiniz. Örneğin: `SELECT myawesomeudf(name) FROM people;`.

HDInsight 'ta Hive ile UDF 'Leri kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [HDInsight 'ta Apache Hive ve Apache Pig ile Python kullanma](python-udf-hdinsight.md)
* [HDInsight 'a özel Apache Hive UDF ekleme](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

## <a name="hive-settings"></a>Hive ayarları

Hive için yürütme altyapısını tez 'den (varsayılan) MapReduce 'ye değiştirme gibi çeşitli Hive ayarlarını değiştirebilirsiniz.

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight 'ta Hive hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive kullanma](hdinsight-use-hive.md)

HDInsight 'ta Hadoop ile birlikte çalışmak için kullanabileceğiniz diğer yollar hakkında daha fazla bilgi için:

* [HDInsight üzerinde Apache Hadoop Apache Pig kullanma](hdinsight-use-pig.md)
* [HDInsight üzerinde Apache Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md)
