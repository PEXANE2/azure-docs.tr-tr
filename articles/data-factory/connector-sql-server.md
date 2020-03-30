---
title: SQL Server'a veri kopyalama
description: Azure Veri Fabrikası'nı kullanarak verileri şirket içinde bulunan SQL Server veritabanına veya Azure VM'de nasıl taşıyarak nasıl taşıyabildiğini öğrenin.
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
ms.date: 03/12/2020
ms.openlocfilehash: e7f91d60a0e181690859da64e491c4c0fa1e1a3e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80238714"
---
# <a name="copy-data-to-and-from-sql-server-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak SQL Server'a veri kopyalama

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Veri Fabrikası sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-sqlserver-connector.md)
> * [Geçerli sürüm](connector-sql-server.md)

Bu makalede, Sql Server veritabanından ve verilerinkopyalanması için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SQL Server bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)

Verileri SQL Server veritabanından desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Veya, desteklenen herhangi bir kaynak veri deposundan bir SQL Server veritabanına veri kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak veya lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SQL Server bağlayıcısı destekler:

- SQL Server sürümleri 2016, 2014, 2012, 2008 R2, 2008 ve 2005.
- SQL veya Windows kimlik doğrulaması kullanarak verileri kopyalama.
- Kaynak olarak, bir SQL sorgusu veya depolanan yordam ı kullanarak veri alma.
- Lavabo olarak, verileri hedef tabloya eklemek veya kopya sırasında özel mantıkla depolanan yordamı çağırmak.

[SQL Server Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-2017) desteklenmez.

>[!NOTE]
>SQL Server [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-2017) artık bu bağlayıcı tarafından desteklenmiyor. Geçici olarak çalışmak için genel bir [ODBC bağlayıcısı](connector-odbc.md) ve SQL Server ODBC sürücüsü kullanabilirsiniz. ODBC sürücü indirme ve bağlantı dize yapılandırmaları ile [bu kılavuzu](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=sql-server-2017) izleyin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [data-factory-v2-integration-runtime-requirements](../../includes/data-factory-v2-integration-runtime-requirements.md)]

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, SQL Server veritabanı bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

SQL Server bağlantılı hizmet için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **SqlServer**olarak ayarlanmalıdır. | Evet |
| Connectionstring |SQL kimlik doğrulaması veya Windows kimlik doğrulaması kullanarak SQL Server veritabanına bağlanmak için gereken **connectionString** bilgilerini belirtin. Aşağıdaki örneklere bakın.<br/>Azure Key Vault'a parola da koyabilirsiniz. SQL kimlik doğrulamasıysa, yapılandırmayı `password` bağlantı dizesinin dışına çekin. Daha fazla bilgi için, Azure Anahtar [Kasası'ndaki](store-credentials-in-key-vault.md)tablo yu ve Mağaza kimlik bilgilerini izleyen JSON örneğine bakın. |Evet |
| userName |Windows kimlik doğrulaması kullanıyorsanız bir kullanıcı adı belirtin. Bir örnek **alan\\adı kullanıcı adıdır.** |Hayır |
| password |Kullanıcı adı için belirttiğiniz kullanıcı hesabı için bir parola belirtin. Bu alanı Azure Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin. Veya Azure [Key Vault'ta depolanan bir gizli ye başvuru](store-credentials-in-key-vault.md)da bulunabilirsiniz. |Hayır |
| connectVia | Bu [tümleştirme çalışma süresi](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılır. [Önkoşullar](#prerequisites) bölümünden daha fazla bilgi edinin. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Hayır |

>[!TIP]
>"UserErrorFailedToConnectToSqlServer" hata kodu ve "Veritabanı için oturum sınırı XXX ve ulaşıldı" gibi bir `Pooling=false` iletiyle bir hata yaptıysanız, bağlantı dizenize ekleyin ve yeniden deneyin.

**Örnek 1: SQL kimlik doğrulamasını kullanın**

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

**Örnek 2: Azure Key Vault'ta parolayla SQL kimlik doğrulamasını kullanın**

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [veri kümeleri](concepts-datasets-linked-services.md) makalesine bakın. Bu bölümde, SQL Server veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Verileri SQL Server veritabanından ve bir SQL Server veritabanına kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği **SqlServerTable**olarak ayarlanmalıdır. | Evet |
| Şema | Şema adı. |Kaynak için hayır, lavabo için Evet  |
| tablo | Tablonun/görünümün adı. |Kaynak için hayır, lavabo için Evet  |
| tableName | Şema ile tablo/görünüm adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü `schema` `table`için, kullanım ve . | Kaynak için hayır, lavabo için Evet |

**Örnek**

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

Etkinlikleri tanımlamak için kullanılabilecek bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, SQL Server kaynağı ve lavabo tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sql-server-as-a-source"></a>Kaynak olarak SQL Server

SQL Server'dan veri kopyalamak için, kopyalama etkinliğindeki kaynak türünü **SqlSource**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği kaynak bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği **SqlSource**olarak ayarlanmalıdır. | Evet |
| sqlReaderQuery |Verileri okumak için özel SQL sorgusunu kullanın. `select * from MyTable` bunun bir örneğidir. |Hayır |
| sqlReaderStoredProcedureNameName |Bu özellik, kaynak tablodaki verileri okuyan depolanan yordamın adıdır. Son SQL deyimi, depolanan yordamda bir SELECT deyimi olmalıdır. |Hayır |
| depolanmışProsedürParametreleri |Bu parametreler depolanan yordam içindir.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |
| ısolationlevel | SQL kaynağı için hareket kilitleme davranışını belirtir. İzin verilen değerler şunlardır: **ReadCommitted** (varsayılan), **ReadUncommitted**, **Tekrarlanabilir Oku**, **Serializable**, **Anlık Görüntü**. Daha fazla bilgi için [bu dokümana](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) bakın. | Hayır |

**Dikkat edilmesi gereken noktalar:**

- **SqlSource**için **sqlReaderQuery** belirtilirse, kopyalama etkinliği verileri almak için bu sorguyu SQL Server kaynağına karşı çalıştırır. Ayrıca **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** belirterek depolanan yordamı belirtebilirsiniz.
- **SqlReaderQuery** veya **sqlReaderStoredProcedureName**belirtmezseniz, JSON veri kümesinin "yapı" bölümünde tanımlanan sütunlar sorgu oluşturmak için kullanılır. Sorgu `select column1, column2 from mytable` SQL Server'a karşı çalışır. Veri kümesi tanımında "yapı" yoksa, tüm sütunlar tablodan seçilir.

**Örnek: SQL sorgusukullanın**

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

**Örnek: Depolanan yordamı kullanma**

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

**Depolanan yordam tanımı**

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

### <a name="sql-server-as-a-sink"></a>Lavabo olarak SQL Server

> [!TIP]
> [SQL Server'a veri yüklemek için en iyi uygulamadan](#best-practice-for-loading-data-into-sql-server)desteklenen yazma davranışları, yapılandırmalar ve en iyi uygulamalar hakkında daha fazla bilgi edinin.

Verileri SQL Server'a kopyalamak için, kopyalama etkinliğindeki lavabo türünü **SqlSink**olarak ayarlayın. Aşağıdaki özellikler kopyalama etkinliği lavabo bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun türü özelliği **SqlSink**olarak ayarlanmalıdır. | Evet |
| yazmaBatchSize |*Toplu iş başına*SQL tablosuna eklenecek satır sayısı.<br/>İzin verilen değerler satır sayısı için arayıcılardır. Varsayılan olarak, Azure Veri Fabrikası satır boyutuna bağlı olarak uygun toplu iş boyutunu dinamik olarak belirler. |Hayır |
| yazmaBatchTimeout |Bu özellik, toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresini belirtir.<br/>İzin verilen değerler zaman kaydırma içindir. Bir örnek 30 dakika için "00:30:00" olduğunu. Değer belirtilmemişse, zaman varsayılan olarak 02:00:00 olarak adlandırılır. |Hayır |
| preCopyScript |Bu özellik, SQL Server'a veri yazmadan önce kopyalama etkinliğinin çalışması için bir SQL sorgusu belirtir. Kopya başına yalnızca bir kez çağrılır. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır |
| sqlWriterStoredProcedureNameName | Kaynak verilerin hedef tabloya nasıl uygulanacağı konusunda tanımlanan depolanan yordamın adı. <br/>Bu depolanan yordam *toplu iş başına çağrılır.* Yalnızca bir kez çalışan ve kaynak verilerle hiçbir ilgisi olmayan işlemler için , `preCopyScript` örneğin silme veya bulanın, özelliği kullanın. | Hayır |
| depolanmışProcedureTableTypeParameterName |Depolanan yordamda belirtilen tablo türünün parametre adı.  |Hayır |
| sqlWriterTableType |Depolanan yordamda kullanılacak tablo türü adı. Kopyalama etkinliği, verilerin bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Depolanan yordam kodu, daha sonra kopyalanan verileri varolan verilerle birleştirilebilir. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler.<br/>İzin verilen değerler ad ve değer çiftleridir. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. | Hayır |
| Tableoption | Kaynak şemaya göre yoksa, lavabo tablosunun otomatik olarak oluşturulup oluşturulmayacağını belirtir. Otomatik tablo oluşturma, lavabo saklı yordamı belirtir veya sahnelenen kopya kopyalama etkinliğinde yapılandırılır desteklenmez. İzin verilen `none` değerler şunlardır: (varsayılan), `autoCreate`. |Hayır |

**Örnek 1: Ek veri**

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
                "writeBatchSize": 100000,
                "tableOption": "autoCreate"
            }
        }
    }
]
```

**Örnek 2: Kopya sırasında depolanan yordamı çağırma**

[SQL lavabodan depolanan yordamı Çağır'dan](#invoke-a-stored-procedure-from-a-sql-sink)daha fazla ayrıntı edinin.

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

## <a name="best-practice-for-loading-data-into-sql-server"></a>SQL Server'a veri yüklemek için en iyi yöntem

Verileri SQL Server'a kopyaladiğinizde, farklı yazma davranışı gerektirebilir:

- [Ek](#append-data): Kaynak verilerimde yalnızca yeni kayıtlar var.
- [Upsert](#upsert-data): Kaynak verilerimde hem ekler hem de güncelleştirmeler vardır.
- [Overwrite](#overwrite-the-entire-table): Her seferinde tüm boyut tablosunu yeniden yüklemek istiyorum.
- [Özel mantıkla yazın](#write-data-with-custom-logic): Hedef tabloya son eklemeden önce ekstra işleme ihtiyacım var.

Azure Veri Fabrikası'nda ve en iyi uygulamalarda nasıl yapılandırılabilenilgili bölümlere bakın.

### <a name="append-data"></a>Veri ekle

Veri eklenmesi, bu SQL Server lavabo bağlayıcısının varsayılan davranışıdır. Azure Veri Fabrikası, tablonuza verimli bir şekilde yazmak için toplu bir ekleme yapar. Kaynağı kopya etkinliğinde buna göre yapılandırabilir ve bekleyebilirsiniz.

### <a name="upsert-data"></a>Verileri upsert etme

**Seçenek 1:** Kopyalamanız gereken büyük miktarda veri varsa, bir yükseltme yapmak için aşağıdaki yaklaşımı kullanın: 

- İlk olarak, kopyalama etkinliğini kullanarak tüm kayıtları toplu yüklemek için geçici bir [tablo](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) kullanın. Geçici tablolara yönelik işlemler günlüğe kaydolmadığından, milyonlarca kaydı saniyeler içinde yükleyebilirsiniz.
- [Birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) veya EKLE/UPDATE deyimi uygulamak için Azure Veri Fabrikası'nda depolanan yordam etkinliği çalıştırın. Tüm güncelleştirmeleri veya eklertek bir işlem olarak gerçekleştirmek için kaynak olarak geçici tabloyu kullanın. Bu şekilde, gidiş-dönüş ve günlük işlemlerinin sayısı azalır. Depolanan yordam etkinliğinin sonunda, geçici tablo bir sonraki yükseltme döngüsüne hazır olması için kesilir.

Örnek olarak, Azure Veri Fabrikası'nda, **Depolanmış Yordam etkinliğiyle**zincirlenmiş bir **Kopyalama etkinliği** içeren bir ardışık işlem hattı oluşturabilirsiniz. Kaynak deponuzdaki eski verileri veri kümesindeki tablo adı olarak bir veritabanı geçici tablosuna (örneğin, **##UpsertTempTable**, veri kümesindeki tablo adı olarak kopyalar. Daha sonra ikincisi, geçici tablodaki kaynak verileri hedef tabloya birleştirmek ve geçici tabloyu temizlemek için depolanmış bir yordam çağırır.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

Veritabanınızda, önceki saklı yordam etkinliğinden işaret edilen aşağıdaki örnekte olduğu gibi, birleştirme mantığıyla depolanmış bir yordam tanımlayın. Hedefin üç sütunlu **Pazarlama** tablosu olduğunu varsayalım: **ProfileID**, **Durum**ve **Kategori**. Yükseltmeyi **ProfileID** sütununa göre yapın.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING ##UpsertTempTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE ##UpsertTempTable
END
```

**Seçenek 2:** Ayrıca [kopyalama etkinliği içinde depolanan yordamı çağırmayı](#invoke-a-stored-procedure-from-a-sql-sink)da seçebilirsiniz. Bu yaklaşım, büyük ölçekli upsert için uygun olmayan kopyalama etkinliğinde varsayılan yaklaşım olarak toplu ekleme kullanmak yerine kaynak tablodaki her satırı çalıştırAr.

### <a name="overwrite-the-entire-table"></a>Tüm tablonun üzerine yazma

**CopyScript** özelliğini bir kopyalama etkinliği lavabosunda yapılandırabilirsiniz. Bu durumda, çalışan her kopyalama etkinliği için önce Azure Veri Fabrikası komut dosyasını çalıştırın. Sonra verileri eklemek için kopya çalışır. Örneğin, en son verilerle tüm tablonun üzerine yazmak için, yeni verileri kaynaktan toplu olarak yüklemeden önce tüm kayıtları silmek için bir komut dosyası belirtin.

### <a name="write-data-with-custom-logic"></a>Özel mantıkla veri yazma

Özel mantıkla veri yazma [adımları, Upsert veri](#upsert-data) bölümünde açıklananlara benzer. Büyük ölçekli için, hedef tabloya kaynak verilerin son eklenmesinden önce ekstra işleme uygulamanız gerektiğinde, iki şeyden birini yapabilirsiniz: 

- Geçici bir tabloya yükleyin ve ardından depolanan yordamı çağırın. 
- Kopyalama sırasında depolanan yordamı çağırın.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a>SQL lavabodan depolanan yordamı çağırma

Verileri bir SQL Server veritabanına kopyaladiğinizde, kullanıcı tarafından belirtilen bir depoyordamsını ek parametrelerle yapılandırabilir ve çağırabilirsiniz. Depolanan yordam özelliği [tablo değerindeki parametrelerden](https://msdn.microsoft.com/library/bb675163.aspx)yararlanır.

> [!TIP]
> Depolanan yordamı çağırmak, büyük ölçekli kopya için önermediğimiz toplu bir işlem kullanmak yerine veri satırını satır satır işler. [SQL Server'a veri yüklemek için en iyi uygulamadan](#best-practice-for-loading-data-into-sql-server)daha fazla bilgi edinin.

Yerleşik kopyalama mekanizmaları amaca hizmet etmiyorsa depolanan yordamı kullanabilirsiniz. Kaynak verilerin hedef tabloya son ekinden önce ek işleme uygulamak istediğinizde buna bir örnektir. Bazı ekstra işleme örnekleri, sütunları birleştirmek, ek değerlere bakmak ve verileri birden fazla tabloya eklemek istediğinizde verilebilir.

Aşağıdaki örnek, SQL Server veritabanındaki bir tabloya bir yükseltme yapmak için depolanan yordamın nasıl kullanılacağını gösterir. Giriş verilerinin ve lavabo **Pazarlama** tablosunun her birinin üç sütunu olduğunu varsayalım: **ProfileID,** **Durum**ve **Kategori**. Yükseltmeyi **ProfileID** sütununa göre yapın ve yalnızca "ProductA" adı verilen belirli bir kategori için uygulayın.

1. **Veritabanınızda, sqlWriterTableType**ile aynı ada sahip tablo türünü tanımlayın. Tablo türünün şeması, giriş verileriniz tarafından döndürülen şema ile aynıdır.

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL,
        [Category] [varchar](256) NOT NULL
    )
    ```

2. Veritabanınızda, **sqlWriterStoredProcedureName**ile aynı ada sahip depolanan yordamı tanımlayın. Belirtilen kaynaktan gelen giriş verilerini işler ve çıktı tablosunda birleştirir. Depolanan yordamdaki tablo türünün parametre adı, veri kümesinde tanımlanan **tablo Adı** ile aynıdır.

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

3. Azure Veri Fabrikası'nda, kopyalama etkinliğindeki **SQL lavabo** bölümünü aşağıdaki gibi tanımlayın:

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

## <a name="data-type-mapping-for-sql-server"></a>SQL Server için veri türü eşleme

Verileri SQL Server'dan ve SQL Server'a kopyaladiğinizde, SQL Server veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| SQL Server veri türü | Azure Veri Fabrikası geçici veri türü |
|:--- |:--- |
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
| Sql_variant |Nesne |
| metin |Dize, Char[] |
| time |TimeSpan |
| timestamp |Bayt[] |
| tinyint |Int16 |
| uniqueidentifier |Guid |
| Varbinary |Bayt[] |
| varchar |Dize, Char[] |
| xml |Xml |

>[!NOTE]
> Ondalık geçici türüyle eşalan veri türleri için, şu anda Azure Veri Fabrikası 28'e kadar hassasiyeti destekler. 28'den büyük kesinlik gerektiren verileriniz varsa, SQL sorgusunda bir dize dönüştürmeyi düşünün.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="getmetadata-activity-properties"></a>Metaveri etkinlik özelliklerini alın

Özellikler hakkında daha fazla bilgi edinmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) kontrol edin 

## <a name="troubleshoot-connection-issues"></a>Bağlantı sorunlarını giderme

1. SQL Server örneğini uzak bağlantıları kabul etmek için yapılandırın. **SQL Server Management Studio'yı**başlatın, **sunucuya**sağ tıklayın ve **Özellikler'i**seçin. Listeden **Bağlantılar'ı** seçin ve bu sunucu onay **kutusuna uzak bağlantılara izin ver'i** seçin.

    ![Uzak bağlantıları etkinleştir](media/copy-data-to-from-sql-server/AllowRemoteConnections.png)

    Ayrıntılı adımlar için bkz. [uzaktan erişim sunucusu yapılandırma seçeneğini yapılandırın.](https://msdn.microsoft.com/library/ms191464.aspx)

2. **SQL Server Configuration Manager'ı**başlatın. İstediğiniz örnek için **SQL Server Network Configuration'ı** genişletin ve **MSSQLSERVER için Protokoller'i**seçin. Protokoller sağ bölmede görünür. TCP/IP'ye sağ tıklayarak ve Etkinleştir'i seçerek TCP/IP'yi **etkinleştirin.** **TCP/IP**

    ![TCP/IP'yi etkinleştirme](./media/copy-data-to-from-sql-server/EnableTCPProptocol.png)

    Daha fazla bilgi ve TCP/IP protokolünü etkinleştirmenin alternatif yolları [için](https://msdn.microsoft.com/library/ms191294.aspx)bkz.

3. Aynı pencerede, **TCP/IP Özellikleri** penceresini başlatmak için **TCP/IP'yi** çift tıklatın.
4. **IP Adresleri** sekmesine geçin. **IPTümü** bölümünü görmek için aşağı kaydırın. **TCP Bağlantı Noktasını**yazın. Varsayılan **değer 1433'dür.**
5. Bu bağlantı noktasından gelen trafiğe izin vermek için makinedeki **Windows Güvenlik Duvarı için** bir kural oluşturun. 
6. **Bağlantıyı doğrula**: Tam nitelikli bir ad kullanarak SQL Server'a bağlanmak için farklı bir makineden SQL Server Management Studio'u kullanın. `"<machine>.<domain>.corp.<company>.com,1433"` bunun bir örneğidir.

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için desteklenen [veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
