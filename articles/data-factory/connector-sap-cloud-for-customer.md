---
title: Müşteri için SAP Bulut'tan/SAP Bulut'a veri kopyalama
description: Desteklenen kaynak veri depolarından desteklenen lavabo veri depolarına (veya) Müşteri için SAP Bulut'a veri fabrikasını kullanarak verileri nasıl kopyalaylaydestekleyeceğini öğrenin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/02/2019
ms.openlocfilehash: 1d3772a17d0429d9b3a5bf95d2060f2dfbbbafe1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418057"
---
# <a name="copy-data-from-sap-cloud-for-customer-c4c-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak MÜŞTERI için SAP Bulutu'ndan (C4C) veri kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Müşteri için SAP Bulut'tan (C4C) verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

>[!TIP]
>Sap veri tümleştirme senaryosunda ADF'nin genel desteğini öğrenmek için, ayrıntılı giriş, karşılaştırma ve kılavuzlu [Azure Veri Fabrikası teknik incelemesini kullanarak SAP veri tümleştirmesine](https://github.com/Azure/Azure-DataFactory/blob/master/whitepaper/SAP%20Data%20Integration%20using%20Azure%20Data%20Factory.pdf) bakın.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Müşteri için bu SAP Bulutu aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Müşteri için SAP Bulutu'ndan desteklenen herhangi bir lavabo veri deposuna verileri kopyalayabilir veya desteklenen herhangi bir kaynak veri deposundan Müşteri için SAP Bulut'a veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle bu bağlayıcı, Azure Veri Fabrikası'nın Müşteriler için SAP Bulutu'ndan/adresinden, Sap Bulutu'nun Satış için Bulutu, Hizmet için SAP Bulutu ve Sosyal Etkileşim çözümleri için SAP Cloud'u kopyalamasına olanak tanır.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Müşteri bağlayıcısı için SAP Bulutuna özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Müşteri bağlantılı hizmetler için SAP Cloud için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **SapCloudForCustomer**. | Evet |
| url | SAP C4C OData hizmetinin URL'si. | Evet |
| kullanıcı adı | SAP C4C'ye bağlanmak için kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Kaynak için hayır, lavabo için Evet |

>[!IMPORTANT]
>Verileri Müşteri için SAP Bulutu'na kopyalamak için, müşteri için SAP Cloud'unuzun yakınında bir konuma sahip [bir Azure IR'si açıkça oluşturun](create-azure-integration-runtime.md#create-azure-ir) ve bağlantılı hizmeti aşağıdaki örnek olarak ilişkilendirin:

**Örnek:**

```json
{
    "name": "SAPC4CLinkedService",
    "properties": {
        "type": "SapCloudForCustomer",
        "typeProperties": {
            "url": "https://<tenantname>.crm.ondemand.com/sap/c4c/odata/v1/c4codata/" ,
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, Müşteri veri kümesi için SAP Cloud tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Müşteri için SAP Cloud'daki verileri kopyalamak için, veri kümesinin tür özelliğini **SapCloudForCustomerResource**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **SapCloudForCustomerResource** |Evet |
| yol | SAP C4C OData varlığının yolunu belirtin. |Evet |

**Örnek:**

```json
{
    "name": "SAPC4CDataset",
    "properties": {
        "type": "SapCloudForCustomerResource",
        "typeProperties": {
            "path": "<path e.g. LeadCollection>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SAP C4C linked service>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Müşteri kaynağı için SAP Cloud tarafından desteklenen özelliklerin bir listesi yer almaktadır.

### <a name="sap-c4c-as-source"></a>KAYNAK OLARAK SAP C4C

Müşteri için SAP Cloud'daki verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **SapCloudForCustomerSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **SapCloudForCustomerSource**  | Evet |
| sorgu | Verileri okumak için özel OData sorgusunu belirtin. | Hayır |

Belirli bir gün için veri almak için örnek sorgu:`"query": "$filter=CreatedOn ge datetimeoffset'2017-07-31T10:02:06.4202620Z' and CreatedOn le datetimeoffset'2017-08-01T10:02:06.4202620Z'"`

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSAPC4C",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP C4C input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SapCloudForCustomerSource",
                "query": "<custom query e.g. $top=10>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sap-c4c-as-sink"></a>SAP C4C lavabo olarak

Müşteri için SAP Cloud'a veri kopyalamak için, kopyalama etkinliğindeki lavabo türünü **SapCloudForCustomerSink**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **lavabo** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **SapCloudForCustomerSink**  | Evet |
| yazmaDavranışı | İşlemin yazma davranışı. "Ekle", "Güncelleştirme" olabilir. | Hayır. Varsayılan "Ekle". |
| yazmaBatchSize | Yazma işleminin toplu boyutu. En iyi performansı elde etmek için toplu iş boyutu farklı tablo veya sunucu için farklı olabilir. | Hayır. Varsayılan 10. |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToSapC4c",
        "type": "Copy",
        "inputs": [{
            "type": "DatasetReference",
            "referenceName": "<dataset type>"
        }],
        "outputs": [{
            "type": "DatasetReference",
            "referenceName": "SapC4cDataset"
        }],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SapCloudForCustomerSink",
                "writeBehavior": "Insert",
                "writeBatchSize": 30
            },
            "parallelCopies": 10,
            "dataIntegrationUnits": 4,
            "enableSkipIncompatibleRow": true,
            "redirectIncompatibleRowSettings": {
                "linkedServiceName": {
                    "referenceName": "ErrorLogBlobLinkedService",
                    "type": "LinkedServiceReference"
                },
                "path": "incompatiblerows"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-cloud-for-customer"></a>Müşteri için SAP Bulutu için veri türü eşleme

Müşteri için SAP Cloud'dan veri kopyalanırken, müşteri veri türleri için SAP Cloud'dan Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopya etkinliği kaynak şemasını ve veri türünü lavaboyla nasıl eşler hakkında bilgi edinmek için Şema ve [veri türü eşlemelerine](copy-activity-schema-and-type-mapping.md) bakın.

| SAP C4C OData Veri Türü | Veri fabrikası geçici veri türü |
|:--- |:--- |
| Edm.İkili | Bayt[] |
| Edm.Boolean | Bool |
| Edm.Bayt | Bayt[] |
| Edm.DateTime | DateTime |
| Edm.Ondalık | Ondalık |
| Edm.Double | Çift |
| Edm.Single | Tek |
| Edm.Guid | Guid |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Dize |
| Edm.Zaman | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |


## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
