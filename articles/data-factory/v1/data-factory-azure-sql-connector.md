---
title: Azure SQL Veritabanına/Azure SQL Veritabanından veri kopyalama
description: Azure Veri Fabrikası'nı kullanarak Azure SQL Veritabanı'na verileri nasıl kopyalayın?'ı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 7fc0b2822195d952c2a4f9c02bf3758c0e2b809a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260507"
---
# <a name="copy-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Azure SQL Veritabanı'na veri kopyalama
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-azure-sql-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-azure-sql-database.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki Azure SQL Veritabanı bağlayıcısı bölümüne](../connector-azure-sql-database.md)bakın.

Bu makalede, verileri Azure SQL Veritabanı'na ve Azure Veritabanı'na taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Verileri Azure **SQL Veritabanı'ndan** aşağıdaki veri depolarına kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sinks](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından Azure **SQL Veritabanına**veri kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a>Desteklenen kimlik doğrulama türü
Azure SQL Veritabanı bağlayıcısı temel kimlik doğrulamasını destekler.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri Azure SQL Veritabanına/Ondan'ya aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Bir **veri fabrikası**oluşturun. Bir veri fabrikası bir veya daha fazla ardışık hat lar içerebilir.
2. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun. Örneğin, bir Azure blob depolama sundaki verileri Azure SQL veritabanına kopyalıyorsanız, Azure depolama hesabınızı ve Azure SQL veritabanınızı veri fabrikanıza bağlamak için iki bağlantılı hizmet oluşturursunuz. Azure SQL Veritabanı'na özgü bağlantılı hizmet özellikleri için [bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.
3. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. Son adımda belirtilen örnekte, giriş verilerini içeren blob kapsayıcısını ve klasörünü belirtmek için bir veri kümesi oluşturursunuz. Ayrıca, Azure SQL veritabanında blob depolamadan kopyalanan verileri tutan SQL tablosunu belirtmek için başka bir veri kümesi oluşturursunuz. Azure Veri Gölü Deposu'na özgü veri kümesi özellikleri için [bkz.](#dataset-properties)
4. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** Daha önce bahsedilen örnekte, blobSource'u kaynak olarak, SqlSink'i ise kopyalama etkinliği için lavabo olarak kullanırsınız. Benzer şekilde, Azure SQL Veritabanı'ndan Azure Blob Depolama'ya kopyalıyorsanız, kopyalama etkinliğinde SqlSource ve BlobSink'i kullanırsınız. Azure SQL Veritabanı'na özgü kopyalama etkinlik özellikleri için [kopyalama etkinlik özellikleri](#copy-activity-properties) bölümüne bakın. Kaynak veya lavabo olarak veri deposunun nasıl kullanılacağı yla ilgili ayrıntılar için, veri deponuzun önceki bölümündeki bağlantıyı tıklatın.

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Azure SQL Veritabanına/Azure SQL Veritabanından veri kopyalamak için kullanılan Veri Fabrikası varlıkları için JSON tanımlı örnekler için bu makalenin [JSON örnekleri](#json-examples-for-copying-data-to-and-from-sql-database) bölümüne bakın.

Aşağıdaki bölümler, Azure SQL Veritabanı'na özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Azure SQL bağlantılı bir hizmet, bir Azure SQL veritabanını veri fabrikanıza bağlar. Aşağıdaki tablo, Azure SQL bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği şu şekilde ayarlanmalıdır: **AzureSqlDatabase** |Evet |
| Connectionstring |ConnectionString özelliği için Azure SQL Veritabanı örneğine bağlanmak için gereken bilgileri belirtin. Yalnızca temel kimlik doğrulaması desteklenir. |Evet |

> [!IMPORTANT]
> [Azure Hizmetlerinin sunucuya erişmesine izin vermek için](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)Azure SQL Veritabanı Güvenlik [Duvarı'nı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) veritabanı sunucusuna yapılandırın. Ayrıca, veri fabrikası ağ geçidine sahip şirket içi veri kaynakları da dahil olmak üzere Azure dışındaki Azure SQL Veritabanı'na veri kopyalıyorsanız, Azure SQL Veritabanı'na veri gönderen makine için uygun IP adres aralığını yapılandırın.

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Azure SQL veritabanındaki giriş veya çıktı verilerini temsil edecek bir veri kümesi belirtmek için, veri kümesinin tür özelliğini şu şekilde ayarlarsınız: **AzureSqlTable**. Veri kümesinin **linkedServiceName** özelliğini Azure SQL bağlantılı hizmetin adına ayarlayın.

Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (Azure SQL, Azure blob, Azure tablosu, vb.) için benzerdir.

typeProperties bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **AzureSqlTable** türünün veri kümesinin **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu Azure SQL Veritabanı örneğinde tablonun veya görünümün adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilke gibi özellikler tüm etkinlik türleri için kullanılabilir.

> [!NOTE]
> Kopyalama Etkinliği yalnızca bir giriş alır ve yalnızca bir çıktı üretir.

Oysa, etkinliğin **typeProperties** bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

Verileri Bir Azure SQL veritabanından taşıyorsanız, kopyalama etkinliğindeki kaynak türünü **SqlSource**olarak ayarlarsınız. Benzer şekilde, verileri bir Azure SQL veritabanına taşıyorsanız, kopyalama etkinliğindeki lavabo türünü **SqlSink**olarak ayarlarsınız. Bu bölümde SqlSource ve SqlSink tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sqlsource"></a>SqlSource
Kopyalama etkinliğinde, kaynak **SqlSource**türünde olduğunda, **typeProperties** bölümünde aşağıdaki özellikler mevcuttur:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örnek: `select * from MyTable`. |Hayır |
| sqlReaderStoredProcedureNameName |Kaynak tablodaki verileri okuyan depolanan yordamın adı. |Depolanan yordamın adı. Son SQL deyimi, depolanan yordamda bir SELECT deyimi olmalıdır. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |

SqlSource için **sqlReaderQuery** belirtilirse, Kopyalama Etkinliği verileri almak için bu sorguyu Azure SQL Veritabanı kaynağına göre çalıştırır. Alternatif olarak, **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** (depolanan yordam parametreleri alıyorsa) belirterek depolanmış bir yordam belirtebilirsiniz.

sqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz, JSON veri kümesinin yapı bölümünde tanımlanan sütunlar, Azure`select column1, column2 from mytable`SQL Veritabanı'na karşı çalışmak üzere bir sorgu ( ) oluşturmak için kullanılır. Veri kümesi tanımıyapısı yoksa, tüm sütunlar tablodan seçilir.

> [!NOTE]
> **sqlReaderStoredProcedureName**kullandığınızda, yine de veri kümesi JSON **tabloAdı** özelliği için bir değer belirtmeniz gerekir. Ancak bu tabloya karşı hiçbir doğrulama yoktur.
>
>

### <a name="sqlsource-example"></a>SqlSource örneği

```JSON
"source": {
    "type": "SqlSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```

**Depolanan yordam tanımı:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqlsink"></a>SqlSink
**SqlSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi. |Timespan<br/><br/> Örnek: "00:30:00" (30 dakika). |Hayır |
| yazmaBatchSize |Arabellek boyutu writeBatchSize'a ulaştığında verileri SQL tablosuna ekler. |Sonsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimin verilerinin temizlenmesi ni gerçekleştirmek için Kopyalama Etkinliği için bir sorgu belirtin. Daha fazla bilgi için [yinelenebilir kopyaya](#repeatable-copy)bakın. |Sorgu deyimi. |Hayır |
| sliceIdentifierSütun Adı |Yeniden çalıştırıldığında belirli bir dilimin verilerini temizlemek için kullanılan otomatik oluşturulan dilim tanımlayıcısıyla doldurmak için Kopyalama Etkinliği için bir sütun adı belirtin. Daha fazla bilgi için [yinelenebilir kopyaya](#repeatable-copy)bakın. |Veri türü ikili olan bir sütunun sütun adı(32). |Hayır |
| sqlWriterStoredProcedureNameName |Kaynak verilerin hedef tabloya nasıl uygulanacağı nızı tanımlayan, örneğin kendi iş mantığınızı kullanarak uçlar yapmak veya dönüştürmek için depolanan yordamın adı. <br/><br/>Bu depolanan **yordamın toplu iş başına çağrılacağını**unutmayın. Yalnızca bir kez çalışan ve kaynak verilerle ilgisi olmayan bir işlem yapmak istiyorsanız, örneğin `sqlWriterCleanupScript` silme/budanma özelliğini kullanın. |Depolanan yordamın adı. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |
| sqlWriterTableType |Depolanan yordamda kullanılacak bir tablo türü adı belirtin. Kopyalama etkinliği, verilerin bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Depolanan yordam kodu daha sonra kopyalanan verileri varolan verilerle birleştirilebilir. |Tablo türü adı. |Hayır |

#### <a name="sqlsink-example"></a>SqlSink örneği

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-sql-database"></a>SQL Veritabanına ve SQL Veritabanından veri kopyalamak için JSON örnekleri
Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Azure SQL Veritabanı'na ve Azure Blob Depolama'ya veri kopyalamanın nasıl yapılacağını gösterirler. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [doğrudan](data-factory-data-movement-activities.md#supported-data-stores-and-formats) herhangi bir **kaynaktan** burada belirtilen lavabolara kopyalanabilir.

### <a name="example-copy-data-from-azure-sql-database-to-azure-blob"></a>Örnek: Azure SQL Veritabanı'ndan Azure Sql Blob'a veri kopyalama
Aynı aşağıdaki Veri Fabrikası varlıkları tanımlar:

1. [AzureSqlDatabase](#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [AzureSqlTable](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [SqlSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan bir Kopyalama etkinliği olan bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, zaman serisi verilerini (saatlik, günlük vb.) Azure SQL veritabanındaki bir tablodan her saat bir blob'a kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure SQL Veritabanı bağlantılı hizmet:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Bu bağlantılı hizmet tarafından desteklenen özellikler listesi için Azure SQL Bağlantılı Hizmet bölümüne bakın.

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
Bu bağlantılı hizmet tarafından desteklenen özellikler listesi için [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) makalesine bakın.


**Azure SQL giriş veri kümesi:**

Örnek, Azure SQL'de "MyTable" tablosu oluşturduğunuzu varsayar ve zaman serisi verileri için "zaman damgası sütunu" adlı bir sütun içerir.

"Dış"ı ayarlamak: "true" Azure Veri Fabrikası hizmetine veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini bildirir.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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

Bu veri kümesi türü tarafından desteklenen özellikler listesi için Azure SQL veri kümesi türü özellikleri bölümüne bakın.

**Azure Blob çıktı veri seti:**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
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
Bu veri kümesi türü tarafından desteklenen özellikler listesi için [Azure Blob veri kümesi türü özellikleri](data-factory-azure-blob-connector.md#dataset-properties) bölümüne bakın.

**SQL kaynağı ve Blob lavabosu olan bir ardışık ardışık ardışık alanda kopyalama etkinliği:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **SqlSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **SqlReaderQuery** özelliği için belirtilen SQL sorgusu, kopyalanacak son bir saat içinde verileri seçer.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "AzureSQLtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
Örnekte, **sqlReaderQuery** sqlSource için belirtilir. Kopyalama Etkinliği, verileri almak için bu sorguyu Azure SQL Veritabanı kaynağına göre çalıştırAr. Alternatif olarak, **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** (depolanan yordam parametreleri alıyorsa) belirterek depolanmış bir yordam belirtebilirsiniz.

sqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz, JSON veri kümesinin yapı bölümünde tanımlanan sütunlar Azure SQL Veritabanı'na karşı çalışacak bir sorgu oluşturmak için kullanılır. Örneğin: `select column1, column2 from mytable`. Veri kümesi tanımıyapısı yoksa, tüm sütunlar tablodan seçilir.

SqlSource ve BlobSink tarafından desteklenen özelliklerin listesi için [Sql Source](#sqlsource) bölümüne ve [BlobSink'e](data-factory-azure-blob-connector.md#copy-activity-properties) bakın.

### <a name="example-copy-data-from-azure-blob-to-azure-sql-database"></a>Örnek: Azure Blob'dan Azure SQL Veritabanı'na veri kopyalama
Örnek, aşağıdaki Veri Fabrikası varlıklarını tanımlar:

1. [AzureSqlDatabase](#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureSqlTable](#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve [SqlSink](#copy-activity-properties)kullanan Copy etkinliği olan bir [ardışık kaynak.](data-factory-create-pipelines.md)

Örnek, zaman serisi verilerini (saatlik, günlük vb.) Azure blob'undan Azure SQL veritabanındaki bir tabloya her saat kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**Azure SQL bağlantılı hizmet:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Bu bağlantılı hizmet tarafından desteklenen özellikler listesi için Azure SQL Bağlantılı Hizmet bölümüne bakın.

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
Bu bağlantılı hizmet tarafından desteklenen özellikler listesi için [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) makalesine bakın.


**Azure Blob giriş veri kümesi:**

Veriler her saat yeni bir damladan alınır (sıklık: saat, aralık: 1). Blob için klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay ve gün kısmını kullanır ve dosya adı başlangıç zamanının saat kısmını kullanır. "dış": "true" ayarı, veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
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
Bu veri kümesi türü tarafından desteklenen özellikler listesi için [Azure Blob veri kümesi türü özellikleri](data-factory-azure-blob-connector.md#dataset-properties) bölümüne bakın.

**Azure SQL Veritabanı çıktı veri kümesi:**

Örnek, verileri Azure SQL'de "MyTable" adlı bir tabloya kopyalar. Tabloyu Azure SQL'de Blob CSV dosyasının içermesini beklediğiniz sütunlarla aynı sayıda sütunla oluşturun. Her saat başı tabloya yeni satırlar eklenir.

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
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
Bu veri kümesi türü tarafından desteklenen özellikler listesi için Azure SQL veri kümesi türü özellikleri bölümüne bakın.

**Blob kaynağı ve SQL lavabosu olan bir ardışık ardışık ardışık alanda kopyalama etkinliği:**

Ardışık iş, giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **BlobSource** olarak ayarlanır ve **lavabo** türü **SqlSink**olarak ayarlanır.

```JSON
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
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
SqlSink ve BlobSource tarafından desteklenen özelliklerin listesi için [Sql Sink](#sqlsink) bölümüne ve [BlobSource'a](data-factory-azure-blob-connector.md#copy-activity-properties) bakın.

## <a name="identity-columns-in-the-target-database"></a>Hedef veritabanındaki kimlik sütunları
Bu bölümde, kimlik sütunu olmayan bir kaynak tablodaki verileri kimlik sütunlu bir hedef tabloya kopyalamak için bir örnek verilmektedir.

**Kaynak tablo:**

```SQL
create table dbo.SourceTbl
(
    name varchar(100),
    age int
)
```
**Hedef tablo:**

```SQL
create table dbo.TargetTbl
(
    identifier int identity(1,1),
    name varchar(100),
    age int
)
```
Hedef tablonun bir kimlik sütunu olduğuna dikkat edin.

**Kaynak veri kümesi JSON tanımı**

```JSON
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Hedef veri kümesi JSON tanımı**

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

Kaynak ve hedef tablofarklı şema (hedef kimlik ile ek bir sütun vardır) var dikkat edin. Bu senaryoda, kimlik sütununa dahil olmayan hedef veri kümesi tanımında **yapı** özelliğini belirtmeniz gerekir.

## <a name="invoke-stored-procedure-from-sql-sink"></a>SQL lavabodan depolanan yordamı çağırma
Bir ardışık işlemin kopya etkinliğinde SQL lavabodan depolanan yordamı çağırmak [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) örneği için bkz.

## <a name="type-mapping-for-azure-sql-database"></a>Azure SQL Veritabanı için tür eşleme
[Veri hareketi etkinliklerinde](data-factory-data-movement-activities.md) belirtildiği gibi makale Kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüştürmeleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Verileri Azure SQL Veritabanı'na ve Azure SQL Veritabanı'na aktarırken, aşağıdaki eşlemeler SQL türünden .NET türüne veya tam tersi olarak kullanılır. Eşleme, ADO.NET için SQL Server Veri Türü Eşlemi ile aynıdır.

| SQL Server Veritabanı Motoru türü | .NET Framework türü |
| --- | --- |
| bigint |Int64 |
| ikili |Bayt[] |
| bit |Boole |
| char |Dize, Char[] |
| date |DateTime |
| Tarih saat |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Ondalık |Ondalık |
| FILESTREAM özniteliği (varbinary(max)) |Bayt[] |
| Kayan |Çift |
| image |Bayt[] |
| int |Int32 |
| Para |Ondalık |
| Nchar |Dize, Char[] |
| Ntext |Dize, Char[] |
| sayısal |Ondalık |
| nvarchar |Dize, Char[] |
| gerçek |Tek |
| Rowversion |Bayt[] |
| Smalldatetime |DateTime |
| smallint |Int16 |
| Smallmoney |Ondalık |
| Sql_variant |Nesne * |
| metin |Dize, Char[] |
| time |TimeSpan |
| timestamp |Bayt[] |
| tinyint |Bayt |
| uniqueidentifier |Guid |
| Varbinary |Bayt[] |
| varchar |Dize, Char[] |
| xml |Xml |

## <a name="map-source-to-sink-columns"></a>Sütunları batırmak için harita kaynağı
Kaynak veri kümesindeki sütunları lavabo veri kümesindeki sütunlara eşleme hakkında bilgi edinmek için Azure [Veri Fabrikası'ndaki veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-copy"></a>Tekrarlanabilir kopya
Verileri SQL Server Database'e kopyalarken, kopyalama etkinliği varsayılan olarak verileri lavabo tablosuna ekler. Bunun yerine bir UPSERT gerçekleştirmek [için SqlSink makalesine yinelenebilir yazma'ya](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) bakın.

İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
