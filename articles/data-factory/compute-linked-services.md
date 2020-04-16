---
title: Azure Veri Fabrikası tarafından desteklenen bilgi işlem ortamları
description: Verileri dönüştürmek veya işlemek için Azure Veri Fabrikası ardışık hatlarında (Azure HDInsight gibi) kullanabileceğiniz bilgi işlem ortamları hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.date: 10/10/2019
ms.openlocfilehash: 63843230b3d4a521df858b00c8e5c887e8f53a7a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415587"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Azure Veri Fabrikası tarafından desteklenen bilgi işlem ortamları

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, verileri işlemek veya dönüştürmek için kullanabileceğiniz farklı bilgi işlem ortamları açıklanmaktadır. Ayrıca, bu bilgi işlem ortamlarını bir Azure veri fabrikasına bağlayan bağlantılı hizmetleri yapılandırırken Veri Fabrikası tarafından desteklenen farklı yapılandırmalar (isteğe bağlı ve kendi yapılandırmanızı getir) hakkında ayrıntılı bilgi sağlar.

Aşağıdaki tablo, Veri Fabrikası tarafından desteklenen bilgi işlem ortamlarının ve bunların üzerinde çalıştırılabilecek etkinliklerin bir listesini sağlar. 

| İşlem ortamı                                          | etkinlikler                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [İsteğe bağlı HDInsight kümesi](#azure-hdinsight-on-demand-linked-service) veya [kendi HDInsight kümeniz](#azure-hdinsight-linked-service) | [Kovan](transform-data-using-hadoop-hive.md), [Domuz](transform-data-using-hadoop-pig.md), [Kıvılcım](transform-data-using-spark.md), [MapReduce](transform-data-using-hadoop-map-reduce.md), [Hadoop Streaming](transform-data-using-hadoop-streaming.md) |
| [Azure Batch](#azure-batch-linked-service)                   | [Özel](transform-data-using-dotnet-custom-activity.md)     |
| [Azure Machine Learning Studio](#azure-machine-learning-studio-linked-service) | [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](transform-data-using-machine-learning.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Azure Machine Learning Execute Pipeline](transform-data-machine-learning-service.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Azure Machine Learning Execute Pipeline](transform-data-machine-learning-service.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md) |
| [Azure SQL](#azure-sql-database-linked-service), [Azure SQL Veri Ambarı](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Depolanan Yordam](transform-data-using-stored-procedure.md) |
| [Azure Databricks](#azure-databricks-linked-service)         | [Not Defteri](transform-data-databricks-notebook.md), [Kavanoz](transform-data-databricks-jar.md), [Python](transform-data-databricks-python.md) |
| [Azure İşlevi](#azure-function-linked-service)         | [Azure İşlevi etkinliği](control-flow-azure-function-activity.md)
>  

## <a name="on-demand-hdinsight-compute-environment"></a>İsteğe bağlı HDInsight bilgi işlem ortamı

Bu tür yapılandırmalarda, bilgi işlem ortamı azure veri fabrikası hizmeti tarafından tamamen yönetilir. Bir iş işlem verilerine gönderilmeden ve iş tamamlandığında kaldırılmadan önce Veri Fabrikası hizmeti tarafından otomatik olarak oluşturulur. İsteğe bağlı bilgi işlem ortamı için bağlantılı bir hizmet oluşturabilir, yapılandırabilir ve iş yürütme, küme yönetimi ve önyükleme eylemleri için parçalı ayarları denetleyebilirsiniz.

> [!NOTE]
> İsteğe bağlı yapılandırma şu anda yalnızca Azure HDInsight kümeleri için desteklenir. Azure Databricks, iş kümelerini kullanarak isteğe bağlı işleri de destekler, daha fazla ayrıntı için [Azure veri tuğlaları bağlantılı hizmete](#azure-databricks-linked-service) bakın.

## <a name="azure-hdinsight-on-demand-linked-service"></a>İsteğe bağlı Azure HDInsight bağlı hizmeti

Azure Veri Fabrikası hizmeti, verileri işlemek için isteğe bağlı HDInsight kümesini otomatik olarak oluşturabilir. Küme, kümeyle ilişkili depolama hesabıyla (JSON'daki linkedServiceName özelliği) aynı bölgede oluşturulur. Depolama hesabı genel amaçlı standart bir Azure Depolama hesabı olmalıdır. 

İsteğe bağlı HDInsight bağlantılı hizmet le ilgili aşağıdaki **önemli** noktalara dikkat edin:

* İsteğe bağlı HDInsight kümesi Azure aboneliğiniz altında oluşturulur. Küme çalışmaya başladığında, azure portalınızda kümeyi görebilirsiniz. 
* İsteğe bağlı HDInsight kümesinde çalıştırılan işlerin günlükleri, HDInsight kümesiyle ilişkili depolama hesabına kopyalanır. ClusterUserName, clusterPassword, clusterSshUserName, clusterSshPassword bağlı hizmet tanımınızda tanımlanan sshPassword kümenin yaşam döngüsü sırasında derinlemesine sorun giderme için kümede oturum açmak için kullanılır. 
* Yalnızca HDInsight kümesinin çalışır durumda olduğu ve çalıştırılan süre için ücretlendirilirsiniz.
* Azure HDInsight isteğe bağlı bağlantılı hizmetile bir **Komut Dosyası Eylemi** kullanabilirsiniz.  

> [!IMPORTANT]
> Bir Azure HDInsight kümesini isteğe bağlı olarak sağlamak genellikle **20 dakika** veya daha uzun sürer.

### <a name="example"></a>Örnek

Aşağıdaki JSON, Linux tabanlı isteğe bağlı HDInsight bağlantılı bir hizmeti tanımlar. Veri Fabrikası hizmeti, gerekli etkinliği işlemek için otomatik olarak **Linux tabanlı** bir HDInsight kümesi oluşturur. 

```json
{
  "name": "HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "clusterType": "hadoop",
      "clusterSize": 1,
      "timeToLive": "00:15:00",
      "hostSubscriptionId": "<subscription ID>",
      "servicePrincipalId": "<service principal ID>",
      "servicePrincipalKey": {
        "value": "<service principal key>",
        "type": "SecureString"
      },
      "tenant": "<tenent id>",
      "clusterResourceGroup": "<resource group name>",
      "version": "3.6",
      "osType": "Linux",
      "linkedServiceName": {
        "referenceName": "AzureStorageLinkedService",
        "type": "LinkedServiceReference"
      }
    },
    "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
  }
}
```

> [!IMPORTANT]
> HDInsight kümesi, JSON **(linkedServiceName)** olarak belirttiğiniz blob depolama alanında varsayılan bir **kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu davranış tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmetiyle, HDInsight kümesi her oluşturulduğunda, burada mevcut canlı bir küme (**timeToLive**) olmadıkça bir dilim gerekir ve işlem bittiğinde silinir. 
>
> Daha fazla etkinlik çalışırken, Azure blob depolama alanınızda birçok kapsayıcı görürsünüz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcı adları bir düzene sahiptir: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](https://storageexplorer.com/) gibi araçları kullanın.

### <a name="properties"></a>Özellikler

| Özellik                     | Açıklama                              | Gerekli |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Tür özelliği **HDInsightOnDemand**olarak ayarlanmalıdır. | Evet      |
| clusterSize                  | Kümedeki alt/veri düğümlerinin sayısı. HDInsight kümesi, bu özellik için belirttiğiniz işçi düğümlerinin sayısıyla birlikte 2 kafa düğümüyle oluşturulur. Düğümler 4 çekirdekli boyut Standard_D3, bu nedenle 4 işçi düğüm kümesi 24\*çekirdek alır (4 4 = 16\*işçi düğümleri için çekirdek, artı 2 4 = kafa düğümleri için 8 çekirdek). Ayrıntılar için [Hadoop, Spark, Kafka ve daha fazlası ile HDInsight'ta kümeleri ayarlama'ya](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) bakın. | Evet      |
| linkedServiceName            | Azure Depolama bağlantılı hizmet, verileri depolamak ve işlemek için isteğe bağlı küme tarafından kullanılacak. HDInsight kümesi, bu Azure Depolama hesabıyla aynı bölgede oluşturulur. Azure HDInsight, desteklediği her bir Azure bölgesinde kullanabileceğiniz toplam çekirdek sayısıyla ilgili sınırlamaya sahiptir. Bu Azure bölgesinde gerekli clusterSize'ı karşılayacak yeterli çekirdek kotanız olduğundan emin olun. Ayrıntılar [için, Hadoop, Spark, Kafka ve daha fazlası ile HDInsight'ta kümeler ayarlama'ya](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) bakın<p>Şu anda, depolama alanı olarak Azure Veri Gölü Depolama (Gen 2) kullanan isteğe bağlı bir HDInsight kümesi oluşturamazsınız. HDInsight işlemenin sonuç verilerini Bir Azure Veri Gölü Depolama 'sında (Gen 2) depolamak istiyorsanız, Verileri Azure Blob Depolama'dan Azure Veri Gölü Depolamasına (Gen 2) kopyalamak için bir Kopyalama Etkinliği kullanın. </p> | Evet      |
| clusterResourceGroup         | HDInsight kümesi bu kaynak grubunda oluşturulur. | Evet      |
| timetolive                   | İsteğe bağlı HDInsight kümesi için izin verilen boşta kalma süresi. İsteğe bağlı HDInsight kümesinin, kümede başka etkin iş yoksa, bir etkinlik çalışması tamamlandıktan sonra ne kadar süre hayatta kalacağını belirtir. İzin verilen minimum değer 5 dakikadır (00:05:00).<br/><br/>Örneğin, bir etkinlik çalışması 6 dakika sürerse ve zaman 5 dakika olarak ayarlanırsa, küme, 6 dakikalık etkinlik çalışması işledikten sonra 5 dakika boyunca canlı kalır. Başka bir etkinlik çalışması 6 dakikalık pencere ile yürütülür, aynı küme tarafından işlenir.<br/><br/>İsteğe bağlı HDInsight kümesi oluşturmak pahalı bir işlemdir (biraz zaman alabilir), bu nedenle isteğe bağlı HDInsight kümesini yeniden kullanarak bir veri fabrikasının performansını artırmak için bu ayarı gerektiği gibi kullanın.<br/><br/>Saati 0'a ayarlarsanız, etkinlik çalışır tamamlanmaz küme silinir. Ancak, yüksek bir değer belirlerseniz, küme bazı sorun giderme amacıyla oturum açmanız için boşta kalabilir, ancak yüksek maliyetlere neden olabilir. Bu nedenle, gereksinimlerinize göre uygun değeri ayarlamanız önemlidir.<br/><br/>Zaman ayarı özellik değeri uygun şekilde ayarlanırsa, birden çok ardışık alan, isteğe bağlı HDInsight kümesinin örneğini paylaşabilir. | Evet      |
| clusterType                  | Oluşturulacak HDInsight kümesinin türü. İzin verilen değerler "hadoop" ve "kıvılcım"tır. Belirtilmemişse, varsayılan değer hadoop'dur. Kurumsal Güvenlik Paketi etkin küme isteğe bağlı oluşturulamaz, bunun yerine varolan bir küme yi [kullanın/ kendi işleminizi getirin.](#azure-hdinsight-linked-service) | Hayır       |
| version                      | HDInsight kümesinin sürümü. Belirtilmemişse, geçerli HDInsight tanımlı varsayılan sürümünü kullanıyor. | Hayır       |
| hostSubscriptionId           | HDInsight kümesini oluşturmak için kullanılan Azure abonelik kimliği. Belirtilmemişse, Azure giriş bağlamınızın Abonelik Kimliğini kullanır. | Hayır       |
| clusterNamePrefix           | HDI küme adının öneki, küme adının sonunda otomatik olarak bir zaman damgası| Hayır       |
| sparkVersion                 | Küme türü "Kıvılcım" ise kıvılcım sürümü | Hayır       |
| ekLinkedServiceNames | Veri Fabrikası hizmetinin sizin adınıza kaydedilebilmeleri için HDInsight bağlantılı hizmet için ek depolama hesapları belirtir. Bu depolama hesapları, linkedServiceName tarafından belirtilen depolama hesabıyla aynı bölgede oluşturulan HDInsight kümesiyle aynı bölgede olmalıdır. | Hayır       |
| osType                       | İşletim sistemi türü. İzin verilen değerler şunlardır: Linux ve Windows (yalnızca HDInsight 3.3 için). Varsayılan Linux'dur. | Hayır       |
| hcatalogLinkedServiceName    | HCatalog veritabanını gösteren Azure SQL bağlantılı hizmetin adı. İsteğe bağlı HDInsight kümesi, Azure SQL Veritabanı'nı metastore olarak kullanarak oluşturulur. | Hayır       |
| connectVia                   | Etkinlikleri bu HDInsight bağlantılı hizmete göndermek için kullanılacak Tümleştirme Çalışma Süresi. İsteğe bağlı HDInsight bağlantılı hizmet için yalnızca Azure Tümleştirme Çalışma Süresini destekler. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Hayır       |
| clusterKullanıcı Adı                   | Kümeye erişmek için kullanıcı adı. | Hayır       |
| clusterPassword                   | Kümeye erişmek için güvenli dize türünde parola. | Hayır       |
| kümeSshUserName         | SSH kullanıcı adı uzaktan küme düğümü (Linux için) bağlanır. | Hayır       |
| kümeSshPassword         | SSH'ye güvenli dize türündeki parola, kümenin düğümünü (Linux için) uzaktan bağlar. | Hayır       |
| scriptEylemler | İsteğe bağlı küme oluşturma sırasında [HDInsight küme özelleştirmeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux) için komut dosyası belirtin. <br />Şu anda, Azure Veri Fabrikası'nın Kullanıcı Arabirimi yazma aracı yalnızca 1 komut dosyası eylemi belirtmeyi destekler, ancak JSON'da bu sınırlamayı atlatabilirsiniz (JSON'da birden çok komut dosyası eylemi belirtin). | Hayır |


> [!IMPORTANT]
> HDInsight, dağıtılabilen birden çok Hadoop küme sürümlerini destekler. Her sürüm seçimi Hortonworks Veri Platformu (HDP) dağıtımının belirli bir sürümünü ve bu dağıtım içinde bulunan bileşenler kümesini oluşturur. Desteklenen HDInsight sürümlerinin listesi, en son Hadoop ekosistem bileşenlerini ve düzeltmelerini sağlamak için güncellenmeye devam ediyor. HDInsight'ın desteklenen sürümünü kullandığınızdan emin olmak için [desteklenen HDInsight sürümü ve Işletim Sistemi Türü](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) ile ilgili en son bilgilere her zaman bakındığından emin olun. 
>
> [!IMPORTANT]
> Şu anda, HDInsight bağlantılı hizmetler HBase, Interactive Query (Hive LLAP), Storm'u desteklemiyor. 

#### <a name="additionallinkedservicenames-json-example"></a>ekLinkedServiceNames JSON örneği

```json
"additionalLinkedServiceNames": [{
    "referenceName": "MyStorageLinkedService2",
    "type": "LinkedServiceReference"          
}]
```

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

İsteğe Bağlı HDInsight bağlantılı hizmet, sizin adınıza HDInsight kümeleri oluşturmak için bir hizmet temel kimlik doğrulaması gerektirir. Hizmet temel kimlik doğrulamasını kullanmak için, azure Etkin Dizini'ne (Azure AD) bir uygulama varlığı kaydedin ve aboneliğin veya HDInsight kümesinin oluşturulduğu kaynak grubunun **Katılımcı** rolünü ona bahşedin. Ayrıntılı adımlar için, [kaynaklara erişebilen bir Azure Etkin Dizin uygulaması ve hizmet ilkesi oluşturmak için Portalı Kullan'a](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal)bakın. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:

- Uygulama Kimliği
- Uygulama anahtarı 
- Kiracı Kimliği

Aşağıdaki özellikleri belirterek hizmet temel kimlik doğrulamasını kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| **hizmetPrincipalId**  | Uygulamanın istemci kimliğini belirtin.     | Evet      |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin.           | Evet      |
| **Kiracı**              | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | Evet      |

### <a name="advanced-properties"></a>Gelişmiş Özellikler

İsteğe bağlı HDInsight kümesinin parçalı yapılandırması için aşağıdaki özellikleri de belirtebilirsiniz.

| Özellik               | Açıklama                              | Gerekli |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | OLUŞTURULACAK HDInsight kümesi için temel yapılandırma parametrelerini (core-site.xml'de olduğu gibi) belirtir. | Hayır       |
| hBaseConfiguration     | HDInsight kümesi için HBase yapılandırma parametrelerini (hbase-site.xml) belirtir. | Hayır       |
| hdfsYapılandırma      | HDInsight kümesi için HDFS yapılandırma parametrelerini (hdfs-site.xml) belirtir. | Hayır       |
| kovanYapılandırma      | HDInsight kümesi için kovan yapılandırma parametrelerini (kovan-site.xml) belirtir. | Hayır       |
| mapReduceConfiguration | HDInsight kümesi için MapReduce yapılandırma parametrelerini (mapred-site.xml) belirtir. | Hayır       |
| oozieConfiguration     | HDInsight kümesi için Oozie yapılandırma parametrelerini (oozie-site.xml) belirtir. | Hayır       |
| stormConfiguration     | HDInsight kümesi için Fırtına yapılandırma parametrelerini (storm-site.xml) belirtir. | Hayır       |
| iplikYapılandırma      | HDInsight kümesi için İplik konfigürasyon parametrelerini (iplik-site.xml) belirtir. | Hayır       |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Örnek – Gelişmiş özelliklere sahip isteğe bağlı HDInsight küme yapılandırması

```json
{
    "name": " HDInsightOnDemandLinkedService",
    "properties": {
      "type": "HDInsightOnDemand",
      "typeProperties": {
          "clusterSize": 16,
          "timeToLive": "01:30:00",
          "hostSubscriptionId": "<subscription ID>",
          "servicePrincipalId": "<service principal ID>",
          "servicePrincipalKey": {
            "value": "<service principal key>",
            "type": "SecureString"
          },
          "tenant": "<tenent id>",
          "clusterResourceGroup": "<resource group name>",
          "version": "3.6",
          "osType": "Linux",
          "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
            },
            "coreConfiguration": {
                "templeton.mapper.memory.mb": "5000"
            },
            "hiveConfiguration": {
                "templeton.mapper.memory.mb": "5000"
            },
            "mapReduceConfiguration": {
                "mapreduce.reduce.java.opts": "-Xmx4000m",
                "mapreduce.map.java.opts": "-Xmx4000m",
                "mapreduce.map.memory.mb": "5000",
                "mapreduce.reduce.memory.mb": "5000",
                "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
            },
            "yarnConfiguration": {
                "yarn.app.mapreduce.am.resource.mb": "5000",
                "mapreduce.map.memory.mb": "5000"
            },
            "additionalLinkedServiceNames": [{
                "referenceName": "MyStorageLinkedService2",
                "type": "LinkedServiceReference"          
            }]
        }
    },
      "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
}
```

### <a name="node-sizes"></a>Düğüm boyutları
Aşağıdaki özellikleri kullanarak baş, veri ve zookeeper düğümlerinin boyutlarını belirtebilirsiniz: 

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Kafa düğümünün boyutunu belirtir. Varsayılan değer: Standard_D3. Ayrıntılar için düğüm **boyutlarını belirtme** bölümüne bakın. | Hayır       |
| dataNodeSize      | Veri düğümünün boyutunu belirtir. Varsayılan değer: Standard_D3. | Hayır       |
| zookeeperNodeSize | Zoo Keeper düğümünün boyutunu belirtir. Varsayılan değer: Standard_D3. | Hayır       |

#### <a name="specifying-node-sizes"></a>Düğüm boyutlarını belirtme
Önceki bölümde belirtilen özellikler için belirtmeniz gereken dize değerleri için [Sanal Makinelerin Boyutları](../virtual-machines/linux/sizes.md) makalesine bakın. Değerlerin, makalede başvurulan **APIS & CMDLE'lere** uyması gerekir. Makalede görebileceğiniz gibi, Büyük (varsayılan) boyutundaki veri düğümü, senaryonuz için yeterli olmayabilir 7 GB belleğe sahiptir. 

D4 boyutlu kafa düğümleri ve alt düğümleri oluşturmak istiyorsanız, headNodeSize ve dataNodeSize özellikleri için değer olarak **Standard_D4** belirtin. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Bu özellikler için yanlış bir değer belirtirseniz, aşağıdaki **hatayı alabilirsiniz:** Küme oluşturmak için başarısız oldu. Özel durum: Küme oluşturma işlemi tamamlanamadı. İşlem '400' koduyla başarısız oldu. Küme geride bırakma durumu: 'Hata'. Mesaj: 'PreClusterCreationValidationFailure'. Bu hatayı aldığınızda, [Sanal Makinelerin Boyutları](../virtual-machines/linux/sizes.md) makalesinde tablodan **CMDLET & APIS** adını kullandığınızdan emin olun.        

## <a name="bring-your-own-compute-environment"></a>Kendi işlem ortamınızı getirin
Bu tür yapılandırmalarda, kullanıcılar zaten var olan bir bilgi işlem ortamını Veri Fabrikası'nda bağlantılı bir hizmet olarak kaydedebilirler. Bilgi işlem ortamı kullanıcı tarafından yönetilir ve Veri Fabrikası hizmeti bunu etkinlikleri yürütmek için kullanır.

Bu tür yapılandırmalar aşağıdaki işlem ortamları için desteklenir:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB, Azure SQL DW, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight bağlantılı hizmet
Kendi HDInsight kümenizi Veri Fabrikası'na kaydettirmek için Azure HDInsight bağlantılı bir hizmet oluşturabilirsiniz.

### <a name="example"></a>Örnek

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
      "type": "HDInsight",
      "typeProperties": {
        "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
        "userName": "username",
        "password": {
            "value": "passwordvalue",
            "type": "SecureString"
          },
        "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```

### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                                                  | Gerekli |
| ----------------- | ------------------------------------------------------------ | -------- |
| type              | Tür özelliği **HDInsight**olarak ayarlanmalıdır.            | Evet      |
| clusterUri        | HDInsight kümesinin URI'si.                            | Evet      |
| kullanıcı adı          | Varolan bir HDInsight kümesine bağlanmak için kullanılacak kullanıcının adını belirtin. | Evet      |
| password          | Kullanıcı hesabı için parola belirtin.                       | Evet      |
| linkedServiceName | HDInsight kümesi tarafından kullanılan Azure blob depolamasını ifade eden Azure Depolama bağlantılı hizmetin adı. <p>Şu anda, bu özellik için Bir Azure Veri Gölü Depolama (Gen 2) bağlantılı hizmet belirtemezsiniz. HDInsight kümesinin Veri Gölü Deposu'na erişimi varsa, Hive/Pig komut dosyasından Azure Veri Gölü Depolama (Gen 2) adresindeki verilere erişebilirsiniz. </p> | Evet      |
| isEspEtkin      | HDInsight kümesi [Kurumsal Güvenlik Paketi](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-architecture) etkinse '*true*' belirtin. Varsayılan '*yanlış*'. | Hayır       |
| connectVia        | Etkinlikleri bu bağlantılı hizmete göndermek için kullanılacak Tümleştirme Çalışma Zamanı. Azure Tümleştirme Çalışma Zamanı'nı veya Kendi kendine barındırılan Tümleştirme Çalışma Zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. <br />Kurumsal Güvenlik Paketi (ESP) için HDInsight cluster etkin küme, kümegörüş hattı olan veya ESP HDInsight küme ile aynı Sanal Ağ içinde dağıtılmalıdır kendi kendine barındırılan bir entegrasyon çalışma süresi kullanın. | Hayır       |

> [!IMPORTANT]
> HDInsight, dağıtılabilen birden çok Hadoop küme sürümlerini destekler. Her sürüm seçimi Hortonworks Veri Platformu (HDP) dağıtımının belirli bir sürümünü ve bu dağıtım içinde bulunan bileşenler kümesini oluşturur. Desteklenen HDInsight sürümlerinin listesi, en son Hadoop ekosistem bileşenlerini ve düzeltmelerini sağlamak için güncellenmeye devam ediyor. HDInsight'ın desteklenen sürümünü kullandığınızdan emin olmak için [desteklenen HDInsight sürümü ve Işletim Sistemi Türü](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) ile ilgili en son bilgilere her zaman bakındığından emin olun. 
>
> [!IMPORTANT]
> Şu anda, HDInsight bağlantılı hizmetler HBase, Interactive Query (Hive LLAP), Storm'u desteklemiyor. 
>
> 

## <a name="azure-batch-linked-service"></a>Azure Toplu İşlem bağlantılı hizmet

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir veri fabrikasına sanal makinelerden (VM) toplu iş havuzukaydetmek için Azure Toplu İşlem bağlantılı bir hizmet oluşturabilirsiniz. Azure Toplu İş'i kullanarak Özel etkinliği çalıştırabilirsiniz.

Azure Toplu İş hizmetinde yeniyseniz aşağıdaki makalelere bakın:

* Azure Toplu İşlem hizmetine genel bakış için [Azure Toplu İşlem temelleri.](../batch/batch-technical-overview.md)
* Azure portalını kullanarak Azure Toplu Hesabı oluşturmak için bir Azure Toplu İş hesabı (veya) [Azure portalı](../batch/batch-account-create-portal.md) oluşturmak için [Yeni-AzBatchAccount](/powershell/module/az.batch/New-azBatchAccount) cmdlet. Cmdlet'i kullanma yla ilgili ayrıntılı talimatlar için Azure Toplu İş Hesabı makalesini yönetmek için [PowerShell'i](https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) kullanma makalesine bakın.
* Azure Toplu İş havuzu oluşturmak için [Yeni-AzBatchPool](/powershell/module/az.batch/New-AzBatchPool) cmdlet.

### <a name="example"></a>Örnek

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
      "type": "AzureBatch",
      "typeProperties": {
        "accountName": "batchaccount",
        "accessKey": {
          "type": "SecureString",
          "value": "access key"
        },
        "batchUri": "https://batchaccount.region.batch.azure.com",
        "poolName": "poolname",
        "linkedServiceName": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```


### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| type              | Tür özelliği **AzureBatch**olarak ayarlanmalıdır. | Evet      |
| accountName       | Azure Toplu İş hesabının adı.         | Evet      |
| Accesskey         | Azure Toplu İş hesabı için erişim anahtarı.  | Evet      |
| batchUri          | Https://*batchaccountname.region*.batch.azure.com biçiminde Azure Toplu İş hesabınıza URL. | Evet      |
| poolName          | Sanal makineler havuzunun adı.    | Evet      |
| linkedServiceName | Bu Azure Toplu İş bağlantılı hizmetle ilişkili Azure Depolama bağlantılı hizmetin adı. Bu bağlantılı hizmet, etkinliği çalıştırmak için gereken dosyaları evreleme için kullanılır. | Evet      |
| connectVia        | Etkinlikleri bu bağlantılı hizmete göndermek için kullanılacak Tümleştirme Çalışma Zamanı. Azure Tümleştirme Çalışma Zamanı'nı veya Kendi kendine barındırılan Tümleştirme Çalışma Zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Hayır       |

## <a name="azure-machine-learning-studio-linked-service"></a>Azure Machine Learning Studio bağlantılı hizmet
Bir Veri fabrikasına Machine Learning toplu puanlama bitiş noktasını kaydetmek için Azure Machine Learning Studio bağlantılı bir hizmet oluşturursunuz.

### <a name="example"></a>Örnek

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
      "type": "AzureML",
      "typeProperties": {
        "mlEndpoint": "https://[batch scoring endpoint]/jobs",
        "apiKey": {
            "type": "SecureString",
            "value": "access key"
        }
     },
     "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
}
```

### <a name="properties"></a>Özellikler
| Özellik               | Açıklama                              | Gerekli                                 |
| ---------------------- | ---------------------------------------- | ---------------------------------------- |
| Tür                   | Tür özelliği şu şekilde ayarlanmalıdır: **AzureML**. | Evet                                      |
| mlEndpoint             | Toplu puanlama URL'si.                   | Evet                                      |
| apiKey                 | Yayınlanan çalışma alanı modelinin API'si.     | Evet                                      |
| updateResourceEndpoint | Öngörülü Web Hizmetini eğitilmiş model dosyasıyla güncelleştirmek için kullanılan Azure Machine Learning Web Hizmeti bitiş noktasının Kaynak URL'sini güncelleştir | Hayır                                       |
| hizmetPrincipalId     | Uygulamanın istemci kimliğini belirtin.     | UpdateResourceEndpoint belirtilirse gerekli |
| servicePrincipalKey    | Uygulamanın anahtarını belirtin.           | UpdateResourceEndpoint belirtilirse gerekli |
| Kiracı                 | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | UpdateResourceEndpoint belirtilirse gerekli |
| connectVia             | Etkinlikleri bu bağlantılı hizmete göndermek için kullanılacak Tümleştirme Çalışma Zamanı. Azure Tümleştirme Çalışma Zamanı'nı veya Kendi kendine barındırılan Tümleştirme Çalışma Zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Hayır                                       |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlantılı hizmet
Bir Azure Machine Learning çalışma alanını bir veri fabrikasına bağlamak için Azure Machine Learning bağlantılı bir hizmet oluşturursunuz.

> [!NOTE]
> Şu anda yalnızca hizmet temel kimlik doğrulaması Azure Machine Learning bağlantılı hizmet için desteklenir.

### <a name="example"></a>Örnek

```json
{
    "name": "AzureMLServiceLinkedService",
    "properties": {
        "type": "AzureMLService",
        "typeProperties": {
            "subscriptionId": "subscriptionId",
            "resourceGroupName": "resourceGroupName",
            "mlWorkspaceName": "mlWorkspaceName",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime?",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="properties"></a>Özellikler
| Özellik               | Açıklama                              | Gerekli                                 |
| ---------------------- | ---------------------------------------- | ---------------------------------------- |
| Tür                   | Tür özelliği şu şekilde ayarlanmalıdır: **AzureMLService**. | Evet                                      |
| subscriptionId         | Azure abonelik kimliği              | Evet                                      |
| resourceGroupName      | ad | Evet                                      |
| mlWorkspaceName        | Azure Machine Learning çalışma alanı adı | Evet  |
| hizmetPrincipalId     | Uygulamanın istemci kimliğini belirtin.     | Hayır |
| servicePrincipalKey    | Uygulamanın anahtarını belirtin.           | Hayır |
| Kiracı                 | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | UpdateResourceEndpoint belirtilirse gerekli | Hayır |
| connectVia             | Etkinlikleri bu bağlantılı hizmete göndermek için kullanılacak Tümleştirme Çalışma Zamanı. Azure Tümleştirme Çalışma Zamanı'nı veya Kendi kendine barındırılan Tümleştirme Çalışma Zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Hayır |    

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Veri Gölü Analizi bağlantılı hizmet
Bir Azure Veri Gölü Analizi bilgi işlem hizmetini bir Azure veri fabrikasına bağlamak için **Azure Veri Gölü Analytics** bağlantılı bir hizmet oluşturursunuz. Ardışık işlem hattındaki Data Lake Analytics U-SQL etkinliği bu bağlantılı hizmete başvurur. 

### <a name="example"></a>Örnek

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics URI",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID",
            "subscriptionId": "<optional, subscription ID of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="properties"></a>Özellikler

| Özellik             | Açıklama                              | Gerekli                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| type                 | Tür özelliği şu şekilde ayarlanmalıdır: **AzureDataLakeAnalytics**. | Evet                                      |
| accountName          | Azure Veri Gölü Analizi Hesap Adı.  | Evet                                      |
| dataLakeAnalyticsUri | Azure Veri Gölü Analytics URI.           | Hayır                                       |
| subscriptionId       | Azure abonelik kimliği                    | Hayır                                       |
| resourceGroupName    | Azure kaynak grubu adı                | Hayır                                       |
| hizmetPrincipalId   | Uygulamanın istemci kimliğini belirtin.     | Evet                                      |
| servicePrincipalKey  | Uygulamanın anahtarını belirtin.           | Evet                                      |
| Kiracı               | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | Evet                                      |
| connectVia           | Etkinlikleri bu bağlantılı hizmete göndermek için kullanılacak Tümleştirme Çalışma Zamanı. Azure Tümleştirme Çalışma Zamanı'nı veya Kendi kendine barındırılan Tümleştirme Çalışma Zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Hayır                                       |



## <a name="azure-databricks-linked-service"></a>Azure Databricks bağlantılı hizmet
Databricks iş yüklerini (not defteri, kavanoz, piton) çalıştırmak için kullandığınız Databricks çalışma alanını kaydetmek için **Azure Databricks bağlantılı hizmet** oluşturabilirsiniz. 
> [!IMPORTANT]
> Databricks bağlantılı hizmetler [Örnek havuzlarını](https://aka.ms/instance-pools)destekler. 

### <a name="example---using-new-job-cluster-in-databricks"></a>Örnek - Databricks'te yeni iş kümesini kullanma

```json
{
    "name": "AzureDatabricks_LS",
    "properties": {
        "type": "AzureDatabricks",
        "typeProperties": {
            "domain": "https://eastus.azuredatabricks.net",
            "newClusterNodeType": "Standard_D3_v2",
            "newClusterNumOfWorker": "1:10",
            "newClusterVersion": "4.0.x-scala2.11",
            "accessToken": {
                "type": "SecureString",
                "value": "dapif33c9c721144c3a790b35000b57f7124f"
            }
        }
    }
}

```

### <a name="example---using-existing-interactive-cluster-in-databricks"></a>Örnek - Databricks'te varolan Etkileşimli kümeyi kullanma

```json
{
    "name": " AzureDataBricksLinedService",
    "properties": {
      "type": " AzureDatabricks",
      "typeProperties": {
        "domain": "https://westeurope.azuredatabricks.net",
        "accessToken": {
            "type": "SecureString", 
            "value": "dapif33c9c72344c3a790b35000b57f7124f"
          },
        "existingClusterId": "{clusterId}"
        }
}

```

### <a name="properties"></a>Özellikler

| Özellik             | Açıklama                              | Gerekli                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| ad                 | Bağlantılı Hizmetin Adı               | Evet   |
| type                 | Tür özelliği şu şekilde ayarlanmalıdır: **Azure Databricks.** | Evet                                      |
| etki alanı               | Veri Tuğlaları çalışma alanının bölgesine göre Azure Bölgesini belirtin. Örnek: https://eastus.azuredatabricks.net | Evet                                 |
| accessToken          | Veri Fabrikası'nın Azure Veri Tuğlaları'na kimlik doğrulaması için erişim belirteci gereklidir. Erişim belirteci veri tuğlaları çalışma alanından oluşturulmalıdır. Erişim jetonunu bulmak için daha ayrıntılı adımlara [buradan](https://docs.azuredatabricks.net/api/latest/authentication.html#generate-token) ulaşabilirsiniz  | Evet                                       |
| mevcutClusterId    | Bu konudaki tüm işleri çalıştırmak için varolan bir kümenin küme kimliği. Bu zaten oluşturulmuş bir Etkileşimli Küme olmalıdır. Yanıt vermeyi durdurursa, kümeyi el ile yeniden başlatmanız gerekebilir. Veri tuğlaları, daha fazla güvenilirlik için yeni kümelerde iş çalıştırmayı önerir. Databricks çalışma alanında Etkileşimli Küme küme kimliğini (> Kümeleri -> İnteraktif Küme Adı -> Yapılandırması -> Etiketleri'nde bulabilirsiniz. [Daha fazla ayrıntı’yı seçin](https://docs.databricks.com/user-guide/clusters/tags.html) | Hayır 
| instancePoolId    | Veri tuğlaları çalışma alanında varolan bir havuzun Örnek Havuz Kimliği.  | Hayır  |
| newClusterVersion    | Kümenin Kıvılcım sürümü. Veri tuğlalarında bir iş kümesi oluşturur. | Hayır  |
| newClusterNumOfWorker| Bu kümenin sahip olması gereken alt düğüm sayısı. Bir küme, toplam num_workers + 1 Kıvılcım düğümü için bir Kıvılcım Sürücüsü ve num_workers Yürütücüsü vardır. "1" gibi bir dize biçimlendirilmiş Int32, numOfWorker 1 veya "1:10" anlamına gelir otoölçek anlamına gelir 1 dk ve 10 max olarak.  | Hayır                |
| newClusterNodeType   | Bu alan, tek bir değer aracılığıyla, bu kümedeki Kıvılcım düğümlerinin her biri için kullanılabilir kaynakları kodlar. Örneğin, Kıvılcım düğümleri bellek veya yoğun iş yüklerini hesaplamak için sağlanabilir ve optimize edilebilir. Bu alan yeni küme için gereklidir                | Hayır               |
| newClusterSparkConf  | isteğe bağlı, kullanıcı tarafından belirtilen Spark yapılandırma anahtar değeri çiftleri kümesi. Kullanıcılar ayrıca sürücü ve uygulayıcıları spark.driver.extraJavaOptions ve spark.executor.extraJavaOptions üzerinden ekstra JVM seçenekleri bir dizi geçirebilirsiniz. | Hayır  |
| newClusterInitScripts| yeni küme için isteğe bağlı, kullanıcı tanımlı bir başlatma komut dosyası kümesi. Init komut dosyaları için DBFS yolunu belirtme. | Hayır  |


## <a name="azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti

Bir Azure SQL bağlantılı hizmet oluşturur sunuz ve veri fabrikası ardışık kaynaktan depolanan yordamı çağırmak için [Saklı Yordam Etkinliği](transform-data-using-stored-procedure.md) ile birlikte kullanırsınız. Bu bağlantılı hizmet le ilgili ayrıntılar için [Azure SQL Bağlayıcısı](connector-azure-sql-database.md#linked-service-properties) makalesine bakın.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Veri Ambarı bağlantılı hizmet

Bir Azure SQL Veri Ambarı bağlantılı bir hizmet oluşturur sunuz ve veri fabrikası ardışık kaynaktan depolanan yordamı çağırmak için [Depolanan Yordam Etkinliği](transform-data-using-stored-procedure.md) ile birlikte kullanırsınız. Bu bağlantılı hizmetle ilgili ayrıntılar için [Azure SQL Veri Ambarı Bağlayıcısı](connector-azure-sql-data-warehouse.md#linked-service-properties) makalesine bakın.

## <a name="sql-server-linked-service"></a>SQL Server bağlantılı hizmet

Bir SQL Server bağlantılı hizmet oluşturur sunuz ve veri fabrikası ardışık hattından depolanan yordamı çağırmak için [Saklı Yordam Etkinliği](transform-data-using-stored-procedure.md) ile birlikte kullanırsınız. Bu bağlantılı hizmet le ilgili ayrıntılar için [SQL Server bağlayıcı](connector-sql-server.md#linked-service-properties) sıyrık makalesine bakın.

## <a name="azure-function-linked-service"></a>Azure İşlevi'ne bağlı hizmet

Bir Azure İşlevi bağlantılı hizmet oluşturur sunuz ve Bunu Veri Fabrikası ardışık bir ardışık alanda Azure İşlevlerini çalıştırmak için [Azure İşlevi etkinliğiyle](control-flow-azure-function-activity.md) birlikte kullanabilirsiniz. Azure işlevinin dönüş türü geçerli `JObject`olmalıdır. [(JArray](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JArray.htm) bir `JObject` *olmadığını* unutmayın.) Başarısız ve kullanıcı `JObject` hatası Yanıt İçeriği yükseltir dışında herhangi bir dönüş türü *geçerli bir JObject değildir.*

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| type   | Tür özelliği şu şekilde ayarlanmalıdır: **Azureİş** | evet |
| fonksiyon uygulaması url | Azure İşlevi Uygulaması'nın URL'si. Biçimi `https://<accountname>.azurewebsites.net`. Bu URL, Azure portalında İşlev Uygulamanızı görüntülerken **URL** bölümünün altındaki değerdir  | evet |
| fonksiyon tuşu | Azure İşlevi için erişim anahtarı. İlgili işlev için **Yönet** bölümüne tıklayın ve **İşlev Anahtarı'nı** veya Ana Bilgisayar **anahtarını**kopyalayın. Burada daha fazla bilgi edinin: [Azure İşlevler HTTP tetikleyicileri ve bağlamaları](../azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) | evet |
|   |   |   |

## <a name="next-steps"></a>Sonraki adımlar

Azure Veri Fabrikası tarafından desteklenen dönüşüm etkinliklerinin listesi için [bkz.](transform-data.md)
