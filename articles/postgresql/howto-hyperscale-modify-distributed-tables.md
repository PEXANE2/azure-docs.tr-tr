---
title: Dağıtılmış tabloları değiştirme-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Azure portal kullanarak dağıtılmış tablolar oluşturmak ve değiştirmek için SQL komutları-hiper ölçek (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 8/10/2020
ms.openlocfilehash: 628944f9763dc79148e0b64c97158064208412bf
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88137057"
---
# <a name="distribute-and-modify-tables"></a>Tabloları dağıtma ve değiştirme

## <a name="distributing-tables"></a>Tabloları dağıtma

Dağıtılmış bir tablo oluşturmak için öncelikle tablo şemasını tanımlamanız gerekir. Bunu yapmak için, [Create Table](http://www.postgresql.org/docs/current/static/sql-createtable.html) ifadesini kullanarak düzenli bir PostgreSQL tablosu ile yaptığınız şekilde bir tablo tanımlayabilirsiniz.

```sql
CREATE TABLE github_events
(
    event_id bigint,
    event_type text,
    event_public boolean,
    repo_id bigint,
    payload jsonb,
    repo jsonb,
    actor jsonb,
    org jsonb,
    created_at timestamp
);
```

Ardından, \_ \_ tablo dağıtım sütununu belirtmek ve çalışan parçaları oluşturmak için dağıtılmış tablo oluştur () işlevini kullanabilirsiniz.

```sql
SELECT create_distributed_table('github_events', 'repo_id');
```

İşlev çağrısı, GitHub \_ olayları tablosunun depo \_ kimliği sütununda (sütun değerini karma olarak) dağıtılması gereken hiper ölçeğe (Citus) bildirir. İşlev Ayrıca citus. Shard \_ Count ve citus. Shard \_ çoğaltma \_ faktörü yapılandırma değerlerini kullanarak çalışan düğümlerinde parçalar oluşturur.

\_Her parça bir karma alanının parçası olan ve varsayılan citus. parça \_ çoğaltma \_ faktörü yapılandırma değerine göre çoğaltılan bir toplam citus. kıard sayısı parçalama sayısı oluşturur. Çalışan üzerinde oluşturulan parça çoğaltmaları, düzenleyiciyle aynı tablo şemasına, dizine ve kısıtlama tanımlarına sahiptir. Çoğaltmalar oluşturulduktan sonra, işlev tüm dağıtılmış meta verileri düzenleyiciye kaydeder.

Oluşturulan her parçaya benzersiz bir parça KIMLIĞI atanır ve tüm çoğaltmalarının aynı parça KIMLIĞI vardır. Parçalar, TableName shardıd adlı normal PostgreSQL tabloları olarak, TableName 'in \' \_ \' Dağıtılmış tablonun adı ve parça kimliği Ise atanan benzersiz kimliktir olarak gösterilir. Çalışan Postgres örneklerine bağlanarak tek tek parçalardaki komutları görüntüleyebilir veya çalıştırabilirsiniz.

Artık dağıtılmış tabloya veri eklemeye ve üzerinde sorgular çalıştırmaya hazır olursunuz. Ayrıca, bu bölümde kullanılan UDF hakkında daha fazla bilgi için [tablo ve parça DDL](reference-hyperscale-functions.md#table-and-shard-ddl) başvurusu ' na de başvurabilirsiniz.

### <a name="reference-tables"></a>Başvuru tabloları

Yukarıdaki yöntem tabloları birden çok yatay parçaya dağıtır.  Başka bir olasılık ise tabloları tek bir parçaya dağıtmakta ve parçayı her çalışan düğümüne çoğaltmaktadır. Bu şekilde dağıtılan tablolara *başvuru tabloları denir.* Bunlar, bir kümede birden çok düğüm tarafından sıklıkla erişilmesi gereken verileri depolamak için kullanılır.

Başvuru tabloları için ortak adaylar şunları içerir:

-   Daha büyük dağıtılmış tablolarla katılması gereken daha küçük tablolar.
-   Kiracı KIMLIĞI sütunu olmayan veya \' bir kiracı ile ilişkili olan çok kiracılı uygulamalardaki tablolar. (Veya, geçiş sırasında bir kiracıyla ilişkili bazı tablolar için bile.)
-   Birden çok sütunda benzersiz kısıtlamalar olması ve yeterince az olması gereken tablolar.

Örneğin, çok kiracılı bir eticaret sitesinin mağazalardan herhangi birinde bulunan işlemler için satış vergisini hesaplaması gerektiğini varsayalım. Vergi bilgileri \' herhangi bir kiracıya özeldir. Paylaşılan bir tabloya koymak mantıklı olur. ABD merkezli bir başvuru tablosu şöyle görünebilir:

```postgresql
-- a reference table

CREATE TABLE states (
  code char(2) PRIMARY KEY,
  full_name text NOT NULL,
  general_sales_tax numeric(4,3)
);

-- distribute it to all workers

SELECT create_reference_table('states');
```

Bir alışveriş sepeti için yalnızca bir hesaplama vergisi gibi sorgular `states` , ağ yükü olmadan tabloya katılabilir ve daha iyi doğrulama için durum koduna yabancı bir anahtar ekleyebilir.

Bir tabloyu tek bir çoğaltılan parça olarak dağıtmanın yanı sıra, `create_reference_table` udf onu Hyperscale (Citus) meta veri tablolarında bir başvuru tablosu olarak işaretler. Hiper ölçek (Citus), bu şekilde işaretlenen tablolarda yapılan değişiklikler için otomatik olarak iki aşamalı işlemeler ([2Pc](https://en.wikipedia.org/wiki/Two-phase_commit_protocol)) gerçekleştirir ve bu da güçlü tutarlılık garantisi sağlar.

Parça sayısı bir olan dağıtılmış bir tablonuz varsa, bunu aşağıdaki gibi tanınan bir başvuru tablosu olacak şekilde yükseltebilirsiniz:

```postgresql
SELECT upgrade_to_reference_table('table_name');
```

Başvuru tablolarının kullanılmasıyla ilgili başka bir örnek için bkz. [çok kiracılı veritabanı öğreticisi](tutorial-design-database-hyperscale-multi-tenant.md).

### <a name="distributing-coordinator-data"></a>Düzenleyici verilerini dağıtma

Mevcut bir PostgreSQL veritabanı, bir hiper ölçek (Citus) kümesi için düzenleyici düğümüne dönüştürülürse, tablolarındaki veriler etkin bir şekilde dağıtılabilir ve bir uygulamaya en düşük kesintiye uğramıştır.

`create_distributed_table`Daha önce açıklanan işlev hem boş hem de boş olmayan tablolar üzerinde çalışır ve ikincisi tablo satırlarını otomatik olarak küme genelinde dağıtır. Verilerin, iletinin varlığına göre \" kopyalandığına ve verilerin yerel tablodan kopyalanmasını fark edersiniz \. . \" Örneğin:

```postgresql
CREATE TABLE series AS SELECT i FROM generate_series(1,1000000) i;
SELECT create_distributed_table('series', 'i');
NOTICE:  Copying data from local table...
 create_distributed_table
 --------------------------

 (1 row)
```

Veriler geçirildiğinde tablodaki yazma işlemleri engellenir ve işlev işleirse, bekleyen yazma işlemleri dağıtılmış sorgular olarak işlenir. (İşlev başarısız olursa, sorgular yerel olarak yeniden olur.) Okuma işlemi normal olarak devam edebilir ve işlev işletikten sonra dağıtılmış sorgular haline gelir.

A ve B tablolarını dağıtırken, A 'nın B 'ye yabancı anahtarı olduğu durumlarda, önce anahtar hedef tablosu B 'yi dağıtın. Yanlış sırada yapmak hataya neden olur:

```
ERROR:  cannot create foreign key constraint
DETAIL:  Referenced table must be a distributed table or a reference table.
```

Doğru sırada dağıtmak mümkün değilse yabancı anahtarları bırakın, tabloları dağıtın ve yabancı anahtarları yeniden oluşturun.

Amazon RDS 'den Hyperscale (Citus) bulutuna benzer bir dış veritabanından veri geçirirken, önce hiper ölçek (Citus) dağıtılmış tabloları aracılığıyla oluşturun `create_distributed_table` ve sonra verileri tabloya kopyalayın.
Dağıtılmış tablolara kopyalama, düzenleyici düğümündeki alanın tükenme durumunu önler.

## <a name="colocating-tables"></a>Tabloları birlikte bulundurma

Birlikte bulundurma, ilgili bilgilerin aynı makinelere yerleştirilmesi anlamına gelir. Veri kümesinin tamamına yönelik yatay ölçeklenebilirliğin avantajlarından yararlanarak verimli sorgular sağlar. Daha fazla bilgi için bkz [. birlikte](concepts-hyperscale-colocation.md)bulundurma.

Tablolar gruplar halinde birlikte bulunur. Bir tablonun birlikte bulundurma grubu atamasını el ile denetlemek için, isteğe bağlı `colocate_with` parametresini kullanın `create_distributed_table` . \'Tablo oluşturma hakkında bilgi için \' Bu parametreyi atlayın. Varsayılan değer olarak `'default'` , tabloyu aynı dağıtım sütunu türü, parça sayısı ve çoğaltma faktörü olan diğer bir varsayılan birlikte bulunan herhangi bir varsayılan birlikte bulunan tablo ile gruplandırır. Bu örtük birlikte bulundurmayı bölmek veya güncelleştirmek istiyorsanız, kullanabilirsiniz `update_distributed_table_colocation()` .

```postgresql
-- these tables are implicitly co-located by using the same
-- distribution column type and shard count with the default
-- co-location group

SELECT create_distributed_table('A', 'some_int_col');
SELECT create_distributed_table('B', 'other_int_col');
```

Yeni bir tablo, kendi Yapılacaklar grubu olan diğer kullanıcılarla ilişkili olmadığında, öğesini belirtin `colocated_with => 'none'` .

```postgresql
-- not co-located with other tables

SELECT create_distributed_table('A', 'foo', colocate_with => 'none');
```

İlişkisiz tabloları kendi birlikte bulundurma gruplarına bölmek, aynı gruptaki parçalar birlikte taşınabilmesi gerektiğinden parça [yeniden dengeleme](howto-hyperscale-scaling.md#rebalance-shards) performansını geliştirir.

Tablolar gerçekten ilişkili olduğunda (örneğin, birleştirildiklerinde), bunları açıkça eklemek mantıklı olabilir. Uygun birlikte bulundurmaya yönelik kazanç, herhangi bir yeniden dengeleme yükünden daha önemlidir.

Birden çok tabloyu açık bir şekilde birleştirmek için, bir tane dağıtın ve ardından diğerlerini birlikte bulundurma grubuna koyun. Örnek:

```postgresql
-- distribute stores
SELECT create_distributed_table('stores', 'store_id');

-- add to the same group as stores
SELECT create_distributed_table('orders', 'store_id', colocate_with => 'stores');
SELECT create_distributed_table('products', 'store_id', colocate_with => 'stores');
```

Birlikte bulundurma grupları hakkında bilgiler [pg_dist_colocation](reference-hyperscale-metadata.md#colocation-group-table) tablosunda depolanır, [pg_dist_partition](reference-hyperscale-metadata.md#partition-table) hangi tabloların hangi gruplara atandığını gösterir.

## <a name="dropping-tables"></a>Tabloları bırakma

Dağıtılmış tablolarınızı kaldırmak için standart PostgreSQL DROP TABLE komutunu kullanabilirsiniz. Normal tablolarda olduğu gibi, DROP TABLE, hedef tablo için mevcut olan tüm dizinleri, kuralları, Tetikleyicileri ve kısıtlamaları kaldırır. Ayrıca, çalışan düğümlerinde parçaları bırakır ve meta verilerini temizler.

```sql
DROP TABLE github_events;
```

## <a name="modifying-tables"></a>Tabloları değiştirme

Hiper ölçek (Citus), birçok tür DDL deyimini otomatik olarak yayar.
Düzenleyici düğümündeki dağıtılmış bir tabloyu değiştirmek, işçilere ait parçaları da güncelleştirir. Diğer DDL deyimleri el ile yayma gerektirir ve bazı diğerleri, bir dağıtım sütununu değiştirecek gibi yasaktır.
Otomatik yayma için uygun olmayan DDL 'yi çalıştırma girişimi bir hata oluşturacak ve tabloları düzenleyici düğümünde bırakacak.

Bu, yayan DDL deyimlerinin kategorilerinin bir başvurusudur.
Otomatik yayma, bir [yapılandırma parametresiyle](reference-hyperscale-parameters.md#citusenable_ddl_propagation-boolean) etkinleştirilebilir veya devre dışı bırakılabilir

### <a name="addingmodifying-columns"></a>Sütunları ekleme/değiştirme

Hiperscale (Citus), çoğu [alter table](https://www.postgresql.org/docs/current/static/ddl-alter.html) komutunu otomatik olarak yayar. Sütunları eklemek veya varsayılan değerlerini değiştirmek, tek makineli bir PostgreSQL veritabanında olduğu gibi çalışır:

```postgresql
-- Adding a column

ALTER TABLE products ADD COLUMN description text;

-- Changing default value

ALTER TABLE products ALTER COLUMN price SET DEFAULT 7.77;
```

Varolan bir sütunda yeniden adlandırma ya da veri türünü değiştirme gibi önemli değişiklikler çok fazla. Ancak [dağıtım sütununun](concepts-hyperscale-nodes.md#distribution-column) veri türü değiştirilemez.
Bu sütun, tablo verilerinin Hyperscale (Citus) kümesi üzerinden nasıl dağıttığını ve veri türünü değiştirmenin verilerin taşınmasını gerektirmesini belirler.

Bunun denenmesine bir hata neden olur:

```postgres
-- assumining store_id is the distribution column
-- for products, and that it has type integer

ALTER TABLE products
ALTER COLUMN store_id TYPE text;

/*
ERROR:  XX000: cannot execute ALTER TABLE command involving partition column
LOCATION:  ErrorIfUnsupportedAlterTableStmt, multi_utility.c:2150
*/
```

### <a name="addingremoving-constraints"></a>Kısıtlama ekleme/kaldırma

Hiper ölçek (Citus) kullanımı, veritabanı kısıtlamaları da dahil olmak üzere ilişkisel bir veritabanının güvenliğini kullanmaktan devam etmenize olanak tanır (bkz. PostgreSQL [belgeleri](https://www.postgresql.org/docs/current/static/ddl-constraints.html)).
Dağıtılmış sistemlerin doğası nedeniyle, hiper ölçek (Citus), çalışan düğümler arasında benzersizlik kısıtlamalarına veya bilgi bütünlüğüne sahip olmaz.

Birlikte bulunan dağıtılmış tablolar arasında yabancı anahtar ayarlamak için, dağıtım sütununu her zaman anahtara ekleyin. Dağıtım sütununun dahil edilmesi, anahtarın bileşik olmasını gerektirebilir.

Yabancı anahtarlar şu durumlarda oluşturulabilir:

-   iki yerel (dağıtılmamış) tablo arasında,
-   iki başvuru tablosu arasında,
-   Anahtar Dağıtım sütununu içerdiğinde, birlikte [bulunan iki dağıtılmış](concepts-hyperscale-colocation.md) tablo arasında veya
-   [başvuru tablosuna](concepts-hyperscale-nodes.md#type-2-reference-tables) başvuran bir dağıtılmış tablo olarak

Başvuru tablolarından dağıtılmış tablolara yabancı anahtarlar desteklenmiyor.

> [!NOTE]
>
> Birincil anahtarlar ve benzersizlik kısıtlamaları dağıtım sütununu içermelidir. Bunları dağıtım dışı bir sütuna eklemek bir hata oluşturur

Bu örnekte, dağıtılmış tablolarda birincil ve yabancı anahtarların nasıl oluşturulacağı gösterilmektedir:

```postgresql
--
-- Adding a primary key
-- --------------------

-- We'll distribute these tables on the account_id. The ads and clicks
-- tables must use compound keys that include account_id.

ALTER TABLE accounts ADD PRIMARY KEY (id);
ALTER TABLE ads ADD PRIMARY KEY (account_id, id);
ALTER TABLE clicks ADD PRIMARY KEY (account_id, id);

-- Next distribute the tables

SELECT create_distributed_table('accounts', 'id');
SELECT create_distributed_table('ads',      'account_id');
SELECT create_distributed_table('clicks',   'account_id');

--
-- Adding foreign keys
-- -------------------

-- Note that this can happen before or after distribution, as long as
-- there exists a uniqueness constraint on the target column(s) which
-- can only be enforced before distribution.

ALTER TABLE ads ADD CONSTRAINT ads_account_fk
  FOREIGN KEY (account_id) REFERENCES accounts (id);
ALTER TABLE clicks ADD CONSTRAINT clicks_ad_fk
  FOREIGN KEY (account_id, ad_id) REFERENCES ads (account_id, id);
```

Benzer şekilde, bir dağıtım sütununu benzersizlik kısıtlamalarına dahil edin:

```postgresql
-- Suppose we want every ad to use a unique image. Notice we can
-- enforce it only per account when we distribute by account id.

ALTER TABLE ads ADD CONSTRAINT ads_unique_image
  UNIQUE (account_id, image_url);
```

Not-null kısıtlamaları herhangi bir sütuna (dağıtım veya değil) uygulanabilir, çünkü çalışanlar arasında hiçbir arama gerektirmez.

```postgresql
ALTER TABLE ads ALTER COLUMN image_url SET NOT NULL;
```

### <a name="using-not-valid-constraints"></a>GEÇERLI DEĞIL kısıtlamaları kullanma

Bazı durumlarda, yeni satırlar için kısıtlamaları zorlamak yararlı olabilir, ancak mevcut olmayan satırların değişmeden kalmasına izin verir. Hiper ölçek (Citus), PostgreSQL ' \' ın \" geçerli kısıtlama ATAMASıNı kullanarak denetim kısıtlamaları ve yabancı anahtarlar için bu özelliği destekler \" .

Örneğin, bir [başvuru tablosunda](concepts-hyperscale-nodes.md#type-2-reference-tables)Kullanıcı profillerini depolayan bir uygulama düşünün.

```postgres
-- we're using the "text" column type here, but a real application
-- might use "citext" which is available in a postgres contrib module

CREATE TABLE users ( email text PRIMARY KEY );
SELECT create_reference_table('users');
```

Zaman içinde, birkaç adres dışı adresin tabloya aktarıldığınızı düşünelim.

```postgres
INSERT INTO users VALUES
   ('foo@example.com'), ('hacker12@aol.com'), ('lol');
```

Adresleri doğrulamak istiyoruz, ancak PostgreSQL genellikle mevcut satırlarda başarısız olan bir DENETIM kısıtlaması eklememize izin vermiyor. Ancak *,* kısıtlama geçersiz olarak işaretlenmiş bir kısıtlamaya izin veriyor:

```postgres
ALTER TABLE users
ADD CONSTRAINT syntactic_email
CHECK (email ~
   '^[a-zA-Z0-9.!#$%&''*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$'
) NOT VALID;
```

Yeni satırlar artık korunuyor.

```postgres
INSERT INTO users VALUES ('fake');

/*
ERROR:  new row for relation "users_102010" violates
        check constraint "syntactic_email_102010"
DETAIL:  Failing row contains (fake).
*/
```

Daha sonra, yoğun olmayan saatlerde, bir veritabanı yöneticisi bozuk satırları düzeltmeye ve kısıtlamayı yeniden doğrulamaya çalışabilir.

```postgres
-- later, attempt to validate all rows
ALTER TABLE users
VALIDATE CONSTRAINT syntactic_email;
```

PostgreSQL belgeleri, [alter table](https://www.postgresql.org/docs/current/sql-altertable.html) bölümünde geçerli DEĞIL ve sınırlamayı doğrulama hakkında daha fazla bilgi içerir.

### <a name="addingremoving-indices"></a>Dizinleri ekleme/kaldırma

Hiper ölçek (Citus), [Dizin](https://www.postgresql.org/docs/current/static/sql-createindex.html)eklemeyi ve kaldırmayı destekler:

```postgresql
-- Adding an index

CREATE INDEX clicked_at_idx ON clicks USING BRIN (clicked_at);

-- Removing an index

DROP INDEX clicked_at_idx;
```

Dizin eklemek bir yazma kilidi alır ve bu, çok kiracılı bir kayıt sisteminde istenmeyen bir yol açabilir \" . \" Uygulama kapalı kalma süresini en aza indirmek için dizini bunun yerine [eşzamanlı](https://www.postgresql.org/docs/current/static/sql-createindex.html#SQL-CREATEINDEX-CONCURRENTLY) olarak oluşturun. Bu yöntem, standart bir dizin yapıından daha fazla toplam iş gerektirir ve daha uzun sürer. Ancak, Dizin derlenirken normal işlemlerin devam etmesine izin verdiğinden, bu yöntem bir üretim ortamında yeni dizinler eklemek için yararlıdır.

```postgresql
-- Adding an index without locking table writes

CREATE INDEX CONCURRENTLY clicked_at_idx ON clicks USING BRIN (clicked_at);
```
