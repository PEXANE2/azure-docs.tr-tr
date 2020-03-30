---
title: Verileri Salesforce Service Cloud'dan ve salesforce Service Cloud'a kopyalama
description: Salesforce Service Cloud'dan desteklenen lavabo veri depolarına veya desteklenen kaynak veri depolarından Salesforce Service Cloud'a veri fabrikası ardışık bir kopya etkinliği kullanarak verileri nasıl kopyalayacağınızı öğrenin.
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
ms.openlocfilehash: 4540b27a9241a14b3d1a153d11bf43900e8ae0ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153869"
---
# <a name="copy-data-from-and-to-salesforce-service-cloud-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Salesforce Service Cloud'dan kopyalayın

Bu makalede, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin Salesforce Service Cloud'dan ve Salesforce Service Cloud'dan verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Kopyalama [etkinliğine](copy-activity-overview.md) genel bir genel bakış sunan Kopyalama Etkinliği genel bakış makalesine dayanıyor.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Salesforce Service Cloud bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)

Salesforce Service Cloud'daki verileri desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundan Salesforce Service Cloud'a veri kopyalayabilirsiniz. Kopyaetkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Salesforce Service Cloud bağlayıcısı destekler:

- Salesforce Developer, Professional, Enterprise veya Unlimited sürümleri.
- Salesforce üretim, kum havuzu ve özel etki alanından ve Salesforce'a veri kopyalama.

Salesforce konektörü Salesforce REST/Bulk API'nin üzerine inşa edilmiştir. Varsayılan olarak, bağlayıcı Salesforce'tan verileri kopyalamak için [v45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) kullanır ve verileri Salesforce'a kopyalamak için [v40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) kullanır. Ayrıca, bağlantılı hizmetteki [ `apiVersion` özellik](#linked-service-properties) üzerinden veri okumak/yazmak için kullanılan API sürümünü de açıkça ayarlayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Salesforce'ta API izni etkinleştirilmelidir. Daha fazla bilgi için bkz: [Salesforce'ta API erişimini izin kümesine göre etkinleştir](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları

Salesforce'un hem toplam API istekleri hem de eşzamanlı API istekleri için sınırları vardır. Aşağıdaki noktalara dikkat edin:

- Eşzamanlı istek sayısı sınırı aşarsa, azaltma oluşur ve rasgele hatalar görürsünüz.
- Toplam istek sayısı sınırı aşarsa, Salesforce hesabı 24 saat boyunca engellenir.

Ayrıca her iki senaryoda da "REQUEST_LIMIT_EXCEEDED" hata iletisi alabilirsiniz. Daha fazla bilgi için [Salesforce geliştirici sınırlarındaki](https://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf)"API istek sınırları" bölümüne bakın.

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler, Salesforce Service Cloud bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Salesforce bağlantılı hizmet için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type |Tür özelliği **SalesforceServiceCloud**olarak ayarlanmalıdır. |Evet |
| çevreUrl | Salesforce Service Cloud örneğinin URL'sini belirtin. <br> - Varsayılan `"https://login.salesforce.com"`dır. <br> - Sanal andan verileri kopyalamak için `"https://test.salesforce.com"`. <br> - Özel etki alanından verileri kopyalamak `"https://[domain].my.salesforce.com"`için, örneğin, belirtin. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin.<br/><br/>Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Evet |
| Securitytoken |Kullanıcı hesabı için bir güvenlik belirteci belirtin. <br/><br/>Genel olarak güvenlik belirteçleri hakkında bilgi edinmek için [Güvenlik ve API'ye](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm)bakın. Güvenlik belirteci, yalnızca Salesforce'taki [güvenilir IP adresi listesine](https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/security_networkaccess.htm) Tümleştirme Runtime IP'sini eklerseniz atlanabilir. Azure IR kullanırken Azure [Tümleştirme Çalışma Zamanı IP adreslerine](azure-integration-runtime-ip-addresses.md)bakın.<br/><br/>Güvenlik belirteci alma ve sıfırlama hakkında talimatlar için [bkz.](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) |Hayır |
| apiVersion | Kullanılacak Salesforce REST/Bulk API sürümünü belirtin, `48.0`örneğin. Varsayılan olarak, bağlayıcı Salesforce'tan verileri kopyalamak için [v45](https://developer.salesforce.com/docs/atlas.en-us.218.0.api_rest.meta/api_rest/dome_versions.htm) kullanır ve verileri Salesforce'a kopyalamak için [v40](https://developer.salesforce.com/docs/atlas.en-us.208.0.api_asynch.meta/api_asynch/asynch_api_intro.htm) kullanır. | Hayır |
| connectVia | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Kaynak için hayır, kaynak bağlantılı hizmetin tümleştirme çalışma süresi yoksa lavabo için Evet |

>[!IMPORTANT]
>Verileri Salesforce Service Cloud'a kopyaladiğinizde, varsayılan Azure Tümleştirme Çalışma Süresi kopyayı yürütmek için kullanılamaz. Başka bir deyişle, kaynak bağlantılı hizmetinizin belirli bir tümleştirme çalışma süresi yoksa, Salesforce Service Cloud örneğinizin yakınında bir konuma sahip [bir Azure Tümleştirme Çalışma Süresi oluşturun.](create-azure-integration-runtime.md#create-azure-ir) Salesforce Service Cloud bağlantılı hizmeti aşağıdaki örnekte olduğu gibi ilişkilendirin.

**Örnek: Veri Fabrikası'nda kimlik bilgilerini depolama**

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

**Örnek: Anahtar Kasası'nda kimlik bilgilerini saklayın**

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde Salesforce Service Cloud veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Salesforce Service Cloud'dan ve Salesforce Service Cloud'a gelen verileri kopyalamak için aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **SalesforceServiceCloudObject**olarak ayarlanmalıdır.  | Evet |
| objectApiName | Veri almak için Salesforce nesne adı. | Kaynak için hayır, lavabo için Evet |

> [!IMPORTANT]
> **API Adı'nın** "__c" bölümü herhangi bir özel nesne için gereklidir.

![Veri Fabrikası Salesforce bağlantı API Adı](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

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
| type | Veri kümesinin tür özelliği **İlişkisel Tablo**olarak ayarlanmalıdır. | Evet |
| tableName | Salesforce Service Cloud'daki tablonun adı. | Hayır (etkinlik kaynağında "sorgu" belirtilmişse) |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde Salesforce Service Cloud kaynak ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="salesforce-service-cloud-as-a-source-type"></a>Kaynak türü olarak Salesforce Service Cloud

Salesforce Service Cloud'dan gelen verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği **SalesforceServiceCloudSource**olarak ayarlanmalıdır. | Evet |
| sorgu |Verileri okumak için özel sorguyu kullanın. [Salesforce Object Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgusu veya SQL-92 sorgusu kullanabilirsiniz. [Sorgu ipuçları](#query-tips) bölümünde daha fazla ipucu na bakın. Sorgu belirtilmemişse, veri kümesinde "objectApiName"de belirtilen Salesforce Service Cloud nesnesinin tüm verileri alınır. | Hayır (veri kümesinde "objectApiName" belirtilirse) |
| readBehavior | Varolan kayıtları sorgulayıp sorgulamayacağını veya silinenler de dahil olmak üzere tüm kayıtları sorgulayıp sorgulayacağını gösterir. Belirtilmemişse, varsayılan davranış eskidir. <br>İzin verilen değerler: **sorgu** (varsayılan), **queryAll**.  | Hayır |

> [!IMPORTANT]
> **API Adı'nın** "__c" bölümü herhangi bir özel nesne için gereklidir.

![Veri Fabrikası Salesforce bağlantı API Ad listesi](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

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

### <a name="salesforce-service-cloud-as-a-sink-type"></a>Lavabo türü olarak Salesforce Service Cloud

Verileri Salesforce Service Cloud'a kopyalamak için, kopyalama etkinliği **lavabo** bölümünde aşağıdaki özellikler desteklenir.

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun türü özelliği **SalesforceServiceCloudSink**olarak ayarlanmalıdır. | Evet |
| yazmaDavranışı | İşlem için yazma davranışı.<br/>İzin verilen değerler Ekle ve **Yukarı satır.** **Insert** | Hayır (varsayılan Insert' tir) |
| externalIdFieldName | Yükseltme işlemi için harici kimlik alanının adı. Belirtilen alan, Salesforce Service Cloud nesnesinde "Dış Kimlik Alanı" olarak tanımlanmalıdır. Karşılık gelen giriş verilerinde NULL değerleri olamaz. | "Upsert" için evet |
| yazmaBatchSize | Her toplu iş partisinde Salesforce Service Cloud'a yazılan verilerin satır sayısı. | Hayır (varsayılan değer 5.000'dir) |
| yoksNullValues | Yazma işlemi sırasında giriş verilerinden NULL değerlerini yoksayıp yoksaymayacağını gösterir.<br/>İzin verilen değerler **doğru** ve **yanlıştır.**<br>- **True**: Bir yükseltme veya güncelleştirme işlemi yaptığınızda hedef nesnedeki verileri değişmeden bırakın. Bir ekleme işlemi yaparken tanımlı bir varsayılan değer ekleyin.<br/>- **False**: Bir yükseltme veya güncelleştirme işlemi yaptığınızda hedef nesnedeki verileri NULL olarak güncelleştirin. Bir ekleme işlemi yaparken NULL değeri ekleyin. | Hayır (varsayılan yanlıştır) |

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

### <a name="retrieve-data-from-a-salesforce-service-cloud-report"></a>Salesforce Service Cloud raporundan veri alma

Salesforce Service Cloud raporlarından bir sorgu 'u `{call "<report name>"}`' olarak belirterek veri alabilirsiniz. `"query": "{call \"TestReport\"}"` bunun bir örneğidir.

### <a name="retrieve-deleted-records-from-the-salesforce-service-cloud-recycle-bin"></a>Salesforce Service Cloud Geri Dönüşüm Kutusu'ndan silinen kayıtları alma

Salesforce Service Cloud Geri Dönüşüm Kutusu'ndan silinen yumuşak `readBehavior` `queryAll`kayıtları sorgulamak için . 

### <a name="difference-between-soql-and-sql-query-syntax"></a>SOQL ve SQL sorgu sözdizimi arasındaki fark

Salesforce Service Cloud'dan veri kopyalarken SOQL sorgusu veya SQL sorgusu kullanabilirsiniz. Bu iki farklı sözdizimi ve işlevsellik desteği olduğunu unutmayın, karıştırmak yok. Salesforce Service Cloud tarafından yerel olarak desteklenen SOQL sorgusunu kullanmanız önerilir. Aşağıdaki tabloda temel farklar listelenebvardır:

| Sözdizimi | SOQL Modu | SQL Modu |
|:--- |:--- |:--- |
| Sütun seçimi | Sorguda kopyalanacak alanları, örneğin,`SELECT field1, filed2 FROM objectname` | `SELECT *`sütun seçimine ek olarak desteklenir. |
| Tırnak | Dosyalanmış/nesne adları alıntı yapılamaz. | Alan/nesne adları alıntılanabilir, örneğin.`SELECT "id" FROM "Account"` |
| Datetime biçimi |  [Ayrıntılara](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_select_dateformats.htm) ve sonraki bölümdeki örneklere bakın. | [Ayrıntılara](https://docs.microsoft.com/sql/odbc/reference/develop-app/date-time-and-timestamp-literals?view=sql-server-2017) ve sonraki bölümdeki örneklere bakın. |
| Boolean değerleri | Olarak `False` temsil `True`ve , örneğin `SELECT … WHERE IsDeleted=True`. | 0 veya 1 olarak temsil edilir, örneğin `SELECT … WHERE IsDeleted=1`. |
| Sütun yeniden adlandırma | Desteklenmiyor. | Desteklenen, örneğin: `SELECT a AS b FROM …`. |
| İlişki | Desteklenen, örneğin `Account_vod__r.nvs_Country__c`. | Desteklenmiyor. |

### <a name="retrieve-data-by-using-a-where-clause-on-the-datetime-column"></a>DateTime sütunundaki bir yer yan tümcesi kullanarak verileri alma

SOQL veya SQL sorgusunu belirtirken, DateTime biçimi farkıyla dikkat edin. Örnek:

* **SOQL örneği**:`SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **SQL örneği**:`SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}`

### <a name="error-of-malformed_querytruncated"></a>MALFORMED_QUERY hatası:Kesildi

"MALFORMED_QUERY: Kesildi" hatasına ulaştıysanız, normalde bunun nedeni verilerde JunctionIdList türü sütunudur ve Salesforce bu tür verileri çok sayıda satırla destekleme sınırlamasına sahiptir. Azaltmak için JunctionIdList sütunu hariç tutmayı deneyin veya kopyalanması gereken satır sayısını sınırlamayı deneyin (birden çok kopyalama etkinliği çalıştırAna bölünebilirsiniz).

## <a name="data-type-mapping-for-salesforce-service-cloud"></a>Salesforce Service Cloud için veri türü eşleme

Salesforce Service Cloud'daki verileri kopyaladiğinizde, Salesforce Service Cloud veri türlerinden Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| Salesforce Service Cloud veri türü | Veri Fabrikası geçici veri türü |
|:--- |:--- |
| Otomatik Numara |Dize |
| Onay kutusu |Boole |
| Para birimi |Ondalık |
| Tarih |DateTime |
| Tarih/Saat |DateTime |
| Email |Dize |
| Kimlik |Dize |
| Arama İlişkisi |Dize |
| Çok Select Picklist |Dize |
| Sayı |Ondalık |
| Yüzde |Ondalık |
| Telefon |Dize |
| Seçim Listesi |Dize |
| Metin |Dize |
| Metin Alanı |Dize |
| Metin Alanı (Uzun) |Dize |
| Metin Alanı (Zengin) |Dize |
| Metin (Şifreli) |Dize |
| URL'si |Dize |

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.


## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi [için](copy-activity-overview.md#supported-data-stores-and-formats)bkz.
