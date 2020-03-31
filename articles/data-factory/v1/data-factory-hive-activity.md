---
title: Kovan Etkinliği'ni kullanarak verileri dönüştürme - Azure
description: İsteğe bağlı/kendi HDInsight kümenizde Hive sorgularını çalıştırmak için Bir Azure veri fabrikasındaHive Etkinliğini nasıl kullanabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.assetid: 80083218-743e-4da8-bdd2-60d1c77b1227
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: d153f8c316cbb76e063f07f7f823c8d9c4a21f87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74703347"
---
# <a name="transform-data-using-hive-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda Kovan Etkinliği'ni kullanarak verileri dönüştürme 
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
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'ndaki Kovan etkinliğini kullanarak verileri dönüştürme bölümüne](../transform-data-using-hadoop-hive.md)bakın.

Veri Fabrikası [ardışık işlonundaki](data-factory-create-pipelines.md) HDInsight Hive etkinliği, Kovan sorgularını kendi veya [isteğe bağlı](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windows/Linux tabanlı HDInsight [kümenizde](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) yürütür. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) makalesi temel almaktadır.

> [!NOTE] 
> Azure Veri Fabrikası'nda yeniyseniz, [Azure Veri Fabrikası'na Giriş'i](data-factory-introduction.md) okuyun ve öğreticiyi yapın: Bu makaleyi okumadan önce ilk veri [ardışık işleminizi oluşturun.](data-factory-build-your-first-pipeline.md) 

## <a name="syntax"></a>Sözdizimi

```JSON
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
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
      "script": "Hive script",
      "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
      "defines": {
        "param1": "param1Value"
      }
    },
   "scheduler": {
      "frequency": "Day",
      "interval": 1
    }
}
```
## <a name="syntax-details"></a>Sözdizimi ayrıntıları
| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Etkinliğin adı |Evet |
| açıklama |Etkinliğin ne için kullanıldığını açıklayan metin |Hayır |
| type |HDinsightHive |Evet |
| Giriş |Hive aktivitesi tarafından tüketilen girdiler |Hayır |
| Çıkış |Kovan aktivitesi tarafından üretilen çıktılar |Evet |
| linkedServiceName |Veri Fabrikası'nda bağlantılı hizmet olarak kayıtlı HDInsight kümesine başvuru |Evet |
| betiğini çalıştırın |Hive komut dosyasını satır satır belirtin |Hayır |
| scriptPath |Hive komut dosyasını Azure blob depolama alanında saklayın ve dosyaya giden yolu sağlayın. 'Komut dosyası' veya 'scriptPath' özelliğini kullanın. Her ikisi de birlikte kullanılamaz. Dosya adı büyük/küçük harf duyarlıdır. |Hayır |
| Tanım -lar |'Kovan' kullanarak Hive komut dosyası içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin |Hayır |

## <a name="example"></a>Örnek
Şirketiniz tarafından başlatılan oyunları oynayan kullanıcılar tarafından harcanan zamanı belirlemek istediğiniz oyun günlükleri analizine bir örnek verelim. 

Aşağıdaki günlük virgül (`,`) ayrılmış ve aşağıdaki alanları içeren örnek bir oyun günlüğü - ProfileID, SessionStart, Süre, SrcIPAddress ve GameType.

```
1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
.....
```

Bu verileri işlemek için **Hive komut dosyası:**

```
DROP TABLE IF EXISTS HiveSampleIn; 
CREATE EXTERNAL TABLE HiveSampleIn 
(
    ProfileID        string, 
    SessionStart     string, 
    Duration         int, 
    SrcIPAddress     string, 
    GameType         string
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 

DROP TABLE IF EXISTS HiveSampleOut; 
CREATE EXTERNAL TABLE HiveSampleOut 
(    
    ProfileID     string, 
    Duration     int
) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';

INSERT OVERWRITE TABLE HiveSampleOut
Select 
    ProfileID,
    SUM(Duration)
FROM HiveSampleIn Group by ProfileID
```

Bu Hive komut dosyasını Veri Fabrikası ardışık bir şekilde yürütmek için aşağıdakileri yapmanız gerekir:

1. [Kendi HDInsight bilgi işlem kümenizi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) kaydetmek veya [isteğe bağlı HDInsight bilgi işlem kümesini](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)yapılandırmak için bağlantılı bir hizmet oluşturun. Bu bağlantılı hizmete "HDInsightLinkedService" diyelim.
2. Verileri barındıran Azure Blob depolamasına olan bağlantıyı yapılandırmak için bağlantılı bir [hizmet](data-factory-azure-blob-connector.md) oluşturun. Bu bağlantılı hizmete "StorageLinkedService" diyelim.
3. Girişve çıktı verilerini gösteren [veri kümeleri](data-factory-create-datasets.md) oluşturun. Giriş veri kümesine "HiveSampleIn" ve çıktı veri kümesini "HiveSampleOut" olarak adlandıralım.
4. Adım #2 yapılandırılan Azure Blob Depolama'ya dosya olarak Hive sorgusunu kopyalayın. Verileri barındırma depolama alanı bu sorgu dosyasını barındıran dosyadan farklıysa, ayrı bir Azure Depolama bağlantılı hizmet oluşturun ve etkinlikte bu hizmete bakın. Komut dosyası dosyasını içeren Azure depolama alanını belirtmek için kovma sorgusu dosyasına ve **komut dosyasıLinkedService'e** giden yolu belirtmek için **scriptPath'i** kullanın. 
   
   > [!NOTE]
   > **Komut dosyası** özelliğini kullanarak etkinlik tanımında Hive komut dosyasını satır satır olarak da sağlayabilirsiniz. JSON belge içindeki komut dosyasındaki tüm özel karakterlerin kaçması gerektiğinden ve hata ayıklama sorunlarına neden olabileceğinden, bu yaklaşımı önermiyoruz. En iyi uygulama adım #4 takip etmektir.
   > 
   > 
5. HDInsightHive etkinliği ile bir boru hattı oluşturun. Etkinlik verileri işler/dönüştürür.

    ```JSON   
    {   
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                {
                    "name": "HiveSampleIn"
                }
                ],
                "outputs": [
                {
                    "name": "HiveSampleOut"
                }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                    "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                    "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
            ]
        }
    }
    ```
6. Boru hattını dağıtın. Ayrıntılar için [ardışık hatlar](data-factory-create-pipelines.md) oluşturma makalesine bakın. 
7. Veri fabrikası izleme ve yönetim görünümlerini kullanarak boru hattını izleyin. Ayrıntılar için Veri Fabrikası boru hatları makalelerine [bakın.](data-factory-monitor-manage-pipelines.md) 

## <a name="specifying-parameters-for-a-hive-script"></a>Hive komut dosyası için parametreleri belirtme
Bu örnekte, oyun günlükleri her gün Azure Blob Depolama'ya alınır ve tarih ve saatle birlikte bölümlenmiş bir klasörde depolanır. Hive komut dosyası parametreve çalışma süresi sırasında dinamik giriş klasörü konumunu geçmek ve aynı zamanda tarih ve saat ile bölümlenmiş çıkış üretmek istiyorum.

Parametreli Hive komut dosyasını kullanmak için aşağıdakileri yapın

* Tanımlardaki parametreleri **tanımlayın.**

    ```JSON  
    {
        "name": "HiveActivitySamplePipeline",
          "properties": {
        "activities": [
             {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                      {
                        "name": "HiveSampleIn"
                      }
                ],
                "outputs": [
                      {
                        "name": "HiveSampleOut"
                    }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                      "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                      "scriptLinkedService": "StorageLinkedService",
                      "defines": {
                        "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)",
                        "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:MM}/dayno={0:dd}/', SliceStart)"
                      },
                       "scheduler": {
                          "frequency": "Hour",
                          "interval": 1
                    }
                }
              }
        ]
      }
    }
    ```
* Kovan Komut Dosyası'nda **,${hiveconf:parameterName}** kullanarak parametreye bakın. 
  
    ```
    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID     string, 
        SessionStart     string, 
        Duration     int, 
        SrcIPAddress     string, 
        GameType     string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 

    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (
        ProfileID     string, 
        Duration     int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';

    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID
    ```
  ## <a name="see-also"></a>Ayrıca Bkz.
* [Domuz Aktivitesi](data-factory-pig-activity.md)
* [MapReduce Etkinliği](data-factory-map-reduce.md)
* [Hadoop Akış Etkinliği](data-factory-hadoop-streaming-activity.md)
* [Spark programları çağırma](data-factory-spark.md)
* [R betikleri çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample)

