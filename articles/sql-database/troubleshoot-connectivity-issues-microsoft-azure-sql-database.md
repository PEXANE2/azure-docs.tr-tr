---
title: Microsoft Azure SQL Veritabanı bağlantı sorunlarını giderme | Microsoft Docs
description: Microsoft Azure SQL Veritabanı bağlantı sorunlarını giderme
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
author: v-miegge
ms.author: ramakoni
ms.reviewer: ''
ms.date: 09/27/2019
ms.openlocfilehash: 3717b36e37f7d9cca206b5a9e8e3cded50393748
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71974428"
---
# <a name="troubleshooting-connectivity-issues-with-microsoft-azure-sql-database"></a>Microsoft Azure SQL Veritabanı bağlantı sorunlarını giderme

Azure SQL Veritabanına bağlantı başarısız olduğunda hata iletileri alırsınız. Bağlantı sorunlarının nedeni SQL Azure veritabanının yeniden yapılandırılması, güvenlik duvarı ayarları, bağlantının zaman aşımına uğraması veya yanlış oturum bilgilerinin sağlanması olabilir. Ayrıca, bazı Azure SQL veritabanı kaynaklarına yönelik maksimum sınıra ulaşıldığında Azure SQL veritabanı 'na bağlanamazsınız.

## <a name="error-40613-database--x--on-server--y--is-not-currently-available"></a>Hata 40613: sunucu < y > üzerinde < x > veritabanı şu anda kullanılamıyor

**Ayrıntılı hata**

``40613: Database <DBname> on server <server name> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of '<Tracing ID>'.``

Bu sorunu çözmek için:

1. Bilinen kesintiler için [Microsoft Azure hizmeti panosunu](https://status.azure.com/status) denetleyin. 
2. Bilinen kesintiler yoksa, destek talebi açmak için [Microsoft Azure Destek Web sitesine](http://azure.microsoft.com/support/options) gidin.

Daha fazla bilgi için bkz. [sorun giderme "sunucu üzerindeki veritabanı şu anda kullanılamıyor" hatası](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-common-connection-issues#troubleshoot-transient-errors).

## <a name="a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server"></a>SQL Server bağlantı kurulurken ağla ilgili veya örneğe özgü bir hata oluştu

Bu sorun, uygulamanın sunucuya bağlanamadığı için oluşur.

Bu sorunu çözmek için, aşağıdaki bölümde yer alarak **yaygın bağlantı sorunlarını gidermeye yönelik adımlar**başlıklı adımları (sırasıyla) deneyin.

## <a name="the-server-was-not-found-or-was-not-accessible-errors-26-40-10053"></a>Sunucu bulunamadı veya erişilebilir değil (hata 26, 40, 10053)

### <a name="error-26-error-locating-serverinstance-specified"></a>Hata 26: belirtilen sunucu/örnek bulunurken hata oluştu

**Ayrıntılı hata**

``System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.(provider: SQL Network Interfaces, error: 26 – Error Locating Server/Instance Specified)``

### <a name="error-40-could-not-open-a-connection-to-sql-server"></a>Hata 40: SQL Server bağlantı açılamadı

**Ayrıntılı hata**

``A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)``

### <a name="error-10053-a-transport-level-error-has-occurred-when-receiving-results-from-the-server"></a>Hata 10053: sunucudan sonuçlar alınırken aktarım düzeyi hatası oluştu.

**Ayrıntılı hata**

``10053: A transport-level error has occurred when receiving results from the server. (Provider: TCP Provider, error: 0 - An established connection was aborted by the software in your host machine)``

Bu sorunlar, uygulamanın sunucuya bağlanamadığı için oluşur.

Bu sorunu çözmek için, aşağıdaki bölümde yer alarak **yaygın bağlantı sorunlarını gidermeye yönelik adımlar**başlıklı adımları (sırasıyla) deneyin.

## <a name="cannot-connect-to--servername--due-to-firewall-issues"></a>Güvenlik Duvarı sorunları nedeniyle < ServerName > 'ye bağlanılamıyor

### <a name="error-40615-cannot-connect-to--servername-"></a>Hata 40615: < ServerName > bağlanılamıyor

Bu sorunu çözmek için [Azure Portal kullanarak SQL veritabanında güvenlik duvarı ayarlarını yapılandırın.](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)

### <a name="error-5-cannot-connect-to--servername-"></a>Hata 5: < ServerName > bağlanılamıyor

Bu sorunu çözmek için, bağlantı noktası 1433 ' nin, istemci ve Internet arasındaki tüm güvenlik duvarlarındaki giden bağlantılar için açık olduğundan emin olun. 

Daha fazla bilgi için bkz. [Windows güvenlik duvarını SQL Server erişimine izin verecek şekilde yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure).

## <a name="unable-to-login-to-the-server-errors-18456-40531"></a>Sunucuda oturum açılamıyor (hatalar 18456, 40531)

### <a name="login-failed-for-user--user-name-"></a>Kullanıcı ' < Kullanıcı adı > ' için oturum açma başarısız

**Ayrıntılı hata**

``Login failed for user '<User name>'.This session has been assigned a tracing ID of '<Tracing ID>'. Provide this tracing ID to customer support when you need assistance. (Microsoft SQL Server, Error: 18456)``

Bu sorunu çözmek için, geçerli bir SQL Kullanıcı adı ve parolası sağlamak üzere hizmet yöneticinize başvurun.

Genellikle, hizmet yöneticisi oturum açma bilgilerini eklemek için aşağıdaki adımları kullanabilir:

1. SQL Server Management Studio (SSMS) kullanarak sunucuda oturum açın.
2. Aşağıdaki SQL sorgusunu kullanarak oturum açma adının devre dışı bırakılıp bırakılmadığını kontrol edin:

   ```
   SELECT name, is_disabled FROM sys.sql_logins
   ```

3. Karşılık gelen ad devre dışıysa, aşağıdaki deyimi kullanarak etkinleştirin: 

   ```
   Alter login <User name> enable
   ```

4. SQL oturum açma Kullanıcı adı yoksa, SSMS kullanarak oluşturun:

   1. **Güvenlik**’e çift tıklayarak genişletin. 
   2. **Oturumlar**’a sağ tıklayıp **Yeni oturum açma**’yı seçin. 
   3. Yer tutucularla oluşturulan betikte aşağıdaki SQL sorgusunu düzenleyip çalıştırabilirsiniz:
 
   ```
   CREATE LOGIN <SQL_login_name, sysname, login_name>
   WITH PASSWORD = ‘<password, sysname, Change_Password>’
   GO
   ```       
5. **Veritabanı**’na çift tıklayın. 
6. Kullanıcıya izni vermek istediğiniz veritabanını seçin.
7. **Güvenlik**’e çift tıklayın. 
8. **Kullanıcılar**’a sağ tıklayıp **Yeni Kullanıcı**’yı seçin. 
9. Yer tutucularla oluşturulan betikte aşağıdaki SQL sorgusunu düzenleyip çalıştırabilirsiniz: 

   ```
   CREATE USER <user_name, sysname, user_name>          
   FOR LOGIN <login_name, sysname, login_name>
   WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
   GO
   
   -- Add user to the database owner role

   EXEC sp_addrolemember N’db_owner’, N’<user_name, sysname, user_name>’
   GO
   ```
   
   > [!NOTE]
   > Belirli kullanıcıları belirli veritabanı rollerine eşlemek için `sp_addrolemember` ' yı da kullanabilirsiniz. 

Daha fazla bilgi edinmek için bkz. [Azure SQL Veritabanında Veritabanlarını ve Oturum Açma İşlemlerini Yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="connection-timeout-expired-errors"></a>Bağlantı zaman aşımı süresi doldu hataları

### <a name="systemdatasqlclientsqlexception-0x80131904-connection-timeout-expired"></a>System. Data. SqlClient. SqlException (0x80131904): bağlantı zaman aşımı süresi doldu.

**Ayrıntılı hata**

``System.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed while attempting to consume the pre-login handshake acknowledgement. This could be because the pre-login handshake failed or the server was unable to respond back in time. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=3; handshake=29995;``

### <a name="systemdatasqlclientsqlexception-0x80131904-timeout-expired"></a>System. Data. SqlClient. SqlException (0x80131904): zaman aşımı süresi doldu.

**Ayrıntılı hata**

``System.Data.SqlClient.SqlException (0x80131904): Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding.``

### <a name="systemdataentitycoreentityexception-the-underlying-provider-failed-on-open"></a>System. Data. Entity. Core. EntityException: temeldeki sağlayıcı açık bir şekilde başarısız oldu.

**Ayrıntılı hata**

``System.Data.Entity.Core.EntityException: The underlying provider failed on Open. -> System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. -> System.ComponentModel.Win32Exception: The wait operation timed out``

### <a name="cannot-connect-to--server-name-"></a>> < Sunucu adına bağlanılamıyor. ' '

**Ayrıntılı hata**

``Cannot connect to <server name>.ADDITIONAL INFORMATION:Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=231; handshake=983; [Login] initialization=0; authentication=0; [Post-Login] complete=13000; (Microsoft SQL Server, Error: -2) For help, click: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=-2&LinkId=20476 The wait operation timed out``

Bu özel durumlar, bağlantı ya da sorgu sorunları nedeniyle oluşabilir. Bu hatayı doğrulamak için bağlantı sorunları nedeniyle, **hatanın bir bağlantı sorunundan kaynaklanıyor olup olmadığını onayla**başlıklı bölümüne bakın:

Uygulamanın sunucuya bağlanamadığı için bağlantı zaman aşımları oluşur. Bu sorunu çözmek için, aşağıdaki bölümde yer alarak **yaygın bağlantı sorunlarını gidermeye yönelik adımlar**başlıklı adımları (sırasıyla) deneyin.

## <a name="transient-errors-errors-40197-40545"></a>Geçici hatalar (hatalar 40197, 40545)

### <a name="error-40197-the-service-has-encountered-an-error-processing-your-request-please-try-again-error-code--code-"></a>Hata 40197: hizmet isteğinizi işlerken bir hatayla karşılaştı. Lütfen yeniden deneyin. Kod < hata kodu >

Bu sorun, arka uçta yeniden yapılandırma/yük devretme sırasında karşılaşılan geçici bir hata nedeniyle oluşur.

Bu sorunu çözmek için kısa bir süre bekleyip yeniden deneyin. Sorun kalıcı kalmadığı sürece destek çalışması gerekmez.

En iyi uygulama olarak, yeniden deneme mantığının yerinde olduğundan emin olun. Yeniden deneme mantığı hakkında daha fazla bilgi için bkz. [SQL veritabanında geçici hata ve bağlantı hatalarını giderme](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues).

## <a name="connection-terminated-due-to-hitting-some-system-defined-limit"></a>Sistem tarafından tanımlanan bazı sınıra ulaşarak bağlantı sonlandırıldı

### <a name="error-10928-resource-id-d"></a>Hata 10928: kaynak KIMLIĞI:% d.

**Ayrıntılı hata**

``10928: Resource ID: %d. The %s limit for the database is %d and has been reached. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. The Resource ID value in error message indicates the resource for which limit has been reached. For sessions, Resource ID = 2.``

Bu sorunu geçici olarak çözmek için aşağıdaki yöntemlerden birini deneyin:

* Uzun süre çalışan sorgular olup olmadığını doğrulayın:

  > [!NOTE]
  > Bu, sorunu çözemeyebilecek en az aList bir yaklaşımdır.

  1. Aşağıdaki SQL sorgusunu yürüterek, tüm engelleyici istekleri görmek için [sys. DM _exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) görünümünü kontrol edin:

     ```
     SELECT * FROM dm_exec_requests
     ```

  2. Baş engelleyicisinin **giriş arabelleğini** belirleme.
  3. Baş engelleyici sorgusunu ayarlayın.

    Derinlemesine bir sorun giderme yordamı için bkz. [My Query, bulutta güzel çalışıyor mu?](http://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

* Veritabanı, adresleme engelleme ve uzun süre çalışan sorgulara rağmen sürekli olarak sınırına ulaşırsa, yeni önizleme sürümlerinden birine ( [Standart veya Premium sürümler](https://azure.microsoft.com/pricing/details/sql-database/)gibi) yükseltmeyi göz önünde bulundurun.

SQL Veritabanı fiyatlandırma seçenekleri hakkında daha fazla bilgi için [Azure SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/single/)' nı gözden geçirin.

Dinamik yönetim görünümleri hakkında daha fazla bilgi için bkz. [sistem dinamik yönetim görünümleri](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views).

Bu hata iletisi hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı sunucusu Için SQL veritabanı kaynak sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server).

### <a name="error-10929-resource-id-1"></a>Hata 10929: kaynak KIMLIĞI: 1.

**Ayrıntılı hata**

``10929: Resource ID: 1. The %s minimum guarantee is %d, maximum limit is %d and the current usage for the database is %d. However, the server is currently too busy to support requests greater than %d for this database. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. Otherwise, please try again later.``

Bu hata hakkında daha fazla bilgi için bkz. [SQL veritabanı istemci programları Için hata iletileri](https://docs.microsoft.com/azure/sql-database/sql-database-develop-error-messages)

### <a name="error-40501-the-service-is-currently-busy"></a>Hata 40501: hizmet şu anda meşgul

**Ayrıntılı hata**

``40501: The service is currently busy. Retry the request after 10 seconds. Incident ID: %ls. Code: %d.``

Bu bir altyapı azaltma hatası, kaynak limitlerinin aşıldığını belirten bir gösterge.

Kaynak limitleri hakkında daha fazla bilgi için bkz. [veritabanı sunucusu kaynak sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server).

### <a name="error-40544--the-database-has-reached-its-size-quota"></a>Hata 40544: veritabanı boyut kotasına ulaştı

**Ayrıntılı hata**

``40544: The database has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions. Incident ID: <ID>. Code: <code>.``

Bu hata, veritabanı boyut kotasına ulaştığında oluşur.

Aşağıdaki adımlar, sorunu geçici olarak gidermenize yardımcı olabilir veya size göz önünde bulundurmanız gereken ek seçenekler sağlar.

1. Azure portal panosunu kullanarak veritabanının geçerli boyutunu denetleyin.

   > [!NOTE]
   > Hangi tabloların en fazla alanı tükettiğini ve temizleme için olası adayları belirlemek için aşağıdaki SQL sorgusunu kullanabilirsiniz:

   ```
   SELECT o.name,
    a.SUM(p.row_count) AS 'Row Count',
    b.SUM(p.reserved_page_count) * 8.0 / 1024 AS 'Table Size (MB)'
   FROM sys.objects o
   JOIN sys.dm_db_partition_stats p on p.object_id = o.object_id
   GROUP BY o.name
   ORDER BY [Table Size (MB)] DESC
   ```

2. Geçerli boyut, sürümünüz için desteklenen en büyük boyutu aşmazsa, MAXSıZE ayarını artırmak için ALTER DATABASE ' i kullanabilirsiniz. 
3. Veritabanı boyutu sürümünüz için desteklenen en büyük boyutu zaten aşdurmamışsa aşağıdaki adımlardan birini kullanabilirsiniz:
   1. Normal veritabanı temizleme etkinliklerini gerçekleştirin (kesme/silme 'yı kullanarak istenmeyen verileri temizleme vb.) veya SSIS, bcp vb. kullanarak verileri dışarı taşıyın.)
   2. Verileri bölümleyin veya silin, dizinleri bırakın veya olası çözümler için belgelere başvurun. 
   
   *  Veritabanı Ölçeklendirme için bkz. [tek veritabanı kaynaklarını ölçeklendirme](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-scale) ve [elastik havuz kaynaklarını ölçeklendirme](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool-scale).

### <a name="error-40549-session-is-terminated-because-you-have-a-long-running-transaction"></a>Hata 40549: uzun süre çalışan bir işlem olduğu için oturum sonlandırıldı.

**Ayrıntılı hata**

``40549: Session is terminated because you have a long-running transaction. Try shortening your transaction.``

Bu hata iletisiyle sürekli olarak karşılaşırsanız, bu sorunu çözmek için aşağıdaki adımları deneyin: 

1. Aşağıdaki SQL betiğini yürüterek total_elapsed_time sütunu için yüksek değere sahip tüm açık oturumları görmek için sys. DM _exec_requests görünümünü kontrol edin:

   ```
   SELECT * FROM dm_exec_requests
   ```
2. Uzun süre çalışan sorgunun giriş arabelleğini belirleme. 
3. Sorguyu ayarlayın.

Ayrıca, sorgularınızı toplu olarak da düşünün. Toplu işleme hakkında bilgi için bkz. [SQL veritabanı uygulama performansını artırmak için toplu işlem kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance).

Derinlemesine bir sorun giderme yordamı için bkz. [My Query, bulutta güzel çalışıyor mu?](http://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

### <a name="error-40551-the-session-has-been-terminated-because-of-excessive-tempdb-usage"></a>Hata 40551: çok sayıda TEMPDB kullanımı nedeniyle oturum sonlandırıldı.

**Ayrıntılı hata**

``40551: The session has been terminated because of excessive TEMPDB usage. Try modifying your query to reduce the temporary table space usage.``

Bu sorunu geçici olarak çözmek için şu adımları izleyin:

1. Geçici tablo alanı kullanımını azaltmak için sorguları değiştirin. 
2. Artık gerekli olmadıklarında geçici nesneleri bırakın. 
3. Tabloları Kes veya kullanılmayan tabloları Kaldır.

### <a name="error-40552-the-session-has-been-terminated-because-of-excessive-transaction-log-space-usage"></a>Hata 40552: oturum, aşırı işlem günlüğü alanı kullanımı nedeniyle sonlandırıldı.

**Ayrıntılı hata**

``40552: The session has been terminated because of excessive transaction log space usage. Try modifying fewer rows in a single transaction.``

Bu sorunu çözmek için aşağıdaki yöntemleri izleyin: 

* Bu sorun INSERT, Update veya delete işlemleri nedeniyle oluşur. Toplu işlem veya birden çok daha küçük işleme bölme uygulayarak hemen üzerinde çalıştırılan satır sayısını azaltmayı deneyin.
* Bu sorun, dizin yeniden oluşturma işlemleri nedeniyle oluşur. Aşağıdaki formüle bağlı olduğunuzdan emin olun: tablo * ' de etkilenen satır sayısı (bayt + 80 ' de güncellenen alanın ortalama boyutu +) < 2 GB

  > [!NOTE]
  > Dizin yeniden oluşturmak için, güncellenen alanın ortalama boyutu, ortalama Dizin boyutu ile değiştirilmelidir.

### <a name="error-40553-the-session-has-been-terminated-because-of-excessive-memory-usage"></a>Hata 40553: aşırı bellek kullanımı nedeniyle oturum sonlandırıldı.

**Ayrıntılı hata**

``40553 : The session has been terminated because of excessive memory usage. Try modifying your query to process fewer rows.``

Bu sorunu geçici olarak çözmek için sorguyu iyileştirmeden çalışın.

Derinlemesine bir sorun giderme yordamı için bkz. [My Query, bulutta güzel çalışıyor mu?](http://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).


### <a name="cannot-open-database-master-requested-by-the-login-the-login-failed"></a>Oturum açma tarafından istenen "ana" veritabanı açılamıyor. Oturum açılamadı.

Bu sorun, hesabın ana veritabanına erişim izni olmadığı için oluşur. Ancak, varsayılan olarak, SQL Server Management Studio (SSMS) ana veritabanına bağlanmayı dener.

Bu sorunu çözmek için şu adımları izleyin:

1. SSMS oturum açma ekranında **Seçenekler**' e ve ardından **bağlantı özellikleri**' ne tıklayın. 
2. **Veritabanına Bağlan**' da, kullanıcının varsayılan veritabanı adını varsayılan oturum açma veritabanı olarak yazın ve ardından **Bağlan**' a tıklayın.

   ![Cannot-Open-Database-Master. png](media/troubleshoot-connectivity-issues-microsoft-azure-sql-database/cannot-open-database-master.png)

## <a name="confirm-whether-an-error-is-due-to-a-connectivity-issue"></a>Bir hatanın bağlantı sorunu olup olmadığını onaylayın

Bir hatanın bağlantı sorunu olup olmadığını doğrulamak için, aşağıdaki gibi bir bağlantıyı açmaya yönelik çağrıları gösteren çerçeveler için yığın izlemesini gözden geçirin ( **SqlConnection** sınıfına yönelik başvuruya göz önünde bulunur):

```
System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
 at System.Data.SqlClient.SqlConnection.Open()
 at AzureConnectionTest.Program.Main(String[] args)
ClientConnectionId:<Client connection ID>
```

Sorgu sorunları nedeniyle özel durum oluştuğunda, aşağıdakine benzer bir çağrı yığını görürsünüz ( **SqlCommand** sınıfına olan başvuruya dikkat edin). Bu senaryolarda, [sorgularınızı ayarlayın](http://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

```
  at System.Data.SqlClient.SqlCommand.ExecuteReader()
  at AzureConnectionTest.Program.Main(String[] args)
  ClientConnectionId:<Client ID>
```
Hassas ayar performansı hakkında ek yönergeler için lütfen aşağıdakilere bakın:

* [Azure SQL dizinlerini ve Istatistiklerini koruma](https://techcommunity.microsoft.com/t5/Azure-Database-Support-Blog/How-to-maintain-Azure-SQL-Indexes-and-Statistics/ba-p/368787)
* [Azure SQL veritabanı 'nda el ile ayarlama sorgusu performansı](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
* [Dinamik yönetim görünümlerini kullanarak performans Azure SQL veritabanı 'nı izleme](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
* [Azure SQL veritabanı 'nda sorgu deposunu çalıştırma](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)


## <a name="steps-to-fix-common-connection-issues"></a>Sık karşılaşılan bağlantı sorunlarını giderme adımları

1. TCP IP 'nin uygulama sunucusunda bir istemci protokolü olarak etkinleştirildiğinden emin olun. Daha fazla bilgi için bkz. [istemci protokollerini yapılandırma](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-client-protocols). SQL Server araçları yüklü olmayan uygulama sunucularında, TCP IP 'nin **cliconfg. exe** ' yi çalıştırarak (SQL Server Istemci ağ yardımcı programı) etkin olduğundan emin olun. 
2. Doğru yapılandırıldığından emin olmak için uygulamanın bağlantı dizesini denetleyin. Örneğin, bağlantı dizesinin doğru bağlantı noktasını (1433) ve tam sunucu adını belirttiğinden emin olun.
Bkz. [SQL Server bağlantı bilgilerini alın](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms#get-sql-server-connection-information).
3. Bağlantı **zaman aşımını**artırmayı deneyin. Microsoft, en az 30 saniyelik bir bağlantı zaman aşımı kullanılmasını önerir. 
4. [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms), bir UDL dosyası, ping ve Telnet kullanarak uygulama sunucusu ve Azure SQL veritabanı arasındaki bağlantıyı test edin. Daha fazla bilgi için bkz. bağlantı sorunlarını [giderme SQL Server](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) bağlantı sorunları ve [Tanılama](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues#diagnostics).

   > [!NOTE]
   > Sorun giderme adımı olarak, farklı bir istemci bilgisayarında bağlantıyı test etmek de deneyebilirsiniz.

5. En iyi uygulama olarak, yeniden deneme mantığının yerinde olduğundan emin olun. Yeniden deneme mantığı hakkında daha fazla bilgi için bkz. [SQL veritabanında geçici hata ve bağlantı hatalarını giderme](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues).

Yukarıdaki adımlar sorununuzu gidermezse, daha fazla veri toplamayı deneyin ve desteğe başvurun. Uygulamanız bir bulut hizmeti ise günlüğe kaydetmeyi etkinleştirin. Bu adım, hatanın UTC zaman damgasını döndürür. Ayrıca, SQL Azure izleme KIMLIĞINI döndürür. [Microsoft Müşteri Destek Hizmetleri](http://azure.microsoft.com/support/options/) , bu bilgileri kullanabilir. 

Günlüğe kaydetmenin nasıl etkinleştirileceği hakkında daha fazla bilgi için, bkz. [Azure App Service Web Apps için tanılama günlüğünü etkinleştirme](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/).

**İlgili belgeler**

* [Azure SQL Bağlantı Mimarisi](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture)<br>
* [Azure SQL veritabanı ve veri ambarı ağ erişim denetimleri](https://docs.microsoft.com/azure/sql-database/sql-database-networkaccess-overview)