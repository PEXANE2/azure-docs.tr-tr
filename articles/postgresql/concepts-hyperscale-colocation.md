---
title: Tablo birlikte bulundurma-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Daha hızlı sorgular için ilgili bilgileri birlikte depolama
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 7e4073ec45f4c21f33d20924a9948e72f961c7f8
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74967346"
---
# <a name="table-colocation-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda tablo bulundurma – hiper ölçek (Citus)

Birlikte bulundurma, ilgili bilgileri aynı düğümlerde depoladığını gösterir. Tüm gerekli veriler herhangi bir ağ trafiği olmadan kullanılabilir olduğunda sorgular hızlı bir şekilde geçebilir. Farklı düğümlerde ilgili verileri birlikte bulundurma, sorguların her düğüm üzerinde verimli bir şekilde çalışmasını sağlar.

## <a name="data-colocation-for-hash-distributed-tables"></a>Karma Dağıtılmış tablolar için veri birlikte bulundurma

PostgreSQL için Azure veritabanı – hiper ölçek (Citus) içinde, dağıtım sütunundaki değerin karması parçanın karma aralığı içinde kalırsa bir satır parça içinde depolanır. Aynı karma aralığa sahip parçalar her zaman aynı düğüme yerleştirilir. Eşit dağıtım sütunu değerleri olan satırlar her zaman tablolar arasında aynı düğüm üzerinde bulunur.

![Parçalar](media/concepts-hyperscale-colocation/colocation-shards.png)

## <a name="a-practical-example-of-colocation"></a>Kolay bir birlikte bulundurma örneği

Çok kiracılı bir Web Analytics SaaS 'nin parçası olabilecek aşağıdaki tabloları göz önünde bulundurun:

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

Artık müşterilere yönelik bir pano tarafından verilmiş olabilecek sorguları yanıtlamak istiyoruz. Örnek bir sorgu ", altı kiracıda '/blog ' ile başlayan tüm sayfalar için geçen hafta içindeki ziyaretlerin sayısını döndürür."

Verilerimizin tek sunuculu dağıtım seçeneğinde olması halinde, SQL tarafından sunulan zengin ilişkisel işlemler kümesini kullanarak sorgumuzu kolayca ifade ediyoruz:

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

Bu sorgunun [çalışma kümesi](https://en.wikipedia.org/wiki/Working_set) belleğe sığdığı sürece, tek sunuculu bir tablo uygun bir çözümdür. Veri modelini Hyperscale (Citus) dağıtım seçeneği ile ölçeklendirmeyle ilgili fırsatları ele alalım.

### <a name="distribute-tables-by-id"></a>Tabloları KIMLIĞE göre dağıt

Tek sunuculu sorgular, kiracıların sayısı ve her kiracı için depolanan veriler büyüdükçe yavaşlamadan başlar. Çalışma kümesi, bellek içinde sığdırma işlemini durduruyor ve CPU bir performans sorunu haline geliyor.

Bu durumda, Hyperscale (Citus) kullanarak verileri birçok düğüme parçalayabilirsiniz. Dağıtım sütunu, parçalara ayırmaya karar verirken yapmanız gereken ilk ve en önemli seçenektir. Olay tablosu için `event_id` kullanma ve `page` tablosu için `page_id` bir Naïve seçimi ile başlayalım:

```sql
-- naively use event_id and page_id as distribution columns

SELECT create_distributed_table('event', 'event_id');
SELECT create_distributed_table('page', 'page_id');
```

Veriler farklı çalışanlara sallandıklarında, tek bir PostgreSQL düğümüne yaptığımız gibi bir JOIN işlemi gerçekleştiremiyoruz. Bunun yerine, iki sorgu yayınlamamız gerekir:

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

Daha sonra, iki adımdan elde edilecek sonuçların uygulama tarafından birleştirilmesi gerekir.

Sorguları çalıştırmak, düğümler arasında dağılmış olan verileri parçalara sağlamalıdır.

![Verimsiz sorgular](media/concepts-hyperscale-colocation/colocation-inefficient-queries.png)

Bu durumda, veri dağıtımı önemli bir sakıncalar oluşturuyor:

-   Her parçayı sorgulama ve birden çok sorgu çalıştırma yükü.
-   Q1 'nin yükü istemciye çok sayıda satır döndürüyor.
-   S2 büyük olur.
-   Birden çok adımda sorgu yazma ihtiyacı, uygulamada değişiklik yapılmasını gerektirir.

Veriler dağınık, bu nedenle sorgular paralelleştirilmiş olabilir. Yalnızca sorgunun yaptığı iş miktarı çok sayıda parçaları sorgulama yüküyle önemli ölçüde büyük olduğunda faydalıdır.

### <a name="distribute-tables-by-tenant"></a>Tabloları kiracıya göre dağıtma

Hiper ölçekte (Citus), aynı dağıtım sütunu değerine sahip satırların aynı düğümde olması garanti edilir. Sürümünden itibaren, `tenant_id` dağıtım sütunu olarak tabloları oluşturuyoruz.

```sql
-- co-locate tables by using a common distribution column
SELECT create_distributed_table('event', 'tenant_id');
SELECT create_distributed_table('page', 'tenant_id', colocate_with => 'event');
```

Artık hiper ölçek (Citus), değişiklik yapılmadan orijinal tek sunuculu sorguyu yanıtlayabilir (S1):

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

Tenant_id FILTER ve JOIN nedeniyle hiper ölçek (Citus), bu belirli bir kiracının verilerini içeren birlikte bulunan parçalı parçalar kümesi kullanılarak tüm sorgunun yanıtlandığını bilir. Tek bir PostgreSQL düğümü, sorguyu tek bir adımda yanıtlayabilir.

![Daha iyi sorgu](media/concepts-hyperscale-colocation/colocation-better-query.png)

Bazı durumlarda, sorgular ve tablo şemaları, kiracı KIMLIĞINI benzersiz kısıtlamalara ve JOIN koşullarına dahil etmek için değiştirilmelidir. Bu değişiklik genellikle basittir.

## <a name="next-steps"></a>Sonraki adımlar

- Kiracı verilerinin [çok kiracılı öğreticide](tutorial-design-database-hyperscale-multi-tenant.md)nasıl birlikte yapıldığını görün.
