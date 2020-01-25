---
title: Azure SQL veritabanı için en iyi güvenlik uygulamaları PlayBook | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı 'nda en iyi güvenlik uygulamaları için genel rehberlik sunulmaktadır.
ms.service: sql-database
ms.subservice: security
author: VanMSFT
ms.author: vanto
ms.topic: article
ms.date: 12/23/2019
ms.reviewer: ''
ms.openlocfilehash: 82297850bf6d03215963a1f81dda166550f2b0d5
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76715185"
---
# <a name="azure-sql-database-security-best-practices-playbook"></a>Azure SQL veritabanı güvenlik en iyi uygulamaları PlayBook

## <a name="overview"></a>Genel Bakış

Bu belge, Azure SQL veritabanı 'nı kullanarak yeni veya mevcut uygulamalar için ortak güvenlik gereksinimlerinin nasıl çözülebileceğine ilişkin yönergeler sağlar. Üst düzey güvenlik alanlarıyla düzenlenmiştir. Belirli tehditleri adreslemek için [ortak güvenlik tehditleri ve olası azaltmaları](#common-security-threats-and-potential-mitigations) bölümüne bakın. Şirket içinden Azure 'a uygulama geçirirken sunulan önerilerin bazıları uygulanabilir olsa da, geçiş senaryoları bu belgenin odağı değildir.

### <a name="azure-sql-database-deployment-offers-covered-in-this-guide"></a>Bu kılavuzda ele alınan Azure SQL veritabanı dağıtımı teklifleri

- [SQL veritabanları](https://docs.microsoft.com/azure/sql-database/sql-database-single-index): [Azure SQL veritabanı sunucularında](sql-database-servers.md) [tek veritabanları](sql-database-single-database.md) ve [elastik havuzlar](sql-database-elastic-pool.md)
- [Yönetilen örnekler](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index)

### <a name="sql-deployment-offers-not-covered-in-this-guide"></a>Bu kılavuzda kapsanmayan SQL dağıtım teklifleri

- Azure SQL Veri Ambarı
- Azure SQL VM 'Leri (IaaS)
- Şirket içi SQL Server

### <a name="audience"></a>Hedef Kitle

Bu kılavuza yönelik hedef kitleler, müşterilerin Azure SQL veritabanı 'nı güvenli hale getirmeye yönelik sorulardır. Bu en iyi uygulama makalesinde ilgilendiğiniz roller şunları içerir, ancak bunlarla sınırlı değildir:

- Güvenlik mimarları
- Güvenlik yöneticileri
- Uyumluluk ofisleri
- Gizlilik ofisleri
- Güvenlik mühendisleri

### <a id="using"></a>Bu kılavuzu kullanma

Bu belge, mevcut [Azure SQL veritabanı güvenlik](sql-database-security-overview.md) belgeleriyle bir yardımcı olarak hazırlanmıştır.

Aksi belirtilmedikçe, ilgili hedefi veya gereksinimi elde etmek için her bölümde listelenen en iyi uygulamaları izlemeniz önerilir. Belirli güvenlik uyumluluk standartlarını veya en iyi uygulamaları karşılamak için, önemli mevzuata uyumluluk denetimleri, uygun yerlerde gereksinimler veya hedefler bölümünde listelenir. Bunlar, bu kağıda başvuruda bulunulan güvenlik standartları ve düzenlemelerdir:

- [Fedrampa](https://www.fedramp.gov/documents/): AC-04, AC-06
- [SOC](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/sorhome.html): cm-3, SDL-3
- [ISO/ıec 27001](https://www.iso27001security.com/html/27001.html): Access Control, şifreleme
- [Microsoft operasyonel güvenlik güvencesi (OSA) uygulamaları](https://www.microsoft.com/en-us/securityengineering/osa/practices): uygulama #1-6 ve #9
- [NIST özel yayını 800-53 güvenlik denetimleri](https://nvd.nist.gov/800-53): AC-5, AC-6
- [PCI DSS](https://www.pcisecuritystandards.org/document_library): 6.3.2, 6.4.2

### <a name="feedback"></a>Geri Bildirim

Burada listelenen öneriler ve en iyi uygulamalar güncelleştirilmeye devam ediyor. Bu makalenin altındaki **geri bildirim** bağlantısını kullanarak bu belge için giriş veya herhangi bir düzeltme sağlayın.

## <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulama, kullanıcının talep ettikleri kim olduğunu kanıtlama işlemidir. Azure SQL veritabanı iki tür kimlik doğrulamasını destekler:

- SQL kimlik doğrulaması
- Azure Active Directory kimlik doğrulaması

### <a name="central-management-for-identities"></a>Kimlikler için merkezi yönetim

Merkezi kimlik yönetimi aşağıdaki avantajları sunar:

- Azure SQL veritabanı sunucuları ve veritabanları arasında oturum açmaları olmadan grup hesaplarını yönetin ve Kullanıcı izinlerini denetleyin.
- Basitleştirilmiş ve esnek izin yönetimi.
- Ölçekteki uygulamaların yönetimi.

**Nasıl uygulanır**:

- Merkezi kimlik yönetimi için Azure Active Directory (Azure AD) kimlik doğrulaması kullanın.

**En iyi uygulamalar**:

- Azure AD kiracısı oluşturun ve insan kullanıcılarını temsil eden [kullanıcılar oluşturun](../active-directory/fundamentals/add-users-azure-active-directory.md) ve uygulamaları, hizmetleri ve otomasyon araçlarını temsil etmek için [hizmet sorumluları](../active-directory/develop/app-objects-and-service-principals.md) oluşturun. Hizmet sorumluları, Windows ve Linux 'ta hizmet hesaplarıyla eşdeğerdir. 

- Kaynaklara erişim haklarını Grup ataması aracılığıyla Azure AD sorumlularına atama: Azure AD grupları oluşturun, gruplara erişim izni verin ve gruplara ayrı ayrı Üyeler ekleyin. Veritabanınızda, Azure AD gruplarınızı eşleyen, kapsanan veritabanı kullanıcıları oluşturun. 
  - SQL ile [Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](sql-database-aad-authentication-configure.md) ve [SQL ile kimlik doğrulaması Için Azure ad kullanma](sql-database-aad-authentication.md)makalelerini inceleyin.
  > [!NOTE]
  > Yönetilen bir örnekte, ana veritabanında Azure AD sorumluları ile eşlenen oturum açma işlemleri de oluşturabilirsiniz. Bkz. [oturum oluşturma (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current).

- Azure AD gruplarının kullanılması, izin yönetimini ve her iki grup sahibini de basitleştirir ve kaynak sahibi gruba/gruptan üye ekleyip kaldırabilir. 

- SQL DB sunucuları için Azure AD yöneticisi için ayrı bir grup oluşturun.

  - [Azure SQL veritabanı sunucunuz için Azure Active Directory Yöneticisi sağlama](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)makalesine bakın.

- Azure AD denetim etkinlik raporlarını kullanarak Azure AD grup üyeliği değişikliklerini izleyin. 

- Yönetilen bir örnek için, Azure AD Yöneticisi oluşturmak için ayrı bir adım gerekir. 
  - [Yönetilen örneğiniz için Azure Active Directory Yöneticisi sağlama](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance)makalesine bakın. 

> [!NOTE]
> - Azure AD kimlik doğrulaması Azure SQL denetim günlüklerine kaydedilir, ancak Azure AD oturum açma günlüklerinde değildir.
> - Azure 'da verilen RBAC izinleri Azure SQL DB izinleri için uygulanmıyor. Mevcut SQL izinleri kullanılarak SQL DB 'de bu tür izinler el ile oluşturulmalıdır/eşlenmelidir.
> - İstemci tarafı Azure AD kimlik doğrulamasında Internet 'e veya bir sanal ağa Kullanıcı tanımlı yol (UDR) aracılığıyla erişim gerekir.
> - Azure AD erişim belirteci, istemci tarafında önbelleğe alınır ve süresi, belirteç yapılandırmasına bağlıdır. [Azure Active Directory yapılandırılabilir belirteç yaşam süreleri](../active-directory/develop/active-directory-configurable-token-lifetimes.md) makalesine bakın

### <a name="multi-factor-authentication-mfa"></a>Multi-Factor Authentication (MFA)

> [!NOTE]
> Bahsedilen: OSA uygulaması #2, ISO Access Control (AC)

Azure Multi-Factor Authentication (MFA), birden fazla kimlik doğrulama biçimi gerektirerek ek güvenlik sağlanmasına yardımcı olur.

**Nasıl uygulanır**:

- Koşullu erişim kullanarak Azure AD 'de [MFA 'Yı etkinleştirin](../active-directory/authentication/concept-mfa-howitworks.md) ve etkileşimli kimlik doğrulaması kullanın. 

- Alternatif, tüm Azure AD veya AD etki alanı için MFA 'yı etkinleştirmektir.

**En iyi uygulamalar**:

- Azure AD 'de koşullu erişimi etkinleştirin (Premium aboneliği gerektirir). 
  - [Azure AD 'de koşullu erişim](../active-directory/conditional-access/overview.md)makalesine bakın.  

- Azure AD grupları oluşturun ve Azure AD koşullu erişim kullanarak seçilen gruplar için MFA ilkesini etkinleştirin. 
  - [Koşullu erişim dağıtımını planlayın](../active-directory/conditional-access/plan-conditional-access.md)başlıklı makaleye bakın. 

- MFA, Azure AD 'nin tamamı için veya Azure AD ile Federal tüm Active Directory için etkinleştirilebilir. 

- Etkileşimli olarak bir parolanın istendiği SQL DB için Azure AD etkileşimli kimlik doğrulaması modunu, ardından MFA kimlik doğrulamasını kullanın:      
  - SSMS 'de evrensel kimlik doğrulaması kullanın. [Azure SQL veritabanı Ile Multi-Factor AAD kimlik doğrulamasını ve Azure SQL veri ambarı 'nı (MFA IÇIN SSMS desteği) kullanarak](sql-database-ssms-mfa-authentication.md)makaleye bakın.
  - SQL Server Veri Araçları (SSDT) içinde desteklenen etkileşimli kimlik doğrulaması kullanın. [SQL Server veri araçları (SSDT) Azure Active Directory destek](https://docs.microsoft.com/sql/ssdt/azure-active-directory?view=azuresqldb-current)makalesine bakın.
  - MFA destekleyen diğer SQL araçlarını kullanın. 
    - Dışarı aktarma/ayıklama/dağıtım veritabanı için SSMS Sihirbazı desteği  
    - [SqlPackage. exe](https://docs.microsoft.com/sql/tools/sqlpackage): '/uA ' seçeneği 
    - [sqlcmd yardımcı programı](https://docs.microsoft.com/sql/tools/sqlcmd-utility): Option-G (etkileşimli)
    - [bcp yardımcı programı](https://docs.microsoft.com/sql/tools/bcp-utility): Option-G (etkileşimli) 

- MFA desteğiyle etkileşimli kimlik doğrulaması kullanarak Azure SQL veritabanı 'na bağlanmak için uygulamalarınızı uygulayın. 
  - Azure [Multi-Factor Authentication Azure SQL veritabanı 'Na bağlanma](active-directory-interactive-connect-azure-sql-db.md)makalesine bakın. 
  > [!NOTE]
  > Bu kimlik doğrulama modu, Kullanıcı tabanlı kimlikler gerektirir. Tek tek Azure AD Kullanıcı kimlik doğrulamasını (ör. Azure kaynakları için yönetilen kimlik kullanarak) atlayarak güvenilir bir kimlik modelinin kullanıldığı durumlarda MFA uygulanmaz.

### <a name="minimize-the-use-of-password-based-authentication-for-users"></a>Kullanıcılar için parola tabanlı kimlik doğrulaması kullanımını en aza indirme

> [!NOTE]
> Bahsedilen: OSA uygulaması #4, ISO Access Control (AC)

Parola tabanlı kimlik doğrulama yöntemleri, daha zayıf bir kimlik doğrulama biçimidir. Kimlik bilgileri tehlikeye girebilir veya yanlışlıkla verilebilir.

**Nasıl uygulanır**:

- Parolaların kullanımını ortadan kaldıran bir Azure AD Tümleşik kimlik doğrulaması kullanın.

**En iyi uygulamalar**:

- Windows kimlik bilgilerini kullanarak çoklu oturum açma kimlik doğrulaması kullanın. Şirket içi AD etki alanını Azure AD ile Federasyonun ve tümleşik Windows kimlik doğrulaması kullanın (Azure AD ile etki alanına katılmış makineler için).
  - [Azure AD Tümleşik kimlik doğrulaması Için SSMS desteği](sql-database-aad-authentication-configure.md#active-directory-integrated-authentication)makalesine bakın.

### <a name="minimize-the-use-of-password-based-authentication-for-applications"></a>Uygulamalar için parola tabanlı kimlik doğrulama kullanımını en aza indirme 

> [!NOTE]
> Bahsedilen: OSA uygulaması #4, ISO Access Control (AC)

**Nasıl uygulanır**:

- Azure yönetilen kimliğini etkinleştirin. Tümleşik veya sertifika tabanlı kimlik doğrulaması da kullanabilirsiniz. 

**En iyi uygulamalar**:

- [Azure kaynakları için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/overview.md)kullanın.
  - [Sistem tarafından atanan yönetilen kimlik](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql.md) 
  - [Kullanıcı tarafından atanan yönetilen kimlik](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
  - [Yönetilen kimliğe sahip App Service 'ten Azure SQL veritabanı 'nı kullanma (kod değişikliği olmadan)](https://github.com/Azure-Samples/app-service-msi-entityframework-dotnet)

- Bir uygulama için sertifika tabanlı kimlik doğrulaması kullanın. 
  - Bu [kod örneğine](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/token)bakın. 

- Tümleşik Federasyon etki alanı ve etki alanına katılmış makine için Azure AD kimlik doğrulamasını kullanın (yukarıdaki bölüme bakın).
  - [Tümleşik kimlik doğrulaması için örnek uygulamaya](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/integrated)bakın.

### <a name="protect-passwords-and-secrets"></a>Parolaları ve gizli dizileri koruma

Parolaların kaçınılmaz olduğu durumlarda, bunların güvenli olduklarından emin olun.

**Nasıl uygulanır**:

- Parolaları ve gizli dizileri depolamak için Azure Key Vault kullanın. Uygun olduğunda, Azure AD kullanıcıları ile Azure SQL veritabanı için MFA 'yı kullanın.

**En iyi uygulamalar**:

- Parola veya gizli dizileri önleme mümkün değilse, Kullanıcı parolalarını ve uygulama gizli dizilerini Azure Key Vault depolayın ve Key Vault erişim ilkeleri aracılığıyla erişimi yönetin. 

- Çeşitli uygulama geliştirme çerçeveleri, uygulamadaki gizli dizileri korumak için çerçeveye özgü mekanizmalar de sunabilir. Örneğin: [ASP.NET Core uygulaması](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows).

### <a name="use-sql-authentication-for-legacy-applications"></a>Eski uygulamalar için SQL kimlik doğrulaması kullan 

SQL kimlik doğrulaması, Kullanıcı adı ve parola kullanarak Azure SQL veritabanı 'na bağlanırken bir kullanıcının kimlik doğrulamasını ifade eder. Her SQL veritabanı sunucusunda veya yönetilen bir örnekte ve her bir veritabanında oluşturulan bir kullanıcı için bir oturum açma gerekir.

**Nasıl uygulanır**:

- SQL kimlik doğrulaması kullanın.

**En iyi uygulamalar**:

- Sunucu Yöneticisi olarak, oturum açma bilgileri ve kullanıcılar oluşturun. İçerilen veritabanı kullanıcıları parolalarla birlikte kullanmadıkça, tüm parolalar ana veritabanında depolanır.
  - [SQL veritabanı ve SQL veri ambarı 'na veritabanı erişimini denetleme ve verme](sql-database-manage-logins.md)makalesine bakın.

- Parola yönetimi en iyi yöntemlerini izleyin:
  - Latin büyük ve küçük harflerden, rakamlardan (0-9) ve alfasayısal olmayan karakterlerden ($,!, # veya%) oluşan karmaşık bir parola sağlayın.
  - Rastgele seçili karakterler yerine daha uzun parolalar kullanın.
  - En az 90 günde bir el ile parola değişikliği uygulayın.

## <a name="access-management"></a>Erişim yönetimi

Erişim yönetimi, yetkili kullanıcıların Azure SQL veritabanı erişimini ve ayrıcalıklarını denetleme ve yönetme işlemidir.

### <a name="implement-principle-of-least-privilege"></a>En az ayrıcalık ilkesini uygula

> [!NOTE]
> Bahsedilen: Fedrampa denetimleri AC-06, NıST: AC-6, OSA uygulama #3

En az ayrıcalık ilkesi, kullanıcıların görevlerini tamamlaması için gerekenden daha fazla ayrıcalığa sahip olmaması gerektiğini belirtir. Daha fazla bilgi için, [yalnızca yeterli yönetim](https://docs.microsoft.com/powershell/scripting/learn/remoting/jea/overview)makalesine bakın.

**Nasıl uygulanır**:

Gerekli görevleri gerçekleştirmek için yalnızca gerekli [izinleri](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) atayın:

- SQL veri düzleminde: 
    - Ayrıntılı izinler ve Kullanıcı tanımlı veritabanı rolleri (ya da mı 'de sunucu rolleri) kullanın: 
        1. Gerekli rolleri oluşturma
            - [ROL OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-role-transact-sql)
            - [SUNUCU ROLÜ OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-server-role-transact-sql)
        1. Gerekli kullanıcıları oluşturun
            - [KULLANıCı OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql)
        1. Rollere üye olarak Kullanıcı ekleme 
            - [ROLÜ DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql)
            - [SUNUCU ROLÜNÜ DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql)
        1. Ardından rollere izinler atayın. 
            - [SEMANTIĞI](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) 
    - Kullanıcıları gereksiz rollere atamadığınızdan emin olun.

- Azure Resource Manager:
  - Kullanılabilir veya özel RBAC rolleri için yerleşik roller kullanın ve gerekli izinleri atayın.
    - [Azure için yerleşik roller](../role-based-access-control/built-in-roles.md) 
    - [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md)

**En iyi uygulamalar**:

Aşağıdaki en iyi yöntemler isteğe bağlıdır, ancak güvenlik stratejinizin daha iyi yönetilebilirlik ve desteklenebilirliği ile sonuçlanır: 

- Mümkünse, mümkün olan en az sayıda izin ile başlayın ve gerçek bir zorunludur (ve gerekçe) varsa izinleri bir tane ekleyerek bir tane, zıt yaklaşımdan farklı olarak bir adım adım. 

- Bireysel kullanıcılara izin atamayı engellemez. Bunun yerine rolleri (veritabanı veya sunucu rolleri) sürekli olarak kullanın. Roller, raporlama ve sorun giderme izinlerinin büyük ölçüde sağlanmasına yardımcı olur. (Azure RBAC yalnızca roller aracılığıyla izin atamasını destekler.) 

- Rollerin izinleri Kullanıcı için gereken izinlerle tam olarak eşleşiyorsa yerleşik rolleri kullanın. Kullanıcıları birden çok role atayabilirsiniz. 

- Yerleşik roller çok fazla veya yetersiz izin vermekle özel roller oluşturup kullanın. Uygulamada kullanılan tipik roller: 
  - Güvenlik dağıtımı 
  - Yönetici 
  - Geliştirici 
  - Destek personeli 
  - Ekleme 
  - Otomatikleştirilmiş süreçler 
  - Son kullanıcı 

- SQL Server veritabanı altyapısındaki izinlerin aşağıdaki kapsamlara uygulanabileceğini unutmayın. Kapsam ne kadar küçükse, verilen izinlerin etkisi daha küçüktür: 
  - Azure SQL veritabanı sunucusu (ana veritabanında özel roller) 
  - Veritabanı 
  - Şema (Ayrıca bkz. [SQL Server Için şema tasarımı: güvenlik göz önünde bulundurularak şema tasarımı önerileri](http://andreas-wolter.com/en/schema-design-for-sql-server-recommendations-for-schema-design-with-security-in-mind/))
  - Nesne (tablo, görünüm, yordam, vb.) 
  > [!NOTE]
  > Bu düzey, genel uygulamaya gereksiz karmaşıklık eklediğinden nesne düzeyinde izinlerin uygulanması önerilmez. Nesne düzeyi izinleri kullanmaya karar verirseniz, bunlar açıkça açıklanmalıdır. Aynı nedenlerden dolayı daha az recommendable olan sütun düzeyi izinler için de geçerlidir. [Reddetme](https://docs.microsoft.com/sql/t-sql/statements/deny-object-permissions-transact-sql) için standart kurallar sütunlar için uygulanmaz.

- Çok fazla izin için test etmek üzere [güvenlik açığı değerlendirmesi (VA)](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) kullanarak düzenli denetimler gerçekleştirin.

### <a name="implement-separation-of-duties"></a>Görevlerin ayrılmasını uygulama

> [!NOTE]
> Bahsedilen: Fedrampa: AC-04, NıST: AC-5, ISO: A. 6.1.2, PCI 6.4.2, SOC: CM-3, SDL-3

Görev ayrımı olarak da adlandırılan görev ayrımı, önemli görevleri, genellikle veri ihlallerinin önlenmesi için farklı kullanıcılara atanan birden çok göreve ayırma gereksinimini açıklar.

**Nasıl uygulanır**:

- Gerekli görevlerin ayrılma düzeyini belirler. Örnekler: 
  - Geliştirme/test ve üretim ortamları arasında 
  - Güvenlik temelinde hassas görevler vs veritabanı Yöneticisi (DBA) Yönetim düzeyi görevler ve geliştirici görevleri. 
    - Örnekler: Denetçi, rol düzeyi güvenlik (RLS) için güvenlik ilkesi oluşturma, DDL izinleri ile SQL veritabanı nesneleri uygulama.

- Sisteme erişen kullanıcıların (ve otomatikleştirilmiş işlemlerin) kapsamlı bir hiyerarşisini belirler.

- Gerekli Kullanıcı gruplarına göre roller oluşturun ve rollere izinler atayın. 
  - Azure portal veya PowerShell aracılığıyla yönetim düzeyi görevler için, RBAC rollerini kullanın. Gereksinimle eşleşen bir yerleşik rol bulun ya da kullanılabilir izinleri kullanarak özel bir RBAC rolü oluşturun 
  - Yönetilen bir örnekte sunucu genelindeki görevler için sunucu rolleri oluşturun (yeni oturumlar, veritabanları oluşturma). 
  - Veritabanı düzeyindeki görevler için veritabanı rolleri oluşturun.

- Belirli hassas görevler için, görevleri Kullanıcı adına yürütmek üzere bir sertifika tarafından imzalanan özel saklı yordamlar oluşturmayı düşünün. 
  - Örnek: [öğretici: saklı yordamları bir sertifikayla imzalama](https://docs.microsoft.com/sql/relational-databases/tutorial-signing-stored-procedures-with-a-certificate) 

- Veri sahibi ve güvenlik sahibi arasında görevlerin ayrılmasını sağlamak için Azure Key Vault müşteri tarafından yönetilen anahtarlarla Saydam Veri Şifrelemesi (TDE) uygulayın. 
  - [Azure Portal Azure depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](../storage/common/storage-encryption-keys-portal.md)makalesine bakın. 

- Bir DBA 'nın yüksek oranda duyarlı kabul edilen verileri görememesini sağlamak ve yine de DBA görevlerini yapabilmeniz için rol ayrımı ile Always Encrypted kullanabilirsiniz. 
  - Bkz. [Always Encrypted Için temel yönetime genel bakış](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted), [rol ayrımı ile anahtar hazırlama](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell#KeyProvisionWithRoles)ve [rol ayrımı ile sütun ana anahtar döndürme](https://docs.microsoft.com/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell#column-master-key-rotation-with-role-separation). 

- En az önemli maliyetler ve sistem kullanım dışı olan çabalara uygun olmadığı durumlarda, comleişler şu gibi telafi denetimleri kullanılarak yapılabilir ve azaltılabilir: 
  - İşlemlerde insan katılımı. 
  - Denetim izleri – denetim hakkında daha fazla bilgi için bkz. [kritik güvenlik olaylarını denetleme](#audit-critical-security-events).

**En iyi uygulamalar**:

- Geliştirme/test ve üretim ortamları için farklı hesapların kullanıldığından emin olun. Farklı hesaplar test & üretim sistemlerinin ayırılmasına uyum sağlamaya yardımcı olur.

- Bireysel kullanıcılara izin atamayı engellemez. Bunun yerine rolleri (veritabanı veya sunucu rolleri) sürekli olarak kullanın. Rollerin olması, raporlama ve sorun giderme izinlerinin büyük ölçüde sağlanmasına yardımcı olur.

- İzinler tam olarak gereken izinlerle eşleşiyorsa yerleşik roller kullanın – birden çok yerleşik rolün tüm izinlerinin birleşimi %100 eşleştirmeye neden olursa, aynı anda birden fazla rol de atayabilirsiniz. 

- Yerleşik roller çok fazla izin veya yetersiz izin vermesi durumunda özel roller oluşturup kullanın. 

- Rol atamaları Ayrıca, T-SQL ' d a SQL Agent Iş adımlarında ya da RBAC rolleri için Azure PıM 'yi kullanarak, Ayrıca, örneğin, dinamik olarak (DSD) (örneğin, bir veya daha fazla işlem), 

- DBAs 'nin şifreleme anahtarlarına veya anahtar depolarına erişimi olmadığından ve anahtarlara erişimi olan güvenlik yöneticilerinin sırasıyla veritabanına erişimi olmadığından emin olun. 

- Güvenlikle ilgili eylemler için her zaman bir denetim izlemesine sahip olduğunuzdan emin olun. 

- Kullanılan izinleri görmek için yerleşik RBAC rollerinin tanımını alabilir ve bunları PowerShell aracılığıyla alıntıları ve birikmelerini temel alan özel bir rol oluşturabilirsiniz 

- Db_owner veritabanı rolünün herhangi bir üyesi Saydam Veri Şifrelemesi (TDE) gibi güvenlik ayarlarını değiştiremediğinden veya SLO 'yu değiştirebileceğinizden, bu üyelikle ilgili Üyelik verilmelidir. Diğer taraftan, DB seçeneklerini değiştirme gibi herhangi bir veritabanı ayarını değiştirme gibi birçok görev db_owner ayrıcalık gerektirir. Denetim, herhangi bir çözümde önemli bir rol oynar.

- Bir db_owner yalnızca izinleri olan kullanıcı verilerini görüntülemesini sağlamak mümkün değildir. Veritabanında son derece hassas veriler varsa, db_owners veya herhangi bir DBA 'nın bunu görüntülemesini güvenli bir şekilde engellemek için Always Encrypted kullanılabilir.

> [!NOTE]
> Güvenlik ile ilgili veya sorun giderme görevleri için, görevlerin (SoD) ayrımı elde etmek zor olabilir. Geliştirme ve son kullanıcı rolleri gibi diğer alanların ayırt etmek daha kolay olur. Uyumlulukla ilgili çoğu denetim, diğer çözümlerin pratik olmadığı durumlarda denetim gibi alternatif denetim işlevlerinin kullanılmasına izin verir.

SoD 'ye daha ayrıntılı bir şekilde geçmek isteyen okuyucular için aşağıdaki kaynakları öneririz: 

- Azure SQL veritabanı için:  
  - [SQL veritabanı ve SQL veri ambarı 'na veritabanı erişimini denetleme ve verme](sql-database-manage-logins.md)
  - [Uygulama geliştiricisi için görevlerin altyapı ayrımı](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/cc974525(v=sql.100)) 
  - [SQL Server 2014 ' de görevlerin ayrımı](https://www.microsoft.com/download/details.aspx?id=39269)
  - [SQL Server saklı yordamları imzalama](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server)

- Azure Kaynak yönetimi için:
  - [Azure için yerleşik roller](../role-based-access-control/built-in-roles.md) 
  - [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md)
  - [Yükseltilmiş erişim için Azure AD Privileged Identity Management kullanma](https://www.microsoft.com/en-us/itshowcase/using-azure-ad-privileged-identity-management-for-elevated-access)

### <a name="perform-regular-code-reviews"></a>Düzenli kod İncelemeleri gerçekleştirin

> [!NOTE]
> Bahsedilen: PCI: 6.3.2, SOC: SDL-3 

Görevlerin ayrımı, veritabanındaki verilerle sınırlı değildir, ancak uygulama kodunu içerir. Kötü amaçlı kod güvenlik denetimlerini değiştirebilir. Üretime özel kod dağıtılmadan önce, nelerin dağıtıldığını gözden geçirmeniz önemlidir.

**Nasıl uygulanır**:

- Kaynak denetimini destekleyen Azure Data Studio gibi bir veritabanı aracı kullanın. 

- Ayrılmış bir kod dağıtım işlemi uygulayın.

- Ana dala geçmeden önce, bir kişinin (kodun kendi yazarından başka biri) olası Ayrıcalıkların yükseltilmesi için kodu incelemektir ve sahte ve standart dışı erişim için kötü amaçlı veri değişiklikleri yapmanız gerekir. Bu, kaynak denetimi mekanizmaları kullanılarak yapılabilir.

**En iyi uygulamalar**:

- Standartlaştırma: herhangi bir kod güncelleştirmesi için izlenen standart bir yordam uygulamaya yardımcı olur. 

- Güvenlik açığı değerlendirmesi, aşırı izin denetleyen kuralları, eski şifreleme algoritmalarının kullanımını ve bir veritabanı şeması içindeki diğer güvenlik sorunlarını kontrol eden kuralları içerir. 

- Daha fazla denetim, SQL yerleştirmeye açık olan kodu tarayan Gelişmiş tehdit koruması kullanılarak QA veya test ortamında yapılabilir.

- Neye baktığın örnekleri: 
  - Otomatik SQL kodu-güncelleştirme dağıtımı içinden bir Kullanıcı oluşturma veya güvenlik ayarlarını değiştirme. 
  - Belirtilen parametrelere bağlı olarak, bir hücredeki parasal bir değeri, uygun olmayan bir şekilde güncelleştiren bir saklı yordam. 

- Gözden geçirmeyi çalıştıran kişinin, kaynak kod yazarı dışında bir bireyin ve kod incelemelerinde ve güvenli kodlarda bilgili olduğundan emin olun.

- Kod değişikliklerinin tüm kaynaklarını bilindiğinizden emin olun: kod T-SQL betiklerine ait olabilir; Bu, çalıştırılacak veya görünümler, Işlevler, Tetikleyiciler ve saklı yordamlar biçiminde dağıtılması için geçici komutlar olabilir. Ayrıca, SQL Agent Iş tanımlarının bir parçası olabilir (adımlar), SSIS paketleri, Azure Data Factory ve diğer hizmetler içinden yürütülür.

## <a name="data-protection"></a>Veri koruması

Veri koruma, şifreleme veya gizleme ile önemli bilgilerin tehlikeye düşmesi için bir dizi özellik kümesidir.

> [!NOTE]
> Microsoft, Azure SQL veritabanı 'na FIPS 140-2 düzey 1 uyumlu olduğu için test eder. Bu işlem, FIPS 140-2 düzey 1 kabul edilebilir algoritmaların katı kullanımını doğruladıktan sonra ve gerekli anahtar uzunlukları, anahtar yönetimi, anahtar oluşturma ve anahtar depolama ile tutarlılık dahil olmak üzere bu algoritmaların FIPS 140-2 düzey 1 tarafından onaylanmış örneklerinin doğrulanmasından sonra yapılır. Bu kanıtlama, müşterilerimizin veri işlemesinde veya sistem veya uygulama tesliminde FIPS 140-2 düzey 1 doğrulanan örneklerin kullanımı gereksinimini veya gereksinimini yanıtlamasını sağlamak için tasarlanmıştır. Yukarıdaki bildirimde "FIPS 140-2 düzey 1 uyumlu" ve "FIPS 140-2 düzey 1 uyumluluk" terimlerini, ABD ve Kanada kamu, farklı "FIPS 140-2 düzey 1" teriminin kullanımı için tasarlanan koşulları gösterecek şekilde tanımladık. 


### <a name="encrypt-data-in-transit"></a>Yoldaki verileri şifreleme

> [!NOTE]
> Bahsedilen: OSA uygulaması #6, ISO denetim ailesi: şifreleme

Verileriniz, istemciniz ve sunucunuz arasında taşınırken verilerinizi korur. [Ağ güvenliği](#network-security)' ne bakın.

### <a name="encrypt-data-at-rest"></a>Bekleyen verileri şifreleme

> [!NOTE]
> Bahsedilen: OSA uygulaması #6, ISO denetim ailesi: şifreleme

Bekleyen şifreleme, veritabanı, günlük ve yedekleme dosyalarında kalıcı hale geldiğinde verilerin şifreleme korumasından oluşur.

**Nasıl uygulanır**:

- Azure SQL veritabanı 'nda 2017 ' den sonra oluşturulan tüm veritabanları için, hizmet tarafından yönetilen anahtarlarla [Saydam veritabanı şifrelemesi (TDE)](transparent-data-encryption-azure-sql.md) varsayılan olarak etkindir.
- Yönetilen bir örnekte, veritabanı şirket içi bir sunucudan geri yükleme işleminin sonucu olarak oluşturulduysa, özgün veritabanının TDE ayarı uygulanır. Özgün veritabanında TDE etkin değilse, yönetilen örnek için TDE 'nın el ile açık olmasını öneririz.

**En iyi uygulamalar**:

- Ana veritabanında bekleyen şifreleme gerektiren verileri depolamayın. Ana veritabanı TDE ile şifrelenemez.

- TDE koruması üzerinde daha fazla saydamlığa ve ayrıntılı denetime ihtiyacınız varsa Azure Key Vault müşteri tarafından yönetilen anahtarları kullanın. Azure Key Vault, veritabanını erişilemez hale getirecek herhangi bir zamanda izinleri iptal etme yeteneğinin yapılmasına izin verir. Diğer anahtarlarla birlikte TDE koruyucuları merkezi olarak yönetebilir veya Azure Key Vault kullanarak TDE koruyucuyu kendi zamanlamanızda döndürebilirsiniz.

- Azure Key Vault ' de müşteri tarafından yönetilen anahtarlar kullanıyorsanız, makaleyi, [TDE ile Azure Key Vault yapılandırma yönergelerini](transparent-data-encryption-byok-azure-sql.md#recommendations-when-configuring-akv) ve [Azure Key Vault Ile coğrafi-Dr](transparent-data-encryption-byok-azure-sql.md#geo-dr-and-customer-managed-tde)'yi yapılandırmayı izleyin.

### <a name="protect-sensitive-data-in-use-from-high-privileged-unauthorized-users"></a>Yüksek ayrıcalıklı, yetkisiz kullanıcılardan kullanımda olan hassas verileri koruyun

Kullanılan veriler, SQL sorgularının yürütülmesi sırasında veritabanı sisteminde bellekte depolanan veriler. Veritabanınız hassas verileri depoluyorsa, kuruluşunuz, kuruluşunuzda Microsoft işleçleri veya DBAs gibi yüksek ayrıcalıklı kullanıcıların SQL Server işlem belleğinden veri ayıklamasının önlendiğinden emin olmak için gerekli olabilir. veritabanını sorgularken düz metin verileri görüntülemek için.

**Nasıl uygulanır**:

- Hassas verilerin bellekte/kullanımda olsa bile Azure SQL veritabanı 'nda düz metin olarak gösterilmemesini sağlamak için [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) kullanın. Bu, verileri veritabanı yöneticilerinin (DBAs) ve bulut yöneticilerinin (ya da yüksek ayrıcalıklı ancak yetkisiz kullanıcıları taklit edebilen kötü aktörlerden) korur ve verilerinize kimlerin erişebileceği hakkında daha fazla denetim sağlar.

**En iyi uygulamalar**:

- Always Encrypted, verileri bekleyen (TDE) veya aktarım (SSL/TLS) ile şifrelemek için bir alternatif değildir ve performans ve işlevsellik etkisini en aza indirmek için hassas olmayan veriler için kullanılmamalıdır. Verilerin Rest, aktarım sırasında ve kullanımda olan kapsamlı korunması için TDE ve TLS ile birlikte Always Encrypted kullanılması önerilir. 

- Kötü amaçlı DBAs verilerini korumak için Always Encrypted kullanıyorsanız rol ayrımı ile Always Encrypted anahtarlarını yönetin. Rol ayrımı ile bir güvenlik yöneticisi fiziksel anahtarları oluşturur ve bir DBA, veritabanında fiziksel anahtarları açıklayan sütun ana anahtarı ve sütun şifreleme anahtarı meta veri nesneleri oluşturur. Bu işlem sırasında güvenlik yöneticisinin veritabanına erişmesi gerekmez ve DBA 'nın fiziksel anahtarlara düz metin olarak erişmesi gerekmez. 
  - Ayrıntılar için, [rol ayrımı Ile anahtarları yönetme](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted#managing-keys-with-role-separation) makalesine bakın. 

- Sütun ana anahtarlarınızı yönetim kolaylığı için Azure Key Vault depolayın. Anahtar yönetimini sabit hale getirmek için Windows sertifika deposu 'nı (ve genel, dağıtılmış anahtar deposu çözümlerini, diğer merkezi anahtar yönetim çözümleri olarak) kullanmaktan kaçının. 

- Birden çok anahtar (sütun ana anahtar veya sütun şifreleme anahtarı) kullanmanın avantajları hakkında dikkatle düşünün. Anahtar yönetim maliyetini azaltmak için anahtar sayısını küçük tutun. Tek bir sütun ana anahtarı ve veritabanı başına bir sütun şifreleme anahtarı genellikle sabit durum ortamlarında (anahtar dönüşünün ortasında değil) yeterlidir, her biri farklı anahtarlar ve farklı verilere erişmediğiniz için farklı Kullanıcı gruplarınız yoktur.  

- Uyumluluk gereksinimlerinize göre sütun ana anahtarlarını döndürün. Ayrıca, sütun şifreleme anahtarlarını döndürmenize gerek duyuyorsanız, uygulama kapalı kalma süresini en aza indirmek için çevrimiçi şifrelemeyi kullanmayı düşünün. 
  - Bkz. [performans ve kullanılabilirlik konuları](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-column-encryption-using-powershell#performance-and-availability-considerations). 

- Veriler üzerinde hesaplamaların (eşitlik) desteklenme ihtiyacı varsa belirleyici şifrelemeyi kullanın. Aksi takdirde, rastgele şifreleme kullanın. Düşük entropi veri kümeleri veya genel olarak bilinen dağıtıma sahip veri kümeleri için belirleyici şifreleme kullanmaktan kaçının. 

- Verilerinize izniniz olmadan üçüncü taraf erişim konusunda endişeleriniz varsa, düz metin olarak bulunan anahtarlar ve verilere erişimi olan tüm uygulama ve araçların Microsoft Azure bulut dışında çalıştırıldığına emin olun. Anahtarlara erişim olmadan, üçüncü tarafın şifrelemeyi atlayamadığı müddetçe verilerin şifresini çözme yöntemi olmayacaktır.

- Always Encrypted, anahtarlara (ve korunan verilere) geçici erişim vermeyi kolay bir şekilde desteklemez. Örneğin, anahtarları bir DBA ile paylaşmanız gerekiyorsa, DBA 'nın hassas ve şifrelenmiş veriler üzerinde bazı temizleme işlemleri gerçekleştirmesini sağlamak için, güvenilirlikten gelen verilere erişimi iptal etmenin tek yolu, hem sütun şifreleme anahtarlarını hem de sütunları 'yı döndürmenizi sağlar söz konusu maliyetli bir işlem olan verileri koruyan MN ana anahtarlar. 

- Şifrelenmiş sütunlarda düz metin değerlerine erişmek için bir kullanıcının CMK 'ye erişimi olması gerekir. Bu, CMK 'yı tutan anahtar deposunda yapılandırılan sütunları koruyor. Ayrıca, kullanıcının herhangi bir sütun ana anahtar tanımını GÖRÜNTÜLEMESI ve herhangi bir sütun ŞIFRELEME anahtarı tanımı veritabanı izinlerini GÖRÜNTÜLEMESI gerekir.

### <a name="control-access-of-application-users-to-sensitive-data-through-encryption"></a>Uygulama kullanıcılarına şifreleme aracılığıyla hassas verilere erişimi denetleme

Şifreleme, yalnızca şifreleme anahtarlarına erişimi olan ve verileri görüntüleyebileceği veya güncelleştirebilen belirli uygulama kullanıcılarının olduğundan emin olmak için bir yol olarak kullanılabilir.

**Nasıl uygulanır**:

- Hücre düzeyinde şifrelemeyi (CLE) kullanın. Ayrıntılar için [bir veri sütununu şifreleyin](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) başlıklı makaleye bakın. 
- Alternatif olarak, Always Encrypted kullanmayı düşünün ancak aşağıda listelenen sınırlamanın farkında olun.

**En iyi uygulamalar**

CLE kullanırken:

- SQL izinleri ve rolleri aracılığıyla anahtarlara erişimi denetleyin. 

- Veri şifreleme için AES (AES 256 önerilir) kullanın. Bu tür RC4, DES ve TripleDES algoritmaları kullanım dışıdır ve bilinen güvenlik açıkları nedeniyle kullanılmamalıdır. 

- 3DES kullanmaktan kaçınmak için simetrik anahtarları asimetrik anahtarlar/sertifikalarla koruyun (parolalar). 

- Dışarı aktarma/içeri aktarma (bacpac dosyaları) aracılığıyla hücre düzeyinde şifreleme kullanarak bir veritabanını geçirirken dikkatli olun. 
  - Verileri geçirirken anahtar kaybını önleme ve diğer en iyi Yöntem Kılavuzu için [Azure SQL veritabanı 'Nda hücre düzeyinde şifrelemeyi kullanma önerileri](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/) başlıklı makaleye bakın.

Always Encrypted kullanırken, Always Encrypted birincil olarak Azure SQL veritabanı 'nın (bulut işletmenleri, DBAs) yüksek ayrıcalıklı kullanıcılardan kullanımda olan hassas verileri korumak için tasarlandığını unutmayın. [yüksek ayrıcalıklı, yetkisiz kullanıcılardan kullanımda olan hassas verileri koruma](#protect-sensitive-data-in-use-from-high-privileged-unauthorized-users)konusuna bakın. Uygulama kullanıcılarından verileri korumak için Always Encrypted kullanırken aşağıdaki güçlükleri göz önünde bulundurun:

- Kullanıcıya, sütun şifreleme anahtarına ve sütun ana anahtarına erişim izinleri vererek gizli verilere erişim izni verdikten sonra, bu erişimi güvenilir bir şekilde iptal etmek için, bu erişimi, gereken pahalı bir işlem olan sütun şifreleme anahtarını Tüm sütunları yeniden şifrelemek, sütun şifreleme anahtarı korunur. 

- Varsayılan olarak, Always Encrypted destekleyen tüm Microsoft istemci sürücüleri, sütun şifreleme anahtarlarının genel (uygulama başına bir) önbelleğini korur. Bir istemci sürücü, bir sütun ana anahtarı tutan bir anahtar deposuyla iletişim kurarak bir düz metin sütunu şifreleme anahtarı aldıktan sonra, düz metin sütunu şifreleme anahtarı önbelleğe alınır ve bu, çok kullanıcılı bir uygulamanın kullanıcılarından veri yalıtma yapmayı zorlaştırır. Uygulamanız bir anahtar deposuyla (örneğin, Azure Key Vault) etkileşim kurarken son kullanıcıları taklit ettiğinde, bir kullanıcının sorgusu önbelleği bir sütun şifreleme anahtarı ile doldurduktan sonra, aynı anahtarı gerektiren, ancak başka bir kullanıcı tarafından tetiklenen bir sonraki sorgu şunu kullanır önbelleğe alınan anahtar. Sürücü, anahtar deposunu çağırmayacak ve ikinci kullanıcının sütun şifreleme anahtarına erişim izni olup olmadığını denetmeyecektir. Sonuç olarak, Kullanıcı anahtarlara erişime sahip olmasa bile şifrelenmiş verileri görebilir. Çok kullanıcılı bir uygulamadaki kullanıcıların yalıtımına ulaşmak için, sürücünün her bir veri şifrelemesi veya şifre çözme için anahtar deposuyla iletişim kurabilmesi gerekeceğinden, daha fazla performansa neden olacak sütun şifreleme anahtarı önbelleğe almayı devre dışı bırakmanız gerekebilir. çalışmasını.

### <a name="protect-data-against-unauthorized-viewing-by-application-users-while-preserving-data-format"></a>Veri biçimini korurken, uygulama kullanıcılarına yönelik verileri yetkisiz görüntülemeye karşı koruma
Yetkisiz kullanıcıların verileri görüntülemesini engellemeye yönelik başka bir teknik ise, kullanıcı uygulamalarının işleme devam edip verileri görüntülemesini sağlamak için veri türlerini ve biçimlerini korurken verileri belirsizleyebilir veya maskeleyebilir.

**Nasıl uygulanır**:

- Tablo sütunlarını belirsizetmek için [dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) kullanın.

> [!NOTE]
> Always Encrypted, dinamik veri maskeleme ile çalışmaz. Aynı sütunu şifrelemek ve maskelemek mümkün değildir; bu, kullanımdaki verilerin korunmasını, dinamik veri maskeleme aracılığıyla uygulama kullanıcılarınız için verileri maskeleme açısından önceliklendirmenize gerek kalmaz.

**En iyi uygulamalar**:

> [!NOTE]
> Dinamik veri maskeleme, yüksek ayrıcalıklı kullanıcılardan verileri korumak için kullanılamaz. Maskeleme ilkeleri, db_owner gibi yönetici erişimi olan kullanıcılara uygulanmaz.

- Uygulama kullanıcılarının geçici sorgular çalıştırmasına izin verme (dinamik veri maskeleme 'nin çevresinde çalışabilen şekilde).  
  - Ayrıntılar için [çıkarım veya deneme yanılma tekniklerini kullanarak maskelemeyi atlayarak](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking#security-note-bypassing-masking-using-inference-or-brute-force-techniques) makaleye bakın.  

- Maskelenmiş sütunlarda güncelleştirme yapmak için Kullanıcı izinlerini sınırlamak üzere uygun bir erişim denetim ilkesi (SQL izinleri, roller, RLS aracılığıyla) kullanın. Bir sütunda maske oluşturma, bu sütun için güncelleştirmeleri engellemez. Bu nedenle, maskelenmiş sütunu sorgularken kullanıcılar maskelenmiş verileri alsa da, yazma izinlerine sahip olmaları durumunda aynı kullanıcılar verileri güncelleştirebilir.    

-  Dinamik veri maskeleme, maskelenmiş değerlerin istatistiksel özelliklerini korumaz, bu da sorgu sonuçlarını etkileyebilir (örneğin, filtreleme koşullarını içeren sorgular veya maskelenmiş verilerdeki birleşimler).

## <a name="network-security"></a>Ağ güvenliği
Ağ güvenliği, Azure SQL veritabanı 'na aktarma sırasında verilerinizi güvenli hale getirmek için erişim denetimlerine ve en iyi yöntemlere başvurur.

### <a name="configure-my-client-to-connect-securely-to-azure-sql-database"></a>İstemcinizi Azure SQL veritabanı 'na güvenli bir şekilde bağlanacak şekilde yapılandırma 
Eski protokollerde ve şifre paketlerinde bağımlılığın olması nedeniyle Azure SQL veritabanına bağlanan istemci makinelerin ve uygulamaların iyi bilinen güvenlik açıklarına karşı yapılmasını engelleyin.

**Nasıl uygulanır**:

- Azure SQL veritabanı 'na bağlanan istemci makinelerin [Aktarım Katmanı Güvenliği (TLS)](sql-database-security-overview.md#transport-layer-security-tls-encryption-in-transit)kullandığından emin olun.

**En iyi uygulamalar**:

- Şifreleme etkinken SQL DB 'ye bağlanmak için tüm uygulamalarınızı ve araçlarınızı yapılandırın 
  - Encrypt = on, TrustServerCertificate = off (veya Microsoft olmayan sürücülerle eşdeğerdir). 

- Uygulamanız TLS desteklemeyen bir sürücü kullanıyorsa veya daha eski bir TLS sürümünü destekliyorsa, mümkünse sürücüyü değiştirin. Mümkün değilse, güvenlik risklerini dikkatle değerlendirin. 

- SSL 2,0, SSL 3,0, TLS 1,0 ve TLS 1,1 güvenlik açıkları aracılığıyla saldırı vektörlerini, [Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarları](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-10)BAŞıNA Azure SQL veritabanı 'na bağlanan istemci makinelerde devre dışı bırakarak azaltabilirsiniz. 

- [TLS/SSL 'deki (Schannel SSP) şifre paketleri](https://docs.microsoft.com/windows/desktop/SecAuthN/cipher-suites-in-schannel) başına istemci üzerindeki şifreleme paketlerini kontrol edın ve [TLS şifre paketi sırasını yapılandırma](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-cipher-suite-order)başına özellikle 3DES 'i devre dışı bırakın. 

- Yönetilen bir örnek kullanıyorsanız, sunucu tarafında şifrelemeyi zorladığı için **proxy** bağlantı türünü (varsayılan) kullanın. **Yeniden yönlendirme** bağlantı türü şu anda şifreleme Zorlamayı desteklemiyor ve yalnızca özel IP bağlantılarında kullanılabilir. 
  - Daha fazla bilgi için bkz. [Azure SQL bağlantı mimarisi-bağlantı ilkesi](sql-database-connectivity-architecture.md#connection-policy).
  - Şifreleme hem proxy hem de yeniden yönlendirme bağlantı türleri için zorlanır. 

### <a name="minimize-attack-surface"></a>Saldırı yüzeyini en aza indirme
Azure SQL veritabanı için ağ erişim denetimleri uygulayarak kötü niyetli bir kullanıcı tarafından saldırıya uğramakta olabilecek Özellik sayısını en aza indirin.

> [!NOTE]
> Bahsedilen: OSA uygulaması #5

**Nasıl uygulanır**:

Bir Azure SQL veritabanı sunucusunda (tek veritabanı veya elastik havuzlar içeren):
- Azure hizmetlerine erişime Izin ver ' i kapalı olarak ayarlayın.

- Sanal ağ hizmet uç noktalarını ve sanal ağ güvenlik duvarı kurallarını kullanın.

- Özel bağlantı (Önizleme) kullanın.

Yönetilen bir örnekte:
- [Ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements)bölümündeki yönergeleri izleyin. 

**En iyi uygulamalar**:

- Özel bir uç noktaya bağlanarak (örneğin, özel bir veri yolu kullanarak) Azure SQL veritabanı 'na erişimi kısıtlama: 
  - Yönetilen bir örnek, dış erişimi engellemek için bir sanal ağ içinde yalıtılabilir. Aynı bölgedeki aynı veya eşlenmiş sanal ağda bulunan uygulamalar ve araçlar doğrudan erişebilir. Farklı bölgedeki uygulamalar ve araçlar, bağlantı kurmak için VNet-VNet bağlantısı veya ExpressRoute bağlantı hattı eşliğini kullanabilir. Müşteri, 1433 numaralı bağlantı noktası üzerinden erişimi yalnızca yönetilen örneğe erişim gerektiren kaynaklarla kısıtlamak için ağ güvenlik grupları (NSG) kullanmalıdır 
  - Bir SQL veritabanı sunucusu için (tek veritabanları veya elastik havuzlar içeren), sanal ağınızın içindeki SQL veritabanı sunucusu için adanmış bir özel IP sağlayan [özel bağlantı](sql-database-private-endpoint-overview.md) özelliğini kullanın. SQL veritabanı sunucularınıza erişimi kısıtlamak için sanal ağ [güvenlik duvarı kurallarıyla sanal ağ hizmet uç noktalarını](sql-database-vnet-service-endpoint-rule-overview.md) da kullanabilirsiniz.
  - Mobil kullanıcıların, veri yolu üzerinden bağlanmak için Noktadan siteye VPN bağlantıları kullanmalarını gerekir.
  - Şirket içi ağına bağlanan kullanıcıların, veri yolu üzerinden bağlanmak için siteden siteye VPN bağlantısı veya ExpressRoute 'u kullanmalarını gerekir.

- Azure SQL veritabanı 'na genel bir uç noktaya bağlanarak erişebilirsiniz (örneğin, bir ortak veri yolu kullanarak). Aşağıdaki en iyi yöntemler göz önünde bulundurulmalıdır: 
  - Bir SQL veritabanı sunucusunda, erişimi yalnızca yetkili IP adresleriyle kısıtlamak için [IP güvenlik duvarı kurallarını](sql-database-firewall-configure.md) kullanın.
  - Yönetilen bir örnek için, 3342 numaralı bağlantı noktası üzerinden erişimi yalnızca gerekli kaynaklarla kısıtlamak için ağ güvenlik grupları (NSG) kullanın. Daha fazla bilgi için bkz. [genel uç noktalarla güvenli bir Azure SQL veritabanı yönetilen örneği kullanma](sql-database-managed-instance-public-endpoint-securely.md). 

> [!NOTE]
> Yönetilen örnek genel uç noktası varsayılan olarak etkinleştirilmemiştir ve açıkça etkinleştirilmelidir. Şirket ilkesi genel uç noktaların kullanılmasına izin vermez, ilk yerde genel bitiş noktalarının etkinleştirilmesini engellemek için [Azure ilkesi](../governance/policy/overview.md) 'ni kullanın.

- Azure ağ bileşenlerini ayarlama: 
  - [Ağ güvenliği için En Iyi Azure uygulamalarını](../security/fundamentals/network-best-practices.md)izleyin.
  - [Azure sanal ağ hakkında sık sorulan sorular (SSS)](../virtual-network/virtual-networks-faq.md) ve plan 'da özetlenen en iyi uygulamalar başına sanal ağ (VNet) yapılandırması planlayın. 
  - Bir sanal ağı birden çok alt ağa bölüm ve benzer rolün kaynaklarını aynı alt ağa (örneğin, ön uç ile arka uç kaynakları) atayın.
  - Azure VNet sınırının içindeki alt ağlar arasındaki trafiği denetlemek için [ağ güvenlik grupları (NSG 'ler)](../virtual-network/security-overview.md) kullanın.
  - Gelen ve giden ağ trafiğini izlemek üzere aboneliğiniz için [Azure Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md) 'ni etkinleştirin.

### <a name="configure-power-bi-for-secure-connections-to-azure-sql-database"></a>Azure SQL veritabanı 'na güvenli bağlantılar için Power BI yapılandırma

**En iyi uygulamalar**:

- Power BI Desktop için, mümkün olan her durumda özel veri yolunu kullanın. 

- İstemci makinesindeki kayıt defteri anahtarını [Aktarım Katmanı Güvenliği (TLS)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) kayıt defteri ayarları başına AYARLAYARAK Power BI Desktop TLS 1.2 kullanarak bağlantı aldığından emin olun. 

- [Power BI Ile satır düzeyi güvenlik (RLS)](https://docs.microsoft.com/power-bi/service-admin-rls)aracılığıyla belirli kullanıcılar için veri erişimini kısıtlayın. 

- Power BI hizmeti için şirket [içi veri ağ geçidini](https://docs.microsoft.com/power-bi/service-gateway-onprem)kullanarak göz önünde bulundurun [ve önemli noktalara dikkat](https://docs.microsoft.com/power-bi/service-gateway-deployment-guidance#installation-considerations-for-the-on-premises-data-gateway)edin.

### <a name="configure-app-service-for-secure-connections-to-azure-sql-database"></a>Azure SQL veritabanı 'na güvenli bağlantılar için App Service yapılandırma

**En iyi uygulamalar**:

- Basit bir Web uygulaması için genel uç nokta üzerinden bağlantı kurmak için **Azure HIZMETLERININ** açık olması gerekir. 

- Yönetilen bir örneğe özel veri yolu bağlantısı için [uygulamanızı bir Azure sanal ağıyla tümleştirin](../app-service/web-sites-integrate-with-vnet.md) . İsteğe bağlı olarak, [App Service ortamları (Ao)](../app-service/environment/intro.md)Ile bir Web uygulaması da dağıtabilirsiniz. 

- ASE veya VNet tümleşik Web uygulamasıyla SQL veritabanı sunucusu 'ndaki bir veritabanına bağlanan Web uygulaması için, [VNET hizmet uç noktalarını ve VNET güvenlik duvarı kurallarını](sql-database-vnet-service-endpoint-rule-overview.md) kullanarak belirli bir VNET ve alt ağdan erişimi sınırlayabilir ve sonra **Azure hizmetlerinin kapalı çalışmasına izin verebilirsiniz** . Ayrıca, ASE 'yi bir özel veri yolu üzerinden yönetilen bir örneğe bağlayabilirsiniz.  

- Web uygulamanızın, [Azure App Service kullanarak PaaS Web ve mobil uygulamaları güvenli hale getirmek Için en iyi uygulamalar](../security/security-paas-applications-using-app-services.md)makalesine göre yapılandırıldığından emin olun. 

- Web uygulamanızı ortak güvenlik açıklarından ve güvenlik açıklarından korumak için [Web uygulaması güvenlik duvarını (WAF)](../application-gateway/waf-overview.md) yükler.

### <a name="configure-azure-vm-hosting-for-secure-connections-to-azure-sql-database"></a>Azure SQL veritabanı 'na yönelik güvenli bağlantılar için Azure VM barındırma yapılandırma

**En iyi uygulamalar**:

- VM 'den hangi bölgelere erişilebileceğini denetlemek için Azure VM 'lerinin NSG 'lerinde Izin verme ve reddetme kurallarının bir birleşimini kullanın.

- [Azure 'Da IaaS iş yükleri için En Iyi güvenlik uygulamaları](../security/azure-security-iaas.md)olan her makale için sanal makinenizin yapılandırıldığından emin olun.

- Tüm VM 'Lerin belirli bir sanal ağ ve alt ağ ile ilişkilendirildiğinden emin olun. 

- [Zorlamalı tünel oluşturma hakkında bilgi](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md#about-forced-tunneling)için 0.0.0.0/Internet varsayılan yoluna ihtiyacınız olup olmadığını değerlendirin. 
  - Evet ise (örneğin, ön uç alt ağı) varsayılan yolu koruyun.
  - Hayır – Örneğin, orta katman veya arka uç alt ağı – sonra, şirket içi (a. k. bir şirket içi) ulaşmak için Internet üzerinden hiçbir trafik kalmayacak şekilde Zorlamalı tünel özelliğini etkinleştirin. 

- Eşleme kullanıyorsanız veya şirket içi sunucuya bağlanıyorsanız [isteğe bağlı varsayılan yollar](../virtual-network/virtual-networks-udr-overview.md#optional-default-routes) uygulayın. 

- VNet 'teki tüm trafiği paket incelemesi için bir ağ sanal gerecine göndermeniz gerekiyorsa [Kullanıcı tanımlı yollar](../virtual-network/virtual-networks-udr-overview.md#user-defined) uygulayın. 

- Azure omurga ağı aracılığıyla Azure depolama gibi PaaS hizmetlerine güvenli erişim için [sanal ağ hizmet uç noktalarını](sql-database-vnet-service-endpoint-rule-overview.md) kullanın. 

### <a name="protect-against-distributed-denial-of-service-ddos-attacks"></a>Dağıtılmış hizmet reddi (DDoS) saldırılarına karşı koruma
Dağıtılmış hizmet reddi (DDoS) saldırıları, kötü niyetli bir kullanıcının Azure SQL veritabanı 'na, Azure altyapısını yoğun hale getirmeye ve geçerli oturum açmaları ve iş yükünü reddetmesine neden olan bir ağ trafiği yükünü bir şekilde göndermesinin denemeleridir.

> [!NOTE]
> Bahsedilen: OSA uygulaması #9

**Nasıl uygulanır**:

DDoS koruması, Azure platformunun bir parçası olarak otomatik olarak etkinleştirilir. Ortak uç noktalarda her zaman açık trafik izleme ve ağ düzeyinde saldırıları gerçek zamanlı olarak hafifletme bilgilerini içerir. 

- Sanal ağlarda dağıtılan kaynaklarla ilişkili genel IP adreslerini izlemek için [Azure DDoS koruması](../virtual-network/ddos-protection-overview.md) 'nı kullanın.

- Veritabanlarına karşı hizmet reddi (DoS) saldırılarını algılamak için [Azure SQL veritabanı Için Gelişmiş tehdit koruması](sql-database-threat-detection-overview.md) 'nı kullanın.

**En iyi uygulamalar**:

- [Saldırı yüzeyini en aza indirmek](#minimize-attack-surface) , DDoS saldırı tehditlerini en aza indirmenize yardımcı olur. 

- Gelişmiş tehdit koruması **deneme YANıLMA SQL kimlik bilgilerini zorla** uyarısı, deneme yanılma saldırılarını algılamaya yardımcı olur. Bazı durumlarda, uyarı, sızma testi iş yüklerini de ayırt edebilir. 

- SQL veritabanı 'na bağlanan Azure VM barındırma uygulamaları için: 
  - Azure Güvenlik Merkezi 'nde Internet 'e yönelik uç noktalar aracılığıyla erişimi kısıtlamak için öneriyi izleyin. 
  - Azure VM 'lerinde uygulamanızın birden çok örneğini çalıştırmak için sanal makine ölçek kümelerini kullanın. 
  - Deneme yanılma saldırısı oluşmasını engellemek için Internet 'ten RDP ve SSH 'yi devre dışı bırakın. 

## <a name="monitoring-logging-and-auditing"></a>İzleme, günlüğe kaydetme ve denetim  
Bu bölüm, veritabanları için olağan dışı ve zararlı olabilecek girişimleri belirten anormal etkinlikleri algılamanıza yardımcı olan özellikleri ifade eder. Ayrıca veritabanı olaylarını izlemek ve yakalamak üzere veritabanı denetimini yapılandırmak için en iyi yöntemleri açıklar.

### <a name="protect-databases-against-attacks"></a>Veritabanlarını saldırılara karşı koruma 
Gelişmiş tehdit koruması, anormal etkinliklerde güvenlik uyarıları sunarak meydana gelebilecek olası tehditleri algılamanıza ve bunlara yanıt vermenize olanak tanır.

**Nasıl uygulanır**:

- [SQL Için Gelişmiş tehdit koruması](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts) 'nı kullanarak veritabanlarına erişme veya bu veritabanlarına yararlanmaya yönelik olağan dışı ve olası zararlı girişimleri tespit etme:
  - SQL ekleme saldırısı.
  - Kimlik bilgileri hırsızlığı/sızıntısı.
  - Ayrıcalık kötüye kullanımı.
  - Veri kaybı.

**En iyi uygulamalar**:

- Belirli bir SQL veritabanı sunucusu veya yönetilen bir örnek için Azure SQL veritabanı için [Gelişmiş veri güvenliği](sql-database-advanced-data-security.md#getting-started-with-ads) veya [Azure Güvenlik Merkezi Standart katmanına](../security-center/security-center-pricing.md)geçerek bir abonelikteki tüm SQL veritabanı sunucuları ve yönetilen örnekler için yapılandırın. 

- Tam araştırma deneyimi için, [SQL veritabanı denetimini](sql-database-auditing.md) veritabanı olaylarını izlemek ve bir Azure depolama hesabındaki veya Azure Log Analytics çalışma alanındaki bir denetim günlüğüne yazmak için etkinleştirmeniz önerilir. 

### <a name="audit-critical-security-events"></a>Kritik güvenlik olaylarını denetleme
Veritabanı olaylarının izlenmesi, veritabanı etkinliğini anlamanıza yardımcı olur ve iş kaygılarını veya şüpheli güvenlik ihlallerini gösterebilen tutarsızlıklar ve bozukluklar elde edebilir. Ayrıca uyumluluk standartlarını sağlar ve kolaylaştırır. 

**Nasıl uygulanır**:

- Veritabanı olaylarını izlemek ve bunları Azure Depolama hesabınızdaki bir denetim günlüğüne yazmak için [SQL veritabanı denetimini](sql-database-auditing.md) etkinleştirin, Log Analytics çalışma alanı (Önizleme) veya Event Hubs (Önizleme). 

- Denetim günlükleri bir Azure depolama hesabına, Azure Izleyici günlükleri tarafından tüketim için bir Log Analytics çalışma alanına veya Olay Hub 'ı kullanılarak tüketim için Olay Hub 'ına yazılabilir. Bu seçeneklerin herhangi bir birleşimini yapılandırabilirsiniz ve denetim günlükleri her birine yazılır. 

**En iyi uygulamalar**:

- Veritabanı sunucunuzda denetim olayları için [SQL veritabanı denetimini](sql-database-auditing.md) yapılandırarak, bu sunucudaki tüm mevcut ve yeni oluşturulan veritabanları denetlenir.
- Varsayılan olarak denetim ilkesi, veritabanlarına karşı tüm eylemleri (sorgular, saklı yordamlar ve başarılı ve başarısız oturumlar) içerir ve bu da yüksek düzeyde denetim günlüklerine neden olabilir. Müşterilerin [PowerShell kullanarak farklı türlerde eylemler ve eylem grupları için denetim yapılandırması](sql-database-auditing.md#subheading-7), denetlenen eylemlerin sayısını denetlemesi ve olay kaybı riskini en aza indirmek için önerilir. Bu, müşterilerin yalnızca gerçekten gerekli denetim verilerini yakalamasına olanak tanır.
- Denetim günlükleri doğrudan [Azure Portal](https://portal.azure.com/)veya yapılandırılan depolama konumundan tüketilebilir. 


> [!NOTE]
> Log Analytics için denetimin etkinleştirilmesi, alma ücretlerine göre maliyet doğurur. Lütfen bu [seçeneği](https://azure.microsoft.com/pricing/details/monitor/)kullanarak ilişkili maliyetten haberdar olun veya denetim günlüklerini bir Azure depolama hesabında depolamayı düşünün. 

**Daha fazla kaynak**:

- [SQL veritabanı denetimi](sql-database-auditing.md)
- [SQL Server denetimi](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) 

### <a name="secure-audit-logs"></a>Güvenli denetim günlükleri
Görevlerin ayrılmasını ve DBA 'yı denetçilerin ayrı olarak desteklemek için, depolama hesabına erişimi kısıtlamak üzere önlemler almanız gerekir. 

**Nasıl uygulanır**:

- Denetim günlüklerini Azure Storage 'a kaydederken, depolama hesabına erişimi denetleme kullanılarak depolama hesabına erişimin en düşük güvenlik ilkelerine kısıtlı olduğundan emin olun.

- Daha fazla bilgi için bkz. [Azure depolama 'ya erişimi yetkilendirme](../storage/common/storage-auth.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

**En iyi uygulamalar**:

- Denetim hedefine erişimi denetlemek, DBA 'daki denetçilerin ayrıldıkları bir temel kavramdır. 

- Gizli verilere erişimi denetlerken, denetçi ile ilgili bilgi sızıntısını önlemek için veri şifreleme ile verilerin güvenliğini sağlamayı göz önünde bulundurun. Daha fazla bilgi için, [yüksek ayrıcalıklı, yetkisiz kullanıcılardan kullanılan hassas verileri koruma](#protect-sensitive-data-in-use-from-high-privileged-unauthorized-users)bölümüne bakın.

## <a name="security-management"></a>Güvenlik yönetimi

Bu bölümde, veritabanlarının güvenlik duruşunuzu yönetmek için farklı yönleri ve en iyi uygulamalar açıklanmaktadır. Veritabanlarınızı, veritabanlarınızdaki potansiyel olarak duyarlı verilere yönelik verileri bulmak ve sınıflandırmak için güvenlik standartlarını karşılayacak şekilde yapılandırmak için en iyi yöntemleri içerir. 

### <a name="ensure-that-the-databases-are-configured-to-meet-security-best-practices"></a>Veritabanlarının en iyi güvenlik uygulamalarını karşılayacak şekilde yapılandırıldığından emin olun 

Olası veritabanı güvenlik açıklarını bulup çözerek veritabanı güvenlerinizi önceden geliştirebilirsiniz.

**Nasıl uygulanır**:

- [SQL güvenlik açığı değerlendirmesini](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) (VA), veritabanınızı güvenlik sorunları açısından taramak ve veritabanlarınızdaki düzenli aralıklarla otomatik olarak çalıştırmak için etkinleştirin.

**En iyi uygulamalar**:

- Başlangıçta, veritabanlarınızda VA 'yı çalıştırın ve güvenlik açısından en iyi uygulamaları değiştiren başarısız denetimleri seçerek yineleyerek ve tarama ' temiz ' (tüm denetimler başarılı olana kadar kabul edilebilir yapılandırmalara yönelik taban çizgileri ayarlayarak) yineleyin.  

- Düzenli aralıklarla yinelenen taramaların haftada bir kez çalışacak şekilde yapılandırın ve ilgili kişiyi Özet e-postaları alacak şekilde yapılandırın. 

- Her haftalık taramanın ardından VA özetini gözden geçirin. Bulunan her türlü güvenlik açığı için, önceki tarama sonucundan DRFT 'yi değerlendirin ve denetimin çözülmesi gerekip gerekmediğini veya yapılandırmada değişiklik için meşru bir neden olup olmadığını saptayın.   

- İlgili yerlerde denetimleri ve güncelleştirme temellerini çözümleyin veya eylemleri çözümlemek için bilet öğeleri oluşturun ve bunlar çözümlenene kadar bunları izleyin. 

**Daha fazla kaynak**:

- [SQL güvenlik açığı değerlendirmesi](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) 
- [SQL güvenlik açığı değerlendirme hizmeti, veritabanı güvenlik açıklarını belirlemenize yardımcı olur](sql-vulnerability-assessment.md)

### <a name="identify-and-tag-sensitive-data"></a>Hassas verileri tanımla ve etiketleme 

Büyük olasılıkla hassas veriler içeren sütunları bulur. Sütunları, gelişmiş duyarlılık tabanlı denetim ve koruma senaryolarından yararlanmak için sınıflandırın. 

**Nasıl uygulanır**:

- Veritabanlarınızdaki hassas verileri bulmak, sınıflandırmak, etiketlemek ve korumak için [SQL veri bulma ve sınıflandırmasını](sql-database-data-discovery-and-classification.md) kullanın. 
  - SQL veri bulma ve sınıflandırma panosundaki otomatik bulma tarafından oluşturulan sınıflandırma önerilerini görüntüleyin ve duyarlı verilerinizin kalıcı olarak sınıflandırmayla etiketlenmesi gibi ilgili sınıflandırmaları kabul edin. etikete. 
  - Otomatik mekanizmaya bulunmayan ek gizli veri alanları için sınıflandırmaları el ile ekleyin. 
- Daha fazla bilgi için bkz. [SQL veri bulma & sınıflandırması](https://docs.microsoft.com/sql/relational-databases/security/sql-data-discovery-and-classification).

**En iyi uygulamalar**:

- Veritabanının sınıflandırma durumunun doğru değerlendirmesi için sınıflandırma panosunu düzenli olarak izleyin. Veritabanı sınıflandırması durumundaki bir rapor, uyumluluk ve denetim amaçlarıyla paylaşmak üzere aktarılabilir veya yazdırılabilir.

- Hassas veri bulma kuralını izleyerek ve sınıflandırma için önerilen sütunlarda herhangi bir DRI tanımlayarak, önerilen hassas verilerin durumunu sürekli olarak SQL güvenlik açığı değerlendirmesinde izleyin.  

- Sınıflandırmayı, kuruluşunuzun belirli ihtiyaçlarına göre uyarlanmış bir şekilde kullanın. Azure Güvenlik Merkezi 'nde [SQL Information Protection](../security-center/security-center-info-protection-policy.md) ilkesinde Information Protection ilkenizi (duyarlılık etiketleri, bilgi türleri, bulma mantığı) özelleştirin. 

### <a name="track-access-to-sensitive-data"></a>Gizli verilere erişimi izleme 
Gizli verilere erişen ve denetim günlüklerindeki gizli verilerdeki sorguları yakalayan izleyici.

**Nasıl uygulanır**:

- SQL denetim ve veri sınıflandırması birleşimini kullanın. 
  - [SQL veritabanı denetim](sql-database-auditing.md) günlüğlemenizde, erişimi gizli verilere özel olarak izleyebilir ve erişilen veriler gibi bilgileri ve duyarlık etiketini görüntüleyebilirsiniz (bkz. [gizli verilere erişimi denetleme](sql-database-data-discovery-and-classification.md#subheading-3)). 

**En iyi uygulamalar**:

- Bkz. denetim ve veri sınıflandırması bölümleri için en iyi uygulamalar: 
  - [Kritik güvenlik olaylarını denetleme](#audit-critical-security-events) 
  - [Hassas verileri tanımla ve etiketleme](#identify-and-tag-sensitive-data) 

### <a name="visualize-security-and-compliance-status"></a>Güvenlik ve uyumluluk durumunu görselleştirin 

Veri merkezlerinizin güvenlik duruşunu güçlendirir (SQL veritabanları dahil) Birleşik bir altyapı güvenliği yönetim sistemi kullanın. Veritabanlarının güvenliğine ve uyumluluk durumuna ilişkin önerilerin bir listesini görüntüleyin.

**Nasıl uygulanır**:

- [Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/services/security-center/)'nde SQL ile ilgili güvenlik önerilerini ve etkin tehditleri izleyin.

## <a name="common-security-threats-and-potential-mitigations"></a>Yaygın güvenlik tehditleri ve olası azaltmaları

Bu bölüm bazı saldırı vektörlerine karşı korumaya yönelik güvenlik önlemleri bulmanıza yardımcı olur. Yukarıdaki güvenlik yönergelerinden biri veya daha fazlası izlenerek en çok azaltmaları elde edilebilir.

### <a name="security-threat-data-exfiltration"></a>Güvenlik tehdidi: veri taşalımı

Veri tanımı, bir bilgisayardan veya sunucudan veri kopyalama, aktarma veya alma yetkisi yok. Vivon 'da [veri](https://en.wikipedia.org/wiki/Data_exfiltration) akışı için bir tanım görüntüleyin.

Azure SQL veritabanı sunucusuna genel bir uç nokta üzerinden bağlanmak, müşterilerin güvenlik duvarlarını ortak IP 'lere açmasını gerektirdiğinden veri sızdırma riskini gösterir.  

**Senaryo 1**: Azure VM 'deki bir uygulama, Azure SQL veritabanı sunucusundaki bir veritabanına bağlanır. Standart dışı bir aktör VM 'ye erişim sağlar ve uygulamayı bu şekilde alır. Bu senaryoda, veri alma işlemi, standart dışı VM kullanan bir dış varlığın veritabanına bağlanmasını, kişisel verileri kopyaladığını ve bir BLOB depolama alanında ya da farklı bir abonelikte farklı bir SQL veritabanını depoladığını gösterir.

**Senaryo 2**: bir kaba DBA. Bu senaryo, genellikle güvenliği önemli müşteriler tarafından düzenlenen sektörlerden oluşur. Bu senaryoda, yüksek ayrıcalıklı bir kullanıcı verileri Azure SQL veritabanından veri sahibi tarafından denetlenen başka bir aboneliğe kopyalayabilir.

**Olası azaltmaları**:

Bugün, Azure SQL veritabanı, veri azaltma tehditlerini azaltmak için aşağıdaki teknikleri sunmaktadır: 

- VM 'den hangi bölgelere erişilebileceğini denetlemek için Azure VM 'lerinin NSG 'lerinde Izin verme ve reddetme kurallarının bir birleşimini kullanın. 
- Azure SQL veritabanı sunucusu (Singleton veritabanları veya elastik havuzlar içeren) kullanıyorsanız, aşağıdakileri ayarlayın:
  - Azure hizmetlerinin kapalı çalışmasına izin verin.
  - Yalnızca VNet güvenlik duvarı kuralı ayarlayarak, Azure VM 'nizi içeren alt ağdan gelen trafiğe izin verin.
  - [Özel bağlantı](sql-database-private-endpoint-overview.md) kullan
- Yönetilen bir örnek için, özel IP erişiminin kullanılması standart dışı bir VM 'nin ilk veri savunma sorunu ele alınmıştır. Yönetilen bir örnek alt ağında en kısıtlayıcı ilkeyi otomatik olarak ayarlamak üzere hizmet verecek alt ağ üzerinde alt ağ temsili özelliğini açın.
- Bir yönetilen örnekle daha büyük bir yüzey alanı olduğu ve ağ gereksinimlerinin müşteriler tarafından görülebilmesi için standart olmayan DBA sorunu daha fazla açığa çıkarmıştır. Bunun için en iyi şekilde risk azaltma, bu güvenlik kılavuzundaki tüm uygulamaların ilk yerinde (yalnızca veri ayıklanma değil) standart dışı DBA senaryosunu önlemesini sağlar. Always Encrypted hassas verileri şifreleyerek korumak ve anahtarı DBA için erişilemez durumda tutmak için bir yöntemdir.

## <a name="security-aspects-of-business-continuity-and-availability"></a>İş sürekliliği ve kullanılabilirliğinin güvenlik yönleri

Çoğu güvenlik standardı, tek hata noktalarından kaçınmak için artıklık ve yük devretme özellikleri uygulayarak gerçekleştirilen işlemsel sürekliliği açısından veri kullanılabilirliğini ele geçirir. Olağanüstü durum senaryolarında verilerin ve günlük dosyalarının yedeklerini tutan yaygın bir uygulamadır. Aşağıdaki bölümde, Azure 'da yerleşik olan özelliklere ve belirli ihtiyaçları karşılamak üzere yapılandırılabilecek ek seçeneklere yönelik yüksek düzeyde bir genel bakış sunulmaktadır: 

- Azure, yerleşik yüksek kullanılabilirlik sağlar: [yüksek kullanılabilirlik ve Azure SQL veritabanı](sql-database-high-availability.md) 

- İş Açısından Kritik katmanı, yük devretme grupları, çoklu kullanılabilirlik alanları, tam ve fark günlüğü yedeklemeleri ve varsayılan olarak etkin olan noktadan sonra geri yükleme yedeklemelerini içerir:  
  - [Yüksek kullanılabilirlik ve Azure SQL veritabanı yedekli yapılandırma](sql-database-high-availability.md#zone-redundant-configuration)
  - [Otomatik yedeklemeler](sql-database-automated-backups.md)
  - [Otomatik veritabanı yedeklemeleri kullanarak bir Azure SQL veritabanını kurtarma-zaman içinde geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

- Farklı Azure coğrafyalar genelindeki otomatik yük devretme grupları gibi ek iş sürekliliği özellikleri, burada açıklandığı gibi yapılandırılabilir: [Azure SQL veritabanı ile iş sürekliliği 'ne genel bakış](sql-database-business-continuity.md)

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure SQL veritabanı güvenlik özelliklerine genel bakış](sql-database-security-overview.md)