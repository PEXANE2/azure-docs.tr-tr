---
title: "Bilinen sorunlar: PostgreSQL için PostgreSQL'den Azure Veritabanı'na çevrimiçi geçişler"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak PostgreSQL'den Azure Veritabanı'na postgreSQL'den Azure Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar ve geçiş sınırlamaları hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom:
- seo-lt-2019
- seo-dt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: 3d1bc627ccb8814ab2dfb61fb0653ef0ac644038
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235260"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-from-postgresql-to-azure-db-for-postgresql"></a>PostgreSQL için PostgreSQL'den Azure DB'ye çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları

PostgreSQL için Azure Veritabanı'ndan Azure Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar ve sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

## <a name="online-migration-configuration"></a>Çevrimiçi geçiş yapılandırması

- Kaynak PostgreSQL sunucusu sürüm 9.4, 9.5, 9.6, 10 veya 11 çalışıyor olmalıdır. Daha fazla bilgi için, [makaleye](../postgresql/concepts-supported-versions.md)bkz.
- Yalnızca aynı veya daha yüksek sürüme geçişler desteklenir. Örneğin, PostgreSQL 9.5'i PostgreSQL 9.6 veya 10 için Azure Veritabanına geçirmek desteklenir, ancak PostgreSQL 11'den PostgreSQL 9.6'ya geçiş desteklenmez.
- **Kaynak PostgreSQL postgresql.conf** dosyasında mantıksal çoğaltmayı etkinleştirmek için aşağıdaki parametreleri ayarlayın:
  - **wal_level** = mantıksal
  - **max_replication_slots** = [geçiş için en az en az veri tabanı sayısı]; dört veritabanlarını geçirmek istiyorsanız, değeri en az 4 olarak ayarlayın.
  - **max_wal_senders** = [aynı anda çalışan veritabanlarının sayısı]; önerilen değer 10'dur
- Kaynak PostgreSQL pg_hba.conf DMS aracısı IP ekle
  1. Azure Veritabanı Geçiş Hizmeti'nin bir örneğini sağlamayı tamamladıktan sonra DMS IP adresini not alın.
  2. IP adresini gösterildiği gibi pg_hba.conf dosyasına ekleyin:

      ```
          host  all     172.16.136.18/10    md5
          host  replication postgres    172.16.136.18/10    md5
      ```

- Kullanıcı, kaynak veritabanını barındıran sunucuda ÇOĞALTMA rolüne sahip olmalıdır.
- Kaynak ve hedef veritabanı şemaları eşleşmelidir.
- PostgreSQL-Single sunucusu için hedef Azure Veritabanı'ndaki şemada yabancı anahtarlar olmamalıdır. Yabancı anahtarları bırakmak için aşağıdaki sorguyu kullanın:

    ```
                                SELECT Queries.tablename
           ,concat('alter table ', Queries.tablename, ' ', STRING_AGG(concat('DROP CONSTRAINT ', Queries.foreignkey), ',')) as DropQuery
                ,concat('alter table ', Queries.tablename, ' ', 
                                                STRING_AGG(concat('ADD CONSTRAINT ', Queries.foreignkey, ' FOREIGN KEY (', column_name, ')', 'REFERENCES ', foreign_table_name, '(', foreign_column_name, ')' ), ',')) as AddQuery
        FROM
        (SELECT
        tc.table_schema, 
        tc.constraint_name as foreignkey, 
        tc.table_name as tableName, 
        kcu.column_name, 
        ccu.table_schema AS foreign_table_schema,
        ccu.table_name AS foreign_table_name,
        ccu.column_name AS foreign_column_name 
    FROM 
        information_schema.table_constraints AS tc 
        JOIN information_schema.key_column_usage AS kcu
          ON tc.constraint_name = kcu.constraint_name
          AND tc.table_schema = kcu.table_schema
        JOIN information_schema.constraint_column_usage AS ccu
          ON ccu.constraint_name = tc.constraint_name
          AND ccu.table_schema = tc.table_schema
    WHERE constraint_type = 'FOREIGN KEY') Queries
      GROUP BY Queries.tablename;
    
    ```

    Sorgu sonucunda bırakma yabancı anahtarını (ikinci sütun) çalıştırın.

- PostgreSQL-Single sunucusu için hedef Azure Veritabanı'ndaki şemada tetikleyici olmamalıdır. Hedef veritabanındaki tetikleyicileri devre dışı kardırmak için şeyi kullan

     ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
     ```

## <a name="datatype-limitations"></a>Datatype sınırlamaları

  **Sınırlama**: Tablolarda birincil anahtar yoksa, değişiklikler hedef veritabanına eşitlenmeyebilir.

  **Geçici Çözüm**: Geçiş tablosunun devam etmesi için geçici olarak birincil anahtarı ayarlayın. Veri geçişi tamamlandıktan sonra birincil anahtarı kaldırabilirsiniz.

## <a name="limitations-when-migrating-online-from-aws-rds-postgresql"></a>AWS RDS PostgreSQL'den çevrimiçi geçiş yaparken sınırlamalar

AWS RDS PostgreSQL'den PostgreSQL için Azure Veritabanı'na çevrimiçi geçiş yapmaya çalıştığınızda aşağıdaki hatalarla karşılaşabilirsiniz.

- **Hata**: '{database}' veritabanındaki '{table}' tablosundaki '{column}' sütununun varsayılan değeri kaynak ve hedef sunucularda farklıdır. Değer kaynakta '{value on source}', hedefte ise '{value on target}'.

  **Sınırlama**: Bu hata, bir sütun şemasındaki varsayılan değer kaynak ve hedef veritabanları arasında farklı olduğunda oluşur.
  **Geçici Çözüm**: Hedefteki şemanın kaynaktaki şemayla eşleştiğinden emin olun. Geçiş şemasıyla ilgili ayrıntılar için [Azure PostgreSQL çevrimiçi geçiş belgelerine](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema)bakın.

- **Hata**: Hedef veritabanı '{database}' '{tablo sayısı}' tablolarına sahipken, kaynak veritabanı '{database}' '{tables}' tabloları vardır. Kaynak ve hedef veritabanlarındaki tablo sayısı aynı olmalıdır.

  **Sınırlama**: Tablo sayısı kaynak ve hedef veritabanları arasında farklı olduğunda bu hata oluşur.

  **Geçici Çözüm**: Hedefteki şemanın kaynaktaki şemayla eşleştiğinden emin olun. Geçiş şemasıyla ilgili ayrıntılar için [Azure PostgreSQL çevrimiçi geçiş belgelerine](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema)bakın.

- **Hata:** Kaynak veritabanı {database} boş.

  **Sınırlama**: Kaynak veritabanı boş olduğunda bu hata oluşur. Kaynak olarak yanlış veritabanını seçtiğinizden büyük olasılıkla.

  **Geçici Çözüm**: Geçiş için seçtiğiniz kaynak veritabanını iki kez denetleyin ve sonra yeniden deneyin.

- **Hata:** Hedef veritabanı {database} boş. Lütfen şemayı geçirin.

  **Sınırlama**: Bu hata, hedef veritabanında şema olmadığında oluşur. Hedefteki şemanın kaynaktaki şemayla eşleştiğinden emin ol.
  **Geçici Çözüm**: Hedefteki şemanın kaynaktaki şemayla eşleştiğinden emin olun. Geçiş şemasıyla ilgili ayrıntılar için [Azure PostgreSQL çevrimiçi geçiş belgelerine](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online#migrate-the-sample-schema)bakın.

## <a name="other-limitations"></a>Diğer sınırlamalar

- Veritabanı adı bir yarı-iki nokta üst üste (;) içeremez.
- Yakalanan bir tablonun Birincil Anahtarı olmalıdır. Bir tabloda birincil anahtar yoksa, DELETE ve UPDATE kayıt işlemlerinin sonucu tahmin edilemez olacaktır.
- Birincil Anahtar kesimini güncelleştirmegözardı edilir. Bu gibi durumlarda, bu tür bir güncelleştirmenin uygulanması, hedef tarafından herhangi bir satırı güncelleştirmeyen bir güncelleştirme olarak tanımlanır ve özel durumlar tablosuna yazılmış bir kayıtla sonuçlanır.
- Aynı ada sahip ancak farklı bir büyük/küçük harfle (örneğin tablo1, TABLO1 ve Tablo1) birden fazla tablonun geçişi öngörülemeyen davranışlara neden olabilir ve bu nedenle desteklenmez.
- [CREATE | ALTER | DAMLA | TRUNCATE] tablosu DDL'leri desteklenmez.
- Azure Veritabanı Geçiş Hizmeti'nde, tek bir geçiş etkinliği yalnızca dört veritabanına kadar barındırabilir.
