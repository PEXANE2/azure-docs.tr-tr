---
title: Hive ambarı Bağlayıcısı ile Apache Spark ve Apache Hive tümleştirme
description: Apache Spark ve Apache Hive Azure HDInsight 'ta Hive ambarı Bağlayıcısı ile tümleştirmeyi öğrenin.
ms.service: hdinsight
author: nakhanha
ms.author: nakhanha
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 04/29/2019
ms.openlocfilehash: f3a0fa1ecdb2db94b43a5380f9497b4b1c266e47
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68441939"
---
# <a name="integrate-apache-spark-and-apache-hive-with-the-hive-warehouse-connector"></a>Hive ambarı Bağlayıcısı ile Apache Spark ve Apache Hive tümleştirme

Apache Hive ambar Bağlayıcısı (HWC), verileri Spark veri çerçeveleri ve Hive tabloları arasında taşıma ve ayrıca Spark akış verilerini Hive tablolarına yönlendirme gibi görevleri destekleyerek Apache Spark ve Apache Hive ile daha kolay çalışmanıza olanak tanıyan bir kitaplıktır. Hive ambar Bağlayıcısı Spark ve Hive arasında bir köprü gibi çalışmaktadır. Geliştirme için Scala, Java ve Python 'u destekler.

Hive ambarı Bağlayıcısı, güçlü büyük veri uygulamaları oluşturmak için Hive ve Spark 'ın benzersiz özelliklerinden yararlanmanıza olanak tanır. Apache Hive atomik, tutarlı, yalıtılmış ve dayanıklı (ACID) veritabanı işlemleri için destek sunar. Hive içindeki ACID ve işlemler hakkında daha fazla bilgi için bkz. [Hive işlemleri](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions). Hive Ayrıca Apache Ranger aracılığıyla ayrıntılı güvenlik denetimleri ve düşük gecikme süreli analitik Işlem Apache Spark kullanılamaz.

Apache Spark, Apache Hive içinde kullanılamayan akış özellikleri sağlayan yapılandırılmış bir akış API 'SI vardır. HDInsight 4,0 ' den başlayarak, Apache Spark 2.3.1 ve Apache Hive 3.1.0, birlikte çalışabilirliğin zor olmasına yönelik ayrı metastores 'e sahiptir. Hive ambarı Bağlayıcısı Spark ve Hive 'yi birlikte kullanmayı kolaylaştırır. HWC kitaplığı, LLAP Daemon 'ları 'ten Spark yürüticilerine verileri paralel olarak yükler ve Spark 'tan Hive 'e standart bir JDBC bağlantısı kullanmaktan daha verimli ve ölçeklenebilir hale getirir.

![Mimari](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Hive ambarı Bağlayıcısı tarafından desteklenen işlemlerden bazıları şunlardır:

* Bir tabloyu açıklama
* ORC biçimli veriler için tablo oluşturma
* Hive verilerini seçme ve bir veri çerçevesini alma
* Toplu işteki bir veri çerçevesini Hive 'e yazma
* Hive güncelleştirme ekstresi yürütülüyor
* Hive 'den tablo verilerini okuma, Spark 'ta dönüştürme ve yeni bir Hive tablosuna yazma
* HiveStreaming kullanarak Hive 'e veri çerçevesi veya Spark akışı yazma

## <a name="hive-warehouse-connector-setup"></a>Hive ambar Bağlayıcısı kurulumu

Azure HDInsight 'ta Spark ve etkileşimli sorgu kümesi arasında Hive ambarı bağlayıcısını ayarlamak için aşağıdaki adımları izleyin:

1. Depolama hesabı ve özel bir Azure sanal ağı ile Azure portal kullanarak bir HDInsight Spark 4,0 kümesi oluşturun. Azure sanal ağında bir küme oluşturma hakkında daha fazla bilgi için bkz. [mevcut bir sanal ağa HDInsight ekleme](../../hdinsight/hdinsight-plan-virtual-network-deployment.md#existingvnet).
1. Spark kümesiyle aynı depolama hesabı ve Azure sanal ağı ile Azure portal kullanarak HDInsight etkileşimli sorgu (LLAP) 4,0 kümesi oluşturun.
1. Etkileşimli sorgu kümenizin `/etc/hosts` headnode0 üzerindeki `/etc/hosts` dosya içeriğini Spark kümenizdeki headnode0 dosyasına kopyalayın. Bu adım, Spark kümenizin etkileşimli sorgu kümesindeki düğümlerin IP adreslerini çözümlemesine izin verir. Güncelleştirilmiş dosyanın içeriğini ile `cat /etc/hosts`görüntüleyin. Çıktı aşağıdaki ekran görüntüsünde gösterilen gibi görünmelidir.

    ![Hosts dosyasını görüntüleme](./media/apache-hive-warehouse-connector/hive-warehouse-connector-hosts-file.png)

1. Aşağıdaki adımları uygulayarak Spark kümesi ayarlarını yapılandırın: 
    1. Azure portal ' e gidin, HDInsight kümeleri ' ni seçin ve ardından Küme adına tıklayın.
    1. Sağ tarafta, **küme panoları**' nın altında, **ambarı giriş**' i seçin.
    1. Ambarı Web Kullanıcı arabiriminde, **SPARK2** > **configs** > **Custom SPARK2-Defaults**' a tıklayın.

        ![Spark2 ambarı yapılandırması](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

    1. `spark.hadoop.hive.llap.daemon.service.hosts` **Gelişmiş Hive-Interactive-env**altındaki **LLAP uygulama adı** özelliğiyle aynı değere ayarlayın. Örneğin, `@llap0`

    1. Etkileşimli `spark.sql.hive.hiveserver2.jdbc.url` sorgu kümesinde Hiveserver2 'e bağlanan JDBC bağlantı dizesine ayarlayın. Kümeniz için bağlantı dizesi aşağıdaki URI gibi görünür. `CLUSTERNAME`Spark Kümenizin adı ve `user` ve `password` parametreleri kümeniz için doğru değerlere ayarlanır.

        ```
        jdbc:hive2://LLAPCLUSTERNAME.azurehdinsight.net:443/;user=admin;password=PWD;ssl=true;transportMode=http;httpPath=/hive2
        ```

        >[!Note] 
        > JDBC URL 'SI, Kullanıcı adı ve parola dahil olmak üzere Hiveserver2 'e bağlanmak için kimlik bilgilerini içermelidir.

    1. Uygun `spark.datasource.hive.warehouse.load.staging.dir` bir ile uyumlu hazırlama dizinine ayarlayın. İki farklı kümeniz varsa, HiveServer2 erişimi olması için, hazırlama dizini LLAP kümesinin depolama hesabının hazırlama dizininde bir klasör olmalıdır. Örneğin, `wasb://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp` `STORAGE_ACCOUNT_NAME` küme tarafından kullanılan depolama hesabının adı ve `STORAGE_CONTAINER_NAME` depolama kapsayıcısının adıdır.

    1. Etkileşimli `spark.datasource.hive.warehouse.metastoreUri` sorgu kümesinin meta veri deposu URI 'si değeri ile ayarlanır. LLAP kümeniz için metastoreurı 'sini bulmak için, **Hive** > **Gelişmiş** > **genel**altındaki LLAP kümenizin Hive Kullanıcı arabirimindeki **Hive. metasileri. uris** özelliğini arayın. Değer şu URI 'ye benzer şekilde görünür:

        ```
        thrift://hn0-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083,
        thrift://hn1-hwclla.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:9083
        ```

    1. Yarn `false` istemci dağıtım modu için olarak ayarlayın `spark.security.credentials.hiveserver2.enabled` .
    1. LLAP kümenizin Zookeeper çekirdeğini ayarlayın `spark.hadoop.hive.zookeeper.quorum` . LLAP kümeniz için Zookeeper çekirdeğini bulmak üzere **Hive** > **Gelişmiş** > **Gelişmiş Hive-site**altındaki LLAP kümenizin Hive Kullanıcı arabirimindeki **Hive. Zookeeper. Quorum** özelliğini arayın. Değer aşağıdaki dizeye benzer bir şekilde görünür:

        ```
        zk1-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk4-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181,
        zk6-nkhvne.0iv2nyrmse1uvp2caa4e34jkmf.cx.internal.cloudapp.net:2181
        ```

Hive ambar bağlayıcınızın yapılandırmasını test etmek için [sorguları bağlama ve çalıştırma](#connecting-and-running-queries)bölümündeki adımları izleyin.

## <a name="using-the-hive-warehouse-connector"></a>Hive ambarı bağlayıcısını kullanma

### <a name="connecting-and-running-queries"></a>Sorguları bağlama ve çalıştırma

Etkileşimli sorgu kümenize bağlanmak ve Hive ambarı bağlayıcısını kullanarak sorguları yürütmek için birkaç farklı yöntem arasından seçim yapabilirsiniz. Desteklenen yöntemler aşağıdaki araçları içerir:

* [Spark-Shell](../spark/apache-spark-shell.md)
* PySpark
* Spark-gönder
* [Zeppelin](../spark/apache-spark-zeppelin-notebook.md)
* [Livy](../spark/apache-spark-livy-rest-interface.md)

Bu makalede sunulan tüm örnekler Spark-Shell aracılığıyla yürütülür.

Spark-Shell oturumu başlatmak için aşağıdaki adımları uygulayın:

1. Kümenizin baş düğümüne SSH. SSH ile kümenize bağlanma hakkında daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
1. Spark-Shell komutunda parametre olarak kullanılan `cd /usr/hdp/current/hive_warehouse_connector` tüm jar dosyalarının tam yolunu yazarak veya yazarak doğru dizine geçin.
1. Spark kabuğunu başlatmak için aşağıdaki komutu girin:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

1. Bir hoş geldiniz iletisi ve `scala>` komut girebileceğiniz bir istem görürsünüz.

1. Spark-Shell ' i başlattıktan sonra, aşağıdaki komutları kullanarak bir Hive ambar Bağlayıcısı örneği başlatılabilir:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="connecting-and-running-queries-on-enterprise-security-package-esp-clusters"></a>Kurumsal Güvenlik Paketi (ESP) kümelerinde sorguları bağlama ve çalıştırma

Kurumsal Güvenlik Paketi (ESP), Azure HDInsight 'ta Apache Hadoop kümelerine yönelik Active Directory tabanlı kimlik doğrulaması, çoklu Kullanıcı desteği ve rol tabanlı erişim denetimi gibi kurumsal düzeyde yetenekler sağlar. ESP hakkında daha fazla bilgi için bkz. [HDInsight 'ta kurumsal güvenlik paketi kullanma](../domain-joined/apache-domain-joined-architecture.md).

1. [Sorguları bağlama ve çalıştırma](#connecting-and-running-queries)altında 1 ve 2 ilk adımları izleyin.
1. Bir `kinit` etki alanı kullanıcısı ile yazın ve oturum açın.
1. Spark-Shell ' i aşağıda gösterildiği gibi yapılandırma parametrelerinin tam listesiyle başlatın. Açılı ayraçlar arasındaki tüm büyük harflerde bulunan tüm değerler kümenize göre belirtilmelidir. Aşağıdaki parametrelerden herhangi birine giriş yapmak için değerleri bulmanız gerekiyorsa, [Hive ambarı bağlayıcı kurulumu](#hive-warehouse-connector-setup)'ndaki bölümüne bakın.:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/3.0.1.0-183/hive_warehouse_connector/hive-warehouse-connector-assembly-1.0.0.3.0.1.0-183.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    --conf spark.hadoop.hive.llap.daemon.service.hosts='<LLAP_APP_NAME>'
    --conf spark.sql.hive.hiveserver2.jdbc.url='jdbc:hive2://<ZOOKEEPER_QUORUM>;serviceDiscoveryMode=zookeeper;zookeeperNamespace=hiveserver2-interactive'
    --conf spark.datasource.hive.warehouse.load.staging.dir='<STAGING_DIR>'
    --conf spark.datasource.hive.warehouse.metastoreUri='<METASTORE_URI>'
    --conf spark.hadoop.hive.zookeeper.quorum='<ZOOKEEPER_QUORUM>'
   ```

### <a name="creating-spark-dataframes-from-hive-queries"></a>Hive sorgularından Spark veri çerçeveleri oluşturma

HWC kitaplığını kullanan tüm sorguların sonuçları bir veri çerçevesi olarak döndürülür. Aşağıdaki örneklerde, temel bir sorgunun nasıl oluşturulacağı gösterilmektedir.

```scala
hive.setDatabase("default")
val df = hive.executeQuery("select * from hivesampletable")
df.filter("state = 'Colorado'").show()
```

Sorgunun sonuçları, MLIB ve mini-SQL gibi Spark kitaplıklarıyla kullanılabilen Spark veri çerçevelerinden oluşur.

### <a name="writing-out-spark-dataframes-to-hive-tables"></a>Spark veri çerçevelerini Hive tablolarına yazma

Spark, Hive 'un yönetilen ACID tablolarına yazmayı yerel olarak desteklemez. Ancak, HWC 'yi kullanarak herhangi bir veri çerçevesini Hive tablosuna yazabilirsiniz. Aşağıdaki örnekte, bu işlevselliği çalışır durumda görebilirsiniz:

1. Adlı `sampletable_colorado` bir tablo oluşturun ve aşağıdaki komutu kullanarak sütunlarını belirtin:

    ```scala
    hive.createTable("sampletable_colorado").column("clientid","string").column("querytime","string").column("market","string").column("deviceplatform","string").column("devicemake","string").column("devicemodel","string").column("state","string").column("country","string").column("querydwelltime","double").column("sessionid","bigint").column("sessionpagevieworder","bigint").create()
    ```

2. Sütunun `hivesampletable` `state` eşit olduğutabloyufiltreleyin.`Colorado` Hive tablosunun bu sorgusu Spark veri çerçevesi olarak döndürülür. Ardından dataframe `sampletable_colorado` `write` işlevi kullanılarak Hive tablosuna kaydedilir.
    
    ```scala
    hive.table("hivesampletable").filter("state = 'Colorado'").write.format(HiveWarehouseSession.HIVE_WAREHOUSE_CONNECTOR).option("table","sampletable_colorado").save()
    ```

Elde edilen tabloyu aşağıdaki ekran görüntüsünde görebilirsiniz.

![Sonuç tablosunu göster](./media/apache-hive-warehouse-connector/hive-warehouse-connector-show-hive-table.png)

### <a name="structured-streaming-writes"></a>Yapılandırılmış akış yazmaları

Hive ambarı bağlayıcısını kullanarak, verileri Hive tablolarına yazmak için Spark akışını kullanabilirsiniz.

Localhost bağlantı noktası 9999 üzerindeki bir Spark akışından Hive tablosuna veri döndüren bir Hive ambarı bağlayıcı örneği oluşturmak için aşağıdaki adımları izleyin.

1. Spark kümenizde bir Terminal açın.
1. Spark akışını aşağıdaki komutla başlatın:

    ```scala
    val lines = spark.readStream.format("socket").option("host", "localhost").option("port",9988).load()
    ```

1. Aşağıdaki adımları uygulayarak oluşturduğunuz Spark akışı için veri oluşturun:
    1. Aynı Spark kümesinde başka bir Terminal açın.
    1. Komut istemine yazın `nc -lk 9999`. Bu komut, komut satırından belirtilen bağlantı noktasına veri göndermek için netcat yardımcı programını kullanır.
    1. Spark akışının içe dönmesini istediğiniz sözcükleri ve ardından satır başı öğesini yazın.
        ![Spark akışına veri girişi](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark-stream-data-input.png)
1. Akış verilerini tutmak için yeni bir Hive tablosu oluşturun. Spark-Shell ' te aşağıdaki komutları yazın:

    ```scala
    hive.createTable("stream_table").column("value","string").create()
    ```

1. Akış verilerini, aşağıdaki komutu kullanarak yeni oluşturulan tabloya yazın:

    ```scala
    lines.filter("value = 'HiveSpark'").writeStream.format(HiveWarehouseSession.STREAM_TO_STREAM).option("database", "default").option("table","stream_table").option("metastoreUri",spark.conf.get("spark.datasource.hive.warehouse.metastoreUri")).option("checkpointLocation","/tmp/checkpoint1").start()
    ```

    >[!Important]
    > `metastoreUri` Ve`database` seçenekleri şu anda Apache Spark ' deki bilinen bir sorundan dolayı el ile ayarlanmalıdır. Bu sorun hakkında daha fazla bilgi için bkz. [Spark-25460](https://issues.apache.org/jira/browse/SPARK-25460).

1. Tabloya yerleştirilen verileri aşağıdaki komutla görüntüleyebilirsiniz:

    ```scala
    hive.table("stream_table").show()
    ```

### <a name="securing-data-on-spark-esp-clusters"></a>Spark ESP kümelerinde verilerin güvenliğini sağlama

1. Aşağıdaki komutları girerek `demo` bazı örnek verilerle tablo oluşturun:

    ```scala
    create table demo (name string);
    INSERT INTO demo VALUES ('HDinsight');
    INSERT INTO demo VALUES ('Microsoft');
    INSERT INTO demo VALUES ('InteractiveQuery');
    ```

1. Aşağıdaki komutla tablonun içeriğini görüntüleyin. İlkeyi uygulamadan önce, `demo` tablo tam sütunu gösterir.

    ```scala
    hive.executeQuery("SELECT * FROM demo").show()
    ```

    ![Ranger ilkesini uygulamadan önce demo tablosu](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-before-ranger-policy.png)

1. Sütunun yalnızca son dört karakterini gösteren bir sütun maskeleme ilkesi uygulayın.  
    1. Adresindeki `https://CLUSTERNAME.azurehdinsight.net/ranger/`Ranger Yönetici Kullanıcı arabirimine gidin.
    1. **Hive**altındaki kümeniz için Hive hizmetine tıklayın.
        ![Ranger ilkesini uygulamadan önce demo tablosu](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. **Maskeleme** sekmesine tıklayın ve ardından **Yeni ilke** ![ilkesi listesi ekleyin](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)
    1. İstenen bir ilke adı belirtin. Veritabanı seçin: **Varsayılan**, Hive tablosu: **demo**, Hive sütunu: **ad**, Kullanıcı: **Rsadmin2**, erişim türleri: **Select**ve **kısmi maske:** **maskeleme seç seçenek** menüsünden son 4 ' ü göster.           **Ekle**'yi tıklatın.
                ![ilke listesi](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. Tablonun içeriğini yeniden görüntüleyin. Ranger ilkesini uyguladıktan sonra sütunun yalnızca son dört karakterini görebiliriz.

    ![Ranger ilkesini uyguladıktan sonra demo tablosu](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Etkileşimli Sorguyu HDInsight ile Kullanma](https://docs.microsoft.com/azure/hdinsight/interactive-query/apache-interactive-query-get-started)
* [Zeppelin, Livy, Spark-gönder ve pyspark kullanılarak Hive ambar Bağlayıcısı ile etkileşim örnekleri](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
