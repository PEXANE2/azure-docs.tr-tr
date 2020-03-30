---
title: Verileri SQL Server'a ve SQL Server'dan taşıma
description: Azure Veri Fabrikası'nı kullanarak verileri şirket içinde bulunan SQL Server veritabanına veya Azure VM'de nasıl taşıyarak nasıl taşıyabildiğini öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 5e4bbe1e6bd944787d47c5e3ed98de582c088a52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265772"
---
# <a name="move-data-to-and-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak verileri SQL Server'a şirket içinde veya IaaS'da (Azure VM) taşıyın
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-sqlserver-connector.md)
> * [Sürüm 2 (geçerli sürüm)](../connector-sql-server.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [V2'deki SQL Server bağlayıcısı bölümüne](../connector-sql-server.md)bakın.

Bu makalede, verileri şirket içi bir SQL Server veritabanına/şirket içi BIR SQL Server veritabanına taşımak için Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin nasıl kullanılacağı açıklanmaktadır. Kopya etkinliğiyle birlikte veri hareketine genel bir genel bakış sunan [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makalesine dayanmaktadır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Verileri SQL **Server veritabanından** aşağıdaki veri depolarına kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sink](../../../includes/data-factory-supported-sinks.md)]

Aşağıdaki veri depolarından bir **SQL Server veritabanına**veri kopyalayabilirsiniz:

[!INCLUDE [data-factory-supported-sources](../../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a>Desteklenen SQL Server sürümleri
Bu SQL Server bağlayıcısı, sql server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005, SQL Server 2005

## <a name="enabling-connectivity"></a>Bağlantı sağlama
Şirket içinde veya Azure IaaS (Hizmet Olarak Altyapı) VM'lerinde barındırılan SQL Server'a bağlanmak için gereken kavramlar ve adımlar aynıdır. Her iki durumda da bağlantı için Veri Yönetimi Ağ Geçidi'ni kullanmanız gerekir.

Veri Yönetimi Ağ Geçidi ve ağ geçidini ayarlama yla ilgili adım adım talimatlar hakkında bilgi edinmek için [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasında taşınan verileri görün. Ağ geçidi örneğini ayarlamak, SQL Server'a bağlanmak için bir ön koşuldur.

Ağ geçidini daha iyi performans için SQL Server ile aynı şirket içi makineye veya bulut VM örneğine yükleyebilirsiniz, ancak bunları ayrı makinelere yüklemenizi tavsiye ettik. Ağ geçidi ve SQL Server'ın ayrı makinelerde olması kaynak çekişmesini azaltır.

## <a name="getting-started"></a>Başlarken
Farklı araçlar/API'ler kullanarak verileri şirket içi BIR SQL Server veritabanına/şirket içi SQL Server veritabanına aktaran bir kopyalama etkinliği içeren bir ardışık kaynak oluşturabilirsiniz.

Bir ardışık yol oluşturmanın en kolay yolu **Kopyalama Sihirbazı'nı**kullanmaktır. Bkz. Öğretici: Veri kopyala sihirbazını kullanarak bir ardışık yol oluşturma konusunda hızlı bir geçiş için Kopya Sihirbazı kullanarak bir [ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

Bir ardışık kaynak oluşturmak için aşağıdaki araçları da kullanabilirsiniz: **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager şablonu**, **.NET API**ve **REST API**. Kopyalama etkinliği içeren bir ardışık hatlar oluşturmak için adım adım yönergeleri için [etkinlik öğreticisini](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.

Araçları veya API'leri kullanın, verileri kaynak veri deposundan bir lavabo veri deposuna aktaran bir ardışık işlem hattı oluşturmak için aşağıdaki adımları gerçekleştirirsiniz:

1. Bir **veri fabrikası**oluşturun. Bir veri fabrikası bir veya daha fazla ardışık hat lar içerebilir.
2. Giriş ve çıktı veri depolarını veri fabrikanıza bağlamak için **bağlantılı hizmetler** oluşturun. Örneğin, bir SQL Server veritabanından Azure blob depolamasına veri kopyalıyorsanız, SQL Server veritabanınızı ve Azure depolama hesabınızı veri fabrikanıza bağlamak için iki bağlantılı hizmet oluşturursunuz. SQL Server veritabanına özgü bağlantılı hizmet özellikleri için [bağlantılı hizmet özellikleri](#linked-service-properties) bölümüne bakın.
3. Kopyalama işlemi için giriş ve çıktı verilerini temsil edecek **veri kümeleri** oluşturun. Son adımda belirtilen örnekte, SQL Server veritabanınızda giriş verilerini içeren SQL tablosunu belirtmek için bir veri kümesi oluşturursunuz. Ayrıca, blob kapsayıcısını ve SQL Server veritabanından kopyalanan verileri tutan klasörü belirtmek için başka bir veri kümesi oluşturursunuz. SQL Server veritabanına özgü veri kümesi özellikleri için [dataset özellikleri](#dataset-properties) bölümüne bakın.
4. Giriş olarak veri kümesi ve çıktı olarak veri kümesi alan bir kopyalama etkinliği içeren bir **ardışık işlem oluşturma.** Daha önce bahsedilen örnekte, kaynak olarak SqlSource ve kopyalama etkinliği için bir lavabo olarak BlobSink kullanın. Benzer şekilde, Azure Blob Depolama'dan SQL Server Veritabanı'na kopyalıyorsanız, kopyalama etkinliğinde BlobSource ve SqlSink'i kullanırsınız. SQL Server Veritabanı'na özgü kopyalama etkinlik özellikleri için [kopyalama etkinlik özellikleri](#copy-activity-properties) bölümüne bakın. Kaynak veya lavabo olarak veri deposunun nasıl kullanılacağı yla ilgili ayrıntılar için, veri deponuzun önceki bölümündeki bağlantıyı tıklatın.

Sihirbazı kullandığınızda, bu Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri ve ardışık kuruluş) için JSON tanımları sizin için otomatik olarak oluşturulur. Araçları/API'leri (.NET API hariç) kullandığınızda, Bu Veri Fabrikası varlıklarını JSON biçimini kullanarak tanımlarsınız. Şirket içi bir SQL Server veritabanına/verileri kopyalamak için kullanılan Veri Fabrikası varlıkları için [JSON](#json-examples-for-copying-data-from-and-to-sql-server) tanımlı örnekler için bu makalenin JSON örnekleri bölümüne bakın.

Aşağıdaki bölümler, SQL Server'a özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan JSON özellikleri hakkında ayrıntılı bilgi sağlar:

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri
Şirket içi BIR SQL Server veritabanını bir veri fabrikasına bağlamak için **OnPremisesSqlServer** türünde bağlantılı bir hizmet oluşturursunuz. Aşağıdaki tablo, şirket içi SQL Server bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

Aşağıdaki tablo, SQL Server bağlantılı hizmete özgü JSON öğeleri için açıklama sağlar.

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| type |Tür özelliği ayarlanmalıdır: **OnPremisesSqlServer**. |Evet |
| Connectionstring |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak şirket içi SQL Server veritabanına bağlanmak için gereken connectionString bilgilerini belirtin. |Evet |
| ağ geçidiAdı |Veri Fabrikası hizmetinin şirket içi SQL Server veritabanına bağlanmak için kullanması gereken ağ geçidinin adı. |Evet |
| kullanıcı adı |Windows Kimlik Doğrulama'yı kullanıyorsanız kullanıcı adını belirtin. Örnek: **alan\\adı kullanıcı adı**. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabının parolasını belirtin. |Hayır |

Kimlik bilgilerini **New-AzDataFactoryEncryptValue** cmdlet kullanarak şifreleyebilir ve aşağıdaki örnekte gösterildiği gibi bağlantı dizesinde kullanabilirsiniz **(EncryptedCredential** özelliği):

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a>Örnekler
**SQL Kimlik Doğrulaması kullanmak için JSON**

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
**Windows Kimlik Doğrulama'yı kullanmak için JSON**

Veri Yönetimi Ağ Geçidi, şirket içi SQL Server veritabanına bağlanmak için belirtilen kullanıcı hesabını taklit edecektir.

```json
{
    "Name": " MyOnPremisesSQLDB",
    "Properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
            "username": "<domain\\username>",
            "password": "<password>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri
Örneklerde, SQL Server veritabanındaki bir tabloyu temsil etmek için **SqlServerTable** türünden bir veri kümesi kullandınız.

Veri kümelerini tanımlamak için kullanılabilen bölümlerin & özelliklerin tam listesi için [veri kümelerini oluşturma](data-factory-create-datasets.md) makalesine bakın. Bir veri kümesi JSON'un yapısı, kullanılabilirliği ve ilkesi gibi bölümler tüm veri kümesi türleri (SQL Server, Azure blob, Azure tablosu, vb.) için benzerdir.

typeProperties bölümü her veri kümesi türü için farklıdır ve veri deposundaki verilerin konumu hakkında bilgi sağlar. **SqlServerTable** türü veri kümesinin **typeProperties** bölümü aşağıdaki özelliklere sahiptir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| tableName |Bağlı hizmetin atıfta bulunduğu SQL Server Veritabanı örneğinde tablo nun veya görünümün adı. |Evet |

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri
Verileri bir SQL Server veritabanından taşıyorsanız, kopyalama etkinliğindeki kaynak türünü **SqlSource**olarak ayarlarsınız. Benzer şekilde, verileri bir SQL Server veritabanına taşıyorsanız, kopyalama etkinliğindeki lavabo türünü **SqlSink**olarak ayarlarsınız. Bu bölümde SqlSource ve SqlSink tarafından desteklenen özelliklerin bir listesini sağlar.

Etkinlikleri tanımlamak için kullanılabilen bölümlerin & özelliklerinin tam listesi [için, Kaynak Hatları Oluşturma](data-factory-create-pipelines.md) makalesine bakın. Ad, açıklama, giriş ve çıktı tabloları ve ilkeler gibi özellikler tüm etkinlik türleri için kullanılabilir.

> [!NOTE]
> Kopyalama Etkinliği yalnızca bir giriş alır ve yalnızca bir çıktı üretir.

Oysa, etkinliğin typeProperties bölümünde bulunan özellikler her etkinlik türüne göre değişir. Kopyalama etkinliği için, kaynak ve lavabo türlerine bağlı olarak değişir.

### <a name="sqlsource"></a>SqlSource
Bir kopyalama etkinliğindekaynak **SqlSource**türünde olduğunda, **typeProperties** bölümünde aşağıdaki özellikler kullanılabilir:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| sqlReaderQuery |Verileri okumak için özel sorguyu kullanın. |SQL sorgu dizesi. Örneğin: MyTable'dan * seçin. Giriş veri kümesi tarafından başvurulan veritabanından birden çok tabloya başvuru yapabilir. Belirtilmemişse, yürütülen SQL deyimi: MyTable'dan seçin. |Hayır |
| sqlReaderStoredProcedureNameName |Kaynak tablodaki verileri okuyan depolanan yordamın adı. |Depolanan yordamın adı. Son SQL deyimi, depolanan yordamda bir SELECT deyimi olmalıdır. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |

SqlSource için **sqlReaderQuery** belirtilirse, Kopyalama Etkinliği verileri almak için bu sorguyu SQL Server Veritabanı kaynağına göre çalıştırır.

Alternatif olarak, **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** (depolanan yordam parametreleri alıyorsa) belirterek depolanmış bir yordam belirtebilirsiniz.

SqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz, yapı bölümünde tanımlanan sütunlar SQL Server Veritabanı'na karşı çalıştırmak için select sorgusu oluşturmak için kullanılır. Veri kümesi tanımıyapısı yoksa, tüm sütunlar tablodan seçilir.

> [!NOTE]
> **sqlReaderStoredProcedureName**kullandığınızda, yine de veri kümesi JSON **tabloAdı** özelliği için bir değer belirtmeniz gerekir. Ancak bu tabloya karşı hiçbir doğrulama yoktur.

### <a name="sqlsink"></a>SqlSink
**SqlSink** aşağıdaki özellikleri destekler:

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| yazmaBatchTimeout |Toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresi. |Timespan<br/><br/> Örnek: "00:30:00" (30 dakika). |Hayır |
| yazmaBatchSize |Arabellek boyutu writeBatchSize'a ulaştığında verileri SQL tablosuna ekler. |Sonsayı (satır sayısı) |Hayır (varsayılan: 10000) |
| sqlWriterCleanupScript |Belirli bir dilimin verilerinin temizlenmesini sağlamak için Kopyalama Etkinliği sorgusubelirtin. Daha fazla bilgi için [yinelenebilir kopya](#repeatable-copy) bölümüne bakın. |Sorgu deyimi. |Hayır |
| sliceIdentifierSütun Adı |Yeniden çalıştırıldığında belirli bir dilimin verilerini temizlemek için kullanılan otomatik oluşturulan dilim tanımlayıcısıyla doldurmak için Kopyalama Etkinliği için sütun adını belirtin. Daha fazla bilgi için [yinelenebilir kopya](#repeatable-copy) bölümüne bakın. |Veri türü ikili olan bir sütunun sütun adı(32). |Hayır |
| sqlWriterStoredProcedureNameName |Kaynak verilerin hedef tabloya nasıl uygulanacağı nızı tanımlayan, örneğin kendi iş mantığınızı kullanarak uçlar yapmak veya dönüştürmek için depolanan yordamın adı. <br/><br/>Bu depolanan **yordamın toplu iş başına çağrılacağını**unutmayın. Yalnızca bir kez çalışan ve kaynak verilerle ilgisi olmayan bir işlem yapmak istiyorsanız, örneğin `sqlWriterCleanupScript` silme/budanma özelliğini kullanın. |Depolanan yordamın adı. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler. |Ad/değer çiftleri. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |
| sqlWriterTableType |Depolanan yordamda kullanılacak tablo türü adını belirtin. Kopyalama etkinliği, verilerin bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Depolanan yordam kodu daha sonra kopyalanan verileri varolan verilerle birleştirilebilir. |Tablo türü adı. |Hayır |


## <a name="json-examples-for-copying-data-from-and-to-sql-server"></a>SQL Server'dan ve SQL Server'dan veri kopyalamak için JSON örnekleri
Aşağıdaki örnekler, [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) veya [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)kullanarak bir ardışık hat lar oluşturmak için kullanabileceğiniz örnek JSON tanımları sağlar. Aşağıdaki örnekler, SQL Server ve Azure Blob Depolama'ya verilerin nasıl kopyalanır olduğunu gösterir. Ancak veriler, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği kullanılarak [doğrudan](data-factory-data-movement-activities.md#supported-data-stores-and-formats) herhangi bir **kaynaktan** burada belirtilen lavabolara kopyalanabilir.

## <a name="example-copy-data-from-sql-server-to-azure-blob"></a>Örnek: SQL Server'dan Azure Blob'a veri kopyalama
Aşağıdaki örnek gösterir:

1. [OnPremisesSqlServer](#linked-service-properties)türünde bağlantılı bir hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türüne bağlı bir hizmet.
3. [SqlServerTable](#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [SqlSource](#copy-activity-properties) ve [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties)kullanan Copy etkinliği ile [boru hattı.](data-factory-create-pipelines.md)

Örnek, zaman serisi verilerini bir SQL Server tablosundan her saat başı bir Azure blob'una kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

İlk adım olarak, veri yönetimi ağ geçidini kur. Yönergeler, [şirket içi konumlar ve bulut](data-factory-move-data-between-onprem-and-cloud.md) makalesi arasındaki hareketli verilerde dir.

**SQL Server bağlantılı hizmet**
```json
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
**Azure Blob depolama bağlantılı hizmet**

```json
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
**SQL Server giriş veri seti**

Örnek, SQL Server'da "MyTable" tablosu oluşturduğunuzu varsayar ve zaman serisi verileri için "zaman damgası sütunu" adlı bir sütun içerir. Tek bir veri kümesini kullanarak aynı veritabanı içinde birden çok tablo üzerinde sorgu yapabilirsiniz, ancak veri kümesinin tableName typeProperty için tek bir tablo kullanılmalıdır.

"Dış"ı ayarlamak: "true" veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```json
{
  "name": "SqlServerInput",
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
**Azure Blob çıktı veri kümesi**

Veriler her saat yeni bir blob 'a yazılır (sıklık: saat, aralık: 1). Blob için klasör yolu, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay, gün ve saat bölümlerini kullanır.

```json
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
**Kopyalama etkinliği olan boru hattı**

Ardışık iş, bu giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **SqlSource** olarak ayarlanır ve **lavabo** türü **BlobSink**olarak ayarlanır. **SqlReaderQuery** özelliği için belirtilen SQL sorgusu, kopyalanacak son bir saat içinde verileri seçer.

```json
{
  "name":"SamplePipeline",
  "properties":{
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
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
Bu örnekte, **sqlReaderQuery** sqlSource için belirtilir. Kopyalama Etkinliği, verileri almak için bu sorguyu SQL Server Veritabanı kaynağına karşı çalıştırAr. Alternatif olarak, **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** (depolanan yordam parametreleri alıyorsa) belirterek depolanmış bir yordam belirtebilirsiniz. sqlReaderQuery giriş veri kümesi tarafından başvurulan veritabanı içinde birden fazla tablo başvuru yapabilirsiniz. Yalnızca veri kümesinin tablo Adı typeProperty olarak ayarlanan tabloyla sınırlı değildir.

sqlReaderQuery veya sqlReaderStoredProcedureName belirtmezseniz, yapı bölümünde tanımlanan sütunlar SQL Server Veritabanı'na karşı çalıştırmak için select sorgusu oluşturmak için kullanılır. Veri kümesi tanımıyapısı yoksa, tüm sütunlar tablodan seçilir.

SqlSource ve BlobSink tarafından desteklenen özelliklerin listesi için [Sql Source](#sqlsource) bölümüne ve [BlobSink'e](data-factory-azure-blob-connector.md#copy-activity-properties) bakın.

## <a name="example-copy-data-from-azure-blob-to-sql-server"></a>Örnek: Azure Blob'dan SQL Server'a veri kopyalama
Aşağıdaki örnek gösterir:

1. [OnPremisesSqlServer](#linked-service-properties)türüne bağlı hizmet.
2. [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties)türünün bağlantılı hizmeti.
3. [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties)türünden bir giriş [veri kümesi.](data-factory-create-datasets.md)
4. [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties)türünden bir çıktı [veri kümesi.](data-factory-create-datasets.md)
5. [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) ve SqlSink kullanan Copy etkinliği olan [ardışık hatlar.](data-factory-create-pipelines.md)

Örnek, zaman serisi verilerini her saat başı bir Azure blob'undan SQL Server tablosuna kopyalar. Bu örneklerde kullanılan JSON özellikleri, örnekleri izleyen bölümlerde açıklanmıştır.

**SQL Server bağlantılı hizmet**

```json
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
**Azure Blob depolama bağlantılı hizmet**

```json
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
**Azure Blob giriş veri kümesi**

Veriler her saat yeni bir damladan alınır (sıklık: saat, aralık: 1). Blob için klasör yolu ve dosya adı, işlenen dilimin başlangıç saatine göre dinamik olarak değerlendirilir. Klasör yolu, başlangıç zamanının yıl, ay ve gün kısmını kullanır ve dosya adı başlangıç zamanının saat kısmını kullanır. "dış": "true" ayarı, veri kümesinin veri fabrikasının dışında olduğunu ve veri fabrikasındaki bir etkinlik tarafından üretilmediğini Veri Fabrikası hizmetine bildirir.

```json
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
**SQL Server çıktı veri seti**

Örnek, verileri SQL Server'da "MyTable" adlı bir tabloya kopyalar. Sql Server'da Blob CSV dosyasının içermesini beklediğiniz sütun sayısıyla tabloyu oluşturun. Her saat başı tabloya yeni satırlar eklenir.

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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
**Kopyalama etkinliği olan boru hattı**

Ardışık iş, bu giriş ve çıktı veri kümelerini kullanacak şekilde yapılandırılan ve her saat çalışacak şekilde zamanlanan bir Kopyalama Etkinliği içerir. JSON ardışık hatlar **tanımında, kaynak** türü **BlobSource** olarak ayarlanır ve **lavabo** türü **SqlSink**olarak ayarlanır.

```json
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
            "name": " SqlServerOutput "
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

## <a name="troubleshooting-connection-issues"></a>Bağlantı sorunlarını giderme
1. SQL Server'ınızı uzak bağlantıları kabul etmek için yapılandırın. **SQL Server Management Studio'yı**başlatın, **sunucuya**sağ tıklayın ve **Özellikler'i**tıklatın. Listeden **Bağlantılar'ı** seçin ve **sunucuya uzak bağlantılara izin ver'i**işaretleyin.

    ![Uzak bağlantıları etkinleştir](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    Bkz. Ayrıntılı adımlar için [uzaktan erişim Sunucu Yapılandırma Seçeneğini Yapılandırın.](https://msdn.microsoft.com/library/ms191464.aspx)
2. **SQL Server Configuration Manager'ı**başlatın. İstediğiniz örnek için **SQL Server Network Configuration'ı** genişletin ve **MSSQLSERVER için Protokoller'i**seçin. Protokolleri sağ bölmede görmelisin. TCP/IP'ye sağ tıklayarak ve Etkinleştir'i tıklatarak TCP/IP'yi **etkinleştirin.** **TCP/IP**

    ![TCP/IP'yi etkinleştirme](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    TCP/IP protokolünü etkinleştirmenin ayrıntıları ve alternatif yolları için [Sunucu Ağı Protokolünü Etkinleştir veya Devre Dışı Kınama](https://msdn.microsoft.com/library/ms191294.aspx) bölümüne bakın.
3. Aynı pencerede, **TCP/IP Özellikleri** penceresini başlatmak için **TCP/IP'yi** çift tıklatın.
4. **IP Adresleri** sekmesine geçin. **IPTümü** bölümünü görmek için aşağı kaydırın. **TCP Bağlantı Noktası'nı**not edin (varsayılan **değer 1433'dür).**
5. Bu bağlantı noktasından gelen trafiğe izin vermek için makinedeki **Windows Güvenlik Duvarı için** bir kural oluşturun.
6. **Bağlantıyı doğrula**: TAM nitelikli ad kullanarak SQL Server'a bağlanmak için farklı bir makineden SQL Server Management Studio'yu kullanın. Örneğin: "\<\>makine . \<etki\>alanı\<.corp. company\>.com,1433."

   > [!IMPORTANT]
   > 
   > Ayrıntılı bilgi için [Veri Yönetimi Ağ Geçidi ile şirket içi kaynaklar ve bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md) ya da veri taşıma hakkında bilgi bakın.
   > 
   > Sorun giderme bağlantısı/ağ geçidi yle ilgili sorunlarla ilgili ipuçları için [sorun giderme ağ geçidi sorunlarına](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) bakın.


## <a name="identity-columns-in-the-target-database"></a>Hedef veritabanındaki kimlik sütunları
Bu bölümde, kimlik sütunu olmayan bir kaynak tablodaki verileri kimlik sütunu olan bir hedef tabloya kopyalayan bir örnek verilmektedir.

**Kaynak tablo:**

```sql
create table dbo.SourceTbl
(
    name varchar(100),
    age int
)
```
**Hedef tablo:**

```sql
create table dbo.TargetTbl
(
    identifier int identity(1,1),
    name varchar(100),
    age int
)
```

Hedef tablonun bir kimlik sütunu olduğuna dikkat edin.

**Kaynak veri kümesi JSON tanımı**

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
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

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
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
Bir ardışık işlemin kopya etkinliğinde SQL lavabodan depolanan yordamı çağırma örneği [için kopyalama etkinliği makalesinde SQL lavabosu için Invoke saklı yordamı](data-factory-invoke-stored-procedure-from-copy-activity.md) bakın.

## <a name="type-mapping-for-sql-server"></a>SQL sunucusu için tür eşleme
[Veri hareketi etkinlikleri](data-factory-data-movement-activities.md) makalesinde belirtildiği gibi, Kopyalama etkinliği aşağıdaki 2 adımlı yaklaşımla kaynak türlerinden lavabo türlerine otomatik tür dönüşümleri gerçekleştirir:

1. Yerel kaynak türlerinden .NET türüne dönüştürme
2. .NET türünden yerel lavabo türüne dönüştürme

Verileri SQL sunucusundan & aktarırken, sql türünden .NET türüne aşağıdaki eşlemeler kullanılır ve bunun tersi de kullanılır.

Eşleme, ADO.NET için SQL Server Veri Türü Eşlemi ile aynıdır.

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

## <a name="mapping-source-to-sink-columns"></a>Batmak için kaynak eşleme
Kaynak veri kümesinden sütunlara kadar sütunları haritalamak için Azure [Veri Fabrikası'nda veri kümesi sütunlarını eşleme](data-factory-map-columns.md)konusuna bakın.

## <a name="repeatable-copy"></a>Tekrarlanabilir kopya
Verileri SQL Server Database'e kopyalarken, kopyalama etkinliği varsayılan olarak verileri lavabo tablosuna ekler. Bunun yerine bir UPSERT gerçekleştirmek [için SqlSink makalesine yinelenebilir yazma'ya](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) bakın.

İlişkisel veri depolarından veri kopyalarken, istenmeyen sonuçlardan kaçınmak için tekrarlanabilirliği aklınızda bulundurun. Azure Veri Fabrikası'nda, bir dilimi el ile yeniden çalıştırabilirsiniz. Bir hata oluştuğunda dilimin yeniden çalıştırılaması için bir veri kümesi için yeniden deneme ilkesini de yapılandırabilirsiniz. Bir dilim her iki şekilde de yeniden çalıştırıldığında, bir dilim kaç kez çalıştırılırsa çalıştırın aynı verilerin okunduğundan emin olmanız gerekir. Bkz. [İlişkisel kaynaklardan tekrarlanabilir okuma.](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Performans ve Tuning
Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli faktörler (Kopyalama Etkinliği) ve bunu optimize etmenin çeşitli yolları hakkında bilgi edinmek için [Etkinlik performansını & Tuning Kılavuzu'na](data-factory-copy-activity-performance.md) bakın.
