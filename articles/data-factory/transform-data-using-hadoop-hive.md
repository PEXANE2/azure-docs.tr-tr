---
title: Hadoop Hive etkinliğini kullanarak verileri dönüştürme
description: Bir Azure Data Factory 'deki Hive etkinliğini isteğe bağlı/kendi HDInsight kümeniz üzerinde kullanarak Hive sorguları çalıştırmak için nasıl kullanabileceğinizi öğrenin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: anandsub
ms.custom: seo-lt-2019
ms.date: 01/15/2019
ms.openlocfilehash: 8c5c917e12b1314c40763f58a7723a4df787ffa0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81418941"
---
# <a name="transform-data-using-hadoop-hive-activity-in-azure-data-factory"></a>Azure Data Factory Hadoop Hive etkinliğini kullanarak verileri dönüştürme

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-hive-activity.md)
> * [Geçerli sürüm](transform-data-using-hadoop-hive.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir Data Factory işlem [hattının](concepts-pipelines-activities.md) HDInsight Hive etkinliği, [kendi kendinize](compute-linked-services.md#azure-hdinsight-linked-service) veya [isteğe](compute-linked-services.md#azure-hdinsight-on-demand-linked-service) bağlı HDInsight kümenizde Hive sorguları yürütür. Bu makale, veri dönüştürme ve desteklenen dönüştürme etkinliklerine genel bir bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesinde oluşturulur.

Azure Data Factory yeni bir deyişle, [Azure Data Factory 'ye giriş](introduction.md) ile okuyun ve [öğreticiyi yapın:](tutorial-transform-data-spark-powershell.md) bu makaleyi okumadan önce verileri dönüştürün. 

## <a name="syntax"></a>Sözdizimi

```json
{
    "name": "Hive Activity",
    "description": "description",
    "type": "HDInsightHive",
    "linkedServiceName": {
        "referenceName": "MyHDInsightLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "scriptLinkedService": {
            "referenceName": "MyAzureStorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "scriptPath": "MyAzureStorage\\HiveScripts\\MyHiveSript.hql",
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
| Özellik            | Açıklama                                                  | Gerekli |
| ------------------- | ------------------------------------------------------------ | -------- |
| ad                | Etkinliğin adı                                         | Yes      |
| açıklama         | Etkinliğin ne için kullanıldığını açıklayan metin                | Hayır       |
| type                | Hive etkinliği için etkinlik türü Hdınsighthive        | Yes      |
| linkedServiceName   | Data Factory bağlı hizmet olarak kaydedilen HDInsight kümesine başvuru. Bu bağlı hizmet hakkında bilgi edinmek için bkz. [işlem bağlı hizmetleri](compute-linked-services.md) makalesi. | Yes      |
| scriptLinkedService | Yürütülecek Hive betiğini depolamak için kullanılan bir Azure depolama bağlı hizmetine yönelik başvuru. Bu bağlı hizmeti belirtmezseniz, HDInsight bağlı hizmetinde tanımlanan Azure depolama bağlı hizmeti kullanılır. | Hayır       |
| scriptPath          | ScriptLinkedService tarafından başvurulan Azure depolama alanında depolanan betik dosyasının yolunu belirtin. Dosya adı büyük/küçük harfe duyarlıdır. | Yes      |
| GetDebugInfo        | Günlük dosyalarının, HDInsight kümesi tarafından kullanılan (veya) scriptLinkedService tarafından belirtilen Azure depolama 'ya ne zaman kopyalanacağını belirtir. İzin verilen değerler: None, Always veya Failure. Varsayılan değer: Hiçbiri. | Hayır       |
| değişkenlerinden           | Bir Hadoop işi için bir bağımsız değişken dizisi belirtir. Bağımsız değişkenler her göreve komut satırı bağımsız değişkeni olarak geçirilir. | Hayır       |
| tanımlar             | Hive betiği içinde başvurmak için parametreleri anahtar/değer çiftleri olarak belirtin. | Hayır       |
| queryTimeout        | Sorgu zaman aşımı değeri (dakika). HDInsight kümesi Kurumsal Güvenlik Paketi etkinken geçerlidir. | Hayır       |

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka yollarla nasıl dönüştürebileceğinizi açıklayan aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akışı etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Machine Learning Batch yürütme etkinliği](transform-data-using-machine-learning.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
