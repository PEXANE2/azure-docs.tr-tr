---
title: Databricks Python ile veri dönüştürme
description: Databricks Python çalıştırarak verileri nasıl işleyeceğini veya dönüştüreceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/15/2018
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: anandsub
ms.openlocfilehash: be2e389a0f103983a566a3f74d201e5589d84586
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74926731"
---
# <a name="transform-data-by-running-a-python-activity-in-azure-databricks"></a>Azure Databricks bir Python etkinliği çalıştırarak verileri dönüştürme

Bir [Data Factory işlem hattının](concepts-pipelines-activities.md) Azure Databricks python etkinliği Azure Databricks kümenizdeki bir Python dosyası çalıştırır. Bu makale, veri dönüştürme ve desteklenen dönüştürme etkinliklerine genel bir bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesine yönelik olarak oluşturulur. Azure Databricks, Apache Spark çalıştırmak için yönetilen bir platformdur.

Bu özelliğe yönelik on bir dakikalık bir giriş ve tanıtım için, aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-python-activity-definition"></a>Databricks Python etkinlik tanımı

Databricks Python etkinliğinin örnek JSON tanımı aşağıda verilmiştir:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksSparkPython",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "pythonFile": "dbfs:/docs/pi.py",
            "parameters": [
                "10"
            ],
            "libraries": [
                {
                    "pypi": {
                        "package": "tensorflow"
                    }
                }
            ]
        }
    }
}
```

## <a name="databricks-python-activity-properties"></a>Databricks Python etkinlik özellikleri

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

|Özellik|Açıklama|Gereklidir|
|---|---|---|
|ad|İşlem hattındaki etkinliğin adı.|Yes|
|açıklama|Etkinliğin ne yaptığını açıklayan metin.|Hayır|
|type|Databricks Python etkinliği için etkinlik türü Databricksmini Python ' dır.|Yes|
|linkedServiceName|Python etkinliğinin çalıştırıldığı Databricks bağlı hizmetinin adı. Bu bağlı hizmet hakkında bilgi edinmek için bkz. [işlem bağlantılı hizmetleri](compute-linked-services.md) makalesi.|Yes|
|pythonFile|Yürütülecek Python dosyasının URI 'SI. Yalnızca DBFS yolları desteklenir.|Yes|
|parametreler|Python dosyasına geçirilecek komut satırı parametreleri. Bu bir dizeler dizisidir.|Hayır|
|kitaplıklar|İşi yürütecek olan kümeye yüklenecek kitaplıkların listesi. < Dize, nesne > dizisi olabilir|Hayır|

## <a name="supported-libraries-for-databricks-activities"></a>Databricks etkinlikleri için desteklenen kitaplıklar

Yukarıdaki databricks etkinlik tanımında şu kitaplık türlerini belirtirsiniz: *jar*, *Egg*, *Maven*, *Pypı*, *Cran*.

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "https://cran.us.r-project.org"
            }
        }
    ]
}

```

Daha fazla ayrıntı için kitaplık türleri için [Databricks belgeleri](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) bölümüne bakın.

## <a name="how-to-upload-a-library-in-databricks"></a>Databricks 'te bir kitaplığı karşıya yükleme

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Databricks çalışma alanı kullanıcı arabirimini kullanma](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Kullanıcı arabirimi kullanılarak eklenen kitaplığın dBFS yolunu almak için [Databricks CLI (yükleme)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli)kullanabilirsiniz. 

Genellikle jar kitaplıkları, Kullanıcı arabirimi kullanılırken dBFS:/FileStore/jars altında depolanır. CLı aracılığıyla tümünü listeleyebilirsiniz: *databricks FS ls dBFS:/FileStore/jars* 



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Databricks CLı kullanarak Kitaplığı kopyalama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Örnek: *databricks FS CP sparkpi-Assembly-0.1. jar dBFS:/FileStore/jars*