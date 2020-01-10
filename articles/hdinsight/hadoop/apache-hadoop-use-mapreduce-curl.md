---
title: HDInsight 'ta Apache Hadoop MapReduce kullanma ve kıvrımlı-Azure
description: Bkz. HDInsight 'ta MapReduce işlerini HDInsight üzerinde Apache Hadoop ile uzaktan çalıştırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/27/2018
ms.openlocfilehash: 274d8dc80d9318aa3ddf4a904a5b623319ea01f4
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645013"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-rest"></a>REST kullanarak HDInsight üzerinde Apache Hadoop MapReduce işleri çalıştırma

MapReduce işlerini HDInsight kümesindeki bir Apache Hadoop üzerinde çalıştırmak için Apache Hive WebHCat REST API nasıl kullanacağınızı öğrenin. Kıvrımlı, MapReduce işlerini çalıştırmak için ham HTTP isteklerini kullanarak HDInsight ile nasıl etkileşim kurabileceğinizi göstermek için kullanılır.

> [!NOTE]  
> Linux tabanlı Hadoop sunucuları kullanmayı zaten biliyorsanız, ancak HDInsight 'a yeni bir adım daha sahipseniz [HDInsight 'Ta Linux tabanlı Apache Hadoop hakkında bilmeniz](../hdinsight-hadoop-linux-information.md) gerekenler bölümüne bakın.


## <a id="prereq"></a>Önkoşullar

* HDInsight kümesinde Hadoop kümesi
* Windows PowerShell veya [kıvrımlı](https://curl.haxx.se/) ve [JQ](https://stedolan.github.io/jq/)

## <a id="curl"></a>MapReduce işi çalıştırma

> [!NOTE]  
> WebHCat ile kıvrımlı veya diğer REST iletişimini kullandığınızda, HDInsight kümesi Yönetici Kullanıcı adı ve parolasını sağlayarak isteklerin kimlik doğrulamasını yapmanız gerekir. İstekleri sunucuya göndermek için kullanılan URI 'nin bir parçası olarak küme adını kullanmanız gerekir.
>
> REST API, [temel erişim kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication)kullanılarak güvenli hale getirilir. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesini sağlamak için her zaman HTTPS kullanarak istek yapmalısınız.

1. Bu belgedeki betikler tarafından kullanılan küme oturum açma bilgilerini ayarlamak için aşağıdaki komutlardan birini kullanın:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Küme adını ayarlamak için aşağıdaki komutlardan birini kullanın:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    Aşağıdaki JSON 'a benzer bir yanıt alırsınız:

        {"status":"ok","version":"v1"}

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-u**: isteğin kimliğini doğrulamak için kullanılan Kullanıcı adını ve parolayı belirtir
   * **-G**: Bu IŞLEMIN bir get isteği olduğunu belirtir

   URI 'nin başı `https://CLUSTERNAME.azurehdinsight.net/templeton/v1`, tüm istekler için aynıdır.

4. MapReduce işi göndermek için aşağıdaki komutu kullanın:

    ```bash
    JOBID=`curl -u $LOGIN -d user.name=$LOGIN -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{}
    $reqParams."user.name" = "admin"
    $reqParams.jar = "/example/jars/hadoop-mapreduce-examples.jar"
    $reqParams.class = "wordcount"
    $reqParams.arg = @()
    $reqParams.arg += "/example/data/gutenberg/davinci.txt"
    $reqparams.arg += "/example/data/output"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/mapreduce/jar" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    URI 'nin sonu (/MapReduce/jar), bu isteğin bir jar dosyasındaki bir sınıftan MapReduce işi başlattığı Web Hcat 'e bildirir. Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-d**: `-G` kullanılmaz, bu nedenle istek Post yöntemine varsayılan olarak ayarlanır. `-d` istekle birlikte gönderilen veri değerlerini belirtir.
     * **User.Name**: komutunu çalıştıran Kullanıcı
     * **jar**: çalıştırılacak sınıfı içeren jar dosyasının konumu
     * **Class**: MapReduce mantığını içeren sınıf
     * **arg**: MapReduce işine geçirilecek bağımsız değişkenler. Bu durumda, giriş metin dosyası ve çıkış için kullanılan dizin

   Bu komut, işin durumunu denetlemek için kullanılabilecek bir iş KIMLIĞI döndürmelidir:

       job_1415651640909_0026

5. İşin durumunu denetlemek için şu komutu kullanın:

    ```bash
    curl -G -u $LOGIN -d user.name=$LOGIN https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/$JOBID | jq .status.state
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

    İş tamamlandıysanız döndürülen durum `SUCCEEDED`.

   > [!NOTE]  
   > Bu kıvrımlı istek, iş hakkında bilgi içeren bir JSON belgesi döndürüyor. JQ yalnızca durum değerini almak için kullanılır.

6. İşin durumu `SUCCEEDED`olarak değiştiğinde, Azure Blob depolamadan iş sonuçlarını alabilirsiniz. Sorguyla geçirilen `statusdir` parametresi, çıkış dosyasının konumunu içerir. Bu örnekte, konum `/example/curl`. Bu adres, `/example/curl`konumundaki kümelerin varsayılan depolama alanında işin çıkışını depolar.

[Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)kullanarak bu dosyaları listeleyebilir ve indirebilirsiniz. Azure CLı 'dan blob 'larla çalışma hakkında daha fazla bilgi için bkz. Azure [CLI 'Yı Azure depolama belgesiyle kullanma](../../storage/common/storage-azure-cli.md#create-and-manage-blobs) .

## <a id="nextsteps"></a>Sonraki adımlar

HDInsight 'ta MapReduce işleri hakkında genel bilgi için:

* [HDInsight üzerinde Apache Hadoop MapReduce kullanma](hdinsight-use-mapreduce.md)

HDInsight 'ta Hadoop ile birlikte çalışmak için kullanabileceğiniz diğer yollar hakkında daha fazla bilgi için:

* [HDInsight üzerinde Apache Hadoop ile Apache Hive kullanma](hdinsight-use-hive.md)
* [HDInsight üzerinde Apache Hadoop Apache Pig kullanma](hdinsight-use-pig.md)

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için bkz. [Webhcat başvurusu](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
