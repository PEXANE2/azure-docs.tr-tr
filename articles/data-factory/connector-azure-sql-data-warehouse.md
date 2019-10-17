---
title: Azure Data Factory kullanarak Azure SQL veri ambarı 'na veri kopyalama Microsoft Docs
description: Desteklenen kaynak depolardan Azure SQL veri ambarı 'na veya SQL veri ambarı 'ndan Data Factory kullanarak desteklenen havuz depolarına veri kopyalamayı öğrenin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: jingwang
ms.openlocfilehash: 5351f7f01bbe99b1e3ebc3c94a0805f0419cc1cf
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72387908"
---
# <a name="copy-data-to-or-from-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SQL veri ambarı 'na veri kopyalama 
> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Data Factory hizmeti sürümünü seçin:"]
> * [Version1](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Geçerli sürüm](connector-azure-sql-data-warehouse.md)

Bu makalede, verilerin Azure SQL veri ambarı 'na veya Azure 'a nasıl kopyalanacağı özetlenmektedir. Azure Data Factory hakkında bilgi edinmek için [tanıtım makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure SQL veri ambarı Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Etkinliği](copy-activity-overview.md) [Desteklenen kaynak/havuz matris](copy-activity-overview.md) tablosuyla Kopyala
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)

Özellikle, bu Azure SQL veri ambarı Bağlayıcısı şu işlevleri destekler:

- Azure kaynakları için hizmet sorumlusu veya yönetilen kimlikler ile SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulamasını kullanarak verileri kopyalayın.
- Kaynak olarak, bir SQL sorgusu veya saklı yordam kullanarak verileri alın.
- Havuz olarak, PolyBase veya toplu ekleme kullanarak veri yükleyin. Daha iyi kopyalama performansı için PolyBase önerilir.

> [!IMPORTANT]
> Azure Data Factory Integration Runtime kullanarak verileri kopyalarsanız Azure hizmetlerinin sunucuya erişebilmesi için bir [Azure SQL Server güvenlik duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) yapılandırın.
> Şirket içinde barındırılan tümleştirme çalışma zamanı kullanarak verileri kopyalarsanız, Azure SQL Server güvenlik duvarını uygun IP aralığına izin verecek şekilde yapılandırın. Bu Aralık, Azure SQL veritabanına bağlanmak için kullanılan makinenin IP 'sini içerir.

## <a name="get-started"></a>Kullanmaya Başlayın

> [!TIP]
> En iyi performansı elde etmek için PolyBase 'i kullanarak Azure SQL veri ambarı 'na veri yükleyin. [Verileri Azure SQL veri ambarı 'na yüklemek Için PolyBase kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse) bölümünde ayrıntılar bulunur. Kullanım örneği ile ilgili bir anlatım için, [Azure Data Factory ile 15 dakika altında 1 TB 'Yi Azure SQL veri ambarı 'Na yükleme](load-azure-sql-data-warehouse.md)bölümüne bakın.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, bir Azure SQL veri ambarı bağlayıcısına özgü Data Factory varlıkları tanımlayan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmet özellikleri

Aşağıdaki özellikler bir Azure SQL veri ambarı bağlı hizmeti için desteklenir:

| Özellik            | Açıklama                                                  | Gereklidir                                                     |
| :------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| type                | Type özelliği **Azuresqldw**olarak ayarlanmalıdır.             | Yes                                                          |
| Dizisi    | **ConnectionString** özelliği IÇIN Azure SQL veri ambarı örneğine bağlanmak için gereken bilgileri belirtin. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca, parola/hizmet sorumlusu anahtarını Azure Key Vault de koyabilirsiniz ve SQL kimlik doğrulaması, bağlantı dizesinden `password` yapılandırmasını çeker. Daha ayrıntılı bilgi için tablonun altındaki JSON örneğine bakın ve [kimlik bilgilerini Azure Key Vault makalesine saklayın](store-credentials-in-key-vault.md) . | Yes                                                          |
| Serviceprincipalıd  | Uygulamanın istemci KIMLIĞINI belirtin.                         | Evet, Azure AD kimlik doğrulamasını bir hizmet sorumlusu ile kullandığınızda. |
| Servicesprincipalkey | Uygulamanın anahtarını belirtin. Data Factory güvenli bir şekilde depolamak için bu alanı SecureString olarak işaretleyin veya [Azure Key Vault depolanan bir gizli dizi başvurusu](store-credentials-in-key-vault.md)yapın. | Evet, Azure AD kimlik doğrulamasını bir hizmet sorumlusu ile kullandığınızda. |
| Kiracı              | Uygulamanızın altında bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı KIMLIĞI) belirtin. Fareyi, Azure portal sağ üst köşesine getirerek alabilirsiniz. | Evet, Azure AD kimlik doğrulamasını bir hizmet sorumlusu ile kullandığınızda. |
| connectVia          | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma zamanı](concepts-integration-runtime.md) . Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanını (veri depolubir özel ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Integration Runtime kullanır. | Hayır                                                           |

Farklı kimlik doğrulama türleri için sırasıyla Önkoşullar ve JSON örnekleri hakkında aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- Azure AD uygulama belirteci kimlik doğrulaması: [hizmet sorumlusu](#service-principal-authentication)
- Azure AD uygulama belirteci kimlik doğrulaması: [Azure kaynakları Için Yönetilen kimlikler](#managed-identity)

>[!TIP]
>"UserErrorFailedToConnectToSqlServer" olarak hata kodu ile hata ve "veritabanı için oturum sınırı XXX ve ulaşıldığında" iletisiyle karşılaşırsanız, Bağlantı dizenizi `Pooling=false` ekleyip yeniden deneyin.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulaması kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
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
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            },
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

1. Azure portal **[bir Azure Active Directory uygulaması oluşturun](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)** . Uygulama adını ve bağlı hizmeti tanımlayan aşağıdaki değerleri unutmayın:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Daha önce yapmadıysanız, Azure portal Azure SQL Server için **[Azure Active Directory Yöneticisi sağlayın](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** . Azure AD yöneticisi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Gruba yönetilen kimliğe sahip bir yönetici rolü verirseniz, 3 ve 4. adımları atlayın. Yöneticinin veritabanına tam erişimi olur.

3. Hizmet sorumlusu için **[Kapsanan Veritabanı kullanıcıları oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** . SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz veri ambarına ya da en az BIR kullanıcı izni olan bir Azure AD kimliğiyle bağlanın. Aşağıdaki T-SQL ' i çalıştırın:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. SQL kullanıcıları veya diğerleri için normalde yaptığınız sürece **Hizmet sorumlusuna gerekli Izinleri verin** . Aşağıdaki kodu çalıştırın veya daha [fazla seçeneğe bakın](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Verileri yüklemek için PolyBase kullanmak istiyorsanız [gerekli veritabanı iznini](#required-database-permission)öğrenin.

    ```sql
    EXEC sp_addrolemember db_owner, [your application name];
    ```

5. Azure Data Factory **Azure SQL veri ambarı bağlı hizmetini yapılandırın** .


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması kullanan bağlı hizmet örneği

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
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

### <a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için Yönetilen kimlikler

Bir veri fabrikası, belirli bir fabrikası temsil eden [Azure kaynakları için yönetilen bir kimlikle](data-factory-service-identity.md) ilişkilendirilebilir. Bu yönetilen kimliği, Azure SQL veri ambarı kimlik doğrulaması için kullanabilirsiniz. Belirlenen fabrika, bu kimliği kullanarak veri ambarınıza veya verilere erişebilir ve bu verileri kopyalayabilir.

Yönetilen kimlik kimlik doğrulamasını kullanmak için şu adımları izleyin:

1. Daha önce yapmadıysanız, Azure portal Azure SQL Server için **[Azure Active Directory Yöneticisi sağlayın](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** . Azure AD yöneticisi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Gruba yönetilen kimliğe sahip bir yönetici rolü verirseniz, 3 ve 4. adımları atlayın. Yöneticinin veritabanına tam erişimi olur.

2. Data Factory yönetilen kimlik için **[Kapsanan Veritabanı kullanıcıları oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** . SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz veri ambarına ya da en az BIR kullanıcı izni olan bir Azure AD kimliğiyle bağlanın. Aşağıdaki T-SQL ' i çalıştırın. 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. SQL kullanıcıları ve diğerleri için normalde yaptığınız gibi **Data Factory yönetilen kimliğe gerekli Izinleri verin** . Aşağıdaki kodu çalıştırın veya daha [fazla seçeneğe bakın](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Verileri yüklemek için PolyBase kullanmak istiyorsanız [gerekli veritabanı iznini](#required-database-permission)öğrenin.

    ```sql
    EXEC sp_addrolemember db_owner, [your Data Factory name];
    ```

5. Azure Data Factory **Azure SQL veri ambarı bağlı hizmetini yapılandırın** .

**Örnek:**

```json
{
    "name": "AzureSqlDWLinkedService",
    "properties": {
        "type": "AzureSqlDW",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
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

Veri kümelerini tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. [veri kümeleri](concepts-datasets-linked-services.md) makalesi. Bu bölüm, Azure SQL veri ambarı veri kümesi tarafından desteklenen özelliklerin bir listesini sağlar.

Veya Azure SQL veri ambarı 'ndan veri kopyalamak için aşağıdaki özellikler desteklenir:

| Özellik  | Açıklama                                                  | Gereklidir                    |
| :-------- | :----------------------------------------------------------- | :-------------------------- |
| type      | Veri kümesinin **Type** özelliği **Azuresqldwtable**olarak ayarlanmalıdır. | Yes                         |
| manızı | Şemanın adı. |Kaynak için Hayır, havuz için Evet  |
| Tablosundan | Tablo/görünüm adı. |Kaynak için Hayır, havuz için Evet  |
| tableName | Şema ile tablonun/görünümün adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü için `schema` ve `table` ' i kullanın. | Kaynak için Hayır, havuz için Evet |

#### <a name="dataset-properties-example"></a>DataSet özellikleri örneği

```json
{
    "name": "AzureSQLDWDataset",
    "properties":
    {
        "type": "AzureSqlDWTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Data Warehouse linked service name>",
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

Etkinlikleri tanımlamaya yönelik bölümlerin ve özelliklerin tam listesi için bkz. işlem [hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Azure SQL veri ambarı kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-sql-data-warehouse-as-the-source"></a>Kaynak olarak Azure SQL veri ambarı

Azure SQL veri ambarı 'ndan veri kopyalamak için kopyalama etkinliği kaynağındaki **Type** özelliğini **sqldwsource**olarak ayarlayın. Aşağıdaki özellikler, etkinlik **kaynağını** kopyalama bölümünde desteklenir:

| Özellik                     | Açıklama                                                  | Gereklidir |
| :--------------------------- | :----------------------------------------------------------- | :------- |
| type                         | Kopyalama etkinliği kaynağının **Type** özelliği **sqldwsource**olarak ayarlanmalıdır. | Yes      |
| sqlReaderQuery               | Verileri okumak için özel SQL sorgusunu kullanın. Örnek: `select * from MyTable`. | Hayır       |
| sqlReaderStoredProcedureName | Kaynak tablodaki verileri okuyan saklı yordamın adı. Son SQL ifadesinin saklı yordamda bir SELECT ifadesinin olması gerekir. | Hayır       |
| storedProcedureParameters    | Saklı yordamın parametreleri.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve büyük harfleri, saklı yordam parametrelerinin adlarıyla ve büyük küçük harfleriyle aynı olmalıdır. | Hayır       |

### <a name="points-to-note"></a>Nota işaret eder

- SQLSource için **Sqlreaderquery** belirtilmişse, kopyalamaetkinliği verileri almak IÇIN bu sorguyu Azure SQL veri ambarı kaynağında çalıştırır. Ya da bir saklı yordam belirtebilirsiniz. Saklı yordam parametreleri alırsa **sqlReaderStoredProcedureName** ve **storedProcedureParameters** belirtin.
- **Sqlreaderquery** veya **SQLREADERSTOREDPROCEDURENAME**belirtmezseniz, JSON veri kümesinin **Yapı** bölümünde tanımlanan sütunlar bir sorgu oluşturmak için kullanılır. `select column1, column2 from mytable` Azure SQL veri ambarı 'nda çalışır. Veri kümesi tanımının **yapısı**yoksa, tablodan tüm sütunlar seçilir.

#### <a name="sql-query-example"></a>SQL sorgu örneği

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

#### <a name="stored-procedure-example"></a>Saklı yordam örneği

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

### <a name="azure-sql-data-warehouse-as-sink"></a>Havuz olarak Azure SQL veri ambarı

Azure SQL veri ambarı 'na veri kopyalamak için kopyalama etkinliğindeki havuz türünü **Sqldwsink**olarak ayarlayın. Aşağıdaki özellikler, kopyalama etkinliği **havuzu** bölümünde desteklenir:

| Özellik          | Açıklama                                                  | Gereklidir                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| type              | Kopyalama etkinliği havuzunun **Type** özelliği **sqldwsink**olarak ayarlanmalıdır. | Yes                                           |
| allowPolyBase     | , Varsa, BULKıNSERT mekanizması yerine PolyBase 'in kullanılıp kullanılmayacağını gösterir. <br/><br/> PolyBase kullanarak SQL veri ambarı 'na veri yüklemeniz önerilir. Kısıtlamalar ve Ayrıntılar için [Azure SQL veri ambarı 'na veri yüklemek Için PolyBase kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse) bölümüne bakın.<br/><br/>İzin verilen değerler **true** ve **false** (varsayılan) şeklindedir. | Hayır                                            |
| polyBaseSettings  | **Allowpolybase** özelliği **true**olarak ayarlandığında belirtilenebilir bir özellik grubu. | Hayır                                            |
| rejectValue       | Sorgu başarısız olmadan önce reddedilecek satırların sayısını veya yüzdesini belirtir.<br/><br/>[Dış tablo oluşturma (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx)öğesinin bağımsız değişkenler bölümünde PolyBase 'in reddetme seçenekleri hakkında daha fazla bilgi edinin. <br/><br/>İzin verilen değerler 0 (varsayılan), 1, 2, vb. olabilir. | Hayır                                            |
| rejectType        | **RejectValue** seçeneğinin sabit değer veya yüzde değeri olup olmadığını belirtir.<br/><br/>İzin verilen değerler **değer** (varsayılan) ve **yüzde**değeridir. | Hayır                                            |
| rejectSampleValue | PolyBase reddedilen satırların yüzdesini yeniden hesaplamadan önce alınacak satır sayısını belirler.<br/><br/>İzin verilen değerler 1, 2, vb. olabilir. | Evet, **rejectType** **ise.** |
| useTypeDefault    | PolyBase metin dosyasından verileri aldığında, sınırlandırılmış metin dosyalarında eksik değerlerin nasıl işleneceğini belirtir.<br/><br/>[Dış dosya biçimi oluşturma (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx)Içindeki bağımsız değişkenler bölümünden bu özellik hakkında daha fazla bilgi edinin.<br/><br/>İzin verilen değerler **true** ve **false** (varsayılan) şeklindedir.<br><br> | Hayır                                            |
| writeBatchSize    | **Toplu iş BAŞıNA**SQL tablosuna eklenecek satır sayısı. Yalnızca PolyBase kullanılmazsa geçerlidir.<br/><br/>İzin verilen değer **Integer** (satır sayısı). Varsayılan olarak, Data Factory satır boyutuna göre uygun toplu iş boyutunu dinamik olarak belirler. | Hayır                                            |
| writeBatchTimeout | Toplu ekleme işleminin, zaman aşımına uğramadan önce tamamlaması için bekleme süresi. Yalnızca PolyBase kullanılmazsa geçerlidir.<br/><br/>İzin verilen değer **TimeSpan**değeridir. Örnek: "00:30:00" (30 dakika). | Hayır                                            |
| Ön Copyscrıpt     | Her çalıştırmada Azure SQL veri ambarı 'na veri yazmadan önce çalıştırılacak bir SQL sorgusu belirtin. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanın. | Hayır                                            |
| tableOption | Kaynak şemasına göre yoksa havuz tablosunun otomatik olarak oluşturulup oluşturulmayacağını belirtir. Hazırlanan kopya kopyalama etkinliğinde yapılandırıldığında otomatik tablo oluşturma desteklenmez. İzin verilen değerler: `none` (varsayılan), `autoCreate`. |Hayır |
| disableMetricsCollection | Data Factory, kopyalama performansı iyileştirmesi ve önerileri için SQL veri ambarı DWUs gibi ölçümleri toplar. Bu davranışla ilgileniyorlarsa, kapatmak için `true` belirtin. | Hayır (varsayılan `false`) |

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

Bir sonraki bölümde SQL veri ambarını etkin bir şekilde yüklemek için PolyBase kullanma hakkında daha fazla bilgi edinin.

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Azure SQL veri ambarı 'na veri yüklemek için PolyBase kullanma

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) 'in kullanılması, yüksek aktarım hızı Ile Azure SQL veri ambarı 'na büyük miktarda veri yüklemek için etkili bir yoldur. Varsayılan BULKıNSERT mekanizması yerine PolyBase kullanarak üretilen iş için büyük bir kazanç görürsünüz. Kullanım örneği ile ilgili bir anlatım için bkz. [1 TB 'Yi Azure SQL veri ambarı 'Na yükleme](v1/data-factory-load-sql-data-warehouse.md).

* Kaynak verileriniz Azure Blob 'dayken **, Azure Data Lake Storage 1. veya Azure Data Lake Storage 2.** ve **Biçim PolyBase UYUMLUYSA**, Azure SQL veri ambarı 'nın verileri kaynaktan çekmesini sağlamak üzere doğrudan PolyBase 'i çağırmak için kopyalama etkinliğini kullanabilirsiniz. Ayrıntılar için bkz. **[PolyBase kullanarak doğrudan kopyalama](#direct-copy-by-using-polybase)** .
* Kaynak veri depoluü ve biçimlendirmeniz ilk olarak PolyBase tarafından desteklenmiyorsa, bunun yerine **[PolyBase özelliğini kullanarak hazırlanan kopyayı](#staged-copy-by-using-polybase)** kullanın. Hazırlanan kopya özelliği de size daha iyi aktarım hızı sağlar. Verileri otomatik olarak PolyBase uyumlu biçime dönüştürür. Verileri Azure Blob depolama alanında depolar. Daha sonra verileri SQL veri ambarı 'na yükler.

>[!TIP]
>[PolyBase 'i kullanmaya yönelik en iyi uygulamalar](#best-practices-for-using-polybase)hakkında daha fazla bilgi edinin.

### <a name="direct-copy-by-using-polybase"></a>PolyBase kullanarak doğrudan kopyalama

SQL veri ambarı PolyBase doğrudan Azure blob, Azure Data Lake Storage 1. ve Azure Data Lake Storage 2. destekler. Kaynak verileriniz bu bölümde açıklanan ölçütleri karşılıyorsa, doğrudan kaynak veri deposundan Azure SQL veri ambarı 'na kopyalamak için PolyBase kullanın. Aksi takdirde, [PolyBase kullanarak hazırlanmış kopyayı](#staged-copy-by-using-polybase)kullanın.

> [!TIP]
> Verileri SQL veri ambarı 'na verimli bir şekilde kopyalamak için Azure Data Factory 'tan daha fazla bilgi edinin, [SQL veri ambarı ile Data Lake Store kullanırken verilerin öngörülerini daha kolay ve](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/)kolay bir şekilde açığa çıkarabilir.

Gereksinimler karşılanmazsa, Azure Data Factory ayarları denetler ve veri taşıma için otomatik olarak BULKıNSERT mekanizmasına geri döner.

1. **Kaynak bağlı hizmeti** aşağıdaki türler ve kimlik doğrulama yöntemleriyle aynıdır:

    | Desteklenen kaynak veri deposu türü                             | Desteklenen kaynak kimlik doğrulama türü                        |
    | :----------------------------------------------------------- | :---------------------------------------------------------- |
    | [Azure blobu](connector-azure-blob-storage.md)                | Hesap anahtarı kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |
    | [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | Hizmet sorumlusu kimlik doğrulaması                            |
    | [Azure Data Lake Storage 2.](connector-azure-data-lake-storage.md) | Hesap anahtarı kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |

    >[!IMPORTANT]
    >Azure depolama alanı sanal ağ hizmeti uç noktası ile yapılandırıldıysa, yönetilen kimlik kimlik doğrulaması kullanmanız gerekir- [VNET hizmet uç noktalarını Azure depolama ile kullanmanın etkileri](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)için başvurun. [Azure Blob tarafından yönetilen kimlik doğrulama](connector-azure-blob-storage.md#managed-identity) ve [Azure Data Lake Storage 2. tarafından yönetilen kimlik doğrulama](connector-azure-data-lake-storage.md#managed-identity) bölümündeki Data Factory gereken konfigürasyonları öğrenin.

2. **Kaynak veri biçimi** , aşağıdaki yapılandırmalara **sahip Parquet**, **orc**veya **sınırlandırılmış bir metindir**:

   1. Klasör yolu joker karakter filtresi içermiyor.
   2. Dosya adı boş veya tek bir dosyaya işaret ediyor. Kopyalama etkinliğinde joker karakter dosya adı belirtirseniz, yalnızca `*` veya `*.*` olabilir.
   3. `rowDelimiter` **varsayılan**, **\n**, **\r\n**veya **\r**'dir.
   4. `nullValue` varsayılan olarak bırakılır veya **boş dize** ("") olarak ayarlanır ve `treatEmptyAsNull` varsayılan olarak bırakılır veya true olarak ayarlanır.
   5. `encodingName` varsayılan olarak bırakılır veya **UTF-8**olarak ayarlanır.
   6. `quoteChar`, `escapeChar` ve `skipLineCount` belirtilmez. PolyBase desteği, ADF 'de `firstRowAsHeader` olarak yapılandırılabilen üst bilgi satırını atlar.
   7. `compression` **sıkıştırma**, **gzip**veya **söndür**olamaz.

3. Kaynağınız bir klasörse, Copy etkinliğinde `recursive` değeri true olarak ayarlanmalıdır.

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
                    "type": "AzureBlobStorageReadSetting",
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

Kaynak verileriniz önceki bölümdeki ölçütlere uymazsa, geçici hazırlama Azure Blob depolama örneği aracılığıyla veri kopyalamayı etkinleştirin. Azure Premium depolama alanı olamaz. Bu durumda Azure Data Factory, PolyBase 'in veri biçimi gereksinimlerini karşılamak için verileri otomatik olarak dönüşümlerde çalıştırır. Daha sonra SQL veri ambarı 'na veri yüklemek için PolyBase 'i kullanır. Son olarak, blob depolamadan geçici verilerinizi temizler. Hazırlama Azure Blob depolama örneği aracılığıyla veri kopyalama hakkındaki ayrıntılar için bkz. [aşamalı kopya](copy-activity-performance.md#staged-copy) .

Bu özelliği kullanmak için, ara BLOB depolama alanı ile Azure depolama hesabına başvuran bir [Azure Blob depolama bağlı hizmeti](connector-azure-blob-storage.md#linked-service-properties) oluşturun. Ardından, aşağıdaki kodda gösterildiği gibi kopyalama etkinliğinin `enableStaging` ve `stagingSettings` özelliklerini belirtin.

>[!IMPORTANT]
>Hazırlama Azure depolama alanı sanal ağ hizmeti uç noktası ile yapılandırıldıysa, yönetilen kimlik kimlik doğrulaması kullanmanız gerekir- [VNET hizmet uç noktalarını Azure depolama ile kullanmanın etkileri](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)için başvurun. [Azure Blob tarafından yönetilen kimlik kimlik doğrulamasından](connector-azure-blob-storage.md#managed-identity)Data Factory, gerekli konfigürasyonları öğrenin.

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

## <a name="best-practices-for-using-polybase"></a>PolyBase 'i kullanmak için en iyi uygulamalar

Aşağıdaki bölümlerde, [Azure SQL veri ambarı Için en iyi yöntemler](../sql-data-warehouse/sql-data-warehouse-best-practices.md)bölümünde bahsedilenlere ek olarak en iyi uygulamalar sağlanmaktadır.

### <a name="required-database-permission"></a>Gerekli veritabanı izni

PolyBase 'i kullanmak için SQL veri ambarı 'na veri yükleyen kullanıcının hedef veritabanında ["Control" iznine](https://msdn.microsoft.com/library/ms191291.aspx) sahip olması gerekir. Bunu yapmanın bir yolu, kullanıcıyı **db_owner** rolünün bir üyesi olarak eklemektir. [SQL veri ambarı 'na genel bakış](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization)bölümünde bunu nasıl yapacağınızı öğrenin.

### <a name="row-size-and-data-type-limits"></a>Satır boyutu ve veri türü sınırları

PolyBase yükleri 1 MB 'tan küçük satırlarla sınırlıdır. VARCHR (MAX), NVARCHAR (MAX) veya VARBINARY (MAX) ' e yüklemek için kullanılamaz. Daha fazla bilgi için bkz. [SQL veri ambarı hizmet kapasitesi sınırları](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Kaynak verilerinizde 1 MB 'den büyük satırlar varsa, kaynak tabloları birkaç küçük olanlara dikey olarak bölmek isteyebilirsiniz. Her satırın en büyük boyutunun sınırı aşmadığından emin olun. Daha sonra, PolyBase kullanılarak yüklenebilir ve Azure SQL veri ambarı 'nda birlikte birleştirilir.

Alternatif olarak, bu tür geniş sütunlara sahip veriler için, "PolyBase 'e izin ver" ayarını kapatarak ADF kullanarak verileri yüklemek için PolyBase 'i kullanabilirsiniz.

### <a name="sql-data-warehouse-resource-class"></a>SQL veri ambarı kaynak sınıfı

Olası en iyi verimi elde etmek için, PolyBase aracılığıyla SQL Data Warehouse 'a veri yükleyen kullanıcıya daha büyük bir kaynak sınıfı atayın.

### <a name="polybase-troubleshooting"></a>PolyBase sorunlarını giderme

**Ondalık sütuna yükleniyor**

Kaynak verileriniz Metin biçimindeyse veya PolyBase ile uyumlu olmayan diğer depolarda (hazırlanan kopya ve PolyBase kullanılarak) ve SQL veri ambarı ondalık sütununa yüklenecek boş bir değer içeriyorsa, şu hatayla karşılaşabilirsiniz:

```
ErrorCode=FailedDbOperation, ......HadoopSqlException: Error converting data type VARCHAR to DECIMAL.....Detailed Message=Empty string can't be converted to DECIMAL.....
```

Çözüm, "**tür Varsayılanı kullan**" seçeneğinin (false olarak) kopyalama etkinliği havuzu-> PolyBase ayarları ' nda seçimini kaldırır. "[USE_TYPE_DEFAULT](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql?view=azure-sqldw-latest#arguments
)", PolyBase metin dosyasından verileri aldığında sınırlandırılmış metin dosyalarında eksik değerlerin nasıl Işleneceğini belirten PolyBase yerel bir yapılandırmadır. 

**Azure SQL veri ambarı 'nda `tableName`**

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

NULL değeri, varsayılan değer olan özel bir formdur. Sütun null atanabilir ise, bu sütun için blobdaki giriş verileri boş olabilir. Ancak giriş veri kümesinde eksik olamaz. PolyBase, Azure SQL veri ambarı 'nda eksik değerler için NULL ekliyor.

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki [kaynak dönüşümden](data-flow-source.md) ve [Havuz dönüşümünde](data-flow-sink.md) ayrıntıları öğrenin.

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için veri türü eşlemesi

Verileri Azure SQL veri ambarı 'ndan veya sürümünden kopyaladığınızda, Azure SQL veri ambarı veri türleri ' nden, geçici veri türlerini Azure Data Factory için aşağıdaki eşlemeler kullanılır. Kopyalama etkinliğinin kaynak şemayı ve veri türünü havuza nasıl eşlediğini öğrenmek için bkz. [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) .

>[!TIP]
>SQL DW tarafından desteklenen veri türleri ve desteklenmeyen çözümler için [Azure SQL veri ambarı makalesindeki tablo veri türleri](../sql-data-warehouse/sql-data-warehouse-tables-data-types.md) bölümüne bakın.

| Azure SQL veri ambarı veri türü    | Data Factory geçici veri türü |
| :------------------------------------ | :----------------------------- |
| bigint                                | Int64                          |
| ý                                | Byte []                         |
| sürümleri                                   | Boole                        |
| Char                                  | Dize, Char []                 |
| date                                  | Tarih Saat                       |
| Hem                              | Tarih Saat                       |
| datetime2                             | Tarih Saat                       |
| Türünde                        | Türünde                 |
| Kategori                               | Kategori                        |
| FıLESTREAM özniteliği (varbinary (max)) | Byte []                         |
| Float                                 | Çift                         |
| image                                 | Byte []                         |
| int                                   | Int32                          |
| etmenize                                 | Kategori                        |
| nchar                                 | Dize, Char []                 |
| rakamlardan                               | Kategori                        |
| nvarchar                              | Dize, Char []                 |
| gerçek                                  | Tek                         |
| rowversion                            | Byte []                         |
| girişin                         | Tarih Saat                       |
| smallint                              | Int16                          |
| küçük para                            | Kategori                        |
| time                                  | TimeSpan                       |
| tinyint                               | Bayt                           |
| benzersiz tanımlayıcı                      | Guid                           |
| ikili                             | Byte []                         |
| varchar                               | Dize, Char []                 |

## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory ' de kopyalama etkinliğine göre kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimleri](copy-activity-overview.md##supported-data-stores-and-formats).
