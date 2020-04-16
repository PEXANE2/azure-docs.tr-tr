---
title: Azure SQL Veritabanı'nda verileri kopyalama ve dönüştürme
description: Azure SQL Veritabanı'na ve Azure SQL Veritabanı'ndan verileri nasıl kopyalayarak dönüştüreceklerini ve Azure Veri Fabrikası'nı kullanarak Azure SQL Veritabanı'ndaki verileri nasıl dönüştüreceklerini öğrenin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/12/2020
ms.openlocfilehash: db55e685fb50c89eb850e1b9ee9dcf13d20fb614
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417531"
---
# <a name="copy-and-transform-data-in-azure-sql-database-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak Azure SQL Veritabanı'ndaki verileri kopyalama ve dönüştürme

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Veri Fabrikası sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-azure-sql-connector.md)
> * [Geçerli sürüm](connector-azure-sql-database.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, Azure Veri Fabrikası'ndaki Kopyalama Etkinliği'nin Azure SQL Veritabanı'ndan ve Azure SQL Veritabanı'ndan kopyalanması ve Azure SQL Veritabanı'ndaki verileri dönüştürmek için Veri Akışı'nın nasıl kullanılacağı açıklanmaktadır. Azure Veri Fabrikası hakkında bilgi edinmek için [giriş makalesini](introduction.md)okuyun.

## <a name="supported-capabilities"></a>Desteklenen yetenekler

Bu Azure SQL Veritabanı bağlayıcısı aşağıdaki etkinlikler için desteklenir:

- [Desteklenen kaynak/lavabo matris](copy-activity-overview.md) tablosu yla [etkinliği](copy-activity-overview.md) kopyalama
- [Veri akışını eşleme](concepts-data-flow-overview.md)
- [Arama etkinliği](control-flow-lookup-activity.md)
- [Meta veri etkinliğini alın](control-flow-get-metadata-activity.md)

Kopyalama etkinliği için bu Azure SQL Veritabanı bağlayıcısı şu işlevleri destekler:

- SQL kimlik doğrulaması ve Azure Etkin Dizini (Azure AD) Uygulama belirteç kimlik doğrulaması kullanarak azure kaynakları için bir hizmet yöneticisi veya yönetilen kimliklerle verileri kopyalama.
- Kaynak olarak, bir SQL sorgusu veya depolanan yordam ı kullanarak veri alma.
- Lavabo olarak, verileri hedef tabloya eklemek veya kopya sırasında özel mantıkla depolanan yordamı çağırmak.

>[!NOTE]
>Azure SQL Veritabanı [Her Zaman Şifrelenmiş](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current) artık bu bağlayıcı tarafından desteklenmez. Geçici olarak çalışmak için, kendi barındırılan tümleştirme çalışma süresi aracılığıyla genel bir [ODBC bağlayıcısı](connector-odbc.md) ve SQL Server ODBC sürücüsü kullanabilirsiniz. ODBC sürücü indirme ve bağlantı dize yapılandırmaları ile [bu kılavuzu](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-current) izleyin.

> [!IMPORTANT]
> Azure Veri Fabrikası tümleştirme çalışma zamanını kullanarak verileri kopyalarsanız, Azure hizmetlerinin sunucuya erişebilmeleri için bir [Azure SQL Server güvenlik duvarı](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) yapılandırın.
> Kendi kendine barındırılan bir tümleştirme çalışma zamanı kullanarak verileri kopyalarsanız, Azure SQL Server güvenlik duvarını uygun IP aralığına izin verecek şekilde yapılandırın. Bu aralık, makinenin Azure SQL Veritabanı'na bağlanmak için kullanılan IP'sini içerir.

## <a name="get-started"></a>başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümlerde, Azure SQL Veritabanı bağlayıcısına özgü Azure Veri Fabrikası varlıklarını tanımlamak için kullanılan özellikler hakkında ayrıntılar sağlanmaktadır.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Bu özellikler, Azure SQL Veritabanı'na bağlı bir hizmet için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | **Tür** özelliği **AzureSqlDatabase**olarak ayarlanmalıdır. | Evet |
| Connectionstring | **ConnectionString** özelliği için Azure SQL Veritabanı örneğine bağlanmak için gereken bilgileri belirtin. <br/>Azure Key Vault'a parola veya hizmet temel anahtarı da koyabilirsiniz. SQL kimlik doğrulamasıysa, yapılandırmayı `password` bağlantı dizesinin dışına çekin. Daha fazla bilgi için, Azure Anahtar [Kasası'ndaki](store-credentials-in-key-vault.md)tablo yu ve Mağaza kimlik bilgilerini izleyen JSON örneğine bakın. | Evet |
| hizmetPrincipalId | Uygulamanın istemci kimliğini belirtin. | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulaması kullandığınızda |
| servicePrincipalKey | Uygulamanın anahtarını belirtin. Bu alanı Azure Veri Fabrikası'nda güvenli bir şekilde depolamak için **SecureString** olarak işaretleyin veya [Azure Key Vault'ta depolanan bir gizli ye başvuruyapın.](store-credentials-in-key-vault.md) | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulaması kullandığınızda |
| Kiracı | Uygulamanızın bulunduğu alan adı veya kiracı kimliği gibi kiracı bilgilerini belirtin. Azure portalının sağ üst köşesinde fareyi gezdirerek alın. | Evet, bir hizmet ilkesiyle Azure AD kimlik doğrulaması kullandığınızda |
| connectVia | Bu [tümleştirme çalışma süresi](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılır. Veri mağazanız özel bir ağda bulunuyorsa Azure tümleştirme çalışma zamanını veya kendi kendine barındırılan tümleştirme çalışma zamanını kullanabilirsiniz. Belirtilmemişse, varsayılan Azure tümleştirme çalışma zamanı kullanılır. | Hayır |

Farklı kimlik doğrulama türleri için, sırasıyla önkoşullar ve JSON örnekleri yle ilgili aşağıdaki bölümlere bakın:

- [SQL kimlik doğrulaması](#sql-authentication)
- [Azure AD uygulama belirteç kimlik doğrulaması: Hizmet sorumlusu](#service-principal-authentication)
- [Azure AD uygulama belirteç kimlik doğrulaması: Azure kaynakları için yönetilen kimlikler](#managed-identity)

>[!TIP]
>"UserErrorFailedToConnectToSqlServer" hata kodu ve "Veritabanı için oturum sınırı XXX ve ulaşıldı" gibi bir `Pooling=false` iletiyle bir hata yaptıysanız, bağlantı dizenize ekleyin ve yeniden deneyin.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

#### <a name="linked-service-example-that-uses-sql-authentication"></a>SQL kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

**Azure Anahtar Kasasında Parola** 

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

Hizmet temeli tabanlı Azure AD uygulama belirteç kimlik doğrulaması kullanmak için aşağıdaki adımları izleyin:

1. Azure portalından [bir Azure Etkin Dizin uygulaması oluşturun.](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application) Uygulama adını ve bağlantılı hizmeti tanımlayan aşağıdaki değerleri not edin:

    - Uygulama Kimliği
    - Uygulama anahtarı
    - Kiracı Kimliği

2. Azure SQL Sunucunuz için Azure Sql Server'ınız için azure portalında [bir Azure Active Directory yöneticisi sağlama.](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) Azure AD yöneticisi bir Azure REKLAM kullanıcısı veya Azure REKLAM grubu olmalıdır, ancak bir hizmet ilkesi olamaz. Bu adım, bir sonraki adımda hizmet sorumlusu için bir veritabanı kullanıcısı oluşturmak için bir Azure AD kimliği kullanabilmeniz için yapılır.

3. Hizmet ilkesi için [veritabanı kullanıcıları içeren oluşturun.](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) EN AZ KULLANICI İZNİ KULLANABİlEN bir Azure AD kimliğine sahip SQL Server Management Studio gibi araçları kullanarak verileri kopyalamak istediğiniz veritabanına bağlanın. Aşağıdaki T-SQL'i çalıştırın: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. Normalde SQL kullanıcıları veya başkaları için yaptığınız gibi hizmet ilkesine gereken izinleri ver. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için [bu belgeye](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017)bakın.

    ```sql
    ALTER ROLE [role name] ADD MEMBER [your application name];
    ```

5. Azure Veri Fabrikası'nda Azure SQL Veritabanı bağlantılı bir hizmeti yapılandırın.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Hizmet temel kimlik doğrulaması kullanan bağlantılı hizmet örneği

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

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Azure kaynakları kimlik doğrulaması için yönetilen kimlikler

Bir veri fabrikası, belirli veri fabrikasını temsil eden Azure kaynakları için yönetilen bir [kimlikle](data-factory-service-identity.md) ilişkilendirilebilir. Bu yönetilen kimliği Azure SQL Veritabanı kimlik doğrulaması için kullanabilirsiniz. Atanan fabrika, bu kimliği kullanarak verilere erişebilir ve veritabanınızdan erişebilir ve verileri kopyalayabilir.

Yönetilen kimlik doğrulamasını kullanmak için aşağıdaki adımları izleyin.

1. Azure SQL Sunucunuz için Azure Sql Server'ınız için azure portalında [bir Azure Active Directory yöneticisi sağlama.](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) Azure AD yöneticisi bir Azure AD kullanıcısı veya Azure REKLAM grubu olabilir. Yönetilen kimliğe sahip gruba yönetici rolü verirseniz, 3 ve 4 adımlarını atlayın. Yöneticiveritabanına tam erişime sahiptir.

2. Azure Veri Fabrikası yönetilen kimliği için [veritabanı kullanıcıları oluşturun.](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) EN AZ KULLANICI İZNİ KULLANABİlEN bir Azure AD kimliğine sahip SQL Server Management Studio gibi araçları kullanarak verileri kopyalamak istediğiniz veritabanına bağlanın. Aşağıdaki T-SQL'i çalıştırın: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. Veri Fabrikası yönetilen kimlik normalde SQL kullanıcıları ve diğerleri için yaptığınız gibi gerekli izinleri verin. Aşağıdaki kodu çalıştırın. Daha fazla seçenek için [bu belgeye](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017)bakın.

    ```sql
    ALTER ROLE [role name] ADD MEMBER [your Data Factory name];
    ```

4. Azure Veri Fabrikası'nda Azure SQL Veritabanı bağlantılı bir hizmeti yapılandırın.

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

Veri kümelerini tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) 

Azure SQL Veritabanı veri kümesi için aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesinin **tür** özelliği **AzureSqlTable**olarak ayarlanmalıdır. | Evet |
| Şema | Şema adı. |Kaynak için hayır, lavabo için Evet  |
| tablo | Tablonun/görünümün adı. |Kaynak için hayır, lavabo için Evet  |
| tableName | Şema ile tablo/görünüm adı. Bu özellik geriye dönük uyumluluk için desteklenir. Yeni iş yükü `schema` `table`için, kullanım ve . | Kaynak için hayır, lavabo için Evet |

#### <a name="dataset-properties-example"></a>Dataset özellikleri örneği

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

Etkinlikleri tanımlamak için kullanılabilen bölümlerin ve özelliklerin tam listesi için [bkz.](concepts-pipelines-activities.md) Bu bölümde, Azure SQL Veritabanı kaynağı ve lavabotarafından desteklenen özelliklerin bir listesi bulunmaktadır.

### <a name="azure-sql-database-as-the-source"></a>Kaynak olarak Azure SQL Veritabanı

Azure SQL Veritabanı'ndaki verileri kopyalamak için, kopyalama etkinliği **kaynak** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinlik kaynağının **tür** özelliği **AzureSqlSource**olarak ayarlanmalıdır. "SqlSource" türü hala geriye dönük uyumluluk için desteklenir. | Evet |
| sqlReaderQuery | Bu özellik, verileri okumak için özel SQL sorgusunu kullanır. `select * from MyTable` bunun bir örneğidir. | Hayır |
| sqlReaderStoredProcedureNameName | Kaynak tablodaki verileri okuyan depolanan yordamın adı. Son SQL deyimi, depolanan yordamda bir SELECT deyimi olmalıdır. | Hayır |
| depolanmışProsedürParametreleri | Depolanan yordam için parametreler.<br/>İzin verilen değerler ad veya değer çiftleridir. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. | Hayır |
| ısolationlevel | SQL kaynağı için hareket kilitleme davranışını belirtir. İzin verilen değerler şunlardır: **ReadCommitted** (varsayılan), **ReadUncommitted**, **Tekrarlanabilir Oku**, **Serializable**, **Anlık Görüntü**. Daha fazla bilgi için [bu dokümana](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) bakın. | Hayır |

**Dikkat edilmesi gereken noktalar:**

- **SqlReaderQuery** **AzureSqlSource**için belirtilirse, kopyalama etkinliği verileri almak için bu sorguyu Azure SQL Veritabanı kaynağına göre çalıştırır. Ayrıca **sqlReaderStoredProcedureName** ve **storedProcedureParametreleri** belirterek depolanan yordamı belirtebilirsiniz.
- **SqlReaderQuery** veya **sqlReaderStoredProcedureName**belirtmezseniz, JSON veri kümesinin "yapı" bölümünde tanımlanan sütunlar sorgu oluşturmak için kullanılır. Sorgu `select column1, column2 from mytable` Azure SQL Veritabanı'na karşı çalışır. Veri kümesi tanımında "yapı" yoksa, tüm sütunlar tablodan seçilir.

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

### <a name="stored-procedure-definition"></a>Depolanan yordam tanımı

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

### <a name="azure-sql-database-as-the-sink"></a>Lavabo olarak Azure SQL Veritabanı

> [!TIP]
> Azure SQL Veritabanı'na veri yüklemek için En İyi [Uygulama'dan](#best-practice-for-loading-data-into-azure-sql-database)desteklenen yazma davranışları, yapılandırmalar ve en iyi uygulamalar hakkında daha fazla bilgi edinin.

Verileri Azure SQL Veritabanı'na kopyalamak için, kopyalama etkinliği **lavabosu** bölümünde aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği lavabonun **türü** özelliği **AzureSqlSink**olarak ayarlanmalıdır. "SqlSink" türü hala geriye dönük uyumluluk için desteklenir. | Evet |
| yazmaBatchSize | *Toplu iş başına*SQL tablosuna eklenecek satır sayısı.<br/> İzin verilen **değer, tümseci** (satır sayısı) olarak bilinir. Varsayılan olarak, Azure Veri Fabrikası satır boyutuna bağlı olarak uygun toplu iş boyutunu dinamik olarak belirler. | Hayır |
| yazmaBatchTimeout | Toplu ekleme işleminin zaman dolmadan bitmeden tamamlanması için bekleme süresi.<br/> İzin verilen değer **zaman ası'** dır. Bir örnek "00:30:00" (30 dakika). | Hayır |
| preCopyScript | Verileri Azure SQL Veritabanı'na yazmadan önce çalışacak kopyalama etkinliği için bir SQL sorgusu belirtin. Kopya başına yalnızca bir kez çağrılır. Önceden yüklenmiş verileri temizlemek için bu özelliği kullanın. | Hayır |
| sqlWriterStoredProcedureNameName | Kaynak verilerin hedef tabloya nasıl uygulanacağı konusunda tanımlanan depolanan yordamın adı. <br/>Bu depolanan yordam *toplu iş başına çağrılır.* Yalnızca bir kez çalışan ve kaynak verilerle hiçbir ilgisi olmayan işlemler için , `preCopyScript` örneğin silme veya bulanın, özelliği kullanın. | Hayır |
| depolanmışProcedureTableTypeParameterName |Depolanan yordamda belirtilen tablo türünün parametre adı.  |Hayır |
| sqlWriterTableType |Depolanan yordamda kullanılacak tablo türü adı. Kopyalama etkinliği, verilerin bu tablo türüyle geçici bir tabloda kullanılabilir hale getirir. Depolanan yordam kodu, daha sonra kopyalanan verileri varolan verilerle birleştirilebilir. |Hayır |
| depolanmışProsedürParametreleri |Depolanan yordam için parametreler.<br/>İzin verilen değerler ad ve değer çiftleridir. Parametrelerin adları ve kasası, depolanan yordam parametrelerinin adları ve kasalarıyla eşleşmelidir. | Hayır |
| Tableoption | Kaynak şemaya göre yoksa, lavabo tablosunun otomatik olarak oluşturulup oluşturulmayacağını belirtir. Otomatik tablo oluşturma, lavabo saklı yordamı belirtir veya sahnelenen kopya kopyalama etkinliğinde yapılandırılır desteklenmez. İzin verilen `none` değerler şunlardır: (varsayılan), `autoCreate`. |Hayır |
| devre dışı KalımMetricsCollection | Veri Fabrikası, kopya performans optimizasyonu ve öneriler için Azure SQL Database DTUs gibi ölçümler toplar. Bu davranışla ilgileniyorsanız, `true` kapatmak için belirtin. | Hayır (varsayılan) `false` |

**Örnek 1: Ek veri**

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

## <a name="best-practice-for-loading-data-into-azure-sql-database"></a>Azure SQL Veritabanına veri yüklemek için en iyi yöntem

Verileri Azure SQL Veritabanı'na kopyalarken, farklı yazma davranışı gerektirebilir:

- [Ek](#append-data): Kaynak verilerimde yalnızca yeni kayıtlar var.
- [Upsert](#upsert-data): Kaynak verilerimde hem ekler hem de güncelleştirmeler vardır.
- [Overwrite](#overwrite-the-entire-table): Ben her zaman bir boyut tablosu nu yeniden yüklemek istiyorum.
- [Özel mantıkla yazın](#write-data-with-custom-logic): Hedef tabloya son eklemeden önce ekstra işleme ihtiyacım var.

Azure Veri Fabrikası'nda ve en iyi uygulamalarda nasıl yapılandırılabilenlerle ilgili ilgili bölümlere bakın.

### <a name="append-data"></a>Veri ekle

Veri ekle, bu Azure SQL Veritabanı lavabo bağlayıcısının varsayılan davranışıdır. Azure Veri Fabrikası, tablonuza verimli bir şekilde yazmak için toplu bir ekleme yapar. Kaynağı kopya etkinliğinde buna göre yapılandırabilir ve bekleyebilirsiniz.

### <a name="upsert-data"></a>Verileri upsert etme

**Seçenek 1:** Kopyalamanız gereken büyük miktarda veri varsa, bir yükseltme yapmak için aşağıdaki yaklaşımı kullanın: 

- İlk olarak, kopyalama etkinliğini kullanarak tüm kayıtları toplu yüklemek için bir [veritabanı kapsamlı geçici tablo](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current#database-scoped-global-temporary-tables-azure-sql-database) kullanın. Veritabanı kapsamına giren geçici tablolara yönelik işlemler günlüğe kaydolmadığından, milyonlarca kaydı saniyeler içinde yükleyebilirsiniz.
- [Birleştirme](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) veya EKLE/UPDATE deyimi uygulamak için Azure Veri Fabrikası'nda depolanan yordam etkinliği çalıştırın. Tüm güncelleştirmeleri veya eklertek bir işlem olarak gerçekleştirmek için kaynak olarak geçici tabloyu kullanın. Bu şekilde, gidiş-dönüş ve günlük işlemlerinin sayısı azalır. Depolanan yordam etkinliğinin sonunda, geçici tablo bir sonraki yükseltme döngüsüne hazır olması için kesilir.

Örnek olarak, Azure Veri Fabrikası'nda, **Depolanmış Yordam etkinliğiyle**zincirlenmiş bir **Kopyalama etkinliği** içeren bir ardışık işlem hattı oluşturabilirsiniz. Kaynak deponuzdaki eski verileri, örneğin veri kümesindeki tablo adı olarak **##UpsertTempTable**gibi bir Azure SQL Veritabanı geçici tablosuna kopyalar. Daha sonra ikincisi, geçici tablodaki kaynak verileri hedef tabloya birleştirmek ve geçici tabloyu temizlemek için depolanmış bir yordam çağırır.

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

**Seçenek 2:** Ayrıca [kopyalama etkinliği içinde depolanan yordamı çağırmayı](#invoke-a-stored-procedure-from-a-sql-sink)da seçebilirsiniz. Bu yaklaşım, kopyalama etkinliğinde varsayılan `writeBatchSize` yaklaşım olarak toplu ekleme kullanmak yerine kaynak tablodaki her bir toplu işlemi (özellik tarafından yönetilir şekilde) çalıştırır.

### <a name="overwrite-the-entire-table"></a>Tüm tablonun üzerine yazma

CopyScript özelliğini **preCopyScript** kopyalama etkinliği lavabosunda yapılandırabilirsiniz. Bu durumda, çalışan her kopyalama etkinliği için önce Azure Veri Fabrikası komut dosyasını çalıştırın. Sonra verileri eklemek için kopya çalışır. Örneğin, en son verilerle tüm tablonun üzerine yazmak için, yeni verileri kaynaktan toplu olarak yüklemeden önce tüm kayıtları silmek için bir komut dosyası belirtin.

### <a name="write-data-with-custom-logic"></a>Özel mantıkla veri yazma

Özel mantıkla veri yazma [adımları, Upsert veri](#upsert-data) bölümünde açıklananlara benzer. Büyük ölçekli için, hedef tabloya kaynak verilerin son eklenmesinden önce ekstra işleme uygulamanız gerektiğinde, iki şeyden birini yapabilirsiniz:

- Geçici tablo kapsamına giren bir veritabanına yükleyin ve ardından depolanan yordamı çağırın. 
- Kopyalama sırasında depolanan yordamı çağırın.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a>SQL lavabodan depolanan yordamı çağırma

Verileri Azure SQL Veritabanı'na kopyaladiğinizde, kaynak tablonun her bir partisinde ek parametreleriçeren kullanıcı tarafından belirtilen bir depoyordamyı yapılandırabilir ve çağırabilirsiniz. Depolanan yordam özelliği [tablo değerindeki parametrelerden](https://msdn.microsoft.com/library/bb675163.aspx)yararlanır.

Yerleşik kopyalama mekanizmaları amaca hizmet etmiyorsa depolanan yordamı kullanabilirsiniz. Kaynak verilerin hedef tabloya son ekinden önce ek işleme uygulamak istediğinizde buna bir örnektir. Bazı ekstra işleme örnekleri, sütunları birleştirmek, ek değerleraramak ve birden fazla tabloya eklemek istediğinizde verilebilir.

Aşağıdaki örnek, Azure SQL Veritabanı'ndaki bir tabloya bir yükseltme yapmak için depolanan yordamın nasıl kullanılacağını gösterir. Giriş verilerinin ve lavabo **Pazarlama** tablosunun her birinin üç sütunu olduğunu varsayalım: **ProfileID,** **Durum**ve **Kategori**. Yükseltmeyi **ProfileID** sütununa göre yapın ve yalnızca "ProductA" adı verilen belirli bir kategori için uygulayın.

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

Veri akışını eşlemede verileri dönüştürürken, Azure SQL Veritabanı'ndan tablolara okuma ve yazma yapabilirsiniz. Daha fazla bilgi için, veri akışlarını eşlemedeki [kaynak dönüşümü](data-flow-source.md) ve [lavabo dönüşümüne](data-flow-sink.md) bakın.

### <a name="source-transformation"></a>Kaynak dönüşümü

Azure SQL Veritabanına özgü ayarlar kaynak dönüştürmenin **Kaynak Seçenekleri** sekmesinde kullanılabilir. 

**Giriş:** Kaynağınızı bir masaya (eşdeğeri) ```Select * from <table-name>```mi yoksa özel bir SQL sorgusu mu gireceğinizi seçin.

**Sorgu**: Giriş alanında Sorgu'yu seçerseniz, kaynağınız için bir SQL sorgusu girin. Bu ayar, veri kümesinde seçtiğiniz tüm tabloyu geçersiz kılar. **Sipariş By** yan tümceleri burada desteklenmez, ancak tam bir SELECT FROM deyimi ayarlayabilirsiniz. Kullanıcı tanımlı tablo işlevlerini de kullanabilirsiniz. **select * from udfGetData()** SQL'de bir tablo döndüren bir UDF'dir. Bu sorgu, veri akışınızda kullanabileceğiniz bir kaynak tablo üretir. Sorguları kullanmak, sınama veya arama yapmak için satırları azaltmak için de harika bir yoldur. 

* SQL Örneği:```Select * from MyTable where customerId > 1000 and customerId < 2000```

**Toplu iş boyutu**: Büyük verileri okumalara parçalamak için bir toplu iş boyutu girin.

**İzolasyon Düzeyi**: Veri akışını eşlemedeki SQL kaynakları için varsayılan değer taahhütsüz olarak okunur. Buradaki yalıtım düzeyini aşağıdaki değerlerden biriyle değiştirebilirsiniz:
* Taahhüt'i Okuyun
* Okunmamış oku
* Tekrarlanabilir Okuma
* Serileştirilebilir
* Yok (yalıtım düzeyini yoksay)

![Yalıtım Düzeyi](media/data-flow/isolationlevel.png "Yalıtım Düzeyi")

### <a name="sink-transformation"></a>Lavabo dönüşümü

Azure SQL Veritabanı'na özgü ayarlar, lavabo dönüştürmenin **Ayarlar** sekmesinde kullanılabilir.

**Güncelleme yöntemi:** Veritabanı hedefinizde hangi işlemlere izin verileni belirler. Varsayılan yalnızca eklere izin vermektir. Satırları güncelleştirmek, yükseltmek veya silmek için, bu eylemler için satırları etiketlemek için bir alter-row dönüşümü gerekir. Güncelleştirmeler, ekler ve silmeler için, hangi satırı değiştireceğini belirlemek için bir anahtar sütunu veya sütun ayarlanmalıdır.

![Anahtar Sütunlar](media/data-flow/keycolumn.png "Anahtar Sütunlar")

Burada anahtar olarak seçtiğiniz sütun adı, sonraki güncelleştirme, yukarı ekleme, silme nin bir parçası olarak ADF tarafından kullanılacaktır. Bu nedenle, Lavabo eşleme var bir sütun seçmeniz gerekir. Bu anahtar sütuna değer yazmak istemiyorsanız, "Anahtar sütunları atla"yı tıklatın.

**Tablo eylemi:** Yazmadan önce tüm satırları n için yeniden oluşturup oluşturmayacağını veya kaldırıp kaldırmayacağını belirler.
* Yok: Tabloya hiçbir işlem yapılmaz.
* Yeniden oluşturma: Tablo bırakılır ve yeniden oluşturulur. Dinamik olarak yeni bir tablo oluşturuyorsa gereklidir.
* Truncate: Hedef tablodaki tüm satırlar kaldırılır.

**Toplu iş boyutu**: Her kovaya kaç satır yazıldığını denetler. Daha büyük toplu iş boyutları sıkıştırma ve bellek optimizasyonunu artırır, ancak verileri önbelleğe alırken bellek özel durumları riski.

**Öncesi ve Sonrası SQL komut dosyaları**: Lavabo veritabanınıza veri yazılmadan önce (ön işleme) ve sonra (işlem sonrası) verileri yürütecek çok satırlı SQL komut dosyalarını girin

![öncesi ve sonrası SQL işleme komut dosyaları](media/data-flow/prepost1.png "SQL işleme komut dosyaları")

## <a name="data-type-mapping-for-azure-sql-database"></a>Azure SQL Veritabanı için veri türü eşleme

Veriler Azure SQL Veritabanı'ndan veya Azure SQL Veritabanı'na kopyalandığında, Aşağıdaki eşlemeler Azure SQL Veritabanı veri türlerinden Azure Veri Fabrikası geçici veri türlerine kadar kullanılır. Kopyalama etkinliğinin kaynak şemasını ve veri türünü lavaboyla nasıl eşlenebildiğini öğrenmek için Bkz. [Şema ve veri türü eşlemeleri.](copy-activity-schema-and-type-mapping.md)

| Azure SQL Veritabanı veri türü | Azure Veri Fabrikası geçici veri türü |
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
| tinyint |Bayt |
| uniqueidentifier |Guid |
| Varbinary |Bayt[] |
| varchar |Dize, Char[] |
| xml |Xml |

>[!NOTE]
> Ondalık geçici türüyle eşalan veri türleri için, şu anda Azure Veri Fabrikası 28'e kadar hassasiyeti destekler. 28'den büyük hassas verileriniz varsa, SQL sorgusunda bir dize dönüştürmeyi düşünün.

## <a name="lookup-activity-properties"></a>Arama etkinlik özellikleri

Özellikler hakkında daha fazla bilgi edinmek için [Arama etkinliğini](control-flow-lookup-activity.md)kontrol edin.

## <a name="getmetadata-activity-properties"></a>Metaveri etkinlik özelliklerini alın

Özellikler hakkında daha fazla bilgi edinmek için [GetMetadata etkinliğini](control-flow-get-metadata-activity.md) kontrol edin 

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası'ndaki kopyalama etkinliği tarafından kaynak ve lavabo olarak desteklenen veri depolarının listesi için desteklenen [veri depolarına bakın.](copy-activity-overview.md#supported-data-stores-and-formats)
