---
title: Tablo colocation - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Daha hızlı sorgular için ilgili bilgileri birlikte depolama
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 7e4073ec45f4c21f33d20924a9948e72f961c7f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74967346"
---
# <a name="table-colocation-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure Veritabanında Tablo uyumluhale saplan - Hyperscale (Citus)

Birlikte konumlandırma, ilgili bilgileri aynı düğümlerde birlikte depolamak anlamına gelir. Gerekli tüm veriler herhangi bir ağ trafiği olmadan kullanılabilir olduğunda sorgular hızlı gidebilir. İlgili verilerin farklı düğümlerde kolonyalanması, sorguların her düğümde paralel olarak verimli bir şekilde çalışmasını sağlar.

## <a name="data-colocation-for-hash-distributed-tables"></a>Karma dağıtılmış tablolar için veri colocation

PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'nda, dağıtım sütunundaki değerin karma kısmı parçanın karma aralığına düşerse, bir satır bir parça da depolanır. Aynı karma aralığına sahip kırıklar her zaman aynı düğümüzerine yerleştirilir. Eşit dağılım sütun değerlerine sahip satırlar, tablolar arasında her zaman aynı düğümüzerindedir.

![Kırıklar](media/concepts-hyperscale-colocation/colocation-shards.png)

## <a name="a-practical-example-of-colocation"></a>Birlikte konumuygulamanın pratik bir örneği

Çok kiracılı web analizi SaaS'ın bir parçası olabilecek aşağıdaki tabloları göz önünde bulundurun:

```sql
CREATE TABLE event (
  tenant_id int,
  event_id bigint,
  page_id int,
  payload jsonb,
  primary key (tenant_id, event_id)
);

CREATE TABLE page (
  tenant_id int,
  page_id int,
  path text,
  primary key (tenant_id, page_id)
);
```

Şimdi, müşteriye yönelik bir pano tarafından verilebilen sorguları yanıtlamak istiyoruz. Örnek bir sorgu, "Kiracı altıda '/blog' ile başlayan tüm sayfalar için son haftadaki ziyaret sayısını iade edin."

Verilerimiz Tek Sunucu dağıtım seçeneğindeyse, SQL tarafından sunulan zengin ilişkisel işlemler kümesini kullanarak sorgumuzu kolayca ifade edebiliriz:

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Bu sorgu için [çalışma kümesi](https://en.wikipedia.org/wiki/Working_set) belleğe sığtığı sürece, tek sunuculu tablo uygun bir çözümdür. Hiperölçek (Citus) dağıtım seçeneği yle veri modelini ölçeklendirme fırsatlarını ele alalım.

### <a name="distribute-tables-by-id"></a>Tabloları id'e göre dağıtma

Kiracı sayısı arttıkça ve her kiracı için depolanan veriler arttıkça tek sunuculu sorgular yavaşlamaya başlar. Çalışma kümesi belleğe uyumu durdurur ve CPU bir darboğaz haline gelir.

Bu durumda, Hyperscale (Citus) kullanarak verileri birçok düğüm üzerinde parçalayabiliriz. Parçaya karar verdiğimizde yapmamız gereken ilk ve en önemli seçim dağıtım sütunudur. Olay tablosu ve `event_id` `page_id` `page` tablo için saf bir kullanma seçeneğiyle başlayalım:

```sql
-- naively use event_id and page_id as distribution columns

SELECT create_distributed_table('event', 'event_id');
SELECT create_distributed_table('page', 'page_id');
```

Veriler farklı çalışanlar arasında dağıtıldığında, tek bir PostgreSQL düğümünde yaptığımız gibi bir birleştirme gerçekleştiremeyiz. Bunun yerine, iki sorgu vermemiz gerekir:

```sql
-- (Q1) get the relevant page_ids
SELECT page_id FROM page WHERE path LIKE '/blog%' AND tenant_id = 6;

-- (Q2) get the counts
SELECT page_id, count(*) AS count
FROM event
WHERE page_id IN (/*…page IDs from first query…*/)
  AND tenant_id = 6
  AND (payload->>'time')::date >= now() - interval '1 week'
GROUP BY page_id ORDER BY count DESC LIMIT 10;
```

Daha sonra, iki adımın sonuçlarının uygulama tarafından birleştirilmesi gerekir.

Sorguları çalıştırmak düğümleri dağılmış kırıkları veri danışmalısınız.

![Verimsiz sorgular](media/concepts-hyperscale-colocation/colocation-inefficient-queries.png)

Bu durumda, veri dağıtımı önemli dezavantajlar oluşturur:

-   Her parçayı sorgulayan ve birden çok sorgu çalıştıran genel merkez.
-   Q1'in yükü istemciye birçok satırı döndürerek.
-   Q2 büyük olur.
-   Birden çok adımda sorgu yazma gereksinimi, uygulamada değişiklik gerektirir.

Veriler dağıtılır, böylece sorgular paralellenebilir. Yalnızca sorgunun yaptığı iş miktarı, çok sayıda parçayı sorgulamanın yükünden önemli ölçüde daha büyükse yararlıdır.

### <a name="distribute-tables-by-tenant"></a>Tabloları kiracıya göre dağıtma

Hyperscale 'de (Citus) aynı dağılım sütun değerine sahip satırların aynı düğümüzerinde olması garanti edilir. Baştan başlayarak, dağıtım sütunu `tenant_id` olarak tablolarımızı oluşturabiliriz.

```sql
-- co-locate tables by using a common distribution column
SELECT create_distributed_table('event', 'tenant_id');
SELECT create_distributed_table('page', 'tenant_id', colocate_with => 'event');
```

Şimdi Hyperscale (Citus) değişiklik olmadan orijinal tek sunucu sorgusu nu yanıtlayabilir (Q1):

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

tenant_id filtreleme ve birleştirme nedeniyle, Hyperscale (Citus) tüm sorgunun belirli bir kiracıiçin verileri içeren ortak parça kümesi kullanılarak yanıtlanabileceğini bilir. Tek bir PostgreSQL düğümü sorguyu tek bir adımda yanıtlayabilir.

![Daha iyi sorgu](media/concepts-hyperscale-colocation/colocation-better-query.png)

Bazı durumlarda, sorgular ve tablo şemaları, kiracı kimliğini benzersiz kısıtlamalara ve birleştirme koşullarına dahil etmek üzere değiştirilmelidir. Bu değişiklik genellikle basittir.

## <a name="next-steps"></a>Sonraki adımlar

- Kiracı verilerinin [çok kiracılı öğreticide](tutorial-design-database-hyperscale-multi-tenant.md)nasıl bir konuma bulunduğunu görün.
