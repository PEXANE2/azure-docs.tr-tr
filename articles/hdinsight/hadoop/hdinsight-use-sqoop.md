---
title: Azure HDInsight (Apache Hadoop) ile Apache Sqoop işlerini çalıştırın
description: Bir Hadoop kümesi ile Azure SQL veritabanı arasında Sqoop alma ve dışa aktarma çalıştırmak için bir iş istasyonundan Azure PowerShell'i nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/06/2019
ms.openlocfilehash: 8353c0fba034022a79570d09b320b7b5c4c3e60a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74951862"
---
# <a name="use-apache-sqoop-with-hadoop-in-hdinsight"></a>HDInsight'ta Hadoop ile Apache Sqoop'u kullanma

[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

HDInsight kümesi ile Azure SQL veritabanı arasında veri almak ve dışa aktarmak için HDInsight'ta Apache Sqoop'u nasıl kullanacağınızı öğrenin.

Apache Hadoop, günlükler ve dosyalar gibi yapılandırılmamış ve yarı yapılandırılmış verileri işlemek için doğal bir seçim olsa da, ilişkisel veritabanlarında depolanan yapılandırılmış verileri işlemek için de bir ihtiyaç olabilir.

[Apache Sqoop](https://sqoop.apache.org/docs/1.99.7/user.html) Hadoop kümeleri ve ilişkisel veritabanları arasında veri aktarmak için tasarlanmış bir araçtır. SQL Server, MySQL veya Oracle gibi ilişkisel veritabanı yönetim sisteminden (RDBMS) Hadoop dağıtılmış dosya sistemine (HDFS) veri almak, MapReduce veya Apache Hive ile Hadoop'taki verileri dönüştürmek ve verileri rdbms'e aktarmak için kullanabilirsiniz. . Bu makalede, ilişkisel veritabanınız için bir SQL Server veritabanı kullanıyorsunuz.

> [!IMPORTANT]  
> Bu makalede, veri aktarımını gerçekleştirmek için bir test ortamı ayarlar. Daha sonra daha aşağıda, bölüm [Run Sqoop işleri](#run-sqoop-jobs)yöntemlerinden biri bu ortam için bir veri aktarım yöntemi seçin.

HDInsight kümelerinde desteklenen Sqoop sürümleri için [HDInsight tarafından sağlanan küme sürümlerinde yeniliklere bakın?](../hdinsight-component-versioning.md)

## <a name="understand-the-scenario"></a>Senaryoyu anlama

HDInsight kümesi bazı örnek verilerle birlikte gelir. Aşağıdaki iki örneği kullanırsınız:

* Bir Apache Log4j günlük dosyası, `/example/data/sample.log`hangi bulunmaktadır . Aşağıdaki günlükler dosyadan ayıklanır:

```text
2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
...
```

* 'de bulunan `hivesampletable`veri dosyasına başvuran hive tablosu adlı bir tablo `/hive/warehouse/hivesampletable` Tablo bazı mobil cihaz verileri içerir.
  
  | Alan | Veri türü |
  | --- | --- |
  | Clientıd |string |
  | sorgu süresi |string |
  | Pazar |string |
  | cihaz platformu |string |
  | cihaz make |string |
  | cihaz modeli |string |
  | durum |string |
  | ülke |string |
  | querydwelltime |double |
  | Sessionıd |bigint |
  | sessionpagevieworder |bigint |

Bu makalede, Sqoop alma ve dışa aktarma yı test etmek için bu iki veri kümesini kullanırsınız.

## <a name="set-up-test-environment"></a><a name="create-cluster-and-sql-database"></a>Test ortamını ayarlama

Küme, SQL veritabanı ve diğer nesneler Azure portalı aracılığıyla bir Azure Kaynak Yöneticisi şablonu kullanılarak oluşturulur. Şablon Azure [quickstart şablonlarında](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/)bulunabilir. Kaynak Yöneticisi şablonu, tablo şemalarını bir SQL veritabanına dağıtmak için bir bacpac paketi çağırır.  Bacpac paketi halka açık bir blob https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpackabında yer alır. Bacpac dosyaları için özel bir kapsayıcı kullanmak istiyorsanız, şablonda aşağıdaki değerleri kullanın:

```json
"storageKeyType": "Primary",
"storageKey": "<TheAzureStorageAccountKey>",
```

> [!NOTE]  
> Şablon kullanarak içe aktarma veya Azure portalı yalnızca Azure blob depolamadan bir BACPAC dosyası alma yı destekler.

1. Azure portalında Kaynak Yöneticisi şablonu açmak için aşağıdaki resmi seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

2. Aşağıdaki özellikleri girin:

    |Alan |Değer |
    |---|---|
    |Abonelik |Açılan listeden Azure aboneliğinizi seçin.|
    |Kaynak grubu |Açılan listeden kaynak grubunuzu seçin veya yeni bir tane oluşturun|
    |Konum |Açılan listeden bir bölge seçin.|
    |Küme Adı |Hadoop kümesi için bir ad girin. Yalnızca küçük harf kullanın.|
    |Küme Oturum Açma Kullanıcı Adı |Önceden doldurulmuş değeri `admin`koruyun.|
    |Küme Oturum Açma Parolası |Parola girin.|
    |Ssh Kullanıcı Adı |Önceden doldurulmuş değeri `sshuser`koruyun.|
    |Ssh Şifre |Parola girin.|
    |Sql Yönetici Girişi |Önceden doldurulmuş değeri `sqluser`koruyun.|
    |Sql Yönetici Şifresi |Parola girin.|
    |_artifacts Konumu | Kendi bacpac dosyanızı farklı bir konumda kullanmak istemiyorsanız varsayılan değeri kullanın.|
    |_artifacts Konum Sas Jeton |Boş bırakın.|
    |Bacpac Dosya Adı |Kendi bacpac dosyanızı kullanmak istemiyorsanız varsayılan değeri kullanın.|
    |Konum |Varsayılan değeri kullanın.|

    Azure SQL Server adı `<ClusterName>dbserver`. Veritabanı `<ClusterName>db`adı. Varsayılan depolama hesabı adı `e6qhezrh2pdqu`.

3. **Seç ben yukarıda belirtilen hüküm ve koşulları kabul .**

4. **Satın al**'ı seçin. Şablon dağıtımı için dağıtım gönderme başlıklı yeni bir döşeme görürsünüz. Kümenin ve SQL Database’in oluşturulması yaklaşık 20 dakika sürer.

## <a name="run-sqoop-jobs"></a>Sqoop işleri çalıştırın

HDInsight çeşitli yöntemler kullanarak Sqoop işleri çalıştırabilirsiniz. Hangi yöntemin sizin için doğru olduğuna karar vermek için aşağıdaki tabloyu kullanın ve ardından bir gözden geçirme için bağlantıyı izleyin.

| **İstersenbunu kullan...** | ... **etkileşimli** bir kabuk | ... **toplu** işleme | ... bu **istemci işletim sisteminden** |
|:--- |:---:|:---:|:--- |:--- |
| [Ssh](apache-hadoop-use-sqoop-mac-linux.md) |? |? |Linux, Unix, Mac OS X veya Windows |
| [Hadoop için .NET SDK](apache-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |?  |Windows (şimdilik) |
| [Azure PowerShell](apache-hadoop-use-sqoop-powershell.md) |&nbsp; |? |Windows |

## <a name="limitations"></a>Sınırlamalar

* Toplu dışa aktarma - Linux tabanlı HDInsight ile, Verileri Microsoft SQL Server veya Azure SQL Veritabanı'na aktarmak için kullanılan Sqoop bağlayıcısı şu anda toplu ekleri desteklemiyor.
* Toplu İşlem - Linux tabanlı HDInsight `-batch` ile, Eklemeler yaparken anahtarı kullanırken, Sqoop ekleme işlemlerini toplu olarak yapmak yerine birden çok kesici uç gerçekleştirir.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Sqoop'u kullanmayı öğrendin. Daha fazla bilgi için bkz:

* [HDInsight ile Apache Hive'ı kullanma](../hdinsight-use-hive.md)
* [HDInsight'a veri yükleme](../hdinsight-upload-data.md): HDInsight/Azure Blob depolamasına veri yüklemek için başka yöntemler bulun.
* [HDInsight üzerinde Apache Hadoop ile SQL Veritabanı arasında veri içeri ve dışarı aktarma işlemleri için Apache Sqoop'u kullanma](./apache-hadoop-use-sqoop-mac-linux.md)