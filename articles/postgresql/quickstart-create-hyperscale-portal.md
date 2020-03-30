---
title: Dağıtılmış tablolar oluşturma - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: PostgreSQL Hyperscale (Citus) için Azure Veritabanı'nda dağıtılmış tablolar oluşturmaya ve sorgulamaya hızlı başlayın.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/14/2019
ms.openlocfilehash: 02e009e6fff2e717693d1579d409199ab179d941
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241517"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---hyperscale-citus-in-the-azure-portal"></a>Hızlı başlatma: Azure portalında PostgreSQL - Hyperscale (Citus) için bir Azure Veritabanı oluşturma

PostgreSQL için Azure Veritabanı, bulutta son derece kullanılabilir olan PostgreSQL veritabanlarını çalıştırmak, yönetmek ve ölçeklendirmek için kullandığınız, yönetilen bir hizmettir. Bu Quickstart, Azure portalını kullanarak PostgreSQL - Hyperscale (Citus) sunucu grubu için nasıl bir Azure Veritabanı oluşturabileceğinizi gösterir. Dağıtılmış verileri keşfedeceksiniz: düğümler arasında tabloları niçin parçalama, örnek verileri yutma ve birden çok düğümde çalışan sorguları çalıştırma.

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="create-and-distribute-tables"></a>Tablo oluşturma ve dağıtma

Psql kullanarak hiperölçekli koordinatör düğüme bağlandıktan sonra bazı temel görevleri tamamlayabilirsiniz.

Hyperscale sunucularında üç tür tablo vardır:

- Dağıtılmış veya büçğülü tablolar (performans ve paralelleştirme için ölçeklendirmeye yardımcı olmak için yayılır)
- Başvuru tabloları (birden çok kopya tutuldu)
- Yerel tablolar (genellikle iç yönetici tabloları için kullanılır)

Bu hızlı başlangıçta, öncelikle dağıtılmış tablolara odaklanacağız ve bunları öğreneceğiz.

Üzerinde çalışacağımız veri modeli basittir: GitHub'daki kullanıcı ve olay verileri. Olaylar çatal oluşturma, git bir kuruluş ile ilgili taahhüt ve daha fazlasını içerir.

Psql üzerinden bağlandıktan sonra tablolarımızı oluşturalım. psql konsol çalıştırın:

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

Alanın `payload` JSONB veri tipi `github_events` vardır. JSONB Postgres ikili formda JSON veri türüdür. Veri türü, esnek bir şemayı tek bir sütunda depolamayı kolaylaştırır.

Postgres, bu `GIN` türdeki her anahtarı ve değeri dizine ekleyecek bir dizin oluşturabilir. Bir dizin ile, hızlı ve çeşitli koşullarla yük sorgulamak kolay olur. Verilerimizi yüklemeden önce birkaç dizin oluşturalım. Psql olarak:

```sql
CREATE INDEX event_type_index ON github_events (event_type);
CREATE INDEX payload_index ON github_events USING GIN (payload jsonb_path_ops);
```

Sonra koordinatör düğümdeki Postgres masalarını alacağız ve Hyperscale'e onları işçiler arasında parçalamalarını söyleyeceğiz. Bunu yapmak için, her tablo için üzerinde parçalamak için anahtar belirten bir sorgu çalıştırırız. Geçerli örnekte hem olayları hem de kullanıcılar tablosunu `user_id`parçalayacağız:

```sql
SELECT create_distributed_table('github_events', 'user_id');
SELECT create_distributed_table('github_users', 'user_id');
```

Veri yüklemeye hazırız. Psql hala, dosyaları indirmek için dışarı kabuk:

```sql
\! curl -O https://examples.citusdata.com/users.csv
\! curl -O https://examples.citusdata.com/events.csv
```

Ardından, dosyalardaki verileri dağıtılmış tablolara yükleyin:

```sql
SET CLIENT_ENCODING TO 'utf8';

\copy github_events from 'events.csv' WITH CSV
\copy github_users from 'users.csv' WITH CSV
```

## <a name="run-queries"></a>Sorgu çalıştırma

Şimdi eğlenceli kısmı için zamanı, aslında bazı sorgular çalışan. Ne kadar veri yüklediğimizi görmek için basit `count (*)` bir şeyle başlayalım:

```sql
SELECT count(*) from github_events;
```

Çok işe yaradı. Birazdan bu tür bir toplama geri geleceğiz, ama şimdi birkaç sorgu daha bakalım. JSONB `payload` sütununda iyi bir veri parçası vardır, ancak olay türüne göre değişir. `PushEvent`olaylar, itme için ayrı commits sayısını içeren bir boyut içerir. Bunu, saatte toplam taahhüt sayısını bulmak için kullanabiliriz:

```sql
SELECT date_trunc('hour', created_at) AS hour,
       sum((payload->>'distinct_size')::int) AS num_commits
FROM github_events
WHERE event_type = 'PushEvent'
GROUP BY hour
ORDER BY hour;
```

Şimdiye kadar sorgular sadece github\_olayları dahil var, ama biz\_github kullanıcıları ile bu bilgileri birleştirebilirsiniz. Aynı tanımlayıcıda hem kullanıcıları hem de olayları parçaladığımıziçin,`user_id`eşleşen kullanıcı kimlikleriyle her iki tablonun satırları aynı veritabanı düğümlerinde [biraraya](https://docs.citusdata.com/en/stable/sharding/data_modeling.html#colocation) gelecek ve kolayca birlenebilebilir.

Eğer `user_id`katılırsak, Hyperscale birleştirme yürütmesini alt düğümlere paralel olarak yürütme için parçalara itebilir. Örneğin, en çok sayıda depo oluşturan kullanıcıları bulalım:

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

Önceki adımlarda, bir sunucu grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymayı beklemiyorsanız, sunucu grubunu silin. Sunucu grubunuzun **Genel Bakış** **sayfasındasil** düğmesine basın. Açılır pencerede istendiğinde, sunucu grubunun adını onaylayın ve son **Sil** düğmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Hyperscale (Citus) sunucu grubunu nasıl sağlayabileceğinizi öğrendiniz. Ona psql ile bağlandınız, bir şema oluşturdunuz ve veri dağıttınız.

Ardından, ölçeklenebilir çok kiracılı uygulamalar oluşturmak için bir öğretici izleyin.
> [!div class="nextstepaction"]
> [Çok Kiracılı Veritabanı Tasarla](https://aka.ms/hyperscale-tutorial-multi-tenant)
