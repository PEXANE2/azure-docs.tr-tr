---
title: Mantıksal kod çözme - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL için Azure Veritabanı'nda veri yakalamayı değiştirmek için mantıksal kod çözme ve wal2json'u açıklar - Tek Sunucu
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/31/2020
ms.openlocfilehash: 1213b38f2b67e8fed179cfda4308943808893e1b
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522153"
---
# <a name="logical-decoding"></a>Mantıksal kod çözme
 
[PostgreSQL'de mantıksal kod çözme,](https://www.postgresql.org/docs/current/logicaldecoding.html) veri değişikliklerini harici tüketicilere aktaranıza olanak tanır. Mantıksal kod çözme, genel olarak olay akışı ve veri yakalama senaryolarını değiştirmek için kullanılır.

Mantıksal kod çözme, Postgres'in yazma giriş günlüğünü (WAL) okunabilir bir biçime dönüştürmek için bir çıktı eklentisi kullanır. PostgreSQL için Azure Veritabanı iki çıkış eklentisi sağlar: [test_decoding](https://www.postgresql.org/docs/current/test-decoding.html) ve [wal2json.](https://github.com/eulerto/wal2json)
 

> [!NOTE]
> Mantıksal kod çözme, PostgreSQL - Single Server için Azure Veritabanı'nda genel önizlemede dir.


## <a name="set-up-your-server"></a>Sunucunuzu ayarlama
Mantıksal kod çözme kullanmaya başlamak için, sunucunuzun WAL'ı kaydetmesini ve akış lamasını etkinleştirin. 

1. Azure.replication_support'yi `logical` Azure CLI'yi kullanmaya ayarlayın. 
   ```
   az postgres server configuration set --resource-group mygroup --server-name myserver --name azure.replication_support --value logical
   ```

   > [!NOTE]
   > Okuma yinelemeleri kullanıyorsanız, azure.replication_support `logical` yinelemelerin çalışmasına da izin vermek üzere ayarlanmış. Mantıksal kod çözmeyi durdurursanız, ayarı `replica`'ye geri değiştirin. 


2. Değişiklikleri uygulamak için sunucuyu yeniden başlatın.
   ```
   az postgres server restart --resource-group mygroup --name myserver
   ```

## <a name="start-logical-decoding"></a>Mantıksal çözmeyi başlatma

Mantıksal kod çözme akış protokolü veya SQL arabirimi üzerinden tüketilebilir. Her iki yöntem de [çoğaltma yuvaları](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS)kullanın. Yuva, tek bir veritabanından gelen bir değişiklik akışını temsil eder.

Çoğaltma yuvası kullanmak Postgres'in çoğaltma ayrıcalıklarını gerektirir. Şu anda, çoğaltma ayrıcalığı yalnızca sunucunun yönetici kullanıcısı için kullanılabilir. 

### <a name="streaming-protocol"></a>Akış protokolü
Akış protokolünü kullanarak değiştirme ler genellikle tercih edilir. Kendi tüketici / konektör oluşturabilir veya [Debezium](https://debezium.io/)gibi bir araç kullanabilirsiniz. 

pg_recvlogical ile akış protokolünü kullanarak bir örnek için wal2json belgelerini ziyaret [edin.](https://github.com/eulerto/wal2json#pg_recvlogical)


### <a name="sql-interface"></a>SQL arabirimi
Aşağıdaki örnekte, wal2json eklentisi ile SQL arabirimini kullanıyoruz.
 
1. Bir yuva oluşturun.
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. SQL komutlarını sorun. Örnek:
   ```SQL
   CREATE TABLE a_table (
      id varchar(40) NOT NULL,
      item varchar(40),
      PRIMARY KEY (id)
   );
   
   INSERT INTO a_table (id, item) VALUES ('id1', 'item1');
   DELETE FROM a_table WHERE id='id1';
   ```

3. Değişiklikleri tüketin.
   ```SQL
   SELECT data FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', '1');
   ```

   Çıktı aşağıdaki gibi görünecektir:
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

4. Kullanımı bittikten sonra yuvayı bırakın.
   ```SQL
   SELECT pg_drop_replication_slot('test_slot'); 
   ```


## <a name="monitoring-slots"></a>İzleme yuvaları

Mantıksal kod çözmeyi izlemeniz gerekir. Kullanılmayan çoğaltma yuvası bırakılmalıdır. Yuvalar, değişiklikler bir tüketici tarafından okunana kadar Postgres WAL günlüklerini ve ilgili sistem kataloglarını saklar. Tüketiciniz başarısız olursa veya düzgün bir şekilde yapılandırılmamışsa, tüketilmemiş günlükler yığılır ve deponuzu doldurur. Ayrıca, tüketilmemiş günlükler işlem kimliği kaydırma riskini artırır. Her iki durum da sunucunun kullanılamamasına neden olabilir. Bu nedenle, mantıksal çoğaltma yuvaları sürekli tüketilen önemlidir. Mantıksal çoğaltma yuvası artık kullanılmazsa, hemen düşün.

pg_replication_slots görünümündeki 'etkin' sütun, yuvaya bağlı bir tüketici olup olmadığını gösterir.
```SQL
SELECT * FROM pg_replication_slots;
```

*Kullanılan Depolama'da* [uyarıları ayarlayın](howto-alert-on-metric.md) ve değerler normal eşikleri geçtiğinde sizi bilgilendirmek için yineleme ölçümleri arasında *Max gecikmesi.* 

> [!IMPORTANT]
> Kullanılmayan çoğaltma yuvalarını bırakmalısınız. Aksi takdirde sunucu nun kullanılamamasına neden olabilir.

## <a name="how-to-drop-a-slot"></a>Nasıl bir yuva bırak
Bir çoğaltma yuvasını etkin olarak tüketmiyorsanız, bu yuvayı bırakmanız gerekir.

SQL kullanılarak çağrılan `test_slot` bir çoğaltma yuvasını düşürmek için:
```SQL
SELECT pg_drop_replication_slot('test_slot');
```

> [!IMPORTANT]
> Mantıksal kod çözmeyi durdurursanız, azure.replication_support'ı değiştirin `replica` veya `off`. Tarafından `logical` tutulan WAL ayrıntıları daha ayrıntılıdır ve mantıksal kod çözme kullanılmadığında devre dışı bırakılmalı. 

 
## <a name="next-steps"></a>Sonraki adımlar

* Mantıksal kod çözme hakkında daha fazla bilgi edinmek için Postgres belgelerini ziyaret [edin.](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html)
* Mantıksal kod çözme ile ilgili sorularınız varsa [ekibimize](mailto:AskAzureDBforPostgreSQL@service.microsoft.com) ulaşın.
* [Okuma yinelemeleri](concepts-read-replicas.md)hakkında daha fazla bilgi edinin.

