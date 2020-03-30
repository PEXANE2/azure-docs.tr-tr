---
title: Apache Spark & Kovan - Kovan Depo Konektörü - Azure HDInsight
description: Azure HDInsight'ta Apache Spark ve Apache Hive'ı Hive Warehouse Konektörüyle nasıl entegre edeceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/02/2020
ms.openlocfilehash: f386530ffb3a074a5c1db1d9f28535d28c8b1284
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78252416"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Apache Spark ve Apache Hive'ı Hive Depo Konektörüyle bütünleştirin

Apache Hive Warehouse Connector (HWC), Spark DataFrame'ler ve Hive tabloları arasında veri taşıma gibi görevleri destekleyerek ve Ayrıca Spark akış verilerini Hive tablolarına yönlendirerek Apache Spark ve Apache Hive ile daha kolay çalışmanızı sağlayan bir kitaplıktır. Kovan Depo Konektörü Kıvılcım ve Hive arasında bir köprü gibi çalışır. Geliştirme için Scala, Java ve Python'u destekler.

Kovan Ambarı Konektörü, güçlü büyük veri uygulamaları oluşturmak için Hive ve Spark'ın benzersiz özelliklerinden yararlanmanızı sağlar. Apache Hive Atomik, Tutarlı, İzole ve Dayanıklı (ACID) veritabanı işlemleri için destek sunar. Kovan'daki ASİt ve işlemler hakkında daha fazla bilgi için [Kovan İşlemleri'ne](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions)bakın. Kovan ayrıca Apache Ranger ve Düşük Gecikmeli Analitik İşleme apache Spark mevcut olmayan aracılığıyla ayrıntılı güvenlik kontrolleri sunuyor.

Apache Spark, Apache Hive'da bulunmayan akış özellikleri sağlayan yapılandırılmış bir Akış API'si vardır. HDInsight 4.0 ile başlayan Apache Spark 2.3.1 ve Apache Hive 3.1.0 birlikte çalışabilirliği zorlaştıran ayrı meta mağazalara sahiptir. Kovan Ambarı Konektörü, Kıvılcım ve Kovan'ı birlikte kullanmayı kolaylaştırır. HWC kitaplığı LLAP daemons gelen verileri paralel olarak Spark uygulayıcıları yükler, daha verimli ve ölçeklenebilir Spark hive standart bir JDBC bağlantısı kullanarak daha yapma.

![kovan depo konektör mimarisi](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Hive Ambarı Bağlayıcısı tarafından desteklenen işlemlerden bazıları şunlardır:

* Tabloyu açıklama
* ORC biçimlendirilmiş veriler için tablo oluşturma
* Kovan verilerini seçme ve DataFrame alma
* Toplu olarak Kovan'a DataFrame Yazma
* Hive güncelleştirme deyimini yürütme
* Hive'dan gelen tablo verilerini okuma, Kıvılcım'da dönüştürme ve yeni bir Hive tablosuna yazma
* HiveStreaming kullanarak Kovana DataFrame veya Spark akışı yazma

## <a name="hive-warehouse-connector-setup"></a>Kovan Depo Konektörü kurulumu

Azure HDInsight'ta Bir Kıvılcım ve Etkileşimli Sorgu kümesi arasında Kovan Ambarı Bağlayıcısı'nı ayarlamak için aşağıdaki adımları izleyin:

### <a name="create-clusters"></a>Küme oluşturma

1. Bir depolama hesabı ve özel bir Azure sanal ağı yla bir HDInsight Spark **4.0** kümesi oluşturun. Azure sanal ağında küme oluşturma hakkında bilgi için, [varolan bir sanal ağa HDInsight ekle'ye](../../hdinsight/hdinsight-plan-virtual-network-deployment.md#existingvnet)bakın.

1. Spark kümesiyle aynı depolama hesabı ve Azure sanal ağı olan bir HDInsight Interactive Query (LLAP) **4.0** kümesi oluşturun.

### <a name="modify-hosts-file"></a>Ana bilgisayar dosyasini değiştirme

İnteraktif Sorgu kümenizin `/etc/hosts` headnode0'ündeki dosyadaki düğüm bilgilerini kopyalayın ve bilgileri Kıvılcım kümenizin headnode0'undaki `/etc/hosts` dosyaya yorumlayın. Bu adım, Spark kümenizin Etkileşimli Sorgu kümesindeki düğümlerin IP adreslerini çözmesine olanak sağlar. Güncelleştirilmiş dosyanın içeriğini `cat /etc/hosts`' le görüntüleyin. Son çıktı aşağıdaki ekran görüntüsünde gösterilen gibi bir şey görünmelidir.

![kovan ambar konektörü hosts dosya](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

### <a name="gather-preliminary-information"></a>Ön bilgileri toplama

#### <a name="from-your-interactive-query-cluster"></a>Etkileşimli Sorgu kümenizden

1. İnteraktif Sorgu kümenizin adını kullanarak `https://LLAPCLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/configs` `LLAPCLUSTERNAME` kümenin Apache Ambari Hive sayfasına gidin.

1. **Advanced** > **General** > **kovan.metastore.uris'e** gidin ve değeri not edin. Değer benzer olabilir: `thrift://iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:9083,thrift://hn1-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:9083`.

1. Gelişmiş **Gelişmiş** > **kovan sitesi** > **kovan.zookeeper.quorum** gidin ve değeri not. Değer benzer olabilir: `zk0-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181,zk1-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181,zk4-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181`.

#### <a name="from-your-apache-spark-cluster"></a>Apache Spark kümenizden

1. Apache Spark kümenizin adını kullanarak `https://SPARKCLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/configs` `SPARKCLUSTERNAME` kümenin Apache Ambari Hive sayfasına gidin.

1. Gelişmiş **Gelişmiş** > **kovan-interaktif-site** > **kovan.llap.daemon.service.hosts** gidin ve değeri not. Değer benzer olabilir: `@llap0`.

### <a name="configure-spark-cluster-settings"></a>Kıvılcım küme ayarlarını yapılandırma

Spark Ambari web Kullanıcı Bira'nızdan, **Spark2** > **CONFIGS** > **Özel kıvılcım2-varsayılanlara**gidin.

![Apache Ambari Spark2 yapılandırması](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

Aşağıdakileri eklemek/güncellemek için **Özellik Ekle'yi seçin...**

| Anahtar | Değer |
|----|----|
|`spark.hadoop.hive.llap.daemon.service.hosts`|Daha önce **kovan.llap.daemon.service.hosts**elde edilen değer .|
|`spark.sql.hive.hiveserver2.jdbc.url`|`jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2`. Etkileşimli Sorgu kümesinde Hiveserver2'ye bağlanan JDBC bağlantı dizesine ayarlayın. Etkileşimli Sorgu kümenizin adıyla DEĞIŞTIRin. `LLAPCLUSTERNAME` Gerçek `PWD` parolayla değiştirin.|
|`spark.datasource.hive.warehouse.load.staging.dir`|`wasbs://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp`. Uygun bir HDFS uyumlu evreleme dizinine ayarlayın. İki farklı kümeniz varsa, evreleme dizini, HiveServer2'nin bu kümeye erişebilmeleri için LLAP kümesinin depolama hesabının evreleme dizininde bir klasör olmalıdır.  Küme `STORAGE_ACCOUNT_NAME` tarafından kullanılan depolama hesabının adı ve `STORAGE_CONTAINER_NAME` depolama kapsayıcısının adı ile değiştirin.|
|`spark.datasource.hive.warehouse.metastoreUri`|**Kovan.metastore.uris'ten**daha önce elde ettiğiniz değer.|
|`spark.security.credentials.hiveserver2.enabled`|`false`YARN istemci dağıtma modu için.|
|`spark.hadoop.hive.zookeeper.quorum`|Daha önce **kovan.zookeeper.quorum**elde edilen değer .|

Değişiklikleri kaydedin ve gerektiğinde bileşenleri yeniden başlatın.

## <a name="using-the-hive-warehouse-connector"></a>Kovan Ambarı Konektörünü Kullanma

### <a name="connecting-and-running-queries"></a>Sorguları bağlama ve çalıştırma

İnteraktif Sorgu kümenize bağlanmak ve Kovan Ambarı Bağlayıcısı'nı kullanarak sorguları yürütmek için birkaç farklı yöntem arasında seçim yapabilirsiniz. Desteklenen yöntemler aşağıdaki araçları içerir:

* [kıvılcım kabuğu](../spark/apache-spark-shell.md)
* PySpark
* kıvılcım gönder
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Bu makalede sağlanan tüm örnekler kıvılcım kabuğu üzerinden yürütülecektir.

Bir kıvılcım kabuğu oturumu başlatmak için aşağıdaki adımları yapın:

1. Apache Spark kümeniz için headnode içine SSH. SSH ile kümenize bağlanma hakkında daha fazla bilgi için, [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

1. Kıvılcım kabuğunu başlatmak için aşağıdaki komutu girin:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<STACK_VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

    Bir karşılama iletisi `scala>` ve komutları girebileceğiniz bir istem görürsünüz.

1. Kıvılcım kabuğunu çalıştırdıktan sonra, aşağıdaki komutlar kullanılarak bir Kovan Ambarı Bağlayıcısı örneği başlatılabilir:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Kurumsal Güvenlik Paketi (ESP) kümelerinde sorguları bağlama ve çalıştırma

Kurumsal Güvenlik Paketi (ESP), Azure HDInsight'ta Active Directory tabanlı kimlik doğrulama, çok kullanıcılı destek ve Apache Hadoop kümeleri için rol tabanlı erişim denetimi gibi kurumsal dereceli özellikler sağlar. ESP hakkında daha fazla bilgi için [HDInsight'ta Kurumsal Güvenlik Paketini Kullan'a](../domain-joined/apache-domain-joined-architecture.md)bakın.

1. Apache Spark kümeniz için headnode içine SSH. SSH ile kümenize bağlanma hakkında daha fazla bilgi için, [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

1. Bir `kinit` etki alanı kullanıcısıyla yazın ve giriş yapın.

1. Kıvılcım kabuğunu, aşağıda gösterildiği gibi yapılandırma parametrelerinin tam listesiyle başlatın. Açı braketleri arasındaki tüm büyük harflerdeki tüm değerler kümenize göre belirtilmelidir. Aşağıdaki parametrelerden herhangi biri için giriş yapmak için gereken değerleri bulmanız gerekiyorsa, [Kovan Ambarı Bağlayıcısı kurulumubölümüne](#hive-warehouse-connector-setup)bakın.:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<STACK_VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    --conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
    --conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
    --conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
    --conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
    --conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
   ```

### <a name="creating-spark-dataframes-from-hive-queries"></a>Kovan sorgularından Spark DataFrame'leri oluşturma

HWC kitaplığını kullanan tüm sorguların sonuçları DataFrame olarak döndürülür. Aşağıdaki örnekler, temel bir sorgunun nasıl oluşturulabildiğini gösterir.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

Sorgunun sonuçları, MLIB ve SparkSQL gibi Spark kitaplıklarında kullanılabilen Spark DataFrames'tir.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Spark DataFrame'leri Hive tablolarına yazma

Kıvılcım, Hive'ın yönetilen ACID tablolarına yazmayı desteklemez. Ancak HWC kullanarak, herhangi bir DataFrame'i kovan tablosuna yazabilirsiniz. Bu işlevselliği iş başında aşağıdaki örnekte görebilirsiniz:

1. Çağrılan `sampletable_colorado` bir tablo oluşturun ve aşağıdaki komutu kullanarak sütunlarını belirtin:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

1. Sütunun `hivesampletable` `state` eşit olduğu tabloyu `Colorado`filtreleyin. Hive tablosunun bu sorgusu Bir Kıvılcım DataFrame olarak döndürülür. Daha sonra DataFrame işlevi kullanılarak `sampletable_colorado` Hive `write` tablosuna kaydedilir.

    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

1. Sonuçları aşağıdaki komutla görüntüleyin:

    ```scala
    hive.table("sampletable_colorado").show()
    ```

    ![kovan depo konektör gösteri kovan tablosu](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Yapılandırılmış akış yazmaları

Kovan Ambarı Bağlayıcısı'nı kullanarak, Kovan tablolarına veri yazmak için Spark akışını kullanabilirsiniz.

Yerel ana bilgisayar bağlantı noktası 9999'daki bir Spark akışından gelen verileri Bir Kovan tablosuna gönderen bir Kovan Ambarı Bağlayıcısı örneği oluşturmak için aşağıdaki adımları izleyin.

1. [Sorguları Bağlama ve çalıştırma](#connecting-and-running-queries)altındaki adımları izleyin.

1. Kıvılcım akışını aşağıdaki komutla başlatın:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9999).load()
    ```

1. Oluşturduğunuz Kıvılcım akışı için aşağıdaki adımları yaparak veri oluşturun:
    1. Aynı Spark kümesinde ikinci bir SSH oturumu açın.
    1. Komut isteminde, `nc -lk 9999`yazın. Bu komut, komut satırından belirtilen bağlantı noktasına veri göndermek için netcat yardımcı programını kullanır.

1. İlk SSH oturumuna dönün ve akış verilerini tutmak için yeni bir Hive tablosu oluşturun. Kıvılcım kabuğunda aşağıdaki komutu girin:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. Ardından akış verilerini aşağıdaki komutu kullanarak yeni oluşturulan tabloya yazın:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > `metastoreUri` Ve `database` seçenekleri şu anda Apache Spark bilinen bir sorun nedeniyle el ile ayarlanmalıdır. Bu konu hakkında daha fazla bilgi için Bkz. [SPARK-25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. İkinci SSH oturumuna dönün ve aşağıdaki değerleri girin:

    ```bash
    foo
    HiveSpark
    bar
    ```

1. İlk SSH oturumuna dönün ve kısa aktiviteye dikkat edin. Verileri görüntülemek için aşağıdaki komutu kullanın:

    ```scala
    hive.table("stream_table").show()
    ```

İkinci SSH oturumunda netcat'i durdurmak için **Ctrl + C'yi** kullanın. İlk `:q` SSH oturumunda kıvılcım kabuğundan çıkmak için kullanın.

### <a name="securing-data-on-spark-esp-clusters"></a>Kıvılcım ESP kümeleri hakkında veri güvenliğini sağlama

1. Aşağıdaki komutları girerek bazı örnek verileri içeren bir tablo `demo` oluşturun:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Tablonun içeriğini aşağıdaki komutla görüntüleyin. İlkeyi uygulamadan `demo` önce tablo tam sütunu gösterir.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![ranger politikası uygulamadan önce demo tablo](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Yalnızca sütunun son dört karakterini gösteren bir sütun maskeleme ilkesi uygulayın.  
    1. Ranger Admin UI gidin. `https://CLUSTERNAME.azurehdinsight.net/ranger/`
    1. **Hive**altında küme için Hive hizmeti tıklayın.
        ![ranger servis müdürü](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. **Maskeleme** sekmesine tıklayın ve ardından **Yeni İlke Ekle**

        ![kovan depo konektörü ranger kovan politikası listesi](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)

    a. İstenilen bir ilke adı sağlayın. Veritabanı seçin: **Varsayılan**, Kovan tablosu: **demo**, Hive sütun: **ad**, Kullanıcı: **rsadmin2**, Erişim Türleri: **seçin**ve Kısmi maske: **Select Maskeleme Seçeneği** menüsünden son 4 **göster.** **Ekle**’ye tıklayın.
                ![ilke oluşturma](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. Tablonun içeriğini yeniden görüntüleyin. Ranger ilkesini uyguladıktan sonra, sütunun yalnızca son dört karakterini görebiliriz.

    ![ranger politikası uygulandıktan sonra demo tablo](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Etkileşimli Sorguyu HDInsight ile Kullanma](./apache-interactive-query-get-started.md)
* [Zeppelin, Livy, kıvılcım-gönder ve pyspark kullanarak Kovan Depo Konektörü ile etkileşim örnekleri](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
