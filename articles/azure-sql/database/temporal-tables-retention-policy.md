---
title: Zamana bağlı tablolardaki geçmiş verileri yönetme
description: Geçmiş verileri denetiminizin altında tutmak için zamana bağlı saklama ilkesini nasıl kullanacağınızı öğrenin.
services: sql-database
ms.service: sql-db-mi
ms.subservice: development
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
ms.date: 09/25/2018
ms.openlocfilehash: 8c5ea1f7ef094944c3e5a20dd19bce6d8cce294d
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85985452"
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Bekletme ilkesiyle zamana bağlı tablolardaki geçmiş verileri yönetme
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Zamana bağlı tablolar, özellikle geçmiş verileri daha uzun bir süre boyunca saklamanız durumunda, normal tablolardan daha fazla veritabanı boyutunu artırabilir. Bu nedenle, geçmiş veriler için bekletme ilkesi, her zamana bağlı tablonun yaşam döngüsünün planlanmasına ve yönetilmesine ilişkin önemli bir yönüdür. Azure SQL veritabanı ve Azure SQL yönetilen örneği 'ndeki zamana bağlı tablolar, bu görevi gerçekleştirmenize yardımcı olan kullanımı kolay bekletme mekanizmasıyla birlikte gelir.

Zamana bağlı geçmiş tutma, kullanıcıların esnek eskime ilkeleri oluşturmalarına olanak sağlayan ayrı tablo düzeyinde yapılandırılabilir. Zamana bağlı saklama uygulamak basittir: tablo oluşturma veya şema değişikliği sırasında yalnızca bir parametre ayarlanmasını gerektirir.

Saklama ilkesini tanımladıktan sonra, otomatik veri temizleme için uygun geçmiş satırları varsa Azure SQL veritabanı ve Azure SQL yönetilen örneği düzenli olarak denetlemeye başlar. Eşleşen satırların tanımlanması ve geçmiş tablosundan kaldırılması, sistem tarafından zamanlanan ve çalıştırılan arka plan görevinde saydam bir şekilde gerçekleşir. Geçmiş tablosu satırları için yaş koşulu, SYSTEM_TIME döneminin sonunu temsil eden sütuna göre denetlenir. Örneğin, saklama dönemi altı aya ayarlanırsa, temizleme için uygun tablo satırları aşağıdaki koşulu karşılar:

```sql
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```

Yukarıdaki örnekte, **ValidTo** sütununun SYSTEM_TIME döneminin sonuna karşılık geldiğini varsaydık.

## <a name="how-to-configure-retention-policy"></a>Saklama ilkesini yapılandırma

Zamana bağlı bir tablo için bekletme ilkesini yapılandırmadan önce, *veritabanı düzeyinde*zamana bağlı geçmiş saklama özelliğinin etkinleştirilip etkinleştirilmediğini denetleyin.

```sql
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

Veritabanı bayrağı **is_temporal_history_retention_enabled** varsayılan olarak açık olarak ayarlanmıştır, ancak KULLANıCıLAR onu alter database ifadesiyle değiştirebilir. Ayrıca, bir [süre geri yükleme](recovery-using-backups.md) işleminden sonra OTOMATIK olarak kapalı olarak ayarlanır. Veritabanınız için zamana bağlı geçmiş bekletme temizleme işlemini etkinleştirmek için aşağıdaki ifadeyi yürütün:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
```

> [!IMPORTANT]
> **IS_TEMPORAL_HISTORY_RETENTION_ENABLED** kapalı olsa bile, zamana bağlı tablolar için bekletme yapılandırabilirsiniz, ancak eski satırlar için otomatik temizleme bu durumda tetiklenmez.

Bekletme ilkesi, HISTORY_RETENTION_PERIOD parametresi için değer belirtilerek tablo oluşturma sırasında yapılandırılır:

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

Azure SQL veritabanı ve Azure SQL yönetilen örneği, farklı zaman birimleri kullanarak bekletme dönemi belirtmenize izin verir: günler, haftalar, aylar ve YıLLAR. HISTORY_RETENTION_PERIOD atlanırsa, sonsuz saklama varsayılır. SONSUZ anahtar sözcüğünü açıkça de kullanabilirsiniz.

Bazı senaryolarda, tablo oluşturulduktan sonra bekletme yapılandırmak veya daha önce yapılandırılan değeri değiştirmek isteyebilirsiniz. Bu durumda ALTER TABLE deyimini kullanın:

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

> [!IMPORTANT]
> SYSTEM_VERSIONING kapalı olarak ayarlamak *,* saklama süresi değerini korumaz. SYSTEM_VERSIONING HISTORY_RETENTION_PERIOD olmadan açık olarak ayarlanması, sonsuz saklama süresi içinde açıkça sonuçlanır.

Bekletme ilkesinin geçerli durumunu gözden geçirmek için, tek tek tablolar için bekletme dönemleriyle veritabanı düzeyinde zamana bağlı saklama etkinleştirme bayrağını birleştiren aşağıdaki sorguyu kullanın:

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

## <a name="how-ages-rows-are-deleted"></a>Yaş satırları nasıl silinir

Temizleme işlemi, geçmiş tablosunun Dizin düzenine bağlıdır. *Yalnızca kümelenmiş dizine (B-ağacı veya columnstore) sahip geçmiş tablolarının sınırlı bekletme ilkesi yapılandırıldığını*fark etmek önemlidir. Sınırlı saklama süresine sahip tüm zamana bağlı tablolar için eski veri temizleme işlemini gerçekleştirmek üzere bir arka plan görevi oluşturulur.
Rowstore (B-ağacı) kümelenmiş dizini için Temizleme mantığı, daha küçük öbeklerdeki eski satırı (10.000 'ye kadar) siler ve veritabanı günlüğü ve GÇ alt sistemi üzerindeki basınç en aza indirir. Temizleme mantığı gerekli B-ağacı dizinini kullanmasına karşın, saklama süresinden eski olan satırlarda silme işlemlerinin sırası en kesin garanti edilemez. Bu nedenle, *uygulamalarınızda Temizleme sırası üzerinde hiçbir bağımlılık*kullanmayın.

Kümelenmiş columnstore için temizleme görevi tüm [satır gruplarını](/sql/relational-databases/indexes/columnstore-indexes-overview) tek seferde kaldırır (genellikle 1.000.000 satır içerir), özellikle de geçmiş veriler yüksek bir hızda oluşturulduktan sonra oldukça etkilidir.

![Kümelenmiş columnstore saklama](./media/temporal-tables-retention-policy/cciretention.png)

Mükemmel veri sıkıştırma ve verimli bekletme Temizleme, iş yükünüz hızlı bir şekilde yüksek miktarda geçmiş veri oluşturduğunda, kümelenmiş columnstore dizinini senaryolar için kusursuz bir tercih yapar. Bu model, değişiklik izleme ve denetim, eğilim analizi veya IoT veri alımı için zamana bağlı [tabloları kullanan yoğun işlem temelli işleme iş yükleri](/sql/relational-databases/tables/temporal-table-usage-scenarios) için tipik bir işlemdir.

## <a name="index-considerations"></a>Dizin değerlendirmeleri

Rowstore kümelenmiş dizini olan tablolar için temizleme görevi, SYSTEM_TIME döneminin sonuna karşılık gelen sütunla başlamak için Dizin gerektirir. Böyle bir dizin yoksa, sınırlı bir saklama süresi yapılandıramazsınız:

*İleti 13765, düzey 16, durum 1 <br> </br> ayarı sistem sürümü tutulan zamana bağlı ' temporalstagetestdb. dbo. websiteuserınfo ' tablosunda, ' temporalstagetestdb. dbo. Websiteuserınfohistory ' geçmiş tablosu gerekli kümelenmiş dizini içermediğinden başarısız oldu. Geçmiş tablosunda SYSTEM_TIME döneminin sonuyla eşleşen sütundan başlayarak kümelenmiş bir columnstore veya B-ağacı dizini oluşturmayı düşünün.*

Azure SQL veritabanı ve Azure SQL yönetilen örneği tarafından oluşturulan varsayılan geçmiş tablosunun, bekletme ilkesi için uyumlu olan kümelenmiş dizine zaten sahip olduğunu fark etmek önemlidir. Sınırlı saklama süresine sahip bir tabloda bu dizini kaldırmaya çalışırsanız, işlem aşağıdaki hatayla başarısız olur:

*İleti 13766, düzey 16, durum 1, <br> </br> eski verilerin otomatik olarak temizlenmesi için kullanıldığından ' Websiteuserınfohistory. IX_WebsiteUserInfoHistory ' kümelenmiş dizinini bırakamıyor. Bu dizini bırakmalısınız, ilgili sistem sürümü tutulan zamana bağlı tabloda HISTORY_RETENTION_PERIOD sonsuz olarak ayarlamayı düşünün.*

Geçmiş satırları artan düzende (dönem sonuna kadar sıralanır) eklenirse ve bu durum, geçmiş tablosu SYSTEM_VERSIONIOING mekanizması tarafından özel olarak doldurulduğu zaman büyük/küçük harf olarak, kümelenmiş columnstore dizininde Temizleme işlemi en iyi şekilde gerçekleşir. Geçmiş tablosundaki satırlar dönem sonuna göre sıralı değilse (mevcut geçmiş verileri geçirdiyseniz bu durum söz konusu olabilir), en iyi performansı elde etmek için, kümelenmiş columnstore dizinini, doğru şekilde sıralanan B-Tree rowstore dizininin üstünde yeniden oluşturmanız gerekir.

Geçmiş tablosunda, sistem sürümü oluşturma işlemi tarafından doğal olarak uygulanan satır gruplarındaki sıralamayı değiştirebileceğinden, sınırlı saklama süresine sahip kümelenmiş columnstore dizinini yeniden oluşturmayı önleyin. Geçmiş tablosunda kümelenmiş columnstore dizinini yeniden oluşturmanız gerekiyorsa, bunu uyumlu B ağacı dizininin en üstünde yeniden oluşturarak, normal veri temizleme için gereken RowGroups sıralamasını koruyarak yapın. Garantili veri sırası olmayan kümelenmiş sütun dizini olan mevcut geçmiş tablosuyla zamana bağlı tablo oluşturursanız aynı yaklaşım gerçekleştirilmelidir:

```sql
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
```

Kümelenmiş columnstore diziniyle geçmiş tablosu için sınırlı saklama süresi yapılandırıldığında, bu tabloda ek kümelenmemiş B-ağacı dizinleri oluşturamazsınız:

```sql
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
```

Yukarıdaki deyimin yürütülmesi girişimi aşağıdaki hatayla başarısız olur:

*İleti 13772, düzey 16, durum 1, <br> </br> sınırlı saklama süresi ve kümelenmiş columnstore dizini tanımlı olduğundan, zamana bağlı ' Websiteuserınfohistory ' geçmiş tablosunda kümelenmemiş dizin oluşturamaz.*

## <a name="querying-tables-with-retention-policy"></a>Tabloları bekletme ilkesiyle sorgulama

Eski satırlar temizleme görevi tarafından *herhangi bir zamanda ve isteğe bağlı*olarak silinebildiğinden, geçici tablodaki tüm sorgular sonlu bekletme ilkesiyle eşleşen geçmiş satırları otomatik olarak filtreler.

Aşağıdaki resimde basit bir sorgu için sorgu planı gösterilmektedir:

```sql
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
```

Sorgu planı, geçmiş tablosundaki (vurgulanmış) kümelenmiş dizin tarama işlecinde dönem sonu sütununa (ValidTo) uygulanan ek filtre içerir. Bu örnekte, Websiteuserınfo tablosunda bir ay bekletme döneminin ayarlandığı varsayılır.

![Bekletme sorgu filtresi](./media/temporal-tables-retention-policy/queryexecplanwithretention.png)

Ancak, geçmiş tablosunu doğrudan sorgularsınız, belirtilen saklama süresinden eski olan ve yinelenebilir sorgu sonuçları garantisi olmadan daha eski olan satırları görebilirsiniz. Aşağıdaki resimde, ek filtreler uygulanmadan geçmiş tablosundaki sorgu için sorgu yürütme planı gösterilmektedir:

![Saklama filtresi olmadan geçmişi sorgulama](./media/temporal-tables-retention-policy/queryexecplanhistorytable.png)

Tutarsız veya beklenmedik sonuçlar elde ettiğiniz için, saklama süresinden daha fazla bilgi almak için iş mantığınızı dayanmayın. Zamana bağlı tablolardaki verileri analiz etmek için SYSTEM_TIME yan tümcesiyle birlikte zamana bağlı sorgular kullanmanızı öneririz.

## <a name="point-in-time-restore-considerations"></a>Zaman içindeki bir noktaya geri yükleme konuları

[Mevcut veritabanını belirli bir noktaya geri yükleyerek](recovery-using-backups.md)yeni veritabanı oluşturduğunuzda, veritabanı düzeyinde zamana bağlı saklama devre dışıdır. (**is_temporal_history_retention_enabled** bayrak kapalı olarak ayarlanır). Bu işlevsellik, geri yükleme sırasında geçmiş tüm satırları inceleyerek, eski satırlar sorgulanmadan önce kaldırılmalarını kaygılanmadan incelemenizi sağlar. Bu işlemi, *geçmiş verileri yapılandırılan bekletme döneminin ötesinde incelemek*için kullanabilirsiniz.

Zamana bağlı bir tabloda bir ay Bekletme dönemi olduğunu varsayalım. Veritabanınız Premium hizmet katmanında oluşturulduysa, son olarak 35 güne kadar veritabanı durumu ile veritabanı kopyası oluşturabilirsiniz. Bu, geçmiş tablosunu doğrudan sorgulayarak 65 güne kadar eski olan geçmiş satırları analiz etmenize olanak tanır.

Zamana bağlı saklama temizlemeyi etkinleştirmek istiyorsanız, zaman sonra geri yükleme sonrasında aşağıdaki Transact-SQL ifadesini çalıştırın:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
```

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınızda zamana bağlı tabloları nasıl kullanacağınızı öğrenmek için, zamana bağlı tabloları kullanmaya [başlama](../temporal-tables.md)konusunu inceleyin.

Müşteri tarafından zamana bağlı bir [uygulama başarısını](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) dinlemek ve canlı bir zamana bağlı [tanıtım](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)izlemek için Channel 9 ' a gidin.

Zamana bağlı tablolar hakkında ayrıntılı bilgi için, zamana bağlı [tabloları](/sql/relational-databases/tables/temporal-tables)gözden geçirin.
