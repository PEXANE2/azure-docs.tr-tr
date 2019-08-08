---
title: MySQL için Azure veritabanı 'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları hakkında makale | Microsoft Docs
description: MySQL için Azure veritabanı 'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 08/06/2019
ms.openlocfilehash: fc5565ab9e3be21b96ce5aa5a938cf22ec3caeb0
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848494"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-db-for-mysql"></a>MySQL için Azure DB 'ye çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları

MySQL 'in MySQL için Azure veritabanı 'na çevrimiçi geçişlerle ilişkili bilinen sorunlar ve sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

## <a name="online-migration-configuration"></a>Çevrimiçi geçiş yapılandırması

- Kaynak MySQL Server sürümü Version 5.6.35, 5.7.18 veya üzeri olmalıdır
- MySQL için Azure veritabanı şunları destekler:
  - MySQL Community sürümü
  - InnoDB altyapısı
- Aynı sürüm geçişi. MySQL 5,6 ' i MySQL için Azure veritabanı 5,7 ' e geçirme desteklenmez.
- My. ini (Windows) veya My. cnf (Unix) içinde ikili günlüğü etkinleştir
  - Server_id ' i daha büyük veya eşittir 1 olarak ayarlayın, örneğin, Server_id = 1 (yalnızca MySQL 5,6 için)
  - Set log-bin = \<Path > (yalnızca MySQL 5,6 için)
  - Set binlog_format = Row
  - Expire_logs_days = 5 (yalnızca MySQL 5,6 için önerilir)
- Kullanıcının ReplicationAdmin rolü olmalıdır.
- Kaynak MySQL veritabanı için tanımlanan harmanlamalar, MySQL için hedef Azure veritabanı 'nda tanımlandıklardır.
- MySQL için Azure veritabanı 'ndaki kaynak MySQL veritabanı ile hedef veritabanı arasında şemanın eşleşmesi gerekir.
- MySQL için Azure veritabanı hedef şemasının yabancı anahtarları olmamalıdır. Yabancı anahtarları bırakmak için aşağıdaki sorguyu kullanın:
    ```
    SET group_concat_max_len = 8192;
    SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
    FROM
    (SELECT 
    KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = ['schema_name']) Queries
      GROUP BY SchemaName;
    ```

    Sorgu sonucunda bırakma yabancı anahtarını (ikinci sütun) çalıştırın.
- MySQL için Azure veritabanı hedef şemasının hiçbir tetikleyicisi olmamalıdır. Tetikleyicileri hedef veritabanında bırakmak için:
    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="datatype-limitations"></a>Veri türü sınırlamaları

- **Sınırlama**: Kaynak MySQL veritabanında bir JSON veri türü varsa, sürekli eşitleme sırasında geçiş başarısız olur.

    **Geçici çözüm**: JSON veri türünü, kaynak MySQL veritabanında orta metin veya LONGTEXT olarak değiştirin.

- **Sınırlama**: Tablolarda birincil anahtar yoksa sürekli eşitleme başarısız olur.

    **Geçici çözüm**: Geçiş işleminin devam etmesi için geçici olarak tablonun birincil anahtarını ayarlayın. Veri geçişi tamamlandıktan sonra birincil anahtarı kaldırabilirsiniz.

## <a name="lob-limitations"></a>LOB sınırlamaları

Büyük nesne (LOB) sütunları, boyutu büyük büyüyerek kullanılan sütunlardır. MySQL için, orta metin, LONGTEXT, blob, düz blob, LONGBLOB vb., LOB 'un bazı veri türlerine yöneliktir.

- **Sınırlama**: LOB veri türleri birincil anahtar olarak kullanılıyorsa, geçiş başarısız olur.

    **Geçici çözüm**: Birincil anahtarı diğer veri türleri veya LOB olmayan sütunlarla değiştirin.

- **Sınırlama**: Büyük nesne (LOB) sütununun uzunluğu 32 KB 'tan büyükse, veriler hedefte kesilebilir. Bu sorguyu kullanarak LOB sütununun uzunluğunu kontrol edebilirsiniz:
    ```
    SELECT max(length(description)) as LEN from catalog;
    ```

    **Geçici çözüm**: 32 KB 'den büyük LOB nesneniz varsa [Azure veritabanı geçişleri sorun](mailto:AskAzureDatabaseMigrations@service.microsoft.com)' da mühendislik ekibine başvurun. 

## <a name="limitations-when-migrating-online-from-aws-rds-mysql"></a>AWS RDS MySQL 'ten çevrimiçi geçiş yapma sınırlamaları

AWS RDS MySQL 'ten MySQL için Azure veritabanı 'na çevrimiçi geçiş gerçekleştirmeye çalıştığınızda, aşağıdaki hatalarda gelebiliriz.

- **Hata:** '{0}' Veritabanında, hedefte yabancı anahtar (ler) vardır. Hedefi düzeltin ve yeni bir veri geçişi etkinliği başlatın. Yabancı anahtar (ler) i listelemek için aşağıdaki betiği hedefle yürütün

  **Sınırlama**: Şemanızda yabancı anahtarlar varsa geçişe ilişkin ilk yük ve sürekli eşitleme başarısız olur.
  **Geçici çözüm**: DROP ve ADD yabancı anahtar betiklerini ayıklamak için MySQL Workbench'te şu betiği yürütün:

  ```
  SET group_concat_max_len = 8192; SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery FROM (SELECT KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC WHERE KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA AND KCU.REFERENCED_TABLE_SCHEMA = 'SchemaName') Queries GROUP BY SchemaName;
  ```

- **Hata:** '{0}' Veritabanı sunucuda yok. Sağlanan MySQL kaynak sunucusu büyük/küçük harfe duyarlı. Lütfen veritabanı adını denetleyin.

  **Sınırlama**: Kullanıcılar Komut Satırı Arabirimi'ni (CLI) kullanarak MySQL veritabanını Azure'a geçirirken bu hatayla karşılaşabilir. Hizmet, kaynak sunucuda veritabanını bulamadı, bu durum yanlış veritabanı adı sağlamış olabilirsiniz veya listelenen sunucuda veritabanı yok olabilir. Note veritabanı adları büyük/küçük harfe duyarlıdır.

  **Geçici çözüm**: Tam veritabanı adını girip yeniden deneyin.

- **Hata:** ' {Database} ' veritabanında aynı ada sahip tablolar var. MySQL için Azure Veritabanı, büyük/küçük harfe duyarlı tabloları desteklemez.

  **Sınırlama**: Kaynak veritabanında aynı adlı iki tablonuz olduğunda bu hata oluştur. MySQL için Azure veritabanı, büyük/küçük harfe duyarlı tabloları desteklemez.

  **Geçici çözüm**: Tablo adlarını benzersiz olarak güncelleştirip yeniden deneyin.

- **Hata:** Hedef veritabanı {Database} boş. Lütfen şemayı geçirin.

  **Sınırlama**: Bu hata, MySQL için Azure veritabanı veritabanının gerekli şemasına sahip olmadığı durumlarda oluşur. Hedefinizdeki verilerin geçirilmesini sağlamak için şema geçişi gereklidir.

  **Geçici çözüm**: [Şemayı](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#migrate-the-sample-schema) kaynak veritabanından hedef veritabanına geçirin.

## <a name="other-limitations"></a>Diğer sınırlamalar

- Parola dizesinin başındaki ve sonundaki küme ayracı {} açan ve kapatan bir parola dizesi desteklenmiyor. Bu sınırlama, hem kaynak MySQL 'e bağlanmak hem de MySQL için Azure veritabanı 'na yöneliktir.
- Aşağıdaki DDLs 'ler desteklenmez:
  - Tüm bölüm DDLs 'Leri
  - Tabloyu bırak
  - Tabloyu yeniden adlandır
- Tablo eklemek için *alter table < table_name > Add column < column_name >* deyimini kullanarak tablonun başına veya ortasına sütun ekleme desteklenmez. *Alter table < table_name > Sütun ekle < column_name >* tablonun sonundaki sütunu ekler.
- Yalnızca sütun verilerinin bir kısmında oluşturulan dizinler desteklenmez. Aşağıdaki ifade, yalnızca sütun verilerinin bir kısmını kullanarak bir dizin oluşturan bir örnektir:

    ``` 
    CREATE INDEX partial_name ON customer (name(10));
    ```

- DMS 'de, tek bir geçiş etkinliğinde geçirilecek veritabanlarının sınırı dördü.
