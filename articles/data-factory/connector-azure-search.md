---
title: Azure Data Factory kullanarak arama dizinine veri kopyalama | Microsoft Docs
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak bir Azure Search dizinine veri gönderme veya kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: edf475ac11168c33a6b11ccda3482ac44579e8d8
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726214"
---
# <a name="copy-data-to-an-azure-search-index-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Search dizine veri kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-search-connector.md)
> * [Geçerli sürüm](connector-azure-search.md)

Bu makalede, verileri Azure Search dizine kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Desteklenen herhangi bir kaynak veri deposundan verileri Azure Search dizine kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler Azure Search bağlayıcıya özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Azure Search bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği şu şekilde ayarlanmalıdır: **AzureSearch** | Evet |
| url | Azure Search hizmeti URL 'SI. | Evet |
| key | Azure Search hizmeti için yönetici anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel ağında bulunuyorsa), Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. |Hayır |

> [!IMPORTANT]
> Verileri bir bulut veri deposundan Azure Search dizinine kopyalarken, Azure Search bağlı hizmette, Connactto aracılığıyla açık bölge ile bir Azure Integration Runtime başvurmanız gerekir. Bölgeyi Azure Search yer aldığı bir şekilde ayarlayın. [Azure Integration Runtime](concepts-integration-runtime.md#azure-integration-runtime)hakkında daha fazla bilgi edinin.

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölüm Azure Search veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Azure Search verileri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **AzureSearchIndex** | Evet |
| indexName | Azure Search dizininin adı. Data Factory dizini oluşturmaz. Dizinin Azure Search olması gerekir. | Evet |

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
            "referenceName": "<Azure Search linked service name>",
            "type": "LinkedServiceReference"
        }
   }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm Azure Search kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-search-as-sink"></a>Havuz olarak Azure Search

Azure Search içine veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **AzureSearchIndexSink**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği şu şekilde ayarlanmalıdır: **AzureSearchIndexSink** | Evet |
| writeBehavior | Dizinde bir belgenin zaten mevcut olduğu zaman birleştirilip birleştirilmeyeceğini veya değiştirip edilmeyeceğini belirtir. Bkz. [Writebehavior özelliği](#writebehavior-property).<br/><br/>İzin verilen değerler şunlardır: **Birleştirme** (varsayılan) ve **karşıya yükleyin**. | Hayır |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında verileri Azure Search dizinine yükler. Ayrıntılar için bkz. [Writebatchsize özelliği](#writebatchsize-property) .<br/><br/>İzin verilen değerler: tamsayı 1 ile 1.000 arasında; Varsayılan değer 1000 ' dir. | Hayır |

### <a name="writebehavior-property"></a>WriteBehavior özelliği

Veri yazarken AzureSearchSink upları. Diğer bir deyişle, bir belge yazarken belge anahtarı Azure Search dizininde zaten mevcutsa, bir çakışma özel durumu oluşturmak yerine mevcut belgeyi günceller Azure Search.

AzureSearchSink aşağıdaki iki yukarı yönlü davranışı sağlar (AzureSearch SDK kullanarak):

- **Birleştir**: yeni belgedeki tüm sütunları mevcut bir belge ile birleştirin. Yeni belgede null değeri olan sütunlarda, var olan bir değer korunur.
- **Karşıya yükle**: Yeni belge, var olan birinin yerini alır. Yeni belgede belirtilmeyen sütunlar için, varolan belgede null olmayan bir değer olup olmadığı için değer null olarak ayarlanır.

Varsayılan davranış **birleştirilir**.

### <a name="writebatchsize-property"></a>WriteBatchSize özelliği

Azure Search hizmet, toplu iş olarak belge yazmayı destekler. Batch, 1 ile 1.000 arasında eylem içerebilir. Bir eylem, karşıya yükleme/birleştirme işlemini gerçekleştirmek için bir belgeyi işler.

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
                "referenceName": "<Azure Search output dataset name>",
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

### <a name="data-type-support"></a>Veri türü desteği

Aşağıdaki tablo Azure Search veri türünün desteklenip desteklenmediğini belirtir.

| Azure Search veri türü | Azure Search havuzunda desteklenir |
| ---------------------- | ------------------------------ |
| Dize | E |
| Int32 | E |
| Int64 | E |
| Double | E |
| Boolean | E |
| Veri Timesapmayı | E |
| String Array | N |
| GeographyPoint | N |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md##supported-data-stores-and-formats).
