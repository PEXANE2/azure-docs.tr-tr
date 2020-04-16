---
title: Öğretici - Azure HDInsight'ta Apache HBase'i kullanın
description: HDInsight'ta hadoop kullanmaya başlamak için bu Apache HBase öğreticisini izleyin. HBase kabuğundan tablolar oluşturun ve Hive kullanarak bunları sorgulayın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 04/14/2020
ms.openlocfilehash: a601d54ebda074a25a988ac2a115f6418dd5c7ee
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81390271"
---
# <a name="tutorial-use-apache-hbase-in-azure-hdinsight"></a>Öğretici: Azure HDInsight'ta Apache HBase'i kullanın

Bu öğretici, Azure HDInsight'ta Bir Apache HBase kümesinin nasıl oluşturulacağını, HBase tablolarının nasıl oluşturulacağını ve Apache Hive'ı kullanarak sorgu tablolarının nasıl oluşturulacağını gösterir.  Genel HBase bilgileri için bkz. [HDInsight HBase’e genel bakış](./apache-hbase-overview.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Apache HBase kümesi oluşturma
> * HBase tabloları oluşturma ve veri ekleme
> * Apache HBase sorgusu için Apache Hive kullanın
> * Curl kullanarak HBase REST API’lerini kullanma
> * Küme durumunu denetleme

## <a name="prerequisites"></a>Ön koşullar

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

* Bash, ne oldu? Bu makaledeki örnekler, curl komutları için Windows 10'daki Bash kabuğunu kullanır. Yükleme adımları [için Windows 10 için Linux Yükleme Kılavuzu](https://docs.microsoft.com/windows/wsl/install-win10) için Windows Alt Sistemi'ne bakın.  Diğer [Unix kabukları](https://www.gnu.org/software/bash/) da çalışacaktır.  Bazı küçük değişikliklerle birlikte kıvırma örnekleri, Windows Komutu komut istemi üzerinde çalışabilir.  Veya Windows PowerShell cmdlet [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-restmethod)kullanabilirsiniz.

## <a name="create-apache-hbase-cluster"></a>Apache HBase kümesi oluşturma

Aşağıdaki yordam, Bir HBase kümesi oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanır. Şablon ayrıca bağımlı varsayılan Azure Depolama hesabı oluşturur. Yordamda ve diğer küme oluşturma yöntemlerinde kullanılan parametreleri anlamak için bkz. [HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md).

1. Azure portalında şablonu açmak için aşağıdaki resmi seçin. Şablon Azure [quickstart şablonlarında](https://azure.microsoft.com/resources/templates/)bulunur.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-tutorial-get-started-linux/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

2. Özel **dağıtım** iletişim kutusundan aşağıdaki değerleri girin:

    |Özellik |Açıklama |
    |---|---|
    |Abonelik|Kümeoluşturmak için kullanılan Azure aboneliğinizi seçin.|
    |Kaynak grubu|Azure Kaynak yönetim grubu oluşturun veya varolan bir grup kullanın.|
    |Konum|Kaynak grubunun konumunu belirtin. |
    |ClusterName|HBase kümesi için bir ad girin.|
    |Küme oturum açma adı ve parolası|Varsayılan giriş adı **yöneticidir.**|
    |SSH kullanıcı adı ve parolası|Varsayılan kullanıcı adı **sshuser** şeklindedir.|

    Diğer parametreler isteğe bağlıdır.  

    Her kümenin bir Azure Depolama hesabı bağımlılığı vardır. Bir kümeyi sildikten sonra veriler depolama hesabında kalır. Kümenin varsayılan depolama hesabı adı, "depo" ifadesi eklenmiş küme adıdır. Şablon değişkenler bölümünde kodlanmış.

3. **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**seçin ve ardından Satın **Alma'yı**seçin. Bir küme oluşturmak yaklaşık 20 dakika sürer.

Bir HBase kümesi silindikten sonra aynı varsayılan blob kapsayıcısını kullanarak başka bir HBase kümesi oluşturabilirsiniz. Yeni küme özgün kümede oluşturduğunuz HBase tablolarını seçer. Tutarsızlıkları önlemek için kümeyi silmeden önce HBase tablolarını devre dışı bırakmanız önerilir.

## <a name="create-tables-and-insert-data"></a>Tablo oluşturma ve veri ekleme

HBase kümelerine bağlanmak için SSH'yi ve ardından HBase tabloları, veri eklemek ve sorgu verileri oluşturmak için [Apache HBase Shell'i](https://hbase.apache.org/0.94/book/shell.html) kullanabilirsiniz.

Çoğu kişi için veriler tablo biçiminde görünür:

![HDInsight Apache HBase tabular veri](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png)

HBase 'de [(Cloud BigTable'ın](https://cloud.google.com/bigtable/)bir uygulaması) aynı veriler aşağıdaki gibi görünür:

![HDInsight Apache HBase BigTable verileri](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png)

**HBase kabuğunu kullanmak için**

1. HBase kümenize bağlanmak için komutu kullanın. `ssh` Kümenizin adını değiştirerek `CLUSTERNAME` aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. HBase etkileşimli kabuğunu başlatmak için komutu kullanın. `hbase shell` SSH bağlantınızda aşağıdaki komutu girin:

    ```bash
    hbase shell
    ```

1. İki `create` sütunlu ailelerden bir HBase tablosu oluşturmak için komutu kullanın. Tablo ve sütun adları büyük/küçük harf duyarlıdır. Aşağıdaki komutu girin:

    ```hbaseshell
    create 'Contacts', 'Personal', 'Office'
    ```

1. HBase'deki tüm tabloları listelemek için komutu kullanın. `list` Aşağıdaki komutu girin:

    ```hbase
    list
    ```

1. Belirli `put` bir tabloda belirli bir satırda belirli bir sütunda değerleri eklemek için komutu kullanın. Aşağıdaki komutları girin:

    ```hbaseshell
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    ```

1. Tablo `scan` verilerini tarayıp döndürmek `Contacts` için komutu kullanın. Aşağıdaki komutu girin:

    ```hbase
    scan 'Contacts'
    ```

    ![HDInsight Apache Hadoop HBase kabuk](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png)

1. Bir `get` satırın içeriğini getirmek için komutu kullanın. Aşağıdaki komutu girin:

    ```hbaseshell
    get 'Contacts', '1000'
    ```

    Yalnızca bir satır olduğundan, benzer sonuçları komutu `scan` kullanmak olarak görürsünüz.

    HBase tablo şeması hakkında daha fazla bilgi için, [Bkz. Apache HBase Şema Tasarıma Giriş](http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf). HBase komutları hakkında daha fazla bilgi için bkz. [Apache HBase başvuru kılavuzu](https://hbase.apache.org/book.html#quickstart).

1. HBase etkileşimli kabuğunu durdurmak için komutu kullanın. `exit` Aşağıdaki komutu girin:

    ```hbaseshell
    exit
    ```

**Verileri kişi HBase tablosuna toplu olarak yüklemek için**

HBase’de verileri tablolara yüklemek için bazı yöntemler vardır.  Daha fazla bilgi için bkz. [Toplu yükleme](https://hbase.apache.org/book.html#arch.bulk.load).

Örnek veri dosyası, ortak blob kapsayıcısı `wasb://hbasecontacts\@hditutorialdata.blob.core.windows.net/contacts.txt` içinde bulunabilir.  Veri dosyasının içeriği şudur:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

İsterseniz, bir metin dosyası oluşturabilir ve dosyayı kendi depolama hesabınıza yükleyebilirsiniz. Talimatlar için, [HDInsight'taki Apache Hadoop işleri için veri yükleme'ye](../hdinsight-upload-data.md)bakın.

Bu yordam, `Contacts` son yordamda oluşturduğunuz HBase tablosunu kullanır.

1. Açık ssh bağlantınızdan, veri dosyasını StoreFiles'a dönüştürmek ve ' tarafından `Dimporttsv.bulk.output`belirtilen göreceli bir yolda depolamak için aşağıdaki komutu çalıştırın.

    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. Verileri HBase tablosuna `/example/data/storeDataFileOutput` yüklemek için aşağıdaki komutu çalıştırın:

    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. HBase kabuğunu açabilir ve tablo `scan` içeriğini listelemek için komutu kullanabilirsiniz.

## <a name="use-apache-hive-to-query-apache-hbase"></a>Apache HBase sorgusu için Apache Hive kullanın

[Apache Hive](https://hive.apache.org/)kullanarak HBase tablolarında verileri sorgulayabilirsiniz. Bu bölümde HBase tablosuyla eşlenen bir Hive tablosu oluşturur ve HBase tablosunda verileri sorgulamak için kullanırsınız.

1. Açık ssh bağlantınızdan, Beeline'i başlatmak için aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Beeline hakkında daha fazla bilgi için bkz. [Beeline ile HDInsight’ta Hadoop ile Hive kullanma](../hadoop/apache-hadoop-use-hive-beeline.md).

1. HBase tablosuyla eşşen bir Hive tablosu oluşturmak için aşağıdaki [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) komut dosyasını çalıştırın. Bu bildirimi çalıştırmadan önce HBase kabuğunu kullanarak bu makalede daha önce başvurulan örnek tabloyu oluşturduğunuzdan emin olun.

    ```hiveql
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

1. HBase tablosundaki verileri sorgulamak için aşağıdaki HiveQL betiğini çalıştırın:

    ```hiveql
    SELECT count(rowkey) AS rk_count FROM hbasecontacts;
    ```

1. Beeline çıkmak için, kullanın. `!exit`

1. ssh bağlantınızdan çıkmak `exit`için .

## <a name="use-hbase-rest-apis-using-curl"></a>Curl kullanarak HBase REST API’lerini kullanma

REST API’sinin güvenliği [temel kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesi için istekleri her zaman Güvenli HTTP (HTTPS) kullanarak yapmalısınız.

1. Kullanım kolaylığı için ortam değişkenini ayarlayın. Küme giriş parolasını değiştirerek `MYPASSWORD` aşağıdaki komutları düzenleme. HBase kümenizin adıyla değiştirin. `MYCLUSTERNAME` Sonra komutları girin.

    ```bash
    export password='MYPASSWORD'
    export clustername=MYCLUSTERNAME
    ```

1. Mevcut HBase tablolarını listelemek için şu komutu kullanın:

    ```bash
    curl -u admin:$password \
    -G https://$clustername.azurehdinsight.net/hbaserest/
    ```

1. İki sütunlu aileler içeren yeni bir HBase tablosu oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    curl -u admin:$password \
    -X PUT "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    Şema JSon biçiminde sağlanır.
1. Bazı verileri eklemek için aşağıdaki komutu kullanın:

    ```bash
    curl -u admin:$password \
    -X PUT "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```

    Base64 -d anahtarında belirtilen değerleri kodlar. Örnekte:

   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ==: Kişisel: İsim
   * Sm9obiBEb2xl: John Dole

     [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) birden fazla (toplu) değer eklemenizi sağlar.

1. Bir satır almak için aşağıdaki komutu kullanın:

    ```bash
    curl -u admin:$password \
    GET "https://$clustername.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

HBase Rest hakkında daha fazla bilgi için bkz. [Apache HBase Başvuru Kılavuzu](https://hbase.apache.org/book.html#_rest).

> [!NOTE]  
> Thrift, HDInsight’ta HBase tarafından desteklenmez.
>
> Curl’ü veya WebHCat ile başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adı ve parolasını sağlayarak isteklerin kimliğini doğrulamanız gerekir. Ayrıca, sunucuya istek göndermek için kullanılan Tekdüzen Kaynak Tanımlayıcısı’nın (URI) bir parçası olarak küme adını kullanmanız gerekir:
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    Aşağıdakine benzer bir yanıt almanız gerekir:
>   
>        {"status":"ok","version":"v1"}

## <a name="check-cluster-status"></a>Küme durumunu denetleme

HDInsight içinde HBase, kümelerin izlenmesi için bir Web Kullanıcı Arabirimi ile birlikte gönderilir. Web Kullanıcı Arabirimini kullanarak istatistikler veya bölgeler hakkında bilgi isteyebilirsiniz.

**HBase Master Kullanıcı Arabirimi’ne erişmek için**

1. HBase kümenizin adının `https://CLUSTERNAME.azurehdinsight.net` bulunduğu `CLUSTERNAME` ambari Web UI'de oturum açın.

1. Sol menüden **HBase'i** seçin.

1. Sayfanın üst **kısmındahızlı bağlantıları** seçin, etkin Zookeeper düğüm bağlantısını işaret ve ardından **HBase Master UI'yi**seçin.  Kullanıcı arabirimi başka bir tarayıcı sekmesinde açılır:

   ![HDInsight Apache HBase HMaster UI](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

   HBase Master Kullanıcı Arabirimi aşağıdaki bölümleri içerir:

   - Bölge sunucuları
   - Yedekleme yöneticileri
   - Tablolar
   - Görevler
   - Yazılım öznitelikleri

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tutarsızlıkları önlemek için kümeyi silmeden önce HBase tablolarını devre dışı bırakmanız önerilir. HBase komutunu `disable 'Contacts'`kullanabilirsiniz. Bu uygulamayı kullanmaya devam etmeyecekseniz, oluşturduğunuz HBase kümesini aşağıdaki adımlarla silin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Üstteki **Arama** kutusuna **HDInsight**yazın.
1. **Hizmetler**altında **HDInsight kümelerini** seçin.
1. Görünen HDInsight kümeleri listesinde, bu öğretici için oluşturduğunuz kümenin yanındaki **...** 'yı tıklatın.
1. **Sil'i**tıklatın. **Evet'i**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Apache HBase kümesi oluşturmayı öğrendiniz. Ve nasıl tablolar oluşturmak ve HBase kabuk bu tablolarda verileri görüntülemek için. Ayrıca, HBase tablolarındaki verilerde Hive sorgusunun nasıl kullanılacağını da öğrendiniz. HBase tablosu oluşturmak ve tablodan veri almak için HBase C# REST API'leri nasıl kullanılır? Daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [HDInsight HBase'e genel bakış](./apache-hbase-overview.md)