---
title: Dağıtılmış veri – Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı 'nda dağıtılmış tablolar, başvuru tabloları, yerel tablolar ve parçalar hakkında bilgi edinin.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 7757fdb4953640597a805c3d74a9e1ef08ef2c07
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86114508"
---
# <a name="distributed-data-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda dağıtılmış veriler – hiper ölçek (Citus)

Bu makalede PostgreSQL için Azure veritabanı 'nın (Citus) üç tablo türü özetlenmektedir.
Dağıtılmış tabloların parçalar halinde nasıl depolandığını ve parçaların düğümlerin üzerine yerleştirilme şeklini gösterir.

## <a name="table-types"></a>Tablo türleri

Her biri farklı amaçlar için kullanılan bir hiper ölçek (Citus) sunucu grubunda üç tür tablo vardır.

### <a name="type-1-distributed-tables"></a>Tür 1: dağıtılmış tablolar

İlk tür ve en yaygın, dağıtılmış tablolardır. Bunlar SQL deyimlerine normal tablolar gibi görünürler, ancak bunlar alt düğümlerde yatay olarak bölümlendirilir. Bunun anlamı, tablo satırlarının parçalar olarak adlandırılan parça tablolarında farklı düğümlerde depolanmasıdır.

Hiper ölçek (Citus), küme genelinde yalnızca SQL ancak DDL deyimlerini değil.
Dağıtılmış tablo basamaklarının şemasını değiştirerek tüm tablo parçaları çalışanlar arasında güncelleştirin.

#### <a name="distribution-column"></a>Dağıtım sütunu

Hiper ölçek (Citus), parçaları parçalara eklemek için algoritmik parçalama kullanır. Atama, dağıtım sütunu olarak adlandırılan tablo sütununun değerine göre belirleyici olarak yapılır. Küme yöneticisinin bir tabloyu dağıtırken bu sütunu belirlemesi gerekir.
Performans ve işlevsellik için doğru seçimi yapmak önemlidir.

### <a name="type-2-reference-tables"></a>Tür 2: başvuru tabloları

Başvuru tablosu, tüm içerikleri tek bir parça halinde yoğunlaştı olan bir dağıtılmış tablo türüdür. Parça her çalışan üzerinde çoğaltılır. Herhangi bir çalışan sorguları, başka bir düğümden satır isteme ağ ek yükü olmadan başvuru bilgilerine yerel olarak erişebilir. Satır başına ayrı parçaları ayırt etmenize gerek olmadığı için başvuru tablolarında dağıtım sütunu yoktur.

Başvuru tabloları genellikle küçüktür ve herhangi bir çalışan düğümünde çalışan sorgularla ilgili verileri depolamak için kullanılır. Sipariş durumları veya ürün kategorileri gibi bir örnek olarak numaralandırılan değerler aşağıda bulunur.

### <a name="type-3-local-tables"></a>Tür 3: yerel tablolar

Hyperscale (Citus) kullandığınızda, bağlandığınız düzenleyici düğümü normal bir PostgreSQL veritabanıdır. Düzenleyici üzerinde sıradan tablolar oluşturabilir ve bunları parçalara parçalamayı seçemezsiniz.

Yerel tablolar için iyi bir aday, JOIN sorgularına katılmayan küçük yönetim tabloları olacaktır. Bir örnek, uygulama oturum açma ve kimlik doğrulama için bir Kullanıcı tablosudur.

## <a name="shards"></a>Parçalar

Önceki bölümde, dağıtılmış tabloların çalışan düğümlerinde parçalar halinde nasıl depolandığı açıklanmıştır. Bu bölümde daha fazla teknik ayrıntı açıklanmaktadır.

`pg_dist_shard`Düzenleyicinizdeki meta veri tablosu, sistemdeki her bir dağıtılmış tablonun her biri için bir satır içerir. Satır bir karma alanındaki (shardminvalue, shardmaxvalue) bir dizi tamsayının bulunduğu parça KIMLIĞIYLE eşleşir.

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

Düzenleyici düğümü, hangi parçanın bir satırı bulundurduğunu belirleme istiyorsa `github_events` , satırdaki dağıtım sütununun değerini karma hale getirir. Sonra düğüm, hangi parça \' aralığının karma değeri içerdiğini denetler. Aralıklar, karma işlevin görüntüsünün ayrık birleşimi olması için tanımlanır.

### <a name="shard-placements"></a>Parça yerleştirme

Parça 102027 ' nin söz konusu satırla ilişkili olduğunu varsayalım. Satır, çalışanlarından birinde çağrılan bir tabloda okunabilir veya yazılır `github_events_102027` . Hangi çalışan? Bu, tamamen meta veri tabloları tarafından belirlenir. Parça ile çalışan arasındaki eşleme, parça yerleşimi olarak bilinir.

Düzenleyici düğümü, gibi belirli tablolara başvuran `github_events_102027` ve ilgili çalışanlar üzerinde bu parçaları çalıştıran parçalara sorguları yeniden yazar. Parça KIMLIĞI 102027 olan düğümü bulmak için arka planda çalıştırılan bir sorgu örneği aşağıda verilmiştir.

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

```output
┌─────────┬───────────┬──────────┐
│ shardid │ nodename  │ nodeport │
├─────────┼───────────┼──────────┤
│  102027 │ localhost │     5433 │
└─────────┴───────────┴──────────┘
```

## <a name="next-steps"></a>Sonraki adımlar

- Dağıtılmış tablolar için [bir dağıtım sütunu seçme](concepts-hyperscale-choose-distribution-column.md) hakkında bilgi edinin.
