---
title: Azure SQL yönetilen örneğine veri kopyalama
description: Azure Data Factory kullanarak Azure SQL yönetilen örneğine veri taşımayı öğrenin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 07/15/2020
ms.openlocfilehash: d67a050ccd590e220c51e02b827013ace7707ee2
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523256"
---
# <a name="copy-data-to-and-from-azure-sql-managed-instance-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL yönetilen örneğine veri kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure SQL yönetilen örneğine veri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir. Kopyalama etkinliğine genel bir bakış sunan [kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesinde oluşturulur.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu SQL yönetilen örnek Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/havuz matrisi](copy-activity-overview.md) ile [kopyalama etkinliği](copy-activity-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)

SQL yönetilen örneğinden, desteklenen herhangi bir havuz veri deposuna veri kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundan verileri SQL yönetilen örneğine kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu SQL yönetilen örnek Bağlayıcısı şunları destekler:

- Azure kaynakları için hizmet sorumlusu veya yönetilen kimlikler ile SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulaması kullanarak veri kopyalama.
- Kaynak olarak, bir SQL sorgusu veya saklı yordam kullanarak verileri alma.
- Havuz olarak, kaynak şemaya bağlı değilse, hedef tablo otomatik olarak oluşturuluyor; verileri bir tabloya ekleme veya kopyalama sırasında özel mantık ile saklı yordam çağırma.

>[!NOTE]
> SQL yönetilen örneği [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-mi-current) artık bu bağlayıcı tarafından desteklenmiyor. Geçici bir çözüm için, şirket içinde barındırılan bir tümleştirme çalışma zamanı aracılığıyla [Genel BIR ODBC Bağlayıcısı](connector-odbc.md) ve SQL Server ODBC sürücüsü kullanabilirsiniz. [Always Encrypted bölümünü kullanarak](#using-always-encrypted) daha fazla bilgi edinin. 

## <a name="prerequisites"></a>Önkoşullar

SQL yönetilen örnek [genel uç noktasına](../azure-sql/managed-instance/public-endpoint-overview.md)erişmek için Azure Data Factory yönetilen bir Azure tümleştirme çalışma zamanı kullanabilirsiniz. Azure Data Factory veritabanınıza bağlanabilmesi için, genel uç noktasını etkinleştirdiğinizden ve ağ güvenlik grubunda ortak uç nokta trafiğine izin verdiğinizden emin olun. Daha fazla bilgi için [bu kılavuza](../azure-sql/managed-instance/public-endpoint-configure.md)bakın.

SQL yönetilen örnek özel uç noktasına erişmek için, veritabanına erişebilen, [Şirket içinde barındırılan bir tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) ayarlayın. Şirket içinde barındırılan tümleştirme çalışma zamanını yönetilen örneğiniz ile aynı sanal ağda temin ediyorsanız, tümleştirme çalışma zamanı makinenizin yönetilen örnekten farklı bir alt ağda olduğundan emin olun. Şirket içinde barındırılan tümleştirme çalışma zamanını yönetilen örneğinizle farklı bir sanal ağda temin ediyorsanız, sanal ağ eşlemesi veya sanal ağ bağlantısı kullanabilirsiniz. Daha fazla bilgi için bkz. [UYGULAMANıZı SQL yönetilen örneğine bağlama](../azure-sql/managed-instance/connect-application-instance.md).

## <a name="get-started"></a>Kullanmaya başlayın

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, SQL yönetilen örnek bağlayıcısına özgü Azure Data Factory varlıkları tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

SQL yönetilen örnek bağlı hizmeti için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Type özelliği **Azuressqlmı**olarak ayarlanmalıdır. | Yes |
| Dizisi |Bu özellik SQL kimlik doğrulaması kullanılarak SQL yönetilen örneğine bağlanmak için gereken **ConnectionString** bilgilerini belirtir. Daha fazla bilgi için aşağıdaki örneklere bakın. <br/>Varsayılan bağlantı noktası 1433'tür. Ortak bir uç nokta ile SQL yönetilen örneği kullanıyorsanız, 3342 numaralı bağlantı noktasını açıkça belirtin.<br> Ayrıca, Azure Key Vault bir parola koyabilirsiniz. SQL kimlik doğrulaması ise, `password` yapılandırmayı bağlantı dizesinin dışına çekin. Daha fazla bilgi için, Azure Key Vault tablo ve [Mağaza kimlik bilgilerini](store-credentials-in-key-vault.md)izleyen JSON örneğine bakın. |Yes |
| Serviceprincipalıd | Uygulamanın istemci KIMLIĞINI belirtin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| Servicesprincipalkey | Uygulamanın anahtarını belirtin. Azure Data Factory güvenli bir şekilde depolamak veya [Azure Key Vault depolanan bir gizli dizi başvurusunda bulunmak](store-credentials-in-key-vault.md)için bu alanı **SecureString** olarak işaretleyin. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| Kiracı | Uygulamanızın bulunduğu etki alanı adı veya kiracı KIMLIĞI gibi kiracı bilgilerini belirtin. Fareyi, Azure portal sağ üst köşesine getirerek alın. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda |
| connectVia | Bu [tümleştirme çalışma zamanı](concepts-integration-runtime.md) , veri deposuna bağlanmak için kullanılır. Yönetilen örneğinizin ortak bir uç noktası varsa ve Azure Data Factory erişmesine izin veriyorsa şirket içinde barındırılan tümleştirme çalışma zamanını veya bir Azure tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Yes |

Farklı kimlik doğrulama türleri için sırasıyla Önkoşullar ve JSON örnekleri hakkında aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: hizmet sorumlusu](#service-principal-authentication)
- [Azure AD uygulama belirteci kimlik doğrulaması: Azure kaynakları için Yönetilen kimlikler](#managed-identity)

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

**Örnek 1: SQL kimlik doğrulaması kullanma**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
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
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;",
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

1. [Yönetilen örneğiniz için Azure Active Directory Yöneticisi sağlamak](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance)için adımları izleyin.

2. Azure portal [bir Azure Active Directory uygulaması oluşturun](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal) . Uygulama adını ve bağlı hizmeti tanımlayan aşağıdaki değerleri unutmayın:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

3. Azure Data Factory yönetilen kimlik için [oturum açma bilgileri oluşturun](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) . SQL Server Management Studio (SSMS) ' de, bir **sysadmin**olan SQL Server bir hesabı kullanarak yönetilen örneğe bağlanın. **Ana** veritabanında aşağıdaki T-SQL ' i çalıştırın:

    ```sql
    CREATE LOGIN [your application name] FROM EXTERNAL PROVIDER
    ```

4. Azure Data Factory yönetilen kimlik için [Kapsanan Veritabanı kullanıcıları oluşturun](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) . Veri kopyalamak istediğiniz veya ' dan veritabanına bağlanın, aşağıdaki T-SQL ' i çalıştırın: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER
    ```

5. SQL kullanıcıları ve diğerleri için normalde yaptığınız gibi Data Factory yönetilen kimliğe gerekli izinleri verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için [Bu belgeye](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql?view=azuresqldb-mi-current)bakın.

    ```sql
    ALTER ROLE [role name e.g. db_owner] ADD MEMBER [your application name]
    ```

6. Azure Data Factory bir SQL yönetilen örnek bağlı hizmetini yapılandırın.

**Örnek: hizmet sorumlusu kimlik doğrulamasını kullanma**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;",
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

Veri Fabrikası, belirli veri fabrikasını temsil eden [Azure kaynakları için yönetilen bir kimlikle](data-factory-service-identity.md) ilişkilendirilebilir. Bu yönetilen kimliği, SQL yönetilen örnek kimlik doğrulaması için kullanabilirsiniz. Belirlenen fabrika, bu kimliği kullanarak, veritabanınıza veya veritabanına erişebilir ve veri kopyalayabilir.

Yönetilen kimlik kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin.

1. [Yönetilen örneğiniz için Azure Active Directory Yöneticisi sağlamak](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance)için adımları izleyin.

2. Azure Data Factory yönetilen kimlik için [oturum açma bilgileri oluşturun](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) . SQL Server Management Studio (SSMS) ' de, bir **sysadmin**olan SQL Server bir hesabı kullanarak yönetilen örneğe bağlanın. **Ana** veritabanında aşağıdaki T-SQL ' i çalıştırın:

    ```sql
    CREATE LOGIN [your Data Factory name] FROM EXTERNAL PROVIDER
    ```

3. Azure Data Factory yönetilen kimlik için [Kapsanan Veritabanı kullanıcıları oluşturun](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) . Veri kopyalamak istediğiniz veya ' dan veritabanına bağlanın, aşağıdaki T-SQL ' i çalıştırın: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER
    ```

4. SQL kullanıcıları ve diğerleri için normalde yaptığınız gibi Data Factory yönetilen kimliğe gerekli izinleri verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için [Bu belgeye](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql?view=azuresqldb-mi-current)bakın.

    ```sql
    ALTER ROLE [role name e.g. db_owner] ADD MEMBER [your Data Factory name]
    ```

5. Azure Data Factory bir SQL yönetilen örnek bağlı hizmetini yapılandırın.

**Örnek: yönetilen kimlik kimlik doğrulamasını kullanır**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Veri kümelerini tanımlamak için kullanılabilecek bölümlerin ve özelliklerin tam listesi için bkz. veri kümeleri makalesi. Bu bölüm, SQL yönetilen örnek veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

SQL yönetilen örneğinden veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Veri kümesinin Type özelliği **Azuressqlmıtable**olarak ayarlanmalıdır. | Yes |
| schema | Şemanın adı. |Kaynak için Hayır, havuz için Evet  |
| table | Tablo/görünüm adı. |Kaynak için Hayır, havuz için Evet  |
| tableName | Şema ile tablonun/görünümün adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü için `schema` ve kullanın `table` . | Kaynak için Hayır, havuz için Evet |

**Örnek**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "AzureSqlMITable",
        "linkedServiceName": {
            "referenceName": "<SQL Managed Instance linked service name>",
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

Etkinlikleri tanımlamak için kullanılabilecek bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, SQL yönetilen örnek kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="sql-managed-instance-as-a-source"></a>Kaynak olarak SQL yönetilen örneği

SQL yönetilen örneğinden veri kopyalamak için, etkinlik kaynağını kopyalama bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği kaynağının Type özelliği **Sqlmisource**olarak ayarlanmalıdır. | Yes |
| sqlReaderQuery |Bu özellik, verileri okumak için özel SQL sorgusu kullanır. `select * from MyTable` bunun bir örneğidir. |No |
| sqlReaderStoredProcedureName |Bu özellik, kaynak tablodaki verileri okuyan saklı yordamın adıdır. Son SQL ifadesinin saklı yordamda bir SELECT ifadesinin olması gerekir. |No |
| storedProcedureParameters |Bu parametreler, saklı yordama yöneliktir.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve büyük harfleri, saklı yordam parametrelerinin adlarıyla ve büyük harfleriyle aynı olmalıdır. |No |
| 'Sinden | SQL kaynağı için işlem kilitleme davranışını belirtir. İzin verilen değerler: **ReadCommitted** (varsayılan), **READUNCOMMITTED**, **RepeatableRead**, **Serializable**, **Snapshot**. Daha fazla ayrıntı için [Bu belgeye](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) başvurun. | No |

**Aşağıdaki noktalara dikkat edin:**

- **Sqlmisource**Için **sqlreaderquery** belirtilmişse, kopyalama etkinliği verileri almak Için bu sorguyu SQL yönetilen örnek kaynağında çalıştırır. Saklı yordam parametreleri alırsa, **sqlReaderStoredProcedureName** ve **storedProcedureParameters** belirterek bir saklı yordam de belirtebilirsiniz.
- **Sqlreaderquery** veya **sqlReaderStoredProcedureName** özelliğini belirtmezseniz, JSON veri kümesinin "Structure" bölümünde tanımlanan sütunlar bir sorgu oluşturmak için kullanılır. Sorgu, `select column1, column2 from mytable` SQL yönetilen örneği üzerinde çalışır. Veri kümesi tanımında "Structure" yoksa, tablodan tüm sütunlar seçilir.

**Örnek: bir SQL sorgusu kullanın**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Managed Instance input dataset name>",
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
                "type": "SqlMISource",
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
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Managed Instance input dataset name>",
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
                "type": "SqlMISource",
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

### <a name="sql-managed-instance-as-a-sink"></a>Havuz olarak SQL yönetilen örneği

> [!TIP]
> [SQL yönetilen örneği 'ne veri yüklemek Için en iyi](#best-practice-for-loading-data-into-sql-managed-instance)uygulamalardan desteklenen yazma davranışları, konfigürasyonlar ve en iyi uygulamalar hakkında daha fazla bilgi edinin.

Verileri SQL yönetilen örneğine kopyalamak için, kopyalama etkinlik havuzu bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| tür | Kopyalama etkinliği havuzunun Type özelliği **Sqlmisink**olarak ayarlanmalıdır. | Yes |
| Ön Copyscrıpt |Bu özellik, SQL yönetilen örneğine veri yazmadan önce, kopyalama etkinliğinin çalıştırılacağı bir SQL sorgusu belirtir. Her kopya çalıştırması için yalnızca bir kez çağrılır. Bu özelliği, önceden yüklenmiş verileri temizlemek için kullanabilirsiniz. |No |
| tableOption | Kaynak şemasına göre yoksa [Havuz tablosunun otomatik olarak oluşturulup](copy-activity-overview.md#auto-create-sink-tables) oluşturulmayacağını belirtir. Havuz saklı yordamı belirttiğinde veya hazırlanan kopya kopyalama etkinliğinde yapılandırıldığında otomatik tablo oluşturma desteklenmez. İzin verilen değerler: `none` (varsayılan), `autoCreate` . |No |
| sqlWriterStoredProcedureName | Hedef tabloya kaynak verilerinin nasıl uygulanacağını tanımlayan saklı yordamın adı. <br/>Bu saklı yordam *toplu iş başına çağırılır*. Yalnızca bir kez çalıştırılan ve kaynak verilerle hiçbir şey olmayan işlemler için, örneğin, DELETE veya TRUNCATE, `preCopyScript` özelliğini kullanın.<br>[BIR SQL havuzundan saklı yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink)örneğine bakın. | No |
| storedProcedureTableTypeParameterName |Saklı yordamda belirtilen tablo türünün parametre adı.  |No |
| sqlWriterTableType |Saklı yordamda kullanılacak tablo türü adı. Kopyalama etkinliği, verileri bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Saklı yordam kodu daha sonra mevcut verilerle Kopyalanmakta olan verileri birleştirebilir. |No |
| storedProcedureParameters |Saklı yordamın parametreleri.<br/>İzin verilen değerler ad ve değer çiftleridir. Parametrelerin adları ve büyük harfleri, saklı yordam parametrelerinin adlarıyla ve büyük küçük harfleriyle aynı olmalıdır. | No |
| writeBatchSize |*Toplu iş BAŞıNA*SQL tablosuna eklenecek satır sayısı.<br/>İzin verilen değerler, satır sayısı için tamsayılardır. Varsayılan olarak, Azure Data Factory satır boyutuna göre uygun toplu iş boyutunu dinamik olarak belirler.  |No |
| writeBatchTimeout |Bu özellik, toplu ekleme işleminin zaman aşımına uğramadan önce tamamlaması için bekleme süresini belirtir.<br/>İzin verilen değerler TimeSpan içindir. Örneğin, 30 dakika olan "00:30:00" bir örnektir. |No |

**Örnek 1: veri ekleme**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlMISink",
                "tableOption": "autoCreate",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Örnek 2: kopyalama sırasında saklı yordam çağırma**

[BIR SQL mı havuzundan saklı yordam çağırma](#invoke-a-stored-procedure-from-a-sql-sink)hakkında daha fazla bilgi edinin.

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlMISink",
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

## <a name="best-practice-for-loading-data-into-sql-managed-instance"></a>SQL yönetilen örneğine veri yüklemek için en iyi uygulama

Verileri SQL yönetilen örneği 'ne kopyaladığınızda, farklı yazma davranışı gerekebilir:

- [Append](#append-data): Kaynak verilerinizde yalnızca yeni kayıtlar vardır.
- [Upsert](#upsert-data): Kaynak verilerinizde hem ekler hem de güncelleştirmeler vardır.
- [Üzerine yaz](#overwrite-the-entire-table): her seferinde boyut tablosunun tamamını yeniden yüklemek istiyorum.
- [Özel mantık Ile yaz](#write-data-with-custom-logic): hedef tabloya son ekleme yapmadan önce fazladan işleme ihtiyacım var. 

Azure Data Factory ve en iyi yöntemleri yapılandırmak için ilgili bölümlere bakın.

### <a name="append-data"></a>Veri Ekle

Verileri eklemek, SQL yönetilen örnek havuzu bağlayıcısının varsayılan davranışıdır. Azure Data Factory tablonuza verimli bir şekilde yazmak için toplu bir ekleme yapar. Kopyalama etkinliğinde kaynağı ve havuzu uygun şekilde yapılandırabilirsiniz.

### <a name="upsert-data"></a>Verileri upsert etme

**Seçenek 1:** Kopyalamak için büyük miktarda veriniz varsa, kopyalama etkinliğini kullanarak tüm kayıtları bir hazırlama tablosuna toplu olarak yükleyebilir, sonra bir bir çekde [birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-mi-current) veya ekleme/güncelleştirme ifadesini uygulamak için bir saklı yordam etkinliği çalıştırabilirsiniz. 

Kopyalama etkinliği şu anda verileri veritabanı geçici tablosuna yüklemeyi yerel olarak desteklemiyor. Birden çok etkinliğin birleşimiyle ayarlanmasının gelişmiş bir yolu vardır ve [SQL veritabanı toplu ön Ekle senaryolarına en iyileştirme](https://github.com/scoriani/azuresqlbulkupsert)bölümüne bakın. Aşağıda, kalıcı bir tablonun hazırlama olarak kullanılması örneği gösterilmektedir.

Örnek olarak, Azure Data Factory, **saklı yordam etkinliği**ile **kopyalama etkinliği** zincirli bir işlem hattı oluşturabilirsiniz. Eski, kaynak deponuzdaki verileri, veri kümesindeki tablo adı olarak **UpsertStagingTable**gıbı Azure SQL yönetilen örnek hazırlama tablosuna kopyalar. İkinci olarak, hazırlama tablosundaki kaynak verileri hedef tabloya birleştirmek ve hazırlama tablosunu temizlemek için bir saklı yordam çağırır.

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

Verileri SQL yönetilen örneği 'ne kopyaladığınızda, kaynak tablodaki her yığın üzerinde ek parametrelerle Kullanıcı tarafından belirtilen bir saklı yordamı yapılandırabilir ve çağırabilirsiniz. Saklı yordam özelliği [tablo değerli parametrelerin](https://msdn.microsoft.com/library/bb675163.aspx)avantajlarından yararlanır.

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

3. Azure Data Factory, kopyalama etkinliğinde **SQL mı havuzu** bölümünü aşağıdaki gibi tanımlayın:

    ```json
    "sink": {
        "type": "SqlMISink",
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

## <a name="data-type-mapping-for-sql-managed-instance"></a>SQL yönetilen örneği için veri türü eşlemesi

Veriler SQL yönetilen örneğine ve öğesinden kopyalandığında, SQL yönetilen örnek veri türleri arasında, geçici veri türlerini Azure Data Factory için aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemadan ve veri türünden havuza nasıl eşlendiğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md).

| SQL yönetilen örnek veri türü | Azure Data Factory geçici veri türü |
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
> Ondalık geçici türle eşlenen veri türleri için şu anda kopyalama etkinliği, 28 ' ye kadar duyarlık destekler. 28 ' den daha büyük bir duyarlık gerektiren verileriniz varsa, bir SQL sorgusunda bir dizeye dönüştürmeyi düşünün.

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="using-always-encrypted"></a>Always Encrypted kullanma

[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-mi-current)Ile Azure SQL yönetilen örneğinden veri kopyaladığınızda, [Genel ODBC Bağlayıcısı](connector-odbc.md) ' nı ve SQL Server ODBC sürücüsünü şirket içinde barındırılan Integration Runtime aracılığıyla kullanın. Bu Azure SQL yönetilen örnek Bağlayıcısı şimdi Always Encrypted desteklemiyor. 

Daha ayrıntılı belirtmek gerekirse:

1. Kendi kendine barındırılan Integration Runtime yoksa ayarlayın. Ayrıntılar için bkz. [Şirket içinde barındırılan Integration Runtime](create-self-hosted-integration-runtime.md) makalesi.

2. SQL Server için 64 bit ODBC sürücüsünü [buradan](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server?view=azuresqldb-mi-current)indirin ve Integration Runtime makinesine yükleyin. Bu sürücünün [SQL Server ODBC sürücüsüyle Always Encrypted kullanarak](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-mi-current#using-the-azure-key-vault-provider)nasıl çalıştığı hakkında daha fazla bilgi edinin.

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
Azure Data Factory içindeki kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).
