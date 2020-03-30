---
title: 'Bilinen sorunlar: SQL Veritabanına çevrimiçi geçişler'
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure SQL Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları hakkında bilgi edinin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: e7efdb7244e2c7e4651a4507b538123f8d320c1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77650784"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-database"></a>Azure SQL Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları

SQL Server'dan Azure SQL Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar ve sınırlamalar aşağıda açıklanmıştır.

> [!IMPORTANT]
> SQL Server'ın Azure SQL Veritabanı'na çevrimiçi geçişleri ile SQL_variant veri türlerinin geçişi desteklenmez.

### <a name="migration-of-temporal-tables-not-supported"></a>Zamansal tabloların geçişi desteklenmiyor

**Belirti**

Kaynak veritabanınız bir veya daha fazla zamansal tablodan oluşuyorsa, veritabanı geçişiniz "Tam veri yükü" işlemi sırasında başarısız olur ve aşağıdaki iletiyi görebilirsiniz:

```
{ "resourceId":"/subscriptions/<subscription id>/resourceGroups/migrateready/providers/Microsoft.DataMigration/services/<DMS Service name>", "errorType":"Database migration error", "errorEvents":"["Capture functionalities could not be set. RetCode: SQL_ERROR SqlState: 42000 NativeError: 13570 Message: [Microsoft][SQL Server Native Client 11.0][SQL Server]The use of replication is not supported with system-versioned temporal table '[Application. Cities]' Line: 1 Column: -1 "]" }
```

 ![Zamansal tablo hataları örneği](media/known-issues-azure-sql-online/dms-temporal-tables-errors.png)

**Geçi -ci çözüm**

Aşağıdaki adımları kullanın:

1. Aşağıdaki sorguyu kullanarak kaynak şemanızdaki zamansal tabloları bulun.

     ``` 
     select name,temporal_type,temporal_type_desc,* from sys.tables where temporal_type <>0
     ```

2. Bu tabloları, geçiş için tabloları belirttiğiniz **Yapıya geçiş ayarları** bıçağından hariç tinler.

3. Geçiş etkinliğini yeniden çalıştırın.

**Kaynaklar**

Daha fazla bilgi için Geçici [Tablolar](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables?view=sql-server-2017)makalesine bakın.

### <a name="migration-of-tables-includes-one-or-more-columns-with-the-hierarchyid-data-type"></a>Tabloların geçişi, hiyerarşik veri türüne sahip bir veya daha fazla sütun içerir

**Belirti**

"Tam veri yükü" işlemi sırasında "ntext hiyerarşisi ile uyumsuzdur" öneren bir SQL Özel Durum görebilirsiniz:

![hiyerarşik hatalar örneği](media/known-issues-azure-sql-online/dms-hierarchyid-errors.png)

**Geçi -ci çözüm**

Aşağıdaki adımları kullanın:

1. Aşağıdaki sorguyu kullanarak hiyerarşik veri türüne sahip sütunları içeren kullanıcı tablolarını bulun.

      ``` 
      select object_name(object_id) 'Table name' from sys.columns where system_type_id =240 and object_id in (select object_id from sys.objects where type='U')
      ```

2. Bu tabloları, geçiş için tabloları belirttiğiniz **Yapıya geçiş ayarları** bıçağından hariç tinler.

3. Geçiş etkinliğini yeniden çalıştırın.

### <a name="migration-failures-with-various-integrity-violations-with-active-triggers-in-the-schema-during-full-data-load-or-incremental-data-sync"></a>"Tam veri yükü" veya "Artımlı veri eşitlemesi" sırasında şemada etkin tetikleyicilerle çeşitli bütünlük ihlalleri nekadar hata

**Geçi -ci çözüm**

Aşağıdaki adımları kullanın:

1. Aşağıdaki sorguyu kullanarak kaynak veritabanında şu anda etkin olan tetikleyicileri bulun:

     ```
     select * from sys.triggers where is_disabled =0
     ```

2. [DISABLE TRIGGER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017)makalesinde sağlanan adımları kullanarak kaynak veritabanınızdaki tetikleyicileri devre dışı bırakın.

3. Geçiş etkinliğini yeniden çalıştırın.

### <a name="support-for-lob-data-types"></a>LOB veri türleri için destek

**Belirti**

Büyük Nesne (LOB) sütununun uzunluğu 32 KB'den büyükse, veriler hedefte kesilir. Aşağıdaki sorguyu kullanarak LOB sütununun uzunluğunu kontrol edebilirsiniz:

``` 
SELECT max(DATALENGTH(ColumnName)) as LEN from TableName
```

**Geçi -ci çözüm**

32 KB'den büyük bir LOB sütununuzun varsa, [Azure Veritabanı Geçişlerini Sor'taki](mailto:AskAzureDatabaseMigrations@service.microsoft.com)mühendislik ekibine başvurun.

### <a name="issues-with-timestamp-columns"></a>Zaman damgası sütunları ile ilgili sorunlar

**Belirti**

Azure Veritabanı Geçiş Hizmeti kaynak zaman damgası değerini geçirmiyor; bunun yerine, Azure Veritabanı Geçiş Hizmeti hedef tabloda yeni bir zaman damgası değeri oluşturur.

**Geçi -ci çözüm**

Kaynak tabloda depolanan tam zaman damgası değerini geçirmek için Azure Veritabanı Geçiş Hizmeti'ne ihtiyacınız varsa, [Azure Veritabanı Geçişlerini Sor'taki](mailto:AskAzureDatabaseMigrations@service.microsoft.com)mühendislik ekibine başvurun.

### <a name="data-migration-errors-dont-provide-additional-details-on-the-database-detailed-status-blade"></a>Veri geçiş hataları Veritabanı ayrıntılı durum bıçak hakkında ek ayrıntılar sağlamaz

**Belirti**

Veritabanları ayrıntıları durum görünümünde geçiş hatalarıyla karşılaştığınızda, üst şeritteki **Veri geçiş hataları** bağlantısını seçmek geçiş hatalarına özgü ek ayrıntılar sağlamayabilir.

![veri geçiş hataları hiçbir ayrıntı örneği](media/known-issues-azure-sql-online/dms-data-migration-errors-no-details.png)

**Geçi -ci çözüm**

Belirli hata ayrıntılarına ulaşmak için aşağıdaki adımları kullanın.

1. Geçiş etkinlik ekranını görüntülemek için Veritabanı ayrıntılı durum bıçakını kapatın.

     ![geçiş etkinlik ekranı](media/known-issues-azure-sql-online/dms-migration-activity-screen.png)

2. Geçiş hatalarını gidermenize yardımcı olan belirli hata iletilerini görüntülemek için **hata ayrıntılarını** gör'i seçin.

### <a name="geography-datatype-not-supported-in-sqldb-online-migration"></a>SQLDB çevrimiçi geçişinde desteklenmeyen coğrafya veri türü

**Belirti**

Geçiş, aşağıdaki metni içeren bir hata iletisiyle başarısız olur:

     “** encountered a fatal error”, "errorEvents":<Table>.<Column> is of type 'GEOGRAPHY', which is not supported by 'Full Load' under 'Full LOB' support mode."

**Geçi -ci çözüm**

Azure Veritabanı Geçiş Hizmeti, çevrimiçi geçişler için Azure SQL Veritabanı'na çevrimdışı geçişler için Coğrafya veri türünü desteklerken, Coğrafya veri türü desteklenmez. Bu veritabanının çevrimiçi geçişi için Azure Veritabanı Geçiş Hizmeti'ni kullanmaya çalışmadan önce kaynaktaki veri türünü desteklenen bir türle değiştirmek için alternatif yöntemler deneyin.

### <a name="supported-editions"></a>Desteklenen sürümler

**Belirti**

Geçiş, aşağıdaki metni içeren bir hata iletisiyle başarısız olur:

    Migration settings validation error: The edition of the server [Business Intelligence Edition (64-bit)] does not match the supported edition(s) [Enterprise,Standard,Developer].

**Geçi -ci çözüm**

Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure SQL Veritabanı'na çevrimiçi geçişdesteği yalnızca Kurumsal, Standart ve Geliştirici sürümlerine kadar uzanır. Geçiş işlemine başlamadan önce desteklenen bir sürümü kullandığınızdan emin olun.
