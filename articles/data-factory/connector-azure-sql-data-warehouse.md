---
title: Azure Synapse Analytics'te verileri kopyalama ve dönüştürme
description: Azure Synapse Analytics'e veri kopyalamayı ve Azure Synapse Analytics'teki verileri Veri Fabrikası'nı kullanarak nasıl dönüştüreceklerini öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/25/2020
ms.openlocfilehash: 822a981b84919670aa476567625cdf914206eaa8
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422176"
---
# <a name="copy-and-transform-data-in-azure-synapse-analytics-formerly-azure-sql-data-warehouse-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Azure Synapse Analytics'teki (eski adıyla Azure SQL Veri Ambarı) verileri kopyalama ve dönüştürme 

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm1](v1/data-factory-azure-sql-data-warehouse-connector.md)
> * [Geçerli sürüm](connector-azure-sql-data-warehouse.md)

Bu makalede, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin Azure Synapse Analytics'ten ve Azure Synapse Analytics'ten kopyalanması ve Azure Veri Gölü Depolama Gen2'deki verileri dönüştürmek için Veri Akışı'nı nasıl kullanacağı açıklanmaktadır. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure Synapse Analytics bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matris](copy-activity-overview.md) tablosu yla [etkinliği](copy-activity-overview.md) kopyalama
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)

Kopyalama etkinliği için bu Azure Synapse Analytics bağlayıcısı aşağıdaki işlevleri destekler:

- SQL kimlik doğrulaması ve Azure Etkin Dizini (Azure AD) Uygulama belirteç kimlik doğrulaması kullanarak verileri azure kaynakları için bir hizmet sorumlusu veya yönetilen kimliklerle kopyalayın.
- Kaynak olarak, bir SQL sorgusu veya depolanan yordam ı kullanarak veri alın.
- Lavabo olarak, [PolyBase](#use-polybase-to-load-data-into-azure-sql-data-warehouse) veya [COPY deyimi](#use-copy-statement) (önizleme) veya toplu ekleme kullanarak veri yükleyin. Daha iyi kopyalama performansı için PolyBase veya COPY deyimini (önizleme) öneririz.

> [!IMPORTANT]
> Azure Veri Fabrikası Tümleştirme Runtime'ı kullanarak verileri kopyalarsanız, Azure hizmetlerinin sunucuya erişebilmeleri için bir [Azure SQL sunucu güvenlik duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) yapılandırın.
> Kendi kendine barındırılan bir tümleştirme çalışma zamanı kullanarak verileri kopyalarsanız, Azure SQL sunucu güvenlik duvarını uygun IP aralığına izin verecek şekilde yapılandırın. Bu aralık, makinenin Azure Synapse Analytics'e bağlanmak için kullanılan IP'sini içerir.

## <a name="get-started"></a>başlarken

> [!TIP]
> En iyi performansı elde etmek için Azure Synapse Analytics'e veri yüklemek için PolyBase'i kullanın. [Verileri Azure Synapse Analytics bölümüne yüklemek için PolyBase'i kullanın,](#use-polybase-to-load-data-into-azure-sql-data-warehouse) ayrıntılar alabilirsiniz. Kullanım örneğine sahip bir yol için Azure [Veri Fabrikası ile 15 dakikanın altında Azure Synapse Analytics'e 1 TB Yükle'ye](load-azure-sql-data-warehouse.md)bakın.

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Bir Azure Synapse Analytics bağlayıcısına özgü Veri Fabrikası varlıklarını tanımlayan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler Azure Synapse Analytics bağlantılı bir hizmet için desteklenir:

| Özellik            | Açıklama                                                  | Gerekli                                                     |
| :------------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| type                | Tür özelliği **AzureSqlDW**olarak ayarlanmalıdır.             | Evet                                                          |
| Connectionstring    | **ConnectionString** özelliği için Azure Synapse Analytics örneğine bağlanmak için gereken bilgileri belirtin. <br/>Bu alanı Güvenli Bir Şekilde Veri Fabrikası'nda depolamak için SecureString olarak işaretleyin. Azure Key Vault'a parola/hizmet ana anahtarını da koyabilirsiniz ve `password` SQL kimlik doğrulamasıysa yapılandırmayı bağlantı dizesinin dışına çekebilirsiniz. Daha fazla ayrıntı içeren [Azure Key Vault makalesinde](store-credentials-in-key-vault.md) tablonun altındaki JSON örneğine ve Mağaza kimlik bilgilerine bakın. | Evet                                                          |
| hizmetPrincipalId  | Uygulamanın istemci kimliğini belirtin.                         | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulamasını kullandığınızda. |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alanı, Veri Fabrikası'nda güvenli bir şekilde depolamak için SecureString olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizliye başvurun.](store-credentials-in-key-vault.md) | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulamasını kullandığınızda. |
| Kiracı              | Uygulamanızın bulunduğu kiracı bilgilerini (etki alanı adı veya kiracı kimliği) belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek geri alabilirsiniz. | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulamasını kullandığınızda. |
| connectVia          | Veri deposuna bağlanmak için kullanılacak [tümleştirme çalışma süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Zamanı'nı veya kendi kendine barındırılan tümleştirme çalışma süresini (veri mağazanız özel bir ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Hayır                                                           |

Farklı kimlik doğrulama türleri için, sırasıyla önkoşullar ve JSON örnekleri yle ilgili aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- Azure AD uygulama belirteç kimlik doğrulaması: [Hizmet sorumlusu](#service-principal-authentication)
- Azure AD uygulama belirteç kimlik doğrulaması: [Azure kaynakları için yönetilen kimlikler](#managed-identity)

>[!TIP]
>"UserErrorFailedToConnectToSqlServer" ve "Veritabanı için oturum sınırı XXX ve ulaşıldı" gibi iletilerle hata `Pooling=false` koduyla hata yaptıysanız, bağlantı dizenize ekleyin ve yeniden deneyin.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

**Azure Anahtar Kasasında Parola:**

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

Hizmet temeltabanlı Azure AD uygulama belirteç kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin:

1. Azure portalından **[bir Azure Etkin Dizin uygulaması oluşturun.](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)** Uygulama adını ve bağlantılı hizmeti tanımlayan aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Azure SQL sunucunuz için Azure Sql sunucunuz için azure portalında **[bir Azure Active Directory yöneticisi sağlama.](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** Azure AD yöneticisi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetilen kimliğe sahip gruba yönetici rolü verirseniz, 3 ve 4 adımlarını atlayın. Yönetici veritabanına tam erişime sahip olacaktır.

3. Hizmet ilkesi için **[veritabanı kullanıcıları içeren oluşturun.](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** En az KULLANICI İzNİ KULLANICISI OLAN Bir Azure REKLAM kimliğiyle SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz veri ambarına bağlanın. Aşağıdaki T-SQL'i çalıştırın:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. Normalde SQL kullanıcıları veya başkaları için yaptığınız gibi **hizmet ilkesine gereken izinleri ver.** Aşağıdaki kodu çalıştırın veya [burada](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017)daha fazla seçeneğe bakın. Verileri yüklemek için PolyBase'i kullanmak istiyorsanız, [gerekli veritabanı iznini](#required-database-permission)öğrenin.

    ```sql
    EXEC sp_addrolemember db_owner, [your application name];
    ```

5. Azure Veri Fabrikası'nda **Azure Synapse Analytics bağlantılı bir hizmeti yapılandırın.**


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet temel kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için yönetilen kimlikler

Bir veri fabrikası, belirli fabrikayı temsil eden Azure kaynakları için yönetilen bir [kimlikle](data-factory-service-identity.md) ilişkilendirilebilir. Bu yönetilen kimliği Azure Synapse Analytics kimlik doğrulaması için kullanabilirsiniz. Atanan fabrika, bu kimliği kullanarak verilere erişebilir ve veri ambarınıza erişebilir ve bunları kopyalayabilir.

Yönetilen kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin:

1. Azure SQL sunucunuz için Azure Sql sunucunuz için azure portalında **[bir Azure Active Directory yöneticisi sağlama.](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** Azure AD yöneticisi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetilen kimliğe sahip gruba yönetici rolü verirseniz, 3 ve 4 adımlarını atlayın. Yönetici veritabanına tam erişime sahip olacaktır.

2. Veri Fabrikası Yönetilen Kimliği için **[veritabanı kullanıcıları içeren oluşturun.](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** En az KULLANICI İzNİ KULLANICISI OLAN Bir Azure REKLAM kimliğiyle SSMS gibi araçları kullanarak verileri kopyalamak istediğiniz veri ambarına bağlanın. Aşağıdaki T-SQL'i çalıştırın. 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. Veri Fabrikası Yönetilen Kimlik normalde SQL kullanıcıları ve diğerleri için yaptığınız gibi **gerekli izinleri verin.** Aşağıdaki kodu çalıştırın veya [burada](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017)daha fazla seçeneğe bakın. Verileri yüklemek için PolyBase'i kullanmak istiyorsanız, [gerekli veritabanı iznini](#required-database-permission)öğrenin.

    ```sql
    EXEC sp_addrolemember db_owner, [your Data Factory name];
    ```

5. Azure Veri Fabrikası'nda **Azure Synapse Analytics bağlantılı bir hizmeti yapılandırın.**

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [Datasets](concepts-datasets-linked-services.md) makalesine bakın. 

Azure Synapse Analytics veri kümesi için aşağıdaki özellikler desteklenir:

| Özellik  | Açıklama                                                  | Gerekli                    |
| :-------- | :----------------------------------------------------------- | :-------------------------- |
| type      | Veri kümesinin **tür** özelliği **AzureSqlDWTable**olarak ayarlanmalıdır. | Evet                         |
| Şema | Şema adı. |Kaynak için hayır, lavabo için Evet  |
| tablo | Tablonun/görünümün adı. |Kaynak için hayır, lavabo için Evet  |
| tableName | Şema ile tablo/görünüm adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü `schema` `table`için, kullanım ve . | Kaynak için hayır, lavabo için Evet |

#### <a name="dataset-properties-example"></a>Dataset özellikleri örneği

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

## <a name="copy-activity-properties"></a>Etkinlik özelliklerini kopyalama

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi [için, Pipelines](concepts-pipelines-activities.md) makalesine bakın. Bu bölümde, Azure Synapse Analytics kaynağı ve lavabosu tarafından desteklenen özelliklerin bir listesi yer almaktadır.

### <a name="azure-synapse-analytics-as-the-source"></a>Kaynak olarak Azure Synapse Analytics

Azure Synapse Analytics'teki verileri kopyalamak için, Kopyalama Etkinliği kaynağındaki **tür** özelliğini **SqlDWSource**olarak ayarlayın. Aşağıdaki özellikler Kopyalama Etkinliği **kaynak** bölümünde desteklenir:

| Özellik                     | Açıklama                                                  | Gerekli |
| :--------------------------- | :----------------------------------------------------------- | :------- |
| type                         | Kopyalama Etkinliği kaynağının **tür** özelliği **SqlDWSource**olarak ayarlanmalıdır. | Evet      |
| sqlReaderQuery               | Verileri okumak için özel SQL sorgusunu kullanın. Örnek: `select * from MyTable`. | Hayır       |
| sqlReaderStoredProcedureNameName | Kaynak tablodaki verileri okuyan depolanan yordamın adı. Son SQL deyimi, depolanan yordamda bir SELECT deyimi olmalıdır. | Hayır       |
| depolanmışProsedürParametreleri    | Depolanan yordam için parametreler.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. | Hayır       |
| ısolationlevel | SQL kaynağı için hareket kilitleme davranışını belirtir. İzin verilen değerler şunlardır: **ReadCommitted** (varsayılan), **ReadUncommitted**, **Tekrarlanabilir Oku**, **Serializable**, **Anlık Görüntü**. Daha fazla bilgi için [bu dokümana](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) bakın. | Hayır |

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

**Örnek: depolanan yordamı kullanma**

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

**Örnek depolanan yordam:**

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

### <a name="azure-synapse-analytics-as-sink"></a><a name="azure-sql-data-warehouse-as-sink"></a>Lavabo olarak Azure Synapse Analytics

Azure Veri Fabrikası, SQL Veri Ambarı'na veri yüklemenin üç yolunu destekler.

![SQL DW lavabo kopyalama seçenekleri](./media/connector-azure-sql-data-warehouse/sql-dw-sink-copy-options.png)

- [PolyBase'i kullanma](#use-polybase-to-load-data-into-azure-sql-data-warehouse) 
- [COPY deyimini kullanma (önizleme)](#use-copy-statement)
- Toplu kesici uç kullanma

Veri yüklemenin en hızlı ve en ölçeklenebilir yolu [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) veya [COPY deyimi](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (önizleme) geçer.

Verileri Azure SQL Veri Ambarı'na kopyalamak için, Kopyalama Etkinliği'ndeki lavabo türünü **SqlDWSink**olarak ayarlayın. Aşağıdaki özellikler Kopyalama Etkinliği **lavabo** bölümünde desteklenir:

| Özellik          | Açıklama                                                  | Gerekli                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| type              | Kopyalama Etkinliği lavabonun **türü** özelliği **SqlDWSink**olarak ayarlanmalıdır. | Evet                                           |
| Izin PolyBase     | Verileri SQL Veri Ambarı'na yüklemek için PolyBase'in kullanılıp kullanılmayacağını gösterir. `allowCopyCommand`ve `allowPolyBase` her ikisi de doğru olamaz. <br/><br/>Kısıtlamalar ve ayrıntılar için [verileri Azure SQL Veri Ambarı bölümüne yüklemek için PolyBase'i kullanın'](#use-polybase-to-load-data-into-azure-sql-data-warehouse) a bakın.<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan) olarak verilir. | Hayır.<br/>PolyBase kullanırken uygulayın.     |
| polyBaseSettings  | `allowPolybase` Özellik **doğru**ayarlandığında belirtilebilen özellikler grubu. | Hayır.<br/>PolyBase kullanırken uygulayın. |
| allowCopyCommand | Verileri SQL Veri Ambarı'na yüklemek için [COPY deyiminin](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (önizleme) kullanılıp kullanılmayacağını gösterir. `allowCopyCommand`ve `allowPolyBase` her ikisi de doğru olamaz. <br/><br/>Kısıtlamalar ve ayrıntılar için [verileri Azure SQL Veri Ambarı bölümüne yüklemek](#use-copy-statement) için COPY ifadesini kullanın bölümüne bakın.<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan) olarak verilir. | Hayır.<br>COPY kullanırken uygulayın. |
| copyCommandSettings | Özellik TRUE olarak ayarlandığında `allowCopyCommand` belirtilebilen bir özellik grubu. | Hayır.<br/>COPY kullanırken uygulayın. |
| yazmaBatchSize    | **Toplu iş başına**SQL tablosuna eklenecek satır sayısı.<br/><br/>İzin verilen **değer, tümseci** (satır sayısı) olarak bilinir. Varsayılan olarak, Veri Fabrikası satır boyutunu temel alarak uygun toplu iş boyutunu dinamik olarak belirler. | Hayır.<br/>Toplu kesici uç kullanırken uygulayın.     |
| yazmaBatchTimeout | Toplu ekleme işleminin zaman bitmeden bitmeden tamamlanması için bekleme süresi.<br/><br/>İzin verilen değer **zaman ası'** dır. Örnek: "00:30:00" (30 dakika). | Hayır.<br/>Toplu kesici uç kullanırken uygulayın.        |
| preCopyScript     | Her çalıştırmada Azure SQL Veri Ambarı'na veri yazmadan önce çalışması için Kopyalama Etkinliği için bir SQL sorgusu belirtin. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanın. | Hayır                                            |
| Tableoption | Kaynak şemaya göre yoksa, lavabo tablosunun otomatik olarak oluşturulup oluşturulmayacağını belirtir. Otomatik tablo oluşturma, sahnelenen kopya kopya etkinliğinde yapılandırıldığında desteklenmez. İzin verilen `none` değerler şunlardır: (varsayılan), `autoCreate`. |Hayır |
| devre dışı KalımMetricsCollection | Veri Fabrikası, kopya performans optimizasyonu ve öneriler için SQL Veri Ambarı DWUs'ları gibi ölçümler toplar. Bu davranışla ilgileniyorsanız, `true` kapatmak için belirtin. | Hayır (varsayılan) `false` |

#### <a name="sql-data-warehouse-sink-example"></a>SQL Veri Ambarı lavabo örneği

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

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a>Verileri Azure SQL Veri Ambarı'na yüklemek için PolyBase'i kullanın

[PolyBase'i](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) kullanmak, yüksek iş gücüyle Azure Synapse Analytics'e büyük miktarda veri yüklemenin etkili bir yoludur. Varsayılan BULKINSERT mekanizması yerine PolyBase kullanarak iş elde edilen iş başına büyük bir kazanç görürsünüz. Kullanım örneğini içeren bir yol için Azure [Synapse Analytics'e Load 1 TB'ye](v1/data-factory-load-sql-data-warehouse.md)bakın.

* Kaynak verileriniz **Azure Blob, Azure Veri Gölü Depolama Gen1 veya Azure Veri Gölü Depolama Gen2'deyse ve**biçim **PolyBase uyumluysa,** Azure SQL Veri Ambarı'nın verileri kaynaktan çekmesine izin vermek için doğrudan PolyBase'i çağırmak için kopyalama etkinliğini kullanabilirsiniz. Ayrıntılar **[için, PolyBase kullanarak Doğrudan kopyaya](#direct-copy-by-using-polybase)** bakın.
* Kaynak veri depolamanız ve biçiminiz başlangıçta PolyBase tarafından desteklenmiyorsa, Bunun yerine PolyBase özelliğini **[kullanarak Staged kopyasını kullanın.](#staged-copy-by-using-polybase)** Aşamalı kopyalama özelliği de daha iyi iş elde sağlar. Verileri otomatik olarak PolyBase uyumlu biçime dönüştürür, verileri Azure Blob depolama alanında depolar.sonra verileri SQL Veri Ambarı'na yüklemek için PolyBase'i arar.

>[!TIP]
>[PolyBase'i kullanmak için en iyi uygulamalar](#best-practices-for-using-polybase)hakkında daha fazla bilgi edinin.

Aşağıdaki PolyBase ayarları kopyalama `polyBaseSettings` etkinliğinde desteklenir:

| Özellik          | Açıklama                                                  | Gerekli                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| rejectValue       | Sorgu başarısız olmadan önce reddedilebilen satırların sayısını veya yüzdesini belirtir.<br/><br/>[CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx)bölümünün Bağımsız Değişkenler bölümünde PolyBase'in reddetme seçenekleri hakkında daha fazla bilgi edinin. <br/><br/>İzin verilen değerler 0 (varsayılan), 1, 2, vb. | Hayır                                            |
| rejectType        | **RejectValue** seçeneğinin gerçek bir değer mi yoksa yüzde mi olduğunu belirtir.<br/><br/>İzin verilen değerler **Değer** (varsayılan) ve **Yüzde'dir.** | Hayır                                            |
| rejectSampleValue | PolyBase reddedilen satırların yüzdesini yeniden hesaplamadan önce alınacak satır sayısını belirler.<br/><br/>İzin verilen değerler 1, 2, vb. dir. | Evet, **rejectType** **yüzdesi**ise. |
| useTypeDefault    | PolyBase metin dosyasından veri aldığında sınırlı metin dosyalarındaki eksik değerlerin nasıl işleyeceğini belirtir.<br/><br/>[CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx)'deki Bağımsız Değişkenler bölümünden bu özellik hakkında daha fazla bilgi edinin.<br/><br/>İzin verilen değerler **True** ve **False** (varsayılan) olarak verilir.<br><br> | Hayır                                            |

### <a name="direct-copy-by-using-polybase"></a>PolyBase kullanarak doğrudan kopyalama

SQL Veri Ambarı PolyBase doğrudan Azure Blob, Azure Veri Gölü Depolama Gen1 ve Azure Veri Gölü Depolama Gen2 destekler. Kaynak verileriniz bu bölümde açıklanan ölçütlere uygunsa, doğrudan kaynak veri deposundan Azure Synapse Analytics'e kopyalamak için PolyBase'i kullanın. Aksi takdirde, [PolyBase kullanarak Staged kopya kullanın.](#staged-copy-by-using-polybase)

> [!TIP]
> Verileri SQL Veri Ambarı'na verimli bir şekilde kopyalamak, Azure Veri Fabrikası'ndan daha fazla bilgi edinmek, [SQL Veri Ambarı ile Data Lake Store'u kullanırken verilerden elde edilen öngörüleri ortaya](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/)çıkarmak için daha da kolay ve kullanışlı hale getirir.

Gereksinimler karşılanmazsa, Azure Veri Fabrikası ayarları denetler ve veri hareketi için OTOMATIK olarak BULKINSERT mekanizmasına geri döner.

1. **Kaynak bağlantılı hizmet** aşağıdaki türleri ve kimlik doğrulama yöntemleri ile:

    | Desteklenen kaynak veri deposu türü                             | Desteklenen kaynak kimlik doğrulama türü                        |
    | :----------------------------------------------------------- | :---------------------------------------------------------- |
    | [Azure Blob](connector-azure-blob-storage.md)                | Hesap anahtarı kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |
    | [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | Hizmet sorumlusu kimlik doğrulaması                            |
    | [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | Hesap anahtarı kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |

    >[!IMPORTANT]
    >Azure Depolama alanınız VNet hizmet bitiş noktasıyla yapılandırıldıysa, yönetilen kimlik doğrulamasını kullanmanız gerekir - [Azure depolama alanıyla VNet Hizmet Bitiş Noktalarını kullanmanın etkisine](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)bakın. Veri Fabrikası'nda gerekli yapılandırmaları [Azure Blob'dan (yönetilen kimlik kimlik doğrulaması](connector-azure-blob-storage.md#managed-identity) ve [Azure Veri Gölü Depolama Gen2)](connector-azure-data-lake-storage.md#managed-identity) sırasıyla yönetilen kimlik kimlik doğrulama bölümünden öğrenin.

2. **Kaynak veri biçimi** **Parke**, **ORC**, veya **Delimited metin**, aşağıdaki yapılandırmaları ile:

   1. Klasör yolu joker karakter filtresi içermez.
   2. Dosya adı boş veya tek bir dosyaya işaret ediyor. Kopya etkinliğinde joker karakter dosya adı belirtirseniz, `*` `*.*`yalnızca .
   3. `rowDelimiter`**varsayılan**, **\n**, **\r\n**veya **\r**.
   4. `nullValue`varsayılan olarak bırakılır veya **boş dize** ("") olarak ayarlanır ve `treatEmptyAsNull` varsayılan olarak bırakılır veya doğru olarak ayarlanır.
   5. `encodingName`varsayılan olarak bırakılır veya **utf-8**olarak ayarlanır.
   6. `quoteChar`, `escapeChar`ve `skipLineCount` belirtilmemiş. ADF'deki gibi `firstRowAsHeader` yapılandırılabilen PolyBase desteği üstbilgi satırLarını atla.
   7. `compression`hiçbir **sıkıştırma**olabilir , **GZip**, veya **Deflate**.

3. Kaynağınız bir klasörse, `recursive` kopyalama etkinliğinde doğru olarak ayarlanmalıdır.

4. `wildcardFolderPath`, `wildcardFilename` `modifiedDateTimeStart`, `modifiedDateTimeEnd` `additionalColumns` , ve belirtilmemiştir.

>[!NOTE]
>Kaynağınız bir klasörse, PolyBase'in klasörden ve tüm alt klasörlerinden dosyaları aldığını ve dosya adının altı çizili (_) veya bir dönemle (.) başladığı dosyalardan veri almadığını unutmayın [- LOCATION bağımsız değişkeni](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql?view=azure-sqldw-latest#arguments-2).

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

### <a name="staged-copy-by-using-polybase"></a>PolyBase kullanarak aşamalı kopya

Kaynak verileriniz PolyBase ile yerel olarak uyumlu değilse, geçici bir evreleme Azure Blob depolama örneği aracılığıyla veri kopyalamayı etkinleştirin (Azure Premium Depolama olamaz). Bu durumda, Azure Veri Fabrikası verileri PolyBase'in veri biçimi gereksinimlerini karşılamak üzere otomatik olarak dönüştürür. Daha sonra, verileri SQL Veri Ambarı'na yüklemek için PolyBase'i çağırır. Son olarak, blob depolama dan geçici verileri temizler. Bir evreleme Azure Blob depolama örneği aracılığıyla verileri kopyalama yla ilgili ayrıntılar için [Aşamalı kopyaya](copy-activity-performance-features.md#staged-copy) bakın.

Bu özelliği kullanmak için, geçici blob depolama alanıyla Birlikte Azure depolama hesabına atıfta bulunan bir [Azure Blob Depolama bağlantısı](connector-azure-blob-storage.md#linked-service-properties) na sahip bir hizmet oluşturun. Ardından, `enableStaging` Aşağıdaki `stagingSettings` kodda gösterildiği gibi Kopyalama Etkinliği'nin özelliklerini ve özelliklerini belirtin.

>[!IMPORTANT]
>Azure Depolama alanınız VNet hizmet bitiş noktasıyla yapılandırıldıysa, yönetilen kimlik doğrulamasını kullanmanız gerekir - [Azure depolama alanıyla VNet Hizmet Bitiş Noktalarını kullanmanın etkisine](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)bakın. [Azure Blob - yönetilen kimlik kimlik doğrulaması](connector-azure-blob-storage.md#managed-identity)adresinden Veri Fabrikası'nda gerekli yapılandırmaları öğrenin.

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

Aşağıdaki bölümler, [Azure Synapse Analytics için En İyi Uygulamalar'da](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-best-practices.md)belirtilenuygulamalara ek olarak en iyi uygulamaları sağlar.

#### <a name="required-database-permission"></a>Gerekli veritabanı izni

PolyBase'i kullanmak için, verileri SQL Veri Ambarı'na yükleyen kullanıcının hedef veritabanında ["CONTROL" iznine](https://msdn.microsoft.com/library/ms191291.aspx) sahip olması gerekir. Bunu başarabilmek için bir yolu **db_owner** rolü bir üyesi olarak kullanıcı eklemektir. [BUNU SQL Veri Ambarı genel bakışta](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization)nasıl yapacağınızı öğrenin.

#### <a name="row-size-and-data-type-limits"></a>Satır boyutu ve veri türü sınırları

PolyBase yükleri 1 MB'dan küçük satırlarla sınırlıdır. VARCHR(MAX), NVARCHAR(MAX) veya VARBINARY(MAX) yüklemek için kullanılamaz. Daha fazla bilgi için SQL [Veri Ambarı hizmet kapasite sınırlarına](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads)bakın.

Kaynak verilerinizin 1 MB'dan büyük satırları varsa, kaynak tabloları dikey olarak birkaç küçük tabloya bölmek isteyebilirsiniz. Her satırın en büyük boyutunun sınırı aşmadığından emin olun. Daha küçük tablolar Daha sonra PolyBase kullanılarak yüklenebilir ve Azure Synapse Analytics'te birleştirilebilir.

Alternatif olarak, bu kadar geniş sütunlara sahip veriler için, "PolyBase'e izin ver" ayarını kapatarak Verileri ADF kullanarak yüklemek için PolyBase olmayan verileri kullanabilirsiniz.

#### <a name="sql-data-warehouse-resource-class"></a>SQL Veri Ambarı kaynak sınıfı

Mümkün olan en iyi iş verisini elde etmek için, PolyBase üzerinden VERILERI SQL Veri Ambarı'na yükleyen kullanıcıya daha büyük bir kaynak sınıfı atayın.

#### <a name="polybase-troubleshooting"></a>PolyBase sorun giderme

**Ondalık sütuna yükleme**

Kaynak verileriniz metin biçiminde veya PolyBase uyumlu olmayan diğer mağazalardaysa (aşamalı kopya ve PolyBase kullanarak) ve SQL Veri Ambarı Ondalık sütuna yüklenmek üzere boş değer içeriyorsa, aşağıdaki hataya ulaşabilirsiniz:

```
ErrorCode=FailedDbOperation, ......HadoopSqlException: Error converting data type VARCHAR to DECIMAL.....Detailed Message=Empty string can't be converted to DECIMAL.....
```

Çözüm, kopya etkinliği lavabosu -> PolyBase ayarlarında " Varsayılan ı**kullan**" seçeneğini (yanlış olarak) seçmektir. "[USE_TYPE_DEFAULT](https://docs.microsoft.com/sql/t-sql/statements/create-external-file-format-transact-sql?view=azure-sqldw-latest#arguments
)", PolyBase metin dosyasından veri aldığında sınırlı metin dosyalarındaki eksik değerlerin nasıl işleyeceğini belirten bir PolyBase yerel yapılandırmasıdır. 

**`tableName`Azure Synapse Analytics içinde**

Aşağıdaki tablo, JSON veri kümesinde **tablo Adı** özelliğinin nasıl belirtilenene ilgili örnekler verir. Bu şema ve tablo adları çeşitli kombinasyonları gösterir.

| DB Şema | Tablo adı | **tabloAdı** JSON özelliği               |
| --------- | ---------- | ----------------------------------------- |
| Dbo       | Mytable    | MyTable veya dbo. MyTable veya [dbo]. [MyTable] |
| dbo1      | Mytable    | dbo1. MyTable veya [dbo1]. [MyTable]          |
| Dbo       | My.Table   | [My.Table] veya [dbo]. [My.Table]            |
| dbo1      | My.Table   | [dbo1]. [My.Table]                         |

Aşağıdaki hatayı görürseniz, sorun **tabloAdı** özelliği için belirttiğiniz değer olabilir. **Tablo Adı** JSON özelliği için değerleri belirtmek için doğru yol için önceki tabloya bakın.

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

**Varsayılan değerlere sahip sütunlar**

Şu anda, Veri Fabrikası'ndaki PolyBase özelliği yalnızca hedef tablodaki sütun sayısını kabul eder. Örnek, bunlardan birinin varsayılan değerle tanımlandığı dört sütunlu bir tablodur. Giriş verilerinin hala dört sütunu olması gerekir. Üç sütunlu giriş veri kümesi aşağıdaki iletiye benzer bir hata verir:

```
All columns of the table must be specified in the INSERT BULK statement.
```

NULL değeri varsayılan değerin özel bir biçimidir. Sütun nullable ise, bu sütun için blob giriş verileri boş olabilir. Ancak giriş veri kümesinden eksik olamaz. PolyBase, Azure Synapse Analytics'te eksik değerler için NULL ekler.

## <a name="use-copy-statement-to-load-data-into-azure-sql-data-warehouse-preview"></a><a name="use-copy-statement"></a>Verileri Azure SQL Veri Ambarı'na yüklemek için COPY deyimini kullanma (önizleme)

SQL Veri Ambarı [COPY deyimi](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) (önizleme), **Azure Blob ve Azure Veri Gölü Depolama Gen2'den**veri yüklemeyi doğrudan destekler. Kaynak verileriniz bu bölümde açıklanan ölçütleri karşılıyorsa, verileri Azure SQL Veri Ambarı'na yüklemek için ADF'deki COPY deyimini kullanmayı seçebilirsiniz. Azure Veri Fabrikası ayarları denetler ve ölçütler karşılanmazsa kopyalama etkinliği çalıştırılmaz.

>[!NOTE]
>Şu anda Veri Fabrikası yalnızca aşağıda belirtilen COPY deyimi uyumlu kaynaklardan kopyayı desteklemeyi desteklememektedir.

COPY deyimini kullanmak aşağıdaki yapılandırmayı destekler:

1. **Kaynak bağlantılı hizmet ve biçim** aşağıdaki türleri ve kimlik doğrulama yöntemleri ile vardır:

    | Desteklenen kaynak veri deposu türü                             | Desteklenen biçim           | Desteklenen kaynak kimlik doğrulama türü                         |
    | :----------------------------------------------------------- | -------------------------- | :----------------------------------------------------------- |
    | [Azure Blob](connector-azure-blob-storage.md)                | [Sınırlı metin](format-delimited-text.md)             | Hesap anahtarı kimlik doğrulaması, paylaşılan erişim imzası kimlik doğrulaması, hizmet asıllığı, yönetilen kimlik kimlik doğrulaması |
    | &nbsp;                                                       | [Parke](format-parquet.md)                    | Hesap anahtarı kimlik doğrulaması, paylaşılan erişim imzası kimlik doğrulaması |
    | &nbsp;                                                       | [Orc](format-orc.md)                        | Hesap anahtarı kimlik doğrulaması, paylaşılan erişim imzası kimlik doğrulaması |
    | [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | [Sınırlı metin](format-delimited-text.md)<br/>[Parke](format-parquet.md)<br/>[Orc](format-orc.md) | Hesap anahtarı kimlik doğrulaması, hizmet asıl kimlik doğrulaması, yönetilen kimlik kimlik doğrulaması |

    >[!IMPORTANT]
    >Azure Depolama alanınız VNet hizmet bitiş noktasıyla yapılandırıldıysa, yönetilen kimlik doğrulamasını kullanmanız gerekir - [Azure depolama alanıyla VNet Hizmet Bitiş Noktalarını kullanmanın etkisine](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage)bakın. Veri Fabrikası'nda gerekli yapılandırmaları [Azure Blob'dan (yönetilen kimlik kimlik doğrulaması](connector-azure-blob-storage.md#managed-identity) ve [Azure Veri Gölü Depolama Gen2)](connector-azure-data-lake-storage.md#managed-identity) sırasıyla yönetilen kimlik kimlik doğrulama bölümünden öğrenin.

2. Biçim ayarları aşağıdaki gibidir:

   1. **Parke**için `compression` : **hiçbir sıkıştırma**olabilir , **Snappy**, veya **GZip**.
   2. **ORC**için `compression` : **hiçbir** **```zlib```** sıkıştırma olabilir , veya **Snappy**.
   3. **Sınırlı metin**için:
      1. `rowDelimiter`**açıkça tek karakter** olarak ayarlanır veya "**\r\n**", varsayılan değer desteklenmez.
      2. `nullValue`varsayılan olarak bırakılır veya **boş dize** ("") olarak ayarlanır.
      3. `encodingName`varsayılan olarak bırakılır veya **utf-8 veya utf-16**olarak ayarlanır.
      4. `escapeChar`olarak aynı `quoteChar`olmalıdır ve boş değildir.
      5. `skipLineCount`varsayılan olarak bırakılır veya 0 olarak ayarlanır.
      6. `compression`hiçbir **sıkıştırma** veya **GZip**olabilir.

3. Kaynağınız bir klasörse, `recursive` kopyalama etkinliğinde doğru olarak ayarlanmalıdır.

4. `wildcardFolderPath`, `wildcardFilename` `modifiedDateTimeStart`, `modifiedDateTimeEnd` `additionalColumns` , ve belirtilmemiştir.

Aşağıdaki COPY deyimi ayarları kopyalama `allowCopyCommand` etkinliğinde desteklenir:

| Özellik          | Açıklama                                                  | Gerekli                                      |
| :---------------- | :----------------------------------------------------------- | :-------------------------------------------- |
| varsayılan Değerler | SQL DW'deki her hedef sütun için varsayılan değerleri belirtir.  Özellikteki varsayılan değerler, veri ambarında varsayılan kısıtlama kümesinin üzerine yazılır ve kimlik sütununda varsayılan bir değer olamaz. | Hayır |
| ekSeçenekler | DOĞRUDAN [COPY deyiminde](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest)"With" yan tümcesinde SQL DW COPY deyimine geçirilecek ek seçenekler. COPY deyimi gereksinimleriyle uyumlu olması için gereken değeri alıntılayın. | Hayır |

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


## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="getmetadata-activity-properties"></a>Metaveri etkinlik özelliklerini alın

Özellikler hakkında daha fazla bilgi edinmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) kontrol edin 

## <a name="data-type-mapping-for-azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı için veri türü eşleme

## <a name="mapping-data-flow-properties"></a>Veri akışı özelliklerini eşleme

Veri akışını eşlemede verileri dönüştürürken, Azure Synapse Analytics'ten tablolara okuma ve yazma yapabilirsiniz. Daha fazla bilgi için, veri akışlarını eşlemedeki [kaynak dönüşümü](data-flow-source.md) ve [lavabo dönüşümüne](data-flow-sink.md) bakın.

### <a name="source-transformation"></a>Kaynak dönüşümü

Azure Synapse Analytics'e özgü ayarlar kaynak dönüşümün **Kaynak Seçenekleri** sekmesinde kullanılabilir. 

**Giriş:** Kaynağınızı bir masaya (eşdeğeri) ```Select * from <table-name>```mi yoksa özel bir SQL sorgusu mu gireceğinizi seçin.

**Sorgu**: Giriş alanında Sorgu'yu seçerseniz, kaynağınız için bir SQL sorgusu girin. Bu ayar, veri kümesinde seçtiğiniz tüm tabloyu geçersiz kılar. **Sipariş By** yan tümceleri burada desteklenmez, ancak tam bir SELECT FROM deyimi ayarlayabilirsiniz. Kullanıcı tanımlı tablo işlevlerini de kullanabilirsiniz. **select * from udfGetData()** SQL'de bir tablo döndüren bir UDF'dir. Bu sorgu, veri akışınızda kullanabileceğiniz bir kaynak tablo üretir. Sorguları kullanmak, sınama veya arama yapmak için satırları azaltmak için de harika bir yoldur. 

* SQL Örneği:```Select * from MyTable where customerId > 1000 and customerId < 2000```

**Toplu iş boyutu**: Büyük verileri okumalara parçalamak için bir toplu iş boyutu girin. Veri akışlarında, ADF bu ayarı Spark sütun kapacını ayarlamak için kullanır. Bu, boş bırakılırsa Spark varsayılanlarını kullanacak bir seçenek alanıdır.

**İzolasyon Düzeyi**: Veri akışını eşlemedeki SQL kaynakları için varsayılan değer taahhütsüz olarak okunur. Buradaki yalıtım düzeyini aşağıdaki değerlerden biriyle değiştirebilirsiniz:
* Taahhüt'i Okuyun
* Okunmamış oku
* Tekrarlanabilir Okuma
* Serileştirilebilir
* Yok (yalıtım düzeyini yoksay)

![Yalıtım Düzeyi](media/data-flow/isolationlevel.png "Yalıtım Düzeyi")

### <a name="sink-transformation"></a>Lavabo dönüşümü

Azure Synapse Analytics'e özgü ayarlar, lavabo dönüşümünün **Ayarlar** sekmesinde kullanılabilir.

**Güncelleme yöntemi:** Veritabanı hedefinizde hangi işlemlere izin verileni belirler. Varsayılan yalnızca eklere izin vermektir. Satırları güncelleştirmek, yükseltmek veya silmek için, bu eylemler için satırları etiketlemek için bir alter-row dönüşümü gerekir. Güncelleştirmeler, ekler ve silmeler için, hangi satırı değiştireceğini belirlemek için bir anahtar sütunu veya sütun ayarlanmalıdır.

**Tablo eylemi:** Yazmadan önce tüm satırları n için yeniden oluşturup oluşturmayacağını veya kaldırıp kaldırmayacağını belirler.
* Yok: Tabloya hiçbir işlem yapılmaz.
* Yeniden oluşturma: Tablo bırakılır ve yeniden oluşturulur. Dinamik olarak yeni bir tablo oluşturuyorsa gereklidir.
* Truncate: Hedef tablodaki tüm satırlar kaldırılır.

**Evreleme etkinleştirme:** Azure Synapse Analytics'e yazarken [PolyBase'in](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-ver15) kullanılıp kullanılmayacağını belirler

**Toplu iş boyutu**: Her kovaya kaç satır yazıldığını denetler. Daha büyük toplu iş boyutları sıkıştırma ve bellek optimizasyonunu artırır, ancak verileri önbelleğe alırken bellek özel durumları riski.

**Öncesi ve Sonrası SQL komut dosyaları**: Lavabo veritabanınıza veri yazılmadan önce (ön işleme) ve sonra (işlem sonrası) verileri yürütecek çok satırlı SQL komut dosyalarını girin

![öncesi ve sonrası SQL işleme komut dosyaları](media/data-flow/prepost1.png "SQL işleme komut dosyaları")

## <a name="data-type-mapping-for-azure-synapse-analytics"></a>Azure Synapse Analytics için veri türü eşleme

Verileri Azure Synapse Analytics'ten veya Azure Synapse Analytics'e kopyaladiğinizde, Azure Synapse Analytics veri türlerinden Azure Veri Fabrikası geçici veri türlerine aşağıdaki eşlemeler kullanılır. Copy Activity'in kaynak şemasını ve veri türünü lavaboyla nasıl eşleştirdiğini öğrenmek için [şemaya](copy-activity-schema-and-type-mapping.md) ve veri türü haritasına bakın.

>[!TIP]
>SQL DW desteklenen veri türleri ve desteklenmeyenlerin geçici çözümleri yle ilgili [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-data-types.md) makalesinde Tablo veri türlerine bakın.

| Azure Synapse Analytics veri türü    | Veri Fabrikası geçici veri türü |
| :------------------------------------ | :----------------------------- |
| bigint                                | Int64                          |
| ikili                                | Bayt[]                         |
| bit                                   | Boole                        |
| char                                  | Dize, Char[]                 |
| date                                  | DateTime                       |
| Tarih saat                              | DateTime                       |
| datetime2                             | DateTime                       |
| Datetimeoffset                        | DateTimeOffset                 |
| Ondalık                               | Ondalık                        |
| FILESTREAM özniteliği (varbinary(max)) | Bayt[]                         |
| Kayan                                 | Çift                         |
| image                                 | Bayt[]                         |
| int                                   | Int32                          |
| Para                                 | Ondalık                        |
| Nchar                                 | Dize, Char[]                 |
| sayısal                               | Ondalık                        |
| nvarchar                              | Dize, Char[]                 |
| gerçek                                  | Tek                         |
| Rowversion                            | Bayt[]                         |
| Smalldatetime                         | DateTime                       |
| smallint                              | Int16                          |
| Smallmoney                            | Ondalık                        |
| time                                  | TimeSpan                       |
| tinyint                               | Bayt                           |
| uniqueidentifier                      | Guid                           |
| Varbinary                             | Bayt[]                         |
| varchar                               | Dize, Char[]                 |

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'nda Kopyalama Etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için [desteklenen veri depolarına ve biçimlere](copy-activity-overview.md#supported-data-stores-and-formats)bakın.
