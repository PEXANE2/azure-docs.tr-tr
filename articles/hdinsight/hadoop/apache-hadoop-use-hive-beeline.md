---
title: Apache Hive ile Apache Beeline kullanın - Azure HDInsight
description: HDInsight'ta Hadoop ile Hive sorgularını çalıştırmak için Beeline istemcisini nasıl kullanacağınızı öğrenin. Beeline JDBC üzerinden HiveServer2 ile çalışmak için bir yardımcı programdır.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/09/2020
ms.openlocfilehash: 4f2b192765aab4c7cf18c62988ae2f6080b4b17c
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436900"
---
# <a name="use-the-apache-beeline-client-with-apache-hive"></a>Apache Beeline istemcisini Apache Hive ile kullanma

Apache Beeline'i HDInsight'ta [Apache](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) Hive sorgularını çalıştırmak için nasıl kullanacağınızı öğrenin.

Beeline, HDInsight kümenizin baş düğümlerinde yer alan bir Hive istemcisidir. Beeline'i yerel olarak yüklemek için aşağıdaki [beeline istemcisini yükle'ye](#install-beeline-client)bakın. Beeline, HDInsight kümenizde barındırılan hiveServer2'ye bağlanmak için JDBC'yi kullanır. Ayrıca Internet üzerinden UZAKTAN HDInsight hive erişmek için Beeline kullanabilirsiniz. Aşağıdaki örnekler, Beeline'den HDInsight'a bağlanmak için kullanılan en yaygın bağlantı dizelerini sağlar.

## <a name="types-of-connections"></a>Bağlantı türleri

### <a name="from-an-ssh-session"></a>Bir SSH oturumundan

Bir SSH oturumundan küme başlığına bağlanırken, bağlantı noktasındaki `headnodehost` `10001`adrese bağlanabilirsiniz:

```bash
beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
```

---

### <a name="over-an-azure-virtual-network"></a>Azure Sanal Ağ Üzerinden

Bir istemciden Bir Azure Sanal Ağı üzerinden HDInsight'a bağlanırken, küme kafa düğümünün tam nitelikli etki alanı adını (FQDN) sağlamanız gerekir. Bu bağlantı doğrudan küme düğümlerine yapıldığından, bağlantı `10001`bağlantı noktasını kullanır:

```bash
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Küme `<headnode-FQDN>` başlığının tam nitelikli etki alanı adı ile değiştirin. Bir başlık tam nitelikli etki alanı adını bulmak [için, Apache Ambari REST API belgesini kullanarak HDInsight yönet'deki](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) bilgileri kullanın.

---

### <a name="to-hdinsight-enterprise-security-package-esp-cluster-using-kerberos"></a>Kerberos kullanarak HDInsight Kurumsal Güvenlik Paketi (ESP) kümesine

İstemciden bir Kurumsal Güvenlik Paketi (ESP) kümesine bağlanArak kümenin aynı alanında bulunan bir makinede Azure Active Directory (AAD)-DS'ye katıldığınızda, kümeye `<AAD-Domain>` `<username>`erişim izni olan etki alanı kullanıcı hesabının etki alanı adını ve adını da belirtmeniz gerekir:

```bash
kinit <username>
beeline -u 'jdbc:hive2://<headnode-FQDN>:10001/default;principal=hive/_HOST@<AAD-Domain>;auth-kerberos;transportMode=http' -n <username>
```

Kümeye erişmek için izinlerle etki alanında bir hesabın adı ile değiştirin. `<username>` Kümenin birleştiği Azure Etkin Dizini (AAD) adıyla değiştirin. `<AAD-DOMAIN>` `<AAD-DOMAIN>` Değer için bir büyük harf dizesi kullanın, aksi takdirde kimlik bilgisi bulunamaz. Gerekirse `/etc/krb5.conf` bölge adlarını kontrol edin.

Ambari'deki JDBC URL'sini bulmak için:

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/#/main/services/HIVE/summary`kümenizin adı nerede' `CLUSTERNAME` ye gidin. HiveServer2'nin çalıştığını sağlayın.

1. HiveServer2 JDBC URL'sini kopyalamak için panoyu kullanın.

---

### <a name="over-public-or-private-endpoints"></a>Genel veya özel uç noktalar üzerinden

Ortak veya özel uç noktaları kullanarak bir kümeye bağlanırken, küme giriş `admin`hesabı adını (varsayılan) ve parolayı sağlamanız gerekir. Örneğin, adrese bağlanmak için istemci sisteminden `clustername.azurehdinsight.net` Beeline'i kullanma. Bu bağlantı bağlantı `443`noktası üzerinden yapılır ve TLS/SSL kullanılarak şifrelenir.

`clustername` değerini HDInsight kümenizin adıyla değiştirin. Kümeniz için küme giriş hesabıile değiştirin. `admin` ESP kümeleri için tam UPN'yi user@domain.comkullanın (örneğin, ). Küme `password` giriş hesabının parolasıyla değiştirin.

```bash
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p 'password'
```

veya özel bitiş noktası için:

```bash
beeline -u 'jdbc:hive2://clustername-int.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p 'password'
```

Özel uç noktalar, yalnızca aynı bölgede görünen VNET'lerden erişilebilen temel bir yük dengeleyicisini işaret eder. Daha fazla bilgi için [küresel VNet eşleme ve yük dengeleyicileri üzerindeki kısıtlamalara](../../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) bakın. Beeline kullanmadan `-v` önce ortak veya özel uç noktaları ile herhangi bir bağlantı sorunları gidermek için seçeneği ile `curl` komutu kullanabilirsiniz.

---

### <a name="use-beeline-with-apache-spark"></a>Apache Spark ile Beeline kullanın

Apache Spark, bazen Spark Thrift sunucusu olarak da adlandırılan HiveServer2'nin kendi uygulamasını sağlar. Bu hizmet, Sorguları Hive yerine çözmek için Spark SQL kullanır ve sorgunuza bağlı olarak daha iyi performans sağlayabilir.

#### <a name="through-public-or-private-endpoints"></a>Genel veya özel uç noktalar aracılığıyla

Kullanılan bağlantı dizesi biraz farklıdır. Onu kontrol `httpPath=/hive2` altına `httpPath/sparkhive2`almak yerine. `clustername` değerini HDInsight kümenizin adıyla değiştirin. Kümeniz için küme giriş hesabıile değiştirin. `admin` ESP kümeleri için tam UPN'yi user@domain.comkullanın (örneğin, ). Küme `password` giriş hesabının parolasıyla değiştirin.

```bash
beeline -u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p 'password'
```

veya özel bitiş noktası için:

```bash
beeline -u 'jdbc:hive2://clustername-int.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/sparkhive2' -n admin -p 'password'
```

Özel uç noktalar, yalnızca aynı bölgede görünen VNET'lerden erişilebilen temel bir yük dengeleyicisini işaret eder. Daha fazla bilgi için [küresel VNet eşleme ve yük dengeleyicileri üzerindeki kısıtlamalara](../../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers) bakın. Beeline kullanmadan `-v` önce ortak veya özel uç noktaları ile herhangi bir bağlantı sorunları gidermek için seçeneği ile `curl` komutu kullanabilirsiniz.

---

#### <a name="from-cluster-head-or-inside-azure-virtual-network-with-apache-spark"></a>Küme başından veya Apache Spark ile Azure Sanal Ağı'nın içinden

Doğrudan küme kafası düğümünden veya HDInsight kümesiyle aynı Azure Sanal Ağı içindeki bir `10002` kaynaktan bağlanırken, bağlantı noktası `10001`Spark Thrift sunucusu yerine kullanılmalıdır. Aşağıdaki örnek, kafa düğümüne doğrudan nasıl bağlanılabildiğini gösterir:

```bash
/usr/hdp/current/spark2-client/bin/beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'
```

---

## <a name="prerequisites-for-examples"></a>Örnekler için ön koşullar

* HDInsight'ta hadoop kümesi. [Linux'ta HDInsight ile başlayın](./apache-hadoop-linux-tutorial-get-started.md)bakın.

* Kümenizin birincil depolama alanı için [URI düzenine](../hdinsight-hadoop-linux-information.md#URI-and-scheme) dikkat edin. Örneğin, `wasb://` Azure Depolama `abfs://` için, Azure Veri Gölü `adl://` Depolama Gen2 için veya Azure Veri Gölü Depolama Gen1 için. Azure Depolama için güvenli aktarım etkinleştirilirse, `wasbs://`URI. Daha fazla bilgi için [güvenli aktarım atarım'a](../../storage/common/storage-require-secure-transfer.md)bakın.

* Seçenek 1: Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın. Bu belgedeki adımların çoğu, Bir SSH oturumundan kümeye Beeline kullandığınızı varsayar.

* Seçenek 2: Yerel bir Beeline istemcisi.

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

Bu örnek, Bir SSH bağlantısından Beeline istemcisi kullanarak dayanmaktadır.

1. Aşağıdaki kodla kümeye bir SSH bağlantısı açın. `sshuser` değerini, kümenizin SSH kullanıcısı ile, `CLUSTERNAME` değerini kümenizin adıyla değiştirin. İstendiğinde, SSH kullanıcı hesabının parolasını girin.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Aşağıdaki komutu girerek açık SSH oturumunuzdan Beeline istemcinizle HiveServer2'ye bağlanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

3. Beeline komutları bir `!` karakterle `!help` başlar, örneğin yardım görüntüler. `!` Ancak bazı komutlar için atlanabilir. Örneğin, `help` aynı zamanda çalışır.

    HiveQL `!sql`ifadelerini yürütmek için kullanılan bir şey var. Ancak, HiveQL o kadar yaygın olarak kullanılır ki, `!sql`önceki . Aşağıdaki iki ifade eşdeğerdir:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Yeni bir kümede yalnızca bir tablo listelenir: **kovan örneği.**

4. Kovan örnekleme sehpası için şemayı görüntülemek için aşağıdaki komutu kullanın:

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

    Bu bilgiler, tablodaki sütunları açıklar.

5. HDInsight kümesiyle sağlanan örnek verileri kullanarak **log4jLogs** adlı bir tablo oluşturmak için aşağıdaki ifadeleri girin: [(URI şemanıza](../hdinsight-hadoop-linux-information.md#URI-and-scheme)göre gerektiği gibi gözden geçirin .)

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

    Bu ifadeler aşağıdaki eylemleri yapmak:

    |Deyim |Açıklama |
    |---|---|
    |DAMLA TABLOSU|Tablo varsa, silinir.|
    |DıŞ TABLO OLUŞTURMA|Hive'da **harici** bir tablo oluşturur. Dış tablolar yalnızca Hive tablo tanımını depolar. Veriler özgün konumda bırakılır.|
    |SATıR BIÇIMI|Verilerin nasıl biçimlendirilir. Bu durumda, her günlükteki alanlar bir boşlukla ayrılır.|
    |TEXTFILE KONUMU OLARAK DEPOLANAN|Verilerin nerede ve hangi dosya biçiminde depolandığı.|
    |SELECT|**T4** sütununun **[HATA]** değerini içerdiği tüm satırların sayısını seçer. Bu değeri içeren üç satır olduğundan, bu sorgu **3** değerini döndürür.|
    |INPUT__FILE__NAME LIKE '%.log'|Hive şemayı dizindeki tüm dosyalara uygulamaya çalışır. Bu durumda, dizin şema eşleşmiyor dosyaları içerir. Sonuçlardaki çöp verilerini önlemek için bu deyim Hive'a yalnızca .log ile biten dosyalardan veri döndürmesi gerektiğini söyler.|

   > [!NOTE]  
   > Temel verilerin harici bir kaynak tarafından güncelleştirilmesini beklerken dış tablolar kullanılmalıdır. Örneğin, otomatik bir veri yükleme işlemi veya MapReduce işlemi.
   >
   > Harici bir tablonun düşmesi verileri **silmez,** yalnızca tablo tanımını siler.

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

6. Çıkış Beeline:

    ```bash
    !exit
    ```

## <a name="run-a-hiveql-file"></a>HiveQL dosyayı çalıştırma

Bu önceki örnekten bir devamıdır. Bir dosya oluşturmak için aşağıdaki adımları kullanın ve ardından Beeline kullanarak çalıştırın.

1. **query.hql**adlı bir dosya oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano query.hql
    ```

1. Aşağıdaki metni dosyanın içeriği olarak kullanın. Bu sorgu **errorLogs**adlı yeni bir 'iç' tablo oluşturur:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Bu ifadeler aşağıdaki eylemleri yapmak:

    |Deyim |Açıklama |
    |---|---|
    |VARSA TABLO OLUŞTUR|Tablo zaten yoksa, oluşturulur. **EXTERNAL** anahtar sözcüğü kullanılmadığından, bu deyim bir iç tablo oluşturur. Dahili tablolar Hive veri ambarında depolanır ve tamamen Hive tarafından yönetilir.|
    |ORK OLARAK DEPOLANAN|Verileri Optimize Edilmiş Satır Sütunu (ORC) biçiminde depolar. ORC biçimi, Kovan verilerini depolamak için son derece optimize edilmiş ve verimli bir biçimdir.|
    |ÜZERINE EKLEME ... Seçin|**[ERROR]** içeren **log4jLogs** tablosundan satırları seçer, ardından verileri **errorLogs** tablosuna ekler.|

    > [!NOTE]  
    > Dış tabloların aksine, dahili tablonun düşmesi temel verileri de siler.

1. Dosyayı kaydetmek için **Ctrl**+**X'i**kullanın, ardından **Y**girin ve son olarak **girin.**

1. Beeline kullanarak dosyayı çalıştırmak için aşağıdakileri kullanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]  
    > Parametre `-i` Beeline'i başlatır ve `query.hql` dosyadaki ifadeleri çalıştırın. Sorgu tamamlandıktan sonra, istem'e `jdbc:hive2://headnodehost:10001/>` ulaşırsınız. Sorgu tamamlandıktan sonra Beeline'den çıkan parametreyi `-f` kullanarak da dosya çalıştırabilirsiniz.

1. **HataLoglar** tablosunun oluşturulduğunu doğrulamak için, **hataGünlük'lerden**tüm satırları döndürmek için aşağıdaki deyimi kullanın:

    ```hiveql
    SELECT * from errorLogs;
    ```

    T4 sütununda **[ERROR]** içeren üç veri satırı döndürülmelidir:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (0.813 seconds)

## <a name="install-beeline-client"></a>Beeline istemcisi yükleme

Beeline, HDInsight kümenizin baş düğümlerine dahil olsa da, bunu yerel bir makineye yüklemek isteyebilirsiniz.  Beeline'i yerel bir makineye yüklemek için aşağıdaki adımlar Linux için bir [Windows Alt Sistemi'ni](https://docs.microsoft.com/windows/wsl/install-win10)temel almaktadır.

1. Paket listelerini güncelleştirin. Bash kabuğunuza aşağıdaki komutu girin:

    ```bash
    sudo apt-get update
    ```

1. Yüklü değilse Java'yı yükleyin. Komutu kontrol `which java` edebilirsiniz.

    1. Java paketi yüklü değilse, aşağıdaki komutu girin:

        ```bash
        sudo apt install openjdk-11-jre-headless
        ```

    1. Bashrc dosyasını açın (genellikle ~/.bashrc bulunur): `nano ~/.bashrc`.

    1. Bashrc dosyasını değiştirin. Dosyanın sonuna aşağıdaki satırı ekleyin:

        ```bash
        export JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64
        ```

        Sonra **Ctrl+X**tuşuna basın, sonra **Y**tuşuna basın, sonra girin.

1. Hadoop ve Beeline arşivlerini indirin, aşağıdaki komutları girin:

    ```bash
    wget https://archive.apache.org/dist/hadoop/core/hadoop-2.7.3/hadoop-2.7.3.tar.gz
    wget https://archive.apache.org/dist/hive/hive-1.2.1/apache-hive-1.2.1-bin.tar.gz
    ```

1. Arşivleri boşaltın, aşağıdaki komutları girin:

    ```bash
    tar -xvzf hadoop-2.7.3.tar.gz
    tar -xvzf apache-hive-1.2.1-bin.tar.gz
    ```

1. Bashrc dosyasını daha da değiştirin. Arşivlerin açılmakta olduğu yolu belirlemeniz gerekir. [Linux için Windows Alt Sistemi'ni](https://docs.microsoft.com/windows/wsl/install-win10)kullanıyorsanız ve adımları tam `/mnt/c/Users/user/`olarak `user` izlediyseniz, yolunuz kullanıcı adınız nerede olacaktır.

    1. Dosyayı açın:`nano ~/.bashrc`

    1. Aşağıdaki komutları uygun yol ile değiştirin ve bashrc dosyasının sonuna girin:

        ```bash
        export HADOOP_HOME=/path_where_the_archives_were_unpacked/hadoop-2.7.3
        export HIVE_HOME=/path_where_the_archives_were_unpacked/apache-hive-1.2.1-bin
        PATH=$PATH:$HIVE_HOME/bin
        ```

    1. Sonra **Ctrl+X**tuşuna basın, sonra **Y**tuşuna basın, sonra girin.

1. Kapatın ve sonra bash oturumunu yeniden açın.

1. Bağlantınızı test edin. [Yukarıdaki genel veya özel uç noktalardan bağlantı](#over-public-or-private-endpoints)biçimini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* HDInsight'ta Hive hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hdinsight-use-hive.md)

* HDInsight'ta Hadoop ile çalışabileceğiniz diğer yollar hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)
