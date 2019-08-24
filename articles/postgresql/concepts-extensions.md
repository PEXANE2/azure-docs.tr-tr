---
title: PostgreSQL için Azure veritabanı 'nda PostgreSQL uzantılarını kullanma-tek sunucu
description: PostgreSQL için Azure veritabanı-tek sunucu içindeki uzantıları kullanarak veritabanınızın işlevselliğini genişletme özelliğini açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 08/23/2019
ms.openlocfilehash: 93cc02fafcfa153c452f37c2bc69bb47e2629f1d
ms.sourcegitcommit: 4b8a69b920ade815d095236c16175124a6a34996
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69998073"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda PostgreSQL uzantıları-tek sunucu
PostgreSQL, uzantıları kullanarak veritabanınızın işlevselliğini genişletmenizi sağlar. Uzantılar, tek bir komutla birlikte tek bir pakette yüklenebilecek veya kaldırılabileceği tek bir pakette birden çok ilgili SQL nesnesini birlikte paketleyebilir. Veritabanına yüklendikten sonra uzantı işlevleri yerleşik özellikler gibi.

## <a name="how-to-use-postgresql-extensions"></a>PostgreSQL uzantılarını kullanma
PostgreSQL uzantılarının kullanabilmeniz için veritabanınıza yüklenmesi gerekir. Belirli bir uzantıyı yüklemek için, psql aracından [uzantı](https://www.postgresql.org/docs/current/static/sql-createextension.html) Oluştur komutunu çalıştırarak paketlenmiş nesneleri veritabanınıza yükleyin.

PostgreSQL için Azure veritabanı aşağıda listelenen bir anahtar uzantıları alt kümesini destekler. Listelenenlerin ötesinde uzantılar desteklenmez. PostgreSQL için Azure veritabanı 'nda kendi uzantınızı oluşturamazsınız.

## <a name="extensions-supported-by-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı tarafından desteklenen uzantılar
Aşağıdaki tablolarda, şu anda PostgreSQL için Azure veritabanı tarafından desteklenen standart PostgreSQL uzantıları listelenmektedir. Bu bilgiler çalıştırılarak `SELECT * FROM pg_available_extensions;`da kullanılabilir.

### <a name="data-types-extensions"></a>Veri türleri uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantının** | **Açıklama** |
> |---|---|
> | [chkpass](https://www.postgresql.org/docs/9.6/static/chkpass.html) | Otomatik şifreli parolalar için bir veri türü sağlar. |
> | [citext](https://www.postgresql.org/docs/9.6/static/citext.html) | Büyük/küçük harf duyarsız bir karakter dizesi türü sağlar. |
> | [ünüzde](https://www.postgresql.org/docs/9.6/static/cube.html) | Çok boyutlu küpler için bir veri türü sağlar. |
> | [HStore](https://www.postgresql.org/docs/9.6/static/hstore.html) | Anahtar/değer çiftleri kümelerini depolamak için bir veri türü sağlar. |
> | [değilse](https://www.postgresql.org/docs/9.6/static/isn.html) | Uluslararası ürün numaralandırma standartları için veri türleri sağlar. |
> | [ltree](https://www.postgresql.org/docs/9.6/static/ltree.html) | Hiyerarşik ağaç benzeri yapılar için bir veri türü sağlar. |

### <a name="functions-extensions"></a>İşlev uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantının** | **Açıklama** |
> |---|---|
> | [earthdistance](https://www.postgresql.org/docs/9.6/static/earthdistance.html) | Dünya yüzeyinde büyük daire mesafelerini hesaplamak için bir yol sağlar. |
> | [belirsizlik zystrmatch](https://www.postgresql.org/docs/9.6/static/fuzzystrmatch.html) | , Dizeler arasındaki benzerlikleri ve mesafeyi belirlemede çeşitli işlevler sağlar. |
> | [ıntarray](https://www.postgresql.org/docs/9.6/static/intarray.html) | Tamsayıların null ve boş dizilerini işlemek için işlevler ve işleçler sağlar. |
> | [pgşifre](https://www.postgresql.org/docs/9.6/static/pgcrypto.html) | Şifreleme işlevleri sağlar. |
> | [sayfalık\_partman](https://pgxn.org/dist/pg_partman/doc/pg_partman.html) | Bölümlenmiş tabloları zamana veya KIMLIĞE göre yönetir. |
> | [sayfalık\_TRGM](https://www.postgresql.org/docs/9.6/static/pgtrgm.html) | Trigram eşleştirmeye göre alfasayısal metnin benzerliğini belirlemek için işlevler ve işleçler sağlar. |
> | [tablofunc](https://www.postgresql.org/docs/9.6/static/tablefunc.html) | Çapraz tablo dahil olmak üzere tüm tabloları düzenleyen işlevler sağlar. |
> | [UUID-ossp](https://www.postgresql.org/docs/9.6/static/uuid-ossp.html) | Evrensel benzersiz tanımlayıcılar (UUID 'ler) oluşturur. (Bu uzantının bir notunun altına bakın). |
> | [orafce](https://github.com/orafce/orafce) | Ticari veritabanlarından Öykünülen işlevlerin ve paketlerin bir alt kümesini sağlar. |

### <a name="full-text-search-extensions"></a>Tam metin arama uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantının** | **Açıklama** |
> |---|---|
> | [Dict\_int](https://www.postgresql.org/docs/9.6/static/dict-int.html) | Tamsayılar için bir metin arama sözlüğü şablonu sağlar. |
> | [vurgu kaldır](https://www.postgresql.org/docs/9.6/static/unaccent.html) | Lexemes 'ten vurguları (aksan işaretleri) kaldıran bir metin arama sözlüğü. |

### <a name="index-types-extensions"></a>Dizin türleri uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantının** | **Açıklama** |
> |---|---|
> | [BTREE\_gın](https://www.postgresql.org/docs/9.6/static/btree-gin.html) | Belirli veri türleri için B ağacı gibi davranışı uygulayan örnek bir işleç sınıfları sağlar. |
> | [BTREE\_GİST](https://www.postgresql.org/docs/9.6/static/btree-gist.html) | B-ağacı uygulayan GiST Dizin işleci sınıfları sağlar. |

### <a name="language-extensions"></a>Dil uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantının** | **Açıklama** |
> |---|---|
> | [plpgsql](https://www.postgresql.org/docs/9.6/static/plpgsql.html) | PL/pgSQL yüklenebilir yordam dili. |
> | [plv8](https://plv8.github.io/) | Bir PostgreSQL için, saklı yordamlar, Tetikleyiciler, vb. için kullanılabilen bir JavaScript dil uzantısı. |

### <a name="miscellaneous-extensions"></a>Çeşitli uzantılar

> [!div class="mx-tableFixed"]
> | **Uzantının** | **Açıklama** |
> |---|---|
> | [PG\_buffercache](https://www.postgresql.org/docs/9.6/static/pgbuffercache.html) | , Paylaşılan arabellek önbelleğinde gerçek zamanlı olarak neler olduğunu incelemek için bir yol sağlar. |
> | [sayfalık\_ön sıcak](https://www.postgresql.org/docs/9.6/static/pgprewarm.html) | Arabellek önbelleğine ilişki verileri yüklemek için bir yol sağlar. |
> | [PG\_stat\_deyimleri](https://www.postgresql.org/docs/9.6/static/pgstatstatements.html) | Bir sunucu tarafından yürütülen tüm SQL deyimlerinin yürütme istatistiklerini izlemek için bir yol sağlar. (Bu uzantının bir notunun altına bakın). |
> | [pgrowlocks](https://www.postgresql.org/docs/9.6/static/pgrowlocks.html) | Satır düzeyinde kilitleme bilgilerini göstermek için bir yol sağlar. |
> | [pgstattuple](https://www.postgresql.org/docs/9.6/static/pgstattuple.html) | Demet düzeyi istatistiklerini göstermek için bir yol sağlar. |
> | [Postgres\_FDW](https://www.postgresql.org/docs/9.6/static/postgres-fdw.html) | Dış PostgreSQL sunucularında depolanan verilere erişmek için kullanılan yabancı veri sarmalayıcı. (Bu uzantının bir notunun altına bakın).|
> | [hypopg](https://hypopg.readthedocs.io/en/latest/) | CPU veya disk maliyeti olmayan kuramsal dizinler oluşturmak için bir yol sağlar. |
> | [dblink](https://www.postgresql.org/docs/current/dblink.html) | Bir veritabanı oturumunda diğer PostgreSQL veritabanlarına bağlantıları destekleyen bir modül. (Bu uzantının bir notunun altına bakın). |


### <a name="postgis-extensions"></a>PostGIS uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantının** | **Açıklama** |
> |---|---|
> | [PostGIS](https://www.postgis.net/), PostGIS\_topolojisi,\_postgıal Tiger\_Geocoder\_, postgısfcgal | PostgreSQL için uzamsal ve coğrafi nesneler. |
> | Adres\_standartlayıcı, adres\_standartlayıcısı\_veri\_ABD | Bir adresi bileşen öğelerine ayrıştırmak için kullanılır. Coğrafi kodlama adres normalleştirme adımını desteklemek için kullanılır. |
> | [pgrouting](https://pgrouting.org/) | Jeo-uzamsal yönlendirme işlevselliği sağlamak için PostGIS/PostgreSQL Jeo uzamsal veritabanını genişletir. |


### <a name="time-series-extensions"></a>Zaman serisi uzantıları

> [!div class="mx-tableFixed"]
> | **Uzantının** | **Açıklama** |
> |---|---|
> | [TimescaleDB](https://docs.timescale.com/latest) | Daha hızlı alma ve sorgular için otomatik Bölümlendirmeyi destekleyen bir zaman serisi SQL veritabanı. Zamana dayalı analitik işlevler, iyileştirmeler sağlar ve zaman serisi iş yükleri için PostgreSQL 'i ölçeklendirir. TimescaleDB, tarafından geliştirilmiştir ve [zaman ölçeği, Inc. '](https://www.timescale.com/) in tescilli ticari markasıdır. (Bu uzantının bir notunun altına bakın). |

## <a name="postgres-11-extensions"></a>Postgres 11 uzantıları

Aşağıdaki uzantılar Postgres sürüm 11 ' in PostgreSQL sunucuları için Azure veritabanı 'nda mevcuttur.

> [!div class="mx-tableFixed"]
> | **Uzantının**| **Uzantı sürümü** | **Açıklama** |
> |---|---|---|
> |[address_standardizer](http://postgis.net/docs/Address_Standardizer.html)         | 2.5.1           | Bir adresi bileşen öğelerine ayrıştırmak için kullanılır. |
> |[address_standardizer_data_us](http://postgis.net/docs/Address_Standardizer.html) | 2.5.1           | Adres Standartlayıcı ABD veri kümesi örneği|
> |[btree_gin](https://www.postgresql.org/docs/11/btree-gin.html)                    | 1.3             | GıN 'te ortak veri türlerini dizinlemeye yönelik destek|
> |[btree_gist](https://www.postgresql.org/docs/11/btree-gist.html)                   | 1.5             | GiST 'de ortak veri türlerini dizinlemeye yönelik destek|
> |[citext](https://www.postgresql.org/docs/11/static/citext.html)                       | 1.5             | büyük/küçük harf duyarsız karakter dizeleri için veri türü|
> |[ünüzde](https://www.postgresql.org/docs/11/static/cube.html)                         | 1.4             | çok boyutlu küpler için veri türü|
> |[dblink](https://www.postgresql.org/docs/11/dblink.html)                       | 1.2             | bir veritabanı içinden diğer PostgreSQL veritabanlarına bağlanma|
> |[dict_int](https://www.postgresql.org/docs/11/static/dict-int.html)                     | 1.0             | tamsayılar için metin arama sözlük şablonu|
> |[earthdistance](https://www.postgresql.org/docs/11/static/earthdistance.html)                | 1.1             | Dünya yüzeyinde büyük daire mesafelerini hesaplama|
> |[belirsizlik zystrmatch](https://www.postgresql.org/docs/11/static/fuzzystrmatch.html)                | 1.1             | dizeler arasındaki benzerlikleri ve mesafeyi belirleme|
> |[HStore](https://www.postgresql.org/docs/11/static/hstore.html)                       | 1.5             | (anahtar, değer) çiftleri kümelerini depolamak için veri türü|
> |[hypopg](https://hypopg.readthedocs.io/en/latest/)                       | 1.1.2           | PostgreSQL için kuramsal dizinler|
> |[ıntarray](https://www.postgresql.org/docs/11/static/intarray.html)                     | 1.2             | tamsayı dizileri için işlevler, işleçler ve Dizin desteği|
> |[değilse](https://www.postgresql.org/docs/11/static/isn.html)                          | 1.2             | Uluslararası ürün numaralandırma standartları için veri türleri|
> |[ltree](https://www.postgresql.org/docs/11/static/ltree.html)                        | 1.1             | hiyerarşik ağaç benzeri yapılar için veri türü|
> |[orafce](https://github.com/orafce/orafce)                       | 3.7             | Ticari RDBMS 'den işlevlerin ve paketlerin bir alt kümesini taklit eden işlevler ve işleçler|
> |[pgşifre](https://www.postgresql.org/docs/11/static/pgcrypto.html)                     | 1.3             | şifreleme işlevleri|
> |[pgrouting](https://pgrouting.org/)                    | 2.6.2 Critical           | pgRouting uzantısı|
> |[pgrowlocks](https://www.postgresql.org/docs/11/static/pgrowlocks.html)                   | 1.2             | satır düzeyinde kilitleme bilgilerini göster|
> |[pgstattuple](https://www.postgresql.org/docs/11/static/pgstattuple.html)                  | 1.5             | demet düzeyi istatistiklerini göster|
> |[pg_buffercache](https://www.postgresql.org/docs/11/static/pgbuffercache.html)               | 1.3             | Paylaşılan arabellek önbelleğini inceleyin|
> |[pg_partman](https://github.com/pgpartman/pg_partman)                   | 4.0.0           | Bölümlenmiş tabloları zamana veya KIMLIĞE göre yönetme uzantısı|
> |[pg_prewarm](https://www.postgresql.org/docs/11/pgprewarm.html)                   | 1.2             | ön sıcak ilişki verileri|
> |[pg_stat_statements](https://www.postgresql.org/docs/11/static/pgstatstatements.html)           | 1.6             | yürütülen tüm SQL deyimlerinin yürütme istatistiklerini izleyin|
> |[pg_trgm](https://www.postgresql.org/docs/11/static/pgtrgm.html)                      | 1.4             | trigram temelinde metin benzerliği ölçümü ve Dizin arama|
> |[plpgsql](https://www.postgresql.org/docs/11/static/plpgsql.html)                      | 1.0             | PL/pgSQL yordam dili|
> |[plv8](https://plv8.github.io/)                         | 2.3.11          | PL/JavaScript (V8) güvenilen yordamsal dil|
> |[PostGIS](https://www.postgis.net/)                      | 2.5.1           | PostGIS geometrisi, coğrafya ve raster uzamsal türleri ve işlevleri|
> |[postgis_sfcgal](https://www.postgis.net/)               | 2.5.1           | Postgısfcgal işlevleri|
> |[postgis_tiger_geocoder](https://www.postgis.net/)       | 2.5.1           | PostGIS Tiger Geocoder ve ters coğrafi dişli|
> |[postgis_topology](https://postgis.net/docs/Topology.html)             | 2.5.1           | PostGIS topolojisi uzamsal türleri ve işlevleri|
> |[postgres_fdw](https://www.postgresql.org/docs/11/static/postgres-fdw.html)                 | 1.0             | uzak PostgreSQL sunucuları için yabancı veri sarmalayıcısı|
> |[tablofunc](https://www.postgresql.org/docs/11/static/tablefunc.html)                    | 1.0             | çapraz tablo dahil olmak üzere tüm tabloları işleyen işlevler|
> |[vurgu kaldır](https://www.postgresql.org/docs/11/static/unaccent.html)                     | 1.1             | vurguları kaldıran metin arama sözlüğü|
> |[UUID-ossp](https://www.postgresql.org/docs/11/static/uuid-ossp.html)                    | 1.1             | evrensel benzersiz tanımlayıcılar oluşturma (UUID 'ler)|


## <a name="pg_stat_statements"></a>pg_stat_statements
Pg_stat_statements uzantısı, SQL deyimlerinin yürütme istatistiklerini izlemek için her PostgreSQL için Azure veritabanı sunucusuna önceden yüklenir.
Uzantı tarafından `pg_stat_statements.track`hangi deyimlerin sayıldığını denetleyen ayar, varsayılan olarak `top`, istemciler tarafından doğrudan verilen tüm deyimler izlenir. Diğer iki izleme düzeyi ve ' `none` `all`dir. Bu ayar, [Azure Portal](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) veya [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli)aracılığıyla bir sunucu parametresi olarak yapılandırılabilir.

Sorgu yürütme bilgileri pg_stat_statements tarafından sağlanır ve her SQL bildirisini günlüğe kaydettiği için sunucu performansı üzerindeki etki arasında bir zorunluluğunu getirir vardır. Etkin olarak pg_stat_statements uzantısını kullanmıyorsanız, ' ye `pg_stat_statements.track` `none`ayarlamanızı öneririz. Bazı üçüncü taraf izleme hizmetlerinin sorgu Performans öngörüleri sunabilmesi için pg_stat_statements 'e güvenebileceğini unutmayın. bu nedenle, sizin için bu durum olup olmadığını onaylayın.

## <a name="dblink-and-postgres_fdw"></a>dblink ve postgres_fdw
dblink ve postgres_fdw, bir PostgreSQL sunucusundan diğerine veya aynı sunucuda başka bir veritabanına bağlanmanıza izin verir. Alıcı sunucunun, gönderme sunucusundan güvenlik duvarı aracılığıyla bağlantılara izin verilmesi gerekir. PostgreSQL için Azure veritabanı sunucuları arasında bağlanmak üzere bu uzantıları kullanırken, bu, "Azure hizmetlerine erişime Izin ver" ayarı kullanılarak yapılabilir. Ayrıca, uzantıları aynı sunucuya geri dönmek için kullanmak istiyorsanız bu da gereklidir. "Azure hizmetlerine erişime Izin ver" ayarı, Postgres sunucusunun Azure portal sayfasında, bağlantı güvenliği altında bulunabilir. "Azure hizmetlerine erişime Izin ver" seçeneği açıldığında tüm Azure IP 'Leri izin verilenler listesine koyar.

Şu anda PostgreSQL için Azure veritabanı 'na giden bağlantılar, PostgreSQL için Azure veritabanı sunucuları için bağlantılar dışında desteklenmez.

## <a name="uuid"></a>uuid
UUID-ossp uzantısından `uuid_generate_v4()` kullanmayı planlıyorsanız performans avantajları için pgşifre Uzantısı ile `gen_random_uuid()` karşılaştırmayı göz önünde bulundurun.


## <a name="timescaledb"></a>TimescaleDB
TimescaleDB, PostgreSQL için bir uzantı olarak paketlenmiş bir zaman serisi veritabanıdır. TimescaleDB zamana dayalı analitik işlevler, iyileştirmeler ve zaman serisi iş yükleri için Postgres ölçekleme sağlar.

[TimescaleDB hakkında daha fazla bilgi](https://docs.timescale.com/latest)için bkz [. zaman ölçeğinin tescilli ticari marka, Inc.](https://www.timescale.com/)

### <a name="installing-timescaledb"></a>TimescaleDB yükleniyor
TimescaleDB yüklemek için, bunu sunucunun paylaşılan önyükleme kitaplıklarına dahil etmeniz gerekir. Postgres 'nin `shared_preload_libraries` parametresinin bir değişikliği, **sunucu yeniden başlatmanın** etkili olmasını gerektirir. [Azure Portal](howto-configure-server-parameters-using-portal.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md)kullanarak parametreleri değiştirebilirsiniz.

> [!NOTE]
> TimescaleDB, PostgreSQL için Azure veritabanı 9,6 ve 10 sürümleri üzerinde etkinleştirilebilir

[Azure Portal](https://portal.azure.com/)kullanarak:

1. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

2. Kenar çubuğunda **sunucu parametreleri**' ni seçin.

3. `shared_preload_libraries` Parametresi için arama yapın.

4. **TimescaleDB**öğesini seçin.

5. Değişikliklerinizi korumak için **Kaydet** ' i seçin. Değişiklik kaydedildikten sonra bir bildirim alırsınız. 

6. Bildirimden sonra, bu değişiklikleri uygulamak için sunucuyu **yeniden başlatın** . Bir sunucuyu yeniden başlatmayı öğrenmek için bkz. [PostgreSQL Için Azure veritabanını yeniden başlatma sunucusu](howto-restart-server-portal.md).


Artık Postgres veritabanınızda TimescaleDB 'ı etkinleştirebilirsiniz. Veritabanına bağlanın ve aşağıdaki komutu verin:
```sql
CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;
```
> [!TIP]
> Bir hata görürseniz, shared_preload_libraries kaydettikten sonra [sunucunuzu yeniden başlattığınızdan](howto-restart-server-portal.md) emin olun. 

Artık, [sıfırdan](https://docs.timescale.com/getting-started/creating-hypertables) bir TimescaleDB hypertable oluşturabilir veya [PostgreSQL içindeki mevcut zaman serisi verilerini](https://docs.timescale.com/getting-started/migrating-data)geçirebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
Kullanmak istediğiniz bir uzantıyı görmüyorsanız bize bize izin verin. Mevcut istekleri oylayın veya [geri bildirim forumumuzda](https://feedback.azure.com/forums/597976-azure-database-for-postgresql)yeni geri bildirim istekleri oluşturun.
