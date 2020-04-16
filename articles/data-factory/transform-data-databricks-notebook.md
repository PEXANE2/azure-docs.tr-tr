---
title: Databricks Notebook ile verileri dönüştürme
description: Databricks not defterini çalıştırarak verileri nasıl işitecek veya dönüştüreceklerini öğrenin.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419026"
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Databricks not defteri çalıştırarak verileri dönüştürme
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

[Veri Fabrikası ardışık alanındaki](concepts-pipelines-activities.md) Azure Databricks Dizüstü Etkinliği, Azure Databricks çalışma alanınızda bir Databricks dizüstü bilgisayarı çalıştırıyor. Bu makalede, veri dönüşümü ve desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [veri dönüştürme etkinlikleri](transform-data.md) makalesi temel almaktadır.Azure Databricks, Apache Spark'ı çalıştırmak için yönetilen bir platformdur.

## <a name="databricks-notebook-activity-definition"></a>Databricks Notebook etkinlik tanımı

Databricks Not Defteri Etkinliği'nin örnek JSON tanımı aşağıda verilmiştir:

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

## <a name="databricks-notebook-activity-properties"></a>Databricks Notebook etkinlik özellikleri

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

|Özellik|Açıklama|Gerekli|
|---|---|---|
|ad|Boru hattındaki etkinliğin adı.|Evet|
|açıklama|Etkinliğin ne yaptığını açıklayan metin.|Hayır|
|type|Databricks Notebook Etkinliği için etkinlik türü DatabricksNotebook'tür.|Evet|
|linkedServiceName|Databricks not defterinin çalıştığı Databricks Bağlantılı Hizmetin adı. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın.|Evet|
|notebookPath|Veri Tuğlaları Çalışma Alanı'nda çalıştırılacak not defterinin mutlak yolu. Bu yol bir kesikle başlamalı.|Evet|
|bazParametreler|Anahtar Değeri çiftlerinden oluşan bir dizi. Her etkinlik çalışması için temel parametreler kullanılabilir. Not defteri belirtilmeyen bir parametre alırsa, not defterindeki varsayılan değer kullanılır. [Databricks Notebook'larda](https://docs.databricks.com/api/latest/jobs.html#jobsparampair)parametreler hakkında daha fazla bilgi edinin.|Hayır|
|kitaplıklar|İşi yürütecek kümeye yüklenecek kitaplıkların listesi. Bu \<dize, nesne> bir dizi olabilir.|Hayır|


## <a name="supported-libraries-for-databricks-activities"></a>Databricks etkinlikleri için desteklenen kütüphaneler

Yukarıdaki Databricks etkinlik tanımında, bu kütüphane türlerini belirtin: *kavanoz,* *yumurta,* *whl,* *maven,* *pypi,* *cran*.

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

Daha fazla ayrıntı için kitaplık türleri için [Databricks belgelerine](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) bakın.

## <a name="passing-parameters-between-notebooks-and-data-factory"></a>Dizüstü bilgisayarlar ve Veri Fabrikası arasında parametreleri geçirme

Veri tuğlaları etkinliğinde *baseParameters* özelliğini kullanarak veri fabrikası parametrelerini not defterlerine aktarabilirsiniz. 

Bazı durumlarda, veri fabrikasında kontrol akışı (koşullu denetimler) için kullanılabilen veya akış aşağı etkinlikleri tarafından tüketilen (boyut sınırı 2MB) olan belirli değerleri not defterinden veri fabrikasına geri geçirmeniz gerekebilir. 

1. Not defterinizde [dbutiller.notebook.exit("returnValue")](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-workflows.html#notebook-workflows-exit) numaralı telefonu arayabilirsiniz ve ilgili "returnValue" veri fabrikasına iade edilir.

2. Veri fabrikasındaki çıktıyı `'@activity('databricks notebook activity name').output.runOutput'`. 

   > [!IMPORTANT]
   > JSON nesnesi geçiyorsanız özellik adlarını ekleyerek değerleri alabilirsiniz. Örnek: `'@activity('databricks notebook activity name').output.runOutput.PropertyName'`

## <a name="how-to-upload-a-library-in-databricks"></a>Databricks'te kitaplık yükleme

#### <a name="using-databricks-workspace-ui"></a>[Databricks çalışma alanı arabirimi kullanma](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Web'si kullanarak eklenen kitaplığın dbfs yolunu elde etmek için [Databricks CLI (yükleme)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli)kullanabilirsiniz. 

Genellikle, Jar kitaplıkları Kullanıcı Aracı'nı kullanırken dbfs:/FileStore/jars altında depolanır. Tüm CLI listeleyebilirsiniz: *databricks fs ls dbfs:/FileStore/jars*.



#### <a name="copy-library-using-databricks-cli"></a>[Databricks CLI kullanarak kitaplığı kopyalama](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Örnek: *databricks fs cp SparkPi-assembly-0.1.jar dbfs:/FileStore/jars*
