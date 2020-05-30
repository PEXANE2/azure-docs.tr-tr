---
title: SQL Server veri kopyalama
description: Azure Data Factory kullanarak şirket içinde veya Azure VM 'de bulunan SQL Server veritabanına veri taşıma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/29/2020
ms.openlocfilehash: e08dca8fee93aa80b585f0df30a1534b6705821c
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84221366"
---
# <a name="copy-data-to-and-from-sql-server-by-using-azure-data-factory"></a>Azure Data Factory kullanarak SQL Server veri kopyalama

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Data Factory sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-sqlserver-connector.md)
> * [Güncel sürüm](connector-sql-server.md)
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, verileri bir SQL Server veritabanına kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SQL Server Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)

SQL Server veritabanından desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Ya da desteklenen kaynak veri mağazalarından verileri bir SQL Server veritabanına kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SQL Server Bağlayıcısı şunları destekler:

- 2016, 2014, 2012, 2008 R2, 2008 ve 2005 sürümlerini SQL Server.
- SQL veya Windows kimlik doğrulaması kullanarak veri kopyalama.
- Kaynak olarak, bir SQL sorgusu veya saklı yordam kullanarak verileri alma.
- Havuz olarak, kaynak şemaya bağlı değilse, hedef tablo otomatik olarak oluşturuluyor; verileri bir tabloya ekleme veya kopyalama sırasında özel mantık ile saklı yordam çağırma. 

[SQL Server Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-2017) desteklenmez.

>[!NOTE]
>SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) artık bu bağlayıcı tarafından desteklenmiyor. Geçici bir çözüm için [Genel BIR ODBC Bağlayıcısı](connector-odbc.md) ve SQL Server ODBC sürücüsü kullanabilirsiniz. ODBC sürücü indirme ve bağlantı dizesi yapılandırmalarına sahip [Bu kılavuzu](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=sql-server-2017) izleyin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, SQL Server veritabanı bağlayıcısına özgü Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

SQL Server bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği **SqlServer**olarak ayarlanmalıdır. | Evet |
| Dizisi |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak SQL Server veritabanına bağlanmak için gereken **ConnectionString** bilgilerini belirtin. Aşağıdaki örneklere bakın.<br/>Ayrıca, Azure Key Vault bir parola koyabilirsiniz. SQL kimlik doğrulaması ise, `password` yapılandırmayı bağlantı dizesinin dışına çekin. Daha fazla bilgi için, Azure Key Vault tablo ve [Mağaza kimlik bilgilerini](store-credentials-in-key-vault.md)izleyen JSON örneğine bakın. |Evet |
| userName |Windows kimlik doğrulaması kullanıyorsanız, bir Kullanıcı adı belirtin. **DomainName \\ Kullanıcı adı**bir örnektir. |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabı için bir parola belirtin. Azure Data Factory güvenli bir şekilde depolamak için bu alanı **SecureString** olarak işaretleyin. Veya [Azure Key Vault depolanan bir gizli](store-credentials-in-key-vault.md)dizi ile başvurabilirsiniz. |Hayır |
| connectVia | Bu [tümleştirme çalışma zamanı](concepts-integration-runtime.md) , veri deposuna bağlanmak için kullanılır. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

>[!TIP]
>"UserErrorFailedToConnectToSqlServer" hata koduyla bir hatayla karşılaşırsanız ve "veritabanı için oturum sınırı XXX ve ulaşıldığında", `Pooling=false` Bağlantı dizenizi ekleyin ve yeniden deneyin.

**Örnek 1: SQL kimlik doğrulaması kullanma**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 2: Azure Key Vault bir parolayla SQL kimlik doğrulaması kullanma**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek 3: Windows kimlik doğrulamasını kullanma**

```json
{
    "name": "SqlServerLinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>\\<instance name if using named instance>;Initial Catalog=<databasename>;Integrated Security=True;",
            "userName": "<domain\\username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm SQL Server veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Ve SQL Server veritabanından veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Veri kümesinin Type özelliği **Sqlservertable**olarak ayarlanmalıdır. | Evet |
| manızı | Şemanın adı. |Kaynak için Hayır, havuz için Evet  |
| tablo | Tablo/görünüm adı. |Kaynak için Hayır, havuz için Evet  |
| tableName | Şema ile tablonun/görünümün adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü için `schema` ve kullanın `table` . | Kaynak için Hayır, havuz için Evet |

**Örneğinde**

```json
{
    "name": "SQLServerDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<SQL Server linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "schema": "<schema_name>",
            "table": "<table_name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Etkinlikleri tanımlamak için kullanılabilecek bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm SQL Server kaynak ve havuz tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sql-server-as-a-source"></a>Kaynak olarak SQL Server

SQL Server verileri kopyalamak için kopyalama etkinliğindeki kaynak türünü **SQLSource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik kaynağını kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının Type özelliği **SQLSource**olarak ayarlanmalıdır. | Evet |
| sqlReaderQuery |Verileri okumak için özel SQL sorgusunu kullanın. `select * from MyTable` bunun bir örneğidir. |Hayır |
| sqlReaderStoredProcedureName |Bu özellik, kaynak tablodaki verileri okuyan saklı yordamın adıdır. Son SQL ifadesinin saklı yordamda bir SELECT ifadesinin olması gerekir. |Hayır |
| storedProcedureParameters |Bu parametreler, saklı yordama yöneliktir.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve büyük harfleri, saklı yordam parametrelerinin adlarıyla ve büyük küçük harfleriyle eşleşmelidir. |Hayır |
| 'Sinden | SQL kaynağı için işlem kilitleme davranışını belirtir. İzin verilen değerler: **ReadCommitted** (varsayılan), **READUNCOMMITTED**, **RepeatableRead**, **Serializable**, **Snapshot**. Daha fazla ayrıntı için [Bu belgeye](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) başvurun. | Hayır |

**Şunlara işaret eder:**

- SQLSource için **Sqlreaderquery** belirtilmişse **SqlSource**, kopyalama etkinliği verileri almak için bu sorguyu SQL Server kaynağına göre çalıştırır. Saklı yordam parametreleri alırsa, **sqlReaderStoredProcedureName** ve **storedProcedureParameters** belirterek bir saklı yordam de belirtebilirsiniz.
- **Sqlreaderquery** veya **SQLREADERSTOREDPROCEDURENAME**belirtmezseniz, JSON veri kümesinin "yapı" bölümünde tanımlanan sütunlar bir sorgu oluşturmak için kullanılır. Sorgu `select column1, column2 from mytable` SQL Server karşı çalışır. Veri kümesi tanımında "Structure" yoksa, tablodan tüm sütunlar seçilir.

**Örnek: SQL sorgusunu kullanın**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Örnek: saklı yordam kullanma**

```json
"activities":[
    {
        "name": "CopyFromSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Server input dataset name>",
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
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Saklı yordam tanımı**

```sql
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

### <a name="sql-server-as-a-sink"></a>Havuz olarak SQL Server

> [!TIP]
> [SQL Server ' ye veri yüklemeye yönelik en iyi](#best-practice-for-loading-data-into-sql-server)uygulamalardan desteklenen yazma davranışları, konfigürasyonlar ve en iyi uygulamalar hakkında daha fazla bilgi edinin.

SQL Server verileri kopyalamak için kopyalama etkinliğindeki havuz türünü **Sqlsink**olarak ayarlayın. Aşağıdaki özellikler, kopyalama etkinliği havuzu bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği havuzunun Type özelliği **Sqlsink**olarak ayarlanmalıdır. | Evet |
| Ön Copyscrıpt |Bu özellik, SQL Server içine veri yazmadan önce, kopyalama etkinliğinin çalıştırılacağı bir SQL sorgusu belirtir. Her kopya çalıştırması için yalnızca bir kez çağrılır. Bu özelliği, önceden yüklenmiş verileri temizlemek için kullanabilirsiniz. |Hayır |
| tableOption | Kaynak şemasına göre yoksa havuz tablosunun otomatik olarak oluşturulup oluşturulmayacağını belirtir. Havuz saklı yordamı belirttiğinde veya hazırlanan kopya kopyalama etkinliğinde yapılandırıldığında otomatik tablo oluşturma desteklenmez. İzin verilen değerler: `none` (varsayılan), `autoCreate` . |Hayır |
| sqlWriterStoredProcedureName | Hedef tabloya kaynak verilerinin nasıl uygulanacağını tanımlayan saklı yordamın adı. <br/>Bu saklı yordam *toplu iş başına çağırılır*. Yalnızca bir kez çalıştırılan ve kaynak verilerle hiçbir şey olmayan işlemler için, örneğin, DELETE veya TRUNCATE, `preCopyScript` özelliğini kullanın.<br>[BIR SQL havuzundan saklı yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink)örneğine bakın. | Hayır |
| storedProcedureTableTypeParameterName |Saklı yordamda belirtilen tablo türünün parametre adı.  |Hayır |
| sqlWriterTableType |Saklı yordamda kullanılacak tablo türü adı. Kopyalama etkinliği, verileri bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle Kopyalanmakta olan verileri birleştirebilir. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>İzin verilen değerler ad ve değer çiftleridir. Parametrelerin adları ve büyük harfleri, saklı yordam parametrelerinin adlarıyla ve büyük küçük harfleriyle aynı olmalıdır. | Hayır |
| writeBatchSize |*Toplu iş BAŞıNA*SQL tablosuna eklenecek satır sayısı.<br/>İzin verilen değerler, satır sayısı için tamsayılardır. Varsayılan olarak, Azure Data Factory satır boyutuna göre uygun toplu iş boyutunu dinamik olarak belirler. |Hayır |
| writeBatchTimeout |Bu özellik, toplu ekleme işleminin zaman aşımına uğramadan önce tamamlaması için bekleme süresini belirtir.<br/>İzin verilen değerler TimeSpan içindir. 30 dakika boyunca "00:30:00" bir örnektir. Hiçbir değer belirtilmemişse, zaman aşımı varsayılan olarak "02:00:00" olur. |Hayır |

**Örnek 1: veri ekleme**

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "tableOption": "autoCreate",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Örnek 2: kopyalama sırasında saklı yordam çağırma**

[BIR SQL havuzundan saklı yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink)hakkında daha fazla bilgi edinin.

```json
"activities":[
    {
        "name": "CopyToSQLServer",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Server output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "storedProcedureTableTypeParameterName": "MyTable",
                "sqlWriterTableType": "MyTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="best-practice-for-loading-data-into-sql-server"></a>SQL Server içine veri yüklemek için en iyi uygulama

SQL Server verileri kopyaladığınızda, farklı yazma davranışı gerekebilir:

- [Append](#append-data): Kaynak verilerinizde yalnızca yeni kayıtlar vardır.
- [Upsert](#upsert-data): Kaynak verilerinizde hem ekler hem de güncelleştirmeler vardır.
- [Üzerine yaz](#overwrite-the-entire-table): her seferinde boyut tablosunun tamamını yeniden yüklemek istiyorum.
- [Özel mantık Ile yaz](#write-data-with-custom-logic): hedef tabloya son ekleme yapmadan önce fazladan işleme ihtiyacım var.

Azure Data Factory ve en iyi yöntemleri yapılandırmak için ilgili bölümlere bakın.

### <a name="append-data"></a>Veri Ekle

Verilerin eklenmesi, bu SQL Server havuz bağlayıcısının varsayılan davranışıdır. Azure Data Factory tablonuza verimli bir şekilde yazmak için toplu bir ekleme yapar. Kopyalama etkinliğinde kaynağı ve havuzu uygun şekilde yapılandırabilirsiniz.

### <a name="upsert-data"></a>Verileri upsert etme

**Seçenek 1:** Kopyalamak için büyük miktarda veriniz varsa, kopyalama etkinliğini kullanarak tüm kayıtları bir hazırlama tablosuna toplu olarak yükleyebilir, sonra bir bir çekde [birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=sql-server-ver15) veya ekleme/güncelleştirme ifadesini uygulamak için bir saklı yordam etkinliği çalıştırabilirsiniz. 

Kopyalama etkinliği şu anda verileri veritabanı geçici tablosuna yüklemeyi yerel olarak desteklemiyor. Birden çok etkinliğin birleşimiyle ayarlanmasının gelişmiş bir yolu vardır ve [SQL veritabanı toplu ön Ekle senaryolarına en iyileştirme](https://github.com/scoriani/azuresqlbulkupsert)bölümüne bakın. Aşağıda, kalıcı bir tablonun hazırlama olarak kullanılması örneği gösterilmektedir.

Örnek olarak, Azure Data Factory, **saklı yordam etkinliği**ile **kopyalama etkinliği** zincirli bir işlem hattı oluşturabilirsiniz. Eski, kaynak deponuzdaki verileri SQL Server hazırlama tablosuna kopyalar, örneğin, **UpsertStagingTable**, veri kümesindeki tablo adı olarak. İkinci olarak, hazırlama tablosundaki kaynak verileri hedef tabloya birleştirmek ve hazırlama tablosunu temizlemek için bir saklı yordam çağırır.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

Veritabanınızda, önceki saklı yordam etkinliğinden işaret edilen aşağıdaki örnekte olduğu gibi, BIRLEŞTIRME mantığı ile bir saklı yordam tanımlayın. Hedefin üç sütunlu **Pazarlama** tablosu olduğunu varsayın: **ProfileId**, **State**ve **category**. **ProfileId** sütununu temel alarak büyük ölçüde yapın.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING UpsertStagingTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE UpsertStagingTable
END
```

**Seçenek 2:** [Kopyalama etkinliği içinde bir saklı yordamı çağırmayı](#invoke-a-stored-procedure-from-a-sql-sink)seçebilirsiniz. Bu yaklaşım, `writeBatchSize` kopyalama etkinliğinde varsayılan yaklaşım olarak toplu ekleme kullanmak yerine kaynak tablodaki her bir toplu işi (özelliği tarafından yönetilir) çalıştırır.

### <a name="overwrite-the-entire-table"></a>Tüm tablonun üzerine yaz

Bir kopyalama etkinliği havuzunda **Precopyscript** özelliğini yapılandırabilirsiniz. Bu durumda, çalıştıran her kopyalama etkinliği için önce betiği çalıştırır Azure Data Factory. Ardından, verileri eklemek için kopyayı çalıştırır. Örneğin, en son verilerle tüm tablonun üzerine yazmak için, kaynaktan yeni verileri toplu yüklemeden önce tüm kayıtları silmek üzere bir komut dosyası belirtin.

### <a name="write-data-with-custom-logic"></a>Özel mantık ile veri yazma

Özel mantık ile veri yazma adımları, [upsert veri](#upsert-data) bölümünde açıklananlara benzerdir. Kaynak verilerin son olarak hedef tabloya eklenmesinin önüne ek işlem uygulamanız gerektiğinde, hazırlama tablosuna yükleyebilir, sonra saklı yordam etkinliğini çağırabilir veya veri uygulamak için kopyalama etkinlik havuzunda saklı bir yordam çağırabilirsiniz.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a>Bir SQL havuzundan saklı yordam çağırma

Verileri SQL Server veritabanına kopyaladığınızda, kaynak tablodaki her yığında ek parametrelerle Kullanıcı tarafından belirtilen bir saklı yordamı yapılandırabilir ve çağırabilirsiniz. Saklı yordam özelliği [tablo değerli parametrelerin](https://msdn.microsoft.com/library/bb675163.aspx)avantajlarından yararlanır.

Yerleşik kopyalama mekanizmaları amaca uygun olmadığında, saklı bir yordam kullanabilirsiniz. Kaynak verilerin son olarak hedef tabloya eklenmesinin önüne daha fazla işlem uygulamak istediğinizde örnek bir örnektir. Bazı ek işleme örnekleri, sütunları birleştirmek, ek değerleri aramak ve birden fazla tabloya eklemek istebilmenizdir.

Aşağıdaki örnek, SQL Server veritabanındaki bir tabloya bir üsert yapmak için saklı yordamın nasıl kullanılacağını göstermektedir. Giriş verilerinin ve havuz **Pazarlama** tablosunun her birinin üç sütunu olduğunu varsayalım: **ProfileId**, **State**ve **category**. **ProfileId** sütununu temel alarak ve yalnızca "Producta" adlı belirli bir kategori için uygulayın.

1. Veritabanınızda, **Sqlwritertabletype**ile aynı ada sahip tablo türünü tanımlayın. Tablo türünün şeması, giriş verileriniz tarafından döndürülen şemayla aynıdır.

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL,
        [Category] [varchar](256) NOT NULL
    )
    ```

2. Veritabanınızda, **sqlWriterStoredProcedureName**ile aynı ada sahip saklı yordamı tanımlayın. Belirtilen kaynağınızdan gelen giriş verilerini işler ve çıkış tablosu ile birleştirir. Saklı yordamdaki tablo türünün parametre adı, veri kümesinde tanımlanan **TableName** ile aynıdır.

    ```sql
    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
    AS
    BEGIN
    MERGE [dbo].[Marketing] AS target
    USING @Marketing AS source
    ON (target.ProfileID = source.ProfileID and target.Category = @category)
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT MATCHED THEN
        INSERT (ProfileID, State, Category)
        VALUES (source.ProfileID, source.State, source.Category);
    END
    ```

3. Azure Data Factory, kopyalama etkinliğinde **SQL havuzu** bölümünü aşağıdaki gibi tanımlayın:

    ```json
    "sink": {
        "type": "SqlSink",
        "sqlWriterStoredProcedureName": "spOverwriteMarketing",
        "storedProcedureTableTypeParameterName": "Marketing",
        "sqlWriterTableType": "MarketingType",
        "storedProcedureParameters": {
            "category": {
                "value": "ProductA"
            }
        }
    }
    ```

## <a name="data-type-mapping-for-sql-server"></a>SQL Server için veri türü eşlemesi

Ve SQL Server verileri kopyaladığınızda, SQL Server veri türlerinden, geçici veri türlerine Azure Data Factory için aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| SQL Server veri türü | Azure Data Factory geçici veri türü |
|:--- |:--- |
| bigint |Int64 |
| ikili |Byte [] |
| bit |Boole |
| char |Dize, Char [] |
| date |DateTime |
| Tarih saat |DateTime |
| datetime2 |DateTime |
| Türünde |DateTimeOffset |
| Ondalık |Ondalık |
| FıLESTREAM özniteliği (varbinary (max)) |Byte [] |
| Float |Çift |
| image |Byte [] |
| int |Int32 |
| etmenize |Ondalık |
| nchar |Dize, Char [] |
| n |Dize, Char [] |
| sayısal |Ondalık |
| nvarchar |Dize, Char [] |
| real |Tek |
| rowversion |Byte [] |
| girişin |DateTime |
| smallint |Int16 |
| küçük para |Ondalık |
| sql_variant |Nesne |
| metin |Dize, Char [] |
| time |TimeSpan |
| timestamp |Byte [] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| ikili |Byte [] |
| varchar |Dize, Char [] |
| xml |Xml |

>[!NOTE]
> Ondalık geçici türle eşlenen veri türleri için şu anda Azure Data Factory en fazla 28 ' ye kadar duyarlık destekler. 28 ' den daha büyük bir duyarlık gerektiren verileriniz varsa, bir SQL sorgusunda bir dizeye dönüştürmeyi düşünün.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="using-always-encrypted"></a>Always Encrypted kullanma

[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-ver15)ile SQL Server verileri kopyaladığınızda, [Genel ODBC bağlayıcısını](connector-odbc.md) kullanın ve şirket IÇINDE barındırılan Integration Runtime aracılığıyla ODBC sürücüsünü SQL Server. Bu SQL Server Bağlayıcısı şimdi Always Encrypted desteklemiyor. 

Daha ayrıntılı belirtmek gerekirse:

1. Kendi kendine barındırılan Integration Runtime yoksa ayarlayın. Ayrıntılar için bkz. [Şirket içinde barındırılan Integration Runtime](create-self-hosted-integration-runtime.md) makalesi.

2. SQL Server için 64 bit ODBC sürücüsünü [buradan](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server?view=sql-server-ver15)indirin ve Integration Runtime makinesine yükleyin. Bu sürücünün [SQL Server ODBC sürücüsüyle Always Encrypted kullanarak](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=sql-server-ver15#using-the-azure-key-vault-provider)nasıl çalıştığı hakkında daha fazla bilgi edinin.

3. SQL veritabanınıza bağlanmak için ODBC türü ile bağlı hizmet oluşturun. SQL kimlik doğrulamasını kullanmak için aşağıdaki gibi ODBC bağlantı dizesini belirtin ve **temel** kimlik doğrulaması ' nı seçerek kullanıcı adını ve parolayı ayarlayın.

    ```
    Driver={ODBC Driver 17 for SQL Server};Server=<serverName>;Database=<databaseName>;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<servicePrincipalKey>;KeyStoreSecret=<servicePrincipalKey>
    ```

4. Veri kümesi ve kopyalama etkinliğini ODBC türü ile buna uygun olarak oluşturun. [ODBC Bağlayıcısı](connector-odbc.md) makalesinden daha fazla bilgi edinin.

## <a name="troubleshoot-connection-issues"></a>Bağlantı sorunlarını giderme

1. SQL Server örneğinizi uzak bağlantıları kabul edecek şekilde yapılandırın. **SQL Server Management Studio**başlatın, **sunucu**' ya sağ tıklayın ve **Özellikler**' i seçin. Listeden **Bağlantılar** ' ı seçin ve **Bu sunucuya uzaktan bağlantılara izin ver** onay kutusunu seçin.

    ![Uzak bağlantıları etkinleştir](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Ayrıntılı adımlar için bkz. [Uzaktan erişim sunucu yapılandırma seçeneğini yapılandırma](https://msdn.microsoft.com/library/ms191464.aspx).

2. **SQL Server Yapılandırma Yöneticisi**başlatın. İstediğiniz örnek için **SQL Server ağ yapılandırması** ' nı GENIŞLETIN ve **MSSQLSERVER protokolleri**' ni seçin. Protokoller sağ bölmede görüntülenir. **TCP/IP ' ye** sağ tıklayıp **Etkinleştir**' i seçerek TCP/IP 'yi etkinleştirin.

    ![TCP/IP 'yi etkinleştir](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    TCP/IP protokolünü etkinleştirmenin daha fazla bilgi ve diğer yolları için bkz. [sunucu ağ protokolünü etkinleştirme veya devre dışı bırakma](https://msdn.microsoft.com/library/ms191294.aspx).

3. Aynı pencerede, TCP/IP **Özellikler** penceresini başlatmak için **TCP/IP** ' ye çift tıklayın.
4. **IP adresleri** sekmesine geçin. **ipall** bölümünü görmek için aşağı kaydırın. **TCP bağlantı noktasını**yazın. Varsayılan değer **1433**' dir.
5. Bu bağlantı noktası üzerinden gelen trafiğe izin vermek için makinede **Windows Güvenlik Duvarı için bir kural** oluşturun. 
6. **Bağlantıyı doğrula**: tam nitelikli adı kullanarak SQL Server bağlanmak için, farklı bir makineden SQL Server Management Studio kullanın. `"<machine>.<domain>.corp.<company>.com,1433"` bunun bir örneğidir.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
