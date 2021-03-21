---
title: Genişletilmiş olaylar
description: Azure SQL veritabanı 'nda genişletilmiş olayları (XEvents) ve olay oturumlarının Microsoft SQL Server olay oturumlarından biraz farklı bir şekilde nasıl farklı olduğunu açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: reference
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.date: 12/19/2018
ms.openlocfilehash: 139673e46421aa0dc19298697872fbff5fe587af
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "96501218"
---
# <a name="extended-events-in-azure-sql-database"></a>Azure SQL veritabanı 'nda genişletilmiş olaylar 
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

[!INCLUDE [sql-database-xevents-selectors-1-include](../../../includes/sql-database-xevents-selectors-1-include.md)]

Azure SQL veritabanı 'ndaki genişletilmiş olaylar özellik kümesi, SQL Server ve Azure SQL yönetilen örneği özelliklerinin sağlam bir alt kümesidir.

*XEvents* , bazen bloglardaki ' genişletilmiş olaylar ' ve diğer resmi olmayan konumlarda kullanılan, resmi olmayan bir takma konumdadır.

Genişletilmiş olaylar hakkında ek bilgilere şu adresten ulaşılabilir:

- [Hızlı başlangıç: SQL Server genişletilmiş olaylar](/sql/relational-databases/extended-events/quick-start-extended-events-in-sql-server)
- [Genişletilmiş Olaylar](/sql/relational-databases/extended-events/extended-events)

## <a name="prerequisites"></a>Önkoşullar

Bu konu başlığı altında zaten bir bilginiz olduğunu varsaymaktadır:

- [Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)
- [Genişletilmiş olaylar](/sql/relational-databases/extended-events/extended-events)

- Genişletilmiş olaylar hakkındaki belgelerimizin toplu bilgileri SQL Server, Azure SQL veritabanı ve Azure SQL yönetilen örneği için geçerlidir.

Aşağıdaki öğelerin önceki pozlaması, olay dosyasını [hedef](#AzureXEventsTargets)olarak seçerken yararlı olur:

- [Azure depolama hizmeti](https://azure.microsoft.com/services/storage/)

- [Azure depolama ile Azure PowerShell](/powershell/module/az.storage/)

## <a name="code-samples"></a>Kod örnekleri

İlgili konular iki kod örneği sağlar:

- [Azure SQL veritabanı 'nda genişletilmiş olaylar için halka arabelleği hedef kodu](xevent-code-ring-buffer.md)

  - Kısa basit Transact-SQL betiği.
  - Bir halka arabellek hedefi ile işiniz bittiğinde, bir alter drop deyimini yürüterek kaynaklarını serbest bırakmanız gerektiğini belirten kod örneği konusunu vurgularız `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` . Daha sonra, tarafından başka bir halka arabelleği örneği ekleyebilirsiniz `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...` .

- [Azure SQL veritabanı 'nda genişletilmiş olaylar için olay dosyası hedef kodu](xevent-code-event-file.md)

  - Aşama 1, Azure depolama kapsayıcısı oluşturmak için PowerShell 'dir.
  - 2. aşama, Azure Storage kapsayıcısını kullanan Transact-SQL ' dir.

## <a name="transact-sql-differences"></a>Transact-SQL farkları

- SQL Server [olay oturumu oluştur](/sql/t-sql/statements/create-event-session-transact-sql) komutunu YÜRÜTTÜĞÜNÜZDE **on Server** yan tümcesini kullanırsınız. Ancak Azure SQL veritabanı 'nda bunun yerine **on Database** yan tümcesini kullanın.
- **On Database** yan TÜMCESI [alter Event SESSION](/sql/t-sql/statements/alter-event-session-transact-sql) ve [Drop Event Session](/sql/t-sql/statements/drop-event-session-transact-sql) Transact-SQL komutları için de geçerlidir.

- En iyi uygulama, **oluşturma olay** oturumunuz veya **alter Event session** DEYIMLERINIZIN **startup_state = üzerinde** olay oturumu seçeneğini içerkullanmaktır.
  - **= On** değeri, yük devretme nedeniyle mantıksal veritabanının yeniden yapılandırması sonrasında otomatik yeniden başlatmayı destekler.

## <a name="new-catalog-views"></a>Yeni Katalog görünümleri

Genişletilmiş olaylar özelliği, çeşitli [Katalog görünümleri](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql)tarafından desteklenir. Katalog görünümleri, geçerli veritabanında kullanıcı tarafından oluşturulan olay oturumlarının *meta verilerini veya tanımlarını* size bildirir. Görünümler, etkin olay oturumlarının örnekleri hakkında bilgi döndürmez.

| Adı<br/>Katalog görünümü | Description |
|:--- |:--- |
| **sys.database_event_session_actions** |Olay oturumunun her bir olayında her eylem için bir satır döndürür. |
| **sys.database_event_session_events** |Olay oturumundaki her olay için bir satır döndürür. |
| **sys.database_event_session_fields** |Olaylar ve hedeflerde açıkça ayarlanmış olan her özelleştirme özellikli sütun için bir satır döndürür. |
| **sys.database_event_session_targets** |Olay oturumu için her olay hedefi için bir satır döndürür. |
| **sys.database_event_sessions** |Veritabanındaki her olay oturumu için bir satır döndürür. |

Microsoft SQL Server, benzer katalog görünümlerinde. *\_ Database* yerine *. Server \_* adı yer alır. Ad deseninin **sys.server_event_%** olduğu görülüyor.

## <a name="new-dynamic-management-views-dmvs"></a>Yeni dinamik yönetim görünümleri [(DMVs)](/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)

Azure SQL veritabanı, genişletilmiş olayları destekleyen [dinamik yönetim görünümlerine (DMVs)](/sql/relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views) sahiptir. DMVs, *etkin* olay oturumları hakkında bilgi ister.

| DMV adı | Description |
|:--- |:--- |
| **sys.dm_xe_database_session_event_actions** |Olay oturumu eylemleri hakkında bilgi döndürür. |
| **sys.dm_xe_database_session_events** |Oturum olayları hakkında bilgi döndürür. |
| **sys.dm_xe_database_session_object_columns** |Bir oturuma bağlanan nesneler için yapılandırma değerlerini gösterir. |
| **sys.dm_xe_database_session_targets** |Oturum hedefleri hakkında bilgi döndürür. |
| **sys.dm_xe_database_sessions** |Geçerli veritabanı kapsamındaki her olay oturumu için bir satır döndürür. |

Microsoft SQL Server, benzer Katalog görünümleri adın *\_ veritabanı* bölümü olmadan adlandırılır, örneğin:

- ad yerine **sys.dm_xe_sessions**<br/>**sys.dm_xe_database_sessions**.

### <a name="dmvs-common-to-both"></a>Her ikisine de genel olarak DMVs

Genişletilmiş olaylar için Azure SQL veritabanı, Azure SQL yönetilen örneği ve Microsoft SQL Server için ortak olan ek DMVs 'ler vardır:

- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**

<a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Kullanılabilir genişletilmiş olayları, eylemleri ve hedefleri bulun

Kullanılabilir olaylar, Eylemler ve hedefin bir listesini almak için basit bir SQL **seçimi** çalıştırabilirsiniz.

```sql
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```

<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;

## <a name="targets-for-your-azure-sql-database-event-sessions"></a>Azure SQL veritabanı olay oturumlarınız için hedefler

Azure SQL veritabanı 'nda olay oturumlarınızdan sonuçları yakalayabileceğiniz hedefler şunlardır:

- [Halka arabelleği hedefi](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)) -olay verilerini kısaca bellekte tutar.
- [Olay sayacı hedefi](/previous-versions/sql/sql-server-2016/ff878025(v=sql.130)) -bir genişletilmiş olaylar oturumu sırasında oluşan tüm olayları sayar.
- [Olay dosyası hedefi](/previous-versions/sql/sql-server-2016/ff878115(v=sql.130)) -bir Azure depolama kapsayıcısına tam arabellekleri yazar.

[Windows Için olay izleme (ETW)](/dotnet/framework/wcf/samples/etw-tracing) API 'Si, Azure SQL veritabanı 'nda genişletilmiş olaylar için kullanılamaz.

## <a name="restrictions"></a>Kısıtlamalar

Azure SQL veritabanı 'nın bulut ortamına yönelik olarak güvenlikle ilgili birkaç fark vardır:

- Genişletilmiş olaylar, tek kiracılı yalıtım modelinde kurulmuştur. Bir veritabanındaki olay oturumu başka bir veritabanından veriye veya olaylara erişemez.
- **Ana** veritabanı BAĞLAMıNDA bir **olay oluştur oturum** bildirisi veremez.

## <a name="permission-model"></a>İzin modeli

**Olay oluştur oturumu oluşturmak** Için veritabanında **Denetim** izninizin olması gerekir. Veritabanı sahibi (dbo) **Denetim** iznine sahiptir.

### <a name="storage-container-authorizations"></a>Depolama kapsayıcısı yetkilendirmeleri

Azure depolama Kapsayıcınız için oluşturduğunuz SAS belirtecinin izinleri için **RwL** belirtmesi gerekir. **RwL** değeri aşağıdaki izinleri sağlar:

- Okuma
- Yazma
- Liste

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Genişletilmiş olayların yoğun kullanımı, genel sistem için sağlıklı olandan daha fazla etkin bellek biriktirilebileceği senaryolar vardır. Bu nedenle Azure SQL veritabanı, bir olay oturumu tarafından birikmiş etkin bellek miktarına göre limitleri dinamik olarak ayarlar ve ayarlar. Birçok etken dinamik hesaplamaya gider.

Bellek üst sınırının zorlandığını belirten bir hata iletisi alırsanız, gerçekleştirebileceğiniz bazı düzeltme işlemleri şunlardır:

- Daha az eşzamanlı olay oturumu çalıştırın.
- Olay oturumlarına yönelik **oluşturma** ve **değiştirme** deyimlerinizi kullanarak, **maksimum \_ bellek** yan tümcesinde belirttiğiniz bellek miktarını azaltın.

### <a name="network-latency"></a>Ağ gecikmesi

**Olay dosyası** hedefi, verileri Azure depolama Blobları ile kalıcı hale getirmeyi sağlarken ağ gecikme süresi veya hata yaşayabilir. Azure SQL veritabanı 'ndaki diğer olaylar, ağ iletişiminin tamamlanmasını beklerken gecikebilir. Bu gecikme, iş yükünüzü yavaşlatabilir.

- Bu performans riskini azaltmak için, **EVENT_RETENTION_MODE** seçeneğini olay oturumu tanımlarınızda **NO_EVENT_LOSS** olarak ayarlamaktan kaçının.

## <a name="related-links"></a>İlgili bağlantılar

- [Azure depolama ile Azure PowerShell kullanma](/powershell/module/az.storage/).
- [Azure depolama cmdlet 'Leri](/powershell/module/Azure.Storage)
- [Azure Storage ile Azure PowerShell’i kullanma](/powershell/module/az.storage/)
- [.NET 'ten blob depolamayı kullanma](../../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [CREATE CREDENTIAL (Transact-SQL)](/sql/t-sql/statements/create-credential-transact-sql)
- [Olay oturumu oluşturma (Transact-SQL)](/sql/t-sql/statements/create-event-session-transact-sql)
- [Microsoft SQL Server 'da genişletilmiş olaylar hakkında Kehayias ' blog gönderileri](https://www.sqlskills.com/blogs/jonathan/category/extended-events/)
- Azure *hizmet güncelleştirmeleri* Web sayfası, parametre tarafından Azure SQL veritabanı 'na göre daha dar:
  - [https://azure.microsoft.com/updates/?service=sql-database](https://azure.microsoft.com/updates/?service=sql-database)

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](/sql/relational-databases/extended-events/determine-which-queries-are-holding-locks)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](/sql/relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them)
-->