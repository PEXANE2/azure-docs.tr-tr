---
title: Azure SQL veritabanı 'nda verileri kopyalama ve dönüştürme
description: Azure SQL veritabanına veri kopyalamayı ve Azure SQL veritabanı 'nda Azure Data Factory kullanarak verileri dönüştürmeyi öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/28/2020
ms.openlocfilehash: 6a71cc080c28cfa6e54de5b942ad1d8ce5b496d3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513962"
---
# <a name="copy-and-transform-data-in-azure-sql-database-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL veritabanındaki verileri kopyalama ve dönüştürme

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Data Factory sürümünü seçin:"]
>
> - [Sürüm 1](v1/data-factory-azure-sql-connector.md)
> - [Güncel sürüm](connector-azure-sql-database.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure SQL veritabanı 'ndan veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir ve veri akışı kullanarak Azure SQL veritabanı 'nda verileri dönüştürebilirsiniz. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure SQL Veritabanı Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Etkinliği](copy-activity-overview.md) [Desteklenen kaynak/havuz matris](copy-activity-overview.md) tablosuyla Kopyala
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)

Kopyalama etkinliği için bu Azure SQL Veritabanı Bağlayıcısı şu işlevleri destekler:

- Azure kaynakları için hizmet sorumlusu veya yönetilen kimlikler ile SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulaması kullanarak veri kopyalama.
- Kaynak olarak, bir SQL sorgusu veya saklı yordam kullanarak verileri alma.
- Havuz olarak, kaynak şemaya bağlı değilse, hedef tablo otomatik olarak oluşturuluyor; bir tabloya veri ekleme veya kopyalama sırasında özel mantık ile saklı yordam çağırma.

>[!NOTE]
> Azure SQL veritabanı [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current) artık bu bağlayıcı tarafından desteklenmiyor. Geçici bir çözüm için, şirket içinde barındırılan bir tümleştirme çalışma zamanı aracılığıyla [Genel BIR ODBC Bağlayıcısı](connector-odbc.md) ve SQL Server ODBC sürücüsü kullanabilirsiniz. [Always Encrypted bölümünü kullanarak](#using-always-encrypted) daha fazla bilgi edinin. 

> [!IMPORTANT]
> Azure Integration Runtime kullanarak verileri kopyalarsanız, Azure hizmetlerinin sunucuya erişebilmesi için [sunucu düzeyinde bir güvenlik duvarı kuralı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) yapılandırın.
> Şirket içinde barındırılan tümleştirme çalışma zamanı kullanarak verileri kopyalarsanız, güvenlik duvarını uygun IP aralığına izin verecek şekilde yapılandırın. Bu Aralık, Azure SQL veritabanına bağlanmak için kullanılan makinenin IP 'sini içerir.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, bir Azure SQL veritabanı bağlayıcısına özgü Azure Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılı bilgiler sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Bu özellikler, Azure SQL veritabanı bağlı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | **Type** özelliği **Azuressqldatabase**olarak ayarlanmalıdır. | Evet |
| Dizisi | **ConnectionString** özelliği IÇIN Azure SQL veritabanı örneğine bağlanmak için gereken bilgileri belirtin. <br/>Ayrıca, Azure Key Vault bir parola veya hizmet sorumlusu anahtarı da koyabilirsiniz. SQL kimlik doğrulaması ise, `password` yapılandırmayı bağlantı dizesinin dışına çekin. Daha fazla bilgi için, Azure Key Vault tablo ve [Mağaza kimlik bilgilerini](store-credentials-in-key-vault.md)izleyen JSON örneğine bakın. | Evet |
| Serviceprincipalıd | Uygulamanın istemci KIMLIĞINI belirtin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| Servicesprincipalkey | Uygulamanın anahtarını belirtin. Azure Data Factory güvenli bir şekilde depolamak veya [Azure Key Vault depolanan bir gizli dizi başvurusunda bulunmak](store-credentials-in-key-vault.md)için bu alanı **SecureString** olarak işaretleyin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| Kiracı | Uygulamanızın bulunduğu etki alanı adı veya kiracı KIMLIĞI gibi kiracı bilgilerini belirtin. Fareyi, Azure portal sağ üst köşesine getirerek alın. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| connectVia | Bu [tümleştirme çalışma zamanı](concepts-integration-runtime.md) , veri deposuna bağlanmak için kullanılır. Veri depolubir özel ağda yer alıyorsa Azure tümleştirme çalışma zamanını veya şirket içinde barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. | Hayır |

Farklı kimlik doğrulama türleri için sırasıyla Önkoşullar ve JSON örnekleri hakkında aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: hizmet sorumlusu](#service-principal-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: Azure kaynakları için Yönetilen kimlikler](#managed-identity)

>[!TIP]
>"UserErrorFailedToConnectToSqlServer" hata koduyla bir hatayla karşılaşırsanız ve "veritabanı için oturum sınırı XXX ve ulaşıldığında", `Pooling=false` Bağlantı dizenizi ekleyin ve yeniden deneyin.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulaması kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Azure Key Vault parola**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
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

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet sorumlusu tabanlı Azure AD uygulama belirteci kimlik doğrulamasını kullanmak için şu adımları izleyin:

1. Azure portal [bir Azure Active Directory uygulaması oluşturun](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal) . Uygulama adını ve bağlı hizmeti tanımlayan aşağıdaki değerleri unutmayın:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Daha önce yapmadıysanız Azure portal sunucunuza yönelik [bir Azure Active Directory Yöneticisi sağlayın](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database) . Azure AD yöneticisi bir Azure AD kullanıcısı veya Azure AD grubu olmalıdır, ancak hizmet sorumlusu olamaz. Bu adım, bir sonraki adımda hizmet sorumlusu için kapsanan bir veritabanı kullanıcısı oluşturmak üzere bir Azure AD kimliği kullanabilmeniz için yapılır.

3. Hizmet sorumlusu için [Kapsanan Veritabanı kullanıcıları oluşturun](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) . En az BIR kullanıcı iznini değiştiren bir Azure AD kimliğiyle, SQL Server Management Studio gibi araçları kullanarak verileri kopyalamak istediğiniz veritabanına bağlanın. Aşağıdaki T-SQL ' i çalıştırın:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. SQL kullanıcıları veya diğerleri için normalde yaptığınız sürece hizmet sorumlusuna gerekli izinleri verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için [Bu belgeye](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017)bakın.

    ```sql
    ALTER ROLE [role name] ADD MEMBER [your application name];
    ```

5. Azure Data Factory Azure SQL veritabanı bağlı hizmetini yapılandırın.

#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için Yönetilen kimlikler

Veri Fabrikası, belirli veri fabrikasını temsil eden [Azure kaynakları için yönetilen bir kimlikle](data-factory-service-identity.md) ilişkilendirilebilir. Bu yönetilen kimliği, Azure SQL veritabanı kimlik doğrulaması için kullanabilirsiniz. Belirlenen fabrika, bu kimliği kullanarak, veritabanınıza veya veritabanına erişebilir ve veri kopyalayabilir.

Yönetilen kimlik kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin.

1. Daha önce yapmadıysanız Azure portal sunucunuza yönelik [bir Azure Active Directory Yöneticisi sağlayın](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database) . Azure AD yöneticisi bir Azure AD kullanıcısı veya bir Azure AD grubu olabilir. Gruba yönetilen kimliğe sahip bir yönetici rolü verirseniz, 3 ve 4. adımları atlayın. Yöneticinin veritabanına tam erişimi vardır.

2. Azure Data Factory yönetilen kimlik için [Kapsanan Veritabanı kullanıcıları oluşturun](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) . En az BIR kullanıcı iznini değiştiren bir Azure AD kimliğiyle, SQL Server Management Studio gibi araçları kullanarak verileri kopyalamak istediğiniz veritabanına bağlanın. Aşağıdaki T-SQL ' i çalıştırın:
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. SQL kullanıcıları ve diğerleri için normalde yaptığınız gibi Data Factory yönetilen kimliğe gerekli izinleri verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için [Bu belgeye](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017)bakın.

    ```sql
    ALTER ROLE [role name] ADD MEMBER [your Data Factory name];
    ```

4. Azure Data Factory Azure SQL veritabanı bağlı hizmetini yapılandırın.

**Örnek**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services).

Azure SQL veritabanı veri kümesi için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | DataSet 'in **Type** özelliği **Azuressqltable**olarak ayarlanmalıdır. | Evet |
| manızı | Şemanın adı. |Kaynak için Hayır, havuz için Evet  |
| tablo | Tablo/görünüm adı. |Kaynak için Hayır, havuz için Evet  |
| tableName | Şema ile tablonun/görünümün adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü için `schema` ve kullanın `table` . | Kaynak için Hayır, havuz için Evet |

### <a name="dataset-properties-example"></a>DataSet özellikleri örneği

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md). Bu bölüm, Azure SQL veritabanı kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-sql-database-as-the-source"></a>Kaynak olarak Azure SQL veritabanı

Azure SQL veritabanından veri kopyalamak için aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının **Type** özelliği **Azuressqlsource**olarak ayarlanmalıdır. "SqlSource" türü, geriye dönük uyumluluk için hala desteklenmektedir. | Evet |
| sqlReaderQuery | Bu özellik, verileri okumak için özel SQL sorgusu kullanır. `select * from MyTable` bunun bir örneğidir. | Hayır |
| sqlReaderStoredProcedureName | Kaynak tablodaki verileri okuyan saklı yordamın adı. Son SQL ifadesinin saklı yordamda bir SELECT ifadesinin olması gerekir. | Hayır |
| storedProcedureParameters | Saklı yordamın parametreleri.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve büyük harfleri, saklı yordam parametrelerinin adlarıyla ve büyük küçük harfleriyle eşleşmelidir. | Hayır |
| 'Sinden | SQL kaynağı için işlem kilitleme davranışını belirtir. İzin verilen değerler: **ReadCommitted** (varsayılan), **READUNCOMMITTED**, **RepeatableRead**, **Serializable**, **Snapshot**. Daha fazla ayrıntı için [Bu belgeye](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) başvurun. | Hayır |

**Şunlara işaret eder:**

- **Azuresopsource**Için **sqlreaderquery** belirtilmişse, kopyalama etkinliği verileri almak IÇIN bu sorguyu Azure SQL veritabanı kaynağına karşı çalıştırır. Saklı yordam parametreleri alırsa, **sqlReaderStoredProcedureName** ve **storedProcedureParameters** belirterek bir saklı yordam de belirtebilirsiniz.
- **Sqlreaderquery** veya **SQLREADERSTOREDPROCEDURENAME**belirtmezseniz, JSON veri kümesinin "yapı" bölümünde tanımlanan sütunlar bir sorgu oluşturmak için kullanılır. Sorgu `select column1, column2 from mytable` Azure SQL veritabanı 'nda çalışır. Veri kümesi tanımında "Structure" yoksa, tablodan tüm sütunlar seçilir.

#### <a name="sql-query-example"></a>SQL sorgu örneği

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "AzureSqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Saklı yordam örneği

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
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
                "type": "AzureSqlSource",
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

### <a name="stored-procedure-definition"></a>Saklı yordam tanımı

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

### <a name="azure-sql-database-as-the-sink"></a>Havuz olarak Azure SQL veritabanı

> [!TIP]
> [Azure SQL veritabanı 'na veri yüklemek Için en iyi](#best-practice-for-loading-data-into-azure-sql-database)uygulamalardan desteklenen yazma davranışları, konfigürasyonlar ve en iyi uygulamalar hakkında daha fazla bilgi edinin.

Azure SQL veritabanı 'na veri kopyalamak için aşağıdaki özellikler, etkinlik **havuzunu** Kopyala bölümünde desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği havuzunun **Type** özelliği **Azuressqlsink**olarak ayarlanmalıdır. "SqlSink" türü, geriye dönük uyumluluk için hala destekleniyor. | Evet |
| Ön Copyscrıpt | Azure SQL veritabanı 'na veri yazmadan önce çalıştırılacak kopyalama etkinliği için bir SQL sorgusu belirtin. Her kopya çalıştırması için yalnızca bir kez çağrılır. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanın. | Hayır |
| tableOption | Kaynak şemasına göre yoksa havuz tablosunun otomatik olarak oluşturulup oluşturulmayacağını belirtir. <br>Havuz saklı yordamı belirttiğinde veya hazırlanan kopya kopyalama etkinliğinde yapılandırıldığında otomatik tablo oluşturma desteklenmez. <br>İzin verilen değerler: `none` (varsayılan), `autoCreate` . | Hayır |
| sqlWriterStoredProcedureName | Hedef tabloya kaynak verilerinin nasıl uygulanacağını tanımlayan saklı yordamın adı. <br/>Bu saklı yordam *toplu iş başına çağırılır*. Yalnızca bir kez çalıştırılan ve kaynak verilerle hiçbir şey olmayan işlemler için, örneğin, DELETE veya TRUNCATE, `preCopyScript` özelliğini kullanın.<br>[BIR SQL havuzundan saklı yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink)örneğine bakın. | Hayır |
| storedProcedureTableTypeParameterName |Saklı yordamda belirtilen tablo türünün parametre adı.  |Hayır |
| sqlWriterTableType |Saklı yordamda kullanılacak tablo türü adı. Kopyalama etkinliği, verileri bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle Kopyalanmakta olan verileri birleştirebilir. |Hayır |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>İzin verilen değerler ad ve değer çiftleridir. Parametrelerin adları ve büyük harfleri, saklı yordam parametrelerinin adlarıyla ve büyük küçük harfleriyle aynı olmalıdır. | Hayır |
| writeBatchSize | *Toplu iş BAŞıNA*SQL tablosuna eklenecek satır sayısı.<br/> İzin verilen değer **Integer** (satır sayısı). Varsayılan olarak, Azure Data Factory satır boyutuna göre uygun toplu iş boyutunu dinamik olarak belirler. | Hayır |
| writeBatchTimeout | Toplu iş ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi.<br/> İzin verilen değer **TimeSpan**değeridir. Örnek olarak "00:30:00" (30 dakika) bulunur. | Hayır |
| disableMetricsCollection | Data Factory, performansı en iyi duruma getirme ve öneriler için Azure SQL veritabanı DTU 'ları gibi ölçümleri toplar. Bu davranışla ilgileniyorlarsa, `true` devre dışı bırakmak için belirtin. | Hayır (varsayılan değer `false` ) |

**Örnek 1: veri ekleme**

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSqlSink",
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
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSqlSink",
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

## <a name="best-practice-for-loading-data-into-azure-sql-database"></a>Azure SQL veritabanı 'na veri yüklemek için en iyi uygulama

Verileri Azure SQL veritabanına kopyaladığınızda, farklı yazma davranışı gerekebilir:

- [Append](#append-data): Kaynak verilerinizde yalnızca yeni kayıtlar vardır.
- [Upsert](#upsert-data): Kaynak verilerinizde hem ekler hem de güncelleştirmeler vardır.
- [Üzerine yaz](#overwrite-the-entire-table): her seferinde bir boyut tablosunun tamamını yeniden yüklemek istiyorum.
- [Özel mantık Ile yaz](#write-data-with-custom-logic): hedef tabloya son ekleme yapmadan önce fazladan işleme ihtiyacım var.

Azure Data Factory ve en iyi uygulamalarında yapılandırma hakkında ilgili bölümlere bakın.

### <a name="append-data"></a>Veri Ekle

Verilerin eklenmesi, bu Azure SQL veritabanı havuz bağlayıcısının varsayılan davranışıdır. Azure Data Factory tablonuza verimli bir şekilde yazmak için toplu bir ekleme yapar. Kopyalama etkinliğinde kaynağı ve havuzu uygun şekilde yapılandırabilirsiniz.

### <a name="upsert-data"></a>Verileri upsert etme

**Seçenek 1:** Kopyalamak için büyük miktarda veriniz varsa, kopyalama etkinliğini kullanarak tüm kayıtları bir hazırlama tablosuna toplu olarak yükleyebilir, sonra bir bir çekde [birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) veya ekleme/güncelleştirme ifadesini uygulamak için bir saklı yordam etkinliği çalıştırabilirsiniz. 

Kopyalama etkinliği şu anda verileri veritabanı geçici tablosuna yüklemeyi yerel olarak desteklemiyor. Birden çok etkinliğin birleşimiyle ayarlanmasının gelişmiş bir yolu vardır ve [Azure SQL veritabanı toplu gelişmiş senaryolarını iyileştirme](https://github.com/scoriani/azuresqlbulkupsert)bölümüne bakın. Aşağıda, kalıcı bir tablonun hazırlama olarak kullanılması örneği gösterilmektedir.

Örnek olarak, Azure Data Factory, **saklı yordam etkinliği**ile **kopyalama etkinliği** zincirli bir işlem hattı oluşturabilirsiniz. Eski, verileri veri kümesindeki tablo adı olarak kaynak deponuzdan Azure SQL veritabanı hazırlama tablosuna (örneğin, **UpsertStagingTable**) kopyalar. İkinci olarak, hazırlama tablosundaki kaynak verileri hedef tabloya birleştirmek ve hazırlama tablosunu temizlemek için bir saklı yordam çağırır.

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

**Seçenek 3:** Yerleşik INSERT/upsert/Update yöntemleri sunan [eşleme veri akışını](#sink-transformation) kullanabilirsiniz.

### <a name="overwrite-the-entire-table"></a>Tüm tablonun üzerine yaz

Kopyalama etkinliği havuzunda **Precopyscript** özelliğini yapılandırabilirsiniz. Bu durumda, çalıştıran her kopyalama etkinliği için önce betiği çalıştırır Azure Data Factory. Ardından, verileri eklemek için kopyayı çalıştırır. Örneğin, en son verilerle tüm tablonun üzerine yazmak için, kaynaktan yeni verileri toplu yüklemeden önce tüm kayıtları silmek üzere bir komut dosyası belirtin.

### <a name="write-data-with-custom-logic"></a>Özel mantık ile veri yazma

Özel mantık ile veri yazma adımları, [upsert veri](#upsert-data) bölümünde açıklananlara benzerdir. Hedef tabloya son kaynak verileri eklemeden önce fazladan işlem uygulamanız gerektiğinde, bir hazırlama tablosuna yükleyebilir, sonra saklı yordam etkinliğini çağırabilir veya kopyalama etkinliği havuzunda verileri uygulamak için bir saklı yordam çağırabilir veya eşleme veri akışını kullanabilirsiniz.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a>Bir SQL havuzundan saklı yordam çağırma

Verileri Azure SQL veritabanına kopyaladığınızda, kaynak tablodaki her yığın üzerinde ek parametrelerle Kullanıcı tarafından belirtilen bir saklı yordamı yapılandırabilir ve çağırabilirsiniz. Saklı yordam özelliği [tablo değerli parametrelerin](https://msdn.microsoft.com/library/bb675163.aspx)avantajlarından yararlanır.

Yerleşik kopyalama mekanizmaları amaca uygun olmadığında, saklı bir yordam kullanabilirsiniz. Kaynak verilerin son olarak hedef tabloya eklenmesinin önüne daha fazla işlem uygulamak istediğinizde örnek bir örnektir. Bazı ek işleme örnekleri, sütunları birleştirmek, ek değerleri aramak ve birden fazla tabloya eklemek istebilmenizdir.

Aşağıdaki örnek, Azure SQL veritabanı 'nda bir tabloyu kullanarak bir saklı yordamın nasıl kullanıldığını gösterir. Giriş verilerinin ve havuz **Pazarlama** tablosunun her birinin üç sütunu olduğunu varsayalım: **ProfileId**, **State**ve **category**. **ProfileId** sütununu temel alarak ve yalnızca "Producta" adlı belirli bir kategori için uygulayın.

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
        "type": "AzureSqlSink",
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

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki verileri dönüştürürken Azure SQL veritabanı 'ndan tabloları okuyabilir ve yazabilirsiniz. Daha fazla bilgi için bkz. veri akışlarını eşleme içindeki [kaynak dönüştürme](data-flow-source.md) ve [Havuz dönüşümü](data-flow-sink.md) .

### <a name="source-transformation"></a>Kaynak dönüştürme

Azure SQL veritabanı 'na özgü ayarlar, kaynak dönüşümünün **kaynak seçenekleri** sekmesinde bulunur.

**Giriş:** Kaynağınızı bir tabloya (eşdeğerini) işaret edip etmeyeceğinizi seçin ```Select * from <table-name>``` ya da özel BIR SQL sorgusu girin.

**Sorgu**: giriş alanında sorgu ' yı seçerseniz, kaynağınız IÇIN bir SQL sorgusu girin. Bu ayar, veri kümesinde seçtiğiniz tüm tabloları geçersiz kılar. **Order by** yan tümceleri burada desteklenmez, ancak BIR tam select from ifadesini ayarlayabilirsiniz. Kullanıcı tanımlı tablo işlevleri de kullanabilirsiniz. **select * from udfGetData ()** , bir tablo döndüren SQL 'de bir UDF 'dir. Bu sorgu, veri akışınızda kullanabileceğiniz bir kaynak tablosu oluşturur. Sorguların kullanılması, test veya aramalar için satırları azaltmanın harika bir yoludur.

- SQL örneği:```Select * from MyTable where customerId > 1000 and customerId < 2000```

**Toplu iş boyutu**: büyük verileri okuma işleminde öbek için bir toplu iş boyutu girin.

**Yalıtım düzeyi**: eşleme VERI akışındaki SQL kaynakları için varsayılan değer read UNCOMMITTED ' dır. Yalıtım düzeyini buradaki değerlerden birine değiştirebilirsiniz:

- Okuma Işlendi
- Kaydedilmeyen oku
- Yinelenebilir okuma
- Seri hale getirilebilir
- Hiçbiri (yalıtım düzeyini yoksay)

![Yalıtım düzeyi](media/data-flow/isolationlevel.png "Yalıtım düzeyi")

### <a name="sink-transformation"></a>Havuz dönüştürme

Azure SQL veritabanı 'na özgü ayarlar, havuz dönüşümünün **Ayarlar** sekmesinde bulunur.

**Güncelleştirme yöntemi:** Veritabanı Hedefinizdeki hangi işlemlere izin verileceğini belirler. Varsayılan değer yalnızca eklemeleri izin verir. Satırları güncelleştirmek, kaldırmak veya silmek için, bu eylemler için satırları etiketlemek üzere alter-Row dönüşümü gereklidir. Güncelleştirmeler, yukarı ve silme için bir anahtar sütunu veya sütunları ayarlanacak satırı belirleyecek şekilde ayarlanmalıdır.

![Anahtar sütunlar](media/data-flow/keycolumn.png "Anahtar sütunlar")

Burada anahtar olarak seçtiğiniz sütun adı, ADF tarafından sonraki güncelleştirme, upsert, DELETE 'in bir parçası olarak kullanılacaktır. Bu nedenle, havuz eşlemesinde var olan bir sütun seçmeniz gerekir. Değeri bu anahtar sütununa yazmak istiyorsanız "anahtar sütunlarını yazmayı atla" seçeneğine tıklayın.

**Tablo eylemi:** Yazmadan önce hedef tablodaki tüm satırların yeniden oluşturulup kaldırılacağını belirler.

- Hiçbiri: tabloya hiçbir eylem yapılmaz.
- Yeniden oluştur: tablo bırakılır ve yeniden oluşturulur. Dinamik olarak yeni bir tablo oluşturuluyoruz gereklidir.
- Kes: hedef tablodaki tüm satırlar kaldırılacak.

**Toplu iş boyutu**: her bir sepete kaç satır yazıldığını denetler. Daha büyük toplu işlem boyutları sıkıştırma ve bellek iyileştirmeyi iyileştirir, ancak verileri önbelleğe alırken bellek dışında özel durumlar riskini ortadan kaldıracak.

**SQL betiklerini ön ve sonrası**: (ön işleme) ve sonra (işlem sonrası) verileri havuz veritabanınıza yazıldıktan sonra yürütülecek çok satırlı SQL betikleri girin

![SQL işleme betikleri ön ve sonrası](media/data-flow/prepost1.png "SQL işleme betikleri")

## <a name="data-type-mapping-for-azure-sql-database"></a>Azure SQL veritabanı için veri türü eşlemesi

Veriler veya Azure SQL veritabanı 'na kopyalandığında, Azure SQL veritabanı veri türleri arasında, geçici veri türlerini Azure Data Factory için aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| Azure SQL veritabanı veri türü | Azure Data Factory geçici veri türü |
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
| tinyint |Bayt |
| uniqueidentifier |Guid |
| ikili |Byte [] |
| varchar |Dize, Char [] |
| xml |Xml |

>[!NOTE]
> Ondalık geçici türle eşlenen veri türleri için şu anda kopyalama etkinliği, 28 ' ye kadar duyarlık destekler. Daha yüksek bir duyarlığa sahip verileriniz varsa, SQL sorgusunda bir dizeye dönüştürmeyi düşünün.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin

## <a name="using-always-encrypted"></a>Always Encrypted kullanma

[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current)Ile Azure SQL veritabanı 'ndan veri kopyaladığınızda, [Genel ODBC Bağlayıcısı](connector-odbc.md) ' nı ve SQL Server ODBC sürücüsünü şirket içinde barındırılan Integration Runtime aracılığıyla kullanın. Bu Azure SQL Veritabanı Bağlayıcısı şimdi Always Encrypted desteklemiyor. 

Daha ayrıntılı belirtmek gerekirse:

1. Kendi kendine barındırılan Integration Runtime yoksa ayarlayın. Ayrıntılar için bkz. [Şirket içinde barındırılan Integration Runtime](create-self-hosted-integration-runtime.md) makalesi.

2. SQL Server için 64 bit ODBC sürücüsünü [buradan](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server?view=azuresqldb-current)indirin ve Integration Runtime makinesine yükleyin. Bu sürücünün [SQL Server ODBC sürücüsüyle Always Encrypted kullanarak](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-current#using-the-azure-key-vault-provider)nasıl çalıştığı hakkında daha fazla bilgi edinin.

3. ODBC türü ile bağlı hizmet oluşturma SQL veritabanınıza bağlanmak için aşağıdaki örneklere bakın:

    - **SQL kimlik doğrulamasını**kullanmak IÇIN: ODBC bağlantı dizesini aşağıda gösterildiği gibi belirtin ve **temel** kimlik doğrulaması ' nı seçerek Kullanıcı adı ve parolasını ayarlayın.

        ```
        Driver={ODBC Driver 17 for SQL Server};Server=<serverName>;Database=<databaseName>;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<servicePrincipalKey>;KeyStoreSecret=<servicePrincipalKey>
        ```

    - **Data Factory yönetilen kimlik doğrulamasını**kullanmak için: 

        1. Yönetilen kimlik için veritabanı kullanıcısı oluşturmak ve veritabanınıza uygun rolü vermek için aynı [önkoşulları](#managed-identity) izleyin.
        2. Bağlı hizmet ' de, aşağıdaki gibi ODBC bağlantı dizesini belirtin ve bağlantı dizesinin kendisi gösterdiği şekilde **anonim** kimlik doğrulaması ' nı seçin `Authentication=ActiveDirectoryMsi` .

        ```
        Driver={ODBC Driver 17 for SQL Server};Server=<serverName>;Database=<databaseName>;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<servicePrincipalKey>;KeyStoreSecret=<servicePrincipalKey>; Authentication=ActiveDirectoryMsi;
        ```

4. Veri kümesi ve kopyalama etkinliğini ODBC türü ile buna uygun olarak oluşturun. [ODBC Bağlayıcısı](connector-odbc.md) makalesinden daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).
