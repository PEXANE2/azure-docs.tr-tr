---
title: Azure SQL Veritabanı Yönetilen Örneği'ne veri kopyalama
description: Azure Veri Fabrikası'nı kullanarak verileri Azure SQL Veritabanı Yönetilen Örneği'ne nasıl taşıyıp taşıyarak nasıl taşıyarak öğrenin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 03/12/2020
ms.openlocfilehash: fe803c918cbf60b8f0af76d8c9a94d022153acbb
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417512"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Verileri Azure SQL Veritabanı Yönetilen Örneği'ne kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure SQL Veritabanı Yönetilen Örneği'ne ve Bu Veritabanı'ndan veri kopyalamak için Azure Veri Fabrikası'ndaki kopyalama etkinliğinin nasıl kullanılacağı açıklanmaktadır. Kopyalama etkinliğine genel bir genel bakış sunan [Kopyalama etkinliğine genel bakış](copy-activity-overview.md) makalesi üzerine inşa edin.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure SQL Veritabanı Yönetilen Örnek bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matrisi](copy-activity-overview.md) ile [etkinliği](copy-activity-overview.md) kopyalama
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)

Verileri Azure SQL Veritabanı Yönetilen Örneği'nden desteklenen herhangi bir lavabo veri deposuna kopyalayabilirsiniz. Ayrıca, desteklenen herhangi bir kaynak veri deposundaki verileri yönetilen örneğe kopyalayabilirsiniz. Kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablosuna bakın.

Özellikle, bu Azure SQL Veritabanı Yönetilen Örnek bağlayıcıdestekler:

- SQL kimlik doğrulaması ve Azure Etkin Dizini (Azure AD) Uygulama belirteç kimlik doğrulaması kullanarak azure kaynakları için bir hizmet yöneticisi veya yönetilen kimliklerle verileri kopyalama.
- Kaynak olarak, bir SQL sorgusu veya depolanan yordam ı kullanarak veri alma.
- Lavabo olarak, verileri hedef tabloya eklemek veya kopya sırasında özel mantıkla depolanan yordamı çağırmak.

>[!NOTE]
>Azure SQL Veritabanı Yönetilen Örnek [Her Zaman Şifrelenmiş](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-mi-current) şimdi bu bağlayıcı tarafından desteklenmez. Geçici olarak çalışmak için, kendi barındırılan tümleştirme çalışma süresi aracılığıyla genel bir [ODBC bağlayıcısı](connector-odbc.md) ve SQL Server ODBC sürücüsü kullanabilirsiniz. ODBC sürücü indirme ve bağlantı dize yapılandırmaları ile [bu kılavuzu](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-mi-current) izleyin.

## <a name="prerequisites"></a>Ön koşullar

Azure SQL Veritabanı Yönetilen Örnek [genel bitiş noktasına](../sql-database/sql-database-managed-instance-public-endpoint-securely.md)erişmek için, Azure Veri Fabrikası tarafından yönetilen Azure tümleştirme çalışma süresini kullanabilirsiniz. Azure Veri Fabrikası'nın veritabanınıza bağlanabilmesi için ortak bitiş noktasını etkinleştirdiğinizden ve ağ güvenlik grubunda ki genel uç nokta trafiğine izin verdiğinizden emin olun. Daha fazla bilgi için [bu kılavuza](../sql-database/sql-database-managed-instance-public-endpoint-configure.md)bakın.

Azure SQL Veritabanı Yönetilen Örnek özel bitiş noktasına erişmek için, veritabanına erişebilen, [kendi kendine barındırılan](create-self-hosted-integration-runtime.md) bir tümleştirme çalışma zamanı ayarlayın. Yönetilen örneğiyle aynı sanal ağda barındırılan tümleştirme çalışma süresini sağlarsanız, tümleştirme çalışma zamanı makinenizin yönetilen örneğinizden farklı bir alt ağda olduğundan emin olun. Kendi barındırılan tümleştirme çalışma sürenizi yönetilen örneğinizden farklı bir sanal ağda sağlarsanız, sanal ağ da sanal ağ bağlantısı için sanal ağ eşlemeyi veya sanal ağ kullanabilirsiniz. Daha fazla bilgi için bkz: [Uygulamanızı Azure SQL Veritabanı Yönetilen Örneği'ne bağlayın.](../sql-database/sql-database-managed-instance-connect-app.md)

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde Azure SQL Veritabanı Yönetilen Örnek bağlayıcısına özgü Azure Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Azure SQL Veritabanı Yönetilen Örnek bağlantılı hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Tür özelliği **AzureSqlMI**olarak ayarlanmalıdır. | Evet |
| Connectionstring |Bu özellik, SQL kimlik doğrulaması kullanarak yönetilen örne bağlanmak için gereken **bağlantıString** bilgilerini belirtir. Daha fazla bilgi için aşağıdaki örneklere bakın. <br/>Varsayılan bağlantı noktası 1433'tür. Azure SQL Veritabanı Yönetilen Örneği'ni ortak bir bitiş noktasıyla kullanıyorsanız, 3342 portu açıkça belirtin.<br> Azure Key Vault'a parola da koyabilirsiniz. SQL kimlik doğrulamasıysa, yapılandırmayı `password` bağlantı dizesinin dışına çekin. Daha fazla bilgi için, Azure Anahtar [Kasası'ndaki](store-credentials-in-key-vault.md)tablo yu ve Mağaza kimlik bilgilerini izleyen JSON örneğine bakın. |Evet |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulaması kullandığınızda |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alanı Azure Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizli ye başvuruyapın.](store-credentials-in-key-vault.md) | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulaması kullandığınızda |
| Kiracı | Uygulamanızın bulunduğu alan adı veya kiracı kimliği gibi kiracı bilgilerini belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek alın. | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulaması kullandığınızda |
| connectVia | Bu [tümleştirme çalışma süresi](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılır. Yönetilen örneğinizin ortak bir bitiş noktası varsa ve Azure Veri Fabrikası'nın bu çalışma süresine erişmesine izin veriyorsa, kendi kendine barındırılan bir tümleştirme çalışma zamanı veya Azure tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. |Evet |

Farklı kimlik doğrulama türleri için, sırasıyla önkoşullar ve JSON örnekleri yle ilgili aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- [Azure AD uygulama belirteç kimlik doğrulaması: Hizmet sorumlusu](#service-principal-authentication)
- [Azure AD uygulama belirteç kimlik doğrulaması: Azure kaynakları için yönetilen kimlikler](#managed-identity)

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

**Örnek 1: SQL kimlik doğrulaması kullanın**

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

**Örnek 2: Azure Key Vault'ta parolayla SQL kimlik doğrulamasını kullanın**

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

Hizmet temeli tabanlı Azure AD uygulama belirteç kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. [Yönetilen Örneğiniz için Bir Azure Etkin Dizin yöneticisi sağlama adımlarını](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance)izleyin.

2. Azure portalından [bir Azure Etkin Dizin uygulaması oluşturun.](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) Uygulama adını ve bağlantılı hizmeti tanımlayan aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

3. Azure Veri Fabrikası yönetilen kimliği için oturum açma [lar oluşturun.](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) SQL Server Management Studio'da (SSMS), yönetilen örneğinize **sysadmin**olan bir SQL Server hesabı nı kullanarak bağlanın. **Ana** veritabanında aşağıdaki T-SQL'i çalıştırın:

    ```sql
    CREATE LOGIN [your application name] FROM EXTERNAL PROVIDER
    ```

4. Azure Veri Fabrikası yönetilen kimliği için [veritabanı kullanıcıları oluşturun.](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) Verileri kopyalamak istediğiniz veritabanına veya veritabanına bağlanın, aşağıdaki T-SQL'i çalıştırın: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER
    ```

5. Veri Fabrikası yönetilen kimlik normalde SQL kullanıcıları ve diğerleri için yaptığınız gibi gerekli izinleri verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için [bu belgeye](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql?view=azuresqldb-mi-current)bakın.

    ```sql
    ALTER ROLE [role name e.g. db_owner] ADD MEMBER [your application name]
    ```

6. Azure Veri Fabrikası'nda Azure SQL Veritabanı Yönetilen Örnek bağlantılı bir hizmeti yapılandırma.

**Örnek: hizmet temel kimlik doğrulaması kullanın**

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

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için yönetilen kimlikler

Bir veri fabrikası, belirli veri fabrikasını temsil eden Azure kaynakları için yönetilen bir [kimlikle](data-factory-service-identity.md) ilişkilendirilebilir. Bu yönetilen kimliği Azure SQL Veritabanı Yönetilen Örnek kimlik doğrulaması için kullanabilirsiniz. Atanan fabrika, bu kimliği kullanarak verilere erişebilir ve veritabanınızdan erişebilir ve verileri kopyalayabilir.

Yönetilen kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin.

1. [Yönetilen Örneğiniz için Bir Azure Etkin Dizin yöneticisi sağlama adımlarını](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance)izleyin.

2. Azure Veri Fabrikası yönetilen kimliği için oturum açma [lar oluşturun.](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) SQL Server Management Studio'da (SSMS), yönetilen örneğinize **sysadmin**olan bir SQL Server hesabı nı kullanarak bağlanın. **Ana** veritabanında aşağıdaki T-SQL'i çalıştırın:

    ```sql
    CREATE LOGIN [your Data Factory name] FROM EXTERNAL PROVIDER
    ```

3. Azure Veri Fabrikası yönetilen kimliği için [veritabanı kullanıcıları oluşturun.](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) Verileri kopyalamak istediğiniz veritabanına veya veritabanına bağlanın, aşağıdaki T-SQL'i çalıştırın: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER
    ```

4. Veri Fabrikası yönetilen kimlik normalde SQL kullanıcıları ve diğerleri için yaptığınız gibi gerekli izinleri verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için [bu belgeye](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql?view=azuresqldb-mi-current)bakın.

    ```sql
    ALTER ROLE [role name e.g. db_owner] ADD MEMBER [your Data Factory name]
    ```

5. Azure Veri Fabrikası'nda Azure SQL Veritabanı Yönetilen Örnek bağlantılı bir hizmeti yapılandırma.

**Örnek: yönetilen kimlik doğrulamasını kullanır**

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

Veri kümelerini tanımlamak için kullanılabilecek bölümlerin ve özelliklerin tam listesi için veri kümeleri makalesine bakın. Bu bölümde, Azure SQL Veritabanı Yönetilen Örnek veri kümesi tarafından desteklenen özelliklerin bir listesi yer almaktadır.

Azure SQL Veritabanı Yönetilen Örneği'ne ve verilerinden kopyalanması için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin tür özelliği **AzureSqlMITable**olarak ayarlanmalıdır. | Evet |
| Şema | Şema adı. |Kaynak için hayır, lavabo için Evet  |
| tablo | Tablonun/görünümün adı. |Kaynak için hayır, lavabo için Evet  |
| tableName | Şema ile tablo/görünüm adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü `schema` `table`için, kullanım ve . | Kaynak için hayır, lavabo için Evet |

**Örnek**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "AzureSqlMITable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
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

Etkinlikleri tanımlamak için kullanılabilecek bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Azure SQL Veritabanı Yönetilen Örnek kaynağı ve lavabotarafından desteklenen özelliklerin bir listesi bulunmaktadır.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Kaynak Olarak Azure SQL Veritabanı Yönetilen Örnek

Azure SQL Veritabanı Yönetilen Örneği'nden verileri kopyalamak için, kopyalama etkinliği kaynak bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının tür özelliği **SqlMISource**olarak ayarlanmalıdır. | Evet |
| sqlReaderQuery |Bu özellik, verileri okumak için özel SQL sorgusunu kullanır. `select * from MyTable` bunun bir örneğidir. |Hayır |
| sqlReaderStoredProcedureNameName |Bu özellik, kaynak tablodaki verileri okuyan depolanan yordamın adıdır. Son SQL deyimi, depolanan yordamda bir SELECT deyimi olmalıdır. |Hayır |
| depolanmışProsedürParametreleri |Bu parametreler depolanan yordam içindir.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. |Hayır |
| ısolationlevel | SQL kaynağı için hareket kilitleme davranışını belirtir. İzin verilen değerler şunlardır: **ReadCommitted** (varsayılan), **ReadUncommitted**, **Tekrarlanabilir Oku**, **Serializable**, **Anlık Görüntü**. Daha fazla bilgi için [bu dokümana](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) bakın. | Hayır |

**Aşağıdaki noktalara dikkat edin:**

- **SqlMISource**için **sqlReaderQuery** belirtilirse, kopyalama etkinliği verileri almak için bu sorguyu yönetilen örnek kaynağa göre çalıştırır. Ayrıca **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** belirterek depolanan yordamı belirtebilirsiniz.
- **SqlReaderQuery** veya **sqlReaderStoredProcedureName** özelliğini belirtmezseniz, JSON veri kümesinin "yapı" bölümünde tanımlanan sütunlar sorgu oluşturmak için kullanılır. Sorgu `select column1, column2 from mytable` yönetilen örneğe karşı çalışır. Veri kümesi tanımında "yapı" yoksa, tüm sütunlar tablodan seçilir.

**Örnek: SQL sorgusu kullanma**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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

**Örnek: Depolanan yordamı kullanma**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Lavabo Olarak Azure SQL Veritabanı Yönetilen Örnek

> [!TIP]
> Azure SQL Veritabanı Yönetilen Örneği'ne veri yüklemek için [en iyi uygulamadan](#best-practice-for-loading-data-into-azure-sql-database-managed-instance)desteklenen yazma davranışları, yapılandırmalar ve en iyi uygulamalar hakkında daha fazla bilgi edinin.

Verileri Azure SQL Veritabanı Yönetilen Örneği'ne kopyalamak için, kopyalama etkinliği bünyesinde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun türü özelliği **SqlMISink**olarak ayarlanmalıdır. | Evet |
| yazmaBatchSize |*Toplu iş başına*SQL tablosuna eklenecek satır sayısı.<br/>İzin verilen değerler satır sayısı için arayıcılardır. Varsayılan olarak, Azure Veri Fabrikası satır boyutuna bağlı olarak uygun toplu iş boyutunu dinamik olarak belirler.  |Hayır |
| yazmaBatchTimeout |Bu özellik, toplu ekleme işleminin zaman dolmadan tamamlanması için bekleme süresini belirtir.<br/>İzin verilen değerler zaman kaydırma içindir. Bir örnek "00:30:00," olan 30 dakikadır. |Hayır |
| preCopyScript |Bu özellik, yönetilen örneğe veri yazmadan önce kopyalama etkinliğinin çalışması için bir SQL sorgusu belirtir. Kopya başına yalnızca bir kez çağrılır. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanabilirsiniz. |Hayır |
| sqlWriterStoredProcedureNameName | Kaynak verilerin hedef tabloya nasıl uygulanacağı konusunda tanımlanan depolanan yordamın adı. <br/>Bu depolanan yordam *toplu iş başına çağrılır.* Yalnızca bir kez çalışan ve kaynak verilerle hiçbir ilgisi olmayan işlemler için , `preCopyScript` örneğin silme veya bulanın, özelliği kullanın. | Hayır |
| depolanmışProcedureTableTypeParameterName |Depolanan yordamda belirtilen tablo türünün parametre adı.  |Hayır |
| sqlWriterTableType |Depolanan yordamda kullanılacak tablo türü adı. Kopyalama etkinliği, verilerin bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Depolanan yordam kodu, daha sonra kopyalanan verileri varolan verilerle birleştirilebilir. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler.<br/>İzin verilen değerler ad ve değer çiftleridir. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. | Hayır |
| Tableoption | Kaynak şemaya göre yoksa, lavabo tablosunun otomatik olarak oluşturulup oluşturulmayacağını belirtir. Otomatik tablo oluşturma, lavabo saklı yordamı belirtir veya sahnelenen kopya kopyalama etkinliğinde yapılandırılır desteklenmez. İzin verilen `none` değerler şunlardır: (varsayılan), `autoCreate`. |Hayır |

**Örnek 1: Ek veri**

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
                "referenceName": "<Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlMISink",
                "writeBatchSize": 100000,
                "tableOption": "autoCreate"
            }
        }
    }
]
```

**Örnek 2: Kopya sırasında depolanan yordamı çağırma**

SQL MI [lavabodan depolanan yordamı Çağır'dan](#invoke-a-stored-procedure-from-a-sql-sink)daha fazla ayrıntı edinin.

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
                "referenceName": "<Managed Instance output dataset name>",
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

## <a name="best-practice-for-loading-data-into-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği'ne veri yüklemek için en iyi yöntem

Verileri Azure SQL Veritabanı Yönetilen Örneği'ne kopyaladiğinizde, farklı yazma davranışı gerektirebilir:

- [Ek](#append-data): Kaynak verilerimde yalnızca yeni kayıtlar var.
- [Upsert](#upsert-data): Kaynak verilerimde hem ekler hem de güncelleştirmeler vardır.
- [Overwrite](#overwrite-the-entire-table): Her seferinde tüm boyut tablosunu yeniden yüklemek istiyorum.
- [Özel mantıkla yazın](#write-data-with-custom-logic): Hedef tabloya son eklemeden önce ekstra işleme ihtiyacım var. 

Azure Veri Fabrikası'nda ve en iyi uygulamalarda nasıl yapılandırılabilenilgili bölümlere bakın.

### <a name="append-data"></a>Veri ekle

Veri ekle, bu Azure SQL Veritabanı Yönetilen Örnek lavabo bağlayıcısının varsayılan davranışıdır. Azure Veri Fabrikası, tablonuza verimli bir şekilde yazmak için toplu bir ekleme yapar. Kaynağı kopya etkinliğinde buna göre yapılandırabilir ve bekleyebilirsiniz.

### <a name="upsert-data"></a>Verileri upsert etme

**Seçenek 1:** Kopyalamanız gereken büyük miktarda veri varsa, bir yükseltme yapmak için aşağıdaki yaklaşımı kullanın: 

- İlk olarak, kopyalama etkinliğini kullanarak tüm kayıtları toplu yüklemek için geçici bir [tablo](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) kullanın. Geçici tablolara yönelik işlemler günlüğe kaydolmadığından, milyonlarca kaydı saniyeler içinde yükleyebilirsiniz.
- [Birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) veya EKLE/UPDATE deyimi uygulamak için Azure Veri Fabrikası'nda depolanan yordam etkinliği çalıştırın. Tüm güncelleştirmeleri veya eklertek bir işlem olarak gerçekleştirmek için kaynak olarak geçici tabloyu kullanın. Bu şekilde, gidiş-dönüş ve günlük işlemlerinin sayısı azalır. Depolanan yordam etkinliğinin sonunda, geçici tablo bir sonraki yükseltme döngüsüne hazır olması için kesilir.

Örnek olarak, Azure Veri Fabrikası'nda, **Depolanmış Yordam etkinliğiyle**zincirlenmiş bir **Kopyalama etkinliği** içeren bir ardışık işlem hattı oluşturabilirsiniz. Kaynak deponuzdaki eski verileri geçici bir tabloya kopyalar, örneğin, **##UpsertTempTable**, veri kümesindeki tablo adı olarak. Daha sonra ikincisi, geçici tablodaki kaynak verileri hedef tabloya birleştirmek ve geçici tabloyu temizlemek için depolanmış bir yordam çağırır.

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

**Seçenek 2:** Ayrıca, bir [kopyalama etkinliği içinde depolanan yordamı çağırmayı](#invoke-a-stored-procedure-from-a-sql-sink)da seçebilirsiniz. Bu yaklaşım, büyük ölçekli upsert için uygun olmayan kopyalama etkinliğinde varsayılan yaklaşım olarak toplu ekleme kullanmak yerine kaynak tablodaki her satırı çalıştırAr.

### <a name="overwrite-the-entire-table"></a>Tüm tablonun üzerine yazma

**CopyScript** özelliğini bir kopyalama etkinliği lavabosunda yapılandırabilirsiniz. Bu durumda, çalışan her kopyalama etkinliği için önce Azure Veri Fabrikası komut dosyasını çalıştırın. Sonra verileri eklemek için kopya çalışır. Örneğin, en son verilerle tüm tablonun üzerine yazmak için, yeni verileri kaynaktan toplu olarak yüklemeden önce tüm kayıtları silmek için bir komut dosyası belirtin.

### <a name="write-data-with-custom-logic"></a>Özel mantıkla veri yazma

Özel mantıkla veri yazma [adımları, Upsert veri](#upsert-data) bölümünde açıklananlara benzer. Büyük ölçekli için, hedef tabloya kaynak verilerin son eklenmesinden önce ekstra işleme uygulamanız gerektiğinde, iki şeyden birini yapabilirsiniz: 

- Geçici bir tabloya yükleyin ve ardından depolanan yordamı çağırın.
- Kopyalama sırasında depolanan yordamı çağırın.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a>SQL lavabodan depolanan yordamı çağırma

Verileri Azure SQL Veritabanı Yönetilen Örneği'ne kopyaladiğinizde, kullanıcı tarafından belirtilen bir depo yordamı ek parametrelerle yapılandırabilir ve çağırabilirsiniz. Depolanan yordam özelliği [tablo değerindeki parametrelerden](https://msdn.microsoft.com/library/bb675163.aspx)yararlanır.

> [!TIP]
> Depolanan yordamı çağırmak, büyük ölçekli kopya için önermediğimiz toplu bir işlem kullanmak yerine veri satırını satır satır işler. [Azure SQL Veritabanı Yönetilen Örneği'ne veri yüklemek için en iyi uygulamadan](#best-practice-for-loading-data-into-azure-sql-database-managed-instance)daha fazla bilgi edinin.

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

3. Azure Veri Fabrikası'nda, kopyalama etkinliğindeki **SQL MI lavabo** bölümünü aşağıdaki gibi tanımlayın:

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

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği için veri türü eşleme

Veriler Azure SQL Veritabanı Yönetilen Örneği'ne ve sürümünden kopyalandığında, Aşağıdaki eşlemeler Azure SQL Veritabanı Yönetilen Örnek veri türlerinden Azure Veri Fabrikası geçici veri türlerine kadar kullanılır. Kopyalama etkinliğinin kaynak şema ve veri türünden lavaboya nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| Azure SQL Veritabanı Yönetilen Örnek veri türü | Azure Veri Fabrikası geçici veri türü |
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

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için desteklenen [veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
