---
title: Sürüm Notları
description: Azure SQL Veritabanı hizmetinde ve Azure SQL Veritabanı belgelerinde yeni özellikler ve geliştirmeler hakkında bilgi edinin
services: sql-database
author: stevestein
ms.service: sql-database
ms.subservice: service
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: sstein
ms.openlocfilehash: 9fa93af72c2869efd7b6d2f1e8b96b0e667f8b16
ms.sourcegitcommit: 515482c6348d5bef78bb5def9b71c01bb469ed80
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80607545"
---
# <a name="sql-database-release-notes"></a>SQL Veritabanı sürüm notları

Bu makalede, şu anda genel önizlemede olan SQL Veritabanı özellikleri listelenir. SQL Veritabanı güncelleştirmeleri ve iyileştirmeleri için [SQL Veritabanı hizmet güncelleştirmelerine](https://azure.microsoft.com/updates/?product=sql-database)bakın. Diğer Azure hizmetlerinde güncelleştirmeler ve iyileştirmeler için [Hizmet güncelleştirmelerine](https://azure.microsoft.com/updates)bakın.

## <a name="features-in-public-preview"></a>Genel önizlemedeki özellikler

### <a name="single-database"></a>[Tek veritabanı](#tab/single-database)

| Özellik | Ayrıntılar |
| ---| --- |
| Yeni Fsv2 serisi ve M serisi donanım nesilleri| Bilgi için Donanım [nesillerini](sql-database-service-tiers-vcore.md#hardware-generations)görün.|
| [Azure özel bağlantısı](https://azure.microsoft.com/updates/private-link-now-available-in-preview/)| Private Link, Azure ağındaki verileri tutarak ağ mimarisini basitleştirir ve Azure'daki uç noktalar arasındaki bağlantıyı güvence altına alarak internete maruz kalmamayı ortadan kaldırır. Private Link ayrıca Azure'da kendi hizmetlerinizi oluşturmanıza ve oluşturmanıza olanak tanır. |
| Tek veritabanları ve elastik havuzlar ile hızlandırılmış veritabanı kurtarma | Daha fazla bilgi için [bkz.](sql-database-accelerated-database-recovery.md)|
|Yaklaşık Sayı Farklı|Bilgi için [bkz.](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing)|
|Rowstore'da Toplu İşlem Modu (uyumluluk düzeyi 150 altında)|Daha fazla bilgi için [Rowstore'daki Toplu İşlem Modu'na](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore)bakın.|
| Veri bulma ve sınıflandırma  |Bilgi için Azure [SQL Veritabanı ve SQL Veri Ambarı veri bulma & sınıflandırmaya](sql-database-data-discovery-and-classification.md)bakın.|
| Esnek veritabanı işleri | Bilgi için [bkz.](elastic-jobs-overview.md) |
| Esnek sorgular | Daha fazla bilgi için [Elastik sorguya genel bakış](sql-database-elastic-query-overview.md)bakın. |
| Elastik işlemler | [Bulut veritabanları arasında dağıtılmış hareketler.](sql-database-elastic-transactions-overview.md) |
|Bellek Hibe Geri Bildirimi (Satır Modu) (uyumluluk düzeyi 150 altında)|Daha fazla bilgi için Bkz. [Bellek Hibe Geri Bildirimi (Satır Modu)](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback).|
| Azure portalında sorgu düzenleyicisi |Daha fazla bilgi [için, verileri bağlamak ve sorgulamak için Azure portalının SQL sorgu düzenleyicisini kullanın' a](sql-database-connect-query-portal.md)bakın.|
| Tek veritabanları ve elastik havuzlar ile R hizmetleri / makine öğrenimi |Daha fazla bilgi için [Azure SQL Veritabanı'ndaki Makine Öğrenimi Hizmetleri'ne](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database)bakın.|
|SQL Analytics|Daha fazla bilgi için Azure [SQL Analytics](../azure-monitor/insights/azure-sql.md)bölümüne bakın.|
|Tablo Değişken Ertelenmiş Derleme (uyumluluk düzeyi 150 altında)|Bilgi için tablo [değişkeni ertelenmiş derlemeye](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation)bakın.|
| &nbsp; |

### <a name="managed-instance"></a>[Yönetilen Örnek](#tab/managed-instance)

| Özellik | Ayrıntılar |
| ---| --- |
| <a href="/azure/sql-database/sql-database-instance-pools">Örnek havuzları</a> | Daha küçük SQL örneklerini buluta geçirmenin kullanışlı ve uygun maliyetli bir yolu. |
| <a href="https://aka.ms/managed-instance-aadlogins">Örnek düzeyinde Azure AD sunucu ilkeleri (oturum açmalar)</a> | <a href="https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">DıŞ Sağlayıcı deyiminden CREATE Gİrİşİ'ni</a> kullanarak sunucu düzeyinde oturum açmalar oluşturun. |
| [İşlemsel Çoğaltma](sql-database-managed-instance-transactional-replication.md) | Tablolarınızdaki değişiklikleri Yönetilen Örnekler, Tek Veritabanları veya SQL Server örneklerine yerleştirilen diğer veritabanlarına çoğaltmaveya diğer Yönetilen Örnekler veya SQL Server örneğinde bazı satırlar değiştirildiğinde tablolarınızı güncelleştirin. Bilgi için bkz. [Azure SQL Veritabanı yönetilen örnek veritabanında çoğaltmayı yapılandırma.](replication-with-sql-database-managed-instance.md) |
| Tehdit algılama |Bilgi için bkz: [Azure SQL Veritabanı yönetilen örneğinde tehdit algılamayı yapılandırma.](sql-database-managed-instance-threat-detection.md)|

---

## <a name="managed-instance---new-features-and-known-issues"></a>Yönetilen örnek - yeni özellikler ve bilinen sorunlar

### <a name="managed-instance-h2-2019-updates"></a>Yönetilen örnek H2 2019 güncelleştirmeleri

- [Servis destekli subnet yapılandırması](https://azure.microsoft.com/updates/service-aided-subnet-configuration-for-managed-instance-in-azure-sql-database-available/) Yönetilen örnek, yönetim trafiğinin kesintisiz akışını sağlarken veri trafiğini kontrol ettiğiniz alt ağ yapılandırmasını yönetmenin güvenli ve kullanışlı bir yolu
- [Bring Your Own Key (BYOK) ile saydam veri şifrelemesi (TDE),](https://azure.microsoft.com/updates/general-avilability-transparent-data-encryption-with-customer-managed-keys-for-azure-sql-database-managed-instance/) veri koruması için kendi anahtarını getir (BYOK) senaryosuna olanak tanır ve kuruluşların anahtarlar ve veriler için yönetim görevlerini ayırmasına olanak tanır.
- [Otomatik hata grupları,](https://azure.microsoft.com/updates/azure-sql-database-auto-failover-groups-feature-now-available-in-all-regions/) birincil örnekten başka bir bölgedeki ikincil örnek için tüm veritabanlarını çoğaltmanızı sağlar.
- Yönetilen örnek davranışınızı [Genel izleme bayraklarıyla](https://azure.microsoft.com/updates/global-trace-flags-are-now-available-in-azure-sql-database-managed-instance/)yapılandırın.

### <a name="managed-instance-h1-2019-updates"></a>Yönetilen örnek H1 2019 güncelleştirmeleri

H1 2019'da Yönetilen örnek dağıtım modelinde aşağıdaki özellikler etkinleştirilir:
  - <a href="https://aka.ms/sql-mi-visual-studio-subscribers">Visual Studio aboneleri için Azure aylık kredisi</a> ve artan bölgesel limitler ile [abonelikler](sql-database-managed-instance-resource-limits.md#regional-resource-limitations)için destek.
  - <a href="https://docs.microsoft.com/sharepoint/administration/deploy-azure-sql-managed-instance-with-sharepoint-servers-2016-2019"> SharePoint 2016 ve SharePoint 2019 </a> ile <a href="https://docs.microsoft.com/business-applications-release-notes/october18/dynamics365-business-central/support-for-azure-sql-database-managed-instance"> Dynamics 365 Business Central </a> için destek
  - Sunucu düzeyinde <a href="https://aka.ms/managed-instance-collation">harmanlama</a> ve seçtiğiniz <a href="https://azure.microsoft.com/updates/managed-instance-time-zone-ga/">saat dilimi</a> ile örnekler oluşturun.
  - Yönetilen örnekler artık <a href="sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md">yerleşik güvenlik duvarıyla</a>korunmaktadır.
  - Daha iyi ağ performansı elde etmek için [genel uç noktaları](sql-database-managed-instance-public-endpoint-configure.md)kullanmak için örnekleri yapılandırın, Proxy geçersiz [kılma](sql-database-connectivity-architecture.md#connection-policy) bağlantısı, <a href="https://aka.ms/four-cores-sql-mi-update">Gen5 donanım üretiminde 4 vCore</a> sayılsın veya zamanında geri yükleme için <a href="https://aka.ms/managed-instance-configurable-backup-retention">35 güne kadar yedekleme bekletme yapılandırın.</a> Uzun süreli yedekleme bekletme (10 yıla kadar) hala etkin değildir, bu nedenle <a href="https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server">yalnızca kopyayedeklerini</a> alternatif olarak kullanabilirsiniz.
  - Yeni işlevler <a href="https://medium.com/@jocapc/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa">PowerShell kullanarak veritabanınızı başka bir veri merkezine coğrafi olarak geri yüklemenizi</a>sağlar, [veritabanını yeniden adlandırın,](https://azure.microsoft.com/updates/azure-sql-database-managed-instance-database-rename-is-supported/) [sanal kümeyi silmenizi](sql-database-managed-instance-delete-virtual-cluster.md).
  - Yeni yerleşik [Örnek Katılımcı rolü,](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-managed-instance-contributor) görev ayrılığı (SoD) güvenlik ilkelerine ve kurumsal standartlara uyuma olanak sağlar.
  - Yönetilen örnek, aşağıdaki Azure Devlet bölgelerinde GA (US Gov Texas, US Gov Arizona) ve Çin Kuzey 2 ve Çin Doğu 2'ye kullanılabilir. Avustralya Merkez, Avustralya Merkez 2, Brezilya Güney, Fransa Güney, BAE Merkez, BAE Kuzey, Güney Afrika Kuzey, Güney Afrika Batı: Ayrıca aşağıdaki ortak bölgelerde mevcuttur.

### <a name="known-issues"></a>Bilinen sorunlar

|Sorun  |Keşfedilen tarih  |Durum  |Çözülen tarih  |
|---------|---------|---------|---------|
|[Yönetilen Örnek'e uygulanmayan kaynak grubundaki izinler](#permissions-on-resource-group-not-applied-to-managed-instance)|Şub 2020|Geçici Çözüm Var||
|[Failover grupları için portal üzerinden manuel failover sınırlaması](#limitation-of-manual-failover-via-portal-for-failover-groups)|Ocak 2020|Geçici Çözüm Var||
|[SQL Agent rolleri olmayan sysadmin girişleri için açık EXECUTE izinleri gerekir](#in-memory-oltp-memory-limits-are-not-applied)|Aralık 2019|Geçici Çözüm Var||
|[SQL Agent işleri Agent process restart tarafından kesilebilir](#sql-agent-jobs-can-be-interrupted-by-agent-process-restart)|Aralık 2019|Geçici Çözüm Yok|Mar 2020|
|[AAD girişleri ve kullanıcılar SSDT'de desteklenmiyor](#aad-logins-and-users-are-not-supported-in-ssdt)|Kasım 2019|Geçici Çözüm Yok||
|[Bellek içi OLTP bellek sınırları uygulanmaz](#in-memory-oltp-memory-limits-are-not-applied)|Eki 2019|Geçici Çözüm Var||
|[Boş olmayan bir dosyayı kaldırmaya çalışırken yanlış hata döndürülür](#wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty)|Eki 2019|Geçici Çözüm Var||
|[Hizmet katmanını değiştirin ve örnek işlemleri oluşturmak devam eden veritabanı geri yüklemesi tarafından engellenir](#change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore)|Eyl 2019|Geçici Çözüm Var||
|[İş Kritik hizmet katmanında Kaynak Yöneticisi'nin başarısız olduktan sonra yeniden yapılandırılması gerekebilir](#resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover)|Eyl 2019|Geçici Çözüm Var||
|[Çapraz veritabanı Hizmet Aracısı iletişim kutuları, hizmet katmanı yükseltmeden sonra yeniden başlatılması gerekir](#cross-database-service-broker-dialogs-must-be-re-initialized-after-service-tier-upgrade)|Ağu 2019|Geçici Çözüm Var||
|[Azure AD oturum açma türlerinin kişileştirilmesi desteklenmiyor](#impersonification-of-azure-ad-login-types-is-not-supported)|Temmuz 2019|Geçici Çözüm Yok||
|[@querysp_send_db_mail'da desteklenmeyen parametre](#-parameter-not-supported-in-sp_send_db_mail)|Nisan 2019|Geçici Çözüm Yok||
|[İşlemsel Çoğaltma, coğrafi hatadan sonra yeniden yapılandırılmalıdır](#transactional-replication-must-be-reconfigured-after-geo-failover)|Mar 2019|Geçici Çözüm Yok||
|[RESTORE işlemi sırasında geçici veritabanı kullanılır](#temporary-database-is-used-during-restore-operation)||Geçici Çözüm Var||
|[TEMPDB yapısı ve içeriği yeniden oluşturulur](#tempdb-structure-and-content-is-re-created)||Geçici Çözüm Yok||
|[Küçük veritabanı dosyalarıyla depolama alanını aşma](#exceeding-storage-space-with-small-database-files)||Geçici Çözüm Var||
|[Veritabanı adları yerine gösterilen GUID değerleri](#guid-values-shown-instead-of-database-names)||Geçici Çözüm Var||
|[Hata günlükleri kalıcı değil](#error-logs-arent-persisted)||Geçici Çözüm Yok||
|[Aynı örnekteki iki veritabanındaki işlem kapsamı desteklenmiyor](#transaction-scope-on-two-databases-within-the-same-instance-isnt-supported)||Geçici Çözüm Var||
|[CLR modülleri ve bağlantılı sunucular bazen yerel bir IP adresine başvuru yapamaz](#clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address)||Geçici Çözüm Var||
|Veritabanı tutarlılığı Azure Blob Depolama veritabanı geri yükledikten sonra DBCC CHECKDB kullanılarak doğrulanmadı.||Çözümlendi|Kasım 2019|
|Kaynak veritabanı bellek içi OLTP nesneleri içeriyorsa, Business Critical katmanından Genel Amaç katmanına zaman içinde veritabanı geri yüklemesi başarılı olmaz.||Çözümlendi|Eki 2019|
|Güvenli bağlantı kullanan harici (Azure olmayan) posta sunucuları ile Veritabanı Posta özelliği||Çözümlendi|Eki 2019|
|Yönetilen örnekte desteklenmeyen veritabanları içerdiği||Çözümlendi|Ağu 2019|

### <a name="permissions-on-resource-group-not-applied-to-managed-instance"></a>Yönetilen örneğe uygulanmayan kaynak grubundaki izinler

Yönetilen Örnek Katılımcı RBAC rolü bir kaynak grubuna (RG) uygulandığında Yönetilen Örnek'e uygulanmaz ve hiçbir etkisi yoktur.

**Geçici Çözüm**: Abonelik düzeyindeki kullanıcılar için Kurulum Yönetilen Örnek Katılımcı rolü.

### <a name="limitation-of-manual-failover-via-portal-for-failover-groups"></a>Failover grupları için portal üzerinden manuel failover sınırlaması

Başarısız grup farklı Azure abonelikleri veya kaynak gruplarındaki örnekler arasında yayılıyorsa, başarısız gruptaki birincil örnekten el ile başarısızlık başlatılamaz.

**Geçici Çözüm**: Jeo-ikincil örnekten portal üzerinden başarısız lık başlatın.

### <a name="sql-agent-roles-need-explicit-execute-permissions-for-non-sysadmin-logins"></a>SQL Agent rolleri olmayan sysadmin girişleri için açık EXECUTE izinleri gerekir

[SQL Agent sabit veritabanı rollerinden](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent-fixed-database-roles)herhangi biri ne sysadmin girişleri eklenirse, bu oturum açmaların çalışması için ana depolanan yordamlara açık EXECUTE izinlerinin verilmesi gereken bir sorun vardır. Bu sorunla karşılaşılırsa, "Execute izni nesne üzerinde reddedildi <object_name> (Microsoft SQL Server, Hata: 229)" hatası iletisi gösterilir.

**Geçici Çözüm**: SQL Agent sabit veritabanı rollerinden birine giriş ekledikten sonra: SQLAgentUserRole, SQLAgentReaderRole veya SQLAgentOperatorRole, bu rollere eklenen girişlerin her biri için aşağıdaki T-SQL komut dosyasını uygulayın ve listelenen kayıtlı yordamlara execute izinleri açıkça verir.

```tsql
USE [master]
GO
CREATE USER [login_name] FOR LOGIN [login_name]
GO
GRANT EXECUTE ON master.dbo.xp_sqlagent_enum_jobs TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_is_starting TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_notify TO [login_name]
```

### <a name="sql-agent-jobs-can-be-interrupted-by-agent-process-restart"></a>SQL Agent işleri Agent process restart tarafından kesilebilir

SQL Agent, iş her başlatıldığında yeni bir oturum oluşturur ve bellek tüketimini kademeli olarak artırır. Zamanlanan işlerin yürütülmesini engelleyecek dahili bellek sınırına çarpmamak için, bellek tüketimi eşiğe ulaştığında Aracı işlemi yeniden başlatılır. Yeniden başlatma anında çalışan işlerin yürütülmesinin kesintiye uğraması ile sonuçlanabilir.

### <a name="in-memory-oltp-memory-limits-are-not-applied"></a>Bellek içi OLTP bellek sınırları uygulanmaz

İş Kritik hizmet katmanı, bazı durumlarda [bellek için en iyi duruma getirilmiş nesneler için maksimum bellek sınırlarını](sql-database-managed-instance-resource-limits.md#in-memory-oltp-available-space) doğru şekilde uygulamaz. Yönetilen örnek, iş yükünün bellek içi OLTP işlemleri için daha fazla bellek kullanmasına olanak sağlayabilir ve bu da örneğin kullanılabilirliğini ve kararlılığını etkileyebilir. Sınırlara ulaşan bellek içi OLTP sorguları hemen başarısız olmayabilir. Bu sorun yakında giderilecektir. Daha fazla Bellek IÇI OLTP belleği kullanan [sorgular, sınırlara](sql-database-managed-instance-resource-limits.md#in-memory-oltp-available-space)ulaşırsa daha erken başarısız olur.

**Geçici Çözüm:** İş yükünün kullanılabilir bellekten daha fazlasını kullanmadığından emin olmak için [SQL Server Management Studio'yu](/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage#bkmk_Monitoring) kullanarak bellek [içi OLTP depolama kullanımını izleyin.](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitoring) vCoresayısına bağlı olan bellek sınırlarını artırın veya daha az bellek kullanmak için iş yükünüzü optimize edin.

### <a name="wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty"></a>Boş olmayan bir dosyayı kaldırmaya çalışırken yanlış hata döndürülür

SQL Server/Yönetilen [Örnek, kullanıcının boş olmayan bir dosyayı bırakmasına izin vermez.](/sql/relational-databases/databases/delete-data-or-log-files-from-a-database#Prerequisites) İfadeyi kullanarak `ALTER DATABASE REMOVE FILE` boş olmayan bir veri dosyasını `Msg 5042 – The file '<file_name>' cannot be removed because it is not empty` kaldırmaya çalışırsanız, hata hemen döndürülmez. Yönetilen Örnek dosyayı düşürmeye çalışırken devam edecek ve işlem `Internal server error`30min sonra başarısız olacak .

**Geçici Çözüm**: Komutu kullanarak `DBCC SHRINKFILE (N'<file_name>', EMPTYFILE)` dosyanın içeriğini kaldırın. Dosya grubundaki tek dosya buysa, dosyayı küçültmeden önce bu dosya grubuyla ilişkili tablo veya bölümverilerini silmeniz ve isteğe bağlı olarak bu verileri başka bir tabloya/bölüme yüklemeniz gerekir.

### <a name="change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore"></a>Hizmet katmanını değiştirin ve örnek işlemleri oluşturmak devam eden veritabanı geri yüklemesi tarafından engellenir

Devam `RESTORE` eden deyim, Veri Geçişi Hizmeti geçiş işlemi ve yerleşik nokta-in time geri yükleme hizmet katmanı güncelleştirmeyi veya varolan örneğin yeniden boyutlandırma sını engeller ve geri yükleme işlemi bitene kadar yeni örnekler oluşturur. Geri yükleme işlemi, geri yükleme işleminin çalıştığı aynı alt ağdaki Yönetilen örnekler ve örnek havuzlarındaki bu işlemleri engeller. Örnek havuzdaki örnekler etkilenmez. Hizmet katmanı oluşturma veya değiştirme işlemleri başarısız olmaz veya zaman acısı olmaz - geri yükleme işlemi tamamlandıktan veya iptal edildikten sonra devam ederler.

**Geçici Çözüm**: Geri yükleme işlemi bitene kadar bekleyin veya oluşturma veya hizmet katmanı işlemini güncelleştirmek daha yüksek önceliğe sahipse geri yükleme işlemini iptal edin.

### <a name="resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover"></a>İş Kritik hizmet katmanında Kaynak Yöneticisi'nin başarısız olduktan sonra yeniden yapılandırılması gerekebilir

Kullanıcı iş yüküne atanan kaynakları sınırlamanızı sağlayan [Kaynak Yöneticisi](/sql/relational-databases/resource-governor/resource-governor) özelliği, başarısız olduktan veya kullanıcı tarafından başlatılan hizmet katmanı değişikliğinden (örneğin, max vCore veya max instance depolama boyutunun değiştirilmesi) sonra bazı kullanıcı iş yükünü yanlış sınıflandırabilir.

**Geçici Çözüm** `ALTER RESOURCE GOVERNOR RECONFIGURE` : [Kaynak Yöneticisi](/sql/relational-databases/resource-governor/resource-governor)kullanıyorsanız, örnek başladığında SQL görevini yürüten SQL Agent Job'u düzenli olarak veya bir parçası olarak çalıştırın.

### <a name="cross-database-service-broker-dialogs-must-be-re-initialized-after-service-tier-upgrade"></a>Çapraz veritabanı Hizmet Aracısı iletişim kutuları, hizmet katmanı yükseltmeden sonra yeniden başlatılması gerekir

Veritabanı arası Hizmet Aracısı iletişim kutuları, hizmet katmanı işlemini değiştirdikten sonra iletileri diğer veritabanlarındaki hizmetlere teslim etmeyi durdurur. İletiler **kaybolmaz** ve gönderen kuyruğunda bulunabilir. Yönetilen Örnek'te vCores veya örnek depolama `service_broke_guid` boyutundaki herhangi bir değişiklik, [sys.databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql) görünümündeki değerin tüm veritabanları için değiştirilmesine neden olur. Diğer `DIALOG` veritabanındaki Servis Aracılarına başvuran [BEGIN DIALOG](/sql/t-sql/statements/begin-dialog-conversation-transact-sql) deyimi kullanılarak oluşturulan tüm bu iletiler hedef hizmete ileti göndermeyi durdurur.

**Geçici çözüm:** Hizmet katmanını güncelleştirmeden önce veritabanı arası Servis Aracısı iletişim konuşmalarını kullanan tüm etkinlikleri durdurun ve sonra yeniden başlatmayı. Hizmet katmanı değiştikten sonra teslim edilmeyen kalan iletiler varsa, iletileri kaynak kuyruktaki leri okuyun ve hedef sıraya yeniden gönderin.

### <a name="impersonification-of-azure-ad-login-types-is-not-supported"></a>Azure AD oturum açma türlerinin kişileştirilmesi desteklenmiyor

AAD ilkelerini kullanarak `EXECUTE AS USER` veya `EXECUTE AS LOGIN` aşağıdaki kimliğin kullanılması desteklenmez:
-   Diğer adlar aad kullanıcıları. Bu durumda `15517`aşağıdaki hata döndürülür.
- AAD uygulamaları veya hizmet ilkelerine dayalı AAD girişleri ve kullanıcıları. Bu durumda `15517` aşağıdaki hatalar döndürülür ve `15406`.

### <a name="query-parameter-not-supported-in-sp_send_db_mail"></a>@querysp_send_db_mail'da desteklenmeyen parametre

sp_send_db_mail `@query` yordamındaki [sp_send_db_mail](/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) parametre çalışmıyor.

### <a name="transactional-replication-must-be-reconfigured-after-geo-failover"></a>İşlemsel Çoğaltma, coğrafi hatadan sonra yeniden yapılandırılmalıdır

İşlem Çoğaltma otomatik hata grubunda bir veritabanında etkinleştirilirse, yönetilen örnek yöneticinin eski birincil deki tüm yayınları temizlemesi ve başka bir bölgeye bir hata oluştuktan sonra bunları yeni birincil üzerinde yeniden yapılandırması gerekir. Daha fazla ayrıntı için [Çoğaltma'ya](sql-database-managed-instance-transact-sql-information.md#replication) bakın.

### <a name="aad-logins-and-users-are-not-supported-in-ssdt"></a>AAD girişleri ve kullanıcılar SSDT'de desteklenmiyor

SQL Server Veri Araçları, Azure Active dizin girişlerini ve kullanıcılarını tam olarak desteklemez.

### <a name="temporary-database-is-used-during-restore-operation"></a>RESTORE işlemi sırasında geçici veritabanı kullanılır

Yönetilen Örnek'te bir veritabanı geri yüklendiğinde, geri yükleme hizmeti önce örnekteki adı ayırmak için istenen ada sahip boş bir veritabanı oluşturur. Bir süre sonra, bu veritabanı bırakılır ve gerçek veritabanı geri başlatılır. *Durum Geri Leştirme'de* bulunan veritabanı, ad yerine rasgele bir GUID değerine geçici olarak sahip olur. Geri yükleme işlemi tamamlandıktan sonra geçici `RESTORE` ad, ifadede belirtilen ada değiştirilir. İlk aşamada, kullanıcı boş veritabanına erişebilir ve hatta bu veritabanında tablolar oluşturabilir veya veri yükleyebilir. Geri yükleme hizmeti ikinci aşamayı başlattığında bu geçici veritabanı bırakılır.

**Geçici Çözüm**: Geri yüklemenin tamamlandığını görene kadar geri yüklediğiniz veritabanına erişme.

### <a name="tempdb-structure-and-content-is-re-created"></a>TEMPDB yapısı ve içeriği yeniden oluşturulur

Veritabanı `tempdb` her zaman 12 veri dosyasına bölünür ve dosya yapısı değiştirilemez. Dosya başına en büyük boyut değiştirilemez ve yeni dosyalar `tempdb`ait eklenemez. `Tempdb`örnek başladığında veya başarısız olduğunda her zaman boş bir veritabanı olarak `tempdb` yeniden oluşturulur ve yapılan değişiklikler korunamaz.

### <a name="exceeding-storage-space-with-small-database-files"></a>Küçük veritabanı dosyalarıyla depolama alanını aşma

`CREATE DATABASE`' `ALTER DATABASE ADD FILE`ve `RESTORE DATABASE` örneğin Azure Depolama sınırına ulaşabileceğinden ifadeler başarısız olabilir.

Her Genel Amaç yönetilen örneği, Azure Premium Disk alanı için ayrılmış en fazla 35 TB depolama alanına sahiptir. Her veritabanı dosyası ayrı bir fiziksel diske yerleştirilir. Disk boyutları 128 GB, 256 GB, 512 GB, 1 TB veya 4 TB olabilir. Diskteki kullanılmayan alan yüklenmemiş, ancak Azure Premium Disk boyutlarının toplamı 35 TB'yi geçemez. Bazı durumlarda, toplamda 8 TB'ye ihtiyaç duymayan yönetilen bir örnek, iç parçalanma nedeniyle depolama boyutundaki 35 TB Azure sınırını aşabilir.

Örneğin, Genel Amaçlı yönetilen bir örnek, 4-TB diske yerleştirilen 1,2 TB boyutunda büyük bir dosyaya sahip olabilir. Ayrıca, her biri ayrı 128 GB disklere yerleştirilen 1 GB boyutunda 248 dosya olabilir. Bu örnekte:

- Toplam ayrılan disk depolama boyutu 1 x 4 TB + 248 x 128 GB = 35 TB'dir.
- Örneğin veritabanları için ayrılan toplam alan 1 x 1,2 TB + 248 x 1 GB = 1,4 TB'dir.

Bu örnek, belirli koşullar altında, belirli bir dosya dağıtımı nedeniyle yönetilen bir örneğin, bunu beklemediğiniz zaman ekli bir Azure Premium Disk için ayrılmış olan 35-TB sınırına ulaşabileceğini göstermektedir.

Bu örnekte, varolan veritabanları çalışmaya devam eder ve yeni dosyalar eklenmedikçe herhangi bir sorun olmadan büyüyebilir. Tüm veritabanlarının toplam boyutu örnek boyutu sınırına ulaşmasa bile, yeni disk sürücüleri için yeterli alan olmadığından, yeni veritabanları oluşturulamaz veya geri yüklenebilir. Bu durumda döndürülen hata açık değil.

Sistem [görünümlerini](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) kullanarak kalan dosya sayısını belirleyebilirsiniz. Bu sınıra ulaşırsanız, [DBCC SHRINKFILE deyimini kullanarak küçük dosyaların bazılarını boşaltmayı ve silmeyi](/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) deneyin veya [bu sınıra sahip olmayan İş Kritik katmanına](/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics)geçin.

### <a name="guid-values-shown-instead-of-database-names"></a>Veritabanı adları yerine gösterilen GUID değerleri

Çeşitli sistem görünümleri, performans sayaçları, hata iletileri, XEvents ve hata günlüğü girişleri gerçek veritabanı adları yerine GUID veritabanı tanımlayıcıları görüntüler. Gelecekte gerçek veritabanı adlarıyla değiştirildikleri için bu GUID tanımlayıcılarına güvenmeyin.

**Geçici Çözüm**: GUID veritabanı tanımlayıcıları biçiminde belirtilen fiziksel veritabanı adından gerçek veritabanı adını çözmek için sys.databases görünümünü kullanın

```tsql
SELECT name as ActualDatabaseName, physical_database_name as GUIDDatabaseIdentifier 
FROM sys.databases
WHERE database_id > 4
```

### <a name="error-logs-arent-persisted"></a>Hata günlükleri kalıcı değil

Yönetilen örnekte kullanılabilen hata günlükleri kalıcı değildir ve boyutları maksimum depolama sınırına dahil değildir. Hata günlükleri, başarısız olursa otomatik olarak silinebilir. Yönetilen Örnek birkaç sanal makinede birkaç kez taşındığından, hata günlüğü geçmişinde boşluklar olabilir.

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>Aynı örnekteki iki veritabanındaki işlem kapsamı desteklenmiyor

.NET'teki `TransactionScope` sınıf, aynı işlem kapsamında aynı örnekteki iki veritabanına iki sorgu gönderilirse çalışmaz:

```csharp
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Bu kod aynı örnekteki verilerle çalışsa da, MSDTC gerekti.

**Geçici çözüm:** İki bağlantı kullanmak yerine bağlantı bağlamında başka bir veritabanı kullanmak için [SqlConnection.ChangeDatabase(String)](/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) kullanın.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>CLR modülleri ve bağlantılı sunucular bazen yerel bir IP adresine başvuru yapamaz

Yönetilen bir örneğe yerleştirilen CLR modülleri ve geçerli bir örne başvuran bağlı sunucular veya dağıtılmış sorgular bazen yerel bir örneğin IP'sini çözemez. Bu hata geçici bir sorundur.

**Geçici çözüm:** Mümkünse clr modülünde bağlam bağlantılarını kullanın.

## <a name="updates"></a>Güncelleştirmeler

SQL Veritabanı güncelleştirmeleri ve geliştirmeleri listesi için [SQL Veritabanı hizmet güncelleştirmelerine](https://azure.microsoft.com/updates/?product=sql-database)bakın.

Tüm Azure hizmetlerinde güncellemeler ve iyileştirmeler için [Hizmet güncelleştirmelerine](https://azure.microsoft.com/updates)bakın.

## <a name="contribute-to-content"></a>İçeriğe katkıda bulunma

Azure SQL Veritabanı belgelerine katkıda bulunmak için [Dokümanlar Katılımcı Kılavuzu'na](https://docs.microsoft.com/contribute/)bakın.
