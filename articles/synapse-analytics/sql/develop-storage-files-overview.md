---
title: SYNAPSE SQL 'de isteğe bağlı SQL (Önizleme) kullanarak depolama dosyalarını sorgulama
description: SYNAPSE SQL 'de isteğe bağlı SQL (Önizleme) kaynaklarını kullanarak depolama dosyalarının sorgulanmasını açıklar.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/19/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: c2e18919b287713f59ba8785006c952134994be0
ms.sourcegitcommit: 223cea58a527270fe60f5e2235f4146aea27af32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84258377"
---
# <a name="query-storage-files-using-sql-on-demand-preview-resources-within-synapse-sql"></a>SYNAPSE SQL 'de isteğe bağlı SQL (Önizleme) kaynaklarını kullanarak depolama dosyalarını sorgulama

İsteğe bağlı SQL (Önizleme), Data Lake 'inizdeki verileri sorgulamanızı sağlar. Yarı yapılandırılmış ve yapılandırılmamış veri sorgularına uyum sağlayan bir T-SQL sorgu yüzeyi alanı sunar.

Sorgulamak için aşağıdaki T-SQL yönleri desteklenir:

- SQL işlevlerinin büyük çoğunluğu, işleçler vb. dahil olmak üzere tam [seçim](/sql/t-sql/queries/select-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) yüzeyi alanı.
- SELECT ([Cetas](develop-tables-cetas.md)) olarak dış tablo oluşturma bir [dış tablo](develop-tables-external-tables.md) oluşturur ve ardından Transact-SQL SELECT ifadesinin sonuçlarını Azure Storage 'a aktarır.

İle ilgili daha fazla bilgi için, şu anda desteklenmeyen [SQL isteğe bağlı genel bakış](on-demand-workspace-overview.md) makalesini okuyun.

Azure AD kullanıcıları sorguları çalıştırladığında, varsayılan olarak depolama hesaplarına Azure AD geçişli kimlik doğrulama protokolü kullanılarak erişilir. Bu nedenle, kullanıcıların kimliğine bürünerek, izinleri depolama düzeyinde denetlenir. Gereksinimlerinize uyacak şekilde, [depolama erişimini denetleyebilirsiniz](develop-storage-files-storage-access-control.md) .

## <a name="extensions"></a>Uzantılar

Azure depolama dosyalarında bulunan verilerin yerinde sorgulanmasına yönelik sorunsuz bir deneyim desteklemek için, SQL isteğe bağlı, ek yetenekler ile [OPENROWSET](develop-openrowset.md) işlevini kullanır:

- [Birden çok dosyayı veya klasörü sorgulama](#query-multiple-files-or-folders)
- [PARQUET dosya biçimi](#parquet-file-format)
- [Sınırlandırılmış metinle çalışmak için ek seçenekler (alan Sonlandırıcı, satır Sonlandırıcı, kaçış karakteri)](#additional-options-for-working-with-delimited-text)
- [Seçili sütunların bir alt kümesini okuyun](#read-a-chosen-subset-of-columns)
- [Şema çıkarımı](#schema-inference)
- [filename işlevi](#filename-function)
- [FilePath işlevi](#filepath-function)
- [Karmaşık türlerle ve iç içe veya yinelenen veri yapıları ile çalışma](#work-with-complex-types-and-nested-or-repeated-data-structures)

### <a name="query-multiple-files-or-folders"></a>Birden çok dosyayı veya klasörü sorgulama

Tek bir varlık veya satır kümesi olarak davranırken bir klasör veya klasör kümesi içindeki bir dosya kümesi üzerinde bir T-SQL sorgusu çalıştırmak için bir dosya veya klasör kümesi üzerinde bir klasöre veya bir modele (joker karakter kullanarak) bir yol sağlayın.

Aşağıdaki kurallar geçerlidir:

- Desenler, bir dizin yolunun veya dosya adının bir kısmında görünebilir.
- Aynı dizin adımında veya dosya adında çeşitli desenler görünebilir.
- Birden çok joker karakter varsa, tüm eşleşen yolların içindeki dosyalar elde edilen dosya kümesine dahil edilir.

```
N'https://myaccount.blob.core.windows.net/myroot/*/mysubfolder/*.csv'
```

Kullanım örnekleri için [sorgu klasörlerine ve birden çok dosyaya](query-folders-multiple-csv-files.md) bakın.

### <a name="parquet-file-format"></a>PARQUET dosya biçimi

Parquet kaynak verilerini sorgulamak için FORMAT = ' PARQUET ' kullanın

```syntaxsql
OPENROWSET
(
    { BULK 'data_file' ,
    { FORMATFILE = 'format_file_path' [ <bulk_options>] } }
)
AS table_alias(column_alias,...n)
<bulk_options> ::=
...
[ , FORMAT = {'CSV' | 'PARQUET'} ]
```

Kullanım örnekleri için [sorgu Parquet dosyaları](query-parquet-files.md) makalesini gözden geçirin.

### <a name="additional-options-for-working-with-delimited-text"></a>Sınırlandırılmış metinle çalışmak için ek seçenekler

Bu ek parametreler CSV (sınırlandırılmış metin) dosyaları ile çalışmaya yönelik olarak sunulmuştur:

```syntaxsql
<bulk_options> ::=
...
[ , FIELDTERMINATOR = 'char' ]
[ , ROWTERMINATOR = 'char' ]
[ , ESCAPE_CHAR = 'char' ]
...
```

- ESCAPE_CHAR = ' Char ', dosyanın kendisini ve tüm sınırlayıcı değerlerini kaçış için kullanılan dosyadaki karakteri belirtir. Kaçış karakterinin arkasından veya kendisi dışında bir değer veya sınırlayıcı değerlerinden herhangi biri gelmesi durumunda, değer okunurken kaçış karakteri bırakılır.
ESCAPE_CHAR parametresi, FIELDQUOTE 'un etkin olup olmadığı veya etkinleştirilmediği için uygulanır. Tırnak işareti karakterini atlamak için kullanılmaz. Tırnak işareti karakteri, Excel CSV davranışına göre hizalamayla birlikte çift tırnak işareti karakteriyle sonuçlanır.
- FIELDSONLANDıRıCı = ' field_terminator ' kullanılacak alan sonlandırıcıyı belirtir. Varsayılan alan Sonlandırıcı bir virgül ("**,**")
- ROWSONLANDıRıCı = ' row_terminator ' kullanılacak satır sonlandırıcıyı belirtir. Varsayılan satır Sonlandırıcı bir yeni satır karakteri: **\r\n**.

### <a name="read-a-chosen-subset-of-columns"></a>Seçili sütunların bir alt kümesini okuyun

Okumak istediğiniz sütunları belirtmek için, OPENROWSET deyiminiz içinde isteğe bağlı bir WıTH yan tümcesi sağlayabilirsiniz.

- CSV veri dosyaları varsa tüm sütunları okumak için sütun adlarını ve bunların veri türlerini belirtin. Sütunların bir alt kümesini isterseniz, kaynak veri dosyalarından sütunları sıralı olarak seçmek için sıralı numaraları kullanın. Sütunlar sıra atamağına göre bağlanacaktır.
- Parquet veri dosyaları varsa, kaynak veri dosyalarındaki sütun adlarıyla eşleşen sütun adları sağlayın. Sütunlar ada göre bağlanacaktır.

```syntaxsql
OPENROWSET
...
| BULK 'data_file',
{ FORMATFILE = 'format_file_path' [ <bulk_options>] } }
) AS table_alias(column_alias,...n) | WITH ( {'column_name' 'column_type' [ 'column_ordinal'] })
```

Örnekler için, [tüm sütunları BELIRTMEDEN CSV dosyalarını oku](query-single-csv-file.md#returning-subset-of-columns)bölümüne bakın.

### <a name="schema-inference"></a>Şema çıkarımı

OPENROWSET ifadesinden WıTH yan tümcesini atlayarak, bu hizmeti temel alınan dosyalardaki şemayı otomatik olarak algılamaya (çıkarsaymasını) söyleyebilirsiniz.

> [!NOTE]
> Bu şu anda yalnızca PARQUET dosya biçimi için geçerlidir.

```sql
OPENROWSET(
BULK N'path_to_file(s)', FORMAT='PARQUET');
```

En iyi performans için, [uygun gösterilen veri türlerinin](best-practices-sql-on-demand.md#check-inferred-data-types) kullanıldığından emin olun. 

### <a name="filename-function"></a>Filename işlevi

Bu işlev, satırın kaynaklandığı dosya adını döndürür. 

Belirli dosyaları sorgulamak için, [dosyaya özel dosyaları sorgula](query-specific-files.md#filename) makalesindeki dosya adı bölümünü okuyun.

Dönüş veri türü nvarchar (1024). En iyi performans için dosya adı işlevinin sonucunu her zaman uygun veri türüne atayın. Karakter veri türü kullanırsanız, uygun uzunluğun kullanıldığından emin olun.

### <a name="filepath-function"></a>FilePath işlevi

Bu işlev, yolun tam yolunu veya bir parçasını döndürür:

- Parametresi olmadan çağrıldığında, bir satırın kaynaklandığı tam dosya yolunu döndürür.
- Parametresi ile çağrıldığında, parametresinde belirtilen konumda joker karakterle eşleşen yolun bir bölümünü döndürür. Örneğin, parametre değeri 1 ilk joker karakterle eşleşen yolun bir bölümünü döndürür.

Daha fazla bilgi için, [özel dosyaları sorgula](query-specific-files.md#filepath) makalesindeki FilePath bölümünü okuyun.

Dönüş veri türü nvarchar (1024). En iyi performans için, her zaman FilePath işlevinin sonucunu uygun veri türüne atayın. Karakter veri türü kullanırsanız, uygun uzunluğun kullanıldığından emin olun.

### <a name="work-with-complex-types-and-nested-or-repeated-data-structures"></a>Karmaşık türlerle ve iç içe veya yinelenen veri yapıları ile çalışma

İç içe geçmiş veya yinelenen veri türlerinde depolanan verilerle çalışırken sorunsuz bir deneyim sağlamak için, SQL isteğe bağlı SQL [Parquet](https://github.com/apache/parquet-format/blob/master/LogicalTypes.md#nested-types) aşağıdaki uzantıları eklemiştir.

#### <a name="project-nested-or-repeated-data"></a>Proje iç içe veya yinelenen veriler

Verileri proje için, iç içe geçmiş veri türlerinin sütunlarını içeren Parquet dosyası üzerinde bir SELECT ifadesini çalıştırın. Çıkışta, iç içe geçmiş değerler JSON 'a serileştirilir ve varchar (8000) SQL veri türü olarak döndürülür.

```sql
    SELECT * FROM
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    [AS alias]
```

Daha ayrıntılı bilgi için, [sorgu Parquet iç içe türler](query-parquet-nested-types.md#project-nested-or-repeated-data) makalesinin iç içe veya yinelenen veri bölümüne bakın.

#### <a name="access-elements-from-nested-columns"></a>İç içe geçmiş sütunlardan öğelere erişin

Yuvalanmış bir sütundan iç içe geçmiş öğelere (struct gibi) erişmek için, alan adlarını yola birleştirmek için "nokta gösterimi" kullanın. Yolu OPENROWSET işlevinin WıTH yan tümcesinde column_name olarak girin.

Sözdizimi parçası örneği aşağıdaki gibidir:

```syntaxsql
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    WITH ({'column_name' 'column_type',})
    [AS alias]
    'column_name' ::= '[field_name.] field_name'
```

Varsayılan olarak, OPENROWSET işlevi WıTH yan tümcesinde belirtilen sütun adlarıyla kaynak alan adı ve yoluyla eşleşir. Aynı kaynak Parquet dosyası içindeki farklı iç içe düzeylerde bulunan öğelere WıTH yan tümcesi aracılığıyla erişilebilir.

**Döndürülen değerler**

- İşlev, Iç Içe geçmiş tür grubunda olmayan tüm Parquet türleri için, belirtilen öğeden ve belirtilen yoldaki int, Decimal ve varchar gibi bir skaler değer döndürür.
- Yol, Iç Içe geçmiş bir türün bir öğesini işaret ediyorsa, işlev belirtilen yoldaki en üstteki öğeden başlayarak bir JSON parçası döndürür. JSON parçası varchar (8000) türündedir.
- Özellik belirtilen column_name bulunamazsa, işlev bir hata döndürür.
- Özellik, [yol moduna](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest#PATHMODE)bağlı olarak belirtilen column_path bulunamazsa, işlev katı modda veya LAX modundayken null olduğunda bir hata döndürür.

Sorgu örnekleri için, [sorgu Parquet iç içe türler](query-parquet-nested-types.md#access-elements-from-nested-columns) makalesindeki iç içe geçmiş sütunlarda bulunan erişim öğeleri bölümünü gözden geçirin.

#### <a name="access-elements-from-repeated-columns"></a>Yinelenen sütunlardan öğelerine erişin

Bir dizi veya haritanın bir öğesi gibi yinelenen bir sütundan öğelere erişmek için, proje yapmanız ve sağlamanız gereken her skalar öğe için [JSON_VALUE](/sql/t-sql/functions/json-value-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işlevini kullanın:

- İlk parametre olarak iç içe veya yinelenen sütun
- İkinci bir parametre olarak, erişim için öğe veya özellik belirten bir [JSON yolu](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

Yinelenen bir sütundan skalar olmayan öğelere erişmek için, proje yapmanız ve sağlamanız gereken skalar olmayan her öğe için [JSON_QUERY](/sql/t-sql/functions/json-query-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) işlevini kullanın:

- İlk parametre olarak iç içe veya yinelenen sütun
- İkinci bir parametre olarak, erişim için öğe veya özellik belirten bir [JSON yolu](/sql/relational-databases/json/json-path-expressions-sql-server?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

Aşağıdaki sözdizimi parçasını inceleyin:

```syntaxsql
    SELECT
       { JSON_VALUE (column_name, path_to_sub_element), }
       { JSON_QUERY (column_name [ , path_to_sub_element ]), )
    FROM
    OPENROWSET
    (   BULK 'unstructured_data_path' ,
        FORMAT = 'PARQUET' )
    [AS alias]
```

[Sorgu Parquet iç içe türler](query-parquet-nested-types.md#access-elements-from-repeated-columns) makalesindeki yinelenen sütunlardan öğelere erişmek için sorgu örnekleri bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Farklı dosya türlerini sorgulama ve görünümleri oluşturma ve kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Tek CSV dosyasını sorgula](query-single-csv-file.md)
- [Parquet dosyalarını sorgulama](query-parquet-files.md)
- [JSON dosyalarını sorgulama](query-json-files.md)
- [Parquet iç içe türlerini sorgulama](query-parquet-nested-types.md)
- [Sorgu klasörleri ve birden çok CSV dosyası](query-folders-multiple-csv-files.md)
- [Sorgularda dosya meta verilerini kullan](query-specific-files.md)
- [Görünümleri oluşturma ve kullanma](create-use-views.md)
