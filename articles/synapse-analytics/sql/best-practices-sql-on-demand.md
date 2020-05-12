---
title: Azure SYNAPSE Analytics 'te isteğe bağlı SQL (Önizleme) için en iyi uygulamalar
description: İsteğe bağlı SQL (Önizleme) ile çalışırken bilmeniz gereken öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 05/01/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: a1a33404982b16e458e97aaf9959ff5dd52d1cce
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198895"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te isteğe bağlı SQL (Önizleme) için en iyi uygulamalar

Bu makalede, isteğe bağlı SQL (Önizleme) kullanmak için en iyi yöntemler koleksiyonunu bulacaksınız. İsteğe bağlı SQL, Azure SYNAPSE Analytics içinde ek bir kaynaktır.

## <a name="general-considerations"></a>Dikkat edilmesi gereken temel noktalar

İsteğe bağlı SQL, Azure depolama hesaplarınızdaki dosyaları sorgulamanızı sağlar. Yerel depolama veya alma özelliklerine sahip değildir. Bu nedenle, sorgunun hedeflediği tüm dosyalar isteğe bağlı SQL 'e ait değildir. Depolama alanından dosyaların okunmayla ilgili her şey, sorgu performansının üzerinde bir etkiye sahip olabilir.

## <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Azure Storage hesabını ve isteğe bağlı SQL 'i birlikte bulundurma

Gecikme süresini en aza indirmek için, Azure depolama hesabınızı ve SQL isteğe bağlı uç noktanızı birlikte bulundurma. Çalışma alanı oluşturma sırasında sağlanan depolama hesapları ve uç noktaları aynı bölgede bulunur.

En iyi performans için, isteğe bağlı SQL ile diğer depolama hesaplarına eriştiğinizde aynı bölgede olduklarından emin olun. Aynı bölgede değillerse, verilerin uzak ve uç noktanın bölgeleri arasındaki ağ aktarımı için gecikme süresi artar.

## <a name="azure-storage-throttling"></a>Azure depolama alanı azaltma

Birden çok uygulama ve hizmet, depolama hesabınıza erişebilir. Depolama alanı azaltma, uygulamalar, hizmetler ve SQL isteğe bağlı iş yükü tarafından oluşturulan Birleşik ıOPS veya üretilen iş yükü depolama hesabının sınırlarını aştığında oluşur. Sonuç olarak, sorgu performansı üzerinde önemli bir olumsuz etkisi yaşarsınız.

Daraltma algılandıktan sonra, SQL isteğe bağlı, bu senaryonun yerleşik olarak işlenmesini içerir. İsteğe bağlı SQL, azaltma çözümlenene kadar daha yavaş bir hızda depolama istekleri yapar.

> [!TIP]
> En iyi sorgu yürütmesi için, sorgu yürütme sırasında depolama hesabını diğer iş yükleri ile vurgumalısınız.

## <a name="prepare-files-for-querying"></a>Dosyaları sorgulamaya hazırlama

Mümkünse, daha iyi performans için dosyaları hazırlayacaksınız:

- CSV ve JSON 'ı Parquet-Parquet, sütunlu biçim olarak dönüştürür. Sıkıştırılmış olduğundan, dosya boyutları CSV veya aynı verilere sahip JSON dosyalarından daha küçüktür. İsteğe bağlı SQL 'i okumak için daha az zaman ve depolama istekleri gerekecektir.
- Bir sorgu tek bir büyük dosyayı hedefliyorsa, onu birden çok daha küçük dosyaya bölmeyi avantajdan yararlanabilirsiniz.
- CSV dosyanızın boyutunu 10 GB 'tan daha düşük tutmaya çalışın.
- Tek bir OPENROWSET yolu veya dış tablo konumu için eşit boyutlu dosyalar olması daha iyidir.
- Bölümleri farklı klasörlere veya dosya adlarına depolayarak verilerinizi bölümleyin; [belirli bölümleri hedeflemek için dosya adı ve FilePath işlevlerini kullanın](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

## <a name="push-wildcards-to-lower-levels-in-path"></a>Yoldaki daha düşük düzeylerde joker karakter gönder

[Birden çok dosya ve klasörü sorgulamak](develop-storage-files-overview.md#query-multiple-files-or-folders)için yolunuzda joker karakterler kullanabilirsiniz. İsteğe bağlı SQL, depolama ortamınızdaki ilk * depolama API 'SI kullanılarak başlayan ve belirtilen yoldan eşleşmeyen dosyaları ortadan kaldıran dosyaları listeler. İlk joker karaktere kadar belirtilen yol ile eşleşen çok sayıda dosya varsa, ilk dosya listesini azaltmak performansı iyileştirebilir.

## <a name="use-appropriate-data-types"></a>Uygun veri türlerini kullan

Sorgunuzda kullanılan veri türleri performansı etkiler. Şunları yaparsanız daha iyi performans alabilirsiniz: 

- Olası en büyük değere uyum sağlayacak en küçük veri boyutunu kullanın.
  - Maksimum karakter değeri uzunluğu 30 karakter ise, 30 uzunluğunda karakter veri türünü kullanın.
  - Tüm karakter sütun değerleri sabit boyutlardır char veya nchar kullanın. Aksi takdirde, varchar veya nvarchar kullanın.
  - En büyük tamsayı sütun değeri 500 ise, bu değere sahip olabilecek en küçük veri türü olduğu için smallint kullanın. Tamsayı veri türü aralıklarını [burada](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql?view=sql-server-ver15)bulabilirsiniz.
- Mümkünse, nvarchar ve nchar yerine varchar ve Char kullanın.
- Mümkünse tamsayı tabanlı veri türlerini kullanın. Sıralama, JOIN ve gruplandırma işlemleri, karakter verilerinden daha hızlı bir şekilde gerçekleştirilir.
- Şema çıkarımı kullanıyorsanız, [gösterilen veri türünü denetleyin](#check-inferred-data-types).

## <a name="check-inferred-data-types"></a>Gösterilen veri türlerini denetle

[Şema çıkarımı](query-parquet-files.md#automatic-schema-inference) , dosya şemasını bilmeden sorguları hızlı bir şekilde yazmanıza ve verileri araştırmanıza yardımcı olur. Bu rahatlık, çıkarsanından daha büyük olan çıkarılan veri türlerinin masrafına gelir. Uygun veri türünün kullanıldığından emin olmak için kaynak dosyalarında yeterli bilgi olmadığında gerçekleşir. Örneğin, Parquet dosyaları en fazla karakter sütun uzunluğu hakkında meta veriler içermez ve isteğe bağlı SQL bu verileri varchar (8000) olarak algılar. 

[Sp_describe_first_results_set](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql?view=sql-server-ver15)kullanarak sorgunuzun sonuç veri türlerini kontrol edebilirsiniz.

Aşağıdaki örnek, çıkarılan veri türlerini nasıl iyileştiribileceğinizi göstermektedir. Yordamı, gösterilen veri türlerini göstermek için kullanılır. 
```sql  
EXEC sp_describe_first_result_set N'
    SELECT
        vendor_id, pickup_datetime, passenger_count
    FROM 
        OPENROWSET(
            BULK ''https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*'',
            FORMAT=''PARQUET''
        ) AS nyc';
```

Sonuç kümesini burada bulabilirsiniz.

|is_hidden|column_ordinal|ad|system_type_name|max_length|
|----------------|---------------------|----------|--------------------|-------------------||
|0|1|vendor_id|varchar (8000)|8000|
|0|2|pickup_datetime|datetime2 (7)|8|
|0|3|passenger_count|int|4|

Sorgu için gösterilen veri türlerini öğrendikten sonra, uygun veri türlerini belirteceğiz:

```sql  
SELECT
    vendor_id, pickup_datetime, passenger_count
FROM 
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*',
        FORMAT='PARQUET'
    ) 
    WITH (
        vendor_id varchar(4), -- we used length of 4 instead of inferred 8000
        pickup_datetime datetime2,
        passenger_count int
    ) AS nyc;
```

## <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Belirli bölümleri hedeflemek için FileInfo ve FilePath işlevlerini kullanın

Veriler genellikle bölümler halinde düzenlenir. Belirli klasörleri ve dosyaları sorgulamak için isteğe bağlı SQL 'e bildirebilirsiniz. Bu işlev, sorgunun okuması ve işlemesi gereken dosya sayısını ve veri miktarını azaltır. Ek bir ödül daha iyi bir performans elde edersiniz.

Daha fazla bilgi için [dosya adı](develop-storage-files-overview.md#filename-function) ve [FilePath](develop-storage-files-overview.md#filepath-function) işlevlerini ve [belirli dosyaları sorgulama](query-specific-files.md)ile ilgili örnekleri denetleyin.

> [!TIP]
> Her zaman FilePath ve FileInfo işlevlerinin sonucunu uygun veri türlerine atayın. Karakter veri türleri kullanıyorsanız, uygun uzunluğun kullanıldığından emin olun.

> [!NOTE]
> Bölüm yok etme, FilePath ve FileInfo için kullanılan işlevler, SYNAPSE Spark içinde oluşturulan her tablo için otomatik olarak oluşturulanlardan farklı dış tablolarda Şu anda desteklenmemektedir.

Depolanan verileriniz bölümlenmemişse, bu dosyaları hedefleyen sorguları iyileştirmek için bu işlevleri kullanabilmek üzere Bölümlendirmeyi düşünün. [Bölümlenmiş Spark TABLOLARıNı](develop-storage-files-spark-tables.md) SQL isteğe bağlı olarak sorgularken, sorgu yalnızca gerekli dosyaları hedefleyecek.

## <a name="use-parser_version-20-for-querying-csv-files"></a>CSV dosyalarını sorgulamak için PARSER_VERSION 2,0 kullanın

CSV dosyalarını sorgularken performans için iyileştirilmiş ayrıştırıcısı kullanabilirsiniz. Ayrıntılar için [PARSER_VERSION](develop-openrowset.md) denetleyin.

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Sorgu performansını ve birleştirmeleri geliştirmek için CETAS kullanın

[Cetas](develop-tables-cetas.md) , SQL isteğe bağlı olarak sunulan en önemli özelliklerden biridir. CETAS, dış tablo meta verileri oluşturan ve SELECT sorgu sonuçlarını Depolama hesabınızdaki bir dosya kümesine dışarı aktaran paralel bir işlemdir.

Birleşik başvuru tabloları gibi sorguların sık kullanılan parçalarını yeni bir dosya kümesine depolamak için CETAS kullanabilirsiniz. Daha sonra, birden çok sorgu içinde ortak birleştirmeleri yinelemek yerine bu tek dış tabloya katabilirsiniz.

CETAS, Parquet dosyalarını oluşturduğunda, ilk sorgu bu dış tabloyu hedeflediğinde istatistikler otomatik olarak oluşturulur ve bu da gelişmiş performansa neden olur.

## <a name="aad-pass-through-performance"></a>AAD geçiş performansı

İsteğe bağlı SQL, AAD geçişli veya SAS kimlik bilgilerini kullanarak depolamadaki dosyalara erişmenize olanak tanır. AAD geçişiyle SAS ile karşılaştırıldığında daha yavaş performans yaşayabilirsiniz. 

Daha iyi performansa ihtiyacınız varsa, AAD geçiş performansı iyileştirilene kadar SAS kimlik bilgilerini depolamaya erişin.

## <a name="next-steps"></a>Sonraki adımlar

Sık karşılaşılan sorunlar ve çözümleri için [sorun giderme](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesini inceleyin. İsteğe bağlı SQL yerine SQL havuzu ile çalışıyorsanız, lütfen belirli yönergeler için [SQL havuzu Için En Iyi uygulamalar](best-practices-sql-pool.md) makalesine bakın.
