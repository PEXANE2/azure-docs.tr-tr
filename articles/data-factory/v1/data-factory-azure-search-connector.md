---
title: Data Factory kullanarak arama dizinine veri gönderme
description: Azure Data Factory kullanarak Azure Search dizine veri gönderme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 09b891ba753291511bb1f203b7ac4437e6b2c542
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73683103"
---
# <a name="push-data-to-an-azure-search-index-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure Search dizinine veri gönderme
> [!div class="op_single_selector" title1="Kullandığınız Data Factory hizmeti sürümünü seçin:"]
> * [Sürüm 1](data-factory-azure-search-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-search.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız, [v2 'de Azure Search Bağlayıcısı](../connector-azure-search.md)' na bakın.

Bu makalede, desteklenen bir kaynak veri deposundan verileri Azure Search dizine göndermek için kopyalama etkinliğinin nasıl kullanılacağı açıklanır. Desteklenen kaynak veri depoları, [desteklenen kaynaklar ve havuzlar](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosunun kaynak sütununda listelenir. Bu makale, kopyalama etkinliği ve desteklenen veri deposu birleşimleri ile veri hareketine genel bir bakış sunan [veri taşıma etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanır.

## <a name="enabling-connectivity"></a>Bağlantı etkinleştiriliyor
Data Factory hizmetinin şirket içi veri deposuna bağlanmasına izin vermek için, Veri Yönetimi ağ geçidini şirket içi ortamınıza yüklersiniz. Ağ geçidini, kaynak veri deposunu barındıran aynı makineye veya veri deposu ile kaynakların rekabeti önlemek için ayrı bir makineye yükleyebilirsiniz.

Veri Yönetimi ağ geçidi, şirket içi veri kaynaklarını bulut hizmetlerine güvenli ve yönetilen bir şekilde bağlar. Veri Yönetimi ağ geçidi hakkındaki ayrıntılar için bkz. Şirket [içi ve bulut hakkındaki verileri taşıma](data-factory-move-data-between-onprem-and-cloud.md) .

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API 'Ler kullanarak bir kaynak veri deposundan verileri Azure Search dizine gönderen bir kopyalama etkinliği ile işlem hattı oluşturabilirsiniz.

İşlem hattı oluşturmanın en kolay yolu **Kopyalama Sihirbazı**' nı kullanmaktır. Veri kopyalama Sihirbazı 'nı kullanarak işlem hattı oluşturma hakkında hızlı bir yol için bkz. [öğretici: kopyalama Sihirbazı 'nı kullanarak işlem hattı oluşturma](data-factory-copy-data-wizard-tutorial.md) .

İşlem hattı oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliğine sahip bir işlem hattı oluşturmak için adım adım yönergeler için bkz. [kopyalama etkinliği öğreticisi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) .

Araçları veya API 'Leri kullanıp kullanmayacağınızı bir kaynak veri deposundan havuz veri deposuna veri taşınan bir işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıkış veri depolarını veri fabrikanıza bağlamak için **bağlı hizmetler** oluşturun.
2. Kopyalama işlemi için girdi ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Bir veri kümesini girdi olarak ve bir veri kümesini çıkış olarak alan kopyalama etkinliği ile bir işlem **hattı** oluşturun.

Sihirbazı kullandığınızda, bu Data Factory varlıkların JSON tanımları (bağlı hizmetler, veri kümeleri ve işlem hattı) sizin için otomatik olarak oluşturulur. Araçlar/API 'Leri (.NET API hariç) kullandığınızda, bu Data Factory varlıkları JSON biçimini kullanarak tanımlarsınız.  Azure Search dizinine veri kopyalamak için kullanılan Data Factory varlıkların JSON tanımlarına sahip bir örnek için, bkz. [JSON örneği: Bu makalenin verileri şirket içi SQL Server Azure Search dizine kopyalama](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) .

Aşağıdaki bölümler Azure Search dizine özgü Data Factory varlıkları tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Aşağıdaki tabloda Azure Search bağlı hizmetine özgü JSON öğelerine yönelik açıklamalar verilmiştir.

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Type özelliği: **Azuresearch**olarak ayarlanmalıdır. | Evet |
| url | Azure Search hizmeti URL 'SI. | Evet |
| anahtar | Azure Search hizmeti için yönetici anahtarı. | Evet |

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri oluşturma](data-factory-create-datasets.md) makalesi. Bir veri kümesinin yapısı, kullanılabilirliği ve İlkesi gibi bölümler, tüm veri kümesi türleri için benzerdir. **Typeproperties** bölümü her bir veri kümesi türü için farklıdır. **AzureSearchIndex** türündeki bir veri kümesinin typeproperties bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| type | Type özelliği **AzureSearchIndex**olarak ayarlanmalıdır.| Evet |
| indexName | Azure Search dizininin adı. Data Factory dizini oluşturmaz. Dizinin Azure Search olması gerekir. | Evet |


## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için, işlem [hatları oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıkış tabloları ve çeşitli ilkeler gibi özellikler, tüm etkinlik türleri için kullanılabilir. Ancak, typeProperties bölümünde kullanılabilen özellikler her etkinlik türüyle farklılık gösterir. Kopyalama etkinliği için, kaynak ve havuz türlerine göre farklılık gösterir.

Kopyalama etkinliği için, havuz **AzureSearchIndexSink**türünde olduğunda, typeproperties bölümünde aşağıdaki özellikler mevcuttur:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Dizinde bir belgenin zaten mevcut olduğu zaman birleştirilip birleştirilmeyeceğini veya değiştirip edilmeyeceğini belirtir. Bkz. [Writebehavior özelliği](#writebehavior-property).| Birleştir (varsayılan)<br/>Karşıya Yükle| Hayır |
| writeBatchSize | Arabellek boyutu writeBatchSize ulaştığında verileri Azure Search dizinine yükler. Ayrıntılar için bkz. [Writebatchsize özelliği](#writebatchsize-property) . | 1 ile 1.000 arasında. Varsayılan değer 1000 ' dir. | Hayır |

### <a name="writebehavior-property"></a>WriteBehavior özelliği
Veri yazarken AzureSearchSink upları. Diğer bir deyişle, bir belge yazarken belge anahtarı Azure Search dizininde zaten mevcutsa, bir çakışma özel durumu oluşturmak yerine mevcut belgeyi günceller Azure Search.

AzureSearchSink aşağıdaki iki yukarı yönlü davranışı sağlar (AzureSearch SDK kullanarak):

- **Birleştir**: yeni belgedeki tüm sütunları mevcut bir belge ile birleştirin. Yeni belgede null değeri olan sütunlarda, var olan bir değer korunur.
- **Karşıya yükle**: yeni belge var olanı yenisiyle değiştirir. Yeni belgede belirtilmeyen sütunlar için, varolan belgede null olmayan bir değer olup olmadığı için değer null olarak ayarlanır.

Varsayılan davranış **birleştirilir**.

### <a name="writebatchsize-property"></a>WriteBatchSize özelliği
Azure Search hizmet, toplu iş olarak belge yazmayı destekler. Batch, 1 ile 1.000 arasında eylem içerebilir. Bir eylem, karşıya yükleme/birleştirme işlemini gerçekleştirmek için bir belgeyi işler.

### <a name="data-type-support"></a>Veri türü desteği
Aşağıdaki tablo Azure Search veri türünün desteklenip desteklenmediğini belirtir.

| Azure Search veri türü | Azure Search havuzunda desteklenir |
| ---------------------- | ------------------------------ |
| Dize | E |
| Int32 | E |
| Int64 | E |
| Çift | E |
| Boole | E |
| Veri Timesapmayı | E |
| Dize dizisi | N |
| GeographyPoint | N |

## <a name="json-example-copy-data-from-on-premises-sql-server-to-azure-search-index"></a>JSON örneği: verileri şirket içi SQL Server Azure Search dizine kopyalama

Aşağıdaki örnek şunu gösterir:

1. [Azuresearch](#linked-service-properties)türünde bağlı bir hizmet.
2. [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties)türünde bağlı bir hizmet.
3. [Sqlservertable](data-factory-sqlserver-connector.md#dataset-properties)türünde bir giriş [veri kümesi](data-factory-create-datasets.md) .
4. [AzureSearchIndex](#dataset-properties)türünde bir çıkış [veri kümesi](data-factory-create-datasets.md) .
4. [SQLSource](data-factory-sqlserver-connector.md#copy-activity-properties) ve [AzureSearchIndexSink](#copy-activity-properties)kullanan kopyalama etkinliğine sahip bir işlem [hattı](data-factory-create-pipelines.md) .

Örnek, saat serisi verilerini şirket içi SQL Server veritabanından saatlik Azure Search dizine kopyalar. Bu örnekte kullanılan JSON özellikleri, örnekleri takip eden bölümlerde açıklanmıştır.

İlk adım olarak, şirket içi makinenizde veri yönetimi ağ geçidini kurun. Yönergeler, [Şirket içi konumlar ve bulut makaleleri arasında hareketli verilerde](data-factory-move-data-between-onprem-and-cloud.md) yer alır.

**Azure Search bağlı hizmet:**

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

**Bağlı hizmet SQL Server**

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

**SQL Server girişi veri kümesi**

Örnek, SQL Server bir "MyTable" tablosu oluşturduğunuzu ve zaman serisi verileri için "timestampcolumn" adlı bir sütun içerdiğini varsayar. Tek bir veri kümesi kullanarak aynı veritabanı içinde birden çok tablo üzerinde sorgulama yapabilirsiniz, ancak DataSet 'in tableName typeProperty için tek bir tablo kullanılmalıdır.

"External": "true" ayarı, veri kümesinin veri fabrikasında dış olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir Data Factory.

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

**Azure Search çıktı veri kümesi:**

Örnek, verileri **Products**adlı bir Azure Search dizinine kopyalar. Data Factory dizini oluşturmaz. Örneği test etmek için bu ada sahip bir dizin oluşturun. Giriş veri kümesiyle aynı sayıda sütuna sahip Azure Search dizinini oluşturun. Yeni girdiler her saat Azure Search dizinine eklenir.

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

**SQL kaynağı ve Azure Search dizin havuzu ile işlem hattındaki etkinliği kopyalama:**

İşlem hattı, giriş ve çıkış veri kümelerini kullanmak üzere yapılandırılmış bir kopyalama etkinliği içerir ve her saat çalışacak şekilde zamanlanır. İşlem hattı JSON tanımında **kaynak** türü **SQLSource** olarak ayarlanır ve **Havuz** türü **AzureSearchIndexSink**olarak ayarlanır. **Sqlreaderquery** özelliği IÇIN belirtilen SQL sorgusu, kopyalamanın Son saatteki verilerini seçer.

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

Bir bulut veri deposundaki verileri Azure Search içine kopyalıyorsanız, `executionLocation` özelliği gereklidir. Aşağıdaki JSON kod parçacığı, örnek olarak kopyalama etkinliği altında gereken değişikliği gösterir `typeProperties`. Desteklenen değerler ve daha fazla ayrıntı için [bulut veri depoları arasında veri kopyalama](data-factory-data-movement-activities.md#global) bölümüne bakın.

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


## <a name="copy-from-a-cloud-source"></a>Bulut kaynağından kopyalama
Bir bulut veri deposundaki verileri Azure Search içine kopyalıyorsanız, `executionLocation` özelliği gereklidir. Aşağıdaki JSON kod parçacığı, örnek olarak kopyalama etkinliği altında gereken değişikliği gösterir `typeProperties`. Desteklenen değerler ve daha fazla ayrıntı için [bulut veri depoları arasında veri kopyalama](data-factory-data-movement-activities.md#global) bölümüne bakın.

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

Ayrıca, kaynak veri kümesindeki sütunları, kopyalama etkinliği tanımındaki havuz veri kümesinden sütunlara eşleyebilirsiniz. Ayrıntılar için bkz. [Azure Data Factory veri kümesi sütunlarını eşleme](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Performans ve ayar
Veri taşıma (kopyalama etkinliği) performansını ve bunu iyileştirmek için çeşitli yollar hakkında bilgi edinmek için [etkinlik performansını ve ayarlama kılavuzunu kopyalama](data-factory-copy-activity-performance.md) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

* Kopyalama etkinliği ile işlem hattı oluşturmaya yönelik adım adım yönergeler için [etkinlik öğreticisini kopyalayın](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) .
