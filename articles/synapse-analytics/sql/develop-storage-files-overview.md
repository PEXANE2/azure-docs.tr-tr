---
title: Synapse SQL içinde isteğe bağlı SQL (önizleme) kullanarak depolama dosyalarını sorgula
description: Synapse SQL'deki SQL isteğe bağlı (önizleme) kaynaklarını kullanarak depolama dosyalarını sorgulamayı açıklar.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 8e6fd4955d6d68764895619efd968a957bcb5184
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460605"
---
# <a name="query-storage-files-using-sql-on-demand-preview-resources-within-synapse-sql"></a>Synapse SQL içindeki SQL isteğe bağlı (önizleme) kaynaklarını kullanarak depolama dosyalarını sorgula

İsteğe bağlı SQL (önizleme), veri gölünüzdeki verileri sorgulamanızı sağlar. Yarı yapılandırılmış ve yapılandırılmamış veri sorgularını barındıran bir T-SQL sorgu yüzey alanı sunar.

Sorgulama için aşağıdaki T-SQL yönleri desteklenir:

- SQL işlevlerinin, işleçlerin ve benzeri işlevlerin çoğunluğu da dahil olmak üzere tam [SELECT](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) yüzey alanı.
- SELECT ([CETAŞ)](develop-tables-cetas.md)OLARAK Dış TABLO OLUŞTUR Harici bir [tablo](develop-tables-external-tables.md) oluşturur ve buna paralel olarak Bir Transact-SQL SELECT deyiminin sonuçlarını Azure Depolama'ya dış tablo olarak dışa aktarır.

Şu anda desteklenmeyenlere karşı neyin ne olduğu hakkında daha fazla bilgi [için, SQL on-demand genel bakış](on-demand-workspace-overview.md) makalesini okuyun.

Azure AD kullanıcıları sorguları çalıştırdığında, varsayılan olarak depolama hesaplarına Azure AD geçiş kimlik doğrulama iletişim kuralını kullanarak erişilmesi gerekir. Bu nedenle, kullanıcılar taklit edilecek ve izinler depolama düzeyinde denetlenir. İhtiyaçlarınıza uygun [depolama erişimini kontrol](develop-storage-files-storage-access-control.md) edebilirsiniz.

## <a name="extensions"></a>Uzantıları

Azure Depolama dosyalarında bulunan verilerin yerinde sorgulanması için sorunsuz bir deneyimi desteklemek için, SQL isteğe bağlı olarak [OPENROWSET](develop-openrowset.md) işlevini ek özelliklerle kullanır:

- [Birden çok dosya veya klasörü sorgula](#query-multiple-files-or-folders)
- [PARKE dosya formatı](#parquet-file-format)
- [Sınırlı metinle çalışmak için ek seçenekler (alan sonlandırıcısı, satır sonlandırıcısı, kaçış char)](#additional-options-for-working-with-delimited-text)
- [Sütunların seçilen bir alt kümesini okuma](#read-a-chosen-subset-of-columns)
- [Şema çıkarımı](#schema-inference)
- [dosya adı fonksiyonu](#filename-function)
- [filepath fonksiyonu](#filepath-function)
- [Karmaşık türleri ve iç içe veya yinelenen veri yapıları ile çalışma](#work-with-complex-types-and-nested-or-repeated-data-structures)

### <a name="query-multiple-files-or-folders"></a>Birden çok dosya veya klasörü sorgula

Tek bir varlık veya satır kümesi olarak davranırken bir klasör veya klasör kümesi içinde bir dosya kümesi üzerinde bir T-SQL sorgusu çalıştırmak için, bir klasör veya desen (joker karakter kullanarak) bir dosya veya klasör kümesi üzerinde bir yol sağlayın.

Aşağıdaki kurallar geçerlidir:

- Desenler dizin yolunun bir bölümünde veya dosya adında görünebilir.
- Aynı dizin adımında veya dosya adında birkaç desen görünebilir.
- Birden çok joker karakter varsa, eşleşen tüm yollardaki dosyalar ortaya çıkan dosya kümesine dahil edilir.

```
N'https://myaccount.blob.core.windows.net/myroot/*/mysubfolder/*.csv'
```

Kullanım örnekleri için [Sorgu klasörlerine ve birden çok dosyaya](query-folders-multiple-csv-files.md) bakın.

### <a name="parquet-file-format"></a>PARKE dosya formatı

Parke kaynak verilerini sorgulamak için FORMAT = 'PARKE'

```sql
OPENROWSET
(
    { BULK 'data_file' ,
    { FORMATFILE = 'format_file_path' [ <bulk_options>] | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB } }
)
AS table_alias(column_alias,...n)
<bulk_options> ::=
...
[ , FORMAT = {'CSV' | 'PARQUET'} ]
```

Kullanım örnekleri için [Sorgu Parke dosyaları](query-parquet-files.md) makalesini inceleyin.

### <a name="additional-options-for-working-with-delimited-text"></a>Sınırlı metinle çalışmak için ek seçenekler

Bu ek parametreler CSV (sınırlı metin) dosyaları ile çalışmak için tanıtıldı:

```syntaxsql
<bulk_options> ::=
...
[ , FIELDTERMINATOR = 'char' ]
[ , ROWTERMINATOR = 'char' ]
[ , ESCAPE_CHAR = 'char' ]
...
```

- ESCAPE_CHAR = 'char' Dosyadaki karakteri kendisive dosyadaki tüm sınırlayıcı değerleri kaçmak için kullanılır belirtir. Kaçış karakteri kendisinden başka bir değer veya sınır layıcı değerlerden herhangi biri tarafından takip edilirse, değeri okurken kaçış karakteri bırakılır.
ESCAPE_CHAR parametresi, FIELDQUOTE'ın etkin olup olmadığına bakılmaksızın uygulanır. Alıntı karakterinden kaçmak için kullanılmaz. Alıntı karakteri, Excel CSV davranışıyla uyumlu çift tırnak işaretleriyle kaçar.
- FIELDTERMINATOR ='field_terminator' Kullanılacak alan terminatörü belirtir. Varsayılan alan sonlandırıcısı virgüldür ("**,**")
- ROWTERMINATOR ='row_terminator' Kullanılacak satır sonlandırıcısını belirtir. Varsayılan satır sonlandırıcısı yeni bir çizgi karakteridir: **\r\n**.

### <a name="read-a-chosen-subset-of-columns"></a>Sütunların seçilen bir alt kümesini okuma

Okumak istediğiniz sütunları belirtmek için OPENROWSET ekstrenizde isteğe bağlı WITH yan tümcesi sağlayabilirsiniz.

- CSV veri dosyaları varsa, tüm sütunları okumak için sütun adları ve veri türlerini sağlayın. Sütunalt kümesi istiyorsanız, gelen veri dosyalarından gelen sütunları ordinal'e göre seçmek için ordinal numaraları kullanın. Sütunlar ordinal atama ile bağlı olacaktır.
- Parke veri dosyaları varsa, kaynak veri dosyalarındaki sütun adlarıyla eşleşen sütun adları sağlayın. Sütunlar ada göre bağlı olacaktır.

```syntaxsql
OPENROWSET
...
| BULK 'data_file',
{ FORMATFILE = 'format_file_path' [ <bulk_options>] | SINGLE_BLOB | SINGLE_CLOB | SINGLE_NCLOB } }
) AS table_alias(column_alias,...n) | WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })
```

Örnekler için, [tüm sütunları belirtmeden CSV dosyalarını oku'ya](query-single-csv-file.md#returning-subset-of-columns)bakın.

### <a name="schema-inference"></a>Şema çıkarımı

OPENROWSET deyimindeki WITH yan tümcesini atlayarak, hizmete şemayı temel dosyalardan otomatik olarak algılaması (çıkartmasını) öğretebilirsiniz.

> [!NOTE]
> Bu şu anda sadece PARKQUET dosya biçimi için çalışır.

```sql
OPENROWSET(
BULK N'path_to_file(s)', FORMAT='PARQUET');

```

### <a name="filename-function"></a>Dosya adı fonksiyonu

Bu işlev, satırın kaynağı olduğu dosya adını döndürür.

Belirli dosyaları sorgulamak için Sorgu [belirli dosyalar](query-specific-files.md#filename) makalesinde Dosya Adı bölümünü okuyun.

### <a name="filepath-function"></a>Filepath işlevi

Bu işlev, tam bir yol veya yolun bir bölümünü döndürür:

- Parametre olmadan çağrıldığında, bir satırın kaynağı olan tam dosya yolunu döndürür.
- Parametre ile çağrıldığında, parametrede belirtilen konumdaki joker karakterle eşleşen yolun bir bölümünü döndürür. Örneğin, parametre değeri 1, ilk joker ile eşleşen yolun bir bölümünü döndürer.

Ek bilgi için, Sorgu belirli [dosyalar](query-specific-files.md#filepath) makalesinin Filepath bölümünü okuyun.

### <a name="work-with-complex-types-and-nested-or-repeated-data-structures"></a>Karmaşık türleri ve iç içe veya yinelenen veri yapıları ile çalışma

[Parke](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#nested-types) dosyaları gibi iç içe veya tekrarlanan veri türlerinde depolanan verilerle çalışırken sorunsuz bir deneyim sağlamak için, SQL isteğe bağlı olarak aşağıdaki uzantıları ekledi.

#### <a name="project-nested-or-repeated-data"></a>İç içe veya yinelenen verileri projelendirin

Verileri yansıtmak için, iç içe veri türlerinin sütunlarını içeren Parke dosyası nın üzerinde bir SELECT deyimi çalıştırın. Çıktıda, iç içe geçen değerler JSON'a serihale edilecek ve varchar(8000) SQL veri türü olarak döndürülür.

```sql
    SELECT * FROM
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    [AS alias]
```

Daha ayrıntılı bilgi için, [Sorgu Parke iç içe türleri](query-parquet-nested-types.md#project-nested-or-repeated-data) makalesinin Project iç içe veya yinelenen veri bölümüne bakın.

#### <a name="access-elements-from-nested-columns"></a>İç içe sütunlardan öğelere erişim

Struct gibi iç içe bir sütundan iç içe kullanılan öğelere erişmek için alan adlarını yola ayırmak için "nokta gösterimi" kullanın. OPENROWSET işlevinin WITH yan tümcesindeki yolu column_name olarak sağlayın.

Sözdizimi parçası örneği aşağıdaki gibidir:

```sql
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    WITH ({'column_name' 'column_type',})
    [AS alias]
    'column_name' ::= '[field_name.] field_name'
```

Varsayılan olarak, OPENROWSET işlevi kaynak alan adı ve yolu WITH yan tümcesinde sağlanan sütun adlarıyla eşleşir. Aynı kaynak Parke dosyası içinde farklı yuvalama düzeylerinde bulunan elemanlara WITH maddesi ile erişilebilir.

**Döndürülen değerler**

- İşlev, Iç Içe Tür grubunda olmayan tüm Parke türleri için belirtilen öğeden ve belirtilen yolda int, ondalık ve varchar gibi skaler bir değer döndürür.
- Yol İç Içe Tür'e ait bir öğeyi işaret ederse, işlev belirtilen yolüzerindeki üst öğeden başlayarak bir JSON parçası döndürür. JSON parçası tip varchar(8000) biridir.
- Özellik belirtilen column_name bulunamazsa, işlev bir hata döndürür.
- Özellik belirtilen column_path bulunamazsa, [Yol moduna](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#PATHMODE)bağlı olarak, işlev, gevşek modda katı modda veya null modundayken bir hata döndürür.

Sorgu örnekleri için, Sorgu Parke iç içe türleri makalesinde iç içe sütunlar bölümünden Erişim öğelerini gözden [geçirin.](query-parquet-nested-types.md#access-elements-from-nested-columns)

#### <a name="access-elements-from-repeated-columns"></a>Yinelenen sütunlardan öğelere erişim

Dizi veya Harita öğesi gibi yinelenen bir sütundan öğelere erişmek için, projelevermeniz ve sağlamanız gereken her skaler öğe için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işlevini kullanın:

- İç içe veya yinelenen sütun, ilk parametre olarak
- İkinci bir parametre olarak öğeyi veya özelliği ni belirten bir [JSON yolu](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

Yinelenen bir sütundan skaler olmayan öğelere erişmek için, projeve sağlamanız gereken her skaler olmayan öğe için [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işlevini kullanın:

- İç içe veya yinelenen sütun, ilk parametre olarak
- İkinci bir parametre olarak öğeyi veya özelliği ni belirten bir [JSON yolu](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

Aşağıdaki sözdizimi fragmanına bakın:

```sql
    SELECT
       { JSON_VALUE (column_name, path_to_sub_element), }
       { JSON_QUERY (column_name [ , path_to_sub_element ]), )
    FROM
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    [AS alias]
```

[Sorgu Parke iç içe](query-parquet-nested-types.md#access-elements-from-repeated-columns) türleri makalesinde yinelenen sütunlardan öğelere erişmek için sorgu örneklerini bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Farklı dosya türlerinin nasıl sorgulanır ve görünümler oluşturma ve kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Tek CSV dosyasorgulama](query-single-csv-file.md)
- [Parke dosyalarını sorgula](query-parquet-files.md)
- [JSON dosyalarını sorgula](query-json-files.md)
- [Sorgu Parke iç içe türleri](query-parquet-nested-types.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)
- [Sorgularda dosya meta verilerini kullanma](query-specific-files.md)
- [Görünümoluşturma ve kullanma](create-use-views.md)
