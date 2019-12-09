---
title: Azure Data Factory (Önizleme) kullanarak QuickBooks Online veri kopyalama
description: Veri QuickBooks Online'dan desteklenen havuz veri depolarına bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
ms.author: jingwang
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/01/2019
ms.openlocfilehash: ca57f4611b9eb483104d8d0b8fc5636726203195
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74927771"
---
# <a name="copy-data-from-quickbooks-online-using-azure-data-factory-preview"></a>Azure Data Factory (Önizleme) kullanarak QuickBooks Online veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de QuickBooks Online'dan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

> [!IMPORTANT]
> Bu bağlayıcı, şu anda Önizleme aşamasındadır. Deneyin ve geri bildirimde bulunun. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu QuickBooks Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Veri QuickBooks Online'dan tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

Şu anda bu bağlayıcı ile 17 Temmuz 2017'den önce oluşturulan uygulamaların bir geliştirici hesabına sahip olması anlamına gelir 1.0a destekleyebilir.

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli QuickBooks bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

QuickBooks bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **QuickBooks** | Yes |
| endpoint | QuickBooks Online sunucu uç noktası. (diğer bir deyişle, quickbooks.api.intuit.com)  | Yes |
| companyId | Yetkilendirmek için QuickBooks şirket şirket kimliği. Şirket kodu bulma hakkında daha fazla bilgi için bkz. [şirket Kimliğimi nasıl bulabilirim?](https://quickbooks.intuit.com/community/Getting-Started/How-do-I-find-my-Company-ID/m-p/185551). | Yes |
| consumerKey | OAuth 1.0 kimlik doğrulaması için tüketici anahtarı. | Yes |
| consumerSecret | OAuth 1.0 kimlik doğrulaması için tüketici gizli bilgisi. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Yes |
| accessToken | OAuth 1.0 kimlik doğrulaması için erişim belirteci. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Yes |
| accessTokenSecret | OAuth 1.0 kimlik doğrulaması için erişim belirteci gizli. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Yes |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "QuickBooksLinkedService",
    "properties": {
        "type": "QuickBooks",
        "typeProperties": {
            "endpoint" : "quickbooks.api.intuit.com",
            "companyId" : "<companyId>",
            "consumerKey": "<consumerKey>",
            "consumerSecret": {
                "type": "SecureString",
                "value": "<consumerSecret>"
            },
            "accessToken": {
                 "type": "SecureString",
                 "value": "<accessToken>"
            },
            "accessTokenSecret": {
                 "type": "SecureString",
                 "value": "<accessTokenSecret>"
            },
            "useEncryptedEndpoints" : true
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, QuickBooks veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

QuickBooks Online'dan veri kopyalamak için dataset öğesinin type özelliği ayarlamak **QuickBooksObject**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **QuickBooksObject** | Yes |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "QuickBooksDataset",
    "properties": {
        "type": "QuickBooksObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<QuickBooks linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, QuickBooks kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="quickbooks-as-source"></a>Kaynak olarak QuickBooks

QuickBooks Online'dan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **QuickBooksSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **source** bölümü:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **QuickBooksSource** | Yes |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM "Bill" WHERE Id = '123'"`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromQuickBooks",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<QuickBooks input dataset name>",
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
                "type": "QuickBooksSource",
                "query": "SELECT * FROM \"Bill\" WHERE Id = '123' "
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="copy-data-from-quickbooks-desktop"></a>Quickbooks masaüstünden veri kopyalama

Azure Data Factory kopyalama etkinliği, doğrudan Quickbooks masaüstünden veri kopyalanamıyor. Quickbooks masaüstünden veri kopyalamak için Quickbooks verilerinizi bir virgülle ayrılmış değerler (CSV) dosyasına dışarı aktarın ve sonra dosyayı Azure Blob depolama alanına yükleyin. Burada, Data Factory, tercih ettiğiniz havuza verileri kopyalamak için kullanabilirsiniz.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
