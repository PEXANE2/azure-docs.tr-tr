---
title: Hadoop Akış Etkinliği'ni kullanarak verileri dönüştürme - Azure
description: İsteğe bağlı/kendi HDInsight kümenizde Hadoop Streaming programlarını çalıştırarak verileri dönüştürmek için Bir Azure veri fabrikasındaki Hadoop Akış Etkinliğini nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.assetid: 4c3ff8f2-2c00-434e-a416-06dfca2c41ec
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: a7f07365da699a40f5b51917104a68a62affa3d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74703378"
---
# <a name="transform-data-using-hadoop-streaming-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Hadoop Akış Etkinliği'ni kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Dönüşüm Faaliyetleri"]
> * [Kovan Etkinliği](data-factory-hive-activity.md) 
> * [Domuz Aktivitesi](data-factory-pig-activity.md)
> * [MapReduce Etkinliği](data-factory-map-reduce.md)
> * [Hadoop Akış Etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Kıvılcım Etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET Özel Etkinlik](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki Hadoop akış etkinliğini kullanarak verileri dönüştürme bölümüne](../transform-data-using-hadoop-streaming.md)bakın.


HDInsightStreamingActivity Etkinliğini, Bir Azure Veri Fabrikası ardışık hattından Hadoop Akış işini başlatabilirsiniz. Aşağıdaki JSON snippet bir ardışık JSON dosyasında HDInsightStreamingActivity kullanmak için sözdizimini gösterir. 

Veri Fabrikası [boru hattındaki](data-factory-create-pipelines.md) HDInsight Akış Etkinliği, Hadoop Streaming programlarını kendi veya [isteğe bağlı](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı HDInsight [kümenizde](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) yürütür. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi temel almaktadır.

> [!NOTE] 
> Azure Veri Fabrikası'nda yeniyseniz, [Azure Veri Fabrikası'na Giriş'i](data-factory-introduction.md) okuyun ve öğreticiyi yapın: Bu makaleyi okumadan önce ilk veri [ardışık işleminizi oluşturun.](data-factory-build-your-first-pipeline.md) 

## <a name="json-sample"></a>JSON örneği
HDInsight kümesi otomatik olarak örnek programlar (wc.exe ve cat.exe) ve verilerle (davinci.txt) doldurulur. Varsayılan olarak, HDInsight kümesi tarafından kullanılan kapsayıcının adı kümenin adıdır. Örneğin, küme adınız myhdicluster ise, ilişkili blob kapsayıcısının adı myhdicluster olacaktır. 

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<nameofthecluster>/example/apps/wc.exe",
                        "<nameofthecluster>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "AzureStorageLinkedService",
                    "getDebugInfo": "Failure"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-04T00:00:00Z",
        "end": "2014-01-05T00:00:00Z"
    }
}
```

Aşağıdaki noktalara dikkat edin:

1. **linkedServiceName'i,** akış lı mapreduce işinin çalıştırıldığı HDInsight kümenize işaret eden bağlantılı hizmetin adına ayarlayın.
2. Etkinliğin türünü **HDInsightStreaming**olarak ayarlayın.
3. **Mapper** özelliği için, yürütülebilir mapper adını belirtin. Örnekte, cat.exe mapper çalıştırılabilir.
4. **Azaltıcı** özelliği için, çalıştırılabilir azaltıcının adını belirtin. Örnekte, wc.exe uygulayabilir indirgeyicidir.
5. **Giriş** türü özelliği için, mapper için giriş dosyasını (konum dahil) belirtin. Örnekte: `wasb://adfsample@<account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt`: adfsample blob kapsayıcı, örnek / veri / Gutenberg klasör, ve davinci.txt blob olduğunu.
6. **Çıktı** türü özelliği için, azaltıcı için çıktı dosyasını (konum dahil) belirtin. Hadoop Streaming işinin çıktısı bu özellik için belirtilen konuma yazılır.
7. **filePaths** bölümünde, mapper ve azaltıcı yürütülebilir yolları belirtin. Örnekte: "adfsample/example/apps/wc.exe", adfsample blob kapsayıcı, örnek /uygulamalar klasör ve wc.exe yürütülebilir.
8. **FileLinkedService** özelliği için, dosya Yolları bölümünde belirtilen dosyaları içeren Azure depolama sını temsil eden Azure Depolama bağlantılı hizmeti belirtin.
9. Bağımsız **değişkenler** özelliği için akış işi için bağımsız değişkenleri belirtin.
10. **getDebugInfo** özelliği isteğe bağlı bir öğedir. Hata olarak ayarlandığında, günlükler yalnızca hata üzerine karşıdan yüklenir. Her zaman olarak ayarlandığında, günlükler yürütme durumuna bakılmaksızın her zaman karşıdan yüklenir.

> [!NOTE]
> Örnekte gösterildiği gibi, çıktılar özelliği için Hadoop Akış Etkinliği için bir çıktı veri kümesi **belirtirsiniz.** Bu veri kümesi, ardışık işlem zamanlamasını sürmek için gereken sahte bir veri kümesidir. **Giriş** özelliği için etkinlik için herhangi bir giriş veri kümesi belirtmeniz gerekmez.  
> 
> 

## <a name="example"></a>Örnek
Bu izlenecek yoldaki ardışık işlem, Azure HDInsight kümenizde Word Count akış Haritası/Azalt programını çalıştırAr. 

### <a name="linked-services"></a>Bağlı hizmetler
#### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti
İlk olarak, Azure HDInsight kümesi tarafından kullanılan Azure Depolama'yı Azure veri fabrikasına bağlamak için bağlantılı bir hizmet oluşturursunuz. Aşağıdaki kodu kopyalar/yapıştırın, hesap adı ve hesap anahtarını Azure Depolama nızın adı ve anahtarıyla değiştirmeyi unutmayın. 

```JSON
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

#### <a name="azure-hdinsight-linked-service"></a>Azure HDInsight bağlantılı hizmet
Ardından, Azure HDInsight kümenizi Azure veri fabrikasına bağlamak için bağlantılı bir hizmet oluşturursunuz. Aşağıdaki kodu kopyalar/yapıştırArsanız, HDInsight küme adını HDInsight kümenizin adıyla değiştirin ve kullanıcı adı ve parola değerlerini değiştirin. 

```JSON
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
            "userName": "admin",
            "password": "**********",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

### <a name="datasets"></a>Veri kümeleri
#### <a name="output-dataset"></a>Çıktı veri kümesi
Bu örnekte boru hattı herhangi bir giriş almaz. HDInsight Akış Etkinliği için bir çıktı veri kümesi belirtirsiniz. Bu veri kümesi, ardışık işlem zamanlamasını sürmek için gereken sahte bir veri kümesidir. 

```JSON
{
    "name": "StreamingOutputDataset",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "adftutorial/streamingdata/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>İşlem hattı
Bu örnekteki ardışık işlemde türünde tek bir etkinlik vardır: **HDInsightStreaming**. 

HDInsight kümesi otomatik olarak örnek programlar (wc.exe ve cat.exe) ve verilerle (davinci.txt) doldurulur. Varsayılan olarak, HDInsight kümesi tarafından kullanılan kapsayıcının adı kümenin adıdır. Örneğin, küme adınız myhdicluster ise, ilişkili blob kapsayıcısının adı myhdicluster olacaktır.  

```JSON
{
    "name": "HadoopStreamingPipeline",
    "properties": {
        "description": "Hadoop Streaming Demo",
        "activities": [
            {
                "type": "HDInsightStreaming",
                "typeProperties": {
                    "mapper": "cat.exe",
                    "reducer": "wc.exe",
                    "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                    "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                    "filePaths": [
                        "<blobcontainer>/example/apps/wc.exe",
                        "<blobcontainer>/example/apps/cat.exe"
                    ],
                    "fileLinkedService": "StorageLinkedService"
                },
                "outputs": [
                    {
                        "name": "StreamingOutputDataset"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "RunHadoopStreamingJob",
                "description": "Run a Hadoop streaming job",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-01-03T00:00:00Z",
        "end": "2017-01-04T00:00:00Z"
    }
}
```
## <a name="see-also"></a>Ayrıca Bkz.
* [Kovan Etkinliği](data-factory-hive-activity.md)
* [Domuz Aktivitesi](data-factory-pig-activity.md)
* [MapReduce Etkinliği](data-factory-map-reduce.md)
* [Spark programları çağırma](data-factory-spark.md)
* [R betikleri çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample)

