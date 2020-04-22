---
title: OPENROWSET isteğe bağlı SQL'de nasıl kullanılır (önizleme)
description: Bu makalede, OPENROWSET'in SÖZdizimi SQL on-demand (önizleme) ve bağımsız değişkenlerin nasıl kullanılacağı açıklanmaktadır.
services: synapse-analytics
author: filippopovic
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 6325d5555b01373b148dce69731ec64896d6e1fd
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680501"
---
# <a name="how-to-use-openrowset-with-sql-on-demand-preview"></a>OPENROWSET'in isteğe bağlı SQL ile nasıl kullanılacağı (önizleme)

OPENROWSET (TOPLU...) işlevi Azure Depolama'daki dosyalara erişmenizi sağlar. İsteğe bağlı SQL (önizleme) kaynağında, OPENROWSET toplu rowset sağlayıcısına OPENROWSET işlevini çağırarak ve TOPLU seçeneğini belirterek erişilir.  

OPENROWSET işlevi, bir sorgunun FROM yan tümcesinde, bir tablo adı OPENROWSET'iymiş olarak başvurulabilir. Bir dosyadaki verilerin satır kümesi olarak okunmasını ve döndürülmesini sağlayan yerleşik bir TOPLU sağlayıcı aracılığıyla toplu işlemleri destekler.

OPENROWSET şu anda SQL havuzunda desteklenmez.

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

Sorgulama için hedef verileri içeren giriş dosyaları için iki seçeneğiniz vardır. Geçerli değerler:

- 'CSV' - Satır/sütun ayırıcıları ile sınırlı herhangi bir metin dosyası içerir. Herhangi bir karakter TSV: FIELDTERMINATOR = sekmesi gibi bir alan ayırıcısı olarak kullanılabilir.

- 'PARKE' - Parke formatında ikili dosya 

**'unstructured_data_path'**

Verilere giden yolu belirleyen unstructured_data_path aşağıdaki gibi yapılandırılır:  
'\<önek\<>://\<storage_account_path>/ storage_path>'
 
 
 Aşağıda, belirli dış veri kaynağınıza bağlantı verecek ilgili depolama hesabı yollarını bulacaksınız. 

| Dış Veri Kaynağı       | Ön ek | Depolama hesabı yolu                                 |
| -------------------------- | ------ | ---------------------------------------------------- |
| Azure Blob Depolama         | https  | \<storage_account>.blob.core.windows.net             |
| Azure Veri Gölü Deposu Gen1 | https  | \<storage_account>.azuredatalakestore.net/webhdfs/v1 |
| Azure Veri Gölü Deposu Gen2 | https  | \<storage_account>.dfs.core.windows.net              |
||||

'\<storage_path>'

 Depolama alanınızda okumak istediğiniz klasöre veya dosyaya işaret eden bir yol belirtir. Yol bir kapsayıcıya veya klasöre işaret ederse, tüm dosyalar o kapsayıcı veya klasörden okunur. Alt klasörlerdeki dosyalar dahil edilmez. 
 
 Birden çok dosya veya klasörü hedeflemek için joker karakterlerkullanabilirsiniz. Birden fazla ardışık olmayan joker karakter kullanımına izin verilir.
Aşağıda */csv/population*ile başlayan tüm klasörlerden *popülasyon* la başlayan tüm *csv* dosyalarını okuyan bir örnek:  
`https://sqlondemandstorage.blob.core.windows.net/csv/population*/population*.csv`

Unstructured_data_path klasör olarak belirtirseniz, isteğe bağlı bir SQL sorgusu bu klasörden dosyaları alır. 

> [!NOTE]
> Hadoop ve PolyBase'in aksine, SQL isteğe bağlı alt klasörleri döndürmez. Ayrıca, Hadoop ve PloyBase'in aksine, SQL on-demand dosya adının altı çizili (_) veya bir dönemle (.) başladığı dosyaları döndürer.

Aşağıdaki örnekte, unstructured_data_path=`https://mystorageaccount.dfs.core.windows.net/webdata/`, bir SQL on-demand sorgusu mydata.txt ve _hidden.txt satırları döndürecek. Mydata2.txt ve mydata3.txt'yi döndürmez çünkü bir alt klasörde bulunurlar.

![Dış tablolar için özyinelemeli veriler](./media/develop-openrowset/folder-traversal.png)

`[WITH ( {'column_name' 'column_type' [ 'column_ordinal'] }) ]`

WITH yan tümcesi, dosyalardan okumak istediğiniz sütunları belirtmenizi sağlar.

- CSV veri dosyaları için, tüm sütunları okumak, sütun adları ve veri türleri sağlayın. Sütunalt kümesi istiyorsanız, gelen veri dosyalarından gelen sütunları ordinal'e göre seçmek için ordinal numaraları kullanın. Sütunlar ordinal atama ile bağlı olacaktır. 

> [!IMPORTANT]
> CSV dosyaları için WITH maddesi zorunludur.
- Parke veri dosyaları için, kaynak veri dosyalarındaki sütun adlarıyla eşleşen sütun adları sağlayın. Sütunlar ada göre bağlı olacaktır. WITH yan tümcesi atlanırsa, Parke dosyalarındaki tüm sütunlar döndürülür.

column_name = Çıkış sütununa ad. Sağlanırsa, bu ad kaynak dosyadaki sütun adını geçersiz kılar.

column_type = Çıktı sütunu için veri türü. Örtük veri türü dönüştürme burada gerçekleşecektir.

column_ordinal = Kaynak dosya(lar)taki sütunun ordinal sayısı. Bağlama adıyla yapıldığından, bu bağımsız değişken Parke dosyaları için yoksayılır. Aşağıdaki örnek, yalnızca bir CSV dosyasından ikinci bir sütun döndürür:

```sql
WITH (
    --[country_code] VARCHAR (5) COLLATE Latin1_General_BIN2,
    [country_name] VARCHAR (100) COLLATE Latin1_General_BIN2 2
    --[year] smallint,
    --[population] bigint
)
```

**\<bulk_options>**

ALAN TERMINATÖR ='field_terminator'

Kullanılacak alan sonlandırıcısını belirtir. Varsayılan alan sonlandırıcıbir virgül ("**,**")

ROWTERMINATOR ='row_terminator''

Kullanılacak satır sonlandırıcıyı belirtir. Varsayılan satır sonlandırıcısı \r\n gibi yeni bir çizgi karakteridir.

ESCAPE_CHAR = 'char'

Dosyadaki karakteri kendisive dosyadaki tüm sınırlayıcı değerleri kaçmak için kullanılır belirtir. Kaçış karakteri kendisinden başka bir değer veya sınır layıcı değerlerden herhangi biri tarafından izlenirse, değeri okurken kaçış karakteri bırakılır. 

ESCAPE_CHAR parametresi, FIELDQUOTE'ın etkin olup olmadığına bakılmaksızın uygulanır. Alıntı karakterinden kaçmak için kullanılmayacak. Alıntı karakteri, Excel CSV davranışıyla uyumlu çift tırnak işaretleriyle kaçar.

FIRSTROW = 'first_row' 

Yüklenmesi gereken ilk satırın sayısını belirtir. Varsayılan değer 1'dir. Bu, belirtilen veri dosyasındaki ilk satırı gösterir. Satır numaraları satır sonlandırıcıları sayarak belirlenir. FIRSTROW 1 tabanlıdır.

FIELDQUOTE = 'field_quote' 

CSV dosyasında teklif karakteri olarak kullanılacak bir karakter belirtir. Belirtilmemişse, teklif karakteri (") kullanılır. 

## <a name="examples"></a>Örnekler

Aşağıdaki örnek, popülasyon*.csv dosyalarından 1 ve 4'ün ordinal numaralarına sahip yalnızca iki sütun döndürür. Dosyalarda üstbilgi satırı olmadığından, ilk satırdan okumaya başlar:

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



Aşağıdaki örnek, sütun adları ve veri türleri belirtmeden parke biçiminde nüfus sayımı veri kümesinden ilk satırın tüm sütunlarını döndürür: 

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

Daha fazla örnek için [hızlı başlangıçlara](query-data-storage.md) gidin veya sorgunuzun sonuçlarını [CETAS](develop-tables-cetas.md)kullanarak Azure Depolama'ya kaydedin.
