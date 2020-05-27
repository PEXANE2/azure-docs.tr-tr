---
title: Hadoop Pig etkinliğini kullanarak verileri dönüştürme
description: Bir Azure Data Factory 'de Pig etkinliğini kullanarak bir isteğe bağlı/kendi HDInsight kümeniz üzerinde Pig betikleri nasıl çalıştırabileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: shwang
ms.custom: seo-lt-2019
ms.date: 05/08/2020
ms.openlocfilehash: 54eff77daa7e69c39e9ec5d6f326f2f64c9fcafb
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83849238"
---
# <a name="transform-data-using-hadoop-pig-activity-in-azure-data-factory"></a>Azure Data Factory Hadoop Pig etkinliğini kullanarak verileri dönüştürme

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-pig-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-pig.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir [Data Factory işlem](concepts-pipelines-activities.md) hattındaki HDInsight Pig etkinliği, [kendi](compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe bağlı](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) HDInsight kümenizdeki Pig sorgularını yürütür. Bu makale, veri dönüştürme ve desteklenen dönüştürme etkinliklerine genel bir bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesinde oluşturulur.

Azure Data Factory yeni bir deyişle, [Azure Data Factory 'ye giriş](introduction.md) ile okuyun ve [öğreticiyi yapın:](tutorial-transform-data-spark-powershell.md) bu makaleyi okumadan önce verileri dönüştürün. 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Pig Activity",
    "description": "description",
    "type": "HDInsightPig",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\PigScripts\\MyPigSript.pig",
        "getDebugInfo": "Failure",
        "arguments": [
            "SampleHadoopJobArgument1"
        ],
        "defines": {
            "param1": "param1Value"
        }
    }   
}
```
## <a name="syntax-details"></a>Söz dizimi ayrıntıları

| Özellik            | Açıklama                              | Gerekli |
| ------------------- | ---------------------------------------- | -------- |
| name                | Etkinliğin adı                     | Yes      |
| açıklama         | Etkinliğin ne için kullanıldığını açıklayan metin | No       |
| tür                | Hive etkinliği için etkinlik türü HDinsightPig | Yes      |
| linkedServiceName   | Data Factory bağlı hizmet olarak kaydedilen HDInsight kümesine başvuru. Bu bağlı hizmet hakkında bilgi edinmek için bkz. [işlem bağlı hizmetleri](compute-linked-services.md) makalesi. | Yes      |
| scriptLinkedService | Yürütülecek Pig betiğini depolamak için kullanılan bir Azure depolama bağlı hizmetine yönelik başvuru. Burada yalnızca **[Azure Blob depolama](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage)** ve **[ADLS 2.](https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-storage)** bağlı hizmetleri desteklenir. Bu bağlı hizmeti belirtmezseniz, HDInsight bağlı hizmetinde tanımlanan Azure depolama bağlı hizmeti kullanılır. | No       |
| scriptPath          | ScriptLinkedService tarafından başvurulan Azure depolama alanında depolanan betik dosyasının yolunu belirtin. Dosya adı büyük/küçük harfe duyarlıdır. | No       |
| GetDebugInfo        | Günlük dosyalarının, HDInsight kümesi tarafından kullanılan (veya) scriptLinkedService tarafından belirtilen Azure depolama 'ya ne zaman kopyalanacağını belirtir. İzin verilen değerler: None, Always veya Failure. Varsayılan değer: Hiçbiri. | No       |
| değişkenlerinden           | Bir Hadoop işi için bir bağımsız değişken dizisi belirtir. Bağımsız değişkenler her göreve komut satırı bağımsız değişkeni olarak geçirilir. | No       |
| tanımlar             | Pig betiği içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka yollarla nasıl dönüştürebileceğinizi açıklayan aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akışı etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
