---
title: Databricks Not defteri ile veri dönüştürme
description: Databricks Not defteri çalıştırarak verileri nasıl işleyeceğini veya dönüştürebileceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: nabhishek
ms.author: abnarain
manager: shwang
ms.reviewer: maghan
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: 6d3c9f0df0d834ffe75d0b56e3c80a432c27ea38
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81419026"
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Databricks Not defteri çalıştırarak verileri dönüştürme
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bir [Data Factory](concepts-pipelines-activities.md) işlem hattındaki Azure Databricks Not defteri etkinliği Azure Databricks çalışma alanınızda bir Databricks Not defteri çalıştırır. Bu makale, veri dönüştürme ve desteklenen dönüştürme etkinliklerine genel bir bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesinde oluşturulur.Azure Databricks, Apache Spark çalıştırmak için yönetilen bir platformdur.

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
|ad|İşlem hattındaki etkinliğin adı.|Yes|
|açıklama|Etkinliğin ne yaptığını açıklayan metin.|Hayır|
|type|Databricks Not defteri etkinliği için etkinlik türü Databricksnot Defterinizdir.|Yes|
|linkedServiceName|Databricks Not defterinin çalıştığı Databricks bağlı hizmetinin adı. Bu bağlı hizmet hakkında bilgi edinmek için bkz. [işlem bağlı hizmetleri](compute-linked-services.md) makalesi.|Yes|
|Not Defteriyolu|Databricks çalışma alanında çalıştırılacak olan Not defterinin mutlak yolu. Bu yol bir eğik çizgiyle başlamalıdır.|Yes|
|baseParameters|Anahtar-değer çiftleri dizisi. Temel parametreler her etkinlik çalıştırması için kullanılabilir. Not defteri belirtilmemiş bir parametre alırsa, not defterindeki varsayılan değer kullanılacaktır. [Databricks not defterlerinde](https://docs.databricks.com/api/latest/jobs.html#jobsparampair)parametreler hakkında daha fazla bilgi bulabilirsiniz.|Hayır|
|kitaplıklar|İşi yürütecek olan kümeye yüklenecek kitaplıkların listesi. Bu bir \<dize dizisi, nesne> olabilir.|Hayır|


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

Daha ayrıntılı bilgi için bkz. kitaplık türleri için [Databricks belgeleri](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) .

## <a name="passing-parameters-between-notebooks-and-data-factory"></a>Not defterleri ve Data Factory arasında parametreleri geçirme

Databricks etkinliğinde *Baseparameters* özelliğini kullanarak, Veri Fabrikası parametrelerini not defterlerine geçirebilirsiniz. 

Belirli durumlarda, veri fabrikasında denetim akışı (koşullu denetimler) için kullanılabilecek veya aşağı akış etkinlikleri tarafından tüketilen (boyut sınırı 2MB), belirli değerleri not defteri 'nden Data Factory 'ye geri geçirmeniz gerekebilir. 

1. Not defterinizde [dbutils. Not defteri. Exit ("ReturnValue")](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-workflows.html#notebook-workflows-exit) ve karşılık gelen "ReturnValue" de Data Factory 'ye döndürülür.

2. Veri fabrikasında, gibi bir ifade kullanarak çıktıyı kullanabilirsiniz `'@activity('databricks notebook activity name').output.runOutput'`. 

   > [!IMPORTANT]
   > JSON nesnesi geçirçalışıyorsanız, özellik adlarını ekleyerek değerleri alabilirsiniz. Örnek: `'@activity('databricks notebook activity name').output.runOutput.PropertyName'`

## <a name="how-to-upload-a-library-in-databricks"></a>Databricks 'te bir kitaplığı karşıya yükleme

#### <a name="using-databricks-workspace-ui"></a>[Databricks çalışma alanı kullanıcı arabirimini kullanma](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Kullanıcı arabirimi kullanılarak eklenen kitaplığın dBFS yolunu almak için [Databricks CLI (yükleme)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli)kullanabilirsiniz. 

Genellikle, jar kitaplıkları Kullanıcı arabirimi kullanılırken dBFS:/FileStore/jars altında depolanır. CLı: *databricks FS ls dBFS:/FileStore/jars*aracılığıyla tümünü listeleyebilirsiniz.



#### <a name="copy-library-using-databricks-cli"></a>[Databricks CLı kullanarak Kitaplığı kopyalama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Örnek: *databricks FS CP sparkpi-Assembly-0.1. jar dBFS:/FileStore/jars*
