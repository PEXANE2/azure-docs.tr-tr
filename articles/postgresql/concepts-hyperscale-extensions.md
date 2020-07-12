---
title: Uzantılar – Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı-hiper ölçek (Citus) içindeki uzantıları kullanarak veritabanınızın işlevselliğini genişletme yeteneğini açıklar
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/09/2020
ms.openlocfilehash: de2579868ad72bdf4cf78c552e9553f289ecabd0
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259068"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql--hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda PostgreSQL uzantıları – hiper ölçek (Citus)

PostgreSQL, uzantıları kullanarak veritabanınızın işlevselliğini genişletmenizi sağlar. Uzantılar, tek bir komutla veritabanınızdan yüklenebilecek veya kaldırılabileceği tek bir pakette birden fazla ilgili SQL nesnesini paketlemeye olanak tanır. Veritabanına yüklendikten sonra uzantılar, yerleşik özellikler gibi çalışabilir. PostgreSQL uzantıları hakkında daha fazla bilgi için bkz. [Package ile ilgili nesneler bir uzantıya](https://www.postgresql.org/docs/current/static/extend-extensions.html).

## <a name="use-postgresql-extensions"></a>PostgreSQL uzantılarını kullanma

PostgreSQL uzantılarının kullanabilmeniz için veritabanınıza yüklenmesi gerekir. Belirli bir uzantıyı yüklemek için, [CREATE EXTENSION](https://www.postgresql.org/docs/current/static/sql-createextension.html)   paketlenmiş nesneleri veritabanınıza yüklemek üzere PSQL aracından Uzantı Oluştur komutunu çalıştırın.

PostgreSQL için Azure veritabanı-hiper ölçek (Citus) Şu anda burada listelenen bir anahtar uzantıları alt kümesini desteklemektedir. Listelenenler dışındaki uzantılar desteklenmez. PostgreSQL için Azure veritabanı ile kendi uzantınızı oluşturamazsınız.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı tarafından desteklenen uzantılar

Aşağıdaki tablolarda, şu anda PostgreSQL için Azure veritabanı tarafından desteklenen standart PostgreSQL uzantıları listelenmektedir. Bu bilgiler çalıştırılarak da kullanılabilir `SELECT * FROM pg_available_extensions;` .

### <a name="data-types-extensions"></a>Veri türleri uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [citext](https://www.postgresql.org/docs/current/static/citext.html) | Büyük/küçük harf duyarsız bir karakter dizesi türü sağlar. |
> | [ünüzde](https://www.postgresql.org/docs/current/static/cube.html) | Çok boyutlu küpler için bir veri türü sağlar. |
> | [HStore](https://www.postgresql.org/docs/current/static/hstore.html) | Anahtar-değer çiftlerinin kümelerini depolamak için bir veri türü sağlar. |
> | [hll](https://github.com/citusdata/postgresql-hll) | Bir HyperLogLog veri yapısı sağlar. |
> | [değilse](https://www.postgresql.org/docs/current/static/isn.html) | Uluslararası ürün numaralandırma standartları için veri türleri sağlar. |
> | [verilsin](https://www.postgresql.org/docs/current/lo.html) | Büyük nesne bakımı. |
> | [ltree](https://www.postgresql.org/docs/current/static/ltree.html) | Hiyerarşik ağaç benzeri yapılar için bir veri türü sağlar. |
> | [SEG](https://www.postgresql.org/docs/current/seg.html) | Satır parçalarını veya kayan nokta aralıklarını göstermek için veri türü. |
> | [üst n](https://github.com/citusdata/postgresql-topn/) | Top-n JSONB için yazın. |

### <a name="full-text-search-extensions"></a>Tam metin arama uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [Dict \_ int](https://www.postgresql.org/docs/current/static/dict-int.html) | Tamsayılar için bir metin arama sözlüğü şablonu sağlar. |
> | [Dict \_ xsyn](https://www.postgresql.org/docs/current/dict-xsyn.html) | Genişletilmiş eş anlamlı işleme için metin arama sözlüğü şablonu. |
> | [vurgu kaldır](https://www.postgresql.org/docs/current/static/unaccent.html) | Lexemes 'ten vurguları (aksan işaretleri) kaldıran bir metin arama sözlüğü. |

### <a name="functions-extensions"></a>İşlev uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [Oto](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.7) | Alanları tekrar arttırın işlevleri. |
> | [earthdistance](https://www.postgresql.org/docs/current/static/earthdistance.html) | Dünya yüzeyinde büyük daire mesafelerini hesaplamak için bir yol sağlar. |
> | [belirsizlik zystrmatch](https://www.postgresql.org/docs/current/static/fuzzystrmatch.html) | , Dizeler arasındaki benzerlikleri ve mesafeyi belirlemede çeşitli işlevler sağlar. |
> | [\_Kullanıcı adı Ekle](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.8) | Bir tablonun kimin tarafından değiştirildiğini izlemeye yönelik işlevler. |
> | [ıntagg](https://www.postgresql.org/docs/current/intagg.html) | Tamsayı toplayıcısı ve Numaralandırıcı (geçersiz). |
> | [ıntarray](https://www.postgresql.org/docs/current/static/intarray.html) | Tamsayıların null ve boş dizilerini işlemek için işlevler ve işleçler sağlar. |
> | [moddatetime](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.9) | Son değiştirme süresini izlemek için işlevler. |
> | [pgşifre](https://www.postgresql.org/docs/current/static/pgcrypto.html) | Şifreleme işlevleri sağlar. |
> | [sayfalık \_ partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Bölümlenmiş tabloları zamana veya KIMLIĞE göre yönetir. |
> | [sayfalık \_ TRGM](https://www.postgresql.org/docs/current/static/pgtrgm.html) | Trigram eşleştirmeye göre alfasayısal metnin benzerliğini belirlemek için işlevler ve işleçler sağlar. |
> | [refınt](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.5) | Bilgi tutarlılığı (kullanım dışı) uygulamak için işlevler. |
> | oturum \_ Analizi | HStore dizilerini sorgulamak için işlevler. |
> | [tablofunc](https://www.postgresql.org/docs/current/static/tablefunc.html) | Çapraz tablo dahil olmak üzere tüm tabloları düzenleyen işlevler sağlar. |
> | [tcn](https://www.postgresql.org/docs/current/tcn.html) | Tetiklenmiş değişiklik bildirimleri. |
> | [timetralevel](https://www.postgresql.org/docs/current/contrib-spi.html#id-1.11.7.45.6) | Saat seyahati uygulama işlevleri. |
> | [UUID-ossp](https://www.postgresql.org/docs/current/static/uuid-ossp.html) | Evrensel benzersiz tanımlayıcılar (UUID 'ler) oluşturur. |

### <a name="hyperscale-extensions"></a>Hiper ölçek uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [citus](https://github.com/citusdata/citus) | Citus dağıtılmış veritabanı. |
> | parça \_ yeniden dengeleyicisi | Düğüm ekleme veya kaldırma durumunda bir sunucu grubundaki verileri güvenli bir şekilde yeniden dengeleyin. |

### <a name="index-types-extensions"></a>Dizin türleri uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [Bloom](https://www.postgresql.org/docs/current/bloom.html) | Bloom erişim yöntemi-imza dosya tabanlı dizin. |
> | [BTREE \_ gın](https://www.postgresql.org/docs/current/static/btree-gin.html) | Belirli veri türleri için B-ağacı benzeri davranışı uygulayan örnek bir işleç sınıfları sağlar. |
> | [BTREE \_ GİST](https://www.postgresql.org/docs/current/static/btree-gist.html) | B-ağacı uygulayan GiST Dizin işleci sınıfları sağlar. |

### <a name="language-extensions"></a>Dil uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/current/static/plpgsql.html) | PL/pgSQL yüklenebilir yordam dili. |

### <a name="miscellaneous-extensions"></a>Çeşitli uzantılar

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [Yönetim Paketi](https://www.postgresql.org/docs/current/adminpack.html) | PostgreSQL için yönetim işlevleri. |
> | [amcheck](https://www.postgresql.org/docs/current/amcheck.html) | İlişki bütünlüğünü doğrulamaya yönelik işlevler. |
> | [Dosya \_ FDW](https://www.postgresql.org/docs/current/file-fdw.html) | Düz dosya erişimi için yabancı veri sarmalayıcısı. |
> | [pageincele](https://www.postgresql.org/docs/current/pageinspect.html) | Veritabanı sayfalarının içeriğini düşük bir düzeyde inceleyin. |
> | [PG \_ buffercache](https://www.postgresql.org/docs/current/static/pgbuffercache.html) | , Paylaşılan arabellek önbelleğinde gerçek zamanlı olarak neler olduğunu incelemek için bir yol sağlar. |
> | [sayfalık \_ cron](https://github.com/citusdata/pg_cron) | PostgreSQL için iş Zamanlayıcısı. |
> | [PG \_ freespacemap](https://www.postgresql.org/docs/current/pgfreespacemap.html) | Boş alan haritasını inceleyin (FSD). |
> | [sayfalık \_ ön sıcak](https://www.postgresql.org/docs/current/static/pgprewarm.html) | Arabellek önbelleğine ilişki verileri yüklemek için bir yol sağlar. |
> | [PG \_ stat \_ deyimleri](https://www.postgresql.org/docs/current/static/pgstatstatements.html) | Bir sunucu tarafından yürütülen tüm SQL deyimlerinin yürütme istatistiklerini izlemek için bir yol sağlar. Bu uzantı hakkında bilgi için "pg_stat_statements" bölümüne bakın. |
> | [sayfalık \_ görünürlük](https://www.postgresql.org/docs/current/pgvisibility.html) | Görünürlük haritasını (VM) ve sayfa düzeyi görünürlük bilgilerini inceleyin. |
> | [pgrowlocks](https://www.postgresql.org/docs/current/static/pgrowlocks.html) | Satır düzeyinde kilitleme bilgilerini göstermek için bir yol sağlar. |
> | [pgstattuple](https://www.postgresql.org/docs/current/static/pgstattuple.html) | Demet düzeyi istatistiklerini göstermek için bir yol sağlar. |
> | [Postgres \_ FDW](https://www.postgresql.org/docs/current/static/postgres-fdw.html) | Dış PostgreSQL sunucularında depolanan verilere erişmek için kullanılan yabancı veri sarmalayıcı. Bu uzantı hakkında bilgi için bkz. "dblink and postgres_fdw" bölümü.|
> | [sslinfo](https://www.postgresql.org/docs/current/sslinfo.html) | TLS/SSL sertifikaları hakkında bilgi. |
> | [TSD \_ sistem \_ satırları](https://www.postgresql.org/docs/current/tsm-system-rows.html) | Bir sınır olarak satır sayısını kabul eden, bu yöntem. |
> | [TSD \_ sistem \_ saati](https://www.postgresql.org/docs/current/tsm-system-time.html) | Bir sınır olarak milisaniye olarak zaman kabul eden, bu yöntem. |
> | [hypopg](https://hypopg.readthedocs.io/en/latest/) | CPU veya disk maliyeti olmayan kuramsal dizinler oluşturmak için bir yol sağlar. |
> | [dblink](https://www.postgresql.org/docs/current/dblink.html) | Bir veritabanı oturumunda diğer PostgreSQL veritabanlarına bağlantıları destekleyen bir modül. Bu uzantı hakkında bilgi için bkz. "dblink and postgres_fdw" bölümü. |
> | [xml2](https://www.postgresql.org/docs/current/xml2.html) | XPath sorgulama ve XSLT. |


### <a name="postgis-extensions"></a>PostGIS uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantı** | **Açıklama** |
> |---|---|
> | [PostGIS](https://www.postgis.net/), PostGIS \_ topolojisi, \_ \_ postgıal Tiger Geocoder, \_ postgısfcgal | PostgreSQL için uzamsal ve coğrafi nesneler. |
> | Adres \_ standartlayıcı, adres \_ standartlayıcısı \_ veri \_ ABD | Bir adresi bileşen öğelerine ayrıştırmak için kullanılır. Coğrafi kodlama adres normalleştirme adımını desteklemek için kullanılır. |
> | \_postgısfcgal | Postgısfcgal işlevleri. |
> | PostGIS \_ Tiger \_ Geocoder | PostGIS kocoder ve ters Geocoder. |
> | PostGIS \_ topolojisi | PostGIS topolojisi uzamsal türleri ve işlevleri. |


## <a name="pg_stat_statements"></a>pg_stat_statements
[Pg \_ stat \_ deyimleri uzantısı](https://www.postgresql.org/docs/current/pgstatstatements.html) , her PostgreSQL için Azure veritabanı sunucusuna önceden yüklenir ve SQL deyimlerinin yürütme istatistiklerini izlemeye yönelik bir yol sağlar.

Ayar, `pg_stat_statements.track` uzantı tarafından hangi deyimlerin sayıldığını denetler. Varsayılan olarak `top` , istemci tarafından doğrudan verilen tüm deyimlerin izlendiği anlamına gelir. Diğer iki izleme düzeyi `none` ve ' dir `all` . Bu ayar, [Azure Portal](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) veya [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli)aracılığıyla bir sunucu parametresi olarak yapılandırılabilir.

Pg_stat_statements sorgu yürütme bilgileri ile her SQL bildirisini günlüğe kaydettiği için sunucu performansı üzerindeki etki arasında bir zorunluluğunu getirir vardır. Pg_stat_statements uzantısını etkin bir şekilde kullanmıyorsanız, ' ye ayarlamanızı öneririz `pg_stat_statements.track` `none` . Bazı üçüncü taraf izleme Hizmetleri sorgu Performans öngörüleri sunmak için pg_stat_statements kullanabilir, bu nedenle bunun sizin için mi olduğunu doğrulayın.

## <a name="dblink-and-postgres_fdw"></a>dblink ve postgres_fdw

\_Tek bir PostgreSQL sunucusundan diğerine veya aynı sunucuda başka bir veritabanına bağlanmak için dblink ve Postgres FDW kullanabilirsiniz.  Alıcı sunucunun, gönderme sunucusundan güvenlik duvarı aracılığıyla bağlantılara izin verilmesi gerekir.  Bu uzantıları, PostgreSQL için Azure veritabanı sunucuları veya hiper ölçek (Citus) sunucu grupları arasında bağlanmak üzere kullanmak için, **Azure hizmetlerinin ve kaynaklarının bu sunucu grubuna (veya sunucusuna) erişmesine Izin ver** ' i ayarlayın.  Aynı sunucuya geri dönmek için uzantıları kullanmak istiyorsanız, bu ayarı da açmanız gerekir.
**Azure hizmetlerinin ve kaynaklarının bu sunucu grubuna erişmesine Izin ver** ayarı, **ağ**altındaki hiper ölçek (Citus) sunucu grubunun Azure Portal sayfasında bulunabilir.  Şu anda, PostgreSQL için Azure veritabanı tek sunuculu ve hiper ölçek (Citus) ile giden bağlantılar, PostgreSQL sunucuları ve hiper ölçek (Citus) sunucu grupları için diğer Azure veritabanı bağlantıları dışında desteklenmez.
