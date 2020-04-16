---
title: Azure Veri Gölü Depolama'dan öğretici yük verileri
description: Synapse SQL için Azure Veri Gölü Depolama'dan veri yüklemek için PolyBase dış tablolarını kullanın.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416108"
---
# <a name="load-data-from-azure-data-lake-storage-for-sql-analytics"></a>SQL Analytics için Azure Veri Gölü Depolama'dan veri yükleme

Bu kılavuz, Azure Veri Gölü Depolama'dan veri yüklemek için PolyBase dış tablolarının nasıl kullanılacağını özetleyçalışır. Veri Gölü Depolama'da depolanan verilerüzerinde geçici sorgular çalıştırabilirsiniz, ancak en iyi performans için verileri almanızı öneririz.

> [!NOTE]  
> Yüklemeye alternatif olarak, şu anda genel önizlemede bulunan [COPY deyimi](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) dir.  COPY deyimi en fazla esnekliği sağlar. COPY bildirimi hakkında geri bildirim sağlamak için aşağıdaki dağıtım sqldwcopypreview@service.microsoft.comlistesine bir e-posta gönderin: .
>
> [!div class="checklist"]
>
> * Veri Gölü Depolama'dan yüklemek için gereken veritabanı nesneleri oluşturun.
> * Veri Gölü Depolama dizinine bağlanın.
> * Verileri veri ambarına yükleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticiye başlamadan önce, [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.

Bu öğreticiyi çalıştırmak için şunları yapmanız gerekir:

* BIR SQL havuzu. Bkz. [BIR SQL havuzu ve sorgu verileri oluşturun.](create-data-warehouse-portal.md)
* Veri Gölü Depolama hesabı. Bkz. [Azure Veri Gölü Depolama'ya başlayın.](../../data-lake-store/data-lake-store-get-started-portal.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) Bu depolama hesabı için, yüklemek için aşağıdaki kimlik bilgilerini yapılandırmanız veya belirtmeniz gerekir: Depolama hesabı anahtarı, Azure Dizin Uygulaması kullanıcısı veya depolama hesabına uygun RBAC rolüne sahip bir AAD kullanıcısı.

## <a name="create-a-credential"></a>Kimlik bilgisi oluşturma

AAD geçişini kullanarak kimlik doğrulaması yaparken bu bölümü atlayabilir ve "Dış veri kaynağı oluşturma" bölümüne devam edebilirsiniz. AAD geçişini kullanırken oluşturulacak veya belirtilmesi gereken bir veritabanı kimlik bilgisi gerekmez, ancak AAD kullanıcınızın depolama hesabına uygun RBAC rolüne (Depolama Blob Veri Okuyucusu, Katkıda Bulunan Veya Sahip Rolü) sahip olduğundan emin olun. Daha fazla bilgi [burada](https://techcommunity.microsoft.com/t5/Azure-SQL-Data-Warehouse/How-to-use-PolyBase-by-authenticating-via-AAD-pass-through/ba-p/862260)özetlenmiştir.

Veri Gölü Depolama hesabınıza erişmek için kimlik bilgilerinizi şifrelemek için bir Veritabanı Ana Anahtarı oluşturmanız gerekir. Daha sonra sırrınızı depolamak için bir Veritabanı Kapsamı Lı Kimlik Bilgileri oluşturursunuz. Veritabanı Kapsamı Kimlik Bilgileri, hizmet ilkeleri (Azure Dizin Uygulaması kullanıcısı) kullanılarak kimlik doğrulaması yaparken, AAD'de ayarlanan hizmet temel kimlik bilgilerini depolar. Gen2 için depolama hesabı anahtarını depolamak için Veritabanı Kapsamı Lı Kimlik Belgesi'ni de kullanabilirsiniz.

Hizmet ilkelerini kullanarak Veri Gölü Depolama'ya bağlanmak için **önce** bir Azure Etkin Dizin Uygulaması oluşturmanız, bir erişim anahtarı oluşturmanız ve uygulamaya Veri Gölü Depolama hesabına erişim izni vermeniz gerekir. Yönergeler için bkz: [Etkin Dizin kullanarak Azure Veri Gölü Depolamasına Kimlik Doğrulaması.](../../data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)

CONTROL düzeyi izinlerine sahip bir kullanıcıyla SQL havuzunuza giriş yapın ve veritabanınıza karşı aşağıdaki SQL deyimlerini çalıştırın:

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

Verilerin konumunu depolamak için bu [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) komutunu kullanın. AAD geçişi ile kimlik doğrulaması yapıyorsunuzsa, Kimlik Kartı parametresi gerekli değildir. Hizmet bitiş noktaları için Yönetilen Kimlik'i kullanarak kimlik doğrulaması yapıyorsunuz, dış veri kaynağını ayarlamak için bu [belgeleri](../../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#azure-sql-data-warehouse-polybase) izleyin.

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

## <a name="configure-data-format"></a>Veri biçimini yapılandırma

Verileri Veri Gölü Depolama'dan almak için Dış Dosya Biçimini belirtmeniz gerekir. Bu nesne, dosyaların Veri Gölü Depolama'da nasıl yazıldığını tanımlar.
Tam liste için, T-SQL belgelerimize bakın [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)

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

## <a name="create-the-external-tables"></a>Dış Tablolar Oluşturma

Artık veri kaynağını ve dosya biçimini belirttiğinize göre, dış tabloları oluşturmaya hazırsınız. Dış tablolar, dış verilerle nasıl etkileşimde olduğunu. Konum parametresi bir dosya veya dizin belirtebilir. Bir dizin belirtirse, dizindeki tüm dosyalar yüklenir.

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

## <a name="external-table-considerations"></a>Dış Tablo Hususlar

Harici bir tablo oluşturmak kolaydır, ancak tartışılması gereken bazı nüanslar vardır.

Dış Tablolar güçlü bir şekilde dizilir. Bu, yutulan verilerin her satırının tablo şeması tanımını karşılaması gerektiği anlamına gelir.
Bir satır şema tanımıyla eşleşmiyorsa, satır yükten reddedilir.

REJECT_TYPE ve REJECT_VALUE seçenekleri, son tabloda kaç satır veya verilerin yüzdesinin bulunması gerektiğini tanımlamanıza olanak sağlar. Yük sırasında, ret değerine ulaşılırsa, yük başarısız olur. Reddedilen satırların en yaygın nedeni şema tanımı uyumsuzluğudur. Örneğin, dosyadaki veriler bir dize olduğunda bir sütun yanlış int şema verilirse, her satır yüklemek için başarısız olur.

Veri Gölü Depolama Gen1, verilere erişimi denetlemek için Role Based Access Control (RBAC) kullanır. Bu, Hizmet Yöneticisinin konum parametresi ve son dizin ve dosyaların çocuklarına tanımlanan dizinlere okuma izinleri olması gerektiği anlamına gelir. Bu, PolyBase'in bu verileri doğrulamasını ve yüklemesini sağlar.

## <a name="load-the-data"></a>Verileri yükleme

Veri Gölü Depolama'dan veri yüklemek için [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) deyimini kullanın.

CTAS yeni bir tablo oluşturur ve onu seçili bir deyimin sonuçlarıyla doldurur. CTAS, yeni tabloyu seçifadenin sonuçlarıyla aynı sütunlara ve veri türlerine sahip olacak şekilde tanımlar. Harici bir tablodaki tüm sütunları seçerseniz, yeni tablo dış tablodaki sütunların ve veri türlerinin bir kopyasıdır.

Bu örnekte, Dış Tablomuzdan DimProduct adlı karma dağıtılmış bir tablo oluşturuyoruz DimProduct_external.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```

## <a name="optimize-columnstore-compression"></a>Sütun deposu sıkıştırmayı optimize edin

Varsayılan olarak, tablolar kümelenmiş sütun deposu dizini olarak tanımlanır. Yükleme tamamlandıktan sonra, bazı veri satırları sütun deposuna sıkıştırılmamış olabilir.  Bunun olmasının çeşitli nedenleri var. Daha fazla bilgi edinmek için [sütun mağazası dizinlerini yönet'e](sql-data-warehouse-tables-index.md)bakın.

Bir yükten sonra sorgu performansını ve sütun mağazası sıkıştırmasını en iyi duruma getirmek için, sütun deposu dizini tüm satırları sıkıştırmaya zorlamak için tabloyu yeniden oluşturun.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>İstatistikleri optimize edin

Yüklemeden hemen sonra tek sütunlu istatistikler oluşturmak en iyisidir. İstatistikler için bazı seçenekler vardır. Örneğin, her sütunda tek sütunlu istatistikler oluşturursanız, tüm istatistikleri yeniden oluşturmak uzun sürebilir. Belirli sütunların sorgu yüklemlerinde olmayacağını biliyorsanız, bu sütunlar üzerinde istatistik oluşturmayı atlayabilirsiniz.

Her tablonun her sütununda tek sütunlu istatistikler oluşturmaya karar verirseniz, `prc_sqldw_create_stats` [istatistik](sql-data-warehouse-tables-statistics.md) makalesinde depolanan yordam kodu örneğini kullanabilirsiniz.

Aşağıdaki örnek, istatistik oluşturmak için iyi bir başlangıç noktasıdır. Boyut tablosundaki her sütunda ve olgu tablolarında her birleştirme sütununda tek sütunlu istatistikler oluşturur. Daha sonra diğer olgu tablosu sütunlarına her zaman tek veya çok sütunlu istatistikleri ekleyebilirsiniz.

## <a name="achievement-unlocked"></a>Başarı kilidi!

Veri ambarınıza başarılı bir şekilde veri yüklediniz. Harika iş!

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Veri Gölü Depolama Gen1'de depolanan verilerin yapısını tanımlamak için harici tablolar oluşturdunuz ve ardından veri ambarınıza veri yüklemek için PolyBase CREATE TABLE AS SELECT deyimini kullandınız.

Şu işlemleri yaptınız:
> [!div class="checklist"]
>
> * Veri Gölü Depolama'dan yüklemek için gereken veritabanı nesneleri oluşturuldu.
> * Veri Gölü Depolama dizinine bağlı.
> * Veri ambarına veri yüklendi.
>

Veri yükleme, Azure Synapse Analytics'i kullanarak bir veri ambarı çözümü geliştirmenin ilk adımıdır. Geliştirme kaynaklarımıza göz atın.

> [!div class="nextstepaction"]
> [Veri depolama için tablolar nasıl geliştireceklerini öğrenin](sql-data-warehouse-tables-overview.md)
