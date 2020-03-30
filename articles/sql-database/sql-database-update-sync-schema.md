---
title: SQL Veri Eşitleme'de şema değişikliklerinin çoğaltılması otomatikleştirin
description: Azure SQL Veri Eşitleme'de şema değişikliklerinin çoğaltılmasını otomatikleştirin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 11/14/2018
ms.openlocfilehash: 639901975bbb66b9f410bea297d9e48cd96d6d1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73822441"
---
# <a name="automate-the-replication-of-schema-changes-in-azure-sql-data-sync"></a>Azure SQL Veri Eşitleme'de şema değişikliklerinin çoğaltılması otomatikleştirin

SQL Veri Eşitleme, kullanıcıların verileri Azure SQL veritabanları ve şirket içi SQL Server arasında tek bir yönde veya her iki yönde eşitlemelerine olanak tanır. SQL Veri Eşitlemesi'nin geçerli sınırlamalarından biri şema değişikliklerinin çoğaltılması için destek eksikliğidir. Tablo şemasını her değiştiriniz, değişiklikleri hub ve tüm üyeler dahil olmak üzere tüm uç noktalara el ile uygulamanız ve ardından eşitleme şemasını güncelleştirmeniz gerekir.

Bu makalede, tüm SQL Veri Eşitleme uç noktalarına şema değişikliklerini otomatik olarak çoğaltmak için bir çözüm tanınır.
1. Bu çözüm, şema değişikliklerini izlemek için bir DDL tetikleyicisi kullanır.
1. Tetikleyici, izleme tablosuna şema değiştirme komutlarını ekler.
1. Bu izleme tablosu, Veri Eşitleme hizmetini kullanarak tüm uç noktalara eşitlenir.
1. Eklemeden sonra DML tetikleyicileri diğer uç noktalarda şema değişiklikleri uygulamak için kullanılır.

Bu makalede, bir şema değişikliğine örnek olarak ALTER TABLE kullanır, ancak bu çözüm diğer şema değişiklikleri türleri için de çalışır.

> [!IMPORTANT]
> Eşitleme ortamınızda otomatik şema değişikliği çoğaltmasını uygulamaya başlamadan önce, özellikle [Sorun Giderme](#troubleshoot) ve [Diğer hususlarla](#other)ilgili bölümleri, bu makaleyi dikkatle okumanızı öneririz. Ayrıca SQL Data [Sync ile birden çok bulut ve şirket içi veritabanlarında Eşitleme verilerini](sql-database-sync-data.md)okumanızı öneririz. Bazı veritabanı işlemleri bu makalede açıklanan çözümü bozabilir. Bu sorunları gidermek için SQL Server ve Transact-SQL hakkında ek etki alanı bilgisi gerekebilir.

![Şema değişikliklerinin çoğaltılması otomatikleştir](media/sql-database-update-sync-schema/automate-schema-changes.png)

## <a name="set-up-automated-schema-change-replication"></a>Otomatik şema değişikliği çoğaltmasını ayarlama

### <a name="create-a-table-to-track-schema-changes"></a>Şema değişikliklerini izlemek için tablo oluşturma

Eşitleme grubundaki tüm veritabanlarındaki şema değişikliklerini izlemek için bir tablo oluşturun:

```sql
CREATE TABLE SchemaChanges (
ID bigint IDENTITY(1,1) PRIMARY KEY,
SqlStmt nvarchar(max),
[Description] nvarchar(max)
)
```

Bu tabloda şema değişikliklerinin sırasını izlemek için bir kimlik sütunu vardır. Gerekirse daha fazla bilgi günlüğe kaydetmek için daha fazla alan ekleyebilirsiniz.

### <a name="create-a-table-to-track-the-history-of-schema-changes"></a>Şema değişikliklerinin geçmişini izlemek için bir tablo oluşturma

Tüm uç noktalarda, en son uygulanan şema değiştirme komutunun kimliğini izlemek için bir tablo oluşturun.

```sql
CREATE TABLE SchemaChangeHistory (
LastAppliedId bigint PRIMARY KEY
)
GO

INSERT INTO SchemaChangeHistory VALUES (0)
```

### <a name="create-an-alter-table-ddl-trigger-in-the-database-where-schema-changes-are-made"></a>Şema değişikliklerinin yapıldığı veritabanında ALTER TABLE DDL tetikleyicisi oluşturma

ALTER TABLE işlemleri için bir DDL tetikleyicisi oluşturun. Bu tetikleyiciyi yalnızca şema değişikliklerinin yapıldığı veritabanında oluşturmanız gerekir. Çakışmaları önlemek için, eşitleme grubunda ki tek bir veritabanında şema değişikliklerine izin verin.

```sql
CREATE TRIGGER AlterTableDDLTrigger
ON DATABASE
FOR ALTER_TABLE
AS

-- You can add your own logic to filter ALTER TABLE commands instead of replicating all of them.

IF NOT (EVENTDATA().value('(/EVENT_INSTANCE/SchemaName)[1]', 'nvarchar(512)') like 'DataSync')

INSERT INTO SchemaChanges (SqlStmt, Description)
    VALUES (EVENTDATA().value('(/EVENT_INSTANCE/TSQLCommand/CommandText)[1]', 'nvarchar(max)'), 'From DDL trigger')
```

Tetikleyici, her ALTER TABLE komutu için şema değiştirme izleme tablosuna bir kayıt ekler. Bu örnek, şema **DataSync**altında yapılan şema değişikliklerini çoğaltmayı önlemek için bir filtre ekler, çünkü bunlar büyük olasılıkla Veri Eşitleme hizmeti tarafından yapılır. Yalnızca belirli şema değişikliklerini çoğaltmak istiyorsanız daha fazla filtre ekleyin.

Diğer şema değişikliklerini çoğaltmak için daha fazla tetikleyici de ekleyebilirsiniz. Örneğin, depolanan yordamlarda değişiklikleri çoğaltmak için CREATE_PROCEDURE, ALTER_PROCEDURE ve DROP_PROCEDURE tetikleyiciler oluşturun.

### <a name="create-a-trigger-on-other-endpoints-to-apply-schema-changes-during-insertion"></a>Ekleme sırasında şema değişiklikleri uygulamak için diğer uç noktalarda tetikleyici oluşturma

Bu tetikleyici, diğer uç noktalara eşitlendiğinde şema değiştirme komutunu çalıştırıyor. Şema değişikliklerinin yapıldığı (diğer bir önceki adımda DDL tetikleyicisinin `AlterTableDDLTrigger` oluşturulduğu veritabanında) dışında, tüm uç noktalarda bu tetikleyiciyi oluşturmanız gerekir.

```sql
CREATE TRIGGER SchemaChangesTrigger
ON SchemaChanges
AFTER INSERT
AS
DECLARE \@lastAppliedId bigint
DECLARE \@id bigint
DECLARE \@sqlStmt nvarchar(max)
SELECT TOP 1 \@lastAppliedId=LastAppliedId FROM SchemaChangeHistory
SELECT TOP 1 \@id = id, \@SqlStmt = SqlStmt FROM SchemaChanges WHERE id \> \@lastAppliedId ORDER BY id
IF (\@id = \@lastAppliedId + 1)
BEGIN
    EXEC sp_executesql \@SqlStmt
        UPDATE SchemaChangeHistory SET LastAppliedId = \@id
    WHILE (1 = 1)
    BEGIN
        SET \@id = \@id + 1
        IF exists (SELECT id FROM SchemaChanges WHERE ID = \@id)
            BEGIN
                SELECT \@sqlStmt = SqlStmt FROM SchemaChanges WHERE ID = \@id
                EXEC sp_executesql \@SqlStmt
                UPDATE SchemaChangeHistory SET LastAppliedId = \@id
            END
        ELSE
            BREAK;
    END
END
```

Bu tetikleyici eklemeden sonra çalışır ve geçerli komutun bir sonraki çalışması gerekip gerekmediğini denetler. Kod mantığı, şema değişikliği deyiminin atlanamamasını ve ekleme bozuk olsa bile tüm değişikliklerin uygulanmasını sağlar.

### <a name="sync-the-schema-change-tracking-table-to-all-endpoints"></a>Şema değişikliği izleme tablosunu tüm uç noktalarla eşitleme

Varolan eşitleme grubunu veya yeni bir eşitleme grubunu kullanarak şema değişikliği izleme tablosunu tüm uç noktalarla eşitleyebilirsiniz. İzleme tablosundaki değişikliklerin, özellikle tek yönlü eşitleme kullanıyorsanız, tüm uç noktalarla eşitlendiğinden emin olun.

Bu tablo farklı uç noktalarda farklı bir durum koruduğundan, şema değiştirme geçmişi tablosunu eşitlemeyin.

### <a name="apply-the-schema-changes-in-a-sync-group"></a>Şema değişikliklerini eşitleme grubunda uygulama

Yalnızca DDL tetikleyicisinin oluşturulduğu veritabanında yapılan şema değişiklikleri çoğaltılır. Diğer veritabanlarında yapılan şema değişiklikleri çoğaltılamaz.

Şema değişiklikleri tüm uç noktalara çoğaltıldıktan sonra, yeni sütunları başlatmak veya eşitlemeyi durdurmak için eşitleme şemasını güncelleştirmek için ek adımlar atmanız gerekir.

#### <a name="add-new-columns"></a>Yeni sütunlar ekleme

1.  Şema değişsin.

1.  Tetikleyiciyi oluşturan adımı tamamlayana kadar yeni sütunların dahil olduğu yerlerde veri değişikliğinden kaçının.

1.  Şema değişikliklerinin tüm uç noktalara uygulanmasını bekleyin.

1.  Veritabanı şemasını yenileyin ve yeni sütunu eşitşesine ekleyin.

1.  Yeni sütundaki veriler sonraki eşitleme işlemi sırasında eşitlenir.

#### <a name="remove-columns"></a>Sütunları kaldırma

1.  Eşitleme şemasından sütunları kaldırın. Veri Eşitleme bu sütunlarda veri eşitleme durur.

1.  Şema değişsin.

1.  Veritabanı şeasını yenileyin.

#### <a name="update-data-types"></a>Veri türlerini güncelleştirme

1.  Şema değişsin.

1.  Şema değişikliklerinin tüm uç noktalara uygulanmasını bekleyin.

1.  Veritabanı şeasını yenileyin.

1.  Yeni ve eski veri türleri tam olarak uyumlu değilse -örneğin, geçiş `int` `bigint` değiştirirseniz- tetikleyicileri oluşturan adımlar tamamlanmadan önce eşitleme başarısız olabilir. Eşitleme yeniden denemeden sonra başarılı olur.

#### <a name="rename-columns-or-tables"></a>Sütunları veya tabloları yeniden adlandır

Sütunları veya tabloları yeniden adlandırmak, Veri Eşitlemesini'nin çalışmayı durdurmasını sağlar. Yeni bir tablo veya sütun oluşturun, verileri geri doldurun ve yeniden adlandırma yerine eski tabloyu veya sütunu silin.

#### <a name="other-types-of-schema-changes"></a>Diğer şema değişiklikleri türleri

Diğer şema değişiklikleri türleri için - örneğin, depolanmış yordamlar oluşturma veya bir dizin bırakarak- eşitleme şema güncelleştirme gerekli değildir.

## <a name="troubleshoot-automated-schema-change-replication"></a><a name="troubleshoot"></a>Sorun giderme otomatik şema değişikliği çoğaltma

Bu makalede açıklanan çoğaltma mantığı bazı durumlarda çalışmayı durdurur( örneğin, Azure SQL Veritabanı'nda desteklenmeyen bir şirket içi veritabanında şema değişikliği yaptıysanız). Bu durumda, şema değişikliği izleme tablosu eşitleme başarısız olur. Bu sorunu el ile gidermeniz gerekir:

1.  DDL tetikleyicisini devre dışı bırakın ve sorun giderilene kadar şema değişikliklerinden kaçının.

1.  Sorunun gerçekleştiği bitiş noktası veritabanında, şema değişikliğinin yapılabildiği bitiş noktasında KI SİNESERT tetikleyicisini devre dışı bırakın. Bu eylem şema değiştirme komutu eşitlenir sağlar.

1.  Şema değişikliği izleme tablosunu eşitlemek için eşitlemeyi tetikle.

1.  Sorunun gerçekleştiği bitiş noktası veritabanında, son uygulanan şema değişikliği komutunun kimliğini almak için şema değiştirme geçmişi tablosunu sorgula.

1.  Önceki adımda aldığınız kimlik değerinden daha büyük bir kimlikle tüm komutları listelemek için şema değişiklik izleme tablosunu sorgula.

    a.  Bitiş noktası veritabanında yürütülemeyen komutları yoksayın. Şema tutarsızlığıyla başa çıkmalısın. Tutarsızlık uygulamanızı etkilerse orijinal şema değişikliklerini geri çevirin.

    b.  Uygulanması gereken komutları el ile uygulayın.

1.  Şema değişiklik geçmişi tablosunu güncelleştirin ve son uygulanan kimliği doğru değere ayarlayın.

1.  Şeanın güncel olup olmadığını iki kez kontrol edin.

1.  İkinci adımda devre dışı bırakılan AFTER INSERT tetikleyicisini yeniden etkinleştirin.

1.  İlk adımda devre dışı bırakılan DDL tetikleyicisini yeniden etkinleştirin.

Şema değişikliği izleme tablosundaki kayıtları temizlemek istiyorsanız, TRUNCATE yerine DELETE'yi kullanın. DBCC CHECKIDENT kullanarak şema değiştirme izleme tablosunda kimlik sütununa asla yeniden tohum lamayın. Yeni şema değişiklik izleme tabloları oluşturabilir ve yeniden tohumlama gerekiyorsa DDL tetikleyicisinde tablo adını güncelleştirebilirsiniz.

## <a name="other-considerations"></a><a name="other"></a>Diğer Hususlar

-   Hub ve üye veritabanlarını yapılandıran veritabanı kullanıcılarının şema değişikliği komutlarını yürütmek için yeterli izne sahip olması gerekir.

-   Yalnızca seçili tablolarda veya işlemlerde şema değişikliğini çoğaltmak için DDL tetikleyicisine daha fazla filtre ekleyebilirsiniz.

-   Yalnızca DDL tetikleyicisinin oluşturulduğu veritabanında şema değişiklikleri yapabilirsiniz.

-   Şirket içi BIR SQL Server veritabanında değişiklik yapıyorsanız, şema değişikliğinin Azure SQL Veritabanı'nda desteklendirildiğinden emin olun.

-   DDL tetikleyicisinin oluşturulduğu veritabanı dışındaki veritabanlarında şema değişiklikleri yapılırsa, değişiklikler çoğaltılmaz. Bu sorunu önlemek için, diğer uç noktalardaki değişiklikleri engellemek için DDL tetikleyicileri oluşturabilirsiniz.

-   Şema değişim izleme tablosunun şemasını değiştirmeniz gerekiyorsa, değişikliği yapmadan önce DDL tetikleyicisini devre dışı bırakın ve değişikliği tüm uç noktalara el ile uygulayın. Şemayı aynı tabloda bir SONRA INSERT tetikleyicisinde güncellemek çalışmaz.

-   DBCC CHECKIDENT kullanarak kimlik sütununa yeniden tohum atmayın.

-   Şema değişikliği izleme tablosundaki verileri temizlemek için TRUNCATE'i kullanmayın.

## <a name="next-steps"></a>Sonraki adımlar

SQL Data Sync hakkında daha fazla bilgi için bkz.:

-   Genel Bakış - [Azure SQL Veri Eşitlemi ile verileri birden çok bulut ve şirket içi veritabanları nda eşitleme](sql-database-sync-data.md)
-   Veri Eşitlemeyi Ayarlama
    - Portalda - [Öğretici: Azure SQL Veritabanı ve SQL Server arasında verileri şirket içinde eşitlemek için SQL Veri Eşitlemeyi'ni ayarlayın](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Veri Eşitleme Aracısı - [Azure SQL Veri Eşitleme için Veri Eşitleme Aracısı](sql-database-data-sync-agent.md)
-   En iyi uygulamalar - [Azure SQL Veri Eşitleme için en iyi uygulamalar](sql-database-best-practices-data-sync.md)
-   Monitör - [Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin](sql-database-sync-monitor-oms.md)
-   Sorun Giderme - [Azure SQL Veri Eşitleme ile sorun giderme](sql-database-troubleshoot-data-sync.md)
-   Eşitleme şeasını güncelleştirme
    -   PowerShell ile - [Varolan bir eşitleme grubunda eşitleme şemasını güncellemek için PowerShell'i kullanın](scripts/sql-database-sync-update-schema.md)
