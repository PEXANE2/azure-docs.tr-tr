---
title: Sorun Giderme SQL Server veritabanı yedekleme
description: Azure VM'lerde çalışan SQL Server veritabanlarını Azure Yedekleme ile yedeklemek için sorun giderme bilgileri.
ms.topic: troubleshooting
ms.date: 06/18/2019
ms.openlocfilehash: 8d49adb0ab741903ccb2989cfeb4ceaef2e8a38d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79408625"
---
# <a name="troubleshoot-sql-server-database-backup-by-using-azure-backup"></a>Azure Yedekleme'yi kullanarak SQL Server veritabanı yedeklemesini sorun giderme

Bu makalede, Azure sanal makinelerde çalışan SQL Server veritabanları için sorun giderme bilgileri sağlanmaktadır.

Yedekleme işlemi ve sınırlamaları hakkında daha fazla bilgi için [Azure VM'lerde SQL Server yedeklemesi hakkında](sql-support-matrix.md#feature-consideration-and-limitations)bilgi alabiliyorum.

## <a name="sql-server-permissions"></a>SQL Server izinleri

Sanal bir makinede bir SQL Server veritabanı için koruma yapılandırmak için, bu sanal makineye **AzureBackupWindowsİş uzantısıyüklemeniz** gerekir. KullanıcıHatası **Alırsanız UserErrorSQLNoSysadminMembership**, bu SQL Server örneğinizin gerekli yedekleme izinlerine sahip olmadığı anlamına gelir. Bu hatayı gidermek için [VM izinleri](backup-azure-sql-database.md#set-vm-permissions)ayarla'daki adımları izleyin.

## <a name="troubleshoot-discover-and-configure-issues"></a>Sorunları bulma ve yapılandırma sorunlarını giderme

Kurtarma Hizmetleri kasası oluşturulduktan ve yapılandırıldıktan sonra, veritabanlarını keşfetmek ve yedeklemeyi yapılandırmak iki adımlı bir işlemdir.<br>

![sql](./media/backup-azure-sql-database/sql.png)

Yedekleme yapılandırması sırasında, SQL VM ve örnekleri **VM'lerde Discovery DB'lerde görünmüyorsa** ve **Yedeklemeyi Yapılandırılıyorsa** (yukarıdaki resme bakın) aşağıdakileri sağlayın:

### <a name="step-1-discovery-dbs-in-vms"></a>Adım 1: VM'lerde Bulma DB'leri

- VM keşfedilen VM listesinde listelenmemişse ve başka bir kasada SQL yedeklemesi için kayıtlı değilse, [Discovery SQL Server yedekleme](https://docs.microsoft.com/azure/backup/backup-sql-server-database-azure-vms#discover-sql-server-databases) adımlarını izleyin.

### <a name="step-2-configure-backup"></a>Adım 2: Yedeklemeyi Yapılandır

- SQL VM'nin veritabanlarını korumak için kullanılan aynı kasaya kayıtlı olduğu kasa, [Yedeklemeyi Yapılandır'ı uygulayın](https://docs.microsoft.com/azure/backup/backup-sql-server-database-azure-vms#configure-backup) adımlarını izleyin.

SQL VM'nin yeni kasaya kaydedilmesi gerekiyorsa, eski kasadan kayıtsız kalmalıdır.  Bir SQL VM'nin kasadan kaydedilmesinin durdurulması, tüm korumalı veri kaynaklarının korunmasını gerektirir ve ardından yedeklenen verileri silebilirsiniz. Yedeklenen verileri silerken yıkıcı bir işlemdir.  SQL VM'nin kaydını iptal etmek için tüm önlemleri gözden geçirdikten ve aldıktan sonra, aynı VM'yi yeni bir kasaya kaydedin ve yedekleme işlemini yeniden deneyin.

## <a name="troubleshoot-backup-and-recovery-issues"></a>Sorun Giderme Yedekleme ve Kurtarma sorunları  

Bazen, yedekleme ve geri yükleme işlemlerinde rasgele hatalar oluşabilir veya bu işlemler sıkışıp kalabilir. Bunun nedeni VM'nizdeki virüsten koruma programları olabilir. En iyi uygulama olarak, aşağıdaki adımları öneririz:

1. Aşağıdaki klasörleri virüsten koruma taramasından hariç tut:

    `C:\Program Files\Azure Workload Backup` `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.RecoveryServices.WorkloadBackup.Edp.AzureBackupWindowsWorkload`

    `C:\` *SystemDrive'ınızın*harfiile değiştirin.

1. Bir VM içinde çalışan aşağıdaki üç işlemi virüsten koruma taramasından hariç tut:

    - IaasWLPluginSvc.exe
    - IaasWorkloadCoordinaorService.exe
    - TriggerExtensionJob.exe

1. SQL ayrıca virüsten koruma programlarıyla çalışmak üzere bazı yönergeler de sunar. Ayrıntılar için [bu makaleye](https://support.microsoft.com/help/309422/choosing-antivirus-software-for-computers-that-run-sql-server) bakın.

## <a name="error-messages"></a>Hata iletileri

### <a name="backup-type-unsupported"></a>Desteklenmeyen yedekleme türü

| Severity | Açıklama | Olası nedenler | Önerilen eylem |
|---|---|---|---|
| Uyarı | Bu veritabanıiçin geçerli ayarlar ilişkili ilke mevcut belirli yedekleme türleri desteklemez. | <li>Ana veritabanında yalnızca tam bir veritabanı yedekleme işlemi gerçekleştirilebilir. Ne diferansiyel yedekleme ne de işlem günlüğü yedeklemesi mümkün değildir. </li> <li>Basit kurtarma modelindeki herhangi bir veritabanı işlem günlüklerinin yedeğine izin vermez.</li> | İlkede tüm yedekleme türleri desteklenir şekilde veritabanı ayarlarını değiştirin. Veya geçerli ilkeyi yalnızca desteklenen yedekleme türlerini içerecek şekilde değiştirin. Aksi takdirde, desteklenmeyen yedekleme türleri zamanlanan yedekleme sırasında atlanır veya yedekleme işi isteğe bağlı yedekleme için başarısız olur.

### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu SQL veritabanı, istenen yedekleme türünü desteklemiyor. | Veritabanı kurtarma modeli istenen yedekleme türüne izin vermediğinde oluşur. Hata aşağıdaki durumlarda gerçekleşebilir: <br/><ul><li>Basit bir kurtarma modeli kullanan bir veritabanı günlük yedeklemeye izin vermez.</li><li>Ana veritabanı için diferansiyel ve günlük yedeklemelerine izin verilmez.</li></ul>Daha fazla ayrıntı için [SQL Server kurtarma modelleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) belgelerine bakın. | Günlük yedeklemesi basit kurtarma modelinde veritabanı için başarısız olursa, aşağıdaki seçeneklerden birini deneyin:<ul><li>Veritabanı basit kurtarma modundaysa, günlük yedeklemelerini devre dışı dışı katın.</li><li>Veritabanı kurtarma modelini tam veya toplu günlüğe kaydedilmiş olarak değiştirmek için [SQL Server belgelerini](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) kullanın. </li><li> Kurtarma modelini değiştirmek istemiyorsanız ve değiştirilmeyen birden çok veritabanını yedeklemek için standart bir ilkeniz varsa, hatayı yoksayın. Tam ve diferansiyel yedeklemeleriniz zamanlamaya göre çalışır. Günlük yedekleri atlanır ve bu durumda bekleniyor.</li></ul>Ana veritabanıysa ve diferansiyel veya günlük yedeklemesini yapılandırmışsanız, aşağıdaki adımlardan birini kullanın:<ul><li>Ana veritabanının yedekleme ilkesi zamanlamasını tam olarak değiştirmek için portalı kullanın.</li><li>Değiştirilmeyen birden çok veritabanını yedeklemek için standart bir ilkeniz varsa, hatayı yoksayın. Tam desteğiniz zamanlamaya göre çalışacaktır. Diferansiyel veya günlük yedeklemeleri gerçekleşmez, bu durumda beklenen.</li></ul> |
| Çakışan bir işlem zaten aynı veritabanında çalışıyordu olarak işlem iptal edildi. | Aynı [anda çalışan yedekleme ve geri yükleme sınırlamaları hakkındaki blog girişine](https://deep.data.blog/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database/) bakın.| [Yedekleme işlerini izlemek için SQL Server Management Studio'yu (SSMS) kullanın.](manage-monitor-sql-database-backup.md) Çakışan işlem başarısız olduktan sonra, işlemi yeniden başlatın.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL veritabanı yok. | Veritabanı silindi veya yeniden adlandırıldı. | Veritabanının yanlışlıkla silinip silinmediğinden veya yeniden adlandırılınmı denetleyin.<br/><br/> Veritabanı yanlışlıkla silinmişse, yedeklemelere devam etmek için veritabanını özgün konuma geri yükleyin.<br/><br/> Veritabanını sildiyseniz ve gelecekteki yedeklemelere ihtiyacınız yoksa, Kurtarma Hizmetleri kasasında **Yedekleme verilerini koru** veya Yedekleme **Verilerini Sil**ile **yedeklemeyi durdur'u** seçin. Daha fazla bilgi için [bkz.](manage-monitor-sql-database-backup.md)

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Günlük zinciri bozuk. | Veritabanı veya VM, günlük zincirini tonuyla gösteren başka bir yedekleme çözümü yle yedeklenir.|<ul><li>Başka bir yedekleme çözümü veya komut dosyasının kullanIlip kullanılmayayayayaolmadığını kontrol edin. Bu öyleyse, diğer yedekleme çözümlerini durdurun. </li><li>Yedekleme isteğe bağlı günlük yedeklemese, yeni bir günlük zinciri başlatmak için tam bir yedeklemeyi tetikler. Zamanlanmış günlük yedeklemeleri için herhangi bir işlem gerekmez, çünkü Azure Yedekleme hizmeti bu sorunu gidermek için otomatik olarak tam bir yedekleme tetikler.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure Yedekleme, SQL örneğine bağlanamaz. | Azure Yedekleme, SQL Server örneğine bağlanabilir. | Temel nedenleri daraltmak için Azure portalı hata menüsündeki ek ayrıntıları kullanın. Hatayı düzeltmek için [SQL yedekleme sorun giderme](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) sorununa bakın.<br/><ul><li>Varsayılan SQL ayarları uzak bağlantılara izin vermiyorsa, ayarları değiştirin. Ayarları değiştirme hakkında bilgi için aşağıdaki makalelere bakın:<ul><li>[MSSQLSERVER_-1](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-1-database-engine-error?view=sql-server-ver15)</li><li>[MSSQLSERVER_2](/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[MSSQLSERVER_53](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Oturum açma sorunları varsa, bunları düzeltmek için bu bağlantıları kullanın:<ul><li>[MSSQLSERVER_18456](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[MSSQLSERVER_18452](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu veri kaynağı için ilk tam yedekleme eksik. | Veritabanı için tam yedekleme eksik. Günlük ve diferansiyel yedeklemeler tam yedeklemeye ebeveynleridir, bu nedenle diferansiyel veya günlük yedeklemelerini tetiklemeden önce tam yedeklemeleri aldığından emin olun. | İsteğe bağlı tam yedeklemeyi tetikler.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veri kaynağı için işlem günlüğü dolu olduğu için yedekleme alamaz. | Veritabanı işlem günlüğü alanı dolu. | Bu sorunu gidermek için [SQL Server belgelerine](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error)bakın. |

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Hedef konumda aynı ada sahip veritabanı zaten var | Hedef geri yükleme hedefi zaten aynı ada sahip bir veritabanına sahiptir.  | <ul><li>Hedef veritabanı adını değiştirin.</li><li>Veya geri yükleme sayfasındaki kuvvet üzerine yazma seçeneğini kullanın.</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veritabanı çevrimdışına alınamadığından geri yükleme başarısız oldu. | Geri yükleme yaparken, hedef veritabanının çevrimdışı duruma getirilmesi gerekir. Azure Yedekleme bu verileri çevrimdışı getiremez. | Temel nedenleri daraltmak için Azure portalı hata menüsündeki ek ayrıntıları kullanın. Daha fazla bilgi için bkz. [SQL Server belgeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |

### <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Hedefte parmak izi olan sunucu sertifikasını bulamıyor. | Hedef örneğindeki ana veritabanında geçerli bir şifreleme parmak izi yoktur. | Kaynak örnekte kullanılan geçerli sertifika parmak izini hedef örneğe aktarın. |

### <a name="usererrorrestorenotpossiblebecauselogbackupcontainsbulkloggedchanges"></a>UserErrorRestoreNotPossibleBecauseLogBackupContainsBulkLoggedChanges

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Kurtarma için kullanılan günlük yedeklemesi toplu olarak günlüğe kaydedilen değişiklikler içeriyor. SQL yönergelerine göre zamanın rastgele bir noktasında durdurmak için kullanılamaz. | Bir veritabanı toplu günlüğe kaydedilmiş kurtarma modundayken, toplu günlüğe kaydedilmiş bir hareket le sonraki günlük hareketi arasındaki veriler kurtarılamaz. | Kurtarma için zaman içinde farklı bir nokta seçin. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server?view=sql-server-ver15).

### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Kullanılabilirlik Grubunun bazı düğümleri kaydedilmediğinden SQL AlwaysOn Kullanılabilirlik Grubu yedekleme tercihi uygulanamadı. | Yedeklemeleri gerçekleştirmek için gereken düğümler kayıtlı değildir veya erişilemez. | <ul><li>Bu veritabanının yedeklemelerini gerçekleştirmek için gereken tüm düğümlerin kayıtlı ve sağlıklı olduğundan emin olun ve ardından işlemi yeniden deneyin.</li><li>KULLANıLABILIRlik grubunda SQL Server Always Her Zaman yedekleme tercihini değiştirin.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL sunucusu VM ya kapatılır ve Azure Yedekleme hizmeti için erişilemez. | VM kapatıldı. | SQL Server örneğinin çalıştığını sağlayın. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure Yedekleme hizmeti yedekleme yapmak için Azure VM konuk aracısını kullanır, ancak konuk aracı hedef sunucuda kullanılamaz. | Konuk aracı etkin değil veya sağlıksız. | [VM konuk aracısını](../virtual-machines/extensions/agent-windows.md) el ile yükleyin. |

### <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Otomatik koruma Amacı kaldırıldı veya artık geçerli değil. | Bir SQL Server örneğinde otomatik korumayı etkinleştirdiğinizde, Yedekleme işlerini **yapılandırın,** bu örnekteki tüm veritabanları için çalıştırın. İşler çalışırken otomatik korumayı devre dışı bederseniz, **Devam Eden** işler bu hata koduyla iptal edilir. | Kalan tüm veritabanlarının korunmasına yardımcı olmak için otomatik korumayı bir kez daha etkinleştirin. |

### <a name="clouddosabsolutelimitreached"></a>CloudDosAbsoluteLimitReached

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
24 saat içinde izin verilen operasyon sayısı sınırına ulaştığınız için işlem engellenir. | 24 saatlik bir süre içinde bir işlem için izin verilen maksimum sınıra ulaştığınızda, bu hata gelir. <br> Örneğin: Günde tetiklenebilen yapılandırma yedekleme işleri sayısı sınırına ulaştıysanız ve yedeklemeyi yeni bir öğede yapılandırmaya çalışıyorsanız, bu hatayı görürsünüz. | Genellikle, 24 saat sonra işlemi yeniden denemek bu sorunu giderir. Ancak, sorun devam ederse, yardım için Microsoft desteğine başvurabilirsiniz.

### <a name="clouddosabsolutelimitreachedwithretry"></a>CloudDosAbsoluteLimitReachedWithRetry

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
Kasa, 24 saatlik bir süre içinde izin verilen bu tür işlemler için maksimum sınırına ulaştığından işlem engellenir. | 24 saatlik bir süre içinde bir işlem için izin verilen maksimum sınıra ulaştığınızda, bu hata gelir. Bu hata genellikle ilke yi değiştirme veya otomatik koruma gibi ölçekte işlemler olduğunda ortaya çıkar. CloudDosAbsoluteLimitReached durumunda aksine, bu durumu çözmek için yapabileceğiniz pek bir şey yoktur, aslında, Azure Yedekleme hizmeti söz konusu tüm öğeler için dahili olarak işlemleri yeniden dener.<br> Örneğin: Bir ilkeyle korunan çok sayıda veri kaynağınız varsa ve bu ilkeyi değiştirmeye çalışırsanız, korunan öğelerin her biri için yapılandırma koruma işlerini tetikler ve bazen bu tür işlemler için günlük izin verilen maksimum sınırı vurabilir.| Azure Yedekleme hizmeti, 24 saat sonra bu işlemi otomatik olarak yeniden dener.

### <a name="usererrorvminternetconnectivityissue"></a>UserErrorVMInternetConnectivityIssue

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
VM, internet bağlantısı sorunları nedeniyle Azure Yedekleme hizmetiyle iletişim kuramaz. | VM' nin Azure Yedekleme Hizmeti, Azure Depolama veya Azure Etkin Dizin hizmetleri yle giden bağlantıya ihtiyacı vardır.| - Bağlantıyı kısıtlamak için NSG kullanıyorsanız, Azure Yedekleme'ye, Azure Yedekleme Hizmetine, Azure Depolama sına veya Azure Etkin Dizin hizmetlerine giden erişime izin vermek için AzureBackup hizmet etiketini kullanmanız gerekir. Erişim sağlamak için aşağıdaki [adımları](https://docs.microsoft.com/azure/backup/backup-sql-server-database-azure-vms#allow-access-using-nsg-tags) izleyin.<br>- DNS'nin Azure uç noktalarını çözdüğünden emin olun.<br>- VM'nin internet erişimini engelleyen bir yük dengeleyicisinin arkasında olup olmadığını kontrol edin. Genel IP'yi VM'lere atayarak, keşif çalışır.<br>- Yukarıdaki üç hedef servise yapılan aramaları engelleyen güvenlik duvarı/virüsten koruma/proxy olmadığını doğrulayın.

## <a name="re-registration-failures"></a>Yeniden kayıt hataları

Yeniden kaydetme işlemini tetiklemeden önce aşağıdaki belirtilerden birini veya birkaçını denetleyin:

* Tüm işlemler (yedekleme, geri yükleme ve yedekleme yapılandırma gibi) aşağıdaki hata kodlarından biriyle VM'de başarısız oluyor: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent , WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**.
* Yedekleme öğesinin **Yedekleme Durumu** alanı **erişilemeyeceğini**gösteriyorsa, aynı durumla sonuçlanabilecek diğer tüm nedenleri ekarte edin:

  * VM'de yedeklemeyle ilgili işlemleri gerçekleştirmek için izin eksikliği.
  * VM'nin kapatılması, bu nedenle yedeklemeler gerçekleşemez.
  * Ağ sorunları.

   ![VM'yi yeniden kaydetme](./media/backup-azure-sql-database/re-register-vm.png)



* Her Zaman Kullanılabilirlik grubunda, yedekleme tercihini değiştirdikten sonra veya bir başarısızlıktan sonra yedeklemeler başarısız lığa başladı.

Bu belirtiler aşağıdaki nedenlerden biri veya daha fazlası için ortaya çıkabilir:

* Bir uzantı silindi veya portaldan kaldırıldı.
* Bir uzantı, VM'deki **Denetim Paneli'nden** **Programı Kaldır veya Değiştir**altında kaldırıldı.
* VM, yerinde disk geri yüklemeyoluyla zamanda geri yüklendi.
* VM uzun bir süre için kapatıldı, bu nedenle üzerindeki uzantı yapılandırması sona erdi.
* VM silindi ve silinen VM ile aynı ada ve aynı kaynak grubunda başka bir VM oluşturuldu.
* Kullanılabilirlik grubu düğümlerinden biri tam yedekleme yapılandırmasını almadı. Bu durum grubu kasaya kaydedildiğinde veya yeni bir düğüm eklendiğinde gerçekleşebilir.

Önceki senaryolarda, VM'de yeniden kaydetme işlemini tetiklemenizi öneririz. PowerShell'de bu görevi nasıl gerçekleştiracağınız hakkındaki talimatlar için [buraya](https://docs.microsoft.com/azure/backup/backup-azure-sql-automation#enable-backup) bakın.

## <a name="size-limit-for-files"></a>Dosyalar için boyut sınırı

Dosyaların toplam dize boyutu yalnızca dosya sayısına değil, adlarına ve yollarına da bağlıdır. Her veritabanı dosyası için mantıksal dosya adını ve fiziksel yolu alın. Bu SQL sorgusuk kullanabilirsiniz:

```sql
SELECT mf.name AS LogicalName, Physical_Name AS Location FROM sys.master_files mf
               INNER JOIN sys.databases db ON db.database_id = mf.database_id
               WHERE db.name = N'<Database Name>'"
```

Şimdi bunları aşağıdaki biçimde düzenleyin:

```json
[{"path":"<Location>","logicalName":"<LogicalName>","isDir":false},{"path":"<Location>","logicalName":"<LogicalName>","isDir":false}]}
```

Bir örneği aşağıda verilmiştir:

```json
[{"path":"F:\\Data\\TestDB12.mdf","logicalName":"TestDB12","isDir":false},{"path":"F:\\Log\\TestDB12_log.ldf","logicalName":"TestDB12_log","isDir":false}]}
```

İçeriğin dize boyutu 20.000 baytı aşarsa, veritabanı dosyaları farklı şekilde depolanır. Kurtarma sırasında, geri yükleme için hedef dosya yolunu ayarlayamazsınız. Dosyalar, SQL Server tarafından sağlanan varsayılan SQL yoluna geri yüklenir.

### <a name="override-the-default-target-restore-file-path"></a>Varsayılan hedef geri yükleme dosya yolunu geçersiz kılma

Veritabanı dosyasının eşlemesini içeren bir JSON dosyasını hedef geri yükleme yoluna yerleştirerek geri yükleme işlemi sırasında hedef geri yükleme dosya yolunu geçersiz kılabilirsiniz. Bir `database_name.json` dosya oluşturun ve *c:\program dosyaları\Azure İş Yükü Yedekleme\bin\eklentileri\SQL*konumuna yerleştirin.

Dosyanın içeriği bu biçimde olmalıdır:

```json
[
  {
    "Path": "<Restore_Path>",
    "LogicalName": "<LogicalName>",
    "IsDir": "false"
  },
  {
    "Path": "<Restore_Path>",
    "LogicalName": "LogicalName",
    "IsDir": "false"
  },  
]
```

Bir örneği aşağıda verilmiştir:

```json
[
  {
   "Path": "F:\\Data\\testdb2_1546408741449456.mdf",
   "LogicalName": "testdb7",
   "IsDir": "false"
  },
  {
    "Path": "F:\\Log\\testdb2_log_1546408741449456.ldf",
    "LogicalName": "testdb7_log",
    "IsDir": "false"
  },  
]
```

Önceki içerikte, aşağıdaki SQL sorgusunu kullanarak veritabanı dosyasının mantıksal adını alabilirsiniz:

```sql
SELECT mf.name AS LogicalName FROM sys.master_files mf
                INNER JOIN sys.databases db ON db.database_id = mf.database_id
                WHERE db.name = N'<Database Name>'"
  ```

Bu dosya, geri yükleme işlemini tetiklemeden önce yerleştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

SQL Server VM'ler için Azure Yedekleme (genel önizleme) hakkında daha fazla bilgi [için SQL VM'ler için Azure Yedekleme'ye](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup)bakın.
