---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 5e7bdce3a195d8171fef1963f2e389bfeec5fffb
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91571918"
---
Azure Data Factory, işlem hatlarına tek tek veya başka bir etkinlikle zincirleme halinde eklenebilecek aşağıdaki dönüştürme etkinliklerini destekler.

| Veri dönüştürme etkinliği | İşlem ortamı |
|:--- |:--- |
| [Hive](../articles/data-factory/v1/data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](../articles/data-factory/v1/data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](../articles/data-factory/v1/data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Akışı](../articles/data-factory/v1/data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Spark](../articles/data-factory/v1/data-factory-spark.md) | HDInsight [Hadoop] |
| [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](../articles/data-factory/v1/data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Saklı yordam](../articles/data-factory/v1/data-factory-stored-proc-activity.md) |Azure SQL, Azure SYNAPSE Analytics veya SQL Server |
| [Data Lake Analytics U-SQL](../articles/data-factory/v1/data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [Olmalı](../articles/data-factory/v1/data-factory-use-custom-activities.md) |HDInsight [Hadoop] veya Azure Batch |

> [!NOTE]
> MapReduce etkinliğini kullanarak HDInsight Spark kümenizde Spark programları çalıştırabilirsiniz. Ayrıntılar için bkz. [Azure Data Factory’den Spark programlarını çağırma](../articles/data-factory/v1/data-factory-spark.md).
> R yüklü HDInsight kümenizde R betiklerini çalıştırmak için özel bir etkinlik oluşturabilirsiniz. Bkz. [Azure Data Factory kullanarak R Betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample).
> 
> 

