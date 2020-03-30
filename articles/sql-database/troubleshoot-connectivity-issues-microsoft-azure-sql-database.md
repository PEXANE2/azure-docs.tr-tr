---
title: SQL veritabanına sık sık bağlantı sorunlarını giderme
description: Azure SQL Veritabanı bağlantı sorunlarını gidermek ve diğer SQL Veritabanı özel sorunlarını gidermek için adımlar sağlar
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
ms.custom: seo-lt-2019, OKR 11/2019
author: ramakoni1
ms.author: ramakoni
ms.reviewer: carlrab,vanto
ms.date: 01/14/2020
ms.openlocfilehash: 578d076dfc2fc383051c0c3a8528cdbb0fbcdb15
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79208775"
---
# <a name="troubleshooting-connectivity-issues-and-other-errors-with-microsoft-azure-sql-database"></a>Microsoft Azure SQL Veritabanı ile ilgili bağlantı sorunlarını ve diğer hataları giderme

Azure SQL Veritabanına bağlantı başarısız olduğunda hata iletileri alırsınız. Bu bağlantı sorunlarına, Azure SQL Veritabanı yeniden yapılandırması, güvenlik duvarı ayarları, bağlantı zaman sonu, yanlış oturum açma bilgileri veya [uygulama tasarım](sql-database-develop-overview.md) işlemi sırasında en iyi uygulamaların ve tasarım yönergelerinin uygulanamaması neden olabilir. Ayrıca, bazı Azure SQL Veritabanı kaynaklarında maksimum sınıra ulaşılırsa, Azure SQL Veritabanı'na bağlanamazsınız.

## <a name="transient-fault-error-messages-40197-40613-and-others"></a>Geçici hata hatası iletileri (40197, 40613 ve diğerleri)

Azure altyapısının SQL Veritabanı hizmetinde ağır iş yükleri ortaya çıktığında sunucuları dinamik olarak yeniden yapılandırabilme özelliği vardır.  Bu dinamik davranış istemci programınızın SQL Veritabanı bağlantısını kaybetmesine neden olabilir. Bu tür hata durumuna *geçici hata*denir. Veritabanı yeniden yapılandırma olayları, planlanmış bir olay (örneğin, yazılım yükseltmesi) veya planlanmamış bir olay (örneğin, bir işlem çökmesi veya yük dengelemesi) nedeniyle oluşur. Çoğu yeniden yapılandırma olayı genellikle kısa ömürlüdür ve en fazla 60 saniye içinde tamamlanmalıdır. Ancak, büyük bir işlemin uzun süren bir kurtarma işlemine neden olması gibi, bu olayların bazen tamamlanması daha uzun sürebilir. Aşağıdaki tabloda, uygulamaların SQL Veritabanı'na bağlanırken alabilecekleri çeşitli geçici hatalar listelenebilir

### <a name="list-of-transient-fault-error-codes"></a>Geçici hata hata kodları listesi


| Hata kodu | Severity | Açıklama |
| ---:| ---:|:--- |
| 4060 |16 |Giriş tarafından istenen "%.&#x2a;ls" veritabanını açamazsınız. Giriş başarısız oldu. Daha fazla bilgi için bkz: [Hatalar 4000 - 4999](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-4000-to-4999)|
| 40197 |17 |Hizmet isteğinizi işleme bir hata yla karşılaştı. Lütfen tekrar deneyin. Hata kodu %d.<br/><br/>Bu hatayı, yazılım veya donanım yükseltmeleri, donanım hataları veya diğer hatalar nedeniyle hizmet çöktüğinde alırsınız. Hata iletisi 40197'nin içine katıştırılmış hata kodu (%d), oluşan hata veya hata türü hakkında ek bilgiler sağlar. Hata kodlarının bazı örnekleri 40020, 40143, 40166 ve 40540 hata iletisi içinde gömülür.<br/><br/>SQL Veritabanı sunucunuza yeniden bağlanmak sizi veritabanınızın sağlıklı bir kopyasına otomatik olarak bağlar. Uygulamanız hata 40197'yi yakalamalı, sorun giderme iletisi içinde gömülü hata kodunu (%d) kaydetmeli ve kaynaklar kullanılabilir olana ve bağlantınız yeniden kurulana kadar SQL Veritabanı'na yeniden bağlanmayı denemelidir. Daha fazla bilgi için [Geçici hatalara](sql-database-connectivity-issues.md#transient-errors-transient-faults)bakın.|
| 40501 |20 |Hizmet şu an meşgul. 10 saniye sonra isteği yeniden deneyin. Olay Kimliği: %ls. Kod: %d. Daha fazla bilgi için bkz. <br/>&bull;&nbsp; [Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull;&nbsp; [Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md)<br/>&bull;&nbsp; [Elastik havuzlar için DTU bazlı limitler](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [tek veritabanları için vCore tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md)|
| 40613 |17 |Sunucudaki '%.&#x2a;ls' veritabanı '%.&#x2a;ls' şu anda kullanılamıyor. Lütfen bağlantıyı daha sonra yeniden deneyin. Sorun devam ederse, müşteri desteğine başvurun ve onlara '%.&#x2a;ls' kimliğini takip eden oturumu sağlayın.<br/><br/> Veritabanına zaten ayrılmış bir yönetici bağlantısı (DAC) varsa, bu hata oluşabilir. Daha fazla bilgi için [Geçici hatalara](sql-database-connectivity-issues.md#transient-errors-transient-faults)bakın.|
| 49918 |16 |İsteği işlenemiyor. İsteği işlemek için yeterli kaynak yok.<br/><br/>Hizmet şu an meşgul. Lütfen isteği daha sonra yeniden deneyin. Daha fazla bilgi için bkz. <br/>&bull;&nbsp; [Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull;&nbsp; [Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md)<br/>&bull;&nbsp; [Elastik havuzlar için DTU bazlı limitler](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [tek veritabanları için vCore tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) |
| 49919 |16 |Oluşturma veya güncelleştirme isteği işleyemez. Abonelik için devam etmekte olan çok fazla oluşturma veya güncelleştirme işlemi "%ld".<br/><br/>Hizmet, aboneliğiniz veya sunucunuz için birden çok oluşturma veya güncelleştirme isteklerini işlemekle meşgul. Kaynak optimizasyonu için istekler şu anda engellenir. Bekleyen işlemler için [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) sorgula. Bekleyen oluşturma veya güncelleştirme istekleri tamamlanana kadar bekleyin veya bekleyen isteklerinizden birini silin ve isteğinizi daha sonra yeniden deneyin. Daha fazla bilgi için bkz. <br/>&bull;&nbsp; [Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull;&nbsp; [Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md)<br/>&bull;&nbsp; [Elastik havuzlar için DTU bazlı limitler](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [tek veritabanları için vCore tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) |
| 49920 |16 |İsteği işlenemiyor. Abonelik için çok fazla işlem "%ld".<br/><br/>Hizmet, bu abonelik için birden çok isteği işlemekle meşgul. Kaynak optimizasyonu için istekler şu anda engellenir. [SYS.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) sorgula işlem durumu için. Bekleyen istekler tamamlanana kadar bekleyin veya bekleyen isteklerinizden birini silin ve isteğinizi daha sonra yeniden deneyin. Daha fazla bilgi için bkz. <br/>&bull;&nbsp; [Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull;&nbsp; [Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md)<br/>&bull;&nbsp; [Elastik havuzlar için DTU bazlı limitler](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [tek veritabanları için vCore tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) |
| 4221 |16 |'HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING' uzun bekleme nedeniyle okuma-ikincil giriş başarısız oldu. Satır sürümleri, yineleme geri dönüştürüldüğünde uçuş sırasında olan işlemler için eksik olduğundan, yineleme oturum açmak için kullanılamaz. Sorun, birincil yinelemede etkin hareketleri geri alarak veya işleyerek çözülebilir. Birincil uzun yazma işlemleri kaçınarak bu durumun oluşumları en aza indirilebilir. |


### <a name="steps-to-resolve-transient-connectivity-issues"></a>Geçici bağlantı sorunlarını çözme adımları

1. Hataların uygulama tarafından raporlandığı süre içinde meydana gelen bilinen kesintiler için [Microsoft Azure Hizmet Panosu'nu](https://azure.microsoft.com/status) denetleyin.
2. Azure SQL Veritabanı gibi bir bulut hizmetine bağlanan uygulamalar, periyodik yeniden yapılandırma olayları beklemelidir ve kullanıcılara uygulama hatası olarak bunları yüzeye çıkarmak yerine bu hataları işlemek için yeniden deneme mantığını uygulamalıdır. 
3. Bir veritabanı kaynak sınırlarına yaklaştıkça, geçici bir bağlantı sorunu gibi görünebilir. [Bkz. Kaynak sınırları.](sql-database-resource-limits-database-server.md#what-happens-when-database-resource-limits-are-reached)
4. Bağlantı sorunları devam ederse veya uygulamanızın hatayla karşılaşma süresi 60 saniyeyi aşarsa veya belirli bir günde birden fazla hata oluşumu nu görürseniz, Azure Destek sitesinde **Destek Al'ı** seçerek bir [Azure](https://azure.microsoft.com/support/options) destek isteği dosyalayın.

#### <a name="implementing-retry-logic"></a>Yeniden Deneme Mantığının Uygulanması
İstemci programınızın, kendisini düzeltmek için geçici hata süresi verdikten sonra bir bağlantıyı yeniden kurabilmek için yeniden deneme mantığına sahip olması önerilir.  İlk denemenizden önce 5 saniye ertelemenizi öneririz. 5 saniyeden kısa bir gecikmeden sonra yeniden deneme, bulut hizmetini ezme riskitaşır. Sonraki her yeniden deneme için gecikme katlanarak, en fazla 60 saniye ye kadar artmalıdır.

Yeniden deneme mantığının kod örnekleri için bkz:
- [ADO.NET ile SQL'e esnek bir şekilde bağlanın](https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net)
- [PHP ile SQL'e esnek bir şekilde bağlanın](https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)

Uygulama incelemenizde geçici hataların işlenmesi hakkında daha fazla bilgi için
* [SQL Veritabanı'nda geçici bağlantı hatalarını giderme sorunu](sql-database-connectivity-issues.md)

[sql Server Connection Pooling 'de (ADO.NET)](https://msdn.microsoft.com/library/8xx3tyca.aspx)ADO.NET kullanan istemciler için *engelleme süresitartışması* mevcuttur.

## <a name="a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-database-server"></a>SQL Veritabanı sunucusuna bağlantı kurarken ağla ilgili veya örne özgü bir hata oluştu

Uygulama sunucuya bağlanamazsa sorun oluşur.

Bu sorunu gidermek [için, sık karşılaşılan bağlantı sorunları](#steps-to-fix-common-connection-issues) bölümünde Adımlar'daki adımları (verilen sırada) deneyin.

## <a name="the-serverinstance-was-not-found-or-was-not-accessible-errors-26-40-10053"></a>Sunucu/örnek bulunamadı veya erişilemedi (hatalar 26, 40, 10053)

#### <a name="error-26-error-locating-server-specified"></a>Hata 26: Hata Bulma sunucusu belirtilen

``System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.(provider: SQL Network Interfaces, error: 26 – Error Locating Server/Instance Specified)``

#### <a name="error-40-could-not-open-a-connection-to-the-server"></a>Hata 40: Sunucuya bağlantı açılmıyor

``A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)``

#### <a name="error-10053-a-transport-level-error-has-occurred-when-receiving-results-from-the-server"></a>Hata 10053: Sunucudan sonuç alırken aktarım düzeyinde bir hata oluştu

``10053: A transport-level error has occurred when receiving results from the server. (Provider: TCP Provider, error: 0 - An established connection was aborted by the software in your host machine)``

Uygulama sunucuya bağlanamazsa bu sorunlar oluşur.

Bu sorunları gidermek için, [sık karşılaşılan bağlantı sorunları](#steps-to-fix-common-connection-issues) bölümünde Adımlar'daki adımları (verilen sırada) deneyin.

## <a name="cannot-connect-to-server-due-to-firewall-issues"></a>Güvenlik duvarı sorunları nedeniyle sunucuya bağlanamıyor

### <a name="error-40615-cannot-connect-to--servername-"></a>Hata 40615: < servername > bağlanamıyor

Bu sorunu gidermek için, [Azure portalı aracılığıyla SQL Veritabanı'ndaki güvenlik duvarı ayarlarını yapılandırın.](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)

### <a name="error-5-cannot-connect-to--servername-"></a>Hata 5: < sunucu adı > bağlanamıyor

Bu sorunu gidermek için, bağlantı noktası 1433 istemci ve internet arasındaki tüm güvenlik duvarlarında giden bağlantılar için açık olduğundan emin olun.

Daha fazla bilgi için, [SQL Server erişimine izin vermek için Windows Güvenlik Duvarını Yapılandır'a](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)bakın.

## <a name="unable-to-log-in-to-the-server-errors-18456-40531"></a>Sunucuda oturum açamıyor (hatalar 18456, 40531)

### <a name="login-failed-for-user--user-name-"></a>Kullanıcı '< Kullanıcı adı >' için giriş başarısız oldu

``Login failed for user '<User name>'.This session has been assigned a tracing ID of '<Tracing ID>'. Provide this tracing ID to customer support when you need assistance. (Microsoft SQL Server, Error: 18456)``

Bu sorunu çözmek için, geçerli bir SQL Server kullanıcı adı ve parolasağlamak için servis yöneticinize başvurun.

Genellikle, hizmet yöneticisi oturum açma kimlik bilgilerini eklemek için aşağıdaki adımları kullanabilir:

1. SQL Server Management Studio (SSMS) kullanarak sunucuya giriş yapın.
2. Oturum açma adının devre dışı bırakıldığını kontrol etmek için aşağıdaki SQL sorgusunu çalıştırın:

   ```sql
   SELECT name, is_disabled FROM sys.sql_logins
   ```

3. Karşılık gelen ad devre dışıysa, aşağıdaki deyimi kullanarak etkinleştirin:

   ```sql
   Alter login <User name> enable
   ```

4. SQL oturum açma kullanıcı adı yoksa, aşağıdaki adımları izleyerek oluşturun:

   1. SSMS'te, genişletmek için **Güvenlik'e** çift tıklayın.
   2. **Oturumlar**’a sağ tıklayıp **Yeni oturum açma**’yı seçin.
   3. Yer tutucularla oluşturulan komut dosyasında, aşağıdaki SQL sorgusunu edin ve çalıştırın:

   ```sql
   CREATE LOGIN <SQL_login_name, sysname, login_name>
   WITH PASSWORD = ‘<password, sysname, Change_Password>’
   GO
   ```

5. **Veritabanı**’na çift tıklayın.
6. Kullanıcıya izin vermek istediğiniz veritabanını seçin.
7. **Güvenlik**’e çift tıklayın.
8. **Kullanıcılar**’a sağ tıklayıp **Yeni Kullanıcı**’yı seçin.
9. Yer tutucularla oluşturulan komut dosyasında, aşağıdaki SQL sorgusunu edin ve çalıştırın:

   ```sql
   CREATE USER <user_name, sysname, user_name>
   FOR LOGIN <login_name, sysname, login_name>
   WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
   GO
   -- Add user to the database owner role

   EXEC sp_addrolemember N’db_owner’, N’<user_name, sysname, user_name>’
   GO
   ```

   > [!NOTE]
   > Belirli kullanıcıları `sp_addrolemember` belirli veritabanı rolleriyle eşlemek için de kullanabilirsiniz.

Daha fazla bilgi için bkz: [Azure SQL Veritabanı'nda veritabanlarını ve girişleri yönetme.](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins)

## <a name="connection-timeout-expired-errors"></a>Bağlantı zaman aşımı süresi dolmuş hatalar

### <a name="systemdatasqlclientsqlexception-0x80131904-connection-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904): Bağlantı Süresi Dolmuş

``System.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed while attempting to consume the pre-login handshake acknowledgement. This could be because the pre-login handshake failed or the server was unable to respond back in time. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=3; handshake=29995;``

### <a name="systemdatasqlclientsqlexception-0x80131904-timeout-expired"></a>System.Data.SqlClient.SqlException (0x80131904): Zaman aşımı süresi doldu

``System.Data.SqlClient.SqlException (0x80131904): Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding.``

### <a name="systemdataentitycoreentityexception-the-underlying-provider-failed-on-open"></a>System.Data.Entity.Core.EntityException: Temel sağlayıcı Açık'ta başarısız oldu

``System.Data.Entity.Core.EntityException: The underlying provider failed on Open. -> System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. -> System.ComponentModel.Win32Exception: The wait operation timed out``

### <a name="cannot-connect-to--server-name-"></a>< sunucu adı > bağlanamıyor

``Cannot connect to <server name>.ADDITIONAL INFORMATION:Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=231; handshake=983; [Login] initialization=0; authentication=0; [Post-Login] complete=13000; (Microsoft SQL Server, Error: -2) For help, click: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=-2&LinkId=20476 The wait operation timed out``

Bu özel durumlar, bağlantı veya sorgu sorunları nedeniyle oluşabilir. Bu hatanın bağlantı sorunlarından kaynaklandığını doğrulamak için [bkz.](#confirm-whether-an-error-is-caused-by-a-connectivity-issue)

Uygulama sunucuya bağlanamadığından bağlantı zaman zaman ları oluşur. Bu sorunu gidermek [için, sık karşılaşılan bağlantı sorunları](#steps-to-fix-common-connection-issues) bölümünde Adımlar'daki adımları (verilen sırada) deneyin.

## <a name="resource-governance-errors"></a>Kaynak yönetim hataları

### <a name="error-10928-resource-id-d"></a>Hata 10928: Kaynak Kimliği: %d

``10928: Resource ID: %d. The %s limit for the database is %d and has been reached. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. The Resource ID value in error message indicates the resource for which limit has been reached. For sessions, Resource ID = 2.``

Bu sorunu çözmek için aşağıdaki yöntemlerden birini deneyin:

* Uzun süren sorgular olup olmadığını doğrulayın.

  > [!NOTE]
  > Bu, sorunu çözmeyen minimalist bir yaklaşımdır.

1. Engelleme isteklerini görmek için [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) görünümünü denetlemek için aşağıdaki SQL sorgusunu çalıştırın:

   ```sql
   SELECT * FROM dm_exec_requests
   ```

2. Kafa engelleyiciiçin **giriş arabelleği** belirleyin.
3. Kafa engelleyici sorgusunu ayarlayın.

   Ayrıntılı bir sorun giderme yordamı [için](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)bkz.

Veritabanı engelleme ve uzun süren sorguları ele rağmen sürekli olarak sınırına [ulaşırsa,](https://azure.microsoft.com/pricing/details/sql-database/)daha fazla kaynak Sürümleri ile bir baskıyükseltme düşünün).

Dinamik yönetim görünümleri hakkında daha fazla bilgi için [Sistem dinamik yönetim görünümleri'ne](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)bakın.

Veritabanı sınırları hakkında daha fazla bilgi için [Azure SQL Veritabanı sunucusu için SQL Veritabanı kaynak sınırlarına](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server)bakın.

### <a name="error-10929-resource-id-1"></a>Hata 10929: Kaynak Kimliği: 1

``10929: Resource ID: 1. The %s minimum guarantee is %d, maximum limit is %d and the current usage for the database is %d. However, the server is currently too busy to support requests greater than %d for this database. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. Otherwise, please try again later.``

### <a name="error-40501-the-service-is-currently-busy"></a>Hata 40501: Hizmet şu anda meşgul

``40501: The service is currently busy. Retry the request after 10 seconds. Incident ID: %ls. Code: %d.``

Bu bir motor azaltma hatasıdır, kaynak sınırlarının aşıldığının bir göstergesidir.

Kaynak sınırları hakkında daha fazla bilgi için [Veritabanı sunucusu kaynak sınırlarına](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server)bakın.

### <a name="error-40544-the-database-has-reached-its-size-quota"></a>Hata 40544: Veritabanı boyut kotasına ulaştı

``40544: The database has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions. Incident ID: <ID>. Code: <code>.``

Bu hata, veritabanı boyut kotasına ulaştığında oluşur.

Aşağıdaki adımlar, sorunu aşmanıza yardımcı olabilir veya ek seçenekler sağlayabilir:

1. Azure portalındaki panoyu kullanarak veritabanının geçerli boyutunu denetleyin.

   > [!NOTE]
   > Hangi tabloların en fazla alanı tükettiğini ve bu nedenle temizleme için potansiyel adaylar olduğunu belirlemek için aşağıdaki SQL sorgusunu çalıştırın:

   ```sql
   SELECT o.name,
    SUM(p.row_count) AS 'Row Count',
    SUM(p.reserved_page_count) * 8.0 / 1024 AS 'Table Size (MB)'
   FROM sys.objects o
   JOIN sys.dm_db_partition_stats p on p.object_id = o.object_id
   GROUP BY o.name
   ORDER BY [Table Size (MB)] DESC
   ```

2. Geçerli boyut, sürümünüz için desteklenen maksimum boyutu aşarsa, MAXSIZE ayarını artırmak için ALTER DATABASE'i kullanabilirsiniz.
3. Veritabanı, sürümünüz için desteklenen maksimum boyutu zaten geçmişse, aşağıdaki adımlardan birini veya birkaçını deneyin:

   * Normal veritabanı temizleme etkinliklerigerçekleştirin. Örneğin, istenmeyen verileri truncate/silme kullanarak temizleyin veya SQL Server Integration Services (SSIS) veya toplu kopyalama programı (bCP) yardımcı programını kullanarak verileri dışarı taşıyın.
   * Verileri bölme veya silme, dizinleri bırakma veya olası çözümler için belgelere başvurun.
   * Veritabanı ölçeklendirmesi için bkz: [Ölçektek veritabanı kaynakları](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-scale) ve [Ölçeke esnek havuz kaynakları.](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool-scale)

### <a name="error-40549-session-is-terminated-because-you-have-a-long-running-transaction"></a>Hata 40549: Uzun süren bir işleminiz olduğundan oturum sonlandırıldı

``40549: Session is terminated because you have a long-running transaction. Try shortening your transaction.``

Bu hatayla tekrar tekrar karşılaşırsanız, aşağıdaki adımları izleyerek sorunu çözmeye çalışın:

1. total_elapsed_time sütunu için yüksek değere sahip açık oturumları görmek için sys.dm_exec_requests görünümünü denetleyin. Aşağıdaki SQL komut dosyasını çalıştırarak bu denetimi gerçekleştirin:

   ```sql
   SELECT * FROM dm_exec_requests
   ```

2. Uzun süren sorgu için giriş arabelleğini belirleyin.
3. Sorguyu ayarlayın.

Ayrıca sorgularınızı toplu düşünün. Toplu iş hakkında bilgi için, [SQL Veritabanı uygulama performansını artırmak için toplu işleme nin nasıl kullanılacağına](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance)bakın.

Ayrıntılı bir sorun giderme yordamı [için](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)bkz.

### <a name="error-40551-the-session-has-been-terminated-because-of-excessive-tempdb-usage"></a>Hata 40551: Aşırı TEMPDB kullanımı nedeniyle oturum sonlandırıldı

``40551: The session has been terminated because of excessive TEMPDB usage. Try modifying your query to reduce the temporary table space usage.``

Bu sorunu çözmek için aşağıdaki adımları izleyin:

1. Geçici tablo alanı kullanımını azaltmak için sorguları değiştirin.
2. Geçici nesneleri artık ihtiyaç duyulmadıktan sonra bırakın.
3. Tabloları truncate veya kullanılmayan tabloları kaldırın.

### <a name="error-40552-the-session-has-been-terminated-because-of-excessive-transaction-log-space-usage"></a>Hata 40552: Oturum aşırı işlem günlüğü alanı kullanımı nedeniyle sonlandırıldı

``40552: The session has been terminated because of excessive transaction log space usage. Try modifying fewer rows in a single transaction.``

Bu sorunu çözmek için aşağıdaki yöntemleri deneyin:

* Ekleme, güncelleştirme veya silme işlemleri nedeniyle sorun oluşabilir.
Toplu işlemi uygulayarak veya birden çok küçük işleme bölünerek hemen çalıştırılan satır sayısını azaltmaya çalışın.
* Dizin yeniden oluşturma işlemleri nedeniyle sorun oluşabilir. Bu sorunu çözmek için tabloda * (bayt + 80 olarak güncelleştirilen alanın ortalama boyutu) < 2 gigabayt (GB) etkilenen satır ların sayısından emin olun.

  > [!NOTE]
  > Dizin yeniden oluşturma için, güncelleştirilen alanın ortalama boyutu ortalama dizin boyutuyla değiştirilmelidir.

### <a name="error-40553-the-session-has-been-terminated-because-of-excessive-memory-usage"></a>Hata 40553: Oturum aşırı bellek kullanımı nedeniyle sonlandırıldı

``40553 : The session has been terminated because of excessive memory usage. Try modifying your query to process fewer rows.``

Bu sorunu çözmek için sorguyu en iyi duruma getirmeye çalışın.

Ayrıntılı bir sorun giderme yordamı [için](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)bkz.

### <a name="table-of-additional-resource-governance-error-messages"></a>Ek kaynak yönetimi hata iletileri tablosu

| Hata kodu | Severity | Açıklama |
| ---:| ---:|:--- |
| 10928 |20 |Kaynak Kimliği: %d. Veritabanı için %s sınırı %d'dir ve ulaşılmıştır. Daha fazla bilgi için, [tek ve havuzlu veritabanları için SQL Veritabanı kaynak sınırlarına](sql-database-resource-limits-database-server.md)bakın.<br/><br/>Kaynak Kimliği, sınıra ulaşmış kaynağı gösterir. Alt iş parçacıkları için Kaynak Kimliği = 1. Oturumlar için Kaynak Kimliği = 2.<br/><br/>Bu hata ve nasıl çözüleceği hakkında daha fazla bilgi için bkz: <br/>&bull;&nbsp; [Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull;&nbsp; [Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md)<br/>&bull;&nbsp; [Elastik havuzlar için DTU bazlı limitler](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [tek veritabanları için vCore tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) |
| 10929 |20 |Kaynak Kimliği: %d. %s minimum garanti %d, maksimum limit %d ve veritabanı için geçerli kullanım %d'dir. Ancak, sunucu şu anda bu veritabanı için %d'den büyük istekleri ni desteklemek için çok meşgul. Kaynak Kimliği, sınıra ulaşmış kaynağı gösterir. Alt iş parçacıkları için Kaynak Kimliği = 1. Oturumlar için Kaynak Kimliği = 2. Daha fazla bilgi için bkz. <br/>&bull;&nbsp; [Veritabanı sunucusu kaynak sınırları](sql-database-resource-limits-database-server.md)<br/>&bull;&nbsp; [Tek veritabanları için DTU tabanlı sınırlar](sql-database-service-tiers-dtu.md)<br/>&bull;&nbsp; [Elastik havuzlar için DTU bazlı limitler](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [tek veritabanları için vCore tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md) <br/>Aksi takdirde, lütfen daha sonra tekrar deneyin. |
| 40544 |20 |Veritabanı boyut kotasına ulaştı. Verileri bölme veya silme, dizinleri bırakma veya olası çözümler için belgelere başvurun. Veritabanı ölçeklendirmesi için bkz: [Ölçektek veritabanı kaynakları](sql-database-single-database-scale.md) ve [Ölçeke esnek havuz kaynakları.](sql-database-elastic-pool-scale.md)|
| 40549 |16 |Uzun süren bir işleminuz olduğundan oturum sonlandırıldı. İşleminizi kısaltmayı deneyin. Toplu iş hakkında bilgi için, [SQL Veritabanı uygulama performansını artırmak için toplu işleme nin nasıl kullanılacağına](sql-database-use-batching-to-improve-performance.md)bakın.|
| 40550 |16 |Çok fazla kilit edindiği için oturum sonlandırıldı. Tek bir işlemde daha az satır okumayı veya değiştirmeyi deneyin. Toplu iş hakkında bilgi için, [SQL Veritabanı uygulama performansını artırmak için toplu işleme nin nasıl kullanılacağına](sql-database-use-batching-to-improve-performance.md)bakın.|
| 40551 |16 |Oturum aşırı `TEMPDB` kullanım nedeniyle sonlandırıldı. Geçici tablo alanı kullanımını azaltmak için sorgunuzu değiştirmeyi deneyin.<br/><br/>Geçici nesneler kullanıyorsanız, oturum tarafından artık `TEMPDB` gerekli olmayan geçici nesneleri bırakarak veritabanında yer tasarrufu yapın. SQL Veritabanı'nda tempdb kullanımı hakkında daha fazla bilgi için [SQL Veritabanı'ndaki Tempdb veritabanına](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database)bakın.|
| 40552 |16 |Oturum, aşırı işlem günlüğü alanı kullanımı nedeniyle sonlandırıldı. Tek bir işlemde daha az satır değiştirmeyi deneyin. Toplu iş hakkında bilgi için, [SQL Veritabanı uygulama performansını artırmak için toplu işleme nin nasıl kullanılacağına](sql-database-use-batching-to-improve-performance.md)bakın.<br/><br/>Yardımcı programı veya `System.Data.SqlClient.SqlBulkCopy` sınıfı kullanarak toplu ekler gerçekleştirirseniz, `-b batchsize` `BatchSize` her işlemde sunucuya kopyalanan satır sayısını sınırlamak için veya seçenekleri kullanmayı deneyin. `bcp.exe` `ALTER INDEX` Deyimle birlikte bir dizini yeniden oluşturuyorsanız, `REBUILD WITH ONLINE = ON` seçeneği kullanmayı deneyin. vCore satın alma modeli için işlem günlüğü boyutları hakkında bilgi için bkz: <br/>&bull;&nbsp; [tek veritabanları için vCore tabanlı sınırlar](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler](sql-database-vcore-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [Yönetilen örnek kaynak sınırları.](sql-database-managed-instance-resource-limits.md)|
| 40553 |16 |Oturum, aşırı bellek kullanımı nedeniyle sonlandırıldı. Sorgunuzu daha az satırı işlemek için değiştirmeyi deneyin.<br/><br/>Transact-SQL `ORDER BY` `GROUP BY` kodunuzdaki işlem ve sayısayısını azaltmak, sorgunuzun bellek gereksinimlerini azaltır. Veritabanı ölçeklendirmesi için bkz: [Ölçektek veritabanı kaynakları](sql-database-single-database-scale.md) ve [Ölçeke esnek havuz kaynakları.](sql-database-elastic-pool-scale.md)|

## <a name="elastic-pool-errors"></a>Elastik havuz hataları

Aşağıdaki hatalar elastik havuzlar oluşturma ve kullanma ile ilgilidir:

| Hata kodu | Severity | Açıklama | Düzeltici eylem |
|:--- |:--- |:--- |:--- |
| 1132 | 17 |Elastik havuz depolama sınırına ulaşmıştır. Elastik havuz için depolama kullanımı (%d) MB'leri geçemez. Elastik havuzun depolama sınırına ulaşıldığında veritabanına veri yazmaya çalışmak. Kaynak sınırları hakkında bilgi için bkz: <br/>&bull;&nbsp; [Elastik havuzlar için DTU bazlı limitler](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler.](sql-database-vcore-resource-limits-elastic-pools.md) <br/> |Depolama limitini artırmak, elastik havuz içindeki veritabanları tarafından kullanılan depolamayı azaltmak veya veritabanlarını elastik havuzdan kaldırmak için mümkünse elastik havuza DEPOLAMA DUS'larını artırmayı ve/veya depolama yı eklemeyi düşünün. Elastik havuz ölçekleme için [bkz.](sql-database-elastic-pool-scale.md)|
| 10929 | 16 |%s minimum garanti %d, maksimum limit %d ve veritabanı için geçerli kullanım %d'dir. Ancak, sunucu şu anda bu veritabanı için %d'den büyük istekleri ni desteklemek için çok meşgul. Kaynak sınırları hakkında bilgi için bkz: <br/>&bull;&nbsp; [Elastik havuzlar için DTU bazlı limitler](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull;&nbsp; [elastik havuzlar için vCore tabanlı limitler.](sql-database-vcore-resource-limits-elastic-pools.md) <br/> Aksi takdirde, lütfen daha sonra tekrar deneyin. DTU / vCore min/ veritabanı başına; DTU / vCore max veritabanı başına. Elastik havuzdaki tüm veritabanlarında eşzamanlı çalışan (istek) toplam sayısı havuz sınırını aşmaya çalıştı. |İşçi limitini artırmak veya veritabanlarını elastik havuzdan kaldırmak için mümkünse elastik havuzun DUS'larını veya vCore'larını artırmayı düşünün. |
| 40844 | 16 |Server '%ls'deki veritabanı '%ls' elastik bir havuzda bulunan '%ls' baskı veritabanıdır ve sürekli bir kopyalama ilişkisi olamaz.  |Yok |
| 40857 | 16 |Sunucu için elastik havuz bulunamadı: '%ls', elastik havuz adı: '%ls'. Belirtilen sunucuda belirtilen elastik havuz yok. | Geçerli bir elastik havuz adı sağlayın. |
| 40858 | 16 |Elastik havuz '%ls' zaten sunucuda var: '%ls'. Belirtilen SQL Veritabanı sunucusunda belirtilen elastik havuz zaten var. | Yeni elastik havuz adı sağlayın. |
| 40859 | 16 |Elastik havuz servis katmanı '%ls' desteklemez. Belirtilen servis katmanı elastik havuz sağlama için desteklenmez. |Varsayılan hizmet katmanını kullanmak için doğru sürümü sağlayın veya hizmet katmanını boş bırakın. |
| 40860 | 16 |Elastik havuz '%ls' ve servis hedefi '%ls' kombinasyonu geçersizdir. Elastik havuz ve servis katmanı, yalnızca kaynak türü 'ElasticPool' olarak belirtilmişse birlikte belirtilebilir. |Elastik havuz ve servis katmanının doğru birleşimini belirtin. |
| 40861 | 16 |Veritabanı sürümü '%. *ls' '%olan elastik havuz servis katmanından farklı olamaz.* Ls'in. Veritabanı sürümü, elastik havuz servis katmanından farklıdır. |Esnek havuz servis katmanından farklı bir veritabanı sürümü belirtmeyin.  Veritabanı sürümünün belirtilmesi gerekmediğini unutmayın. |
| 40862 | 16 |Elastik havuz servis amacı belirtilmişse elastik havuz adı belirtilmelidir. Elastik havuz servis amacı benzersiz bir elastik havuz tanımlamaz. |Elastik havuz hizmeti hedefini kullanıyorsanız, elastik havuz adını belirtin. |
| 40864 | 16 |Elastik havuz için DT'ler servis katmanı '%.*ls' için en az (%d) DT'ler olmalıdır. DUS'ları minimum sınırın altındaki elastik havuz için ayarlamaya çalışArak. |Elastik havuz için DSU'ları en az limite ayarlamayı yeniden deneyin. |
| 40865 | 16 |Elastik havuz için DT'ler servis katmanı '%.*ls' için DT'leri (%d) geçemez. DUS'ları maksimum sınırın üzerindeki elastik havuz için ayarlamaya çalışArak. |Elastik havuz için DSU'ları maksimum limitten büyük olmayacak şekilde ayarlamayı yeniden deneyin. |
| 40867 | 16 |Veri tabanı başına DTU maksimum hizmet katmanı '%.*ls' için en az (%d) olmalıdır. DTU'yu veritabanı başına en üst düzeye çıkarmaya çalışmak desteklenen sınırın altında. | İstenilen ayarı destekleyen elastik havuz servis katmanını kullanmayı düşünün. |
| 40868 | 16 |Veri tabanı başına DTU maksimum ugeçemez (d) hizmet katmanı için '%.*ls'. DTU'yu veritabanı başına en üst düzeye çıkarmaya çalışmak, desteklenen sınırın ötesinde. | İstenilen ayarı destekleyen elastik havuz servis katmanını kullanmayı düşünün. |
| 40870 | 16 |Veri tabanı başına DTU min '%.*ls' hizmet katmanı için (%d) aşamaz. DTU min'i veritabanı başına desteklenen sınırın ötesine ayarlamaya çalışılır. | İstenilen ayarı destekleyen elastik havuz servis katmanını kullanmayı düşünün. |
| 40873 | 16 |Veritabanı başına veri tabanı (%d) ve DTU min sayısı (%d) elastik havuzun DTU'larını (%d) geçemez. Elastik havuzun DTU'larını aşan elastik havuzdaki veritabanları için DTU min belirtmeye çalışılması. | Elastik havuzun DTU'larını artırmayı veya veritabanı başına DTU min'ini azaltmayı veya elastik havuzdaki veritabanlarının sayısını azaltmayı düşünün. |
| 40877 | 16 |Elastik bir havuz, veritabanları içermediği sürece silinemez. Elastik havuz bir veya daha fazla veritabanları içerir ve bu nedenle silinemez. |Veritabanlarını silmek için elastik havuzdan kaldırın. |
| 40881 | 16 |Elastik havuz '%.*ls' veritabanı sayısı sınırına ulaşmıştır.  Elastik havuz için veritabanı sayısı sınırı (%d) DTU'lu elastik bir havuz için (%d) geçemez. Elastik havuzun veritabanı sayısı sınırına ulaşıldığında, elastik havuza veritabanı oluşturmaya veya eklemeye çalışılması. | Veritabanı limitini artırmak veya veritabanlarını elastik havuzdan kaldırmak için mümkünse elastik havuzun DT'lerini artırmayı düşünün. |
| 40889 | 16 |Elastik havuz '%.*ls' için DTU veya depolama sınırı, veritabanları için yeterli depolama alanı sağlamadığından azaltılamaz. Elastik havuzun depolama limitini depolama kullanımının altına düşürmeye çalışmak. | DTUs'larını veya depolama limitini azaltmak için elastik havuzdaki tek tek veritabanlarının depolama kullanımını azaltmayı veya veritabanlarını havuzdan kaldırmayı düşünün. |
| 40891 | 16 |DTU min/database başına (%d) veritabanı başına DTU max'i (%d) geçemez. DTU min'i veritabanı başına DTU max'ten daha yüksek bir veritabanı başına ayarlamaya çalışArak. |Veritabanları başına DTU min'in veritabanı başına DTU maksimum sınırını aşmadığından emin olun. |
| TBD | 16 |Elastik bir havuzdaki tek bir veritabanının depolama boyutu ,'%.*ls' servis katmanı elastik havuzutarafından izin verilen maksimum boyutu aşamaz. Veritabanının maksimum boyutu, elastik havuz servis katmanının izin verdiği maksimum boyutu aşıyor. |Veritabanının maksimum boyutunu, elastik havuz servis katmanının izin verdiği maksimum boyut sınırları içinde ayarlayın. |

## <a name="cannot-open-database-master-requested-by-the-login-the-login-failed"></a>Giriş tarafından istenen veritabanı "master" açamazsınız. Oturum açma başarısız oldu

Hesabın ana veritabanına erişim izni olmadığından bu sorun oluşur. Ancak varsayılan olarak, SQL Server Management Studio (SSMS) ana veritabanına bağlanmaya çalışır.

Bu sorunu çözmek için şu adımları izleyin:

1. SSMS'in giriş ekranında **Seçenekler'i**seçin ve ardından **Bağlantı Özellikleri'ni**seçin.
2. **Veritabanına Bağlan** alanında, varsayılan giriş veritabanı olarak kullanıcının varsayılan veritabanı adını girin ve sonra **Bağlan'ı**seçin.

   ![Bağlantı özellikleri](media/troubleshoot-connectivity-issues-microsoft-azure-sql-database/cannot-open-database-master.png)

## <a name="confirm-whether-an-error-is-caused-by-a-connectivity-issue"></a>Bir hatanın bağlantı sorunundan kaynaklanıp kaynaklanıp kaynaklmadığını onaylama

Bir hatanın bir bağlantı sorunundan kaynaklanıp kaynaklanmadığını doğrulamak için, aşağıdakiler gibi bir bağlantı açmak için çağrılar gösteren çerçeveler için yığın izini gözden geçirin **(SqlConnection** sınıfına başvuruyu not edin):

```
System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
 at System.Data.SqlClient.SqlConnection.Open()
 at AzureConnectionTest.Program.Main(String[] args)
ClientConnectionId:<Client connection ID>
```

Özel durum sorgu sorunları tarafından tetiklendiğinde, aşağıdakine benzer bir çağrı yığını göreceksiniz **(SqlCommand** sınıfına başvuruyu not edin). Bu durumda, [sorgularınızı ayarlayın.](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx)

```
  at System.Data.SqlClient.SqlCommand.ExecuteReader()
  at AzureConnectionTest.Program.Main(String[] args)
  ClientConnectionId:<Client ID>
```

İnce ayar performansı yla ilgili ek kılavuz için aşağıdaki kaynaklara bakın:

* [Azure SQL dizinleri ve istatistikleri nasıl korunur?](https://techcommunity.microsoft.com/t5/Azure-Database-Support-Blog/How-to-maintain-Azure-SQL-Indexes-and-Statistics/ba-p/368787)
* [Azure SQL Veritabanı'nda manuel ayar sorgusu performansı](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
* [Dinamik yönetim görünümlerini kullanarak performansı izleme Azure SQL Veritabanı](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
* [Sorgu Deposunu Azure SQL Veritabanında Çalıştırma](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)

## <a name="steps-to-fix-common-connection-issues"></a>Yaygın bağlantı sorunlarını giderme adımları

1. Uygulama sunucusunda TCP/IP istemci protokolü olarak etkin olduğundan emin olun. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-client-protocols) SQL Server araçlarının yüklü olmadığı uygulama sunucularında, **cliconfg.exe** (SQL Server Client Network utility) çalıştırarak TCP/IP'nin etkinleştirildiğinden doğrulayın.
2. Doğru şekilde yapılandırıldığından emin olmak için uygulamanın bağlantı dizesini denetleyin. Örneğin, bağlantı dizesinin doğru bağlantı noktası (1433) ve tam nitelikli sunucu adını belirttiğinden emin olun.
Bkz. [SQL Server bağlantı bilgilerini alın.](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms#get-sql-server-connection-information)
3. Bağlantı zaman değerini artırmayı deneyin. En az 30 saniyelik bir bağlantı zaman dilimi kullanmanızı öneririz.
4. Bir UDL dosyası, ping veya telnet olan [SQL Server management Studio (SSMS)](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms)kullanarak uygulama sunucusu ile Azure SQL veritabanı arasındaki bağlantıyı test edin. Daha fazla bilgi için, bağlantı sorunları için [Sorun Giderme SQL Server bağlantı sorunları](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) ve [Tanılama'ya](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues#diagnostics)bakın.

   > [!NOTE]
   > Sorun giderme adımı olarak, farklı bir istemci bilgisayarda bağlantı test edebilirsiniz.

5. En iyi uygulama olarak, yeniden deneme mantığının yerinde olduğundan emin olun. Yeniden deneme mantığı hakkında daha fazla bilgi için, [SQL Veritabanı'na geçici hataları ve bağlantı hatalarını giderme sorununa](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues)bakın.

Bu adımlar sorununuzu çözmezse, daha fazla veri toplamaya çalışın ve ardından desteğe başvurun. Uygulamanız bir bulut hizmetiyse, günlüğe kaydetmeyi etkinleştirin. Bu adım, başarısızlığın UTC zaman damgasını döndürür. Ayrıca, SQL Azure izleme kimliğini döndürür. [Microsoft Müşteri Destek Hizmetleri](https://azure.microsoft.com/support/options/) bu bilgileri kullanabilir.

Günlüğe kaydetmeyi etkinleştirme hakkında daha fazla bilgi için Azure [Uygulama Hizmeti'ndeki uygulamalar için tanılama günlemesini etkinleştir'e](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure SQL bağlantı mimarisi](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture)
* [Azure SQL Veritabanı ve Veri Ambarı ağ erişim denetimleri](https://docs.microsoft.com/azure/sql-database/sql-database-networkaccess-overview)
