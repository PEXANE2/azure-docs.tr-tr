---
title: Azure Data Factory (Önizleme) kullanarak Marketo'dan veri kopyalama
description: Desteklenen bir havuz veri depolarına Marketo'dan bir Azure Data Factory işlem hattında kopyalama etkinliği'ni kullanarak veri kopyalama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: jingwang
ms.openlocfilehash: 74d56d553c4049a98b4401c66b27ae33e31da5c0
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74927128"
---
# <a name="copy-data-from-marketo-using-azure-data-factory-preview"></a>Azure Data Factory (Önizleme) kullanarak Marketo'dan veri kopyalama

Bu makalede, kopyalama etkinliği Azure Data Factory'de Marketo'dan veri kopyalamak için nasıl kullanılacağını özetlenmektedir. Yapılar [kopyalama etkinliği'ne genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

> [!IMPORTANT]
> Bu bağlayıcı, şu anda Önizleme aşamasındadır. Deneyin ve geri bildirimde bulunun. Çözümünüzde bir önizleme bağlayıcısı bağımlılığı olmasını istiyorsanız lütfen [Azure desteğine](https://azure.microsoft.com/support/) başvurun.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Marketo Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Marketo'dan veri tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuz kopyalama etkinliği tarafından desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Azure Data Factory bağlantısını etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücü el ile yüklemeniz gerekmez.

>[!NOTE]
>Bu Marketo bağlayıcı Marketo REST API temelinde oluşturulmuştur. Marketo olduğuna dikkat [eşzamanlı istek sınırı](https://developers.marketo.com/rest-api/) hizmet tarafındaki. Hataları belirten ulaşırsanız "REST API'sini kullanmayı çalışırken hata: en yüksek hızı sınırı '100' ile '20' aşıldı saniye (606)" veya "REST API kullanmaya çalışırken hata: eş zamanlı erişim '10' sınırına (615)", eşzamanlı kopyalama etkinliği çalışır azaltmak için göz önünde bulundurun hizmete istek sayısını azaltın.

## <a name="getting-started"></a>Başlangıç

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Data Factory varlıklarını belirli Marketo bağlayıcıya tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Marketo bağlı hizmeti için aşağıdaki özellikleri destekler:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Marketo** | Yes |
| endpoint | Marketo sunucu uç noktası. (yani 123-ABC-321.mktorest.com)  | Yes |
| clientID | İstemci kimliği Marketo hizmetinizin.  | Yes |
| clientSecret | Marketo hizmetinizin istemci gizli anahtarı. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Yes |
| useEncryptedEndpoints | Veri kaynağı uç noktaları HTTPS kullanılarak şifrelenmiş olup olmadığını belirtir. Varsayılan değer true olur.  | Hayır |
| useHostVerification | Ana bilgisayar adı sunucunun sertifikasında SSL üzerinden bağlanırken sunucu ana bilgisayar adıyla eşleşmesi gerekip gerekmediğini belirtir. Varsayılan değer true olur.  | Hayır |
| usePeerVerification | SSL üzerinden bağlanırken sunucu kimliğinin doğrulanıp doğrulanmayacağını belirtir. Varsayılan değer true olur.  | Hayır |

**Örnek:**

```json
{
    "name": "MarketoLinkedService",
    "properties": {
        "type": "Marketo",
        "typeProperties": {
            "endpoint" : "123-ABC-321.mktorest.com",
            "clientId" : "<clientId>",
            "clientSecret": {
                "type": "SecureString",
                "value": "<clientSecret>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölümde, Marketo veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Marketo'dan veri kopyalamak için dataset öğesinin type özelliği ayarlamak **MarketoObject**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Dataset öğesinin type özelliği ayarlanmalıdır: **MarketoObject** | Yes |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı "query" belirtilmişse) |

**Örnek**

```json
{
    "name": "MarketoDataset",
    "properties": {
        "type": "MarketoObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Marketo linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölümde, Marketo kaynak tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="marketo-as-source"></a>Marketo kaynağı olarak

Marketo'dan veri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **MarketoSource**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **source** bölümü:

| Özellik | Açıklama | Gereklidir |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı öğesinin type özelliği ayarlanmalıdır: **MarketoSource** | Yes |
| sorgu | Verileri okumak için özel bir SQL sorgusu kullanın. Örneğin: `"SELECT * FROM Activitiy_Types"`. | Yok (veri kümesinde "tableName" değeri belirtilmişse) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromMarketo",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Marketo input dataset name>",
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
                "type": "MarketoSource",
                "query": "SELECT top 1000 * FROM Activitiy_Types"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
