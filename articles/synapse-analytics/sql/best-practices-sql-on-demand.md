---
title: Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) için en iyi uygulamalar
description: İsteğe bağlı SQL (önizleme) ile çalışırken bilmeniz gereken öneriler ve en iyi uygulamalar.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429076"
---
# <a name="best-practices-for-sql-on-demand-preview-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te isteğe bağlı SQL (önizleme) için en iyi uygulamalar

Bu makalede, isteğe bağlı SQL (önizleme) kullanmak için en iyi uygulamalar koleksiyonunu bulacaksınız. İsteğe bağlı SQL, Azure Synapse Analytics'te ek bir kaynaktır.

## <a name="general-considerations"></a>Dikkat edilmesi gereken temel noktalar

İsteğe bağlı SQL, Azure depolama hesaplarınızdaki dosyaları sorgulamanızı sağlar. Yerel depolama veya yutma yetenekleri yoktur. Bu nedenle, sorgu hedeflerinin SQL on-demand'in dışında olduğu tüm dosyalar. Depolama dan dosya okuma ile ilgili her şey sorgu performansı üzerinde bir etkisi olabilir.

## <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Azure Depolama hesabını ve SQL isteğe bağlı olarak birlikte bulun

Gecikme yi en aza indirmek için Azure depolama hesabınızı ve SQL isteğe bağlı bitiş noktanızı birlikte bulunduun. Çalışma alanı oluşturma sırasında sağlanan depolama hesapları ve uç noktaları aynı bölgede bulunur.

En iyi performans için, SQL isteğe bağlı diğer depolama hesaplarına erişirseniz, aynı bölgede olduğundan emin olun. Aynı bölgede değillerse, uzak ve bitiş noktası bölgeleri arasında veri ağ aktarımı için gecikme sayılmı artar.

## <a name="azure-storage-throttling"></a>Azure Depolama azaltma

Depolama hesabınıza birden çok uygulama ve hizmet erişebilir. Depolama azaltma, uygulamalar, hizmetler ve SQL isteğe bağlı iş yükü tarafından oluşturulan birleşik IOPS veya iş yükü depolama hesabının sınırlarını aştığında oluşur. Sonuç olarak, sorgu performansı üzerinde önemli bir olumsuz etki yaşarsınız.

Azaltma algılandıktan sonra, SQL isteğe bağlı olarak bu senaryoyu yerleşik olarak ele almıştır. İsteğe bağlı SQL, azaltma çözülene kadar depolama isteklerini daha yavaş bir hızda yapar.

> [!TIP]
> En iyi sorgu yürütmesi için, sorgu yürütme sırasında depolama hesabını diğer iş yükleriyle vurgulamamalısınız.

## <a name="prepare-files-for-querying"></a>Sorgu için dosya hazırlama

Mümkünse, dosyaları daha iyi performans için hazırlayabilirsiniz:

- CSV'yi Parke Dönüştürün - Parke columnar formatındadır. Sıkıştırılmış olduğundan, dosya boyutları aynı verilere sahip CSV dosyalarından daha küçüktür. İsteğe bağlı SQL'in okumak için daha az zamana ve depolama isteklerine ihtiyacı olacaktır.
- Bir sorgu tek bir büyük dosyayı hedefliyorsa, dosyayı birden çok küçük dosyaya bölmenin avantajını bulursunuz.
- CSV dosya boyutunuzu 10 GB'ın altında tutmayı deneyin.
- Tek bir OPENROWSET yolu veya harici bir tablo KONUMU için eşit büyüklükte dosyalara sahip olmak daha iyidir.
- Bölümlerinizi farklı klasörlere veya dosya adlarına depolayarak verilerinizi bölme - [belirli bölümleri hedeflemek için dosya adı ve dosya yolu işlevlerini kullanın.](#use-fileinfo-and-filepath-functions-to-target-specific-partitions)

## <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Belirli bölümleri hedeflemek için dosya bilgisi ve dosya yolu işlevlerini kullanma

Veriler genellikle bölümler halinde düzenlenir. Belirli klasörleri ve dosyaları sorgulamak için SQL on-demand'e talimat verebilirsiniz. Bu işlev, sorgunun okuması ve işlemesi gereken dosya sayısını ve veri miktarını azaltır. Ek bir bonus daha iyi performans elde edeceğiz.

Daha fazla bilgi [için, dosya adı](develop-storage-files-overview.md#filename-function) ve dosya [yolu](develop-storage-files-overview.md#filepath-function) işlevlerini ve belirli dosyaları nasıl [sorgulayabildiğini](query-specific-files.md)denetleyin.

Depolanan verileriniz bölümlenmiyorsa, bu dosyaları hedefleyen sorguları en iyi duruma getirmek için bu işlevleri kullanabilmek için bu verileri bölümlemeyi düşünün. [Bölümlenmiş Spark tablolarını](develop-storage-files-spark-tables.md) isteğe bağlı OLARAK SQL'den sorgularken, sorgu otomatik olarak yalnızca gerekli dosyaları hedefler.

## <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Sorgu performansını artırmak için CETAS'ı kullanın ve birleştirir

[CETAS,](develop-tables-cetas.md) isteğe bağlı SQL'de mevcut olan en önemli özelliklerden biridir. CETAS, dış tablo meta verileri oluşturan ve SELECT sorgu sonuçlarını depolama hesabınızdaki bir dosya kümesine aktaran paralel bir işlemdir.

CETAS'ı, birleştirilmiş başvuru tabloları gibi sık kullanılan sorgu bölümlerini yeni bir dosya kümesinde depolamak için kullanabilirsiniz. Ardından, birden çok sorguda ortak birleştirmeleri yinelemek yerine bu tek dış tabloya katılabilirsiniz.

CETAŞ Parke dosyaları oluşturduğunda, ilk sorgu bu dış tabloyu hedefaldığında istatistikler otomatik olarak oluşturulur ve bu da performansın iyileştirilmesine neden olur.

## <a name="next-steps"></a>Sonraki adımlar

Sık karşılaşılan sorunlar ve çözümler için [Sorun Giderme](../sql-data-warehouse/sql-data-warehouse-troubleshoot.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) makalesini gözden geçirin. İsteğe bağlı OLARAK SQL havuzu yerine SQL [havuzuyla](best-practices-sql-pool.md) çalışıyorsanız, belirli kılavuzlar için lütfen SQL havuzu için En İyi Uygulamalar makalesine bakın.
