---
title: Databricks Not defteri ile veri dönüştürme
description: Azure Data Factory bir Databricks Not defteri çalıştırarak verileri nasıl işleyeceğini veya dönüştürebileceğinizi öğrenin.
ms.service: data-factory
author: nabhishek
ms.author: abnarain
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: 486dc2ab3a14917e8c7bdddf8b5b9c6f9da1a1dc
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100374006"
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Databricks Not defteri çalıştırarak verileri dönüştürme
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir [Data Factory](concepts-pipelines-activities.md) işlem hattındaki Azure Databricks Not defteri etkinliği Azure Databricks çalışma alanınızda bir Databricks Not defteri çalıştırır. Bu makale, veri dönüştürme ve desteklenen dönüştürme etkinliklerine genel bir bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesinde oluşturulur. Azure Databricks, Apache Spark çalıştırmak için yönetilen bir platformdur.

## <a name="databricks-notebook-activity-definition"></a>Databricks Not defteri etkinlik tanımı

Databricks Not defteri etkinliğinin örnek JSON tanımı aşağıda verilmiştir:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksNotebook",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "notebookPath": "/Users/user@example.com/ScalaExampleNotebook",
            "baseParameters": {
                "inputpath": "input/folder1/",
                "outputpath": "output/"
            },
            "libraries": [
                {
                "jar": "dbfs:/docs/library.jar"
                }
            ]
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Databricks Not defteri etkinlik özellikleri

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

|Özellik|Açıklama|Gerekli|
|---|---|---|
|name|İşlem hattındaki etkinliğin adı.|Yes|
|açıklama|Etkinliğin ne yaptığını açıklayan metin.|No|
|tür|Databricks Not defteri etkinliği için etkinlik türü Databricksnot Defterinizdir.|Yes|
|linkedServiceName|Databricks Not defterinin çalıştığı Databricks bağlı hizmetinin adı. Bu bağlı hizmet hakkında bilgi edinmek için bkz. [işlem bağlı hizmetleri](compute-linked-services.md) makalesi.|Yes|
|Not Defteriyolu|Databricks çalışma alanında çalıştırılacak olan Not defterinin mutlak yolu. Bu yol bir eğik çizgiyle başlamalıdır.|Yes|
|baseParameters|Key-Value çiftleri dizisi. Temel parametreler her etkinlik çalıştırması için kullanılabilir. Not defteri belirtilmemiş bir parametre alırsa, not defterindeki varsayılan değer kullanılacaktır. [Databricks not defterlerinde](https://docs.databricks.com/api/latest/jobs.html#jobsparampair)parametreler hakkında daha fazla bilgi bulabilirsiniz.|No|
|kitaplıklar|İşi yürütecek olan kümeye yüklenecek kitaplıkların listesi. Bir dizisi olabilir \<string, object> .|No|

## <a name="supported-libraries-for-databricks-activities"></a>Databricks etkinlikleri için desteklenen kitaplıklar

Yukarıdaki databricks etkinlik tanımında şu kitaplık türlerini belirtirsiniz: *jar*, *Egg*, *WHL*, *Maven*, *Pypı*, *Cran*.

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
            "whl": "dbfs:/mnt/libraries/mlflow-0.0.1.dev0-py2-none-any.whl"
        },
        {
            "whl": "dbfs:/mnt/libraries/wheel-libraries.wheelhouse.zip"
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

Daha ayrıntılı bilgi için bkz. kitaplık türleri için [Databricks belgeleri](/azure/databricks/dev-tools/api/latest/libraries#managedlibrarieslibrary) .

## <a name="passing-parameters-between-notebooks-and-data-factory"></a>Not defterleri ve Data Factory arasında parametreleri geçirme

Databricks etkinliğinde *Baseparameters* özelliğini kullanarak, Veri Fabrikası parametrelerini not defterlerine geçirebilirsiniz.

Belirli durumlarda, veri fabrikasında denetim akışı (koşullu denetimler) için kullanılabilecek veya aşağı akış etkinlikleri tarafından tüketilen (boyut sınırı 2MB), belirli değerleri not defteri 'nden Data Factory 'ye geri geçirmeniz gerekebilir.

1. Not defterinizde [dbutils. Not defteri. Exit ("ReturnValue")](/azure/databricks/notebooks/notebook-workflows#notebook-workflows-exit) ve karşılık gelen "ReturnValue" de Data Factory 'ye döndürülür.

2. Veri fabrikasında, gibi bir ifade kullanarak çıktıyı kullanabilirsiniz `'@activity('databricks notebook activity name').output.runOutput'` .

   > [!IMPORTANT]
   > JSON nesnesi geçirçalışıyorsanız, özellik adlarını ekleyerek değerleri alabilirsiniz. Örnek: `'@activity('databricks notebook activity name').output.runOutput.PropertyName'`

## <a name="how-to-upload-a-library-in-databricks"></a>Databricks 'te bir kitaplığı karşıya yükleme

### <a name="you-can-use-the-workspace-ui"></a>Çalışma alanı kullanıcı arabirimini kullanabilirsiniz:

1. [Databricks çalışma alanı kullanıcı arabirimini kullanma](/azure/databricks/libraries/#create-a-library)

2. Kullanıcı arabirimi kullanılarak eklenen kitaplığın dBFS yolunu almak için [Databricks CLI](/azure/databricks/dev-tools/cli/#install-the-cli)kullanabilirsiniz.

   Genellikle jar kitaplıkları, Kullanıcı arabirimi kullanılırken dBFS:/FileStore/jars altında depolanır. CLı aracılığıyla tümünü listeleme: *databricks FS ls dBFS:/FileStore/Job-jars*

### <a name="or-you-can-use-the-databricks-cli"></a>Ya da Databricks CLı 'yi de kullanabilirsiniz:

1. [Databricks CLI kullanarak kitaplığı kopyalamayı](/azure/databricks/dev-tools/cli/#copy-a-file-to-dbfs) izleyin

2. Databricks CLı kullanma [(yükleme adımları)](/azure/databricks/dev-tools/cli/#install-the-cli)

   Örnek olarak, bir JAR 'yi dBFS 'e kopyalamak için: `dbfs cp SparkPi-assembly-0.1.jar dbfs:/docs/sparkpi.jar`
