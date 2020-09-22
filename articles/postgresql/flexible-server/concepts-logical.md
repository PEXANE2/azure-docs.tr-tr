---
title: Mantıksal çoğaltma ve mantıksal kod çözme-PostgreSQL için Azure veritabanı-esnek sunucu
description: PostgreSQL için Azure veritabanı 'nda mantıksal çoğaltma ve mantıksal kod çözme kullanma hakkında bilgi edinin-esnek sunucu
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: fd0826ad11a153d72ee47f35930d25f0df498418
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90942130"
---
# <a name="logical-replication-and-logical-decoding-in-azure-database-for-postgresql---flexible-server"></a>PostgreSQL için Azure veritabanı 'nda mantıksal çoğaltma ve mantıksal kod çözme-esnek sunucu

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

PostgreSQL 'in mantıksal çoğaltma ve mantıksal kod çözme özellikleri, PostgreSQL için Azure veritabanı-esnek sunucu 'da desteklenir.

## <a name="comparing-logical-replication-and-logical-decoding"></a>Mantıksal çoğaltmayı ve mantıksal kod çözmeyi karşılaştırma
Mantıksal çoğaltma ve mantıksal kod çözme çeşitli benzerlikler vardır. İkisi de
* verilerin Postgres 'den çoğaltılmasını sağlar
* değişiklik kaynağı olarak [yazma günlüğü 'nü (WAL)](https://www.postgresql.org/docs/current/wal.html) kullanın
* veri göndermek için [mantıksal çoğaltma yuvaları](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS) kullanın. Yuva bir değişiklik akışını temsil eder.
* hangi değişikliklerin gönderilebileceklerini belirlemek için bir tablonun [çoğaltma kimliği özelliğini](https://www.postgresql.org/docs/current/sql-altertable.html#SQL-CREATETABLE-REPLICA-IDENTITY) kullanın
* DDL değişikliklerini çoğaltma


İki teknolojinin farklılıkları vardır: mantıksal çoğaltma 
* çoğaltılacak tabloları veya tablo kümesini belirtmenizi sağlar
* PostgreSQL örnekleri arasında veri çoğaltır

Mantıksal kod çözme 
* veritabanındaki tüm tablolardaki değişiklikleri ayıklar 
* PostgreSQL örnekleri arasında doğrudan veri gönderilemez


## <a name="pre-requisites-for-logical-replication-and-logical-decoding"></a>Mantıksal çoğaltma ve mantıksal kod çözme için önkoşulların önkoşulları

1. Sunucu parametresini olarak ayarlayın `wal_level` `logical` .
2. Değişikliği uygulamak için sunucuyu yeniden başlatın `wal_level` .
3. PostgreSQL örneğinizin, bağlanan kaynağınızın ağ trafiğine izin verdiğini doğrulayın.
4. Çoğaltma komutlarını yürütürken yönetici kullanıcıyı kullanın.

## <a name="using-logical-replication-and-logical-decoding"></a>Mantıksal çoğaltma ve mantıksal kod çözme kullanma

### <a name="logical-replication"></a>Mantıksal çoğaltma
Mantıksal çoğaltma ' Yayımcı ' ve ' abone ' koşullarını kullanır. 
* Yayımcı **, verileri gönderdiğiniz**PostgreSQL veritabanıdır. 
* Abone **, veri göndermekte**olduğunuz PostgreSQL veritabanıdır.

Aşağıda, mantıksal çoğaltmayı denemek için kullanabileceğiniz bazı örnek kodlar verilmiştir.

1. Yayımcıya bağlanın. Tablo oluşturun ve veri ekleyin.
   ```SQL
   CREATE TABLE basic(id SERIAL, name varchar(40));
   INSERT INTO basic(name) VALUES ('apple');
   INSERT INTO basic(name) VALUES ('banana');
   ```

2. Tablo için bir yayın oluşturun.
   ```SQL
   CREATE PUBLICATION pub FOR TABLE basic;
   ```

3. Abone 'e bağlanın. Yayımcıyla aynı şemaya sahip bir tablo oluşturun.
   ```SQL
   CREATE TABLE basic(id SERIAL, name varchar(40));
   ```

4. Daha önce oluşturduğunuz yayına bağlanacak bir abonelik oluşturun.
   ```SQL
   CREATE SUBSCRIPTION sub CONNECTION 'host=<server>.postgres.database.azure.com user=<admin> dbname=<dbname>' PUBLICATION pub;
   ```

5. Artık abone üzerinde tabloyu sorgulayabilirsiniz. Yayımcının veri aldığını görürsünüz.
   ```SQL
   SELECT * FROM basic;
   ```

Yayımcının tablosuna daha fazla satır ekleyebilir ve abonedeki değişiklikleri görüntüleyebilirsiniz.

[Mantıksal çoğaltma](https://www.postgresql.org/docs/current/logical-replication.html)hakkında daha fazla bilgi edinmek Için PostgreSQL belgelerini ziyaret edin.

### <a name="logical-decoding"></a>Mantıksal kod çözme
Mantıksal kod çözme, akış protokolü veya SQL arabirimi aracılığıyla tüketilebilir. 

#### <a name="streaming-protocol"></a>Akış Protokolü
Akış protokolünü kullanan değişiklikleri kullanmak genellikle tercih edilir. Kendi tüketicisini/bağlayıcınızı oluşturabilir veya [Debezium](https://debezium.io/)gibi bir üçüncü taraf hizmeti kullanabilirsiniz. 

[Pg_recvlogical ile akış protokolünü kullanarak bir örnek](https://github.com/eulerto/wal2json#pg_recvlogical)için wal2json belgelerini ziyaret edin.

#### <a name="sql-interface"></a>SQL arabirimi 
Aşağıdaki örnekte, SQL arabirimini wal2json eklentisi ile kullanıyoruz.
 
1. Yuva oluşturun.
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. SQL komutları verin. Örnek:
   ```SQL
   CREATE TABLE a_table (
      id varchar(40) NOT NULL,
      item varchar(40),
      PRIMARY KEY (id)
   );
   
   INSERT INTO a_table (id, item) VALUES ('id1', 'item1');
   DELETE FROM a_table WHERE id='id1';
   ```

3. Değişiklikleri kullanın.
   ```SQL
   SELECT data FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', '1');
   ```

   Çıktı şöyle görünür:
   ```
   {
         "change": [
         ]
   }
   {
         "change": [
                  {
                           "kind": "insert",
                           "schema": "public",
                           "table": "a_table",
                           "columnnames": ["id", "item"],
                           "columntypes": ["character varying(40)", "character varying(40)"],
                           "columnvalues": ["id1", "item1"]
                  }
         ]
   }
   {
         "change": [
                  {
                           "kind": "delete",
                           "schema": "public",
                           "table": "a_table",
                           "oldkeys": {
                                 "keynames": ["id"],
                                 "keytypes": ["character varying(40)"],
                                 "keyvalues": ["id1"]
                           }
                  }
         ]
   }
   ```

4. Onu kullanarak işiniz bittiğinde yuvayı atın.
   ```SQL
   SELECT pg_drop_replication_slot('test_slot'); 
   ```

[Mantıksal kod çözme](https://www.postgresql.org/docs/current/logicaldecoding.html)hakkında daha fazla bilgi edinmek Için PostgreSQL belgelerini ziyaret edin.


## <a name="monitoring"></a>İzleme
Mantıksal kod çözmeyi izlemeniz gerekir. Kullanılmayan herhangi bir çoğaltma yuvası bırakılmalıdır. Yuvalar, değişiklikler okunana kadar Postgres WAL günlüklerine ve ilgili sistem kataloglarına sahiptir. Abone veya tüketici başarısız olursa ya da düzgün şekilde yapılandırılmamışsa, tüketilmemiş Günlükler depolama alanını doldurur ve dolduracaktır. Ayrıca, tüketilmemiş Günlükler işlem KIMLIĞI wraparound riskini artırır. Her iki durum da sunucunun kullanılamaz hale gelmesine neden olabilir. Bu nedenle, mantıksal çoğaltma yuvalarının sürekli olarak tüketilmesi kritik öneme sahiptir. Mantıksal çoğaltma yuvası artık kullanılmıyorsa, hemen bırakın.

Pg_replication_slots görünümündeki ' etkin ' sütunu, bir yuvaya bağlı bir tüketici olup olmadığını gösterir.
```SQL
SELECT * FROM pg_replication_slots;
```

**En fazla kullanılan Işlem kimliği** ve **depolama alanı** üzerinde [Uyarı ayarlama](howto-alert-on-metrics.md) değerleri, değerlerin geçmiş normal eşiklerini artırması durumunda size bildirimde bulunan esnek sunucu ölçümlerini kullanır. 

## <a name="read-replicas"></a>Okuma amaçlı çoğaltmalar
PostgreSQL için Azure veritabanı okuma çoğaltmaları Şu anda esnek sunucular için desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
* [Ağ seçenekleri](concepts-networking.md) hakkında daha fazla bilgi edinin
* Esnek sunucuda kullanılabilen [Uzantılar](concepts-extensions.md) hakkında bilgi edinin
* [Yüksek kullanılabilirlik](concepts-high-availability.md) hakkında daha fazla bilgi edinin

