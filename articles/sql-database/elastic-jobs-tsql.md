---
title: Transact-SQL (T-SQL) ile Elastik Veritabanı İşleri oluşturma ve yönetme
description: Transact-SQL (T-SQL) kullanarak Elastik Veritabanı İş aracısı ile birçok veritabanlarında komut dosyaları çalıştırın.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
ms.author: jaredmoo
author: jaredmoo
ms.reviewer: sstein
ms.date: 02/07/2020
ms.openlocfilehash: 740a42dc94cdfa8d5c5a91b32b58cbff4c1bcda0
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687779"
---
# <a name="use-transact-sql-t-sql-to-create-and-manage-elastic-database-jobs"></a>Elastik Veritabanı İşlerini oluşturmak ve yönetmek için Transact-SQL'i (T-SQL) kullanın

Bu makalede, T-SQL kullanarak Elastik İşler ile çalışmaya başlamak için birçok örnek senaryo sağlar.

Örnekler, [*iş veritabanında*](sql-database-job-automation-overview.md#job-database)bulunan [depolanmış yordamları](#job-stored-procedures) ve [görünümleri](#job-views) kullanır.

Transact-SQL (T-SQL), işleri oluşturmak, yapılandırmak, yürütmek ve yönetmek için kullanılır. Elastik İş aracısını oluşturma T-SQL'de desteklenmez, bu nedenle önce portalı veya [PowerShell'i](elastic-jobs-powershell.md#create-the-elastic-job-agent)kullanarak bir *Elastik İş aracısı* oluşturmanız gerekir.


## <a name="create-a-credential-for-job-execution"></a>İş yürütme için bir kimlik bilgisi oluşturma

Kimlik bilgisi komut dosyası yürütme için hedef veritabanlarına bağlanmak için kullanılır. Kimlik bilgisi, komut dosyasını başarıyla yürütmek için hedef grup tarafından belirtilen veritabanlarında uygun izinlere ihtiyaç duyar. Bir sunucu ve/veya havuz hedef grubu üyesi kullanırken, iş yürütme sırasında sunucu ve/veya havuz genişletmeden önce kimlik bilgilerini yenilemek için kullanılmak üzere bir ana kimlik bilgisi oluşturulması önerilir. Veritabanı kapsamı kimlik bilgileri iş aracısı veritabanında oluşturulur. Aynı kimlik bilgileri, hedef *veritabanlarında Giriş* Veritabanı İzinleri vermek için Giriş Oluşturma ve *Oturum'dan Kullanıcı Oluşturmak* için kullanılmalıdır.


```sql
--Connect to the job database specified when creating the job agent

-- Create a db master key if one does not already exist, using your own password.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';  
  
-- Create a database scoped credential.  
CREATE DATABASE SCOPED CREDENTIAL myjobcred WITH IDENTITY = 'jobcred',
    SECRET = '<EnterStrongPasswordHere>'; 
GO

-- Create a database scoped credential for the master database of server1.
CREATE DATABASE SCOPED CREDENTIAL mymastercred WITH IDENTITY = 'mastercred',
    SECRET = '<EnterStrongPasswordHere>'; 
GO
```

## <a name="create-a-target-group-servers"></a>Hedef grup (sunucular) oluşturma

Aşağıdaki örnek, bir sunucudaki tüm veritabanlarına karşı bir işin nasıl yürütüldgerektiğini gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:


```sql
-- Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC jobs.sp_add_target_group 'ServerGroup1'

-- Add a server target member
EXEC jobs.sp_add_target_group_member
'ServerGroup1',
@target_type = 'SqlServer',
@refresh_credential_name='mymastercred', --credential required to refresh the databases in server
@server_name='server1.database.windows.net'

--View the recently created target group and target group members
SELECT * FROM jobs.target_groups WHERE target_group_name='ServerGroup1';
SELECT * FROM jobs.target_group_members WHERE target_group_name='ServerGroup1';
```


## <a name="exclude-an-individual-database"></a>Tek bir veritabanını hariç tutma

Aşağıdaki örnek, *MappingDB*adlı veritabanı dışında, bir SQL Veritabanı sunucusundaki tüm veritabanlarına karşı bir işin nasıl yürütüldöşedi gösterilmektedir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC [jobs].sp_add_target_group N'ServerGroup'
GO

-- Add a server target member
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'London.database.windows.net'
GO

-- Add a server target member
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name='server2.database.windows.net'
GO

--Exclude a database target member from the server target group
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@membership_type = N'Exclude',
@target_type = N'SqlDatabase',
@server_name = N'server1.database.windows.net',
@database_name =N'MappingDB'
GO

--View the recently created target group and target group members
SELECT * FROM [jobs].target_groups WHERE target_group_name = N'ServerGroup';
SELECT * FROM [jobs].target_group_members WHERE target_group_name = N'ServerGroup';
```


## <a name="create-a-target-group-pools"></a>Hedef grup (havuzlar) oluşturma

Aşağıdaki örnek, bir veya daha fazla elastik havuzdaki tüm veritabanlarının nasıl hedeflenebildiğini gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing pool(s)
EXEC jobs.sp_add_target_group 'PoolGroup'

-- Add an elastic pool(s) target member
EXEC jobs.sp_add_target_group_member
'PoolGroup',
@target_type = 'SqlElasticPool',
@refresh_credential_name='mymastercred', --credential required to refresh the databases in server
@server_name='server1.database.windows.net',
@elastic_pool_name='ElasticPool-1'

-- View the recently created target group and target group members
SELECT * FROM jobs.target_groups WHERE target_group_name = N'PoolGroup';
SELECT * FROM jobs.target_group_members WHERE target_group_name = N'PoolGroup';
```


## <a name="deploy-new-schema-to-many-databases"></a>Birçok veritabanına yeni şema dağıtma

Aşağıdaki örnek, tüm veritabanlarına yeni şema nasıl dağıtılanın gösterilmektedir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:


```sql
--Connect to the job database specified when creating the job agent

--Add job for create table
EXEC jobs.sp_add_job @job_name='CreateTableTest', @description='Create Table Test'

-- Add job step for create table
EXEC jobs.sp_add_jobstep @job_name='CreateTableTest',
@command=N'IF NOT EXISTS (SELECT * FROM sys.tables 
            WHERE object_id = object_id(''Test''))
CREATE TABLE [dbo].[Test]([TestId] [int] NOT NULL);',
@credential_name='myjobcred',
@target_group_name='PoolGroup'
```


## <a name="data-collection-using-built-in-parameters"></a>Yerleşik parametreleri kullanarak veri toplama

Birçok veri toplama senaryosunda, işin sonuçlarını işleme sonrasına yardımcı olmak için bu komut dosyası değişkenlerinden bazılarını eklemek yararlı olabilir.

- $(job_name)
- $(job_id)
- $(job_version)
- $(step_id)
- $(step_name)
- $(job_execution_id)
- $(job_execution_create_time)
- $(target_group_name)

Örneğin, aynı iş yürütülmesinden elde edilen tüm sonuçları birlikte gruplandırmak için aşağıdaki komutta gösterildiği gibi *$(job_execution_id)* kullanın:


```sql
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());'
```


## <a name="monitor-database-performance"></a>Veritabanı performansını izleme

Aşağıdaki örnek, birden çok veritabanından performans verileri toplamak için yeni bir iş oluşturur.

Varsayılan olarak, iş aracısı döndürülen sonuçları depolamak için çıktı tablosu oluşturur. Bu nedenle, çıktı kimlik bilgisi ile ilişkili veritabanı ilkesi en az `CREATE TABLE` aşağıdaki izinlere `ALTER` `SELECT`sahip `INSERT` `DELETE` olmalıdır: veritabanında, , , `SELECT` çıktı tablosunda veya şema ve [sys.indexes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-indexes-transact-sql) katalog görünümünde.

Tabloyu önceden el ile oluşturmak istiyorsanız, tablonun aşağıdaki özelliklere sahip olması gerekir:
1. Sonuç kümesi için doğru ada ve veri türlerine sahip sütunlar.
2. Benzersiz tanımlayıcının veri türüne sahip internal_execution_id için ek sütun.
3. internal_execution_id sütununda adlandırılmış `IX_<TableName>_Internal_Execution_ID` kümesiz bir dizin.
4. Veritabanında `CREATE TABLE` izin dışında yukarıda listelenen tüm izinler.

[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutları çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Add a job to collect perf results
EXEC jobs.sp_add_job @job_name ='ResultsJob', @description='Collection Performance data from all customers'

-- Add a job step w/ schedule to collect results
EXEC jobs.sp_add_jobstep
@job_name='ResultsJob',
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());',
@credential_name='myjobcred',
@target_group_name='PoolGroup',
@output_type='SqlDatabase',
@output_credential_name='myjobcred',
@output_server_name='server1.database.windows.net',
@output_database_name='<resultsdb>',
@output_table_name='<resutlstable>'
Create a job to monitor pool performance
--Connect to the job database specified when creating the job agent

-- Add a target group containing master database
EXEC jobs.sp_add_target_group 'MasterGroup'

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name='MasterGroup',
@target_type='SqlDatabase',
@server_name='server1.database.windows.net',
@database_name='master'

-- Add a job to collect perf results
EXEC jobs.sp_add_job
@job_name='ResultsPoolsJob',
@description='Demo: Collection Performance data from all pools',
@schedule_interval_type='Minutes',
@schedule_interval_count=15

-- Add a job step w/ schedule to collect results
EXEC jobs.sp_add_jobstep
@job_name='ResultsPoolsJob',
@command=N'declare @now datetime
DECLARE @startTime datetime
DECLARE @endTime datetime
DECLARE @poolLagMinutes datetime
DECLARE @poolStartTime datetime
DECLARE @poolEndTime datetime
SELECT @now = getutcdate ()
SELECT @startTime = dateadd(minute, -15, @now)
SELECT @endTime = @now
SELECT @poolStartTime = dateadd(minute, -30, @startTime)
SELECT @poolEndTime = dateadd(minute, -30, @endTime)

SELECT elastic_pool_name , end_time, elastic_pool_dtu_limit, avg_cpu_percent, avg_data_io_percent, avg_log_write_percent, max_worker_percent, max_session_percent,
        avg_storage_percent, elastic_pool_storage_limit_mb FROM sys.elastic_pool_resource_stats
        WHERE end_time > @poolStartTime and end_time <= @poolEndTime;
'),
@credential_name='myjobcred',
@target_group_name='MasterGroup',
@output_type='SqlDatabase',
@output_credential_name='myjobcred',
@output_server_name='server1.database.windows.net',
@output_database_name='resultsdb',
@output_table_name='resutlstable'
```


## <a name="view-job-definitions"></a>İş tanımlarını görüntüleme

Aşağıdaki örnek, geçerli iş tanımlarının nasıl görüntülenebildiğini gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- View all jobs
SELECT * FROM jobs.jobs

-- View the steps of the current version of all jobs
SELECT js.* FROM jobs.jobsteps js
JOIN jobs.jobs j 
  ON j.job_id = js.job_id AND j.job_version = js.job_version

-- View the steps of all versions of all jobs
select * from jobs.jobsteps
```


## <a name="begin-ad-hoc-execution-of-a-job"></a>Bir işin geçici olarak yürütülmesine başlayın

Aşağıdaki örnek, bir işe hemen nasıl başlan gerektiğini gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Execute the latest version of a job
EXEC jobs.sp_start_job 'CreateTableTest'

-- Execute the latest version of a job and receive the execution id
declare @je uniqueidentifier
exec jobs.sp_start_job 'CreateTableTest', @job_execution_id = @je output
select @je

select * from jobs.job_executions where job_execution_id = @je

-- Execute a specific version of a job (e.g. version 1)
exec jobs.sp_start_job 'CreateTableTest', 1
```


## <a name="schedule-execution-of-a-job"></a>Bir işin yürütülmesini zamanlama

Aşağıdaki örnek, gelecekteki yürütme için bir iş zamanlamak için nasıl gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_update_job
@job_name='ResultsJob',
@enabled=1,
@schedule_interval_type='Minutes',
@schedule_interval_count=15
```

## <a name="monitor-job-execution-status"></a>İş yürütme durumunu izleme

Aşağıdaki örnek, tüm işler için yürütme durumu ayrıntılarının nasıl görüntülenebildiğini gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

--View top-level execution status for the job named 'ResultsPoolJob'
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' and step_id IS NULL
ORDER BY start_time DESC

--View all top-level execution status for all jobs
SELECT * FROM jobs.job_executions WHERE step_id IS NULL
ORDER BY start_time DESC

--View all execution statuses for job named 'ResultsPoolsJob'
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' 
ORDER BY start_time DESC

-- View all active executions
SELECT * FROM jobs.job_executions 
WHERE is_active = 1
ORDER BY start_time DESC
```


## <a name="cancel-a-job"></a>Bir işi iptal etme

Aşağıdaki örnek, bir işin nasıl iptal edilebildiğini gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- View all active executions to determine job execution id
SELECT * FROM jobs.job_executions 
WHERE is_active = 1 AND job_name = 'ResultPoolsJob'
ORDER BY start_time DESC
GO

-- Cancel job execution with the specified job execution id
EXEC jobs.sp_stop_job '01234567-89ab-cdef-0123-456789abcdef'
```


## <a name="delete-old-job-history"></a>Eski iş geçmişini silme

Aşağıdaki örnek, belirli bir tarihten önce iş geçmişini nasıl silerken gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

-- Delete history of a specific job’s executions older than the specified date
EXEC jobs.sp_purge_jobhistory @job_name='ResultPoolsJob', @oldest_date='2016-07-01 00:00:00'

--Note: job history is automatically deleted if it is >45 days old
```

## <a name="delete-a-job-and-all-its-job-history"></a>Bir işi ve tüm iş geçmişini silme

Aşağıdaki örnek, bir işin nasıl silinip silinir ve ilgili tüm iş geçmişini gösterir.  
[*İş veritabanına*](sql-database-job-automation-overview.md#job-database) bağlanın ve aşağıdaki komutu çalıştırın:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_delete_job @job_name='ResultsPoolsJob'

--Note: job history is automatically deleted if it is >45 days old
```




## <a name="job-stored-procedures"></a>İş depolanan yordamlar

Aşağıdaki depolanan yordamlar [iş veritabanında](sql-database-job-automation-overview.md#job-database)dır.



|Saklı yordam  |Açıklama  |
|---------|---------|
|[sp_add_job](#sp_add_job)     |     Yeni bir iş ekler.    |
|[Sp_update_job](#sp_update_job)    |      Varolan bir işi güncelleştirir.   |
|[Sp_delete_job](#sp_delete_job)     |      Varolan bir işi siler.   |
|[Sp_add_jobstep](#sp_add_jobstep)    |    Bir işe bir adım ekler.     |
|[sp_update_jobstep](#sp_update_jobstep)     |     Bir iş adımLarını güncelleştirir.    |
|[sp_delete_jobstep](#sp_delete_jobstep)     |     İş adımLarını siler.    |
|[Sp_start_job](#sp_start_job)    |  Bir işi yürütmeye başlar.       |
|[sp_stop_job](#sp_stop_job)     |     İş infazını durdurur.   |
|[sp_add_target_group](#sp_add_target_group)    |     Bir hedef grup ekler.    |
|[sp_delete_target_group](#sp_delete_target_group)     |    Hedef grubu siler.     |
|[sp_add_target_group_member](#sp_add_target_group_member)     |    Hedef gruba bir veritabanı veya veritabanı grubu ekler.     |
|[sp_delete_target_group_member](#sp_delete_target_group_member)     |     Hedef grup üyesini hedef gruptan kaldırır.    |
|[Sp_purge_jobhistory](#sp_purge_jobhistory)    |    Bir işin geçmiş kayıtlarını kaldırır.     |





### <a name="sp_add_job"></a><a name="sp_add_job"></a>sp_add_job

Yeni bir iş ekler. 
  
#### <a name="syntax"></a>Sözdizimi  
  

```syntaxsql
[jobs].sp_add_job [ @job_name = ] 'job_name'  
    [ , [ @description = ] 'description' ]   
    [ , [ @enabled = ] enabled ]
    [ , [ @schedule_interval_type = ] schedule_interval_type ]  
    [ , [ @schedule_interval_count = ] schedule_interval_count ]   
    [ , [ @schedule_start_time = ] schedule_start_time ]   
    [ , [ @schedule_end_time = ] schedule_end_time ]   
    [ , [ @job_id = ] job_id OUTPUT ]
```

  
#### <a name="arguments"></a>Bağımsız Değişkenler  

[ ** \@job_name =** ] 'job_name'  
İşin adı. Ad benzersiz olmalı ve yüzdeyi içeremez (%) Karakter. job_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@açıklama =** ] 'açıklama'  
İşin tanımı. açıklama null varsayılan ı ile nvarchar(512) olduğunu. Açıklama atlanırsa, boş bir dize kullanılır.

[ ** \@etkin =** ] etkin  
İş programının etkin olup olmadığı. Etkin bit, varsayılan 0 (devre dışı). 0 ise, iş etkinleştirilen değildir ve zamanlamasına göre çalışmaz; ancak, el ile çalıştırılabilir. 1 ise, iş zamanlamasına göre çalışır ve el ile de çalıştırılabilir.

[ ** \@schedule_interval_type =**] schedule_interval_type  
Değer, işin ne zaman yürütüleceğini gösterir. schedule_interval_type bir kez varsayılan ı olan nvarchar(50) ve aşağıdaki değerlerden biri olabilir:
- 'Bir kez',
- 'Dakika',
- 'Saatler',
- 'Gün',
- 'Haftalar',
- 'Aylar'

[ ** \@schedule_interval_count =** ] schedule_interval_count  
İşin her yürütülmesi arasında meydana gelecek schedule_interval_count dönem sayısı. schedule_interval_count, varsayılan 1 ile int olduğunu. Değer 1'den büyük veya eşit olmalıdır.

[ ** \@schedule_start_time =** ] schedule_start_time  
İş yürütmenin başlayabileceğiniz tarih. schedule_start_time DATETIME2, 0001-01-01 00:00:00.000000 varsayılan ile.

[ ** \@schedule_end_time =** ] schedule_end_time  
İş yürütmenin durabileceği tarih. schedule_end_time DATETIME2, 9999-12-31 11:59:59.000000 000 000 0000 0000 00000. 

[ ** \@job_id =** ] job_id Çıktı  
Başarılı bir şekilde oluşturulursa işe atanan iş kimlik numarası. job_id tip benzersiz bir çıkış değişkenidir.

#### <a name="return-code-values"></a>İade Kodu Değerleri

0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
sp_add_job iş aracısı oluştururken belirtilen iş aracısı veritabanından çalıştırılmalıdır.
İş eklemek için sp_add_job yürütüldükten sonra, sp_add_jobstep iş için etkinlikleri gerçekleştiren adımlar eklemek için kullanılabilir. İlk sürüm numarası 0'dır ve ilk adım eklendiğinde 1'e artılacaktır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:

- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.

### <a name="sp_update_job"></a><a name="sp_update_job"></a>Sp_update_job

Varolan bir işi güncelleştirir.

#### <a name="syntax"></a>Sözdizimi

```syntaxsql
[jobs].sp_update_job [ @job_name = ] 'job_name'  
    [ , [ @new_name = ] 'new_name' ]
    [ , [ @description = ] 'description' ]   
    [ , [ @enabled = ] enabled ]
    [ , [ @schedule_interval_type = ] schedule_interval_type ]  
    [ , [ @schedule_interval_count = ] schedule_interval_count ]   
    [ , [ @schedule_start_time = ] schedule_start_time ]   
    [ , [ @schedule_end_time = ] schedule_end_time ]   
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@job_name =** ] 'job_name'  
Güncellenecek işin adı. job_name nvarchar'dır(128).

[ ** \@new_name =** ] 'new_name'  
İşin yeni adı. new_name nvarchar'dır(128).

[ ** \@açıklama =** ] 'açıklama'  
İşin tanımı. açıklaması nvarchar'dır(512).

[ ** \@etkin =** ] etkin  
İş programının etkin olup olmadığını (1) veya etkin olmadığını (0) belirtir. Etkin bittir.

[ ** \@schedule_interval_type=** ] schedule_interval_type  
Değer, işin ne zaman yürütüleceğini gösterir. schedule_interval_type nvarchar(50) ve aşağıdaki değerlerden biri olabilir:

- 'Bir kez',
- 'Dakika',
- 'Saatler',
- 'Gün',
- 'Haftalar',
- 'Aylar'

[ ** \@schedule_interval_count=** ] schedule_interval_count  
İşin her yürütülmesi arasında meydana gelecek schedule_interval_count dönem sayısı. schedule_interval_count, varsayılan 1 ile int olduğunu. Değer 1'den büyük veya eşit olmalıdır.

[ ** \@schedule_start_time=** ] schedule_start_time  
İş yürütmenin başlayabileceğiniz tarih. schedule_start_time DATETIME2, 0001-01-01 00:00:00.000000 varsayılan ile.

[ ** \@schedule_end_time=** ] schedule_end_time  
İş yürütmenin durabileceği tarih. schedule_end_time DATETIME2, 9999-12-31 11:59:59.000000 000 000 0000 0000 00000. 

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
İş eklemek için sp_add_job yürütüldükten sonra, sp_add_jobstep iş için etkinlikleri gerçekleştiren adımlar eklemek için kullanılabilir. İlk sürüm numarası 0'dır ve ilk adım eklendiğinde 1'e artılacaktır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.



### <a name="sp_delete_job"></a><a name="sp_delete_job"></a>Sp_delete_job

Varolan bir işi siler.

#### <a name="syntax"></a>Sözdizimi

```syntaxsql
[jobs].sp_delete_job [ @job_name = ] 'job_name'
    [ , [ @force = ] force ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@job_name =** ] 'job_name'  
Silinecek işin adı. job_name nvarchar'dır(128).

[ ** \@kuvvet =** ] kuvvet  
İşin devam eden herhangi bir yürütmesi olup olmadığını silip silemeyeceğini ve devam eden tüm yürütmeleri iptal edip etmeyeceğini (1) veya herhangi bir iş yürütmedevam ediyorsa başarısız olup olmadığını belirtir (0). kuvvet bittir.

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
Bir iş silindiğinde iş geçmişi otomatik olarak silinir.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.



### <a name="sp_add_jobstep"></a><a name="sp_add_jobstep"></a>Sp_add_jobstep

Bir işe bir adım ekler.

#### <a name="syntax"></a>Sözdizimi


```syntaxsql
[jobs].sp_add_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] step_name ]   
     [ , [ @command_type = ] 'command_type' ]   
     [ , [ @command_source = ] 'command_source' ]  
     , [ @command = ] 'command'
     , [ @credential_name = ] 'credential_name'
     , [ @target_group_name = ] 'target_group_name'
     [ , [ @initial_retry_interval_seconds = ] initial_retry_interval_seconds ]   
     [ , [ @maximum_retry_interval_seconds = ] maximum_retry_interval_seconds ]   
     [ , [ @retry_interval_backoff_multiplier = ] retry_interval_backoff_multiplier ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @step_timeout_seconds = ] step_timeout_seconds ]   
     [ , [ @output_type = ] 'output_type' ]   
     [ , [ @output_credential_name = ] 'output_credential_name' ]   
     [ , [ @output_subscription_id = ] 'output_subscription_id' ]   
     [ , [ @output_resource_group_name = ] 'output_resource_group_name' ]   
     [ , [ @output_server_name = ] 'output_server_name' ]   
     [ , [ @output_database_name = ] 'output_database_name' ]   
     [ , [ @output_schema_name = ] 'output_schema_name' ]   
     [ , [ @output_table_name = ] 'output_table_name' ]
     [ , [ @job_version = ] job_version OUTPUT ]
     [ , [ @max_parallelism = ] max_parallelism ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler

[ ** \@job_name =** ] 'job_name'  
Adımı eklemek için işin adı. job_name nvarchar'dır(128).

[ ** \@step_id =** ] step_id  
İş adımı için sıra tanımlama numarası. Adım tanımlama numaraları 1'den başlar ve boşluksuz olarak artış. Varolan bir adımda zaten bu kimlik varsa, bu adım ve aşağıdaki tüm adımlar, bu yeni adımın diziye eklenabilmesi için kimliklerinin artımını alacaktır. Belirtilmezse, step_id otomatik olarak adım sırasına göre sonuncuya atanır. step_id bir int.

[ ** \@step_name =** ] step_name  
Adımın adı. (Kolaylık için) varsayılan adı 'JobStep' olan bir işin ilk adımı dışında belirtilmelidir. step_name nvarchar'dır(128).

[ ** \@command_type =** ] 'command_type'  
Bu iş adımı tarafından yürütülen komut türü. command_type nvarchar(50), TSql varsayılan değeri ile, @command_type parametre değeri bir T-SQL komut dosyası anlamına gelir.

Belirtilirse, değer TSql olmalıdır.

[ ** \@command_source =** ] 'command_source'  
Komutun depolandığı konum türü. command_source nvarchar(50), Inline varsayılan değeri ile, @command_source parametre değeri komutunun gerçek metin anlamına gelir.

Belirtilirse, değer Satır Lı olmalıdır.

[ ** \@komut =** ] 'komut'  
Komut geçerli T-SQL komut dosyası olmalıdır ve daha sonra bu iş adımı tarafından yürütülür. komutu null varsayılan ı ile nvarchar(max) olduğunu.

[ ** \@credential_name =** ] 'credential_name'  
Bu adım yürütüldüğünde hedef grup içindeki hedef veritabanlarının her birine bağlanmak için kullanılan bu iş denetimi veritabanında depolanan veritabanı kapsamı kapsamı kimlik bilgisi. credential_name nvarchar'dır(128).

[ ** \@target_group_name =** ] 'hedef group_name'  
İş adımının yürütüleceği hedef veritabanlarını içeren hedef grubun adı. target_group_name nvarchar'dır(128).

[ ** \@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
İlk yürütme girişiminde iş adımı başarısız olursa, ilk yeniden deneme girişiminden önceki gecikme. initial_retry_interval_seconds, varsayılan değeri 1 olan int'tir.

[ ** \@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
Yeniden deneme girişimleri arasındaki maksimum gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük büyüyecekse, bunun yerine bu değere kapaklanır. maximum_retry_interval_seconds, varsayılan değeri 120 olan int'tir.

[ ** \@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
Birden çok iş adımı yürütme girişimi başarısız olursa yeniden deneme gecikmesine uygulanacak çarpan. Örneğin, ilk yeniden denemede 5 saniyelik bir gecikme varsa ve geri dönüş çarpanı 2,0 ise, ikinci yeniden denemede 10 saniye gecikme olur ve üçüncü yeniden denemede 20 saniye gecikme olur. retry_interval_backoff_multiplier, varsayılan değeri 2,0 olan gerçektir.

[ ** \@retry_attempts =** ] retry_attempts  
İlk deneme başarısız olursa yürütmeyi yeniden deneme sayısı. Örneğin, retry_attempts değeri 10 ise, toplam 11 deneme vererek 1 ilk deneme ve 10 yeniden deneme olacaktır. Son yeniden deneme girişimi başarısız olursa, iş yürütme Başarısız bir yaşam döngüsü ile sona erer. retry_attempts, varsayılan değeri 10 olan int'tir.

[ ** \@step_timeout_seconds =** ] step_timeout_seconds  
Adımın yürütülmesi için izin verilen maksimum süre. Bu süre aşılırsa, iş yürütme TimedOut bir yaşam döngüsü ile sona erer. step_timeout_seconds, varsayılan değeri 43.200 saniye (12 saat) olan int'tir.

[ ** \@output_type =** ] 'output_type'  
Null değilse, komutun ilk sonuç kümesinin yazıldığı hedef türü. output_type nvarchar(50), NULL varsayılan ı ile.

Belirtilirse, değer SqlDatabase olmalıdır.

[ ** \@output_credential_name =** ] 'output_credential_name'  
Null değilse, çıktı hedef veritabanına bağlanmak için kullanılan veritabanı kapsamı kimlik bilgisi. output_type sqldatabase eşittir belirtilmelidir. output_credential_name nvarchar(128), NULL varsayılan değeri ile.

[ ** \@output_subscription_id =** ] 'output_subscription_id'  
Açıklamaya ihtiyacı var.

[ ** \@output_resource_group_name =** ] 'output_resource_group_name'  
Açıklamaya ihtiyacı var.

[ ** \@output_server_name =** ] 'output_server_name'  
Null değilse, çıktı hedef veritabanını içeren sunucunun tam nitelikli DNS adı. output_type sqldatabase eşittir belirtilmelidir. output_server_name nvarchar(256), NULL varsayılan ı ile.

[ ** \@output_database_name =** ] 'output_database_name'  
Null değilse, çıktı hedef tablosunu içeren veritabanının adı. output_type sqldatabase eşittir belirtilmelidir. output_database_name nvarchar(128), NULL varsayılan ı ile.

[ ** \@output_schema_name =** ] 'output_schema_name'  
Null değilse, çıktı hedef tablosunu içeren SQL şemasının adı. output_type SqlDatabase'e eşitse, varsayılan değer dbo'dur. output_schema_name nvarchar'dır(128).

[ ** \@output_table_name =** ] 'output_table_name'  
Null değilse, komutun ilk sonuç kümesinin yazıldığı tablonun adı yazılır. Tablo zaten yoksa, dönen sonuç kümesinin şemasına göre oluşturulur. output_type sqldatabase eşittir belirtilmelidir. output_table_name nvarchar(128), NULL varsayılan değeri ile.

[ ** \@job_version =** ] job_version Çıktı  
Yeni iş sürüm numarası atanacak çıktı parametresi. job_version int olduğunu.

[ ** \@max_parallelism =** ] max_parallelism ÇıKTı  
Elastik havuz başına maksimum paralellik düzeyi. Ayarlanırsa, iş adımı yalnızca elastik havuz başına bu kadar çok veritabanlarının en fazla üzerinde çalışacak şekilde sınırlandırılacaktır. Bu, doğrudan hedef gruba dahil olan veya hedef gruba dahil olan bir sunucunun içinde bulunan her elastik havuz için geçerlidir. max_parallelism int olduğunu.


#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
sp_add_jobstep başarılı olduğunda, işin geçerli sürüm numarası artımlanır. Bir sonraki iş yürütüldolduğunda, yeni sürüm kullanılır. İş şu anda yürütülmektese, bu yürütme yeni adımı içermez.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:  

- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.



### <a name="sp_update_jobstep"></a><a name="sp_update_jobstep"></a>sp_update_jobstep

Bir iş adımLarını güncelleştirir.

#### <a name="syntax"></a>Sözdizimi

```syntaxsql
[jobs].sp_update_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @new_id = ] new_id ]   
     [ , [ @new_name = ] 'new_name' ]   
     [ , [ @command_type = ] 'command_type' ]   
     [ , [ @command_source = ] 'command_source' ]  
     , [ @command = ] 'command'
     , [ @credential_name = ] 'credential_name'
     , [ @target_group_name = ] 'target_group_name'
     [ , [ @initial_retry_interval_seconds = ] initial_retry_interval_seconds ]   
     [ , [ @maximum_retry_interval_seconds = ] maximum_retry_interval_seconds ]   
     [ , [ @retry_interval_backoff_multiplier = ] retry_interval_backoff_multiplier ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @step_timeout_seconds = ] step_timeout_seconds ]   
     [ , [ @output_type = ] 'output_type' ]   
     [ , [ @output_credential_name = ] 'output_credential_name' ]   
     [ , [ @output_server_name = ] 'output_server_name' ]   
     [ , [ @output_database_name = ] 'output_database_name' ]   
     [ , [ @output_schema_name = ] 'output_schema_name' ]   
     [ , [ @output_table_name = ] 'output_table_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
     [ , [ @max_parallelism = ] max_parallelism ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@job_name =** ] 'job_name'  
Adımın ait olduğu işin adı. job_name nvarchar'dır(128).

[ ** \@step_id =** ] step_id  
Değiştirilecek iş adımı için kimlik numarası. step_id veya step_name belirtilmelidir. step_id bir int.

[ ** \@step_name =** ] 'step_name'  
Değiştirilecek adımın adı. step_id veya step_name belirtilmelidir. step_name nvarchar'dır(128).

[ ** \@new_id =** ] new_id  
İş adımı için yeni sıra tanımlama numarası. Adım tanımlama numaraları 1'den başlar ve boşluksuz olarak artış. Bir adım yeniden sıralanırsa, diğer adımlar otomatik olarak yeniden numaralandırılır.

[ ** \@new_name =** ] 'new_name'  
Adımın yeni adı. new_name nvarchar'dır(128).

[ ** \@command_type =** ] 'command_type'  
Bu iş adımı tarafından yürütülen komut türü. command_type nvarchar(50), TSql varsayılan değeri ile, @command_type parametre değeri bir T-SQL komut dosyası anlamına gelir.

Belirtilirse, değer TSql olmalıdır.

[ ** \@command_source =** ] 'command_source'  
Komutun depolandığı konum türü. command_source nvarchar(50), Inline varsayılan değeri ile, @command_source parametre değeri komutunun gerçek metin anlamına gelir.

Belirtilirse, değer Satır Lı olmalıdır.

[ ** \@komut =** ] 'komut'  
Komut(lar) geçerli T-SQL komut dosyası olmalıdır ve daha sonra bu iş adımı tarafından yürütülür. komutu null varsayılan ı ile nvarchar(max) olduğunu.

[ ** \@credential_name =** ] 'credential_name'  
Bu adım yürütüldüğünde hedef grup içindeki hedef veritabanlarının her birine bağlanmak için kullanılan bu iş denetimi veritabanında depolanan veritabanı kapsamı kapsamı kimlik bilgisi. credential_name nvarchar'dır(128).

[ ** \@target_group_name =** ] 'hedef group_name'  
İş adımının yürütüleceği hedef veritabanlarını içeren hedef grubun adı. target_group_name nvarchar'dır(128).

[ ** \@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
İlk yürütme girişiminde iş adımı başarısız olursa, ilk yeniden deneme girişiminden önceki gecikme. initial_retry_interval_seconds, varsayılan değeri 1 olan int'tir.

[ ** \@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
Yeniden deneme girişimleri arasındaki maksimum gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük büyüyecekse, bunun yerine bu değere kapaklanır. maximum_retry_interval_seconds, varsayılan değeri 120 olan int'tir.

[ ** \@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
Birden çok iş adımı yürütme girişimi başarısız olursa yeniden deneme gecikmesine uygulanacak çarpan. Örneğin, ilk yeniden denemede 5 saniyelik bir gecikme varsa ve geri dönüş çarpanı 2,0 ise, ikinci yeniden denemede 10 saniye gecikme olur ve üçüncü yeniden denemede 20 saniye gecikme olur. retry_interval_backoff_multiplier, varsayılan değeri 2,0 olan gerçektir.

[ ** \@retry_attempts =** ] retry_attempts  
İlk deneme başarısız olursa yürütmeyi yeniden deneme sayısı. Örneğin, retry_attempts değeri 10 ise, toplam 11 deneme vererek 1 ilk deneme ve 10 yeniden deneme olacaktır. Son yeniden deneme girişimi başarısız olursa, iş yürütme Başarısız bir yaşam döngüsü ile sona erer. retry_attempts, varsayılan değeri 10 olan int'tir.

[ ** \@step_timeout_seconds =** ] step_timeout_seconds  
Adımın yürütülmesi için izin verilen maksimum süre. Bu süre aşılırsa, iş yürütme TimedOut bir yaşam döngüsü ile sona erer. step_timeout_seconds, varsayılan değeri 43.200 saniye (12 saat) olan int'tir.

[ ** \@output_type =** ] 'output_type'  
Null değilse, komutun ilk sonuç kümesinin yazıldığı hedef türü. output_type değerini NULL'a geri ayarlamak için, bu parametrenin değerini '' (boş dize) olarak ayarlayın. output_type nvarchar(50), NULL varsayılan ı ile.

Belirtilirse, değer SqlDatabase olmalıdır.

[ ** \@output_credential_name =** ] 'output_credential_name'  
Null değilse, çıktı hedef veritabanına bağlanmak için kullanılan veritabanı kapsamı kimlik bilgisi. output_type sqldatabase eşittir belirtilmelidir. output_credential_name değerini NULL'a sıfırlamak için bu parametrenin değerini '' (boş dize) olarak ayarlayın. output_credential_name nvarchar(128), NULL varsayılan değeri ile.

[ ** \@output_server_name =** ] 'output_server_name'  
Null değilse, çıktı hedef veritabanını içeren sunucunun tam nitelikli DNS adı. output_type sqldatabase eşittir belirtilmelidir. null output_server_name değerini sıfırlamak için, bu parametrenin değerini '' (boş dize) olarak ayarlayın. output_server_name nvarchar(256), NULL varsayılan ı ile.

[ ** \@output_database_name =** ] 'output_database_name'  
Null değilse, çıktı hedef tablosunu içeren veritabanının adı. output_type sqldatabase eşittir belirtilmelidir. output_database_name değerini NULL'a sıfırlamak için bu parametrenin değerini '' (boş dize) olarak ayarlayın. output_database_name nvarchar(128), NULL varsayılan ı ile.

[ ** \@output_schema_name =** ] 'output_schema_name'  
Null değilse, çıktı hedef tablosunu içeren SQL şemasının adı. output_type SqlDatabase'e eşitse, varsayılan değer dbo'dur. output_schema_name değerini NULL'a geri ayarlamak için, bu parametrenin değerini '' (boş dize) olarak ayarlayın. output_schema_name nvarchar'dır(128).

[ ** \@output_table_name =** ] 'output_table_name'  
Null değilse, komutun ilk sonuç kümesinin yazıldığı tablonun adı yazılır. Tablo zaten yoksa, dönen sonuç kümesinin şemasına göre oluşturulur. output_type sqldatabase eşittir belirtilmelidir. null output_server_name değerini sıfırlamak için, bu parametrenin değerini '' (boş dize) olarak ayarlayın. output_table_name nvarchar(128), NULL varsayılan değeri ile.

[ ** \@job_version =** ] job_version Çıktı  
Yeni iş sürüm numarası atanacak çıktı parametresi. job_version int olduğunu.

[ ** \@max_parallelism =** ] max_parallelism ÇıKTı  
Elastik havuz başına maksimum paralellik düzeyi. Ayarlanırsa, iş adımı yalnızca elastik havuz başına bu kadar çok veritabanlarının en fazla üzerinde çalışacak şekilde sınırlandırılacaktır. Bu, doğrudan hedef gruba dahil olan veya hedef gruba dahil olan bir sunucunun içinde bulunan her elastik havuz için geçerlidir. max_parallelism değerini sıfırlamak için bu parametrenin değerini -1 olarak ayarlayın. max_parallelism int olduğunu.


#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
İşin devam eden yürütmeleri etkilenmez. sp_update_jobstep başarılı olduğunda, işin sürüm numarası artımlanır. Bir sonraki iş yürütüldolduğunda, yeni sürüm kullanılır.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:

- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir




### <a name="sp_delete_jobstep"></a><a name="sp_delete_jobstep"></a>sp_delete_jobstep

Bir iş adımLarını bir işten kaldırır.

#### <a name="syntax"></a>Sözdizimi


```syntaxsql
[jobs].sp_delete_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@job_name =** ] 'job_name'  
Adımın kaldırılacağı işin adı. job_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@step_id =** ] step_id  
İş adımının silinecek kimlik numarası. step_id veya step_name belirtilmelidir. step_id bir int.

[ ** \@step_name =** ] 'step_name'  
Silinecek adımın adı. step_id veya step_name belirtilmelidir. step_name nvarchar'dır(128).

[ ** \@job_version =** ] job_version Çıktı  
Yeni iş sürüm numarası atanacak çıktı parametresi. job_version int olduğunu.

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
İşin devam eden yürütmeleri etkilenmez. sp_update_jobstep başarılı olduğunda, işin sürüm numarası artımlanır. Bir sonraki iş yürütüldolduğunda, yeni sürüm kullanılır.

Silinen iş adımından kalan boşluğu doldurmak için diğer iş adımları otomatik olarak yeniden numaralandırılır.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.






### <a name="sp_start_job"></a><a name="sp_start_job"></a>Sp_start_job

Bir işi yürütmeye başlar.

#### <a name="syntax"></a>Sözdizimi


```syntaxsql
[jobs].sp_start_job [ @job_name = ] 'job_name'   
     [ , [ @job_execution_id = ] job_execution_id OUTPUT ]   
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@job_name =** ] 'job_name'  
Adımın kaldırılacağı işin adı. job_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@job_execution_id =** ] job_execution_id ÇıKTı  
İş yürütme kimliğinin atanacak çıktı parametresi. job_version benzersiz bir tanımlayıcıdır.

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
Yok.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.

### <a name="sp_stop_job"></a><a name="sp_stop_job"></a>sp_stop_job

İş infazını durdurur.

#### <a name="syntax"></a>Sözdizimi


```syntaxsql
[jobs].sp_stop_job [ @job_execution_id = ] ' job_execution_id '
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@job_execution_id =** ] job_execution_id  
Durdurulacak iş yürütmekimlik numarası. job_execution_id, NULL varsayılanı ile benzersiz bir tanımlayıcıdır.

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
Yok.
 
#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.


### <a name="sp_add_target_group"></a><a name="sp_add_target_group"></a>sp_add_target_group

Bir hedef grup ekler.

#### <a name="syntax"></a>Sözdizimi


```syntaxsql
[jobs].sp_add_target_group [ @target_group_name = ] 'target_group_name'   
     [ , [ @target_group_id = ] target_group_id OUTPUT ]
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@target_group_name =** ] 'target_group_name'  
Oluşturulacak hedef grubun adı. target_group_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@target_group_id = ]** target_group_id OUTPUT Başarılı bir şekilde oluşturulduğunda işe atanan hedef grup kimlik numarası. target_group_id, NULL varsayılanı olan tip benzersiz tanımlayıcısının bir çıktı değişkenidir.

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
Hedef gruplar, veritabanları koleksiyonundaki bir işi hedeflemenin kolay bir yolunu sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.

### <a name="sp_delete_target_group"></a><a name="sp_delete_target_group"></a>sp_delete_target_group

Hedef grubu siler.

#### <a name="syntax"></a>Sözdizimi


```syntaxsql
[jobs].sp_delete_target_group [ @target_group_name = ] 'target_group_name'
```


#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@target_group_name =** ] 'target_group_name'  
Silmek için hedef grubun adı. target_group_name nvarchar(128), hiçbir varsayılan ile.

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
Yok.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.

### <a name="sp_add_target_group_member"></a><a name="sp_add_target_group_member"></a>sp_add_target_group_member

Hedef gruba bir veritabanı veya veritabanı grubu ekler.

#### <a name="syntax"></a>Sözdizimi

```syntaxsql
[jobs].sp_add_target_group_member [ @target_group_name = ] 'target_group_name'
         [ @membership_type = ] 'membership_type' ]   
        [ , [ @target_type = ] 'target_type' ]   
        [ , [ @refresh_credential_name = ] 'refresh_credential_name' ]   
        [ , [ @server_name = ] 'server_name' ]   
        [ , [ @database_name = ] 'database_name' ]   
        [ , [ @elastic_pool_name = ] 'elastic_pool_name' ]   
        [ , [ @shard_map_name = ] 'shard_map_name' ]   
        [ , [ @target_id = ] 'target_id' OUTPUT ]
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@target_group_name =** ] 'target_group_name'  
Üyenin ekeceği hedef grubun adı. target_group_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@membership_type = ]** 'membership_type'  
Hedef grup üyesinin dahil edilip edilmeyeceğini veya dahil edilip edilmeyeceğini belirtir. target_group_name nvarchar(128), varsayılan olarak 'Include' ile. target_group_name için geçerli değerler 'Dahil Et' veya 'Hariç Tut' değeridir.

[ ** \@target_type =** ] 'target_type'  
Sunucudaki tüm veritabanları, Elastik havuzdaki tüm veritabanları, parça haritadaki tüm veritabanları veya tek bir veritabanı dahil olmak üzere hedef veritabanı veya veritabanlarının toplanması türü. target_type nvarchar(128), hiçbir varsayılan ile. target_type için geçerli değerler 'SqlServer', 'SqlElasticPool', 'SqlDatabase'veya 'SqlShardMap'tir. 

[ ** \@refresh_credential_name =** ] 'refresh_credential_name'  
SQL Veritabanı sunucusunun adı. refresh_credential_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@server_name =** ] 'server_name'  
Belirtilen hedef gruba eklenmesi gereken SQL Veritabanı sunucusunun adı. server_name target_type 'SqlServer' olduğunda belirtilmelidir. server_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@database_name =** ] 'database_name'  
Belirtilen hedef gruba eklenmesi gereken veritabanının adı. target_type 'SqlDatabase' olduğunda database_name belirtilmelidir. database_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@elastic_pool_name =** ] 'elastic_pool_name'  
Belirtilen hedef gruba eklenmesi gereken Elastik havuzun adı. target_type 'SqlElasticPool' olduğunda elastic_pool_name belirtilmelidir. elastic_pool_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@shard_map_name =** ] 'shard_map_name'  
Belirtilen hedef gruba eklenmesi gereken parça harita havuzunun adı. elastic_pool_name target_type 'SqlSqlShardMap' olduğunda belirtilmelidir. shard_map_name nvarchar(128), hiçbir varsayılan ile.

[ ** \@target_id =** ] target_group_id Çıktı  
Oluşturulan hedef gruba eklenirse hedef grup üyesine atanan hedef tanımlama numarası. target_id, NULL varsayılanı olan tip benzersiz identifier bir çıkış değişkenidir.
İade Kodu Değerleri 0 (başarılı) veya 1 (hata)

#### <a name="remarks"></a>Açıklamalar
Bir iş, bir SQL Veritabanı sunucusu veya Elastik havuz hedef gruba dahil edildiğinde, yürütme sırasında bir SQL Veritabanı sunucusu içindeki tüm tek veritabanlarında veya elastik bir havuzda yürütülür.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnek, Londra ve NewYork sunucularında bulunan tüm veritabanlarını Müşteri Bilgilerini Koruyan Sunucular grubuna ekler. Bu durumda ElasticJobs, iş aracısını oluştururken belirtilen iş veritabanına bağlanmanız gerekir.

```sql
--Connect to the jobs database specified when creating the job agent
USE ElasticJobs ; 
GO

-- Add a target group containing server(s)
EXEC jobs.sp_add_target_group @target_group_name =  N'Servers Maintaining Customer Information'
GO

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'London.database.windows.net' ;
GO

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'NewYork.database.windows.net' ;
GO

--View the recently added members to the target group
SELECT * FROM [jobs].target_group_members WHERE target_group_name= N'Servers Maintaining Customer Information';
GO
```

### <a name="sp_delete_target_group_member"></a><a name="sp_delete_target_group_member"></a>sp_delete_target_group_member

Hedef grup üyesini hedef gruptan kaldırır.

#### <a name="syntax"></a>Sözdizimi


```syntaxsql
[jobs].sp_delete_target_group_member [ @target_group_name = ] 'target_group_name'
        [ , [ @target_id = ] 'target_id']
```



Bağımsız @target_group_name değişkenler [ = ] 'target_group_name'  
Hedef grup üyesini kaldırmak için hedef grubun adı. target_group_name nvarchar(128), hiçbir varsayılan ile.

[ @target_id = ] target_id  
 Kaldırılacak hedef grup üyesine atanan hedef kimlik numarası. target_id, NULL varsayılanı olan bir benzersiz leştiricidir.

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (başarısızlık)

#### <a name="remarks"></a>Açıklamalar
Hedef gruplar, veritabanları koleksiyonundaki bir işi hedeflemenin kolay bir yolunu sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnek, Londra sunucusunu Müşteri Bilgilerini Koruyan Sunucular grubundan kaldırır. Bu durumda ElasticJobs, iş aracısını oluştururken belirtilen iş veritabanına bağlanmanız gerekir.

```sql
--Connect to the jobs database specified when creating the job agent
USE ElasticJobs ; 
GO

-- Retrieve the target_id for a target_group_members
declare @tid uniqueidentifier
SELECT @tid = target_id FROM [jobs].target_group_members WHERE target_group_name = 'Servers Maintaining Customer Information' and server_name = 'London.database.windows.net'

-- Remove a target group member of type server
EXEC jobs.sp_delete_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_id = @tid
GO
```

### <a name="sp_purge_jobhistory"></a><a name="sp_purge_jobhistory"></a>Sp_purge_jobhistory

Bir işin geçmiş kayıtlarını kaldırır.

#### <a name="syntax"></a>Sözdizimi


```syntaxsql
[jobs].sp_purge_jobhistory [ @job_name = ] 'job_name'   
      [ , [ @job_id = ] job_id ]
      [ , [ @oldest_date = ] oldest_date []
```

#### <a name="arguments"></a>Bağımsız Değişkenler
[ ** \@job_name =** ] 'job_name'  
Geçmiş kayıtlarını silmek için işin adı. job_name nvarchar(128), NULL varsayılan ı ile. job_id veya job_name belirtilmelidir, ancak her ikisi de belirtilemez.

[ ** \@job_id =** ] job_id  
 Kayıtların silinmesi için işin iş kimlik numarası. job_id, NULL varsayılanı ile benzersiz bir tanımlayıcıdır. job_id veya job_name belirtilmelidir, ancak her ikisi de belirtilemez.

[ ** \@oldest_date =** ] oldest_date  
 Tarihte tutulan en eski kayıt. oldest_date DATETIME2, NULL varsayılan ı ile. oldest_date belirtildiğinde, sp_purge_jobhistory yalnızca belirtilen değerden eski kayıtları kaldırır.

#### <a name="return-code-values"></a>İade Kodu Değerleri
0 (başarı) veya 1 (hata) Açıklamalar Hedef gruplar veritabanları koleksiyonunda bir işi hedeflemek için kolay bir yol sağlar.

#### <a name="permissions"></a>İzinler
Varsayılan olarak, sysadmin sabit sunucu rolünün üyeleri bu depolanan yordamı yürütebilir. Bir kullanıcıyı yalnızca işleri izleyebilmeyi kısıtlarlar, kullanıcıya iş aracısını oluştururken belirtilen iş aracısı veritabanında aşağıdaki veritabanı rolünün bir parçası olmasını sağlayabilirsiniz:
- jobs_reader

Bu rollerin izinleri hakkında ayrıntılı bilgi için bu belgedeki İzin bölümüne bakın. Yalnızca sysadmin üyeleri, diğer kullanıcılara ait işlerin özniteliklerini düzenlemek için bu saklı yordamı kullanabilir.

#### <a name="examples"></a>Örnekler
Aşağıdaki örnek, Londra ve NewYork sunucularında bulunan tüm veritabanlarını Müşteri Bilgilerini Koruyan Sunucular grubuna ekler. Bu durumda ElasticJobs, iş aracısını oluştururken belirtilen iş veritabanına bağlanmanız gerekir.

```sql
--Connect to the jobs database specified when creating the job agent

EXEC sp_delete_target_group_member   
    @target_group_name = N'Servers Maintaining Customer Information',  
    @server_name = N'London.database.windows.net';  
GO
```


## <a name="job-views"></a>İş görünümleri

İş [veritabanında](sql-database-job-automation-overview.md#job-database)aşağıdaki görünümler mevcuttur.


|Görüntüle  |Açıklama  |
|---------|---------|
|[job_executions](#job_executions-view)     |  İş yürütme geçmişini gösterir.      |
|[Işleri](#jobs-view)     |   Tüm işleri gösterir.      |
|[job_versions](#job_versions-view)     |   Tüm iş sürümlerini gösterir.      |
|[Jobsteps](#jobsteps-view)     |     Her işin geçerli sürümündeki tüm adımları gösterir.    |
|[jobstep_versions](#jobstep_versions-view)     |     Her işin tüm sürümlerindeki tüm adımları gösterir.    |
|[target_groups](#target_groups-view)     |      Tüm hedef grupları gösterir.   |
|[target_group_members](#target_group_members-view)     |   Tüm hedef grupların tüm üyelerini gösterir.      |


### <a name="job_executions-view"></a><a name="job_executions-view"></a>job_executions görünümü

[işleri]. [job_executions]

İş yürütme geçmişini gösterir.


|Sütun adı|   Veri türü   |Açıklama|
|---------|---------|---------|
|**job_execution_id**   |uniqueidentifier|  Bir iş yürütme örneğinin benzersiz kimliği.
|**Job_name**   |nvarchar(128)  |İşin adı.
|**Job_id** |uniqueidentifier|  İşin benzersiz kimliği.
|**job_version**    |int    |İşin sürümü (iş her değiştirildiğinde otomatik olarak güncelleştirilir).
|**step_id**    |int|   Adım için benzersiz (bu iş için) tanımlayıcı. NULL, bunun ana iş yürütmesi olduğunu gösterir.
|**is_active**| bit |Bilgilerin etkin veya etkin olmadığını gösterir. 1 etkin işleri gösterir ve 0 etkin olmayan işleri gösterir.
|**Yaşam döngüsü**| nvarchar(50)|İşin durumunu gösteren değer:'Oluşturuldu', 'Devam Ediyor', 'Başarısız', 'Başarılı', 'Atlandı', 'BaşarılıWithSkipped'|
|**create_time**|   datetime2(7)|   İşoluşturulduğu tarih ve saat.
|**Start_time** |datetime2(7)|  İşin yürütmeye başladığı tarih ve saat. İş henüz yürütülmediyse NULL.
|**end_time**|  datetime2(7)    |İşin yürütmenin bittiği tarih ve saat. İş henüz yürütülmediyse veya yürütmeyi henüz tamamlamamışsa NULL.
|**current_attempts**   |int    |Adımın kaç kez yeniden denendiği. Üst iş 0 olacak, alt iş yürütmeler 1 veya daha fazla yürütme ilkesine göre olacaktır.
|**current_attempt_start_time** |datetime2(7)|  İşin yürütmeye başladığı tarih ve saat. NULL, bunun ana iş yürütmesi olduğunu gösterir.
|**last_message**   |nvarchar(maks)| İş veya adım geçmişi iletisi. 
|**target_type**|   nvarchar(128)   |Bir sunucudaki tüm veritabanları, Elastik havuzdaki tüm veritabanları veya veritabanı dahil olmak üzere hedef veritabanı veya veritabanları koleksiyonu türü. target_type için geçerli değerler 'SqlServer', 'SqlElasticPool' veya 'SqlDatabase' dir. NULL, bunun ana iş yürütmesi olduğunu gösterir.
|**target_id**  |uniqueidentifier|  Hedef grup üyesinin benzersiz kimliği.  NULL, bunun ana iş yürütmesi olduğunu gösterir.
|**target_group_name**  |nvarchar(128)  |Hedef grubun adı. NULL, bunun ana iş yürütmesi olduğunu gösterir.
|**target_server_name**|    nvarchar(256)|  Hedef grupta bulunan SQL Veritabanı sunucusunun adı. Yalnızca target_type 'SqlServer' ise belirtilir. NULL, bunun ana iş yürütmesi olduğunu gösterir.
|**target_database_name**   |nvarchar(128)| Hedef grupta bulunan veritabanının adı. Yalnızca target_type 'SqlDatabase' olduğunda belirtilir. NULL, bunun ana iş yürütmesi olduğunu gösterir.


### <a name="jobs-view"></a>iş görünümü

[işleri]. [işler]

Tüm işleri gösterir.

|Sütun adı|   Veri türü|  Açıklama|
|------|------|-------|
|**Job_name**|  nvarchar(128)   |İşin adı.|
|**Job_id**|    uniqueidentifier    |İşin benzersiz kimliği.|
|**job_version**    |int    |İşin sürümü (iş her değiştirildiğinde otomatik olarak güncelleştirilir).|
|**Açıklama**    |nvarchar(512)| İş için açıklama. etkinleştirilmiş bit, işin etkin mi yoksa devre dışı mı olduğunu gösterir. 1 etkin işleri, 0 ise devre dışı bırakılmış işleri gösterir.|
|**schedule_interval_type** |nvarchar(50)   |İşin ne zaman yürütüleceğini gösteren değer:'Bir kez', 'Dakika', 'Saat', 'Gün', 'Haftalar', 'Aylar'
|**schedule_interval_count**|   int|    İşin her yürütülmesi arasında meydana gelecek schedule_interval_type süre sayısı.|
|**schedule_start_time**    |datetime2(7)|  İşin en son yürütünmeye başlandığı tarih ve saat.|
|**schedule_end_time**| datetime2(7)|   İşin en son yürütülmesinin tamamlandığı tarih ve saat.|


### <a name="job_versions-view"></a><a name="job_versions-view"></a>job_versions görünüm

[işleri]. [job_versions]

Tüm iş sürümlerini gösterir.

|Sütun adı|   Veri türü|  Açıklama|
|------|------|-------|
|**Job_name**|  nvarchar(128)   |İşin adı.|
|**Job_id**|    uniqueidentifier    |İşin benzersiz kimliği.|
|**job_version**    |int    |İşin sürümü (iş her değiştirildiğinde otomatik olarak güncelleştirilir).|


### <a name="jobsteps-view"></a>iş adımları görünümü

[işleri]. [iş adımları]

Her işin geçerli sürümündeki tüm adımları gösterir.

|Sütun adı    |Veri türü| Açıklama|
|------|------|-------|
|**Job_name**   |nvarchar(128)| İşin adı.|
|**Job_id** |uniqueidentifier   |İşin benzersiz kimliği.|
|**job_version**|   int|    İşin sürümü (iş her değiştirildiğinde otomatik olarak güncelleştirilir).|
|**step_id**    |int    |Adım için benzersiz (bu iş için) tanımlayıcı.|
|**step_name**  |nvarchar(128)  |Adım için benzersiz (bu iş için) adı.|
|**command_type**   |nvarchar(50)   |İş adımında yürütülecek komut türü. v1 için değer 'TSql'e eşit ve varsayılan olmalıdır.|
|**command_source** |nvarchar(50)|  Komutanlığın yeri. v1 için 'Satır Çizgisi' varsayılan ve yalnızca kabul edilen değerdir.|
|**Komut**|   nvarchar(maks)|  Elastik işler tarafından command_type yoluyla yürütülecek komutlar.|
|**credential_name**|   nvarchar(128)   |İşi yürütmek için kullanılan veritabanı kapsamı kimlik bilgisinin adı.|
|**target_group_name**| nvarchar(128)   |Hedef grubun adı.|
|**target_group_id**|   uniqueidentifier|   Hedef grubun benzersiz kimliği.|
|**initial_retry_interval_seconds**|    int |İlk yeniden deneme denemesinden önceki gecikme. Varsayılan değer 1'dir.|
|**maximum_retry_interval_seconds** |int|   Yeniden deneme girişimleri arasındaki maksimum gecikme. Yeniden denemeler arasındaki gecikme bu değerden daha büyük büyüyecekse, bunun yerine bu değere kapaklanır. Varsayılan değer 120'dir.|
|**retry_interval_backoff_multiplier**  |gerçek|  Birden çok iş adımı yürütme girişimi başarısız olursa yeniden deneme gecikmesine uygulanacak çarpan. Varsayılan değer 2.0'dır.|
|**retry_attempts** |int|   Bu adım başarısız olursa yeniden deneme denemelerinin sayısı. Varsayılan 10, hiçbir yeniden deneme girişimi gösterir.|
|**step_timeout_seconds**   |int|   Yeniden deneme denemeleri arasındaki dakika dakika daki süre. Varsayılan değer 0 dakikalık bir aralığı gösteren 0'dır.|
|**output_type**    |nvarchar(11)|  Komutanlığın yeri. Geçerli önizlemede, 'Satır İçi' varsayılan ve yalnızca kabul edilen değerdir.|
|**output_credential_name**|    nvarchar(128)   |Sonuçları kümesini depolamak için hedef sunucuya bağlanmak için kullanılacak kimlik bilgilerinin adı.|
|**output_subscription_id**|    uniqueidentifier|   Sorgu yürütmebelirlenen sonuçlar için hedef sunucu\veritabanı aboneliği benzersiz kimliği.|
|**output_resource_group_name** |nvarchar(128)| Hedef sunucunun bulunduğu kaynak grubu adı.|
|**output_server_name**|    nvarchar(256)   |Sonuçlar kümesi için hedef sunucunun adı.|
|**output_database_name**   |nvarchar(128)| Sonuç kümesi için hedef veritabanının adı.|
|**output_schema_name** |nvarchar(maks)| Hedef şema adı. Belirtilmemişse varsayılan olarak dbo'ya göre belirlenir.|
|**output_table_name**| nvarchar(maks)|  Sorgu sonuçlarından ayarlanan sonuçları depolamak için tablonun adı. Tablo, zaten yoksa, belirlenen sonuçların şemasına göre otomatik olarak oluşturulur. Şema, belirlenen sonuçların şemasıyla eşleşmelidir.|
|**max_parallelism**|   int|    İş adımının aynı anda çalıştırılacak elastik havuz başına en fazla veritabanları sayısı. Varsayılan, sınır olmadığı anlamına gelen NULL'dur. |


### <a name="jobstep_versions-view"></a><a name="jobstep_versions-view"></a>jobstep_versions görünümü

[işleri]. [jobstep_versions]

Her işin tüm sürümlerindeki tüm adımları gösterir. Şema [iş adımlarının aynısa.](#jobsteps-view)

### <a name="target_groups-view"></a><a name="target_groups-view"></a>target_groups görünümü

[işleri]. [target_groups]

Tüm hedef grupları listeler.

|Sütun adı|Veri türü| Açıklama|
|-----|-----|-----|
|**target_group_name**| nvarchar(128)   |Hedef grubun adı, veritabanları koleksiyonu. 
|**target_group_id**    |uniqueidentifier   |Hedef grubun benzersiz kimliği.

### <a name="target_group_members-view"></a><a name="target_group_members-view"></a>target_group_members görünümü

[işleri]. [target_group_members]

Tüm hedef grupların tüm üyelerini gösterir.

|Sütun adı|Veri türü| Açıklama|
|-----|-----|-----|
|**target_group_name**  |nvarchar(128|Hedef grubun adı, veritabanları koleksiyonu. |
|**target_group_id**    |uniqueidentifier   |Hedef grubun benzersiz kimliği.|
|**membership_type**    |int|   Hedef grup üyesinin hedef gruba dahil edilip edilemeyişveya dışlandığını belirtir. target_group_name için geçerli değerler 'Dahil Et' veya 'Hariç Tut' değeridir.|
|**target_type**    |nvarchar(128)| Bir sunucudaki tüm veritabanları, Elastik havuzdaki tüm veritabanları veya veritabanı dahil olmak üzere hedef veritabanı veya veritabanları koleksiyonu türü. target_type için geçerli değerler 'SqlServer', 'SqlElasticPool', 'SqlDatabase'veya 'SqlShardMap'tir.|
|**target_id**  |uniqueidentifier|  Hedef grup üyesinin benzersiz kimliği.|
|**refresh_credential_name**    |nvarchar(128)  |Hedef grup üyesine bağlanmak için kullanılan veritabanı kapsamı kimlik bilgisinin adı.|
|**subscription_id**    |uniqueidentifier|  Aboneliğin benzersiz kimliği.|
|**resource_group_name**    |nvarchar(128)| Hedef grup üyesinin bulunduğu kaynak grubunun adı.|
|**Server_name**    |nvarchar(128)  |Hedef grupta bulunan SQL Veritabanı sunucusunun adı. Yalnızca target_type 'SqlServer' ise belirtilir. |
|**Veritabanı_adı**  |nvarchar(128)  |Hedef grupta bulunan veritabanının adı. Yalnızca target_type 'SqlDatabase' olduğunda belirtilir.|
|**elastic_pool_name**  |nvarchar(128)| Hedef grupta bulunan Elastik havuzun adı. Yalnızca target_type 'SqlElasticPool' olduğunda belirtilir.|
|**shard_map_name** |nvarchar(128)| Hedef grupta bulunan parça haritanın adı. Yalnızca target_type 'SqlShardMap' olduğunda belirtilir.|


## <a name="resources"></a>Kaynaklar

 - ![Konu bağlantısı simgesi](https://docs.microsoft.com/sql/database-engine/configure-windows/media/topic-link.gif "Konu bağlantısı simgesi") [Transact-SQL Sözdizimi Kuralları](https://docs.microsoft.com/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql)  


## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell'i kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-powershell.md)
- [Yetkilendirme ve İzinler SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server)
