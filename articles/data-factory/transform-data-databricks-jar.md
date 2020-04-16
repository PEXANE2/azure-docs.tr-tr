---
title: Databricks Jar ile verileri dönüştürme
description: Databricks Jar çalıştırarak verileri nasıl işitecek veya dönüştüreceklerini öğrenin.
services: data-factory
documentationcenter: ''
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: abnarain
author: nabhishek
manager: shwang
ms.date: 03/15/2018
ms.openlocfilehash: 6b010000a674e351051c664dd5eeacd40e802439
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414620"
---
# <a name="transform-data-by-running-a-jar-activity-in-azure-databricks"></a>Azure Veri Tuğlaları'nda bir Jar etkinliği çalıştırarak verileri dönüştürme
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

[Veri Fabrikası ardışık işakindeki](concepts-pipelines-activities.md) Azure Veri Tuğlaları Jar Etkinliği, Azure Veri Tuğlaları kümenizde bir Kıvılcım Kavanozu çalıştırAr. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesi temel almaktadır.Azure Databricks, Apache Spark'ı çalıştırmak için yönetilen bir platformdur.

Bu özelliğe yönelik on bir dakikalık bir giriş ve tanıtım için, aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-jar-activity-definition"></a>Databricks Jar etkinlik tanımı

Burada bir Databricks Jar Etkinliği örnek JSON tanımı:

```json
{
    "name": "SparkJarActivity",
    "type": "DatabricksSparkJar",
    "linkedServiceName": {
        "referenceName": "AzureDatabricks",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "mainClassName": "org.apache.spark.examples.SparkPi",
        "parameters": [ "10" ],
        "libraries": [
            {
                "jar": "dbfs:/docs/sparkpi.jar"
            }
        ]
    }
}

```

## <a name="databricks-jar-activity-properties"></a>Databricks Jar etkinlik özellikleri

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

|Özellik|Açıklama|Gerekli|
|:--|---|:-:|
|ad|Boru hattındaki etkinliğin adı.|Evet|
|açıklama|Etkinliğin ne yaptığını açıklayan metin.|Hayır|
|type|Databricks Jar Etkinliği için etkinlik türü DatabricksSparkJar'dır.|Evet|
|linkedServiceName|Jar etkinliğinin çalıştığı Databricks Bağlantılı Hizmetin adı. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın.|Evet|
|mainClassName|Yürütülecek ana yöntemi içeren sınıfın tam adı. Bu sınıf, kitaplık olarak sağlanan bir JAR içinde yer almalıdır.|Evet|
|parametreler|Ana yönteme geçirilecek parametreler.  Bu bir dizi dize.|Hayır|
|kitaplıklar|İşi yürütecek kümeye yüklenecek kitaplıkların listesi. Bu <dize, nesne> bir dizi olabilir|Evet (mainClassName yöntemini içeren en az bir tane)|

> [!NOTE]
> **Bilinen Sorun** - Eşzamanlı Databricks Jar etkinliklerini çalıştırmak için aynı [Etkileşimli kümeyi](compute-linked-services.md#example---using-existing-interactive-cluster-in-databricks) kullanırken (küme yeniden başlatma olmadan), Databricks'te 1. Bu nedenle yanlış parametreler sonraki işlere geçirilen kaynaklanan. Bunun yerine bir İş [kümesi](compute-linked-services.md#example---using-new-job-cluster-in-databricks) kullanın azaltmak için. 

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

Genellikle Jar kitaplıkları Kullanıcı Aracı'nı kullanırken dbfs:/FileStore/jars altında depolanır. Tüm CLI listeleyebilirsiniz: *databricks fs ls dbfs:/FileStore/iş kavanozları* 



#### <a name="copy-library-using-databricks-cli"></a>[Databricks CLI kullanarak kitaplığı kopyalama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)
Databricks CLI [(yükleme adımları)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli)kullanın. 

Örnek - JAR'ı dbfs'e kopyalama: *dbfs cp SparkPi-assembly-0.1.jar dbfs:/docs/sparkpi.jar*
