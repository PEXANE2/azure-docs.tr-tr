---
title: Salesforce Marketing Cloud'dan veri kopyalama
description: Bir Azure Veri Fabrikası ardışık hattında bir kopyalama etkinliği kullanarak Salesforce Marketing Cloud'daki verileri desteklenen lavabo veri depolarına nasıl kopyalayacağınızı öğrenin.
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
ms.date: 10/25/2019
ms.openlocfilehash: a76150756d87b5f2c1b73b8755500e63cdfe05c9
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991748"
---
# <a name="copy-data-from-salesforce-marketing-cloud-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Salesforce Marketing Cloud'dan veri kopyalama

Bu makalede, Salesforce Marketing Cloud'dan verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Salesforce Marketing Cloud konektörü aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Salesforce Marketing Cloud'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Salesforce Marketing Cloud bağlayıcısı OAuth 2 kimlik doğrulamasını destekler. Bu [Salesforce Pazarlama Bulut REST API](https://developer.salesforce.com/docs/atlas.en-us.mc-apis.meta/mc-apis/index-api.htm)üstüne inşa edilmiştir.

>[!NOTE]
>Bu bağlayıcı, özel nesnelerin veya özel veri uzantılarının alınmasını desteklemez.

## <a name="getting-started"></a>Başlarken

.NET SDK, Python SDK, Azure PowerShell, REST API veya Azure Kaynak Yöneticisi şablonunu kullanarak kopyalama etkinliği içeren bir ardışık sistem hattı oluşturabilirsiniz. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](quickstart-create-data-factory-dot-net.md) kopyala'ya bakın.

Aşağıdaki bölümler, Salesforce Marketing Cloud bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Salesforce Marketing Cloud bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **SalesforceMarketingCloud** | Evet |
| clientId | Salesforce Marketing Cloud uygulamasıyla ilişkili istemci kimliği.  | Evet |
| clientSecret | Salesforce Marketing Cloud uygulamasıyla ilişkili istemci sırrı. Bu alanı Güvenli String olarak işaretlemeyi seçip ADF'de güvenli bir şekilde depolamayı veya parolayı Azure Key Vault'ta depolamayı ve ADF'nin veri kopyalama gerçekleştirirken etkinlik çekmesine izin verebilirsiniz - [Key Vault'taki Mağaza kimlik numaralarından](store-credentials-in-key-vault.md)daha fazla bilgi edinin. | Evet |
| useEncryptedEndpoints | Veri kaynağı uç noktalarının HTTPS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| useHostVerification | TLS üzerinden bağlanırken sunucunun sertifikasındaki ana bilgisayar adının sunucunun ana bilgisayar adıile eşleşip eşleşmeyeceğini belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| usePeerVerification | TLS üzerinden bağlanırken sunucunun kimliğinin doğrulanıp doğrulamayacağı belirtilir. Varsayılan değer true şeklindedir.  | Hayır |

**Örnek:**

```json
{
    "name": "SalesforceMarketingCloudLinkedService",
    "properties": {
        "type": "SalesforceMarketingCloud",
        "typeProperties": {
            "clientId" : "<clientId>",
            "clientSecret": {
                 "type": "SecureString",
                 "value": "<clientSecret>"
            },
            "useEncryptedEndpoints" : true,
            "useHostVerification" : true,
            "usePeerVerification" : true
        }
    }
}

```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Salesforce Marketing Cloud veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Salesforce Marketing Cloud'dan veri kopyalamak için, veri kümesinin tür özelliğini **SalesforceMarketingCloudObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **SalesforceMarketingCloudObject** | Evet |
| tableName | Masanın adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "SalesforceMarketingCloudDataset",
    "properties": {
        "type": "SalesforceMarketingCloudObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<SalesforceMarketingCloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Salesforce Marketing Cloud kaynağı tarafından desteklenen özelliklerin bir listesi yer almaktadır.

### <a name="salesforce-marketing-cloud-as-source"></a>Kaynak olarak Salesforce Marketing Cloud

Salesforce Marketing Cloud'dan verileri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **SalesforceMarketingCloudSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **SalesforceMarketingCloudSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM MyTable"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSalesforceMarketingCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SalesforceMarketingCloud input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SalesforceMarketingCloudSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
