---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: e6a1e65f253f33257a0e218f4d347740c07581cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74795818"
---
Azure Data Factory, işlem hatlarına tek tek veya başka bir etkinlikle zincirleme halinde eklenebilecek aşağıdaki dönüştürme etkinliklerini destekler.

| Veri dönüştürme etkinliği | İşlem ortamı |
|:--- |:--- |
| [Hive](../articles/data-factory/v1/data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Domuz](../articles/data-factory/v1/data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](../articles/data-factory/v1/data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Akışı](../articles/data-factory/v1/data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Spark](../articles/data-factory/v1/data-factory-spark.md) | HDInsight [Hadoop] |
| [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](../articles/data-factory/v1/data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Depolanan Yordam](../articles/data-factory/v1/data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Veri Ambarı veya SQL Server |
| [Data Lake Analytics U-SQL](../articles/data-factory/v1/data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [Dotnet](../articles/data-factory/v1/data-factory-use-custom-activities.md) |HDInsight [Hadoop] veya Azure Batch |

> [!NOTE]
> MapReduce etkinliğini kullanarak HDInsight Spark kümenizde Spark programları çalıştırabilirsiniz. Ayrıntılar için bkz. [Azure Data Factory’den Spark programlarını çağırma](../articles/data-factory/v1/data-factory-spark.md).
> R yüklü HDInsight kümenizde R betiklerini çalıştırmak için özel bir etkinlik oluşturabilirsiniz. Bkz. [Azure Data Factory kullanarak R Betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample).
> 
> 

