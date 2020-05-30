---
title: Yenilikler
titleSuffix: Azure SQL Database & SQL Managed Instance
description: SQL yönetilen örnek hizmeti ve belgeleri & Azure SQL veritabanı için yeni özellikler ve geliştirmeler hakkında bilgi edinin
services: sql-database
author: stevestein
ms.service: sql-database
ms.subservice: service
ms.custom: sqldbrb=2
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/13/2020
ms.author: sstein
ms.openlocfilehash: dfd8051c7fc803d57a14cd740a830b8f6b63d9ac
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84195096"
---
# <a name="whats-new-in-azure-sql-database--sql-managed-instance"></a>SQL yönetilen örneği & Azure SQL veritabanı 'ndaki yenilikler nelerdir?

[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Bu makalede, şu anda genel önizleme aşamasında olan Azure SQL veritabanı ve Azure SQL yönetilen örnek özellikleri listelenmektedir. SQL veritabanı ve SQL yönetilen örnek güncelleştirmeleri ve geliştirmeleri için bkz. SQL [veritabanı & SQL yönetilen örnek hizmeti güncelleştirmeleri](https://azure.microsoft.com/updates/?product=sql-database). Diğer Azure hizmetlerine yönelik güncelleştirmeler ve geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates).

## <a name="whats-new"></a>Yenilikler

Azure SQL veritabanı ve Azure SQL yönetilen örneği belgeleri ayrı bölümlere bölündü. *Azure SQL veritabanı yönetilen örneği* Ile *Azure SQL yönetilen örneği*arasında yönetilen bir örneğe nasıl başvurduğumuz de güncelleştiriyoruz.

Bazı özellikler ve işlevler büyük ölçüde tek bir veritabanı ve yönetilen örnek arasında farklılık gösterdiğinden bu işlemi yaptık ve bireysel olarak ' paylaşılan ' makalelerde Azure SQL veritabanı ile Azure SQL yönetilen örneği arasındaki karmaşık nusların açıklanmasının daha fazla zor hale gelmesi.

Farklı Azure SQL ürünleri arasında bu açıklama, Azure SQL veritabanı 'nda tek bir yönetilen veritabanı, Azure SQL yönetilen örneği 'nde birden çok veritabanını barındıran tam olarak güvenilir bir yönetilen örnek veya Azure 'da bir sanal makinede barındırılan tanıdık SQL Server ürün gibi Azure 'da SQL Server Veritabanı altyapısıyla çalışma sürecini basitleştirmeye ve kolaylaştırmalıdır.

Bunun devam eden bir iş olduğunu ve her makale henüz güncelleştirilmemiş olduğunu göz önünde bulundurun. Örneğin Transact-SQL (T-SQL) deyimleri, saklı yordamlar ve Azure SQL veritabanı ile Azure SQL yönetilen örneği arasında paylaşılan birçok özellik henüz tamamlanmadığından, içeriği açıklığa kavuşturmaya devam ettiğimiz için size teşekkür ederiz. 

Bu tablo, terminoloji değişikliği için hızlı bir karşılaştırma sağlar: 


|**Yeni terim**  | **Önceki dönem**  |**Açıklama** |
|---------|---------|---------|
|**Azure SQL Yönetilen Örnek** | Azure SQL veritabanı *yönetilen örneği*| Azure SQL yönetilen örneği, Azure SQL veritabanı 'nda yalnızca bir dağıtım seçeneği yerine Azure SQL ailesi içinde kendi ürünüdür. | 
|**Azure SQL Veritabanı**|Azure SQL veritabanı *tek veritabanı*| Açıkça belirtilmediği takdirde, Azure SQL veritabanı ürün adı, elastik bir havuza dağıtılan tek veritabanlarını ve veritabanlarını içerir. |
|**Azure SQL Veritabanı**|Azure SQL veritabanı *elastik havuzu*| Açıkça belirtilmediği takdirde, Azure SQL veritabanı ürün adı, elastik bir havuza dağıtılan tek veritabanlarını ve veritabanlarını içerir.  |
|**Azure SQL Veritabanı** |Azure SQL Veritabanı | Terim aynı kalır, ancak artık tek veritabanı ve elastik havuz dağıtımları için geçerli olur ve yönetilen örnek içermez. |
| **Azure SQL**| Yok | Bu, Azure 'da kullanılabilen SQL Server veritabanı motoru ürünlerinin ailesini ifade eder: Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure VM 'lerinde SQL Server. | 

## <a name="features-in-public-preview"></a>Genel önizlemede Özellikler

### <a name="azure-sql-database"></a>[Azure SQL Veritabanı](#tab/single-database)

| Öne çıkan özelliği | Ayrıntılar |
| ---| --- |
| Yeni Fsv2 serisi ve d serisi donanım nesilleri| Bilgi için bkz. [donanım nesilleri](service-tiers-vcore.md#hardware-generations).|
| Tek veritabanları ve elastik havuzlarla hızlandırılmış veritabanı kurtarma | Bilgi için bkz. [hızlandırılmış veritabanı kurtarma](../accelerated-database-recovery.md).|
|Yaklaşık sayı farklı|Daha fazla bilgi için bkz. [yaklaşık Count DISTINCT](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#approximate-query-processing).|
|Rowstore 'da Batch modu (uyumluluk düzeyi 150 altında)|Bilgi için bkz. [rowstore 'Da Batch modu](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#batch-mode-on-rowstore).|
| Veri bulma ve sınıflandırma  |Bilgi için bkz. [Azure SQL veritabanı ve Azure SYNAPSE Analytics veri bulma & sınıflandırması](data-discovery-and-classification-overview.md).|
| Esnek veritabanı işleri | Bilgi için bkz. [elastik Işler oluşturma, yapılandırma ve yönetme](elastic-jobs-overview.md). |
| Esnek sorgular | Daha fazla bilgi için bkz. [elastik sorguya genel bakış](elastic-query-overview.md). |
| Elastik işlemler | [Bulut veritabanları arasında dağıtılmış işlemler](elastic-transactions-overview.md). |
|Bellek Izni geri bildirimi (satır modu) (uyumluluk düzeyi 150 altında)|Bilgi için bkz. [bellek verme geri bildirimi (satır modu)](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback).|
| Azure portal sorgu Düzenleyicisi |Bilgi için bkz. [Azure Portal SQL sorgu düzenleyicisini kullanarak bağlanma ve veri sorgulama](connect-query-portal.md).|
| Tek veritabanları ve elastik havuzlarla R Hizmetleri/makine öğrenimi |Bilgi için bkz. [Azure SQL veritabanı 'nda Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#machine-learning-services-in-azure-sql-database).|
|SQL Analytics|Bilgi için bkz. [Azure SQL Analytics](../../azure-monitor/insights/azure-sql.md).|
|Tablo değişkeni ertelenmiş derlemesi (uyumluluk düzeyi 150 altında)|Bilgi için bkz. [tablo değişkeni ertelenmiş derleme](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing#table-variable-deferred-compilation).|
| &nbsp; |

### <a name="azure-sql-managed-instance"></a>[Azure SQL Yönetilen Örnek](#tab/managed-instance)

| Öne çıkan özelliği | Ayrıntılar |
| ---| --- |
| <a href="/azure/sql-database/sql-database-instance-pools">Örnek havuzları</a> | Daha küçük SQL örneklerini buluta geçirmek için kullanışlı ve uygun maliyetli bir yol. |
| <a href="https://aka.ms/managed-instance-aadlogins">Örnek düzeyi Azure AD sunucu sorumluları (oturum açmalar)</a> | <a href="https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">Dış SAĞLAYıCıDAN oturum oluştur</a> ekstresini kullanarak örnek düzeyinde oturum açma oluşturun. |
| [İşlemsel çoğaltma](../managed-instance/replication-transactional-overview.md) | Tablolardaki değişiklikleri SQL yönetilen örnekler, SQL veritabanları veya SQL Server örneklerine yerleştirilmiş diğer veritabanlarına çoğaltın veya diğer SQL yönetilen örneklerde veya SQL Server örneğinde bazı satırlar değiştirildiğinde tablolarınızı güncelleştirin. Daha fazla bilgi için bkz. [Azure SQL yönetilen örneği 'nde çoğaltmayı yapılandırma](../managed-instance/replication-between-two-instances-configure-tutorial.md). |
| Tehdit algılama |Bilgi için bkz. [Azure SQL yönetilen örneği 'nde tehdit algılamayı yapılandırma](../managed-instance/threat-detection-configure.md).|
| Uzun süreli yedek saklama | Daha fazla bilgi için, şu anda sınırlı genel önizleme aşamasında olan [Azure SQL yönetilen örneği 'nde uzun süreli yedek saklama süresini yapılandırma](../managed-instance/long-term-backup-retention-configure.md)konusuna bakın. | 

---

## <a name="sql-managed-instance---new-features-and-known-issues"></a>SQL yönetilen örneği-yeni özellikler ve bilinen sorunlar

### <a name="sql-managed-instance-h2-2019-updates"></a>SQL yönetilen örnek H2 2019 güncelleştirmeleri

- [Hizmet destekli alt ağ yapılandırması](https://azure.microsoft.com/updates/service-aided-subnet-configuration-for-managed-instance-in-azure-sql-database-available/) SQL yönetilen örneği, yönetim trafiğinin kesintisiz akışını sağlarken, veri trafiğini denetlediğiniz alt ağ yapılandırmasını yönetmenin güvenli ve uygun bir yoludur
- [Kendi anahtarını getir (BYOK) Ile saydam veri şifrelemesi (TDE](https://azure.microsoft.com/updates/general-avilability-transparent-data-encryption-with-customer-managed-keys-for-azure-sql-database-managed-instance/) ), bekleyen veri koruması için kendi anahtarını getır (bYok) senaryosunu sağlar ve kuruluşların anahtarlar ve veriler için yönetim görevlerini ayırmasını sağlar.
- [Otomatik yük devretme grupları](https://azure.microsoft.com/updates/azure-sql-database-auto-failover-groups-feature-now-available-in-all-regions/) , birincil örnekten diğer bir bölgedeki ikincil örneğe tüm veritabanlarını çoğaltmanıza olanak sağlar.
- SQL yönetilen örnek davranışınızı [Genel izleme bayraklarıyla](https://azure.microsoft.com/updates/global-trace-flags-are-now-available-in-azure-sql-database-managed-instance/)yapılandırın.

### <a name="sql-managed-instance-h1-2019-updates"></a>SQL yönetilen örnek H1 2019 güncelleştirmeleri

Aşağıdaki özellikler, H1 2019 ' de SQL yönetilen örnek dağıtım modelinde etkinleştirilmiştir:
  - <a href="https://aka.ms/sql-mi-visual-studio-subscribers">Visual Studio aboneleri için aylık Azure kredisi</a> ve artan [bölge sınırları](../managed-instance/resource-limits.md#regional-resource-limitations)ile abonelikler için destek.
  - <a href="https://docs.microsoft.com/sharepoint/administration/deploy-azure-sql-managed-instance-with-sharepoint-servers-2016-2019"> SharePoint 2016 ve SharePoint 2019 </a> ile <a href="https://docs.microsoft.com/business-applications-release-notes/october18/dynamics365-business-central/support-for-azure-sql-database-managed-instance"> Dynamics 365 Business Central </a> için destek
  - <a href="https://aka.ms/managed-instance-collation">Örnek düzeyi harmanlama</a> ve tercih ettiğiniz <a href="https://azure.microsoft.com/updates/managed-instance-time-zone-ga/">zaman dilimi</a> ile SQL yönetilen örnekler oluşturun.
  - SQL yönetilen örnekler artık [yerleşik güvenlik duvarıyla](../managed-instance/management-endpoint-verify-built-in-firewall.md)korunuyor.
  - SQL yönetilen örnekleri, [genel uç noktaları](../managed-instance/public-endpoint-configure.md)kullanacak şekilde yapılandırın, <a href="https://aka.ms/four-cores-sql-mi-update">5. nesil donanım oluşturma konusunda</a> daha iyi ağ performansı elde etmek için [proxy geçersiz kılma](connectivity-architecture.md#connection-policy) bağlantısı yapın veya en son nokta geri yükleme için <a href="https://aka.ms/managed-instance-configurable-backup-retention">35 güne kadar yedekleme bekletmesini yapılandırın</a> . [Uzun süreli yedek saklama](long-term-retention-overview.md#managed-instance-support) (10 yıla kadar) Şu anda sınırlı genel önizleme aşamasındadır.  
  - Yeni işlevler, <a href="https://medium.com/@jocapc/geo-restore-your-databases-on-azure-sql-instances-1451480e90fa">PowerShell kullanarak veritabanınızı başka bir veri merkezine coğrafi olarak geri yükleme</a>, [veritabanını yeniden adlandırma](https://azure.microsoft.com/updates/azure-sql-database-managed-instance-database-rename-is-supported/), [sanal kümeyi silme](../managed-instance/virtual-cluster-delete.md)olanağı sağlar.
  - Yeni yerleşik [örnek katılımcısı rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-managed-instance-contributor) , güvenlik ilkelerine ve kurumsal standartlarla uyumluluğa sahip vergi (SOD) uyumluluğuna izin verebilir.
  - SQL yönetilen örneği, aşağıdaki Azure Kamu bölgelerinde GA (US Gov Teksas, US Gov Arizona) ve Çin Kuzey 2 ve Çin Doğu 2 ' ye kadar sunulmaktadır. Ayrıca, şu ortak bölgelerde de mevcuttur: Avustralya Orta, Avustralya Orta 2, Brezilya Güney, Fransa Güney, BAE Orta, BAE Kuzey, Güney Afrika Kuzey, Güney Afrika Batı.

### <a name="known-issues"></a>Bilinen sorunlar

|Sorun  |Keşfedilen Tarih  |Durum  |Çözümlenme tarihi  |
|---------|---------|---------|---------|
|[SAĞLAMA TOPLAMı olmadan el ile yedeklemenin geri yüklenmesi başarısız olabilir](#restoring-manual-backup-without-checksum-might-fail)|Mayıs 2020|Geçici çözüm vardır| |
|[Aracı, mevcut işleri değiştirme, devre dışı bırakma veya etkinleştirme sırasında yanıt vermemeye başladı](#agent-becomes-unresponsive-upon-modifying-disabling-or-enabling-existing-jobs)|Mayıs 2020|Otomatik olarak azaltıldığında| |
|[Kaynak grubundaki izinler SQL yönetilen örneği 'ne uygulanmadı](#permissions-on-resource-group-not-applied-to-sql-managed-instance)|Şub 2020|Geçici çözüm vardır||
|[Yük devretme grupları için Portal aracılığıyla el ile yük devretme sınırlaması](#limitation-of-manual-failover-via-portal-for-failover-groups)|Ocak 2020|Geçici çözüm vardır||
|[SQL Server Agent rolleri, sysadmin dışındaki oturum açma bilgileri için açık EXECUTE izinlerine ihtiyaç duyar](#in-memory-oltp-memory-limits-are-not-applied)|Dec 2019|Geçici çözüm vardır||
|[SQL Aracısı işleri, aracı işleminin yeniden başlatılmasına göre kesintiye uğrar](#sql-agent-jobs-can-be-interrupted-by-agent-process-restart)|Dec 2019|Çözümlendi|Mar 2020|
|[SSDT 'de AAD oturum açmaları ve kullanıcılar desteklenmez](#aad-logins-and-users-are-not-supported-in-ssdt)|Kas 2019|Geçici çözüm yok||
|[Bellek içi OLTP bellek sınırları uygulanmadı](#in-memory-oltp-memory-limits-are-not-applied)|Eki 2019|Geçici çözüm vardır||
|[Boş olmayan bir dosya kaldırılmaya çalışılırken hatalı hata döndürüldü](#wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty)|Eki 2019|Geçici çözüm vardır||
|[Hizmet katmanını değiştirme ve örnek oluşturma işlemleri sürekli veritabanı geri yükleme tarafından engelleniyor](#change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore)|Eyl 2019|Geçici çözüm vardır||
|[İş Açısından Kritik hizmet katmanındaki Resource Governor yük devretmeden sonra yeniden yapılandırılması gerekebilir](#resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover)|Eyl 2019|Geçici çözüm vardır||
|[Çapraz veritabanı Hizmet Aracısı iletişim kutuları, hizmet katmanı yükseltmesinden sonra yeniden başlatılmalıdır](#cross-database-service-broker-dialogs-must-be-reinitialized-after-service-tier-upgrade)|Ağu 2019|Geçici çözüm vardır||
|[Azure AD oturum açma türleri için ımpersonbirleşme desteklenmez](#impersonification-of-azure-ad-login-types-is-not-supported)|2019 Tem|Geçici çözüm yok||
|[@querysp_send_db_mail içinde parametre desteklenmiyor](#-parameter-not-supported-in-sp_send_db_mail)|2019 Nis|Geçici çözüm yok||
|[İşlemsel çoğaltmanın coğrafi Yük devretme sonrasında yeniden yapılandırılması gerekir](#transactional-replication-must-be-reconfigured-after-geo-failover)|Mar 2019|Geçici çözüm yok||
|[GERI yükleme işlemi sırasında geçici veritabanı kullanılıyor](#temporary-database-is-used-during-restore-operation)||Geçici çözüm vardır||
|[TEMPDB yapısı ve içerik yeniden oluşturuluyor](#tempdb-structure-and-content-is-re-created)||Geçici çözüm yok||
|[Küçük veritabanı dosyalarıyla depolama alanını aşma](#exceeding-storage-space-with-small-database-files)||Geçici çözüm vardır||
|[Veritabanı adları yerine gösterilen GUID değerleri](#guid-values-shown-instead-of-database-names)||Geçici çözüm vardır||
|[Hata günlükleri kalıcı değil](#error-logs-arent-persisted)||Geçici çözüm yok||
|[Aynı örnek içindeki iki veritabanında işlem kapsamı desteklenmiyor](#transaction-scope-on-two-databases-within-the-same-instance-isnt-supported)||Geçici çözüm vardır|Mar 2020|
|[CLR modülleri ve bağlı sunucular bazen yerel bir IP adresine başvuramaz](#clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address)||Geçici çözüm vardır||
|Veritabanı tutarlılığı, Azure Blob depolamadan geri yükleme sonrasında DBCC CHECKDB kullanılarak doğrulanmadı.||Çözümlendi|Kas 2019|
|Kaynak veritabanında bellek içi OLTP nesneleri varsa, İş Açısından Kritik katmanından Genel Amaçlı katmana geri yükleme işlemi başarılı olmaz.||Çözümlendi|Eki 2019|
|Güvenli bağlantı kullanarak harici (Azure dışı) posta sunucularıyla Veritabanı Postası özelliği||Çözümlendi|Eki 2019|
|Kapsanan veritabanları SQL yönetilen örneği 'nde desteklenmiyor||Çözümlendi|Ağu 2019|


### <a name="restoring-manual-backup-without-checksum-might-fail"></a>SAĞLAMA TOPLAMı olmadan el ile yedeklemenin geri yüklenmesi başarısız olabilir

Bazı durumlarda, yönetilen örnek için sağlama TOPLAMı olmadan gerçekleştirilen veritabanlarının el ile yedeklenmesi geri yüklenemeyebilir. Bu gibi durumlarda, lütfen başarılı olana kadar yedeklemeyi geri yüklemeyi yeniden deneyin.

**Geçici çözüm**: yönetilen örnek IÇIN sağlama toplamı etkin olan veritabanlarının el ile yedeklerini alın.

### <a name="agent-becomes-unresponsive-upon-modifying-disabling-or-enabling-existing-jobs"></a>Aracı, mevcut işleri değiştirme, devre dışı bırakma veya etkinleştirme sırasında yanıt vermemeye başladı

Belirli durumlarda, var olan bir işi değiştirme, devre dışı bırakma veya etkinleştirme aracının yanıt vermemesine neden olabilir. Sorun, aracı işleminin yeniden başlatılmasına neden olan algılama sonrasında otomatik olarak azaltıldığında.

### <a name="permissions-on-resource-group-not-applied-to-sql-managed-instance"></a>Kaynak grubundaki izinler SQL yönetilen örneği 'ne uygulanmadı

Bir kaynak grubuna (RG) uygulandığında SQL yönetilen örnek katılımcısı RBAC rolü SQL yönetilen örneğine uygulanmaz ve hiçbir etkiye sahip değildir.

**Geçici çözüm**: kullanıcılar için ABONELIK düzeyindeki SQL yönetilen örnek katılımcısı rolünü ayarlayın.

### <a name="limitation-of-manual-failover-via-portal-for-failover-groups"></a>Yük devretme grupları için Portal aracılığıyla el ile yük devretme sınırlaması

Yük devretme grubu farklı Azure aboneliklerindeki veya kaynak gruplarındaki örneklere yayılmışsa, yük devretme grubundaki birincil örnekten el ile yük devretme işlemi başlatılamaz.

**Geçici çözüm**: coğrafi ikincil örnekten Portal aracılığıyla yük devretmeyi başlatın.

### <a name="sql-agent-roles-need-explicit-execute-permissions-for-non-sysadmin-logins"></a>SQL Server Agent rolleri, sysadmin dışındaki oturum açma bilgileri için açık EXECUTE izinlerine ihtiyaç duyar

Sysadmin olmayan oturum açma işlemleri [SQL Agent sabit veritabanı rollerine](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent-fixed-database-roles)eklenirse, bu oturumların çalışması için ana saklı YORDAMLARA açık yürütme izinlerinin verilmesi gereken bir sorun vardır. Bu sorunla karşılaşılırsa, "nesne üzerinde yürütme izni reddedildi <object_name> (Microsoft SQL Server, hata: 229)" hata mesajını görürsünüz.

**Geçici çözüm**: SQL Aracısı sabit veritabanı rollerinden birine oturum açma işlemleri eklediğinizde: bu rollere eklenen her oturum açma için SQLAgentUserRole, SQLAgentReaderRole veya da SQLAgentOperatorRole rolünün, listelenen saklı YORDAMLARA açıkça yürütme izinleri vermek Için aşağıdaki T-SQL betiğini yürütür.

```tsql
USE [master]
GO
CREATE USER [login_name] FOR LOGIN [login_name]
GO
GRANT EXECUTE ON master.dbo.xp_sqlagent_enum_jobs TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_is_starting TO [login_name]
GRANT EXECUTE ON master.dbo.xp_sqlagent_notify TO [login_name]
```

### <a name="sql-agent-jobs-can-be-interrupted-by-agent-process-restart"></a>SQL Aracısı işleri, aracı işleminin yeniden başlatılmasına göre kesintiye uğrar

**(Mart 2020 ' de çözümlendi)** SQL Aracısı, her defasında iş her başlatıldığında yeni bir oturum oluşturur ve bellek tüketimini kademeli olarak artırır. Zamanlanan işlerin yürütülmesini engelleyebilen iç bellek sınırına ulaşmaktan kaçınmak için, bellek tüketimi eşiğe ulaştığında Aracı işlemi yeniden başlatılır. Yeniden başlatma sırasında çalışan işlerin yürütülmesini kesintiye neden olabilir.

### <a name="in-memory-oltp-memory-limits-are-not-applied"></a>Bellek içi OLTP bellek sınırları uygulanmadı

İş Açısından Kritik hizmet katmanı, bazı durumlarda [bellek için iyileştirilmiş nesneler için maksimum bellek sınırlarını](../managed-instance/resource-limits.md#in-memory-oltp-available-space) doğru bir şekilde uygulamacaktır. SQL yönetilen örneği, iş yükünün bellek Içi OLTP işlemleri için daha fazla bellek kullanmasını sağlayabilir ve bu da örneğin kullanılabilirliğini ve kararlılığını etkileyebilir. Sınırlara ulaşan bellek içi OLTP sorguları hemen başarısız olmayabilir. Bu sorun yakında düzeltilecektir. Daha fazla bellek Içi OLTP belleği kullanan sorgular [sınırlara](../managed-instance/resource-limits.md#in-memory-oltp-available-space)ulaştıklarında daha önce başarısız olur.

**Geçici çözüm:** iş yükünün kullanılabilir bellek kullandığından emin olmak için [SQL Server Management Studio](/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage#bkmk_Monitoring) kullanarak [bellek içi OLTP depolama kullanımını izleyin](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitor-space) . Sanal çekirdek sayısına bağlı olan bellek sınırlarını artırın veya daha az bellek kullanmak için iş yükünüzü iyileştirin.
 
### <a name="wrong-error-returned-while-trying-to-remove-a-file-that-is-not-empty"></a>Boş olmayan bir dosya kaldırılmaya çalışılırken hatalı hata döndürüldü

SQL Server/SQL yönetilen örneği [, kullanıcının boş olmayan bir dosyayı bırakmaya izin vermez](/sql/relational-databases/databases/delete-data-or-log-files-from-a-database#Prerequisites). İfadesini kullanarak boş olmayan bir veri dosyasını kaldırmaya çalışırsanız `ALTER DATABASE REMOVE FILE` , hata `Msg 5042 – The file '<file_name>' cannot be removed because it is not empty` hemen döndürülmeyecektir. SQL yönetilen örneği dosyayı bırakmaya çalışmaya devam eder ve 30 dakikadan sonra işlem başarısız olur `Internal server error` .

**Geçici çözüm**: komutunu kullanarak dosyanın içeriğini kaldırın `DBCC SHRINKFILE (N'<file_name>', EMPTYFILE)` . Dosya grubunda tek dosya varsa, dosyayı daralmadan önce bu dosya grubuyla ilişkili tablodan veya bölümden verileri silmeniz ve isteğe bağlı olarak bu verileri başka bir tabloya/bölüme yüklemeniz gerekir.

### <a name="change-service-tier-and-create-instance-operations-are-blocked-by-ongoing-database-restore"></a>Hizmet katmanını değiştirme ve örnek oluşturma işlemleri sürekli veritabanı geri yükleme tarafından engelleniyor

Devam eden `RESTORE` bildirim, veri geçiş hizmeti geçiş işlemi ve yerleşik nokta geri yükleme, hizmet katmanını güncelleştirmeyi veya mevcut örneği yeniden boyutlandırmayı engeller ve geri yükleme işlemi bitene kadar yeni örnekler oluşturur. Geri yükleme işlemi, geri yükleme işleminin çalıştırıldığı aynı alt ağdaki yönetilen örneklerde ve örnek havuzlardaki bu işlemleri engeller. Örnek havuzlardaki örnekler etkilenmez. Hizmet katmanı işlemleri oluşturma veya değiştirme başarısız olmayacak veya zaman aşımı-geri yükleme işlemi tamamlandıktan veya iptal edildikten sonra devam eder.

**Geçici çözüm**: geri yükleme işlemi tamamlanana kadar bekleyin veya hizmet katmanı oluşturma veya güncelleştirme işlemi daha yüksek önceliğe sahipse geri yükleme işlemini iptal edin.

### <a name="resource-governor-on-business-critical-service-tier-might-need-to-be-reconfigured-after-failover"></a>İş Açısından Kritik hizmet katmanındaki Resource Governor yük devretmeden sonra yeniden yapılandırılması gerekebilir

Kullanıcı iş yüküne atanan kaynakları sınırlandırmanızı sağlayan [Resource Governor](/sql/relational-databases/resource-governor/resource-governor) özellik, yük devretme veya Kullanıcı tarafından başlatılan hizmet katmanı değişikliği sonrasında bazı Kullanıcı iş yükünü yanlış sınıflandırabilir (örneğin, en büyük Vcore veya en büyük örnek depolama boyutu değişikliği).

**Geçici çözüm**: `ALTER RESOURCE GOVERNOR RECONFIGURE` [Resource Governor](/sql/relational-databases/resource-governor/resource-governor)kullanıyorsanız, örnek başladığında SQL aracısını yürüten SQL Aracısı işinin bir parçası olarak veya SQL Agent işinin bir parçası olarak çalıştırın.

### <a name="cross-database-service-broker-dialogs-must-be-reinitialized-after-service-tier-upgrade"></a>Çapraz veritabanı Hizmet Aracısı iletişim kutuları, hizmet katmanı yükseltmesinden sonra yeniden başlatılmalıdır

Çapraz veritabanı Hizmet Aracısı iletişim kutuları, hizmet katmanı işlemini değiştirdikten sonra iletileri diğer veritabanlarındaki hizmetlere teslim eder. İletiler **kaybolmaz** ve gönderici kuyruğunda bulunabilir. SQL yönetilen örneğindeki sanal çekirdekler veya örnek depolama boyutu değişiklikleri `service_broke_guid` [sys. databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql) görünümündeki değerin tüm veritabanları için değiştirilmesine neden olur. `DIALOG`Diğer veritabanındaki hizmet aracılarına başvuran [BEGIN BEGIN iletişim kutusu](/sql/t-sql/statements/begin-dialog-conversation-transact-sql) deyimleri, hedef hizmete Ileti teslim etme işlemini durdurur.

**Geçici çözüm:** Hizmet katmanını güncelleştirmeden önce veritabanları arası Hizmet Aracısı iletişim konuşmaları kullanan tüm etkinlikleri durdurun ve sonra yeniden başlatın. Hizmet katmanı değişikliğinden sonra teslim edilmemiş kalan iletiler varsa, kaynak kuyruktaki iletileri okuyun ve hedef sıraya yeniden gönderin.

### <a name="impersonification-of-azure-ad-login-types-is-not-supported"></a>Azure AD oturum açma türleri için ımpersonbirleşme desteklenmez

`EXECUTE AS USER`AŞAĞıDAKI AAD sorumluları kullanılarak veya kullanarak kimliğe bürünme `EXECUTE AS LOGIN` desteklenmez:
-    Diğer ad AAD kullanıcıları. Bu durumda aşağıdaki hata döndürülür `15517` .
- Aad uygulamaları ve hizmet sorumlularına göre AAD oturum açmaları ve kullanıcılar. Bu durumda ve aşağıdaki hatalar döndürülür `15517` `15406` .

### <a name="query-parameter-not-supported-in-sp_send_db_mail"></a>@querysp_send_db_mail içinde parametre desteklenmiyor

`@query` [Sp_send_db_mail](/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) yordamındaki parametresi çalışmıyor.

### <a name="transactional-replication-must-be-reconfigured-after-geo-failover"></a>İşlemsel çoğaltmanın coğrafi Yük devretme sonrasında yeniden yapılandırılması gerekir

Bir otomatik yük devretme grubundaki bir veritabanında Işlem çoğaltması etkinleştirilmişse, SQL yönetilen örnek Yöneticisi eski birincil üzerindeki tüm yayınları temizlemeli ve başka bir bölgeye yük devretme gerçekleştikten sonra yeni birincil üzerinde yeniden yapılandırmalıdır. Daha fazla ayrıntı için bkz. [çoğaltma](../managed-instance/transact-sql-tsql-differences-sql-server.md#replication) .

### <a name="aad-logins-and-users-are-not-supported-in-ssdt"></a>SSDT 'de AAD oturum açmaları ve kullanıcılar desteklenmez

SQL Server Veri Araçları Azure Active Directory oturumlarını ve kullanıcılarını tam olarak desteklemez.

### <a name="temporary-database-is-used-during-restore-operation"></a>GERI yükleme işlemi sırasında geçici veritabanı kullanılıyor

Bir veritabanı SQL yönetilen örneği 'ne geri yüklerken, geri yükleme hizmeti öncelikle adı örnek üzerinde ayırmak için istenen ada sahip boş bir veritabanı oluşturur. Bir süre sonra, bu veritabanı bırakılır ve gerçek veritabanının geri yüklenmesi başlatılır. *Geri yükleme* durumundaki veritabanı geçici olarak ad yerine rastgele bir GUID değeri olur. Geri yükleme işlemi tamamlandıktan sonra geçici ad, bildiriminde belirtilen istenen ada dönüştürülür `RESTORE` . İlk aşamada, Kullanıcı boş veritabanına erişebilir ve hatta tablo oluşturabilir veya bu veritabanında veri yükleyebilir. Bu geçici veritabanı, geri yükleme hizmeti ikinci aşamayı başlattığında bırakılacak.

**Geçici çözüm**: geri yükleme işleminin tamamlandığını görene kadar geri yüklemekte olduğunuz veritabanına erişmeyin.

### <a name="tempdb-structure-and-content-is-re-created"></a>TEMPDB yapısı ve içerik yeniden oluşturuluyor

`tempdb`Veritabanı her zaman 12 veri dosyasına bölünür ve dosya yapısı değiştirilemez. Dosya başına en büyük boyut değiştirilemez ve yeni dosyalar ' a eklenemez `tempdb` . `Tempdb`örnek başlatıldığında veya başarısız olduğunda her zaman boş bir veritabanı olarak yeniden oluşturulur ve ' de yapılan tüm değişiklikler `tempdb` korunmaz.

### <a name="exceeding-storage-space-with-small-database-files"></a>Küçük veritabanı dosyalarıyla depolama alanını aşma

`CREATE DATABASE``ALTER DATABASE ADD FILE` `RESTORE DATABASE` örnek, Azure depolama sınırına ulaşabileceğinden,, ve deyimleri başarısız olabilir.

Her Genel Amaçlı SQL yönetilen örneği, Azure Premium disk alanı için ayrılmış 35 TB 'a kadar depolama alanına sahiptir. Her veritabanı dosyası ayrı bir fiziksel diske yerleştirilir. Disk boyutları 128 GB, 256 GB, 512 GB, 1 TB veya 4 TB olabilir. Diskteki kullanılmayan alan ücretlendirilmez, ancak Azure Premium disk boyutlarının toplam toplamı 35 TB 'yi aşamaz. Bazı durumlarda, toplamda 8 TB 'lık bir SQL yönetilen örneği, iç parçalanma nedeniyle depolama boyutu 35 TB Azure sınırını aşabilir.

Örneğin, Genel Amaçlı bir SQL yönetilen örneği, 4 TB 'lık bir diske yerleştirilmiş boyutu 1,2 TB olan bir büyük dosyaya sahip olabilir. Ayrıca, her biri ayrı 128 GB disklere yerleştirilmiş 1 GB boyutundaki 248 dosyasına sahip olabilir. Bu örnekte:

- Ayrılan toplam disk depolama boyutu 1 x 4 TB + 248 x 128 GB = 35 TB 'tır.
- Örnekteki veritabanları için ayrılan toplam alan 1 x 1,2 TB + 248 x 1 GB = 1,4 TB 'tır.

Bu örnekte, belirli bir dosya dağıtımına bağlı olarak bir SQL yönetilen örneğinin, bağlı bir Azure Premium diski için ayrılan 35 TB sınırına ulaşamamış olabileceği gösterilmektedir.

Bu örnekte, mevcut veritabanları çalışmaya devam eder ve yeni dosyalar eklenmedikçe herhangi bir sorun olmadan büyüyebilir. Tüm veritabanlarının toplam boyutu örnek boyut sınırına ulaşmasa bile yeni disk sürücüleri için yeterli alan olmadığından yeni veritabanları oluşturulamaz veya geri yüklenemez. Bu durumda döndürülen hata net değildir.

[Kalan dosyaların sayısını](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) sistem görünümlerini kullanarak belirleyebilirsiniz. Bu sınıra ulaştıysanız, [DBCC SHRINKFILE ifadesini kullanarak daha küçük bir dosyayı boş ve silmeyi](/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) deneyin veya [Bu sınıra sahip olmayan iş açısından kritik katmanına](/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics)geçin.

### <a name="guid-values-shown-instead-of-database-names"></a>Veritabanı adları yerine gösterilen GUID değerleri

Çeşitli sistem görünümleri, performans sayaçları, hata iletileri, XEvents ve hata günlüğü girdileri, gerçek veritabanı adları yerine GUID veritabanı tanımlayıcılarını görüntüler. Gelecekte gerçek veritabanı adlarıyla değiştirildiklerinden, bu GUID tanımlayıcılarına güvenmeyin.

**Geçici çözüm**: GUID veritabanı tanımlayıcıları biçiminde belirtilen fiziksel veritabanı adından gerçek veritabanı adını çözümlemek için sys. databases görünümünü kullanın

```tsql
SELECT name as ActualDatabaseName, physical_database_name as GUIDDatabaseIdentifier 
FROM sys.databases
WHERE database_id > 4
```

### <a name="error-logs-arent-persisted"></a>Hata günlükleri kalıcı değil

SQL yönetilen örneği 'nde kullanılabilen hata günlükleri kalıcı değil ve boyutu en fazla depolama sınırına dahil değildir. Yük devretme gerçekleşirse hata günlükleri otomatik olarak silinebilir. SQL yönetilen örnek birkaç sanal makinede birkaç kez taşındığından, hata günlüğü geçmişinde boşluklar olabilir.

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>Aynı örnek içindeki iki veritabanında işlem kapsamı desteklenmiyor

**(Mart 2020 ' de çözümlendi)** `TransactionScope`Aynı işlem kapsamında aynı örnek içindeki iki veritabanına iki sorgu gönderilirse .net 'teki sınıfı çalışmaz:

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

**Geçici çözüm (mart 2020 ' den beri gerekmez):** İki bağlantı kullanmak yerine, başka bir veritabanını bir bağlantı bağlamında kullanmak için [SqlConnection. ChangeDatabase (dize)](/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) kullanın.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>CLR modülleri ve bağlı sunucular bazen yerel bir IP adresine başvuramaz

Bir SQL yönetilen örneği ve bağlı sunucular ya da geçerli örneğe başvuran dağıtılmış sorgulara yerleştirilmiş CLR modülleri bazen yerel bir örneğin IP 'sini çözümleyemeyebilir. Bu hata, geçici bir sorundur.

**Geçici çözüm:** Mümkünse, bir CLR modülünde bağlam bağlantılarını kullanın.

## <a name="updates"></a>Güncelleştirmeler

SQL veritabanı güncelleştirmelerinin ve geliştirmelerin bir listesi için bkz. [SQL Database Service Updates](https://azure.microsoft.com/updates/?product=sql-database).

Tüm Azure hizmetleri için güncelleştirmeler ve geliştirmeler için bkz. [hizmet güncelleştirmeleri](https://azure.microsoft.com/updates).

## <a name="contribute-to-content"></a>İçeriğe katkıda bulunma

Azure SQL belgelerine katkıda bulunmak için [docs katılımcısı Kılavuzu](https://docs.microsoft.com/contribute/)' na bakın.
