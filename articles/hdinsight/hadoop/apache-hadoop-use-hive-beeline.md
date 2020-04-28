---
title: Apache Hive-Azure HDInsight ile Apache Beeline kullanma
description: HDInsight 'ta Hadoop ile Hive sorguları çalıştırmak için Beeline istemcisini nasıl kullanacağınızı öğrenin. Beeline, JDBC üzerinden HiveServer2 ile çalışmaya yönelik bir yardımcı programdır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/17/2020
ms.openlocfilehash: 2396207c88716420d299382006a270eb747ddc03
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82192672"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>Apache Beeline istemcisini Apache Hive ile kullanma

HDInsight 'ta Apache Hive sorguları çalıştırmak için [Apache Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) nasıl kullanacağınızı öğrenin.

Beeline, HDInsight kümenizin baş düğümlerine eklenen bir Hive istemcanıdır. Beeline yerel olarak yüklemek için aşağıdaki [Beeline Istemcisini Install](#install-beeline-client)bölümüne bakın. Beeline, HDInsight kümenizde barındırılan bir hizmet olan HiveServer2 'e bağlanmak için JDBC kullanır. Ayrıca, HDInsight 'ta Internet üzerinden uzaktan erişim sağlamak için Beeline da kullanabilirsiniz. Aşağıdaki örneklerde, Beeline 'dan HDInsight 'a bağlanmak için kullanılan en yaygın bağlantı dizeleri sağlanmaktadır.

## <a name="types-of-connections"></a>Bağlantı türleri

### <a name="from-an-ssh-session"></a>Bir SSH oturumundan

Bir SSH oturumundan bir küme headnode 'a bağlanırken, bağlantı noktasındaki `headnodehost` `10001`adrese bağlanabilirsiniz:

```bash
beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
```

---

### <a name="over-an-azure-virtual-network"></a>Bir Azure sanal ağı üzerinden

Bir istemciden HDInsight 'a bir Azure sanal ağı üzerinden bağlanılırken, bir küme baş düğümünün tam etki alanı adını (FQDN) sağlamanız gerekir. Bu bağlantı doğrudan küme düğümlerine yapıldığından, bağlantı şu bağlantı noktasını `10001`kullanır:

```bash
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Bir `<headnode-FQDN>` küme headnode 'un tam etki alanı adıyla değiştirin. Bir headnode 'un tam etki alanı adını bulmak için [Apache ambarı REST API belgesini kullanarak HDInsight 'ı yönetme](../hdinsight-hadoop-manage-ambari-rest-api.md#get-the-fqdn-of-cluster-nodes) bölümündeki bilgileri kullanın.

---

### <a name="to-hdinsight-enterprise-security-package-esp-cluster-using-kerberos"></a>Kerberos kullanarak HDInsight Kurumsal Güvenlik Paketi (ESP) kümesine

Bir istemciden, kümenin aynı bölgesindeki bir makinede Azure Active Directory (AAD)-DS 'ye katılmış bir Kurumsal Güvenlik Paketi (ESP) kümesine bağlandığında, kümeye `<AAD-Domain>` `<username>`erişim izinleri olan etki alanı kullanıcı hesabının etki alanı adını ve adını da belirtmeniz gerekir:

```bash
kinit <username>
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>
```

Kümeye `<username>` erişim izinleri olan etki alanındaki hesabın adıyla değiştirin. Kümenin `<AAD-DOMAIN>` katıldığı Azure ACTIVE DIRECTORY (AAD) adıyla değiştirin. `<AAD-DOMAIN>` Değer için bir büyük dize kullanın, aksi takdirde kimlik bilgisi bulunamadı. Gerekirse `/etc/krb5.conf` bölge adlarını kontrol edin.

Ambarı 'ndan JDBC URL 'sini bulmak için:

1. Bir Web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/summary`, Kümenizin adı `CLUSTERNAME` olan ' a gidin. HiveServer2 'ın çalıştığından emin olun.

1. HiveServer2 JDBC URL 'sini kopyalamak için Pano 'yu kullanın.

---

### <a name="over-public-or-private-endpoints"></a>Herkese açık veya özel uç noktalar

Ortak veya özel uç noktaları kullanarak bir kümeye bağlanırken, küme oturum açma hesabı adını (varsayılan `admin`) ve parolayı sağlamanız gerekir. Örneğin, `clustername.azurehdinsight.net` adrese bağlanmak için bir Istemci sisteminden Beeline kullanma. Bu bağlantı bağlantı noktası `443`üzerinden yapılır ve TLS/SSL kullanılarak şifrelenir.

`clustername` değerini HDInsight kümenizin adıyla değiştirin. Kümenizin `admin` küme oturum açma hesabı ile değiştirin. ESP kümeleri için tam UPN 'yi kullanın (örneğin, user@domain.com). Küme `password` oturum açma hesabının parolasıyla değiştirin.

```bash
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p 'password'
```

veya özel uç nokta için:

```bash
beeline -u 'jdbc:hive2://clustername-int.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p 'password'
```

Özel uç noktalar, yalnızca aynı bölgedeki VNET 'lerden erişilebilen temel bir yük dengeleyiciye işaret ediyor. Daha fazla bilgi için bkz. [genel VNET eşlemesi ve yük dengeleyiciler üzerindeki kısıtlamalar](../../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) . Beeline kullanmadan önce `curl` ortak veya `-v` özel uç noktalarla bağlantı sorunlarını gidermek için komutunu kullanın.

---

### <a name="use-beeline-with-apache-spark"></a>Apache Spark ile Beeline kullanma

Apache Spark, bazı durumlarda Spark Thrift sunucusu olarak da adlandırılan kendi HiveServer2 uygulamasını sağlar. Bu hizmet, Hive yerine sorguları çözümlemek için Spark SQL 'i kullanır. Ve sorgunuza göre daha iyi performans sağlayabilir.

#### <a name="through-public-or-private-endpoints"></a>Ortak veya özel uç noktalar aracılığıyla

Kullanılan bağlantı dizesi biraz farklı. `httpPath=/hive2` Yerine kullanır `httpPath/sparkhive2`. `clustername` değerini HDInsight kümenizin adıyla değiştirin. Kümenizin `admin` küme oturum açma hesabı ile değiştirin. ESP kümeleri için tam UPN 'yi kullanın (örneğin, user@domain.com). Küme `password` oturum açma hesabının parolasıyla değiştirin.

```bash
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p 'password'
```

veya özel uç nokta için:

```bash
beeline -u 'jdbc:hive2://clustername-int.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p 'password'
```

Özel uç noktalar, yalnızca aynı bölgedeki VNET 'lerden erişilebilen temel bir yük dengeleyiciye işaret ediyor. Daha fazla bilgi için bkz. [genel VNET eşlemesi ve yük dengeleyiciler üzerindeki kısıtlamalar](../../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) . Beeline kullanmadan önce `curl` ortak veya `-v` özel uç noktalarla bağlantı sorunlarını gidermek için komutunu kullanın.

---

#### <a name="from-cluster-head-or-inside-azure-virtual-network-with-apache-spark"></a>Apache Spark ile küme kafası veya Azure sanal ağı içinden

Doğrudan küme baş düğümünden veya HDInsight kümesiyle aynı Azure sanal ağı içindeki bir kaynaktan bağlantı `10002` kurulurken, yerine Spark Thrift sunucusu için kullanılmalıdır. `10001` Aşağıdaki örnek, baş düğüme doğrudan nasıl bağlanılacağını gösterir:

```bash
/usr/hdp/current/spark2-client/bin/beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'
```

---

## <a name="prerequisites-for-examples"></a>Örnek önkoşulları

* HDInsight üzerinde bir Hadoop kümesi. Bkz. [Linux 'Ta HDInsight kullanmaya başlama](./apache-hadoop-linux-tutorial-get-started.md).

* Kümenizin birincil depolama alanı için URI düzenine dikkat edin. Örneğin, `wasb://` Azure depolama `abfs://` için, Azure Data Lake Storage 2. veya `adl://` Azure Data Lake Storage 1. için. Azure depolama için güvenli aktarım etkinse URI olur `wasbs://`. Daha fazla bilgi için bkz. [Güvenli aktarım](../../storage/common/storage-require-secure-transfer.md).

* Seçenek 1: bir SSH istemcisi. Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md). Bu belgedeki adımların çoğu, kümeye bir SSH oturumundan Beeline kullandığınızı varsayar.

* Seçenek 2: yerel bir Beeline istemcisi.

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

Bu örnek, bir SSH bağlantısından Beeline istemcisinin kullanılmasına dayalıdır.

1. Aşağıdaki kodla kümeye bir SSH bağlantısı açın. `sshuser` değerini, kümenizin SSH kullanıcısı ile, `CLUSTERNAME` değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcı hesabının parolasını girin.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Aşağıdaki komutu girerek, açık SSH oturumunuzla Beeline istemcinizden HiveServer2 'e bağlanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

3. Beeline Commands bir `!` karakterle başlar, örneğin `!help` yardımı görüntüler. `!` Ancak bazı komutlar için atlanabilir. Örneğin, `help` Ayrıca işe yarar.

    `!sql`HiveQL deyimlerini yürütmek için kullanılan. Ancak, HiveQL, önceki `!sql`bir sürümünü atlamanızı sağlamak için yaygın olarak kullanılır. Aşağıdaki iki deyim eşdeğerdir:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Yeni bir kümede yalnızca bir tablo listelenir: **hivesampletable**.

4. Hivesampletable için şemayı göstermek üzere aşağıdaki komutu kullanın:

    ```hiveql
    describe hivesampletable;
    ```

    Bu komut aşağıdaki bilgileri döndürür:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Bu bilgiler tablodaki sütunları açıklar.

5. HDInsight kümesiyle birlikte sunulan örnek verileri kullanarak **log4jLogs** adlı bir tablo oluşturmak için aşağıdaki deyimlerini girin: (URI şemanız temelinde gerektiği şekilde gözden geçirin.)

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
    STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs
        WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log'
        GROUP BY t4;
    ```

    Bu deyimler aşağıdaki işlemleri yapılır:

    |Deyim |Açıklama |
    |---|---|
    |TABLOYU BıRAK|Tablo varsa, silinir.|
    |DıŞ TABLO OLUŞTUR|Hive içinde bir **dış** tablo oluşturur. Dış tablolar yalnızca tablo tanımını Hive içinde depolar. Veriler özgün konumda bırakılır.|
    |SATıR BIÇIMI|Verilerin biçimlendirilmesi. Bu durumda, her günlükteki alanlar boşlukla ayrılır.|
    |TEXTFILE KONUMU OLARAK DEPOLANDı|Verilerin depolandığı yer ve dosya biçimi.|
    |SELECT|**T4** sütununun **[Error]** değerini içerdiği tüm satırların sayısını seçer. Bu sorgu, bu değeri içeren üç satır olduğu için **3** değerini döndürür.|
    |'%. Log ' gıbı INPUT__FILE__NAME|Hive, şemayı dizindeki tüm dosyalara uygulamayı dener. Bu durumda, Dizin şemayla eşleşmeyen dosyaları içerir. Sonuçlarda çöp verilerinin oluşmasını engellemek için, bu ifade Hive 'ye yalnızca. log ile biten dosyalardaki verileri döndürmeyeceğini söyler.|

   > [!NOTE]  
   > Dış tablolar, temel alınan verilerin bir dış kaynak tarafından güncelleştirilmesini beklediğinde kullanılmalıdır. Örneğin, otomatik bir veri yükleme işlemi veya MapReduce işlemi.
   >
   > Dış tablonun atılması, yalnızca tablo tanımı olan **verileri silmez.**

    Bu komutun çıktısı aşağıdaki metne benzer:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

6. Beeline çıkış:

    ```bash
    !exit
    ```

## <a name="run-a-hiveql-file"></a>HiveQL dosyası çalıştırma

Bu örnek, önceki örnekteki devamlılık örneğidir. Bir dosya oluşturmak için aşağıdaki adımları kullanın, ardından Beeline kullanarak çalıştırın.

1. **Query. HQL**adlı bir dosya oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano query.hql
    ```

1. Dosyanın içeriği olarak aşağıdaki metni kullanın. Bu sorgu **errorlogs**adlı yeni bir ' internal ' tablosu oluşturur:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Bu deyimler aşağıdaki işlemleri yapılır:

    |Deyim |Açıklama |
    |---|---|
    |MEVCUT DEĞILSE CREATE TABLE|Tablo henüz yoksa, oluşturulur. **Dış** anahtar sözcük kullanılmadığından, bu ifade bir iç tablo oluşturur. İç tablolar Hive veri ambarında depolanır ve tamamen Hive tarafından yönetilir.|
    |ORC OLARAK DEPOLANıR|Verileri Iyileştirilmiş satır sütunlu (ORC) biçimde depolar. ORC biçimi, Hive verilerinin depolanması için yüksek düzeyde iyileştirilmiş ve verimli bir biçimdir.|
    |ÜZERINE YAZMA EKLE... SEÇIN|**Log4jLogs** tablosundan **[Error]** içeren satırları seçer, ardından verileri **errorlogs** tablosuna ekler.|

    > [!NOTE]  
    > Dış tablolardan farklı olarak, iç tablo bırakılırken temel alınan veriler de silinir.

1. Dosyayı kaydetmek için **CTRL**+**X**kullanın, **Y**girin ve son olarak **girin**.

1. Dosyayı Beeline kullanarak çalıştırmak için aşağıdakileri kullanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]  
    > `-i` Parametresi, Beeline başlar ve `query.hql` dosyadaki deyimleri çalıştırır. Sorgu tamamlandıktan sonra, `jdbc:hive2://headnodehost:10001/>` istemde bir uyarı alırsınız. Ayrıca, sorgu tamamlandıktan sonra Beeline çıkış `-f` eden parametresini kullanarak bir dosya çalıştırabilirsiniz.

1. **Errorlogs** tablosunun oluşturulduğunu doğrulamak Için, **hata günlüklerinden**tüm satırları döndürmek için aşağıdaki ifadeyi kullanın:

    ```hiveql
    SELECT * from errorLogs;
    ```

    T4 sütununda **[Error]** içeren, üç satırlık verilerin döndürülmesi gerekir:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (0.813 seconds)

## <a name="install-beeline-client"></a>Beeline istemcisini yükler

Baş düğümlere Beeline dahil edilse de, yerel olarak yüklemek isteyebilirsiniz.  Yerel bir makineye yönelik Install adımları [Linux Için Windows alt sistemine](https://docs.microsoft.com/windows/wsl/install-win10)dayanır.

1. Paket listelerini güncelleştirme. Bash kabuğunuzun aşağıdaki komutunu girin:

    ```bash
    sudo apt-get update
    ```

1. Yüklü değilse Java 'Yı yükler. `which java` Komutuyla kontrol edebilirsiniz.

    1. Java paketi yüklü değilse, aşağıdaki komutu girin:

        ```bash
        sudo apt install openjdk-11-jre-headless
        ```

    1. Bashrc dosyasını açın (genellikle ~/,bashrc içinde bulunur): `nano ~/.bashrc`.

    1. Bashrc dosyasını düzeltme. Dosyanın sonuna aşağıdaki satırı ekleyin:

        ```bash
        export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64
        ```

        Sonra **CTRL + X**ve **Y**tuşlarına basın ve ardından girin.

1. Hadoop ve Beeline arşivlerini indirin, aşağıdaki komutları girin:

    ```bash
    wget https://archive.apache.org/dist/hadoop/core/hadoop-2.7.3/hadoop-2.7.3.tar.gz
    wget https://archive.apache.org/dist/hive/hive-1.2.1/apache-hive-1.2.1-bin.tar.gz
    ```

1. Arşivlerin paketini açın, aşağıdaki komutları girin:

    ```bash
    tar -xvzf hadoop-2.7.3.tar.gz
    tar -xvzf apache-hive-1.2.1-bin.tar.gz
    ```

1. Bashrc dosyasını daha fazla düzeltme. Arşivlerin açıldığı yolu belirlemeniz gerekir. [Linux Için Windows alt sistemi](https://docs.microsoft.com/windows/wsl/install-win10)kullanıyorsanız ve adımları tam olarak takip ediyorsanız, yolunuzda `/mnt/c/Users/user/` `user` Kullanıcı adınız olur.

    1. Dosyayı açın:`nano ~/.bashrc`

    1. Aşağıdaki komutları uygun yol ile değiştirin ve ardından bashrc dosyasının sonuna girin:

        ```bash
        export HADOOP_HOME=/path_where_the_archives_were_unpacked/hadoop-2.7.3
        export HIVE_HOME=/path_where_the_archives_were_unpacked/apache-hive-1.2.1-bin
        PATH=$PATH:$HIVE_HOME/bin
        ```

    1. Sonra **CTRL + X**ve **Y**tuşlarına basın ve ardından girin.

1. Bash oturumunuzu kapatıp yeniden açın.

1. Bağlantınızı test edin. Yukarıdaki [ortak veya özel uç noktalardan](#over-public-or-private-endpoints)bağlantı biçimini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* HDInsight 'ta Hive hakkında daha fazla genel bilgi için bkz. [HDInsight 'ta Apache Hadoop ile Apache Hive kullanma](hdinsight-use-hive.md)

* HDInsight 'ta Hadoop ile çalışmanın diğer yolları hakkında daha fazla bilgi için bkz. [HDInsight 'Ta MapReduce 'yi Apache Hadoop kullanma](hdinsight-use-mapreduce.md)
