---
title: SYNAPSE SQL ile dış tabloları kullanma
description: SYNAPSE SQL ile veri dosyalarını okuma veya yazma
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 4d13d15fe950c89687acfca355d4ed183756536a
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81423981"
---
# <a name="use-external-tables-with-synapse-sql"></a>SYNAPSE SQL ile dış tabloları kullanma

Dış tablo, Hadoop, Azure Depolama Blobu veya Azure Data Lake Storage bulunan verilere işaret eder. Dış tablolar, dosyalardaki verileri okumak veya Azure Storage 'daki dosyalara veri yazmak için kullanılır. SYNAPSE SQL ile, dış tabloları SQL havuzuna veya isteğe bağlı SQL 'e (Önizleme) veri okumak ve yazmak için kullanabilirsiniz.

## <a name="external-tables-in-sql-pool"></a>SQL havuzundaki dış tablolar

SQL havuzunda, bir dış tablo kullanarak şunları yapabilirsiniz:

- Azure Blob depolama ve Azure Data Lake Gen2 Transact-SQL deyimleriyle sorgulayın.
- Azure Blob depolamadan ve Azure Data Lake Storage verileri SQL Pool 'a aktarın ve depolayın.

[Create Table Select](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) ifadesiyle birlikte kullanıldığında, dış tablo ' ın seçilmesi, verileri SQL havuzundaki bir tabloya aktarır. [Copy ifadesine](/sql/t-sql/statements/copy-into-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)ek olarak, dış tablolar veri yüklemek için faydalıdır. Yükleme öğreticisi için bkz. [Azure Blob depolamadan veri yüklemek Için PolyBase kullanma](../sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

## <a name="external-tables-in-sql-on-demand-preview"></a>İsteğe bağlı SQL 'de dış tablolar (Önizleme)

İsteğe bağlı SQL için bir dış tablo kullanacaksınız:

- Azure Blob depolamada veya Azure Data Lake Storage Transact-SQL deyimleriyle veri sorgulama
- Azure Blob depolamada veya [Cetas](develop-tables-cetas.md)kullanarak Azure Data Lake Storage SQL isteğe bağlı sorgu sonuçlarını depolayın.

Aşağıdaki adımlar aracılığıyla, SQL 'i isteğe bağlı olarak kullanarak dış tablolar oluşturabilirsiniz:

1. DıŞ VERI KAYNAĞı OLUŞTUR
2. CREATE EXTERNAL FILE FORMAT
3. DıŞ TABLO OLUŞTUR

## <a name="create-external-data-source"></a>DıŞ VERI KAYNAĞı OLUŞTUR

Dış veri kaynakları, depolama hesaplarına bağlanmak için kullanılır. Tüm belgeler [burada](/sql/t-sql/statements/create-external-data-source-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)özetlenmiştir.

## <a name="syntax-for-create-external-data-source"></a>Dış VERI kaynağı oluşturma söz dizimi

```syntaxsql
CREATE EXTERNAL DATA SOURCE <data_source_name>
WITH
(    LOCATION         = '<prefix>://<path>' )
[;]
```

## <a name="arguments-for-create-external-data-source"></a>Dış VERI kaynağı oluşturma için bağımsız değişkenler

data_source_name-veri kaynağı için Kullanıcı tanımlı adı belirtir. Ad, veritabanı içinde benzersiz olmalıdır.

LOCATION = `'<prefix>://<path>'` -dış veri kaynağının bağlantı protokolünü ve yolunu sağlar. Yol `'<prefix>://<path>/container'`, biçiminde bir kapsayıcı ve biçiminde bir klasör içerebilir `'<prefix>://<path>/container/folder'`.

| Dış veri kaynağı        | Konum ön eki | Konum yolu                                         |
| --------------------------- | --------------- | ----------------------------------------------------- |
| Azure Blob Depolama          | `wasb[s]`       | `<container>@<storage_account>.blob.core.windows.net` |
| Azure Data Lake Store Gen 1 | `adl`           | `<storage_account>.azuredatalake.net`                 |
| Azure Data Lake Store Gen 2 | `abfs[s]`       | `<container>@<storage_account>.dfs.core.windows.net`  |

## <a name="example-for-create-external-data-source"></a>Dış VERI kaynağı oluşturma örneği

Aşağıdaki örnek, New York veri kümesine işaret eden Azure Data Lake Gen2 için bir dış veri kaynağı oluşturur:

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

Azure Blob depolamada veya Azure Data Lake Storage depolanan dış verileri tanımlayan bir dış dosya biçim nesnesi oluşturur. Dış dosya biçimi oluşturmak, dış tablo oluşturmak için bir önkoşuldur. Tüm belgeler [burada](/sql/t-sql/statements/create-external-file-format-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)verilmiştir.

Bir dış dosya biçimi oluşturarak, bir dış tablo tarafından başvurulan verilerin gerçek yerleşimini belirtirsiniz.

## <a name="syntax-for-create-external-file-format"></a>Dış dosya BIÇIMI oluşturma söz dizimi

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

## <a name="arguments-for-create-external-file-format"></a>Dış dosya BIÇIMI oluşturma için bağımsız değişkenler

file_format_name-dış dosya biçimi için bir ad belirtir.

FORMAT_TYPE = [PARQUET | DELIMITEDTEXT]-dış verilerin biçimini belirtir.

- PARQUET-bir Parquet biçimi belirtir.
- DELIMITEDTEXT-alan sonlandırıcılar olarak da adlandırılan sütun sınırlayıcılarıyla bir metin biçimi belirtir.

FIELD_TERMINATOR = *field_terminator* -yalnızca sınırlandırılmış metin dosyaları için geçerlidir. Alan Sonlandırıcı, metin ile ayrılmış dosyadaki her bir alanın (sütun) sonunu işaretleyen bir veya daha fazla karakter belirtir. Varsayılan, kanal karakteridir (ꞌ | ꞌ).

Örnekler:

- FIELD_TERMINATOR = ' | '
- FIELD_TERMINATOR = ' '
- FIELD_TERMINATOR = ꞌ \ t ꞌ

STRING_DELIMITER = *string_delimiter* -metin ile ayrılmış dosyada dize türündeki veriler için alan Sonlandırıcı belirler. Dize sınırlayıcısı bir veya daha fazla karakter uzunluğunda ve tek tırnak içine alınmış. Varsayılan değer boş dizedir ("").

Örnekler:

- STRING_DELIMITER = ' "'
- STRING_DELIMITER = ' * '
- STRING_DELIMITER = ꞌ, ꞌ

FIRST_ROW = *First_row_int* -ilk okunan ve tüm dosyalar için geçerli olan satır numarasını belirtir. Değerin iki olarak ayarlanması, veriler yüklendiğinde her dosyada (başlık satırı) ilk satırın atlanmasına neden olur. Satır Sonlandırıcıların varlığına (/r/n,/r,/n) göre satırlar atlanır.

USE_TYPE_DEFAULT = {TRUE | **False** } -Metin dosyasından verileri alırken sınırlandırılmış metin dosyalarında eksik değerlerin nasıl işleneceğini belirtir.

DOĞRU-metin dosyasından veri alıyorsanız, dış tablo tanımındaki karşılık gelen sütun için varsayılan değerin veri türünü kullanarak eksik her değeri saklayın. Örneğin, eksik bir değeri ile değiştirin:

- sütun sayısal bir sütun olarak tanımlanmışsa 0. Ondalık sütunlar desteklenmez ve hataya neden olur.
- Sütun bir dize sütuneyse boş dize ("").
- sütun bir tarih sütunudur, 1900-01-01.

FALSE-tüm eksik değerleri NULL olarak depola. Sınırlandırılmış metin dosyasında NULL sözcüğü kullanılarak depolanan NULL değerler ' NULL ' dizesi olarak içeri aktarılır.

Kodlama = {' UTF8 ' | ' UTF16 '}-isteğe bağlı SQL, UTF8 ve UTF16 kodlamalı sınırlandırılmış metin dosyalarını okuyabilir.

DATA_COMPRESSION = *data_compression_method* -bu bağımsız değişken dış veriler için veri sıkıştırma yöntemini belirtir. Dış tablolardan okurken yok sayılır. Yalnızca [Cetas](develop-tables-cetas.md)kullanılarak dış tablolara yazılırken kullanılır.

PARQUET dosya biçimi türü aşağıdaki sıkıştırma yöntemlerini destekler:

- DATA_COMPRESSION = ' org. Apache. Hadoop. IO. compress. GzipCodec '
- DATA_COMPRESSION = ' org. Apache. Hadoop. IO. compress. SnappyCodec '

## <a name="example-for-create-external-file-format"></a>Dış dosya BIÇIMI oluşturma örneği

Aşağıdaki örnek, görselleştirmenizdeki dosyaları için bir dış dosya biçimi oluşturur:

```sql
CREATE EXTERNAL FILE FORMAT census_file_format
WITH
(  
    FORMAT_TYPE = PARQUET,
    DATA_COMPRESSION = 'org.apache.hadoop.io.compress.SnappyCodec'
)
```

## <a name="create-external-table"></a>DıŞ TABLO OLUŞTUR

DıŞ tablo oluştur komutu, Azure Blob depolamada veya Azure Data Lake Storage depolanan verilere erişmek üzere SYNAPSE SQL için bir dış tablo oluşturur. 

## <a name="syntax-for-create-external-table"></a>DıŞ tablo oluştur söz dizimi

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

## <a name="arguments-create-external-table"></a>Bağımsız değişkenler dış tablo oluşturma

*{database_name. schema_name. table_name | schema_name. table_name | table_name}*

Oluşturulacak tablonun üç bölümden oluşan bir adı. Dış tablo için, isteğe bağlı SQL yalnızca tablo meta verilerini depolar. İsteğe bağlı SQL 'de hiçbir gerçek veri taşınmaz veya depolanmaz.

<column_definition>,... *n* ]

DıŞ tablo oluşturma, sütun adı, veri türü, null olabilme ve harmanlama yapılandırma yeteneğini destekler. Dış tablolarda varsayılan kısıtlamayı kullanamazsınız.

>[!IMPORTANT]
>Veri türleri ve sütun sayısı da içinde olmak üzere sütun tanımlarının dış dosyalardaki verilerle eşleşmesi gerekir. Bir uyuşmazlık varsa, gerçek veriler sorgulanırken dosya satırları reddedilir.

Parquet dosyalarından okurken yalnızca okumak istediğiniz sütunları belirtebilir ve geri kalanını atlayabilirsiniz.

KONUM = '*folder_or_filepath*'

Azure Blob depolamada gerçek verilerin klasörünü veya dosya yolunu ve dosya adını belirtir. Konum kök klasörden başlar. Kök klasör, dış veri kaynağında belirtilen veri konumudur.

Bir klasör konumu belirtirseniz, SQL isteğe bağlı bir sorgu dış tablodan seçim yapılır ve klasörden dosyaları alır.

> [!NOTE]
> Hadoop ve PolyBase 'in aksine, SQL isteğe bağlı alt klasörler döndürmez. Dosya adının altı çizili (_) veya nokta (.) ile başladığı dosyaları döndürür.

Bu örnekte, LOCATION = '/Webdata/' ise, bir SQL isteğe bağlı sorgusu, mydata. txt ve _hidden. txt ' den satırları döndürür. Bir alt klasörde bulunduğundan mydata2. txt ve mydata3. txt döndürmez.

![Dış tablolar için özyinelemeli veriler](./media/develop-tables-external-tables/folder-traversal.png)

DATA_SOURCE = *external_data_source_name* -dış verilerin konumunu içeren dış veri kaynağının adını belirtir. Dış veri kaynağı oluşturmak için [dış VERI kaynağı oluştur](#create-external-data-source)' u kullanın.

FILE_FORMAT = *external_file_format_name* -dış veriler için dosya türünü ve sıkıştırma yöntemini depolayan harici dosya biçim nesnesinin adını belirtir. Dış dosya biçimi oluşturmak için [dış dosya biçimi oluştur](#create-external-file-format)' u kullanın.

## <a name="permissions-create-external-table"></a>İzinler dış tablo oluşturma

Dış tablodan seçmek için, liste ve okuma izinleriyle doğru kimlik bilgilerine sahip olmanız gerekir.

## <a name="example-create-external-table"></a>Örnek dış tablo oluşturma

Aşağıdaki örnek bir dış tablo oluşturur. İlk satırı döndürür:

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

## <a name="create-and-query-external-tables-from-a-file-in-azure-data-lake"></a>Azure Data Lake bir dosyadan dış tablolar oluşturun ve sorgulayın

Data Lake keşif yeteneklerini kullanarak artık, SQL havuzunu veya isteğe bağlı SQL 'i kullanarak bir dış tablo oluşturabilir ve sorgu üzerinde basit bir sağ tıklamayla sorgulama yapabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- ADLS 2. hesaba en azından Depolama Blobu veri katılımcısı ARM erişim rolüyle çalışma alanına erişiminizin olması gerekir

- SQL havuzunda veya SQL OD üzerinde dış tablo oluşturmak ve sorgulamak için en azından [izinleriniz](/sql/t-sql/statements/create-external-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#permissions-2) olmalıdır

- ADLS 2. hesabıyla ilişkili bağlı hizmetin **dosyaya erişimi olması gerekir**. Örneğin, bağlantılı hizmet kimlik doğrulama mekanizması yönetilen kimlik ise, çalışma alanı yönetilen kimliği, depolama hesabında en az Depolama Blobu okuyucusu iznine sahip olmalıdır

Veri panelinden, dış tabloyu oluşturmak istediğiniz dosyayı seçin:
> [!div class="mx-imgBorder"]
>![externaltable1](./media/develop-tables-external-tables/external-table-1.png)

Bir iletişim kutusu penceresi açılır. SQL havuzu veya isteğe bağlı SQL ' i seçin, tabloya bir ad verin ve betiği aç ' ı seçin:

> [!div class="mx-imgBorder"]
>![externaltable2](./media/develop-tables-external-tables/external-table-2.png)

SQL betiği dosyadaki şemayı göstermek için otomatik olarak oluşturulur:
> [!div class="mx-imgBorder"]
>![externaltable3](./media/develop-tables-external-tables/external-table-3.png)

Betiği çalıştırın. Betik otomatik olarak bir SELECT Top 100 * çalıştırır.:
> [!div class="mx-imgBorder"]
>![externaltable4](./media/develop-tables-external-tables/external-table-4.png)

Dış tablo artık oluşturulmuştur; bu dış tablonun içeriğini daha sonra araştırması için Kullanıcı doğrudan veri bölmesinden sorgulayabilir:
> [!div class="mx-imgBorder"]
>![externaltable5](./media/develop-tables-external-tables/external-table-5.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorgu sonuçlarının Azure Storage 'da bir dış tabloya nasıl kaydedileceği için [Cetas](develop-tables-cetas.md) makalesini denetleyin. Ya da [Spark tablolarını](develop-storage-files-spark-tables.md)sorgulamaya başlayabilirsiniz.
