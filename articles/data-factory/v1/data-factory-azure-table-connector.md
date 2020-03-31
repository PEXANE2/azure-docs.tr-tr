---
title: Verileri Azure Tablosu'na/Azure Tablosundan taşıma
description: Azure Veri Fabrikası'nı kullanarak verileri Azure Tablo Depolama'ya/Azure Tablo Depolama'ya nasıl taşıyarak taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 462d54a9d89d6f03aed5e221fa02609da786c8c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260455"
---
# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri Azure Tablosu'na ve Azure Tablosu'na taşıma
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-azure-table-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-table-storage.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Azure Tablo Depolama bağlayıcısına](../connector-azure-table-storage.md)bakın.

Bu makalede, verileri Azure Tablo Depolama'ya/Azure Depolama'ya taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır. 

Desteklenen herhangi bir kaynak veri deposundan Azure Table Storage'a veya Azure Tablo Depolama'dan desteklenen herhangi bir lavabo veri deposuna veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tablosuna bakın. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri Azure Tablo Depolama alanına/ondan'ya aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın. 

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz: 

1. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun.
2. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. 
3. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** 

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Azure Tablo Depolama Alanına/Azure Tablo Depolama'dan veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı örnekler için bu makalenin [JSON örnekleri](#json-examples) bölümüne bakın.

Aşağıdaki bölümler, Azure Tablo Depolamasına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılar sağlar: 

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Azure blob depolama alanını Azure veri fabrikasına bağlamak için kullanabileceğiniz iki tür bağlantılı hizmet vardır. Bunlar: **AzureStorage** bağlantılı hizmet ve **AzureStorageSas** bağlantılı hizmet. Azure Depolama bağlantılı hizmet, veri fabrikasına Azure Depolama'ya genel erişim sağlar. Ancak Azure Depolama SAS (Paylaşılan Erişim İmzası) bağlantılı hizmet, veri fabrikasına Azure Depolama'ya sınırlı/zamana bağlı erişim sağlar. Bu iki bağlantılı hizmet arasında başka bir fark yoktur. İhtiyaçlarınıza uygun bağlantılı hizmeti seçin. Aşağıdaki bölümlerde bu iki bağlantılı hizmet hakkında daha fazla ayrıntı sağlayabilirsiniz.

[!INCLUDE [data-factory-azure-storage-linked-services](../../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

typeProperties bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **AzureTable** türünün veri kümesinin **typeProperties** bölümü aşağıdaki özelliklere sahiptir.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu Azure Tablo Veritabanı örneğindeki tablonun adı. |Evet. Bir tableName azureTableSourceQuery olmadan belirtildiğinde, tablodaki tüm kayıtlar hedefe kopyalanır. AzureTableSourceQuery de belirtilirse, sorguyu karşılayan tablodaki kayıtlar hedefe kopyalanır. |

### <a name="schema-by-data-factory"></a>Veri Fabrikası'na Göre Şema
Azure Tablo gibi şema içermeyen veri depoları için, Veri Fabrikası hizmeti şema'yı aşağıdaki yollardan biriyle sonuçlandırIr:

1. Veri kümesi tanımındaki **yapı** özelliğini kullanarak verilerin yapısını belirtirseniz, Veri Fabrikası hizmeti bu yapıyı şema olarak onurlandırıyor. Bu durumda, bir satır bir sütun için bir değer içermiyorsa, bunun için null bir değer sağlanır.
2. Veri kümesi tanımındaki **yapı** özelliğini kullanarak verilerin yapısını belirtmezseniz, Veri Fabrikası verilerdeki ilk satırı kullanarak şema çıkar. Bu durumda, ilk satır tam şemayı içermiyorsa, kopyalama işlemi sonucunda bazı sütunlar gözden kaçırılır.

Bu nedenle, şema içermeyen veri kaynakları için en iyi **yöntem, yapı** özelliğini kullanarak verilerin yapısını belirtmektir.

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı veri kümeleri ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir.

Diğer taraftan etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

**AzureTableSource,** typeProperties bölümünde aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableSourceQuery |Verileri okumak için özel sorguyu kullanın. |Azure tablo sorgu dizesi. Sonraki bölümdeki örneklere bakın. |Hayır. Bir tableName azureTableSourceQuery olmadan belirtildiğinde, tablodaki tüm kayıtlar hedefe kopyalanır. AzureTableSourceQuery de belirtilirse, sorguyu karşılayan tablodaki kayıtlar hedefe kopyalanır. |
| azureTableSourceIgnoreTableNotFound |Tablonun özel durum yutmak olup olmadığını belirtin. |TRUE<br/>FALSE |Hayır |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery örnekleri
Azure Tablo sütunu dize türündeyse:

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

Azure Tablo sütunu datetime türündeyse:

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

**AzureTableSink,** typeProperties bölümünde aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Lavabo tarafından kullanılabilecek varsayılan bölüm anahtar değeri. |Dize değeri. |Hayır |
| azureTablePartitionKeyName |Değerleri bölüm anahtarları olarak kullanılan sütunun adını belirtin. Belirtilmemişse, AzureTableDefaultPartitionKeyValue bölüm anahtarı olarak kullanılır. |Sütun adı. |Hayır |
| azureTableTableKeyName |Sütun değerleri satır anahtarı olarak kullanılan sütunun adını belirtin. Belirtilmemişse, her satır için bir GUID kullanın. |Sütun adı. |Hayır |
| azureTableInsertType |Azure tablosuna veri ekleme modu.<br/><br/>Bu özellik, eşleşen bölüm ve satır anahtarları ile çıktı tablosunda varolan satırların değerlerinin değiştirilip değiştirilmediğini veya birleştirilip birleştirilmediğini denetler. <br/><br/>Bu ayarların (birleştirme ve değiştirme) nasıl çalıştığı hakkında bilgi edinmek için Varlık [Ekle veya Birleştir](https://msdn.microsoft.com/library/azure/hh452241.aspx) ve Varlık Ekle veya [Değiştir](https://msdn.microsoft.com/library/azure/hh452242.aspx) konularına bakın. <br/><br> Bu ayar, tablo düzeyinde değil, satır düzeyinde geçerlidir ve her iki seçenek de girişte bulunmayan çıktı tablosundaki satırları siler. |birleştirme (varsayılan)<br/>Değiştirmek |Hayır |
| yazmaBatchSize |WriteBatchSize veya writeBatchTimeout vurulduğunda Azure tablosuna veri ekler. |Sonsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| yazmaBatchTimeout |WriteBatchSize veya writeBatchTimeout vurulduğunda Verileri Azure tablosuna ekler |Timespan<br/><br/>Örnek: "00:20:00" (20 dakika) |Hayır (Varsayılan depolama istemcisi varsayılan zaman kaybı değeri 90 sn) |

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Hedef sütunu azureTablePartitionKeyName olarak kullanmadan önce, çevirmen JSON özelliğini kullanarak bir hedef sütunu hedef sütunla eşle.

Aşağıdaki örnekte, kaynak sütun DivisionID hedef sütuna eşlenir: DivisionID.  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
DivisionID bölüm anahtarı olarak belirtilir.

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a>JSON örnekleri
Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Azure Tablo Depolama ve Azure Tablo Depolama Veritabanı'na ve bu verilerden nasıl kopyalanırlarını gösterirler. Ancak, veriler **doğrudan** kaynaklardan herhangi biri tarafından desteklenen lavabolara kopyalanabilir. Daha fazla bilgi için, [Kopyalama Etkinliği'ni kullanarak Verileri Taşı'ndaki](data-factory-data-movement-activities.md)"Desteklenen veri depoları ve biçimleri" bölümüne bakın.

## <a name="example-copy-data-from-azure-table-to-azure-blob"></a>Örnek: Azure Table'dan Azure Blob'a veri kopyalama
Aşağıdaki örnek gösterir:

1. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) türüne bağlı bir hizmet (her iki tablo & blob için kullanılır).
2. [AzureTable](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
3. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
4. AzureTableSource ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy etkinliği ile [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, Azure Tablosundaki varsayılan bölüme ait verileri her saat başı bir blob'a kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure depolama bağlantılı hizmet:**

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
Azure Veri Fabrikası iki tür Azure Depolama bağlantılı hizmeti destekler: **AzureStorage** ve **AzureStorageSas.** İlki için, hesap anahtarını içeren bağlantı dizesini belirtirsiniz ve daha sonraki nde Paylaşılan Erişim İmzası (SAS) Uri'yi belirtirsiniz. Ayrıntılar için [Bağlantılı Hizmetler](#linked-service-properties) bölümüne bakın.  

**Azure Tablo giriş veri kümesi:**

Örnek, Azure Tablosu'nda "MyTable" tablosu oluşturduğunuzu varsayar.

"Dış"ı ayarlamak: "true" veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**AzureTableSource ve BlobSink ile bir ardışık ardışık alanda etkinliği kopyalama:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. Boru hattı JSON tanımında, **kaynak** türü **AzureTableSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **AzureTableSourceQuery** özelliğiile belirtilen SQL sorgusu, kopyalanacak her saat teki varsayılan bölümden verileri seçer.

```JSON
{
    "name":"SamplePipeline",
    "properties":{
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="example-copy-data-from-azure-blob-to-azure-table"></a>Örnek: Azure Blob'dan Azure Tablosu'na veri kopyalama
Aşağıdaki örnek gösterir:

1. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) türüne bağlı bir hizmet (her iki tablo & blob için kullanılır)
2. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
3. [AzureTable](#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
4. [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [AzureTableSink](#copy-activity-properties)kullanan Copy etkinliği ile [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, zaman serisi verilerini bir Azure örneğinden bir Azure tablosuna saatlik olarak kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure depolama (hem Azure Tablo & Blob) bağlantılı hizmet için:**

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

Azure Veri Fabrikası iki tür Azure Depolama bağlantılı hizmeti destekler: **AzureStorage** ve **AzureStorageSas.** İlki için, hesap anahtarını içeren bağlantı dizesini belirtirsiniz ve daha sonraki nde Paylaşılan Erişim İmzası (SAS) Uri'yi belirtirsiniz. Ayrıntılar için [Bağlantılı Hizmetler](#linked-service-properties) bölümüne bakın.

**Azure Blob giriş veri kümesi:**

Veriler her saat yeni bir damladan alınır (sıklık: saat, aralık: 1). Blob için klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay ve gün kısmını kullanır ve dosya adı başlangıç zamanının saat kısmını kullanır. "dış": "true" ayarı, veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
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
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
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

**Azure Tablo çıktı veri kümesi:**

Örnek, verileri Azure Tablosu'nda "MyTable" adlı bir tabloya kopyalar. Blob CSV dosyasının içermesini beklediğiniz aynı sayıda sütuna sahip bir Azure tablosu oluşturun. Her saat başı tabloya yeni satırlar eklenir.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**BlobSource ve AzureTableSink ile bir ardışık ardışık alanda etkinliği kopyalama:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **BlobSource** olarak ayarlanır ve **lavabo** türü **AzureTableSink**olarak ayarlanır.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
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
## <a name="type-mapping-for-azure-table"></a>Azure Tablosu için Tür Eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki iki adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir.

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Azure Tablo'dan & verileri aktarırken, Azure Tablo hizmeti tarafından tanımlanan aşağıdaki [eşlemeler](https://msdn.microsoft.com/library/azure/dd179338.aspx) Azure Table OData türlerinden .NET türüne veya tam tersi olarak kullanılır.

| OData Veri Türü | .NET Türü | Ayrıntılar |
| --- | --- | --- |
| Edm.İkili |bayt[] |64 KB'ye kadar bayt dizisi. |
| Edm.Boolean |bool |Boole değeri. |
| Edm.DateTime |DateTime |Eşgüdümlü Evrensel Zaman (UTC) olarak ifade edilen 64 bitlik bir değer. Desteklenen DateTime aralığı 1 Ocak 1601'de gece yarısı saat 12:00'den itibaren başlar. (C.E.), UTC. Aralık 31 Aralık 9999 tarihinde sona erer. |
| Edm.Double |double |64 bit kayan nokta değeri. |
| Edm.Guid |Guid |128 bit küresel olarak benzersiz tanımlayıcı. |
| Edm.Int32 |Int32 |32 bit'lik bir tamsayı. |
| Edm.Int64 |Int64 |64 bit'lik bir tamsayı. |
| Edm.String |Dize |UTF-16 kodlanmış bir değer. Dize değerleri 64 KB'ye kadar olabilir. |

### <a name="type-conversion-sample"></a>Tür Dönüştürme Örneği
Aşağıdaki örnek, tür dönüşümleriyle birlikte Azure Blob'dan Azure Tablosu'na veri kopyalamak içindir.

Blob veri kümesinin CSV biçiminde olduğunu ve üç sütun içerdiğini varsayalım. Bunlardan biri, haftanın günü için kısaltılmış Fransızca adlar kullanan özel bir datetime biçimine sahip bir datetime sütunudur.

Sütunlar için yazı tanımları ile birlikte Blob Kaynak veri kümesini aşağıdaki gibi tanımlayın.

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
        "structure":
        [
            { "name": "userid", "type": "Int64"},
            { "name": "name", "type": "String"},
            { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
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
Azure Table OData türünden .NET türüne tür eşleme göz önüne alındığında, aşağıdaki şemaya sahip Azure Tablosu'ndaki tabloyu tanımlarsınız.

**Azure Tablo şeması:**

| Sütun adı | Tür |
| --- | --- |
| Userıd |Edm.Int64 |
| ad |Edm.String |
| Lastlogindate |Edm.DateTime |

Ardından, Azure Tablosu veri kümesini aşağıdaki gibi tanımlayın. Tür bilgileri altta yatan veri deposunda zaten belirtildiğinden, tür bilgilerini içeren "yapı" bölümünü belirtmeniz gerekmez.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

Bu durumda, Veri Fabrikası, Blob'dan Azure Tablo'ya veri taşınırken "fr-fr" kültürünü kullanarak Datetime alanı da dahil olmak üzere özel datetime biçimi içeren tür dönüşümleri otomatik olarak yapar.

> [!NOTE]
> Kaynak veri kümesinden sütunlara kadar sütunları haritalamak için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin (Kopyalama Etkinliği) performansını etkileyen önemli faktörler ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için, Etkinlik Performansını & [Auning Kılavuzu'na](data-factory-copy-activity-performance.md)bakın.
