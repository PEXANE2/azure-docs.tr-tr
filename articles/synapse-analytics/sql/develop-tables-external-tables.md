---
title: Synapse SQL ile harici tabloları kullanma
description: Synapse SQL ile veri dosyalarını okuma veya yazma
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 4d13d15fe950c89687acfca355d4ed183756536a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423981"
---
# <a name="use-external-tables-with-synapse-sql"></a>Synapse SQL ile harici tabloları kullanma

Harici bir tablo Hadoop, Azure Depolama blob veya Azure Veri Gölü Depolama bulunan verileri işaret ediyor. Dış tablolar, dosyalardaki verileri okumak veya Azure Depolama'daki dosyalara veri yazmak için kullanılır. Synapse SQL ile, SQL havuzuna veya isteğe bağlı SQL'e (önizleme) veri okumak ve yazmak için harici tabloları kullanabilirsiniz.

## <a name="external-tables-in-sql-pool"></a>SQL havuzunda dış tablolar

SQL havuzunda, harici bir tablo kullanabilirsiniz:

- Azure Blob Depolama ve Azure Veri Gölü Gen2'yi Transact-SQL deyimleriyle sorgula.
- Azure Blob Depolama ve Azure Veri Gölü Depolama'dan verileri SQL havuzuna aktarın ve depolayın.

CREATE TABLE AS [SELECT](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) deyimi ile birlikte kullanıldığında, harici bir tablodan seçim, verileri SQL havuzu içindeki bir tabloya aktarın. [COPY deyimine](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ek olarak, dış tablolar veri yüklemek için yararlıdır. Yükleme eğitimi için Azure [Blob Depolama'dan veri yüklemek için PolyBase'i kullanın'a](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)bakın.

## <a name="external-tables-in-sql-on-demand-preview"></a>İsteğe bağlı SQL'deki dış tablolar (önizleme)

İsteğe bağlı SQL için harici bir tablo kullanırsınız:

- Verileri Azure Blob Depolama veya Azure Veri Gölü Depolama'da Transact-SQL deyimleriyle sorgula
- SQL isteğe bağlı sorgu sonuçlarını [CETAS](develop-tables-cetas.md)kullanarak Azure Blob Depolama veya Azure Veri Gölü Depolama'daki dosyalara saklayın.

Aşağıdaki adımlarla ISTEĞe bağlı SQL kullanarak harici tablolar oluşturabilirsiniz:

1. DıŞ VERI KAYNAĞı OLUŞTURMA
2. CREATE EXTERNAL FILE FORMAT
3. DıŞ TABLO OLUŞTURMA

## <a name="create-external-data-source"></a>DıŞ VERI KAYNAĞı OLUŞTURMA

Depolama hesaplarına bağlanmak için dış veri kaynakları kullanılır. Tam belgeler [burada](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)özetlenmiştir.

## <a name="syntax-for-create-external-data-source"></a>CREATE EXTERNAL DATA SOURCE için sözdizimi

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
(    LOCATION         = '<prefix>://<path>' )
[;]
```

## <a name="arguments-for-create-external-data-source"></a>Dış VERİ KAYNAĞI OLUŞTURMA Bağımsız Değişkenleri

data_source_name -Veri kaynağı için kullanıcı tanımlı adı belirtir. Ad veritabanı içinde benzersiz olmalıdır.

LOCATION `'<prefix>://<path>'` = - Dış veri kaynağına bağlantı protokolü ve yol sağlar. Yol, bir kapsayıcı şeklinde `'<prefix>://<path>/container'`ve bir klasör şeklinde `'<prefix>://<path>/container/folder'`içerebilir.

| Dış Veri Kaynağı        | Konum öneki | Konum yolu                                         |
| --------------------------- | --------------- | ----------------------------------------------------- |
| Azure Blob Depolama          | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |
| Azure Veri Gölü Deposu Gen 1 | `adl`           | `<storage_account>.azuredatalake.net`                 |
| Azure Veri Gölü Deposu Gen 2 | `abfs[s]`       | `<container>@<storage_account>.dfs.core.windows.net`  |

## <a name="example-for-create-external-data-source"></a>CREATE external DATA SOURCE için örnek

Aşağıdaki örnek, Azure Veri Gölü Gen2 için New York veri kümesini gösteren bir dış veri kaynağı oluşturur:

```sql
CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH
  -- Please note the abfss endpoint when your account has secure transfer enabled
  ( LOCATION = 'abfss://newyorktaxidataset.azuredatalakestore.net' ,
    CREDENTIAL = ADLS_credential ,
    TYPE = HADOOP
  ) ;
```

## <a name="create-external-file-format"></a>CREATE EXTERNAL FILE FORMAT

Azure Blob Depolama veya Azure Veri Gölü Depolama'da depolanan dış verileri tanımlayan harici bir dosya biçimi nesnesi oluşturur. Harici dosya biçimi oluşturmak, harici bir tablo oluşturmak için bir ön koşuldur. Tüm belgeler [burada.](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

Harici bir dosya biçimi oluşturarak, dış tablo tarafından başvurulan verilerin gerçek düzenini belirtirsiniz.

## <a name="syntax-for-create-external-file-format"></a>DıŞ DOSYA BIÇIMI OLUŞTURMA için sözdizimi

```syntaxsql
-- Create an external file format for PARQUET files.  
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = PARQUET  
    [ , DATA_COMPRESSION = {  
        'org.apache.hadoop.io.compress.SnappyCodec'  
      | 'org.apache.hadoop.io.compress.GzipCodec'      }  
    ]);  

--Create an external file format for DELIMITED TEXT files
CREATE EXTERNAL FILE FORMAT file_format_name  
WITH (  
    FORMAT_TYPE = DELIMITEDTEXT  
    [ , FORMAT_OPTIONS ( <format_options> [ ,...n  ] ) ]  
    );  

<format_options> ::=  
{  
    FIELD_TERMINATOR = field_terminator  
    | STRING_DELIMITER = string_delimiter
    | First_Row = integer
    | USE_TYPE_DEFAULT = { TRUE | FALSE }
    | Encoding = {'UTF8' | 'UTF16'}
}
```

## <a name="arguments-for-create-external-file-format"></a>Dış DOSYA BIÇIMI OLUŞTURMA bağımsız değişkenleri

file_format_name- Dış dosya biçimi için bir ad belirtir.

FORMAT_TYPE = [ PARKE | DELIMITEDTEXT]- Dış verilerin biçimini belirtir.

- PARKE - Parke formatı belirtir.
- DELIMITEDTEXT - Alan sonlandırıcıları olarak da adlandırılan sütun sınırlayıcıları içeren bir metin biçimi belirtir.

FIELD_TERMINATOR = *field_terminator* - Yalnızca sınırlı metin dosyaları için geçerlidir. Alan sonlandırıcısı, metin sınırlandırılabilen dosyadaki her alanın (sütunun) sonunu işaretleyen bir veya daha fazla karakter belirtir. Varsayılan olarak boru karakteridir (3|)).

Örnekler:

- FIELD_TERMINATOR = '|'
- FIELD_TERMINATOR = '
- FIELD_TERMINATOR =

STRING_DELIMITER = *string_delimiter* - Metin sınırlandırılatan dosyadaki tür dizesinin verileri için alan sonlandırıcısını belirtir. Dize sınırlayıcı uzunluğu bir veya daha fazla karakter ve tek tırnak ile kapalıdır. Varsayılan boş dize ("") olur.

Örnekler:

- STRING_DELIMITER = '"'
- STRING_DELIMITER = '*'
- STRING_DELIMITER =

FIRST_ROW = *First_row_int* - Önce okunan ve tüm dosyalara uygulanan satır numarasını belirtir. Değeri ikiye ayarlamak, veriler yüklendiğinde her dosyadaki ilk satırın (üstbilgi satırı) atlanmasına neden olur. Satır sonlandırıcıların (/r/n, /r, /n) varlığına göre atlanır.

USE_TYPE_DEFAULT = { DOĞRU | **YANLIŞ** } - Metin dosyasından veri alırken sınırlı metin dosyalarındaki eksik değerlerin nasıl işleyeceğini belirtir.

TRUE - Metin dosyasından veri alıyorsanız, dış tablo tanımında ilgili sütun için varsayılan değerin veri türünü kullanarak her eksik değeri depolayın. Örneğin, eksik bir değeri şu şekilde değiştirin:

- Sütun sayısal sütun olarak tanımlanırsa 0. Ondalık sütunlar desteklenmez ve hataya neden olur.
- Sütun bir dize sütunuise dizeyi ("") boşalır.
- 1900-01-01 sütun bir tarih sütunu ise.

FALSE - Eksik tüm değerleri NULL olarak saklayın. Sınırlandırılmış metin dosyasında NULL sözcüğü kullanılarak depolanan tüm NULL değerleri 'NULL' dizesi olarak alınır.

Kodlama = {'UTF8' | 'UTF16'} - SQL isteğe bağlı olarak UTF8 ve UTF16 kodlanmış sınırlandırılmış metin dosyalarını okuyabilir.

DATA_COMPRESSION = *data_compression_method* - Bu bağımsız değişken, dış veri için veri sıkıştırma yöntemini belirtir. Dış tablolardan okurken, yok sayılır. Sadece [CETAS](develop-tables-cetas.md)kullanarak dış tablolara yazarken kullanılır.

PARKE dosya biçimi türü aşağıdaki sıkıştırma yöntemlerini destekler:

- DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
- DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'

## <a name="example-for-create-external-file-format"></a>DıŞ DOSYA BIÇIMI OLUŞTURMA örneği

Aşağıdaki örnek, nüfus sayımı dosyaları için harici bir dosya biçimi oluşturur:

```sql
CREATE EXTERNAL FILE FORMAT census_file_format
WITH
(  
    FORMAT_TYPE = PARQUET,
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
)
```

## <a name="create-external-table"></a>DıŞ TABLO OLUŞTURMA

CREATE EXTERNAL TABLE komutu, Azure Blob Depolama veya Azure Veri Gölü Depolama'da depolanan verilere erişmek için Synapse SQL için harici bir tablo oluşturur. 

## <a name="syntax-for-create-external-table"></a>CREATE EXTERNAL TABLE için sözdizimi

```syntaxsql
CREATE EXTERNAL TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( <column_definition> [ ,...n ] )  
    WITH (
        LOCATION = 'folder_or_filepath',  
        DATA_SOURCE = external_data_source_name,  
        FILE_FORMAT = external_file_format_name
    )  
[;]  

<column_definition> ::=
column_name <data_type>
    [ COLLATE collation_name ]
```

## <a name="arguments-create-external-table"></a>Bağımsız Değişkenler CREATE EXTERNAL TABLE

*{ database_name.schema_name.table_name | schema_name.table_name | table_name }*

Oluşturmak için tablonun bir ila üç bölümlü adı. Harici bir tablo için, SQL isteğe bağlı olarak yalnızca tablo meta verilerini depolar. Hiçbir gerçek veri SQL'de isteğe bağlı olarak taşınamaz veya depolanır.

<column_definition>, ... *n* ]

CREATE EXTERNAL TABLE sütun adını, veri türünü, nullability ve harmanlama yapılandırma yeteneğini destekler. Varsayılan KıSıTLAMAyı dış tablolarda kullanamazsınız.

>[!IMPORTANT]
>Veri türleri ve sütun sayısı da içinde olmak üzere sütun tanımlarının dış dosyalardaki verilerle eşleşmesi gerekir. Bir uyuşmazlık varsa, gerçek veriler sorgulanırken dosya satırları reddedilir.

Parke dosyalarından okurken, yalnızca okumak istediğiniz sütunları belirtebilir ve geri kalanını atlayabilirsiniz.

YER = '*folder_or_filepath*'

Azure Blob Depolama'daki gerçek veriler için klasörü veya dosya yolunu ve dosya adını belirtir. Konum kök klasöründen başlar. Kök klasör, dış veri kaynağında belirtilen veri konumudur.

Konum klasörü belirtirseniz, harici tablodan bir SQL isteğe bağlı sorgu seçilir ve klasörden dosyaları alırsınız.

> [!NOTE]
> Hadoop ve PolyBase'in aksine, SQL isteğe bağlı alt klasörleri döndürmez. Dosya adının altı çizili (_) veya bir dönemle (.) başladığı dosyaları döndürür.

Bu örnekte, sql isteğe bağlı bir sorgu olan LOCATION='/webdata/', mydata.txt ve _hidden.txt'ten satırdöndürecekse. Mydata2.txt ve mydata3.txt'yi döndürmez çünkü bir alt klasörde bulunurlar.

![Dış tablolar için özyinelemeli veriler](./media/develop-tables-external-tables/folder-traversal.png)

DATA_SOURCE = *external_data_source_name* - Dış verilerin konumunu içeren dış veri kaynağının adını belirtir. Harici bir veri kaynağı oluşturmak için [DıŞ VERI KAYNAĞı OLUŞTUR'u](#create-external-data-source)kullanın.

FILE_FORMAT = *external_file_format_name* - Dış veriler için dosya türünü ve sıkıştırma yöntemini depolayan dış dosya biçimi nesnesinin adını belirtir. Harici bir dosya biçimi oluşturmak [için, CREATE EXTERNAL FILE FORMAT'ı](#create-external-file-format)kullanın.

## <a name="permissions-create-external-table"></a>İzinLER CREATE EXTERNAL TABLO

Harici bir tablodan seçim yapmak için, liste li ve okuma izinleri içeren uygun kimlik bilgilerine ihtiyacınız vardır.

## <a name="example-create-external-table"></a>Örnek DıŞ TABLO OLUŞTUR

Aşağıdaki örnek harici bir tablo oluşturur. İlk satırı döndürür:

```sql
CREATE EXTERNAL TABLE census_external_table
(
    decennialTime varchar(20),
    stateName varchar(100),
    countyName varchar(100),
    population int,
    race varchar(50),
    sex    varchar(10),
    minAge int,
    maxAge int
)  
WITH (
    LOCATION = '/parquet/',
    DATA_SOURCE = population_ds,  
    FILE_FORMAT = census_file_format
)
GO

SELECT TOP 1 * FROM census_external_table
```

## <a name="create-and-query-external-tables-from-a-file-in-azure-data-lake"></a>Azure Veri Gölü'ndeki bir dosyadan harici tablolar oluşturma ve sorgulama

Veri Gölü arama yeteneklerini kullanarak artık dosyaya basit bir sağ tıklama yla SQL havuzu nu veya isteğe bağlı SQL tablosunu kullanarak harici bir tablo oluşturabilir ve sorgulayabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- ADLS Gen2 Hesabına en azından Depolama Blob Veri Katılımcısı ARM Access rolüyle çalışma alanına erişebilmelisiniz

- SQL havuzunda veya SQL OD'de harici tablolar oluşturmak ve [sorgulamak için](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#permissions-2) en azından izinlere sahip olmalısınız

- ADLS Gen2 Hesabı ile ilişkili bağlantılı **hizmetin dosyaya erişimi olmalıdır.** Örneğin, bağlı hizmet kimlik doğrulama mekanizması Yönetilen Kimlik ise, çalışma alanı yönetilen kimlik depolama hesabında en az Depolama blob okuyucu izni olmalıdır

Veri panelinden, dış tabloyu oluşturmak istediğiniz dosyayı seçin:
> [!div class="mx-imgBorder"]
>![dış table1](./media/develop-tables-external-tables/external-table-1.png)

Bir iletişim penceresi açılır. İsteğe bağlı SQL havuzunu veya SQL'i seçin, tabloya bir ad verin ve açık komut dosyası seçin:

> [!div class="mx-imgBorder"]
>![dıştable2](./media/develop-tables-external-tables/external-table-2.png)

SQL Script dosyadan şema inferring otomatik olarak oluşturulur:
> [!div class="mx-imgBorder"]
>![dıştable3](./media/develop-tables-external-tables/external-table-3.png)

Betiği çalıştırın. Komut dosyası otomatik olarak Top 100 Seç *.:
> [!div class="mx-imgBorder"]
>![dış table4](./media/develop-tables-external-tables/external-table-4.png)

Dış tablo şimdi oluşturuldu, bu dış tablonun içeriğinin gelecekteki keşfi için kullanıcı doğrudan Veri bölmesinden sorgulayabilir:
> [!div class="mx-imgBorder"]
>![dıştable5](./media/develop-tables-external-tables/external-table-5.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorgu sonuçlarını Azure Depolama'daki harici bir tabloya nasıl kaydedebilirsiniz için [CETAS](develop-tables-cetas.md) makalesini kontrol edin. Veya [Spark tablolarını](develop-storage-files-spark-tables.md)sorgulamaya başlayabilirsiniz.
