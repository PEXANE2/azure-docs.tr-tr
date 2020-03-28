---
title: 'Öğretici: Çok kiracılı bir veritabanı tasarla - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı'
description: Bu öğretici, PostgreSQL Hyperscale (Citus) için Azure Veritabanı'nda dağıtılmış tabloların nasıl oluşturulup doldurulur ve sorgulanır.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 05/14/2019
ms.openlocfilehash: 17ac29de243f4abfff1cfc83fc6424799978bf0e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74978160"
---
# <a name="tutorial-design-a-multi-tenant-database-by-using-azure-database-for-postgresql--hyperscale-citus"></a>Öğretici: PostgreSQL için Azure Veritabanını kullanarak çok kiracılı bir veritabanı tasarla – Hyperscale (Citus)

Bu öğreticide, postgreSQL için Azure Veritabanını kullanın - Hyperscale (Citus) nasıl yapılacağını öğrenmek için:

> [!div class="checklist"]
> * Hiper Ölçek (Citus) (Citus) sunucu grubu oluşturma
> * Şema oluşturmak için psql yardımcı programını kullanma
> * Düğümler arasında shard tablolar
> * Örnek verileri ekleme
> * Kiracı verilerini sorgula
> * Verileri kiracılar arasında paylaşma
> * Kiracı başına şema özelleştirme

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="use-psql-utility-to-create-a-schema"></a>Şema oluşturmak için psql yardımcı programını kullanma

Psql kullanarak PostgreSQL - Hyperscale (Citus) için Azure Veritabanına bağlandıktan sonra bazı temel görevleri tamamlayabilirsiniz. Bu öğretici, reklamverenlerin kampanyalarını izlemelerine olanak tanıyan bir web uygulaması oluşturmada size yol alar.

Birden fazla şirket uygulamayı kullanabilir, bu nedenle şirketleri tutmak için bir tablo ve kampanyaları için başka bir tablo oluşturalım. psql konsolunda şu komutları çalıştırın:

```sql
CREATE TABLE companies (
  id bigserial PRIMARY KEY,
  name text NOT NULL,
  image_url text,
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL
);

CREATE TABLE campaigns (
  id bigserial,
  company_id bigint REFERENCES companies (id),
  name text NOT NULL,
  cost_model text NOT NULL,
  state text NOT NULL,
  monthly_budget bigint,
  blacklisted_site_urls text[],
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL,

  PRIMARY KEY (company_id, id)
);
```

Her kampanya reklam yayınlamak için ödeme yapar. Yukarıdaki koddan sonra psql'de aşağıdaki kodu çalıştırarak reklamlar için de bir tablo ekleyin:

```sql
CREATE TABLE ads (
  id bigserial,
  company_id bigint,
  campaign_id bigint,
  name text NOT NULL,
  image_url text,
  target_url text,
  impressions_count bigint DEFAULT 0,
  clicks_count bigint DEFAULT 0,
  created_at timestamp without time zone NOT NULL,
  updated_at timestamp without time zone NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, campaign_id)
    REFERENCES campaigns (company_id, id)
);
```

Son olarak, her reklam için tıklamalar ve gösterimlerle ilgili istatistikleri izleyeceğiz:

```sql
CREATE TABLE clicks (
  id bigserial,
  company_id bigint,
  ad_id bigint,
  clicked_at timestamp without time zone NOT NULL,
  site_url text NOT NULL,
  cost_per_click_usd numeric(20,10),
  user_ip inet NOT NULL,
  user_data jsonb NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, ad_id)
    REFERENCES ads (company_id, id)
);

CREATE TABLE impressions (
  id bigserial,
  company_id bigint,
  ad_id bigint,
  seen_at timestamp without time zone NOT NULL,
  site_url text NOT NULL,
  cost_per_impression_usd numeric(20,10),
  user_ip inet NOT NULL,
  user_data jsonb NOT NULL,

  PRIMARY KEY (company_id, id),
  FOREIGN KEY (company_id, ad_id)
    REFERENCES ads (company_id, id)
);
```

Yeni oluşturulan tabloları psql'de şimdi psql'de çalıştırarak görebilirsiniz:

```postgres
\dt
```

Çok kiracılı uygulamalar yalnızca kiracı başına benzersizliği zorlayabilir, bu nedenle tüm birincil ve yabancı anahtarlar şirket kimliğini içerir.

## <a name="shard-tables-across-nodes"></a>Düğümler arasında shard tablolar

Hiper ölçekli dağıtım, kullanıcı tarafından atanan sütunun değerini temel alan tablo satırlarını farklı düğümlerde depolar. Bu "dağıtım sütunu" kiracının hangi satırlara sahip olduğunu işaretler.

Dağıtım sütununa şirket\_kimliği, kiracı tanımlayıcısı olarak ayarlayalım. psql'de şu işlevleri çalıştırın:

```sql
SELECT create_distributed_table('companies',   'id');
SELECT create_distributed_table('campaigns',   'company_id');
SELECT create_distributed_table('ads',         'company_id');
SELECT create_distributed_table('clicks',      'company_id');
SELECT create_distributed_table('impressions', 'company_id');
```

## <a name="ingest-sample-data"></a>Örnek verileri ekleme

Şimdi psql dışında, normal komut satırında, örnek veri kümelerini indirin:

```bash
for dataset in companies campaigns ads clicks impressions geo_ips; do
  curl -O https://examples.citusdata.com/mt_ref_arch/${dataset}.csv
done
```

Psql'in içine geri dön, verileri toplu yükleyin. Veri dosyalarını indirdiğiniz aynı dizinde psql çalıştırdığınızdan emin olun.

```sql
SET CLIENT_ENCODING TO 'utf8';

\copy companies from 'companies.csv' with csv
\copy campaigns from 'campaigns.csv' with csv
\copy ads from 'ads.csv' with csv
\copy clicks from 'clicks.csv' with csv
\copy impressions from 'impressions.csv' with csv
```

Bu veriler artık alt düğümlere yayılacak.

## <a name="query-tenant-data"></a>Kiracı verilerini sorgula

Uygulama tek bir kiracı için veri istediğinde, veritabanı sorguyu tek bir alt düğümüzerinde yürütebilir. Tek kiracılı sorgular tek bir kiracı kimliğine göre filtrefiltreler. Örneğin, reklamlar ve gösterimler için aşağıdaki sorgu filtreleri. `company_id = 5` Sonuçları görmek için psql çalıştırarak deneyin.

```sql
SELECT a.campaign_id,
       RANK() OVER (
         PARTITION BY a.campaign_id
         ORDER BY a.campaign_id, count(*) desc
       ), count(*) as n_impressions, a.id
  FROM ads as a
  JOIN impressions as i
    ON i.company_id = a.company_id
   AND i.ad_id      = a.id
 WHERE a.company_id = 5
GROUP BY a.campaign_id, a.id
ORDER BY a.campaign_id, n_impressions desc;
```

## <a name="share-data-between-tenants"></a>Verileri kiracılar arasında paylaşma

Şimdiye kadar tüm tablolar `company_id`tarafından dağıtıldı, ancak bazı veriler doğal olarak özellikle herhangi bir kiracıya "ait" değildir ve paylaşılabilir. Örneğin, örnek reklam platformundaki tüm şirketler, IP adreslerine bağlı olarak hedef kitlelerine coğrafi bilgi almak isteyebilir.

Paylaşılan coğrafi bilgileri tutmak için bir tablo oluşturun. Psql'de aşağıdaki komutları çalıştırın:

```sql
CREATE TABLE geo_ips (
  addrs cidr NOT NULL PRIMARY KEY,
  latlon point NOT NULL
    CHECK (-90  <= latlon[0] AND latlon[0] <= 90 AND
           -180 <= latlon[1] AND latlon[1] <= 180)
);
CREATE INDEX ON geo_ips USING gist (addrs inet_ops);
```

Sonraki `geo_ips` her işçi düğümünde tablonun bir kopyasını depolamak için bir "başvuru tablosu" olun.

```sql
SELECT create_reference_table('geo_ips');
```

Örnek verilerle yükleyin. Bu komutu veri kümesini indirdiğiniz dizinin içinden psql'de çalıştırmayı unutmayın.

```sql
\copy geo_ips from 'geo_ips.csv' with csv
```

Tıklama tablosuna geo\_ips ile katılmak tüm düğümlerde etkilidir.
Burada reklam tıklayan herkesin konumlarını bulmak için bir birleştirme
290. Sorguyu psql'de çalıştırmayı deneyin.

```sql
SELECT c.id, clicked_at, latlon
  FROM geo_ips, clicks c
 WHERE addrs >> c.user_ip
   AND c.company_id = 5
   AND c.ad_id = 290;
```

## <a name="customize-the-schema-per-tenant"></a>Kiracı başına şema özelleştirme

Her kiracının başkaları tarafından gerekmeyen özel bilgileri depolaması gerekebilir. Ancak, tüm kiracılar aynı veritabanı şeması ile ortak bir altyapı paylaşır. İlave veriler nereye gidebilir?

Bir hile PostgreSQL JSONB gibi açık uçlu sütun türü kullanmaktır.  Şemamızın `clicks` jsonb `user_data`alanı var.
Bir şirket (şirket beş deyin), kullanıcının mobil cihazda olup olmadığını izlemek için sütunu kullanabilir.

Burada kimlerin daha fazla tıkladığına dikkat çeken bir sorgu ver: mobil veya geleneksel ziyaretçiler.

```sql
SELECT
  user_data->>'is_mobile' AS is_mobile,
  count(*) AS count
FROM clicks
WHERE company_id = 5
GROUP BY user_data->>'is_mobile'
ORDER BY count DESC;
```

Kısmi [bir dizin](https://www.postgresql.org/docs/current/static/indexes-partial.html)oluşturarak bu sorguyabiliriz.

```sql
CREATE INDEX click_user_data_is_mobile
ON clicks ((user_data->>'is_mobile'))
WHERE company_id = 5;
```

Daha genel olarak, sütundaki her anahtar ve değer için bir [GIN dizinleri](https://www.postgresql.org/docs/current/static/gin-intro.html) oluşturabiliriz.

```sql
CREATE INDEX click_user_data
ON clicks USING gin (user_data);

-- this speeds up queries like, "which clicks have
-- the is_mobile key present in user_data?"

SELECT id
  FROM clicks
 WHERE user_data ? 'is_mobile'
   AND company_id = 5;
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir sunucu grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymayı beklemiyorsanız, sunucu grubunu silin. Sunucu grubunuzun *Genel Bakış* *sayfasındasil* düğmesine basın. Açılır pencerede istendiğinde, sunucu grubunun adını onaylayın ve son *Sil* düğmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Hyperscale (Citus) sunucu grubunu nasıl sağlarsınız. Ona psql ile bağlandınız, bir şema oluşturdunuz ve veri dağıttınız. Hem kiracılar içinde hem de kiracılar arasında veri sorgulamayı ve kiracı başına şemayı özelleştirmeyi öğrendiniz.

Ardından, hiperölçek kavramları hakkında bilgi edinin.
> [!div class="nextstepaction"]
> [Hiper ölçekli düğüm türleri](https://aka.ms/hyperscale-concepts)
