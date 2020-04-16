---
title: Verileri Arama dizinine kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık ardışık yapıda Kopyalama Etkinliği'ni kullanarak verileri Azure arama dizinine nasıl itinveya kopyalayamahakkında bilgi edinin.
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
ms.openlocfilehash: dfa1ad318ccc9e891b646ec050f6a0776e108206
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418244"
---
# <a name="copy-data-to-an-azure-cognitive-search-index-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Azure Bilişsel Arama dizinine kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-search-connector.md)
> * [Geçerli sürüm](connector-azure-search.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, verileri Azure Bilişsel Arama dizinine kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Desteklenen herhangi bir kaynak veri deposundan gelen verileri arama dizinine kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure Bilişsel Arama bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Azure Bilişsel Arama bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **AzureSearch** | Evet |
| url | Arama hizmetinin URL'si. | Evet |
| anahtar | Arama hizmeti için yönetici anahtarı. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. |Hayır |

> [!IMPORTANT]
> Azure Bilişsel Arama bağlantılı bir hizmette bulut veri deposundan arama dizinine veri kopyalarken connactVia'da açık bölgeiçeren bir Azure Tümleştirme Çalışma Süresi'ne başvurmanız gerekir. Bölgeyi arama hizmetinizin bulunduğu bölge olarak ayarlayın. [Azure Tümleştirme Runtime'dan](concepts-integration-runtime.md#azure-integration-runtime)daha fazla bilgi edinin.

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Azure Bilişsel Arama veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Verileri Azure Bilişsel Arama'ya kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **AzureSearchIndex** | Evet |
| indexName | Arama dizininin adı. Veri Fabrikası dizin oluşturmaz. Dizin Azure Bilişsel Arama'da bulunmalıdır. | Evet |

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Azure Bilişsel Arama kaynağı tarafından desteklenen özelliklerin bir listesi sağlanmaktadır.

### <a name="azure-cognitive-search-as-sink"></a>Lavabo Olarak Azure Bilişsel Arama

Verileri Azure Bilişsel Arama'ya kopyalamak için, kopyalama etkinliğindeki kaynak türünü **AzureSearchIndexSink**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **lavabo** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği şu şekilde ayarlanmalıdır: **AzureSearchIndexSink** | Evet |
| yazmaDavranışı | Diziniçinde zaten bir belge varken birleşip değiştirilmeyeceğini belirtir. [WriteBehavior özelliğine](#writebehavior-property)bakın.<br/><br/>İzin verilen değerler şunlardır: **Birleştirme** (varsayılan) ve **Yükle**. | Hayır |
| yazmaBatchSize | Arabellek boyutu writeBatchSize'a ulaştığında verileri arama dizinine yükler. Ayrıntılar için [WriteBatchSize özelliğine](#writebatchsize-property) bakın.<br/><br/>İzin verilen değerler şunlardır: 1 ile 1.000 arasında bir insa; varsayılan değer 1000'dir. | Hayır |

### <a name="writebehavior-property"></a>WriteBehavior özelliği

AzureSearchSink veri yazarken ekler. Başka bir deyişle, belge yazarken, belge anahtarı arama dizininde zaten varsa, Azure Bilişsel Arama çakışma özel durumu oluşturmak yerine varolan belgeyi güncelleştirir.

AzureSearchSink aşağıdaki iki yükseltme davranışı sağlar (AzureSearch SDK kullanarak):

- **Birleştirme**: Yeni belgedeki tüm sütunları varolan sütunla birleştirin. Yeni belgede null değeri olan sütunlar için, varolan sütundaki değer korunur.
- **Upload**: Yeni belge varolanBelgenin yerini alır. Yeni belgede belirtilmeyen sütunlar için, değer varolan belgede null olmayan bir değer olup olmadığını geçersiz kılacak şekilde ayarlanır.

Varsayılan davranış **Birleştirme'dir.**

### <a name="writebatchsize-property"></a>WriteBatchSize Özelliği

Azure Bilişsel Arama hizmeti, belge toplu olarak yazmayı destekler. Bir toplu iş 1 ila 1.000 Eylem içerebilir. Eylem, yükleme/birleştirme işlemini gerçekleştirmek için bir belgeyi işler.

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

Aşağıdaki tabloda, Azure Bilişsel Arama veri türünün desteklenip desteklenmediğini belirtin.

| Azure Bilişsel Arama veri türü | Azure Bilişsel Arama Lavabosu'nda desteklendi |
| ---------------------- | ------------------------------ |
| Dize | E |
| Int32 | E |
| Int64 | E |
| Çift | E |
| Boole | E |
| DataTimeOffset | E |
| Dize Dizisi | N |
| Coğrafya Noktası | N |

Şu anda ComplexType gibi diğer veri türleri desteklenmez. Azure Bilişsel Arama destekli veri türlerinin tam listesi için desteklenen [veri türlerine (Azure Bilişsel Arama)](https://docs.microsoft.com/rest/api/searchservice/supported-data-types)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
