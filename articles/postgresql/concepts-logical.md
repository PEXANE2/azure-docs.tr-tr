---
title: Mantıksal kod çözme-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı 'nda değişiklik verilerini yakalama için mantıksal kod çözme ve wal2json açıklar-tek sunucu
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: 1213b38f2b67e8fed179cfda4308943808893e1b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80522153"
---
# <a name="logical-decoding"></a>Mantıksal kod çözme
 
[PostgreSQL Içindeki mantıksal kod çözme](https://www.postgresql.org/docs/current/logicaldecoding.html) , dış tüketicilerle veri değişiklikleri akışına olanak sağlar. Mantıksal kod çözme, olay akışı ve değişiklik verilerini yakalama senaryoları için popudöngüsel olarak kullanılır.

Mantıksal kod çözme, Postgres 'nin yazma sonrası günlüğünü (WAL) okunabilir bir biçime dönüştürmek için bir çıktı eklentisi kullanır. PostgreSQL için Azure veritabanı iki çıkış eklentileri sağlar: [test_decoding](https://www.postgresql.org/docs/current/test-decoding.html) ve [wal2json](https://github.com/eulerto/wal2json).
 

> [!NOTE]
> Mantıksal kod çözme, PostgreSQL için Azure veritabanı-tek sunucu genel önizlemede.


## <a name="set-up-your-server"></a>Sunucunuzu ayarlama
Mantıksal kod çözme 'yı kullanmaya başlamak için sunucunuzun WAL 'yi kaydetmesine ve akışına izin vermek için etkinleştirin. 

1. Azure CLı 'yi kullanmak için `logical` azure. replication_support ayarlayın. 
   ```
   az postgres server configuration set --resource-group mygroup --server-name myserver --name azure.replication_support --value logical
   ```

   > [!NOTE]
   > Okuma çoğaltmaları kullanıyorsanız, Azure. replication_support `logical` kopyaların çalışmasına izin verir. Mantıksal kod çözme kullanmayı durdurursanız, ayarı olarak `replica`değiştirin. 


2. Değişiklikleri uygulamak için sunucuyu yeniden başlatın.
   ```
   az postgres server restart --resource-group mygroup --name myserver
   ```

## <a name="start-logical-decoding"></a>Mantıksal kod çözmeyi Başlat

Mantıksal kod çözme, akış protokolü veya SQL arabirimi aracılığıyla tüketilebilir. Her iki yöntem de [çoğaltma yuvaları](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS)kullanır. Yuva, tek bir veritabanındaki değişikliklerin akışını temsil eder.

Çoğaltma yuvası kullanılması için Postgres 'nin çoğaltma ayrıcalıkları gerekir. Şu anda, çoğaltma ayrıcalığı yalnızca sunucunun yönetici kullanıcısı için kullanılabilir. 

### <a name="streaming-protocol"></a>Akış Protokolü
Akış protokolünü kullanan değişiklikleri kullanmak genellikle tercih edilir. Kendi tüketicisini/bağlayıcınızı oluşturabilir veya [Debezium](https://debezium.io/)gibi bir araç kullanabilirsiniz. 

[Pg_recvlogical ile akış protokolünü kullanarak bir örnek](https://github.com/eulerto/wal2json#pg_recvlogical)için wal2json belgelerini ziyaret edin.


### <a name="sql-interface"></a>SQL arabirimi
Aşağıdaki örnekte, SQL arabirimini wal2json eklentisi ile kullanıyoruz.
 
1. Yuva oluşturun.
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. SQL komutları verin. Örneğin:
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


## <a name="monitoring-slots"></a>İzleme Yuvaları

Mantıksal kod çözmeyi izlemeniz gerekir. Kullanılmayan herhangi bir çoğaltma yuvası bırakılmalıdır. Yuvalar, değişiklikler bir tüketici tarafından okunana kadar Postgres WAL günlüklerine ve ilgili sistem kataloglarına sahiptir. Tüketicinizin başarısız olması veya düzgün bir şekilde yapılandırılmamışsa, tüketilmemiş Günlükler depolama alanını doldurup dolduracaktır. Ayrıca, tüketilmemiş Günlükler işlem KIMLIĞI wraparound riskini artırır. Her iki durum da sunucunun kullanılamaz hale gelmesine neden olabilir. Bu nedenle, mantıksal çoğaltma yuvalarının sürekli olarak tüketilmesi kritik öneme sahiptir. Mantıksal çoğaltma yuvası artık kullanılmıyorsa, hemen bırakın.

Pg_replication_slots görünümündeki ' etkin ' sütunu, bir yuvaya bağlı bir tüketici olup olmadığını gösterir.
```SQL
SELECT * FROM pg_replication_slots;
```

Değerler, geçmiş normal eşiklerini arttırabilmeniz için, *kullanılan depolamada* [uyarıları](howto-alert-on-metric.md) ve *çoğaltmalar ölçümleri genelinde en fazla gecikme* değerini ayarlayın. 

> [!IMPORTANT]
> Kullanılmayan çoğaltma yuvalarını bırakmalısınız. Bunun başarısız olması sunucunun kullanılamamasına yol açabilir.

## <a name="how-to-drop-a-slot"></a>Yuva bırakma
Bir çoğaltma yuvasını etkin bir şekilde kullanmıyorsanız bırakmalısınız.

SQL kullanılarak adlandırılan `test_slot` bir çoğaltma yuvasını bırakmak için:
```SQL
SELECT pg_drop_replication_slot('test_slot');
```

> [!IMPORTANT]
> Mantıksal kod çözme kullanmayı durdurursanız, Azure. replication_support öğesini veya `replica` `off`olarak değiştirin. Tarafından `logical` tutulan Wal ayrıntıları daha ayrıntılıdır ve mantıksal kod çözme kullanımda olmadığında devre dışı bırakılmalıdır. 

 
## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal kod çözme hakkında daha fazla bilgi edinmek](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html)Için Postgres belgelerini ziyaret edin.
* Mantıksal kod çözme hakkında sorularınız varsa [ekibimize](mailto:AskAzureDBforPostgreSQL@service.microsoft.com) ulaşın.
* [Okuma çoğaltmaları](concepts-read-replicas.md)hakkında daha fazla bilgi edinin.

