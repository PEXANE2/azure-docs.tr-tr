---
title: Databricks Python ile verileri dönüştürme
description: Databricks Python çalıştırarak verileri nasıl işitecek veya dönüştüreceklerini öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74926731"
---
# <a name="transform-data-by-running-a-python-activity-in-azure-databricks"></a>Azure Veri Tuğlaları'nda bir Python etkinliği çalıştırarak verileri dönüştürme

[Veri Fabrikası ardışık dizisindeki](concepts-pipelines-activities.md) Azure Veri Tuğlaları Python Etkinliği, Azure Veri Tuğlaları kümenizde bir Python dosyasını çalıştırAr. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesi temel almaktadır.Azure Databricks, Apache Spark'ı çalıştırmak için yönetilen bir platformdur.

Bu özelliğe yönelik on bir dakikalık bir giriş ve tanıtım için, aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-python-activity-definition"></a>Databricks Python etkinlik tanımı

Databricks Python Etkinliği'nin örnek JSON tanımı aşağıda verilmiştir:

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

|Özellik|Açıklama|Gerekli|
|---|---|---|
|ad|Boru hattındaki etkinliğin adı.|Evet|
|açıklama|Etkinliğin ne yaptığını açıklayan metin.|Hayır|
|type|Databricks Python Etkinliği için etkinlik türü DatabricksSparkPython'dur.|Evet|
|linkedServiceName|Python etkinliğinin çalıştığı Databricks Bağlantılı Hizmetin adı. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın.|Evet|
|pythonFile|Yürütülecek Python dosyasının URI'si. Yalnızca DBFS yolları desteklenir.|Evet|
|parametreler|Python dosyasına geçirilecek komut satırı parametreleri. Bu bir dizi dize.|Hayır|
|kitaplıklar|İşi yürütecek kümeye yüklenecek kitaplıkların listesi. Bu <dize, nesne> bir dizi olabilir|Hayır|

## <a name="supported-libraries-for-databricks-activities"></a>Databricks etkinlikleri için desteklenen kütüphaneler

Yukarıdaki Databricks etkinlik tanımında bu kütüphane türlerini belirtin: *kavanoz,* *yumurta,* *maven,* *pypi,* *cran*.

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

Daha fazla bilgi için kitaplık türleri için [Databricks belgelerine](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) bakın.

## <a name="how-to-upload-a-library-in-databricks"></a>Databricks'te kitaplık yükleme

#### <a name="using-databricks-workspace-ui"></a>[Databricks çalışma alanı arabirimi kullanma](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Web'si web si kullanılarak eklenen kitaplığın dbfs yolunu elde etmek için [Databricks CLI (yükleme)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli)kullanabilirsiniz. 

Genellikle Jar kitaplıkları Kullanıcı Aracı'nı kullanırken dbfs:/FileStore/jars altında depolanır. Tüm CLI listeleyebilirsiniz: *databricks fs ls dbfs:/FileStore/jars* 



#### <a name="copy-library-using-databricks-cli"></a>[Databricks CLI kullanarak kitaplığı kopyalama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Örnek: *databricks fs cp SparkPi-assembly-0.1.jar dbfs:/FileStore/jars*