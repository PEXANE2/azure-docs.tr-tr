---
title: Apache Hive ve Apache Pig ile Python UDF - Azure HDInsight
description: Azure'daki Apache Hadoop teknoloji yığını OLAN HDInsight'taki Apache Hive ve Apache Pig'den Python Kullanıcı Tanımlı İşlevlerini (UDF) nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/15/2019
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 201bb40e5024442587f5508886da7e844f35be40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74148397"
---
# <a name="use-python-user-defined-functions-udf-with-apache-hive-and-apache-pig-in-hdinsight"></a>HDInsight'ta Apache Hive ve Apache Pig ile Python Kullanıcı Tanımlı Fonksiyonlar (UDF) kullanın

Azure HDInsight'ta Apache Hadoop'ta Apache Hive ve Apache Pig ile Python kullanıcı tanımlı işlevleri (UDF) nasıl kullanacağınızı öğrenin.

## <a name="python-on-hdinsight"></a><a name="python"></a>PYTHON HDInsight üzerinde

Python2.7 varsayılan olarak HDInsight 3.0 ve sonraki üzerine yüklenir. Apache Hive akış işleme için Python bu sürümü ile kullanılabilir. Akış işleme Hive ve UDF arasında veri aktarmak için STDOUT ve STDIN kullanır.

HDInsight ayrıca Java'da yazılmış bir Python uygulaması olan Jython'u da içerir. Jython doğrudan Java Sanal Makine üzerinde çalışır ve akış kullanmaz. Jython, Pig ile Python kullanırken önerilen Python yorumlayıcısıdır.

## <a name="prerequisites"></a>Ön koşullar

* **HDInsight'ta hadoop kümesi.** [Linux'ta HDInsight ile başlayın](apache-hadoop-linux-tutorial-get-started.md)bakın.
* **Bir SSH istemcisi**. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.
* Kümeleriniz birincil depolama için [URI düzeni.](../hdinsight-hadoop-linux-information.md#URI-and-scheme) Bu, `wasb://` Azure Depolama `abfs://` için, Azure Veri Gölü Depolama Gen2 için veya Azure Veri Gölü Depolama Gen1 için adl:// için dir. Azure Depolama için güvenli aktarım etkinleştirilirse, URI wasbs://.  Ayrıca bakınız, [güvenli aktarım.](../../storage/common/storage-require-secure-transfer.md)
* **Depolama yapılandırmasına olası değişiklik.**  Bkz. Depolama hesabı türünü `BlobStorage`kullanıyorsanız [Depolama yapılandırması.](#storage-configuration)
* İsteğe bağlı.  PowerShell'i kullanmayı planlıyorsanız, [AZ modülünün](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) yüklü olması gerekir.

> [!NOTE]  
> Bu makalede kullanılan depolama [hesabı, güvenli aktarım](../../storage/common/storage-require-secure-transfer.md) `wasbs` etkinleştirilmiş Azure Depolama'ydı ve böylece makale boyunca kullanılır.

## <a name="storage-configuration"></a>Depolama yapılandırması

Kullanılan depolama hesabı tür `Storage (general purpose v1)` veya `StorageV2 (general purpose v2)`.  Bu makaledeki işlem en az `/tezstaging`çıktı üretecektir.  Varsayılan hadoop `/tezstaging` yapılandırması, `fs.azure.page.blob.dir` hizmet `HDFS`için `core-site.xml` yapılandırma değişkeninde yer alır.  Bu yapılandırma, dizin çıkışının depolama hesabı türü `BlobStorage`için desteklenmeyen sayfa lekeleri olarak adlandırılmasına neden olur.  Bu `BlobStorage` makalede kullanmak için `/tezstaging` yapılandırma `fs.azure.page.blob.dir` değişkeninden kaldırın.  Yapılandırma [Ambari UI](../hdinsight-hadoop-manage-ambari.md)erişilebilir.  Aksi takdirde, hata iletisi alırsınız:`Page blob is not supported for this account type.`

> [!WARNING]  
> Bu belgedeki adımlar aşağıdaki varsayımları yapar:  
>
> * Yerel geliştirme ortamınızda Python komut dosyalarını oluşturursunuz.
> * Komutu veya sağlanan PowerShell komut `scp` dosyasını kullanarak komut dosyalarını HDInsight'a yüklersiniz.
>
> HDInsight ile çalışmak için [Azure Bulut Kabuğu'nu (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) kullanmak istiyorsanız, şunları
>
> * Bulut kabuğu ortamının içindeki komut dosyalarını oluşturun.
> * Dosyaları `scp` bulut kabuğundan HDInsight'a yüklemek için kullanın.
> * HDInsight'a bağlanmak ve örnekleri çalıştırmak için bulut kabuğundan kullanın. `ssh`

## <a name="apache-hive-udf"></a><a name="hivepython"></a>Apache Hive UDF

Python HiveQL `TRANSFORM` deyimi ile Hive bir UDF olarak kullanılabilir. Örneğin, aşağıdaki HiveQL küme `hiveudf.py` için varsayılan Azure Depolama hesabında depolanan dosyayı çağırır.

```hiveql
add file wasbs:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLabel string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

Bu örnek şu anlama şeyi yapar:

1. Dosyanın `add file` başındaki deyim, dosyayı `hiveudf.py` dağıtılmış önbelleğe ekler, böylece kümedeki tüm düğümler tarafından erişilebilir.
2. İfade, `SELECT TRANSFORM ... USING` `hivesampletable`''den veri seçer. Ayrıca istemci, aygıt ve aygıt modeli değerlerini komut `hiveudf.py` dosyasına geçirir.
3. Yan `AS` tümce, 'den `hiveudf.py`döndürülen alanları açıklar.

<a name="streamingpy"></a>

### <a name="create-file"></a>Dosya oluşturma

Geliştirme ortamınızda, '' adlı `hiveudf.py`bir metin dosyası oluşturun. Dosyanın içeriği olarak aşağıdaki kodu kullanın:

```python
#!/usr/bin/env python
import sys
import string
import hashlib

while True:
    line = sys.stdin.readline()
    if not line:
        break

    line = string.strip(line, "\n ")
    clientid, devicemake, devicemodel = string.split(line, "\t")
    phone_label = devicemake + ' ' + devicemodel
    print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])
```

Bu komut dosyası aşağıdaki eylemleri gerçekleştirir:

1. STDIN'den gelen bir veri satırını okur.
2. Sondaki yeni çizgi karakteri `string.strip(line, "\n ")`kullanılarak kaldırılır.
3. Akış işleme yaparken, tek bir satır her değer arasında bir sekme karakteri olan tüm değerleri içerir. Yani, `string.split(line, "\t")` her sekmedeki girişi bölmek için kullanılabilir, sadece alanları döndürür.
4. İşleme tamamlandığında, çıktı stdout'a her alan arasında bir sekme yle tek bir satır olarak yazılmalıdır. Örneğin, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. Döngü `while` okunmaz `line` ken tekrarlanır.

Komut dosyası çıktısı, girdi değerlerinin `devicemake` `devicemodel`ve katlanılan değerin karmasının bir araya edilmesidir.

### <a name="upload-file-shell"></a>Dosya yükleme (kabuk)

Aşağıdaki komutlarda, farklıysa gerçek kullanıcı adı ile değiştirin. `sshuser`  Gerçek `mycluster` küme adı ile değiştirin.  Çalışma dizininizin dosyanın bulunduğu yer olduğundan emin olun.

1. Dosyaları `scp` HDInsight kümenize kopyalamak için kullanın. Edin ve aşağıdaki komutu girin:

    ```cmd
    scp hiveudf.py sshuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Kümeye bağlanmak için SSH'yi kullanın.  Edin ve aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. SSH oturumundan, kümenin depolama alanına daha önce yüklenen python dosyalarını ekleyin.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    ```

### <a name="use-hive-udf-shell"></a>Kovan UDF (kabuk) kullanın

1. Kovan'a bağlanmak için açık SSH oturumunuzdaki aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Bu komut Beeline istemcisini başlatır.

2. İstem de aşağıdaki `0: jdbc:hive2://headnodehost:10001/>` sorguyu girin:

   ```hive
   add file wasbs:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. Son satıra girdikten sonra, iş başlamalıdır. İş tamamlandıktan sonra, aşağıdaki örneğe benzer çıktı döndürür:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

4. Beeline'den çıkmak için aşağıdaki komutu girin:

    ```hive
    !q
    ```

### <a name="upload-file-powershell"></a>Dosya yükleme (PowerShell)

PowerShell, Kovan sorgularını uzaktan çalıştırmak için de kullanılabilir. Çalışma dizininizin bulunduğu `hiveudf.py` yer olduğundan emin olun.  Komut dosyasını kullanan bir Hive sorgusunu `hiveudf.py` çalıştırmak için aşağıdaki PowerShell komut dosyasını kullanın:

```PowerShell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Revise file path as needed
$pathToStreamingFile = ".\hiveudf.py"

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzStorageAccountKey `
   -ResourceGroupName $resourceGroup `
   -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

# Upload local files to an Azure Storage blob
Set-AzStorageBlobContent `
    -File $pathToStreamingFile `
    -Blob "hiveudf.py" `
    -Container $container `
    -Context $context
```

> [!NOTE]  
> Dosya yükleme hakkında daha fazla bilgi için [HDInsight belgesindeki Apache Hadoop işleri için Yükleme verilerine](../hdinsight-upload-data.md) bakın.

#### <a name="use-hive-udf"></a>Kovan UDF kullanın

```PowerShell
# Script should stop on failures
$ErrorActionPreference = "Stop"

# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -UserName "admin" -Message "Enter the login for the cluster"

$HiveQuery = "add file wasbs:///hiveudf.py;" +
                "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                "USING 'python hiveudf.py' AS " +
                "(clientid string, phoneLabel string, phoneHash string) " +
                "FROM hivesampletable " +
                "ORDER BY clientid LIMIT 50;"

# Create Hive job object
$jobDefinition = New-AzHDInsightHiveJobDefinition `
    -Query $HiveQuery

# For status bar updates
$activity="Hive query"

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Starting query..."

# Start defined Azure HDInsight job on specified cluster.
$job = Start-AzHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Waiting on query to complete..."

# Wait for completion or failure of specified job
Wait-AzHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds

# Uncomment the following to see stderr output
<#
Get-AzHDInsightJobOutput `
   -Clustername $clusterName `
   -JobId $job.JobId `
   -HttpCredential $creds `
   -DisplayOutputType StandardError
#>

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Retrieving output..."

# Gets the log output
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

**Kovan** iş çıktısı aşağıdaki örneğe benzer görünmelidir:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

## <a name="apache-pig-udf"></a><a name="pigpython"></a>Apaçi Domuz UDF

Python komut dosyası, tablo aracılığıyla `GENERATE` Pig'den UDF olarak kullanılabilir. Komut dosyasını Jython veya C Python kullanarak çalıştırabilirsiniz.

* Jython JVM üzerinde çalışır ve yerli Pig çağrılabilir.
* C Python harici bir işlemdir, bu nedenle JVM'deki Pig verileri Python işleminde çalışan komut dosyasına gönderilir. Python komut dosyasının çıktısı Pig'e geri gönderilir.

Python yorumlayıcısını belirtmek `register` için Python komut dosyasına başvururken kullanın. Aşağıdaki örnekler pig ile komut `myfuncs`kaydı:

* **Jython kullanmak için:**`register '/path/to/pigudf.py' using jython as myfuncs;`
* **C Python kullanmak için:**`register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]  
> Jython kullanırken, pig_jython dosyasına giden yol yerel bir yol veya WASBS:// bir yol olabilir. Ancak, C Python kullanırken, Pig iş göndermek için kullandığınız düğüm yerel dosya sisteminde bir dosya başvuru gerekir.

Bir kez geçmiş kayıt, bu örnek için Pig Latince her ikisi için aynıdır:

```pig
LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Bu örnek şu anlama şeyi yapar:

1. İlk satır örnek veri `sample.log` dosyasını `LOGS`. Ayrıca her kaydı bir `chararray`.
2. Bir sonraki satır, işlemin sonucunu `LOG`.
3. Daha sonra, içinde kayıtları üzerinde `LOG` yinelenir ve `create_structure` Python / Jython komut dosyası `myfuncs`olarak yüklenen metodu çağırmak için kullanır. `GENERATE` `LINE`geçerli kaydı işleve geçirmek için kullanılır.
4. Son olarak, çıktılar komutu `DUMP` kullanarak STDOUT'a atılır. Bu komut, işlem tamamlandıktan sonra sonuçları görüntüler.

### <a name="create-file"></a>Dosya oluşturma

Geliştirme ortamınızda, '' adlı `pigudf.py`bir metin dosyası oluşturun. Dosyanın içeriği olarak aşağıdaki kodu kullanın:

<a name="streamingpy"></a>

```python
# Uncomment the following if using C Python
#from pig_util import outputSchema


@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

Pig Latin örneğinde, `LINE` giriş bir chararray olarak tanımlanır, çünkü giriş için tutarlı bir şema yoktur. Python komut dosyası, verileri çıktı için tutarlı bir şema haline dönüştürür.

1. Deyim, `@outputSchema` Pig'e döndürülen verilerin biçimini tanımlar. Bu durumda, bir **veri torbası**, bir Pig veri türüdür. Çanta, hepsi chararray (dizeleri) olan aşağıdaki alanları içerir:

   * tarih - günlük girişinin oluşturulduğu tarih
   * zaman - günlük girişinin oluşturulduğu saat
   * sınıf adı - giriş için oluşturulan sınıf adı
   * düzeyi - günlük düzeyi
   * detay - günlük girişi için ayrıntılı bilgi

2. Ardından, `def create_structure(input)` Pig satır öğeleri geçer işlevi tanımlar.

3. Örnek veriler, `sample.log`çoğunlukla tarih, saat, sınıf adı, düzey ve ayrıntı şemasına uygundur. Ancak, ile `*java.lang.Exception*`başlayan birkaç satır içerir. Bu satırlar şemaya uyacak şekilde değiştirilmelidir. İfade, `if` bu kişileri denetler, daha sonra `*java.lang.Exception*` dizeyi sonuna taşımak için giriş verilerine masaj yaparsa, verileri beklenen çıktı şemasıyla aynı hizaya getirir.

4. Ardından, `split` komut ilk dört uzay karakterindeki verileri bölmek için kullanılır. Çıktı `date`, , `time` `classname`, `level`, `detail`ve .

5. Son olarak, değerler Pig döndürülür.

Veriler Pig'e döndürüldüğünde, `@outputSchema` deyimde tanımlandığı şekilde tutarlı bir şema vardır.

### <a name="upload-file-shell"></a>Dosya yükleme (kabuk)

Aşağıdaki komutlarda, farklıysa gerçek kullanıcı adı ile değiştirin. `sshuser`  Gerçek `mycluster` küme adı ile değiştirin.  Çalışma dizininizin dosyanın bulunduğu yer olduğundan emin olun.

1. Dosyaları `scp` HDInsight kümenize kopyalamak için kullanın. Edin ve aşağıdaki komutu girin:

    ```cmd
    scp pigudf.py sshuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Kümeye bağlanmak için SSH'yi kullanın.  Edin ve aşağıdaki komutu girin:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. SSH oturumundan, kümenin depolama alanına daha önce yüklenen python dosyalarını ekleyin.

    ```bash
    hdfs dfs -put pigudf.py /pigudf.py
    ```

### <a name="use-pig-udf-shell"></a>Domuz UDF (kabuk) kullanın

1. Domuza bağlanmak için, açık SSH oturumunuzdaki aşağıdaki komutu kullanın:

    ```bash
    pig
    ```

2. `grunt>` İstemde aşağıdaki ifadeleri girin:

   ```pig
   Register wasbs:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Aşağıdaki satırı girdikten sonra, iş başlamalıdır. İş tamamlandıktan sonra, aşağıdaki verilere benzer çıktı döndürür:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Grunt kabuğundan çıkmak için kullanın `quit` ve ardından yerel dosya sistemindeki pigudf.py dosyasını düzenlemek için aşağıdakileri kullanın:

    ```bash
    nano pigudf.py
    ```

5. Editöre girince, `#` karakteri satırın başından kaldırarak aşağıdaki satırı açıklamamaya bırakın:

    ```bash
    #from pig_util import outputSchema
    ```

    Bu satır, Python komut dosyasını Jython yerine C Python ile çalışacak şekilde değiştirir. Değişiklik yapıldıktan sonra, düzenleyiciden çıkmak için **Ctrl+X'i** kullanın. Değişiklikleri kaydetmek için **Y'yi**ve sonra **Enter'ı** seçin.

6. Kabuğu `pig` yeniden başlatmak için komutu kullanın. Komut istemine `grunt>` başladıktan sonra, C Python yorumlayıcısını kullanarak Python komut dosyasını çalıştırmak için aşağıdakileri kullanın.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Bu iş tamamlandıktan sonra, daha önce Jython kullanarak komut dosyası çalıştırdığınızda aynı çıktıyı görmeniz gerekir.

### <a name="upload-file-powershell"></a>Dosya yükleme (PowerShell)

PowerShell, Kovan sorgularını uzaktan çalıştırmak için de kullanılabilir. Çalışma dizininizin bulunduğu `pigudf.py` yer olduğundan emin olun.  Komut dosyasını kullanan bir Hive sorgusunu `pigudf.py` çalıştırmak için aşağıdaki PowerShell komut dosyasını kullanın:

```PowerShell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Revise file path as needed
$pathToJythonFile = ".\pigudf.py"


# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzStorageAccountKey `
   -ResourceGroupName $resourceGroup `
   -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

# Upload local files to an Azure Storage blob
Set-AzStorageBlobContent `
    -File $pathToJythonFile `
    -Blob "pigudf.py" `
    -Container $container `
    -Context $context
```

### <a name="use-pig-udf-powershell"></a>Domuz UDF (PowerShell) kullanın

> [!NOTE]  
> PowerShell kullanarak uzaktan bir iş gönderirken, c Python'u yorumlayıcı olarak kullanmak mümkün değildir.

PowerShell ayrıca Pig Latin işleri çalıştırmak için kullanılabilir. Komut dosyasını `pigudf.py` kullanan bir Pig Latin işini çalıştırmak için aşağıdaki PowerShell komut dosyasını kullanın:

```PowerShell
# Script should stop on failures
$ErrorActionPreference = "Stop"

# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -UserName "admin" -Message "Enter the login for the cluster"


$PigQuery = "Register wasbs:///pigudf.py using jython as myfuncs;" +
            "LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);" +
            "LOG = FILTER LOGS by LINE is not null;" +
            "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
            "DUMP DETAILS;"

# Create Pig job object
$jobDefinition = New-AzHDInsightPigJobDefinition -Query $PigQuery

# For status bar updates
$activity="Pig job"

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Starting job..."

# Start defined Azure HDInsight job on specified cluster.
$job = Start-AzHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Waiting for the Pig job to complete..."

# Wait for completion or failure of specified job
Wait-AzHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds

# Uncomment the following to see stderr output
<#
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds `
    -DisplayOutputType StandardError
#>

# Progress bar (optional)
Write-Progress -Activity $activity "Retrieving output..."

# Gets the log output
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

**Pig** iş çıktısı aşağıdaki verilere benzer görünmelidir:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a><a name="troubleshooting"></a>Sorun giderme

### <a name="errors-when-running-jobs"></a>İşleri çalıştırırken hatalar

Kovan işini çalıştırırken, aşağıdaki metne benzer bir hatayla karşılaşabilirsiniz:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Bu sorun Python dosyasındaki satır sonları neden olabilir. Birçok Windows düzenleyicisi satır bitişi olarak CRLF kullanarak varsayılan, ancak Linux uygulamaları genellikle LF bekliyoruz.

Dosyayı HDInsight'a yüklemeden önce CR karakterlerini kaldırmak için aşağıdaki PowerShell ifadelerini kullanabilirsiniz:

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=148-150)]

### <a name="powershell-scripts"></a>PowerShell komut dosyaları

Örnekleri çalıştırmak için kullanılan powershell komut dosyalarının her ikisi de iş için hata çıktısı görüntüleyen bir yorum satırı içerir. İş için beklenen çıktıyı görmüyorsanız, aşağıdaki satırı açıklamayı bırakın ve hata bilgilerinin bir sorun olup olmadığını görün.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=135-139)]

Hata bilgileri (STDERR) ve işin sonucu (STDOUT) da HDInsight depolama alanınızda günlüğe kaydedilir.

| Bu iş için... | Blob kabındaki şu dosyalara bak. |
| --- | --- |
| Hive |/HivePython/stderr<p>/HivePython/stdout |
| Pig |/PigPython/stderr<p>/PigPython/stdout |

## <a name="next-steps"></a><a name="next"></a>Sonraki adımlar

Varsayılan olarak sağlanmayan Python modüllerini yüklemeniz gerekiyorsa, [azure HDInsight'a nasıl bir modül dağıtılayabildiğinize](https://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx)bakın.

Pig, Hive'ı kullanmanın diğer yolları ve MapReduce'ı kullanma hakkında bilgi edinmek için aşağıdaki belgelere bakın:

* [HDInsight ile Apache Hive'ı kullanma](hdinsight-use-hive.md)
* [HDInsight ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)
