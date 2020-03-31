---
title: Azure HDInsight'ta Iş kümesine iş göndermek için Livy Spark'ı kullanın
description: Spark işlerini bir Azure HDInsight kümesine uzaktan göndermek için Apache Spark REST API'yi nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 02/28/2020
ms.openlocfilehash: ac3904284ebf20fa1d5e75f9249732be3963f677
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206291"
---
# <a name="use-apache-spark-rest-api-to-submit-remote-jobs-to-an-hdinsight-spark-cluster"></a>HDInsight Spark kümesine uzak işleri göndermek için Apache Spark REST API’sini kullanma

Bir Azure HDInsight Spark kümesine uzak işleri göndermek için kullanılan [Apache Livy](https://livy.incubator.apache.org/), Apache Spark REST API, nasıl kullanılacağını öğrenin. Ayrıntılı belgeler için [Bkz. Apache Livy](https://livy.incubator.apache.org/docs/latest/rest-api.html).

Etkileşimli Kıvılcım kabukları çalıştırmak veya Spark'ta çalıştırılmak üzere toplu iş göndermek için Livy'yi kullanabilirsiniz. Bu makalede, toplu iş göndermek için Livy kullanma hakkında görüşmeler. Bu makaledeki parçacıklar, Livy Spark bitiş noktasına REST API çağrıları yapmak için cURL'yi kullanır.

## <a name="prerequisites"></a>Ön koşullar

HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

## <a name="submit-an-apache-livy-spark-batch-job"></a>Bir Apache Livy Spark toplu iş gönder

Toplu iş göndermeden önce, uygulama jarını kümeyle ilişkili küme depolama alanına yüklemeniz gerekir. Bunu yapmak için, bir komut satırı yardımcı programı olan [AzCopy](../../storage/common/storage-use-azcopy.md)’yi kullanabilirsiniz. Veri yüklemek için kullanabileceğiniz çeşitli diğer istemciler vardır. [HDInsight'ta Apache Hadoop işleri için Upload verilerinde](../hdinsight-upload-data.md)onlar hakkında daha fazla bilgi bulabilirsiniz.

```cmd
curl -k --user "admin:password" -v -H "Content-Type: application/json" -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches' -H "X-Requested-By: admin"
```

### <a name="examples"></a>Örnekler

* Kavanoz dosyası küme depolama (WASBS) üzerinde ise

    ```cmd  
    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
    ```

* Jar dosya adını ve sınıf adını giriş dosyasının bir parçası olarak geçirmek istiyorsanız (bu örnekte, input.txt)

    ```cmd
    curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
    ```

## <a name="get-information-on-livy-spark-batches-running-on-the-cluster"></a>Kümeüzerinde çalışan Livy Spark toplu işserileri hakkında bilgi alın

Söz dizimi:

```cmd
curl -k --user "admin:password" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"
```

### <a name="examples"></a>Örnekler

* Kümeüzerinde çalışan tüm Livy Spark toplu toplarını almak istiyorsanız:

    ```cmd
    curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"
    ```

* Belirli bir toplu iş kimliğiyle belirli bir toplu iş almak istiyorsanız

    ```cmd
    curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"
    ```

## <a name="delete-a-livy-spark-batch-job"></a>Livy Spark toplu iş silme

```cmd
curl -k --user "admin:mypassword1!" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"
```

### <a name="example"></a>Örnek

Toplu iş kimliği `5`ile toplu iş silme.

```cmd
curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/5"
```

## <a name="livy-spark-and-high-availability"></a>Livy Kıvılcım ve yüksek kullanılabilirlik

Livy küme üzerinde çalışan Spark işleri için yüksek kullanılabilirlik sağlar. İşte birkaç örnek.

* Bir Spark kümesine uzaktan bir iş gönderdikten sonra Livy hizmeti düşerse, iş arka planda devam eder. Livy geri döndüğünde, işin durumunu geri yükler ve geri bildirir.
* HDInsight için jupyter dizüstü bilgisayarlar arka uçta Livy tarafından desteklenmektedir. Bir not defteri bir Spark işi çalıştırıyorsa ve Livy hizmeti yeniden başlatılırsa, not defteri kod hücrelerini çalıştırmaya devam eder.

## <a name="show-me-an-example"></a>Bana bir örnek göster

Bu bölümde, toplu iş göndermek, işin ilerlemesini izlemek ve sonra silmek için Livy Spark'ı kullanan örneklere bakıyoruz. Bu örnekte kullandığımız uygulama, bağımsız [bir Scala uygulaması oluşturun ve HDInsight Spark kümesinde çalıştırmak için](apache-spark-create-standalone-application.md)makalede geliştirilen uygulamadır. Burada varsayalım:

* Uygulama kavanozu üzerinde kümeyle ilişkili depolama hesabına zaten kopyaladınız.
* Bu adımları denediğiniz bilgisayarda CuRL yüklü.

Aşağıdaki adımları uygulayın:

1. Kullanım kolaylığı için ortam değişkenlerini ayarlayın. Bu örnek, bir Windows ortamını temel alınarak, değişkenleri ortamınız için gerektiği gibi gözden geçirin. Değiştirin `CLUSTERNAME` `PASSWORD` ve uygun değerlerle değiştirin.

    ```cmd
    set clustername=CLUSTERNAME
    set password=PASSWORD
    ```

1. Livy Spark'ın kümede çalıştığını doğrulayın. Bunu, çalışan toplu işlerden oluşan bir liste alarak yapabiliriz. Livy'yi ilk kez kullanarak bir iş çalıştırıyorsanız, çıktı sıfır döndürülmelidir.

    ```cmd
    curl -k --user "admin:%password%" -v -X GET "https://%clustername%.azurehdinsight.net/livy/batches"
    ```

    Aşağıdaki parçacıkbenzer bir çıktı almalısınız:

    ```output
    < HTTP/1.1 200 OK
    < Content-Type: application/json; charset=UTF-8
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Fri, 20 Nov 2015 23:47:53 GMT
    < Content-Length: 34
    <
    {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
    ```

    Çıktıdaki son satırın **toplam:0**, çalışan toplu iş yok gibi yazdığına dikkat edin.

1. Şimdi bir toplu iş gönderelim. Aşağıdaki parçacık, kavanoz adını ve sınıf adını parametre olarak geçirmek için bir giriş dosyası (input.txt) kullanır. Bu adımları bir Windows bilgisayardan çalıştırıyorsanız, giriş dosyası kullanmak önerilir yaklaşımdır.

    ```cmd
    curl -k --user "admin:%password%" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://%clustername%.azurehdinsight.net/livy/batches" -H "X-Requested-By: admin"
    ```

    Dosya **girişi.txt'deki** parametreler aşağıdaki gibi tanımlanır:

    ```text
    { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }
    ```

    Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

    ```output
    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /0
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Fri, 20 Nov 2015 23:51:30 GMT
    < Content-Length: 36
    <
    {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
    ```

    Çıktının son satırında nasıl durum yazdığına dikkat **edin:başlangıç**. Ayrıca, **id:0**diyor. İşte, **0** toplu iş kimliğidir.

1. Artık toplu iş kimliğini kullanarak bu belirli toplu iş durumunu alabilirsiniz.

    ```cmd
    curl -k --user "admin:%password%" -v -X GET "https://%clustername%.azurehdinsight.net/livy/batches/0"
    ```

    Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

    ```output
    < HTTP/1.1 200 OK
    < Content-Type: application/json; charset=UTF-8
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Fri, 20 Nov 2015 23:54:42 GMT
    < Content-Length: 509
    <
    {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://myspar.lpel.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
    ```

    Çıktı şimdi **durumu gösterir:başarı,** hangi iş başarıyla tamamlandığını göstermektedir.

1. İsterseniz, artık toplu işlemi silebilirsiniz.

    ```cmd
    curl -k --user "admin:%password%" -v -X DELETE "https://%clustername%.azurehdinsight.net/livy/batches/0"
    ```

    Aşağıdaki kod parçacığına benzer bir çıktı görmeniz gerekir:

    ```output
    < HTTP/1.1 200 OK
    < Content-Type: application/json; charset=UTF-8
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Sat, 21 Nov 2015 18:51:54 GMT
    < Content-Length: 17
    <
    {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact
    ```

    Çıktının son satırı, toplu iş sayısının başarıyla silindiğini gösterir. Bir işi silerken, çalışırken, aynı zamanda işi de öldürür. Tamamlanmış, başarılı bir şekilde veya başka bir şekilde tamamlanmış bir işi silerseniz, iş bilgilerini tamamen siler.

## <a name="updates-to-livy-configuration-starting-with-hdinsight-35-version"></a>HDInsight 3.5 sürümüyle başlayan Livy yapılandırmagüncellemeleri

HDInsight 3.5 kümeleri ve üzeri, varsayılan olarak, örnek veri dosyalarına veya kavanozlarına erişmek için yerel dosya yollarının kullanımını devre dışı bırakılır. Kümedeki `wasbs://` kavanozlara veya örnek veri dosyalarına erişmek için yolu kullanmanızı öneririz.

## <a name="submitting-livy-jobs-for-a-cluster-within-an-azure-virtual-network"></a>Azure sanal ağındaki bir küme için Livy işleri gönderme

Bir Azure Sanal Ağı'nın içinden bir HDInsight Spark kümesine bağlanırsanız, kümedeki Livy'ye doğrudan bağlanabilirsiniz. Böyle bir durumda, Livy bitiş noktası `http://<IP address of the headnode>:8998/batches`için URL'dir. Burada, **8998** Livy küme headnode üzerinde çalışan bağlantı noktasıdır. Halka açık olmayan bağlantı noktalarındaki hizmetlere erişim hakkında daha fazla bilgi için, [HDInsight'ta Apache Hadoop hizmetleri tarafından kullanılan Bağlantı Noktaları'na](../hdinsight-hadoop-port-settings-for-services.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Livy REST API belgeleri](https://livy.incubator.apache.org/docs/latest/rest-api.html)
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
