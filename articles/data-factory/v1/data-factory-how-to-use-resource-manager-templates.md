---
title: Veri Fabrikasında Kaynak Yöneticisi şablonlarını kullanma
description: Veri Fabrikası varlıkları oluşturmak için Azure Kaynak Yöneticisi şablonlarını nasıl oluşturup kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 73498b3537f4cf9313fc9e2464785f63c2af0d5a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460741"
---
# <a name="use-templates-to-create-azure-data-factory-entities"></a>Azure Veri Fabrikası varlıkları oluşturmak için şablonları kullanma
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. 

## <a name="overview"></a>Genel Bakış
Veri tümleştirme gereksinimleriniz için Azure Veri Fabrikası'nı kullanırken, kendinizi farklı ortamlarda aynı deseni yeniden kullanırken veya aynı görevi aynı çözüm içinde tekrar tekrar uygularken bulabilirsiniz. Şablonlar, bu senaryoları kolay bir şekilde uygulamanıza ve yönetmenize yardımcı olur. Azure Veri Fabrikası'ndaki şablonlar, yeniden kullanılabilirlik ve yineleme içeren senaryolar için idealdir.

Bir kuruluşun dünya çapında 10 üretim tesisi olduğu durumu göz önünde bulundurun. Her tesisin günlükleri ayrı bir şirket içi SQL Server veritabanında depolanır. Şirket, özel analitik için bulutta tek bir veri ambarı oluşturmak istiyor. Ayrıca aynı mantık ama geliştirme, test ve üretim ortamları için farklı yapılandırmaları istiyor.

Bu durumda, bir görevin aynı ortamda yinelanması gerekir, ancak her üretim tesisi için 10 veri fabrikaları arasında farklı değerlere sahip. Aslında, **yineleme** mevcuttur. Templating bu genel akışı soyutlama sağlar (yani, her veri fabrikasında aynı faaliyetleri olan boru hatları), ancak her üretim tesisi için ayrı bir parametre dosyası kullanır.

Ayrıca, kuruluş bu 10 veri fabrikasını farklı ortamlarda birden çok kez dağıtmak istediğinden, şablonlar geliştirme, test ve üretim ortamları için ayrı parametre dosyalarını kullanarak bu **yeniden kullanılabilirliği** kullanabilir.

## <a name="templating-with-azure-resource-manager"></a>Azure Kaynak Yöneticisi ile Templating
[Azure Kaynak Yöneticisi şablonları,](../../azure-resource-manager/templates/overview.md) Azure Veri Fabrikası'nda geçici bir şekilde gerçekleştirmenin harika bir yoludur. Kaynak Yöneticisi şablonları, Bir JSON dosyası aracılığıyla Azure çözümünüzün altyapısını ve yapılandırmasını tanımlar. Azure Kaynak Yöneticisi şablonları tüm/çoğu Azure hizmetiyle birlikte çalıştığı için, Azure varlıklarınızın tüm kaynaklarını kolayca yönetmek için yaygın olarak kullanılabilir. Genel olarak Kaynak Yöneticisi Şablonları hakkında daha fazla bilgi edinmek için [Azure Kaynak Yöneticisi şablonları yazma'ya](../../azure-resource-manager/templates/template-syntax.md) bakın.

## <a name="tutorials"></a>Öğreticiler
Kaynak Yöneticisi şablonlarını kullanarak Veri Fabrikası varlıkları oluşturmak için adım adım yönergeler için aşağıdaki öğreticilere bakın:

* [Öğretici: Azure Kaynak Yöneticisi şablonu kullanarak verileri kopyalamak için bir ardışık yol oluşturma](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [Öğretici: Azure Kaynak Yöneticisi şablonu kullanarak verileri işlemek için bir ardışık kaynak oluşturma](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>GitHub'da Veri Fabrikası şablonları
GitHub'da aşağıdaki Azure hızlı başlangıç şablonlarına göz atın:

* [Azure Blob Depolama'dan Azure SQL Veritabanına veri kopyalamak için bir Veri fabrikası oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [Azure HDInsight kümesinde Hive etkinliği yle bir Veri fabrikası oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [Salesforce'tan Azure Blobs'a veri kopyalamak için bir Veri fabrikası oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [Etkinlikleri zincirleyen bir Veri fabrikası oluşturun: FTP sunucusundaki verileri Azure Blobs'a kopyalayan, verileri dönüştürmek için isteğe bağlı HDInsight kümesinde kovan komut dosyası çağırır ve sonucu Azure SQL Veritabanına kopyalar](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

Azure Hızlı başlangıcında Azure Veri Fabrikası şablonlarınızı paylaşmakiçin [çekinmeyin.](https://azure.microsoft.com/documentation/templates/) Bu depo aracılığıyla paylaşılabilen şablonlar geliştirirken [katkı kılavuzuna](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) bakın.

Aşağıdaki bölümler, Kaynak Yöneticisi şablonundaki Veri Fabrikası kaynaklarını tanımlama hakkında ayrıntılar sağlar.

## <a name="defining-data-factory-resources-in-templates"></a>Şablonlarda Veri Fabrikası kaynaklarını tanımlama
Bir veri fabrikasını tanımlamak için üst düzey şablon:

```JSON
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": { ...
},
"variables": { ...
},
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
    "resources": [
    { "type": "linkedservices",
        ...
    },
    {"type": "datasets",
        ...
    },
    {"type": "dataPipelines",
        ...
    }
}
```

### <a name="define-data-factory"></a>Veri fabrikası tanımlama
Resource Manager şablonunda bir veri fabrikasını aşağıdaki örnekte gösterildiği gibi tanımlayın:

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
dataFactoryName "değişkenler" olarak tanımlanır:

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a>Bağlantılı hizmetleri tanımlama

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

Dağıtmak istediğiniz belirli bağlantılı hizmetin JSON özellikleri hakkında ayrıntılı bilgi için [Depolama Bağlantılı Hizmet](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya Bilgi İşlem Bağlantılı [Hizmetler'e](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) bakın. "Bağımlı" parametresi, ilgili veri fabrikasının adını belirtir. Azure Depolama için bağlantılı bir hizmet tanımlamaya örnek aşağıdaki JSON tanımında gösterilmiştir:

### <a name="define-datasets"></a>Veri kümelerini tanımlama

```JSON
"type": "datasets",
"name": "[variables('<myDatasetName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<myDatasetLinkedServiceName>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    ...
}
```
Dağıtmak istediğiniz belirli veri kümesi türü için JSON özellikleri hakkında ayrıntılı bilgi için [Desteklenen veri depolarına](data-factory-data-movement-activities.md#supported-data-stores-and-formats) bakın. "Bağlı" parametresinin ilgili veri fabrikasının ve depolamabağlantılı hizmetin adını belirtemesini unutmayın. Azure blob depolama veri kümesi türünü tanımlamaya bir örnek aşağıdaki JSON tanımında gösterilmiştir:

```JSON
"type": "datasets",
"name": "[variables('storageDataset')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('storageLinkedServiceName')]"
],
"apiVersion": "2015-10-01",
"properties": {
"type": "AzureBlob",
"linkedServiceName": "[variables('storageLinkedServiceName')]",
"typeProperties": {
    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
    "fileName": "[parameters('sourceBlobName')]",
    "format": {
        "type": "TextFormat"
    }
},
"availability": {
    "frequency": "Hour",
    "interval": 1
}
```

### <a name="define-pipelines"></a>Boru hatlarını tanımlama

```JSON
"type": "dataPipelines",
"name": "[variables('<mypipelineName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<inputDatasetLinkedServiceName>')]",
    "[variables('<outputDatasetLinkedServiceName>')]",
    "[variables('<inputDataset>')]",
    "[variables('<outputDataset>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    activities: {
        ...
    }
}
```

Dağıtmak istediğiniz belirli ardışık hattı ve etkinlikleri tanımlamak için JSON özellikleri hakkında ayrıntılar için [boru hatlarının tanımlanmasına](data-factory-create-pipelines.md#pipeline-json) bakın. "Bağlı" parametresinin veri fabrikasının adını ve ilgili bağlantılı hizmetleri veya veri kümelerini belirtemelerini unutmayın. Azure Blob Depolama'dan Azure SQL Veritabanı'na verileri kopyalayan bir ardışık kaynak örneği aşağıdaki JSON snippet'inde gösterilmiştir:

```JSON
"type": "datapipelines",
"name": "[variables('pipelineName')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('azureStorageLinkedServiceName')]",
    "[variables('azureSqlLinkedServiceName')]",
    "[variables('blobInputDatasetName')]",
    "[variables('sqlOutputDatasetName')]"
],
"apiVersion": "2015-10-01",
"properties": {
    "activities": [
    {
        "name": "CopyFromAzureBlobToAzureSQL",
        "description": "Copy data frm Azure blob to Azure SQL",
        "type": "Copy",
        "inputs": [
            {
                "name": "[variables('blobInputDatasetName')]"
            }
        ],
        "outputs": [
            {
                "name": "[variables('sqlOutputDatasetName')]"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "BlobSource"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
            },
            "translator": {
                "type": "TabularTranslator",
                "columnMappings": "Column0:FirstName,Column1:LastName"
            }
        },
        "Policy": {
            "concurrency": 1,
            "executionPriorityOrder": "NewestFirst",
            "retry": 3,
            "timeout": "01:00:00"
        }
    }
    ],
    "start": "2016-10-03T00:00:00Z",
    "end": "2016-10-04T00:00:00Z"
}
```
## <a name="parameterizing-data-factory-template"></a>Parametreleme Veri Fabrikası şablonu
Parametreoluşturma yla ilgili en iyi uygulamalar [için Azure Kaynak Yöneticisi şablonları oluşturmak için en iyi uygulamalara](../../azure-resource-manager/resource-manager-template-best-practices.md)bakın. Genel olarak, parametre kullanımı en aza indirilmelidir, özellikle de bunun yerine değişkenler kullanılabilse. Yalnızca aşağıdaki senaryolarda parametreler sağlayın:

* Ayarlar ortama göre değişir (örnek: geliştirme, test ve üretim)
* Sırlar (parolalar gibi)

Şablonları kullanarak Azure Veri Fabrikası varlıklarını dağıtırken [Azure Key Vault'tan](../../key-vault/general/overview.md) sırları çekmeniz gerekiyorsa, aşağıdaki örnekte gösterildiği gibi **anahtar kasasını** ve **gizli adı** belirtin:

```JSON
"parameters": {
    "storageAccountKey": {
        "reference": {
            "keyVault": {
                "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
             },
            "secretName": "<secretName>"
           },
       },
       ...
}
```

> [!NOTE]
> Varolan veri fabrikaları için şablonlar dışa aktarma şu anda henüz desteklenmezken, şu anda çalışıyor.
>
>
