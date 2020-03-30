---
title: Azure Veri Fabrikası'ndan MapReduce Programını Başlat
description: Azure veri fabrikasından Bir Azure HDInsight kümesinde MapReduce programlarını çalıştırarak verileri nasıl işleyerek verileri nasıl işleniriz öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.assetid: c34db93f-570a-44f1-a7d6-00390f4dc0fa
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 598a16d25ba375b984a966cba190181edbda3d15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74703146"
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a>Veri Fabrikası'ndan MapReduce Programlarını Başlat
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
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki MapReduce etkinliğini kullanarak verileri dönüştürme bölümüne](../transform-data-using-hadoop-map-reduce.md)bakın.


Bir Veri Fabrikası [boru hattındaki](data-factory-create-pipelines.md) HDInsight MapReduce etkinliği, MapReduce programlarını kendi veya [isteğe bağlı](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı HDInsight [kümenizde](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) yürütür. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi temel almaktadır.

> [!NOTE] 
> Azure Veri Fabrikası'nda yeniyseniz, [Azure Veri Fabrikası'na Giriş'i](data-factory-introduction.md) okuyun ve öğreticiyi yapın: Bu makaleyi okumadan önce ilk veri [ardışık işleminizi oluşturun.](data-factory-build-your-first-pipeline.md)  

## <a name="introduction"></a>Giriş
Azure veri fabrikasındaki bir ardışık işlem, bağlantılı bilgi işlem hizmetlerini kullanarak bağlantılı depolama hizmetlerindeki verileri işler. Her etkinliğin belirli bir işleme işlemi gerçekleştirdiği bir dizi etkinlik içerir. Bu makalede, HDInsight MapReduce Etkinliği kullanılarak açıklanmaktadır.

HDInsight Pig ve [Hive](data-factory-hive-activity.md) etkinliklerini kullanarak bir boru hattından Windows/Linux tabanlı HDInsight kümesinde Pig/Hive komut dosyaları çalıştırma hakkında ayrıntılı bilgi için [Pig](data-factory-pig-activity.md) ve Hive'a bakın. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>HDInsight MapReduce Etkinliği için JSON
HDInsight Etkinliği için JSON tanımında: 

1. **Etkinliğin** **türünü** **HDInsight**olarak ayarlayın.
2. **className** özelliği için sınıfın adını belirtin.
3. **JarFilePath** özelliğiiçin dosya adını da içeren JAR dosyasına giden yolu belirtin.
4. **JarLinkedService** özelliği için JAR dosyasını içeren Azure Blob Depolamasına başvuran bağlantılı hizmeti belirtin.   
5. **Bağımsız değişkenler** bölümünde MapReduce programı için bağımsız değişkenler belirtin. Çalışma zamanında MapReduce çerçevesinden birkaç ek bağımsız değişken (örneğin: mapreduce.job.tags) görürsünüz. Bağımsız değişkenlerinizi MapReduce bağımsız değişkenleriyle ayırt etmek için, aşağıdaki örnekte (-s, --giriş, --çıktı vb.) hem seçenek hem de değeri bağımsız değişkenolarak kullanmayı düşünün, değerleri tarafından hemen izlenen seçeneklerdir.

    ```JSON   
    {
        "name": "MahoutMapReduceSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "-s",
                            "SIMILARITY_LOGLIKELIHOOD",
                            "--input",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                            "--output",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                            "--maxSimilaritiesPerItem",
                            "500",
                            "--tempDir",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                        ]
                    },
                    "inputs": [
                        {
                            "name": "MahoutInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "MahoutOutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MahoutActivity",
                    "description": "Custom Map Reduce to generate Mahout result",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-01-03T00:00:00Z",
            "end": "2017-01-04T00:00:00Z"
        }
    }
    ```
   Bir HDInsight kümesinde herhangi bir MapReduce jar dosyasını çalıştırmak için HDInsight MapReduce Etkinliğini kullanabilirsiniz. Aşağıdaki örnek JSON ardışık hatlar tanımında, HDInsight Etkinliği bir Mahout JAR dosyasını çalıştıracak şekilde yapılandırılır.

## <a name="sample-on-github"></a>GitHub'da örnek
HDInsight MapReduce Activity'i kullanmak için bir örneği [GitHub'daki Veri Fabrikası Örneklerinden](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/JSON/MapReduce_Activity_Sample)indirebilirsiniz.  

## <a name="running-the-word-count-program"></a>Sözcük Sayısı programını çalıştırma
Bu örnekteki ardışık işlem, Azure HDInsight kümenizde Word Count Map/Reduce programını çalıştırAr.   

### <a name="linked-services"></a>Bağlı Hizmetler
İlk olarak, Azure HDInsight kümesi tarafından kullanılan Azure Depolama'yı Azure veri fabrikasına bağlamak için bağlantılı bir hizmet oluşturursunuz. Aşağıdaki kodu kopyalar/yapıştırın, **hesap adı** ve **hesap anahtarını** Azure Depolama nızın adı ve anahtarıyla değiştirmeyi unutmayın. 

#### <a name="azure-storage-linked-service"></a>Azure Storage bağlı hizmeti

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
Ardından, Azure HDInsight kümenizi Azure veri fabrikasına bağlamak için bağlantılı bir hizmet oluşturursunuz. Aşağıdaki kodu kopyalar/yapıştırArsanız, **HDInsight küme adını** HDInsight kümenizin adıyla değiştirin ve kullanıcı adı ve parola değerlerini değiştirin.   

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
Bu örnekte boru hattı herhangi bir giriş almaz. HDInsight MapReduce Etkinliği için bir çıktı veri kümesi belirtirsiniz. Bu veri kümesi, ardışık işlem zamanlamasını sürmek için gereken sahte bir veri kümesidir.  

```JSON
{
    "name": "MROutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "WordCountOutput1.txt",
            "folderPath": "example/data/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>İşlem hattı
Bu örnekteki ardışık işlemde yalnızca bir etkinlik vardır: HDInsightMapReduce. JSON'daki önemli özelliklerden bazıları şunlardır: 

| Özellik | Notlar |
|:--- |:--- |
| type |Türü **HDInsightMapReduce**olarak ayarlanmalıdır. |
| Classname |Sınıfın adı: **wordcount** |
| jarFilePath |Sınıfı içeren kavanoz dosyasına giden yol. Aşağıdaki kodu kopyalar/yapıştırarsanız, kümenin adını değiştirmeyi unutmayın. |
| jarLinkedService |Kavanoz dosyasını içeren Azure Depolama bağlantılı hizmet. Bu bağlantılı hizmet, HDInsight kümesiyle ilişkili depolama alanı anlamına gelir. |
| Bağımsız değişken |Wordcount programı iki bağımsız değişken, bir giriş ve bir çıktı alır. Giriş dosyası davinci.txt dosyasıdır. |
| frequency/interval |Bu özelliklerin değerleri çıktı veri kümesiyle eşleşir. |
| linkedServiceName |daha önce oluşturduğunuz HDInsight bağlantılı hizmeti ifade eder. |

```JSON
{
    "name": "MRSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run the Word Count Program",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "wordcount",
                    "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": [
                        "/example/data/gutenberg/davinci.txt",
                        "/example/data/WordCountOutput1"
                    ]
                },
                "outputs": [
                    {
                        "name": "MROutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "MRActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-03T00:00:00Z",
        "end": "2014-01-04T00:00:00Z"
    }
}
```

## <a name="run-spark-programs"></a>Kıvılcım programlarını çalıştırın
MapReduce etkinliğini kullanarak HDInsight Spark kümenizde Spark programları çalıştırabilirsiniz. Ayrıntılar için bkz. [Azure Data Factory’den Spark programlarını çağırma](data-factory-spark.md).  

[developer-reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: https://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: https://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: https://portal.azure.com

## <a name="see-also"></a>Ayrıca Bkz.
* [Kovan Etkinliği](data-factory-hive-activity.md)
* [Domuz Aktivitesi](data-factory-pig-activity.md)
* [Hadoop Akış Etkinliği](data-factory-hadoop-streaming-activity.md)
* [Spark programları çağırma](data-factory-spark.md)
* [R betikleri çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample)

