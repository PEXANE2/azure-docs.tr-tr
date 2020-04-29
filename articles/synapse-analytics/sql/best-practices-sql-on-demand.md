---
title: Azure SYNAPSE Analytics 'te isteğe bağlı SQL (Önizleme) için en iyi uygulamalar
description: İsteğe bağlı SQL (Önizleme) ile çalışırken bilmeniz gereken öneriler ve en iyi uygulamalar.
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 1d4203141973c10fe7673f6ab9dedbc3bfdc8999
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429076"
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

- CSV 'yi Parquet-Parquet, sütunlu biçim olarak dönüştürür. Sıkıştırılmış olduğundan, dosya boyutları aynı verilerle CSV dosyalarından daha küçüktür. İsteğe bağlı SQL 'i okumak için daha az zaman ve depolama istekleri gerekecektir.
- Bir sorgu tek bir büyük dosyayı hedefliyorsa, onu birden çok daha küçük dosyaya bölmeyi avantajdan yararlanabilirsiniz.
- CSV dosyanızın boyutunu 10 GB 'tan daha düşük tutmaya çalışın.
- Tek bir OPENROWSET yolu veya dış tablo konumu için eşit boyutlu dosyalar olması daha iyidir.
- Bölümleri farklı klasörlere veya dosya adlarına depolayarak verilerinizi bölümleyin; [belirli bölümleri hedeflemek için dosya adı ve FilePath işlevlerini kullanın](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

## <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Belirli bölümleri hedeflemek için FileInfo ve FilePath işlevlerini kullanın

Veriler genellikle bölümler halinde düzenlenir. Belirli klasörleri ve dosyaları sorgulamak için isteğe bağlı SQL 'e bildirebilirsiniz. Bu işlev, sorgunun okuması ve işlemesi gereken dosya sayısını ve veri miktarını azaltır. Ek bir ödül daha iyi bir performans elde edersiniz.

Daha fazla bilgi için [dosya adı](develop-storage-files-overview.md#filename-function) ve [FilePath](develop-storage-files-overview.md#filepath-function) işlevlerini ve [belirli dosyaları sorgulama](query-specific-files.md)ile ilgili örnekleri denetleyin.

Depolanan verileriniz bölümlenmemişse, bu dosyaları hedefleyen sorguları iyileştirmek için bu işlevleri kullanabilmek üzere Bölümlendirmeyi düşünün. [Bölümlenmiş Spark TABLOLARıNı](develop-storage-files-spark-tables.md) SQL isteğe bağlı olarak sorgularken, sorgu yalnızca gerekli dosyaları hedefleyecek.

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Sorgu performansını ve birleştirmeleri geliştirmek için CETAS kullanın

[Cetas](develop-tables-cetas.md) , SQL isteğe bağlı olarak sunulan en önemli özelliklerden biridir. CETAS, dış tablo meta verileri oluşturan ve SELECT sorgu sonuçlarını Depolama hesabınızdaki bir dosya kümesine dışarı aktaran paralel bir işlemdir.

Birleşik başvuru tabloları gibi sorguların sık kullanılan parçalarını yeni bir dosya kümesine depolamak için CETAS kullanabilirsiniz. Daha sonra, birden çok sorgu içinde ortak birleştirmeleri yinelemek yerine bu tek dış tabloya katabilirsiniz.

CETAS, Parquet dosyalarını oluşturduğunda, ilk sorgu bu dış tabloyu hedeflediğinde istatistikler otomatik olarak oluşturulur ve bu da gelişmiş performansa neden olur.

## <a name="next-steps"></a>Sonraki adımlar

Sık karşılaşılan sorunlar ve çözümleri için [sorun giderme](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesini inceleyin. İsteğe bağlı SQL yerine SQL havuzu ile çalışıyorsanız, lütfen belirli yönergeler için [SQL havuzu Için En Iyi uygulamalar](best-practices-sql-pool.md) makalesine bakın.
