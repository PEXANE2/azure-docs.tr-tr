---
title: Elastik Veritabanı İşleri (önizleme)
description: Bir veya daha fazla Azure SQL veritabanı kümesinde Transact-SQL (T-SQL) komut dosyalarını çalıştırmak için Elastik Veritabanı İşlerini (önizleme) yapılandırın
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: srinia
ms.author: srinia
ms.reviewer: sstein
ms.date: 12/18/2018
ms.openlocfilehash: e5b07ac0e9421cbca034b17c573cab16641f49f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79214486"
---
# <a name="create-configure-and-manage-elastic-jobs"></a>Esnek işler oluşturma, yapılandırma ve yönetme

Bu makalede, elastik işleri nasıl oluşturacağınızı, yapılandıracağınızı ve yöneteceğinizi öğreneceksiniz.

Elastik işleri kullanmadıysanız, [Azure SQL Veritabanı'ndaki iş otomasyonu kavramları hakkında daha fazla bilgi edinin.](sql-database-job-automation-overview.md)

## <a name="create-and-configure-the-agent"></a>Aracıyı oluşturma ve yapılandırma

1. Boş bir S0 veya üzeri SQL veritabanı oluşturun ya da tanımlayın. Bu veritabanı, Elastik İş aracısı oluşturma sırasında *İş veritabanı* olarak kullanılacaktır.
2. [Portalda](https://portal.azure.com/#create/Microsoft.SQLElasticJobAgent) veya [PowerShell](elastic-jobs-powershell.md#create-the-elastic-job-agent)ile bir Elastik İş aracısı oluşturun.

   ![Elastik İş aracısı oluşturma](media/elastic-jobs-overview/create-elastic-job-agent.png)

## <a name="create-run-and-manage-jobs"></a>İşleri oluşturma, çalıştırma ve yönetme

1. [PowerShell](elastic-jobs-powershell.md) veya [T-SQL](elastic-jobs-tsql.md#create-a-credential-for-job-execution)kullanarak *İş veritabanında* iş yürütme için bir kimlik bilgisi oluşturun.
2. [PowerShell](elastic-jobs-powershell.md) veya [T-SQL](elastic-jobs-tsql.md#create-a-target-group-servers)kullanarak hedef grubu (işi çalıştırmak istediğiniz veritabanları) tanımlayın.
3. İşin çalışacağı her veritabanında bir iş aracısı kimlik bilgisi oluşturun [(kullanıcıyı (veya rolü) gruptaki her bir veritabanına ekleyin)](sql-database-manage-logins.md). Örnek için bkz. [PowerShell öğreticisi](elastic-jobs-powershell.md).
4. [PowerShell](elastic-jobs-powershell.md) veya [T-SQL](elastic-jobs-tsql.md#deploy-new-schema-to-many-databases)kullanarak bir iş oluşturun.
5. [PowerShell](elastic-jobs-powershell.md) veya [T-SQL](elastic-jobs-tsql.md#deploy-new-schema-to-many-databases) kullanarak iş adımlarını ekleyin.
6. [PowerShell](elastic-jobs-powershell.md#run-the-job) veya [T-SQL](elastic-jobs-tsql.md#begin-ad-hoc-execution-of-a-job)kullanarak bir iş çalıştırın.
7. [Portal, PowerShell](elastic-jobs-powershell.md#monitor-status-of-job-executions) veya [T-SQL'i](elastic-jobs-tsql.md#monitor-job-execution-status)kullanarak iş yürütme durumunu izleyin.

   ![Portal](media/elastic-jobs-overview/elastic-job-executions-overview.png)

## <a name="credentials-for-running-jobs"></a>İşleri çalıştırmak için kullanılan kimlik bilgileri

İşler, yürütme sırasında hedef grup tarafından belirtilen veritabanlarına bağlanmak için [veritabanı kapsamlı kimlik bilgilerini](/sql/t-sql/statements/create-database-scoped-credential-transact-sql) kullanır. Hedef grupta sunucular veya havuzlar varsa bu veritabanı kapsamlı kimlik bilgileri, kullanılabilir durumdaki veritabanlarını numaralandırmak amacıyla asıl veritabanına bağlanmak için kullanılır.

Bir işi çalıştırmak için uygun kimlik bilgilerinin ayarlanması kafa karışıklığına neden olabileceğinden aşağıdaki noktaları göz önünde bulundurun:

- Veritabanı kapsamı kimlik bilgileri İş *veritabanında*oluşturulmalıdır.
- **Tüm hedef veritabanları, işin başarıyla tamamlanması için [yeterli izinlere](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) sahip bir oturum** açmanız gerekir (aşağıdaki`jobuser` diyagramda).
- Kimlik bilgileri işler arasında yeniden kullanılabilir ve kimlik bilgileri parolaları şifrelenir ve yalnızca iş nesnelerine salt okunur erişimi olan kullanıcılardan güvenli hale alınır.

Aşağıdaki resim, uygun iş kimlik bilgilerinin anlaşılması ve ayarlanması konusunda yardımcı olmak üzere tasarlanmıştır. **Kullanıcının, işin çalıştırılacağı her veritabanında (tüm *hedef kullanıcı veritabanlarında*) oluşturulması gerektiğini unutmayın**.

![Elastik İşler kimlik bilgileri](media/elastic-jobs-overview/job-credentials.png)

## <a name="security-best-practices"></a>En iyi güvenlik uygulamaları

Elastik İşlerle çalışırken dikkat etmeniz gereken en iyi deneyimlerin bazıları:

- API’lerin kullanımını güvenilir kişilerle sınırlayın.
- Kimlik bilgileri iş adımını gerçekleştirmek için gerekli olan en düşük ayrıcalıklara sahip olmalıdır. Daha fazla bilgi için Yetkilendirme [ve İzinler SQL Server'a](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server)bakın.
- Bir sunucu ve/veya havuz hedef grubu üyesi kullanırken, iş yürütmeden önce sunucu(lar) ve/veya havuz(lar) veritabanı listelerini genişletmek için kullanılan veritabanlarını görüntülemek/listelemek için ana veritabanındaki haklarla ayrı bir kimlik bilgisi oluşturulması önerilir.

## <a name="agent-performance-capacity-and-limitations"></a>Aracı performansı, kapasitesi ve sınırlamaları

Elastik İşler, uzun süren işlerin tamamlanması sırasında en az düzeyde işlem kaynağı kullanır.

Hedef veritabanı grubunun boyutuna ve bir işin istenen yürütme süresine (eşzamanlı çalışan sayısı) bağlı olarak aracı için gerekli olan işlem süresi ve *İş veritabanı* performansı değişiklik gösterir (hedef ve iş sayısı ne kadar yüksek olursa gereken işlem zamanı o kadar fazla olur).

Önizleme şu an için 100 eşzamanlı işle sınırlıdır.

### <a name="prevent-jobs-from-reducing-target-database-performance"></a>İşlerin hedef veritabanının performansını düşürmesini engelleme

Bir SQL elastik havuzundaki veritabanları üzerinde iş çalıştırılması sırasında kaynakların aşırı yüklenmesini önlemek için işler aynı anda üzerinde çalışılabilecek veritabanı sayısını sınırlayacak şekilde yapılandırılabilir.

Depolanan yordamın `sp_add_jobstep` `@max_parallelism` parametresini T-SQL'de veya `Add-AzSqlElasticJobStep -MaxParallelism` PowerShell'de ayarlayarak bir işin çalıştığı eşzamanlı veritabanlarının sayısını ayarlayın.

## <a name="best-practices-for-creating-jobs"></a>İş oluşturmak için en iyi deneyimler

### <a name="idempotent-scripts"></a>Bir kez etkili betikler
Bir işin T-SQL betiklerinin [bir kez etkili](https://en.wikipedia.org/wiki/Idempotence) olması gerekir. **Bir kez etkili**, betiğin başarılı olması ve tekrar çalıştırılması durumunda aynı sonucun ortaya çıkması anlamına gelir. Bir betik, geçici ağ sorunları nedeniyle başarısız olabilir. Bu durumda iş, betiği atlamadan önce otomatik olarak önceden belirtilen sayıda yeniden deneme gerçekleştirir. Bir kez etkili betik, iki kez (veya daha fazla) çalıştırılsa dahi aynı sonucu verir.

Basit bir yöntem, bir nesneyi oluşturmadan önce mevcut olup olmadığını test etmektir.


```sql
IF NOT EXISTS (some_object)
    -- Create the object
    -- If it exists, drop the object before recreating it.
```

Benzer şekilde bir betiğin mantıksal olarak test ederek ve bulduğu sonuçlara göre kendini ayarlayarak başarılı şekilde yürütülebilmesi gerekir.



## <a name="next-steps"></a>Sonraki adımlar

- [PowerShell'i kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-powershell.md)
- [Transact-SQL (T-SQL) kullanarak Elastik İşler oluşturma ve yönetme](elastic-jobs-tsql.md)
