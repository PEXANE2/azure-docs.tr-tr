---
title: Azure Data Factory kullanarak verileri Salesforce hizmeti bulutlarından kopyalama | Microsoft Docs
description: Veri Fabrikası ardışık düzeninde kopyalama etkinliği kullanarak Salesforce hizmeti bulutlarından desteklenen havuz veri depolarına veya desteklenen kaynak veri depolarından verileri Salesforce hizmeti bulutuna kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: 729ea0fa667a11f710fd815003bc0995cb08ae70
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68842562"
---
# <a name="copy-data-from-and-to-salesforce-service-cloud-by-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri Salesforce hizmeti bulutlarından kopyalama

Bu makalede, verileri Salesforce hizmeti bulutlarından kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Yapılar [kopyalama etkinliğine genel bakış](copy-activity-overview.md) kopyalama etkinliği genel bir bakış sunan makalesi.

## <a name="supported-capabilities"></a>Desteklenen özellikler

Salesforce hizmeti bulutundaki verileri desteklenen herhangi bir havuz veri deposuna kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundan verileri Salesforce hizmeti bulutuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Salesforce hizmeti bulutu Bağlayıcısı şunları destekler:

- Salesforce geliştiricisi, Professional, Enterprise veya sınırsız sürümler.
- Ve Salesforce üretimden, korumalı alana ve özel etki alanından veri kopyalama.

Salesforce hizmeti bulut bağlayıcısı, Salesforce REST/toplu API 'nin üzerine kurulmuştur. Bu, verileri kopyalamak için [V45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) ve V40 veri kopyalama için [](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) .

## <a name="prerequisites"></a>Önkoşullar

Salesforce 'ta API izninin etkinleştirilmiş olması gerekir. Daha fazla bilgi için bkz. [izin kümesine göre Salesforce 'TA API erişimini etkinleştirme](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları

Salesforce, hem toplam API istekleri hem de eşzamanlı API istekleri için sınırlara sahiptir. Aşağıdaki noktalara dikkat edin:

- Eşzamanlı isteklerin sayısı sınırı aşarsa, kısıtlama gerçekleşir ve rastgele sorunlar görürsünüz.
- Toplam istek sayısı sınırı aşarsa, Salesforce hesabı 24 saat için engellenir.

Ayrıca, her iki senaryoda da "REQUEST_LIMIT_EXCEEDED" hata iletisini alabilirsiniz. Daha fazla bilgi için [Salesforce geliştirici limitlerinin](https://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf)"API isteği sınırları" bölümüne bakın.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler Salesforce hizmeti bulut bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Salesforce bağlantılı hizmeti için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği **SalesforceServiceCloud**olarak ayarlanmalıdır. |Evet |
| environmentUrl | Salesforce hizmeti bulut örneğinin URL 'sini belirtin. <br> -Varsayılan değer `"https://login.salesforce.com"`. <br> -Korumalı verileri veri kopyalamak için belirtin `"https://test.salesforce.com"`. <br> -Özel etki alanından veri kopyalamak için, örneğin, `"https://[domain].my.salesforce.com"`. |Hayır |
| username |Kullanıcı hesabı için bir Kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin.<br/><br/>Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| Belirtilmedi |Kullanıcı hesabı için bir güvenlik belirteci belirtin. Bir güvenlik belirtecini sıfırlama ve alma hakkında yönergeler için bkz. [güvenlik belirteci alma](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Genel olarak güvenlik belirteçleri hakkında daha fazla bilgi edinmek için bkz. [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). |Evet |
| connectVia | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Kaynak için Hayır, kaynak bağlı hizmette tümleştirme çalışma zamanı yoksa, havuz için Evet |

>[!IMPORTANT]
>Verileri Salesforce hizmeti bulutuna kopyaladığınızda, varsayılan Azure Integration Runtime kopyayı yürütmek için kullanılamaz. Diğer bir deyişle, kaynak bağlı hizmetinizin belirtilen bir tümleştirme çalışma zamanı yoksa Salesforce hizmeti bulut örneğinizin yakınında bir konum ile açık [bir Azure Integration Runtime oluşturun](create-azure-integration-runtime.md#create-azure-ir) . Salesforce hizmeti bulutu bağlı hizmetini aşağıdaki örnekte olduğu gibi ilişkilendirin.

**Örnek: Data Factory kimlik bilgilerini depolama**

```json
{
    "name": "SalesforceServiceCloudLinkedService",
    "properties": {
        "type": "SalesforceServiceCloud",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Key Vault kimlik bilgilerini depolama**

```json
{
    "name": "SalesforceServiceCloudLinkedService",
    "properties": {
        "type": "SalesforceServiceCloud",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Salesforce hizmeti bulutu veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Ve Salesforce hizmeti bulutuna veri kopyalamak için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Type özelliği **SalesforceServiceCloudObject**olarak ayarlanmalıdır.  | Evet |
| objectApiName | Verilerin alınması için Salesforce nesne adı. | Kaynak, havuz için Evet Hayır |

> [!IMPORTANT]
> **API adının** "__C" kısmı herhangi bir özel nesne için gereklidir.

![Data Factory Salesforce bağlantısı API 'SI adı](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Örnek:**

```json
{
    "name": "SalesforceServiceCloudDataset",
    "properties": {
        "type": "SalesforceServiceCloudObject",
        "typeProperties": {
            "objectApiName": "MyTable__c"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Salesforce Service Cloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Veri kümesinin Type özelliği **Relationaltable**olarak ayarlanmalıdır. | Evet |
| tableName | Salesforce hizmeti bulutundaki tablonun adı. | Hayır (etkinlik kaynağında "sorgu" belirtilmişse) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Salesforce hizmeti bulut kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="salesforce-service-cloud-as-a-source-type"></a>Kaynak türü olarak Salesforce hizmeti bulutu

Salesforce hizmeti bulutundaki verileri kopyalamak için, etkinlik **kaynağını** kopyalama bölümünde aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Kopyalama etkinliği kaynağının Type özelliği **SalesforceServiceCloudSource**olarak ayarlanmalıdır. | Evet |
| query |Verileri okumak için özel sorguyu kullanın. [Salesforce nesne sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) SORGUSUNU veya SQL-92 sorgusunu kullanabilirsiniz. [Sorgu ipuçları](#query-tips) bölümünde daha fazla ipucu görüntüleyin. Sorgu belirtilmemişse, veri kümesindeki "objectApiName" içinde belirtilen Salesforce hizmeti bulut nesnesinin tüm verileri alınır. | Hayır (veri kümesindeki "objectApiName" belirtilmişse) |
| readBehavior | Mevcut kayıtların sorgulanıp sorgulanmayacağını veya silinen kayıtlar dahil olmak üzere tüm kayıtları sorganıp sorgulanmayacağını gösterir. Belirtilmemişse, varsayılan davranış eski ' dir. <br>İzin verilen değerler: **sorgu** (varsayılan), **queryall**.  | Hayır |

> [!IMPORTANT]
> **API adının** "__C" kısmı herhangi bir özel nesne için gereklidir.

![Data Factory Salesforce bağlantısı API 'SI ad listesi](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromSalesforceServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce Service Cloud input dataset name>",
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
                "type": "SalesforceServiceCloudSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="salesforce-service-cloud-as-a-sink-type"></a>Havuz türü olarak Salesforce hizmeti bulutu

Verileri Salesforce hizmeti bulutuna kopyalamak için, kopyalama etkinliği **havuzu** bölümünde aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| türü | Kopyalama etkinliği havuzunun Type özelliği **SalesforceServiceCloudSink**olarak ayarlanmalıdır. | Evet |
| writeBehavior | İşlem için yazma davranışı.<br/>İzin verilen değerler **Insert** ve **upsert**. | Hayır (varsayılan değer ekler) |
| externalIdFieldName | Upsert işlem için dış KIMLIK alanının adı. Belirtilen alanın Salesforce hizmeti bulut nesnesinde "dış kimlik alanı" olarak tanımlanması gerekir. Karşılık gelen giriş verilerinde NULL değer bulunamaz. | "Upsert" için Evet |
| writeBatchSize | Her toplu işte Salesforce hizmeti bulutuna yazılan verilerin satır sayısı. | Hayır (varsayılan değer 5.000) |
| ıgnorenullvalues | Bir yazma işlemi sırasında giriş verilerinden NULL değerlerin yoksayılıp yoksayılmayacağını gösterir.<br/>İzin verilen değerler **true** ve **false**şeklindedir.<br>- **Doğru**: Bir upsert veya Update işlemi gerçekleştirdiğinizde verileri hedef nesnede değiştirmeden bırakın. Ekleme işlemi yaparken tanımlanmış bir varsayılan değer ekleyin.<br/>- **Yanlış**: Bir yukarı veya güncelleştirme işlemi gerçekleştirdiğinizde hedef nesnesindeki verileri NULL olarak güncelleştirin. Ekleme işlemi yaparken NULL değer ekleyin. | Hayır (varsayılan değer false) |

**Örnek:**

```json
"activities":[
    {
        "name": "CopyToSalesforceServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce Service Cloud output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceServiceCloudSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Sorgu ipuçları

### <a name="retrieve-data-from-a-salesforce-service-cloud-report"></a>Salesforce hizmeti bulutu raporundan veri alma

Bir sorgu `{call "<report name>"}`belirterek Salesforce hizmeti bulut raporlarından veri alabilirsiniz. `"query": "{call \"TestReport\"}"` bunun bir örneğidir.

### <a name="retrieve-deleted-records-from-the-salesforce-service-cloud-recycle-bin"></a>Salesforce hizmeti bulutu geri dönüşüm kutusu 'ndan silinen kayıtları alma

Salesforce hizmeti bulutu geri dönüşüm kutusu 'ndaki geçici silinen kayıtları sorgulamak için, olarak `readBehavior` `queryAll`belirtebilirsiniz. 

### <a name="difference-between-soql-and-sql-query-syntax"></a>SOQL ve SQL sorgu söz dizimi arasındaki fark

Salesforce hizmeti bulutlarından verileri kopyalarken, SOQL sorgusu veya SQL sorgusu kullanabilirsiniz. Bu ikisinin farklı sözdizimi ve işlevsellik desteğine sahip olduğunu ve bunu karıştırmadığını unutmayın. Salesforce hizmeti bulutu tarafından yerel olarak desteklenen SOQL sorgusunun kullanılması önerilir. Aşağıdaki tabloda başlıca farklılıklar listelenmektedir:

| Sözdizimi | SOQL modu | SQL modu |
|:--- |:--- |:--- |
| Sütun seçimi | Sorguda kopyalanacak alanların numaralandırılması gerekir, ör.`SELECT field1, filed2 FROM objectname` | `SELECT *`, sütun seçimine ek olarak desteklenir. |
| Tırnak işaretleri | Dosyalanmış/nesne adları tırnak içine alınamaz. | Alan/nesne adları tırnak içine alınabilir, ör.`SELECT "id" FROM "Account"` |
| Tarih saat biçimi |  [Buradaki](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_dateformats.htm) ayrıntılara ve sonraki bölümde örneklere bakın. | [Buradaki](https://docs.microsoft.com/sql/odbc/reference/develop-app/date-time-and-timestamp-literals?view=sql-server-2017) ayrıntılara ve sonraki bölümde örneklere bakın. |
| Boole değerleri | `SELECT … WHERE IsDeleted=True`Ve `False` olaraktemsil`True`edilir, ör. | 0 veya 1 olarak temsil edilir, ör `SELECT … WHERE IsDeleted=1`. |
| Sütun yeniden adlandırma | Desteklenmiyor. | Desteklenir, örneğin: `SELECT a AS b FROM …`. |
| İlişki | Desteklenir, ör `Account_vod__r.nvs_Country__c`. | Desteklenmiyor. |

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>DateTime sütununda WHERE yan tümcesini kullanarak veri alma

SOQL veya SQL sorgusu belirttiğinizde, tarih saat biçimi farklılığı ile ilgilenyin. Örneğin:

* **Soql örneği**:`SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL örneği**:`SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}`

### <a name="error-of-malformed_querytruncated"></a>MALFORMED_QUERY hatası: kesildi

"MALFORMED_QUERY" hatası alırsanız: Kesildi ", normalde bu, verilerde Junctionıdlist türünde sütununuzu ve Salesforce 'ın çok sayıda satır ile bu tür verileri desteklemeye yönelik sınırlamalara sahip olmasından kaynaklanır. Azaltmak için, Junctionıdlist sütununu dışlamanızı veya Kopyalanacak satır sayısını sınırlamayı deneyin (birden çok kopyalama etkinliği çalışmasına bölüm oluşturabilirsiniz).

## <a name="data-type-mapping-for-salesforce-service-cloud"></a>Salesforce hizmeti bulutu için veri türü eşlemesi

Salesforce hizmeti bulutlarından verileri kopyaladığınızda, veri türleri Data Factory için Salesforce hizmeti bulutu veri türlerinden aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Salesforce hizmeti bulutu veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Auto Number |Dize |
| Checkbox |Boole değeri |
| Currency |Decimal |
| Date |Datetime |
| Date/Time |Datetime |
| Email |Dize |
| Id |Dize |
| Lookup Relationship |Dize |
| Multi-Select Picklist |Dize |
| Number |Decimal |
| Percent |Decimal |
| Phone |Dize |
| Picklist |Dize |
| Text |Dize |
| Text Area |Dize |
| Text Area (Long) |Dize |
| Text Area (Rich) |Dize |
| Text (Encrypted) |Dize |
| URL |Dize |

## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
