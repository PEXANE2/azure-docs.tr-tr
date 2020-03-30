---
title: Azure Veri Fabrikası'nda Pig Activity kullanarak verileri dönüştürme
description: İsteğe bağlı/kendi HDInsight kümenizde Pig komut dosyalarını çalıştırmak için Bir Azure veri fabrikasındaki Pig Etkinliği'ni nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.assetid: 5af07a1a-2087-455e-a67b-a79841b4ada5
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 5b8e7201a6239ef1fe83fb89d4b361995e305bbf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74703202"
---
# <a name="transform-data-using-pig-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Pig Activity kullanarak verileri dönüştürme
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
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki Pig etkinliğini kullanarak verileri dönüştürme bölümüne](../transform-data-using-hadoop-pig.md)bakın.


Veri Fabrikası [ardışık](data-factory-create-pipelines.md) işakindeki HDInsight Pig etkinliği, Pig sorgularını kendi veya [isteğe bağlı](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı HDInsight [kümenizde](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) yürütür. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi temel almaktadır.

> [!NOTE] 
> Azure Veri Fabrikası'nda yeniyseniz, [Azure Veri Fabrikası'na Giriş'i](data-factory-introduction.md) okuyun ve öğreticiyi yapın: Bu makaleyi okumadan önce ilk veri [ardışık işleminizi oluşturun.](data-factory-build-your-first-pipeline.md) 

## <a name="syntax"></a>Sözdizimi

```JSON
{
  "name": "HiveActivitySamplePipeline",
  "properties": {
    "activities": [
      {
        "name": "Pig Activity",
        "description": "description",
        "type": "HDInsightPig",
        "inputs": [
          {
            "name": "input tables"
          }
        ],
        "outputs": [
          {
            "name": "output tables"
          }
        ],
        "linkedServiceName": "MyHDInsightLinkedService",
        "typeProperties": {
          "script": "Pig script",
          "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
        "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
      }
    ]
  }
}
```

## <a name="syntax-details"></a>Sözdizimi ayrıntıları

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Etkinliğin adı |Evet |
| açıklama |Etkinliğin ne için kullanıldığını açıklayan metin |Hayır |
| type |HDinsightPig |Evet |
| Giriş |Pig aktivitesi tarafından tüketilen bir veya daha fazla girdi |Hayır |
| Çıkış |Pig aktivitesi tarafından üretilen bir veya daha fazla çıktı |Evet |
| linkedServiceName |Veri Fabrikası'nda bağlantılı hizmet olarak kayıtlı HDInsight kümesine başvuru |Evet |
| betiğini çalıştırın |Domuz komut dosyasını satır satır belirtin |Hayır |
| scriptPath |Pig komut dosyasını Azure blob depolama alanında saklayın ve dosyaya giden yolu sağlayın. 'Komut dosyası' veya 'scriptPath' özelliğini kullanın. Her ikisi de birlikte kullanılamaz. Dosya adı büyük/küçük harf duyarlıdır. |Hayır |
| Tanım -lar |Pig komut dosyası içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin |Hayır |

## <a name="example"></a>Örnek
Şirketiniz tarafından başlatılan oyunları oynayan oyuncular tarafından harcanan zamanı belirlemek istediğiniz oyun günlükleri analizine bir örnek verelim.

Aşağıdaki örnek oyun günlüğü virgül (,) ayrılmış bir dosyadır. ProfileID, SessionStart, Duration, SrcIPAddress ve GameType gibi aşağıdaki alanları içerir.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

Bu verileri işlemek için **Pig komut dosyası:**

```
PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);

GroupProfile = Group PigSampleIn all;

PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);

Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');
```

Bu Pig komut dosyasını Veri Fabrikası ardışık bir şekilde yürütmek için aşağıdaki adımları yapın:

1. [Kendi HDInsight bilgi işlem kümenizi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) kaydetmek veya [isteğe bağlı HDInsight bilgi işlem kümesini](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)yapılandırmak için bağlantılı bir hizmet oluşturun. Bu bağlantılı hizmet **hdInsightLinkedService**diyelim.
2. Verileri barındıran Azure Blob depolamasına olan bağlantıyı yapılandırmak için bağlantılı bir [hizmet](data-factory-azure-blob-connector.md) oluşturun. Bu bağlantılı hizmet **StorageLinkedService**diyelim.
3. Girişve çıktı verilerini gösteren [veri kümeleri](data-factory-create-datasets.md) oluşturun. Giriş veri seti **PigSampleIn** ve çıkış dataset **PigSampleOut**çağıralım.
4. #2 adım olarak yapılandırılan Azure Blob Depolama dosyasındaki Pig sorgusunu kopyalayın. Verileri barındıran Azure depolama alanı sorgu dosyasını barındıran aygıttan farklıysa, ayrı bir Azure Depolama bağlantısı hizmeti oluşturun. Etkinlik yapılandırmasındaki bağlantılı hizmete bakın. Pig script dosyası ve **scriptLinkedService**yolunu belirtmek için **scriptPath** kullanın. 
   
   > [!NOTE]
   > **Ayrıca, komut dosyası** özelliğini kullanarak etkinlik tanımında Pig komut dosyasını satır satır olarak da sağlayabilirsiniz. Ancak, komut dosyasındaki tüm özel karakterlerin kaçması gerektiğinden ve hata ayıklama sorunlarına neden olabileceğinden, bu yaklaşımı önermiyoruz. En iyi uygulama adım #4 takip etmektir.
   >
   >
5. HDInsightPig etkinliği yle ardışık hattı oluşturun. Bu etkinlik, HDInsight kümesinde Pig komut dosyası çalıştırarak giriş verilerini işler.

    ```JSON
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
              "scriptLinkedService": "StorageLinkedService"
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            }
          }
        ]
      }
    }
    ```
6. Boru hattını dağıtın. Ayrıntılar için [ardışık hatlar](data-factory-create-pipelines.md) oluşturma makalesine bakın. 
7. Veri fabrikası izleme ve yönetim görünümlerini kullanarak boru hattını izleyin. Ayrıntılar için Veri Fabrikası boru hatları makalelerine [bakın.](data-factory-monitor-manage-pipelines.md)

## <a name="specifying-parameters-for-a-pig-script"></a>Pig komut dosyası için parametreleri belirtme
Aşağıdaki örneği göz önünde bulundurun: Oyun günlükleri her gün Azure Blob Depolama'ya alınır ve tarih ve saate göre bölümlenmiş bir klasörde saklanır. Pig komut dosyası parametreve çalışma süresi sırasında dinamik giriş klasörü konumunu geçmek ve aynı zamanda tarih ve saat ile bölümlenmiş çıkış üretmek istiyorum.

Parametreli Pig komut dosyasını kullanmak için aşağıdakileri yapın:

* Tanımlardaki parametreleri **tanımlayın.**

    ```JSON
    {
      "name": "PigActivitySamplePipeline",
      "properties": {
        "activities": [
          {
            "name": "PigActivitySample",
            "type": "HDInsightPig",
            "inputs": [
              {
                "name": "PigSampleIn"
              }
            ],
            "outputs": [
              {
                "name": "PigSampleOut"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "typeproperties": {
              "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0:MM}/dayno={0: dd}/',SliceStart)",
                "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            }
          }
        ]
      }
    }
    ```
* Pig Script'te aşağıdaki örnekte gösterildiği gibi '**$parameterName**' kullanarak parametrelere bakın:

    ```
    PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    GroupProfile = Group PigSampleIn all;
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    Store PigSampleOut into '$Output' USING PigStorage (','); 
    ```

## <a name="see-also"></a>Ayrıca Bkz.
* [Kovan Etkinliği](data-factory-hive-activity.md)
* [MapReduce Etkinliği](data-factory-map-reduce.md)
* [Hadoop Akış Etkinliği](data-factory-hadoop-streaming-activity.md)
* [Spark programları çağırma](data-factory-spark.md)
* [R betikleri çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample)
