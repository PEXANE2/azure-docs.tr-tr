---
title: 'Bilinen sorunlar: MySQL için Azure Veritabanına çevrimiçi geçişler'
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanırken MySQL için Azure Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar ve geçiş sınırlamaları hakkında bilgi edinin.
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
ms.openlocfilehash: 8c3de28ea934302086a5b14e61482e6a4ab9a7ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235286"
---
# <a name="online-migration-issues--limitations-to-azure-db-for-mysql-with-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti ile MySQL için Azure DB'ye & sınırlamalar çevrimiçi geçiş sorunları

MySQL için MySQL'den Azure Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar ve sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

## <a name="online-migration-configuration"></a>Çevrimiçi geçiş yapılandırması


- Kaynak MySQL Server sürümü sürüm 5.6.35, 5.7.18 veya daha sonra olmalıdır
- MySQL için Azure Veritabanı destekler:
  - MySQL topluluk sürümü
  - InnoDB motoru
- Aynı sürüm geçişi. MySQL 5.6'nın MySQL 5.7 için Azure Veritabanına geçirilmesi desteklenmez.
- my.ini (Windows) veya my.cnf (Unix) ikili günlük etkinleştirme
  - Server_id daha büyük veya 1'e eşit olarak ayarlayın, örneğin Server_id=1 (yalnızca MySQL 5,6 için)
  - Log-bin = \<yol> ayarlayın (yalnızca MySQL 5.6 için)
  - set binlog_format = satır
  - Expire_logs_days = 5 (önerilir - sadece MySQL 5.6 için)
- Kullanıcı ReplicationAdmin rolüne sahip olmalıdır.
- Kaynak MySQL veritabanı için tanımlanan harmanlamalar, MySQL için hedef Azure Veritabanı'nda tanımlananlarla aynıdır.
- Schema, MySQL için Azure Veritabanı'ndaki kaynak MySQL veritabanı ile hedef veritabanı arasında eşleşmelidir.
- MySQL için hedef Azure Veritabanı'ndaki şemayabancı anahtarlara sahip olmamalıdır. Yabancı anahtarları bırakmak için aşağıdaki sorguyu kullanın:
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
- MySQL için hedef Azure Veritabanı'ndaki şemada tetikleyici olmamalıdır. Hedef veritabanında tetikleyicileri bırakmak için:
    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="datatype-limitations"></a>Datatype sınırlamaları

- **Sınırlama**: Kaynak MySQL veritabanında json veri türü varsa, geçiş sürekli eşitleme sırasında başarısız olur.

    **Geçici Çözüm**: Kaynak MySQL veritabanında orta metin veya uzun metin JSON veri türünü değiştirin.

- **Sınırlama**: Tablolarda birincil anahtar yoksa, sürekli eşitleme başarısız olur.

    **Geçici Çözüm**: Geçiş tablosunun devam etmesi için geçici olarak birincil anahtarı ayarlayın. Veri geçişi tamamlandıktan sonra birincil anahtarı kaldırabilirsiniz.

## <a name="lob-limitations"></a>LOB sınırlamaları

Büyük Nesne (LOB) sütunları boyutu büyük büyüyebilir sütunlar vardır. MySQL için, Orta metin, Longtext, Blob, Mediumblob, Longblob, vb, lob veri tipleri bazılarıdır.

- **Sınırlama**: LOB veri türleri birincil anahtar olarak kullanılırsa, geçiş başarısız olur.

    **Geçici Çözüm**: Birincil anahtarı lob olmayan diğer veri türleri veya sütunlarla değiştirin.

- **Sınırlama**: Büyük Nesne (LOB) sütununun uzunluğu 32 KB'den büyükse, veriler hedefte kesilir. Bu sorguyu kullanarak LOB sütununun uzunluğunu denetleyebilirsiniz:
    ```
    SELECT max(length(description)) as LEN from catalog;
    ```

    **Geçici Çözüm**: 32 KB'den büyük LOB nesneniz varsa, [Azure Veritabanı Geçişlerini Sor'taki](mailto:AskAzureDatabaseMigrations@service.microsoft.com)mühendislik ekibine başvurun.

## <a name="limitations-when-migrating-online-from-aws-rds-mysql"></a>AWS RDS MySQL'den çevrimiçi geçiş yaparken sınırlamalar

AWS RDS MySQL'den MySQL için Azure Veritabanı'na çevrimiçi geçiş gerçekleştirmeye çalıştığınızda aşağıdaki hatalarla karşılaşabilirsiniz.

- **Hata:** Database{0}' ' ' hedefte yabancı anahtar(lar) vardır. Hedefi düzeltin ve yeni bir veri geçişi etkinliği başlatın. Yabancı anahtarı listelemek için hedefteki komut dosyasını uygulayın

  **Sınırlama**: Şemanızda yabancı anahtarlar varsa, geçişin ilk yükü ve sürekli senkronizasyonu başarısız olur.
  **Geçici Çözüm**: Açılan yabancı anahtar komut dosyasını ayıklamak ve yabancı anahtar komut dosyasını eklemek için MySQL çalışma tezgahında aşağıdaki komut dosyasını çalıştırın:

  ```
  SET group_concat_max_len = 8192; SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery FROM (SELECT KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC WHERE KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA AND KCU.REFERENCED_TABLE_SCHEMA = 'SchemaName') Queries GROUP BY SchemaName;
  ```

- **Hata:** Veritabanı{0}' ' ' sunucuda yok. Sağlanan MySQL kaynak sunucusu büyük/küçük harfe duyarlı. Lütfen veritabanı adını denetleyin.

  **Sınırlama**: Komut Satırı Arabirimi (CLI) kullanarak bir MySQL veritabanını Azure'a geçirdiğinizde, kullanıcılar bu hataya neden olabilir. Hizmet kaynak sunucuda veritabanını bulamamış olabilir, çünkü yanlış veritabanı adı vermiş olabilirsiniz veya veritabanı listelenen sunucuda yok. Not veritabanı adları büyük/küçük harf duyarlıdır.

  **Geçici Çözüm**: Tam veritabanı adını sağlayın ve sonra yeniden deneyin.

- **Hata:** '{database}' veritabanında aynı ada sahip tablolar vardır. MySQL için Azure Veritabanı, büyük/küçük harfe duyarlı tabloları desteklemez.

  **Sınırlama**: Kaynak veritabanında aynı ada sahip iki tablonuz olduğunda bu hata olur. MySQL için Azure Veritabanı büyük/küçük harf duyarlı tabloları desteklemez.

  **Geçici Çözüm**: Tablo adlarını benzersiz olacak şekilde güncelleştirin ve sonra yeniden deneyin.

- **Hata:** Hedef veritabanı {database} boş. Lütfen şemayı geçirin.

  **Sınırlama**: Bu hata, MySQL veritabanı için hedef Azure Veritabanı gerekli şema olmadığında oluşur. Hedefe veri geçişini etkinleştirmek için şema geçişi gereklidir.

  **Geçici Çözüm**: [Şemayı](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#migrate-the-sample-schema) kaynak veritabanınızdan hedef veritabanına geçirin.

## <a name="other-limitations"></a>Diğer sınırlamalar

- Parola dizesinin başında ve sonunda { } kıvırcık köşeli ayraçları açılıp kapanan bir parola dizesi desteklenmez. Bu sınırlama hem kaynak MySQL'e bağlanmak hem de MySQL için hedeflenen Azure Veritabanı için geçerlidir.
- Aşağıdaki DDL'ler desteklenmez:
  - Tüm bölüm DDL'leri
  - Açılan tablo
  - Tabloyu yeniden adlandır
- Table_name *sütun eklemek* için sütun <column_name>deyimi eklemek için değişiklik tablosu <> <değiştirilmesi tablosunun kullanılması desteklenmez. THe *alter tablo <table_name> sütun <column_name>* tablonun sonuna sütun ekler.
- Sütun verilerinin yalnızca bir bölümünde oluşturulan dizinler desteklenmez. Aşağıdaki deyim, sütun verilerinin yalnızca bir bölümünü kullanarak bir dizin oluşturan bir örnektir:

    ``` 
    CREATE INDEX partial_name ON customer (name(10));
    ```

- Azure Veritabanı Geçiş Hizmeti'nde, tek bir geçiş etkinliğinde geçiş yapmak için veritabanlarının sınırı dörttür.

- **Hata:** Satır boyutu çok büyük (> 8126). Bazı sütunları TEXT veya BLOB olarak değiştirmek yardımcı olabilir. Geçerli satır biçiminde, 0 baytBLOB öneki satır içinde depolanır.

  **Sınırlama**: Bu hata, InnoDB depolama altyapısını kullanarak MySQL için Azure Veritabanı'na geçiş yaptığınızda ve herhangi bir tablo satırı boyutu çok büyükolduğunda (>8126 bayt) olur.

  **Geçici Çözüm**: 8126 bayttan büyük bir satır boyutuolan tablonun şeasını güncelleştirin. Veriler kesilir, çünkü sıkı modu değiştirmenizi önermiyoruz. page_size değiştirilmesi desteklenmez.
