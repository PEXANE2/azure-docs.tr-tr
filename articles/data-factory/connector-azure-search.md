---
title: Verileri arama dizinine Kopyala
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak bir Azure Search dizinine veri gönderme veya kopyalama hakkında bilgi edinin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/13/2019
ms.openlocfilehash: 418026d5569cd7e4a7c5239f99650833b1b9514d
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75892932"
---
# <a name="copy-data-to-an-azure-cognitive-search-index-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Bilişsel Arama dizinine veri kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-search-connector.md)
> * [Geçerli sürüm](connector-azure-search.md)

Bu makalede, Azure Bilişsel Arama dizinine veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Desteklenen herhangi bir kaynak veri deposundan verileri arama dizinine kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure Bilişsel Arama Connector 'a özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Bilişsel Arama bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği: **Azuresearch** olarak ayarlanmalıdır | Evet |
| url | Arama Hizmeti URL 'SI. | Evet |
| anahtar | Arama hizmeti için yönetici anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

> [!IMPORTANT]
> Verileri bir bulut veri deposundan arama dizinine kopyalarken, Azure Bilişsel Arama bağlı hizmetinde, Connactto aracılığıyla açık bölge ile bir Azure Integration Runtime başvurmanız gerekir. Bölgeyi, arama hizmetinizin bulunduğu konum olarak ayarlayın. [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime)hakkında daha fazla bilgi edinin.

**Örnek:**

```json
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": {
                "type": "SecureString",
                "value": "<AdminKey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Azure Bilişsel Arama veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Verileri Azure Bilişsel Arama 'e kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | DataSet 'in Type özelliği: **AzureSearchIndex** olarak ayarlanmalıdır | Evet |
| indexName | Arama dizininin adı. Data Factory dizini oluşturmaz. Dizinin Azure Bilişsel Arama mevcut olması gerekir. | Evet |

**Örnek:**

```json
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "typeProperties" : {
            "indexName": "products"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Cognitive Search linked service name>",
            "type": "LinkedServiceReference"
        }
   }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Azure Bilişsel Arama kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-cognitive-search-as-sink"></a>Havuz olarak Azure Bilişsel Arama

Verileri Azure Bilişsel Arama 'e kopyalamak için kopyalama etkinliğindeki kaynak türünü **AzureSearchIndexSink**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği: **AzureSearchIndexSink** olarak ayarlanmalıdır | Evet |
| writeBehavior | Dizinde bir belgenin zaten mevcut olduğu zaman birleştirilip birleştirilmeyeceğini veya değiştirip edilmeyeceğini belirtir. Bkz. [Writebehavior özelliği](#writebehavior-property).<br/><br/>İzin verilen değerler şunlardır: **birleştirme** (varsayılan) ve **karşıya yükleme**. | Hayır |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında verileri arama dizinine yükler. Ayrıntılar için bkz. [Writebatchsize özelliği](#writebatchsize-property) .<br/><br/>İzin verilen değerler: tamsayı 1 ile 1.000 arasında; Varsayılan değer 1000 ' dir. | Hayır |

### <a name="writebehavior-property"></a>WriteBehavior özelliği

Veri yazarken AzureSearchSink upları. Diğer bir deyişle, bir belge yazarken belge anahtarı arama dizininde zaten mevcutsa Azure Bilişsel Arama bir çakışma özel durumu oluşturmak yerine mevcut belgeyi güncelleştirir.

AzureSearchSink aşağıdaki iki yukarı yönlü davranışı sağlar (AzureSearch SDK kullanarak):

- **Birleştir**: yeni belgedeki tüm sütunları mevcut bir belge ile birleştirin. Yeni belgede null değeri olan sütunlarda, var olan bir değer korunur.
- **Karşıya yükle**: yeni belge var olanı yenisiyle değiştirir. Yeni belgede belirtilmeyen sütunlar için, varolan belgede null olmayan bir değer olup olmadığı için değer null olarak ayarlanır.

Varsayılan davranış **birleştirilir**.

### <a name="writebatchsize-property"></a>WriteBatchSize özelliği

Azure Bilişsel Arama hizmeti, toplu iş olarak belge yazmayı destekler. Batch, 1 ile 1.000 arasında eylem içerebilir. Bir eylem, karşıya yükleme/birleştirme işlemini gerçekleştirmek için bir belgeyi işler.

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToAzureSearch",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Cognitive Search output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSearchIndexSink",
                "writeBehavior": "Merge"
            }
        }
    }
]
```

## <a name="data-type-support"></a>Veri türü desteği

Aşağıdaki tabloda bir Azure Bilişsel Arama veri türünün desteklenip desteklenmediğini belirtir.

| Azure Bilişsel Arama veri türü | Azure Bilişsel Arama havuzunda desteklenir |
| ---------------------- | ------------------------------ |
| Dize | E |
| Int32 | E |
| Int64 | E |
| Double | E |
| Boole | E |
| Veri Timesapmayı | E |
| String Array | N |
| GeographyPoint | N |

Şu anda diğer veri türleri, yani ComplexType desteklenmez. Desteklenen Azure Bilişsel Arama veri türlerinin tam listesi için bkz. [desteklenen veri türleri (Azure bilişsel arama)](https://docs.microsoft.com/rest/api/searchservice/supported-data-types).

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
