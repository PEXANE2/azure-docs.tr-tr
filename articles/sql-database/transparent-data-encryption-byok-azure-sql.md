---
title: Müşteri tarafından yönetilen şeffaf veri şifreleme (TDE)
description: SQL Veritabanı ve Azure Synapse için Azure Key Vault ile Şeffaf Veri Şifreleme (TDE) için Kendi Anahtar (BYOK) desteğinizi getirin. BYOK genel bakış, yararları, nasıl çalıştığını, hususlar ve öneriler ile TDE.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-lt-2019, azure-synapse
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 03/18/2020
ms.openlocfilehash: 462326fb16663a6f25ff4b51ea11791201086fd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528737"
---
# <a name="azure-sql-transparent-data-encryption-with-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarla Azure SQL Şeffaf Veri Şifreleme

Azure SQL [Saydam Veri Şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) müşteri tarafından yönetilen anahtar ile, veri koruması için Kendi Anahtarınızı Getir (BYOK) senaryosunu sağlar ve kuruluşların anahtar ve veri yönetiminde görev ayrımını uygulamasına olanak tanır. Müşteri tarafından yönetilen saydam veri şifrelemesi ile müşteri, anahtar yaşam döngüsü yönetiminden (anahtar oluşturma, yükleme, döndürme, silme), anahtar kullanım izinlerinden ve anahtarlar üzerindeki işlemlerin denetlenmesinden ve tam denetiminden sorumludur.

Bu senaryoda, TDE koruyucusu olarak adlandırılan Veritabanı Şifreleme Anahtarının (DEK) şifrelemesi için kullanılan anahtar, bulut tabanlı harici anahtar yönetim sistemi olan müşteri tarafından yönetilen ve müşteri tarafından yönetilen [Azure Anahtar Kasası'nda (AKV)](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)depolanan müşteri tarafından yönetilen asimetrik bir anahtardır. Key Vault, isteğe bağlı olarak FIPS 140-2 Düzey 2 onaylı donanım güvenlik modülleri (HSM) tarafından desteklenen RSA şifreleme anahtarları için son derece kullanılabilir ve ölçeklenebilir güvenli depolama alanıdır. Depolanan bir anahtara doğrudan erişime izin vermez, ancak yetkili kuruluşlara anahtarı kullanarak şifreleme/şifre çözme hizmetleri sağlar. Anahtar, bir [ön-prem HSM aygıtından anahtar](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)kasası tarafından oluşturulabilir, içe aktarılabilir veya anahtar kasasına aktarılabilir.

Azure SQL Veritabanı ve Azure Synapse için, TDE koruyucusu mantıksal sunucu düzeyinde ayarlanır ve bu sunucuyla ilişkili tüm şifrelenmiş veritabanları tarafından devralınır. Azure SQL Yönetilen Örnek için, TDE koruyucusu örnek düzeyinde ayarlanır ve bu örnekteki tüm şifrelenmiş veritabanları tarafından devralır. *Sunucu* terimi, farklı belirtilmedikçe, bu belge boyunca hem SQL Veritabanı mantıksal sunucusuna hem de yönetilen örneğe başvurur.

> [!IMPORTANT]
> Müşteri tarafından yönetilen TDE'yi kullanmaya başlamak isteyenler için, geçiş işlemi sırasında veriler şifrelenmiş olarak kalır ve veritabanı dosyalarının ne kesinti süresi ne de yeniden şifrelemesi vardır. Hizmet tarafından yönetilen bir anahtardan müşteri tarafından yönetilen bir anahtara geçiş yapmak, yalnızca hızlı ve çevrimiçi bir işlem olan DEK'in yeniden şifresini gerektirir.

## <a name="benefits-of-the-customer-managed-tde"></a>Müşteri tarafından yönetilen TDE'nin faydaları

Müşteri tarafından yönetilen TDE, müşteriye aşağıdaki avantajları sağlar:

- TDE koruyucunun kullanımı ve yönetimi üzerinde tam ve parçalı kontrol;

- TDE koruyucu kullanımının şeffaflığı;

- Kuruluş içinde anahtar ve veri yönetiminde görev ayrımını uygulayabilme;

- Key Vault yöneticisi, şifrelenmiş veritabanını erişilemez hale getirmek için anahtar erişim izinlerini iptal edebilir;

- AKV tuşları merkezi yönetimi;

- AKV, Microsoft'un şifreleme anahtarlarını göremeyeceği veya ayıklayamayacağı şekilde tasarlandığından, son müşterilerinizden daha fazla güven;

## <a name="how-customer-managed-tde-works"></a>Müşteri tarafından yönetilen TDE nasıl çalışır?

![Müşteri tarafından yönetilen TDE'nin kurulumu ve işleyişi](./media/transparent-data-encryption-byok-azure-sql/customer-managed-tde-with-roles.PNG)

Sunucunun AKV'de DEPOLANAN TDE koruyucuyu DEK'nin şifrelemesi için kullanabilmesi için, anahtar kasa yöneticisinin benzersiz AAD kimliğini kullanarak sunucuya aşağıdaki erişim haklarını vermesi gerekir:

- **almak** - Key Vault anahtar kamu parçası ve özellikleri almak için

- **wrapKey** - (şifreleme) DEK korumak için

- **unwrapKey** - korumak (decrypt) DEK edebilmek için

Anahtar kasa yöneticisi, daha sonra denetlenebilmeleri için [önemli kasa denetim olaylarının günlüğe kaydedilmesini](https://docs.microsoft.com/azure/azure-monitor/insights/azure-key-vault)de sağlayabilir.

Sunucu AKV'den bir TDE koruyucusu kullanacak şekilde yapılandırıldığında, sunucu her TDE özellikli veritabanının DEK'ini şifreleme için anahtar kasasına gönderir. Anahtar kasası, daha sonra kullanıcı veritabanında depolanan şifreli DEK'i döndürür.

Gerektiğinde, sunucu şifre çözme için anahtar kasasına korumalı DEK gönderir.

Tetkikçiler, günlük kullanımı etkinse, önemli kasa AuditEvent günlüklerini incelemek için Azure Monitor'u kullanabilir.

[!INCLUDE [sql-database-akv-permission-delay](includes/sql-database-akv-permission-delay.md)]

## <a name="requirements-for-configuring-customer-managed-tde"></a>Müşteri tarafından yönetilen TDE yapılandırma gereksinimleri

### <a name="requirements-for-configuring-akv"></a>AKV yapılandırma gereksinimleri

- Anahtar kasası ve SQL Veritabanı/yönetilen örnek aynı Azure Active Directory kiracısına ait olmalıdır. Çapraz kiracı anahtar kasası ve sunucu etkileşimleri desteklenmez. Daha sonra kaynakları taşımak için, AKV ile TDE yeniden yapılandırılması gerekir. [Kaynakları taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)hakkında daha fazla bilgi edinin.

- [Veri](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) kaybına karşı korunmak için anahtar kasasında yumuşak silme özelliği etkinleştirilmelidir yanlışlıkla anahtar (veya anahtar kasası) silme işlemi gerçekleşir. Bu arada müşteri tarafından kurtarılmadığı veya tasfiye edildiği sürece, silinen kaynaklar 90 gün boyunca saklanır. *Kurtarma* ve *temizleme* eylemlerinin, önemli bir kasa erişim ilkesinde ilişkili kendi izinleri vardır. Yumuşak silme özelliği varsayılan olarak kapalıdır ve [PowerShell](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell#enabling-soft-delete) veya [CLI](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-cli#enabling-soft-delete)üzerinden etkinleştirilebilir. Azure portalı üzerinden etkinleştirilemez.  

- Azure Active Directory kimliğini kullanarak SQL Veritabanı sunucusuna veya yönetilen örnek erişimini anahtar kasasına (get, wrapKey, unwrapKey) ver. Azure portalı kullanırken Azure AD kimliği otomatik olarak oluşturulur. PowerShell veya CLI kullanırken Azure AD kimliğinin açıkça oluşturulması ve tamamlanmasının doğrulanması gerekir. PowerShell'i kullanırken ayrıntılı adım adım talimatlar için [BYOK ile TDE'yi BYOK ile yapılandırın](transparent-data-encryption-byok-azure-sql-configure.md) ve [TDE'yi Yapılandırın'ı](https://aka.ms/sqlmibyoktdepowershell) bkz.

- AKV ile güvenlik duvarı kullanırken, *güvenlik duvarını atlamak için güvenilen Microsoft hizmetlerine İzin Ver*seçeneğini etkinleştirmeniz gerekir.

### <a name="requirements-for-configuring-tde-protector"></a>TDE koruyucusu yapılandırma gereksinimleri

- TDE koruyucu sadece asimetrik, RSA 2048 veya RSA HSM 2048 tuşu olabilir.

- Anahtar etkinleştirme tarihi (eğer ayarlanmışsa) geçmişte bir tarih ve saat olmalıdır. Son kullanma tarihi (ayarlanmışsa) gelecekteki bir tarih ve saat olmalıdır.

- Anahtar *Etkin* durumda olmalıdır.

- Varolan anahtarı anahtar kasasına aktlıyorsanız, desteklenen dosya biçimlerinde (.pfx, .byok veya .yedekleme) sağladığından emin olun.

## <a name="recommendations-when-configuring-customer-managed-tde"></a>Müşteri tarafından yönetilen TDE'yi yapılandırırken öneriler

### <a name="recommendations-when-configuring-akv"></a>AKV yapılandırırken öneriler

- Sunucu, anahtar kasasındaki TDE koruyucuya eriştığında yüksek kullanılabilirlik sağlamak için tek bir abonelikteki anahtar kasasıyla toplamda en fazla 500 Genel Amaç veya 200 İş Kritik veritabanını ilişkilendirin. Bu rakamlar deneyime dayanmaktadır ve [anahtar kasa hizmet limitlerinde](https://docs.microsoft.com/azure/key-vault/key-vault-service-limits)belgelenmiştir. Burada amaç, sunucunun başarısız olması durumunda, bu sunucuda veritabanları olduğu kadar kasaya karşı birçok önemli işlemi tetikleyeceği için sorunları önlemektir.

- Bu kritik kaynağı kimin silecegini ve kazara veya yetkisiz silmeyi kimin engelleyecegini kontrol etmek için anahtar kasasına bir kaynak kilidi ayarlayın. [Kaynak kilitleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources)hakkında daha fazla bilgi edinin.

- Tüm şifreleme anahtarlarının denetimini ve raporlamasını etkinleştirin: Key vault, diğer güvenlik bilgilerine ve olay yönetimi araçlarına kolayca enjekte edilebilen günlükler sağlar. Operations Management Suite [Log Analytics,](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault) zaten entegre edilmiş bir hizmetin bir örneğidir.

- Her sunucuyu, farklı bölgelerde bulunan ve aynı anahtar malzemeyi tutan iki anahtar kasasıyla bağlayıp, şifreli veritabanlarının yüksek kullanılabilirliğini sağlayın. Yalnızca anahtarı tde koruyucusuyla aynı bölgedeki anahtar kasasından işaretleyin. Aynı bölgedeki anahtar kasasını etkileyen bir kesinti varsa sistem otomatik olarak uzak bölgedeki anahtar kasasına geçer.

### <a name="recommendations-when-configuring-tde-protector"></a>TDE koruyucusu yapılandırırken öneriler
- TDE koruyucunun bir kopyasını güvenli bir yerde saklayın veya emanet servisine emanet edin.

- Anahtar kasasında oluşturulursa, anahtarı ILK kez AKV'de kullanmadan önce bir anahtar yedeklemesi oluşturun. Yedekleme yalnızca Azure Anahtar Kasası'na geri yüklenebilir. [Backup-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey) komutu hakkında daha fazla bilgi edinin.

- Anahtarda herhangi bir değişiklik yapıldığında (örn. anahtar öznitelikleri, etiketler, ALA'lar) yeni bir yedekleme oluşturun.

- Anahtarları döndürürken anahtarın **önceki sürümlerini** anahtar kasasında tutun, böylece eski veritabanı yedeklemeleri geri yüklenebilir. Bir veritabanı için TDE koruyucusu değiştirildiğinde, veritabanının eski yedekleri en son TDE koruyucusu kullanacak şekilde **güncelleştirilmez.** Geri yükleme zamanında, her yedeklemenin oluşturma zamanında şifrelenmiş TDE koruyucuya ihtiyacı vardır. PowerShell kullanarak [Şeffaf Veri Şifreleme Koruyucusunu Döndür'deki](transparent-data-encryption-byok-azure-sql-key-rotation.md)talimatlara göre anahtar döndürmeler yapılabilir.

- Hizmet tarafından yönetilen anahtarlara geçtikten sonra bile daha önce kullandığınız tüm tuşları AKV'de tutun. Akv'de depolanan TDE koruyucuları ile veritabanı yedeklemelerinin geri yüklenmesini sağlar.  Azure Key Vault ile oluşturulan TDE koruyucuları, kalan tüm depolanmış yedeklemeler hizmet yönetilen anahtarlarla oluşturulana kadar muhafaza edilmelidir. [Backup-AzKeyVaultKey'i](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey)kullanarak bu anahtarların kurtarılabilir yedek kopyalarını yapın.

- Veri kaybı riski olmadan bir güvenlik olayı sırasında tehlikeye atabilecek bir anahtarı kaldırmak için, [olası tehlikeye atılabilme anahtarını kaldır'daki](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)adımları izleyin.

## <a name="inaccessible-tde-protector"></a>Erişilemeyen TDE koruyucusu

Saydam veri şifrelemesi müşteri tarafından yönetilen bir anahtar kullanacak şekilde yapılandırıldığında, veritabanının çevrimiçi kalması için TDE koruyucuya sürekli erişim gerekir. Sunucu AKV'deki müşteri tarafından yönetilen TDE koruyucuya erişimini kaybederse, 10 dakikaya kadar bir veritabanı ilgili hata iletisiyle tüm bağlantıları reddetmeye başlar ve durumunu *Erişilemez*olarak değiştirir. Erişilemeyen durumdaki bir veritabanında izin verilen tek eylem, veritabanını siler.

> [!NOTE]
> Aralıklı bir ağ kesintisi nedeniyle veritabanına erişilemiyorsa, herhangi bir işlem gerekmez ve veritabanları otomatik olarak çevrimiçi olarak geri gelir.

Anahtara erişim geri yüklendikten sonra, veritabanını yeniden çevrimiçi duruma getirmek için ek süre ve adımlar gerektirir ve bu süre, anahtara ve veritabanındaki verilerin boyutuna erişmeden geçen süreye bağlı olarak değişebilir:

- Anahtar erişimi 8 saat içinde geri yüklenirse, veritabanı önümüzdeki bir saat içinde otomatik olarak iyileşir.

- Anahtar erişimi 8 saatten fazla sonra geri yüklenirse, otomatik iyileşme mümkün değildir ve veritabanını geri getirmek portalda ek adımlar gerektirir ve veritabanının boyutuna bağlı olarak önemli miktarda zaman alabilir. Veritabanı yeniden çevrimiçi olduğunda, [failover grubu](https://docs.microsoft.com/azure/sql-database/sql-database-auto-failover-group) yapılandırması, zaman içinde geri yükleme geçmişi ve etiketler gibi önceden yapılandırılmış sunucu düzeyi ayarları **kaybolur.** Bu nedenle, 8 saat içinde temel anahtar erişim sorunlarını tanımlamanıza ve bunlara çözüm lamanıza olanak tanıyan bir bildirim sistemi uygulanması önerilir.

### <a name="accidental-tde-protector-access-revocation"></a>Kazara TDE koruyucu erişim iptali

Anahtar kasasına yeterli erişim hakkına sahip olan birinin sunucunun anahtara erişimini yanlışlıkla devre dışı kılabileceği olabilir:

- anahtar kasası *almak*iptal *, wrapKey*, sunucudan *unwrapKey* izinleri

- anahtarı silme

- anahtar kasasını silme

- anahtar kasanın güvenlik duvarı kurallarını değiştirme

- Azure Active Directory'de sunucunun yönetilen kimliğini silme

[Veritabanının erişilememiş olmasının yaygın nedenleri](https://docs.microsoft.com/sql/relational-databases/security/encryption/troubleshoot-tde?view=azuresqldb-current#common-errors-causing-databases-to-become-inaccessible)hakkında daha fazla bilgi edinin.

## <a name="monitoring-of-the-customer-managed-tde"></a>Müşteri tarafından yönetilen TDE'nin izlenmesi

Veritabanı durumunu izlemek ve TDE koruyucu erişiminin kaybedilmesi ne kadar uyarı olmasını sağlamak için aşağıdaki Azure özelliklerini yapılandırın:
- [Azure Kaynak Durumu](https://docs.microsoft.com/azure/service-health/resource-health-overview). TDE koruyucusuna erişimini kaybetmiş erişilemeyen bir veritabanı, veritabanına ilk bağlantı reddedildikten sonra "Kullanılamıyor" olarak gösterecektir.
- Müşteri tarafından yönetilen anahtar kasasındaki TDE koruyucuya erişim başarısız olduğunda [Etkinlik Günlüğü,](https://docs.microsoft.com/azure/service-health/alerts-activity-log-service-notifications) girişler etkinlik günlüğüne eklenir.  Bu olaylar için uyarılar oluşturmak, erişimi mümkün olan en kısa sürede yeniden başlatmanızı sağlar.
- E-posta/SMS/Push/Voice, Logic App, Webhook, ITSM veya Automation Runbook gibi tercihlerinize göre bildirim ve uyarı göndermek için [Eylem Grupları](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) tanımlanabilir.

## <a name="database-backup-and-restore-with-customer-managed-tde"></a>Müşteri tarafından yönetilen TDE ile veritabanı yedekleme ve geri yükleme

Bir veritabanı Key Vault'un anahtarı kullanılarak TDE ile şifrelendikten sonra, yeni oluşturulan yedeklemeler de aynı TDE koruyucusuyla şifrelenir. TDE koruyucusu değiştirildiğinde, veritabanının eski yedekleri en son TDE koruyucusu kullanmak üzere **güncelleştirilmez.**

Key Vault'tan TDE koruyucusuyla şifrelenmiş bir yedeklemeyi geri yüklemek için, anahtar malzemenin hedef sunucuda kullanılabilir olduğundan emin olun. Bu nedenle, veritabanı yedeklemelerinin geri yüklenilebilmeleri için TDE koruyucusu'nun tüm eski sürümlerini anahtar kasasında tutmanızı öneririz.

> [!IMPORTANT]
> Herhangi bir anda bir sunucu için ayarlanmış birden fazla TDE koruyucusu olamaz. Azure portal bıçağında "Anahtarı varsayılan TDE koruyucusu yapın" ile işaretlenmiş anahtardır. Ancak, birden çok ek anahtar, tde koruyucusu olarak işaretlemeden bir sunucuya bağlanabilir. Bu anahtarlar DEK'i korumak için kullanılmaz, ancak yedekleme dosyası ilgili parmak izi ile anahtarla şifrelenirse yedeklemeden geri yükleme sırasında kullanılabilir.

Yedeklemeyi geri yüklemek için gereken anahtar artık hedef sunucuda kullanılamıyorsa, geri yükleme denemesinde aşağıdaki hata `<Servername>` iletisi döndürülür: "Hedef \<sunucu, Timestamp \<#1> ve Timestamp #2> arasında oluşturulan tüm AKV URI'lerine erişmez. Lütfen tüm AKV URI'leri geri verdikten sonra tekrar çalışmayı deneyin."

Azaltmak için, kullanılabilir anahtarların listesini döndürmek ve eksik olanları tanımlamak için hedef SQL Veritabanı mantıksal sunucusu veya Hedef yönetilen örnek için [Get-AzSqlInstanceKeyKey](/powershell/module/az.sql/get-azsqlinstancekeyvaultkey) için [Get-AzSqlServerKeyKey](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) cmdlet çalıştırın. Tüm yedeklemelerin geri yüklenebilmesini sağlamak için, geri yükleme için hedef sunucunun gereken tüm anahtarlara erişebilmesini sağlayın. Bu anahtarların TDE koruyucusu olarak işaretlenebilmelerine gerek yoktur.

SQL Veritabanı için yedekleme kurtarma hakkında daha fazla bilgi edinmek için bir [Azure SQL veritabanını kurtar'a](sql-database-recovery-using-backups.md)bakın. SQL Pool için yedekleme kurtarma hakkında daha fazla bilgi edinmek için [bkz.](../synapse-analytics/sql-data-warehouse/backup-and-restore.md) SQL Server'ın yerel yedekleme/geri yükleme ile yönetilen örnek için Bkz. [Quickstart: Yönetilen Örnek veritabanını geri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore)

Günlük dosyaları için ek göz: Yedeklenen günlük dosyaları döndürülmüş olsa ve veritabanı artık yeni bir TDE koruyucusu kullanıyor olsa bile, orijinal TDE koruyucusu ile şifrelenir.  Geri yükleme zamanında, veritabanını geri yüklemek için her iki anahtar gerekir.  Günlük dosyası Azure Anahtar Kasası'nda depolanan bir TDE koruyucusu kullanıyorsa, veritabanı bu arada hizmet tarafından yönetilen TDE'yi kullanacak şekilde değiştirilmiş olsa bile, bu anahtar geri yükleme zamanında gerekli olacaktır.

## <a name="high-availability-with-customer-managed-tde"></a>Müşteri tarafından yönetilen TDE ile yüksek kullanılabilirlik

Sunucu için yapılandırılmış coğrafi artıklık olmadığı durumlarda bile, sunucunun aynı anahtar malzemeye sahip iki farklı bölgede iki farklı anahtar tonoz kullanacak şekilde yapılandırması önerilir. Sunucuyla aynı bölgede bulunan birincil anahtar kasasını kullanarak bir TDE koruyucusu oluşturarak ve anahtarı farklı bir Azure bölgesinde anahtar kasasına klonlayarak gerçekleştirilebilir, böylece sunucu birincil anahtar kasasına ikinci bir anahtar kasasına erişebilir veritabanı çalışır durumdayken bir kesinti yaşayın.

Anahtarı birincil anahtar kasasından şifreli biçimde almak için Backup-AzKeyVaultKey cmdlet'ini kullanın ve ardından Geri Yükleme-AzKeyVaultKey cmdlet'ini kullanın ve anahtarı klonlamak için ikinci bölgede bir anahtar kasası belirtin. Alternatif olarak, anahtarı yedeklemek ve geri yüklemek için Azure portalını kullanın. Diğer bölgedeki ikincil anahtar kasasındaki anahtar TDE koruyucusu olarak işaretlenmemeli ve buna izin bile verilmemelidir.

 Birincil anahtar kasasını etkileyen bir kesinti varsa ve ancak o zaman sistem varsa, ikincil anahtar kasasında aynı parmak izi ile otomatik olarak diğer bağlantılı anahtara geçer. İptal edilen erişim hakları nedeniyle TDE koruyucuya erişilemiyorsa veya müşterinin sunucunun anahtara erişimini kasıtlı olarak kısıtlamak istediğini gösterebileceğinden, anahtar veya anahtar kasası silindiği için bu anahtar alamayacaktır.

![Tek SunucuHA](./media/transparent-data-encryption-byok-azure-sql/customer-managed-tde-with-ha.png)

## <a name="geo-dr-and-customer-managed-tde"></a>Geo-DR ve müşteri tarafından yönetilen TDE

Hem [etkin coğrafi çoğaltma](https://docs.microsoft.com/azure/sql-database/sql-database-active-geo-replication) hem de [başarısız gruplar](https://docs.microsoft.com/azure/sql-database/sql-database-auto-failover-group) senaryolarında, ilgili her sunucu, aynı Azure bölgesindeki sunucuyla birlikte bulunması gereken ayrı bir anahtar kasası gerektirir. Müşteri, anahtar kasalar arasındaki anahtar malzemeyi tutarlı tutmaktan sorumludur, böylece jeo-ikincil eşitlenir ve birincil olarak bölgedeki bir kesinti nedeniyle erişilemez hale gelirse ve bir arıza tetiklenirse aynı anahtarı yerel anahtar kasasından kullanarak devralma yı devralabilir . En fazla dört saniye yapılandırılabilir ve zincirleme (ikinciler) desteklenmez.

Eksik anahtar malzeme nedeniyle coğrafi çoğaltma sırasında veya oluştururken sorunları önlemek için, müşteri tarafından yönetilen TDE yapılandırırken aşağıdaki kurallara uymak önemlidir:

- İlgili tüm önemli kasaların aynı özelliklere ve ilgili sunucular için aynı erişim haklarına sahip olması gerekir.

- İlgili tüm anahtar kasaları aynı anahtar malzemeyi içermelidir. Yalnızca geçerli TDE koruyucusu için değil, yedekleme dosyalarında kullanılabilecek önceki tüm TDE koruyucuları için de geçerlidir.

- TDE koruyucunun hem ilk kurulumu hem de rotasyonu önce ikincil, sonra birincil olarak yapılmalıdır.

![Failover grupları ve geo-dr](./media/transparent-data-encryption-byok-azure-sql/customer-managed-tde-with-bcdr.png)

Bir başarısızı test etmek için [Etkin coğrafi çoğaltma genel bakışındaki](sql-database-geo-replication-overview.md)adımları izleyin. Her iki anahtar kasasına SQL erişim izinlerinin onaylandığını doğrulamak için düzenli olarak yapılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, müşteri tarafından yönetilen TDE ile ortak işlemler için aşağıdaki PowerShell örnek komut dosyalarını denetlemek isteyebilirsiniz:

- [PowerShell'i kullanarak SQL Veritabanı için Şeffaf Veri Şifreleme Koruyucusunu Döndürün](transparent-data-encryption-byok-azure-sql-key-rotation.md)

- [PowerShell kullanarak SQL Veritabanı için Saydam Veri Şifreleme (TDE) koruyucusunu kaldırma](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-remove-tde-protector)

- [PowerShell'i kullanarak kendi anahtarınızla Yönetilen Örnekte Şeffaf Veri Şifrelemeyi Yönetin](https://docs.microsoft.com/azure/sql-database/scripts/transparent-data-encryption-byok-sql-managed-instance-powershell?toc=%2fpowershell%2fmodule%2ftoc.json)
