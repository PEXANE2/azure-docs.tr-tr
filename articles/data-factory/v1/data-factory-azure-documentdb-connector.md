---
title: Verileri Azure Cosmos DB'ye/Azure Cosmos DB'ye taşıma
description: Azure Veri Fabrikası'nı kullanarak verileri Azure Cosmos DB koleksiyonuna/azure'dan nasıl taşıyın
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: c9297b71-1bb4-4b29-ba3c-4cf1f5575fac
ms.service: multiple
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: a638184d5232de916ebd25360147301a93309dd9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260520"
---
# <a name="move-data-to-and-from-azure-cosmos-db-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Azure Cosmos DB'ye taşıyın
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-azure-documentdb-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-cosmos-db.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Azure Cosmos DB konektörüne](../connector-azure-cosmos-db.md)bakın.

Bu makalede, verileri Azure Cosmos DB'ye (SQL API) taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

Desteklenen herhangi bir kaynak veri deposundan Azure Cosmos DB'ye veya Azure Cosmos DB'den desteklenen herhangi bir lavabo veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın.

> [!IMPORTANT]
> Azure Cosmos DB konektörü yalnızca SQL API'sini destekler.

JSON dosyalarına veya başka bir Cosmos DB koleksiyonundan verileri olduğu gibi kopyalamak [için, Alma/Dışa Aktarma JSON belgelerine](#importexport-json-documents)bakın.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri Azure Cosmos DB'ye/veya Azure Cosmos DB'ye aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun.
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.**

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Cosmos DB'ye/Cosmos DB'ye veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı örnekler için bu makalenin [JSON örnekleri](#json-examples) bölümüne bakın.

Aşağıdaki bölümler, Cosmos DB'ye özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Aşağıdaki tablo, Azure Cosmos DB bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **DocumentDb** |Evet |
| Connectionstring |Azure Cosmos DB veritabanına bağlanmak için gereken bilgileri belirtin. |Evet |

Örnek:

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi için lütfen [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

typeProperties bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **DocumentDbCollection** türüveri kümesi için typeProperties bölümü aşağıdaki özelliklere sahiptir.

| **Özellik** | **Açıklama** | **Gerekli** |
| --- | --- | --- |
| Toplamaadı |Cosmos DB belge koleksiyonunun adı. |Evet |

Örnek:

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
### <a name="schema-by-data-factory"></a>Veri Fabrikası'na Göre Şema
Azure Cosmos DB gibi şema içermeyen veri depoları için Veri Fabrikası hizmeti şema'yı aşağıdaki yollardan biriyle ortaya çıkartır:

1. Veri kümesi tanımındaki **yapı** özelliğini kullanarak verilerin yapısını belirtirseniz, Veri Fabrikası hizmeti bu yapıyı şema olarak onurlandırıyor. Bu durumda, bir satır bir sütun için bir değer içermiyorsa, bunun için null bir değer sağlanacaktır.
2. Veri kümesi tanımındaki **yapı** özelliğini kullanarak verilerin yapısını belirtmezseniz, Veri Fabrikası hizmeti verideki ilk satırı kullanarak şemayı çıkartır. Bu durumda, ilk satır tam şemayı içermiyorsa, kopyalama işlemi sonucunda bazı sütunlar eksik olur.

Bu nedenle, şema içermeyen veri kaynakları için en iyi **yöntem, yapı** özelliğini kullanarak verilerin yapısını belirtmektir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi için lütfen [Boru Hattı Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

> [!NOTE]
> Kopyalama Etkinliği yalnızca bir giriş alır ve yalnızca bir çıktı üretir.

Diğer taraftan, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir ve Kopyalama etkinliği durumunda kaynak ve lavabo türlerine bağlı olarak değişir.

Kaynak **DocumentDbCollectionSource** türünde olduğunda Kopyalama etkinliği durumunda aşağıdaki özellikler **typeProperties** bölümünde mevcuttur:

| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| sorgu |Verileri okumak için sorguyu belirtin. |Azure Cosmos DB tarafından desteklenen sorgu dizesi. <br/><br/>Örnek: `SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Hayır <br/><br/>Belirtilmemişse, yürütülen SQL deyimi:`select <columns defined in structure> from mycollection` |
| nestingSeparator |Belgenin iç içe olduğunu gösteren özel karakter |Herhangi bir karakter. <br/><br/>Azure Cosmos DB, Iç içe yapıların izin verildiği JSON belgeleri için bir NoSQL deposudur. Azure Veri Fabrikası, kullanıcının nestingSeparator aracılığıyla hiyerarşiyi belirtmesini sağlar, yani "." yukarıdaki örneklerde. Ayırıcı ile kopyalama etkinliği, tablo tanımındaki "Name.First", "Name.Middle" ve "Name.Last" ad,orta ve soyad öğelerinden "Ad" nesnesini oluşturur. |Hayır |

**DocumentDbCollectionSink** aşağıdaki özellikleri destekler:

| **Özellik** | **Açıklama** | **İzin verilen değerler** | **Gerekli** |
| --- | --- | --- | --- |
| nestingSeparator |İç içe belge nin gerekli olduğunu belirtmek için kaynak sütun adında özel bir karakter. <br/><br/>Yukarıdaki örnekte: `Name.First` çıktı tablosunda Cosmos DB belgesinde aşağıdaki JSON yapısını oluşturur:<br/><br/>"Adı": {<br/>    "İlk": "John"<br/>}, |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir.<br/><br/>Varsayılan değer `.` (nokta). |İç içe geçme düzeylerini ayırmak için kullanılan karakterdir. <br/><br/>Varsayılan değer `.` (nokta). |
| yazmaBatchSize |Belge oluşturmak için Azure Cosmos DB hizmetine paralel istek sayısı.<br/><br/>Bu özelliği kullanarak Cosmos DB'ye/Cosmos DB'ye veri kopyalarken performansta ince ayar yapabilirsiniz. Cosmos DB'ye daha fazla paralel istek gönderildiğinden, writeBatchSize'ı artırdığınızda daha iyi bir performans bekleyebilirsiniz. Ancak hata iletisi atabilir azaltma önlemek gerekir: "İstek oranı büyüktür".<br/><br/>Azaltma, belgelerin boyutu, belgelerdeki terimlerin sayısı, hedef toplama nın dizine eklenmesi vb. gibi bir dizi faktöre göre belirlenir. Kopyalama işlemleri için, en fazla iş elde etmek için daha iyi bir koleksiyon (örn. S3) kullanabilirsiniz (2.500 istek birimi/saniye). |Tamsayı |Hayır (varsayılan: 5) |
| yazmaBatchTimeout |Zaman geçmeden işlemin tamamlanması için zaman bekleyin. |Timespan<br/><br/> Örnek: "00:30:00" (30 dakika). |Hayır |

## <a name="importexport-json-documents"></a>İthalat/İhracat JSON belgeleri
Bu Cosmos DB konektörünü kullanarak, kolayca

* Azure Blob, Azure Veri Gölü, şirket içi Dosya Sistemi veya Azure Veri Fabrikası tarafından desteklenen diğer dosya tabanlı mağazalar dahil olmak üzere çeşitli kaynaklardan JSON belgelerini Cosmos DB'ye aktarın.
* Cosmos DB koleksiyonundaki JSON belgelerini çeşitli dosya tabanlı mağazalara aktarın.
* Verileri olduğu gibi iki Cosmos DB koleksiyonu arasında geçirin.

Böyle şema-agnostik kopya elde etmek için,
* Kopyalama sihirbazını **kullanırken, "JSON dosyalarına veya Cosmos DB koleksiyonuna olduğu gibi dışa** aktar" seçeneğini işaretleyin.
* JSON düzenlemesini kullanırken, kopya etkinliğinde Cosmos DB kaynak/lavabosundaki Cosmos DB dataset(ler) veya "nestingSeparator" özelliğindeki "yapı" bölümünü belirtmeyin. JSON dosyalarından alma/dışa aktarma için, dosya deposunda veri kümesinde biçim türünü "JsonFormat", config "filePattern" olarak belirtin ve geri kalan biçim ayarlarını atlayın, ayrıntılardaki [JSON biçimi](data-factory-supported-file-and-compression-formats.md#json-format) bölümüne bakın.

## <a name="json-examples"></a>JSON örnekleri
Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Azure Cosmos DB ve Azure Blob Depolama'ya ve Bunlardan gelen verilerin nasıl kopyalanır olduğunu gösterirler. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [doğrudan](data-factory-data-movement-activities.md#supported-data-stores-and-formats) **kaynaklardan** herhangi bir lavaboya kopyalanabilir.

## <a name="example-copy-data-from-azure-cosmos-db-to-azure-blob"></a>Örnek: Azure Cosmos DB'den Azure Blob'a veri kopyalama
Aşağıdaki örnek te gösterir:

1. [DocumentDb](#linked-service-properties)türüne bağlı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [DocumentDbCollection](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [DocumentDbCollectionSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Kopyalama Etkinliği ile bir [ardışık.](data-factory-create-pipelines.md)

Örnek, Azure Cosmos DB'deki verileri Azure Blob'a kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure Cosmos DB bağlantılı hizmet:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Azure Blob depolama bağlantılı hizmet:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Belge DB giriş veri kümesi:**

Örnek, Azure Cosmos DB veritabanında **Kişi** adlı bir koleksiyona sahip olduğunuzu varsayar.

"Dış"u ayarlama: "true" ve dış Veri ilkesi bilgilerini belirten Azure Veri Fabrikası hizmeti, tablonun veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini belirtir.

```JSON
{
  "name": "PersonCosmosDbTable",
  "properties": {
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```

**Azure Blob çıktı veri seti:**

Veriler, saat parçalı belirli bir tarihle belirli datetime yansıtan blob yolu ile her saat yeni bir blob kopyalanır.

```JSON
{
  "name": "PersonBlobTableOut",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Cosmos DB veritabanındaki Kişi koleksiyonundaki örnek JSON belgesi:

```JSON
{
  "PersonId": 2,
  "Name": {
    "First": "Jane",
    "Middle": "",
    "Last": "Doe"
  }
}
```
Cosmos DB, hiyerarşik JSON belgeleri üzerinde sözdizimi gibi bir SQL kullanarak belgeleri sorgulamayı destekler.

Örnek:

```sql
SELECT Person.PersonId, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person
```

Aşağıdaki ardışık alan, Azure Cosmos DB veritabanındaki Kişi koleksiyonundaki verileri Azure blob'una kopyalar. Kopyalama etkinliğinin bir parçası olarak giriş ve çıktı veri kümeleri belirtilmiştir.

```JSON
{
  "name": "DocDbToBlobPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "DocumentDbCollectionSource",
            "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
            "nestingSeparator": "."
          },
          "sink": {
            "type": "BlobSink",
            "blobWriterAddHeader": true,
            "writeBatchSize": 1000,
            "writeBatchTimeout": "00:00:59"
          }
        },
        "inputs": [
          {
            "name": "PersonCosmosDbTable"
          }
        ],
        "outputs": [
          {
            "name": "PersonBlobTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromDocDbToBlob"
      }
    ],
    "start": "2015-04-01T00:00:00Z",
    "end": "2015-04-02T00:00:00Z"
  }
}
```
## <a name="example-copy-data-from-azure-blob-to-azure-cosmos-db"></a>Örnek: Azure Blob'dan Azure Cosmos DB'ye veri kopyalama
Aşağıdaki örnek te gösterir:

1. DocumentDb türünün bağlantılı bir hizmeti.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. DocumentDbCollection türünün çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve DocumentDbCollectionSink kullanan Kopyalama Etkinliği içeren bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, Azure blob'undan Azure Cosmos DB'ye kadar olan verileri kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure Blob depolama bağlantılı hizmet:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Cosmos DB bağlantılı hizmet:**

```JSON
{
  "name": "CosmosDbLinkedService",
  "properties": {
    "type": "DocumentDb",
    "typeProperties": {
      "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
    }
  }
}
```
**Azure Blob giriş veri kümesi:**

```JSON
{
  "name": "PersonBlobTableIn",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "MiddleName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "fileName": "input.csv",
      "folderPath": "docdb",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "nullValue": "NULL"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
**Azure Cosmos DB çıktı veri seti:**

Örnek verileri "Kişi" adlı bir koleksiyona kopyalar.

```JSON
{
  "name": "PersonCosmosDbTableOut",
  "properties": {
    "structure": [
      {
        "name": "Id",
        "type": "Int"
      },
      {
        "name": "Name.First",
        "type": "String"
      },
      {
        "name": "Name.Middle",
        "type": "String"
      },
      {
        "name": "Name.Last",
        "type": "String"
      }
    ],
    "type": "DocumentDbCollection",
    "linkedServiceName": "CosmosDbLinkedService",
    "typeProperties": {
      "collectionName": "Person"
    },
    "availability": {
      "frequency": "Day",
      "interval": 1
    }
  }
}
```
Aşağıdaki ardışık alan, Azure Blob'daki verileri Cosmos DB'deki Kişi koleksiyonuna kopyalar. Kopyalama etkinliğinin bir parçası olarak giriş ve çıktı veri kümeleri belirtilmiştir.

```JSON
{
  "name": "BlobToDocDbPipeline",
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "DocumentDbCollectionSink",
            "nestingSeparator": ".",
            "writeBatchSize": 2,
            "writeBatchTimeout": "00:00:00"
          },
          "translator": {
              "type": "TabularTranslator",
              "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
          }
        },
        "inputs": [
          {
            "name": "PersonBlobTableIn"
          }
        ],
        "outputs": [
          {
            "name": "PersonCosmosDbTableOut"
          }
        ],
        "policy": {
          "concurrency": 1
        },
        "name": "CopyFromBlobToDocDb"
      }
    ],
    "start": "2015-04-14T00:00:00Z",
    "end": "2015-04-15T00:00:00Z"
  }
}
```
Örnek blob girişi aşağıdaki gibi ise

```
1,John,,Doe
```
Sonra çıkış JSON Cosmos DB olarak olacaktır:

```JSON
{
  "Id": 1,
  "Name": {
    "First": "John",
    "Middle": null,
    "Last": "Doe"
  },
  "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
}
```
Azure Cosmos DB, Iç içe yapıların izin verildiği JSON belgeleri için bir NoSQL deposudur. Azure Veri Fabrikası, kullanıcının **nestingSeparator**aracılığıyla hiyerarşiyi belirtmesini sağlar , yani "." bu örnekte. Ayırıcı ile kopyalama etkinliği, tablo tanımındaki "Name.First", "Name.Middle" ve "Name.Last" ad,orta ve soyad öğelerinden "Ad" nesnesini oluşturur.

## <a name="appendix"></a>Ek
1. **Soru:** Kopyalama Etkinliği varolan kayıtların güncelleştirinir mi?

    **Cevap:** №.
2. **Soru:** Azure Cosmos DB'ye kopyalanmış bir kopyanın yeniden denemesi, zaten kopyalanmış kayıtlarla nasıl başa çıkıyor?

    **Cevap:** Kayıtlarda bir "kimlik" alanı varsa ve kopyalama işlemi aynı kimlikle bir kayıt eklemeye çalışırsa, kopyalama işlemi bir hata atar.
3. **Soru:** Veri Fabrikası [aralıklarını veya karma tabanlı veri bölümlemelerini](../../cosmos-db/sql-api-partition-data.md)destekler mi?

    **Cevap:** №.
4. **Soru:** Bir tablo için birden fazla Azure Cosmos DB koleksiyonu belirtebilir miyim?

    **Cevap:** №. Şu anda yalnızca bir koleksiyon belirtilebilir.

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
