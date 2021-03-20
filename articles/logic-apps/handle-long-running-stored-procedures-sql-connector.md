---
title: SQL Connector 'da uzun süre çalışan saklı yordamları işleme
description: Azure Logic Apps içinde SQL Bağlayıcısı kullanılırken zaman aşımına geçen saklı yordamları işleme
services: logic-apps
ms.suite: integration
ms.reviewer: camerost, logicappspm
ms.topic: conceptual
ms.date: 10/27/2020
ms.openlocfilehash: f5b04c563dc81497f591788dc4890d379c0f898f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93103110"
---
# <a name="handle-stored-procedure-timeouts-in-the-sql-connector-for-azure-logic-apps"></a>Azure Logic Apps için SQL Connector 'da saklı yordam zaman aşımlarını işle

Mantıksal uygulamanız sonuç kümeleriyle birlikte çalışarak [SQL bağlayıcısının](../connectors/connectors-create-api-sqlazure.md) aynı anda tüm sonuçları döndürmemesi veya sonuç kümelerinizin boyut ve yapısı üzerinde daha fazla denetime sahip olmak istiyorsanız, sonuçları istediğiniz şekilde düzenleyen bir [saklı yordam](/sql/relational-databases/stored-procedures/stored-procedures-database-engine) oluşturabilirsiniz. SQL Bağlayıcısı, SQL veritabanı tablolarıyla çalışan iş görevlerini daha kolay otomatikleştirebilmeniz için [Azure Logic Apps](../logic-apps/logic-apps-overview.md) kullanarak erişebileceğiniz birçok arka uç özelliği sağlar.

Örneğin, birden çok satır alırken veya eklerken, mantıksal uygulamanız bu [sınırlar](../logic-apps/logic-apps-limits-and-config.md)dahilinde bir [ **until** döngüsü](../logic-apps/logic-apps-control-flow-loops.md#until-loop) kullanarak bu satırları yineleyebilir. Ancak, mantıksal uygulamanızın binlerce veya milyonlarca satır ile çalışması gerektiğinde, veritabanına yapılan çağrılardan kaynaklanan maliyetleri en aza indirmek istersiniz. Daha fazla bilgi için bkz. [SQL bağlayıcısını kullanarak toplu verileri işleme](../connectors/connectors-create-api-sqlazure.md#handle-bulk-data).

<a name="timeout-limit"></a>

## <a name="timeout-limit-on-stored-procedure-execution"></a>Saklı yordam yürütülürken zaman aşımı sınırı

SQL Connector 'ın [2 dakikadan kısa](/connectors/sql/#known-issues-and-limitations)bir saklı yordam zaman aşımı sınırı vardır. Bazı saklı yordamların tamamlanması bu sınırdan daha uzun sürebilir ve hataya neden olur `504 Timeout` . Bazen bu uzun süre çalışan işlemler, bu amaçla açıkça saklı yordamlar olarak kodlanır. Zaman aşımı sınırı nedeniyle Azure Logic Apps bu yordamları çağırmak sorun oluşturabilir. SQL Bağlayıcısı bir zaman uyumsuz modu yerel olarak desteklememesine karşın, bu sorunu geçici olarak bir SQL tamamlama tetikleyicisi, yerel SQL geçişli sorgu, durum tablosu ve sunucu tarafı işleri kullanarak bu modun benzetimini yapabilirsiniz. Bu görev için Azure [SQL veritabanı](../azure-sql/database/sql-database-paas-overview.md)Için [Azure elastik iş Aracısı](../azure-sql/database/elastic-jobs-overview.md) 'nı kullanabilirsiniz. [Şirket içi](/sql/sql-server/sql-server-technical-documentation) ve [Azure SQL yönetilen örneği](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)SQL Server için [SQL Server Agent](/sql/ssms/agent/sql-server-agent)kullanabilirsiniz.

Örneğin, zaman aşımı sınırının çalışmaya bitiş süresinden daha uzun süren aşağıdaki uzun süre çalışan saklı yordama sahip olduğunuzu varsayalım. Bu saklı yordamı SQL bağlayıcısını kullanarak bir mantıksal uygulamadan çalıştırırsanız, `HTTP 504 Gateway Timeout` Sonuç olarak bir hata alırsınız.

```sql
CREATE PROCEDURE [dbo].[WaitForIt]
   @delay char(8) = '00:03:00'
AS
BEGIN
   SET NOCOUNT ON;
   WAITFOR DELAY @delay
END
```

Saklı yordamı doğrudan çağırmak yerine, bir *İş Aracısı* kullanarak yordamı arka planda zaman uyumsuz olarak çalıştırabilirsiniz. Giriş ve çıkışları, mantıksal uygulamanız aracılığıyla etkileşime girebilmeniz için bir durum tablosunda saklayabilirsiniz. Girişlere ve çıkışlara ihtiyacınız yoksa veya sonuçları zaten saklı yordamdaki bir tabloya yazıyorsanız, bu yaklaşımı basitleştirebilirsiniz.

> [!IMPORTANT]
> Saklı yordamınız ve tüm işlerin *ıdempotent* olduğundan emin olun. Bu, sonuçları etkilemeden birden çok kez çalıştırabilecekleri anlamına gelir. Zaman uyumsuz işleme başarısız olursa veya zaman aşımına uğrarsa, İş Aracısı adımı ve bu nedenle Saklı yordamınızın birden çok kez yeniden denenmesiyle karşılaşabilirsiniz. Herhangi bir nesne oluşturmadan önce çıkışın yinelenmesini önlemek için, bu [en iyi yöntemleri ve yaklaşımları](../azure-sql/database/elastic-jobs-overview.md#idempotent-scripts)inceleyin.

Sonraki bölümde Azure SQL veritabanı için Azure elastik Iş aracısını nasıl kullanabileceğiniz açıklanmaktadır. SQL Server ve Azure SQL yönetilen örneği için SQL Server Agent kullanabilirsiniz. Bazı yönetim ayrıntıları farklı olur, ancak temel adımlar Azure SQL veritabanı için bir iş Aracısı ayarlama ile aynı kalır.

<a name="azure-sql-database"></a>

## <a name="job-agent-for-azure-sql-database"></a>Azure SQL veritabanı için iş Aracısı

[Azure SQL veritabanı](../azure-sql/database/sql-database-paas-overview.md)için saklı yordamı çalıştıraabilecek bir iş oluşturmak Için [Azure elastik iş aracısını](../azure-sql/database/elastic-jobs-overview.md)kullanın. Azure portal iş aracınızı oluşturun. Bu yaklaşım, aracı tarafından kullanılan ve *Aracı veritabanı* olarak da bilinen birkaç saklı yordam ekler. Ardından, hedef veritabanında saklı yordamınızın çalıştırıldığı ve tamamlandığında çıktıyı yakalayan bir iş oluşturabilirsiniz.

İşi oluşturabilmeniz için önce [Azure elastik Iş aracısının tam belgelerinde](../azure-sql/database/elastic-jobs-overview.md)açıklandığı gibi izinleri, grupları ve hedefleri ayarlamanız gerekir. Ayrıca, aşağıdaki bölümlerde açıklandığı gibi, hedef veritabanında bir destekleyici tablo oluşturmanız gerekir.

<a name="create-state-table"></a>

### <a name="create-state-table-for-registering-parameters-and-storing-inputs"></a>Parametreleri kaydetmek ve girdileri depolamak için durum tablosu oluşturma

SQL Aracısı Işleri giriş parametrelerini kabul etmez. Bunun yerine, hedef veritabanında, parametreleri kaydettiğiniz ve saklı yordamlarınızı çağırmak için kullanılacak girdileri depolayabileceğiniz bir durum tablosu oluşturun. Tüm aracı iş adımları hedef veritabanına karşı çalışır, ancak işin saklı yordamları aracı veritabanına göre çalışır. 

Durum tablosu oluşturmak için bu şemayı kullanın:

```sql
CREATE TABLE [dbo].[LongRunningState](
   [jobid] [uniqueidentifier] NOT NULL,
   [rowversion] [timestamp] NULL,
   [parameters] [nvarchar](max) NULL,
   [start] [datetimeoffset](7) NULL,
   [complete] [datetimeoffset](7) NULL,
   [code] [int] NULL,
   [result] [nvarchar](max) NULL,
   CONSTRAINT [PK_LongRunningState] PRIMARY KEY CLUSTERED
      (   [jobid] ASC
      )WITH (STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF) ON [PRIMARY]
      ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
```

Sonuçta elde edilen tablonun [SQL Server Management Studio (SMSS)](/sql/ssms/download-sql-server-management-studio-ssms)nasıl göründüğü aşağıda verilmiştir:

![Saklı yordamın girişlerini depolayan oluşturulmuş durum tablosunu gösteren ekran görüntüsü.](media/handle-long-running-stored-procedures-sql-connector/state-table-input-parameters.png)

İyi bir performans sağlamak ve aracı işinin ilişkili kaydı bulaerişebildiğinden emin olmak için tablo, iş yürütme KIMLIĞINI ( `jobid` ) birincil anahtar olarak kullanır. İsterseniz, giriş parametreleri için ayrı sütunlar da ekleyebilirsiniz. Daha önce açıklanan şema birden çok parametreyi işleyebilir, ancak tarafından hesaplanan boyutla sınırlıdır `NVARCHAR(MAX)` .

<a name="create-top-level-job"></a>

### <a name="create-a-top-level-job-to-run-the-stored-procedure"></a>Saklı yordamı çalıştırmak için üst düzey bir iş oluşturma

Uzun süre çalışan saklı yordamı yürütmek için, aracı veritabanında bu üst düzey iş aracısını oluşturun:

```sql
EXEC jobs.sp_add_job 
   @job_name='LongRunningJob',
   @description='Execute Long-Running Stored Proc',
   @enabled = 1
```

Şimdi, saklı yordamı parametreli, çalıştıran ve tamamladığınız işe adımlar ekleyin. Varsayılan olarak, 12 saat sonra bir iş adımı zaman aşımına uğrar. Saklı yordamınız daha fazla zaman alıyorsa veya yordamın daha önce zaman aşımına uğrar olmasını istiyorsanız `step_timeout_seconds` parametresini Saniyeler içinde belirtilen başka bir değer olarak değiştirebilirsiniz. Varsayılan olarak, bir adım, avantajınıza kullanabileceğiniz her yeniden deneme arasında geri dönüş zaman aşımı ile 10 yerleşik yeniden deneme içerir.

Ekleme adımları şunlardır:

1. Parametrelerin tabloda görünmesini bekleyin `LongRunningState` .

   Bu ilk adım, parametrelerin tabloya eklenmesini bekler ve bu, `LongRunningState` iş başladıktan hemen sonra meydana gelir. İş yürütme KIMLIĞI ( `jobid` ) tabloya eklenmezse `LongRunningState` , adım yalnızca başarısız olur ve varsayılan yeniden deneme veya geri alma zaman aşımı süresi bekliyor:

   ```sql
   EXEC jobs.sp_add_jobstep
      @job_name='LongRunningJob',
      @step_name= 'Parameterize WaitForIt',
      @step_timeout_seconds = 30,
      @command= N'
         IF NOT EXISTS(SELECT [jobid] FROM [dbo].[LongRunningState]
            WHERE jobid = $(job_execution_id))
            THROW 50400, ''Failed to locate call parameters (Step1)'', 1',
      @credential_name='JobRun',
      @target_group_name='DatabaseGroupLongRunning'
   ```

1. Durum tablosundan parametreleri sorgulayın ve bunları saklı yordama geçirin. Bu adım Ayrıca, yordamı arka planda çalıştırır. 

   Saklı yordamınız parametrelere ihtiyaç duymazsa, saklı yordamı doğrudan çağırmanız yeterlidir. Aksi takdirde, parametresini geçirmek için, `@timespan` `@callparams` ek parametreleri geçirmek için de genişletebilirsiniz öğesini kullanın.

   ```sql
   EXEC jobs.sp_add_jobstep
      @job_name='LongRunningJob',
      @step_name='Execute WaitForIt',
      @command=N'
         DECLARE @timespan char(8)
         DECLARE @callparams NVARCHAR(MAX)
         SELECT @callparams = [parameters] FROM [dbo].[LongRunningState]
            WHERE jobid = $(job_execution_id))
         SET @timespan = @callparams
         EXECUTE [dbo].[WaitForIt] @delay = @timespan', 
      @credential_name='JobRun',
      @target_group_name='DatabaseGroupLongRunning'
   ```

1. İşi tamamlayıp sonuçları kaydedin.

   ```sql
   EXEC jobs.sp_add_jobstep
      @job_name='LongRunningJob',
      @step_name='Complete WaitForIt',
      @command=N'
         UPDATE [dbo].[LongRunningState]
            SET [complete] = GETUTCDATE(),
               [code] = 200,
               [result] = ''Success''
            WHERE jobid = $(job_execution_id)',
      @credential_name='JobRun',
      @target_group_name='DatabaseGroupLongRunning'
   ```

<a name="start-job-pass-parameters"></a>

### <a name="start-the-job-and-pass-the-parameters"></a>İşi başlatın ve parametreleri geçirin

İşi başlatmak için, [ **bir SQL sorgusu Yürüt** eylemi](/connectors/sql/#execute-a-sql-query-(v2)) ile bir geçiş yerel sorgusu kullanın ve işin parametrelerini hemen durum tablosuna gönderin. `jobid`Hedef tablodaki özniteliğe giriş sağlamak için Logic Apps, önceki eylemden alınan tablo çıktısı boyunca yinelenen **her döngü için** bir ekler. Her iş yürütme KIMLIĞI için, dinamik veri çıkışını kullanan bir **satır ekle** eylemi çalıştırın, bu, `ResultSets JobExecutionId` iş için parametreleri paketten çıkarmak ve hedef saklı yordama geçirmek için ekleyin.

![İşi başlatmak ve parametreleri saklı yordama geçirmek için kullanılacak eylemleri gösteren ekran görüntüsü.](media/handle-long-running-stored-procedures-sql-connector/start-job-actions.png)

İş tamamlandığında, iş, `LongRunningState` [ **bir öğe değiştirildiğinde** tetikleyicisi](/connectors/sql/#when-an-item-is-modified-(v2))kullanarak sonuca kolayca tetikleyebilmeniz için tabloyu günceller. Çıktıya gerek duymuyorsanız veya bir çıkış tablosunu izleyen bir Tetikleyiciniz varsa, bu bölümü atlayabilirsiniz.

![Bir öğenin değiştirildiği zaman için SQL tetikleyicisini gösteren ekran görüntüsü.](media/handle-long-running-stored-procedures-sql-connector/trigger-on-results-after-job-completes.png)

<a name="sql-on-premises-or-managed-instance"></a>

## <a name="job-agent-for-sql-server-or-azure-sql-managed-instance"></a>SQL Server veya Azure SQL yönetilen örneği için iş Aracısı

Aynı senaryo için, [Şirket içi](/sql/sql-server/sql-server-technical-documentation) ve [Azure SQL yönetilen örneği](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md)SQL Server [SQL Server Agent](/sql/ssms/agent/sql-server-agent) kullanabilirsiniz. Bazı yönetim ayrıntıları farklı olsa da, temel adımlar Azure SQL veritabanı için bir iş aracısının ayarıyla aynı kalır.

## <a name="next-steps"></a>Sonraki adımlar

[SQL Server, Azure SQL veritabanı veya Azure SQL yönetilen örneği 'ne bağlanma](../connectors/connectors-create-api-sqlazure.md)

