---
title: Ve Salesforce 'a veri kopyalama
description: Veri Fabrikası ardışık düzeninde kopyalama etkinliği kullanarak Salesforce 'tan desteklenen havuz veri depolarına veya desteklenen kaynak veri depolarından verileri Salesforce 'a kopyalamayı öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: 68480f5b3b52d2347369f878802c71672213940a
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82146870"
---
# <a name="copy-data-from-and-to-salesforce-by-using-azure-data-factory"></a>Azure Data Factory kullanarak verileri ve Salesforce 'a kopyalama

> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-salesforce-connector.md)
> * [Geçerli sürüm](connector-salesforce.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, verileri Salesforce 'tan ve Salesforce 'a kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Salesforce Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)

Salesforce 'tan verileri desteklenen herhangi bir havuz veri deposuna kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundan verileri Salesforce 'a kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Salesforce Bağlayıcısı şunları destekler:

- Salesforce geliştiricisi, Professional, Enterprise veya sınırsız sürümler.
- Ve Salesforce üretimden, korumalı alana ve özel etki alanından veri kopyalama.

Salesforce Bağlayıcısı, Salesforce REST/toplu API 'nin üzerine kurulmuştur (bağlayıcı, daha iyi performans için otomatik olarak bir tane seçer). Varsayılan olarak, bağlayıcı Salesforce 'tan veri kopyalamak için [V45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) kullanır ve verileri Salesforce 'a kopyalamak için [V40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) kullanır. Ayrıca, bağlantılı hizmette [ `apiVersion` özelliği](#linked-service-properties) aracılığıyla verileri okumak/yazmak için kullanılan API sürümünü açıkça ayarlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Salesforce 'ta API izninin etkinleştirilmiş olması gerekir. Daha fazla bilgi için bkz. [izin kümesine göre Salesforce 'TA API erişimini etkinleştirme](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları

Salesforce, hem toplam API istekleri hem de eşzamanlı API istekleri için sınırlara sahiptir. Aşağıdaki noktalara dikkat edin:

- Eşzamanlı isteklerin sayısı sınırı aşarsa, kısıtlama gerçekleşir ve rastgele sorunlar görürsünüz.
- Toplam istek sayısı sınırı aşarsa, Salesforce hesabı 24 saat için engellenir.

Ayrıca, her iki senaryoda da "REQUEST_LIMIT_EXCEEDED" hata iletisini alabilirsiniz. Daha fazla bilgi için [Salesforce geliştirici limitlerinin](https://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf)"API isteği sınırları" bölümüne bakın.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler Salesforce bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özelliklerle ilgili ayrıntıları sağlar.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Salesforce bağlantılı hizmeti için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Type özelliği **Salesforce**olarak ayarlanmalıdır. |Yes |
| environmentUrl | Salesforce örneğinin URL 'sini belirtin. <br> -Varsayılan değer `"https://login.salesforce.com"`. <br> -Korumalı verileri veri kopyalamak için belirtin `"https://test.salesforce.com"`. <br> -Özel etki alanından veri kopyalamak için, örneğin, `"https://[domain].my.salesforce.com"`. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir Kullanıcı adı belirtin. |Yes |
| password |Kullanıcı hesabı için bir parola belirtin.<br/><br/>Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. |Yes |
| Belirtilmedi |Kullanıcı hesabı için bir güvenlik belirteci belirtin. <br/><br/>Genel olarak güvenlik belirteçleri hakkında daha fazla bilgi edinmek için bkz. [güvenlik ve API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). Güvenlik belirteci yalnızca Integration Runtime IP 'sini Salesforce üzerindeki [GÜVENILIR IP adresi listesine](https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/security_networkaccess.htm) eklediğinizde atlanabilir. Azure IR kullanırken [Azure INTEGRATION RUNTIME IP adresleri](azure-integration-runtime-ip-addresses.md)' ne bakın.<br/><br/>Güvenlik belirtecini alma ve sıfırlamaya ilişkin yönergeler için bkz. [güvenlik belirteci alma](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm). Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. |Hayır |
| apiVersion | Kullanılacak Salesforce REST/toplu API sürümünü belirtin, ör `48.0`. Varsayılan olarak, bağlayıcı Salesforce 'tan veri kopyalamak için [V45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) kullanır ve verileri Salesforce 'a kopyalamak için [V40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) kullanır. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. | Kaynak için Hayır, kaynak bağlı hizmette tümleştirme çalışma zamanı yoksa, havuz için Evet |

>[!IMPORTANT]
>Verileri Salesforce 'a kopyaladığınızda, varsayılan Azure Integration Runtime kopyayı yürütmek için kullanılamaz. Diğer bir deyişle, kaynak bağlı hizmetinizin belirtilen bir tümleştirme çalışma zamanı yoksa Salesforce örneğinizin yakınında bir konum ile açık [bir Azure Integration Runtime oluşturun](create-azure-integration-runtime.md#create-azure-ir) . Salesforce bağlantılı hizmetini aşağıdaki örnekte olduğu gibi ilişkilendirin.

**Örnek: Data Factory kimlik bilgilerini depolama**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
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
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Salesforce veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Ve Salesforce 'tan verileri kopyalamak için, veri kümesinin Type özelliğini **Salesforceobject**olarak ayarlayın. Aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği **Salesforceobject**olarak ayarlanmalıdır.  | Yes |
| objectApiName | Verilerin alınması için Salesforce nesne adı. | Kaynak için Hayır, havuz için Evet |

> [!IMPORTANT]
> Tüm özel nesneler için **API adının** "__C" kısmı gereklidir.

![Data Factory Salesforce bağlantısı API 'SI adı](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Örneğinde**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "typeProperties": {
            "objectApiName": "MyTable__c"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

>[!NOTE]
>Geriye dönük uyumluluk için: Salesforce 'tan veri kopyaladığınızda, önceki "RelationalTable" türü veri kümesini kullanırsanız, yeni "SalesforceObject" türüne geçiş önerisi gördüğünüz sırada çalışmaya devam eder.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin Type özelliği **Relationaltable**olarak ayarlanmalıdır. | Yes |
| tableName | Salesforce 'daki tablonun adı. | Hayır (etkinlik kaynağında "sorgu" belirtilmişse) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Salesforce kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="salesforce-as-a-source-type"></a>Kaynak türü olarak Salesforce

Salesforce 'tan veri kopyalamak için kopyalama etkinliğindeki kaynak türünü **Salesforcesource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağının Type özelliği **Salesforcesource**olarak ayarlanmalıdır. | Yes |
| sorgu |Verileri okumak için özel sorguyu kullanın. [Salesforce nesne sorgu dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) SORGUSUNU veya SQL-92 sorgusunu kullanabilirsiniz. [Sorgu ipuçları](#query-tips) bölümünde daha fazla ipucu görüntüleyin. Sorgu belirtilmemişse, veri kümesindeki "objectApiName" içinde belirtilen Salesforce nesnesinin tüm verileri alınır. | Hayır (veri kümesindeki "objectApiName" belirtilmişse) |
| readBehavior | Mevcut kayıtların sorgulanıp sorgulanmayacağını veya silinen kayıtlar dahil olmak üzere tüm kayıtları sorganıp sorgulanmayacağını gösterir. Belirtilmemişse, varsayılan davranış eski ' dir. <br>İzin verilen değerler: **sorgu** (varsayılan), **queryall**.  | Hayır |

> [!IMPORTANT]
> Tüm özel nesneler için **API adının** "__C" kısmı gereklidir.

![Data Factory Salesforce bağlantısı API 'SI ad listesi](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Örneğinde**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Geriye dönük uyumluluk için: Salesforce 'tan veri kopyaladığınızda önceki "RelationalSource" tür kopyasını kullanırsanız, yeni "SalesforceSource" türüne geçiş önerisi gördüğünüz sürece kaynak çalışmaya devam eder.

### <a name="salesforce-as-a-sink-type"></a>Havuz türü olarak Salesforce

Verileri Salesforce 'a kopyalamak için kopyalama etkinliğindeki havuz türünü **Salesforcesink**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **havuzunu** Kopyala bölümünde desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği havuzunun Type özelliği **Salesforcesink**olarak ayarlanmalıdır. | Yes |
| writeBehavior | İşlem için yazma davranışı.<br/>İzin verilen değerler **Insert** ve **upsert**. | Hayır (varsayılan değer ekler) |
| externalIdFieldName | Upsert işlem için dış KIMLIK alanının adı. Belirtilen alanın Salesforce nesnesinde "dış kimlik alanı" olarak tanımlanması gerekir. Karşılık gelen giriş verilerinde NULL değer bulunamaz. | "Upsert" için Evet |
| writeBatchSize | Her toplu işte Salesforce 'a yazılan verilerin satır sayısı. | Hayır (varsayılan değer 5.000) |
| ıgnorenullvalues | Bir yazma işlemi sırasında giriş verilerinden NULL değerlerin yoksayılıp yoksayılmayacağını gösterir.<br/>İzin verilen değerler **true** ve **false**şeklindedir.<br>- **Doğru**: bir yukarı veya güncelleştirme işlemi gerçekleştirdiğinizde verileri hedef nesnede değiştirmeden bırakın. Ekleme işlemi yaparken tanımlanmış bir varsayılan değer ekleyin.<br/>- **Yanlış**: bir yukarı veya güncelleştirme işlemi gerçekleştirdiğinizde hedef NESNESINDEKI verileri null olarak güncelleştirin. Ekleme işlemi yaparken NULL değer ekleyin. | Hayır (varsayılan değer false) |

**Örnek: bir kopyalama etkinliğinde Salesforce havuzu**

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
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

### <a name="retrieve-data-from-a-salesforce-report"></a>Salesforce raporundan veri alma

Bir sorgu belirterek Salesforce raporlarından veri alabilirsiniz `{call "<report name>"}`. `"query": "{call \"TestReport\"}"` bunun bir örneğidir.

### <a name="retrieve-deleted-records-from-the-salesforce-recycle-bin"></a>Silinen kayıtları Salesforce geri dönüşüm kutusu 'ndan alma

Geçici olarak silinen kayıtları Salesforce geri dönüşüm kutusu 'ndan sorgulamak için, olarak `readBehavior` `queryAll`belirtebilirsiniz. 

### <a name="difference-between-soql-and-sql-query-syntax"></a>SOQL ve SQL sorgu söz dizimi arasındaki fark

Salesforce 'tan veri kopyalarken, SOQL sorgusu veya SQL sorgusu kullanabilirsiniz. Bu ikisinin farklı sözdizimi ve işlevsellik desteğine sahip olduğunu ve bunu karıştırmadığını unutmayın. Salesforce tarafından yerel olarak desteklenen SOQL sorgusunun kullanılması önerilir. Aşağıdaki tabloda başlıca farklılıklar listelenmektedir:

| Sözdizimi | SOQL modu | SQL modu |
|:--- |:--- |:--- |
| Sütun seçimi | Sorguda kopyalanacak alanların numaralandırılması gerekir, ör.`SELECT field1, filed2 FROM objectname` | `SELECT *`, sütun seçimine ek olarak desteklenir. |
| Tırnak işaretleri | Dosyalanmış/nesne adları tırnak içine alınamaz. | Alan/nesne adları tırnak içine alınabilir, ör.`SELECT "id" FROM "Account"` |
| Tarih saat biçimi |  [Buradaki](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_dateformats.htm) ayrıntılara ve sonraki bölümde örneklere bakın. | [Buradaki](https://docs.microsoft.com/sql/odbc/reference/develop-app/date-time-and-timestamp-literals?view=sql-server-2017) ayrıntılara ve sonraki bölümde örneklere bakın. |
| Boole değerleri | Ve `False` `True`olarak temsil edilir, ör `SELECT … WHERE IsDeleted=True`. | 0 veya 1 olarak temsil edilir, ör `SELECT … WHERE IsDeleted=1`. |
| Sütun yeniden adlandırma | Desteklenmiyor. | Desteklenir, örneğin: `SELECT a AS b FROM …`. |
| İlişki | Desteklenir, ör `Account_vod__r.nvs_Country__c`. | Desteklenmiyor. |

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>DateTime sütununda WHERE yan tümcesini kullanarak veri alma

SOQL veya SQL sorgusu belirttiğinizde, tarih saat biçimi farklılığı ile ilgilenyin. Örneğin:

* **Soql örneği**:`SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL örneği**:`SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}`

### <a name="error-of-malformed_querytruncated"></a>MALFORMED_QUERY hatası: kesildi

"MALFORMED_QUERY: kesildi" hatasıyla karşılaşırsanız, normalde bu durum veride Junctionıdlist türünde sütununuzu ve Salesforce 'un bu verileri çok sayıda satırla desteklemeye yönelik sınırlaması vardır. Azaltmak için, Junctionıdlist sütununu dışlamanızı veya Kopyalanacak satır sayısını sınırlamayı deneyin (birden çok kopyalama etkinliği çalışmasına bölüm oluşturabilirsiniz).

## <a name="data-type-mapping-for-salesforce"></a>Salesforce için veri türü eşlemesi

Salesforce 'tan verileri kopyaladığınızda, veri türleri Data Factory için Salesforce veri türlerinden aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Salesforce veri türü | Data Factory geçici veri türü |
|:--- |:--- |
| Otomatik sayı |Dize |
| Onay kutusu |Boole |
| Para birimi |Ondalık |
| Tarih |DateTime |
| Tarih/Saat |DateTime |
| E-posta |Dize |
| Kimlik |Dize |
| Arama Ilişkisi |Dize |
| Çoklu seçim listesi |Dize |
| Sayı |Ondalık |
| Yüzde |Ondalık |
| Telefon |Dize |
| Seçim Listesi |Dize |
| Metin |Dize |
| Metin alanı |Dize |
| Metin alanı (uzun) |Dize |
| Metin alanı (zengin) |Dize |
| Metin (şifrelenmiş) |Dize |
| URL'si |Dize |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.


## <a name="next-steps"></a>Sonraki adımlar
Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
