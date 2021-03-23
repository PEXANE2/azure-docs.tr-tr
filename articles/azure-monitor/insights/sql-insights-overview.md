---
title: SQL Insights ile SQL dağıtımlarınızı izleme (Önizleme)
description: Azure Izleyici 'de SQL öngörülerine genel bakış
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/15/2021
ms.openlocfilehash: b9c5db14bec87b30e51d39b1430ecc1f3cbef855
ms.sourcegitcommit: ba3a4d58a17021a922f763095ddc3cf768b11336
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104798298"
---
# <a name="monitor-your-sql-deployments-with-sql-insights-preview"></a>SQL Insights ile SQL dağıtımlarınızı izleme (Önizleme)
SQL Insights, SQL dağıtımlarınızın performansını ve sistem durumunu izler.  Performans sorunlarını ve sorunlarını tanımlayarak bir SQL arka ucunun çevresinde oluşturduğunuz önemli iş yüklerinin öngörülebilir bir performans ve kullanılabilirliğini sunmaya yardımcı olabilir. SQL Insights, verilerini [Azure Izleyici günlüklerinde](../logs/data-platform-logs.md)depolar ve bu sayede güçlü toplama ve filtreleme olanağı sunar ve zaman içinde veri eğilimlerini analiz edebilir. Bu verileri Azure Izleyici 'den bu teklifin bir parçası olarak gönderdiğimiz görünümlerde görüntüleyebilir ve sorguları çalıştırmak ve eğilimleri çözümlemek için doğrudan günlük verilerine Delve yapabilirsiniz.

SQL Insights, SQL IaaS dağıtımlarınıza hiçbir şey yüklemez. Bunun yerine, SQL PaaS ve SQL IaaS dağıtımları için verileri uzaktan toplamak üzere adanmış izleme sanal makineleri kullanır.  SQL Insights izleme profili, Azure SQL VERITABANı, Azure SQL yönetilen örneği ve bir Azure sanal makinesinde çalışan SQL Server dahil olmak üzere SQL türüne göre toplanacak veri kümelerini yönetmenizi sağlar.

## <a name="pricing"></a>Fiyatlandırma

SQL Insights için doğrudan maliyet yoktur, ancak Log Analytics çalışma alanındaki etkinliği için ücretlendirilirsiniz. [Azure izleyici fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/monitor/)yayınlanan fiyatlandırmaya bağlı olarak, SQL Insights için faturalandırılır:

- Aracılardan alınan ve çalışma alanında depolanan veriler.
- Günlük verilerine göre uyarı kuralları.
- Uyarı kurallarından gönderilen bildirimler.

Günlük boyutu, toplanan verilerin dize uzunluklarına göre değişir ve veritabanı etkinliğinin miktarı ile artabilir. 

## <a name="supported-versions"></a>Desteklenen sürümler 
SQL Insights, SQL Server aşağıdaki sürümlerini destekler:

- SQL Server 2012 ve üzeri

SQL Insights, aşağıdaki ortamlarda çalışan SQL Server destekler:

- Azure SQL Veritabanı
- Azure SQL Yönetilen Örnek
- Azure SQL VM 'Leri
- Azure VM’leri

SQL Insights, aşağıdakiler için destek veya sınırlı destek içermez:

- Azure dışındaki sanal makinelerde çalışan SQL Server şu anda desteklenmiyor.
- Azure SQL veritabanı elastik havuzları: genel önizleme sırasında sınırlı destek. Genel kullanıma göre tam olarak desteklenecektir.  
- Azure SQL sunucusuz dağıtımlar: etkin coğrafi-DR gibi, geçerli izleme aracıları sunucusuz dağıtımın uykuya geçmesini engeller. Bu, sunucusuz dağıtımlardan beklenen maliyetlerin daha yüksek olmasına neden olabilir.  
- Okunabilir Ikincil öğeler: Şu anda yalnızca tek bir okunabilir ikincil uç nokta (İş Açısından Kritik veya Hyperscale) olan dağıtım türleri desteklenecektir. Hiper ölçekli dağıtımlar adlandırılmış çoğaltmaları destekledikleri zaman, tek bir mantıksal veritabanı için birden çok okunabilir ikincil uç nokta desteklenebilecek olacaktır.  
- Azure Active Directory: Şu anda yalnızca Izleme Aracısı için SQL oturum açmaları destekliyoruz. Gelecek bir sürümde Azure Active Directory 'yi desteklemeyi planlıyoruz ve bir Bespoke etki alanı denetleyicisindeki Active Directory 'yi kullanarak SQL VM kimlik doğrulaması için geçerli bir destek sunarız.  


## <a name="open-sql-insights"></a>SQL Insights 'ı aç
Azure portal **Azure izleyici** menüsünün **Öngörüler** bölümünde **SQL (Önizleme)** öğesini seçerek SQL Insights 'ı açın. İzlemekte olduğunuz SQL türüne yönelik deneyimi yüklemek için bir kutucuğa tıklayın.

:::image type="content" source="media/sql-insights/portal.png" alt-text="Azure portal 'de SQL Insights.":::


## <a name="enable-sql-insights"></a>SQL Insights 'ı etkinleştir 
Sorun gidermeye yönelik adımlara ek olarak SQL Insights 'ı etkinleştirmek için ayrıntılı yordam için bkz. [SQL Insights 'ı etkinleştirme](sql-insights-enable.md) .


## <a name="data-collected-by-sql-insights"></a>SQL Insights tarafından toplanan veriler
Genel önizlemede, SQL Insights yalnızca izleme uzak yöntemini destekler. SQL Server [telegraf Aracısı](https://www.influxdata.com/time-series-platform/telegraf/) yüklü değil. [Telegraf için SQL Server giriş eklentisini](https://www.influxdata.com/integration/microsoft-sql-server/) kullanır ve farklı türde SQL It izleyicilerinde üç sorgu grubunu kullanır: Azure SQL veritabanı, Azure SQL yönetilen örneği, BIR Azure VM ÜZERINDE çalışan SQL Server. 

Aşağıdaki tablolarda aşağıdakiler özetlenmektedir:

- SqlServer telegraf eklentisindeki sorgunun adı
- Sorgu çağrılarının dinamik yönetilen görünümleri
- Ad alanı verileri *ınsighstölçümlerini* tablosunda görünür
- Verilerin varsayılan olarak toplanıp toplanmayacağı
- Verilerin varsayılan olarak ne sıklıkta toplandığı
 
İzleme profilinizi oluştururken hangi sorguların çalıştırılacağını ve veri toplama sıklığını değiştirebilirsiniz. 

### <a name="azure-sql-db-data"></a>Azure SQL DB verileri 

| Sorgu adı | DMV | Ad Alanı | Varsayılan olarak etkin | Varsayılan toplama sıklığı |
|:---|:---|:---|:---|:---|
| Azuresddbwaitstats |  sys.dm_db_wait_stats | sqlserver_azuredb_waitstats | Hayır | NA |
| Azuresddbresourcestats | sys.dm_db_resource_stats | sqlserver_azure_db_resource_stats | Yes | 60 saniye |
| Azuressqldbresourceidare | sys.dm_user_db_resource_governance | sqlserver_db_resource_governance | Yes | 60 saniye |
| Azuressqldbdatabaseıo | sys.dm_io_virtual_file_stats<br>sys.database_files<br>tempdb.sys .database_files | sqlserver_database_io | Yes | 60 saniye |
| Azuresddbserverproperties | sys.dm_os_job_object<br>sys.database_files<br>dosyasında. erişebileceğiniz<br>dosyasında. [database_service_objectives] | sqlserver_server_properties | Yes | 60 saniye |
| AzureSQLDBOsWaitstats | sys.dm_os_wait_stats | sqlserver_waitstats | Yes | 60 saniye |
| AzureSQLDBMemoryClerks | sys.dm_os_memory_clerks | sqlserver_memory_clerks | Yes | 60 saniye |
| Azuresddbperformancecounters | sys.dm_os_performance_counters<br>sys.databases | sqlserver_performance | Yes | 60 saniye |
| Azuresddbrequests | sys.dm_exec_sessions<br>sys.dm_exec_requests<br>sys.dm_exec_sql_text | sqlserver_requests | Hayır | NA |
| AzureSQLDBSchedulers | sys.dm_os_schedulers | sqlserver_schedulers | Hayır | NA  |

### <a name="azure-sql-managed-instance-data"></a>Azure SQL yönetilen örnek verileri 

| Sorgu adı | DMV | Ad Alanı | Varsayılan olarak etkin | Varsayılan toplama sıklığı |
|:---|:---|:---|:---|:---|
| Azuressqlmıresourcestats | sys.server_resource_stats | sqlserver_azure_db_resource_stats | Yes | 60 saniye |
| Azuressqlmıresourceidare | sys.dm_instance_resource_governance | sqlserver_instance_resource_governance | Yes | 60 saniye |
| Azuressqlmıdatabaseıo | sys.dm_io_virtual_file_stats<br>sys.master_files | sqlserver_database_io | Yes | 60 saniye |
| Azuressqlmiserverproperties | sys.server_resource_stats | sqlserver_server_properties | Yes | 60 saniye |
| AzureSQLMIOsWaitstats | sys.dm_os_wait_stats | sqlserver_waitstats | Yes | 60 saniye |
| AzureSQLMIMemoryClerks | sys.dm_os_memory_clerks | sqlserver_memory_clerks | Yes | 60 saniye |
| Azuressqlmıperformancecounters | sys.dm_os_performance_counters<br>sys.databases | sqlserver_performance | Yes | 60 saniye |
| Azuressqlmırequests | sys.dm_exec_sessions<br>sys.dm_exec_requests<br>sys.dm_exec_sql_text | sqlserver_requests | Hayır | NA |
| Azuressqlmischedulers | sys.dm_os_schedulers | sqlserver_schedulers | Hayır | NA |

### <a name="sql-server-data"></a>SQL Server verileri

| Sorgu adı | DMV | Ad Alanı | Varsayılan olarak etkin | Varsayılan toplama sıklığı |
|:---|:---|:---|:---|:---|
| SQLServerPerformanceCounters | sys.dm_os_performance_counters | sqlserver_performance | Yes | 60 saniye |
| Sqlserverwaitstatskategorize | sys.dm_os_wait_stats | sqlserver_waitstats | Yes | 60 saniye | 
| Sqlserverdatabaseıo | sys.dm_io_virtual_file_stats<br>sys.master_files | sqlserver_database_io | Yes | 60 saniye |
| SQLServerProperties | sys.dm_os_sys_info | sqlserver_server_properties | Yes | 60 saniye |
| SQLServerMemoryClerks | sys.dm_os_memory_clerks | sqlserver_memory_clerks | Yes | 60 saniye |
| SQLServerSchedulers | sys.dm_os_schedulers | sqlserver_schedulers | Hayır | NA |
| SQLServerRequests | sys.dm_exec_sessions<br>sys.dm_exec_requests<br>sys.dm_exec_sql_text | sqlserver_requests | Hayır | NA |
| SQLServerVolumeSpace | sys.master_files | sqlserver_volume_space | Yes | 60 saniye |
| SQLServerCpu | sys.dm_os_ring_buffers | sqlserver_cpu | Yes | 60 saniye |
| Sqlserverkullanılabilirliği Bilityreplicastates | sys.dm_hadr_availability_replica_states<br>sys.availability_replicas<br>sys.availability_groups<br>sys.dm_hadr_availability_group_states | sqlserver_hadr_replica_states | | 60 saniye |
| SQLServerDatabaseReplicaStates | sys.dm_hadr_database_replica_states<br>sys.availability_replicas | sqlserver_hadr_dbreplica_states | | 60 saniye |




## <a name="next-steps"></a>Sonraki adımlar

SQL öngörülerini etkinleştirmek için ayrıntılı yordam için bkz. [SQL Insights 'ı etkinleştirme](sql-insights-enable.md) .
SQL Insights hakkında sık sorulan sorular için [sık sorulan sorular](../faq.md#sql-insights-preview) bölümüne bakın.
