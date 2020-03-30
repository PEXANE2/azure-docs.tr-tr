---
title: Dağıtılmış veriler – Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: PostgreSQL için Azure Veritabanı'nda dağıtılmış tablolar, başvuru tabloları, yerel tablolar ve kırıklar hakkında bilgi edinin.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: ade7632dc042741a07bdb59e34e30b3fb464e0e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79243659"
---
# <a name="distributed-data-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure Veritabanında dağıtılmış veriler – Hyperscale (Citus)

Bu makalede, PostgreSQL için Azure Veritabanı'ndaki üç tablo türü özetlenmiştir - Hyperscale (Citus).
Dağıtılmış tabloların parça olarak nasıl depolanır ve düğümlere parçaların nasıl yerleştirildiğini gösterir.

## <a name="table-types"></a>Tablo türleri

Bir Hyperscale (Citus) sunucu grubunda, her biri farklı amaçlar için kullanılan üç tür tablo vardır.

### <a name="type-1-distributed-tables"></a>Tip 1: Dağıtılmış tablolar

İlk tür ve en yaygın, dağıtılmış tablolar. SQL deyimleri için normal tablolar gibi görünürler, ancak alt düğümler arasında yatay olarak bölümlere ayrılmıştır. Bunun anlamı, tablonun satırlarının parça adı verilen parça tablolarda farklı düğümlerde depolanmasıdır.

Hyperscale (Citus) bir küme boyunca yalnızca SQL değil, DDL deyimlerini de çalıştırAr.
Dağıtılmış bir tablonun şemasını değiştirme, tablonun tüm parçalarını çalışanlar arasında güncelleştirmek için basamaklar.

#### <a name="distribution-column"></a>Dağıtım sütunu

Hyperscale (Citus), satırları kırıklara atamak için algoritmik parçalama kullanır. Atama, dağıtım sütunu adı verilen bir tablo sütununun değerine göre deterministically yapılır. Küme yöneticisi, bir tablo dağıtırken bu sütunu belirlemelidir.
Doğru seçimi yapmak performans ve işlevsellik için önemlidir.

### <a name="type-2-reference-tables"></a>Tip 2: Başvuru tabloları

Başvuru tablosu, tüm içeriği tek bir parçaya yoğunlaşmış dağıtılmış tablo türüdür. Parça her işçide kopyalanır. Herhangi bir çalışandaki sorgular, başka bir düğümden satır isteme ağı yükü olmadan başvuru bilgilerine yerel olarak erişebilir. Satır başına ayrı parçaları ayırmaya gerek olmadığından, başvuru tablolarının dağıtım sütunu yoktur.

Başvuru tabloları genellikle küçüktür ve herhangi bir alt düğümüzerinde çalışan sorgularla ilgili verileri depolamak için kullanılır. Örnek, sipariş durumları veya ürün kategorileri gibi numaralandırılmış değerlerdir.

### <a name="type-3-local-tables"></a>Tür 3: Yerel tablolar

Hyperscale (Citus) kullandığınızda, bağlandığınız koordinatör düğüm normal bir PostgreSQL veritabanıdır. Koordinatör üzerinde sıradan tablolar oluşturabilir ve bunları parçalamayarak seçim ememezsiniz.

Yerel tablolar için iyi bir aday, birleştirme sorgularına katılmayan küçük yönetim tabloları olacaktır. Bir örnek, uygulama oturum açma ve kimlik doğrulaması için bir kullanıcı tablosudur.

## <a name="shards"></a>Kırıklar

Önceki bölümde, dağıtılmış tabloların alt düğümlerde kırık olarak nasıl depolandığı açıklanmıştır. Bu bölümde daha fazla teknik ayrıntı tartışılmaktadır.

Koordinatörteki meta veri tablosu, `pg_dist_shard` sistemdeki her dağıtılmış tablonun her parçası için bir satır içerir. Satır, karma alanda (parça değeri, shardmaxvalue) tamsayı aralığıyla bir parça kimliğiyle eşleşir.

```sql
SELECT * from pg_dist_shard;
 logicalrelid  | shardid | shardstorage | shardminvalue | shardmaxvalue 
---------------+---------+--------------+---------------+---------------
 github_events |  102026 | t            | 268435456     | 402653183
 github_events |  102027 | t            | 402653184     | 536870911
 github_events |  102028 | t            | 536870912     | 671088639
 github_events |  102029 | t            | 671088640     | 805306367
 (4 rows)
```

Koordinatör düğüm hangi parçanın `github_events`bir satır tuttuğunu belirlemek istiyorsa, satırdaki dağıtım sütununun değerini işşer. Daha sonra shard\'s aralığının haşlanmış değerini içeren düğüm denetler. Aralıklar, karma işlevin görüntüsünün ayrık birleşimleri olacak şekilde tanımlanır.

### <a name="shard-placements"></a>Parça yerleşimleri

102027'nin söz konusu satırla ilişkili olduğunu varsayalım. Satır, işçilerden birinde adı `github_events_102027` verilen bir tabloda okunur veya yazılır. Hangi işçi? Bu tamamen meta veri tabloları tarafından belirlenir. Parçanın işçiye eşlenemesi, parça yerleşimi olarak bilinir.

Koordinatör düğüm, sorguları belirli tablolara başvuran parçalar halinde `github_events_102027` yeniden yazar ve bu parçaları uygun işçiler üzerinde çalıştırın. Burada, basılı kimlik 102027 tutan düğümü bulmak için arka planda çalıştırılatan bir sorgu örneği verilmiştir.

```sql
SELECT
    shardid,
    node.nodename,
    node.nodeport
FROM pg_dist_placement placement
JOIN pg_dist_node node
  ON placement.groupid = node.groupid
 AND node.noderole = 'primary'::noderole
WHERE shardid = 102027;
```

    ┌─────────┬───────────┬──────────┐
    │ shardid │ nodename  │ nodeport │
    ├─────────┼───────────┼──────────┤
    │  102027 │ localhost │     5433 │
    └─────────┴───────────┴──────────┘

## <a name="next-steps"></a>Sonraki adımlar
- Dağıtılmış tablolar için [dağıtım sütunu](concepts-hyperscale-choose-distribution-column.md) nasıl seçeceğinizi öğrenin.
