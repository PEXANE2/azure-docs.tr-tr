---
title: Uzantılar – Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: PostgreSQL için Azure Veritabanı uzantılarını kullanarak veritabanınızın işlevselliğini genişletme yeteneğini açıklar - Hyperscale (Citus)
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: 84b28096904db49f98c16601c5927928ad38743b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77485412"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure Veritabanı'ndaki PostgreSQL uzantıları – Hyperscale (Citus)

PostgreSQL uzantıları kullanarak veritabanınızın işlevselliğini genişletmek için yeteneği sağlar. Uzantılar, birden çok ilişkili SQL nesnesini tek bir komutla yüklenebilen veya veritabanınızdan kaldırılabilen tek bir pakette biraraya getirmenize olanak sağlar. Veritabanına yüklendikten sonra, uzantılar yerleşik özellikler gibi çalışabilir. PostgreSQL uzantıları hakkında daha fazla bilgi için, [paketle ilgili nesneleri bir uzantıya](https://www.postgresql.org/docs/current/static/extend-extensions.html)bakın.

## <a name="use-postgresql-extensions"></a>PostgreSQL uzantılarını kullanma

PostgreSQL uzantılarını kullanamadan önce veritabanınıza yüklenmesi gerekir. Belirli bir uzantıyı yüklemek için, paketlenmiş nesneleri veritabanınıza yüklemek için psql aracından [CREATE EXTENSION](https://www.postgresql.org/docs/current/static/sql-createextension.html) komutunu çalıştırın.

PostgreSQL için Azure Veritabanı - Hyperscale (Citus) şu anda burada listelenen bir alt anahtar uzantıları kümesini desteklemektedir. Listelenenler dışındaki uzantılar desteklenmez. PostgreSQL için Azure Veritabanı ile kendi uzantınızı oluşturamazsınız.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı tarafından desteklenen uzantılar

Aşağıdaki tablolar, Şu anda PostgreSQL için Azure Veritabanı tarafından desteklenen standart PostgreSQL uzantılarını listelemaktadır. Bu bilgiler çalıştırılarak `SELECT * FROM pg_available_extensions;`da kullanılabilir.

### <a name="data-types-extensions"></a>Veri türleri uzantıları

> [!div class="mx-tableFixed"]
> | **Dahili numara** | **Açıklama** |
> |---|---|
> | [citext](https://www.postgresql.org/docs/current/static/citext.html) | Büyük/küçük harf duyarsız karakter dize türü sağlar. |
> | [Küp](https://www.postgresql.org/docs/current/static/cube.html) | Çok boyutlu küpler için bir veri türü sağlar. |
> | [hstore](https://www.postgresql.org/docs/current/static/hstore.html) | Anahtar değer çiftleri kümelerini depolamak için bir veri türü sağlar. |
> | [hll](https://github.com/citusdata/postgresql-hll) | HyperLogLog veri yapısı sağlar. |
> | [ısn](https://www.postgresql.org/docs/current/static/isn.html) | Uluslararası ürün numaralandırma standartları için veri türleri sağlar. |
> | [Lo](https://www.postgresql.org/docs/current/lo.html) | Büyük Nesne bakımı. |
> | [ltree](https://www.postgresql.org/docs/current/static/ltree.html) | Hiyerarşik ağaç benzeri yapılar için bir veri türü sağlar. |
> | [Sönmez](https://www.postgresql.org/docs/current/seg.html) | Satır kesimlerini veya kayan nokta aralıklarını temsil etmek için veri türü. |
> | [Topn](https://github.com/citusdata/postgresql-topn/) | Top-n JSONB için yazın. |

### <a name="full-text-search-extensions"></a>Tam metin arama uzantıları

> [!div class="mx-tableFixed"]
> | **Dahili numara** | **Açıklama** |
> |---|---|
> | [dikte\_int](https://www.postgresql.org/docs/current/static/dict-int.html) | Tamsayılar için metin arama sözlüğü şablonu sağlar. |
> | [dict\_xsyn](https://www.postgresql.org/docs/current/dict-xsyn.html) | Genişletilmiş eşanlamlı işleme için metin arama sözlüğü şablonu. |
> | [aksansız](https://www.postgresql.org/docs/current/static/unaccent.html) | Aksanları (aksan işaretleri) lexemes'ten kaldıran bir metin arama sözlüğü. |

### <a name="functions-extensions"></a>Fonksiyon uzantıları

> [!div class="mx-tableFixed"]
> | **Dahili numara** | **Açıklama** |
> |---|---|
> | [autoinc](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.7) | Otomatikleştirme alanları için işlevler. |
> | [toprak mesafesi](https://www.postgresql.org/docs/current/static/earthdistance.html) | Dünya yüzeyinde büyük daire mesafeleri hesaplamak için bir araç sağlar. |
> | [bulanıkça](https://www.postgresql.org/docs/current/static/fuzzystrmatch.html) | Dizeleri arasındaki benzerlikleri ve mesafeyi belirlemek için çeşitli işlevler sağlar. |
> | [kullanıcı\_adı ekleme](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.8) | Tabloyu kimin değiştirdiğini izlemek için işlevler. |
> | [intagg](https://www.postgresql.org/docs/current/intagg.html) | İnteger toplayıcı ve sayılatıcı (eski). |
> | [intarray](https://www.postgresql.org/docs/current/static/intarray.html) | Null-free tümleç dizilerini işlemek için işlevler ve işleçler sağlar. |
> | [moddatetime](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.9) | Son değişiklik saatini izlemek için işlevler. |
> | [pgcrypto](https://www.postgresql.org/docs/current/static/pgcrypto.html) | Şifreleme işlevleri sağlar. |
> | [pg\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Bölümlenmiş tabloları zamana veya kimlikle yönetir. |
> | [pg\_trgm](https://www.postgresql.org/docs/current/static/pgtrgm.html) | Trigram eşleştirmesine dayalı alfasayısal metnin benzerliğini belirlemek için işlevler ve işleçler sağlar. |
> | [refint](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.5) | Başvuru bütünlüğünü (eski) uygulamak için işlevler. |
> | oturum\_analitiği | Hstore dizilerini sorgulamak için işlevler. |
> | [tablefunc](https://www.postgresql.org/docs/current/static/tablefunc.html) | Çapraz sekme de dahil olmak üzere tüm tabloları işleyen işlevler sağlar. |
> | [tcn](https://www.postgresql.org/docs/current/tcn.html) | Tetikleyici değişiklik bildirimleri. |
> | [zaman yolculuğu](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.6) | Zaman yolculuğunu uygulamak için işlevler. |
> | [uuid-ossp](https://www.postgresql.org/docs/current/static/uuid-ossp.html) | Evrensel olarak benzersiz tanımlayıcılar (UUIDs) oluşturur. |

### <a name="hyperscale-extensions"></a>Hiper ölçek uzantıları

> [!div class="mx-tableFixed"]
> | **Dahili numara** | **Açıklama** |
> |---|---|
> | [citus](https://github.com/citusdata/citus) | Citus dağıtılan veritabanı. |
> | shard\_rebalancer | Düğüm eklenmesi veya kaldırılması durumunda bir sunucu grubundaki verileri güvenli bir şekilde yeniden dengeleyin. |

### <a name="index-types-extensions"></a>Dizin türleri uzantıları

> [!div class="mx-tableFixed"]
> | **Dahili numara** | **Açıklama** |
> |---|---|
> | [Bloom](https://www.postgresql.org/docs/current/bloom.html) | Bloom erişim yöntemi - imza dosya tabanlı dizin. |
> | [btree\_cin](https://www.postgresql.org/docs/current/static/btree-gin.html) | Belirli veri türleri için B-ağacı benzeri davranış uygulayan örnek GIN işleci sınıfları sağlar. |
> | [btree\_ana gist](https://www.postgresql.org/docs/current/static/btree-gist.html) | B-ağacını uygulayan GiST dizin işleci sınıflarını sağlar. |

### <a name="language-extensions"></a>Dil uzantıları

> [!div class="mx-tableFixed"]
> | **Dahili numara** | **Açıklama** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/current/static/plpgsql.html) | PL/pgSQL yüklenebilir yordam dili. |

### <a name="miscellaneous-extensions"></a>Çeşitli uzantılar

> [!div class="mx-tableFixed"]
> | **Dahili numara** | **Açıklama** |
> |---|---|
> | [adminpack](https://www.postgresql.org/docs/current/adminpack.html) | PostgreSQL için yönetim işlevleri. |
> | [amcheck](https://www.postgresql.org/docs/current/amcheck.html) | İlişki bütünlüğünü doğrulamak için işlevler. |
> | [dosya\_fdw](https://www.postgresql.org/docs/current/file-fdw.html) | Düz dosya erişimi için yabancı veri sarıcı. |
> | [sayfa incelemesi](https://www.postgresql.org/docs/current/pageinspect.html) | Veritabanı sayfalarının içeriğini düşük düzeyde inceleyin. |
> | [pg\_buffercache](https://www.postgresql.org/docs/current/static/pgbuffercache.html) | Paylaşılan arabellek önbelleğinde neler olduğunu gerçek zamanlı olarak incelemek için bir araç sağlar. |
> | [pg\_cron](https://github.com/citusdata/pg_cron) | PostgreSQL için iş zamanlayıcısı. |
> | [pg\_freespacemap](https://www.postgresql.org/docs/current/pgfreespacemap.html) | Boş alan haritasını (FSM) inceleyin. |
> | [pg\_prewarm](https://www.postgresql.org/docs/current/static/pgprewarm.html) | İlişki verilerini arabellek önbelleğine yüklemek için bir yol sağlar. |
> | [pg\_\_stat ifadeleri](https://www.postgresql.org/docs/current/static/pgstatstatements.html) | Bir sunucu tarafından yürütülen tüm SQL deyimlerinin yürütme istatistiklerini izlemek için bir araç sağlar. Bu uzantı hakkında bilgi için "pg_stat_statements" bölümüne bakın. |
> | [pg\_görünürlüğü](https://www.postgresql.org/docs/current/pgvisibility.html) | Görünürlük haritasını (VM) ve sayfa düzeyinde görünürlük bilgilerini inceleyin. |
> | [pgrowlocks](https://www.postgresql.org/docs/current/static/pgrowlocks.html) | Satır düzeyinde kilitleme bilgilerini göstermek için bir araç sağlar. |
> | [pgstattuple](https://www.postgresql.org/docs/current/static/pgstattuple.html) | Tuple düzeyinde istatistikleri göstermek için bir araç sağlar. |
> | [postgres\_fdw](https://www.postgresql.org/docs/current/static/postgres-fdw.html) | Dış PostgreSQL sunucularında depolanan verilere erişmek için kullanılan yabancı veri sarıcı. Bu uzantı hakkında bilgi için "dblink ve postgres_fdw" bölümüne bakın.|
> | [sslinfo](https://www.postgresql.org/docs/current/sslinfo.html) | SSL sertifikaları hakkında bilgi. |
> | [tsm\_\_sistem satırları](https://www.postgresql.org/docs/current/tsm-system-rows.html) | Satır sayısını bir sınır olarak kabul eden TABLESAMPLE yöntemi. |
> | [tsm\_\_sistem süresi](https://www.postgresql.org/docs/current/tsm-system-time.html) | Milisaniye cinsinden zamanı bir sınır olarak kabul eden TABLESAMPLE yöntemi. |
> | [hipopg](https://hypopg.readthedocs.io/en/latest/) | CPU veya disk maliyeti olmayan varsayımsal dizinler oluşturma aracı sağlar. |
> | [Dblink](https://www.postgresql.org/docs/current/dblink.html) | Veritabanı oturumu içindeki diğer PostgreSQL veritabanlarına bağlantıları destekleyen bir modül. Bu uzantı hakkında bilgi için "dblink ve postgres_fdw" bölümüne bakın. |
> | [xml2](https://www.postgresql.org/docs/current/xml2.html) | XPath sorgulama ve XSLT. |


### <a name="postgis-extensions"></a>PostGIS uzantıları

> [!div class="mx-tableFixed"]
> | **Dahili numara** | **Açıklama** |
> |---|---|
> | [PostGIS](https://www.postgis.net/),\_postgis topoloji,\_\_postgis kaplan geocoder, postgis\_sfcgal | PostgreSQL için uzamsal ve coğrafi nesneler. |
> | adres\_standardizatör,\_adres\_standardizatör veri\_bize | Bir adresi kurucu öğelere ayrışdırmak için kullanılır. Coğrafi kodlama adresi normalleştirme adımLarını desteklemek için kullanılır. |
> | postgis\_sfcgal | PostGIS SFCGAL fonksiyonları. |
> | postgis\_\_kaplan geocoder | PostGIS kaplan geocoder ve ters geocoder. |
> | postgis\_topolojisi | PostGIS topoloji mekansal türleri ve fonksiyonları. |


## <a name="pg_stat_statements"></a>pg_stat_statements
[PG\_stat\_deyimleri uzantısı,](https://www.postgresql.org/docs/current/pgstatstatements.html) SQL deyimlerinin yürütme istatistiklerini izleme aracı sağlamak için PostgreSQL sunucusu için her Azure Veritabanına önceden yüklenir.

Ayar, `pg_stat_statements.track` uzantı tarafından hangi deyimlerin sayıldığını denetler. Varsayılan `top`olarak, istemciler tarafından doğrudan verilen tüm deyimlerin izlendiği anlamına gelir. Diğer iki izleme `none` düzeyi `all`ve. Bu ayar, [Azure portalı](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) veya [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli)üzerinden sunucu parametresi olarak yapılandırılabilir.

pg_stat_statements sağlayan sorgu yürütme bilgileri ile her SQL deyimini kaydederken sunucu performansı üzerindeki etkisi arasında bir denge vardır. pg_stat_statements uzantısını etkin olarak kullanmıyorsanız, 'yi `pg_stat_statements.track` `none`ayarlamanızı öneririz. Bazı üçüncü taraf izleme hizmetleri sorgu performansı öngörüleri sunmak için pg_stat_statements güvenebilir, bu nedenle bunun sizin için geçerli olup olmadığını doğrulayın.

## <a name="dblink-and-postgres_fdw"></a>dblink ve postgres_fdw
Bir PostgreSQL sunucusundan diğerine veya aynı sunucudaki başka bir veritabanına bağlanmak için dblink ve postgres_fdw kullanabilirsiniz. Alıcı sunucunun, güvenlik duvarı üzerinden gönderen sunucudan bağlantılara izin sağlaması gerekir. PostgreSQL sunucuları için Azure Veritabanı arasında bağlantı kurmak için bu uzantıları kullanmak **için, Azure hizmetlerine erişimi** ON'a bağlayın'ı ayarlayın. Uzantıları kullanmak istiyorsanız, aynı sunucuya geri dönmek için bu ayarı AYARı'yı da açmanız gerekir. **Azure hizmetlerine erişime izin verme** ayarını, Bağlantı **Güvenliği**altındaki Postgres sunucusunun Azure portal sayfasında bulunabilir. Döndürme Azure **hizmetlerine erişime izin ver** ON beyaz listeler tüm Azure IP'leri.

Şu anda, PostgreSQL için Azure Veritabanı'ndan giden bağlantılar, PostgreSQL sunucuları için diğer Azure Veritabanı bağlantıları dışında desteklenmez.
