---
title: HBase verilerini okumak ve yazmak için Spark'ı kullanın - Azure HDInsight
description: Bir Spark kümesinden HBase kümesine veri okumak ve yazmak için Spark HBase Bağlayıcısı'nı kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/20/2020
ms.openlocfilehash: 4f2e8b2a691a6b17b5ed075745d556db4e330535
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682475"
---
# <a name="use-apache-spark-to-read-and-write-apache-hbase-data"></a>Apache HBase verilerini okuyup yazmak için Apache Spark kullanma

Apache HBase genellikle düşük düzeyLI API'si (tarar, alır ve koyar) veya Apache Phoenix kullanarak bir SQL sözdizimi ile sorgulanır. Apache ayrıca Apache Spark HBase Konektörü sağlar. Bağlayıcı, HBase tarafından depolanan verileri sorgulamak ve değiştirmek için kullanışlı ve performanslı bir alternatiftir.

## <a name="prerequisites"></a>Ön koşullar

* Aynı [sanal ağda](./hdinsight-plan-virtual-network-deployment.md)dağıtılan iki ayrı HDInsight kümesi. Bir HBase ve en az Spark 2.1 (HDInsight 3.6) yüklü bir Kıvılcım. Daha fazla bilgi için, [Azure portalını kullanarak HDInsight'ta Linux tabanlı kümeler oluştur'a](hdinsight-hadoop-create-linux-clusters-portal.md)bakın.

* Kümeleriniz birincil depolama için URI düzeni. Bu şema Azure Blob Depolama, `abfs://` Azure Veri Gölü Depolama Gen2 veya Azure Veri Gölü Depolama Gen1 için adl:// için wasb:// olacaktır. Blob Depolama için güvenli aktarım etkinleştirilirse, URI `wasbs://`.  Ayrıca bakınız, [güvenli aktarım.](../storage/common/storage-require-secure-transfer.md)

## <a name="overall-process"></a>Genel süreç

Spark kümenizin HDInsight kümenizi sorgulamasını sağlamak için üst düzey işlem aşağıdaki gibidir:

1. HBase'de bazı örnek veriler hazırlayın.
2. HBase küme yapılandırma klasörünüzden (/etc/hbase/conf) hbase-site.xml dosyasını edinin.
3. Spark 2 yapılandırma klasörünüze (/etc/spark2/conf) hbase-site.xml'in bir kopyasını yerleştirin.
4. Spark HBase Konektörünü opsiyondaki Maven koordinatlarına `packages` göre çalıştırın. `spark-shell`
5. Spark'tan HBase'e şemayı eşleyen bir katalog tanımlayın.
6. RDD veya DataFrame API'lerini kullanarak HBase verileriyle etkileşim kurun.

## <a name="prepare-sample-data-in-apache-hbase"></a>Apache HBase'de örnek verileri hazırlama

Bu adımda, Apache HBase'de spark kullanarak sorgulayabildiğiniz bir tablo oluşturur ve doldurursunuz.

1. HBase `ssh` kümenize bağlanmak için komutu kullanın. HBase kümenizin adını `HBASECLUSTER` değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@HBASECLUSTER-ssh.azurehdinsight.net
    ```

2. HBase `hbase shell` etkileşimli kabuğunu başlatmak için komutu kullanın. SSH bağlantınızda aşağıdaki komutu girin:

    ```bash
    hbase shell
    ```

3. İki `create` sütunlu ailelerden bir HBase tablosu oluşturmak için komutu kullanın. Aşağıdaki komutu girin:

    ```hbase
    create 'Contacts', 'Personal', 'Office'
    ```

4. Belirli `put` bir tabloda belirli bir satırda belirli bir sütunda değerleri eklemek için komutu kullanın. Aşağıdaki komutu girin:

    ```hbase
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    put 'Contacts', '8396', 'Personal:Name', 'Calvin Raji'
    put 'Contacts', '8396', 'Personal:Phone', '230-555-0191'
    put 'Contacts', '8396', 'Office:Phone', '230-555-0191'
    put 'Contacts', '8396', 'Office:Address', '5415 San Gabriel Dr.'
    ```

5. HBase `exit` etkileşimli kabuğunu durdurmak için komutu kullanın. Aşağıdaki komutu girin:

    ```hbase
    exit
    ```

## <a name="copy-hbase-sitexml-to-spark-cluster"></a>Hbase-site.xml'i Kıvılcım kümesine kopyala

Yerel depolamadan Spark kümenizin varsayılan depolama sının köküne kadar hbase-site.xml'i kopyalayın.  Yapılandırmanızı yansıtacak şekilde aşağıdaki komutu düzenleme.  Daha sonra, açık SSH oturumunuzdan HBase kümesine, komutu girin:

| Sözdizimi değeri | Yeni değer|
|---|---|
|[URI şeması](hdinsight-hadoop-linux-information.md#URI-and-scheme) | Depolama alanınızı yansıtacak şekilde değiştirin.  Aşağıdaki sözdizimi, güvenli aktarım etkin blob depolama içindir.|
|`SPARK_STORAGE_CONTAINER`|Kıvılcım kümesi için kullanılan varsayılan depolama kapsayıcısı adı ile değiştirin.|
|`SPARK_STORAGE_ACCOUNT`|Kıvılcım kümesi için kullanılan varsayılan depolama hesabı adı ile değiştirin.|

```bash
hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml wasbs://SPARK_STORAGE_CONTAINER@SPARK_STORAGE_ACCOUNT.blob.core.windows.net/
```

Ardından HBase kümenize ssh bağlantınızdan çıkın.

```bash
exit
```

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>Kıvılcım kümenize hbase-site.xml koyun

1. SSH kullanarak Kıvılcım kümenizin baş düğümüne bağlanın. Aşağıdaki komutu Kıvılcım kümenizin adıyla değiştirerek `SPARKCLUSTER` düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@SPARKCLUSTER-ssh.azurehdinsight.net
    ```

2. Spark kümenizin `hbase-site.xml` varsayılan depolamasından kümenin yerel depolama sındaki Spark 2 yapılandırma klasörüne kopyalamak için aşağıdaki komutu girin:

    ```bash
    sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf
    ```

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Kıvılcım HBase Konektörünü referans alan Kıvılcım Kabuğunu çalıştırın

1. Açık SSH oturumunuzdan Kıvılcım kümesine, kıvılcım kabuğu başlatmak için aşağıdaki komutu girin:

    ```bash
    spark-shell --packages com.hortonworks:shc-core:1.1.1-2.1-s_2.11 --repositories https://repo.hortonworks.com/content/groups/public/
    ```  

2. Bu Kıvılcım Kabuğu örneğini açık tutun ve bir sonraki adıma devam edin.

## <a name="define-a-catalog-and-query"></a>Katalog ve Sorgu Tanımlama

Bu adımda, Apache Spark'tan Apache HBase'e şemayı eşleyen bir katalog nesnesi tanımlarsınız.  

1. Açık Kıvılcım Kabuğunuzda aşağıdaki `import` ifadeleri girin:

    ```scala
    import org.apache.spark.sql.{SQLContext, _}
    import org.apache.spark.sql.execution.datasources.hbase._
    import org.apache.spark.{SparkConf, SparkContext}
    import spark.sqlContext.implicits._
    ```  

1. HBase'te oluşturduğunuz Kişiler tablosu için bir katalog tanımlamak için aşağıdaki komutu girin:

    ```scala
    def catalog = s"""{
        |"table":{"namespace":"default", "name":"Contacts"},
        |"rowkey":"key",
        |"columns":{
        |"rowkey":{"cf":"rowkey", "col":"key", "type":"string"},
        |"officeAddress":{"cf":"Office", "col":"Address", "type":"string"},
        |"officePhone":{"cf":"Office", "col":"Phone", "type":"string"},
        |"personalName":{"cf":"Personal", "col":"Name", "type":"string"},
        |"personalPhone":{"cf":"Personal", "col":"Phone", "type":"string"}
        |}
    |}""".stripMargin
    ```

    Kod aşağıdaki eylemleri yapar:  

     a. HBase tablosu adlı bir katalog şeması `Contacts`tanımlayın.  
     b. Satır anahtarını `key`, olarak tanımlayın ve Spark'ta kullanılan sütun adlarını HBase'de kullanılan sütun ailesi, sütun adı ve sütun türüyle eşleştirin.  
     c. Rowkey de ayrıntılı olarak adlandırılmış bir sütun`rowkey`olarak tanımlanmalıdır ( `cf` `rowkey`), hangi belirli bir sütun ailesi vardır .  

1. HBase tablonuzun `Contacts` etrafında Bir DataFrame sağlayan bir yöntem tanımlamak için aşağıdaki komutu girin:

    ```scala
    def withCatalog(cat: String): DataFrame = {
        spark.sqlContext
        .read
        .options(Map(HBaseTableCatalog.tableCatalog->cat))
        .format("org.apache.spark.sql.execution.datasources.hbase")
        .load()
     }
    ```

1. DataFrame'in bir örneğini oluşturun:

    ```scala
    val df = withCatalog(catalog)
    ```  

1. DataFrame'i sorgula:

    ```scala
    df.show()
    ```

    İki veri satırı görmeniz gerekir:

    ```output
    +------+--------------------+--------------+-------------+--------------+
    |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
    +------+--------------------+--------------+-------------+--------------+
    |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
    |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
    +------+--------------------+--------------+-------------+--------------+
    ```

1. Spark SQL kullanarak HBase tablosunu sorgulayabilmek için geçici bir tablo kaydedin:

    ```scala
    df.createTempView("contacts")
    ```

1. Tabloya karşı bir `contacts` SQL sorgusu sorunu:

    ```scala
    spark.sqlContext.sql("select personalName, officeAddress from contacts").show
    ```

    Bu gibi sonuçlar görmelisiniz:

    ```output
    +-------------+--------------------+
    | personalName|       officeAddress|
    +-------------+--------------------+
    |    John Dole|1111 San Gabriel Dr.|
    |  Calvin Raji|5415 San Gabriel Dr.|
    +-------------+--------------------+
    ```

## <a name="insert-new-data"></a>Yeni veri ekleme

1. Yeni bir Kişi kaydı eklemek `ContactRecord` için bir sınıf tanımlayın:

    ```scala
    case class ContactRecord(
        rowkey: String,
        officeAddress: String,
        officePhone: String,
        personalName: String,
        personalPhone: String
        )
    ```

1. Bir örnek `ContactRecord` oluşturun ve bir dizi koyun:

    ```scala
    val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

    var newData = new Array[ContactRecord](1)
    newData(0) = newContact
    ```

1. Yeni veri dizisini HBase'e kaydedin:

    ```scala
    sc.parallelize(newData).toDF.write.options(Map(HBaseTableCatalog.tableCatalog -> catalog, HBaseTableCatalog.newTable -> "5")).format("org.apache.spark.sql.execution.datasources.hbase").save()
    ```

1. Sonuçları inceleyin:

    ```scala  
    df.show()
    ```

    Şunun gibi bir çıktı görmeniz gerekir:

    ```output
    +------+--------------------+--------------+------------+--------------+
    |rowkey|       officeAddress|   officePhone|personalName| personalPhone|
    +------+--------------------+--------------+------------+--------------+
    |  1000|1111 San Gabriel Dr.|1-425-000-0002|   John Dole|1-425-000-0001|
    | 16891|        40 Ellis St.|  674-555-0110|John Jackson|  230-555-0194|
    |  8396|5415 San Gabriel Dr.|  230-555-0191| Calvin Raji|  230-555-0191|
    +------+--------------------+--------------+------------+--------------+
    ```

1. Aşağıdaki komutu girerek kıvılcım kabuğunu kapatın:

    ```scala
    :q
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Kıvılcım HBase Konektörü](https://github.com/hortonworks-spark/shc)
