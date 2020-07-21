---
title: Apache Spark & Hive-Hive ambar Bağlayıcısı-Azure HDInsight
description: Apache Spark ve Apache Hive Azure HDInsight 'ta Hive ambarı Bağlayıcısı ile tümleştirmeyi öğrenin.
author: nis-goel
ms.author: nisgoel
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 05/28/2020
ms.openlocfilehash: 24968511d038b2cea41a59187c0a361684c6720e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511900"
---
# <a name="integrate-apache-spark-and-apache-hive-with-hive-warehouse-connector-in-azure-hdinsight"></a>Azure HDInsight 'ta Hive ambarı Bağlayıcısı ile Apache Spark ve Apache Hive tümleştirme

Apache Hive ambar Bağlayıcısı (HWC), Apache Spark ve Apache Hive ile daha kolay çalışmanıza olanak sağlayan bir kitaplıktır. Spark DataFrames ve Hive tabloları arasında veri taşıma gibi görevleri destekler. Ayrıca, Spark akış verilerini Hive tablolarına yönlendirerek. Hive ambar Bağlayıcısı Spark ve Hive arasında bir köprü gibi çalışmaktadır. Geliştirme için programlama dilleri olarak Scala, Java ve Python da destekler.

Hive ambarı Bağlayıcısı, güçlü büyük veri uygulamaları oluşturmak için Hive ve Spark 'ın benzersiz özelliklerinden yararlanmanıza olanak tanır.

Apache Hive atomik, tutarlı, yalıtılmış ve dayanıklı (ACID) veritabanı işlemleri için destek sunar. Hive içindeki ACID ve işlemler hakkında daha fazla bilgi için bkz. [Hive işlemleri](https://cwiki.apache.org/confluence/display/Hive/Hive+Transactions). Hive Ayrıca, Apache Ranger aracılığıyla ayrıntılı güvenlik denetimleri ve düşük gecikme süreli analitik Işleme (LLAP) Apache Spark kullanılamaz.

Apache Spark, Apache Hive içinde kullanılamayan akış özellikleri sağlayan yapılandırılmış bir akış API 'SI vardır. HDInsight 4,0 ' den başlayarak, Apache Spark 2.3.1 ve Apache Hive 3.1.0 ayrı bir metastores içermelidir. Ayrı metasa, birlikte çalışabilirliğini zorlaştırır. Hive ambarı Bağlayıcısı Spark ve Hive 'yi birlikte kullanmayı kolaylaştırır. HWC kitaplığı, LLAP Daemon 'ları 'den Spark yürüticilerine paralel olarak veri yükler. Bu işlem, Spark 'tan Hive 'e standart bir JDBC bağlantısı kullanmaktan daha verimli ve uyarlanabilir hale getirir.

![Hive ambarı bağlayıcı mimarisi](./media/apache-hive-warehouse-connector/hive-warehouse-connector-architecture.png)

Hive ambarı Bağlayıcısı tarafından desteklenen işlemlerden bazıları şunlardır:

* Bir tabloyu açıklama
* ORC biçimli veriler için tablo oluşturma
* Hive verilerini seçme ve bir veri çerçevesini alma
* Toplu işteki bir veri çerçevesini Hive 'e yazma
* Hive güncelleştirme ekstresi yürütülüyor
* Hive 'den tablo verilerini okuma, Spark 'ta dönüştürme ve yeni bir Hive tablosuna yazma
* HiveStreaming kullanarak Hive 'e veri çerçevesi veya Spark akışı yazma

## <a name="hive-warehouse-connector-setup"></a>Hive ambar Bağlayıcısı kurulumu

> [!IMPORTANT]
> Spark 2,4 Kurumsal Güvenlik Paketi kümelerinde yüklü olan HiveServer2 Interactive örneği, Hive ambarı Bağlayıcısı ile kullanım için desteklenmez. Bunun yerine, HiveServer2 etkileşimli iş yüklerinizi barındırmak için ayrı bir HiveServer2 Interactive kümesi yapılandırmanız gerekir. Tek bir Spark 2,4 kümesinden yararlanan bir Hive ambar Bağlayıcısı yapılandırması desteklenmez.

Hive ambarı Bağlayıcısı Spark ve etkileşimli sorgu iş yükleri için ayrı kümeler gerektirir. Bu kümeleri Azure HDInsight 'ta ayarlamak için aşağıdaki adımları izleyin.

### <a name="create-clusters"></a>Küme oluşturma

1. Depolama hesabı ve özel bir Azure sanal ağı ile bir HDInsight Spark **4,0** kümesi oluşturun. Azure sanal ağında bir küme oluşturma hakkında daha fazla bilgi için bkz. [mevcut bir sanal ağa HDInsight ekleme](../../hdinsight/hdinsight-plan-virtual-network-deployment.md#existingvnet).

1. Spark kümesi ile aynı depolama hesabı ve Azure sanal ağı ile bir HDInsight etkileşimli sorgu (LLAP) **4,0** kümesi oluşturun.

### <a name="configure-hwc-settings"></a>HWC ayarlarını yapılandırma

#### <a name="gather-preliminary-information"></a>Ön bilgileri toplayın

1. Bir Web tarayıcısından, `https://LLAPCLUSTERNAME.azurehdinsight.net/#/main/services/HIVE` LLAPCLUSTERNAME ' ın etkileşimli sorgu Kümenizin adı olduğu yere gidin.

1. **Summary**  >  **HiveServer2 Interactive JDBC URL** 'sine gidin ve değeri aklınızda edin. Değer şuna benzer olabilir: `jdbc:hive2://zk0-iqgiro.rekufuk2y2ce.bx.internal.cloudapp.net:2181,zk1-iqgiro.rekufuk2y2ce.bx.internal.cloudapp.net:2181,zk4-iqgiro.rekufuk2y2ce.bx.internal.cloudapp.net:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2-interactive` .

1. **Configs**  >  **Gelişmiş**  >  **Gelişmiş Hive-site**  >  **Hive. Zookeeper. Quorum** ' a gidin ve değeri aklınızda edin. Değer şuna benzer olabilir: `zk0-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181,zk1-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181,zk4-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:2181` .

1. **Configs**  >  **Gelişmiş**  >  **genel**  >  **Hive. metasileri. uris** sayfasına gidin ve değeri aklınızda yapın. Değer şuna benzer olabilir: `thrift://iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:9083,thrift://hn1-iqgiro.rekufuk2y2cezcbowjkbwfnyvd.bx.internal.cloudapp.net:9083` .

1. **Configs**  >  **Gelişmiş**  >  **Gelişmiş Hive-Interactive-site**  >  **Hive. LLAP. Daemon. Service. konaklarına** gidin ve değeri aklınızda yapın. Değer şuna benzer olabilir: `@llap0` .

#### <a name="configure-spark-cluster-settings"></a>Spark kümesi ayarlarını yapılandırma

1. Bir Web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/#/main/services/SPARK2/configs` CLUSTERNAME Apache Spark Kümenizin adı ' na gidin.

1. **Özel spark2-varsayılanlar**' ı genişletin.

    ![Apache ambarı Spark2 yapılandırması](./media/apache-hive-warehouse-connector/hive-warehouse-connector-spark2-ambari.png)

1. Aşağıdaki konfigürasyonları eklemek için **Özellik Ekle...** öğesini seçin:

    | Yapılandırma | Değer |
    |----|----|
    |`spark.datasource.hive.warehouse.load.staging.dir`|`wasbs://STORAGE_CONTAINER_NAME@STORAGE_ACCOUNT_NAME.blob.core.windows.net/tmp`. <br> Uygun bir ile uyumlu hazırlama dizinine ayarlayın. İki farklı kümeniz varsa, HiveServer2 erişimi olması için, hazırlama dizini LLAP kümesinin depolama hesabının hazırlama dizininde bir klasör olmalıdır.  `STORAGE_ACCOUNT_NAME`Küme tarafından kullanılan depolama hesabının adıyla ve `STORAGE_CONTAINER_NAME` depolama kapsayıcısının adıyla değiştirin. |
    |`spark.sql.hive.hiveserver2.jdbc.url`| **HiveServer2 ıNTERACTIVE JDBC URL** 'sinden daha önce edindiğiniz değer |
    |`spark.datasource.hive.warehouse.metastoreUri`| Daha önce **Hive. metaser. Uri**'lerden edindiğiniz değer. |
    |`spark.security.credentials.hiveserver2.enabled`|`true`YARN küme modu ve `false` Yarn istemci modu için. |
    |`spark.hadoop.hive.zookeeper.quorum`| Daha önce **Hive. Zookeeper. Quorum**öğesinden edindiğiniz değer. |
    |`spark.hadoop.hive.llap.daemon.service.hosts`| Daha önce **Hive. LLAP. Daemon. Service. konaklarından**edindiğiniz değer. |

1. Değişiklikleri kaydedin ve etkilenen tüm bileşenleri yeniden başlatın.

### <a name="configure-hwc-for-enterprise-security-package-esp-clusters"></a>Kurumsal Güvenlik Paketi (ESP) kümeleri için HWC 'yi yapılandırma

Kurumsal Güvenlik Paketi (ESP), Azure HDInsight 'ta Apache Hadoop kümelerine yönelik Active Directory tabanlı kimlik doğrulaması, çoklu Kullanıcı desteği ve rol tabanlı erişim denetimi gibi kurumsal düzeyde yetenekler sağlar. ESP hakkında daha fazla bilgi için bkz. [HDInsight 'ta kurumsal güvenlik paketi kullanma](../domain-joined/apache-domain-joined-architecture.md).

Önceki bölümde bahsedilen yapılandırmalardan ayrı olarak, ESP kümelerinde HWC kullanmak için aşağıdaki yapılandırmayı ekleyin.

1. Spark kümesinin ambarı Web kullanıcı arabiriminden **Spark2**  >  **configs**  >  **Custom Spark2-Defaults**dizinine gidin.

1. Aşağıdaki özelliği güncelleştirin.

    | Yapılandırma | Değer |
    |----|----|
    | `spark.sql.hive.hiveserver2.jdbc.url.principal`    | `hive/<llap-headnode>@<AAD-Domain>` |
    
    * Bir Web tarayıcısından, CLUSTERNAME öğesine gidin ve `https://CLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/summary` etkileşimli sorgu kümenizin adıdır. **HiveServer2 Interactive**seçeneğine tıklayın. Ekran görüntüsünde gösterildiği gibi LLAP 'nin çalıştığı baş düğümün tam etki alanı adını (FQDN) görürsünüz. `<llap-headnode>`Bu değerle değiştirin.

        ![Hive ambar Bağlayıcısı baş düğümü](./media/apache-hive-warehouse-connector/head-node-hive-server-interactive.png)

    * Etkileşimli sorgu kümenize bağlanmak için [SSH komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. `default_realm`Dosyadaki parametreyi bulun `/etc/krb5.conf` . `<AAD-DOMAIN>`Bu değerle değiştirin, aksi takdirde kimlik bilgisi bulunamadı.

        ![Hive ambar Bağlayıcısı AAD etki alanı](./media/apache-hive-warehouse-connector/aad-domain.png)

    * Örneğin, `hive/hn0-ng36ll.mjry42ikpruuxgs2qy2kpg4q5e.cx.internal.cloudapp.net@PKRSRVUQVMAE6J85.D2.INTERNAL.CLOUDAPP.NET` .
    
1. Değişiklikleri kaydedin ve gerektiğinde bileşenleri yeniden başlatın.

## <a name="hive-warehouse-connector-usage"></a>Hive ambarı bağlayıcı kullanımı

Etkileşimli sorgu kümenize bağlanmak ve Hive ambarı bağlayıcısını kullanarak sorguları yürütmek için birkaç farklı yöntem arasından seçim yapabilirsiniz. Desteklenen yöntemler aşağıdaki araçları içerir:

* [Spark-Shell/PySpark](../spark/apache-spark-shell.md)
* [Spark-gönder](#spark-submit)
* [Zeppelin](./apache-hive-warehouse-connector-zeppelin.md)


Spark 'tan HWC 'ye bağlanmak için bazı örnekler aşağıda verilmiştir.

### <a name="spark-shell"></a>Spark-Shell

1. Apache Spark kümenize bağlanmak için [SSH komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. SSH oturumınızdan, sürümü göz önünde çalıştırmak için aşağıdaki komutu yürütün `hive-warehouse-connector-assembly` :

    ```bash
    ls /usr/hdp/current/hive_warehouse_connector
    ```

1. Aşağıdaki kodu `hive-warehouse-connector-assembly` yukarıda belirtilen sürümle düzenleyin. Ardından Spark kabuğunu başlatmak için komutunu yürütün:

    ```bash
    spark-shell --master yarn \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    ```

1. Spark kabuğunu başlattıktan sonra, aşağıdaki komutları kullanarak bir Hive ambar Bağlayıcısı örneği başlatılabilir:

    ```scala
    import com.hortonworks.hwc.HiveWarehouseSession
    val hive = HiveWarehouseSession.session(spark).build()
    ```

### <a name="spark-submit"></a>Spark-gönder

Bir bütünleştirilmiş kod jar içindeki bağımlılıklarla birlikte Scala/Java kodunu derleyip, bir Spark uygulaması başlatmak için aşağıdaki komutu kullanın. `<VERSION>`Ve `<APP_JAR_PATH>` değerlerini gerçek değerlerle değiştirin.

* YARN Istemci modu
    
    ```scala
    spark-submit \
    --class myHwcApp \
    --master yarn \
    --deploy-mode client \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=false
    /<APP_JAR_PATH>/myHwcAppProject.jar
    ```

* YARN küme modu
    ```scala
    spark-submit \
    --class myHwcApp \
    --master yarn \
    --deploy-mode cluster \
    --jars /usr/hdp/current/hive_warehouse_connector/hive-warehouse-connector-assembly-<VERSION>.jar \
    --conf spark.security.credentials.hiveserver2.enabled=true
    /<APP_JAR_PATH>/myHwcAppProject.jar
    ```

Python için aşağıdaki yapılandırmayı da ekleyin. 

```python
--py-files /usr/hdp/current/hive_warehouse_connector/pyspark_hwc-<VERSION>.zip
```
    
## <a name="run-queries-on-enterprise-security-package-esp-clusters"></a>Kurumsal Güvenlik Paketi (ESP) kümelerinde sorguları çalıştırma

`kinit`Spark-Shell veya Spark-Gönder ' i başlatmadan önce kullanın. Kullanıcı adını kümeye erişim izinleri olan bir etki alanı hesabının adıyla değiştirin, ardından aşağıdaki komutu yürütün:

```bash
kinit USERNAME
```

### <a name="securing-data-on-spark-esp-clusters"></a>Spark ESP kümelerinde verilerin güvenliğini sağlama

1. `demo`Aşağıdaki komutları girerek bazı örnek verilerle tablo oluşturun:

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
    1. Adresindeki Ranger Yönetici Kullanıcı arabirimine gidin `https://LLAPCLUSTERNAME.azurehdinsight.net/ranger/` .
    1. **Hive**altındaki kümeniz için Hive hizmetine tıklayın.
        ![Ranger Service Manager](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-service-manager.png)
    1. **Maskeleme** sekmesine tıklayın ve ardından **Yeni ilke ekleyin**

        ![Hive ambar Bağlayıcısı Ranger Hive ilke listesi](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-hive-policy-list.png)

    1. İstenen bir ilke adı belirtin. Veritabanı seçin: **varsayılan**, Hive tablosu: **demo**, Hive sütunu: **ad**, Kullanıcı: **Rsadmin2**, erişim türleri: **Select**ve **kısmi maske:** **maskeleme seç seçenek** menüsünden son 4 ' ü göster. **Ekle**'ye tıklayın.
                ![ilke oluştur](./media/apache-hive-warehouse-connector/hive-warehouse-connector-ranger-create-policy.png)
1. Tablonun içeriğini yeniden görüntüleyin. Ranger ilkesini uyguladıktan sonra sütunun yalnızca son dört karakterini görebiliriz.

    ![Ranger ilkesini uyguladıktan sonra demo tablosu](./media/apache-hive-warehouse-connector/hive-warehouse-connector-table-after-ranger-policy.png)

## <a name="next-steps"></a>Sonraki adımlar

* [HWC ve Apache Spark işlemleri](./apache-hive-warehouse-connector-operations.md)
* [Etkileşimli Sorguyu HDInsight ile Kullanma](./apache-interactive-query-get-started.md)
* [Apache Zeppelin ile HWC tümleştirmesi](./apache-hive-warehouse-connector-zeppelin.md)
* [Zeppelin, Livy, Spark-gönder ve pyspark kullanılarak Hive ambar Bağlayıcısı ile etkileşim örnekleri](https://community.hortonworks.com/articles/223626/integrating-apache-hive-with-apache-spark-hive-war.html)
