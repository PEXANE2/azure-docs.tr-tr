---
title: Verileri OData kaynaklarından taşıma
description: Azure Veri Fabrikası'nı kullanarak OData kaynaklarından veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: de28fa56-3204-4546-a4df-21a21de43ed7
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 95f92d4e5616d7754c355610685701a8e089b84e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265915"
---
# <a name="move-data-from-an-odata-source-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Bir OData kaynağından taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-odata-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-odata.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki OData konektörüne](../connector-odata.md)bakın.


Bu makalede, verileri Bir OData kaynağından taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Verileri Bir OData kaynağından desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Kopyalama etkinliği tarafından lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. Veri fabrikası şu anda yalnızca bir OData kaynağından diğer veri depolarına veri taşımayı destekler, ancak verileri diğer veri depolarından Bir OData kaynağına taşımak için değildir.

## <a name="supported-versions-and-authentication-types"></a>Desteklenen sürümler ve kimlik doğrulama türleri
Bu OData bağlayıcısı OData sürüm 3.0 ve 4.0'ı destekler ve hem bulut OData'dan hem de şirket içi OData kaynaklarından verileri kopyalayabilirsiniz. İkincisi için, Veri Yönetimi Ağ Geçidi'ni yüklemeniz gerekir. Veri Yönetimi Ağ Geçidi hakkında ayrıntılar için [verileri şirket içi ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşıyın.

Aşağıda kimlik doğrulama türleri desteklenir:

* **Bulut** OData akışına erişmek için anonim, temel (kullanıcı adı ve parola) veya Azure Active Directory tabanlı OAuth kimlik doğrulamasını kullanabilirsiniz.
* Şirket **içi** OData akışına erişmek için anonim, temel (kullanıcı adı ve parola) veya Windows kimlik doğrulaması kullanabilirsiniz.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri Bir OData kaynağından hareket ettiren bir kopyalama etkinliği içeren bir ardışık hatlar oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Verileri Bir OData kaynağından kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek [için Bkz. JSON örneği: Verileri OData kaynağından Azure Blob bölümüne kopyalayın.](#json-example-copy-data-from-odata-source-to-azure-blob)

Aşağıdaki bölümler, OData kaynağına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı Hizmet özellikleri
Aşağıdaki tablo, OData bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği şu şekilde ayarlanmalıdır: **OData** |Evet |
| url |OData hizmetinin url'si. |Evet |
| authenticationType |OData kaynağına bağlanmak için kullanılan kimlik doğrulama türü. <br/><br/> Bulut OData için olası değerler Anonim, Temel ve OAuth'dur (azure veri fabrikasının şu anda yalnızca Azure Active Directory tabanlı OAuth'u desteklediğini unutmayın). <br/><br/> Şirket içi OData için olası değerler Anonim, Temel ve Windows'lardır. |Evet |
| kullanıcı adı |Temel kimlik doğrulamasını kullanıyorsanız kullanıcı adını belirtin. |Evet (yalnızca Temel kimlik doğrulaması kullanıyorsanız) |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Evet (yalnızca Temel kimlik doğrulaması kullanıyorsanız) |
| yetkiliCredential |OAuth kullanıyorsanız, Veri Fabrikası Kopyalama Sihirbazı veya Düzenleyici'deki **Yetkilendirme** düğmesini tıklatın ve kimlik bilgilerinizi girin, bu özelliğin değeri otomatik olarak oluşturulur. |Evet (yalnızca OAuth kimlik doğrulaması kullanıyorsanız) |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi OData hizmetine bağlanmak için kullanması gereken ağ geçidinin adı. Yalnızca şirket içi OData kaynağından veri kopyalıyorsanız belirtin. |Hayır |

### <a name="using-basic-authentication"></a>Temel kimlik doğrulamasını kullanma
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Basic",
            "username": "username",
            "password": "password"
        }
    }
}
```

### <a name="using-anonymous-authentication"></a>Anonim kimlik doğrulamasını kullanma
```json
{
    "name": "ODataLinkedService",
    "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Şirket içi OData kaynağına erişen Windows kimlik doğrulamayı kullanma
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
            "authenticationType": "Windows",
            "username": "domain\\user",
            "password": "password",
            "gatewayName": "mygateway"
        }
    }
}
```

### <a name="using-oauth-authentication-accessing-cloud-odata-source"></a>Bulut OData kaynağına erişen OAuth kimlik doğrulamasını kullanma
```json
{
    "name": "inputLinkedService",
    "properties":
    {
        "type": "OData",
            "typeProperties":
        {
            "url": "<endpoint of cloud OData source e.g. https://<tenant>.crm.dynamics.com/XRMServices/2011/OrganizationData.svc>",
            "authenticationType": "OAuth",
            "authorizedCredential": "<auto generated by clicking the Authorize button on UI>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

**typeProperties** bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **ODataResource** türü veri kümesi için typeProperties bölümü (OData veri kümesini içerir) aşağıdaki özelliklere sahiptir

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| yol |OData kaynağına giden yol |Hayır |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

Diğer taraftan etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Kaynak, **RelationalSource** türünden (OData dahil) olduğunda, typeProperties bölümünde aşağıdaki özellikleri kullanılabilir:

| Özellik | Açıklama | Örnek | Gerekli |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için özel sorguyu kullanın. |"?$select=İsim, Açıklama&$top=5" |Hayır |

## <a name="type-mapping-for-odata"></a>OData için Tür Eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki iki adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir.

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Verileri OData'dan hareket ettirirken, aşağıdaki eşlemeler OData türlerinden .NET türüne kullanılır.

| OData Veri Türü | .NET Türü |
| --- | --- |
| Edm.İkili |Bayt[] |
| Edm.Boolean |Bool |
| Edm.Bayt |Bayt[] |
| Edm.DateTime |DateTime |
| Edm.Ondalık |Ondalık |
| Edm.Double |Çift |
| Edm.Single |Tek |
| Edm.Guid |Guid |
| Edm.Int16 |Int16 |
| Edm.Int32 |Int32 |
| Edm.Int64 |Int64 |
| Edm.SByte |Int16 |
| Edm.String |Dize |
| Edm.Zaman |TimeSpan |
| Edm.DateTimeOffset |DateTimeOffset |

> [!Note]
> OData karmaşık veri türleri örneğin Nesne desteklenmez.

## <a name="json-example-copy-data-from-odata-source-to-azure-blob"></a>JSON örneği: Verileri OData kaynağından Azure Blob'a kopyalama
Bu örnek, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Bir OData kaynağından Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterirler. Ancak, veriler Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats) belirtilen lavabolardan herhangi biri için kopyalanabilir. Örnekte aşağıdaki Veri Fabrikası varlıkları vardır:

1. [OData](#linked-service-properties)türüne bağlı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [ODataResource](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [RelationalSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy Activity içeren bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, bir OData kaynağına karşı sorgulamadan her saat başı bir Azure blob'una kadar verileri kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**OData bağlantılı hizmet:** Bu örnek, Anonim kimlik doğrulaması kullanır. Kullanabileceğiniz farklı kimlik doğrulama türleri için [OData bağlantılı hizmet](#linked-service-properties) bölümüne bakın.

```json
{
    "name": "ODataLinkedService",
    "properties":
    {
        "type": "OData",
        "typeProperties":
        {
            "url": "https://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        }
    }
}
```

**Azure Depolama bağlantılı hizmet:**

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

**OData giriş veri seti:**

"Dış"ı ayarlamak: "true" veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "typeProperties":
        {
            "path": "Products"
        },
        "linkedServiceName": "ODataLinkedService",
        "structure": [],
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
        }
    }
}
```

Veri kümesi tanımında **yol** belirtme isteğe bağlıdır.

**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
{
    "name": "AzureBlobODataDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**OData kaynağı ve Blob lavabosu içeren bir ardışık ardışık ardışık alanda etkinliği kopyalama:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **RelationalSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **Sorgu** özelliği için belirtilen SQL sorgusu, OData kaynağından en son (en yeni) verileri seçer.

```json
{
    "name": "CopyODataToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "?$select=Name, Description&$top=5",
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "ODataDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobODataDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "ODataToBlob"
            }
        ],
        "start": "2017-02-01T18:00:00Z",
        "end": "2017-02-03T19:00:00Z"
    }
}
```

Ardışık hatlar tanımında **sorgu** belirtilmesi isteğe bağlıdır. Veri Fabrikası hizmetinin veri almak için kullandığı **URL:** Bağlı hizmette belirtilen URL (gerekli) + veri kümesinde belirtilen yol (isteğe bağlı) + ardışık ardışık alanda sorgu (isteğe bağlı).

### <a name="type-mapping-for-odata"></a>OData için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

OData veri depolarından veri taşınırken, OData veri türleri .NET türlerine eşlenir.

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-read-from-relational-sources"></a>İlişkisel kaynaklardan tekrarlanabilir okuma
İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
