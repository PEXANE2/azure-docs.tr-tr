---
title: Azure Data Factory kullanarak müşteri için SAP buluttan verileri Kopyala | Microsoft Docs
description: Müşteri için SAP buluttan verileri desteklenen kaynak veri depolarından (veya), Data Factory kullanarak müşteri için SAP bulutuna kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: jingwang
ms.openlocfilehash: 30025499ae3073a04863d711423bd9556e7fc6c4
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726030"
---
# <a name="copy-data-from-sap-cloud-for-customer-c4c-using-azure-data-factory"></a>Azure Data Factory kullanarak müşteri için SAP bulutlarından (C4C) veri kopyalama

Bu makalede, müşteri için SAP buluttan (C4C) verileri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Müşteri için SAP buluttan verileri desteklenen herhangi bir havuz veri deposuna kopyalayabilir veya desteklenen herhangi bir kaynak veri deposundan verileri, müşteri için SAP bulutuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu bağlayıcı Sales için SAP bulutu, hizmet için SAP bulutu ve sosyal katılım çözümleri için SAP bulutu dahil olmak üzere, müşteri için SAP buluttan veri kopyalamasını Azure Data Factory sağlar.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, müşteri Bağlayıcısı için SAP bulutuna özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Müşteri bağlantılı hizmeti için SAP bulutu için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği şu şekilde ayarlanmalıdır: **Sapcloudforcustomer**. | Evet |
| url | SAP C4C OData hizmetinin URL 'SI. | Evet |
| username | SAP C4C bağlanmak için Kullanıcı adını belirtin. | Evet |
| password | Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet |
| connectVia | [Integration Runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Kaynak, havuz için Evet Hayır |

>[!IMPORTANT]
>Müşteri için SAP bulutuna veri kopyalamak için, müşteri için SAP bulutunuzun yakınında bir konum ile açık [bir Azure IR oluşturun](create-azure-integration-runtime.md#create-azure-ir) ve bağlı hizmette aşağıdaki örnekte ilişkilendirin:

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, müşteri veri kümesi için SAP bulutu tarafından desteklenen özelliklerin bir listesini sağlar.

Müşteri için SAP bulutundaki verileri kopyalamak için veri kümesinin Type özelliğini **Sapcloudforcustomerresource**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği şu şekilde ayarlanmalıdır: **SapCloudForCustomerResource** |Evet |
| path | SAP C4C OData varlığının yolunu belirtin. |Evet |

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

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, müşteri kaynağı için SAP bulutu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sap-c4c-as-source"></a>Kaynak olarak SAP C4C

Müşteri için SAP buluttan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **SapCloudForCustomerSource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği şu şekilde ayarlanmalıdır: **SapCloudForCustomerSource**  | Evet |
| query | Verileri okumak için özel OData sorgusunu belirtin. | Hayır |

Belirli bir güne ait verileri almak için örnek sorgu:`"query": "$filter=CreatedOn ge datetimeoffset'2017-07-31T10:02:06.4202620Z' and CreatedOn le datetimeoffset'2017-08-01T10:02:06.4202620Z'"`

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

### <a name="sap-c4c-as-sink"></a>Havuz olarak SAP C4C

Müşteri için SAP bulutuna veri kopyalamak için kopyalama etkinliğindeki havuz türünü **Sapcloudforcustomersink**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği şu şekilde ayarlanmalıdır: **SapCloudForCustomerSink**  | Evet |
| writeBehavior | İşlemin yazma davranışı. "INSERT", "Update" olabilir. | Hayır. Varsayılan "Ekle". |
| writeBatchSize | Yazma işleminin toplu iş boyutu. En iyi performansı elde etmek için toplu iş boyutu, farklı tablo veya sunucu için farklı olabilir. | Hayır. Varsayılan 10. |

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

## <a name="data-type-mapping-for-sap-cloud-for-customer"></a>Müşteri için SAP bulutu için veri türü eşlemesi

Müşteri için SAP bulutlarından veri kopyalarken aşağıdaki eşlemeler, istemci veri türleri için SAP bulutu 'ndan, geçici veri türlerini Azure Data Factory için kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) eşlemelerini nasıl yapar? kopyalama etkinliği kaynak şema ve veri türü için havuz hakkında bilgi edinmek için.

| SAP C4C OData veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| EDM. Binary | Byte[] |
| Edm.Boolean | Bool |
| EDM. Byte | Byte[] |
| Edm.DateTime | Datetime |
| EDM. Decimal | Decimal |
| Edm.Double | Double |
| EDM. Single | Single |
| EDM. Guid | Guid |
| EDM.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | String |
| EDM.Time | TimeSpan |
| Edm.DateTimeOffset | DateTimeOffset |


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
