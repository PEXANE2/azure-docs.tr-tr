---
title: 'Hızlı başlangıç: sunucu grubu oluşturma-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı'
description: PostgreSQL için Azure veritabanı hiper ölçek (Citus) üzerinde dağıtılmış tablolar oluşturmak ve sorgulamak için hızlı başlangıç.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 08/17/2020
ms.openlocfilehash: 03a6e927a074067e85f1a3adca38cae386d1af38
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "95026225"
---
# <a name="quickstart-create-a-hyperscale-citus-server-group-in-the-azure-portal"></a>Hızlı başlangıç: Azure portal hiper ölçek (Citus) sunucu grubu oluşturma

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız, yönetilen bir hizmettir. Bu hızlı başlangıçta, Azure portal kullanarak bir PostgreSQL için Azure veritabanı-hiper ölçek (Citus) sunucu grubu oluşturma gösterilmektedir. Dağıtılmış verileri keşfedeceğiz: düğümler arasında parça tabloları, örnek verileri geri almak ve birden çok düğümde yürütülen sorguları çalıştırmak isteyeceksiniz.

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="create-and-distribute-tables"></a>Tablo oluşturma ve dağıtma

Psql kullanarak hiper ölçek düzenleyici düğümüne bağlandıktan sonra bazı temel görevleri tamamlayabilirsiniz.

Hiper ölçek (Citus) sunucuları içinde üç tür tablo vardır:

- Dağıtılmış veya parçalı tablolar (performans ve paralelleştirme için ölçeklendirilmesine yardımcı olmak için yayılmış)
- Başvuru tabloları (birden fazla kopya korunur)
- Yerel tablolar (genellikle dahili yönetici tabloları için kullanılır)

Bu hızlı başlangıçta, öncelikle dağıtılmış tablolara odaklanacağız ve bunlarla ilgili bilgi edineceğiz.

Çalışacağız veri modeli basittir: GitHub 'dan Kullanıcı ve olay verileri. Olaylar çatal oluşturma, bir kuruluşla ilgili git işlemeleri ve daha fazlasını içerir.

Psql ile bağlandıktan sonra tablolarımızı oluşturalım. Psql konsolunda şunu çalıştırın:

```sql
CREATE TABLE github_events
(
    event_id bigint,
    event_type text,
    event_public boolean,
    repo_id bigint,
    payload jsonb,
    repo jsonb,
    user_id bigint,
    org jsonb,
    created_at timestamp
);

CREATE TABLE github_users
(
    user_id bigint,
    url text,
    login text,
    avatar_url text,
    gravatar_id text,
    display_login text
);
```

`payload`Alanının `github_events` BIR jsonb veri türü vardır. JSONB, Postgres 'de ikili biçimdeki JSON veri türüdür. Veri türü, esnek bir şemayı tek bir sütunda depolamayı kolaylaştırır.

Postgres `GIN` Bu türde bir dizin oluşturabilir ve bu, içindeki her anahtar ve değeri dizine alır. Bir dizin ile yükü çeşitli koşullarla sorgulamak hızlı ve kolay hale gelir. Biz de verilerimizi yüklemeden önce birkaç dizin oluşturalım. Psql 'de:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Ardından, düzenleyici düğümünde bu Postgres tablolarını alacak ve şirket genelinde bunları parçalara ayırmaları için hiper ölçeğe (Citus) söyleriz. Bunu yapmak için, üzerine gelecek anahtarı belirten her tablo için bir sorgu çalıştıracağız. Geçerli örnekte, hem olaylar hem de Kullanıcı tablosu ' nu parçalara parçalarız `user_id` :

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

[!INCLUDE [azure-postgresql-hyperscale-dist-alert](../../includes/azure-postgresql-hyperscale-dist-alert.md)]

Verileri yüklemeye hazırız. Hala psql 'de, dosyaları indirmek için Shell Out:

```sql
\! curl -O https://examples.citusdata.com/users.csv
\! curl -O https://examples.citusdata.com/events.csv
```

Sonra, verileri dosyaları dağıtılmış tablolara yükleyin:

```sql
SET CLIENT_ENCODING TO 'utf8';

\copy github_events from 'events.csv' WITH CSV
\copy github_users from 'users.csv' WITH CSV
```

## <a name="run-queries"></a>Sorgu çalıştırma

Şimdi eğlenceli parçanın süresi, aslında bazı sorgular çalıştırıyor. `count (*)`Ne kadar veri yüklediğimiz hakkında daha basit bir başlangıç yapın:

```sql
SELECT count(*) from github_events;
```

Bu, işe yaramakta. Bu toplama sıralamasına bir bit içinde geri döneceğiz, ancak şimdilik diğer birkaç sorguya göz atalım. JSONB sütununun içinde `payload` iyi bir veri bulunur, ancak olay türüne göre farklılık gösterir. `PushEvent` olaylar, gönderim için ayrı yürütmelerin sayısını içeren bir boyut içerir. Saat başına toplam işleme sayısını bulmak için kullanabiliriz:

```sql
SELECT date_trunc('hour', created_at) AS hour,
       sum((payload->>'distinct_size')::int) AS num_commits
FROM github_events
WHERE event_type = 'PushEvent'
GROUP BY hour
ORDER BY hour;
```

Şimdiye kadar sorgular GitHub \_ olaylarını özel olarak katıldı, ancak bu bilgileri GitHub kullanıcılarıyla birleştirebiliriz \_ . Aynı tanımlayıcı () üzerinde hem Kullanıcı hem de olay paylaşdığımız için `user_id` , eşleşen Kullanıcı kimliklerine sahip her iki tablonun satırları aynı veritabanı düğümlerine dahil [edilir](concepts-hyperscale-colocation.md) ve kolayca eklenebilir.

Katılıyoruz `user_id` , hiper ölçek (Citus), çalışan düğümlerinde paralel olarak yürütülmesi için birleştirmeyi parçalara ayırır. Örneğin, en fazla sayıda depo oluşturan kullanıcıları bulalım:

```sql
SELECT gu.login, count(*)
  FROM github_events ge
  JOIN github_users gu
    ON ge.user_id = gu.user_id
 WHERE ge.event_type = 'CreateEvent'
   AND ge.payload @> '{"ref_type": "repository"}'
 GROUP BY gu.login
 ORDER BY count(*) DESC;
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yukarıdaki adımlarda, bir sunucu grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymazsanız, sunucu grubunu silin. Sunucu grubunuzun **genel bakış** sayfasında **Sil** düğmesine basın. Bir açılır sayfada istendiğinde, sunucu grubunun adını onaylayın ve son **Sil** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir hiper ölçek (Citus) sunucu grubu sağlamayı öğrendiniz. Bu ağa psql ile bağlanırsınız, bir şema oluşturdunuz ve dağıtılmış veriler.

- [Ölçeklenebilir çok kiracılı uygulamalar oluşturmak](./tutorial-design-database-hyperscale-multi-tenant.md) için öğreticiyi izleyin
- Sunucu grubunuz için en iyi [ilk boyutu](howto-hyperscale-scale-initial.md) belirleme
