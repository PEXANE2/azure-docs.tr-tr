---
title: HDInsight'ta Apache Hadoop ile MapReduce ve Curl'u kullanma - Azure
description: Curl'ü kullanarak HDInsight'ta Apache Hadoop ile MapReduce işlerini uzaktan nasıl çalıştıracağınız hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/13/2020
ms.openlocfilehash: abc3cc8c526e37e18f1e67b109a9a8e15ff8c989
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302721"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-rest"></a>REST kullanarak HDInsight'ta Apache Hadoop ile MapReduce işleri çalıştırın

HdInsight kümesindeki Bir Apache Hadoop'ta MapReduce işlerini çalıştırmak için Apache Hive WebHCat REST API'yi nasıl kullanacağınızı öğrenin. Curl, MapReduce işlerini çalıştırmak için ham HTTP isteklerini kullanarak HDInsight ile nasıl etkileşimkurabileceğinizi göstermek için kullanılır.

> [!NOTE]  
> Linux tabanlı Hadoop sunucularını kullanmaya zaten aşinaysanız, ancak HDInsight'ta [yeniyseniz, HDInsight belgesinde Linux tabanlı Apache Hadoop hakkında bilmeniz gerekenlere](../hdinsight-hadoop-linux-information.md) bakın.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Hadoop kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun.](../hdinsight-hadoop-create-linux-clusters-portal.md)

Şunlardan biri:
  * Windows PowerShell veya,
  * [Jq](https://stedolan.github.io/jq/) ile [curl](https://curl.haxx.se/)

## <a name="run-a-mapreduce-job"></a>MapReduce işini çalıştırın

> [!NOTE]  
> Curl veya WebHCat ile başka bir REST iletişimi kullandığınızda, HDInsight küme yöneticisi kullanıcı adı ve parolasını sağlayarak istekleri ndoğrulaması gerekir. İstekleri sunucuya göndermek için kullanılan URI'nin bir parçası olarak küme adını kullanmanız gerekir.
>
> REST API temel erişim [kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication)kullanılarak güvenlidir. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesini sağlamak için HTTPS'yi kullanarak her zaman istekte bulunmalısınız.

### <a name="curl"></a>Curl

1. Kullanım kolaylığı için aşağıdaki değişkenleri ayarlayın. Bu örnek, ortamınız için gerektiği gibi gözden geçirin, windows ortamını temel alın.

    ```cmd
    set CLUSTERNAME=
    set PASSWORD=
    ```

1. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```bash
    curl -u admin:%PASSWORD% -G https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/status
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-u**: İsteğin kimliğini doğrulamak için kullanılan kullanıcı adı ve parolayı gösterir
   * **-G**: Bu işlemin bir GET isteği olduğunu gösterir

   URI başlangıcı, `https://CLUSTERNAME.azurehdinsight.net/templeton/v1`tüm istekler için aynıdır.

    Aşağıdaki JSON'a benzer bir yanıt alırsınız:

    ```output
    {"version":"v1","status":"ok"}
    ```

1. MapReduce işini göndermek için aşağıdaki komutu kullanın. Gerektiği gibi **jq** yolunu değiştirin.

    ```cmd
    curl -u admin:%PASSWORD% -d user.name=admin ^
    -d jar=/example/jars/hadoop-mapreduce-examples.jar ^
    -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output ^
    https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/mapreduce/jar | ^
    C:\HDI\jq-win64.exe .id
    ```

    URI'nin (/mapreduce/jar) sonunda, WebHCat'e bu isteğin jar dosyasındaki bir sınıftan mapreduce işi başlattığını söyler. Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-d** `-G` : kullanılmaz, bu nedenle istek POST yöntemine varsayılandır. `-d`istekle birlikte gönderilen veri değerlerini belirtir.
     * **user.name**: Komutu çalıştıran kullanıcı
     * **jar**: Çalıştırılacak sınıfı içeren kavanoz dosyasının konumu
     * **sınıf**: MapReduce mantığını içeren sınıf
     * **arg**: MapReduce işine geçirilecek bağımsız değişkenler. Bu durumda, giriş metin dosyası ve çıktı için kullanılan dizin

    Bu komut, işin durumunu denetlemek için kullanılabilecek bir iş kimliği döndürmelidir:

       job_1415651640909_0026

1. İşin durumunu denetlemek için aşağıdaki komutu kullanın. Değeri önceki `JOBID` adımda döndürülen **gerçek** değerle değiştirin. JQ'nun **jq** konumunu gerektiği gibi gözden geçirin.

    ```cmd
    set JOBID=job_1415651640909_0026

    curl -G -u admin:%PASSWORD% -d user.name=admin https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/jobs/%JOBID% | ^
    C:\HDI\jq-win64.exe .status.state
    ```

### <a name="powershell"></a>PowerShell

1. Kullanım kolaylığı için aşağıdaki değişkenleri ayarlayın. Gerçek `CLUSTERNAME` küme adınız ile değiştirin. Komutu çalıştırın ve istendiğinde küme giriş parolasını girin.

    ```powershell
    $clusterName="CLUSTERNAME"
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login password"
    ```

1. HDInsight kümenize bağlanabileceğinizi doğrulamak için aşağıdaki komutu kullanın:

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    Aşağıdaki JSON'a benzer bir yanıt alırsınız:

    ```output
    {"version":"v1","status":"ok"}
    ```

1. MapReduce işini göndermek için aşağıdaki komutu kullanın:

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

    URI'nin (/mapreduce/jar) sonunda, WebHCat'e bu isteğin jar dosyasındaki bir sınıftan mapreduce işi başlattığını söyler. Bu komutta kullanılan parametreler aşağıdaki gibidir:

    * **user.name**: Komutu çalıştıran kullanıcı
    * **jar**: Çalıştırılacak sınıfı içeren kavanoz dosyasının konumu
    * **sınıf**: MapReduce mantığını içeren sınıf
    * **arg**: MapReduce işine geçirilecek bağımsız değişkenler. Bu durumda, giriş metin dosyası ve çıktı için kullanılan dizin

   Bu komut, işin durumunu denetlemek için kullanılabilecek bir iş kimliği döndürmelidir:

       job_1415651640909_0026

1. İşin durumunu denetlemek için aşağıdaki komutu kullanın:

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

### <a name="both-methods"></a>Her iki yöntem

1. İş tamamlanırsa, döndürülen `SUCCEEDED`durum.

1. İşin durumu `SUCCEEDED`,'nin durumu , Azure Blob depolama dan iş sonuçlarını alabilirsiniz. Sorguyla `statusdir` geçirilen parametre, çıktı dosyasının konumunu içerir. Bu örnekte, konum `/example/curl`. Bu adres, iş çıktısını kümeler varsayılan `/example/curl`depolama sında depolar.

[Azure CLI'yi](/cli/azure/install-azure-cli)kullanarak bu dosyaları listeleyebilir ve indirebilirsiniz. Azure Blob depolama alanıyla çalışmak için Azure CLI'yi kullanma hakkında daha fazla bilgi için [Quickstart: Azure CLI ile blob oluşturma, indirme ve listele metotları](../../storage/blobs/storage-quickstart-blobs-cli.md)' na bakın.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight'ta Hadoop ile çalışabileceğiniz diğer yollar hakkında bilgi için:

* [HDInsight'ta Apache Hadoop ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)
* [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hdinsight-use-hive.md)

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi için [WebHCat Başvuru'ya](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference)bakın.
