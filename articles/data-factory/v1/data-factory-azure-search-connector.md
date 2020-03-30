---
title: Veri Fabrikası'nı kullanarak verileri Arama dizinine itme
description: Azure Veri Fabrikası'nı kullanarak verileri Azure Bilişsel Arama Dizini'ne nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 5b1170f721cf8521cfe1762df0cc616c938ddf28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281567"
---
# <a name="push-data-to-an-azure-cognitive-search-index-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Azure Bilişsel Arama dizinine itme
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-azure-search-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-search.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Azure Bilişsel Arama bağlayıcısı'na](../connector-azure-search.md)bakın.

Bu makalede, desteklenen bir kaynak veri deposundan Azure Bilişsel Arama dizinine veri itmek için Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Desteklenen kaynak veri depoları desteklenen kaynaklar [ve lavabolar](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosunun Kaynak sütununda listelenir. Bu makalede, Kopya Etkinliği ve desteklenen veri deposu kombinasyonları ile veri hareketinin genel bir genel bakış sunan [veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesi üzerine bir temel oluşturur.

## <a name="enabling-connectivity"></a>Bağlantı sağlama
Veri Fabrikası hizmetinin şirket içi bir veri deposuna bağlanmasına izin vermek için, şirket içi ortamınıza Veri Yönetimi Ağ Geçidi yüklersiniz. Ağ geçidini, kaynak veri deposunu barındıran makineye veya veri deposuyla kaynaklar için rekabet etmeyi önlemek için ayrı bir makineye yükleyebilirsiniz.

Veri Yönetimi Ağ Geçidi, şirket içi veri kaynaklarını bulut hizmetlerine güvenli ve yönetilen bir şekilde bağlar. Veri Yönetimi Ağ Geçidi hakkında ayrıntılar için [verileri şirket içi ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşıyın.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri kaynak veri deposundan arama dizinine iten bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız.  Arama dizini için verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı bir örnek için Bkz. [JSON örneği: Şirket içi SQL Server'dan](#json-example-copy-data-from-on-premises-sql-server-to-azure-cognitive-search-index) bu makalenin Azure Bilişsel Arama dizini bölümüne veri kopyalama.

Aşağıdaki bölümler, arama Dizinine özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki tablo, Azure Bilişsel Arama bağlantılı hizmetine özgü JSON öğeleri için açıklamalar sağlar.

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Tür özelliği şu şekilde ayarlanmalıdır: **AzureSearch.** | Evet |
| url | Arama hizmetinin URL'si. | Evet |
| anahtar | Arama hizmeti için yönetici anahtarı. | Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri için benzerdir. **typeProperties** bölümü her veri kümesi türü için farklıdır. **AzureSearchIndex** türündeki bir veri kümesinin typeProperties bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Tür özelliği **AzureSearchIndex**olarak ayarlanmalıdır.| Evet |
| indexName | Arama dizininin adı. Veri Fabrikası dizin oluşturmaz. Dizin Azure Bilişsel Arama'da bulunmalıdır. | Evet |


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, ardışık hatlar oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve çeşitli ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir. Oysa, typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama Etkinliği için bunlar kaynak ve lavabo türlerine bağlı olarak değişir.

Copy Activity için, lavabo **AzureSearchIndexSink**türünden olduğunda, typeProperties bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| -------- | ----------- | -------------- | -------- |
| Yazma Davranışı | Diziniçinde zaten bir belge varken birleşip değiştirilmeyeceğini belirtir. [WriteBehavior özelliğine](#writebehavior-property)bakın.| Birleştirme (varsayılan)<br/>Karşıya Yükle| Hayır |
| WriteBatchSize | Arabellek boyutu writeBatchSize'a ulaştığında verileri arama dizinine yükler. Ayrıntılar için [WriteBatchSize özelliğine](#writebatchsize-property) bakın. | 1'den 1000'e kadar. Varsayılan değer 1000'dir. | Hayır |

### <a name="writebehavior-property"></a>WriteBehavior özelliği
AzureSearchSink veri yazarken ekler. Başka bir deyişle, belge yazarken, belge anahtarı arama dizininde zaten varsa, Azure Bilişsel Arama çakışma özel durumu oluşturmak yerine varolan belgeyi güncelleştirir.

AzureSearchSink aşağıdaki iki yükseltme davranışı sağlar (AzureSearch SDK kullanarak):

- **Birleştirme**: Yeni belgedeki tüm sütunları varolan sütunla birleştirin. Yeni belgede null değeri olan sütunlar için, varolan sütundaki değer korunur.
- **Upload**: Yeni belge varolanBelgenin yerini alır. Yeni belgede belirtilmeyen sütunlar için, değer varolan belgede null olmayan bir değer olup olmadığını geçersiz kılacak şekilde ayarlanır.

Varsayılan davranış **Birleştirme'dir.**

### <a name="writebatchsize-property"></a>WriteBatchSize Özelliği
Azure Bilişsel Arama hizmeti, belge toplu olarak yazmayı destekler. Bir toplu iş 1 ila 1.000 Eylem içerebilir. Eylem, yükleme/birleştirme işlemini gerçekleştirmek için bir belgeyi işler.

### <a name="data-type-support"></a>Veri türü desteği
Aşağıdaki tabloda, Azure Bilişsel Arama veri türünün desteklenip desteklenmediğini belirtin.

| Azure Bilişsel Arama veri türü | Azure Bilişsel Arama Lavabosu'nda desteklendi |
| ---------------------- | ------------------------------ |
| Dize | E |
| Int32 | E |
| Int64 | E |
| Çift | E |
| Boole | E |
| DataTimeOffset | E |
| Dize Dizisi | N |
| Coğrafya Noktası | N |

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-cognitive-search-index"></a>JSON örneği: Şirket içi SQL Server'dan Azure Bilişsel Arama dizinine veri kopyalama

Aşağıdaki örnek gösterir:

1. [AzureSearch](#linked-service-properties)türüne bağlı bir hizmet.
2. [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties)türünde bağlantılı bir hizmet.
3. [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureSearchIndex](#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
4. [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) ve [AzureSearchIndexSink](#copy-activity-properties)kullanan kopyalama etkinliği içeren bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, şirket içi bir SQL Server veritabanından saat başı arama dizini aramak için zaman serisi verilerini kopyalar. Bu örnekte kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, şirket içi makinenizde veri yönetimi ağ geçidini kurun. Yönergeler, [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasındaki hareketli verilerde dir.

**Azure Bilişsel Arama bağlantılı hizmet:**

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

**SQL Server bağlantılı hizmet**

```JSON
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```

**SQL Server giriş veri seti**

Örnek, SQL Server'da "MyTable" tablosu oluşturduğunuzu varsayar ve zaman serisi verileri için "zaman damgası sütunu" adlı bir sütun içerir. Tek bir veri kümesini kullanarak aynı veritabanı içinde birden çok tablo üzerinde sorgu yapabilirsiniz, ancak veri kümesinin tableName typeProperty için tek bir tablo kullanılmalıdır.

"Dış"ı ayarlamak: "true" veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
{
  "name": "SqlServerDataset",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

**Azure Bilişsel Arama çıktı veri seti:**

Örnek, verileri Azure Bilişsel Arama dizinine **kopyalar.** Veri Fabrikası dizin oluşturmaz. Örneği sınamak için bu ada sahip bir dizin oluşturun. Giriş veri kümesindekiyle aynı sayıda sütuniçeren arama dizini oluşturun. Arama dizinine her saat başı yeni girişler eklenir.

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
    }
}
```

**SQL kaynağı ve Azure Bilişsel Arama Dizini olan bir ardışık ardışık ardışık ardışık alanda etkinliği kopyalama:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **SqlSource** olarak ayarlanır ve **lavabo** türü **AzureSearchIndexSink**olarak ayarlanır. **SqlReaderQuery** özelliği için belirtilen SQL sorgusu, kopyalanacak son bir saat içinde verileri seçer.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

Bir bulut veri deposundan Azure Bilişsel Arama'ya `executionLocation` veri kopyalıyorsanız, özellik gereklidir. Aşağıdaki JSON snippet, Kopyalama Etkinliği `typeProperties` altında gereken değişikliği örnek olarak gösterir. Desteklenen değerler ve daha fazla ayrıntı için [bulut veri depoları](data-factory-data-movement-activities.md#global) bölümü arasındaki verileri kopyala'yı denetleyin.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a>Bulut kaynağından kopyala
Bir bulut veri deposundan Azure Bilişsel Arama'ya `executionLocation` veri kopyalıyorsanız, özellik gereklidir. Aşağıdaki JSON snippet, Kopyalama Etkinliği `typeProperties` altında gereken değişikliği örnek olarak gösterir. Desteklenen değerler ve daha fazla ayrıntı için [bulut veri depoları](data-factory-data-movement-activities.md#global) bölümü arasındaki verileri kopyala'yı denetleyin.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

Ayrıca, kopya etkinliği tanımında kaynak veri kümesinden sütunlara, lavabo veri kümesinden sütunlara kadar sütunları eşleyebilirsiniz. Ayrıntılar için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)bilgisine bakın.

## <a name="performance-and-tuning"></a>Performans ve ayar
Veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için Kopyalama Etkinliği performansı ve [atoks kılavuzuna](data-factory-copy-activity-performance.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Kopyalama Etkinliği ile bir ardışık hatlar oluşturmak için adım adım yönergeleri için [Etkinlik öğreticikopyalayın.](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
