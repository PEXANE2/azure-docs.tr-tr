---
title: Uzantılar - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL için Azure Veritabanı'ndaki mevcut Postgres uzantıları hakkında bilgi edinin - Tek Sunucu
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: a12738f5de783c8a34718b8d9cb4bbf54f230589
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201280"
---
# <a name="postgresql-extensions-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı'nda PostgreSQL uzantıları - Single Server
PostgreSQL uzantıları kullanarak veritabanınızın işlevselliğini genişletmek için yeteneği sağlar. Uzantılar, birden çok ilişkili SQL nesnesini, tek bir komutla veritabanınızdan yüklenebilecek veya kaldırılabilen tek bir pakette bir araya getirir. Veritabanına yüklendikten sonra, uzantılar yerleşik özellikler gibi çalışır.

## <a name="how-to-use-postgresql-extensions"></a>PostgreSQL uzantıları nasıl kullanılır?
PostgreSQL uzantılarını kullanamadan önce veritabanınıza yüklenmesi gerekir. Belirli bir uzantıyı yüklemek için, paketlenmiş nesneleri veritabanınıza yüklemek için psql aracından [CREATE EXTENSION](https://www.postgresql.org/docs/current/sql-createextension.html) komutunu çalıştırın.

PostgreSQL için Azure Veritabanı, aşağıda listelenen anahtar uzantılarının bir alt kümesini destekler. Bu bilgiler çalıştırılarak `SELECT * FROM pg_available_extensions;`da kullanılabilir. Listelenenuzantıları n dışındaki uzantılar desteklenmez. PostgreSQL için Azure Veritabanı'nda kendi uzantınızı oluşturamazsınız.

## <a name="postgres-11-extensions"></a>Postgres 11 uzantıları

Aşağıdaki uzantılar, Postgres sürüm 11'e sahip PostgreSQL sunucuları için Azure Veritabanı'nda kullanılabilir. 

> [!div class="mx-tableFixed"]
> | **Dahili numara**| **Uzantı sürümü** | **Açıklama** |
> |---|---|---|
> |[address_standardizer](http://postgis.net/docs/Address_Standardizer.html)         | 2.5.1           | Bir adresi kurucu öğelere ayrışdırmak için kullanılır. |
> |[address_standardizer_data_us](http://postgis.net/docs/Address_Standardizer.html) | 2.5.1           | Adres Standardizer ABD veri seti örneği|
> |[btree_gin](https://www.postgresql.org/docs/11/btree-gin.html)                    | 1.3             | GIN'de ortak veri türlerini dizine alma desteği|
> |[btree_gist](https://www.postgresql.org/docs/11/btree-gist.html)                   | 1,5             | GiST'de ortak veri türlerini dizine alma desteği|
> |[citext](https://www.postgresql.org/docs/11/citext.html)                       | 1,5             | büyük/küçük harf duyarsız karakter dizeleri için veri türü|
> |[Küp](https://www.postgresql.org/docs/11/cube.html)                         | 1.4             | çok boyutlu küpler için veri türü|
> |[Dblink](https://www.postgresql.org/docs/11/dblink.html)                       | 1.2             | bir veritabanı içinden diğer PostgreSQL veritabanlarına bağlanmak|
> |[dict_int](https://www.postgresql.org/docs/11/dict-int.html)                     | 1.0             | sondalar için metin arama sözlüğü şablonu|
> |[toprak mesafesi](https://www.postgresql.org/docs/11/earthdistance.html)                | 1.1             | Dünya yüzeyinde büyük daire mesafeleri hesaplamak|
> |[bulanıkça](https://www.postgresql.org/docs/11/fuzzystrmatch.html)                | 1.1             | dizeleri arasındaki benzerlikleri ve mesafeyi belirlemek|
> |[hstore](https://www.postgresql.org/docs/11/hstore.html)                       | 1,5             | (anahtar, değer) çiftleri kümeleri depolamak için veri türü|
> |[hipopg](https://hypopg.readthedocs.io/en/latest/)                       | 1.1.2           | PostgreSQL için varsayımsal dizinler|
> |[intarray](https://www.postgresql.org/docs/11/intarray.html)                     | 1.2             | 1-B dizileri için fonksiyonlar, işleçler ve dizin desteği|
> |[ısn](https://www.postgresql.org/docs/11/isn.html)                          | 1.2             | uluslararası ürün numaralandırma standartları için veri türleri|
> |[ltree](https://www.postgresql.org/docs/11/ltree.html)                        | 1.1             | hiyerarşik ağaç benzeri yapılar için veri türü|
> |[orafce](https://github.com/orafce/orafce)                       | 3.7             | Ticari RDBMS'den fonksiyonlar ve paketlerin bir alt kümesini taklit eden işlevler ve işleçler|
> |[pgaudit](https://www.pgaudit.org/)                     | 1.3.1             | denetim işlevselliği sağlar|
> |[pgcrypto](https://www.postgresql.org/docs/11/pgcrypto.html)                     | 1.3             | şifreleme fonksiyonları|
> |[pgrouting](https://pgrouting.org/)                    | 2.6.2           | pgRouting Uzantısı|
> |[pgrowlocks](https://www.postgresql.org/docs/11/pgrowlocks.html)                   | 1.2             | satır düzeyinde kilitleme bilgilerini gösterme|
> |[pgstattuple](https://www.postgresql.org/docs/11/pgstattuple.html)                  | 1,5             | tuple düzeyindeki istatistikleri gösterme|
> |[pg_buffercache](https://www.postgresql.org/docs/11/pgbuffercache.html)               | 1.3             | paylaşılan arabellek önbelleğini inceleme|
> |[pg_partman](https://github.com/pgpartman/pg_partman)                   | 4.0.0           | Bölümlenmiş tabloları zamana veya kimlikle yönetme uzantısı|
> |[pg_prewarm](https://www.postgresql.org/docs/11/pgprewarm.html)                   | 1.2             | prewarm ilişki verileri|
> |[pg_stat_statements](https://www.postgresql.org/docs/11/pgstatstatements.html)           | 1.6             | yürütülen tüm SQL deyimlerinin izleme yürütme istatistikleri|
> |[pg_trgm](https://www.postgresql.org/docs/11/pgtrgm.html)                      | 1.4             | trigrams dayalı metin benzerlik ölçümü ve dizin arama|
> |[plpgsql](https://www.postgresql.org/docs/11/plpgsql.html)                      | 1.0             | PL/pgSQL yordam dili|
> |[plv8](https://plv8.github.io/)                         | 2.3.11          | PL/JavaScript (v8) güvenilir yordam dili|
> |[postgis](https://www.postgis.net/)                      | 2.5.1           | PostGIS geometri, coğrafya ve raster mekansal türleri ve fonksiyonları|
> |[postgis_sfcgal](https://www.postgis.net/)               | 2.5.1           | PostGIS SFCGAL fonksiyonları|
> |[postgis_tiger_geocoder](https://www.postgis.net/)       | 2.5.1           | PostGIS kaplan geocoder ve ters geocoder|
> |[postgis_topology](https://postgis.net/docs/Topology.html)             | 2.5.1           | PostGIS topoloji mekansal türleri ve fonksiyonları|
> |[postgres_fdw](https://www.postgresql.org/docs/11/postgres-fdw.html)                 | 1.0             | uzak PostgreSQL sunucuları için yabancı veri sarıcı|
> |[tablefunc](https://www.postgresql.org/docs/11/tablefunc.html)                    | 1.0             | çapraz sekme de dahil olmak üzere tüm tabloları manipüle eden işlevler|
> |[zaman ölçeğidb](https://docs.timescale.com/latest)                    | 1.3.2             | Zaman serisi verileri için ölçeklenebilir ekler ve karmaşık sorgular sağlar|
> |[aksansız](https://www.postgresql.org/docs/11/unaccent.html)                     | 1.1             | vurguları kaldıran metin arama sözlüğü|
> |[uuid-ossp](https://www.postgresql.org/docs/11/uuid-ossp.html)                    | 1.1             | evrensel olarak benzersiz tanımlayıcılar (UUIDs) oluşturmak|

## <a name="postgres-10-extensions"></a>Postgres 10 uzantıları 

Aşağıdaki uzantılar, Postgres sürüm 10'a sahip PostgreSQL sunucuları için Azure Veritabanı'nda kullanılabilir.

> [!div class="mx-tableFixed"]
> | **Dahili numara**| **Uzantı sürümü** | **Açıklama** |
> |---|---|---|
> |[address_standardizer](http://postgis.net/docs/Address_Standardizer.html)         | 2.5.1           | Bir adresi kurucu öğelere ayrışdırmak için kullanılır. |
> |[address_standardizer_data_us](http://postgis.net/docs/Address_Standardizer.html) | 2.5.1           | Adres Standardizer ABD veri seti örneği|
> |[btree_gin](https://www.postgresql.org/docs/10/btree-gin.html)                    | 1.3             | GIN'de ortak veri türlerini dizine alma desteği|
> |[btree_gist](https://www.postgresql.org/docs/10/btree-gist.html)                   | 1,5             | GiST'de ortak veri türlerini dizine alma desteği|
> |[chkpass](https://www.postgresql.org/docs/10/chkpass.html)                       | 1.0             | otomatik şifrelenmiş parolalar için veri türü|
> |[citext](https://www.postgresql.org/docs/10/citext.html)                       | 1.4             | büyük/küçük harf duyarsız karakter dizeleri için veri türü|
> |[Küp](https://www.postgresql.org/docs/10/cube.html)                         | 1.2             | çok boyutlu küpler için veri türü|
> |[Dblink](https://www.postgresql.org/docs/10/dblink.html)                       | 1.2             | bir veritabanı içinden diğer PostgreSQL veritabanlarına bağlanmak|
> |[dict_int](https://www.postgresql.org/docs/10/dict-int.html)                     | 1.0             | sondalar için metin arama sözlüğü şablonu|
> |[toprak mesafesi](https://www.postgresql.org/docs/10/earthdistance.html)                | 1.1             | Dünya yüzeyinde büyük daire mesafeleri hesaplamak|
> |[bulanıkça](https://www.postgresql.org/docs/10/fuzzystrmatch.html)                | 1.1             | dizeleri arasındaki benzerlikleri ve mesafeyi belirlemek|
> |[hstore](https://www.postgresql.org/docs/10/hstore.html)                       | 1.4             | (anahtar, değer) çiftleri kümeleri depolamak için veri türü|
> |[hipopg](https://hypopg.readthedocs.io/en/latest/)                       | 1.1.1           | PostgreSQL için varsayımsal dizinler|
> |[intarray](https://www.postgresql.org/docs/10/intarray.html)                     | 1.2             | 1-B dizileri için fonksiyonlar, işleçler ve dizin desteği|
> |[ısn](https://www.postgresql.org/docs/10/isn.html)                          | 1.1             | uluslararası ürün numaralandırma standartları için veri türleri|
> |[ltree](https://www.postgresql.org/docs/10/ltree.html)                        | 1.1             | hiyerarşik ağaç benzeri yapılar için veri türü|
> |[orafce](https://github.com/orafce/orafce)                       | 3.7             | Ticari RDBMS'den fonksiyonlar ve paketlerin bir alt kümesini taklit eden işlevler ve işleçler|
> |[pgaudit](https://www.pgaudit.org/)                     | 1.2             | denetim işlevselliği sağlar|
> |[pgcrypto](https://www.postgresql.org/docs/10/pgcrypto.html)                     | 1.3             | şifreleme fonksiyonları|
> |[pgrouting](https://pgrouting.org/)                    | 2.5.2           | pgRouting Uzantısı|
> |[pgrowlocks](https://www.postgresql.org/docs/10/pgrowlocks.html)                   | 1.2             | satır düzeyinde kilitleme bilgilerini gösterme|
> |[pgstattuple](https://www.postgresql.org/docs/10/pgstattuple.html)                  | 1,5             | tuple düzeyindeki istatistikleri gösterme|
> |[pg_buffercache](https://www.postgresql.org/docs/10/pgbuffercache.html)               | 1.3             | paylaşılan arabellek önbelleğini inceleme|
> |[pg_partman](https://github.com/pgpartman/pg_partman)                   | 2.6.3           | Bölümlenmiş tabloları zamana veya kimlikle yönetme uzantısı|
> |[pg_prewarm](https://www.postgresql.org/docs/10/pgprewarm.html)                   | 1.1             | prewarm ilişki verileri|
> |[pg_stat_statements](https://www.postgresql.org/docs/10/pgstatstatements.html)           | 1.6             | yürütülen tüm SQL deyimlerinin izleme yürütme istatistikleri|
> |[pg_trgm](https://www.postgresql.org/docs/10/pgtrgm.html)                      | 1.3             | trigrams dayalı metin benzerlik ölçümü ve dizin arama|
> |[plpgsql](https://www.postgresql.org/docs/10/plpgsql.html)                      | 1.0             | PL/pgSQL yordam dili|
> |[plv8](https://plv8.github.io/)                         | 2.1.0          | PL/JavaScript (v8) güvenilir yordam dili|
> |[postgis](https://www.postgis.net/)                      | 2.4.3           | PostGIS geometri, coğrafya ve raster mekansal türleri ve fonksiyonları|
> |[postgis_sfcgal](https://www.postgis.net/)               | 2.4.3           | PostGIS SFCGAL fonksiyonları|
> |[postgis_tiger_geocoder](https://www.postgis.net/)       | 2.4.3           | PostGIS kaplan geocoder ve ters geocoder|
> |[postgis_topology](https://postgis.net/docs/Topology.html)             | 2.4.3           | PostGIS topoloji mekansal türleri ve fonksiyonları|
> |[postgres_fdw](https://www.postgresql.org/docs/10/postgres-fdw.html)                 | 1.0             | uzak PostgreSQL sunucuları için yabancı veri sarıcı|
> |[tablefunc](https://www.postgresql.org/docs/10/tablefunc.html)                    | 1.0             | çapraz sekme de dahil olmak üzere tüm tabloları manipüle eden işlevler|
> |[zaman ölçeğidb](https://docs.timescale.com/latest)                    | 1.1.1             | Zaman serisi verileri için ölçeklenebilir ekler ve karmaşık sorgular sağlar|
> |[aksansız](https://www.postgresql.org/docs/10/unaccent.html)                     | 1.1             | vurguları kaldıran metin arama sözlüğü|
> |[uuid-ossp](https://www.postgresql.org/docs/10/uuid-ossp.html)                    | 1.1             | evrensel olarak benzersiz tanımlayıcılar (UUIDs) oluşturmak|

## <a name="postgres-96-extensions"></a>Postgres 9.6 uzantıları 

Aşağıdaki uzantılar, Postgres sürümü 9.6 olan PostgreSQL sunucuları için Azure Veritabanı'nda kullanılabilir.

> [!div class="mx-tableFixed"]
> | **Dahili numara**| **Uzantı sürümü** | **Açıklama** |
> |---|---|---|
> |[address_standardizer](http://postgis.net/docs/Address_Standardizer.html)         | 2.3.2           | Bir adresi kurucu öğelere ayrışdırmak için kullanılır. |
> |[address_standardizer_data_us](http://postgis.net/docs/Address_Standardizer.html) | 2.3.2           | Adres Standardizer ABD veri seti örneği|
> |[btree_gin](https://www.postgresql.org/docs/9.6/btree-gin.html)                    | 1.0             | GIN'de ortak veri türlerini dizine alma desteği|
> |[btree_gist](https://www.postgresql.org/docs/9.6/btree-gist.html)                   | 1.2             | GiST'de ortak veri türlerini dizine alma desteği|
> |[chkpass](https://www.postgresql.org/docs/9.6/chkpass.html)                       | 1.0             | otomatik şifrelenmiş parolalar için veri türü|
> |[citext](https://www.postgresql.org/docs/9.6/citext.html)                       | 1.3             | büyük/küçük harf duyarsız karakter dizeleri için veri türü|
> |[Küp](https://www.postgresql.org/docs/9.6/cube.html)                         | 1.2             | çok boyutlu küpler için veri türü|
> |[Dblink](https://www.postgresql.org/docs/9.6/dblink.html)                       | 1.2             | bir veritabanı içinden diğer PostgreSQL veritabanlarına bağlanmak|
> |[dict_int](https://www.postgresql.org/docs/9.6/dict-int.html)                     | 1.0             | sondalar için metin arama sözlüğü şablonu|
> |[toprak mesafesi](https://www.postgresql.org/docs/9.6/earthdistance.html)                | 1.1             | Dünya yüzeyinde büyük daire mesafeleri hesaplamak|
> |[bulanıkça](https://www.postgresql.org/docs/9.6/fuzzystrmatch.html)                | 1.1             | dizeleri arasındaki benzerlikleri ve mesafeyi belirlemek|
> |[hstore](https://www.postgresql.org/docs/9.6/hstore.html)                       | 1.4             | (anahtar, değer) çiftleri kümeleri depolamak için veri türü|
> |[hipopg](https://hypopg.readthedocs.io/en/latest/)                       | 1.1.1           | PostgreSQL için varsayımsal dizinler|
> |[intarray](https://www.postgresql.org/docs/9.6/intarray.html)                     | 1.2             | 1-B dizileri için fonksiyonlar, işleçler ve dizin desteği|
> |[ısn](https://www.postgresql.org/docs/9.6/isn.html)                          | 1.1             | uluslararası ürün numaralandırma standartları için veri türleri|
> |[ltree](https://www.postgresql.org/docs/9.6/ltree.html)                        | 1.1             | hiyerarşik ağaç benzeri yapılar için veri türü|
> |[orafce](https://github.com/orafce/orafce)                       | 3.7             | Ticari RDBMS'den fonksiyonlar ve paketlerin bir alt kümesini taklit eden işlevler ve işleçler|
> |[pgaudit](https://www.pgaudit.org/)                     | 1.1.2             | denetim işlevselliği sağlar|
> |[pgcrypto](https://www.postgresql.org/docs/9.6/pgcrypto.html)                     | 1.3             | şifreleme fonksiyonları|
> |[pgrouting](https://pgrouting.org/)                    | 2.3.2           | pgRouting Uzantısı|
> |[pgrowlocks](https://www.postgresql.org/docs/9.6/pgrowlocks.html)                   | 1.2             | satır düzeyinde kilitleme bilgilerini gösterme|
> |[pgstattuple](https://www.postgresql.org/docs/9.6/pgstattuple.html)                  | 1.4             | tuple düzeyindeki istatistikleri gösterme|
> |[pg_buffercache](https://www.postgresql.org/docs/9.6/pgbuffercache.html)               | 1.2             | paylaşılan arabellek önbelleğini inceleme|
> |[pg_partman](https://github.com/pgpartman/pg_partman)                   | 2.6.3           | Bölümlenmiş tabloları zamana veya kimlikle yönetme uzantısı|
> |[pg_prewarm](https://www.postgresql.org/docs/9.6/pgprewarm.html)                   | 1.1             | prewarm ilişki verileri|
> |[pg_stat_statements](https://www.postgresql.org/docs/9.6/pgstatstatements.html)           | 1.4             | yürütülen tüm SQL deyimlerinin izleme yürütme istatistikleri|
> |[pg_trgm](https://www.postgresql.org/docs/9.6/pgtrgm.html)                      | 1.3             | trigrams dayalı metin benzerlik ölçümü ve dizin arama|
> |[plpgsql](https://www.postgresql.org/docs/9.6/plpgsql.html)                      | 1.0             | PL/pgSQL yordam dili|
> |[plv8](https://plv8.github.io/)                         | 2.1.0          | PL/JavaScript (v8) güvenilir yordam dili|
> |[postgis](https://www.postgis.net/)                      | 2.3.2           | PostGIS geometri, coğrafya ve raster mekansal türleri ve fonksiyonları|
> |[postgis_sfcgal](https://www.postgis.net/)               | 2.3.2           | PostGIS SFCGAL fonksiyonları|
> |[postgis_tiger_geocoder](https://www.postgis.net/)       | 2.3.2           | PostGIS kaplan geocoder ve ters geocoder|
> |[postgis_topology](https://postgis.net/docs/Topology.html)             | 2.3.2           | PostGIS topoloji mekansal türleri ve fonksiyonları|
> |[postgres_fdw](https://www.postgresql.org/docs/9.6/postgres-fdw.html)                 | 1.0             | uzak PostgreSQL sunucuları için yabancı veri sarıcı|
> |[tablefunc](https://www.postgresql.org/docs/9.6/tablefunc.html)                    | 1.0             | çapraz sekme de dahil olmak üzere tüm tabloları manipüle eden işlevler|
> |[zaman ölçeğidb](https://docs.timescale.com/latest)                    | 1.1.1             | Zaman serisi verileri için ölçeklenebilir ekler ve karmaşık sorgular sağlar|
> |[aksansız](https://www.postgresql.org/docs/9.6/unaccent.html)                     | 1.1             | vurguları kaldıran metin arama sözlüğü|
> |[uuid-ossp](https://www.postgresql.org/docs/9.6/uuid-ossp.html)                    | 1.1             | evrensel olarak benzersiz tanımlayıcılar (UUIDs) oluşturmak|

## <a name="postgres-95-extensions"></a>Postgres 9.5 uzantıları 

Aşağıdaki uzantılar, Postgres sürümü 9.5 olan PostgreSQL sunucuları için Azure Veritabanı'nda kullanılabilir.

> [!div class="mx-tableFixed"]
> | **Dahili numara**| **Uzantı sürümü** | **Açıklama** |
> |---|---|---|
> |[address_standardizer](http://postgis.net/docs/Address_Standardizer.html)         | 2.3.0           | Bir adresi kurucu öğelere ayrışdırmak için kullanılır. |
> |[address_standardizer_data_us](http://postgis.net/docs/Address_Standardizer.html) | 2.3.0           | Adres Standardizer ABD veri seti örneği|
> |[btree_gin](https://www.postgresql.org/docs/9.5/btree-gin.html)                    | 1.0             | GIN'de ortak veri türlerini dizine alma desteği|
> |[btree_gist](https://www.postgresql.org/docs/9.5/btree-gist.html)                   | 1.1             | GiST'de ortak veri türlerini dizine alma desteği|
> |[chkpass](https://www.postgresql.org/docs/9.5/chkpass.html)                       | 1.0             | otomatik şifrelenmiş parolalar için veri türü|
> |[citext](https://www.postgresql.org/docs/9.5/citext.html)                       | 1.1             | büyük/küçük harf duyarsız karakter dizeleri için veri türü|
> |[Küp](https://www.postgresql.org/docs/9.5/cube.html)                         | 1.0             | çok boyutlu küpler için veri türü|
> |[Dblink](https://www.postgresql.org/docs/9.5/dblink.html)                       | 1.1             | bir veritabanı içinden diğer PostgreSQL veritabanlarına bağlanmak|
> |[dict_int](https://www.postgresql.org/docs/9.5/dict-int.html)                     | 1.0             | sondalar için metin arama sözlüğü şablonu|
> |[toprak mesafesi](https://www.postgresql.org/docs/9.5/earthdistance.html)                | 1.0             | Dünya yüzeyinde büyük daire mesafeleri hesaplamak|
> |[bulanıkça](https://www.postgresql.org/docs/9.5/fuzzystrmatch.html)                | 1.0             | dizeleri arasındaki benzerlikleri ve mesafeyi belirlemek|
> |[hstore](https://www.postgresql.org/docs/9.5/hstore.html)                       | 1.3             | (anahtar, değer) çiftleri kümeleri depolamak için veri türü|
> |[hipopg](https://hypopg.readthedocs.io/en/latest/)                       | 1.1.1           | PostgreSQL için varsayımsal dizinler|
> |[intarray](https://www.postgresql.org/docs/9.5/intarray.html)                     | 1.0             | 1-B dizileri için fonksiyonlar, işleçler ve dizin desteği|
> |[ısn](https://www.postgresql.org/docs/9.5/isn.html)                          | 1.0             | uluslararası ürün numaralandırma standartları için veri türleri|
> |[ltree](https://www.postgresql.org/docs/9.5/ltree.html)                        | 1.0             | hiyerarşik ağaç benzeri yapılar için veri türü|
> |[orafce](https://github.com/orafce/orafce)                       | 3.7             | Ticari RDBMS'den fonksiyonlar ve paketlerin bir alt kümesini taklit eden işlevler ve işleçler|
> |[pgaudit](https://www.pgaudit.org/)                     | 1.0.7             | denetim işlevselliği sağlar|
> |[pgcrypto](https://www.postgresql.org/docs/9.5/pgcrypto.html)                     | 1.2             | şifreleme fonksiyonları|
> |[pgrouting](https://pgrouting.org/)                    | 2.3.0           | pgRouting Uzantısı|
> |[pgrowlocks](https://www.postgresql.org/docs/9.5/pgrowlocks.html)                   | 1.1             | satır düzeyinde kilitleme bilgilerini gösterme|
> |[pgstattuple](https://www.postgresql.org/docs/9.5/pgstattuple.html)                  | 1.3             | tuple düzeyindeki istatistikleri gösterme|
> |[pg_buffercache](https://www.postgresql.org/docs/9.5/pgbuffercache.html)               | 1.1             | paylaşılan arabellek önbelleğini inceleme|
> |[pg_partman](https://github.com/pgpartman/pg_partman)                   | 2.6.3           | Bölümlenmiş tabloları zamana veya kimlikle yönetme uzantısı|
> |[pg_prewarm](https://www.postgresql.org/docs/9.5/pgprewarm.html)                   | 1.0             | prewarm ilişki verileri|
> |[pg_stat_statements](https://www.postgresql.org/docs/9.5/pgstatstatements.html)           | 1.3             | yürütülen tüm SQL deyimlerinin izleme yürütme istatistikleri|
> |[pg_trgm](https://www.postgresql.org/docs/9.5/pgtrgm.html)                      | 1.1             | trigrams dayalı metin benzerlik ölçümü ve dizin arama|
> |[plpgsql](https://www.postgresql.org/docs/9.5/plpgsql.html)                      | 1.0             | PL/pgSQL yordam dili|
> |[postgis](https://www.postgis.net/)                      | 2.3.0           | PostGIS geometri, coğrafya ve raster mekansal türleri ve fonksiyonları|
> |[postgis_sfcgal](https://www.postgis.net/)               | 2.3.0           | PostGIS SFCGAL fonksiyonları|
> |[postgis_tiger_geocoder](https://www.postgis.net/)       | 2.3.0           | PostGIS kaplan geocoder ve ters geocoder|
> |[postgis_topology](https://postgis.net/docs/Topology.html)             | 2.3.0           | PostGIS topoloji mekansal türleri ve fonksiyonları|
> |[postgres_fdw](https://www.postgresql.org/docs/9.5/postgres-fdw.html)                 | 1.0             | uzak PostgreSQL sunucuları için yabancı veri sarıcı|
> |[tablefunc](https://www.postgresql.org/docs/9.5/tablefunc.html)                    | 1.0             | çapraz sekme de dahil olmak üzere tüm tabloları manipüle eden işlevler|
> |[aksansız](https://www.postgresql.org/docs/9.5/unaccent.html)                     | 1.0             | vurguları kaldıran metin arama sözlüğü|
> |[uuid-ossp](https://www.postgresql.org/docs/9.5/uuid-ossp.html)                    | 1.0             | evrensel olarak benzersiz tanımlayıcılar (UUIDs) oluşturmak|


## <a name="pg_stat_statements"></a>pg_stat_statements
[pg_stat_statements uzantısı,](https://www.postgresql.org/docs/current/pgstatstatements.html) SQL deyimlerinin yürütme istatistiklerini izleme aracı sağlamak için PostgreSQL sunucusu için her Azure Veritabanına önceden yüklenir.
Uzantı `pg_stat_statements.track`tarafından hangi deyimlerin sayıldığını kontrol eden `top`ayar, varsayılan olarak, istemciler tarafından doğrudan verilen tüm deyimlerizizilir. Diğer iki izleme `none` düzeyi `all`ve. Bu ayar, [Azure portalı](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-portal) veya [Azure CLI](https://docs.microsoft.com/azure/postgresql/howto-configure-server-parameters-using-cli)üzerinden sunucu parametresi olarak yapılandırılabilir.

pg_stat_statements sağlayan sorgu yürütme bilgileri ile her SQL deyimini kaydederken sunucu performansı üzerindeki etkisi arasında bir denge vardır. pg_stat_statements uzantısını etkin olarak kullanmıyorsanız, 'ye `pg_stat_statements.track` `none`ayarlamanızı öneririz. Bazı üçüncü taraf izleme hizmetlerinin sorgu performansı öngörüleri sunmak için pg_stat_statements güvenebileceğini unutmayın, bu nedenle bunun sizin için geçerli olup olmadığını doğrulayın.

## <a name="dblink-and-postgres_fdw"></a>dblink ve postgres_fdw
[dblink](https://www.postgresql.org/docs/current/contrib-dblink-function.html) ve [postgres_fdw](https://www.postgresql.org/docs/current/postgres-fdw.html) bir PostgreSQL sunucusundan diğerine veya aynı sunucudaki başka bir veritabanına bağlanmanızı sağlar. Alıcı sunucunun, güvenlik duvarı üzerinden gönderen sunucudan bağlantılara izin sağlaması gerekir. Bu uzantıları PostgreSQL sunucuları için Azure Veritabanı arasında bağlanmak için kullanırken, bu işlem "Azure hizmetlerine erişime izin ver" ayarlayarak ON'a yapılabilir. Uzantıları aynı sunucuya geri dönmek için kullanmak istiyorsanız, bu da gereklidir. "Azure hizmetlerine erişime izin ver" ayarı, Bağlantı Güvenliği altında Postgres sunucusunun Azure portal sayfasında bulunabilir. "Azure hizmetlerine erişime izin ver" ON'u açmak, tüm Azure IP'lerini izin listesine koyar.

Şu anda, PostgreSQL için Azure Veritabanı'ndan giden bağlantılar, PostgreSQL sunucuları için diğer Azure Veritabanı bağlantıları dışında desteklenmez.

## <a name="uuid"></a>uuid
[Uuid-ossp uzantısı](https://www.postgresql.org/docs/current/uuid-ossp.html)kullanmayı `uuid_generate_v4()` planlıyorsanız, performans yararları için `gen_random_uuid()` [pgcrypto uzantısı](https://www.postgresql.org/docs/current/pgcrypto.html) ile karşılaştırarak düşünün.

## <a name="pgaudit"></a>pgAudit
[pgAudit uzantısı](https://github.com/pgaudit/pgaudit/blob/master/README.md) oturum ve nesne denetim günlüğü sağlar. PostgreSQL için Azure Veritabanı'nda bu uzantıyı nasıl kullanacağınızı öğrenmek için denetim kavramları makalesini ziyaret [edin.](concepts-audit.md) 

## <a name="pg_prewarm"></a>pg_prewarm
pg_prewarm uzantı, ilişkisel verileri önbelleğe yükler. Önbelleklerinizi önceden ısıtmak, sorgularınızın yeniden başlatmadan sonra ilk çalıştırmalarında daha iyi yanıt sürelerine sahip olduğu anlamına gelir. Postgres 10 ve altında, prewarming [prewarm fonksiyonu](https://www.postgresql.org/docs/10/pgprewarm.html)kullanılarak el ile yapılır.

Postgres 11 ve üzeri, önceden ısınmayı [otomatik olarak](https://www.postgresql.org/docs/current/pgprewarm.html)gerçekleşmesi için yapılandırabilirsiniz. Pg_prewarm parametrenizin `shared_preload_libraries` listesine eklemeniz ve değişikliği uygulamak için sunucuyu yeniden başlatmanız gerekir. Parametreler [Azure portalından,](howto-configure-server-parameters-using-portal.md) [CLI,](howto-configure-server-parameters-using-cli.md)REST API'den veya ARM şablonundan ayarlanabilir. 

## <a name="timescaledb"></a>Zaman ölçeğiDB
TimescaleDB, PostgreSQL uzantısı olarak paketlenmiş bir zaman serisi veritabanıdır. TimescaleDB zaman odaklı analitik işlevler, optimizasyonlar ve zaman serisi iş yükleri için Postgres ölçekler sağlar.

[Timescale, Inc.'in](https://www.timescale.com/)tescilli ticari markası [TimescaleDB hakkında daha fazla bilgi edinin.](https://docs.timescale.com/latest) PostgreSQL için Azure Veritabanı, Timescale'in açık kaynak sürümünü sağlar. Bu sürümde hangi Timescale özelliklerinin kullanılabildiği hakkında bilgi edinmek için [Timescale ürün karşılaştırmasına](https://www.timescale.com/products/)bakın.

### <a name="installing-timescaledb"></a>Zaman ÖlçeğiDB Kurulumu
TimescaleDB'yi yüklemek için sunucunun paylaşılan ön yükleme kitaplıklarına eklemeniz gerekir. Postgres'in `shared_preload_libraries` parametresindeki değişiklik, bir **sunucunun yeniden başlatılmasını** gerektirir. [Azure portalını](howto-configure-server-parameters-using-portal.md) veya [Azure CLI'yi](howto-configure-server-parameters-using-cli.md)kullanarak parametreleri değiştirebilirsiniz.

Azure [portalını](https://portal.azure.com/)kullanma:

1. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

2. Kenar çubuğunda **Sunucu Parametreleri'ni**seçin.

3. Parametreyi `shared_preload_libraries` arayın.

4. **TimescaleDB'yi**seçin.

5. Değişikliklerinizi korumak için **Kaydet'i** seçin. Değişiklik kaydedildikten sonra bir bildirim alırsınız. 

6. Bildirimden sonra, bu değişiklikleri uygulamak için sunucuyu **yeniden başlatın.** Bir sunucunun nasıl yeniden başlatılsüreceğini öğrenmek [için PostgreSQL sunucusu için Azure Veritabanını Yeniden Başlat'a](howto-restart-server-portal.md)bakın.


Artık Postgres veritabanınızda TimescaleDB'yi etkinleştirebilirsiniz. Veritabanına bağlanın ve aşağıdaki komutu sorun:
```sql
CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;
```
> [!TIP]
> Bir hata görürseniz, shared_preload_libraries kaydettikten sonra [sunucunuzu yeniden başlattığınızı](howto-restart-server-portal.md) onaylayın. 

Artık [sıfırdan](https://docs.timescale.com/getting-started/creating-hypertables) bir TimescaleDB hipertablosu oluşturabilir veya [PostgreSQL'de varolan zaman serisi verilerini geçirebilirsiniz.](https://docs.timescale.com/getting-started/migrating-data)

### <a name="restoring-a-timescale-database"></a>Zaman Ölçeği veritabanını geri alma
pg_dump ve pg_restore kullanarak bir Timescale veritabanını geri yüklemek için, hedef `timescaledb_pre_restore()` veritabanında iki yardımcı yordamları çalıştırmanız gerekir: ve `timescaledb_post restore()`.

Önce hedef veritabanını hazırlayın:

```SQL
--create the new database where you'll perform the restore
CREATE DATABASE tutorial;
\c tutorial --connect to the database 
CREATE EXTENSION timescaledb;

SELECT timescaledb_pre_restore();
```

Şimdi orijinal veritabanında pg_dump çalıştırabilirsiniz ve sonra pg_restore. Geri yüklemeden sonra, geri yüklenen veritabanında aşağıdaki komutu çalıştırdığından emin olun:

```SQL
SELECT timescaledb_post_restore();
```


## <a name="next-steps"></a>Sonraki adımlar
Kullanmak istediğiniz bir uzantı görmüyorsanız, bize bildirin. Mevcut isteklere oy verin veya [geri bildirim forumumuzda](https://feedback.azure.com/forums/597976-azure-database-for-postgresql)yeni geri bildirim istekleri oluşturun.
