---
title: Azure SYNAPSE Analytics 'te verileri kopyalama ve dönüştürme
description: Azure SYNAPSE Analytics 'e ve veri kopyalama ve Data Factory kullanarak Azure SYNAPSE Analytics 'teki verileri dönüştürme hakkında bilgi edinin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/28/2020
ms.openlocfilehash: cd14a183ae1434af83c96b7f8d6575186412b534
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89051228"
---
# <a name="copy-and-transform-data-in-azure-synapse-analytics-formerly-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SYNAPSE Analytics 'te (eski adıyla Azure SQL veri ambarı) veri kopyalama ve dönüştürme

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Data Factory hizmeti sürümünü seçin:"]
>
> - [Version1](v1/data-factory-azure-sql-data-warehouse-connector.md)
> - [Güncel sürüm](connector-azure-sql-data-warehouse.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure SYNAPSE Analytics 'ten verileri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir ve Azure Data Lake Storage 2. verileri dönüştürmek için veri akışı kullanılır. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure SYNAPSE Analytics Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Etkinliği](copy-activity-overview.md) [Desteklenen kaynak/havuz matris](copy-activity-overview.md) tablosuyla Kopyala
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)

Kopyalama etkinliği için bu Azure SYNAPSE Analytics Bağlayıcısı şu işlevleri destekler:

- Azure kaynakları için hizmet sorumlusu veya yönetilen kimlikler ile SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulamasını kullanarak verileri kopyalayın.
- Kaynak olarak, bir SQL sorgusu veya saklı yordam kullanarak verileri alın. Ayrıca, Azure SYNAPSE Analytics kaynağından paralel kopyalama seçeneğini de belirleyebilirsiniz. Ayrıntılar için [SYNAPSE Analytics 'Ten paralel kopyalama](#parallel-copy-from-synapse-analytics) bölümüne bakın.
- Havuz olarak, [PolyBase](#use-polybase-to-load-data-into-azure-sql-data-warehouse) veya [Copy deyiminizi](#use-copy-statement) (Önizleme) veya toplu ekleme 'yi kullanarak verileri yükleyin. Daha iyi kopyalama performansı için PolyBase veya COPY deyiminizi (Önizleme) öneririz. Bağlayıcı, kaynak şemasına bağlı değilse otomatik olarak hedef tablo oluşturmayı da destekler.

> [!IMPORTANT]
> Azure Data Factory Integration Runtime kullanarak verileri kopyalarsanız, Azure hizmetlerinin [MANTıKSAL SQL Server](../azure-sql/database/logical-servers.md)'a erişebilmesi için [sunucu düzeyinde bir güvenlik duvarı kuralı](../azure-sql/database/firewall-configure.md) yapılandırın.
> Şirket içinde barındırılan tümleştirme çalışma zamanı kullanarak verileri kopyalarsanız, güvenlik duvarını uygun IP aralığına izin verecek şekilde yapılandırın. Bu Aralık, Azure SYNAPSE Analytics 'e bağlanmak için kullanılan makinenin IP 'sini içerir.

## <a name="get-started"></a>başlarken

> [!TIP]
> En iyi performansı elde etmek için PolyBase 'i kullanarak Azure SYNAPSE Analytics 'e veri yükleyin. [Azure SYNAPSE Analytics 'e veri yüklemek Için PolyBase kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse) bölümünde ayrıntılar bulunur. Kullanım örneği ile ilgili bir anlatım için, [Azure Data Factory ile 15 dakika altında Azure SYNAPSE Analytics 'e 1 TB yükleme](load-azure-sql-data-warehouse.md)bölümüne bakın.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, bir Azure SYNAPSE Analytics bağlayıcısına özgü Data Factory varlıkları tanımlayan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Aşağıdaki özellikler bir Azure SYNAPSE Analytics bağlı hizmeti için desteklenir:

| Özellik            | Açıklama                                                  | Gerekli                                                     |
| :------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| tür                | Type özelliği **Azuresqldw**olarak ayarlanmalıdır.             | Yes                                                          |
| Dizisi    | **ConnectionString** özelliği Için Azure SYNAPSE Analytics örneğine bağlanmak için gereken bilgileri belirtin. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca, parola/hizmet sorumlusu anahtarını Azure Key Vault de koyabilirsiniz ve SQL kimlik doğrulaması ise `password` yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha ayrıntılı bilgi için tablonun altındaki JSON örneğine bakın ve [kimlik bilgilerini Azure Key Vault makalesine saklayın](store-credentials-in-key-vault.md) . | Yes                                                          |
| Serviceprincipalıd  | Uygulamanın istemci KIMLIĞINI belirtin.                         | Evet, Azure AD kimlik doğrulamasını bir hizmet sorumlusu ile kullandığınızda. |
| Servicesprincipalkey | Uygulamanın anahtarını belirtin. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Evet, Azure AD kimlik doğrulamasını bir hizmet sorumlusu ile kullandığınızda. |
| Kiracı              | Uygulamanızın altında bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı KIMLIĞI) belirtin. Fareyi, Azure portal sağ üst köşesine getirerek alabilirsiniz. | Evet, Azure AD kimlik doğrulamasını bir hizmet sorumlusu ile kullandığınızda. |
| Azurecses türü | Hizmet sorumlusu kimlik doğrulaması için, Azure AD uygulamanızın kaydedildiği Azure bulut ortamının türünü belirtin. <br/> İzin verilen değerler **Azucumhuriyeti**, **AzureChina**, **AzureUsGovernment**ve **AzureGermany**. Varsayılan olarak, Data Factory 'nin bulut ortamı kullanılır. | Hayır |
| connectVia          | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolubir özel ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. | Hayır                                                           |

Farklı kimlik doğrulama türleri için sırasıyla Önkoşullar ve JSON örnekleri hakkında aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- Azure AD uygulama belirteci kimlik doğrulaması: [hizmet sorumlusu](#service-principal-authentication)
- Azure AD uygulama belirteci kimlik doğrulaması: [Azure kaynakları Için Yönetilen kimlikler](#managed-identity)

>[!TIP]
>"UserErrorFailedToConnectToSqlServer" adlı hata kodu ile hata ve "veritabanı için oturum sınırı XXX ve erişildi." gibi bir hatayla karşılaşırsanız, `Pooling=false` Bağlantı dizenizi ekleyin ve yeniden deneyin.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulaması kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

**Azure Key Vault parola:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
            "password": {
                "type": "AzureKeyVaultSecret",
                "store": {
                    "referenceName": "<Azure Key Vault linked service name>",
                    "type": "LinkedServiceReference"
                },
                "secretName": "<secretName>"
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

1. Azure portal **[bir Azure Active Directory uygulaması oluşturun](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal)** . Uygulama adını ve bağlı hizmeti tanımlayan aşağıdaki değerleri unutmayın:

   - Uygulama Kimliği
   - Uygulama anahtarı
   - Kiracı Kimliği

2. Daha önce yapmadıysanız Azure portal sunucunuz için **[bir Azure Active Directory Yöneticisi sağlayın](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database)** . Azure AD yöneticisi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Gruba yönetilen kimliğe sahip bir yönetici rolü verirseniz, 3 ve 4. adımları atlayın. Yöneticinin veritabanına tam erişimi olur.

3. Hizmet sorumlusu için **[Kapsanan Veritabanı kullanıcıları oluşturun](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities)** . SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz veri ambarına ya da en az BIR kullanıcı izni olan bir Azure AD kimliğiyle bağlanın. Aşağıdaki T-SQL ' i çalıştırın:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. SQL kullanıcıları veya diğerleri için normalde yaptığınız sürece **Hizmet sorumlusuna gerekli Izinleri verin** . Aşağıdaki kodu çalıştırın veya daha [fazla seçeneğe bakın](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Verileri yüklemek için PolyBase kullanmak istiyorsanız [gerekli veritabanı iznini](#required-database-permission)öğrenin.

    ```sql
    EXEC sp_addrolemember db_owner, [your application name];
    ```

5. Azure Data Factory 'de **Azure SYNAPSE Analytics bağlı hizmetini yapılandırın** .

#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a> Azure kaynakları kimlik doğrulaması için Yönetilen kimlikler

Bir veri fabrikası, belirli bir fabrikası temsil eden [Azure kaynakları için yönetilen bir kimlikle](data-factory-service-identity.md) ilişkilendirilebilir. Bu yönetilen kimliği, Azure SYNAPSE Analytics kimlik doğrulaması için kullanabilirsiniz. Belirlenen fabrika, bu kimliği kullanarak veri ambarınıza veya verilere erişebilir ve bu verileri kopyalayabilir.

Yönetilen kimlik kimlik doğrulamasını kullanmak için şu adımları izleyin:

1. Daha önce yapmadıysanız Azure portal sunucunuza yönelik **[bir Azure Active Directory Yöneticisi sağlayın](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database)** . Azure AD yöneticisi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Gruba yönetilen kimliğe sahip bir yönetici rolü verirseniz, 3 ve 4. adımları atlayın. Yöneticinin veritabanına tam erişimi olur.

2. Data Factory yönetilen kimlik için **[Kapsanan Veritabanı kullanıcıları oluşturun](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities)** . SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz veri ambarına ya da en az BIR kullanıcı izni olan bir Azure AD kimliğiyle bağlanın. Aşağıdaki T-SQL ' i çalıştırın.
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. SQL kullanıcıları ve diğerleri için normalde yaptığınız gibi **Data Factory yönetilen kimliğe gerekli Izinleri verin** . Aşağıdaki kodu çalıştırın veya daha [fazla seçeneğe bakın](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Verileri yüklemek için PolyBase kullanmak istiyorsanız [gerekli veritabanı iznini](#required-database-permission)öğrenin.

    ```sql
    EXEC sp_addrolemember db_owner, [your Data Factory name];
    ```

4. Azure Data Factory 'de **Azure SYNAPSE Analytics bağlı hizmetini yapılandırın** .

**Örnek:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi.

Aşağıdaki özellikler Azure SYNAPSE Analytics veri kümesi için desteklenir:

| Özellik  | Açıklama                                                  | Gerekli                    |
| :-------- | :----------------------------------------------------------- | :-------------------------- |
| tür      | Veri kümesinin **Type** özelliği **Azuresqldwtable**olarak ayarlanmalıdır. | Yes                         |
| schema | Şemanın adı. |Kaynak için Hayır, havuz için Evet  |
| table | Tablo/görünüm adı. |Kaynak için Hayır, havuz için Evet  |
| tableName | Şema ile tablonun/görünümün adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü için `schema` ve kullanın `table` . | Kaynak için Hayır, havuz için Evet |

### <a name="dataset-properties-example"></a>DataSet özellikleri örneği

```json
{
    "name": "AzureSQLDWDataset",
    "properties":
    {
        "type": "AzureSqlDWTable",
        "linkedServiceName": {
            "referenceName": "<Azure Synapse Analytics linked service name>",
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

## <a name="copy-activity-properties"></a>Kopyalama etkinliği özellikleri

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Azure SYNAPSE Analytics kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-synapse-analytics-as-the-source"></a>Kaynak olarak Azure SYNAPSE Analytics

>[!TIP]
>Veri bölümleme kullanarak Azure SYNAPSE Analytics 'ten verileri verimli bir şekilde yüklemek için [SYNAPSE Analytics 'Ten paralel kopyadan](#parallel-copy-from-synapse-analytics)daha fazla bilgi edinin.

Azure SYNAPSE Analytics 'ten veri kopyalamak için kopyalama etkinliği kaynağındaki **Type** özelliğini **sqldwsource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik                     | Açıklama                                                  | Gerekli |
| :--------------------------- | :----------------------------------------------------------- | :------- |
| tür                         | Kopyalama etkinliği kaynağının **Type** özelliği **sqldwsource**olarak ayarlanmalıdır. | Yes      |
| sqlReaderQuery               | Verileri okumak için özel SQL sorgusunu kullanın. Örnek: `select * from MyTable`. | Hayır       |
| sqlReaderStoredProcedureName | Kaynak tablodaki verileri okuyan saklı yordamın adı. Son SQL ifadesinin saklı yordamda bir SELECT ifadesinin olması gerekir. | Hayır       |
| storedProcedureParameters    | Saklı yordamın parametreleri.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve büyük harfleri, saklı yordam parametrelerinin adlarıyla ve büyük küçük harfleriyle aynı olmalıdır. | Hayır       |
| 'Sinden | SQL kaynağı için işlem kilitleme davranışını belirtir. İzin verilen değerler: **ReadCommitted**, **READUNCOMMITTED**, **RepeatableRead**, **Serializable**, **Snapshot**. Belirtilmemişse, veritabanının varsayılan yalıtım düzeyi kullanılır. Daha fazla ayrıntı için [Bu belgeye](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) başvurun. | Hayır |
| partitionOptions | Azure SYNAPSE Analytics 'ten veri yüklemek için kullanılan veri bölümleme seçeneklerini belirtir. <br>İzin verilen değerler: **none** (default), **Physicalpartitionsoftable**ve **DynamicRange**.<br>Bir bölüm seçeneği etkinleştirildiğinde (yani `None` ), Azure SYNAPSE Analytics 'ten eş zamanlı olarak veri yükleme derecesi, [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) kopyalama etkinliğindeki ayar tarafından denetlenir. | Hayır |
| partitionSettings | Veri bölümleme için ayarların grubunu belirtin. <br>Bölüm seçeneği olmadığında uygulayın `None` . | Hayır |
| ***Altında `partitionSettings` :*** | | |
| partitionColumnName | Paralel kopya için Aralık bölümleme tarafından kullanılacak **tamsayı veya tarih/saat türü içinde** kaynak sütunun adını belirtin. Belirtilmemişse, tablonun dizini veya birincil anahtarı otomatik olarak algılanır ve bölüm sütunu olarak kullanılır.<br>Bölüm seçeneği olduğunda uygulayın `DynamicRange` . Kaynak verileri almak için bir sorgu kullanırsanız,  `?AdfDynamicRangePartitionCondition ` WHERE yan tümcesinde kanca. Örnek için, [SQL veritabanı 'Ndan paralel kopyalama](#parallel-copy-from-synapse-analytics) bölümüne bakın. | Hayır |
| Partitionüstsınırı | Bölüm aralığı bölme için bölüm sütununun en büyük değeri. Bu değer, tablodaki satırları filtrelemeye yönelik değil, bölümün ilerlemesine karar vermek için kullanılır. Tablodaki veya sorgu sonucundaki tüm satırlar bölümlenecek ve kopyalanabilir. Belirtilmemişse, kopyalama etkinliği değeri otomatik olarak algılar.  <br>Bölüm seçeneği olduğunda uygulayın `DynamicRange` . Örnek için, [SQL veritabanı 'Ndan paralel kopyalama](#parallel-copy-from-synapse-analytics) bölümüne bakın. | Hayır |
| Partitionalme sınırı | Bölüm aralığı bölme için bölüm sütununun en küçük değeri. Bu değer, tablodaki satırları filtrelemeye yönelik değil, bölümün ilerlemesine karar vermek için kullanılır. Tablodaki veya sorgu sonucundaki tüm satırlar bölümlenecek ve kopyalanabilir. Belirtilmemişse, kopyalama etkinliği değeri otomatik olarak algılar.<br>Bölüm seçeneği olduğunda uygulayın `DynamicRange` . Örnek için, [SQL veritabanı 'Ndan paralel kopyalama](#parallel-copy-from-synapse-analytics) bölümüne bakın. | Hayır |

**Örnek: SQL sorgusu kullanma**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
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
                "type": "SqlDWSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Örnek: saklı yordamı kullanma**

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDW",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL DW input dataset name>",
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
                "type": "SqlDWSource",
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

**Örnek saklı yordam:**

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

### <a name="azure-synapse-analytics-as-sink"></a><a name="azure-sql-data-warehouse-as-sink"></a> Havuz olarak Azure SYNAPSE Analytics

Azure Data Factory, verileri SQL veri ambarı 'na yüklemek için üç yolu destekler.

![SQL DW havuz kopyalama seçenekleri](./media/connector-azure-sql-data-warehouse/sql-dw-sink-copy-options.png)

- [PolyBase kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse)
- [COPY ifadesini kullanma (Önizleme)](#use-copy-statement)
- Toplu ekleme kullan

Veri yükleme en hızlı ve en ölçeklenebilir yolu [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) veya [Copy deyimindedir](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (Önizleme).

Azure SQL veri ambarı 'na veri kopyalamak için kopyalama etkinliğindeki havuz türünü **Sqldwsink**olarak ayarlayın. Aşağıdaki özellikler, kopyalama etkinliği **havuzu** bölümünde desteklenir:

| Özellik          | Açıklama                                                  | Gerekli                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| tür              | Kopyalama etkinliği havuzunun **Type** özelliği **sqldwsink**olarak ayarlanmalıdır. | Yes                                           |
| allowPolyBase     | SQL veri ambarı 'na veri yüklemek için PolyBase kullanılıp kullanılmayacağını belirtir. `allowCopyCommand` ve `allowPolyBase` her ikisi de true olamaz. <br/><br/>Kısıtlamalar ve Ayrıntılar için bkz. [PolyBase 'ı kullanarak Azure SQL veri ambarı 'na veri yükleme](#use-polybase-to-load-data-into-azure-sql-data-warehouse) bölümüne bakın.<br/><br/>İzin verilen değerler **true** ve **false** (varsayılan) şeklindedir. | Hayır.<br/>PolyBase kullanırken uygulayın.     |
| polyBaseSettings  | `allowPolybase`Özelliği **true**olarak ayarlandığında belirtilenebilir bir özellik grubu. | Hayır.<br/>PolyBase kullanırken uygulayın. |
| allowCopyCommand | SQL veri ambarı 'na veri yüklemek için [kopyalama ifadesinin](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (Önizleme) kullanılıp kullanılmayacağını gösterir. `allowCopyCommand` ve `allowPolyBase` her ikisi de true olamaz. <br/><br/>Kısıtlamalar ve Ayrıntılar için bkz. [kopyalama Ifadesini kullanarak Azure SQL veri ambarı 'na veri yükleme](#use-copy-statement) bölümüne bakın.<br/><br/>İzin verilen değerler **true** ve **false** (varsayılan) şeklindedir. | Hayır.<br>KOPYALAMA kullanırken uygulayın. |
| copyCommandSettings | Özelliği TRUE olarak ayarlandığında belirtilenebilir bir özellik grubu `allowCopyCommand` . | Hayır.<br/>KOPYALAMA kullanırken uygulayın. |
| writeBatchSize    | **Toplu iş BAŞıNA**SQL tablosuna eklenecek satır sayısı.<br/><br/>İzin verilen değer **Integer** (satır sayısı). Varsayılan olarak, Data Factory satır boyutuna göre uygun toplu iş boyutunu dinamik olarak belirler. | Hayır.<br/>Toplu ekleme kullanırken uygulayın.     |
| writeBatchTimeout | Toplu ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi.<br/><br/>İzin verilen değer **TimeSpan**değeridir. Örnek: "00:30:00" (30 dakika). | Hayır.<br/>Toplu ekleme kullanırken uygulayın.        |
| Ön Copyscrıpt     | Her çalıştırmada Azure SQL veri ambarı 'na veri yazmadan önce çalıştırılacak bir SQL sorgusu belirtin. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanın. | Hayır                                            |
| tableOption | Kaynak şemasına göre yoksa [Havuz tablosunun otomatik olarak oluşturulup](copy-activity-overview.md#auto-create-sink-tables) oluşturulmayacağını belirtir. İzin verilen değerler: `none` (varsayılan), `autoCreate` . |Hayır |
| disableMetricsCollection | Data Factory, kopyalama performansı iyileştirmesi ve önerileri için SQL veri ambarı DWUs gibi ölçümleri toplar. Bu davranışla ilgileniyorlarsa, `true` devre dışı bırakmak için belirtin. | Hayır (varsayılan değer `false` ) |

#### <a name="sql-data-warehouse-sink-example"></a>SQL veri ambarı havuz örneği

```json
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

## <a name="parallel-copy-from-synapse-analytics"></a>SYNAPSE Analytics 'ten paralel kopya

Copy etkinliğinde Azure SYNAPSE Analytics Connector, verileri paralel olarak kopyalamak için yerleşik veri bölümlendirme sağlar. Kopyalama etkinliğinin **kaynak** sekmesinde veri bölümleme seçeneklerini bulabilirsiniz.

![Bölüm seçeneklerinin ekran görüntüsü](./media/connector-sql-server/connector-sql-partition-options.png)

Bölümlenmiş kopyayı etkinleştirdiğinizde kopyalama etkinliği, verileri bölümlere göre yüklemek için Azure SYNAPSE Analytics kaynağınıza karşı paralel sorgular çalıştırır. Paralel derece [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) kopyalama etkinliğindeki ayar tarafından denetlenir. Örneğin, `parallelCopies` dört olarak ayarlarsanız, Data Factory aynı anda, belirtilen bölüm seçeneğiniz ve ayarlarınıza göre dört sorgu üretir ve çalışır ve her sorgu Azure SYNAPSE analizinizden verilerin bir kısmını alır.

Azure SYNAPSE Analizinizden büyük miktarda veri yüklerken özellikle veri bölümleme ile paralel kopyayı etkinleştirmeniz önerilir. Farklı senaryolar için önerilen yapılandırma aşağıda verilmiştir. Dosya tabanlı veri deposuna veri kopyalarken, bir klasöre birden çok dosya (yalnızca klasör adı belirtin) yazılması önerilir, bu durumda performans tek bir dosyaya yazılmasından daha iyidir.

| Senaryo                                                     | Önerilen ayarlar                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Fiziksel bölümlerle büyük tablodan tam yük.        | **Bölüm seçeneği**: tablonun fiziksel bölümleri. <br><br/>Yürütme sırasında Data Factory fiziksel bölümleri otomatik olarak algılar ve verileri bölümlere göre kopyalar. |
| Fiziksel bölümler olmadan, veri bölümlendirme için bir tamsayı veya tarih saat sütunuyla büyük tablodan tam yük. | **Bölüm seçenekleri**: Dinamik Aralık bölümü.<br>**Bölüm sütunu** (isteğe bağlı): verileri bölümlemek için kullanılan sütunu belirtin. Belirtilmezse, dizin veya birincil anahtar sütunu kullanılır.<br/>**Bölüm üst sınırı** ve **bölüm alt sınırı** (isteğe bağlı): Bölüm Ilerlemesiyle mı belirlemek istediğinizi belirtin. Bu, tablodaki satırları filtrelemeye yönelik değildir, tablodaki tüm satırlar bölümlenecek ve kopyalanır. Belirtilmemişse, kopyalama etkinliği değerleri otomatik olarak algılar.<br><br>Örneğin, "ID" adlı bölüm sütununuzu 1 ile 100 arasında değerler varsa ve alt sınırı 20 ve üst sınır olarak 80 olarak ayarlarsanız, paralel kopyalama Data Factory 4 ' e kadar, verileri 4 bölüm kimliği aralığında <= 20, [21, 50], [51, 80] ve >= 81 sırasıyla alır. |
| Fiziksel bölümler olmadan, veri bölümlendirme için bir tamsayı veya tarih/tarih/saat sütunuyla büyük miktarda veri yükleyin. | **Bölüm seçenekleri**: Dinamik Aralık bölümü.<br>**Sorgu**: `SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>` .<br>**Bölüm sütunu**: verileri bölümlemek için kullanılan sütunu belirtin.<br>**Bölüm üst sınırı** ve **bölüm alt sınırı** (isteğe bağlı): Bölüm Ilerlemesiyle mı belirlemek istediğinizi belirtin. Bu, tablodaki satırları filtrelemeye yönelik değildir, sorgu sonucundaki tüm satırlar bölümlenecek ve kopyalanır. Belirtilmemişse, kopyalama etkinliği değeri otomatik olarak algılar.<br><br>Yürütme sırasında, Data Factory `?AdfRangePartitionColumnName` her bölüm için gerçek sütun adı ve değer aralıklarıyla değiştirilir ve Azure SYNAPSE Analytics 'e gönderir. <br>Örneğin, "ID" adlı bölüm sütununuzu 1 ile 100 arasında değerler varsa ve alt sınırı 20 ve üst sınır olarak 80 olarak ayarlarsanız, paralel kopyalama Data Factory 4 ' e kadar, verileri 4 bölüm kimliği aralığında <= 20, [21, 50], [51, 80] ve >= 81 sırasıyla alır. |

Bölüm seçeneğiyle verileri yüklemek için en iyi uygulamalar:

1. Veri eğriliğini önlemek için bölüm sütunu olarak (birincil anahtar veya benzersiz anahtar gibi) farklı bir sütun seçin. 
2. Tabloda yerleşik bölüm varsa, daha iyi performans sağlamak için "tablonun fiziksel bölümleri" Bölüm seçeneğini kullanın.
3. Verileri kopyalamak için Azure Integration Runtime kullanırsanız daha fazla bilgi işlem kaynağı kullanmak için daha büyük "[veri tümleştirme birimleri (DIU)](copy-activity-performance-features.md#data-integration-units)" (>4) ayarlayabilirsiniz. İlgili senaryolara göz atın.
4. "[Kopya paralelliği derecesi](copy-activity-performance-features.md#parallel-copy)", Bölüm numaralarını denetler, bu sayının performansı karşılamamasından çok büyük bir şekilde ayarlanması, bu sayıyı (ya da şirket IÇINDE barındırılan IR düğümlerinin sayısı) * (2-4) olarak ayarlamayı öneririz.
5. Azure SYNAPSE Analytics bir anda en fazla 32 sorgu yürütebilir, "kopya paralellik derecesi" ayarı çok büyük olduğunda SYNAPSE azaltma sorununa neden olabilir.

**Örnek: fiziksel bölümlerle büyük tablodan tam yük**

```json
"source": {
    "type": "SqlDWSource",
    "partitionOption": "PhysicalPartitionsOfTable"
}
```

**Örnek: Dinamik Aralık bölümü ile sorgulama**

```json
"source": {
    "type": "SqlDWSource",
    "query": "SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column (optional) to decide the partition stride, not as data filter>",
        "partitionLowerBound": "<lower_value_of_partition_column (optional) to decide the partition stride, not as data filter>"
    }
}
```

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Azure SQL veri ambarı 'na veri yüklemek için PolyBase kullanma

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) 'in kullanılması, yüksek aktarım hızı Ile Azure SYNAPSE Analytics 'e büyük miktarda veri yüklemek için etkili bir yoldur. Varsayılan BULKıNSERT mekanizması yerine PolyBase kullanarak üretilen iş için büyük bir kazanç görürsünüz. Kullanım örneği ile ilgili bir anlatım için bkz. [1 TB 'Yi Azure SYNAPSE Analytics 'e yükleme](v1/data-factory-load-sql-data-warehouse.md).

- Kaynak verileriniz Azure Blob 'dayken **, Azure Data Lake Storage 1. veya Azure Data Lake Storage 2.** ve **Biçim PolyBase UYUMLUYSA**, Azure SQL veri ambarı 'nın verileri kaynaktan çekmesini sağlamak üzere doğrudan PolyBase 'i çağırmak için kopyalama etkinliğini kullanabilirsiniz. Ayrıntılar için bkz. **[PolyBase kullanarak doğrudan kopyalama](#direct-copy-by-using-polybase)**.
- Kaynak veri depoluü ve biçimlendirmeniz ilk olarak PolyBase tarafından desteklenmiyorsa, bunun yerine **[PolyBase özelliğini kullanarak hazırlanan kopyayı](#staged-copy-by-using-polybase)** kullanın. Hazırlanan kopya özelliği de size daha iyi aktarım hızı sağlar. Verileri otomatik olarak PolyBase uyumlu biçime dönüştürür, verileri Azure Blob depolama alanında depolar ve sonra SQL veri ambarı 'na veri yüklemek için PolyBase 'i çağırır.

> [!TIP]
> [PolyBase 'i kullanmaya yönelik en iyi uygulamalar](#best-practices-for-using-polybase)hakkında daha fazla bilgi edinin.

Kopyalama etkinliğinde aşağıdaki PolyBase ayarları desteklenir `polyBaseSettings` :

| Özellik          | Açıklama                                                  | Gerekli                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| rejectValue       | Sorgu başarısız olmadan önce reddedilecek satırların sayısını veya yüzdesini belirtir.<br/><br/>[Dış tablo oluşturma (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx)öğesinin bağımsız değişkenler bölümünde PolyBase 'in reddetme seçenekleri hakkında daha fazla bilgi edinin. <br/><br/>İzin verilen değerler 0 (varsayılan), 1, 2, vb. olabilir. | Hayır                                            |
| rejectType        | **RejectValue** seçeneğinin sabit değer veya yüzde değeri olup olmadığını belirtir.<br/><br/>İzin verilen değerler **değer** (varsayılan) ve **yüzde**değeridir. | Hayır                                            |
| rejectSampleValue | PolyBase reddedilen satırların yüzdesini yeniden hesaplamadan önce alınacak satır sayısını belirler.<br/><br/>İzin verilen değerler 1, 2, vb. olabilir. | Evet, **rejectType** **ise.** |
| useTypeDefault    | PolyBase metin dosyasından verileri aldığında, sınırlandırılmış metin dosyalarında eksik değerlerin nasıl işleneceğini belirtir.<br/><br/>[Dış dosya biçimi oluşturma (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx)Içindeki bağımsız değişkenler bölümünden bu özellik hakkında daha fazla bilgi edinin.<br/><br/>İzin verilen değerler **true** ve **false** (varsayılan) şeklindedir.<br><br> | Hayır                                            |

### <a name="direct-copy-by-using-polybase"></a>PolyBase kullanarak doğrudan kopyalama

SQL veri ambarı PolyBase doğrudan Azure blob, Azure Data Lake Storage 1. ve Azure Data Lake Storage 2. destekler. Kaynak verileriniz bu bölümde açıklanan ölçütleri karşılıyorsa, doğrudan kaynak veri deposundan Azure SYNAPSE Analytics 'e kopyalamak için PolyBase kullanın. Aksi takdirde, [PolyBase kullanarak hazırlanmış kopyayı](#staged-copy-by-using-polybase)kullanın.

> [!TIP]
> Verileri SQL veri ambarı 'na verimli bir şekilde kopyalamak için Azure Data Factory 'tan daha fazla bilgi edinin, [SQL veri ambarı ile Data Lake Store kullanırken verilerin öngörülerini daha kolay ve](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/)kolay bir şekilde açığa çıkarabilir.

Gereksinimler karşılanmazsa, Azure Data Factory ayarları denetler ve veri taşıma için otomatik olarak BULKıNSERT mekanizmasına geri döner.

1. **Kaynak bağlı hizmeti** aşağıdaki türler ve kimlik doğrulama yöntemleriyle aynıdır:

    | Desteklenen kaynak veri deposu türü                             | Desteklenen kaynak kimlik doğrulama türü                        |
    | :----------------------------------------------------------- | :---------------------------------------------------------- |
    | [Azure Blob](connector-azure-blob-storage.md)                | Hesap anahtarı kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |
    | [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | Hizmet sorumlusu kimlik doğrulaması                            |
    | [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md) | Hesap anahtarı kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |

    >[!IMPORTANT]
    >Azure depolama alanı sanal ağ hizmeti uç noktası ile yapılandırıldıysa, yönetilen kimlik kimlik doğrulaması kullanmanız gerekir- [VNET hizmet uç noktalarını Azure depolama ile kullanmanın etkileri](../azure-sql/database/vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)için başvurun. [Azure Blob tarafından yönetilen kimlik doğrulama](connector-azure-blob-storage.md#managed-identity) ve [Azure Data Lake Storage 2. tarafından yönetilen kimlik doğrulama](connector-azure-data-lake-storage.md#managed-identity) bölümündeki Data Factory gereken konfigürasyonları öğrenin.

2. **Kaynak veri biçimi** , aşağıdaki yapılandırmalara **sahip Parquet**, **orc**veya **sınırlandırılmış bir metindir**:

   1. Klasör yolu joker karakter filtresi içermiyor.
   2. Dosya adı boş veya tek bir dosyaya işaret ediyor. Kopyalama etkinliğinde joker karakter dosya adı belirtirseniz, yalnızca `*` veya olabilir `*.*` .
   3. `rowDelimiter`**varsayılan**, **\n**, **\r\n**veya **\r**'dir.
   4. `nullValue` Varsayılan olarak bırakılır veya **boş dize** ("") olarak ayarlanır ve `treatEmptyAsNull` Varsayılan olarak bırakılır ya da true olarak ayarlanır.
   5. `encodingName` Varsayılan olarak bırakılır veya **UTF-8**olarak ayarlanır.
   6. `quoteChar`, `escapeChar` ve `skipLineCount` belirtilmedi. PolyBase desteği, ADF 'de olduğu gibi yapılandırılabilen üst bilgi satırını atlar `firstRowAsHeader` .
   7. `compression`**sıkıştırma**, **gzip**veya **söndür**olamaz.

3. Kaynağınız bir klasörse, `recursive` Copy etkinliğinin true olarak ayarlanması gerekir.

4. `wildcardFolderPath` ,,,, `wildcardFilename` `modifiedDateTimeStart` `modifiedDateTimeEnd` `prefix` `enablePartitionDiscovery` ve `additionalColumns` belirtilmedi.

>[!NOTE]
>Kaynağınız bir klasörssa, PolyBase dosyaları klasörden ve tüm alt klasörlerinden alır ve dosya adının bir alt çizgiyle (_) veya nokta (.) ile başladığı dosyalardan veri almaz ( [burada, konum bağımsız değişkeni](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql?view=azure-sqldw-latest#arguments-2)olarak belgelenmiştir).

```json
"activities":[
    {
        "name": "CopyFromAzureBlobToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "ParquetDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "ParquetSource",
                "storeSettings":{
                    "type": "AzureBlobStorageReadSettings",
                    "recursive": true
                }
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            }
        }
    }
]
```

### <a name="staged-copy-by-using-polybase"></a>PolyBase kullanarak hazırlanmış kopya

Kaynak verileriniz PolyBase ile yerel olarak uyumlu değilse, geçici hazırlama Azure Blob depolama örneği aracılığıyla veri kopyalamayı etkinleştirin (Azure Premium Depolama olamaz). Bu durumda Azure Data Factory, verileri, PolyBase 'in veri biçimi gereksinimlerini karşılayacak şekilde otomatik olarak dönüştürür. Daha sonra SQL veri ambarı 'na veri yüklemek için PolyBase 'i çağırır. Son olarak, blob depolamadan geçici verilerinizi temizler. Hazırlama Azure Blob depolama örneği aracılığıyla veri kopyalama hakkındaki ayrıntılar için bkz. [aşamalı kopya](copy-activity-performance-features.md#staged-copy) .

Bu özelliği kullanmak için, ara BLOB depolama alanı ile Azure depolama hesabına başvuran bir [Azure Blob depolama bağlı hizmeti](connector-azure-blob-storage.md#linked-service-properties) oluşturun. Ardından, `enableStaging` `stagingSettings` aşağıdaki kodda gösterildiği gibi kopyalama etkinliğinin ve özelliklerini belirtin.

>[!IMPORTANT]
>Hazırlama Azure depolama alanı sanal ağ hizmeti uç noktası ile yapılandırıldıysa, yönetilen kimlik kimlik doğrulaması kullanmanız gerekir- [VNET hizmet uç noktalarını Azure depolama ile kullanmanın etkileri](../azure-sql/database/vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)için başvurun. [Azure Blob tarafından yönetilen kimlik kimlik doğrulamasından](connector-azure-blob-storage.md#managed-identity)Data Factory, gerekli konfigürasyonları öğrenin.

```json
"activities":[
    {
        "name": "CopyFromSQLServerToSQLDataWarehouseViaPolyBase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "SQLServerDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlDWSink",
                "allowPolyBase": true
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingBlob",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
]
```

### <a name="best-practices-for-using-polybase"></a>PolyBase 'i kullanmak için en iyi uygulamalar

Aşağıdaki bölümlerde, [Azure SYNAPSE Analytics Için en iyi yöntemler](../synapse-analytics/sql/best-practices-sql-pool.md)bölümünde bahsedilenlere ek olarak en iyi uygulamalar sağlanmaktadır.

#### <a name="required-database-permission"></a>Gerekli veritabanı izni

PolyBase 'i kullanmak için SQL veri ambarı 'na veri yükleyen kullanıcının hedef veritabanında ["Control" iznine](https://msdn.microsoft.com/library/ms191291.aspx) sahip olması gerekir. Bunu yapmanın bir yolu, kullanıcıyı **db_owner** rolünün bir üyesi olarak eklemektir. [SQL veri ambarı 'na genel bakış](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization)bölümünde bunu nasıl yapacağınızı öğrenin.

#### <a name="row-size-and-data-type-limits"></a>Satır boyutu ve veri türü sınırları

PolyBase yükleri 1 MB 'tan küçük satırlarla sınırlıdır. VARCHR (MAX), NVARCHAR (MAX) veya VARBINARY (MAX) ' e yüklemek için kullanılamaz. Daha fazla bilgi için bkz. [SQL veri ambarı hizmet kapasitesi sınırları](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Kaynak verilerinizde 1 MB 'den büyük satırlar varsa, kaynak tabloları birkaç küçük olanlara dikey olarak bölmek isteyebilirsiniz. Her satırın en büyük boyutunun sınırı aşmadığından emin olun. Daha sonra, PolyBase kullanarak ve Azure SYNAPSE Analytics 'te birlikte birleştirilmiş daha küçük tablolar yüklenebilir.

Alternatif olarak, bu tür geniş sütunlara sahip veriler için, "PolyBase 'e izin ver" ayarını kapatarak ADF kullanarak verileri yüklemek için PolyBase 'i kullanabilirsiniz.

#### <a name="sql-data-warehouse-resource-class"></a>SQL veri ambarı kaynak sınıfı

Olası en iyi verimi elde etmek için, PolyBase aracılığıyla SQL Data Warehouse 'a veri yükleyen kullanıcıya daha büyük bir kaynak sınıfı atayın.

#### <a name="polybase-troubleshooting"></a>PolyBase sorunlarını giderme

**Ondalık sütuna yükleniyor**

Kaynak verileriniz Metin biçimindeyse veya PolyBase ile uyumlu olmayan diğer depolarda (hazırlanan kopya ve PolyBase kullanılarak) ve SQL veri ambarı ondalık sütununa yüklenecek boş bir değer içeriyorsa, şu hatayla karşılaşabilirsiniz:

```
ErrorCode=FailedDbOperation, ......HadoopSqlException: Error converting data type VARCHAR to DECIMAL.....Detailed Message=Empty string can't be converted to DECIMAL.....
```

Çözüm, "**tür Varsayılanı kullan**" seçeneğinin (false olarak) kopyalama etkinliği havuzu-> PolyBase ayarları ' nda seçimini kaldırır. "[USE_TYPE_DEFAULT](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql?view=azure-sqldw-latest#arguments
)", PolyBase metin dosyasından verileri aldığında sınırlandırılmış metin dosyalarında eksik değerlerin nasıl Işleneceğini belirten PolyBase yerel bir yapılandırmadır.

**`tableName` Azure SYNAPSE Analytics 'te**

Aşağıdaki tabloda, JSON veri kümesindeki **TableName** özelliğinin nasıl belirtilme örnekleri verilmiştir. Şema ve tablo adlarının birkaç birleşimini gösterir.

| DB şeması | Tablo adı | **TableName** JSON özelliği               |
| --------- | ---------- | ----------------------------------------- |
| dbo       | MyTable    | MyTable veya dbo. MyTable veya [dbo]. MyTable |
| dbo1      | MyTable    | dbo1. MyTable veya [dbo1]. MyTable          |
| dbo       | My. Table   | [My. Table] veya [dbo]. [My. Table]            |
| dbo1      | My. Table   | [dbo1]. [My. Table]                         |

Aşağıdaki hatayı görürseniz, sorun **TableName** özelliği için belirttiğiniz değer olabilir. **TableName** JSON özelliğinin değerlerini belirtmenin doğru yolu için yukarıdaki tabloya bakın.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

**Varsayılan değerlere sahip sütunlar**

Şu anda, Data Factory PolyBase özelliği hedef tablodaki gibi yalnızca aynı sayıda sütunu kabul eder. Örnek olarak, biri varsayılan değerle tanımlanmış dört sütunlu bir tablodur. Giriş verilerinde hala dört sütun olması gerekir. Üç sütunlu bir giriş veri kümesi aşağıdaki iletiye benzer bir hata verir:

```
All columns of the table must be specified in the INSERT BULK statement.
```

NULL değeri, varsayılan değer olan özel bir formdur. Sütun null atanabilir ise, bu sütun için blobdaki giriş verileri boş olabilir. Ancak giriş veri kümesinde eksik olamaz. PolyBase, Azure SYNAPSE Analytics 'te eksik değerler için NULL ekliyor.

## <a name="use-copy-statement-to-load-data-into-azure-sql-data-warehouse-preview"></a><a name="use-copy-statement"></a> Azure SQL veri ambarı 'na veri yüklemek için kopyalama ifadesini kullanma (Önizleme)

SQL veri ambarı [kopyalama ekstresi](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (Önizleme) **, Azure Blob ve Azure Data Lake Storage 2.** verilerini doğrudan yüklemeyi destekler. Kaynak verileriniz bu bölümde açıklanan kriterleri karşılıyorsa, verileri Azure SQL veri ambarı 'na yüklemek için ADF 'de COPY ifadesini kullanmayı seçebilirsiniz. Azure Data Factory, ayarları denetler ve ölçütler karşılanmazsa kopyalama etkinliğinin çalıştırılmasına başarısız olur.

>[!NOTE]
>Şu anda Data Factory yalnızca aşağıda belirtilen COPY deyimiyle uyumlu kaynaklardan kopyalama desteklenir.

COPY ifadesinin kullanılması aşağıdaki yapılandırmayı destekler:

1. **Kaynak bağlı hizmeti ve biçimi** aşağıdaki türler ve kimlik doğrulama yöntemleriyle aynıdır:

    | Desteklenen kaynak veri deposu türü                             | Desteklenen biçim           | Desteklenen kaynak kimlik doğrulama türü                         |
    | :----------------------------------------------------------- | -------------------------- | :----------------------------------------------------------- |
    | [Azure Blob](connector-azure-blob-storage.md)                | [Sınırlandırılmış metin](format-delimited-text.md)             | Hesap anahtarı kimlik doğrulaması, paylaşılan erişim imzası kimlik doğrulaması, hizmet sorumlusu kimlik doğrulaması, yönetilen kimlik doğrulama |
    | &nbsp;                                                       | [Parquet](format-parquet.md)                    | Hesap anahtarı kimlik doğrulaması, paylaşılan erişim imzası kimlik doğrulaması |
    | &nbsp;                                                       | [ORC](format-orc.md)                        | Hesap anahtarı kimlik doğrulaması, paylaşılan erişim imzası kimlik doğrulaması |
    | [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md) | [Sınırlandırılmış metin](format-delimited-text.md)<br/>[Parquet](format-parquet.md)<br/>[ORC](format-orc.md) | Hesap anahtarı kimlik doğrulaması, hizmet sorumlusu kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |

    >[!IMPORTANT]
    >Azure depolama alanı sanal ağ hizmeti uç noktası ile yapılandırıldıysa, yönetilen kimlik kimlik doğrulaması kullanmanız gerekir- [VNET hizmet uç noktalarını Azure depolama ile kullanmanın etkileri](../azure-sql/database/vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)için başvurun. [Azure Blob tarafından yönetilen kimlik doğrulama](connector-azure-blob-storage.md#managed-identity) ve [Azure Data Lake Storage 2. tarafından yönetilen kimlik doğrulama](connector-azure-data-lake-storage.md#managed-identity) bölümündeki Data Factory gereken konfigürasyonları öğrenin.

2. Biçim ayarları şunlardır:

   1. **Parquet**: `compression` **Compression**, **Snappy**veya **gzip**olamaz.
   2. **Orc**için: `compression` **Compression**, **```zlib```** veya **Snappy**olabilir.
   3. **Sınırlandırılmış metin**için:
      1. `rowDelimiter` açık olarak **tek karakter** veya "**\r\n**" olarak ayarlanmıştır, varsayılan değer desteklenmez.
      2. `nullValue` Varsayılan olarak bırakılır veya **boş dize** ("") olarak ayarlanır.
      3. `encodingName` Varsayılan olarak bırakılır veya **UTF-8 veya UTF-16**olarak ayarlanır.
      4. `escapeChar` ile aynı olmalı `quoteChar` ve boş olmamalıdır.
      5. `skipLineCount` Varsayılan olarak bırakılır veya 0 olarak ayarlanır.
      6. `compression`**sıkıştırma** veya **gzip**olamaz.

3. Kaynağınız bir klasörse, `recursive` kopyalama etkinliği ' nde true olarak ayarlanmalıdır ve `wildcardFilename` olması gerekir `*` . 

4. `wildcardFolderPath` , `wildcardFilename` (dışındaki `*` ),, `modifiedDateTimeStart` , `modifiedDateTimeEnd` `prefix` `enablePartitionDiscovery` ve `additionalColumns` belirtilmedi.

Kopyalama etkinliğinde aşağıdaki COPY deyimin ayarları desteklenir `allowCopyCommand` :

| Özellik          | Açıklama                                                  | Gerekli                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| defaultValues | SQL DW 'de her bir hedef sütun için varsayılan değerleri belirtir.  Özelliğindeki varsayılan değerler, veri ambarında ayarlanan varsayılan kısıtlamanın üzerine yazılır ve kimlik sütunu varsayılan değere sahip olamaz. | Hayır |
| additionalOptions | [Copy deyimindeki](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest)"with" yan TÜMCESINDE doğrudan SQL DW Copy ifadesine geçirilecek ek seçenekler. COPY deyimlerine göre hizalamak için değeri gereken şekilde tırnak içine edin. | Hayır |

```json
"activities":[
    {
        "name": "CopyFromAzureBlobToSQLDataWarehouseViaCOPY",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "ParquetDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "AzureSQLDWDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "ParquetSource",
                "storeSettings":{
                    "type": "AzureBlobStorageReadSettings",
                    "recursive": true
                }
            },
            "sink": {
                "type": "SqlDWSink",
                "allowCopyCommand": true,
                "copyCommandSettings": {
                    "defaultValues": [
                        {
                            "columnName": "col_string",
                            "defaultValue": "DefaultStringValue"
                        }
                    ],
                    "additionalOptions": {
                        "MAXERRORS": "10000",
                        "DATEFORMAT": "'ymd'"
                    }
                }
            },
            "enableSkipIncompatibleRow": true
        }
    }
]
```

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki verileri dönüştürürken Azure SYNAPSE Analytics 'teki tabloları okuyabilir ve yazabilirsiniz. Daha fazla bilgi için bkz. veri akışlarını eşleme içindeki [kaynak dönüştürme](data-flow-source.md) ve [Havuz dönüşümü](data-flow-sink.md) .

### <a name="source-transformation"></a>Kaynak dönüştürme

Azure SYNAPSE Analytics 'e özgü ayarlar, kaynak dönüşümünün **kaynak seçenekleri** sekmesinde bulunabilir.

**Giriş** Kaynağınızı bir tabloya (eşdeğerini) işaret edip etmeyeceğinizi seçin ```Select * from <table-name>``` ya da özel BIR SQL sorgusu girin.

**Hazırlamayı etkinleştir** Bu seçeneği, SYNAPSE DW kaynaklarıyla üretim iş yükleri içinde kullanmanız önemle önerilir. Bir işlem hattından Synapase kaynaklarıyla bir veri akışı etkinliği yürüttüğünüzde, ADF sizden bir hazırlama konumu depolama hesabı ister ve bunu hazırlanan veri yüklemesi için kullanır. SYNAPSE DW 'den veri yüklemek için en hızlı mekanizmadır.

**Sorgu**: giriş alanında sorgu ' yı seçerseniz, kaynağınız IÇIN bir SQL sorgusu girin. Bu ayar, veri kümesinde seçtiğiniz tüm tabloları geçersiz kılar. **Order by** yan tümceleri burada desteklenmez, ancak BIR tam select from ifadesini ayarlayabilirsiniz. Kullanıcı tanımlı tablo işlevleri de kullanabilirsiniz. **select * from udfGetData ()** , bir tablo döndüren SQL 'de bir UDF 'dir. Bu sorgu, veri akışınızda kullanabileceğiniz bir kaynak tablosu oluşturur. Sorguların kullanılması, test veya aramalar için satırları azaltmanın harika bir yoludur.

SQL örneği: ```Select * from MyTable where customerId > 1000 and customerId < 2000```

**Toplu iş boyutu**: büyük verileri okuma işleminde öbek için bir toplu iş boyutu girin. Veri akışlarında, ADF bu ayarı Spark sütunlu önbelleğe alma ayarlamak için kullanır. Bu bir seçenek alanıdır ve boş bırakılırsa Spark varsayılanlarını kullanır.

**Yalıtım düzeyi**: eşleme VERI akışındaki SQL kaynakları için varsayılan değer read UNCOMMITTED ' dır. Yalıtım düzeyini buradaki değerlerden birine değiştirebilirsiniz:

- Okuma Işlendi
- Kaydedilmeyen oku
- Yinelenebilir okuma
- Seri hale getirilebilir *-yok (yalıtım düzeyini yoksay)

![Yalıtım düzeyi](media/data-flow/isolationlevel.png "Yalıtım düzeyi")

### <a name="sink-transformation"></a>Havuz dönüştürme

Azure SYNAPSE Analytics 'e özgü ayarlar, havuz dönüşümünün **Ayarlar** sekmesinde bulunur.

**Güncelleştirme yöntemi:** Veritabanı Hedefinizdeki hangi işlemlere izin verileceğini belirler. Varsayılan değer yalnızca eklemeleri izin verir. Satırları güncelleştirmek, kaldırmak veya silmek için, bu eylemler için satırları etiketlemek üzere alter-Row dönüşümü gereklidir. Güncelleştirmeler, yukarı ve silme için bir anahtar sütunu veya sütunları ayarlanacak satırı belirleyecek şekilde ayarlanmalıdır.

**Tablo eylemi:** Yazmadan önce hedef tablodaki tüm satırların yeniden oluşturulup kaldırılacağını belirler.

- Hiçbiri: tabloya hiçbir eylem yapılmaz.
- Yeniden oluştur: tablo bırakılır ve yeniden oluşturulur. Dinamik olarak yeni bir tablo oluşturuluyoruz gereklidir.
- Kes: hedef tablodaki tüm satırlar kaldırılacak.

**Hazırlamayı etkinleştir:** Azure SYNAPSE Analytics 'e yazarken [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-ver15) 'i kullanıp kullanmayacağınızı belirler

**Toplu iş boyutu**: her bir sepete kaç satır yazıldığını denetler. Daha büyük toplu işlem boyutları sıkıştırma ve bellek iyileştirmeyi iyileştirir, ancak verileri önbelleğe alırken bellek dışında özel durumlar riskini ortadan kaldıracak.

**SQL betiklerini ön ve sonrası**: (ön işleme) ve sonra (işlem sonrası) verileri havuz veritabanınıza yazıldıktan sonra yürütülecek çok satırlı SQL betikleri girin

![SQL işleme betikleri ön ve sonrası](media/data-flow/prepost1.png "SQL işleme betikleri")

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin

## <a name="data-type-mapping-for-azure-synapse-analytics"></a>Azure SYNAPSE Analytics için veri türü eşlemesi

Veya Azure SYNAPSE Analytics 'ten veri kopyaladığınızda, Azure SYNAPSE Analytics veri türleri arasında, geçici veri türlerini Azure Data Factory için aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) .

>[!TIP]
>SQL DW tarafından desteklenen veri türleri ve desteklenmeyen çözümler için [Azure SYNAPSE Analytics makalesindeki tablo veri türleri](../synapse-analytics/sql/develop-tables-data-types.md) bölümüne bakın.

| Azure SYNAPSE Analytics veri türü    | Data Factory geçici veri türü |
| :------------------------------------ | :----------------------------- |
| bigint                                | Int64                          |
| ikili                                | Byte []                         |
| bit                                   | Boole                        |
| char                                  | Dize, Char []                 |
| date                                  | DateTime                       |
| Tarih saat                              | DateTime                       |
| datetime2                             | DateTime                       |
| Türünde                        | DateTimeOffset                 |
| Ondalık                               | Ondalık                        |
| FıLESTREAM özniteliği (varbinary (max)) | Byte []                         |
| Float                                 | Çift                         |
| image                                 | Byte []                         |
| int                                   | Int32                          |
| etmenize                                 | Ondalık                        |
| nchar                                 | Dize, Char []                 |
| sayısal                               | Ondalık                        |
| nvarchar                              | Dize, Char []                 |
| real                                  | Tek                         |
| rowversion                            | Byte []                         |
| girişin                         | DateTime                       |
| smallint                              | Int16                          |
| küçük para                            | Ondalık                        |
| time                                  | TimeSpan                       |
| tinyint                               | Bayt                           |
| uniqueidentifier                      | Guid                           |
| ikili                             | Byte []                         |
| varchar                               | Dize, Char []                 |

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Factory ' de kopyalama etkinliğine göre kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).
