---
title: Günlük yeniden yürütme hizmeti 'ni kullanarak veritabanlarını SQL yönetilen örneği 'ne geçirme
description: Günlük yeniden yürütme hizmeti kullanarak SQL Server veritabanlarını SQL yönetilen örneğine geçirmeyi öğrenin
services: sql-database
ms.service: sql-managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.author: danil
ms.reviewer: sstein
ms.date: 02/17/2021
ms.openlocfilehash: 7892b1fe0fcad77d1fde8b44f4a8745b5c7dd334
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100654594"
---
# <a name="migrate-databases-from-sql-server-to-sql-managed-instance-using-log-replay-service"></a>Günlük yeniden yürütme hizmeti 'ni kullanarak SQL Server veritabanlarını SQL yönetilen örneği 'ne geçirme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, günlük yeniden yürütme hizmeti (LRS) kullanarak SQL Server 2008-2019 ' dan SQL yönetilen örneği 'ne veritabanı geçişini el ile yapılandırma açıklanmaktadır. Bu, kurtarma modunda SQL Server günlük aktarma teknolojisine bağlı olarak yönetilen örnek için etkinleştirilen bir bulut hizmetidir. LRS 'ler, daha fazla denetim gerektiğinde veya kapalı kalma süresi için çok az tolerans varsa veri geçiş hizmeti 'nin (DMS) kullanılamaz durumda kullanılması durumunda kullanılmalıdır.

## <a name="when-to-use-log-replay-service"></a>Günlük yeniden yürütme hizmeti ne zaman kullanılır?

[Azure DMS](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance) 'in geçiş için kullanılabilmesi IÇIN, LRS bulut hizmeti doğrudan POWERSHELL, CLI cmdlet 'LERI veya API ile bırlıkte kullanılarak SQL yönetilen örneği için veritabanı geçişlerini el ile oluşturup düzenleyebilirsiniz. 

Aşağıdaki bazı durumlarda LRS bulut hizmetini kullanmayı düşünmek isteyebilirsiniz:
- Veritabanı geçiş projeniz için daha fazla denetim gerekiyor
- Geçiş tam geçişi 'teki kapalı kalma süresi için küçük bir tolerans var
- DMS yürütülebiliri ortamınıza yüklenemez
- DMS yürütülebilirinin veritabanı yedeklerine dosya erişimi yok
- Konak işletim sistemine erişim yok veya yönetici ayrıcalıklarına sahip değil

> [!NOTE]
> SQL Server veritabanlarını SQL yönetilen örneğine geçirmek için önerilen otomatik yol Azure DMS kullanmaktır. Bu hizmet, kurtarma yok modunda günlük gönderimi ile arka uçta aynı LRS bulut hizmetini kullanıyor. Azure DMS, senaryolarınızı tam olarak desteklemediğinden, geçişleri yönetmek için LRS kullanarak el ile göz önünde bulundurmanız gerekir.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Bir veritabanını buluta geçirmek için LRS kullanarak özel bir çözüm oluşturmak, diyagramda gösterilen ve aşağıdaki tabloda özetlenen birkaç düzenleme adımı gerektirir.

Geçiş, SQL Server ve yedekleme dosyalarını Azure Blob depolamaya kopyalamak için tam veritabanı yedeklemeleri yapılmasını gerektirir. LRS, yedekleme dosyalarını Azure Blob depolamadan SQL yönetilen örneğine geri yüklemek için kullanılır. Azure Blob depolama, SQL Server ile SQL yönetilen örneği arasında bir ara depolama alanı olarak kullanılır.

LRS, tüm yeni değişiklikler için Azure Blob Storage 'ı veya tam yedekleme geri yüklendikten sonra eklenen günlük yedeklemelerini izler ve eklenen yeni dosyaları otomatik olarak geri yükler. SQL yönetilen örneği üzerinde geri yüklenmekte olan yedekleme dosyalarının ilerlemesi, hizmet kullanılarak izlenebilir ve bu işlem gerekirse da iptal edilebilir. Geçiş işlemi sırasında geri yüklenen veritabanları geri yükleme modunda olur ve işlem tamamlanana kadar okumak veya yazmak için kullanılamaz.

LRS, otomatik tamamlama veya sürekli modda başlatılabilir. Otomatik tamamlama modunda başlatıldığında, belirtilen son yedekleme dosyası geri yüklendiğinde geçiş işlemi otomatik olarak tamamlanır. Sürekli modda başlatıldığında hizmet, eklenen tüm yeni yedekleme dosyalarını sürekli olarak geri yükler ve geçiş işlemi yalnızca el ile tam geçişi üzerinde tamamlanır. Son tam geçişi adımı, veritabanlarını SQL yönetilen örneği üzerinde okuma ve yazma kullanımı için kullanılabilir hale getirir. 

  ![SQL yönetilen örneği için açıklanan günlük yeniden yürütme hizmeti düzenleme adımları](./media/log-replay-service-migrate/log-replay-service-conceptual.png)

| İşlem | Ayrıntılar |
| :----------------------------- | :------------------------- |
| **1. SQL Server veritabanı yedeklerini Azure Blob depolama alanına kopyalayın**. | - [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10) veya [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)kullanarak SQL Server 'dan Azure Blob depolama 'ya tam, değişiklik ve günlük yedeklemeleri kopyalayın. <br />-Çeşitli veritabanlarını geçirirken her veritabanı için ayrı bir klasör gerekir. |
| **2. LRS hizmetini bulutta başlatın**. | -Hizmet, bir cmdlet seçimi ile başlatılabilir: <br /> PowerShell [Start-azsqlınstancedatabaselogreplay](https://docs.microsoft.com/powershell/module/az.sql/start-azsqlinstancedatabaselogreplay) <br /> CLı [az_sql_midb_log_replay_start cmdlet 'leri](https://docs.microsoft.com/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_start). <br /><br />-Başlatıldıktan sonra, hizmet Azure Blob depolama alanından yedeklemeleri alacak ve bunları SQL yönetilen örneği 'ne geri yüklemeye başlayacaktır. <br /> -Başlangıçta karşıya yüklenen tüm yedeklemeler geri yüklendikten sonra hizmet, klasöre yüklenen yeni dosyaları izleyebilir ve hizmet durduruluncaya kadar, LSN zincirine göre günlükleri sürekli olarak uygular. |
| **2,1. işlem Ilerlemesini izleyin**. | -Geri yükleme işleminin ilerlemesi, veya cmdlet 'leri arasından izlenebilir: <br /> PowerShell [Get-azsqlınstancedatabaselogreplay](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstancedatabaselogreplay) <br /> CLı [az_sql_midb_log_replay_show cmdlet 'leri](https://docs.microsoft.com/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_show). |
| **2,2. Stop\işlemi iptal** edin. | -Geçiş işleminin iptal edilmesi gerekebileceği durumlarda, işlem cmdlet 'leri seçimiyle durdurulabilir: <br /> PowerShell [stop-azsqlınstancedatabaselogreplay](https://docs.microsoft.com/powershell/module/az.sql/stop-azsqlinstancedatabaselogreplay) <br /> CLı [az_sql_midb_log_replay_stop](https://docs.microsoft.com/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_stop) cmdlet 'leri. <br /><br />-Bu, SQL yönetilen örneği üzerinde veritabanını geri yükleme işleminin silinmesine neden olur. <br />-Bir kez durdurulduktan sonra, bir veritabanı için LRS devam ettirilemez. Geçiş işleminin sıfırdan yeniden başlatılması gerekiyor. |
| **3. hazırlık sırasında buluta ver**. | -Tüm yedeklemeler SQL yönetilen örneğine geri yüklendikten sonra, bir API çağrısı veya cmdlet 'leri seçimiyle LRS tamamlanmış işlemini başlatarak tam geçişi ' i doldurun: <br />PowerShell [tam-azsqlınstancedatabaselogreplay](https://docs.microsoft.com/powershell/module/az.sql/complete-azsqlinstancedatabaselogreplay) <br /> CLı [az_sql_midb_log_replay_complete](https://docs.microsoft.com/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_complete) cmdlet 'leri. <br /><br />-Bu, LRS hizmetinin durdurulmasına ve yönetilen örnekteki veritabanının kurtarılmasını sağlar. <br />-Uygulama bağlantı dizesinin SQL Server 'den SQL yönetilen örneği 'ne yeniden işaret edin. <br />-On işlemi tamamlanma veritabanı, bulutta R/W işlemleri için kullanılabilir. |

## <a name="requirements-for-getting-started"></a>Başlangıç gereksinimleri

### <a name="sql-server-side"></a>SQL Server tarafı
- SQL Server 2008-2019
- Veritabanlarının tam yedeklemesi (bir veya daha fazla dosya)
- Değişiklik yedekleme (bir veya birden çok dosya)
- Günlük yedeklemesi (işlem günlüğü dosyası için bölünmemiş)
- **Sağlama toplamı** zorunlu olarak etkinleştirilmelidir

### <a name="azure-side"></a>Azure tarafı
-   PowerShell az. SQL Module Version 2.16.0 veya üzeri (Azure [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/)'yi[yükler](https://www.powershellgallery.com/packages/Az.Sql/)veya kullanın)
-   CLı sürüm 2.19.0 veya üstü ([Install](https://docs.microsoft.com/cli/azure/install-azure-cli))
-   Sağlanan Azure Blob depolama alanı
-   Yalnızca BLOB depolama için oluşturulan **okuma** ve **LISTELEME** izinlerine sahip SAS güvenlik belirteci

## <a name="best-practices"></a>En iyi uygulamalar

En iyi uygulamalar olarak aşağıdakiler önerilir:
- Veritabanlarınızı doğrulamak için [Data Migration Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview) ÇALıŞTıRıN, SQL yönetilen örneği 'ne geçirilmekte olan herhangi bir sorun olmayacaktır. 
- Tam ve değişiklik yedeklemelerini tek bir dosya yerine birden çok dosyaya ayırın.
- Yedekleme sıkıştırmasını etkinleştirin.
- Her zaman yayınlanan en son cmdlet 'lere güncelleştirildiği için, betikleri yürütmek üzere Cloud Shell kullanın.
- LRS hizmeti başlatıldığından beri geçişi 47 saat içinde tamamlamayı planlayın.

> [!IMPORTANT]
> - LRS kullanılarak geri yüklenen veritabanı, geçiş işlemi tamamlanana kadar kullanılamaz. Bunun nedeni, temeldeki teknolojinin kurtarma modunda günlük aktarma olmasından kaynaklanır.
> - SQL yönetilen örneği ve en son Pazar içi SQL Server sürümü arasındaki sürüm farklılıkları nedeniyle günlük aktarımı için bekleme modu LRS tarafından desteklenmez.

## <a name="steps-to-execute"></a>Yürütülecek adımlar

## <a name="copy-backups-from-sql-server-to-azure-blob-storage"></a>SQL Server yedeklemeleri Azure Blob depolamaya kopyalama

Aşağıdaki iki yaklaşım, veritabanlarını, LRS kullanarak yönetilen örneğe geçirme sırasında blob depolamaya kopyalamak için kullanılabilir:
- URL işlevine yerel [yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url) SQL Server kullanma.
- [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)veya [Azure Depolama Gezgini](https://azure.microsoft.com/en-us/features/storage-explorer)kullanarak yedeklemeleri blob kapsayıcısına kopyalama. 

## <a name="create-azure-blob-and-sas-authentication-token"></a>Azure Blob ve SAS kimlik doğrulama belirteci oluşturma

Azure Blob depolama, SQL Server ile SQL yönetilen örneği arasında yedekleme dosyaları için bir ara depolama alanı olarak kullanılır. Azure Blob depolama kapsayıcısı oluşturmak için aşağıdaki adımları izleyin:

1. [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)
2. Depolama hesabı içinde [bir blob kapsayıcısı Crete](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

Blob kapsayıcısı oluşturulduktan sonra, aşağıdaki adımları izleyerek, okuma ve listeleme izinleriyle SAS kimlik doğrulama belirteci oluşturun:

1. Azure portal kullanarak depolama hesabına erişin
2. Depolama Gezgini git
3. Blob kapsayıcılarını Genişlet
4. Blob kapsayıcısına sağ tıklayın
5. Paylaşılan erişim Imzasını Al ' ı seçin
6. Belirteç süre sonu zaman çerçevesini seçin. Belirtecin geçişinizin süresi için geçerli olduğundan emin olun.
7. Yalnızca okuma ve listeleme izinlerinin seçili olduğundan emin olun
8. Oluştur 'a tıklayın
9. Kodunuzda kullanmak için URI içinde "ZF =" ile başlayan belirteci kopyalayın

> [!IMPORTANT]
> Azure Blob depolama için SAS belirtecinin izinlerinin salt okunmalıdır ve yalnızca liste olması gerekir. SAS kimlik doğrulama belirteci için verilen diğer izinler söz konusu olduğunda, LRS hizmeti 'nin başlatılması başarısız olur. Bu güvenlik gereksinimleri tasarıma göre yapılır.

## <a name="log-in-to-azure-and-select-subscription"></a>Azure 'da oturum açın ve abonelik ' ı seçin

Azure 'da oturum açmak için aşağıdaki PowerShell cmdlet 'ini kullanın:

```powershell
Login-AzAccount
```

Aşağıdaki PowerShell cmdlet 'ini kullanarak SQL yönetilen örneğinizin bulunduğu uygun aboneliği seçin:

```powershell
Select-AzSubscription -SubscriptionId <subscription ID>
```

## <a name="start-the-migration"></a>Geçişi başlatma

Geçiş, LRS hizmeti başlatılarak başlatılır. Hizmet otomatik tamamlama veya sürekli modda başlatılabilir. Otomatik tamamlama modunda başlatıldığında, belirtilen son yedekleme dosyası geri yüklendiğinde geçiş işlemi otomatik olarak tamamlanır. Bu seçenek, son yedekleme dosyasının dosya adını belirtmek için Start komutunu gerektirir. LRS sürekli modda başlatıldığında hizmet, eklenen yeni yedekleme dosyalarını sürekli olarak geri yükler ve geçiş işlemi yalnızca el ile tam geçişi üzerinde tamamlanır. 

### <a name="start-lrs-in-autocomplete-mode"></a>LRS 'yi otomatik tamamlama modunda Başlat

LRS hizmetini otomatik tamamlama modunda başlatmak için aşağıdaki PowerShell veya CLı komutlarını kullanın. -LastBackupName parametresiyle birlikte son yedekleme dosya adını belirtin. Belirtilen son yedekleme dosyası adı geri yüklendikten sonra hizmet otomatik olarak bir cutover başlatır.

LRS 'yi otomatik tamamlama modunda Başlat-PowerShell örneği:

```powershell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" `
    -StorageContainerUri "https://test.blob.core.windows.net/testing" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D" `
    -AutoComplete -LastBackupName "last_backup.bak"
```

LRS 'yi otomatik tamamlama modunda Başlat-CLı örneği:

```cli
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb -a --last-bn "backup.bak"
    --storage-uri "https://test.blob.core.windows.net/testing"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="start-lrs-in-continuous-mode"></a>LRS 'yi sürekli modda Başlat

LRS 'yi sürekli modda başlatın-PowerShell örneği:

```powershell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" -StorageContainerUri "https://test.blob.core.windows.net/testing" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

LRS 'yi sürekli modda başlatın-CLı örneği:

```cli
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb
    --storage-uri "https://test.blob.core.windows.net/testing"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

> [!IMPORTANT]
> LRS başlatıldıktan sonra, sistem tarafından yönetilen tüm yazılım düzeltme ekleri sonraki 47 saat boyunca durdurulur. Bu pencerenin geçirilmesi sırasında, sonraki otomatikleştirilmiş yazılım düzeltme eki, devam eden LRS 'yi otomatik olarak durdurur. Böyle bir durumda, geçiş devam ettirilemez ve sıfırdan yeniden başlatılması gerekiyor. 

## <a name="monitor-the-migration-progress"></a>Geçiş ilerlemesini izleme

Geçiş işleminin ilerlemesini izlemek için aşağıdaki PowerShell komutunu kullanın:

```powershell
Get-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Geçiş işleminin ilerlemesini izlemek için aşağıdaki CLı komutunu kullanın:

```cli
az sql midb log-replay show -g mygroup --mi myinstance -n mymanageddb
```

## <a name="stop-the-migration"></a>Geçişi durdur

Geçişi durdurmanız gerekirse, aşağıdaki cmdlet 'leri kullanın. Geçişin durdurulması, geçiş işleminin sürdürülebilmesi için SQL yönetilen örneğindeki geri yükleme veritabanını siler.

Geçiş işlemini durdurmak için aşağıdaki PowerShell komutunu kullanın:

```powershell
Stop-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Geçiş işlemini durdurmak için aşağıdaki CLı komutunu kullanın:

```cli
az sql midb log-replay stop -g mygroup --mi myinstance -n mymanageddb
```

## <a name="complete-the-migration-continuous-mode"></a>Geçişi (sürekli mod) doldurun

LRS 'nin sürekli modda başlatılması durumunda, tüm yedeklemelerin geri yüklendiğinden emin olduktan sonra tam geçişi ' i başlatmak geçiş işlemi tamamlanır. Tam geçişi tamamlandığında, veritabanı geçirilir ve okuma ve yazma erişimi için hazırlanacaktır.

LRS sürekli modundaki geçiş işlemini gerçekleştirmek için aşağıdaki PowerShell komutunu kullanın:

```powershell
Complete-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
-InstanceName "ManagedInstance01" `
-Name "ManagedDatabaseName" -LastBackupName "last_backup.bak"
```

LRS sürekli modundaki geçiş işlemini gerçekleştirmek için aşağıdaki CLı komutunu kullanın:

```cli
az sql midb log-replay complete -g mygroup --mi myinstance -n mymanageddb --last-backup-name "backup.bak"
```

## <a name="next-steps"></a>Sonraki adımlar
- [SQL Server SQL yönetilen örneği 'Ne geçirme](../migration-guides/managed-instance/sql-server-to-managed-instance-guide.md)hakkında daha fazla bilgi edinin.
- [SQL Server Ile Azure SQL yönetilen örneği arasındaki farklılıklar](transact-sql-tsql-differences-sql-server.md)hakkında daha fazla bilgi edinin.
- [Azure 'a geçirilen iş yükleri Için en iyi uygulamalar](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs)hakkında daha fazla bilgi edinin.
