---
title: ServiceNow 'dan veri kopyalama
description: Azure Data Factory bir işlem hattındaki kopyalama etkinliğini kullanarak ServiceNow 'dan desteklenen havuz veri depolarına veri kopyalamayı öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81415345"
---
# <a name="copy-data-from-servicenow-using-azure-data-factory"></a>Azure Data Factory kullanarak ServiceNow 'dan veri kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, ServiceNow 'dan veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu ServiceNow Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

ServiceNow 'dan, desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak/havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Azure Data Factory, bağlantıyı etkinleştirmek için yerleşik bir sürücü sağlar, bu nedenle bu bağlayıcıyı kullanarak herhangi bir sürücüyü el ile yüklemeniz gerekmez.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler ServiceNow bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

ServiceNow bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği: **ServiceNow** olarak ayarlanmalıdır | Yes |
| endpoint | ServiceNow sunucusu 'nun () uç noktası `http://<instance>.service-now.com` .  | Yes |
| authenticationType | Kullanılacak kimlik doğrulaması türü. <br/>İzin verilen değerler: **Basic**, **OAuth2** | Yes |
| kullanıcı adı | Temel ve OAuth2 kimlik doğrulaması için ServiceNow sunucusuna bağlanmak için kullanılan Kullanıcı adı.  | Yes |
| password | Temel ve OAuth2 kimlik doğrulaması için Kullanıcı adına karşılık gelen parola. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Yes |
| clientId | OAuth2 kimlik doğrulaması için istemci KIMLIĞI.  | No |
| clientSecret | OAuth2 kimlik doğrulaması için istemci parolası. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | No |
| useEncryptedEndpoints | Veri kaynağı uç noktalarının HTTPS kullanılarak şifrelenip şifrelenmediğini belirtir. Varsayılan değer true şeklindedir.  | No |
| Usehostdoğrulaması | Sunucu sertifikasında, TLS üzerinden bağlanırken sunucunun ana bilgisayar adıyla eşleşecek şekilde, ana bilgisayar adının istenip istenmeyeceğini belirtir. Varsayılan değer true şeklindedir.  | No |
| Usepeerdoğrulaması | TLS üzerinden bağlanılırken sunucu kimliğinin doğrulanıp doğrulanmayacağını belirtir. Varsayılan değer true şeklindedir.  | No |

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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm ServiceNow veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

ServiceNow 'dan verileri kopyalamak için, veri kümesinin Type özelliğini **Servicenowobject**olarak ayarlayın. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | DataSet 'in Type özelliği: **Servicenowobject** olarak ayarlanmalıdır | Yes |
| tableName | Tablonun adı. | Hayır (etkinlik kaynağı içinde "sorgu" belirtilmişse) |

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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, ServiceNow kaynağı tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="servicenow-as-source"></a>Kaynak olarak ServiceNow

ServiceNow 'dan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **Servicenowsource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının Type özelliği: **Servicenowsource** olarak ayarlanmalıdır | Yes |
| sorgu | Verileri okumak için özel SQL sorgusunu kullanın. Örneğin: `"SELECT * FROM Actual.alm_asset"`. | Hayır (veri kümesinde "tableName" belirtilmişse) |

Sorgudaki ServiceNow için şemayı ve sütunu belirtirken aşağıdaki gibi bir yere göz önünde, **kopyalama performansı ile Ilgili [performans ipuçlarına](#performance-tips) bakın**.

- **Şema:** `Actual` `Display` dosyayı ServiceNow sorgusunda veya olarak belirtin. Bu, bunu doğru olarak parametre olarak veya `sysparm_display_value` [ServiceNow restalow API 'lerini](https://developer.servicenow.com/app.do#!/rest_api_doc?v=jakarta&id=r_AggregateAPI-GET)çağırırken false olarak görebilirsiniz. 
- **Sütun:** şema altındaki gerçek değer için sütun adı `Actual` `[column name]_value` , şema altındaki görüntü değeri ise `Display` olur `[column name]_display_value` . Sütun adının sorguda kullanılan şemaya eşlenmesi gerektiğini aklınızda olacak şekilde ayarlayın.

**Örnek sorgu:** 
 `SELECT col_value FROM Actual.alm_asset` VEYA 
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

### <a name="schema-to-use"></a>Kullanılacak şema

ServiceNow 2 farklı şemaya sahiptir, biri gerçek verileri döndüren **"gerçek"** , diğeri ise veri görüntüleme değerlerini döndüren **"görüntü"** olur. 

Sorgunuzda bir filtreniz varsa, daha iyi kopyalama performansı olan "gerçek" şemayı kullanın. "Gerçek" şemaya göre sorgulama yaparken ServiceNow, verileri yalnızca filtrelenmiş sonuç kümesi döndürecek şekilde alırken filtreyi yerel olarak destekler, ancak "Display" şeması sorgulanırken, ADF tüm verileri alın ve filtreyi dahili olarak uygulayın.

### <a name="index"></a>Dizin oluşturma

ServiceNow tablo dizini sorgu performansının artırılmasına yardımcı olabilir ve [Tablo dizini oluşturma](https://docs.servicenow.com/bundle/geneva-servicenow-platform/page/administer/table_administration/task/t_CreateCustomIndex.html)bölümüne bakın.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
