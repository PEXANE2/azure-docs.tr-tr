---
title: Azure Veri Fabrikası tarafından desteklenen bilgi işlem ortamları
description: Verileri dönüştürmek veya işlemek için Azure Veri Fabrikası ardışık hatlarında (Azure HDInsight gibi) kullanabileceğiniz bilgi işlem ortamları hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 0cc7c3b7d8b364e0bcca671efaff2cf324695428
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281554"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Azure Veri Fabrikası tarafından desteklenen bilgi işlem ortamları
> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, ['deki İşlem bağlantılı hizmetlere](../compute-linked-services.md)bakın.

Bu makalede, verileri işlemek veya dönüştürmek için kullanabileceğiniz bilgi işlem ortamları açıklanmaktadır. Ayrıca, bu bilgi işlem ortamlarını bir Azure veri fabrikasına bağlayan bağlantılı hizmetleri yapılandırırken Veri Fabrikası'nın desteklediği farklı yapılandırmalar (isteğe bağlı ve kendi yapılandırmaları) hakkında ayrıntılar da sağlar.

Aşağıdaki tablo, Veri Fabrikası tarafından desteklenen bilgi işlem ortamlarının ve bunların üzerinde çalıştırılabilecek etkinliklerin bir listesini sağlar. 

| İşlem ortamı                      | Etkinlikler                               |
| ---------------------------------------- | ---------------------------------------- |
| [İsteğe bağlı Azure HDInsight kümesi](#azure-hdinsight-on-demand-linked-service) veya [kendi HDInsight kümeniz](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Domuz](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) | [Dotnet](data-factory-use-custom-activities.md) |
| [Azure Makine Öğrenimi](#azure-machine-learning-linked-service) | [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL Veri Ambarı](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md) |

## <a name="hdinsight-versions-supported-in-data-factory"></a><a name="supported-hdinsight-versions-in-azure-data-factory"></a>Veri Fabrikası'nda desteklenen HDInsight sürümleri
Azure HDInsight, istediğiniz zaman dağıtabileceğiniz birden çok Hadoop küme sürümlerini destekler. Desteklenen her sürüm Hortonworks Veri Platformu (HDP) dağıtımının belirli bir sürümünü ve dağıtımdaki bileşenleri oluşturur. 

Microsoft, desteklenen HDInsight sürümlerinin listesini en son Hadoop ekosistem bileşenleri ve düzeltmeleriyle güncelleştirir. Ayrıntılı bilgi için desteklenen [HDInsight sürümlerine](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions)bakın.

> [!IMPORTANT]
> Linux tabanlı HDInsight sürüm 3.3 31 Temmuz 2017'de kullanımdan kaldırıldı. Data Factory sürüm 1 isteğe bağlı HDInsight bağlantılı hizmetler müşterilerine, HDInsight'ın daha sonraki bir sürümünü test etmek ve yükseltmek için 15 Aralık 2017'ye kadar süre verildi. Windows tabanlı HDInsight 31 Temmuz 2018'de kullanımdan kaldırılacaktır.
>
> 

### <a name="after-the-retirement-date"></a>Emeklilik tarihinden sonra 

15 Aralık 2017 tarihinden sonra:

- Data Factory sürüm 1'de isteğe bağlı HDInsight bağlantılı bir hizmeti kullanarak artık Linux tabanlı HDInsight sürüm 3.3 (veya önceki sürümler) kümelerini oluşturamaz. 
- [ **OsType** ve **Sürüm** özellikleri,](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) varolan bir Veri Fabrikası sürüm 1 isteğe bağlı HDInsight bağlantılı hizmet için JSON tanımında açıkça belirtilmemişse, varsayılan değer **Sürüm=3.1, osType=Windows** to **\<Version= en son\>HDI varsayılan sürümü (https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning), osType=Linux**olarak değiştirilir.

31 Temmuz 2018'den sonra:

- Data Factory sürüm 1'de isteğe bağlı HDInsight bağlantılı bir hizmet kullanarak artık Windows tabanlı HDInsight kümelerinin herhangi bir sürümünü oluşturamazsınız. 

### <a name="recommended-actions"></a>Önerilen eylemler 

- En son Hadoop ekosistem bileşenlerini ve düzeltmelerini kullanabileceğinizden emin olmak için, etkilenen Data Factory sürüm 1'in [ **işletim sistemi** ve **sürüm** özelliklerini](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) isteğe bağlı HDInsight bağlantılı hizmet tanımlarını daha yeni Linux tabanlı HDInsight sürümlerine (HDInsight 3.6) güncelleyin. 
- 15 Aralık 2017'den önce, etkilenen bağlantılı hizmete başvuran Data Factory sürüm 1 Hive, Pig, MapReduce ve Hadoop akış etkinliklerini test edin. Yeni **osType** ve **Sürüm** varsayılan değerleri **(Sürüm=3.6**, **osType=Linux)** veya yükselttiğiniz açık HDInsight sürümü ve işletim sistemi türüyle uyumlu olduğundan emin olun. 
  Uyumluluk hakkında daha fazla bilgi edinmek için windows [tabanlı HDInsight kümesinden Linux tabanlı kümeye Geçir'e](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) bakın ve [HDInsight ile kullanılabilir Hadoop bileşenleri ve sürümleri nelerdir?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions) 
- Windows tabanlı HDInsight kümeleri oluşturmak için isteğe bağlı HDInsight bağlantılı bir Veri Fabrikası sürüm 1'i kullanmaya devam etmek için, 15 Aralık 2017'den önce **osType'ı** **Windows'a** açıkça ayarlayın. 31 Temmuz 2018'den önce Linux tabanlı HDInsight kümelerine geçiş yapmanızı öneririz. 
- Veri Fabrikası sürüm 1 DotNet Özel Etkinliği yürütmek için isteğe bağlı HDInsight bağlantılı bir hizmet kullanıyorsanız, Bunun yerine bir Azure Toplu Iş bağlantılı hizmeti kullanmak için DotNet Özel Etkinlik JSON tanımını güncelleştirin. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities) 

> [!Note]
> Varolan, kendi kümenizi getir HDInsight bağlantılı cihazınızı Veri Fabrikası sürüm 1'de veya Azure Veri Fabrikası'nda kendi ve isteğe bağlı HDInsight bağlantılı hizmetini kullanıyorsanız, herhangi bir işlem gerekmez. Bu senaryolarda, HDInsight kümelerinin en son sürüm destek ilkesi zaten uygulanır. 
>
> 


## <a name="on-demand-compute-environment"></a>İsteğe bağlı işlem ortamı
İsteğe bağlı yapılandırmada, Veri Fabrikası bilgi işlem ortamını tam olarak yönetir. Veri Fabrikası, bir iş veri işleme için gönderilmeden önce otomatik olarak bilgi işlem ortamı oluşturur. İş bittiğinde, Veri Fabrikası bilgi işlem ortamını kaldırır. 

İsteğe bağlı bir işlem ortamı için bağlantılı bir hizmet oluşturabilirsiniz. Bilgi işlem ortamını yapılandırmak ve iş yürütme, küme yönetimi ve önyükleme eylemleri için parçalı ayarları denetlemek için bağlantılı hizmeti kullanın.

> [!NOTE]
> Şu anda, isteğe bağlı yapılandırma yalnızca HDInsight kümeleri için desteklenir.
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>İsteğe bağlı Azure HDInsight bağlı hizmeti
Veri Fabrikası, verileri işlemek için otomatik olarak Windows veya Linux tabanlı isteğe bağlı HDInsight kümesi oluşturabilir. Küme, kümeyle ilişkili depolama hesabıyla aynı bölgede oluşturulur. Kümeoluşturmak için JSON **linkedServiceName** özelliğini kullanın.

İsteğe bağlı HDInsight bağlantılı hizmet le ilgili aşağıdaki *önemli* noktalara dikkat edin:

* İsteğe bağlı HDInsight kümesi Azure aboneliğinizde görünmez. Veri Fabrikası hizmeti, sizin adınıza isteğe bağlı HDInsight kümesini yönetir.
* İsteğe bağlı HDInsight kümesinde çalıştırılan işlerin günlükleri, HDInsight kümesiyle ilişkili depolama hesabına kopyalanır. Bu günlüklere erişmek için Azure portalında **Etkinlik Çalıştır Ayrıntıları** bölmesine gidin. Daha fazla bilgi [için, bkz.](data-factory-monitor-manage-pipelines.md)
* Yalnızca HDInsight kümesinin çalışır durumda olduğu ve çalıştırdığı süre için ücretlendirilirsiniz.

> [!IMPORTANT]
> İsteğe bağlı bir HDInsight kümesini sağlamak genellikle *20 dakika* veya daha uzun sürer.
>
> 

### <a name="example"></a>Örnek
Aşağıdaki JSON, Linux tabanlı isteğe bağlı HDInsight bağlantılı bir hizmeti tanımlar. Veri Fabrikası, bir veri dilimini işlediğinde otomatik olarak *Linux tabanlı* bir HDInsight kümesi oluşturur. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.6",
            "osType": "Linux",
            "clusterSize": 1,
            "timeToLive": "00:05:00",            
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

> [!IMPORTANT]
> HDInsight kümesi, Azure Blob depolama alanında JSON **linkedServiceName** özelliğinde belirttiğiniz varsayılan bir *kapsayıcı* oluşturur. Tasarım gereği HDInsight, küme silindiğinde bu kapsayıcıyı silmez. İsteğe bağlı HDInsight bağlantılı bir hizmette, varolan bir canlı küme **(timeToLive)** olmadığı sürece, bir dilimin işlenmesi gerektiğinde bir HDInsight kümesi oluşturulur. İşlem tamamlandığında küme silinir. 
>
> Daha fazla dilim işlendikçe, Blob depolama alanınızda birçok kapsayıcı görürsünüz. Sorun giderme işleri için kapsayıcılara ihtiyacınız yoksa, depolama maliyetini azaltmak için kapsayıcıları silmek isteyebilirsiniz. Bu kapsayıcı adları bir düzene sahiptir: `adf<your Data Factory name>-<linked service name>-<date and time>`. Blob depolama sındaki kapsayıcıları silmek için [Microsoft Storage Explorer](https://storageexplorer.com/) gibi bir araç kullanabilirsiniz.
>
> 

### <a name="properties"></a>Özellikler
| Özellik                     | Açıklama                              | Gerekli |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Tür özelliğini **HDInsightOnDemand**olarak ayarlayın. | Evet      |
| clusterSize                  | Kümedeki alt ve veri düğümlerinin sayısı. HDInsight kümesi, bu özellik için belirttiğiniz alt düğüm sayısına ek olarak 2 kafa düğümüyle oluşturulur. Düğümler boyutu Standard_D3, 4 çekirdek vardır. 4-worker düğüm kümesi 24 çekirdek alır\*(4 4 = 16 işçi düğümleri için çekirdek, artı 2\*4 = kafa düğümleri için çekirdek). Standard_D3 katmanı hakkında ayrıntılı bilgi için [HDInsight'ta Linux tabanlı Hadoop kümeleri oluşturun'a](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)bakın. | Evet      |
| timeToLive                   | İsteğe bağlı HDInsight kümesi için izin verilen boşta kalma süresi. İsteğe bağlı HDInsight kümesinin, kümede başka etkin iş yoksa, bir etkinlik çalışması tamamlandığında ne kadar süre yle hayatta kalacağını belirtir.<br /><br />Örneğin, bir etkinlik çalışması 6 dakika sürerse ve **ToLive 5** dakika olarak ayarlanırsa, küme etkinliği işledikten 6 dakika sonra 5 dakika boyunca canlı kalır. Başka bir etkinlik çalışması 6 dakikalık pencerede yürütülürse, aynı küme tarafından işlenir.<br /><br />İsteğe bağlı HDInsight kümesi oluşturmak pahalı bir işlemdir (biraz zaman alabilir). İsteğe bağlı HDInsight kümesini yeniden kullanarak bir veri fabrikasının performansını artırmak için bu ayarı gerektiği gibi kullanın.<br /><br />**ZamanToLive** değerini **0**olarak ayarlarsanız, etkinlik çalışması biter bitmez küme silinir. Ancak, yüksek bir değer ayarlarsanız, küme boşta kalabilir ve gereksiz yere yüksek maliyetlere neden olabilir. İhtiyaçlarınıza göre uygun değeri ayarlamak önemlidir.<br /><br />**TimeToLive** değeri uygun şekilde ayarlanırsa, birden çok ardışık alan, isteğe bağlı HDInsight kümesinin örneğini paylaşabilir. | Evet      |
| version                      | HDInsight kümesinin sürümü. İzin verilen HDInsight sürümleri için [Desteklenen HDInsight sürümlerine](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions)bakın. Bu değer belirtilmemişse, [en son HDI varsayılan sürümü](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning) kullanılır. | Hayır       |
| linkedServiceName            | Verileri depolamak ve işlemek için isteğe bağlı küme tarafından kullanılacak Azure Depolama bağlantılı hizmet. HDInsight kümesi, bu depolama hesabıyla aynı bölgede oluşturulur.<p>Şu anda, depolama alanı olarak Azure Veri Gölü Deposu'nu kullanan isteğe bağlı bir HDInsight kümesi oluşturamazsınız. HDInsight işlemedeki sonuç verilerini Data Lake Store'da depolamak istiyorsanız, Blob depolamadan Data Lake Store'a verileri kopyalamak için Kopyalama Etkinliği'ni kullanın. </p> | Evet      |
| ekLinkedServiceNames | HDInsight bağlantılı hizmet için ek depolama hesapları belirtir. Veri Fabrikası depolama hesaplarını sizin adınıza kaydeder. Bu depolama hesapları HDInsight kümesiyle aynı bölgede olmalıdır. HDInsight kümesi, **linkedServiceName** özelliği tarafından belirtilen depolama hesabıyla aynı bölgede oluşturulur. | Hayır       |
| osType                       | İşletim sisteminin türü. İzin verilen değerler **Linux** ve **Windows'a**aittir. Bu değer belirtilmemişse, **Linux** kullanılır.  <br /><br />Linux tabanlı HDInsight kümelerini kullanmanızı şiddetle öneririz. HDInsight on Windows için emeklilik tarihi 31 Temmuz 2018'dir. | Hayır       |
| hcatalogLinkedServiceName    | HCatalog veritabanını işaret eden Azure SQL bağlantılı hizmetin adı. İsteğe bağlı HDInsight kümesi, SQL veritabanını metastore olarak kullanarak oluşturulur. | Hayır       |

#### <a name="example-linkedservicenames-json"></a>Örnek: LinkedServiceNames JSON

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Gelişmiş özellikler
İsteğe bağlı HDInsight kümesinin parçalı yapılandırması için aşağıdaki özellikleri belirtebilirsiniz:

| Özellik               | Açıklama                              | Gerekli |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | OLUŞTURULACAK HDInsight kümesi için temel yapılandırma parametrelerini (core-site.xml) belirtir. | Hayır       |
| hBaseConfiguration     | HDInsight kümesi için HBase yapılandırma parametrelerini (hbase-site.xml) belirtir. | Hayır       |
| hdfsYapılandırma      | HDInsight kümesi için HDFS yapılandırma parametrelerini (hdfs-site.xml) belirtir. | Hayır       |
| kovanYapılandırma      | HDInsight kümesi için Hive yapılandırma parametrelerini (kovan-site.xml) belirtir. | Hayır       |
| mapReduceConfiguration | HDInsight kümesi için MapReduce yapılandırma parametrelerini (mapred-site.xml) belirtir. | Hayır       |
| oozieConfiguration     | HDInsight kümesi için Oozie yapılandırma parametrelerini (oozie-site.xml) belirtir. | Hayır       |
| stormConfiguration     | HDInsight kümesi için Fırtına yapılandırma parametrelerini (storm-site.xml) belirtir. | Hayır       |
| iplikYapılandırma      | HDInsight kümesi için İplik konfigürasyon parametrelerini (iplik-site.xml) belirtir. | Hayır       |

#### <a name="example-on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Örnek: Gelişmiş özelliklere sahip isteğe bağlı HDInsight küme yapılandırması

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "version": "3.6",
      "osType": "Linux",
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
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
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>Düğüm boyutları
Kafa, veri ve ZooKeeper düğümlerinin boyutunu belirtmek için aşağıdaki özellikleri kullanın: 

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Baş düğümünün boyutunu ayarlar. Varsayılan değer **Standard_D3.** Ayrıntılar için düğüm [boyutlarını belirt'e](#specify-node-sizes)bakın. | Hayır       |
| dataNodeSize      | Veri düğümünün boyutunu ayarlar. Varsayılan değer **Standard_D3.** | Hayır       |
| zookeeperNodeSize | ZooKeeper düğümünün boyutunu ayarlar. Varsayılan değer **Standard_D3.** | Hayır       |

#### <a name="specify-node-sizes"></a>Düğüm boyutlarını belirtin
Önceki bölümde açıklanan özellikler için belirtmeniz gereken dize değerleri için [Sanal makine boyutlarına](../../virtual-machines/linux/sizes.md)bakın. Değerler, [Sanal makine boyutlarında](../../virtual-machines/linux/sizes.md)başvurulan cmdlets ve API'lere uygun olmalıdır. Büyük (varsayılan) veri düğümü boyutu 7 GB belleğe sahiptir. Bu, senaryonuz için yeterli olmayabilir. 

D4 boyutunda kafa düğümleri ve alt düğümler oluşturmak istiyorsanız, **Standard_D4** **headNodeSize** ve **dataNodeSize** özellikleri için değer olarak belirtin: 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Bu özellikler için yanlış bir değer ayarlarsanız, aşağıdaki iletiyi görebilirsiniz:

  Küme oluşturmak için başarısız oldu. Özel durum: Küme oluşturma işlemi tamamlanamadı. İşlem '400' koduyla başarısız oldu. Küme geride bırakma durumu: 'Hata'. Mesaj: 'PreClusterCreationValidationFailure'. 
  
Bu iletiyi görürseniz, [sanal makine boyutlarında](../../virtual-machines/linux/sizes.md)tablodaki cmdlet ve API adlarını kullandığınızdan emin olun.  

> [!NOTE]
> Şu anda, Veri Fabrikası birincil depo olarak Data Lake Store kullanan HDInsight kümelerini desteklemez. HDInsight kümeleri için birincil depo olarak Azure Depolama'yı kullanın. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Kendi işlem ortamını getirin
Varolan bir bilgi işlem ortamını Veri Fabrikası'nda bağlantılı bir hizmet olarak kaydedebilirsiniz. İşlem ortamını siz yönetirsiniz. Veri Fabrikası hizmeti, etkinlikleri yürütmek için bilgi işlem ortamını kullanır.

Bu tür yapılandırmalar aşağıdaki işlem ortamları için desteklenir:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL Veritabanı, Azure SQL Veri Ambarı, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight bağlantılı hizmet
Kendi HDInsight kümenizi Veri Fabrikası'na kaydetmek için HDInsight bağlantılı bir hizmet oluşturabilirsiniz.

### <a name="example"></a>Örnek

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| type              | Tür özelliğini **HDInsight**olarak ayarlayın. | Evet      |
| clusterUri        | HDInsight kümesinin URI'si.        | Evet      |
| kullanıcı adı          | Varolan bir HDInsight kümesine bağlanmak için kullanılacak kullanıcı hesabının adı. | Evet      |
| password          | Kullanıcı hesabının şifresi.   | Evet      |
| linkedServiceName | HDInsight kümesi tarafından kullanılan Blob depolamasını ifade eden depolama bağlantılı hizmetin adı. <p>Şu anda, bu özellik için Bir Veri Gölü Deposu bağlantılı hizmet belirtemezsiniz. HDInsight kümesinin Data Lake Store'a erişimi varsa, Veri Gölü Deposu'ndaki verilere Kovan veya Pig komut dosyalarından erişebilirsiniz. </p> | Evet      |

## <a name="azure-batch-linked-service"></a>Azure Toplu İşlem bağlantılı hizmet
Bir veri fabrikasına sanal makinelerden (VM) toplu iş havuzukaydetmek için Toplu İşlem bağlantılı bir hizmet oluşturabilirsiniz. Microsoft .NET özel etkinliklerini Toplu Veya HDInsight'ı kullanarak çalıştırabilirsiniz.

Toplu İşlem hizmetini kullanmaya yeni başladıysanız:

* Azure [Toplu İş temelleri](../../batch/batch-technical-overview.md)hakkında bilgi edinin.
* [Yeni AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) cmdlet hakkında bilgi edinin. Toplu Iş hesabı oluşturmak için bu cmdlet'i kullanın. Veya [Azure portalını](../../batch/batch-account-create-portal.md)kullanarak Toplu İş hesabı oluşturabilirsiniz. Cmdlet kullanma hakkında ayrıntılı bilgi için Toplu [hesabı yönetmek için PowerShell'i kullanma'ya](https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx)bakın.
* [New-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) cmdlet hakkında bilgi edinin. Toplu iş havuzu oluşturmak için bu cmdlet'i kullanın.

### <a name="example"></a>Örnek

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

**AccountName** özelliği için, **ek.\< \> ** toplu iş hesabınızın adına bölge adı. Örnek:

```json
"accountName": "mybatchaccount.eastus"
```

Başka bir seçenek **batchUri** bitiş noktası sağlamaktır. Örnek:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| type              | Tür özelliğini **AzureBatch**olarak ayarlayın. | Evet      |
| accountName       | Toplu İşlem hesabının adı.         | Evet      |
| Accesskey         | Toplu İşlem hesabının erişim anahtarı.  | Evet      |
| poolName          | VM havuzunun adı.    | Evet      |
| linkedServiceName | Bu Toplu İşlem bağlantılı hizmetle ilişkili depolama bağlantılı hizmetin adı. Bu bağlı hizmet, etkinliği çalıştırmak için gereken dosyaları evreleme ve etkinlik yürütme günlüklerini depolamak için kullanılır. | Evet      |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlantılı hizmet
Bir Makine Öğrenimi toplu iş sonu puanlama uç noktasını bir veri fabrikasına kaydetmek için Machine Learning bağlantılı bir hizmet oluşturabilirsiniz.

### <a name="example"></a>Örnek

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>Özellikler
| Özellik   | Açıklama                              | Gerekli |
| ---------- | ---------------------------------------- | -------- |
| Tür       | Tür özelliğini **AzureML**olarak ayarlayın. | Evet      |
| mlEndpoint | Toplu puanlama URL'si.                   | Evet      |
| apiKey     | Yayınlanan çalışma alanı modelinin API'si.     | Evet      |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Veri Gölü Analizi bağlantılı hizmet
Bir Data Lake Analytics bilgi işlem hizmetini bir Azure veri fabrikasına bağlamak için Bir Data Lake Analytics bağlantılı hizmet oluşturabilirsiniz. Ardışık işlem hattındaki Data Lake Analytics U-SQL etkinliği bu bağlantılı hizmete başvurur. 

Aşağıdaki tabloda JSON tanımında kullanılan genel özellikler açıklanmaktadır:

| Özellik                 | Açıklama                              | Gerekli                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| type                 | Tür özelliğini **AzureDataLakeAnalytics**olarak ayarlayın. | Evet                                      |
| accountName          | Data Lake Analytics hesap adı.  | Evet                                      |
| dataLakeAnalyticsUri | Veri Gölü Analytics URI.           | Hayır                                       |
| subscriptionId       | Azure abonelik kimliği.                    | Hayır<br /><br />(Belirtilmemişse, veri fabrikası aboneliği kullanılır.) |
| resourceGroupName    | Azure kaynak grubu adı.                | Hayır<br /><br /> (Belirtilmemişse, veri fabrikası kaynak grubu kullanılır.) |

### <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri
Data Lake Analytics bağlantılı hizmetiniz için, bir hizmet sorumlusu veya kullanıcı kimlik bilgilerini kullanarak kimlik doğrulama arasında seçim yapabilirsiniz.

#### <a name="service-principal-authentication-recommended"></a>Hizmet temel kimlik doğrulaması (önerilir)
Hizmet temel kimlik doğrulamasını kullanmak için Azure Etkin Dizini'ne (Azure AD) bir uygulama varlığı kaydedin. Ardından, Azure AD'nin Veri Gölü Deposu'na erişimini tanıyın. Ayrıntılı adımlar için [hizmete kimlik doğrulamasına](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md)bakın. Bağlantılı hizmeti tanımlamak için kullandığınız aşağıdaki değerlere dikkat edin:
* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Aşağıdaki özellikleri belirterek hizmet temel kimlik doğrulamasını kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| hizmetPrincipalId  | Uygulamanın istemci kimliği.     | Evet      |
| servicePrincipalKey | Uygulamanın anahtarı.           | Evet      |
| Kiracı              | Uygulamanızın bulunduğu kiracı bilgileri (alan adı veya kiracı kimliği). Bu bilgileri almak için, Azure portalının sağ üst köşesinde farenizi gezin. | Evet      |

**Örnek: Hizmet temel kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="user-credential-authentication"></a>Kullanıcı kimlik bilgisi kimlik doğrulaması
Data Lake Analytics için kullanıcı kimlik bilgisi kimlik doğrulaması için aşağıdaki özellikleri belirtin:

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| yetkilendirme | Veri Fabrikası Düzenleyicisi'nde **Yetkilendirme** düğmesini seçin. Bu özelliğe otomatik olarak oluşturulmuş yetkilendirme URL'sini atayan kimlik belgesini girin. | Evet      |
| Sessionıd     | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersizdir ve yalnızca bir kez kullanılabilir. Veri Fabrikası Düzenleyicisi'ni kullandığınızda bu ayar otomatik olarak oluşturulur. | Evet      |

**Örnek: Kullanıcı kimlik bilgisi kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Belirteç sona ermesi
**Yetkilendirme** düğmesini seçerek oluşturduğunuz yetkilendirme kodunun süresi, belirli bir aralıktan sonra sona erer. 

Kimlik doğrulama belirteci süresi dolduğunda aşağıdaki hata iletisini görebilirsiniz: 

  Kimlik doğrulama hatası: invalid_grant - AADSTS70002: Kimlik bilgilerini doğrulayan hata. AADSTS70008: Sağlanan erişim izninin süresi doldu veya iptal edildi. İz ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korelasyon Id: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 Timestamp: 2015-12-15 21:09:31Z

Aşağıdaki tabloda kullanıcı hesap türüne göre son kullanma tarihleri gösterilmektedir: 

| Kullanıcı türü                                | Sonra sona erer                            |
| :--------------------------------------- | :--------------------------------------- |
| Azure AD (Hotmail, Live vb.) tarafından *yönetilmeyen* kullanıcı hesapları | 12 saat.                                 |
| Azure AD tarafından *yönetilen* kullanıcı hesapları | Son dilim çalışmasından 14 gün sonra. <br /><br />OAuth tabanlı bağlantılı bir hizmeti temel alan bir dilim en az 14 günde bir çalışıyorsa, 90 gün. |

Bu hatayı önlemek veya gidermek için, belirteç sona erdiğinde **Yetkilendirme** düğmesini seçerek yeniden yetki verin. Ardından, bağlantılı hizmeti yeniden dağıtın. Ayrıca aşağıdaki kodu kullanarak **programlı sessionId** ve **yetkilendirme** özellikleri için değerler oluşturabilirsiniz:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Bu kod örneğinde kullanılan Veri Fabrikası sınıfları hakkında ayrıntılar için bkz:
* [AzureDataLakeStoreLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
* [AzureDataLakeAnalyticsLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* [YetkilendirmeSessionGetResponse sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx)

**WindowsFormsWebAuthenticationDialog** sınıfı için Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll adresine başvuru ekleyin. 

## <a name="azure-sql-linked-service"></a>Azure SQL bağlı hizmeti
Bir SQL bağlantılı hizmet oluşturabilir ve Veri Fabrikası ardışık kaynaktan depolanan yordamı çağırmak için [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md) ile kullanabilirsiniz. Daha fazla bilgi için Azure [SQL bağlayıcısı'na](data-factory-azure-sql-connector.md#linked-service-properties)bakın.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Veri Ambarı bağlantılı hizmet
Bir SQL Veri Ambarı bağlantılı hizmet oluşturabilir ve Veri Fabrikası ardışık kaynaktan depolanan yordamı çağırmak için [Depolanan Yordam Etkinliği](data-factory-stored-proc-activity.md) ile kullanabilirsiniz. Daha fazla bilgi için Bkz. [Azure SQL Veri Ambarı bağlayıcısı.](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties)

## <a name="sql-server-linked-service"></a>SQL Server bağlantılı hizmet
Bir SQL Server bağlantılı hizmet oluşturabilir ve Veri Fabrikası ardışık kaynaktan depolanan yordamı çağırmak için [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md) ile kullanabilirsiniz. Daha fazla bilgi için [SQL Server bağlayıcısı.'na](data-factory-sqlserver-connector.md#linked-service-properties)bakın.

