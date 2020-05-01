---
title: Eğitim Azure Data Lake Storage verileri yükleme
description: SYNAPSE SQL için Azure Data Lake Storage 'den veri yüklemek için PolyBase dış tabloları kullanın.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/08/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 9713d73ee132f743ceea98cbaca6a83f36fd3a45
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81416108"
---
# <a name="load-data-from-azure-data-lake-storage-for-sql-analytics"></a>SQL Analytics için Azure Data Lake Storage verileri yükleme

Bu kılavuzda, Azure Data Lake Storage verileri yüklemek için PolyBase dış tablolarının nasıl kullanılacağı özetlenmektedir. Data Lake Storage depolanan verilerde geçici sorgular çalıştırabilmenize karşın, en iyi performans için verilerin içeri aktarılmasını öneririz.

> [!NOTE]  
> Yükleme alternatifi Şu anda genel önizleme aşamasında olan [Copy deyimidir](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) .  COPY ifadesinde en çok esneklik sağlanır. COPY ifadesiyle geri bildirim sağlamak için şu dağıtım listesine bir e-posta gönderin: sqldwcopypreview@service.microsoft.com.
>
> [!div class="checklist"]
>
> * Data Lake Storage yüklemek için gereken veritabanı nesneleri oluşturun.
> * Data Lake Storage dizinine bağlanın.
> * Verileri veri ambarına yükleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticiye başlamadan önce, [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.

Bu öğreticiyi çalıştırmak için şunlar gerekir:

* Bir SQL Havuzu. Bkz. [SQL havuzu oluşturma ve verileri sorgulama](create-data-warehouse-portal.md).
* Data Lake Storage hesabı. Bkz. [Azure Data Lake Storage kullanmaya başlama](../../data-lake-store/data-lake-store-get-started-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json). Bu depolama hesabında, yüklemek için aşağıdaki kimlik bilgilerinden birini yapılandırmanız veya belirtmeniz gerekir: depolama hesabı anahtarı, Azure dizin uygulama kullanıcısı veya depolama hesabına uygun RBAC rolüne sahip bir AAD kullanıcısı.

## <a name="create-a-credential"></a>Kimlik bilgisi oluşturma

Bu bölümü atlayabilir ve AAD geçişli geçiş kullanarak kimlik doğrularken "dış veri kaynağını oluşturma" işlemine geçebilirsiniz. AAD geçişi kullanılırken bir veritabanı kapsamlı kimlik bilgisinin oluşturulması veya belirtilmesi gerekmez, ancak AAD kullanıcılarınızın depolama hesabına uygun RBAC rolüne (Depolama Blobu veri okuyucusu, katkıda bulunanlar veya Owner rolü) sahip olduğundan emin olun. Daha fazla ayrıntı [burada](https://techcommunity.microsoft.com/t5/Azure-SQL-Data-Warehouse/How-to-use-PolyBase-by-authenticating-via-AAD-pass-through/ba-p/862260)özetlenmiştir.

Data Lake Storage hesabınıza erişmek için, kimlik bilgileri gizli anahtarını şifrelemek üzere bir veritabanı ana anahtarı oluşturmanız gerekir. Daha sonra gizli dizinizi depolamak için bir veritabanı kapsamlı kimlik bilgisi oluşturursunuz. Hizmet sorumlularını (Azure Directory Uygulama kullanıcısı) kullanarak kimlik doğrulaması yaparken, veritabanı kapsamlı kimlik bilgileri AAD 'de ayarlanan hizmet sorumlusu kimlik bilgilerini depolar. Gen2 için depolama hesabı anahtarını depolamak üzere veritabanı kapsamlı kimlik bilgilerini de kullanabilirsiniz.

Hizmet sorumlularını kullanarak Data Lake Storage bağlanmak için, **önce** bir Azure Active Directory uygulaması oluşturmanız, bir erişim anahtarı oluşturmanız ve uygulamaya Data Lake Storage hesabına erişim vermeniz gerekir. Yönergeler için bkz. [Active Directory kullanarak Azure Data Lake Storage kimlik doğrulama](../../data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

DENETIM düzeyi izinlerine sahip bir kullanıcıyla SQL havuzunuza oturum açın ve aşağıdaki SQL deyimlerini veritabanınıza yürütün:

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://docs.microsoft.com/sql/t-sql/statements/create-master-key-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest

CREATE MASTER KEY;


-- B (for service principal authentication): Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://docs.microsoft.com/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    -- Always use the OAuth 2.0 authorization endpoint (v1)
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- B (for Gen2 storage key authentication): Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.

CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;

-- It should look something like this when authenticating using service principal:
CREATE DATABASE SCOPED CREDENTIAL ADLSCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```

## <a name="create-the-external-data-source"></a>Dış veri kaynağını oluşturma

Verilerin konumunu depolamak için bu [dış VERI kaynağı oluştur](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) komutunu kullanın. AAD geçişiyle kimlik doğrulaması yapıyorsanız, KIMLIK BILGISI parametresi gerekli değildir. Hizmet uç noktaları için yönetilen kimliği kullanarak kimlik doğrulaması yapıyorsanız, dış veri kaynağını ayarlamak için bu [belgeleri](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#azure-sql-data-warehouse-polybase) izleyin.

```sql
-- C (for Gen1): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen1 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<datalakestoregen1accountname>.azuredatalakestore.net',
    CREDENTIAL = ADLSCredential
);

-- C (for Gen2): Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Storage.
-- LOCATION: Provide Data Lake Storage Gen2 account name and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStorage
WITH (
    TYPE = HADOOP,
    LOCATION='abfs[s]://<container>@<AzureDataLake account_name>.dfs.core.windows.net', -- Please note the abfss endpoint for when your account has secure transfer enabled
    CREDENTIAL = ADLSCredential
);
```

## <a name="configure-data-format"></a>Veri biçimini Yapılandır

Data Lake Storage verileri içeri aktarmak için dış dosya biçimini belirtmeniz gerekir. Bu nesne, dosyaların Data Lake Storage nasıl yazıldığını tanımlar.
Tam liste için T-SQL belgelerimize [dış dosya biçimi oluşturma](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ' ya bakın

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store missing values as default for datatype.

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-external-tables"></a>Dış tabloları oluşturma

Artık veri kaynağını ve dosya biçimini belirtolduğunuza göre, dış tabloları oluşturmaya hazırsınız demektir. Dış tablolar, dış verilerle etkileşime girirsiniz. Konum parametresi, bir dosya veya dizin belirtebilir. Bir dizin belirtiyorsa, dizin içindeki tüm dosyalar yüklenir.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the Data Lake Storage root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStorage
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Dış tablo konuları

Dış tablo oluşturma kolaydır, ancak ele almanız gereken bazı bildirimler vardır.

Dış tablolar kesin olarak türdedir. Bu, çağrılan verilerin her satırının tablo şeması tanımını karşılaması gerektiği anlamına gelir.
Bir satır şema tanımıyla eşleşmezse, satır yük tarafından reddedilir.

REJECT_TYPE ve REJECT_VALUE seçenekleri, son tabloda kaç satır veya veri yüzdesinin mevcut olması gerektiğini tanımlamanızı sağlar. Yük sırasında reddetme değerine ulaşıldığında, yükleme başarısız olur. Reddedilen satırların en yaygın nedeni, bir şema tanımı uyuşmazlığıdır. Örneğin, bir sütun, dosyadaki verilerin bir dize olduğu zaman int şemasına yanlış verildiyse, her satır yükleme başarısız olur.

Data Lake Storage 1., verilere erişimi denetlemek için rol tabanlı Access Control (RBAC) kullanır. Bu, hizmet sorumlusunun konum parametresinde tanımlanan dizinlerde ve son dizin ve dosyaların alt öğelerinde okuma izinlerinin olması gerektiği anlamına gelir. Bu, PolyBase 'in kimlik doğrulamasını ve verileri yüklemesini sağlar.

## <a name="load-the-data"></a>Verileri yükleme

Data Lake Storage verileri yüklemek için [Create Table Select (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ifadesini kullanın.

CTAS yeni bir tablo oluşturur ve bunu bir SELECT ifadesinin sonuçlarıyla doldurur. CTAS, yeni tabloyu SELECT ifadesinin sonuçlarıyla aynı sütunlara ve veri türlerine sahip olacak şekilde tanımlar. Dış tablodaki tüm sütunları seçerseniz, yeni tablo dış tablodaki sütunların ve veri türlerinin bir çoğaltmasıdır.

Bu örnekte, dış Tablomızdan DimProduct adlı bir karma dağıtılmış tablo oluşturacağız DimProduct_external.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```

## <a name="optimize-columnstore-compression"></a>Columnstore sıkıştırmasını iyileştirme

Tablolar, varsayılan olarak kümelenmiş bir columnstore dizini olarak tanımlanır. Yükleme tamamlandıktan sonra, bazı veri satırları columnstore ' de sıkıştırılmayabilir.  Bunun gerçekleşebileceği çeşitli nedenler vardır. Daha fazla bilgi için bkz. [columnstore dizinlerini yönetme](sql-data-warehouse-tables-index.md).

Bir yüklemeden sonra sorgu performansını ve columnstore sıkıştırmasını iyileştirmek için, columnstore dizinini tüm satırları sıkıştırmak üzere zorlamak için tabloyu yeniden derleyin.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>İstatistikleri iyileştirme

Bir yükden hemen sonra tek sütunlu istatistikler oluşturmak en iyisidir. İstatistikler için bazı seçimler vardır. Örneğin, her sütunda tek sütunlu istatistikler oluşturursanız tüm istatistiklerin yeniden oluşturulması uzun sürebilir. Bazı sütunların sorgu koşullarına sahip olmadığını biliyorsanız, bu sütunlarda istatistik oluşturmayı atlayabilirsiniz.

Her tablonun her sütununda tek sütunlu istatistikler oluşturmaya karar verirseniz, `prc_sqldw_create_stats` [istatistik](sql-data-warehouse-tables-statistics.md) makalesindeki saklı yordam kodu örneğini kullanabilirsiniz.

Aşağıdaki örnek, istatistik oluşturmak için iyi bir başlangıç noktasıdır. Boyut tablosundaki her bir sütunda ve olgu tablolarındaki her bir birleştirme sütununda tek sütunlu istatistikler oluşturur. Daha sonra, daha sonra diğer olgu tablosu sütunlarına tek veya çok sütunlu istatistikler ekleyebilirsiniz.

## <a name="achievement-unlocked"></a>Başarı kilidi açıldı!

Verileri veri ambarınıza başarıyla yüklesahipsiniz. Harika iş!

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Data Lake Storage 1. depolanan verilerin yapısını tanımlamak için dış tablolar oluşturdunuz ve sonra veri ambarınıza veri yüklemek için PolyBase CREATE TABLE AS SELECT ifadesini kullandınız.

Şu işlemleri yaptınız:
> [!div class="checklist"]
>
> * Data Lake Storage yüklemek için gereken veritabanı nesneleri oluşturuldu.
> * Bir Data Lake Storage dizinine bağlanıldı.
> * Veriler veri ambarına yüklendi.
>

Verilerin yüklenmesi, Azure SYNAPSE Analytics kullanılarak veri ambarı çözümü geliştirmenin ilk adımıdır. Geliştirme kaynaklarımıza göz atın.

> [!div class="nextstepaction"]
> [Veri depolama için tablo geliştirmeyi öğrenin](sql-data-warehouse-tables-overview.md)
