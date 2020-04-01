---
title: 'Öğretici: PostgreSQL için gerçek zamanlı bir pano tasarla - Hyperscale (Citus) - Azure Veritabanı'
description: Bu öğretici, PostgreSQL Hyperscale (Citus) için Azure Veritabanı'nda dağıtılmış tabloların nasıl oluşturulup doldurulur ve sorgulanır.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.custom: mvc
ms.topic: tutorial
ms.date: 05/14/2019
ms.openlocfilehash: f4eeb646de8b68c2c8d30586d0c75cece5317e40
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76716321"
---
# <a name="tutorial-design-a-real-time-analytics-dashboard-by-using-azure-database-for-postgresql--hyperscale-citus"></a>Öğretici: PostgreSQL için Azure Veritabanını kullanarak gerçek zamanlı analiz panosu tasarlayın – Hyperscale (Citus)

Bu öğreticide, postgreSQL için Azure Veritabanını kullanın - Hyperscale (Citus) nasıl yapılacağını öğrenmek için:

> [!div class="checklist"]
> * Hiper Ölçek (Citus) (Citus) sunucu grubu oluşturma
> * Şema oluşturmak için psql yardımcı programını kullanma
> * Düğümler arasında shard tablolar
> * Örnek veri oluşturma
> * Toplama işlemleri gerçekleştirin
> * Ham ve toplu verileri sorgula
> * Verilerin süresi nin dolması

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [azure-postgresql-hyperscale-create-db](../../includes/azure-postgresql-hyperscale-create-db.md)]

## <a name="use-psql-utility-to-create-a-schema"></a>Şema oluşturmak için psql yardımcı programını kullanma

Psql kullanarak PostgreSQL - Hyperscale (Citus) için Azure Veritabanına bağlandıktan sonra bazı temel görevleri tamamlayabilirsiniz. Bu öğretici, web analitiğinden trafik verilerini sindirerek, ardından bu verilere dayalı gerçek zamanlı panolar sağlamak için verileri yuvarlamanıza yol açtır.

Tüm ham web trafiği verilerimizi tüketecek bir tablo oluşturalım. Psql terminalinde aşağıdaki komutları çalıştırın:

```sql
CREATE TABLE http_request (
  site_id INT,
  ingest_time TIMESTAMPTZ DEFAULT now(),

  url TEXT,
  request_country TEXT,
  ip_address TEXT,

  status_code INT,
  response_time_msec INT
);
```

Ayrıca dakika başına toplamlarımızı tutacak bir masa ve son topladığımız ın konumunu koruyan bir tablo yaratacağız. Psql'de aşağıdaki komutları da çalıştırın:

```sql
CREATE TABLE http_request_1min (
  site_id INT,
  ingest_time TIMESTAMPTZ, -- which minute this row represents

  error_count INT,
  success_count INT,
  request_count INT,
  average_response_time_msec INT,
  CHECK (request_count = error_count + success_count),
  CHECK (ingest_time = date_trunc('minute', ingest_time))
);

CREATE INDEX http_request_1min_idx ON http_request_1min (site_id, ingest_time);

CREATE TABLE latest_rollup (
  minute timestamptz PRIMARY KEY,

  CHECK (minute = date_trunc('minute', minute))
);
```

Bu psql komutu ile şimdi tablolar listesinde yeni oluşturulan tabloları görebilirsiniz:

```postgres
\dt
```

## <a name="shard-tables-across-nodes"></a>Düğümler arasında shard tablolar

Hiper ölçekli dağıtım, kullanıcı tarafından atanan sütunun değerini temel alan tablo satırlarını farklı düğümlerde depolar. Bu "dağıtım sütunu", verilerin düğümler arasında nasıl parçalandığını işaretler.

Dağıtım sütununa site\_kimliği, parça anahtarı olarak ayarlayalım. psql'de şu işlevleri çalıştırın:

  ```sql
SELECT create_distributed_table('http_request',      'site_id');
SELECT create_distributed_table('http_request_1min', 'site_id');
```

## <a name="generate-sample-data"></a>Örnek veri oluşturma

Şimdi sunucu grubumuz bazı verileri yutmaya hazır olmalıdır. Sürekli veri eklemek için bağlantımızdan `psql` aşağıdakileri yerel olarak çalıştırabiliriz.

```sql
DO $$
  BEGIN LOOP
    INSERT INTO http_request (
      site_id, ingest_time, url, request_country,
      ip_address, status_code, response_time_msec
    ) VALUES (
      trunc(random()*32), clock_timestamp(),
      concat('http://example.com/', md5(random()::text)),
      ('{China,India,USA,Indonesia}'::text[])[ceil(random()*4)],
      concat(
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2), '.',
        trunc(random()*250 + 2)
      )::inet,
      ('{200,404}'::int[])[ceil(random()*2)],
      5+trunc(random()*150)
    );
    COMMIT;
    PERFORM pg_sleep(random() * 0.25);
  END LOOP;
END $$;
```

Sorgu her saniye yaklaşık sekiz satır ekler. Satırlar, dağıtım sütunu tarafından yönlendirilen farklı alt düğümlerde depolanır. `site_id`

   > [!NOTE]
   > Veri oluşturma sorgusunu çalışır durumda bırakın ve bu öğreticide kalan komutlar için ikinci bir psql bağlantısı açın.
   >

## <a name="query"></a>Sorgu

Hiper ölçekli barındırma seçeneği, birden çok düğümün sorguları hız için paralel olarak işlemesine olanak tanır. Örneğin, veritabanı toplamları işçi düğümlerinde TOPLAM ve COUNT gibi hesaplar ve sonuçları son yanıtta birleştirir.

Web isteklerini birkaç istatistikle birlikte dakika başına saymak için bir sorgu aşağıda veda edebilirsiniz.
Psql'de çalıştırmayı deneyin ve sonuçları gözlemleyin.

```sql
SELECT
  site_id,
  date_trunc('minute', ingest_time) as minute,
  COUNT(1) AS request_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
  SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
  SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
FROM http_request
WHERE date_trunc('minute', ingest_time) > now() - '5 minutes'::interval
GROUP BY site_id, minute
ORDER BY minute ASC;
```

## <a name="rolling-up-data"></a>Verileri yuvarlama

Önceki sorgu erken aşamalarında iyi çalışır, ancak verileriniz ölçeklendikçe performansı düşer. Dağıtılmış işleme bile, verileri tekrar tekrar hesaplamak için önceden hesaplamak için daha hızlıdır.

Ham verileri düzenli olarak bir toplu tabloya yuvarlayarak gösterge panelimizin hızlı kalmasını sağlayabiliriz. Toplama süresini deneyebilirsiniz. Dakikada toplama tablosu kullandık, ancak verileri 5, 15 veya 60 dakikaya ayırabilirsiniz.

Bu roll-up'ı daha kolay çalıştırmak için plpgsql fonksiyonuna koyacağız. İşlev oluşturmak için bu komutları psql'de çalıştırın. `rollup_http_request`

```sql
-- initialize to a time long ago
INSERT INTO latest_rollup VALUES ('10-10-1901');

-- function to do the rollup
CREATE OR REPLACE FUNCTION rollup_http_request() RETURNS void AS $$
DECLARE
  curr_rollup_time timestamptz := date_trunc('minute', now());
  last_rollup_time timestamptz := minute from latest_rollup;
BEGIN
  INSERT INTO http_request_1min (
    site_id, ingest_time, request_count,
    success_count, error_count, average_response_time_msec
  ) SELECT
    site_id,
    date_trunc('minute', ingest_time),
    COUNT(1) as request_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 1 ELSE 0 END) as success_count,
    SUM(CASE WHEN (status_code between 200 and 299) THEN 0 ELSE 1 END) as error_count,
    SUM(response_time_msec) / COUNT(1) AS average_response_time_msec
  FROM http_request
  -- roll up only data new since last_rollup_time
  WHERE date_trunc('minute', ingest_time) <@
          tstzrange(last_rollup_time, curr_rollup_time, '(]')
  GROUP BY 1, 2;

  -- update the value in latest_rollup so that next time we run the
  -- rollup it will operate on data newer than curr_rollup_time
  UPDATE latest_rollup SET minute = curr_rollup_time;
END;
$$ LANGUAGE plpgsql;
```

İşlevimiz yerinde olduğundan, verileri yuvarlamak için çalıştırın:

```sql
SELECT rollup_http_request();
```

Ve verilerimiz önceden toplu bir biçimde olduğu için, daha önce olduğu gibi aynı raporu almak için toplama tablosunu sorgulayabiliriz. Aşağıdaki sorguyu çalıştırın:

```sql
SELECT site_id, ingest_time as minute, request_count,
       success_count, error_count, average_response_time_msec
  FROM http_request_1min
 WHERE ingest_time > date_trunc('minute', now()) - '5 minutes'::interval;
 ```

## <a name="expiring-old-data"></a>Eski verilerin süresi doluyor

Toplamalar sorguları daha hızlı yapar, ancak sınırsız depolama maliyetlerini önlemek için yine de eski verilerin süresinin dolması gerekir. Her bir parçalı veri birimi için verileri ne kadar süreyle tutmak istediğinize karar verin ve süresi dolmuş verileri silmek için standart sorguları kullanın. Aşağıdaki örnekte, ham verileri bir gün, dakika başına toplamaları bir ay boyunca saklamaya karar verdik:

```sql
DELETE FROM http_request WHERE ingest_time < now() - interval '1 day';
DELETE FROM http_request_1min WHERE ingest_time < now() - interval '1 month';
```

Üretimde, bu sorguları bir işlevde sarın ve her dakika bir cron işinde çağırabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Önceki adımlarda, bir sunucu grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymayı beklemiyorsanız, sunucu grubunu silin. Sunucu grubunuzun *Genel Bakış* *sayfasındasil* düğmesine basın. Açılır pencerede istendiğinde, sunucu grubunun adını onaylayın ve son *Sil* düğmesini tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Hyperscale (Citus) sunucu grubunu nasıl sağlarsınız. Ona psql ile bağlandınız, bir şema oluşturdunuz ve veri dağıttınız. Ham formda verileri sorgulamayı, bu verileri düzenli olarak toplamayı, toplanan tabloları sorgulamayı ve eski verilerin süresinin dolmasını öğrendiniz.

Ardından, hiperölçek kavramları hakkında bilgi edinin.
> [!div class="nextstepaction"]
> [Hiper ölçekli düğüm türleri](https://aka.ms/hyperscale-concepts)
