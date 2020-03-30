---
title: HDInsight'ta Curl ile Apache Hadoop Kovanı Kullanma - Azure
description: Curl'ü kullanarak Apache Pig işlerini Azure HDInsight'a uzaktan nasıl göndereceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/06/2020
ms.openlocfilehash: 10a2f413142124db7547e68280a0d5e9abac9b98
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298759"
---
# <a name="run-apache-hive-queries-with-apache-hadoop-in-hdinsight-using-rest"></a>REST kullanarak HDInsight'ta Apache Hadoop ile Apache Hive sorgularını çalıştırın

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Azure HDInsight kümesinde Apache Hadoop ile Apache Hive sorgularını çalıştırmak için WebHCat REST API'sını nasıl kullanacağınızı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Hadoop kümesi. [Linux'ta HDInsight ile başlayın](./apache-hadoop-linux-tutorial-get-started.md)bakın.

* Bir REST müşterisi. Bu belge, Windows PowerShell'de [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) ve [Bash'te](https://docs.microsoft.com/windows/wsl/install-win10) [Curl'u](https://curl.haxx.se/) kullanır.

* Bash kullanıyorsanız, aynı zamanda jq, bir komut satırı JSON işlemci gerekir.  Bkz. [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).

## <a name="base-uri-for-rest-api"></a>Rest API için Baz URI

HDInsight'taki `https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME`REST API'nin temel Tekdüzen Kaynak Tanımlayıcısı (URI), kümenizin adıdır. `CLUSTERNAME`  ÜRB'lerde küme adları **büyük/küçük harf duyarlıdır.**  URI'nin`CLUSTERNAME.azurehdinsight.net`tam nitelikli alan adı (FQDN) bölümündeki küme adı büyük/küçük harf duyarsız olsa da, URI'deki diğer olaylar büyük/küçük harf duyarlıdır.

## <a name="authentication"></a>Kimlik doğrulaması

WebHCat ile cURL veya başka bir REST iletişimi kullanırken, HDInsight küme yöneticisi için kullanıcı adı ve parola sağlayarak istekleri doğrulamanız gerekir. REST API’sinin güvenliği [temel kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesini sağlamak için Güvenli HTTP (HTTPS) kullanarak her zaman istekte bulunun.

### <a name="setup-preserve-credentials"></a>Kurulum (Kimlik bilgilerini koru)

Her örnek için yeniden girmemek için kimlik bilgilerinizi koruyun.  Küme adı ayrı bir adımda korunur.

**A. Bash**  
Gerçek parolanızı değiştirerek `PASSWORD` aşağıdaki komut dosyasını düzenleme.  Sonra komutu girin.

```bash
export password='PASSWORD'
```  

**B. PowerShell** Aşağıdaki kodu çalıştırın ve açılan pencerede kimlik bilgilerinizi girin:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
```

### <a name="identify-correctly-cased-cluster-name"></a>Doğru örneklenmiş küme adını tanımlama

Küme adının gerçek gövdesi, kümenin nasıl oluşturulduğuna bağlı olarak beklediğiniz den farklı olabilir.  Buradaki adımlar gerçek kasayı gösterir ve daha sonraki tüm örnekler için bir değişkende saklar.

Küme adınız ile değiştirmek `CLUSTERNAME` için aşağıdaki komut dosyalarını edin. Sonra komutu girin. (FQDN'nin küme adı büyük/küçük harf duyarlı değildir.)

```bash
export clusterName=$(curl -u admin:$password -sS -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
echo $clusterName
```  

```powershell
# Identify properly cased cluster name
$resp = Invoke-WebRequest -Uri "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters" `
    -Credential $creds -UseBasicParsing
$clusterName = (ConvertFrom-Json $resp.Content).items.Clusters.cluster_name;

# Show cluster name
$clusterName
```

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

1. HDInsight kümenize bağlanabileceğinizi doğrulamak için aşağıdaki komutlardan birini kullanın:

    ```bash
    curl -u admin:$password -G https://$clusterName.azurehdinsight.net/templeton/v1/status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/status" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Aşağıdaki metne benzer bir yanıt alırsınız:

    ```json
    {"status":"ok","version":"v1"}
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * `-u`- İsteğin kimliğini doğrulamak için kullanılan kullanıcı adı ve parola.
    * `-G`- Bu isteğin bir GET işlemi olduğunu gösterir.

1. URL'nin başlangıcı, `https://$CLUSTERNAME.azurehdinsight.net/templeton/v1`tüm istekler için aynıdır. Yol,, `/status`istek webhcat (ayrıca Templeton olarak da bilinir) sunucu için bir durum dönmek olduğunu gösterir. Ayrıca aşağıdaki komutu kullanarak Hive sürümünü talep edebilirsiniz:

    ```bash
    curl -u admin:$password -G https://$clusterName.azurehdinsight.net/templeton/v1/version/hive
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/version/hive" `
       -Credential $creds `
       -UseBasicParsing
    $resp.Content
    ```

    Bu istek, aşağıdaki metne benzer bir yanıt döndürür:

    ```json
    {"module":"hive","version":"1.2.1000.2.6.5.3008-11"}
    ```

1. **log4jLogs**adlı bir tablo oluşturmak için aşağıdakileri kullanın:

    ```bash
    jobid=$(curl -s -u admin:$password -d user.name=admin -d execute="DROP+TABLE+log4jLogs;CREATE+EXTERNAL+TABLE+log4jLogs(t1+string,t2+string,t3+string,t4+string,t5+string,t6+string,t7+string)+ROW+FORMAT+DELIMITED+FIELDS+TERMINATED+BY+' '+STORED+AS+TEXTFILE+LOCATION+'/example/data/';SELECT+t4+AS+sev,COUNT(*)+AS+count+FROM+log4jLogs+WHERE+t4+=+'[ERROR]'+AND+INPUT__FILE__NAME+LIKE+'%25.log'+GROUP+BY+t4;" -d statusdir="/example/rest" https://$clusterName.azurehdinsight.net/templeton/v1/hive | jq -r .id)
    echo $jobid
    ```

    ```powershell
    $reqParams = @{"user.name"="admin";"execute"="DROP TABLE log4jLogs;CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED BY ' ' STORED AS TEXTFILE LOCATION '/example/data/;SELECT t4 AS sev,COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;";"statusdir"="/example/rest"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/hive" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    Bu istek, isteğin bir parçası olarak REST API'ye veri gönderen POST yöntemini kullanır. Aşağıdaki veri değerleri istekle birlikte gönderilir:

     * `user.name`- Komutu çalıştıran kullanıcı.
     * `execute`- HiveQL ifadeleri yürütmek için.
     * `statusdir`- Bu işin durumunun yazıldığı dizin.

   Bu ifadeler aşağıdaki eylemleri gerçekleştirir:

   * `DROP TABLE`- Tablo zaten varsa, silinir.
   * `CREATE EXTERNAL TABLE`- Hive yeni bir 'dış' tablo oluşturur. Dış tablolar yalnızca Hive'da tablo tanımını depolar. Veriler özgün konumda bırakılır.

     > [!NOTE]  
     > Temel verilerin harici bir kaynak tarafından güncelleştirilmesini beklerken dış tablolar kullanılmalıdır. Örneğin, otomatik bir veri yükleme işlemi veya başka bir MapReduce işlemi.
     >
     > Harici bir tablonun düşmesi verileri **silmez,** yalnızca tablo tanımını siler.

   * `ROW FORMAT`- Verilerin nasıl biçimlendirilir. Her günlükteki alanlar bir boşlukla ayrılır.
   * `STORED AS TEXTFILE LOCATION`- Verilerin depolandığı (örnek/veri dizini) ve metin olarak depolandığı yer.
   * `SELECT`- **T4** sütununun **[HATA]** değerini içerdiği tüm satırların sayısını seçer. Bu değeri içeren üç satır olduğundan, bu ifade **3** değerini döndürür.

     > [!NOTE]  
     > HiveQL deyimleri arasındaki boşlukların Curl `+` ile kullanıldığında karakterle değiştirildiğinden dikkat edin. Delimiter gibi bir boşluk içeren alıntı değerleri , `+`.

      Bu komut, işin durumunu denetlemek için kullanılabilecek bir iş kimliği döndürür.

1. İşin durumunu denetlemek için aşağıdaki komutu kullanın:

    ```bash
    curl -u admin:$password -d user.name=admin -G https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobid | jq .status.state
    ```

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing
    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

    İş bittiyse, devlet **BAŞARILI.**

1. İşin durumu **BAŞARıLı**olarak değiştirildikten sonra, işin sonuçlarını Azure Blob depolama dan alabilirsiniz. Sorguyla `statusdir` geçirilen parametre, çıktı dosyasının konumunu içerir; Bu durumda, `/example/rest`. Bu adres, çıktıyı `example/curl` kümeler varsayılan depolamasında dizinde depolar.

    [Azure CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli)kullanarak bu dosyaları listeleyebilir ve indirebilirsiniz. Azure Depolama ile Azure CLI'yi kullanma hakkında daha fazla bilgi için [Azure Depolama ile Azure CLI'yi kullan](https://docs.microsoft.com/azure/storage/storage-azure-cli) belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight'ta Hadoop ile çalışabileceğiniz diğer yollar hakkında bilgi için:

* [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hdinsight-use-hive.md)
* [HDInsight'ta Apache Hadoop ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)

Bu belgede kullanılan REST API'si hakkında daha fazla bilgi için [WebHCat başvuru](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference) belgesine bakın.