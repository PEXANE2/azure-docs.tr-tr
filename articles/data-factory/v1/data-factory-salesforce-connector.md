---
title: Veri Fabrikası'nı kullanarak Salesforce'tan veri taşıma
description: Azure Veri Fabrikası'nı kullanarak Salesforce'tan verileri nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: dbe3bfd6-fa6a-491a-9638-3a9a10d396d1
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 8b94f6388d77cca2ef74c802aec7648091172775
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281138"
---
# <a name="move-data-from-salesforce-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Salesforce'tan veri taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-salesforce-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-salesforce.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Salesforce konektörüne](../connector-salesforce.md)bakın.

Bu makalede, Salesforce'tan desteklenen kaynaklar ve lavabolar tablosunda Lavabo sütunu altında listelenen herhangi bir veri deposuna veri kopyalamak için Bir Azure veri fabrikasında Kopyalama Etkinliği'ni nasıl kullanabileceğiniz [özetlenmiştir.](data-factory-data-movement-activities.md#supported-data-stores-and-formats) Bu makalede, Kopya Etkinliği ve desteklenen veri deposu kombinasyonları ile veri hareketinin genel bir genel bakış sunan [veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesi üzerine bir temel oluşturur.

Azure Veri Fabrikası şu anda yalnızca Salesforce'tan [desteklenen lavabo veri depolarına](data-factory-data-movement-activities.md#supported-data-stores-and-formats)veri taşımayı destekler, ancak diğer veri depolarından Salesforce'a veri taşımayı desteklemez.

## <a name="supported-versions"></a>Desteklenen sürümler
Bu bağlayıcı Salesforce'un aşağıdaki sürümlerini destekler: Developer Edition, Professional Edition, Enterprise Edition veya Unlimited Edition. Salesforce üretim, kum havuzu ve özel alan adı kopyalamayı destekler.

## <a name="prerequisites"></a>Ön koşullar
* API izni etkinleştirilmelidir. [Salesforce'ta API erişimini izin kümesiyle nasıl etkinleştirebilirim?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)
* Salesforce'tan şirket içi veri depolarına verileri kopyalamak için, şirket içi ortamınızda en az Veri Yönetimi Ağ Geçidi 2.0 yüklü olmalıdır.

## <a name="salesforce-request-limits"></a>Salesforce istek sınırları
Salesforce'un hem toplam API istekleri hem de eşzamanlı API istekleri için sınırları vardır. Aşağıdaki noktalara dikkat edin:

- Eşzamanlı istek sayısı sınırı aşarsa, azaltma oluşur ve rasgele hatalar görürsünüz.
- Toplam istek sayısı sınırı aşarsa, Salesforce hesabı 24 saat boyunca bloke edilir.

Ayrıca her iki senaryoda da "REQUEST_LIMIT_EXCEEDED" hatası alabilirsiniz. Ayrıntılar için [Salesforce Developer Limits](https://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) makalesindeki "API İstek Sınırları" bölümüne bakın.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak Salesforce'tan veri aktaran bir kopyalama etkinliği içeren bir ardışık hatlar oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Salesforce'tan verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Salesforce'tan Azure Blob bölümüne verileri kopyalayın.](#json-example-copy-data-from-salesforce-to-azure-blob)

Aşağıdaki bölümler, Salesforce'a özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, Salesforce bağlantılı hizmete özgü JSON öğeleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **Salesforce**. |Evet |
| çevreUrl | Salesforce örneğinin URL'sini belirtin. <br><br> - Varsayılan "https:\//login.salesforce.com"dir. <br> - Sanal andan verileri kopyalamakhttps://test.salesforce.comiçin " ". <br> - Özel etki alanından verileri kopyalamak için, örneğin "https://[domain].my.salesforce.com" adresini belirtin. |Hayır |
| kullanıcı adı |Kullanıcı hesabı için bir kullanıcı adı belirtin. |Evet |
| password |Kullanıcı hesabı için bir parola belirtin. |Evet |
| Securitytoken |Kullanıcı hesabı için bir güvenlik belirteci belirtin. Bkz. Bir güvenlik belirteci sıfırlama/alma hakkında talimatlar için [güvenlik belirteci alın.](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) Genel olarak güvenlik belirteçleri hakkında bilgi edinmek için [Güvenlik ve API'ye](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm)bakın. |Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **RelationalTable** türündeki bir veri kümesinin typeProperties bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Salesforce'taki tablonun adı. |Hayır **(İlişkisel Kaynak** **sorgusu** belirtilirse) |

> [!IMPORTANT]
> API Adı'nın "__c" bölümü herhangi bir özel nesne için gereklidir.

![Veri Fabrikası - Salesforce bağlantısı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, ardışık hatlar oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve çeşitli ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir.

Diğer taraftan, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama Etkinliği için bunlar kaynak ve lavabo türlerine bağlı olarak değişir.

Kopyalama etkinliğinde, kaynak **RelationalSource** türünden (Salesforce'u içerir) olduğunda, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |Bir SQL-92 sorgusu veya [Salesforce Nesne Sorgu Dili (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) sorgusu. Örneğin: `select * from MyTable__c`. |Hayır **(veri kümesinin** **tablo adı** belirtilirse) |

> [!IMPORTANT]
> API Adı'nın "__c" bölümü herhangi bir özel nesne için gereklidir.

![Veri Fabrikası - Salesforce bağlantısı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)

## <a name="query-tips"></a>Sorgu ipuçları
### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>DateTime sütunundaki where yan tümcesi kullanarak veri alma
SOQL veya SQL sorgusunu belirtirken, DateTime biçimi farkıyla dikkat edin. Örnek:

* **SOQL örneği**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', WindowStart, WindowEnd)`
* **SQL örneği**:
    * **Sorguyu belirtmek için kopya sihirbazını kullanma:**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\'{0:yyyy-MM-dd HH:mm:ss}\'}} AND LastModifiedDate < {{ts\'{1:yyyy-MM-dd HH:mm:ss}\'}}', WindowStart, WindowEnd)`
    * **Sorguyu belirtmek için JSON düzenlemesini kullanma (kaçış char düzgün):**`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', WindowStart, WindowEnd)`

### <a name="retrieving-data-from-salesforce-report"></a>Salesforce Raporu'ndan veri alma
Sorgu'u "örneğin" olarak `{call "<report name>"}`belirterek Salesforce raporlarından veri alabilirsiniz. `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Salesforce Geri Dönüşüm Kutusu'ndan silinen kayıtları alma
Salesforce Geri Dönüşüm Kutusu'ndan silinen yumuşak kayıtları sorgulamak için, sorgunuzda **"Silinmiş = 1"** belirtebilirsiniz. Örneğin,

* Yalnızca silinen kayıtları sorgulamak için "select * from MyTable__c **where IsDeleted= 1**"
* Varolan ve silinen ler de dahil olmak üzere tüm kayıtları sorgulamak için "select * from MyTable__c **where IsDeleted = 0 veya IsDeleted = 1**"

## <a name="json-example-copy-data-from-salesforce-to-azure-blob"></a>JSON örneği: Salesforce'tan Azure Blob'a veri kopyalama
Aşağıdaki örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell'i](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Salesforce'tan Azure Blob Depolama'ya kadar verilerin nasıl kopyalanır olduğunu gösterirler. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için kopyalanabilir.

Senaryoyu uygulamak için oluşturmanız gereken Veri Fabrikası yapıları aşağıda verilmiştir. Listeyi izleyen bölümler, bu adımlar hakkında ayrıntılı bilgi sağlar.

* [Salesforce](#linked-service-properties) türünün bağlantılı bir hizmeti
* [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) türünün bağlantılı bir hizmeti
* [İlişkisel Tablo](#dataset-properties) türüne ait bir giriş [veri kümesi](data-factory-create-datasets.md)
* [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties) türünün çıktı [veri kümesi](data-factory-create-datasets.md)
* [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) kullanan Kopyalama Etkinliği olan bir [ardışık](data-factory-create-pipelines.md)

**Salesforce bağlantılı hizmet**

Bu örnek, **Salesforce** bağlantılı hizmeti kullanır. Bu bağlantılı hizmet tarafından desteklenen özellikler için [Salesforce bağlantılı hizmet](#linked-service-properties) bölümüne bakın. Bkz. Güvenlik belirteci sıfırlama/alma hakkında talimatlar için [güvenlik belirteci alın.](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm)

```json
{
    "name": "SalesforceLinkedService",
    "properties":
    {
        "type": "Salesforce",
        "typeProperties":
        {
            "username": "<user name>",
            "password": "<password>",
            "securityToken": "<security token>"
        }
    }
}
```
**Azure Depolama bağlantılı hizmet**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```
**Salesforce giriş veri kümesi**

```json
{
    "name": "SalesforceInput",
    "properties": {
        "linkedServiceName": "SalesforceLinkedService",
        "type": "RelationalTable",
        "typeProperties": {
            "tableName": "AllDataType__c"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**True'nun** **dışında** ayar, Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

> [!IMPORTANT]
> API Adı'nın "__c" bölümü herhangi bir özel nesne için gereklidir.

![Veri Fabrikası - Salesforce bağlantısı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name.png)

**Azure blob çıktı veri seti**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1).

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties":
        {
            "folderPath": "adfgetstarted/alltypes_c"
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Kopyalama Etkinliği olan Boru Hattı**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **RelationalSource**olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır.

İlişkisel Kaynak tarafından desteklenen özelliklerin listesi için [RelationalSource türü özelliklerine](#copy-activity-properties) bakın.

```json
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2016-06-01T18:00:00",
        "end":"2016-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[
        {
            "name": "SalesforceToAzureBlob",
            "description": "Copy from Salesforce to an Azure blob",
            "type": "Copy",
            "inputs": [
            {
                "name": "SalesforceInput"
            }
            ],
            "outputs": [
            {
                "name": "AzureBlobOutput"
            }
            ],
            "typeProperties": {
                "source": {
                    "type": "RelationalSource",
                    "query": "SELECT Id, Col_AutoNumber__c, Col_Checkbox__c, Col_Currency__c, Col_Date__c, Col_DateTime__c, Col_Email__c, Col_Number__c, Col_Percent__c, Col_Phone__c, Col_Picklist__c, Col_Picklist_MultiSelect__c, Col_Text__c, Col_Text_Area__c, Col_Text_AreaLong__c, Col_Text_AreaRich__c, Col_URL__c, Col_Text_Encrypt__c, Col_Lookup__c FROM AllDataType__c"
                },
                "sink": {
                    "type": "BlobSink"
                }
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "concurrency": 1,
                "executionPriorityOrder": "OldestFirst",
                "retry": 0,
                "timeout": "01:00:00"
            }
        }
        ]
    }
}
```
> [!IMPORTANT]
> API Adı'nın "__c" bölümü herhangi bir özel nesne için gereklidir.

![Veri Fabrikası - Salesforce bağlantısı - API adı](media/data-factory-salesforce-connector/data-factory-salesforce-api-name-2.png)


### <a name="type-mapping-for-salesforce"></a>Salesforce için tür eşleme

| Salesforce türü | . NET tabanlı tip |
| --- | --- |
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

> [!NOTE]
> Kaynak veri kümesinden sütunlara kadar sütunları haritalamak için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Performans ve ayar
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için Kopyalama Etkinliği performansı ve [atoks kılavuzuna](data-factory-copy-activity-performance.md) bakın.
