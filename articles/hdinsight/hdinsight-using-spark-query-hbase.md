---
title: HBase verilerini okumak ve yazmak için Spark kullanma-Azure HDInsight
description: Spark kümesindeki verileri bir HBase kümesine okumak ve yazmak için Spark HBase bağlayıcısını kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/24/2020
ms.openlocfilehash: 888f24e13ce67c878592068927383dd8cbfefa60
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77623097"
---
# <a name="use-apache-spark-to-read-and-write-apache-hbase-data"></a>Apache HBase verilerini okuyup yazmak için Apache Spark kullanma

Apache HBase genellikle alt düzey API (taramalar, alır ve koyar) ile veya Apache Phoenix kullanarak bir SQL söz dizimi ile sorgulanır. Apache ayrıca, HBase tarafından depolanan verileri sorgulamak ve değiştirmek için uygun ve iyi bir alternatif olan Apache Spark HBase bağlayıcısını de sağlar.

## <a name="prerequisites"></a>Önkoşullar

* Aynı [Sanal ağda](./hdinsight-plan-virtual-network-deployment.md)dağıtılan Iki ayrı HDInsight kümesi. Bir HBase ve en az Spark 2,1 (HDInsight 3,6) yüklü bir Spark. Daha fazla bilgi için bkz. [HDInsight 'ta Azure Portal kullanarak Linux tabanlı kümeler oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md).

* Bir SSH istemcisi. Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](hdinsight-hadoop-linux-use-ssh-unix.md).

* Kümelerinizin birincil depolama alanı için [URI şeması](hdinsight-hadoop-linux-information.md#URI-and-scheme) . Bu düzen Azure Blob depolama için wasb://, Azure Data Lake Storage 2. için abfs://veya adl://için Azure Data Lake Storage 1. olur. BLOB depolama için güvenli aktarım etkinse, URI `wasbs://`olur.  Ayrıca bkz. [Güvenli aktarım](../storage/common/storage-require-secure-transfer.md).

## <a name="overall-process"></a>Genel işlem

Spark kümenizi HDInsight kümenizi sorgulamak üzere etkinleştirmeye yönelik üst düzey işlem aşağıdaki gibidir:

1. Bazı örnek verileri HBase 'de hazırlayın.
2. HBase küme yapılandırma klasörünüzdeki HBase-site. xml dosyasını edinin (/etc/HBase/conf).
3. Spark 2 yapılandırma klasörünüze bir HBase-site. xml kopyası yerleştirin (/etc/mini 2/conf).
4. Spark HBase bağlayıcısından `packages` seçeneğinde Maven koordinatları ile başvuran `spark-shell` çalıştırın.
5. Spark ile HBase arasındaki şemayı eşleyen bir katalog tanımlayın.
6. RDD veya DataFrame API 'Lerini kullanarak HBase verileriyle etkileşim kurun.

## <a name="prepare-sample-data-in-apache-hbase"></a>Apache HBase 'de örnek verileri hazırlama

Bu adımda, Apache HBase 'de, daha sonra Spark kullanarak sorgulayabilmeniz için bir tablo oluşturup doldurursunuz.

1. HBase kümenize bağlanmak için `ssh` komutunu kullanın. `HBASECLUSTER` değerini HBase kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@HBASECLUSTER-ssh.azurehdinsight.net
    ```

2. HBase etkileşimli kabuğunu başlatmak için `hbase shell` komutunu kullanın. SSH bağlantınıza aşağıdaki komutu girin:

    ```bash
    hbase shell
    ```

3. İki sütunlu ailelerle bir HBase tablosu oluşturmak için `create` komutunu kullanın. Aşağıdaki komutu girin:

    ```hbase
    create 'Contacts', 'Personal', 'Office'
    ```

4. Belirli bir tablodaki belirtilen bir sütundaki değerleri eklemek için `put` komutunu kullanın. Aşağıdaki komutu girin:

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

5. HBase etkileşimli kabuğunu durdurmak için `exit` komutunu kullanın. Aşağıdaki komutu girin:

    ```hbase
    exit
    ```

## <a name="copy-hbase-sitexml-to-spark-cluster"></a>HBase-site. xml ' i Spark kümesine Kopyala

HBase-site. xml ' i yerel depolamadan Spark kümenizin varsayılan depolama alanının köküne kopyalayın.  Yapılandırmanızı yansıtmak için aşağıdaki komutu düzenleyin.  Ardından, açık SSH oturumunından HBase kümesine şu komutu girin:

| Söz dizimi değeri | Yeni değer|
|---|---|
|[URI düzeni](hdinsight-hadoop-linux-information.md#URI-and-scheme) | Depolama alanınızı yansıtacak şekilde değiştirin.  Aşağıdaki sözdizimi, güvenli aktarım özellikli blob depolamaya yöneliktir.|
|`SPARK_STORAGE_CONTAINER`|Spark kümesi için kullanılan varsayılan depolama kapsayıcısı adıyla değiştirin.|
|`SPARK_STORAGE_ACCOUNT`|Spark kümesi için kullanılan varsayılan depolama hesabı adıyla değiştirin.|

```bash
hdfs dfs -copyFromLocal /etc/hbase/conf/hbase-site.xml wasbs://SPARK_STORAGE_CONTAINER@SPARK_STORAGE_ACCOUNT.blob.core.windows.net/
```

Ardından, SSH bağlantınızı HBase kümenize çıkın.

```bash
exit
```

## <a name="put-hbase-sitexml-on-your-spark-cluster"></a>HBase-site. xml ' i Spark kümenize yerleştirme

1. SSH kullanarak Spark kümenizin baş düğümüne bağlanın. `SPARKCLUSTER` Spark kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@SPARKCLUSTER-ssh.azurehdinsight.net
    ```

2. Spark kümenizin varsayılan depolamadan `hbase-site.xml` kümenin yerel depolama alanındaki Spark 2 yapılandırma klasörüne kopyalamak için aşağıdaki komutu girin:

    ```bash
    sudo hdfs dfs -copyToLocal /hbase-site.xml /etc/spark2/conf
    ```

## <a name="run-spark-shell-referencing-the-spark-hbase-connector"></a>Spark HBase bağlayıcısına başvuran Spark kabuğunu Çalıştır

1. Açık SSH oturumunuzla Spark kümesine, bir Spark kabuğu başlatmak için aşağıdaki komutu girin:

    ```bash
    spark-shell --packages com.hortonworks:shc-core:1.1.1-2.1-s_2.11 --repositories https://repo.hortonworks.com/content/groups/public/
    ```  

2. Bu Spark kabuğu örneğini açık tutun ve sonraki adıma geçin.

## <a name="define-a-catalog-and-query"></a>Katalog ve sorgu tanımlama

Bu adımda, Apache Spark şemayı Apache HBase ile eşleyen bir katalog nesnesi tanımlarsınız.  

1. Açık Spark kabuğunuzun içinde aşağıdaki `import` deyimlerini girin:

    ```scala
    import org.apache.spark.sql.{SQLContext, _}
    import org.apache.spark.sql.execution.datasources.hbase._
    import org.apache.spark.{SparkConf, SparkContext}
    import spark.sqlContext.implicits._
    ```  

1. HBase 'de oluşturduğunuz kişiler tablosu için bir katalog tanımlamak üzere aşağıdaki komutu girin:

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

    Kod şunları yapar:  

     a. `Contacts`adlı HBase tablosu için bir katalog şeması tanımlayın.  
     b. Rowkey 'i `key`olarak ve Spark içinde kullanılan sütun adlarını HBase 'de kullanılan sütun ailesi, sütun adı ve sütun türüyle eşleyin.  
     c. Rowkey Ayrıca `rowkey`belirli bir sütun ailesi `cf` bulunan adlandırılmış bir sütun (`rowkey`) olarak ayrıntılı olarak tanımlanmalıdır.  

1. HBase 'de `Contacts` tablonuz etrafında bir veri çerçevesi sağlayan bir yöntemi tanımlamak için aşağıdaki komutu girin:

    ```scala
    def withCatalog(cat: String): DataFrame = {
        spark.sqlContext
        .read
        .options(Map(HBaseTableCatalog.tableCatalog->cat))
        .format("org.apache.spark.sql.execution.datasources.hbase")
        .load()
     }
    ```

1. Veri çerçevesinin bir örneğini oluşturun:

    ```scala
    val df = withCatalog(catalog)
    ```  

1. Veri çerçevesini sorgulama:

    ```scala
    df.show()
    ```

    İki satırlık veri görmeniz gerekir:

    ```output
    +------+--------------------+--------------+-------------+--------------+
    |rowkey|       officeAddress|   officePhone| personalName| personalPhone|
    +------+--------------------+--------------+-------------+--------------+
    |  1000|1111 San Gabriel Dr.|1-425-000-0002|    John Dole|1-425-000-0001|
    |  8396|5415 San Gabriel Dr.|  230-555-0191|  Calvin Raji|  230-555-0191|
    +------+--------------------+--------------+-------------+--------------+
    ```

1. Spark SQL kullanarak HBase tablosunu sorgulayabilmeniz için geçici bir tablo kaydedin:

    ```scala
    df.createTempView("contacts")
    ```

1. `contacts` tabloda SQL sorgusu verme:

    ```scala
    spark.sqlContext.sql("select personalName, officeAddress from contacts").show
    ```

    Şöyle bir sonuç görmeniz gerekir:

    ```output
    +-------------+--------------------+
    | personalName|       officeAddress|
    +-------------+--------------------+
    |    John Dole|1111 San Gabriel Dr.|
    |  Calvin Raji|5415 San Gabriel Dr.|
    +-------------+--------------------+
    ```

## <a name="insert-new-data"></a>Yeni veri Ekle

1. Yeni bir kişi kaydı eklemek için `ContactRecord` bir sınıf tanımlayın:

    ```scala
    case class ContactRecord(
        rowkey: String,
        officeAddress: String,
        officePhone: String,
        personalName: String,
        personalPhone: String
        )
    ```

1. Bir `ContactRecord` örneği oluşturun ve bir diziye koyun:

    ```scala
    val newContact = ContactRecord("16891", "40 Ellis St.", "674-555-0110", "John Jackson","230-555-0194")

    var newData = new Array[ContactRecord](1)
    newData(0) = newContact
    ```

1. Yeni veri dizisini HBase 'e Kaydet:

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

1. Aşağıdaki komutu girerek Spark kabuğunu kapatın:

    ```scala
    :q
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark HBase Bağlayıcısı](https://github.com/hortonworks-spark/shc)
