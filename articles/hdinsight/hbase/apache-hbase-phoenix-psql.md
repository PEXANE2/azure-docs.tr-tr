---
title: Psql kullanarak Apache Phoenix'e toplu yükleme - Azure HDInsight
description: Azure HDInsight'ta Toplu yük verilerini Apache Phoenix tablolarına yüklemek için psql aracını kullanın
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: 845c4a62aee04a8acdc645ba4c41f1f5496537c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552619"
---
# <a name="bulk-load-data-into-apache-phoenix-using-psql"></a>Psql kullanarak Apache Phoenix’e toplu veri yükleme

[Apache Phoenix](https://phoenix.apache.org/) açık kaynak kodlu, kitlesel paralel ilişkisel veritabanı [Apache HBase](../hbase/apache-hbase-overview.md)üzerine inşa edilmiştir. Phoenix, HBase üzerinden SQL benzeri sorgular sağlar. Phoenix, kullanıcıların SQL tabloları, dizinleri, görünümleri ve dizileri oluşturmasını, silmesini ve değiştirmesini ve satırları tek tek ve toplu olarak yükseltmesini sağlamak için JDBC sürücülerini kullanır. Phoenix, HBase'in üstünde düşük gecikme süresi uygulamaları oluşturmak için sorguları derlemek için MapReduce'i kullanmak yerine noSQL yerel derlemesini kullanır. Phoenix, sunucunun adres alanında istemci tarafından sağlanan kodu çalıştırmayı desteklemek için yardımcı işlemciler ekler ve verilerle birlikte bulunan kodu çalıştırılır. Bu, istemci/sunucu veri aktarımını en aza indirir.  HDInsight'ta Phoenix'i kullanarak verilerle çalışmak için önce tablolar oluşturun ve ardından verilere yükleyin.

## <a name="bulk-loading-with-apache-phoenix"></a>Apache Phoenix ile toplu yükleme

İstemci API'lerini kullanma, TableOutputFormat ile bir MapReduce işi veya HBase kabuğunu kullanarak verileri el ile girme dahil olmak üzere, Verileri HBase'e girmenin birden çok yolu vardır. Phoenix, CSV verilerini Phoenix tablolarına yüklemek için iki `psql`yöntem sağlar: adlandırılmış bir istemci yükleme aracı ve MapReduce tabanlı toplu yükleme aracı.

Araç `psql` tek dişli ve en iyi megabayt veya gigabayt veri yüklemek için uygundur. Yüklenecek tüm CSV dosyaları '.csv' dosya uzantısına sahip olmalıdır.  '.sql' dosya uzantısı `psql` ile komut satırında SQL komut dosyası dosyalarını da belirtebilirsiniz.

MapReduce ile toplu yükleme, mapreduce birden çok iş parçacığı kullandığından, genellikle üretim senaryolarında çok daha büyük veri hacimleri için kullanılır.

Verileri yüklemeye başlamadan önce, Phoenix'in etkin olduğunu ve sorgu zaman aşımı ayarlarının beklendiği gibi olduğunu doğrulayın.  HDInsight kümenize [erişin Apache Ambari](https://ambari.apache.org/) panosuna, HBase'i ve ardından Yapılandırma sekmesini seçin.  Apache Phoenix'in gösterildiği gibi `enabled` ayarlı olduğunu doğrulamak için aşağı kaydırın:

![Apache Phoenix HDInsight Küme Ayarları](./media/apache-hbase-phoenix-psql/apache-ambari-phoenix.png)

### <a name="use-psql-to-bulk-load-tables"></a>Toplu `psql` yük tabloları için kullanın

1. Adlı `createCustomersTable.sql`bir dosya oluşturun ve aşağıdaki kodu dosyaya kopyalayın. Sonra dosyayı kaydedin ve kapatın.

    ```sql
    CREATE TABLE Customers (
        ID varchar NOT NULL PRIMARY KEY,
        Name varchar,
        Income decimal,
        Age INTEGER,
        Country varchar);
    ```

1. Adlı `listCustomers.sql`bir dosya oluşturun ve aşağıdaki kodu dosyaya kopyalayın. Sonra dosyayı kaydedin ve kapatın.

    ```sql
    SELECT * from Customers;
    ```

1. Adlı `customers.csv`bir dosya oluşturun ve aşağıdaki kodu dosyaya kopyalayın. Sonra dosyayı kaydedin ve kapatın.

    ```txt
    1,Samantha,260000.0,18,US
    2,Sam,10000.5,56,US
    3,Anton,550150.0,42,Norway
    ```

1. Adlı `customers2.csv`bir dosya oluşturun ve aşağıdaki kodu dosyaya kopyalayın. Sonra dosyayı kaydedin ve kapatın.

    ```txt
    4,Nicolle,180000.0,22,US
    5,Kate,210000.5,24,Canada
    6,Ben,45000.0,32,Poland
    ```

1. Komut istemini açın ve dizinini yeni oluşturulan dosyaların konumuyla değiştirin. CLUSTERNAME'yi aşağıda HBase kümenizin gerçek adı ile değiştirin. Ardından dosyaları kümenizin headnode yüklemek için kodu yürütmek:

    ```cmd
    scp customers.csv customers2.csv createCustomersTable.sql listCustomers.sql sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/tmp
    ```

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. ssh oturumunuzdan dizini **psql** aracının konumuna değiştirin. Aşağıdaki komutu uygulayın:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

1. Verileri toplu yükleyin. Aşağıdaki kod hem **Müşteriler** tablosunu oluşturur hem de verileri yükler.

    ```bash
    python psql.py /tmp/createCustomersTable.sql /tmp/customers.csv
    ```

    `psql` İşlem tamamlandıktan sonra aşağıdaki gibi benzer bir ileti görmeniz gerekir:

    ```output
    csv columns from database.
    CSV Upsert complete. 3 rows upserted
    Time: 0.081 sec(s)
    ```

1. Müşteriler tablosunun `psql` içeriğini görüntülemek için kullanmaya devam edebilirsiniz. Aşağıdaki kodu çalıştırın:

    ```bash
    python psql.py /tmp/listCustomers.sql
    ```

    Alternatif olarak, verileri sorgulamak için [HBase kabuğunu](./query-hbase-with-hbase-shell.md)veya [Apache Zeppelin'i](./apache-hbase-phoenix-zeppelin.md) kullanabilirsiniz.

1. Ek veri yükleyin. Tablo zaten var olduğuna göre, komut tabloyu belirtir. Aşağıdaki komutu uygulayın:

    ```bash
    python psql.py -t CUSTOMERS /tmp/customers2.csv
    ```

## <a name="use-mapreduce-to-bulk-load-tables"></a>Toplu yük tabloları için MapReduce'i kullanma

Küme üzerinde dağıtılan daha yüksek iş yükü için MapReduce yük aracını kullanın. Bu yükleyici önce tüm verileri HFiles'a dönüştürür ve ardından oluşturulan HFiles'ı HBase'e sağlar.

1. Bu bölüm ssh oturumu ve daha önce oluşturulan nesnelerle devam eder. Yukarıdaki adımları kullanarak **gerektiğinde Müşteriler** tablosunu ve **customers.csv** dosyasını oluşturun. Gerekirse, ssh bağlantınızı yeniden kurun.

1. **Müşteriler** tablosunun içeriğini truncate. Açık ssh oturumunuzdan aşağıdaki komutları uygulayın:

    ```bash
    hbase shell
    truncate 'CUSTOMERS'
    exit
    ```

1. Dosyayı `customers.csv` kafa düğümünüzden Azure Depolama'ya kopyalayın.

    ```bash
    hdfs dfs -put /tmp/customers.csv wasbs:///tmp/customers.csv
    ```

1. MapReduce toplu yük komutu için yürütme dizinine değiştirin:

    ```bash
    cd /usr/hdp/current/phoenix-client
    ```

1. Phoenix istemci kavanozu ile komutu `hadoop` kullanarak CSV MapReduce yükleyicibaşlatın:

    ```bash
    HADOOP_CLASSPATH=/usr/hdp/current/hbase-client/lib/hbase-protocol.jar:/etc/hbase/conf hadoop jar phoenix-client.jar org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input /tmp/customers.csv
    ```

    Yükleme tamamlandıktan sonra aşağıdaki gibi benzer bir ileti görmeniz gerekir:

    ```output
    19/12/18 18:30:57 INFO client.ConnectionManager$HConnectionImplementation: Closing master protocol: MasterService
    19/12/18 18:30:57 INFO client.ConnectionManager$HConnectionImplementation: Closing zookeeper sessionid=0x26f15dcceff02c3
    19/12/18 18:30:57 INFO zookeeper.ZooKeeper: Session: 0x26f15dcceff02c3 closed
    19/12/18 18:30:57 INFO zookeeper.ClientCnxn: EventThread shut down
    19/12/18 18:30:57 INFO mapreduce.AbstractBulkLoadTool: Incremental load complete for table=CUSTOMERS
    19/12/18 18:30:57 INFO mapreduce.AbstractBulkLoadTool: Removing output directory /tmp/50254426-aba6-400e-88eb-8086d3dddb6
    ```

1. Azure Veri Gölü Depolama ile MapReduce'i kullanmak için, `hbase.rootdir` 'deki değer `hbase-site.xml`olan Veri Gölü Depolama kök dizinini bulun. Aşağıdaki komutta, Veri Gölü Depolama kök `adl://hdinsightconf1.azuredatalakestore.net:443/hbase1`dizini. Bu komutta, Veri Gölü Depolama giriş ve çıkış klasörlerini parametre olarak belirtin:

    ```bash
    cd /usr/hdp/current/phoenix-client

    $HADOOP_CLASSPATH=$(hbase mapredcp):/etc/hbase/conf hadoop jar /usr/hdp/2.4.2.0-258/phoenix/phoenix-4.4.0.2.4.2.0-258-client.jar

    org.apache.phoenix.mapreduce.CsvBulkLoadTool --table Customers --input adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/temp/input/customers.csv –zookeeper ZookeeperQuorum:2181:/hbase-unsecure --output  adl://hdinsightconf1.azuredatalakestore.net:443/hbase1/data/hbase/output1
    ```

1. Verileri sorgulamak ve görüntülemek için **psql'i** daha önce açıklandığı gibi kullanabilirsiniz. Ayrıca [HBase kabuk](./query-hbase-with-hbase-shell.md)veya [Apache Zeppelin](./apache-hbase-phoenix-zeppelin.md)kullanabilirsiniz.

## <a name="recommendations"></a>Öneriler

* Azure Depolama (WASB) veya Azure Veri Gölü Depolama (ADL) gibi giriş ve çıktı klasörleri için aynı depolama ortamını kullanın. Azure Depolama'dan Veri Gölü Depolamasına veri `distcp` aktarmak için aşağıdaki komutu kullanabilirsiniz:

    ```bash
    hadoop distcp wasb://@.blob.core.windows.net/example/data/gutenberg adl://.azuredatalakestore.net:443/myfolder
    ```

* Daha büyük boyutlu alt düğümler kullanın. MapReduce toplu kopyanın harita işlemleri, mevcut DFS olmayan alanı dolduran büyük miktarda geçici çıktı üretir. Büyük miktarda toplu yükleme için, daha fazla ve daha büyük boyutlu alt düğümler kullanın. Kümenize ayırdığınız işçi düğümlerinin sayısı işlem hızını doğrudan etkiler.

* Giriş dosyalarını ~10 GB'lık parçalara bölün. Toplu yükleme depolama alanı yoğun bir işlemdir, bu nedenle giriş dosyalarınızı birden çok parçaya bölmek daha iyi performans sağlar.

* Bölge sunucusu etkin noktalarından kaçının. Satır anahtarınız monoton olarak artıyorsa, HBase sıralı yazmaları bölge sunucusu etkin noktaya yönelik bir noktaya neden olabilir. Satır tuşunu *tuzlama* sıralı yazmaları azaltır. Phoenix, aşağıda belirtildiği gibi, belirli bir tablo için bir tuzlama bayt ile satır anahtarı şeffaf tuz için bir yol sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Phoenix ile Toplu Veri Yükleme](https://phoenix.apache.org/bulk_dataload.html)
* [HDInsight'ta Linux tabanlı Apache HBase kümeleriyle Apache Phoenix'i kullanma](../hbase/apache-hbase-query-with-phoenix.md)
* [Tuzlu Tablolar](https://phoenix.apache.org/salted.html)
* [Apache Phoenix Dilbilgisi](https://phoenix.apache.org/language/index.html)
