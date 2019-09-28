---
title: TDE Azure Key Vault tümleştirme veya Kendi Anahtarını Getir (BYOK)-Azure SQL veritabanı | Microsoft Docs
description: Kendi Anahtarını Getir (BYOK) SQL veritabanı ve veri ambarı için Azure Key Vault ile Saydam Veri Şifrelemesi (TDE) desteği. BYOK 'a genel bakış, avantajlar, nasıl çalıştığı, önemli noktalar ve öneriler ile TDE.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
ms.date: 07/18/2019
ms.openlocfilehash: 095ecc360e5639a5d47dff4bc4675fc237cf81da
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71348927"
---
# <a name="azure-sql-transparent-data-encryption-with-customer-managed-keys-in-azure-key-vault-bring-your-own-key-support"></a>Azure SQL Saydam Veri Şifrelemesi Azure Key Vault müşteri tarafından yönetilen anahtarlarla: Kendi Anahtarını Getir desteği

Azure Key Vault tümleştirmeyle [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) , veritabanı şifreleme anahtarını (dek) TDE koruyucusu adlı, müşteri tarafından yönetilen bir asimetrik anahtarla şifrelemesine olanak tanır. Bu Ayrıca, Saydam Veri Şifrelemesi için genellikle Kendi Anahtarını Getir (BYOK) desteği olarak adlandırılır.  BYOK senaryosunda, TDE koruyucusu, Azure 'un bulut tabanlı dış anahtar yönetim sistemi olan, müşterinin sahip olduğu ve yönetilen [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)depolanır. TDE koruyucusu, Anahtar Kasası tarafından [oluşturulabilir](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) veya şirket ıçı bir HSM cihazından anahtar kasasına [aktarılabilir](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) . Bir veritabanının önyükleme sayfasında depolanan TDE DEK, Azure Key Vault depolanan TDE koruyucusu tarafından şifrelenir ve şifresi çözülür.  SQL veritabanı 'nın, DEK şifre çözme ve şifreleme için müşterinin sahip olduğu Anahtar Kasası 'na izin verilmesi gerekir. Mantıksal SQL Server 'ın Anahtar Kasası için izinleri iptal edildiğinde, bir veritabanına erişilemeyecek, bağlantılar reddedilir ve tüm veriler şifrelenir. Azure SQL veritabanı için, TDE koruyucusu mantıksal SQL Server düzeyinde ayarlanır ve bu sunucuyla ilişkili tüm veritabanları tarafından devralınır. [Azure SQL yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-howto-managed-instance)IÇIN, TDE koruyucusu örnek düzeyinde ayarlanır ve bu örnekteki tüm *şifreli* veritabanları tarafından devralınır. *Sunucu* terimi, farklı belirtilmedikçe, bu belge boyunca hem sunucu hem de örneğe başvurur.

> [!NOTE]
> Azure SQL veritabanı yönetilen örneği için Azure Key Vault Tümleştirme (Kendi Anahtarını Getir) ile Saydam Veri Şifrelemesi önizleme aşamasındadır.


TDE Azure Key Vault tümleştirmeyle, kullanıcılar anahtar döndürmeler, Anahtar Kasası izinleri, anahtar yedeklemeleri dahil olmak üzere önemli yönetim görevlerini denetleyebilir ve Azure Key Vault işlevselliğini kullanarak tüm TDE koruyucuda denetim/raporlamayı etkinleştirebilir. Key Vault, merkezi anahtar yönetimi sağlar, sıkı izlenen donanım güvenlik modüllerini (HSM 'ler) kullanır ve güvenlik ilkeleriyle uyumluluğu karşılamak için anahtar ve veri yönetimi arasında görev ayrımı sağlar.  

Azure Key Vault tümleştirme ile TDE aşağıdaki avantajları sağlar:

- TDE koruyucusunu kendi kendine yönetme özelliği sayesinde daha fazla saydamlık ve ayrıntılı denetim
- Veritabanını işlemek için herhangi bir zamanda izinleri iptal etme özelliği
- Key Vault barındırarak TDE koruyucuların Merkezi Yönetimi (diğer Azure hizmetlerinde kullanılan diğer anahtarlar ve gizli diziler ile birlikte)
- Kuruluş içindeki anahtar ve veri yönetimi sorumluluklarını, görevlerin ayrılmasını desteklemek için ayırma
- Key Vault, Microsoft 'un herhangi bir şifreleme anahtarını görmemesi veya ayıklanmaması için tasarlandığından, kendi istemcilerinizden daha fazla güven.
- Anahtar döndürme desteği

> [!IMPORTANT]
> Key Vault kullanmaya başlamak isteyen hizmet tarafından yönetilen TDE kullanan kullanıcılar için, Key Vault içindeki bir TDE koruyucuya geçiş işlemi sırasında TDE etkin kalır. Veritabanı dosyalarının kapalı kalması veya yeniden şifrelenmesi yoktur. Hizmet tarafından yönetilen anahtardan Key Vault anahtara geçiş yapmak, hızlı ve çevrimiçi bir işlem olan veritabanı şifreleme anahtarı 'nın (DEK) yeniden şifrelenmesini gerektirir.

## <a name="how-does-tde-with-azure-key-vault-integration-support-work"></a>Azure Key Vault tümleştirme desteğiyle TDE nasıl çalışır?

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Resource Manager modülü Azure SQL veritabanı tarafından hala desteklenmektedir, ancak gelecekteki tüm geliştirmeler az. SQL modülüne yöneliktir. Bu cmdlet 'ler için bkz. [Azurerd. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az Module ve Azurerd modüllerinde komutların bağımsız değişkenleri önemli ölçüde aynıdır.

![Sunucunun Key Vault kimlik doğrulaması](./media/transparent-data-encryption-byok-azure-sql/tde-byok-server-authentication-flow.PNG)

TDE, Key Vault ' dan bir TDE koruyucusu kullanacak şekilde yapılandırıldığında, sunucu, her bir TDE etkin veritabanının i 'yi bir Wrap anahtar isteği için Key Vault olarak gönderir. Key Vault, Kullanıcı veritabanında depolanan şifrelenmiş veritabanı şifreleme anahtarını döndürür.  

> [!IMPORTANT]
> **BIR TDE koruyucusu Azure Key Vault depolandıktan sonra, Azure Key Vault hiçbir şekilde ayrılmadığını**unutmayın. Sunucu yalnızca Key Vault içindeki TDE koruyucu anahtar malzemesine anahtar işlem istekleri gönderebilir ve **TDE koruyucusunu hiçbir şekilde erişmez veya önbelleğe almaz**. Key Vault Yöneticisi herhangi bir noktada sunucunun Key Vault izinlerini iptal etme hakkına sahiptir ve bu durumda veritabanına yönelik tüm bağlantılar reddedilir.

## <a name="guidelines-for-configuring-tde-with-azure-key-vault"></a>Azure Key Vault ile TDE yapılandırma yönergeleri

### <a name="general-guidelines"></a>Genel Yönergeler

- Azure Key Vault ve Azure SQL veritabanı/yönetilen örneğinin aynı kiracıda yer aldığından emin olun.  Platformlar arası anahtar kasası ve sunucu etkileşimleri **desteklenmez**.
- Bir kiracı taşıma planlıyorsanız, AKV ile TDE yeniden yapılandırılması gerekir, [kaynakları taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)hakkında daha fazla bilgi edinin.
- TDE Azure Key Vault ile yapılandırırken, yük, yinelenen sarmalama/sarmalama işlemlerine göre anahtar kasasında yer alan yükü göz önünde bulundurmanız önemlidir. Örneğin, bir SQL veritabanı sunucusuyla ilişkili tüm veritabanları aynı TDE koruyucuyu kullandığından, sunucuda veritabanları olduğundan, bu sunucunun yük devretmesi kasada çok sayıda anahtar işlem tetikleyecektir. Deneyimimize ve belgelenen [Anahtar Kasası hizmeti sınırlarına](https://docs.microsoft.com/azure/key-vault/key-vault-service-limits)bağlı olarak, en çok 500 standart/genel amaçlı ya da 200 Premium/iş açısından kritik veritabanlarının tek bir abonelikte tek bir Azure Key Vault ilişkilendirilmesi önerilir ve tutarlı bir şekilde yüksek Kasadaki TDE koruyucuya erişirken kullanılabilirlik.
- Önerilen: Şirket içinde TDE koruyucunun bir kopyasını saklayın.  Bu, TDE koruyucu yerel olarak bir tde koruyucusu oluşturmak için bir HSM cihazının ve bir TDE 'nın yerel bir kopyasını depolayabileceği bir anahtar Emanet sistemi gerektirir.  [Yerel BIR HSM 'den Azure Key Vault bir anahtarı aktarmayı](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)öğrenin.


### <a name="guidelines-for-configuring-azure-key-vault"></a>Azure Key Vault yapılandırma yönergeleri

- Yanlışlıkla anahtar veya Anahtar Kasası – silme durumunda veri kaybından korunmak için [geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) ve Temizleme koruması etkinleştirilmiş bir Anahtar Kasası oluşturun.  Anahtar kasasındaki ["geçici silme" özelliğini etkinleştirmek Için PowerShell](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) kullanmanız gerekir (Bu seçenek şu an Için, Azure SQL tarafından gereklidir):  
  - Geçici olarak silinen kaynaklar, kurtarılmadıkları veya temizlenmedikleri takdirde ayarlanan süre, 90 gün boyunca tutulur.
  - **Kurtarma** ve **Temizleme** eylemlerinin bir Anahtar Kasası erişim ilkesiyle ilişkili kendi izinleri vardır.
- Bu kritik kaynağı kimlerin silebilen ve yanlışlıkla veya yetkisiz silme işleminin önlenmesine yardımcı olan anahtar kasasında bir kaynak kilidi ayarlayın.  [Kaynak kilitleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources)

- Azure Active Directory (Azure AD) kimliğini kullanarak SQL veritabanı sunucusuna anahtar kasasına erişim izni verin.  Portal Kullanıcı arabirimini kullanırken, Azure AD kimliği otomatik olarak oluşturulur ve sunucuya Anahtar Kasası erişim izinleri verilir.  BYOK ile TDE yapılandırmak için PowerShell 'i kullanarak Azure AD kimliğinin oluşturulması ve tamamlanmasının doğrulanması gerekir. PowerShell kullanırken ayrıntılı adım adım yönergeler için bkz. [bYok Ile TDE yapılandırma](transparent-data-encryption-byok-azure-sql-configure.md) ve [yönetilen örnek için bYok Ile TDE yapılandırma](https://aka.ms/sqlmibyoktdepowershell) .

   > [!NOTE]
   > Azure AD kimliği **yanlışlıkla silinirse veya sunucu izinleri** anahtar kasasının erişim ilkesi kullanılarak iptal edildiğinde veya sunucu farklı bir kiracıya taşınarak yanlışlıkla iptal edildiğinde, sunucu anahtar kasasına erişimi kaybeder ve şifreli veritabanları erişilemez olacak ve mantıksal sunucunun Azure AD kimliği ve izinleri geri yüklenene kadar oturum açma işlemleri reddedilir.  

- Azure Key Vault ile güvenlik duvarları ve sanal ağlar kullanılırken, güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına izin vermeniz gerekir. Evet ' i seçin.

   > [!NOTE]
   > TDE şifreli SQL veritabanları, güvenlik duvarını atlayamadığı için anahtar kasasına erişimi kaybederse, veritabanlarına erişilemez ve güvenlik duvarı atlama izinleri geri yüklenene kadar oturum açma işlemleri reddedilir.

- Tüm şifreleme anahtarlarında denetim ve raporlamayı etkinleştir: Key Vault diğer güvenlik bilgilerine ve olay yönetimi (SıEM) araçlarına eklemek kolay olan Günlükler sağlar. Operations Management Suite (OMS) [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault) , zaten tümleştirilmiş bir hizmetin bir örneğidir.
- Şifrelenmiş veritabanlarının yüksek oranda kullanılabilir olmasını sağlamak için her SQL veritabanı sunucusunu farklı bölgelerde bulunan iki Azure Anahtar Kasası ile yapılandırın.


### <a name="guidelines-for-configuring-the-tde-protector-asymmetric-key"></a>TDE koruyucuyu yapılandırma yönergeleri (asimetrik anahtar)

- Yerel bir HSM cihazında şifreleme anahtarınızı yerel olarak oluşturun. Bunun asimetrik, RSA 2048 veya RSA HSM 2048 anahtarı olduğundan emin olun, çünkü Azure Key Vault.
- Anahtarı bir anahtar Emanet sisteminde emanet.  
- Azure Key Vault için şifreleme anahtarı dosyasını (. pfx,. bYok veya. Backup) içeri aktarın.

   > [!NOTE]
   > Sınama amacıyla, Azure Key Vault bir anahtar oluşturulabilir, ancak özel anahtar anahtar kasasından hiçbir şekilde ayrılamadığından, bu anahtar esutlenemez.  Anahtar kaybı (anahtar kasasındaki yanlışlıkla silme, süre sonu vb.), kalıcı veri kaybına neden olacak şekilde, üretim verilerini şifrelemek için her zaman yedekleme ve emanet tuşlarını kullanır.

- Sona erme tarihi olan bir anahtar kullanırsanız, anahtarı süresi dolmadan önce döndürmek için bir süre sonu uyarı sistemi uygulayın: **anahtarın süresi dolduğunda, şifrelenen veritabanları TDE koruyucularına erişimi kaybeder ve erişilebilir** hale gelir ve tüm oturum açmalar anahtar yeni bir anahtara döndürüldü ve mantıksal SQL Server için yeni anahtar ve varsayılan TDE koruyucu olarak seçildi.
- Anahtarın etkinleştirildiğinden ve anahtar *Al*, sarmalama ve *sarmalama* 'yıkaldırma işlemleri gerçekleştirmek için izinlere sahip olduğundan emin olun.
- Anahtarı ilk kez Azure Key Vault kullanmadan önce Azure Key Vault anahtar yedeklemesi oluşturun. [Backup-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey) komutu hakkında daha fazla bilgi edinin.
- Anahtara her değişiklik yapıldığında yeni bir yedekleme oluşturun (örneğin, ACL 'Ler ekleyin, Etiketler ekleyin, anahtar öznitelikleri ekleyin).
- Anahtarları döndürürken anahtar kasasında anahtarın **önceki sürümlerini saklayın** , bu nedenle eski veritabanı yedeklemeleri geri yüklenebilir. Bir veritabanı için TDE koruyucusu değiştirildiğinde, veritabanının eski yedekleri en son TDE koruyucuyu kullanacak şekilde **güncellenmez** .  Her bir yedeklemenin, geri yükleme zamanında oluşturulduğu TDE koruyucusu vardır. Anahtar döndürmeler [, PowerShell kullanarak saydam veri şifrelemesi koruyucusunu döndürme](transparent-data-encryption-byok-azure-sql-key-rotation.md)bölümündeki yönergeleri izleyerek gerçekleştirilebilir.
- Hizmet tarafından yönetilen anahtarlara geri döndüğünüzde Azure Key Vault daha önce kullanılmış olan tüm anahtarları saklayın.  Bu, veritabanı yedeklerinin Azure Key Vault depolanan TDE koruyucularla geri yüklenmesini sağlar.  Azure Key Vault ile oluşturulan TDE koruyucuları, tüm depolanan yedeklemeler hizmet tarafından yönetilen anahtarlarla oluşturuluncaya kadar korunmalıdır.  
- [Backup-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey)kullanarak bu anahtarların kurtarılabilir yedek kopyalarını oluşturun.
- Bir güvenlik olayı sırasında, veri kaybı riski olmadan riskli olabilecek bir anahtarı kaldırmak için, riskli [olabilecek bir anahtarı kaldırma](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)bölümündeki adımları izleyin.

### <a name="guidelines-for-monitoring-the-tde-with-azure-key-vault-configuration"></a>Azure Key Vault yapılandırma ile TDE izleme yönergeleri

Mantıksal SQL Server, Azure Key Vault ' deki müşteri tarafından yönetilen TDE koruyucuya erişimi kaybederse, veritabanı tüm bağlantıları reddeder ve Azure portal erişilemez olarak görünür.  Bunun en yaygın nedenleri şunlardır:
- Anahtar Kasası yanlışlıkla bir güvenlik duvarının arkasına veya arkasına silindi
- Anahtar Kasası Anahtarı yanlışlıkla silindi, devre dışı veya zaman aşımına uğradı
- Mantıksal SQL Server örneği AppID yanlışlıkla silindi
- Mantıksal SQL Server örneği AppID 'Sine yönelik anahtara özel izinler iptal edildi

 > [!NOTE]
 > Müşteri tarafından yönetilen TDE koruyucusu erişimi 48 saat içinde geri yüklenirse, veritabanı kendinden otomatik olarak alınır ve otomatik olarak çevrimiçi hale gelir.  Zaman aralıklı bir ağ kesintisi nedeniyle veritabanına erişilemezse, hiçbir eylem gerekmez ve veritabanları otomatik olarak yeniden çevrimiçi olur.
  
- Mevcut yapılandırmaların sorunlarını giderme hakkında daha fazla bilgi için bkz. [TDE sorunlarını giderme](https://docs.microsoft.com/sql/relational-databases/security/encryption/troubleshoot-tde)

- Veritabanı durumunu izlemek ve TDE koruyucu erişimi kaybı nedeniyle uyarı etkinleştirmek için aşağıdaki Azure özelliklerini yapılandırın:
    - [Azure Kaynak durumu](https://docs.microsoft.com/azure/service-health/resource-health-overview). Veritabanına ilk bağlantı reddedildikten sonra TDE koruyucusu erişimi kayıp olan erişilemeyen bir veritabanı "kullanılamaz" olarak gösterilir.
    - Müşteri tarafından yönetilen anahtar kasasındaki TDE koruyucuya erişim başarısız olduğunda [etkinlik günlüğü](https://docs.microsoft.com/azure/service-health/alerts-activity-log-service-notifications) etkinlik günlüğüne eklenir.  Bu olaylar için uyarı oluşturulması mümkün olan en kısa sürede erişimi yeniden etkinleştirmenizi sağlar.
    - [Eylem grupları](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) , tercihlerinize, örn. e-posta/SMS/Push/Voice, Logic App, Web KANCASı, ISM veya Otomasyon Runbook 'una göre bildirim ve uyarı göndermek için tanımlanabilir.
    

## <a name="high-availability-geo-replication-and-backup--restore"></a>Yüksek kullanılabilirlik, coğrafi çoğaltma ve yedekleme/geri yükleme

### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Azure Key Vault ile yüksek kullanılabilirliği yapılandırma, veritabanınızın ve SQL veritabanı sunucunuzun yapılandırmasına bağlıdır ve iki ayrı durum için önerilen yapılandırmalar aşağıda verilmiştir.  İlk durumda, yapılandırılmış coğrafi artıklık olmayan tek başına bir veritabanı veya SQL veritabanı sunucusudur.  İkinci durumda, coğrafi Yük devretme işlemleri için, coğrafi olarak yedekli her kopyasının yük devretme grubu içinde yerel bir Azure Key Vault sahip olduğu bir veritabanı veya bir SQL veritabanı sunucusu veya coğrafi yedeklilik ile yapılandırılmış bir veritabanıdır.

İlk durumda, yapılandırılmış coğrafi yedeklilik olmadan bir veritabanının ve SQL veritabanı sunucusunun yüksek oranda kullanılabilir olmasını istiyorsanız, sunucuyu aynı anahtar malzemesine sahip iki farklı bölgede iki farklı Anahtar Kasası kullanacak şekilde yapılandırmak kesinlikle önerilir. Bu, SQL veritabanı sunucusuyla aynı bölgede birlikte bulunan birincil Key Vault ile bir TDE koruyucu oluşturularak ve anahtarı farklı bir Azure bölgesindeki bir anahtar kasasında kopyalayarak, sunucunun ikinci bir anahtar kasasının birincil Anahtar Kasası, veritabanı çalışmaya çalışırken bir kesinti yaşar. Anahtarı birincil anahtar kasasından şifrelenmiş biçimde almak için Backup-AzKeyVaultKey cmdlet 'ini kullanın ve ardından restore-AzKeyVaultKey cmdlet 'ini kullanın ve ikinci bölgede bir Anahtar Kasası belirtin.

![Tek sunuculu HA ve coğrafi olmayan-Dr](./media/transparent-data-encryption-byok-azure-sql/SingleServer_HA_Config.PNG)

## <a name="how-to-configure-geo-dr-with-azure-key-vault"></a>Azure Key Vault coğrafi DR 'yi yapılandırma

Şifrelenmiş veritabanları için TDE koruyucuların yüksek oranda kullanılabilirliğini sürdürmek için, var olan veya istenen SQL veritabanı yük devretme gruplarına veya etkin coğrafi çoğaltma örneklerine göre gereksiz Azure anahtar kasalarını yapılandırmak gerekir.  Her coğrafi çoğaltılan sunucu, aynı Azure bölgesindeki sunucu ile birlikte bulunması gereken ayrı bir Anahtar Kasası gerektirir. Birincil veritabanının bir bölgedeki kesinti nedeniyle erişilemez olması ve yük devretme tetiklenmesi gerekir, ikincil veritabanı ikincil anahtar kasasını kullanarak devredilebiliyor.

Coğrafi olarak çoğaltılan Azure SQL veritabanları için aşağıdaki Azure Key Vault yapılandırması gereklidir:

- Bölgede Anahtar Kasası olan bir birincil veritabanı ve bölgede Anahtar Kasası olan bir ikincil veritabanı.
- En az bir ikincil gereklidir, en fazla dört ikincil değer desteklenir.
- İkincilin ikincilleri (zincirleme) desteklenmez.

Aşağıdaki bölümde, kurulum ve yapılandırma adımlarının daha ayrıntılı bir şekilde ele alınacaktır.

### <a name="azure-key-vault-configuration-steps"></a>Azure Key Vault yapılandırma adımları

- [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps) yüklensin
- Anahtar kasalarında ["geçici silme" özelliğini etkinleştirmek Için PowerShell](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) 'i kullanarak iki farklı bölgede Iki Azure Anahtar Kasası oluşturun (Bu seçenek, AKTARıLAN ancak SQL için gereklidir).
- Anahtarların yedeklenmesi ve geri yüklenmesi için Azure anahtar kasalarının her ikisi de aynı Azure coğrafi konumunda bulunan iki bölgede bulunmalıdır.  SQL coğrafi-Dr gereksinimlerini karşılamak için iki anahtar kasalarının farklı bir coğrafyalar 'da konumlandırılabilmesi gerekiyorsa, anahtarların şirket içi bir HSM 'den içeri aktarılmasını sağlayan [bYok işlemini](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) izleyin.
- İlk anahtar kasasında yeni bir anahtar oluşturun:  
  - RSA/RSA-HSM 2048 anahtarı
  - Sona erme tarihi yok
  - Anahtar etkin ve anahtar al ve sarmalama kaldırma işlemleri gerçekleştirme izinlerine sahip
- Birincil anahtarı yedekleyin ve anahtarı ikinci anahtar kasasına geri yükleyin.  Bkz. [BackupAzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey) and [restore-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey).

### <a name="azure-sql-database-configuration-steps"></a>Azure SQL veritabanı yapılandırma adımları

Aşağıdaki yapılandırma adımları, yeni bir SQL dağıtımıyla başlayıp başlatılmayacağı veya zaten var olan bir SQL coğrafi-DR dağıtımıyla çalışılmasına göre farklılık gösterir.  Önce yeni bir dağıtım için yapılandırma adımlarını özetler ve ardından Azure Key Vault ' de depolanan TDE koruyucuların, zaten bir coğrafi DR bağlantısı kurulu olan mevcut bir dağıtıma nasıl atanacağını açıkladık.

**Yeni dağıtım adımları**:

- Daha önce oluşturulan anahtar kasalarıyla aynı iki bölgede iki SQL veritabanı sunucusu oluşturun.
- SQL veritabanı sunucusu TDE bölmesini seçin ve her SQL veritabanı sunucusu için:  
  - Aynı bölgedeki AKV 'yi seçin
  - TDE koruyucusu olarak kullanılacak anahtarı seçin – her bir sunucu, TDE koruyucunun yerel kopyasını kullanır.
  - Bunu portalda yapmak, SQL veritabanı sunucusu için anahtar kasasına erişmek üzere SQL veritabanı sunucusu izinlerini atamak için kullanılan bir [AppID](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) oluşturur – bu kimliği silmez. SQL veritabanı sunucusu yerine, anahtar kasasına erişmek için SQL veritabanı sunucusu izinlerini atamak için kullanılan Azure Key Vault izinleri kaldırılarak erişim iptal edilebilir.
- Birincil veritabanını oluşturun.
- [Etkin coğrafi çoğaltma kılavuzunu](sql-database-geo-replication-overview.md) izleyerek senaryoyu tamamlayın, bu adım ikincil veritabanını oluşturur.

![Yük devretme grupları ve coğrafi-Dr](./media/transparent-data-encryption-byok-azure-sql/Geo_DR_Config.PNG)

> [!NOTE]
> Veritabanları arasında coğrafi bağlantıyı kurmaya devam etmeden önce, her iki anahtar kasasında aynı TDE koruyucularının bulunduğundan emin olmanız önemlidir.

**COĞRAFI Dr dağıtımı ile mevcut BIR SQL DB Için adımlar**:

SQL veritabanı sunucuları zaten var ve birincil ve ikincil veritabanları zaten atanmış olduğundan, Azure Key Vault yapılandırma adımları aşağıdaki sırada gerçekleştirilmelidir:

- İkincil veritabanını barındıran SQL Database Server 'ı kullanmaya başlayın:
  - Aynı bölgede bulunan anahtar kasasını atama
  - TDE koruyucusunu atama
- Şimdi birincil veritabanını barındıran SQL veritabanı sunucusuna gidin:
  - İkincil VERITABANı için kullanılan aynı TDE koruyucuyu seçin

![Yük devretme grupları ve coğrafi-Dr](./media/transparent-data-encryption-byok-azure-sql/geo_DR_ex_config.PNG)

> [!NOTE]
> Anahtar kasasını sunucuya atarken, ikincil sunucu ile başlamak önemlidir.  İkinci adımda, birincil sunucuya anahtar kasasını atayın ve TDE koruyucuyu güncelleştirin, bu noktada çoğaltılan veritabanı tarafından kullanılan TDE koruyucusu her iki sunucu için de kullanılabilir.

Bir SQL veritabanı coğrafi-DR senaryosunda Azure Key Vault içindeki müşteri tarafından yönetilen anahtarlarla TDE 'yı etkinleştirmeden önce, SQL veritabanı coğrafi çoğaltma için kullanılacak bölgelerde aynı içerikle iki Azure Anahtar Kasası oluşturmanız ve korunması önemlidir.  "Özdeş içerikler" özellikle, her iki sunucunun da aynı TDE koruyucuların kopyalarını içermesi gerektiği anlamına gelir. böylece her iki sunucu da tüm veritabanları tarafından kullanılan TDE koruyucularına erişebilir.  Bundan sonra her iki anahtar kasasın de eşit tutulması gerekir, bu, anahtar rotasyondan sonra aynı TDE koruyucuların kopyalarını içermesi ve günlük dosyalarında veya yedeklemelerde kullanılan anahtarların eski sürümlerini saklamaları, TDE koruyucuların aynı anahtar özelliklerini ve anahtarı koruması gerekir. kasaların SQL için aynı erişim izinlerini koruması gerekir.  

[Etkin coğrafi Çoğaltmaya genel bakış](sql-database-geo-replication-overview.md) ' daki adımları izleyerek bir yük devretmeyi test edin ve tetikleyin. Bu, her iki anahtar KASASıNDA da SQL için erişim izinlerinin korunduğundan emin olmak için düzenli olarak yapılmalıdır.

### <a name="backup-and-restore"></a>Yedekleme ve Geri Yükleme

Bir veritabanı TDE Key Vault 'ten anahtar kullanılarak şifrelendikten sonra, oluşturulan tüm yedeklemeler de aynı TDE koruyucusu ile şifrelenir.

Key Vault bir TDE koruyucusu ile şifrelenmiş bir yedeklemeyi geri yüklemek için, anahtar malzemenin orijinal anahtar adı altındaki özgün kasada hala olduğundan emin olun. Bir veritabanı için TDE koruyucusu değiştirildiğinde, veritabanının eski yedekleri en son TDE koruyucuyu kullanacak **şekilde güncellenmez.** Bu nedenle, veritabanı yedeklerinin geri yüklenebilmesi için Key Vault 'de TDE koruyucunun tüm eski sürümlerini tutmanız önerilir.

Bir yedeklemenin geri yüklenmesi için gerekebilecek bir anahtar artık özgün anahtar kasasında yoksa, aşağıdaki hata iletisi döndürülür: "Hedef sunucunun `<Servername>` , \<zaman damgası #1 > ve \<zaman damgası #2 > tarafından oluşturulan tüm Akv URI 'lere erişimi yok. Lütfen tüm AKV URI 'Lerini geri yükledikten sonra işlemi yeniden deneyin. "

Bunu azaltmak için [Get-AzSqlServerKeyVaultKey](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) cmdlet 'ini çalıştırarak sunucuya eklenen Key Vault anahtarların listesini döndürün (bir kullanıcı tarafından silinmedikleri durumlar dışında). Tüm yedeklemelerin geri yüklenebildiğinden emin olmak için, yedeklemenin hedef sunucusunun bu anahtarların tamamına erişebildiğinden emin olun.

```powershell
Get-AzSqlServerKeyVaultKey `
  -ServerName <LogicalServerName> `
  -ResourceGroup <SQLDatabaseResourceGroupName>
```

SQL veritabanı için yedekleme kurtarması hakkında daha fazla bilgi edinmek için bkz. [Azure SQL veritabanını kurtarma](sql-database-recovery-using-backups.md). SQL veri ambarı için yedekleme kurtarması hakkında daha fazla bilgi edinmek için bkz. [Azure SQL veri ambarını kurtarma](../sql-data-warehouse/backup-and-restore.md).

Yedeklenen günlük dosyaları için ek dikkat: , TDE koruyucusu döndürülse ve veritabanı şimdi yeni bir TDE koruyucu kullanıyorsa bile, yedeklenen günlük dosyaları özgün TDE Şifreleyici ile şifreli olarak kalır.  Geri yükleme sırasında, veritabanını geri yüklemek için her iki anahtar de gerekecektir.  Günlük dosyası Azure Key Vault depolanan bir TDE koruyucu kullanıyorsa, veritabanı hizmet tarafından yönetilen TDE kullanacak şekilde değiştirilse bile, bu anahtar geri yükleme zamanında gerekecektir.
