---
title: Azure HDInsight'ta Apache Sqoop ile veri aktarmak için Curl'i kullanın
description: Curl'ü kullanarak Apache Sqoop işlerini Azure HDInsight'a uzaktan nasıl göndereceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: da29785547d1b6eb4b38d07f020ba885dc5137ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75767595"
---
# <a name="run-apache-sqoop-jobs-in-hdinsight-with-curl"></a>Curl ile HDInsight'ta Apache Sqoop işlerini çalıştırın

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

HDInsight'ta Apache Hadoop kümesinde Apache Sqoop işlerini çalıştırmak için Curl'u nasıl kullanacağınızı öğrenin. Bu makalede, Azure Depolama'dan veri dışa aktarılabilmek ve Curl kullanarak sql server veritabanına nasıl aktarılanın. Bu makale, [HDInsight Hadoop ile Kullanım Apache Sqoop](./hdinsight-use-sqoop.md)bir devamıdır.

Curl, Sqoop işlerinin sonuçlarını çalıştırmak, izlemek ve almak için ham HTTP isteklerini kullanarak HDInsight ile nasıl etkileşimkurabileceğinizi göstermek için kullanılır. Bu, HDInsight kümeniz tarafından sağlanan WebHCat REST API'sını (eski adıyla Templeton olarak da bilinir) kullanarak çalışır.

## <a name="prerequisites"></a>Ön koşullar

* [HDInsight'ta Hadoop ile Apache Sqoop'u Kullanın'dan](./hdinsight-use-sqoop.md) [test ortamını ayarlama.](./hdinsight-use-sqoop.md#create-cluster-and-sql-database)

* Azure SQL Veritabanı'nı sorgulamak için bir istemci. SQL [Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md) veya Visual Studio [Code'u](../../sql-database/sql-database-connect-query-vscode.md)kullanmayı düşünün.

* [Kıvrılmak](https://curl.haxx.se/). Curl, HDInsight kümesinden veya HDInsight kümesine veri aktarmak için bir araçtır.

* [jq](https://stedolan.github.io/jq/). JQ yardımcı programı, REST isteklerinden döndürülen JSON verilerini işlemek için kullanılır.

* Sqoop'a aşinalık. Daha fazla bilgi için [Sqoop Kullanım Kılavuzu'na](https://sqoop.apache.org/docs/1.4.7/SqoopUserGuide.html)bakın.

## <a name="submit-apache-sqoop-jobs-by-using-curl"></a>Curl kullanarak Apache Sqoop işleri gönderin

Azure Depolama'dan SQL Server'a Apache Sqoop işlerini kullanarak veri dışa aktarmak için Curl'i kullanın.

> [!NOTE]  
> Curl’ü veya WebHCat ile başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adı ve parolasını sağlayarak isteklerin kimliğini doğrulamanız gerekir. Ayrıca, sunucuya istek göndermek için kullanılan Tekdüzen Kaynak Tanımlayıcısı’nın (URI) bir parçası olarak küme adını kullanmanız gerekir.

Bu bölümdeki komutlar için, kümeye kimlik doğrulaması yapmak için kullanıcıyla değiştirin `USERNAME` ve kullanıcı hesabının parolasıyla değiştirin. `PASSWORD` `CLUSTERNAME` değerini kümenizin adıyla değiştirin.

REST API’sinin güvenliği [temel kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesi için istekleri her zaman Güvenli HTTP (HTTPS) kullanarak yapmanız gerekir.

1. Kullanım kolaylığı için aşağıdaki değişkenleri ayarlayın. Bu örnek, ortamınız için gerektiği gibi gözden geçirin, windows ortamını temel alın.

    ```cmd
    set CLUSTERNAME=
    set USERNAME=admin
    set PASSWORD=
    set SQLDATABASESERVERNAME=
    set SQLDATABASENAME=
    set SQLPASSWORD=
    set SQLUSER=sqluser
    ```

1. HDInsight kümenize bağlanabildiğinizi doğrulamak için bir komut satırında aşağıdaki komutu kullanın:

    ```cmd
    curl -u %USERNAME%:%PASSWORD% -G https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/status
    ```

    Aşağıdakine benzer bir yanıt almanız gerekir:

    ```json
    {"status":"ok","version":"v1"}
    ```

1. Bir sqoop iş göndermek için aşağıdakileri kullanın:

    ```cmd
    curl -u %USERNAME%:%PASSWORD% -d user.name=%USERNAME% -d command="export --connect jdbc:sqlserver://%SQLDATABASESERVERNAME%.database.windows.net;user=%SQLUSER%@%SQLDATABASESERVERNAME%;password=%PASSWORD%;database=%SQLDATABASENAME% --table log4jlogs --export-dir /example/data/sample.log --input-fields-terminated-by \0x20 -m 1" -d statusdir="wasb:///example/data/sqoop/curl" https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/sqoop
    ```

    Bu komutta kullanılan parametreler aşağıdaki gibidir:

   * **-d** - `-G` Kullanılmadığından, istek VARSAYıLAN OLARAK POST yöntemine göre kullanılır. `-d`istekle birlikte gönderilen veri değerlerini belirtir.

       * **user.name** - Komutu çalıştıran kullanıcı.

       * **komutu** - Sqoop komutu yürütmek için.

       * **statusdir** - Bu işin durumunun yazılacağına dair dizin.

     Bu komut, işin durumunu denetlemek için kullanılabilecek bir iş kimliği döndürecek.

       ```json
       {"id":"job_1415651640909_0026"}
       ```

1. İşin durumunu denetlemek için aşağıdaki komutu kullanın. Önceki `JOBID` adımda döndürülen değerle değiştirin. Örneğin, iade değeri ise, `{"id":"job_1415651640909_0026"}` `JOBID` o `job_1415651640909_0026`zaman . Gerektiğinde konumunu `jq` gözden geçirin.

    ```cmd
    set JOBID=job_1415651640909_0026

    curl -G -u %USERNAME%:%PASSWORD% -d user.name=%USERNAME% https://%CLUSTERNAME%.azurehdinsight.net/templeton/v1/jobs/%JOBID% | C:\HDI\jq-win64.exe .status.state
    ```

    İş bittiyse, devlet **BAŞARILI**OLACAKTIR.

   > [!NOTE]  
   > Bu Curl isteği, iş hakkında bilgi içeren bir JavaScript Nesne Gösterimi (JSON) belgesini döndürür; jq yalnızca durum değerini almak için kullanılır.

1. İşin durumu **BAŞARıLı**olarak değiştirildikten sonra, işin sonuçlarını Azure Blob depolama dan alabilirsiniz. Sorguyla `statusdir` geçirilen parametre, çıktı dosyasının konumunu içerir; Bu durumda, `wasb:///example/data/sqoop/curl`. Bu adres, HDInsight kümeniz `example/data/sqoop/curl` tarafından kullanılan varsayılan depolama kapsayıcısında iş çıktısını dizinde saklar.

    Stderr ve stdout blobs erişmek için Azure portalını kullanabilirsiniz.

1. Verilerin dışa aktarıldığını doğrulamak için, dışa aktarılan verileri görüntülemek için SQL istemcinizden aşağıdaki sorguları kullanın:

    ```sql
    SELECT COUNT(*) FROM [dbo].[log4jlogs] WITH (NOLOCK);
    SELECT TOP(25) * FROM [dbo].[log4jlogs] WITH (NOLOCK);
    ```

## <a name="limitations"></a>Sınırlamalar

* Toplu dışa aktarma - Linux tabanlı HDInsight ile, Verileri Microsoft SQL Server veya Azure SQL Veritabanı'na aktarmak için kullanılan Sqoop bağlayıcısı şu anda toplu ekleri desteklemiyor.
* Toplu İşlem - Linux tabanlı HDInsight `-batch` ile, eklemeleri gerçekleştirirken anahtarı kullanırken, Sqoop ekleme işlemlerini toplu olarak yapmak yerine birden çok kesici uç gerçekleştirecektir.

## <a name="summary"></a>Özet

Bu belgede gösterildiği gibi, HDInsight kümenizde Sqoop işlerinin sonuçlarını çalıştırmak, izlemek ve görüntülemek için ham bir HTTP isteği kullanabilirsiniz.

Bu makalede kullanılan REST arabirimi hakkında daha fazla bilgi <a href="https://sqoop.apache.org/docs/1.99.3/RESTAPI.html" target="_blank">için, Apache Sqoop REST API kılavuzuna</a>bakın.

## <a name="next-steps"></a>Sonraki adımlar

[HDInsight'ta Apache Hadoop ile Apache Sqoop kullanın](hdinsight-use-sqoop.md)

Curl içeren diğer HDInsight makaleleri için:

* [Azure REST API'sini kullanarak Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)
* [REST kullanarak HDInsight'ta Apache Hadoop ile Apache Hive sorgularını çalıştırın](apache-hadoop-use-hive-curl.md)
* [REST kullanarak HDInsight'ta Apache Hadoop ile MapReduce işleri çalıştırın](apache-hadoop-use-mapreduce-curl.md)