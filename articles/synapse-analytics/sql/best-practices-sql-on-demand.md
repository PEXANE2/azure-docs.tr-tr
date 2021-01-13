---
title: Sunucusuz SQL havuzu için en iyi yöntemler
description: Sunucusuz SQL havuzu ile çalışmaya yönelik öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 05/01/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 93ac8cd3e462c244840a5ed569d685a9d67fa6c2
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98165884"
---
# <a name="best-practices-for-serverless-sql-pool-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te sunucusuz SQL havuzu için en iyi yöntemler

Bu makalede sunucusuz SQL havuzu kullanmak için en iyi yöntemler koleksiyonunu bulacaksınız. Sunucusuz SQL havuzu, Azure SYNAPSE Analytics 'teki bir kaynaktır.

## <a name="general-considerations"></a>Dikkat edilmesi gereken temel noktalar

Sunucusuz SQL havuzu, Azure depolama hesaplarınızdaki dosyaları sorgulamanızı sağlar. Yerel depolama veya alma özelliklerine sahip değildir. Bu nedenle, sorgunun hedeflediği tüm dosyalar sunucusuz SQL havuzu dışında. Depolama alanından dosyaların okunmayla ilgili her şey, sorgu performansının üzerinde bir etkiye sahip olabilir.

## <a name="colocate-your-storage-and-serverless-sql-pool"></a>Depolama ve sunucusuz SQL havuzunuzu birlikte bulundurma

Gecikme süresini en aza indirmek için, Azure Storage hesabınızı veya CosmosDB analitik depolama alanınızı ve sunucusuz SQL havuzu uç noktanızı birlikte bulundurma. Çalışma alanı oluşturma sırasında sağlanan depolama hesapları ve uç noktaları aynı bölgede bulunur.

En iyi performans için, sunucusuz SQL havuzu ile diğer depolama hesaplarına eriştiğinizde aynı bölgede olduklarından emin olun. Aynı bölgede yoksa, uzak bölge ve uç noktanın bölgesi arasındaki ağ aktarımı için gecikme süresi artacaktır.

## <a name="azure-storage-throttling"></a>Azure depolama alanı azaltma

Birden çok uygulama ve hizmet depolama hesabınıza erişebilir. Depolama alanı azaltma, uygulamalar, hizmetler ve sunucusuz SQL havuzu iş yükü tarafından oluşturulan Birleşik ıOPS veya üretilen iş yükü depolama hesabının sınırlarını aştığında meydana gelir. Sonuç olarak, sorgu performansı üzerinde önemli bir olumsuz etkisi yaşarsınız.

Daraltma algılandığında sunucusuz SQL havuzu bunu çözmek için yerleşik işleme sahiptir. Sunucusuz SQL havuzu, azaltma çözümlenene kadar daha yavaş bir hızda depolama istekleri yapar.

> [!TIP]
> En iyi sorgu yürütmesi için, sorgu yürütme sırasında diğer iş yükleriyle depolama hesabını kaçırmayın.

## <a name="prepare-files-for-querying"></a>Dosyaları sorgulamaya hazırlama

Mümkünse, daha iyi performans için dosyaları hazırlayacaksınız:

- Büyük CSV ve JSON 'ı Parquet 'e dönüştürün. Parquet, sütunlu bir biçimdir. Sıkıştırılmış olduğundan, dosya boyutları CSV 'den veya aynı verileri içeren JSON dosyalarından daha küçüktür. Parquet dosyalarını okuyorsanız, sunucusuz SQL havuzu sorgu için gerekli olmayan sütunları ve satırları atlayabilir. Sunucusuz SQL havuzu, okumak için daha az zaman ve daha az depolama isteğinde olmalıdır.
- Bir sorgu tek bir büyük dosyayı hedefliyorsa, onu birden çok daha küçük dosyaya bölmeyi avantajdan yararlanabilirsiniz.
- CSV dosyanızın boyutunu 100 MB ile 10 GB arasında tutmaya çalışın.
- Tek bir OPENROWSET yolu veya dış tablo konumu için eşit boyutlu dosyalar olması daha iyidir.
- Bölümleri farklı klasörlere veya dosya adlarına depolayarak verilerinizi bölümleyin. Bkz. [belirli bölümleri hedeflemek için filename ve FilePath Işlevlerini kullanma](#use-filename-and-filepath-functions-to-target-specific-partitions).

## <a name="push-wildcards-to-lower-levels-in-the-path"></a>Yoldaki daha düşük düzeylerde joker karakter gönder

[Birden çok dosya ve klasörü sorgulamak](query-data-storage.md#query-multiple-files-or-folders)için yolunuzda joker karakterler kullanabilirsiniz. Sunucusuz SQL havuzu, Depolama hesabınızdaki dosyaları, ilk * depolama API 'SI kullanılarak başlayarak listeler. Belirtilen yolla eşleşmeyen dosyaları ortadan kaldırır. İlk joker karaktere kadar belirtilen yol ile eşleşen çok sayıda dosya varsa, ilk dosya listesini azaltmak performansı iyileştirebilir.

## <a name="use-appropriate-data-types"></a>Uygun veri türlerini kullan

Sorgunuzda kullandığınız veri türleri performansı etkiler. Aşağıdaki yönergeleri izlerseniz daha iyi performans alabilirsiniz: 

- Olası en büyük değere uyum sağlayacak en küçük veri boyutunu kullanın.
  - En fazla karakter değeri uzunluğu 30 karakter ise, 30 uzunluğunda bir karakter veri türü kullanın.
  - Tüm karakter sütun değerleri sabit boyutlardır **char** veya **nchar** kullanın. Aksi takdirde, **varchar** veya **nvarchar** kullanın.
  - En büyük tamsayı sütun değeri 500 ise, bu değere sahip olabilecek en küçük veri türü olduğundan, **smallint** kullanın. [Bu makalede](/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql?view=azure-sqldw-latest&preserve-view=true), tamsayı veri türü aralıklarını bulabilirsiniz.
- Mümkünse, **nvarchar** ve **nchar** yerine **varchar** ve **char** kullanın.
- Mümkünse tamsayı tabanlı veri türlerini kullanın. İşlemler, karakter verilerinden daha fazla tamsayı üzerinde tamamlanır, BIRLEŞTIRIN ve GRUPLANDıRıN.
- Şema çıkarımı kullanıyorsanız, [gösterilen veri türlerini kontrol](#check-inferred-data-types)edin.

## <a name="check-inferred-data-types"></a>Gösterilen veri türlerini denetle

[Şema çıkarımı](query-parquet-files.md#automatic-schema-inference) , dosya şemalarını bilmeden sorguları hızlı bir şekilde yazmanıza ve verileri araştırmanıza yardımcı olur. Bu rahatlığının maliyeti, gösterilen veri türlerinin gerçek veri türlerinden daha büyük olması olabilir. Bu, uygun veri türünün kullanıldığından emin olmak için kaynak dosyalarında yeterli bilgi olmadığında oluşur. Örneğin, Parquet dosyaları en fazla karakter sütun uzunluğu hakkında meta veri içermez. Bu nedenle sunucusuz SQL havuzu bunu varchar (8000) olarak algılar.

Sorgunuzun elde edilen veri türlerini denetlemek için [sp_describe_first_results_set](/sql/relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql?view=sql-server-ver15&preserve-view=true) kullanabilirsiniz.

Aşağıdaki örnek, çıkarılan veri türlerini nasıl iyileştiribileceğinizi göstermektedir. Bu yordam, çıkarılan veri türlerini göstermek için kullanılır: 
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

Sonuç kümesi şu şekildedir:

|is_hidden|column_ordinal|name|system_type_name|max_length|
|----------------|---------------------|----------|--------------------|-------------------||
|0|1|vendor_id|varchar (8000)|8000|
|0|2|pickup_datetime|datetime2 (7)|8|
|0|3|passenger_count|int|4|

Sorgu için gösterilen veri türlerini öğrendikten sonra, uygun veri türlerini belirtebilirsiniz:

```sql  
SELECT
    vendor_id, pickup_datetime, passenger_count
FROM 
    OPENROWSET(
        BULK 'https://sqlondemandstorage.blob.core.windows.net/parquet/taxi/*/*/*',
        FORMAT='PARQUET'
    ) 
    WITH (
        vendor_id varchar(4), -- we used length of 4 instead of the inferred 8000
        pickup_datetime datetime2,
        passenger_count int
    ) AS nyc;
```

## <a name="use-filename-and-filepath-functions-to-target-specific-partitions"></a>Belirli bölümleri hedeflemek için filename ve FilePath işlevlerini kullanın

Veriler genellikle bölümler halinde düzenlenir. Belirli klasörleri ve dosyaları sorgulamak için sunucusuz SQL havuzu söyleyebilirsiniz. Bu işlem, sorgunun okuması ve işlenmesi gereken dosya sayısını ve veri miktarını azaltır. Ek bir ödül daha iyi bir performans elde edersiniz.

Daha fazla bilgi için [dosya adı](query-data-storage.md#filename-function) ve [FilePath](query-data-storage.md#filepath-function) işlevleri hakkında bilgi edinin ve [belirli dosyaları sorgulama](query-specific-files.md)örneklerine bakın.

> [!TIP]
> FilePath ve filename işlevlerinin sonuçlarını her zaman uygun veri türlerine atayın. Karakter veri türleri kullanıyorsanız, uygun uzunluğu kullandığınızdan emin olun.

> [!NOTE]
> Bölüm yok etme, FilePath ve filename için kullanılan işlevler, Azure SYNAPSE Analytics için Apache Spark oluşturulan her tablo için otomatik olarak oluşturulanlardan farklı olarak, şu anda dış tablolarda desteklenmemektedir.

Depolanan verileriniz bölümlenmemişse, Bölümlendirmeyi düşünün. Bu şekilde, bu dosyaları hedefleyen sorguları iyileştirmek için bu işlevleri kullanabilirsiniz. [Azure SYNAPSE tabloları için bölümlenmiş Apache Spark](develop-storage-files-spark-tables.md) SUNUCUSUZ SQL havuzundan sorgulayıp, sorgu otomatik olarak yalnızca gerekli dosyaları hedefleyecek.

## <a name="use-parser_version-20-to-query-csv-files"></a>CSV dosyalarını sorgulamak için PARSER_VERSION 2,0 kullanın

CSV dosyalarını sorguladığınızda performans için iyileştirilmiş bir Ayrıştırıcı kullanabilirsiniz. Ayrıntılar için bkz. [PARSER_VERSION](develop-openrowset.md).

## <a name="manually-create-statistics-for-csv-files"></a>CSV dosyaları için el ile istatistikleri oluşturma

Sunucusuz SQL havuzu, en iyi sorgu yürütme planlarını oluşturmak için istatistiklere bağımlıdır. İstatistikler, gerektiğinde Parquet dosyalarındaki sütunlar için otomatik olarak oluşturulur. Şu anda, otomatik olarak CSV dosyalarındaki sütunlar için istatistikler oluşturulmaz ve sorgularda kullandığınız sütunlar, özellikle de DISTINCT, JOIN, WHERE, ORDER BY ve GROUP BY olarak kullanılanlar için el ile istatistikler oluşturmalısınız. Ayrıntılar için bkz. [sunucusuz SQL havuzundaki istatistikler] (geliştirme-tablolar-istatistikler. MD # istatistikler-sunucusuz-SQL-Pool.

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Sorgu performansını ve birleştirmeleri geliştirmek için CETAS kullanın

[Cetas](develop-tables-cetas.md) , SUNUCUSUZ SQL havuzunda bulunan en önemli özelliklerden biridir. CETAS, dış tablo meta verileri oluşturan ve SELECT sorgu sonuçlarını Depolama hesabınızdaki bir dosya kümesine dışarı aktaran paralel bir işlemdir.

Birleşik başvuru tabloları gibi sorguların sık kullanılan parçalarını yeni bir dosya kümesine depolamak için CETAS kullanabilirsiniz. Daha sonra birden çok sorgu içinde ortak birleştirmeleri yinelemek yerine bu tek bir dış tabloya katılabilir.

CETAS, Parquet dosyalarını oluşturduğunda, ilk sorgu bu dış tabloyu hedeflediğinde istatistikler otomatik olarak oluşturulur ve bu da CETAS ile oluşturulan tablo hedefleme adlı sonraki sorgular için iyileştirilmiş performansa neden olur.

## <a name="azure-ad-pass-through-performance"></a>Azure AD geçiş performansı

Sunucusuz SQL havuzu Azure Active Directory (Azure AD) doğrudan geçiş veya SAS kimlik bilgilerini kullanarak depolamadaki dosyalara erişmenize olanak tanır. Azure AD geçişinden, SAS ile yaptığınız kadar daha yavaş performans sağlayabilirsiniz.

Daha iyi performansa ihtiyacınız varsa, Azure AD geçişli performans iyileştirene kadar, depolama alanına erişmek için SAS kimlik bilgilerini kullanmayı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Sık karşılaşılan sorunların çözümleri için [sorun giderme](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesini gözden geçirin. Sunucusuz SQL havuzu yerine adanmış SQL havuzu ile çalışıyorsanız, bkz. özel rehberlik için [ADANMıŞ SQL havuzları Için en iyi uygulamalar](best-practices-sql-pool.md) .
