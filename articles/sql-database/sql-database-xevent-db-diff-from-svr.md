---
title: Genişletilmiş olaylar
description: Azure SQL Veritabanı'ndaki genişletilmiş olayları (XEvents) ve olay oturumlarının Microsoft SQL Server'daki olay oturumlarından nasıl biraz farklı olduğunu açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: jrasnik
ms.date: 12/19/2018
ms.openlocfilehash: cb4eb4474ad074a3e69dc146c97b48d54343595b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79213954"
---
# <a name="extended-events-in-sql-database"></a>SQL Veritabanı'nda genişletilmiş olaylar
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Bu konu, Azure SQL Veritabanı'nda genişletilmiş olayların uygulanmasının Microsoft SQL Server'daki genişletilmiş olaylarla karşılaştırıldığında nasıl biraz farklı olduğunu açıklar.

- SQL Database V12, 2015 takviminin ikinci yarısında genişletilmiş olaylar özelliğini kazandı.
- SQL Server 2008 yılından bu yana olayları genişletti.
- SQL Veritabanı'ndaki genişletilmiş olayların özellik kümesi, SQL Server'daki özelliklerin sağlam bir alt kümesidir.

*XEvents* bazen bloglar ve diğer gayri resmi yerlerde 'genişletilmiş olaylar' için kullanılan gayri resmi bir takma addır.

Azure SQL Veritabanı ve Microsoft SQL Server için genişletilmiş olaylar hakkında ek bilgiler şu anda mevcuttur:

- [Hızlı Başlangıç: SQL Server'da genişletilmiş olaylar](https://msdn.microsoft.com/library/mt733217.aspx)
- [Genişletilmiş Etkinlikler](https://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a>Ön koşullar

Bu konu zaten bazı bilgilere sahip varsayar:

- [Azure SQL Veritabanı hizmeti.](https://azure.microsoft.com/services/sql-database/)
- Microsoft SQL Server'da [genişletilmiş olaylar.](https://msdn.microsoft.com/library/bb630282.aspx)

- Genişletilmiş olaylarla ilgili belgelerimizin büyük bölümü hem SQL Server hem de SQL Database için geçerlidir.

Hedef olarak Olay Dosyası seçilirken aşağıdaki öğelere daha önce maruz kalmak [yararlıdır:](#AzureXEventsTargets)

- [Azure Depolama hizmeti](https://azure.microsoft.com/services/storage/)


- PowerShell
    - [Azure Depolama ile Azure PowerShell'i kullanma](../storage/common/storage-powershell-guide-full.md) - PowerShell ve Azure Depolama hizmeti hakkında kapsamlı bilgiler sağlar.

## <a name="code-samples"></a>Kod örnekleri

İlgili konular iki kod örneği sağlar:


- [SQL Veritabanı'nda genişletilmiş olaylar için Zil Arabelleği hedef kodu](sql-database-xevent-code-ring-buffer.md)
    - Kısa basit Transact-SQL komut dosyası.
    - Kod örneği konusunda, bir Zil Arabelleği hedefiyle yaptığınızda, bir alter-drop `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` deyimi çalıştırarak kaynaklarını serbest bırakmanız gerektiğini vurguluyoruz. Daha sonra tarafından Halka Buffer `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`başka bir örnek ekleyebilirsiniz.


- [SQL Veritabanı'ndaki genişletilmiş olaylar için Olay Dosyası hedef kodu](sql-database-xevent-code-event-file.md)
    - Aşama 1, Azure Depolama kapsayıcısı oluşturmak için PowerShell'dir.
    - Aşama 2, Azure Depolama kapsayıcısını kullanan Transact-SQL'dir.

## <a name="transact-sql-differences"></a>Transact-SQL farkları


- SQL Server'da [CREATE EVENT SESSION](https://msdn.microsoft.com/library/bb677289.aspx) komutunu çalıştırdığınızda, ON **SERVER** yan tümcesini kullanırsınız. Ancak SQL Database'te bunun yerine **ON DATABASE** yan tümcesini kullanırsınız.


- **ON DATABASE** yan tümcesi ALTER EVENT SESSION ve DROP [EVENT](https://msdn.microsoft.com/library/bb630368.aspx) [SESSION](https://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL komutları için de geçerlidir.


- En iyi yöntem, **CREATE EVENT SESSION** veya ALTER EVENT **SESSION** deyimlerinize STARTUP_STATE **= ON** olay oturumu seçeneğini eklemektir.
    - **= ON** değeri, mantıksal veritabanının bir hata nedeniyle yeniden yapılandırılmasından sonra otomatik yeniden başlatmayı destekler.

## <a name="new-catalog-views"></a>Yeni katalog görünümleri

Genişletilmiş olaylar özelliği çeşitli katalog [görünümleri](https://msdn.microsoft.com/library/ms174365.aspx)tarafından desteklenir. Katalog *görünümleri,* geçerli veritabanında kullanıcı tarafından oluşturulan olay oturumlarının meta verileri veya tanımları hakkında bilgi sağlar. Görünümler, etkin olay oturumları örnekleri hakkında bilgi döndürmez.

| Adı<br/>katalog görünümü | Açıklama |
|:--- |:--- |
| **sys.database_event_session_actions** |Bir olay oturumunun her olayı için bir satır döndürür. |
| **sys.database_event_session_events** |Olay oturumundaki her olay için bir satır döndürür. |
| **sys.database_event_session_fields** |Olaylar ve hedefler üzerinde açıkça ayarlanmış her özelleştirilebilir sütun için bir satır döndürür. |
| **sys.database_event_session_targets** |Olay oturumu için her olay hedefi için bir satır döndürür. |
| **sys.database_event_sessions** |SQL Veritabanı veritabanındaki her olay oturumu için bir satır döndürür. |

Microsoft SQL Server'da, benzer katalog görünümlerinde *.database\_* yerine *.server\_ * içeren adlar vardır. Ad deseni **sys.server_event_%** gibidir.

## <a name="new-dynamic-management-views-dmvs"></a>Yeni dinamik yönetim görünümleri [(DMV' ler)](https://msdn.microsoft.com/library/ms188754.aspx)

Azure SQL Veritabanı, genişletilmiş olayları destekleyen [dinamik yönetim görünümleri (DMV'ler)](https://msdn.microsoft.com/library/bb677293.aspx) vardır. DMVs *etkin* olay oturumları hakkında size.

| DMV adı | Açıklama |
|:--- |:--- |
| **sys.dm_xe_database_session_event_actions** |Olay oturumu eylemleri hakkında bilgi verir. |
| **sys.dm_xe_database_session_events** |Oturum olayları hakkında bilgi verir. |
| **sys.dm_xe_database_session_object_columns** |Oturuma bağlı nesnelerin yapılandırma değerlerini gösterir. |
| **sys.dm_xe_database_session_targets** |Oturum hedefleri hakkında bilgi verir. |
| **sys.dm_xe_database_sessions** |Geçerli veritabanına kapsamlı her olay oturumu için bir satır döndürür. |

Microsoft SQL Server'da, benzer katalog görünümleri, adın * \_veritabanı* bölümü olmadan adlandırılır:

- **sys.dm_xe_sessions**, isim yerine<br/>**sys.dm_xe_database_sessions**.

### <a name="dmvs-common-to-both"></a>Her ikisi için de yaygın olan DMV'ler
Genişletilmiş olaylar için hem Azure SQL Veritabanı hem de Microsoft SQL Server'da ortak olan ek DMV'ler vardır:

- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**

  <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Kullanılabilir genişletilmiş olayları, eylemleri ve hedefleri bulma

Kullanılabilir olayların, eylemlerin ve hedefin listesini almak için basit bir SQL **SELECT** çalıştırabilirsiniz.

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

## <a name="targets-for-your-sql-database-event-sessions"></a>SQL Veritabanı etkinlik oturumlarınız için hedefler

SQL Veritabanı'ndaki etkinlik oturumlarınızdan sonuçları yakalayabilen hedefler şunlardır:

- [Zil Arabelleği hedefi](https://msdn.microsoft.com/library/ff878182.aspx) - Olay verilerini kısaca bellekte tutar.
- [Olay Sayacı hedefi](https://msdn.microsoft.com/library/ff878025.aspx) - Genişletilmiş olaylar oturumu sırasında meydana gelen tüm olayları sayar.
- [Olay Dosyası hedefi](https://msdn.microsoft.com/library/ff878115.aspx) - Azure Depolama kapsayıcısına tam arabellek yazar.

[Windows (ETW) API'si için Olay İzleme,](https://msdn.microsoft.com/library/ms751538.aspx) SQL Veritabanı'ndaki genişletilmiş olaylar için kullanılamaz.

## <a name="restrictions"></a>Kısıtlamalar

SQL Veritabanı'nın bulut ortamına yakışır güvenlikle ilgili birkaç fark vardır:

- Genişletilmiş olaylar tek kiracılı yalıtım modeli üzerine kurulur. Bir veritabanındaki olay oturumu başka bir veritabanından veri veya olaylara erişemez.
- **Ana** veritabanı bağlamında **CREATE EVENT SESSION** deyimi düzenleyemezsiniz.

## <a name="permission-model"></a>İzin modeli

**CREATE EVENT SESSION** deyimi ni vermek için veritabanında **Denetim** iznine sahip olmalısınız. Veritabanı sahibi (dbo) **Denetim** izni vardır.

### <a name="storage-container-authorizations"></a>Depolama konteyneri yetkilendirmeleri

Azure Depolama kapsayıcınız için oluşturduğunuz SAS belirteci, izinler için **rwl** belirtmelidir. **Rwl** değeri aşağıdaki izinleri sağlar:

- Okuma
- Yazma
- Liste

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

Genişletilmiş olayların yoğun kullanımının genel sistem için sağlıklı olandan daha fazla etkin bellek birikebileceği senaryolar vardır. Bu nedenle Azure SQL Veritabanı sistemi, bir olay oturumu tarafından birikebilecek etkin bellek miktarıüzerindeki sınırları dinamik olarak ayarlar ve ayarlar. Birçok faktör dinamik hesaplama gidin.

Bellek en büyük lerinin zorlandığını belirten bir hata iletisi alırsanız, yapabileceğiniz bazı düzeltici eylemler şunlardır:

- Daha az eşzamanlı olay oturumu çalıştırın.
- Olay oturumları için **CREATE** ve **ALTER** deyimleriniz **aracılığıyla, MAX\_MEMORY** yan tümcesi üzerinde belirttiğiniz bellek miktarını azaltın.

### <a name="network-latency"></a>Ağ gecikmesi

**Olay Dosyası** hedefi, verileri Azure Depolama blobs'larında kalıcı olarak sağlarken ağ gecikmesi veya hatalarıyla karşılaşabilir. SQL Veritabanı'ndaki diğer olaylar, ağ iletişiminin tamamlanmasını beklerken gecikebilir. Bu gecikme iş yükünüzü yavaşlatabilir.

- Bu performans riskini azaltmak için, olay oturumu tanımlarınızda **EVENT_RETENTION_MODE** seçeneğini **NO_EVENT_LOSS** ayarlamaktan kaçının.

## <a name="related-links"></a>İlgili bağlantılar

- [Azure Depolama ile Azure PowerShell'i kullanma.](../storage/common/storage-powershell-guide-full.md)
- [Azure Depolama Cmdlets](https://docs.microsoft.com/powershell/module/Azure.Storage)
- [Azure Depolama ile Azure PowerShell'i kullanma](../storage/common/storage-powershell-guide-full.md) - PowerShell ve Azure Depolama hizmeti hakkında kapsamlı bilgiler sağlar.
- [.NET'ten Blob depolama nasıl kullanılır?](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [CREATE CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/ms189522.aspx)
- [EtkİnLİk OTURUMU OLUŞTURMA (Transact-SQL)](https://msdn.microsoft.com/library/bb677289.aspx)
- [Jonathan Kehayias'ın Microsoft SQL Server'daki genişletilmiş olaylarla ilgili blog gönderileri](https://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- Azure *Hizmet Güncelleştirmeleri* web sayfası, parametreye göre Azure SQL Veritabanı'na daraltılmış:
    - [https://azure.microsoft.com/updates/?service=sql-database](https://azure.microsoft.com/updates/?service=sql-database)


Genişletilmiş olaylar için diğer kod örnek konuları aşağıdaki bağlantılarda mevcuttur. Ancak, örneğin Microsoft SQL Server ile Azure SQL Veritabanı'nı hedefleyip hedeflemediğini görmek için herhangi bir örneği düzenli olarak denetlemeniz gerekir. Ardından, örneği çalıştırmak için küçük değişiklikler gerekip gerekmediğine karar verebilirsiniz.

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](https://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](https://msdn.microsoft.com/library/bb630355.aspx)
-->
