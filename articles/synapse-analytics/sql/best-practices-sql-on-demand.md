---
title: İsteğe bağlı SQL için en iyi uygulamalar (Önizleme)
description: İsteğe bağlı SQL (Önizleme) ile çalışırken bilmeniz gereken öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 05/01/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.openlocfilehash: 0b926491ef3fc672626317184fb7ed2760919866
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85210516"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te isteğe bağlı SQL (Önizleme) için en iyi uygulamalar

Bu makalede, isteğe bağlı SQL (Önizleme) kullanmak için en iyi yöntemler koleksiyonunu bulacaksınız. İsteğe bağlı SQL, Azure SYNAPSE Analytics 'teki bir kaynaktır.

## <a name="general-considerations"></a>Dikkat edilmesi gereken temel noktalar

İsteğe bağlı SQL, Azure depolama hesaplarınızdaki dosyaları sorgulamanızı sağlar. Yerel depolama veya alma özelliklerine sahip değildir. Bu nedenle, sorgunun hedeflediği tüm dosyalar isteğe bağlı SQL 'e ait değildir. Depolama alanından dosyaların okunmayla ilgili her şey, sorgu performansının üzerinde bir etkiye sahip olabilir.

## <a name="colocate-your-azure-storage-account-and-sql-on-demand"></a>Azure depolama hesabınızı ve isteğe bağlı SQL 'i birlikte bulundurma

Gecikme süresini en aza indirmek için, Azure depolama hesabınızı ve SQL isteğe bağlı uç noktanızı birlikte bulundurma. Çalışma alanı oluşturma sırasında sağlanan depolama hesapları ve uç noktaları aynı bölgede bulunur.

En iyi performans için, isteğe bağlı SQL ile diğer depolama hesaplarına eriştiğinizde aynı bölgede olduklarından emin olun. Aynı bölgede yoksa, uzak bölge ve uç noktanın bölgesi arasındaki ağ aktarımı için gecikme süresi artacaktır.

## <a name="azure-storage-throttling"></a>Azure depolama alanı azaltma

Birden çok uygulama ve hizmet depolama hesabınıza erişebilir. Depolama alanı azaltma, uygulamalar, hizmetler ve SQL isteğe bağlı iş yükü tarafından oluşturulan Birleşik ıOPS veya üretilen iş yükü depolama hesabının sınırlarını aştığında oluşur. Sonuç olarak, sorgu performansı üzerinde önemli bir olumsuz etkisi yaşarsınız.

Daraltma algılandığında, SQL isteğe bağlı, çözümü çözümlemek için yerleşik işleme sahip olur. İsteğe bağlı SQL, azaltma çözümlenene kadar daha yavaş bir hızda depolama istekleri yapar.

> [!TIP]
> En iyi sorgu yürütmesi için, sorgu yürütme sırasında diğer iş yükleriyle depolama hesabını kaçırmayın.

## <a name="prepare-files-for-querying"></a>Dosyaları sorgulamaya hazırlama

Mümkünse, daha iyi performans için dosyaları hazırlayacaksınız:

- CSV ve JSON 'ı Parquet 'e dönüştürün. Parquet, sütunlu bir biçimdir. Sıkıştırılmış olduğundan, dosya boyutları CSV 'den veya aynı verileri içeren JSON dosyalarından daha küçüktür. İsteğe bağlı SQL 'i okumak için daha az zaman ve daha az depolama isteği gerekecektir.
- Bir sorgu tek bir büyük dosyayı hedefliyorsa, onu birden çok daha küçük dosyaya bölmeyi avantajdan yararlanabilirsiniz.
- CSV dosyanızın boyutunu 10 GB 'tan daha düşük tutmaya çalışın.
- Tek bir OPENROWSET yolu veya dış tablo konumu için eşit boyutlu dosyalar olması daha iyidir.
- Bölümleri farklı klasörlere veya dosya adlarına depolayarak verilerinizi bölümleyin. Bkz. [belirli bölümleri hedeflemek için filename ve FilePath Işlevlerini kullanma](#use-filename-and-filepath-functions-to-target-specific-partitions).

## <a name="push-wildcards-to-lower-levels-in-the-path"></a>Yoldaki daha düşük düzeylerde joker karakter gönder

[Birden çok dosya ve klasörü sorgulamak](develop-storage-files-overview.md#query-multiple-files-or-folders)için yolunuzda joker karakterler kullanabilirsiniz. SQL isteğe bağlı, depolama ortamınızdaki dosyaları, ilk * depolama API 'SI kullanılarak başlayarak, depolama hesabınızda listeler. Belirtilen yolla eşleşmeyen dosyaları ortadan kaldırır. İlk joker karaktere kadar belirtilen yol ile eşleşen çok sayıda dosya varsa, ilk dosya listesini azaltmak performansı iyileştirebilir.

## <a name="use-appropriate-data-types"></a>Uygun veri türlerini kullan

Sorgunuzda kullandığınız veri türleri performansı etkiler. Aşağıdaki yönergeleri izlerseniz daha iyi performans alabilirsiniz: 

- Olası en büyük değere uyum sağlayacak en küçük veri boyutunu kullanın.
  - En fazla karakter değeri uzunluğu 30 karakter ise, 30 uzunluğunda bir karakter veri türü kullanın.
  - Tüm karakter sütun değerleri sabit boyutlardır **char** veya **nchar**kullanın. Aksi takdirde, **varchar** veya **nvarchar**kullanın.
  - En büyük tamsayı sütun değeri 500 ise, bu değere sahip olabilecek en küçük veri türü olduğundan, **smallint** kullanın. [Bu makalede](https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql?view=sql-server-ver15), tamsayı veri türü aralıklarını bulabilirsiniz.
- Mümkünse, **nvarchar** ve **nchar**yerine **varchar** ve **char** kullanın.
- Mümkünse tamsayı tabanlı veri türlerini kullanın. İşlemler, karakter verilerinden daha fazla tamsayı üzerinde tamamlanır, BIRLEŞTIRIN ve GRUPLANDıRıN.
- Şema çıkarımı kullanıyorsanız, [gösterilen veri türlerini kontrol](#check-inferred-data-types)edin.

## <a name="check-inferred-data-types"></a>Gösterilen veri türlerini denetle

[Şema çıkarımı](query-parquet-files.md#automatic-schema-inference) , dosya şemalarını bilmeden sorguları hızlı bir şekilde yazmanıza ve verileri araştırmanıza yardımcı olur. Bu rahatlığının maliyeti, gösterilen veri türlerinin gerçek veri türlerinden daha büyük olması. Bu, uygun veri türünün kullanıldığından emin olmak için kaynak dosyalarında yeterli bilgi olmadığında oluşur. Örneğin, Parquet dosyaları en fazla karakter sütun uzunluğu hakkında meta veri içermez. Bu nedenle, SQL isteğe bağlı, onu varchar (8000) olarak algılar.

Sorgunuzun elde edilen veri türlerini denetlemek için [sp_describe_first_results_set](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql?view=sql-server-ver15) kullanabilirsiniz.

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

Veriler genellikle bölümler halinde düzenlenir. Belirli klasörleri ve dosyaları sorgulamak için isteğe bağlı SQL 'e bildirebilirsiniz. Bu işlem, sorgunun okuması ve işlenmesi gereken dosya sayısını ve veri miktarını azaltır. Ek bir ödül daha iyi bir performans elde edersiniz.

Daha fazla bilgi için [dosya adı](develop-storage-files-overview.md#filename-function) ve [FilePath](develop-storage-files-overview.md#filepath-function) işlevleri hakkında bilgi edinin ve [belirli dosyaları sorgulama](query-specific-files.md)örneklerine bakın.

> [!TIP]
> FilePath ve filename işlevlerinin sonuçlarını her zaman uygun veri türlerine atayın. Karakter veri türleri kullanıyorsanız, uygun uzunluğu kullandığınızdan emin olun.

> [!NOTE]
> Bölüm yok etme, FilePath ve filename için kullanılan işlevler, Azure SYNAPSE Analytics için Apache Spark oluşturulan her tablo için otomatik olarak oluşturulanlardan farklı olarak, şu anda dış tablolarda desteklenmemektedir.

Depolanan verileriniz bölümlenmemişse, Bölümlendirmeyi düşünün. Bu şekilde, bu dosyaları hedefleyen sorguları iyileştirmek için bu işlevleri kullanabilirsiniz. [Azure SYNAPSE tabloları için bölümlenmiş Apache Spark](develop-storage-files-spark-tables.md) SQL isteğe bağlı olarak sorgulayıp sorgu yalnızca gerekli dosyaları hedefleyecek.

## <a name="use-parser_version-20-to-query-csv-files"></a>CSV dosyalarını sorgulamak için PARSER_VERSION 2,0 kullanın

CSV dosyalarını sorguladığınızda performans için iyileştirilmiş bir Ayrıştırıcı kullanabilirsiniz. Ayrıntılar için bkz. [PARSER_VERSION](develop-openrowset.md).

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Sorgu performansını ve birleştirmeleri geliştirmek için CETAS kullanın

[Cetas](develop-tables-cetas.md) , SQL isteğe bağlı olarak sunulan en önemli özelliklerden biridir. CETAS, dış tablo meta verileri oluşturan ve SELECT sorgu sonuçlarını Depolama hesabınızdaki bir dosya kümesine dışarı aktaran paralel bir işlemdir.

Birleşik başvuru tabloları gibi sorguların sık kullanılan parçalarını yeni bir dosya kümesine depolamak için CETAS kullanabilirsiniz. Daha sonra birden çok sorgu içinde ortak birleştirmeleri yinelemek yerine bu tek bir dış tabloya katılabilir.

CETAS, Parquet dosyalarını oluşturduğunda, ilk sorgu bu dış tabloyu hedeflediğinde istatistikler otomatik olarak oluşturulur ve bu da gelişmiş performansa neden olur.

## <a name="azure-ad-pass-through-performance"></a>Azure AD geçiş performansı

İsteğe bağlı SQL, Azure Active Directory (Azure AD) doğrudan veya SAS kimlik bilgilerini kullanarak depolama alanındaki dosyalara erişmenize olanak tanır. Azure AD geçişinden, SAS ile yaptığınız kadar daha yavaş performans sağlayabilirsiniz.

Daha iyi performansa ihtiyacınız varsa, Azure AD geçişli performans iyileştirene kadar, depolama alanına erişmek için SAS kimlik bilgilerini kullanmayı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Sık karşılaşılan sorunların çözümleri için [sorun giderme](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesini gözden geçirin. İsteğe bağlı SQL yerine SQL havuzlarıyla çalışıyorsanız bkz. belirli yönergeler için [SQL havuzları Için en iyi uygulamalar](best-practices-sql-pool.md) .
