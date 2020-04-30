---
title: SQL isteğe bağlı olarak OPENROWSET kullanma (Önizleme)
description: Bu makalede, SQL isteğe bağlı (Önizleme) için OPENROWSET sözdizimi açıklanmakta ve bağımsız değişkenlerin nasıl kullanılacağı açıklanmaktadır.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 6325d5555b01373b148dce69731ec64896d6e1fd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81680501"
---
# <a name="how-to-use-openrowset-with-sql-on-demand-preview"></a>OPENROWSET 'yi isteğe bağlı SQL ile kullanma (Önizleme)

OPENROWSET (toplu...) işlevi, Azure Storage 'daki dosyalara erişmenize olanak tanır. SQL isteğe bağlı (Önizleme) kaynağı içinde, OPENROWSET işlevi çağırarak ve toplu seçeneği belirtilerek OPENROWSET toplu satır kümesi sağlayıcısına erişilir.  

OPENROWSET işlevine, bir sorgunun FROM yan tümcesinde bir tablo adı OPENROWSET gibi başvurulabilir. Bir dosyadaki verilerin bir satır kümesi olarak okunmasını ve döndürülmesini sağlayan yerleşik bir toplu sağlayıcı aracılığıyla toplu işlemleri destekler.

OPENROWSET Şu anda SQL havuzunda desteklenmiyor.

## <a name="syntax"></a>Sözdizimi

```syntaxsql
--OPENROWSET syntax for reading Parquet files
OPENROWSET  
( { BULK 'unstructured_data_path' , 
    FORMAT='PARQUET' }  
)  
[WITH ( {'column_name' 'column_type' }) ]
[AS] table_alias(column_alias,...n)

--OPENROWSET syntax for reading delimited text files
OPENROWSET  
( { BULK 'unstructured_data_path' , 
    FORMAT = 'CSV'
    [ <bulk_options> ] }  
)  
WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })  
[AS] table_alias(column_alias,...n)
 
<bulk_options> ::=  
[ , FIELDTERMINATOR = 'char' ]    
[ , ROWTERMINATOR = 'char' ] 
[ , ESCAPE_CHAR = 'char' ] 
[ , FIRSTROW = 'first_row'  ]     
[ , FIELDQUOTE = 'quote_characters']
```

## <a name="arguments"></a>Bağımsız Değişkenler

Sorgulamak için hedef verileri içeren giriş dosyaları için iki seçeneğiniz vardır. Geçerli değerler:

- ' CSV '-satır/sütun ayırıcılarına sahip tüm sınırlandırılmış metin dosyalarını Içerir. Herhangi bir karakter, TSV: FIELDSONLANDıRıCı = sekmesi gibi bir alan ayırıcısı olarak kullanılabilir.

- ' PARQUET '-Parquet biçiminde Ikili dosya 

**' unstructured_data_path '**

Verilerin yolunu oluşturan unstructured_data_path aşağıdaki gibi yapılandırılır:  
'\<önek>://\<storage_account_path>/\<storage_path> '
 
 
 Aşağıda, belirli dış veri kaynağınıza bağlanacak ilgili depolama hesabı yolları bulacaksınız. 

| Dış veri kaynağı       | Ön ek | Depolama hesabı yolu                                 |
| -------------------------- | ------ | ---------------------------------------------------- |
| Azure Blob Depolama         | https  | \<storage_account>. blob.core.windows.net             |
| Azure Data Lake Store Gen1 | https  | \<storage_account>. azuredatalakestore.net/webhdfs/v1 |
| Azure Data Lake Store Gen2 | https  | \<storage_account>. dfs.core.windows.net              |
||||

'\<storage_path> '

 Depolama alanınızı, okumak istediğiniz klasörü veya dosyayı işaret eden bir yolu belirtir. Yol bir kapsayıcıya veya klasöre işaret ediyorsa, tüm dosyalar söz konusu kapsayıcı veya klasörden okunacaktır. Alt klasörlerdeki dosyalar dahil değildir. 
 
 Birden çok dosya veya klasörü hedeflemek için joker karakterler kullanabilirsiniz. Ardışık olmayan birden çok joker karakter kullanımına izin verilir.
Aşağıda, */CSV/popülasyonu*ile başlayan tüm klasörlerden *popülasyon* ile başlayan tüm *CSV* dosyalarını okuyan bir örnek verilmiştir:  
`https://sqlondemandstorage.blob.core.windows.net/csv/population*/population*.csv`

Bir klasör olarak unstructured_data_path belirtirseniz, bir SQL isteğe bağlı sorgusu bu klasörden dosyaları alır. 

> [!NOTE]
> Hadoop ve PolyBase 'in aksine, SQL isteğe bağlı alt klasörler döndürmez. Ayrıca, Hadoop ve PloyBase farklı olarak, SQL isteğe bağlı, dosya adının altı çizili (_) veya nokta (.) ile başladığı dosyaları döndürür.

Aşağıdaki örnekte, unstructured_data_path =`https://mystorageaccount.dfs.core.windows.net/webdata/`ise, bir SQL isteğe bağlı sorgusu, mydata. txt ve _Hidden. txt ' den satırları döndürür. Bir alt klasörde bulunduğundan mydata2. txt ve mydata3. txt döndürmez.

![Dış tablolar için özyinelemeli veriler](./media/develop-openrowset/folder-traversal.png)

`[WITH ( {'column_name' 'column_type' [ 'column_ordinal'] }) ]`

WıTH yan tümcesi, dosyalardan okumak istediğiniz sütunları belirtmenize olanak tanır.

- CSV veri dosyaları için tüm sütunları okumak üzere sütun adlarını ve bunların veri türlerini belirtin. Sütunların bir alt kümesini isterseniz, kaynak veri dosyalarından sütunları sıralı olarak seçmek için sıralı numaraları kullanın. Sütunlar sıra atamağına göre bağlanacaktır. 

> [!IMPORTANT]
> WıTH yan tümcesi CSV dosyaları için zorunludur.
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

Kullanılacak satır sonlandırıcıyı belirtir. Varsayılan satır Sonlandırıcı, \r\ngibi bir yeni satır karakteridir.

ESCAPE_CHAR = ' Char '

Dosyanın kendisini ve dosyadaki tüm sınırlayıcı değerlerini kaçış için kullanılan karakteri belirtir. Kaçış karakterinin kendisi dışında bir değer veya sınırlayıcı değerlerinden herhangi biri gelmesi durumunda, değer okunurken kaçış karakteri bırakılır. 

ESCAPE_CHAR parametresi, FIELDQUOTE 'un etkin olup olmamasından bağımsız olarak uygulanır. Tırnak işareti karakterini atlamak için kullanılmaz. Tırnak işareti karakteri, Excel CSV davranışına göre hizalamayla birlikte çift tırnak işareti karakteriyle sonuçlanır.

FIRSTROW = ' first_row ' 

Yüklenecek ilk satırın numarasını belirtir. Varsayılan değer 1'dir. Bu, belirtilen veri dosyasındaki ilk satırı belirtir. Satır numaraları, satır sonlandırıcıları sayarak belirlenir. FIRSTROW 1 tabanlıdır.

FIELDQUOTE = ' field_quote ' 

CSV dosyasında tırnak karakteri olarak kullanılacak bir karakter belirtir. Belirtilmezse, tırnak karakteri (") kullanılır. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, population*. csv dosyalarından yalnızca 1 ve 4 sıralı numaralarına sahip iki sütun döndürür. Dosyalarda üstbilgi satırı olmadığından, ilk satırdan okumaya başlar:

```sql
/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

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
/* make sure you have credentials for storage account access created
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]
GO

CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = ''
GO
*/

SELECT 
    TOP 1 *
FROM  
    OPENROWSET(
        BULK 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer/release/us_population_county/year=20*/*.parquet',
        FORMAT='PARQUET'
    ) AS [r]
```



## <a name="next-steps"></a>Sonraki adımlar

Daha fazla örnek için [hızlı](query-data-storage.md) başlangıçlara gidin veya [Cetas](develop-tables-cetas.md)kullanarak sorgunuzun sonuçlarını Azure depolama 'ya kaydedin.
