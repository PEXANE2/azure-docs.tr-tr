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
ms.date: 12/12/2019
ms.openlocfilehash: f009b438cb0dc227289d65604d89c11fd382b675
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75892972"
---
# <a name="copy-and-transform-data-in-azure-synapse-analytics-formerly-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Data Factory kullanarak Azure SYNAPSE Analytics 'te (eski adıyla Azure SQL veri ambarı) veri kopyalama ve dönüştürme 

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Data Factory hizmeti sürümünü seçin:"]
> * [Version1](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Geçerli sürüm](connector-azure-sql-data-warehouse.md)

Bu makalede, Azure SYNAPSE Analytics 'ten verileri kopyalamak için Azure Data Factory kopyalama etkinliğinin nasıl kullanılacağı özetlenmektedir ve Azure Data Lake Storage 2. verileri dönüştürmek için veri akışı kullanılır. Azure Data Factory hakkında bilgi edinmek için [giriş makalesi](introduction.md).

## <a name="supported-capabilities"></a>Desteklenen özellikler

Bu Azure SYNAPSE Analytics Bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Etkinliği](copy-activity-overview.md) [Desteklenen kaynak/havuz matris](copy-activity-overview.md) tablosuyla Kopyala
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [GetMetadata etkinliği](control-flow-get-metadata-activity.md)

Kopyalama etkinliği için bu Azure SYNAPSE Analytics Bağlayıcısı şu işlevleri destekler:

- SQL kimlik doğrulaması ve Azure Active Directory (Azure AD) uygulama belirteci kimlik doğrulamasını Azure kaynakları için bir hizmet sorumlusu veya yönetilen kimliklerle kullanarak verileri kopyalayın.
- Bir kaynak olarak bir SQL sorgusu veya saklı yordamı kullanarak veri alın.
- Havuz olarak, [PolyBase](#use-polybase-to-load-data-into-azure-sql-data-warehouse) veya [Copy deyiminizi](#use-copy-statement) (Önizleme) veya toplu ekleme 'yi kullanarak verileri yükleyin. Daha iyi kopyalama performansı için PolyBase veya COPY deyiminizi (Önizleme) öneririz.

> [!IMPORTANT]
> Azure Data Factory Integration Runtime'ı kullanarak verileri kopyalama, yapılandırma bir [Azure SQL sunucusu güvenlik duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) Azure hizmetlerinin sunucunun erişebilmesi için.
> Şirket içinde barındırılan Integration runtime'ı kullanarak verileri kopyalama, Azure SQL sunucusu güvenlik duvarı uygun IP aralığı izin verecek şekilde yapılandırın. Bu Aralık, Azure SYNAPSE Analytics 'e bağlanmak için kullanılan makinenin IP 'sini içerir.

## <a name="get-started"></a>Kullanmaya Başlayın

> [!TIP]
> En iyi performansı elde etmek için PolyBase 'i kullanarak Azure SYNAPSE Analytics 'e veri yükleyin. [Azure SYNAPSE Analytics 'e veri yüklemek Için PolyBase kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse) bölümünde ayrıntılar bulunur. Kullanım örneği ile ilgili bir anlatım için, [Azure Data Factory ile 15 dakika altında Azure SYNAPSE Analytics 'e 1 TB yükleme](load-azure-sql-data-warehouse.md)bölümüne bakın.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, bir Azure SYNAPSE Analytics bağlayıcısına özgü Data Factory varlıkları tanımlayan özellikler hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlı hizmeti özellikleri

Aşağıdaki özellikler bir Azure SYNAPSE Analytics bağlı hizmeti için desteklenir:

| Özellik            | Açıklama                                                  | Gereklidir                                                     |
| :------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| type                | Type özelliği ayarlanmalıdır **AzureSqlDW**.             | Evet                                                          |
| connectionString    | **ConnectionString** özelliği Için Azure SYNAPSE Analytics örneğine bağlanmak için gereken bilgileri belirtin. <br/>Bu alanı, Data Factory güvenli bir şekilde depolamak için SecureString olarak işaretleyin. Ayrıca, parola/hizmet sorumlusu anahtarını Azure Key Vault de koyabilirsiniz ve SQL kimlik doğrulaması, `password` yapılandırmasını bağlantı dizesinden dışarı çeker. Daha ayrıntılı bilgi için tablonun altındaki JSON örneğine bakın ve [kimlik bilgilerini Azure Key Vault makalesine saklayın](store-credentials-in-key-vault.md) . | Evet                                                          |
| servicePrincipalId  | Uygulamanın istemci kimliği belirtin.                         | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| servicePrincipalKey | Uygulama anahtarını belirtin. Data Factory'de güvenle depolamak için bir SecureString olarak bu alanı işaretleyin veya [Azure Key Vault'ta depolanan bir gizli dizi başvuru](store-credentials-in-key-vault.md). | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| tenant              | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet, bir hizmet sorumlusu ile Azure AD kimlik doğrulaması kullandığınızda. |
| connectVia          | [Integration runtime](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deponuz özel bir ağda yer alıyorsa) Azure Integration Runtime veya şirket içinde barındırılan tümleştirme çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır                                                           |

Farklı kimlik doğrulama türleri için sırasıyla önkoşulları ve JSON örnekleri aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- Azure AD uygulama belirteci kimlik doğrulamasını: [hizmet sorumlusu](#service-principal-authentication)
- Azure AD uygulama belirteci kimlik doğrulamasını: [kimliklerini Azure kaynakları için yönetilen](#managed-identity)

>[!TIP]
>Hata olarak "UserErrorFailedToConnectToSqlServer" hata koduyla isabet ve gibi ileti "veritabanı için oturum sınırı xxx ve üst sınırına ulaşıldı.", ekleme `Pooling=false` bağlantı dizesi ve yeniden deneyin.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulamasını kullanan bağlı hizmet örneği

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

Hizmet sorumlusu tabanlı Azure AD uygulama belirteci kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin:

1. **[Bir Azure Active Directory uygulaması oluşturma](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)**  Azure portalından. Uygulama adı ve bağlı hizmetini tanımlamak aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. **[Bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi, Azure AD kullanıcısı veya Azure AD grubu olabilir. Gruba yönetilen kimliğe sahip bir yönetici rolü verirseniz, 3 ve 4. adımları atlayın. Yönetici veritabanında tam erişiminiz olacaktır.

3. **[Bağımsız veritabanı kullanıcılarını oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  hizmet sorumlusu için. Veri ambarından ya da en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz herhangi bir kullanıcı ALTER izni. Aşağıdaki T-SQL çalıştırın:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Hizmet sorumlusuna gerekli izinleri vermek** SQL kullanıcıları veya diğerleri için normalde yaptığınız gibi. Aşağıdaki kodu çalıştırın veya daha [fazla seçeneğe bakın](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Verileri yüklemek için PolyBase kullanmak istiyorsanız [gerekli veritabanı iznini](#required-database-permission)öğrenin.

    ```sql
    EXEC sp_addrolemember db_owner, [your application name];
    ```

5. Azure Data Factory 'de **Azure SYNAPSE Analytics bağlı hizmetini yapılandırın** .


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulamasını kullanan bağlı hizmet örneği

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

### <a name="managed-identity"></a> Azure kaynaklarında kimlik doğrulaması için yönetilen kimlik

Veri Fabrikası ile ilişkilendirilmiş bir [yönetilen Azure kaynakları için kimliği](data-factory-service-identity.md) , belirli üretecini temsil eder. Bu yönetilen kimliği, Azure SYNAPSE Analytics kimlik doğrulaması için kullanabilirsiniz. Belirtilen Üreteç erişebilir ve bu kimlik kullanarak veri kopyalama ya da verilerinize ambarı.

Yönetilen kimlik kimlik doğrulamasını kullanmak için şu adımları izleyin:

1. **[Bir Azure Active Directory Yöneticisi sağlama](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)**  zaten yapmadıysanız Azure Portal'da Azure SQL sunucunuzun. Azure AD Yöneticisi, Azure AD kullanıcısı veya Azure AD grubu olabilir. Gruba yönetilen kimliğe sahip bir yönetici rolü verirseniz, 3 ve 4. adımları atlayın. Yönetici veritabanında tam erişiminiz olacaktır.

2. Data Factory yönetilen kimlik için **[Kapsanan Veritabanı kullanıcıları oluşturun](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** . Veri ambarından ya da en az bir Azure AD kimlik ile SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz herhangi bir kullanıcı ALTER izni. Aşağıdaki T-SQL çalıştırın. 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. SQL kullanıcıları ve diğerleri için normalde yaptığınız gibi **Data Factory yönetilen kimliğe gerekli Izinleri verin** . Aşağıdaki kodu çalıştırın veya daha [fazla seçeneğe bakın](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017). Verileri yüklemek için PolyBase kullanmak istiyorsanız [gerekli veritabanı iznini](#required-database-permission)öğrenin.

    ```sql
    EXEC sp_addrolemember db_owner, [your Data Factory name];
    ```

5. Azure Data Factory 'de **Azure SYNAPSE Analytics bağlı hizmetini yapılandırın** .

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

Bölümleri ve veri kümeleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [veri kümeleri](concepts-datasets-linked-services.md) makalesi. 

Aşağıdaki özellikler Azure SYNAPSE Analytics veri kümesi için desteklenir:

| Özellik  | Açıklama                                                  | Gereklidir                    |
| :-------- | :----------------------------------------------------------- | :-------------------------- |
| type      | **type** kümesinin özelliği ayarlanmalıdır **AzureSqlDWTable**. | Evet                         |
| schema | Şemanın adı. |Kaynak, havuz için Evet Hayır  |
| table | Tablo/görünüm adı. |Kaynak, havuz için Evet Hayır  |
| tableName | Şema ile tablonun/görünümün adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü için `schema` ve `table`kullanın. | Kaynak, havuz için Evet Hayır |

#### <a name="dataset-properties-example"></a>Veri kümesi özellikleri örneği

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

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için mevcut özelliklerin tam listesi için bkz: [işlem hatları](concepts-pipelines-activities.md) makalesi. Bu bölüm, Azure SYNAPSE Analytics kaynağı ve havuzu tarafından desteklenen özelliklerin bir listesini sağlar.

### <a name="azure-synapse-analytics-as-the-source"></a>Kaynak olarak Azure SYNAPSE Analytics

Azure SYNAPSE Analytics 'ten veri kopyalamak için kopyalama etkinliği kaynağındaki **Type** özelliğini **sqldwsource**olarak ayarlayın. Kopyalama etkinliği aşağıdaki özellikler desteklenir **kaynak** bölümü:

| Özellik                     | Açıklama                                                  | Gereklidir |
| :--------------------------- | :----------------------------------------------------------- | :------- |
| type                         | **type** kopyalama etkinliği kaynak özelliği ayarlanmalıdır **SqlDWSource**. | Evet      |
| sqlReaderQuery               | Verileri okumak için özel bir SQL sorgusu kullanın. Örnek: `select * from MyTable`. | Hayır       |
| sqlReaderStoredProcedureName | Kaynak tablo verilerini okuyan saklı yordamın adı. Son SQL deyim bir SELECT deyimi saklı yordam içinde olmalıdır. | Hayır       |
| storedProcedureParameters    | Saklı yordamın parametreleri.<br/>İzin verilen değerler, ad veya değer çiftleridir. Adları ve parametreleri büyük küçük harfleri, adları ve saklı yordam parametreleri büyük küçük harfleri eşleşmelidir. | Hayır       |

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

### <a name="azure-sql-data-warehouse-as-sink"></a>Havuz olarak Azure SYNAPSE Analytics

Azure Data Factory, verileri SQL veri ambarı 'na yüklemek için üç yolu destekler.

![SQL DW havuz kopyalama seçenekleri](./media/connector-azure-sql-data-warehouse/sql-dw-sink-copy-options.png)

- [PolyBase kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse) 
- [COPY ifadesini kullanma (Önizleme)](#use-copy-statement)
- Toplu ekleme kullan

Veri yükleme en hızlı ve en ölçeklenebilir yolu [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) veya [Copy deyimindedir](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (Önizleme).

Azure SQL veri ambarı'na veri kopyalamak için kopyalama etkinliği Havuz türü ayarlayın. **SqlDWSink**. Kopyalama etkinliği aşağıdaki özellikler desteklenir **havuz** bölümü:

| Özellik          | Açıklama                                                  | Gereklidir                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| type              | **type** kopyalama etkinliği havuz özelliği ayarlanmalıdır **SqlDWSink**. | Evet                                           |
| allowPolyBase     | SQL veri ambarı 'na veri yüklemek için PolyBase kullanılıp kullanılmayacağını belirtir. `allowCopyCommand` ve `allowPolyBase` ikisi de true olamaz. <br/><br/>Kısıtlamalar ve Ayrıntılar için bkz. [PolyBase 'ı kullanarak Azure SQL veri ambarı 'na veri yükleme](#use-polybase-to-load-data-into-azure-sql-data-warehouse) bölümüne bakın.<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan). | Hayır.<br/>PolyBase kullanırken uygulayın.     |
| polyBaseSettings  | `allowPolybase` özelliği **true**olarak ayarlandığında belirtilenebilir bir özellik grubu. | Hayır.<br/>PolyBase kullanırken uygulayın. |
| allowCopyCommand | SQL veri ambarı 'na veri yüklemek için [kopyalama ifadesinin](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (Önizleme) kullanılıp kullanılmayacağını gösterir. `allowCopyCommand` ve `allowPolyBase` ikisi de true olamaz. <br/><br/>Kısıtlamalar ve Ayrıntılar için bkz. [kopyalama Ifadesini kullanarak Azure SQL veri ambarı 'na veri yükleme](#use-copy-statement) bölümüne bakın.<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan). | Hayır.<br>KOPYALAMA kullanırken uygulayın. |
| copyCommandSettings | `allowCopyCommand` özelliği TRUE olarak ayarlandığında belirtilenebilir bir özellik grubu. | Hayır.<br/>KOPYALAMA kullanırken uygulayın. |
| writeBatchSize    | **Toplu iş BAŞıNA**SQL tablosuna eklenecek satır sayısı.<br/><br/>İzin verilen değer **tamsayı** (satır sayısı). Varsayılan olarak, Data Factory satır boyutuna göre uygun toplu iş boyutunu dinamik olarak belirler. | Hayır.<br/>Toplu ekleme kullanırken uygulayın.     |
| writeBatchTimeout | Toplu ekleme işlemi zaman aşımına uğramadan önce tamamlanması için bir süre bekleyin.<br/><br/>İzin verilen değer **timespan**. Örnek: "00: 30:00" (30 dakika). | Hayır.<br/>Toplu ekleme kullanırken uygulayın.        |
| preCopyScript     | Her bir çalıştırmada Azure SQL Data Warehouse'a veri yazılmadan önce çalıştırmak kopyalama etkinliği için bir SQL sorgusunu belirtin. Önceden yüklenmiş ve verileri temizlemek için bu özelliği kullanın. | Hayır                                            |
| tableOption | Kaynak şemasına göre yoksa havuz tablosunun otomatik olarak oluşturulup oluşturulmayacağını belirtir. Hazırlanan kopya kopyalama etkinliğinde yapılandırıldığında otomatik tablo oluşturma desteklenmez. İzin verilen değerler: `none` (varsayılan), `autoCreate`. |Hayır |
| disableMetricsCollection | Data Factory, kopyalama performansı iyileştirmesi ve önerileri için SQL veri ambarı DWUs gibi ölçümleri toplar. Bu davranışla ilgileniyorlarsa, kapatmak için `true` belirtin. | Hayır (varsayılan `false`) |

#### <a name="sql-data-warehouse-sink-example"></a>SQL veri ambarı havuzu örnek

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

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Azure SQL veri ambarı'na veri yüklemek için PolyBase kullanma

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) 'in kullanılması, yüksek aktarım hızı Ile Azure SYNAPSE Analytics 'e büyük miktarda veri yüklemek için etkili bir yoldur. Yerine varsayılan BULKINSERT mekanizmasını PolyBase kullanarak büyük bir kazanç aktarım hızının görürsünüz. Kullanım örneği ile ilgili bir anlatım için bkz. [1 TB 'Yi Azure SYNAPSE Analytics 'e yükleme](v1/data-factory-load-sql-data-warehouse.md).

* Kaynak verileriniz Azure Blob 'dayken **, Azure Data Lake Storage 1. veya Azure Data Lake Storage 2.** ve **Biçim PolyBase UYUMLUYSA**, Azure SQL veri ambarı 'nın verileri kaynaktan çekmesini sağlamak üzere doğrudan PolyBase 'i çağırmak için kopyalama etkinliğini kullanabilirsiniz. Ayrıntılar için bkz  **[kopyalama PolyBase kullanarak doğrudan](#direct-copy-by-using-polybase)** .
* Kaynak veri deposu ve biçim başlangıçta desteklenmez, PolyBase tarafından kullanın **[kopyalama PolyBase kullanarak hazırlanmış](#staged-copy-by-using-polybase)** bunun yerine özellik. Hazırlanmış kopya özelliğini de daha iyi aktarım hızı sağlar. Verileri otomatik olarak PolyBase uyumlu biçime dönüştürür, verileri Azure Blob depolama alanında depolar. daha sonra verileri SQL veri ambarı 'na yüklemek için PolyBase 'i çağırır.

>[!TIP]
>[PolyBase 'i kullanmaya yönelik en iyi uygulamalar](#best-practices-for-using-polybase)hakkında daha fazla bilgi edinin.

Aşağıdaki PolyBase ayarları kopyalama etkinliğinde `polyBaseSettings` altında desteklenir:

| Özellik          | Açıklama                                                  | Gereklidir                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| rejectValue       | Sayı veya sorgu başarısız olmadan önce reddedilemiyor satırları yüzdesini belirtir.<br/><br/>Bağımsız değişkenler bölümünde PolyBase'nın reddetme seçenekleri hakkında daha fazla bilgi [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx). <br/><br/>İzin verilen değerler: 0 (varsayılan), 1, 2, vs. | Hayır                                            |
| rejectType        | Belirtir olup olmadığını **rejectValue** seçenektir değişmez değer veya bir yüzdesi.<br/><br/>İzin verilen değerler **değer** (varsayılan) ve **yüzdesi**. | Hayır                                            |
| rejectSampleValue | Reddedilen satırların yüzdesi PolyBase yeniden hesaplar önce almak için satır sayısını belirler.<br/><br/>İzin verilen değerler: 1, 2, vs. | Evet, varsa **rejectType** olduğu **yüzdesi**. |
| useTypeDefault    | PolyBase metin dosyasından veri aldığında sınırlandırılmış metin dosyaları eksik değerleri nasıl ele alınacağını belirtir.<br/><br/>Bağımsız değişkenler bölümünden bu özellik hakkında daha fazla bilgi [oluşturma EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan).<br><br> | Hayır                                            |

### <a name="direct-copy-by-using-polybase"></a>PolyBase kullanarak doğrudan Kopyala

SQL veri ambarı PolyBase doğrudan Azure blob, Azure Data Lake Storage 1. ve Azure Data Lake Storage 2. destekler. Kaynak verileriniz bu bölümde açıklanan ölçütleri karşılıyorsa, doğrudan kaynak veri deposundan Azure SYNAPSE Analytics 'e kopyalamak için PolyBase kullanın. Aksi takdirde kullanın [kopyalama PolyBase kullanarak hazırlanmış](#staged-copy-by-using-polybase).

> [!TIP]
> Verileri SQL veri ambarı 'na verimli bir şekilde kopyalamak için Azure Data Factory 'tan daha fazla bilgi edinin, [SQL veri ambarı ile Data Lake Store kullanırken verilerin öngörülerini daha kolay ve](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/)kolay bir şekilde açığa çıkarabilir.

Gereksinimleri karşılanmadığı takdirde, Azure Data Factory ayarları denetler ve veri taşıma BULKINSERT mekanizması için otomatik olarak geri döner.

1. **Kaynak bağlı hizmeti** aşağıdaki türler ve kimlik doğrulama yöntemleriyle aynıdır:

    | Desteklenen kaynak veri deposu türü                             | Desteklenen kaynak kimlik doğrulama türü                        |
    | :----------------------------------------------------------- | :---------------------------------------------------------- |
    | [Azure Blob](connector-azure-blob-storage.md)                | Hesap anahtarı kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |
    | [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | Hizmet sorumlusu kimlik doğrulaması                            |
    | [Azure Data Lake Storage 2. Nesil](connector-azure-data-lake-storage.md) | Hesap anahtarı kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |

    >[!IMPORTANT]
    >Azure depolama alanı sanal ağ hizmeti uç noktası ile yapılandırıldıysa, yönetilen kimlik kimlik doğrulaması kullanmanız gerekir- [VNET hizmet uç noktalarını Azure depolama ile kullanmanın etkileri](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)için başvurun. [Azure Blob tarafından yönetilen kimlik doğrulama](connector-azure-blob-storage.md#managed-identity) ve [Azure Data Lake Storage 2. tarafından yönetilen kimlik doğrulama](connector-azure-data-lake-storage.md#managed-identity) bölümündeki Data Factory gereken konfigürasyonları öğrenin.

2. **Kaynak veri biçimi** , aşağıdaki yapılandırmalara **sahip Parquet**, **orc**veya **sınırlandırılmış bir metindir**:

   1. Klasör yolu joker karakter filtresi içermiyor.
   2. Dosya adı boş veya tek bir dosyaya işaret ediyor. Kopyalama etkinliğinde joker karakter dosya adı belirtirseniz, yalnızca `*` veya `*.*`olabilir.
   3. **varsayılan**, **\n**, **\r\n**veya **\r**`rowDelimiter`.
   4. `nullValue` varsayılan olarak bırakılır veya **boş dize** ("") olarak ayarlanır ve `treatEmptyAsNull` varsayılan olarak bırakılır veya true olarak ayarlanır.
   5. `encodingName` varsayılan olarak bırakılır veya **UTF-8**olarak ayarlanır.
   6. `quoteChar`, `escapeChar`ve `skipLineCount` belirtilmedi. PolyBase desteği, ADF 'de `firstRowAsHeader` olarak yapılandırılabilen üst bilgi satırını atlar.
   7. `compression` olabilir **sıkıştırma**, **GZip**, veya **Deflate**.

3. Kaynağınız bir klasörse `recursive` Copy etkinliğinin true olarak ayarlanması gerekir.

4. `wildcardFolderPath`, `wildcardFilename`, `modifiedDateTimeStart`ve `modifiedDateTimeEnd` belirtilmemiş.

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

### <a name="staged-copy-by-using-polybase"></a>PolyBase kullanarak hazırlanmış Kopyala

Kaynak verileriniz PolyBase ile yerel olarak uyumlu değilse, geçici hazırlama Azure Blob depolama örneği aracılığıyla veri kopyalamayı etkinleştirin (Azure Premium Depolama olamaz). Bu durumda Azure Data Factory, verileri, PolyBase 'in veri biçimi gereksinimlerini karşılayacak şekilde otomatik olarak dönüştürür. Daha sonra SQL veri ambarı 'na veri yüklemek için PolyBase 'i çağırır. Son olarak, blob depolama alanından, geçici verileri temizler. Bkz: [kopyalama aşamalı](copy-activity-performance.md#staged-copy) hazırlama bir Azure Blob Depolama örneği aracılığıyla veri kopyalama hakkında ayrıntılar için.

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

### <a name="best-practices-for-using-polybase"></a>PolyBase kullanmak için en iyi uygulamalar

Aşağıdaki bölümlerde, [Azure SYNAPSE Analytics Için en iyi yöntemler](../sql-data-warehouse/sql-data-warehouse-best-practices.md)bölümünde bahsedilenlere ek olarak en iyi uygulamalar sağlanmaktadır.

#### <a name="required-database-permission"></a>Gerekli database izni

PolyBase kullanmak için SQL Data Warehouse'a veri yükleyen kullanıcının olmalıdır ["CONTROL" izni](https://msdn.microsoft.com/library/ms191291.aspx) hedef veritabanında. Elde etmenin bir yolu olan bir üye olarak kullanıcı eklemek için **db_owner** rol. Bunu yapmak öğrenin [SQL veri ambarı genel bakış](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).

#### <a name="row-size-and-data-type-limits"></a>Satır boyutu ve veri sınırları yazın

PolyBase yükleri satır 1 MB'tan küçük sınırlıdır. VARCHR (MAX), NVARCHAR (MAX) veya VARBINARY (MAX) ' e yüklemek için kullanılamaz. Daha fazla bilgi için [SQL veri ambarı hizmet kapasite sınırları](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).

Veri kaynağınızı satırları 1 MB'den büyük olduğunda, dikey olarak kaynak tablolarda birkaç küçük parçalara bölmek isteyebilirsiniz. Her satırın en büyük boyut sınırı aşmadığından emin olun. Daha sonra, PolyBase kullanarak ve Azure SYNAPSE Analytics 'te birlikte birleştirilmiş daha küçük tablolar yüklenebilir.

Alternatif olarak, bu tür geniş sütunlara sahip veriler için, "PolyBase 'e izin ver" ayarını kapatarak ADF kullanarak verileri yüklemek için PolyBase 'i kullanabilirsiniz.

#### <a name="sql-data-warehouse-resource-class"></a>SQL veri ambarı kaynak sınıfı

Olası en iyi verimi elde etmek için PolyBase aracılığıyla SQL veri ambarı'na veri yükleyen kullanıcının daha büyük bir kaynak sınıfına atayın.

#### <a name="polybase-troubleshooting"></a>PolyBase sorunlarını giderme

**Ondalık sütuna yükleniyor**

Kaynak verileriniz Metin biçimindeyse veya PolyBase ile uyumlu olmayan diğer depolarda (hazırlanan kopya ve PolyBase kullanılarak) ve SQL veri ambarı ondalık sütununa yüklenecek boş bir değer içeriyorsa, şu hatayla karşılaşabilirsiniz:

```
ErrorCode=FailedDbOperation, ......HadoopSqlException: Error converting data type VARCHAR to DECIMAL.....Detailed Message=Empty string can't be converted to DECIMAL.....
```

Çözüm, "**tür Varsayılanı kullan**" seçeneğinin (false olarak) kopyalama etkinliği havuzu-> PolyBase ayarları ' nda seçimini kaldırır. "[USE_TYPE_DEFAULT](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql?view=azure-sqldw-latest#arguments
)", PolyBase metin dosyasından verileri aldığında sınırlandırılmış metin dosyalarında eksik değerlerin nasıl Işleneceğini belirten PolyBase yerel bir yapılandırmadır. 

**Azure SYNAPSE Analytics 'te `tableName`**

Aşağıdaki tabloda belirtmek örnekler verilmektedir **tableName** JSON veri kümesi özelliği. Bu, çeşitli birleşimlerini şema ve tablo adları gösterir.

| DB şema | Tablo adı | **tableName** JSON özelliği               |
| --------- | ---------- | ----------------------------------------- |
| dbo       | MyTable    | MyTable ya da dbo. MyTable veya [dbo].[MyTable] |
| dbo1      | MyTable    | dbo1. MyTable veya [dbo1].[MyTable]          |
| dbo       | My.Table   | [My.Table] veya [dbo].[My.Table]            |
| dbo1      | My.Table   | [dbo1].[My.Table]                         |

Aşağıdaki hatayı görürseniz, sorunu için belirtilen değer olabilir **tableName** özelliği. Doğru şekilde değerlerini belirtmek için önceki tabloya bakın **tableName** JSON özelliği.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

**Varsayılan değerlere sahip sütunlar**

Şu anda, Data Factory, PolyBase özelliği yalnızca aynı sayıda sütun hedef tabloda olduğu gibi kabul eder. Bunlardan biri, varsayılan bir değerle tanımlandığı dört sütunlarını içeren bir tablo buna bir örnektir. Giriş verilerini hala dört sütun olması gerekir. Üç sütunlu giriş veri kümesi aşağıdaki iletiye benzer bir hata verir:

```
All columns of the table must be specified in the INSERT BULK statement.
```

NULL değerini varsayılan değer özel bir biçimidir. Sütun null ise, söz konusu sütun için BLOB giriş veri boş olabilir. Ancak, giriş veri kümesinden eksik olamaz. PolyBase, Azure SYNAPSE Analytics 'te eksik değerler için NULL ekliyor.

## <a name="use-copy-statement"></a>Azure SQL veri ambarı 'na veri yüklemek için kopyalama ifadesini kullanma (Önizleme)

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
    >Azure depolama alanı sanal ağ hizmeti uç noktası ile yapılandırıldıysa, yönetilen kimlik kimlik doğrulaması kullanmanız gerekir- [VNET hizmet uç noktalarını Azure depolama ile kullanmanın etkileri](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)için başvurun. [Azure Blob tarafından yönetilen kimlik doğrulama](connector-azure-blob-storage.md#managed-identity) ve [Azure Data Lake Storage 2. tarafından yönetilen kimlik doğrulama](connector-azure-data-lake-storage.md#managed-identity) bölümündeki Data Factory gereken konfigürasyonları öğrenin.

2. Biçim ayarları şunlardır:

   1. **Parquet**: `compression` **sıkıştırma**, **Snappy**veya **gzip**olamaz.
   2. **Orc**: `compression`, **Compression**, **zlib**veya **Snappy**olabilir.
   3. **Sınırlandırılmış metin**için:
      1. `rowDelimiter`, açıkça **tek karakter** veya " **\r\n**" olarak ayarlanmıştır, varsayılan değer desteklenmez.
      2. `nullValue`, varsayılan olarak bırakılır veya **boş dize** ("") olarak ayarlanır.
      3. `encodingName` varsayılan olarak bırakılır veya **UTF-8 veya UTF-16**olarak ayarlanır.
      4. `escapeChar` `quoteChar`ile aynı olmalı ve boş değil.
      5. `skipLineCount` varsayılan olarak bırakılır veya 0 olarak ayarlanır.
      6. `compression`, **sıkıştırma** veya **gzip**olamaz.

3. Kaynağınız bir klasörse `recursive` Copy etkinliğinin true olarak ayarlanması gerekir.

4. `wildcardFolderPath`, `wildcardFilename`, `modifiedDateTimeStart`ve `modifiedDateTimeEnd` belirtilmemiş.

Kopyalama etkinliğinde `allowCopyCommand` altında aşağıdaki kopyalama ekstresi ayarları desteklenir:

| Özellik          | Açıklama                                                  | Gereklidir                                      |
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


## <a name="lookup-activity-properties"></a>Arama etkinliği özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [arama etkinliğini](control-flow-lookup-activity.md)denetleyin.

## <a name="getmetadata-activity-properties"></a>GetMetadata etkinlik özellikleri

Özelliklerle ilgili ayrıntıları öğrenmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) denetleyin 

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için veri türü eşlemesi

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Eşleme veri akışındaki verileri dönüştürürken Azure SYNAPSE Analytics 'teki tabloları okuyabilir ve yazabilirsiniz. Daha fazla bilgi için bkz. veri akışlarını eşleme içindeki [kaynak dönüştürme](data-flow-source.md) ve [Havuz dönüşümü](data-flow-sink.md) .

### <a name="source-transformation"></a>Kaynak dönüştürme

Azure SYNAPSE Analytics 'e özgü ayarlar, kaynak dönüşümünün **kaynak seçenekleri** sekmesinde bulunabilir. 

**Giriş:** Kaynağınızı bir tabloya mi işaret etmeyeceğinizi (```Select * from <table-name>```eşdeğerini) seçin veya özel bir SQL sorgusu girin.

**Sorgu**: giriş alanında sorgu ' yı seçerseniz, kaynağınız IÇIN bir SQL sorgusu girin. Bu ayar, veri kümesinde seçtiğiniz tüm tabloları geçersiz kılar. **Order by** yan tümceleri burada desteklenmez, ancak BIR tam select from ifadesini ayarlayabilirsiniz. Kullanıcı tanımlı tablo işlevleri de kullanabilirsiniz. **select * from udfGetData ()** , bir tablo döndüren SQL 'de bir UDF 'dir. Bu sorgu, veri akışınızda kullanabileceğiniz bir kaynak tablosu oluşturur. Sorguların kullanılması, test veya aramalar için satırları azaltmanın harika bir yoludur. 

* SQL örneği: ```Select * from MyTable where customerId > 1000 and customerId < 2000```

**Toplu iş boyutu**: büyük verileri okuma işleminde öbek için bir toplu iş boyutu girin.

**Yalıtım düzeyi**: eşleme VERI akışındaki SQL kaynakları için varsayılan değer read UNCOMMITTED ' dır. Yalıtım düzeyini buradaki değerlerden birine değiştirebilirsiniz:
* Okuma Işlendi
* Kaydedilmeyen oku
* Yinelenebilir okuma
* Seri hale getirilebilir
* Hiçbiri (yalıtım düzeyini yoksay)

![Yalıtım düzeyi](media/data-flow/isolationlevel.png "Yalıtım düzeyi")

### <a name="sink-transformation"></a>Havuz dönüştürme

Azure SYNAPSE Analytics 'e özgü ayarlar, havuz dönüşümünün **Ayarlar** sekmesinde bulunur.

**Güncelleştirme yöntemi:** Veritabanı Hedefinizdeki hangi işlemlere izin verileceğini belirler. Varsayılan değer yalnızca eklemeleri izin verir. Satırları güncelleştirmek, kaldırmak veya silmek için, bu eylemler için satırları etiketlemek üzere alter-Row dönüşümü gereklidir. Güncelleştirmeler, yukarı ve silme için bir anahtar sütunu veya sütunları ayarlanacak satırı belirleyecek şekilde ayarlanmalıdır.

**Tablo eylemi:** Yazmadan önce hedef tablodaki tüm satırların yeniden oluşturulup kaldırılacağını belirler.
* Hiçbiri: tabloya hiçbir eylem yapılmaz.
* Yeniden oluştur: tablo bırakılır ve yeniden oluşturulur. Dinamik olarak yeni bir tablo oluşturuluyoruz gereklidir.
* Kes: hedef tablodaki tüm satırlar kaldırılacak.

**Hazırlamayı etkinleştir:** Azure SYNAPSE Analytics 'e yazarken [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-ver15) 'i kullanıp kullanmayacağınızı belirler

**Toplu iş boyutu**: her bir sepete kaç satır yazıldığını denetler. Daha büyük toplu işlem boyutları sıkıştırma ve bellek iyileştirmeyi iyileştirir, ancak verileri önbelleğe alırken bellek dışında özel durumlar riskini ortadan kaldıracak.

**SQL betiklerini ön ve sonrası**: (ön işleme) ve sonra (işlem sonrası) verileri havuz veritabanınıza yazıldıktan sonra yürütülecek çok satırlı SQL betikleri girin

![SQL işleme betikleri ön ve sonrası](media/data-flow/prepost1.png "SQL işleme betikleri")

## <a name="data-type-mapping-for-azure-synapse-analytics"></a>Azure SYNAPSE Analytics için veri türü eşlemesi

Veya Azure SYNAPSE Analytics 'ten veri kopyaladığınızda, Azure SYNAPSE Analytics veri türleri arasında, geçici veri türlerini Azure Data Factory için aşağıdaki eşlemeler kullanılır. Bkz: [şema ve veri türü eşlemeleri](copy-activity-schema-and-type-mapping.md) kopyalama etkinliği havuz için kaynak şema ve veri türü eşlemelerini nasıl öğrenin.

>[!TIP]
>SQL DW tarafından desteklenen veri türleri ve desteklenmeyen çözümler için [Azure SYNAPSE Analytics makalesindeki tablo veri türleri](../sql-data-warehouse/sql-data-warehouse-tables-data-types.md) bölümüne bakın.

| Azure SYNAPSE Analytics veri türü    | Veri Fabrikası geçici veri türü |
| :------------------------------------ | :----------------------------- |
| bigint                                | Int64                          |
| binary                                | Byte[]                         |
| bit                                   | Boole                        |
| char                                  | String, Char[]                 |
| date                                  | Tarih Saat                       |
| Datetime                              | Tarih Saat                       |
| datetime2                             | Tarih Saat                       |
| Datetimeoffset                        | DateTimeOffset                 |
| Decimal                               | Decimal                        |
| FILESTREAM attribute (varbinary(max)) | Byte[]                         |
| Kayan                                 | Double                         |
| image                                 | Byte[]                         |
| int                                   | Int32                          |
| money                                 | Decimal                        |
| nchar                                 | String, Char[]                 |
| numeric                               | Decimal                        |
| nvarchar                              | String, Char[]                 |
| real                                  | Tek                         |
| rowversion                            | Byte[]                         |
| smalldatetime                         | Tarih Saat                       |
| smallint                              | Int16                          |
| smallmoney                            | Decimal                        |
| time                                  | TimeSpan                       |
| tinyint                               | Bayt                           |
| uniqueidentifier                      | Guid                           |
| varbinary                             | Byte[]                         |
| varchar                               | String, Char[]                 |

## <a name="next-steps"></a>Sonraki adımlar
Azure veri fabrikasında kopyalama etkinliği tarafından kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).
