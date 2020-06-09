---
title: SQL isteğe bağlı olarak OPENROWSET kullanma (Önizleme)
description: Bu makalede, SQL isteğe bağlı (Önizleme) için OPENROWSET sözdizimi açıklanmakta ve bağımsız değişkenlerin nasıl kullanılacağı açıklanmaktadır.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 05/07/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 9c2a2d7059e24b37b0f47d0b568a3929f296d8c6
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84560863"
---
# <a name="how-to-use-openrowset-with-sql-on-demand-preview"></a>OPENROWSET 'yi isteğe bağlı SQL ile kullanma (Önizleme)

`OPENROWSET(BULK...)`İşlevi, Azure Storage 'daki dosyalara erişmenizi sağlar. `OPENROWSET`işlev, uzak bir veri kaynağının (örneğin dosya) içeriğini okur ve içeriği bir dizi satır olarak döndürür. SQL isteğe bağlı (Önizleme) kaynağı içinde, OPENROWSET işlevi çağırarak ve toplu seçeneği belirtilerek OPENROWSET toplu satır kümesi sağlayıcısına erişilir.  

İşlevine, bir `OPENROWSET` `FROM` tablo adı gibi bir sorgunun yan tümcesinde başvurulabilir `OPENROWSET` . Bir dosyadaki verilerin bir satır kümesi olarak okunmasını ve döndürülmesini sağlayan yerleşik bir toplu sağlayıcı aracılığıyla toplu işlemleri destekler.

## <a name="data-source"></a>Veri kaynağı

SYNAPSE SQL içindeki OPENROWSET işlevi, bir veri kaynağından dosya içeriğini okur. Veri kaynağı bir Azure Storage hesabıdır ve işlevde açıkça başvurulabilir `OPENROWSET` ya da okumak istediğiniz dosyaların URL 'sinden dinamik olarak çıkarsanamıyor.
`OPENROWSET`İşlevi, isteğe bağlı olarak `DATA_SOURCE` , dosyaları içeren veri kaynağını belirtmek için bir parametre içerebilir.
- `OPENROWSET`olmadan `DATA_SOURCE` , bu dosya içeriğini seçenek olarak BELIRTILEN URL konumundan doğrudan okumak için kullanılabilir `BULK` :

    ```sql
    SELECT *
    FROM OPENROWSET(BULK 'http://storage..../container/folder/*.parquet',
                    TYPE = 'PARQUET') AS file
    ```

Bu, önceden yapılandırma olmadan dosyaların içeriğini okumak için hızlı ve kolay bir yoldur. Bu seçenek, depolama alanına erişmek için temel kimlik doğrulama seçeneğini kullanmanıza olanak sağlar (Azure AD oturum açmaları için Azure AD geçişi ve SQL oturum açma için SAS belirteci). 

- `OPENROWSET``DATA_SOURCE`, ile belirtilen depolama hesabındaki dosyalara erişmek için kullanılabilir:

    ```sql
    SELECT *
    FROM OPENROWSET(BULK '/folder/*.parquet',
                    DATA_SOURCE='storage', --> Root URL is in LOCATION of DATA SOURCE
                    TYPE = 'PARQUET') AS file
    ```


    Bu seçenek, veri kaynağındaki depolama hesabının konumunu yapılandırmanıza ve depolamaya erişmek için kullanılması gereken kimlik doğrulama yöntemini belirtmenize olanak sağlar. 
    
    > [!IMPORTANT]
    > `OPENROWSET`olmadan `DATA_SOURCE` , depolama dosyalarına erişmek için hızlı ve kolay bir yol sağlar, ancak sınırlı kimlik doğrulama seçenekleri sunar. Örnek olarak, Azure AD sorumlusu yalnızca [Azure AD kimliklerini](develop-storage-files-storage-access-control.md?tabs=user-identity#force-azure-ad-pass-through) kullanarak dosyalara erişebilir ve genel kullanıma açık dosyalara erişemez. Daha güçlü kimlik doğrulama seçeneklerine ihtiyacınız varsa, `DATA_SOURCE` seçeneğini kullanın ve depolama alanına erişmek için kullanmak istediğiniz kimlik bilgisini tanımlayın.


## <a name="security"></a>Güvenlik

Bir veritabanı kullanıcısının `ADMINISTER BULK OPERATIONS` işlevini kullanma izni olması gerekir `OPENROWSET` .

Depolama Yöneticisi ayrıca, bir kullanıcının geçerli SAS belirteci sağlayarak veya Azure AD sorumlusunu depolama dosyalarına erişmesine olanak tanıyarak dosyalara erişmesini sağlamalıdır. [Bu makaledeki](develop-storage-files-storage-access-control.md)depolama erişimi denetimi hakkında daha fazla bilgi edinin.

`OPENROWSET`depolama alanının kimliğini nasıl doğrulayacağınızı öğrenmek için aşağıdaki kuralları kullanın:
- `OPENROWSET` `DATA_SOURCE` Kimlik doğrulama mekanizması içinde, arayan türüne bağlıdır.
  - Azure depolama, Azure AD Kullanıcı 'nın temel dosyalara erişmesine izin veriyorsa (örneğin, arayan, depolama üzerinde depolama okuyucusu iznine sahipse) ve SYNAPSE SQL hizmetinde [Azure AD PASSTHROUGH kimlik doğrulamasını etkinleştirirseniz](develop-storage-files-storage-access-control.md#force-azure-ad-pass-through) , Azure AD oturum açmaları yalnızca kendı [Azure AD kimliklerini](develop-storage-files-storage-access-control.md?tabs=user-identity#supported-storage-authorization-types) kullanarak dosyalara erişebilir.
  - SQL oturumları `OPENROWSET` `DATA_SOURCE` , genel kullanıma açık dosyalara erişmek için olmadan da KULLANABILIR, SAS belirteci kullanılarak korunan dosyalar veya SYNAPSE çalışma alanının yönetilen kimliği. Depolama dosyalarına erişim izni vermek için [sunucu kapsamlı kimlik bilgileri oluşturmanız](develop-storage-files-storage-access-control.md#examples) gerekir. 
- İçindeki `OPENROWSET` `DATA_SOURCE` kimlik doğrulama mekanizması, başvurulan veri kaynağına atanan veritabanı kapsamlı kimlik bilgileri içinde tanımlanmıştır. Bu seçenek, genel olarak kullanılabilir depolamaya erişmenizi veya SAS belirtecini, çalışma alanının yönetilen kimliğini veya [arayanın Azure AD kimliğini](develop-storage-files-storage-access-control.md?tabs=user-identity#supported-storage-authorization-types) (arayan Azure AD sorumlusu ise) kullanarak depolamaya erişmenizi sağlar. `DATA_SOURCE`Ortak olmayan Azure Storage 'a başvuruyorsa, depolama dosyalarına erişime izin vermek için, [veritabanı kapsamlı kimlik bilgileri oluşturmanız](develop-storage-files-storage-access-control.md#examples) ve içinde başvurulmasına gerek duyarsınız `DATA SOURCE` .

Çağıranın, `REFERENCES` depolamada kimlik doğrulaması yapmak için kimlik bilgisi üzerinde izni olması gerekir.

## <a name="syntax"></a>Söz dizimi

```syntaxsql
--OPENROWSET syntax for reading Parquet files
OPENROWSET  
( { BULK 'unstructured_data_path' , [DATA_SOURCE = <data source name>, ]
    FORMAT='PARQUET' }  
)  
[WITH ( {'column_name' 'column_type' }) ]
[AS] table_alias(column_alias,...n)

--OPENROWSET syntax for reading delimited text files
OPENROWSET  
( { BULK 'unstructured_data_path' , [DATA_SOURCE = <data source name>, ] 
    FORMAT = 'CSV'
    [ <bulk_options> ] }  
)  
WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })  
[AS] table_alias(column_alias,...n)
 
<bulk_options> ::=  
[ , FIELDTERMINATOR = 'char' ]    
[ , ROWTERMINATOR = 'char' ] 
[ , ESCAPE_CHAR = 'char' ] 
[ , FIRSTROW = 'first_row' ]     
[ , FIELDQUOTE = 'quote_characters' ]
[ , DATA_COMPRESSION = 'data_compression_method' ]
[ , PARSER_VERSION = 'parser_version' ]
```

## <a name="arguments"></a>Bağımsız değişkenler

Sorgulamak için hedef verileri içeren giriş dosyaları için iki seçeneğiniz vardır. Geçerli değerler:

- ' CSV '-satır/sütun ayırıcılarına sahip tüm sınırlandırılmış metin dosyalarını Içerir. Herhangi bir karakter, TSV: FIELDSONLANDıRıCı = sekmesi gibi bir alan ayırıcısı olarak kullanılabilir.

- ' PARQUET '-Parquet biçiminde Ikili dosya 

**' unstructured_data_path '**

Verilerin yolunu oluşturan unstructured_data_path mutlak veya göreli bir yol olabilir:
- ' \<prefix> ://' Biçimindeki mutlak yol, \<storage_account_path> / \<storage_path> bir kullanıcının dosyaları doğrudan okumasını sağlar.
- ' <storage_path> ' biçimindeki göreli yol, parametresiyle kullanılması gerekir `DATA_SOURCE` ve <storage_account_path> konum içindeki dosya modelini tanımlar `EXTERNAL DATA SOURCE` . 

 Aşağıda, <storage account path> belirli dış veri kaynağınıza bağlanacak ilgili değerleri bulacaksınız. 

| Dış veri kaynağı       | Ön ek | Depolama hesabı yolu                                 |
| -------------------------- | ------ | ---------------------------------------------------- |
| Azure Blob Depolama         | https  | \<storage_account>.blob.core.windows.net             |
| Azure Data Lake Store Gen1 | https  | \<storage_account>. azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Store Gen2 | https  | \<storage_account>. dfs.core.windows.net              |
||||

'\<storage_path>'

 Depolama alanınızı, okumak istediğiniz klasörü veya dosyayı işaret eden bir yolu belirtir. Yol bir kapsayıcıya veya klasöre işaret ediyorsa, tüm dosyalar söz konusu kapsayıcı veya klasörden okunacaktır. Alt klasörlerdeki dosyalar dahil değildir. 

 Birden çok dosya veya klasörü hedeflemek için joker karakterler kullanabilirsiniz. Ardışık olmayan birden çok joker karakter kullanımına izin verilir.
Aşağıda, */CSV/popülasyonu*ile başlayan tüm klasörlerden *popülasyon* ile başlayan tüm *CSV* dosyalarını okuyan bir örnek verilmiştir:  
`https://sqlondemandstorage.blob.core.windows.net/csv/population*/population*.csv`

Bir klasör olarak unstructured_data_path belirtirseniz, bir SQL isteğe bağlı sorgusu bu klasörden dosyaları alır. 

> [!NOTE]
> Hadoop ve PolyBase 'in aksine, SQL isteğe bağlı alt klasörler döndürmez. Ayrıca, Hadoop ve PolyBase 'den farklı olarak, SQL isteğe bağlı, dosya adının altı çizili (_) veya nokta (.) ile başladığı dosyaları döndürür.

Aşağıdaki örnekte, unstructured_data_path = ise `https://mystorageaccount.dfs.core.windows.net/webdata/` , BIR SQL isteğe bağlı sorgusu, mydata. txt ve _Hidden. txt ' den satırları döndürür. Bir alt klasörde bulunduğundan mydata2. txt ve mydata3. txt döndürmez.

![Dış tablolar için özyinelemeli veriler](./media/develop-openrowset/folder-traversal.png)

`[WITH ( {'column_name' 'column_type' [ 'column_ordinal'] }) ]`

WıTH yan tümcesi, dosyalardan okumak istediğiniz sütunları belirtmenize olanak tanır.

- CSV veri dosyaları için tüm sütunları okumak üzere sütun adlarını ve bunların veri türlerini belirtin. Sütunların bir alt kümesini isterseniz, kaynak veri dosyalarından sütunları sıralı olarak seçmek için sıralı numaraları kullanın. Sütunlar sıra atamağına göre bağlanacaktır. 

    > [!IMPORTANT]
    > WıTH yan tümcesi CSV dosyaları için zorunludur.
    >
    
- Parquet veri dosyaları için, kaynak veri dosyalarındaki sütun adlarıyla eşleşen sütun adları sağlayın. Sütunlar ada göre bağlanacaktır. WıTH yan tümcesi atlanırsa, Parquet dosyalarındaki tüm sütunlar döndürülür.

çıkış sütununun adı column_name. Sağlanmışsa, bu ad kaynak dosyadaki sütun adını geçersiz kılar.

column_type = çıkış sütunu için veri türü. Örtük veri türü dönüştürmesi burada gerçekleşmeyecektir.

column_ordinal = kaynak dosya (lar) daki sütunun sıra numarası. Bağlama ada göre yapıldığından, bu bağımsız değişken Parquet dosyaları için yok sayılır. Aşağıdaki örnek yalnızca bir CSV dosyasından ikinci bir sütun döndürür:

```sql
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
)
```

**\<bulk_options>**

FIELDSONLANDıRıCı = ' field_terminator '

Kullanılacak alan sonlandırıcıyı belirtir. Varsayılan alan Sonlandırıcı virgüldür ("**,**").

ROWSONLANDıRıCı = ' row_terminator ' '

Kullanılacak satır sonlandırıcıyı belirtir. Satır Sonlandırıcı belirtilmemişse, varsayılan Sonlandırıcıların biri kullanılacaktır. PARSER_VERSION = ' 1,0 ' için varsayılan sonlandırıcılar \r\n, \n ve \r. PARSER_VERSION = ' 2,0 ' için varsayılan sonlandırıcılar \r\n ve \n

ESCAPE_CHAR = ' Char '

Dosyanın kendisini ve dosyadaki tüm sınırlayıcı değerlerini kaçış için kullanılan karakteri belirtir. Kaçış karakterinin kendisi dışında bir değer veya sınırlayıcı değerlerinden herhangi biri gelmesi durumunda, değer okunurken kaçış karakteri bırakılır. 

ESCAPE_CHAR parametresi, FIELDQUOTE 'un etkin olup olmamasından bağımsız olarak uygulanır. Tırnak işareti karakterini atlamak için kullanılmaz. Tırnak işareti karakteri, Excel CSV davranışına göre hizalamayla birlikte çift tırnak işareti karakteriyle sonuçlanır.

FIRSTROW = ' first_row ' 

Yüklenecek ilk satırın numarasını belirtir. Varsayılan değer 1'dir. Bu, belirtilen veri dosyasındaki ilk satırı belirtir. Satır numaraları, satır sonlandırıcıları sayarak belirlenir. FIRSTROW 1 tabanlıdır.

FIELDQUOTE = ' field_quote ' 

CSV dosyasında tırnak karakteri olarak kullanılacak bir karakter belirtir. Belirtilmezse, tırnak karakteri (") kullanılır. 

DATA_COMPRESSION = ' data_compression_method '

Sıkıştırma yöntemini belirtir. Aşağıdaki sıkıştırma yöntemi destekleniyor:

- org. Apache. Hadoop. IO. compress. GzipCodec

PARSER_VERSION = ' parser_version '

Dosyalar okunurken kullanılacak ayrıştırıcı sürümünü belirtir. Şu anda desteklenen CSV ayrıştırıcı sürümleri 1,0 ve 2,0:

- PARSER_VERSION = ' 1,0 '
- PARSER_VERSION = ' 2,0 '

CSV Ayrıştırıcısı sürüm 1,0, varsayılan ve özellik bakımından zengin olmakla, 2,0 performans için oluşturulmuştur ve tüm seçenekleri ve kodlamaları desteklemez. 

CSV Ayrıştırıcısı sürüm 2,0 özellikleri:

- Tüm veri türleri desteklenmez.
- En büyük satır boyutu sınırı 8 MB 'tır.
- Şu seçenekler desteklenmez: DATA_COMPRESSION.
- Tırnak işareti boş dize ("") boş dize olarak yorumlanır.

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, population*. csv dosyalarından yalnızca 1 ve 4 sıralı numaralarına sahip iki sütun döndürür. Dosyalarda üstbilgi satırı olmadığından, ilk satırdan okumaya başlar:

```sql
SELECT * 
FROM OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/csv/population/population*.csv',
        FORMAT = 'CSV',
        FIRSTROW = 1
    )
WITH (
    [country_code] VARCHAR (5) COLLATE Latin1_General_BIN2 1,
    [population] bigint 4
) AS [r]
```

Aşağıdaki örnek, sütun adları ve veri türleri belirtmeden Parquet biçimindeki görselleştirmenizdeki veri kümesindeki ilk satırın tüm sütunlarını döndürür: 

```sql
SELECT 
    TOP 1 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
        FORMAT='PARQUET'
    ) AS [r]
```

Dosyaların listelenmediğinden ilgili bir hata alıyorsanız, SYNAPSE SQL isteğe bağlı olarak genel depolamaya erişimi etkinleştirmeniz gerekir:
- SQL oturumu kullanıyorsanız, [genel depolamaya erişime izin veren sunucu kapsamlı kimlik bilgileri oluşturmanız](develop-storage-files-storage-access-control.md#examples)gerekir.
- Genel depolamaya erişmek için bir Azure AD sorumlusu kullanıyorsanız, genel depolamaya erişime izin veren ve [Azure AD PASSTHROUGH kimlik doğrulamasını](develop-storage-files-storage-access-control.md#disable-forcing-azure-ad-pass-through)devre dışı bırakan [sunucu kapsamlı kimlik bilgileri oluşturmanız](develop-storage-files-storage-access-control.md#examples) gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek için bkz. [CSV](query-single-csv-file.md), [Parquet](query-parquet-files.md)ve [JSON](query-json-files.md) dosya biçimlerini okumak için ' OPENROWSET ' i nasıl kullanacağınızı öğrenmek için [Query Data Storage hızlı](query-data-storage.md) başlangıcı. Ayrıca, [Cetas](develop-tables-cetas.md)kullanarak sorgunuzun sonuçlarını Azure depolama 'ya kaydetmeyi de öğrenebilirsiniz.
