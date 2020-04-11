---
title: Ortak Güvenlik Gereksinimlerini Ele Almak Için Oyun Kitabı | Microsoft Dokümanlar
titleSuffix: Azure SQL Database
description: Bu makalede, Azure SQL Veritabanı'nda ortak güvenlik gereksinimleri ve en iyi uygulamalar sağlar.
ms.service: sql-database
ms.subservice: security
author: VanMSFT
ms.author: vanto
ms.topic: article
ms.date: 02/20/2020
ms.reviewer: ''
ms.openlocfilehash: 7b3a223ca504bff380afad54afda73880717814f
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115380"
---
# <a name="playbook-for-addressing-common-security-requirements-with-azure-sql-database"></a>Azure SQL Veritabanı ile Ortak Güvenlik Gereksinimlerini Ele Almak Için Oyun Kitabı

> [!NOTE]
> Bu belge, ortak güvenlik gereksinimlerinin nasıl çözüleceğinin en iyi uygulamalarını sağlar. Tüm gereksinimler tüm ortamlar için geçerli değildir ve uygulanacak özellikleri veritabanınıza ve güvenlik ekibinize danışmalısınız.

## <a name="solving-common-security-requirements"></a>Ortak güvenlik gereksinimlerini çözme

Bu belge, Azure SQL Veritabanı'nı kullanarak yeni veya varolan uygulamalar için ortak güvenlik gereksinimlerinin nasıl çözüleceğiniz hakkında kılavuz sağlar. Üst düzey güvenlik bölgeleri tarafından organize edilmiş. Belirli tehditleri ele almak için [Ortak güvenlik tehditleri ve olası azaltıcı etkenler](#common-security-threats-and-potential-mitigations) bölümüne bakın. Sunulan önerilerden bazıları, uygulamaları şirket içinden Azure'a geçirilirken uygulanabilir olsa da, geçiş senaryoları bu belgenin odak noktası değildir.

### <a name="azure-sql-database-deployment-offers-covered-in-this-guide"></a>Azure SQL Veritabanı dağıtım teklifleri bu kılavuzda yer almaktadır

- [SQL Veritabanları](https://docs.microsoft.com/azure/sql-database/sql-database-single-index): Azure SQL Veritabanı [sunucularında](sql-database-servers.md) tek [veritabanları](sql-database-single-database.md) ve [elastik havuzlar](sql-database-elastic-pool.md)
- [Yönetilen örnekler](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index)

### <a name="sql-deployment-offers-not-covered-in-this-guide"></a>SQL dağıtım teklifleri bu kılavuzda kapsanmıyor

- Azure SQL Veri Ambarı
- Azure SQL VM'ler (IaaS)
- SQL Server şirket içi

### <a name="audience"></a>Hedef kitle

Bu kılavuzun hedef kitlesi, Azure SQL Veritabanı'nın nasıl güvenli hale ne kadar güvenli hale ne kadar güvene ne kadar güvene ne kadar güvene ne kadar güvenene ilişkin sorularla karşı karşıya kalan müşterilerdir. Bu en iyi uygulama makalesiile ilgilenen roller şunlardır, ancak bunlarla sınırlı değildir:

- Güvenlik Mimarları
- Güvenlik Yöneticileri
- Uyum Görevlileri
- Gizlilik Görevlileri
- Güvenlik Mühendisleri

### <a name="using-this-guide"></a><a id="using"></a>Bu kılavuzu kullanma

Bu belge, mevcut [Azure SQL Veritabanı güvenlik](sql-database-security-overview.md) belgelerimize eşlik etmek için tasarlanmıştır.

Aksi belirtilmedikçe, ilgili hedefe veya gereksinime ulaşmak için her bölümde listelenen tüm en iyi uygulamaları izlemenizi öneririz. Belirli güvenlik uyumluluk standartlarını veya en iyi uygulamaları karşılamak için, önemli mevzuata uygunluk denetimleri, uygun olan her yerde Gereksinimler veya Hedefler bölümünde listelenir. Bu makalede atıfta bulunulan güvenlik standartları ve düzenlemeleri şunlardır:

- [FedRAMP](https://www.fedramp.gov/documents/): AC-04, AC-06
- [SOK](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/sorhome.html): CM-3, SDL-3
- [ISO/IEC 27001](https://www.iso27001security.com/html/27001.html): Erişim Kontrolü, Şifreleme
- [Microsoft Operasyonel Güvenlik Güvencesi (OSA) uygulamaları](https://www.microsoft.com/en-us/securityengineering/osa/practices): Uygulama #1-6 ve #9
- [NIST Özel Yayın 800-53 Güvenlik Kontrolleri](https://nvd.nist.gov/800-53): AC-5, AC-6
- [PCI DSS](https://www.pcisecuritystandards.org/document_library): 6.3.2, 6.4.2

### <a name="feedback"></a>Geri Bildirim

Burada listelenen önerileri ve en iyi uygulamaları güncellemeye devam etmeyi planlıyoruz. Bu makalenin altındaki **Geri Bildirim** bağlantısını kullanarak bu belge için giriş veya düzeltme sağlayın.

## <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulama, kullanıcının iddia ettikleri kişi olduğunu kanıtlama işlemidir. Azure SQL Veritabanı iki tür kimlik doğrulamayı destekler:

- SQL kimlik doğrulaması
- Azure Active Directory kimlik doğrulaması

> [!NOTE]
> Azure Active Directory kimlik doğrulaması tüm araçlar ve üçüncü taraf uygulamaları için desteklenmeyebilir.

### <a name="central-management-for-identities"></a>Kimlikler için merkezi yönetim

Merkezi kimlik yönetimi aşağıdaki avantajları sunar:

- Azure SQL Veritabanı sunucuları ve veritabanlarında oturum açmaları çoğaltmadan grup hesaplarını yönetin ve kullanıcı izinlerini denetleyin.
- Basitleştirilmiş ve esnek izin yönetimi.
- Uygulamaların ölçekte yönetilmesi.

**Nasıl uygulanır:**

- Merkezi kimlik yönetimi için Azure Etkin Dizin (Azure AD) kimlik doğrulamasını kullanın.

**En iyi uygulamalar:**

- Bir Azure AD kiracıoluşturun ve insan kullanıcıları temsil edecek [kullanıcılar oluşturun](../active-directory/fundamentals/add-users-azure-active-directory.md) ve uygulamaları, hizmetleri ve otomasyon araçlarını temsil edecek [hizmet ilkeleri](../active-directory/develop/app-objects-and-service-principals.md) oluşturun. Hizmet ilkeleri, Windows ve Linux'taki hizmet hesaplarına eşdeğerdir. 

- Grup ataması yoluyla kaynakların kaynaklarının erişim haklarını Azure REKLAM ilkelerine atama: Azure REKLAM grupları oluşturun, gruplara erişim izni ver ve gruplara tek tek üyeler ekleyin. Veritabanınızda, Azure REKLAM gruplarınızı eşleyen içerdiği veritabanı kullanıcıları oluşturun. İzinleri veritabanına atamak için, Azure REKLAM gruplarınızla ilişkili kullanıcıları uygun izinlerle veritabanı rollerine koyun.
  - Makaleleri görün, [Azure Active Directory kimlik doğrulamasını SQL ile yapılandırın ve yönetin](sql-database-aad-authentication-configure.md) ve [SQL ile kimlik doğrulama için Azure AD'yi kullanın.](sql-database-aad-authentication.md)
  > [!NOTE]
  > Yönetilen bir durumda, ana veritabanında Azure AD ilkelerine eşleyen oturum açmalar da oluşturabilirsiniz. Bkz. [CREATE Gİrİş (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current).

- Azure AD gruplarını kullanmak, izin yönetimini ve hem grup sahibini basitleştirir hem de kaynak sahibi gruba üye ekleyebilir/kaldırabilir. 

- Her SQL DB sunucusu için Azure AD yöneticileri için ayrı bir grup oluşturun.

  - Makaleye bakın, [Azure SQL Veritabanı sunucunuz için bir Azure Etkin Dizin yöneticisi sağlama](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server).

- Azure AD denetim etkinlik raporlarını kullanarak Azure REKLAM grubu üyelik değişikliklerini izleyin. 

- Yönetilen bir örnek için, Azure AD yöneticisi oluşturmak için ayrı bir adım gerekir. 
  - Makaleye bakın, [Yönetilen örneğiniz için bir Azure Etkin Dizin yöneticisi sağlama](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance). 

> [!NOTE]
> - Azure AD kimlik doğrulaması Azure SQL denetim günlüklerinde kaydedilir, ancak Azure AD oturum açma günlüklerinde kaydedilmez.
> - Azure'da verilen RBAC izinleri Azure SQL DB izinleri için geçerli değildir. Bu tür izinler, sql DB'de varolan SQL izinleri kullanılarak el ile oluşturulmalı/eşlenmelidir.
> - İstemci tarafında, Azure AD kimlik doğrulaması için Internet'e veya Kullanıcı Tanımlı Rota (UDR) üzerinden VNet'e erişim gerekir.
> - Azure AD erişim belirteci istemci tarafında önbelleğe alınmış ve kullanım ömrü belirteç yapılandırmasına bağlıdır. Makaleye bakın, [Azure Etkin Dizini'nde yapılandırılabilir belirteç ömürleri](../active-directory/develop/active-directory-configurable-token-lifetimes.md)
> - Azure AD Kimlik Doğrulama sorunlarıyla ilgili sorun giderme kılavuzu için aşağıdaki bloga bakın:<https://techcommunity.microsoft.com/t5/azure-sql-database/troubleshooting-problems-related-to-azure-ad-authentication-with/ba-p/1062991>

### <a name="multi-factor-authentication-mfa"></a>Multi-Factor Authentication (MFA)

> Bahsedilen: OSA Uygulama #2, ISO Erişim Kontrolü (AC)

Azure Çok Faktörlü Kimlik Doğrulama (MFA), birden fazla kimlik doğrulama biçimi gerektirerek ek güvenlik sağlar.

**Nasıl uygulanır:**

- Koşullu Erişim'i kullanarak Azure AD'da [MFA'yı etkinleştirin](../active-directory/authentication/concept-mfa-howitworks.md) ve etkileşimli kimlik doğrulaması kullanın. 

- Alternatif, Tüm Azure AD veya AD etki alanı için MFA'yı etkinleştirmektir.

**En iyi uygulamalar:**

- Azure AD'de Koşullu Erişimi etkinleştirin (Premium abonelik gerektirir). 
  - Makaleye bakın, [Azure AD'de Koşullu Erişim.](../active-directory/conditional-access/overview.md)  

- Azure AD grubu(lar) oluşturun ve Azure AD Koşullu Erişimi kullanarak seçili gruplar için MFA ilkesini etkinleştirin. 
  - Makaleye bakın, [Koşullu Erişim Dağıtım Planı](../active-directory/conditional-access/plan-conditional-access.md). 

- MFA, Azure AD'nin tamamı veya Azure AD ile beslenen tüm Active Directory için etkinleştirilebilir. 

- Parolanın etkileşimli olarak istendiği SQL DB için Azure AD Etkileşimli kimlik doğrulama modunu ve ardından MFA kimlik doğrulaması'nı kullanın:      
  - SSMS'te Evrensel Kimlik Doğrulama'yı kullanın. Azure SQL [Veritabanı ve Azure SQL Veri Ambarı (MFA için SSMS desteği) ile çok faktörlü AAD kimlik doğrulamasını kullanma](sql-database-ssms-mfa-authentication.md)makalesine bakın.
  - SQL Server Veri Araçları (SSDT) desteklenen Etkileşimli Kimlik Doğrulama'yı kullanın. Sql Server [Veri Araçları (SSDT) makaleye, Azure Active Directory desteğine](https://docs.microsoft.com/sql/ssdt/azure-active-directory?view=azuresqldb-current)bakın.
  - MFA'yı destekleyen diğer SQL araçlarını kullanın. 
    - Dışa aktarma/ayıklama/dağıtma veritabanı için SSMS Sihirbazı desteği  
    - [sqlpackage.exe](https://docs.microsoft.com/sql/tools/sqlpackage): seçenek '/ua' 
    - [sqlcmd Utility](https://docs.microsoft.com/sql/tools/sqlcmd-utility): seçenek -G (interaktif)
    - [bcp Yardımcı Program](https://docs.microsoft.com/sql/tools/bcp-utility): seçenek -G (etkileşimli) 

- MFA desteği ile etkileşimli kimlik doğrulamasını kullanarak Azure SQL Veritabanı'na bağlanmak için uygulamalarınızı uygulayın. 
  - Makaleye bakın, [Azure Çok Faktörlü Kimlik Doğrulama ile Azure SQL Veritabanına Bağlanın.](active-directory-interactive-connect-azure-sql-db.md) 
  > [!NOTE]
  > Bu kimlik doğrulama modu kullanıcı tabanlı kimlikler gerektirir. Tek tek Azure AD kullanıcı kimlik doğrulamasını atlayan güvenilir bir kimlik modelinin kullanıldığı durumlarda (örneğin, Azure kaynakları için yönetilen kimliğin kullanılması), MFA geçerli değildir.

### <a name="minimize-the-use-of-password-based-authentication-for-users"></a>Kullanıcılar için parola tabanlı kimlik doğrulama kullanımını en aza indirmek

> Bahsedilen: OSA Uygulama #4, ISO Erişim Kontrolü (AC)

Parola tabanlı kimlik doğrulama yöntemleri, kimlik doğrulamanın daha zayıf bir biçimidir. Kimlik bilgileri tehlikeye veya yanlışlıkla uzağa verilebilir.

**Nasıl uygulanır:**

- Parola kullanımını ortadan kaldıran Azure AD tümleşik kimlik doğrulaması kullanın.

**En iyi uygulamalar:**

- Windows kimlik bilgilerini kullanarak tek oturum açma kimlik doğrulaması kullanın. Azure AD ile şirket içi AD etki alanını feclendirin ve Tümleşik Windows kimlik doğrulamasını kullanın (Azure AD'ye sahip etki alanı birleştirilmiş makineler için).
  - Makaleye bakın, [Azure AD Tümleşik kimlik doğrulaması için SSMS desteği.](sql-database-aad-authentication-configure.md#active-directory-integrated-authentication)

### <a name="minimize-the-use-of-password-based-authentication-for-applications"></a>Uygulamalar için parola tabanlı kimlik doğrulama kullanımını en aza indirmek 

> Bahsedilen: OSA Uygulama #4, ISO Erişim Kontrolü (AC)

**Nasıl uygulanır:**

- Azure Yönetilen Kimliği etkinleştirin. Tümleşik veya sertifika tabanlı kimlik doğrulaması da kullanabilirsiniz. 

**En iyi uygulamalar:**

- [Azure kaynakları için yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/overview.md)kullanın.
  - [Sistem le atanmış yönetilen kimlik](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-sql.md) 
  - [Kullanıcı tarafından atanan yönetilen kimlik](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
  - [Yönetilen kimliğe sahip uygulama hizmetinden Azure SQL Veritabanı'nı kullanma (kod değişikliği olmadan)](https://github.com/Azure-Samples/app-service-msi-entityframework-dotnet)

- Bir uygulama için sertifika tabanlı kimlik doğrulamasını kullanın. 
  - Bu [kod örneğine](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/token)bakın. 

- Tümleşik federe etki alanı ve etki alanı birleştirilmiş makine için Azure AD kimlik doğrulamasını kullanın (yukarıdaki bölüme bakın).
  - [Tümleşik kimlik doğrulama için örnek uygulamaya](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth/integrated)bakın.

### <a name="protect-passwords-and-secrets"></a>Parolaları ve sırları koruyun

Parolaların önlenebileceği durumlarda, güvenli olduğundan emin olun.

**Nasıl uygulanır:**

- Parolaları ve sırları depolamak için Azure Key Vault'u kullanın. Uygun olduğunda, Azure AD kullanıcılarıyla birlikte Azure SQL Veritabanı için MFA'yı kullanın.

**En iyi uygulamalar:**

- Parolalardan veya sırlardan kaçınmak mümkün değilse, kullanıcı parolalarını ve uygulama sırlarını Azure Key Vault'ta saklayın ve Key Vault erişim ilkeleri aracılığıyla erişimi yönetin. 

- Çeşitli uygulama geliştirme çerçeveleri, uygulamadaki sırları korumak için çerçeveye özgü mekanizmalar da sunabilir. Örneğin: [ASP.NET çekirdek uygulaması](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows).

### <a name="use-sql-authentication-for-legacy-applications"></a>Eski uygulamalar için SQL kimlik doğrulamasını kullanma 

SQL kimlik doğrulaması, kullanıcı adı ve parola kullanarak Azure SQL Veritabanı'na bağlanırken kullanıcının kimlik doğrulaması anlamına gelir. Her SQL Veritabanı sunucusunda veya yönetilen bir örnekte ve her veritabanında oluşturulan bir kullanıcıda bir giriş oluşturulması gerekir.

**Nasıl uygulanır:**

- SQL kimlik doğrulamasını kullanın.

**En iyi uygulamalar:**

- Sunucu yöneticisi olarak, oturum açma ve kullanıcılar oluşturun. Parolaları olan içerdiği veritabanı kullanıcıları kullanmadığı sürece, tüm parolalar ana veritabanında depolanır.
  - SQL Veritabanı [ve SQL Veri Ambarı](sql-database-manage-logins.md)veritabanı erişimi denetleme ve verme makaleye bakın.

## <a name="access-management"></a>Erişim yönetimi

Erişim yönetimi (Yetkilendirme olarak da adlandırılır), yetkili kullanıcıların Azure SQL Veritabanı'na erişimini ve ayrıcalıklarını denetleme ve yönetme işlemidir.

### <a name="implement-principle-of-least-privilege"></a>En az ayrıcalık ilkesini uygulayın

> Bahsedilen: FedRamp kontrolAC-06, NIST: AC-6, OSA Uygulama #3

En az ayrıcalık ilkesi, kullanıcıların görevlerini tamamlamak için gerekenden daha fazla ayrıcalığa sahip olmaması gerektiğini belirtir. Daha fazla bilgi için, [makaleye](https://docs.microsoft.com/powershell/scripting/learn/remoting/jea/overview)bakın Yeterli yönetim .

**Nasıl uygulanır:**

Yalnızca gerekli görevleri tamamlamak için gerekli [izinleri](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) atayın:

- SQL Veritabanlarında: 
    - Parçalı izinleri ve kullanıcı tanımlı veritabanı rollerini (veya MI'deki sunucu rollerini) kullanın: 
        1. Gerekli rolleri oluşturma
            - [ROL OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-role-transact-sql)
            - [SUNUCU ROLÜ OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-server-role-transact-sql)
        1. Gerekli kullanıcıları oluşturma
            - [KULLANıCı OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql)
        1. Rolleri üye olarak ekleme 
            - [ROLÜ DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-role-transact-sql)
            - [SUNUCU ROLÜNÜ DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-server-role-transact-sql)
        1. Ardından rollere izin atayın. 
            - [Grant](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql) 
    - Kullanıcıları gereksiz rollere atamadığından emin olun.

- Azure Kaynak Yöneticisi'nde:
  - Varsa yerleşik rolleri veya özel RBAC rollerini kullanın ve gerekli izinleri atayın.
    - [Azure için yerleşik roller](../role-based-access-control/built-in-roles.md) 
    - [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md)

**En iyi uygulamalar:**

Aşağıdaki en iyi uygulamalar isteğe bağlıdır, ancak güvenlik stratejinizin daha iyi yönetilebilirliği ve desteklenebilirliği ile sonuçlanır: 

- Mümkünse, mümkün olan en az izin kümesiyle başlayın ve gerçek bir gereklilik (ve gerekçe) varsa izinleri tek tek eklemeye başlayın – tam tersi yaklaşımın aksine: izinleri adım adım uzaklaştırmak. 

- Tek tek kullanıcılara izin atamaktan kaçının. Bunun yerine rolleri (veritabanı veya sunucu rolleri) tutarlı bir şekilde kullanın. Roller, raporlama ve sorun giderme izinleri konusunda büyük ölçüde yardımcı olur. (Azure RBAC yalnızca roller aracılığıyla izin atamayı destekler.) 

- Gereken tam izinlerle özel roller oluşturun ve kullanın. Uygulamada kullanılan tipik roller: 
  - Güvenlik dağıtımı 
  - Yönetici 
  - Geliştirici 
  - Destek personeli 
  - Denetçi 
  - Otomatik leştirilmiş süreçler 
  - Son kullanıcı 
  
- Yerleşik rolleri yalnızca rollerin izinleri kullanıcı için gereken izinlerle tam olarak eşleştiğinde kullanın. Kullanıcıları birden çok rol için atayabilirsiniz. 

- SQL Server Database Engine'deki izinlerin aşağıdaki kapsamlarda uygulanabileceğini unutmayın. Kapsam ne kadar küçükse, verilen izinlerin etkisi o kadar küçük türdedir: 
  - Azure SQL Veritabanı sunucusu (ana veritabanında özel roller) 
  - Veritabanı 
  - Şema
      - Şemaları veritabanı içinde izin vermek için kullanmak en iyi yöntemdir. (ayrıca bakınız: [SQL Server için şema tasarımı: güvenlik göz önünde bulundurularak Şema tasarımı için öneriler)](http://andreas-wolter.com/en/schema-design-for-sql-server-recommendations-for-schema-design-with-security-in-mind/)
  - Nesne (tablo, görünüm, yordam, vb.) 
  > [!NOTE]
  > Bu düzey genel uygulamaya gereksiz karmaşıklık kattığıiçin izinlerin nesne düzeyinde uygulanması önerilmez. Nesne düzeyinde izinleri kullanmaya karar verirseniz, bunlar açıkça belgelenmelidir. Aynı durum, aynı nedenlerden dolayı daha az tavsiye edilen sütun düzeyindeki izinler için de geçerlidir. Ayrıca varsayılan olarak bir tablo düzeyi [DENY](https://docs.microsoft.com/sql/t-sql/statements/deny-object-permissions-transact-sql) sütun düzeyinde GRANT geçersiz kılmaz unutmayın. Bu, [ortak ölçütuyumluluğu Sunucu](https://docs.microsoft.com/sql/database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option) Yapılandırması'nın etkinleştirilmesini gerektirir.

- Çok fazla izin için test etmek için [Güvenlik Açığı Değerlendirmesi 'ni (VA)](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) kullanarak düzenli denetimler gerçekleştirin.

### <a name="implement-separation-of-duties"></a>Görev Ayrımını Uygulayın

> Bahsedilen: FedRamp: AC-04, NIST: AC-5, ISO: A.6.1.2, PCI 6.4.2, SOC: CM-3, SDL-3

Görevlerin Ayrılması olarak da adlandırılan Görevlerin Ayrılması, hassas görevleri farklı kullanıcılara atanan birden çok göreve bölme gereksinimini açıklar. Görevlerin Ayrılması veri ihlallerini önlemeye yardımcı olur.

**Nasıl uygulanır:**

- Görevlerin Ayrılması için gerekli düzeyi belirleyin. Örnekler: 
  - Geliştirme/Test ve Üretim ortamları arasında 
  - Güvenlik açısından hassas görevler vs Veritabanı Yöneticisi (DBA) yönetim düzeyi görevleri vs geliştirici görevleri. 
    - Örnekler: Denetçi, Rol Düzeyi Güvenlik (RLS) için güvenlik ilkesi oluşturulması, DDL izinleri ile SQL Veritabanı nesneleri uygulama.

- Sisteme erişen kullanıcıların (ve otomatikleştirilmiş işlemlerin) kapsamlı bir hiyerarşisini tanımlayın.

- Gerekli kullanıcı gruplarına göre roller oluşturun ve rollere izinler atayın. 
  - Azure portalında veya PowerShell otomasyonu üzerinden yönetim düzeyinde görevler için RBAC rollerini kullanın. Gereksinimle eşleşen yerleşik bir rol bulun veya kullanılabilir izinleri kullanarak özel bir RBAC rolü oluşturun 
  - Yönetilen bir durumda sunucu genelindeki görevler (yeni oturum açmalar, veritabanları oluşturma) için Sunucu rolleri oluşturun. 
  - Veritabanı düzeyindeki görevler için Veritabanı Rolleri oluşturun.

- Belirli hassas görevler için, görevleri kullanıcılar adına yürütmek için sertifika tarafından imzalanmış özel depolanmış yordamlar oluşturmayı düşünün. Dijital olarak imzalanmış depolanmış yordamların önemli bir avantajı, yordam değiştirilirse, yordamın önceki sürümüne verilen izinlerin hemen kaldırılmasıdır.
  - Örnek: [Öğretici: Depolanan Yordamları Sertifikayla İmzalama](https://docs.microsoft.com/sql/relational-databases/tutorial-signing-stored-procedures-with-a-certificate) 

- Veri sahibi ve güvenlik sahibi arasında Görev Ayrımını etkinleştirmek için Azure Anahtar Kasası'nda müşteri tarafından yönetilen anahtarlarla Saydam Veri Şifreleme (TDE) uygulayın. 
  - Makaleye bakın, [Azure portalından Azure Depolama şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırın.](../storage/common/storage-encryption-keys-portal.md) 

- Bir DBA'nın son derece hassas kabul edilen verileri görememesi ve DBA görevlerini hala yapabilmesini sağlamak için, her zaman rol ayrımıyla Şifrelenmiş'i kullanabilirsiniz. 
  - Makalelere bakın, [Her Zaman Şifrelenmiş Anahtar Yönetimine Genel Bakış](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted), Rol Ayrımı ile Anahtar Sağlama ve Rol [Ayırma](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell#KeyProvisionWithRoles) [ile Sütun Yöneticisi Anahtar Döndürme](https://docs.microsoft.com/sql/relational-databases/security/encryption/rotate-always-encrypted-keys-using-powershell#column-master-key-rotation-with-role-separation). 

- Her Zaman Şifrelenmiş kullanımının mümkün olmadığı durumlarda, ya da en azından sistemi kullanılamaz hale getirebilecek büyük maliyetler ve çabalar olmadan, aşağıdaki ler gibi telafi edici kontroller aracılığıyla tavizler verilebilir ve azaltılabilir: 
  - Süreçlere insan müdahalesi. 
  - Denetim izleri – Denetim hakkında daha fazla [Audit critical security events](#audit-critical-security-events)bilgi için bkz.

**En iyi uygulamalar:**

- Geliştirme/Test ve Üretim ortamları için farklı hesapların kullanıldığından emin olun. Farklı hesaplar Test & Üretim sistemlerinin ayrılmasına uymaya yardımcı olur.

- Tek tek kullanıcılara izin atamaktan kaçının. Bunun yerine rolleri (veritabanı veya sunucu rolleri) tutarlı bir şekilde kullanın. Rollere sahip olmak, raporlama ve sorun giderme izinleri konusunda büyük ölçüde yardımcı olur.

- İzinler tam olarak gerekli izinlerle eşleştiğinde yerleşik rolleri kullanın ( birden çok yerleşik roldeki tüm izinlerin birleşmesi %100 eşleşmeye yol açıyorsa, aynı anda birden çok rol atayabilirsiniz. 

- Yerleşik roller çok fazla izin verdiğinde veya yetersiz izinverdiğinde kullanıcı tarafından tanımlanan roller oluşturun ve kullanın. 

- Rol atamaları, T-SQL'deki SQL Agent Job adımları içinde veya RBAC rolleri için Azure PIM'yi kullanarak, Dinamik Görev Ayrımı (DSD) olarak da bilinen geçici olarak da yapılabilir. 

- DKA'ların şifreleme anahtarlarına veya anahtar depolarına erişimi olmadığından ve anahtarlara erişimi olan Güvenlik Yöneticilerinin veritabanına erişime sahip olmadığından emin olun. [Genişletilebilir Anahtar Yönetimi 'nin (EKM)](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-ekm) kullanımı bu ayrımın sağlanmasını kolaylaştırabilir. [Azure Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) EKM'i uygulamak için kullanılabilir. 

- Güvenlikle ilgili eylemler için her zaman bir Denetim izi olduğundan emin olun. 

- Kullanılan izinleri görmek ve PowerShell üzerinden bunların alıntılar ve kümülasyonlarına dayalı özel bir rol oluşturmak için yerleşik RBAC rollerinin tanımını alabilirsiniz.

- db_owner veritabanı rolünün herhangi bir üyesi Saydam Veri Şifreleme (TDE) gibi güvenlik ayarlarını değiştirebileceğinden veya SLO'yu değiştirebildiği için, bu üyeliğe özenle verilmelidir. Ancak, db_owner ayrıcalıkları gerektiren birçok görev vardır. DB seçeneklerini değiştirme gibi herhangi bir veritabanı ayarını değiştirme gibi görev. Denetim herhangi bir çözümde önemli bir rol oynar.

- Bir db_owner izinlerini kısıtlamak ve bu nedenle yönetim hesabının kullanıcı verilerini görüntülemesini engellemek mümkün değildir. Bir veritabanında son derece hassas veriler varsa, her zaman şifrelenmiş db_owners veya başka bir DBA'nın bu verileri görüntülemesini güvenli bir şekilde önlemek için kullanılabilir.

> [!NOTE]
> Görevlerin Ayrılmasını (SSD) gerçekleştirmek, güvenlikle ilgili görevler veya sorun giderme görevleri için zordur. Geliştirme ve son kullanıcı rolleri gibi diğer alanları ayırmak daha kolaydır. Uyumlulukla ilgili denetimlerin çoğu, diğer çözümler pratik olmadığında Denetim gibi alternatif denetim işlevlerinin kullanılmasına izin verir.

SoD'ye daha derindin isteyen okuyucular için aşağıdaki kaynakları öneririz: 

- Azure SQL Veritabanı için:  
  - [SQL Veritabanı ve SQL Veri Ambarı veritabanı erişimini denetleme ve verme](sql-database-manage-logins.md)
  - [Uygulama Geliştiricisi Için Motor Görev Ayırma](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/cc974525(v=sql.100)) 
  - [SQL Server 2014'te Görev Ayrılığı](https://www.microsoft.com/download/details.aspx?id=39269)
  - [SQL Server'da Saklı Yordam İmzalama](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server)

- Azure Kaynak Yönetimi için:
  - [Azure için yerleşik roller](../role-based-access-control/built-in-roles.md) 
  - [Azure kaynakları için özel roller](../role-based-access-control/custom-roles.md)
  - [Yüksek erişim için Azure AD Ayrıcalıklı Kimlik Yönetimi'ni kullanma](https://www.microsoft.com/en-us/itshowcase/using-azure-ad-privileged-identity-management-for-elevated-access)

### <a name="perform-regular-code-reviews"></a>Düzenli kod incelemeleri gerçekleştirin

> Bahsedilen: PCI: 6.3.2, SOC: SDL-3 

Görevlerin Ayrılması veritabanındaki verilerle sınırlı değildir, ancak uygulama kodunu içerir. Kötü amaçlı kod, güvenlik denetimlerini atlatma potansiyeline olabilir. Üretime özel kod dağıtmadan önce, dağıtılan kodu gözden geçirmek önemlidir.

**Nasıl uygulanır:**

- Azure Veri Stüdyosu gibi kaynak denetimini destekleyen bir veritabanı aracı kullanın. 

- Ayrılmış kod dağıtım işlemi uygulayın.

- Ana şubeye bağlanmadan önce, bir kişi (kodun kendisi dışında) dolandırıcılık ve sahte erişime karşı korumak için ayrıcalıkrisklerinin yanı sıra kötü amaçlı veri modifikasyonlarının olası yükseltilmesi için kodu incelemek zorundadır. Bu kaynak kontrol mekanizmaları kullanılarak yapılabilir.

**En iyi uygulamalar:**

- Standartlaştırma: Herhangi bir kod güncelleştirmesi için izlenecek standart bir yordamın uygulanmasına yardımcı olur. 

- Güvenlik Açığı Değerlendirmesi, aşırı izinleri, eski şifreleme algoritmalarının kullanımını ve veritabanı şemasındaki diğer güvenlik sorunlarını denetleyen kurallar içerir. 

- SQL enjeksiyonuna karşı savunmasız kodu tarayan Gelişmiş Tehdit Koruması kullanılarak bir QA veya test ortamında daha fazla denetim yapılabilir.

- Dikkat etmesi gerekenlere örnekler: 
  - Otomatik bir SQL kodu güncelleştirme dağıtımı nın içinden bir kullanıcı oluşturulması veya güvenlik ayarlarını değiştirme. 
  - Sağlanan parametrelere bağlı olarak, bir hücredeki parasal değeri uygun olmayan bir şekilde güncelleyen depolanmış bir yordam. 

- İncelemeyi gerçekleştiren kişinin, kod yazarı nın kaynağı ve kod incelemeleri ve güvenli kodlama konusunda bilgili olan kişi dışında bir birey olduğundan emin olun.

- Tüm kod değişikliği kaynaklarını bildiğinizden emin olun. Kod T-SQL Scripts olabilir. Yürütülecek veya Görünümler, Işlevler, Tetikleyiciler ve Depolanan Yordamlar biçimlerinde dağıtılmak üzere geçici komutlar olabilir. SQL Agent Job tanımlarının (Adımlar) bir parçası olabilir. Ayrıca SSIS paketleri, Azure Veri Fabrikası ve diğer hizmetler içinden de yürütülebilir.

## <a name="data-protection"></a>Veri koruma

Veri koruması, önemli bilgileri şifreleme veya gizleme ile tehlikeye girmeden korumak için bir dizi özelliktir.

> [!NOTE]
> Microsoft, Azure SQL Veritabanı'nı FIPS 140-2 Düzey 1 uyumlu olarak doğrular. Bu, FIPS 140-2 Düzey 1 kabul edilebilir algoritmalar ve FIPS 140-2 Düzey 1 gerekli anahtar uzunlukları, anahtar yönetimi, anahtar oluşturma ve anahtar depolama ile tutarlılık da dahil olmak üzere bu algoritmaların doğrulanmış örnekleri nin sıkı kullanımını doğruladıktan sonra yapılır. Bu attestation müşterilerimizin ihtiyaç veya fips 140-2 Düzey 1 doğrulanmış örnekleri veri işleme veya sistemlerin veya uygulamaların teslim ilerler ve sayılsa gereksinimlerine yanıt vermek için izin vermek içindir. Yukarıdaki ifadede kullanılan "FIPS 140-2 Düzey 1 uyumlu" ve "FIPS 140-2 Düzey 1 uyumluluğu" terimlerini, ABD ve Kanada hükümetinin "FIPS 140-2 Düzey 1 doğrulanmış" terimini kullanma amaçlarını göstermek için tanımlıyoruz. 


### <a name="encrypt-data-in-transit"></a>Aktarımsırasında verileri şifreleme

> Bahsedilen: OSA Uygulama #6, ISO Kontrol Ailesi: Kriptografi

Veri istemciniz ve sunucunuz arasında hareket ederken verilerinizi korur. Ağ [Güvenliği'ne](#network-security)bakın.

### <a name="encrypt-data-at-rest"></a>Bekleyen verileri şifreleme

> Bahsedilen: OSA Uygulama #6, ISO Kontrol Ailesi: Kriptografi

Şifreleme, veritabanı, günlük ve yedekleme dosyalarında kalıcı olduğunda verilerin şifreleme korumasıdır.

**Nasıl uygulanır:**

- Hizmet yönetilen anahtarlarla [saydam veritabanı şifrelemesi (TDE),](transparent-data-encryption-azure-sql.md) Azure SQL Veritabanı'nda 2017'den sonra oluşturulan tüm veritabanları için varsayılan olarak etkinleştirilir.
- Yönetilen bir durumda, veritabanı şirket içi bir sunucu kullanarak geri yükleme işleminden oluşturulursa, özgün veritabanının TDE ayarı onurlanır. Özgün veritabanıTDE etkin değilse, yönetilen örnek için TDE'nin el ile açık olmasını öneririz.

**En iyi uygulamalar:**

- Şifreleme-at-rest gerektiren verileri ana veritabanında depolamayın. Ana veritabanı TDE ile şifrelenemez.

- TDE koruması üzerinde daha fazla saydamlık ve parçalı denetime ihtiyacınız varsa Azure Anahtar Kasası'nda müşteri tarafından yönetilen anahtarları kullanın. Azure Key Vault, veritabanını erişilemez hale getirmek için izinleri istediği zaman iptal etme olanağı sağlar. TDE koruyucularını diğer tuşlarla birlikte merkezi olarak yönetebilir veya Azure Key Vault'u kullanarak TDE koruyucuyu kendi zamanlamanızda döndürebilirsiniz.

- Azure Key Vault'ta müşteri tarafından yönetilen anahtarları kullanıyorsanız, makaleleri, [TDE'yi Azure Key Vault ile yapılandırma yönergelerini](transparent-data-encryption-byok-azure-sql.md#recommendations-when-configuring-akv) ve [Azure Key Vault ile Geo-DR'yi yapılandırmaya yönelik](transparent-data-encryption-byok-azure-sql.md#geo-dr-and-customer-managed-tde)yönergeleri izleyin.

### <a name="protect-sensitive-data-in-use-from-high-privileged-unauthorized-users"></a>Kullanımdaki hassas verileri yüksek ayrıcalıklı, yetkisiz kullanıcılardan koruyun

Kullanılan veriler, SQL sorgularının yürütülmesi sırasında veritabanı sisteminin belleğinde depolanan verilerdir. Veritabanınız hassas verileri depolarsa, yüksek ayrıcalıklı kullanıcıların veritabanınızdaki hassas verileri görüntülemesini engellemek için kuruluşunuz gerekebilir. Kuruluşunuzdaki Microsoft operatörleri veya DAB'ler gibi yüksek ayrıcalıklı kullanıcıların veritabanını yönetebilmesi, ancak sql server işleminin belleğinden veya veritabanını sorgulayarak hassas verileri görüntülemesi ve büyük ölçüde boşaltması engellenmelidir.

Hangi verilerin hassas olduğunu ve hassas verilerin bellekte şifrelenip şifrelenmemesi ve yöneticiler tarafından düz metin olarak erişilemeyeceğini belirleyen ilkeler, kuruluşunuz ve uymanız gereken uyumluluk düzenlemelerine özgüdür. Lütfen ilgili gereksinime bakın: [Hassas verileri tanımlayın ve etiketleyin.](#identify-and-tag-sensitive-data)

**Nasıl uygulanır:**

- Azure SQL Veritabanı'nda, bellekte/kullanımda bile hassas verilerin düz metinde açıkta olmadığından emin olmak için [Her Zaman Şifrelenmiş'i](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) kullanın. Her Zaman Şifrelenmiş, Veritabanı Yöneticilerinden (DTA) ve bulut yöneticilerinden (veya yüksek ayrıcalıklı ancak yetkisiz kullanıcıların kimlerin erişebileceği üzerinde daha fazla denetim edebildiğinize) karşı koruma sağlar.

**En iyi uygulamalar:**

- Her zaman Şifrelenmiş, verileri istirahatte (TDE) veya aktarım sırasında (SSL/TLS) şifrelemenin yerini tutamaz. Her zaman Şifrelenmiş performans ve işlevsellik etkisini en aza indirmek için hassas olmayan veriler için kullanılmamalıdır. Her Zaman TDE ve Aktarım Katmanı Güvenliği (TLS) ile birlikte şifrelenmiş olarak kullanılması, dinleni, aktarım ve kullanım sırasında verilerin kapsamlı bir şekilde korunması için önerilir. 

- Her Zaman Şifreli'yi bir üretim veritabanında dağıtmadan önce tanımlanan hassas veri sütunlarını şifrelemenin etkisini değerlendirin. Genel olarak, Her Zaman Şifrelenmiş, şifrelenmiş sütunlarda sorguların işlevselliğini azaltır ve [Her Zaman Şifrelenmiş - Özellik Ayrıntıları'nda](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine#feature-details)listelenen diğer sınırlamaları vardır. Bu nedenle, istemci tarafında bir sorgu desteklemiyor veya/ve depolanan yordamların, işlevlerin, görünümlerin ve tetikleyicilerin tanımları da dahil olmak üzere veritabanı şemanızı yeniden düzenlemeniz için uygulamanızı yeniden düzenlemeniz gerekebilir. Varolan uygulamalar, Her Zaman Şifrelenmiş'in kısıtlamalarına ve sınırlamalarına uymazlarsa, şifrelenmiş sütunlarla çalışmayabilir. Her Zaman Şifrelenmiş'i destekleyen Microsoft araçları, ürünleri ve hizmetleri ekosistemi büyürken, bunların bir kısmı şifreli sütunlarla çalışmaz. Bir sütunu şifrelemek, iş yükünüzün özelliklerine bağlı olarak sorgu performansını da etkileyebilir. 

- Verileri kötü amaçlı DB'lerden korumak için Her Zaman Şifreli kullanıyorsanız, her zaman şifrelenmiş tuşları rol ayrımıyla yönetin. Rol ayrımında, bir güvenlik yöneticisi fiziksel anahtarları oluşturur. DBA, veritabanında fiziksel anahtarları açıklayan sütun ana anahtarı ve sütun şifreleme anahtarı meta veri nesnelerini oluşturur. Bu işlem sırasında, güvenlik yöneticisinin veritabanına erişmesi gerekmez ve DBA'nın düz metin olarak fiziksel anahtarlara erişmesi gerekmez. 
  - Ayrıntılar için [Rol Ayırma ile Anahtarları Yönetme](https://docs.microsoft.com/sql/relational-databases/security/encryption/overview-of-key-management-for-always-encrypted#managing-keys-with-role-separation) başlıklı makaleye bakın. 

- Yönetim kolaylığı için sütun ana anahtarlarınızı Azure Key Vault'ta saklayın. Anahtar yönetimini zorlaştıran Windows Certificate Store'u (ve genel olarak dağıtılan anahtar depolama çözümleri, merkezi anahtar yönetimi çözümlerinin aksine) kullanmaktan kaçının. 

- Birden çok anahtar (sütun ana anahtar veya sütun şifreleme anahtarları) kullanma nın dengeleri ile dikkatlice düşünün. Anahtar yönetimi maliyetini azaltmak için anahtar sayısını küçük tutun. Bir sütun ana anahtar ve veritabanı başına bir sütun şifreleme anahtarı genellikle sabit durum ortamlarında yeterlidir (anahtar döndürmenin ortasında değil). Her biri farklı anahtarları kullanarak ve farklı verilere erişen farklı kullanıcı gruplarınız varsa ek anahtarlara ihtiyacınız olabilir.  

- Uyumluluk gereksinimlerinize göre sütun ana anahtarlarını döndürün. Sütun şifreleme anahtarlarını da döndürmeniz gerekiyorsa, uygulama kapalı kalma süresini en aza indirmek için çevrimiçi şifreleme kullanmayı düşünün. 
  - Makaleye bakın, [Performans ve Kullanılabilirlik Hususlar.](https://docs.microsoft.com/sql/relational-databases/security/encryption/configure-column-encryption-using-powershell#performance-and-availability-considerations) 

- Verilerdeki hesaplamaların (eşitlik) desteklenmesi gerekiyorsa deterministik şifrelemeyi kullanın. Aksi takdirde, rasgele şifreleme kullanın. Düşük entropi veri kümeleri veya genel olarak bilinen dağıtıma sahip veri kümeleri için deterministik şifreleme kullanmaktan kaçının. 

- Üçüncü tarafların izniniz olmadan verilerinize yasal olarak erişmelerinden endişe ediyorsanız, anahtarlara ve verilere erişimi olan tüm uygulama ve araçların Microsoft Azure Bulutu dışında düz metin olarak çalıştırdığından emin olun. Anahtarlara erişim olmadan, şifrelemeyi atlamadıkları sürece üçüncü tarafın verilerin şifresini çözmesinin bir yolu yoktur.

- Her zaman Şifreli kolayca anahtarlara (ve korunan verilere) geçici erişim verilmesi desteklemez. Örneğin, DBA'nın hassas ve şifrelenmiş veriler üzerinde bazı temizleme işlemleri yapmasına izin vermek için anahtarları Bir DBA ile paylaşmanız gerekiyorsa. DBA'dan verilere erişimi iptal etmenin güvenilirliğini geri almanın tek yolu, hem sütun şifreleme anahtarlarını hem de pahalı bir işlem olan verileri koruyan sütun ana anahtarlarını döndürmektir. 

- Şifreli sütunlarda düz metin değerlerine erişmek için, kullanıcının CMK'yı tutan anahtar deposunda yapılandırılan sütunları koruyan Sütun Ana Anahtarına (CMK) erişmesi gerekir. Kullanıcı da **VIEW ANY COLUMN MASTER ANAHTAR TANıMı** olması ve HERHANGI BIR SÜTUN ŞIFRELEME ANAHTAR TANıMı veritabanı izinleri **görüntülemek** gerekir.

### <a name="control-access-of-application-users-to-sensitive-data-through-encryption"></a>Şifreleme yoluyla uygulama kullanıcılarının hassas verilere erişimini kontrol etme

Şifreleme, yalnızca şifreleme anahtarlarına erişimi olan belirli uygulama kullanıcılarının verileri görüntüleyebilmesini veya güncellebilmesini sağlamanın bir yolu olarak kullanılabilir.

**Nasıl uygulanır:**

- Hücre düzeyinde Şifreleme (CLE) kullanın. Makaleye bakın, Ayrıntılar için [Bir Veri Sütunu Şifrele.](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) 
- Her Zaman Şifreli'yi kullanın, ancak sınırlamasına dikkat edin. Sınırlamalar aşağıda listelenmiştir.

**En iyi uygulamalar**

CLE kullanırken:

- SQL izinleri ve rolleri aracılığıyla anahtarlara erişimi denetle. 

- Veri şifreleme için AES (AES 256 önerilir) kullanın. RC4, DES ve TripleDES gibi algoritmalar amortismana alınır ve bilinen güvenlik açıkları nedeniyle kullanılmamalıdır. 

- 3DES kullanmaktan kaçınmak için simetrik tuşları asimetrik tuşlarla/sertifikalarla (parolalarla değil) koruyun. 

- Dışa aktarma/alma (bacpac dosyaları) aracılığıyla Hücre Düzeyi Şifreleme'yi kullanarak bir veritabanını taşırken dikkatli olun. 
  - Makaleye bakın, [Azure SQL Veritabanı'nda Veri](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/) Aktarırken anahtarları niçin kaybedeceğinizi ve diğer en iyi uygulama kılavuzunu kullanma önerilerini inceleyin.

Her Zaman Şifrelenmiş'in öncelikle Azure SQL Veritabanı'nın yüksek ayrıcalıklı kullanıcılarından (bulut operatörleri, DAB' ler) kullanımdaki hassas [verileri](#protect-sensitive-data-in-use-from-high-privileged-unauthorized-users)korumak için tasarlanır olduğunu unutmayın - bkz. Verileri uygulama kullanıcılarından korumak için Her Zaman Şifrelenmiş'i kullanırken aşağıdaki zorluklara dikkat edin:

- Varsayılan olarak, Her Zaman Şifrelenmiş'i destekleyen tüm Microsoft istemci sürücüleri sütun şifreleme anahtarlarının genel (uygulama başına bir) önbelleğini korur. Bir istemci sürücü, sütun ana anahtarı nı tutan bir anahtar deposuna başvurarak düz metin sütun şifreleme anahtarı nı satın aldıktan sonra, düz metin sütun şifreleme anahtarı önbelleğe çıkar. Bu, çok kullanıcılı bir uygulamanın kullanıcılarından gelen verileri yalıtmalarını zorlaştırır. Uygulamanız bir anahtar deposuyla (Azure Key Vault gibi) etkileşimde yken son kullanıcıların kimliğine bürünüyorsa, bir kullanıcının sorgusu önbelleği sütun şifreleme anahtarıyla doldurduktan sonra, aynı anahtarı gerektiren ancak başka bir kullanıcı tarafından tetiklenen sonraki bir sorgu önbelleğe alınmış anahtarı kullanır. Sürücü anahtar mağazasını aramaz ve ikinci kullanıcının sütun şifreleme anahtarına erişmek için izni olup olmadığını denetlemez. Sonuç olarak, kullanıcı anahtarlara erişimi olmasa bile, kullanıcı şifrelenmiş verileri görebilir. Çok kullanıcılı bir uygulama içinde kullanıcıların yalıtımını elde etmek için sütun şifreleme anahtarı önbelleğe alma devre dışı kullanabilirsiniz. Sürücünün her veri şifrelemesi veya şifre çözme işlemi için anahtar deposuna başvurması gerektiğinden, önbelleğe alma ek akışeklerine neden olur.

### <a name="protect-data-against-unauthorized-viewing-by-application-users-while-preserving-data-format"></a>Veri biçimini korurken verileri uygulama kullanıcıları tarafından yetkisiz görüntülemeye karşı koruyun
Yetkisiz kullanıcıların verileri görüntülemesini engellemenin bir diğer tekniği de, kullanıcı uygulamalarının verileri işlemeye ve görüntülemeye devam edebilmesini sağlamak için veri türlerini ve biçimleri korurken verileri gizlemek veya maskelemektir.

**Nasıl uygulanır:**

- Tablo sütunlarını gizlemek için [Dinamik Veri Maskeleme'yi](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) kullanın.

> [!NOTE]
> Her zaman Şifreli Dinamik Veri Maskeleme ile çalışmıyor. Dinamik Veri Maskeleme yoluyla uygulama kullanıcılarınız için verileri maskelemek ve kullanımdaki verilerin korunmasına öncelik vermeniz gerektiği anlamına gelen aynı sütunu şifrelemek ve maskelemek mümkün değildir.

**En iyi uygulamalar:**

> [!NOTE]
> Dinamik Veri Maskeleme, verileri yüksek ayrıcalıklı kullanıcılardan korumak için kullanılamaz. Maskeleme ilkeleri, db_owner gibi yönetim erişimi olan kullanıcılar için geçerli değildir.

- Uygulama kullanıcılarının geçici sorgular çalıştırmalarına izin verme (Dinamik Veri Maskeleme'yi çözebilecekleri için).  
  - Makaleye bakın, Ayrıntılar için [çıkarım veya kaba kuvvet teknikleri kullanarak maskeleme atlayarak.](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking#security-note-bypassing-masking-using-inference-or-brute-force-techniques)  

- Maskeli sütunlarda güncelleştirmeler yapmak için kullanıcı izinlerini sınırlamak için uygun bir erişim denetimi ilkesi (SQL izinleri, roller, RLS yoluyla) kullanın. Sütunda maske oluşturmak, o sütunun güncelleştirmelerini engellemez. Maskeli sütunu sorgularken maskeli veri alan kullanıcılar, yazma izinleri varsa verileri güncelleştirebilirler.    

-  Dinamik Veri Maskeleme, maskeli değerlerin istatistiksel özelliklerini korumaz. Bu, sorgu sonuçlarını etkileyebilir (örneğin, filtreleme yüklemleri içeren sorgular veya maskeli verilerde birleştirilmiş sorgular).

## <a name="network-security"></a>Ağ güvenliği
Ağ güvenliği, verilerinizi Azure SQL Veritabanı'na aktarılıolarak güvence altına almak için erişim denetimleri ve en iyi uygulamalar anlamına gelir.

### <a name="configure-my-client-to-connect-securely-to-azure-sql-database"></a>Müşterimi Azure SQL Veritabanına güvenli bir şekilde bağlanmak için yapılandırma 
Tanınmış güvenlik açıklarına sahip istemci makinelerinin ve uygulamaların (örneğin, eski TLS protokollerini ve şifre paketlerini kullanarak) Azure SQL Veritabanı'na bağlanmasını nasıl önleyeceğiniz en iyi uygulamalar.

**Nasıl uygulanır:**

- Azure SQL Veritabanı'na bağlanan istemci [makinelerinin Aktarım Katmanı Güvenliği (TLS)](sql-database-security-overview.md#transport-layer-security-tls-encryption-in-transit)kullandığından emin olun.

**En iyi uygulamalar:**

- Tüm uygulamalarınızı ve araçlarınızı şifreleme etkinken SQL DB'ye bağlanmak için yapılandırın 
  - Encrypt = On, TrustServerCertificate = Kapalı (veya Microsoft olmayan sürücülerle eşdeğer). 

- Uygulamanız TLS'yi desteklemeyen veya TLS'nin eski bir sürümünü destekleyen bir sürücü kullanıyorsa, mümkünse sürücüyü değiştirin. Mümkün değilse, güvenlik risklerini dikkatle değerlendirin. 

- [Aktarım Katmanı Güvenliği (TLS) kayıt defteri ayarlarına](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-10)göre Azure SQL Veritabanına bağlanan istemci makinelerde devre dışı bırakarak SSL 2.0, SSL 3.0, TLS 1.0 ve TLS 1.1'deki güvenlik açıkları aracılığıyla saldırı vektörlerini azaltın. 

- İstemcide bulunan şifre paketlerini kontrol edin: [TLS/SSL'deki Cipher Suites (Schannel SSP)](https://docs.microsoft.com/windows/desktop/SecAuthN/cipher-suites-in-schannel). Özellikle, [TLS Cipher Suite Sipariş Yapılandırma](https://docs.microsoft.com/windows-server/security/tls/manage-tls#configuring-tls-cipher-suite-order)başına 3DES devre dışı. 

- Azure SQL Veritabanı için şifreleme hem Proxy hem de Yeniden Yönlendirme bağlantı türleri için zorlanır. Yönetilen bir örnek kullanıyorsanız, sunucu tarafından şifreleme zorladığı için **Proxy** bağlantı türünü (varsayılan) kullanın. **Yönlendirme** bağlantı türü şu anda şifreleme zorlamayı desteklemez ve yalnızca özel IP bağlantılarında kullanılabilir. 

- Daha fazla bilgi için Azure [SQL Bağlantı Mimarisi - Bağlantı ilkesine](sql-database-connectivity-architecture.md#connection-policy)bakın.


### <a name="minimize-attack-surface"></a>Saldırı Yüzeyini En Aza Indirin
Kötü amaçlı bir kullanıcı tarafından saldırıya uğrayabileceğiniz özellik sayısını en aza indirin. Azure SQL Veritabanı için ağ erişim denetimleri uygulayın.

> Bahsedilen: OSA Uygulama #5

**Nasıl uygulanır:**

Azure SQL Veritabanı sunucusunda (singleton veritabanı veya elastik havuzlar içeren):
- Azure hizmetlerine Erişime İzin Ver'i ILE KAPASine'yi ayarlayın.

- VNet Hizmeti uç noktalarını ve VNet Güvenlik Duvarı Kurallarını kullanın.

- Özel Bağlantı (önizleme) kullanın.

Yönetilen bir örnekte:
- [Ağ gereksinimlerindeki](sql-database-managed-instance-connectivity-architecture.md#network-requirements)yönergeleri izleyin. 

**En iyi uygulamalar:**

- Özel bir bitiş noktasına bağlanarak Azure SQL Veritabanına erişimi kısıtlamak (örneğin, özel bir veri yolu kullanma): 
  - Yönetilen bir örnek, harici erişimi önlemek için bir VNet içinde yalıtılabilir. Aynı bölgede aynı veya eşlenen VNet'te bulunan uygulamalar ve araçlar doğrudan erişebilir. Farklı bölgelerdeki uygulamalar ve araçlar, bağlantı kurmak için VNet-to-VNet bağlantısını veya ExpressRoute devresini kullanabilir. Müşteri, bağlantı noktası 1433 üzerinden erişimi yalnızca yönetilen bir örne erişim gerektiren kaynaklarla sınırlamak için Ağ Güvenlik Grupları'nı (NSG) kullanmalıdır 
  - Bir SQL Veritabanı sunucusu (tek veritabanları veya elastik havuzlar içeren) için, VNet'inizin içindeki SQL Veritabanı sunucusu için özel bir ÖZEL IP sağlayan [Özel Bağlantı](sql-database-private-endpoint-overview.md) özelliğini kullanın. SQL Veritabanı sunucularınıza erişimi kısıtlamak için [VNet Hizmeti uç noktalarını VNet Güvenlik Duvarı Kuralları ile](sql-database-vnet-service-endpoint-rule-overview.md) de kullanabilirsiniz.
  - Mobil kullanıcılar veri yolu üzerinden bağlanmak için noktadan siteye VPN bağlantılarını kullanmalıdır.
  - Şirket içi ağlarına bağlı kullanıcılar, veri yolu üzerinden bağlanmak için siteden siteye VPN bağlantısını veya ExpressRoute'u kullanmalıdır.

- Azure SQL Veritabanı'na ortak bir uç noktaya bağlanarak (örneğin, ortak veri yolunu kullanarak) erişebilirsiniz. Aşağıdaki en iyi uygulamalar göz önünde bulundurulmalıdır: 
  - Bir SQL Veritabanı sunucusu için, yalnızca yetkili IP adreslerine erişimi kısıtlamak için [IP güvenlik duvarı kurallarını](sql-database-firewall-configure.md) kullanın.
  - Yönetilen bir örnek için, bağlantı noktası 3342 üzerinden erişimi yalnızca gerekli kaynaklarla sınırlamak için Ağ Güvenlik Grupları'nı (NSG) kullanın. Daha fazla bilgi için [bkz.](sql-database-managed-instance-public-endpoint-securely.md) 

> [!NOTE]
> Yönetilen örnek ortak bitiş noktası varsayılan olarak etkinleştirilmez ve açıkça etkinleştirilmiş olmalıdır. Şirket ilkesi ortak uç noktaların kullanımına izin veriyorsa, ilk etapta ortak uç noktalarıetkinleştirmemek için [Azure İlkesi'ni](../governance/policy/overview.md) kullanın.

- Azure Ağ bileşenlerini ayarlama: 
  - [Ağ güvenliği için Azure en iyi uygulamalarını](../security/fundamentals/network-best-practices.md)izleyin.
  - Azure Sanal Ağı'nda özetlenen en iyi uygulamalara göre Sanal Ağ (VNet) yapılandırmasını planlayın [sık sorulan sorular (SSS)](../virtual-network/virtual-networks-faq.md) ve planlayın. 
  - VNet'i birden çok alt ağ halinde segmente ayırın ve benzer rol için kaynakları aynı alt ağa (örneğin, ön uç vs arka uç kaynakları) atayın.
  - Azure VNet sınırı içindeki alt ağlar arasındaki trafiği denetlemek için [Ağ Güvenlik Grupları'nı (NSGs)](../virtual-network/security-overview.md) kullanın.
  - Gelen ve giden ağ trafiğini izlemek için aboneliğiniz için [Azure Ağ İzleyicisini](../network-watcher/network-watcher-monitoring-overview.md) etkinleştirin.

### <a name="configure-power-bi-for-secure-connections-to-azure-sql-database"></a>Azure SQL Veritabanı'na güvenli bağlantılar için Power BI'yi yapılandırın

**En iyi uygulamalar:**

- Power BI Desktop için mümkün olduğunca özel veri yolunu kullanın. 

- Power BI Desktop'ın, kayıt defteri anahtarını istemci makinesinde [Taşıma Katmanı Güvenliği (TLS)](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) kayıt defteri ayarlarına göre ayarlayarak TLS1.2 kullanarak bağlandığından emin olun. 

- [Power BI ile Satır düzeyinde güvenlik (RLS)](https://docs.microsoft.com/power-bi/service-admin-rls)aracılığıyla belirli kullanıcılar için veri erişimini kısıtlayın. 

- Power BI Service için, [sınırlamaları ve dikkatleri](https://docs.microsoft.com/power-bi/service-gateway-deployment-guidance#installation-considerations-for-the-on-premises-data-gateway)göz önünde bulundurarak [şirket içi veri ağ geçidini](https://docs.microsoft.com/power-bi/service-gateway-onprem)kullanın.

### <a name="configure-app-service-for-secure-connections-to-azure-sql-database"></a>Azure SQL Veritabanı'na güvenli bağlantılar için Uygulama Hizmetini yapılandırma

**En iyi uygulamalar:**

- Basit bir Web Uygulaması için, genel uç noktaya bağlanmak için **Azure Hizmetlerine İzin Verme'nin** A)'ya bağlanması nı gerektirir. 

- Yönetilen bir örniğe özel veri yolu bağlantısı için [uygulamanızı bir Azure Sanal Ağı ile tümleştirin.](../app-service/web-sites-integrate-with-vnet.md) İsteğe bağlı olarak, [Uygulama Hizmet Ortamları (ASE)](../app-service/environment/intro.md)ile bir Web Uygulaması da dağıtabilirsiniz. 

- ASE veya VNet Integrated Web App ile SQL Veritabanı sunucusunda bir veritabanına bağlanan Web Uygulaması için, belirli bir VNet ve alt ağdan erişimi sınırlamak için [VNet Hizmeti uç noktalarını ve VNet Güvenlik Duvarı Kurallarını](sql-database-vnet-service-endpoint-rule-overview.md) kullanabilirsiniz. Ardından **Azure Hizmetlerine İzin Ver'i** KAPA Ayrıca, ASE'yi özel bir veri yolu üzerinden yönetilen bir örne de bağlayabilirsiniz.  

- Web Uygulamanızın makaleye göre yapılandırıldığından emin olun, [Azure Uygulama Hizmeti'ni kullanarak PaaS web ve mobil uygulamaları güvence altına almak için en iyi uygulamalar.](../security/security-paas-applications-using-app-services.md) 

- Web uygulamanızı sık karşılaşılan açıklardan ve güvenlik açıklarından korumak için [Web Uygulama Güvenlik Duvarı'nı (WAF)](../application-gateway/waf-overview.md) yükleyin.

### <a name="configure-azure-vm-hosting-for-secure-connections-to-azure-sql-database"></a>Azure SQL Veritabanı'na güvenli bağlantılar için Azure VM barındırmayı yapılandırma

**En iyi uygulamalar:**

- VM'den hangi bölgelere erişilebileceğini denetlemek için Azure VM'lerinin NSG'leri üzerindeki İzin ver ve reddet kurallarının bir birleşimini kullanın.

- Azure'daki [IaaS iş yükleri için](../security/azure-security-iaas.md)VM'nizin makaleye göre yapılandırıldığından emin olun.

- Tüm VM'lerin belirli bir VNet ve alt ağla ilişkili olduğundan emin olun. 

- Zorunlu [tünelleme ile ilgili](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md#about-forced-tunneling)kılavuz başına varsayılan rota 0.0.0.0/Internet'e ihtiyacınız olup olmadığını değerlendirin. 
  - Evet ise - örneğin, ön uç alt ağ - sonra varsayılan rota tutmak.
  - Eğer hayır - örneğin, orta katman veya arka uç alt ağ - o zaman hiçbir trafik internet üzerinden şirket içinde (aka çapraz tesislerinde) ulaşmak için gider böylece kuvvet tünel etkinleştirin. 

- Şirket içinde bakış lama veya bağlantı kullanıyorsanız [isteğe bağlı varsayılan yolları](../virtual-network/virtual-networks-udr-overview.md#optional-default-routes) uygulayın. 

- VNet'teki tüm trafiği paket denetimi için Bir Ağ Sanal Cihazı'na göndermeniz gerekiyorsa [Kullanıcı Tanımlı Rotalar](../virtual-network/virtual-networks-udr-overview.md#user-defined) uygulayın. 

- Azure omurga ağı üzerinden Azure Depolama gibi PaaS hizmetlerine güvenli erişim için [VNet Hizmeti uç noktalarını](sql-database-vnet-service-endpoint-rule-overview.md) kullanın. 

### <a name="protect-against-distributed-denial-of-service-ddos-attacks"></a>Dağıtılmış Hizmet Reddi (DDoS) saldırılarına karşı koruyun
Dağıtılmış Hizmet Reddi (DDoS) saldırıları, kötü amaçlı bir kullanıcının Azure altyapısını bunaltıcı ve geçerli girişleri ve iş yükünü reddetmesine neden olmak amacıyla Azure SQL Veritabanı'na ağ trafiği seli gönderme girişimleridir.

> Bahsedilen: OSA Uygulama #9

**Nasıl uygulanır:**

Azure Platformu'nun bir parçası olarak DDoS koruması otomatik olarak etkinleştirilir. Her zaman açık trafik izleme ve ortak uç noktalara ağ düzeyinde saldırıların gerçek zamanlı hafifletilmesi içerir. 

- VNets'te dağıtılan kaynaklarla ilişkili genel IP adreslerini izlemek için [Azure DDoS Koruması'nı](../virtual-network/ddos-protection-overview.md) kullanın.

- Veritabanlarına yönelik Hizmet Reddi (DoS) saldırılarını algılamak [için Azure SQL Veritabanı için Gelişmiş Tehdit Koruması'nı](sql-database-threat-detection-overview.md) kullanın.

**En iyi uygulamalar:**

- [Saldırı Yüzeyini En Aza Indir'de](#minimize-attack-surface) açıklanan uygulamaları izleyin, DDoS saldırı tehditlerinin en aza indirgemesine yardımcı olur. 

- Gelişmiş Tehdit Koruma **Brute kuvvet SQL kimlik bilgileri** uyarısı kaba kuvvet saldırıları algılamak için yardımcı olur. Bazı durumlarda, uyarı penetrasyon test iş yüklerini ayırt edebilir. 

- SQL Veritabanı'na bağlanan Azure VM barındırma uygulamaları için: 
  - Azure Güvenlik Merkezi'nde Internet'e bakan uç noktalarüzerinden erişimi kısıtlama önerisini izleyin. 
  - Azure Sanal M'lerde uygulamanızın birden çok örneğini çalıştırmak için sanal makine ölçeği kümelerini kullanın. 
  - Kaba kuvvet saldırısını önlemek için RDP ve SSH'yi Internet'ten devre dışı kayırın. 

## <a name="monitoring-logging-and-auditing"></a>İzleme, Günlüğe Kaydetme ve Denetleme  
Bu bölümde, veritabanlarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılamanıza yardımcı olacak yetenekler ifade eder. Ayrıca, veritabanı olaylarını izlemek ve yakalamak için veritabanı denetimini yapılandırmak için en iyi uygulamaları açıklar.

### <a name="protect-databases-against-attacks"></a>Veritabanlarını saldırılara karşı koruma 
Gelişmiş tehdit koruması, anormal etkinliklerde güvenlik uyarıları sağlayarak olası tehditleri algılayabilmenizi ve bunlara yanıt vermenizi sağlar.

**Nasıl uygulanır:**

- Şu lar da dahil olmak üzere veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılamak [için SQL için Gelişmiş Tehdit Koruması'nı](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts) kullanın:
  - SQL enjeksiyon saldırısı.
  - Kimlik bilgileri hırsızlığı/sızıntısı.
  - Ayrıcalık istismarı.
  - Veri sızma.

**En iyi uygulamalar:**

- Azure SQL Veritabanı için Belirli bir SQL Veritabanı sunucusu veya yönetilen bir örnek için [Gelişmiş Veri Güvenliği (ADS)](sql-database-advanced-data-security.md#getting-started-with-ads) yapılandırın. Ayrıca, [Azure Güvenlik Merkezi Standart katmanına](../security-center/security-center-pricing.md)geçerek tüm SQL Veritabanı sunucuları ve yönetilen örnekler için ADS yapılandırabilirsiniz. 

- Tam bir araştırma deneyimi için SQL [Veritabanı Denetimi'ni](sql-database-auditing.md)etkinleştirmeönerilir. Denetim de veritabanı olaylarını izleyebilir ve bunları bir Azure Depolama hesabındaki veya Azure Log Analytics çalışma alanında bir denetim günlüğüne yazabilirsiniz. 

### <a name="audit-critical-security-events"></a>Kritik güvenlik olaylarını denetleme
Veritabanı olaylarının izlenmesi, veritabanı etkinliğini anlamanıza yardımcı olur. İş le ilgili endişeleri veya şüpheli güvenlik ihlallerini gösterebilecek tutarsızlıklar ve anormallikler hakkında bilgi edinebilirsiniz. Ayrıca uyumluluk standartlarına uyulmasını sağlar ve kolaylaştırır. 

**Nasıl uygulanır:**

- Veritabanı olaylarını izlemek ve Bunları Azure Depolama hesabınızdaki bir denetim günlüğüne, Log Analytics çalışma alanına (önizleme) veya Etkinlik Hub'larına (önizleme) yazmak için [SQL Veritabanı Denetimi'ni](sql-database-auditing.md) etkinleştirin. 

- Denetim günlükleri bir Azure Depolama hesabına, Azure Monitor günlükleri tarafından tüketilmesi için Günlük Analizi çalışma alanına veya olay merkezini kullanarak tüketim için etkinlik merkezine yazılabilir. Bu seçeneklerin herhangi bir birleşimini yapılandırabilirsiniz ve denetim günlükleri her birine yazılır. 

**En iyi uygulamalar:**

- Veritabanı sunucunuzdaki [SQL Veritabanı Denetimi'ni](sql-database-auditing.md) denetim olaylarıyla yapılandırarak, bu sunucudaki varolan ve yeni oluşturulan tüm veritabanları denetlenir.
- Varsayılan denetim ilkesi, veritabanlarına karşı tüm eylemleri (sorgular, depolanan yordamlar ve başarısız ve başarısız oturum açmalar) içerir ve bu da yüksek miktarda denetim günlüğüyle sonuçlanabilir. Müşterilerin [PowerShell'i kullanarak farklı eylem türleri ve eylem grupları için denetim yapılandırması](sql-database-auditing.md#manage-auditing)önerilir. Bunu yapılandırılacak şekilde, denetlenen eylemlerin sayısının denetlenmeleri ve olay kaybı riskinin en aza indirilmesi yardımcı olur. Özel denetim yapılandırması, müşterilerin yalnızca gereken denetim verilerini yakalamasına olanak sağlar.
- Denetim günlükleri doğrudan Azure [portalında](https://portal.azure.com/)veya yapılandırılan depolama konumundan tüketilebilir. 


> [!NOTE]
> Log Analytics denetiminin etkinleştirilmesi, yutma oranlarına bağlı olarak maliyete tabi olacaktır. Lütfen bu [seçeneği](https://azure.microsoft.com/pricing/details/monitor/)kullanmanın ilişkili maliyetine dikkat edin veya denetim günlüklerini bir Azure depolama hesabında depolamayı düşünün. 

**Diğer kaynaklar:**

- [SQL Veritabanı Denetimi](sql-database-auditing.md)
- [SQL Server Denetimi](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) 

### <a name="secure-audit-logs"></a>Güvenli denetim günlükleri
Görevlerin Ayrılmasını desteklemek ve DBA'yı Denetçilerden ayırmak için depolama hesabına erişimi kısıtlayın. 

**Nasıl uygulanır:**

- Denetim günlüklerini Azure Depolama'ya kaydederken, Depolama Hesabına erişimin minimum güvenlik ilkeleriyle sınırlı olduğundan emin olun. Depolama hesabına kimlerin erişebilenleri kontrol edin.
    - Daha fazla bilgi için [bkz.](../storage/common/storage-auth.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

**En iyi uygulamalar:**

- Denetim Hedefine Erişimi Denetlemek, DBA'yı Denetçilerden ayırmada önemli bir kavramdır. 

- Hassas verilere erişimi denetlerken, Denetçi'ye bilgi sızıntısını önlemek için verileri veri şifrelemeile güvence altına almayı düşünün. Daha fazla bilgi için [Protect sensitive data in use from high-privileged, unauthorized users](#protect-sensitive-data-in-use-from-high-privileged-unauthorized-users)bkz.

## <a name="security-management"></a>Güvenlik Yönetimi

Bu bölümde, veritabanlarınızın güvenlik duruşunu yönetmek için farklı yönleri ve en iyi uygulamaları açıklanmaktadır. Veritabanlarınızın güvenlik standartlarını karşılayacak şekilde yapılandırılmasını sağlamak, veritabanlarınızdaki hassas olabilecek verileri bulmak ve sınıflandırmak ve izlemek için en iyi uygulamaları içerir. 

### <a name="ensure-that-the-databases-are-configured-to-meet-security-best-practices"></a>Veritabanı(lar) güvenlik en iyi uygulamaları karşılamak için yapılandırıldığından emin olun 

Olası veritabanı güvenlik açıklarını keşfederek ve düzelterek veritabanı güvenliğinizi proaktif olarak geliştirin.

**Nasıl uygulanır:**

- [Veritabanınızı](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) güvenlik sorunları için tarayan ve veritabanlarınızda düzenli aralıklarla çalışacak şekilde SQL Güvenlik Açığı Değerlendirmesi'ni (VA) etkinleştirin.

**En iyi uygulamalar:**

- Başlangıçta, veritabanlarınızda VA çalıştırın ve güvenlik en iyi uygulamalarına karşı başarısız denetimleri düzelterek yineleyin. Taramaya _temiz_çıkana veya tüm denetimler geçene kadar kabul edilebilir yapılandırmalar için taban çizgileri ayarlayın.  

- Düzenli yinelenen taramaları haftada bir kez çalışacak şekilde yapılandırın ve ilgili kişiyi özet e-postalar alacak şekilde yapılandırın. 

- Her haftalık taradan sonra VA özetini gözden geçirin. Bulunan güvenlik açıkları için, önceki tarama sonucundaki sürüklenme değerlendirmek ve denetimin çözülüp çözülmemesi gerektiğini belirleyin. Yapılandırma değişikliğinin geçerli bir nedeni olup olmadığını gözden geçirin.   

- Denetimleri çözümleyin ve ilgili yerlerde taban çizgilerini güncelleştirin. Eylemleri çözmek için bilet öğeleri oluşturun ve bunlar çözülene kadar bunları izleyin. 

**Diğer kaynaklar:**

- [SQL Güvenlik Açığı Değerlendirmesi](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment) 
- [SQL Güvenlik Açığı Değerlendirmesi hizmeti, veritabanı güvenlik açıklarını belirlemenize yardımcı olur](sql-vulnerability-assessment.md)

### <a name="identify-and-tag-sensitive-data"></a>Hassas verileri tanımlama ve etiketleme 

Hassas veriler içerebilecek sütunları keşfedin. Hassas veri olarak kabul edilen veriler müşteriye, uyumluluk düzenlemesine vb. bağlıdır ve bu verilerden sorumlu kullanıcılar tarafından değerlendirilmesi gerekir. Gelişmiş duyarlılık tabanlı denetim ve koruma senaryolarını kullanmak için sütunları sınıflandırın. 

**Nasıl uygulanır:**

- Veritabanlarınızdaki hassas verileri keşfetmek, sınıflandırmak, etiketlemek ve korumak için [SQL Veri Bulma ve Sınıflandırma'yı](sql-database-data-discovery-and-classification.md) kullanın. 
  - SQL Veri Bulma ve Sınıflandırma panosunda otomatik bulma tarafından oluşturulan sınıflandırma önerilerini görüntüleyin. Hassas verilerinizin ısrarla sınıflandırma etiketleri ile etiketlenmiş olması gibi ilgili sınıflandırmaları kabul edin. 
  - Otomatik mekanizma tarafından keşfedilmemiş diğer hassas veri alanları için sınıflandırmaları el ile ekleyin. 
- Daha fazla bilgi için [SQL Veri Bulma & Sınıflandırması'na](https://docs.microsoft.com/sql/relational-databases/security/sql-data-discovery-and-classification)bakın.

**En iyi uygulamalar:**

- Veritabanının sınıflandırma durumunun doğru bir değerlendirmesi için sınıflandırma panosunu düzenli olarak izleyin. Veritabanı sınıflandırma durumu yla ilgili bir rapor, uyumluluk ve denetim amacıyla paylaşmak üzere dışa aktarılabilir veya yazdırılabilir.

- SQL Güvenlik Açığı Değerlendirmesi'nde önerilen hassas verilerin durumunu sürekli olarak izleyin. Hassas veri bulma kuralını izleyin ve sınıflandırma için önerilen sütunlarda herhangi bir sürüklenme tanımlayın.  

- Sınıflandırmayı kuruluşunuzun özel ihtiyaçlarına göre uyarlanmış bir şekilde kullanın. Azure Güvenlik Merkezi'ndeki [SQL Bilgi Koruma](../security-center/security-center-info-protection-policy.md) ilkesinde Bilgi Koruma ilkenizi (duyarlılık etiketleri, bilgi türleri, keşif mantığı) özelleştirin. 

### <a name="track-access-to-sensitive-data"></a>Hassas verilere erişimi izleme 
Hassas verilere kimlerin erişmesini izleyin ve denetim günlüklerinde hassas verilerdeki sorguları yakalayın.

**Nasıl uygulanır:**

- SQL Denetim ve Veri Sınıflandırması'nı birlikte kullanın. 
  - SQL [Veritabanı Denetimi](sql-database-auditing.md) günlüğünde, özellikle hassas verilere erişimi izleyebilirsiniz. Ayrıca, erişilen veriler gibi bilgileri ve duyarlılık etiketini görüntüleyebilirsiniz. Daha fazla bilgi için bkz [Auditing access to sensitive data](sql-database-data-discovery-and-classification.md#audit-sensitive-data) [&.](sql-database-data-discovery-and-classification.md) 

**En iyi uygulamalar:**

- Denetim ve Veri Sınıflandırma bölümleri için en iyi uygulamalara bakın: 
  - [Kritik güvenlik olaylarını denetleme](#audit-critical-security-events) 
  - [Hassas verileri tanımlama ve etiketleme](#identify-and-tag-sensitive-data) 

### <a name="visualize-security-and-compliance-status"></a>Güvenlik ve uyumluluk durumunu görselleştirin 

Veri merkezlerinizin (SQL Veritabanları dahil) güvenlik duruşunu güçlendiren birleşik bir altyapı güvenlik yönetim sistemi kullanın. Veritabanlarınızın güvenliği ve uyumluluk durumuyla ilgili öneriler listesini görüntüleyin.

**Nasıl uygulanır:**

- [Azure Güvenlik Merkezi'nde](https://azure.microsoft.com/documentation/services/security-center/)SQL ile ilgili güvenlik önerilerini ve etkin tehditleri izleyin.

## <a name="common-security-threats-and-potential-mitigations"></a>Yaygın güvenlik tehditleri ve olası azaltıcı etkenler

Bu bölüm, belirli saldırı vektörlerine karşı korumak için güvenlik önlemleri bulmanıza yardımcı olur. Çoğu azaltıcı etkenin yukarıdaki güvenlik yönergelerinden birini veya birkaçını izleyerek elde edilmesi beklenmektedir.

### <a name="security-threat-data-exfiltration"></a>Güvenlik tehdidi: Veri sızması

Veri sızma, bir bilgisayardan veya sunucudan verilerin yetkisiz kopyalanması, aktarılması veya alınmasıdır. Vikipedi'de [veri sızma](https://en.wikipedia.org/wiki/Data_exfiltration) için bir tanım bakın.

Azure SQL Veritabanı sunucusuna genel bir uç noktası üzerinden bağlanmak, müşterilerin güvenlik duvarlarını genel IP'lere açmalarını gerektirdiği için veri sızma riski sunar.  

**Senaryo 1**: Azure VM'deki bir uygulama, Azure SQL Veritabanı sunucusundaki bir veritabanına bağlanır. Haydut bir aktör VM'ye erişir ve bundan ödün verebilmiştir. Bu senaryoda, veri sızma, sahte VM kullanan harici bir varlığın veritabanına bağlandığı, kişisel verileri kopyaladığı ve bunu bir blob depolama alanında veya farklı bir SQL Veritabanında farklı bir abonelikte depolandığı anlamına gelir.

**Senaryo 2**: Bir Rouge DBA. Bu senaryo genellikle düzenlenmiş sektörlerden güvenlik duyarlı müşteriler tarafından yükseltilir. Bu senaryoda, yüksek ayrıcalıklı bir kullanıcı Azure SQL Veritabanı'ndaki verileri veri sahibi tarafından denetlenmeyen başka bir aboneye kopyalayabilir.

**Potansiyel azaltıcı etkenler:**

Bugün, Azure SQL Veritabanı veri sızma tehditlerini hafifletmek için aşağıdaki teknikleri sunar: 

- VM'den hangi bölgelere erişilebileceğini denetlemek için Azure VM'lerinin NSG'leri üzerindeki İzin ver ve reddet kurallarının bir birleşimini kullanın. 
- Bir Azure SQL Veritabanı sunucusu kullanıyorsanız (singleton veritabanları veya elastik havuzlar içeriyorsa), aşağıdaki seçenekleri ayarlayın:
  - Azure Hizmetlerinin KAPALI'ye izin ver.
  - Yalnızca Bir VNet Güvenlik Duvarı kuralı ayarlayarak Azure VM'nizi içeren alt ağdan gelen trafiğe izin verin.
  - [Özel Bağlantıyı](sql-database-private-endpoint-overview.md) Kullanma
- Yönetilen bir örnek için, varsayılan olarak özel IP erişimi nin kullanılması, haydut bir VM'nin ilk veri sızma endişesini giderir. Yönetilen bir örnek alt netindeki en kısıtlayıcı ilkeyi otomatik olarak ayarlamak için bir alt net üzerindeki alt ağ delegasyonu özelliğini açın.
- Rogue DBA endişesi, daha geniş bir yüzey alanına sahip olduğundan ve ağ gereksinimleri müşteriler tarafından görülebildiğinden yönetilen bir örnekle daha fazla açığa çıkar. Bunun için en iyi azaltma ilk etapta Rogue DBA senaryosunu önlemek için bu güvenlik kılavuzundaki tüm uygulamaları uygulamaktır (yalnızca veri sızması için değil). Her zaman Şifrelenmiş, hassas verileri şifreleyerek ve anahtarı DBA için erişilemez tutarak korumak için kullanılan bir yöntemdir.

## <a name="security-aspects-of-business-continuity-and-availability"></a>İş sürekliliğinin ve kullanılabilirliğinin güvenlik yönleri

Çoğu güvenlik standardı, tek bir hata noktasını önlemek için artıklık ve arızalama yetenekleri uygulanarak elde edilen operasyonel süreklilik açısından veri kullanılabilirliğini ele alabildi. Olağanüstü durum senaryoları için, Veri ve Günlük dosyalarının yedeklerini tutmak yaygın bir uygulamadır.Aşağıdaki bölüm, Azure'da yerleşik olan yeteneklere üst düzey bir genel bakış sağlar. Ayrıca, belirli gereksinimleri karşılamak üzere yapılandırılabilir ek seçenekler sağlar: 

- Azure yerleşik yüksek kullanılabilirlik sunar: [Yüksek kullanılabilirlik ve Azure SQL Veritabanı](sql-database-high-availability.md) 

- İş Açısından Kritik katmanı, varsayılan olarak etkinleştirilen başarısız gruplar, çoklu kullanılabilirlik bölgeleri, tam ve diferansiyel günlük yedeklemeleri ve zamanında geri yükleme yedeklerini içerir:  
  - [Yüksek kullanılabilirlik ve Azure SQL Veritabanı - Bölge yedek yapılandırması](sql-database-high-availability.md#zone-redundant-configuration)
  - [Otomatik yedeklemeler](sql-database-automated-backups.md)
  - [Otomatik veritabanı yedeklemelerini kullanarak Azure SQL Veritabanı kurtarma - Zamanında geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

- Farklı Azure coğrafyalarında otomatik arıza grupları gibi ek iş sürekliliği özellikleri burada açıklandığı gibi yapılandırılabilir: [Azure SQL Veritabanı ile iş sürekliliğine genel bakış](sql-database-business-continuity.md)

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure SQL Veritabanı güvenlik özelliklerine genel bakış](sql-database-security-overview.md)
