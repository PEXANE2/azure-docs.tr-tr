---
title: Günlük yeniden yürütme hizmeti 'ni kullanarak veritabanlarını SQL yönetilen örneği 'ne geçirme
description: Günlük yeniden yürütme hizmeti kullanarak SQL Server veritabanlarını SQL yönetilen örneğine geçirmeyi öğrenin
services: sql-database
ms.service: sql-managed-instance
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: danimir
ms.reviewer: sstein
ms.date: 03/01/2021
ms.openlocfilehash: bc0dc72c7547c8f74aec53b7153fc5384c6b634b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101690796"
---
# <a name="migrate-databases-from-sql-server-to-sql-managed-instance-using-log-replay-service-preview"></a>Günlük yeniden yürütme hizmeti 'ni (Önizleme) kullanarak SQL Server veritabanlarını SQL yönetilen örneğine geçirme
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Bu makalede, şu anda genel önizlemede olan günlük yeniden yürütme hizmeti 'ni (LRS) kullanarak SQL Server 2008-2019 ' dan SQL yönetilen örneği 'ne veritabanı geçişini el ile yapılandırma açıklanmaktadır. Bu, SQL Server günlük aktarma teknolojisine bağlı olarak yönetilen örnek için etkinleştirilen bir bulut hizmetidir. LRS, karmaşık özel geçişler ve karma mimariler varsa, daha fazla denetim gerektiğinde, kapalı kalma süresi için çok az tolerans varsa veya Azure veri geçiş hizmeti (DMS) kullanılamadığında kullanılmalıdır.

Hem DMS hem de LRS aynı temel geçiş teknolojisini ve aynı API 'Leri kullanır. LRS 'yi serbest bırakarak, şirket içi olarak karmaşık özel geçişler ve karma mimari de etkinleştiriliyor. SQL Server ve SQL yönetilen örnekleri.

## <a name="when-to-use-log-replay-service"></a>Günlük yeniden yürütme hizmeti ne zaman kullanılır?

[Azure DMS](/azure/dms/tutorial-sql-server-to-managed-instance.md) 'in geçiş için kullanılabilmesi IÇIN, LRS bulut hizmeti doğrudan POWERSHELL, CLI cmdlet 'LERI veya API ile bırlıkte kullanılarak SQL yönetilen örneği için veritabanı geçişlerini el ile oluşturup düzenleyebilirsiniz. 

Aşağıdaki bazı durumlarda LRS bulut hizmetini kullanmayı düşünmek isteyebilirsiniz:
- Veritabanı geçiş projeniz için daha fazla denetim gerekiyor
- Geçiş tam geçişi 'teki kapalı kalma süresi için küçük bir tolerans var
- DMS yürütülebiliri ortamınıza yüklenemez
- DMS yürütülebilirinin veritabanı yedeklerine dosya erişimi yok
- Konak işletim sistemine erişim yok veya yönetici ayrıcalıklarına sahip değil
- Ortamınızdan Azure 'a ağ bağlantı noktaları açılamıyor
- Yedeklemeler, URL 'yi kullanarak doğrudan Azure Blob depolama alanına depolanır
- Değişiklik yedeklemelerini kullanma gereksinimi var

> [!NOTE]
> SQL Server veritabanlarını SQL yönetilen örneğine geçirmek için önerilen otomatik yol Azure DMS kullanmaktır. Bu hizmet, günlük aktarımına NORECOVERY modunda, arka uçta aynı LRS bulut hizmetini kullanıyor. Azure DMS, senaryolarınızı tam olarak desteklemediğinden, geçişleri yönetmek için LRS kullanarak el ile göz önünde bulundurmanız gerekir.

## <a name="how-does-it-work"></a>Nasıl çalışır?

Veritabanlarını buluta geçirmek için LRS kullanarak özel bir çözüm oluşturmak, diyagramda gösterilen ve aşağıdaki tabloda özetlenen birkaç düzenleme adımı gerektirir.

Geçiş, sağlama TOPLAMı etkinken SQL Server tam veritabanı yedeklemeleri yapmaktan ve yedekleme dosyalarını Azure Blob depolamaya kopyalamaktan oluşur. LRS, yedekleme dosyalarını Azure Blob depolamadan SQL yönetilen örneğine geri yüklemek için kullanılır. Azure Blob depolama, SQL Server ile SQL yönetilen örneği arasında bir ara depolama alanı olarak kullanılır.

LRS, tüm yeni değişiklikler için Azure Blob Storage 'ı veya tam yedekleme geri yüklendikten sonra eklenen günlük yedeklemelerini izler ve eklenen yeni dosyaları otomatik olarak geri yükler. SQL yönetilen örneği üzerinde geri yüklenmekte olan yedekleme dosyalarının ilerlemesi, hizmet kullanılarak izlenebilir ve bu işlem gerekirse da iptal edilebilir.

LRS, Azure Blob depolama alanına yerleştirilmiş tüm dosyaları taradığından ve yedekleme zincirinin yalnızca dosya üstbilgilerini okumasını oluşturduğu belirli bir yedekleme dosya adlandırma kuralına gerek yoktur. Veritabanları, geçiş işlemi sırasında, [norecovery](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql#comparison-of-recovery-and-norecovery) modunda geri yüklendiği için "geri yükleme" durumundadır ve geçiş işlemi tam olarak tamamlanana kadar okuma veya yazma için kullanılamaz. 

Çeşitli veritabanlarını geçirirken, her bir veritabanı için yedeklemelerin Azure Blob depolamada ayrı bir klasöre yerleştirilmesi gerekir. LRS 'lerin her veritabanı için ayrı olarak başlatılması gerekir ve ayrı Azure Blob depolama klasörlerinin farklı yolları belirtilmelidir. 

LRS, otomatik tamamlama veya sürekli modda başlatılabilir. Otomatik tamamlama modunda başlatıldığında, belirtilen son yedekleme dosya adı geri yüklendiğinde geçiş işlemi otomatik olarak tamamlanır. Sürekli modda başlatıldığında hizmet, eklenen tüm yeni yedekleme dosyalarını sürekli olarak geri yükler ve geçiş işlemi yalnızca el ile tam geçişi üzerinde tamamlanır. El ile tam geçişi, yalnızca son günlük kuyruğu yedeklemesi alındıktan ve SQL yönetilen örneği 'nde geri yüklenmiş olarak gösterildiğinde yürütülür. Son tam geçişi adımı, veritabanının çevrimiçi hale gelmesini ve SQL yönetilen örneği üzerinde okuma ve yazma kullanımı için kullanılabilir hale getirir.

LRS durdurulduktan sonra otomatik olarak otomatik tamamlama veya cutover üzerinde el ile, SQL yönetilen örneği üzerinde çevrimiçi hale getirilen bir veritabanı için geri yükleme işlemi devam ettirilemez. Geçiş otomatik tamamlama veya cutover üzerinde el ile tamamlandıktan sonra ek yedekleme dosyalarını geri yüklemek için, veritabanının silinmesi ve tüm yedekleme zincirinin, LRS yeniden başlatılarak sıfırdan geri yüklenmesi gerekir.

![SQL yönetilen örneği için açıklanan günlük yeniden yürütme hizmeti düzenleme adımları](./media/log-replay-service-migrate/log-replay-service-conceptual.png)
    
| İşlem | Ayrıntılar |
| :----------------------------- | :------------------------- |
| **1. SQL Server veritabanı yedeklerini Azure Blob depolama alanına kopyalayın**. | - [AzCopy](/azure/storage/common/storage-use-azcopy-v10)veya [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)kullanarak SQL Server Azure Blob depolama kapsayıcısına tam, değişiklik ve günlük yedeklemeleri kopyalayın. <br />-LRS 'nin belirli bir dosya adlandırma kuralı gerektirmediğinden, herhangi bir dosya adı kullanın.<br />-Çeşitli veritabanlarını geçirirken her veritabanı için ayrı bir klasör gerekir. |
| **2. LRS hizmetini bulutta başlatın**. | -Hizmet, bir cmdlet seçimi ile başlatılabilir: <br /> PowerShell [Start-azsqlınstancedatabaselogreplay](/powershell/module/az.sql/start-azsqlinstancedatabaselogreplay) <br /> CLı [az_sql_midb_log_replay_start cmdlet 'leri](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_start). <br /> -Azure Blob depolamada farklı bir yedekleme klasörüne işaret eden her farklı veritabanı için LRS 'yi ayrı olarak başlatın. <br />-Başlatıldıktan sonra, hizmet Azure Blob depolama kapsayıcısından yedeklemeleri alacak ve bunları SQL yönetilen örneği 'ne geri yüklemeye başlayacaktır.<br /> -LRS 'nin sürekli modda başlatılmış olması durumunda, başlangıçta karşıya yüklenen tüm yedeklemeler geri yüklendikten sonra hizmet, klasöre yüklenen yeni dosyaları izleyebilir ve hizmet durduruluncaya kadar, LSN zincirine göre günlükleri sürekli olarak uygular. |
| **2,1. işlem Ilerlemesini izleyin**. | -Geri yükleme işleminin ilerlemesi, veya cmdlet 'leri arasından izlenebilir: <br /> PowerShell [Get-azsqlınstancedatabaselogreplay](/powershell/module/az.sql/get-azsqlinstancedatabaselogreplay) <br /> CLı [az_sql_midb_log_replay_show cmdlet 'leri](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_show). |
| **2,2. Stop\işlemi iptal** edin. | -Geçiş işleminin iptal edilmesi gerekebileceği durumlarda, işlem cmdlet 'leri seçimiyle durdurulabilir: <br /> PowerShell [stop-azsqlınstancedatabaselogreplay]/PowerShell/Module/az.exe SQL/stop-azsqlınstancedatabaselogreplay) <br /> CLı [az_sql_midb_log_replay_stop](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_stop) cmdlet 'leri. <br /><br />-Bu, SQL yönetilen örneği üzerinde geri yüklenmekte olan veritabanının silinmesine neden olur. <br />-Bir kez durdurulduktan sonra, bir veritabanı için LRS devam ettirilemez. Geçiş işleminin sıfırdan yeniden başlatılması gerekiyor. |
| **3. hazırlık sırasında buluta ver**. | -Uygulamayı ve iş yükünü durdurun. Son günlük kuyruğu yedeklemesini gerçekleştirin ve Azure Blob depolama 'ya yükleyin.<br /> -LRS tam işlemini bir cmdlet seçimi ile başlatarak tam geçişi ' i doldurun: <br />PowerShell [tam-azsqlınstancedatabaselogreplay](/powershell/module/az.sql/complete-azsqlinstancedatabaselogreplay) <br /> CLı [az_sql_midb_log_replay_complete](/cli/azure/sql/midb/log-replay#az_sql_midb_log_replay_complete) cmdlet 'leri. <br /><br />-Bu, LRS hizmetinin durdurulmasına ve veritabanının SQL yönetilen örneği üzerinde okuma ve yazma kullanımı için çevrimiçi duruma gelmesine neden olur.<br /> -Uygulama bağlantı dizesinin SQL Server 'den SQL yönetilen örneği 'ne yeniden işaret edin. |

## <a name="requirements-for-getting-started"></a>Başlangıç gereksinimleri

### <a name="sql-server-side"></a>SQL Server tarafı
- SQL Server 2008-2019
- Veritabanlarının tam yedeklemesi (bir veya daha fazla dosya)
- Değişiklik yedekleme (bir veya birden çok dosya)
- Günlük yedeklemesi (işlem günlüğü dosyası için bölünmemiş)
- Yedekler için **sağlama toplamı etkinleştirilmelidir** (zorunlu)

### <a name="azure-side"></a>Azure tarafı
- PowerShell az. SQL Module Version 2.16.0 veya üzeri (Azure [Cloud Shell](/azure/cloud-shell/)'yi[yükler](https://www.powershellgallery.com/packages/Az.Sql/)veya kullanın)
- CLı sürüm 2.19.0 veya üstü ([Install](/cli/azure/install-azure-cli))
- Sağlanan Azure Blob depolama kapsayıcısı
- Yalnızca BLOB depolama kapsayıcısı için oluşturulan **okuma** ve **LISTELEME** izinlerine sahip SAS güvenlik belirteci

### <a name="migrating-multiple-databases"></a>Birden çok veritabanını geçirme
- Farklı veritabanlarına yönelik yedekleme dosyaları, Azure Blob depolamada ayrı klasörlere yerleştirilmelidir.
- LRS 'ler, Azure Blob depolamada uygun bir klasöre işaret eden her bir veritabanı için ayrı olarak başlatılmalıdır.
- LRS, tek bir SQL yönetilen örneği başına en fazla 100 eşzamanlı geri yükleme işlemini destekleyebilir.

### <a name="azure-rbac-permissions-required"></a>Azure RBAC izinleri gerekli
Belirtilen istemcilerle LRS 'nin yürütülmesi için aşağıdaki Azure rollerinden biri gerekir:
- Abonelik sahibi rolü veya
- [Yönetilen örnek katılımcısı](../../role-based-access-control/built-in-roles.md#sql-managed-instance-contributor) rolü veya
- Aşağıdaki izne sahip özel rol:
  - `Microsoft.Sql/managedInstances/databases/*`

## <a name="best-practices"></a>En iyi uygulamalar

En iyi uygulamalar olarak aşağıdakiler önerilir:
- Veritabanlarınızı SQL yönetilen örneği 'ne geçirilmeye hazırlandığını doğrulamak için [Data Migration Yardımcısı](/sql/dma/dma-overview) çalıştırın. 
- Tam ve değişiklik yedeklemelerini tek bir dosya yerine birden çok dosyaya ayırın.
- Yedekleme sıkıştırmasını etkinleştirin.
- Her zaman yayınlanan en son cmdlet 'lere güncelleştirildiği için, betikleri yürütmek üzere Cloud Shell kullanın.
- LRS hizmeti başlatıldığından beri geçişi 47 saat içinde tamamlamayı planlayın. Bu bir yetkisiz kullanım süresi, LRS başlatıldıktan sonra sistem tarafından yönetilen yazılım düzeltme eklerini önler.

> [!IMPORTANT]
> - LRS kullanılarak geri yüklenen veritabanı, geçiş işlemi tamamlanana kadar kullanılamaz.
> - Geçiş sırasında veritabanlarına salt okuma erişimi LRS tarafından desteklenmez.
> - Geçiş tamamlandıktan sonra, LRS geri yükleme sürdürmeyi desteklemediğinden geçiş işlemi sonlandırılır.

## <a name="steps-to-execute"></a>Yürütülecek adımlar

### <a name="make-backups-on-the-sql-server"></a>SQL Server yedeklemeleri yapın

SQL Server yedeklemeler aşağıdaki iki seçenekten biri ile yapılabilir:

- Yerel disk depolama alanına yedekleme yapın ve ardından, ortamınız Azure Blob depolama alanına doğrudan yedeklemenin kısıtlayıcıysa Azure Blob depolama alanına dosya yükleyin.
- Ortamınız ve güvenlik yordamlarınızın bunu yapmasına izin vermek için T-SQL içindeki "URL 'ye" seçeneğiyle doğrudan Azure Blob depolama alanına yedekleme yapın. 

Günlük yedeklemelerine izin vermek için tam kurtarma moduna geçirmek istediğiniz veritabanlarını ayarlayın.

```SQL
-- To permit log backups, before the full database backup, modify the database to use the full recovery
USE master
ALTER DATABASE SampleDB
SET RECOVERY FULL
GO
```

Yerel depolama alanında veritabanınızın tam, fark ve günlük yedeklemesini el ile yapmak için aşağıdaki örnek T-SQL komut dosyalarını kullanın. Bu, LRS için zorunlu bir gereksinim olduğundan, sağlama TOPLAMı seçeneğinin etkinleştirildiğinden emin olun.

```SQL
-- Example on how to make full database backup to the local disk
BACKUP DATABASE [SampleDB]
TO DISK='C:\BACKUP\SampleDB_full.bak'
WITH INIT, COMPRESSION, CHECKSUM
GO

-- Example on how to make differential database backup to the locak disk
BACKUP DATABASE [SampleDB]
TO DISK='C:\BACKUP\SampleDB_diff.bak'
WITH DIFFERENTIAL, COMPRESSION, CHECKSUM
GO

-- Example on how to make the transactional log backup to the local disk
BACKUP LOG [SampleDB]
TO DISK='C:\BACKUP\SampleDB_log.trn'
WITH COMPRESSION, CHECKSUM
GO
```

### <a name="create-azure-blob-storage"></a>Azure Blob depolama oluşturma

Azure Blob depolama, SQL Server ile SQL yönetilen örneği arasında yedekleme dosyaları için bir ara depolama alanı olarak kullanılır. Depolama hesabı içinde yeni bir depolama hesabı ve bir blob kapsayıcısı oluşturmak için aşağıdaki adımları izleyin:

1. [Depolama hesabı oluşturma](../../storage/common/storage-account-create.md?tabs=azure-portal)
2. Depolama hesabı içinde [bir blob kapsayıcısı Crete](../../storage/blobs/storage-quickstart-blobs-portal.md)

### <a name="copy-backups-from-sql-server-to-azure-blob-storage"></a>SQL Server yedeklemeleri Azure Blob depolamaya kopyalama

Aşağıdaki yaklaşımlardan bazıları, LRS kullanarak veritabanlarını yönetilen örneğe geçirme sırasında blob depolamaya yedeklemeleri yüklemek için kullanılabilir:
- Yedeklemeleri bir blob kapsayıcısına yüklemek için [AzCopy](/azure/storage/common/storage-use-azcopy-v10)veya [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer) kullanma.
- Azure portal Depolama Gezgini kullanma.

### <a name="make-backups-from-sql-server-directly-to-azure-blob-storage"></a>SQL Server yedeklemeleri doğrudan Azure Blob depolama 'ya yapın

Şirket ve ağ ilkenizin izin veriyorsa, alternatif olarak, Azure Blob depolama 'ya SQL Server yedeklemeleri doğrudan [URL 'ye](/sql/relational-databases/backup-restore/sql-server-backup-to-url) SQL Server. Bu seçeneği, yerel depolama üzerinde yedeklemeler yapmak ve bunları Azure Blob depolama alanına yüklemek için gerekli değildir.

İlk adım olarak, bu işlem Azure Blob depolama alanı için SAS kimlik doğrulama belirtecinin oluşturulmasını gerektirir ve belirtecin SQL Server içe aktarılması gerekir. İkinci adım T-SQL ' i "URL ile" seçeneğiyle yedeklemeleri yapmak. CHEKSUM seçeneğinin etkin olduğu tüm yedeklemelerin yapıldığından emin olun.

Başvuru için, Azure Blob Storage 'a yedeklemeler oluşturmak için örnek kod aşağıda verilmiştir. Bu örnek SAS belirtecinin nasıl içeri aktarılacağı hakkında yönergeler içermez. SQL Server için SAS belirtecini oluşturma ve içeri aktarma dahil olmak üzere ayrıntılı yönergeler aşağıdaki öğreticide verilmiştir: [Azure Blob depolama hizmetini SQL Server ile birlikte kullanın](/sql/relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016#1---create-stored-access-policy-and-shared-access-storage). 

```SQL
-- Example on how to make full database backup to URL
BACKUP DATABASE [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_full.bak'
WITH INIT, COMPRESSION, CHECKSUM
GO

-- Example on how to make differential database backup to URL
BACKUP DATABASE [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_diff.bak'  
WITH DIFFERENTIAL, COMPRESSION, CHECKSUM
GO

-- Example on how to make the transactional log backup to URL
BACKUP LOG [SampleDB]
TO URL = 'https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>/SampleDB_log.trn'  
WITH COMPRESSION, CHECKSUM
```

### <a name="generate-azure-blob-storage-sas-authentication-for-lrs"></a>LRS için Azure Blob Storage SAS kimlik doğrulaması oluşturma

Azure Blob depolama, SQL Server ile SQL yönetilen örneği arasında yedekleme dosyaları için bir ara depolama alanı olarak kullanılır. LRS hizmeti tarafından kullanılmak üzere liste ve salt okuma izinlerinin bulunduğu SAS kimlik doğrulama belirtecinin oluşturulması gerekir. Bu, LRS hizmetinin Azure Blob depolama alanına erişmesini ve yedekleme dosyalarını kullanarak bunları SQL yönetilen örneği 'ne geri yüklemesini sağlayacaktır. LRS kullanımı için SAS kimlik doğrulaması oluşturmak için aşağıdaki adımları izleyin:

1. Azure portal Depolama Gezgini erişin
2. Blob kapsayıcılarını Genişlet
3. Blob kapsayıcısına sağ tıklayın ve paylaşılan erişim Imzası  ![ günlüğü yeniden yürütme hizmeti al SAS kimlik doğrulama belirteci oluştur ' u seçin.](./media/log-replay-service-migrate/lrs-sas-token-01.png)
4. Belirteç süre sonu zaman çerçevesini seçin. Belirtecin geçişinizin süresi için geçerli olduğundan emin olun.
5. Belirtecin UTC için saat dilimini veya yerel saati seçin
    - Belirtecin saat dilimi ve SQL yönetilen örneğiniz uyuşmayabilir. SAS belirtecinin, saat dilimlerinin dikkate alınması için uygun zaman geçerliliği olduğundan emin olun. Mümkünse, saat dilimini planlı geçiş pencerenizin daha önceki ve sonraki bir zamanına ayarlayın.
6. Yalnızca oku ve yalnızca izinleri Listele ' ı seçin
    - Başka hiçbir izin seçilmelidir, aksi takdirde LRS başlatılamaz. Bu güvenlik gereksinimi tasarıma göre yapılır.
7. Oluştur düğme günlüğü yeniden  ![ yürütme HIZMETI SAS kimlik doğrulama belirteci oluştur ' a tıklayın](./media/log-replay-service-migrate/lrs-sas-token-02.png)

SAS kimlik doğrulaması, daha önce belirttiğiniz zaman geçerliliği ile oluşturulacaktır. Aşağıdaki ekran görüntüsünde gösterildiği gibi, oluşturulan belirtecin URI sürümüne ihtiyacınız olacaktır.

![Günlük yeniden yürütme hizmeti tarafından üretilen SAS kimlik doğrulama URI örneği](./media/log-replay-service-migrate/lrs-generated-uri-token.png)

### <a name="copy-parameters-from-sas-token-generated"></a>Oluşturulan SAS belirtecinden parametreleri Kopyala

LRS 'yi başlatmak için SAS belirtecini düzgün bir şekilde kullanabilmek için yapısını anlamanız gerekir. Oluşturulan SAS belirtecinin URI 'SI iki bölümden oluşur:
- StorageContainerUri ve 
- Aşağıdaki görüntüde gösterildiği gibi, bir soru işareti (?) ile ayrılmış StorageContainerSasToken.

    ![Günlük yeniden yürütme hizmeti tarafından üretilen SAS kimlik doğrulama URI örneği](./media/log-replay-service-migrate/lrs-token-structure.png)

- "Https://" ile başlayan ilk bölüm, soru işareti (?), LRS 'ye girişte olarak beslendiği StorageContainerURI parametresi için kullanılmaz. Bu, veritabanı yedekleme dosyalarının depolandığı klasör hakkında LRS bilgileri verir.
- İkinci bölüm, soru işaretinden (?) sonra, "SP =" örneğinde ve dizenin sonu StorageContainerSasToken parametresi olana kadar tüm şekilde başlar. Bu, belirtilen süre boyunca geçerli olan gerçek imzalı kimlik doğrulama belirtecidir. Bu bölümün gösterildiği gibi "SP =" ile başlaması gerekmez ve servis talebinizi farklılık gösterebilir.

Parametreleri şu şekilde kopyalayın:

1. Belirtecin ilk bölümünü, soru işaretine (?) kadar https://' dan başlayarak, aşağıdaki ekran görüntüsünde gösterildiği gibi, PowerShell 'de StorageContainerUri parametresi veya LRS 'yi başlatmak için CLı olarak kullanana kadar kopyalayın.

    ![Günlük yeniden yürütme hizmeti kopya StorageContainerUri parametresi](./media/log-replay-service-migrate/lrs-token-uri-copy-part-01.png)

2. Belirtecin, soru işareti (?) ile başlayan ikinci bölümünü, dizenin sonuna kadar tüm şekilde kopyalayın ve aşağıdaki ekran görüntüsünde gösterildiği gibi PowerShell ya da LRS 'yi başlatmak için PowerShell veya CLı ' de StorageContainerSasToken parametresi olarak kullanın.

    ![Günlük yeniden yürütme hizmeti kopyalama StorageContainerSasToken parametresi](./media/log-replay-service-migrate/lrs-token-uri-copy-part-02.png)

> [!IMPORTANT]
> - Azure Blob depolama için SAS belirtecinin izinlerinin salt okunmalıdır ve yalnızca liste olması gerekir. SAS kimlik doğrulama belirteci için başka herhangi bir izin verildiyse, LRS hizmetinin başlaması başarısız olur. Bu güvenlik gereksinimleri tasarıma göre yapılır.
> - Belirtecin uygun zaman geçerliliği olması gerekir. Belirteç ve yönetilen örnek arasındaki saat dilimlerinin dikkate alınması için dikkat edin.
> - PowerShell veya CLı için StorageContainerUri parametresinin, sorular işaretine (?) kadar https://'den başlayarak oluşturulan belirtecin URI 'sinden kopyalandığından emin olun. Soru işaretini eklemeyin.
> CLı PowerShell için StorageContainerSasToken parametresinin, dize sonuna kadar soru işaretinden (?) başlayarak oluşturulan belirtecin URI 'sinden kopyalandığından emin olun. Soru işaretini eklemeyin.

### <a name="log-in-to-azure-and-select-subscription"></a>Azure 'da oturum açın ve abonelik ' ı seçin

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

```PowerShell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" `
    -StorageContainerUri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D" `
    -AutoCompleteRestore `
    -LastBackupName "last_backup.bak"
```

LRS 'yi otomatik tamamlama modunda Başlat-CLı örneği:

```CLI
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb -a --last-bn "backup.bak"
    --storage-uri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="start-lrs-in-continuous-mode"></a>LRS 'yi sürekli modda Başlat

LRS 'yi sürekli modda başlatın-PowerShell örneği:

```PowerShell
Start-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName" `
    -Collation "SQL_Latin1_General_CP1_CI_AS" -StorageContainerUri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>" `
    -StorageContainerSasToken "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

LRS 'yi sürekli modda başlatın-CLı örneği:

```CLI
az sql midb log-replay start -g mygroup --mi myinstance -n mymanageddb
    --storage-uri "https://<mystorageaccountname>.blob.core.windows.net/<mycontainername>"
    --storage-sas "sv=2019-02-02&ss=b&srt=sco&sp=rl&se=2023-12-02T00:09:14Z&st=2019-11-25T16:09:14Z&spr=https&sig=92kAe4QYmXaht%2Fgjocqwerqwer41s%3D"
```

### <a name="scripting-lrs-start-in-continuous-mode"></a>LRS komut dosyası sürekli modda başladı

LRS 'yi sürekli modda başlatmak için PowerShell ve CLı istemcileri zaman uyumludur. Diğer bir deyişle, istemcilerin işi başlatmak için başarılı veya başarısız durumu raporlamak üzere API yanıtının bekleyeceği anlamına gelir. Bu bekleme sırasında komut, denetimi komut istemine geri döndürmez. Geçiş deneyimini el ile yapmanız ve LRS start komutunun, betiğin geri kalanı ile devam etmek için hemen yeniden denetim vermesini istiyorsanız, PowerShell 'i-AsJob anahtarı olan bir arka plan işi olarak çalıştırabilirsiniz. Örnek:

```PowerShell
$lrsjob = Start-AzSqlInstanceDatabaseLogReplay <required parameters> -AsJob
```

Bir arka plan işi başlattığınızda, işin bitiş tarihi uzasa bile bir iş nesnesi hemen döndürülür. İş çalışırken kesintiye uğramadan oturumda çalışmaya devam edebilirsiniz. PowerShell 'i bir arka plan işi olarak çalıştırmaya ilişkin ayrıntılar için bkz. [PowerShell başlangıç-iş](/powershell/module/microsoft.powershell.core/start-job#description) belgeleri.

Benzer şekilde, Linux üzerinde bir CLı komutunu arka plan işlemi olarak başlatmak için LRS start komutunun sonundaki ve işareti (&) işaretini kullanın.

```CLI
az sql midb log-replay start <required parameters> &
```

> [!IMPORTANT]
> LRS başlatıldıktan sonra, sistem tarafından yönetilen tüm yazılım düzeltme ekleri sonraki 47 saat boyunca durdurulur. Bu pencerenin geçirilmesi sırasında, sonraki otomatikleştirilmiş yazılım düzeltme eki, devam eden LRS 'yi otomatik olarak durdurur. Böyle bir durumda, geçiş devam ettirilemez ve sıfırdan yeniden başlatılması gerekiyor. 

## <a name="monitor-the-migration-progress"></a>Geçiş ilerlemesini izleme

Geçiş işleminin ilerlemesini izlemek için aşağıdaki PowerShell komutunu kullanın:

```PowerShell
Get-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Geçiş işleminin ilerlemesini izlemek için aşağıdaki CLı komutunu kullanın:

```CLI
az sql midb log-replay show -g mygroup --mi myinstance -n mymanageddb
```

## <a name="stop-the-migration"></a>Geçişi durdur

Geçişi durdurmanız gerekirse, aşağıdaki cmdlet 'leri kullanın. Geçişin durdurulması, geçiş işleminin sürdürülebilmesi için SQL yönetilen örneğindeki geri yükleme veritabanını siler.

Geçiş işlemini durdurmak için aşağıdaki PowerShell komutunu kullanın:

```PowerShell
Stop-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
    -InstanceName "ManagedInstance01" `
    -Name "ManagedDatabaseName"
```

Geçiş işlemini durdurmak için aşağıdaki CLı komutunu kullanın:

```CLI
az sql midb log-replay stop -g mygroup --mi myinstance -n mymanageddb
```

## <a name="complete-the-migration-continuous-mode"></a>Geçişi (sürekli mod) doldurun

LRS 'nin sürekli modda başlatılması durumunda, tüm yedeklemelerin geri yüklendiğinden emin olduktan sonra tam geçişi ' i başlatmak geçiş işlemi tamamlanır. Tam geçişi tamamlandığında, veritabanı geçirilir ve okuma ve yazma erişimi için hazırlanacaktır.

LRS sürekli modundaki geçiş işlemini gerçekleştirmek için aşağıdaki PowerShell komutunu kullanın:

```PowerShell
Complete-AzSqlInstanceDatabaseLogReplay -ResourceGroupName "ResourceGroup01" `
-InstanceName "ManagedInstance01" `
-Name "ManagedDatabaseName" `
-LastBackupName "last_backup.bak"
```

LRS sürekli modundaki geçiş işlemini gerçekleştirmek için aşağıdaki CLı komutunu kullanın:

```CLI
az sql midb log-replay complete -g mygroup --mi myinstance -n mymanageddb --last-backup-name "backup.bak"
```

## <a name="functional-limitations"></a>İşlevsel sınırlamalar

Günlük yeniden yürütme hizmeti 'nin (LRS) işlevsel sınırlamaları:
- Geri yüklenen veritabanı, geçiş işlemi sırasında salt okuma erişimi için kullanılamaz.
- Sistem tarafından yönetilen yazılım yamaları, LRS 'nin başlatılmasından bu yana 47 saat boyunca engellenir. Bu zaman süresi dolduktan sonra, bir sonraki yazılım güncelleştirmesi LRS 'yi durdurur. Bu durumda, LRS 'nin sıfırdan yeniden başlatılması gerekir.
- LRS, SQL Server sağlama TOPLAMı seçeneği etkinken yedeklenmek üzere bir veritabanı gerektirir.
- LRS tarafından kullanılacak SAS belirtecinin tüm Azure Blob depolama kapsayıcısı için oluşturulması gerekir ve yalnızca okuma ve listeleme izinlerine sahip olmalıdır.
- Farklı veritabanlarına yönelik yedekleme dosyaları, Azure Blob depolamada ayrı klasörlere yerleştirilmelidir.
- LRS 'lerin her bir veritabanı için, Azure Blob depolamada yedekleme dosyaları olan ayrı klasörlere işaret eden ayrı başlatılmalıdır.
- LRS, tek bir SQL yönetilen örneği başına en fazla 100 eşzamanlı geri yükleme işlemini destekleyebilir.

## <a name="troubleshooting"></a>Sorun giderme

LRS 'yi başlattığınızda, işlemin durumunu görmek için izleme cmdlet 'lerini (Get-azsqlınstancedatabaselogreplay veya az_sql_midb_log_replay_show) kullanın. Bir süre sonra LRS bir hata ile başlamazsa, en yaygın sorunlardan bazılarını kontrol edin:
- SQL Server geçirmeyi denediğiniz SQL MI üzerinde aynı ada sahip bir veritabanı zaten var mı? Veritabanlarından birini yeniden adlandırarak bu çakışmayı çözün.
- Veritabanı yedeklemesi, **sağlama toplamı** seçeneği kullanılarak yapılan SQL Server mı?
- SAS belirteci için **okuma** ve **Listeleme** izinleri yalnızca LRS hizmeti için mi?
- LRS için SAS belirteci, "SV = 2020-02-10..." aşağıdakine benzer şekilde başlayan içerik ile "?" soru işaretinden sonra başlayarak kopyalanabilir. 
- Geçiş işlemini başlatma ve tamamlama zaman penceresi için SAS **belirteci geçerlilik** süresi geçerli mi? SQL yönetilen örneği ve SAS belirteci için kullanılan farklı **saat dilimlerine** bağlı olarak uyuşmazlıklar olabilir. Geçerli tarihten önce ve sonra zaman penceresinin belirteç geçerliliğini genişleterek SAS belirtecini yeniden üretmeyi deneyin.
- Veritabanı adı, kaynak grubu adı ve yönetilen örnek adı doğru şekilde yazılmış mı?
- LRS, otomatik tamamlama modunda başlatıldıysa, belirtilen son yedekleme dosyası için geçerli bir dosya adı idi mı?

## <a name="next-steps"></a>Sonraki adımlar
- [SQL Server SQL yönetilen örneği 'Ne geçirme](../migration-guides/managed-instance/sql-server-to-managed-instance-guide.md)hakkında daha fazla bilgi edinin.
- [SQL Server Ile Azure SQL yönetilen örneği arasındaki farklılıklar](transact-sql-tsql-differences-sql-server.md)hakkında daha fazla bilgi edinin.
- [Azure 'a geçirilen iş yükleri Için en iyi uygulamalar](/azure/cloud-adoption-framework/migrate/azure-best-practices/migrate-best-practices-costs)hakkında daha fazla bilgi edinin.
