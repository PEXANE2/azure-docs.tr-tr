---
title: ServiceNow'dan verileri kopyalama
description: Azure Veri Fabrikası ardışık bir ardışık ardışık ardışık ardışık bir kopyalama etkinliği kullanarak ServiceNow'dan desteklenen lavabo veri depolarına verileri nasıl kopyalayatılayışimditiz öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/01/2019
ms.openlocfilehash: bc48f651a1adb099017e8f47d9fa6bcfa8078fa1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415345"
---
# <a name="copy-data-from-servicenow-using-azure-data-factory"></a>Azure Veri Fabrikasını kullanarak ServiceNow'dan veri kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, ServiceNow'daki verileri kopyalamak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu ServiceNow bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

ServiceNow'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Veri Fabrikası, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, ServiceNow bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

ServiceNow bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği ayarlanmalıdır: **ServiceNow** | Evet |
| endpoint | ServiceNow sunucusunun bitiş noktası`http://<instance>.service-now.com`( ).  | Evet |
| authenticationType | Kullanılacak kimlik doğrulama türü. <br/>İzin verilen değerler şunlardır: **Temel**, **OAuth2** | Evet |
| kullanıcı adı | Temel ve OAuth2 kimlik doğrulaması için ServiceNow sunucusuna bağlanmak için kullanılan kullanıcı adı.  | Evet |
| password | Temel ve OAuth2 kimlik doğrulaması için kullanıcı adına karşılık gelen parola. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet |
| clientId | OAuth2 kimlik doğrulaması için istemci kimliği.  | Hayır |
| clientSecret | OAuth2 kimlik doğrulamaiçin müşteri sırrı. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Hayır |
| useEncryptedEndpoints | Veri kaynağı uç noktalarının HTTPS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| useHostVerification | TLS üzerinden bağlanırken sunucunun sertifikasındaki ana bilgisayar adının sunucunun ana bilgisayar adıile eşleşip eşleşmeyeceğini belirtir. Varsayılan değer true şeklindedir.  | Hayır |
| usePeerVerification | TLS üzerinden bağlanırken sunucunun kimliğinin doğrulanıp doğrulamayacağı belirtilir. Varsayılan değer true şeklindedir.  | Hayır |

**Örnek:**

```json
{
    "name": "ServiceNowLinkedService",
    "properties": {
        "type": "ServiceNow",
        "typeProperties": {
            "endpoint" : "http://<instance>.service-now.com",
            "authenticationType" : "Basic",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde ServiceNow veri kümesi tarafından desteklenen özelliklerin bir listesi sağlar.

ServiceNow'dan veri kopyalamak için, veri kümesinin tür özelliğini **ServiceNowObject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği şu şekilde ayarlanmalıdır: **ServiceNowObject** | Evet |
| tableName | Masanın adı. | Hayır (etkinlik kaynağında "sorgu" belirtilirse) |

**Örnek**

```json
{
    "name": "ServiceNowDataset",
    "properties": {
        "type": "ServiceNowObject",
        "typeProperties": {},
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<ServiceNow linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde ServiceNow kaynağı tarafından desteklenen özelliklerin bir listesi sağlar.

### <a name="servicenow-as-source"></a>Kaynak olarak ServiceNow

ServiceNow'dan veri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **ServiceNowSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği **kaynak** bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının türü özelliği ayarlanmalıdır: **ServiceNowSource** | Evet |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM Actual.alm_asset"`. | Hayır (veri kümesinde "tablo Adı" belirtilirse) |

Sorguda ServiceNow için şema ve sütun belirtirken aşağıdakileri not edin ve **kopyalama performansı imasına ilişkin [Performans ipuçlarına](#performance-tips) bakın.**

- **Şema:** ServiceNow `Actual` [dinlendirici API'leri](https://developer.servicenow.com/app.do#!/rest_api_doc?v=jakarta&id=r_AggregateAPI-GET)ararken doğru veya yanlış `sysparm_display_value` olarak bakabileceğiniz ServiceNow `Display` sorgusunda şema belirtin. 
- **Sütun:** şema `Actual` altında gerçek değeri için `[column name]_value`sütun adı , `Display` şema `[column name]_display_value`altında görüntü değeri için ise . Sütun adının sorguda kullanılan şemaya eşleneihtiyacı olduğunu unutmayın.

**Örnek sorgu:** 
 `SELECT col_value FROM Actual.alm_asset` OR 
`SELECT col_display_value FROM Display.alm_asset`

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromServiceNow",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ServiceNow input dataset name>",
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
                "type": "ServiceNowSource",
                "query": "SELECT * FROM Actual.alm_asset"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```
## <a name="performance-tips"></a>Performans ipuçları

### <a name="schema-to-use"></a>Schema kullanmak için

ServiceNow 2 farklı şema vardır, bir gerçek veri döndürür **"Gerçek",** diğer veri ekran değerlerini döndürür **"Ekran".** 

Sorgunuzda bir filtre varsa, daha iyi kopyalama performansına sahip "Gerçek" şeması kullanın. "Gerçek" şemaya karşı sorgu yaparken ServiceNow, verileri yalnızca filtrelenen sonuç kümesini döndürmek için alırken filtreyi yerel olarak desteklerken, "Görüntüle" şemasını sorgularken ADF tüm verileri alır ve dahili olarak filtre uygular.

### <a name="index"></a>Dizin oluşturma

ServiceNow tablo dizini sorgu performansını artırmaya yardımcı olabilir, [tablo dizini oluştur'a](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/table_administration/task/t_CreateCustomIndex.html)bakın.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
