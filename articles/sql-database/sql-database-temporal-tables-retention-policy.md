---
title: Geçici Tablolar'da geçmiş verileri yönetme
description: Geçmiş verileri denetiminiz altında tutmak için zamansal saklama ilkesini nasıl kullanacağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
ms.date: 09/25/2018
ms.openlocfilehash: 3c2460c6f5e0905f45106148ecc3e8a949cf221f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820680"
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Geçici Tablolar'daki geçmiş verileri bekletme ilkesiyle yönetme

Zamansal Tablolar, özellikle geçmiş verileri daha uzun bir süre saklarsanız, veritabanı boyutunu normal tablolardan daha fazla artırabilir. Bu nedenle, geçmiş veriler için saklama politikası planlama ve her zamansal tablonun yaşam döngüsü yönetmek önemli bir yönüdür. Azure SQL Veritabanı'ndaki Geçici Tablolar, bu görevi gerçekleştirmenize yardımcı olan kullanımı kolay bekletme mekanizmasıyla birlikte gelir.

Zamansal geçmiş tutma, kullanıcıların esnek yaşlanan polisler oluşturmasına olanak tanıyan tek tek tablo düzeyinde yapılandırılabilir. Zamansal bekletme uygulamak basittir: tablo oluşturma veya şema değişikliği sırasında ayarlanması için yalnızca bir parametre gerekir.

Bekletme ilkesini tanımladıktan sonra, Azure SQL Veritabanı otomatik veri temizleme için uygun geçmiş satırlar olup olmadığını düzenli olarak denetlemeye başlar. Eşleşen satırların tanımlanması ve geçmiş tablosundan kaldırılması, sistem tarafından zamanlanan ve çalıştırılan arka plan görevinde saydam bir şekilde gerçekleşir. Geçmiş tablo satırlarıiçin yaş koşulu, SYSTEM_TIME dönemin sonunu temsil eden sütuna göre denetlenir. Örneğin bekletme süresi altı aya ayarlanmışsa, temizleme için uygun tablo satırları aşağıdaki koşulu karşılar:

```
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```

Önceki örnekte, **ValidTo** sütununun SYSTEM_TIME dönemin sonuna karşılık geldiğini varsaydık.

## <a name="how-to-configure-retention-policy"></a>Bekletme ilkesi nasıl yapılandırılabilen

Zamansal tablo için bekletme ilkesini yapılandırmadan önce, önce *zamansal*geçmiş bekletme veritabanı düzeyinde etkin olup olmadığını denetleyin.

```
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

Veritabanı bayrağı **is_temporal_history_retention_enabled** varsayılan olarak AÇIK olarak ayarlanır, ancak kullanıcılar ALTER DATABASE deyimi ile değiştirebilirsiniz. Ayrıca otomatik olarak zaman geri yükleme işlemi [noktasından](sql-database-recovery-using-backups.md) sonra OFF ayarlanır. Veritabanınız için zamansal geçmiş tutma temizlemesini etkinleştirmek için aşağıdaki ifadeyi uygulayın:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
```

> [!IMPORTANT]
> **is_temporal_history_retention_enabled** KAPALI olsa bile zamansal tablolar için bekletme yapılandırabilirsiniz, ancak bu durumda yaşlı satırlar için otomatik temizleme tetiklenmez.

Bekletme ilkesi tablo oluşturma sırasında HISTORY_RETENTION_PERIOD parametresi için değer belirterek yapılandırılır:

```sql
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
```

Azure SQL Veritabanı, gün, hafta, ay ve YILlar gibi farklı zaman birimlerini kullanarak bekletme süresini belirtmenize olanak tanır. HISTORY_RETENTION_PERIOD atlanırsa, SONSUZ bekletme kabul edilir. SONSUZ anahtar sözcük'ünde de açıkça kullanabilirsiniz.

Bazı senaryolarda, tablo oluşturulduktan sonra bekletme yapılandırmak veya daha önce yapılandırılmış değeri değiştirmek isteyebilirsiniz. Bu durumda ALTER TABLE deyimini kullanın:

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

> [!IMPORTANT]
> SYSTEM_VERSIONING'ı OFF'a ayarlamak bekletme süresi değerini *korumaz.* SYSTEM_VERSIONING HISTORY_RETENTION_PERIOD belirtilmeden AÇIK olarak ayarlamak, SONSUZ bekletme dönemine neden olabilir.

Bekletme ilkesinin geçerli durumunu gözden geçirmek için, veritabanı düzeyinde zamansal bekletme etkinleştirme bayrağını tek tek tablolar için bekletme süreleriyle birleştiren aşağıdaki sorguyu kullanın:

```sql
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
```


## <a name="how-sql-database-deletes-aged-rows"></a>SQL Veritabanı yaşlı satırları nasıl siler?

Temizleme işlemi, geçmiş tablosunun dizin düzenine bağlıdır. *Yalnızca kümelenmiş dizin (B-ağacı veya sütun deposu) olan geçmiş tablolarının sonlu bekletme ilkesiyapılandırabileceğini*fark etmek önemlidir. Sonlu bekletme süresi olan tüm zamansal tablolar için yaşlı veri temizleme gerçekleştirmek için bir arka plan görevi oluşturulur.
Rowstore (B-tree) kümelenmiş dizin için temizleme mantığı veritabanı günlüğü ve IO alt sistemi üzerindeki basıncı en aza indiren küçük parçalar (10K)'a kadar yaşlı satırı siler. Temizleme mantığı gerekli B-ağacı dizini kullansa da, bekletme süresinden daha eski satırlar için silme sırası kesin olarak garanti edilemez. Bu nedenle, *uygulamalarınızda temizleme siparişine herhangi bir bağımlılık yapmayın.*

Kümelenmiş sütun deposunun temizleme görevi, özellikle geçmiş veriler yüksek hızda oluşturulduğunda çok verimli olan tüm [satır gruplarını](https://msdn.microsoft.com/library/gg492088.aspx) aynı anda kaldırır (genellikle her biri 1 milyon satır içerir).

![Kümelenmiş sütun deposu tutma](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

Mükemmel veri sıkıştırma ve verimli bekletme temizleme, iş yükünüz hızla yüksek miktarda geçmiş veri oluşturduğunda kümelenmiş sütun deposu dizini senaryolar için mükemmel bir seçim haline getirir. Bu desen, değişiklik izleme ve denetleme, eğilim çözümlemesi veya IoT veri alımı için zamansal tabloları kullanan yoğun [işlem işleme iş yükleri](https://msdn.microsoft.com/library/mt631669.aspx) için tipiktir.

## <a name="index-considerations"></a>Dizin hususları

Rowstore kümelenmiş dizinli tabloların temizleme görevi, dizinin SYSTEM_TIME dönemin sonuna karşılık gelen sütunla başlamasını gerektirir. Bu tür bir dizin yoksa, sonlu bir bekletme süresini yapılandıramazsınız:

*Msg 13765, Düzey 16, Durum 1 <br> </br> Sonlu bekletme süresi sistem versiyonu nda başarısız oldu temporal tablo 'temporalstagetestdb.dbo.WebsiteUserInfo' çünkü tarih tablosu 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' gerekli kümelenmiş dizin içermez. SYSTEM_TIME dönemin sonuyla eşleşen sütundan başlayarak, geçmiş tablosunda kümelenmiş bir sütun deposu veya B-ağacı dizini oluşturmayı düşünün.*

Azure SQL Veritabanı tarafından oluşturulan varsayılan geçmiş tablosunun bekletme ilkesi için uyumlu kümelenmiş dizini olduğunu fark etmek önemlidir. Bu dizini sonlu bekletme süresine sahip bir tabloda kaldırmaya çalışırsanız, işlem aşağıdaki hatayla başarısız olur:

*Msg 13766, Düzey 16, Durum 1 <br> </br> yaşlı verilerin otomatik temizliği için kullanılıyor çünkü kümelenmiş dizin 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' bırakamazsınız. Bu dizini düşürmeniz gerekiyorsa, ilgili sistem sürümlü zamansal tabloda HISTORY_RETENTION_PERIOD SONSUZ olarak ayarlamayı düşünün.*

Kümelenmiş sütun deposu dizinindeki temizleme, geçmiş satırları artan sıraya (dönem sonu sütununa sıralanmış) eklenirse en iyi şekilde çalışır, bu da tarih tablosunun yalnızca SYSTEM_VERSIONIOING mekanizması tarafından doldurulduğu nda her zaman olduğu gibi çalışır. Geçmiş tablosundaki satırlar dönem sonu sütununa göre sıralanmazsa (varolan geçmiş verileri aktarırsanız durum böyle olabilir), en iyi şekilde sıralanmış B-ağaç satır mağazası dizininin üzerinde kümelenmiş sütun deposu dizini oluşturmanız gerekir Performans.

Sistem sürüm çalışması tarafından doğal olarak dayatılan satır gruplarında sıralamayı değiştirebileceğinden, sonlu bekletme süresiyle geçmiş tablosunda kümelenmiş sütun deposu dizini yeniden oluşturmaktan kaçının. Geçmiş tablosunda kümelenmiş sütun deposu dizini yeniden oluşturmanız gerekiyorsa, bunu uyumlu B-ağacı dizininin üzerine yeniden oluşturarak ve düzenli veri temizleme için gerekli satır gruplarında sıralamayı koruyarak yapın. Garantili veri sırası olmadan sütun dizini kümelenmiş varolan geçmiş tablosu ile zamansal tablo oluşturursanız aynı yaklaşım alınmalıdır:

```sql
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
```

Kümelenmiş sütun deposu dizini ile geçmiş tablosu için sonlu bekletme süresi yapılandırıldığında, bu tabloda kümelenmiş olmayan b-ağacı dizinleri oluşturamazsınız:

```sql
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
```

Yukarıdaki ifadeyi yürütme girişimi aşağıdaki hataile başarısız olur:

*Msg 13772, Düzey 16, Durum 1 <br> </br> sınırlı bekletme süresi ve kümelenmiş sütun deposu dizini tanımlı olduğundan zamansal tarih tablosu 'WebsiteUserInfoHistory' üzerinde kümelenmiş olmayan dizin oluşturamaz.*

## <a name="querying-tables-with-retention-policy"></a>Tabloları bekletme ilkesiyle sorgulama

Zaman çizelgesindeki tüm sorgular, öngörülemeyen ve tutarsız sonuçlardan kaçınmak için, zaman içinde *herhangi bir noktada ve rasgele sırada*temizleme görevi tarafından silinebileceğinden, sonlu bekletme ilkesiyle eşleşen geçmiş satırları otomatik olarak filtreler.

Aşağıdaki resim, basit bir sorgu için sorgu planını gösterir:

```sql
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
```

Sorgu planı, geçmiş tablosundaki (vurgulanan) Kümelenmiş Dizin Scan işlecinde dönem sonu sütununa (ValidTo) uygulanan ek filtreyi içerir. Bu örnek, Bir AY saklama süresi Ninli Kullanıcı Bilgisi tablosunda ayarlandığını varsayar.

![Bekletme sorgu filtresi](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Ancak, geçmiş tablosunu doğrudan sorgularsanız, belirtilen bekletme döneminden daha eski, ancak yinelenebilir sorgu sonuçları için herhangi bir garanti olmadan satırlar görebilirsiniz. Aşağıdaki resimde, ek filtreler uygulanmadan geçmiş tablosundaki sorgu için sorgu yürütme planı gösterilmektedir:

![Bekletme filtresi olmadan geçmişi sorgulama](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Tutarsız veya beklenmeyen sonuçlar elde edebilirsiniz gibi bekletme süresi ötesinde tarih tablosu okuma işinizi mantık güvenmeyin. Zamansal tablolardaki verileri çözümleme için FOR SYSTEM_TIME yan tümcesi ile zamansal sorgular kullanmanızı öneririz.

## <a name="point-in-time-restore-considerations"></a>Zaman geri yükleme hususlar noktası

[Varolan veritabanını belirli bir noktaya geri vererek](sql-database-recovery-using-backups.md)yeni bir veritabanı oluşturduğunuzda, veritabanı düzeyinde zamansal bekletme devre dışı bırakılır. (**is_temporal_history_retention_enabled** bayrak OFF için ayarlanmış). Bu işlev, eski satırları n sorgulayabaşlamadan önce kaldırıldığından endişe etmeden, geri yükleme sırasında tüm geçmiş satırları incelemenize olanak tanır. *Yapılandırılmış bekletme süresinin ötesindeki geçmiş verileri incelemek*için kullanabilirsiniz.

Geçici bir tablonun bir AY bekletme süresi nin belirtildiğini varsan. Veritabanınız Premium Hizmet katmanında oluşturulduysa, veritabanı durumu yla birlikte veritabanı kopyasını geçmişte 35 güne kadar oluşturabilirsiniz. Bu, geçmiş tablosunu doğrudan sorgulayarak 65 güne kadar olan geçmiş satırları nızı etkili bir şekilde çözümlemenize olanak sağlar.

Zamansal bekletme temizlemesini etkinleştirmek istiyorsanız, aşağıdaki Transact-SQL deyimini zaman geri yükleme noktasından sonra çalıştırın:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
```

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınızda Geçici Tabloları nasıl kullanacağınızı öğrenmek için [Azure SQL Veritabanı'nda Geçici Tablolarla Başlarken'e](sql-database-temporal-tables.md)göz atın.

Gerçek bir müşteri [geçici uygulama başarı öyküsü](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) duymak ve canlı bir [zamansal gösteri](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)izlemek için Kanal 9 ziyaret edin.

Zamansal Tablolar hakkında ayrıntılı bilgi için [MSDN belgelerini](https://msdn.microsoft.com/library/dn935015.aspx)inceleyin.
