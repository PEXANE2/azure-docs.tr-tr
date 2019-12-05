---
title: Azure Active Directory kimlik doğrulaması yönetim işlemleri başvuru kılavuzu
description: Bu işlemler başvuru kılavuzu, kimlik doğrulama yönetimini güvenli hale getirmek için gerçekleştirmeniz gereken denetimleri ve eylemleri açıklar
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: fundamentals
ms.date: 10/31/2019
ms.author: martinco
ms.openlocfilehash: 40e0ba21d472097e34938878ddc1fa0c47b30417
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74803742"
---
# <a name="azure-active-directory-authentication-management-operations-reference-guide"></a>Azure Active Directory kimlik doğrulaması yönetim işlemleri başvuru kılavuzu

[Azure AD işlemler başvuru kılavuzu](active-directory-ops-guide-intro.md) 'nun bu bölümünde, kimlik bilgilerini güvenli hale getirmek ve yönetmek, kimlik doğrulama deneyimini tanımlamak, temsilci atama, kullanım ölçmek ve kurumsal güvenlik duruşunu temel alan erişim ilkeleri tanımlamak için gerçekleştirmeniz gereken denetimler ve eylemler açıklanmaktadır.

> [!NOTE]
> Bu öneriler, yayımlama tarihi itibariyle geçerli olmakla kalmaz, zaman içinde değişebilir. Kuruluşlar, Microsoft ürün ve Hizmetleri zaman içinde geliştikçe kimlik uygulamalarını sürekli olarak değerlendirmelidir.

## <a name="key-operational-processes"></a>Anahtar işletimsel işlemler

### <a name="assign-owners-to-key-tasks"></a>Anahtar görevlere sahipler atama

Azure Active Directory Yönetimi, önemli işlem görevlerinin ve işlemlerin sürekli yürütülmesini gerektirir ve bu, bir dağıtım projesinin parçası olmayabilir. Ortamınızı iyileştirmek için bu görevleri ayarlamanız de önemlidir. Önemli görevler ve bunların önerilen sahipleri şunlardır:

| Görev | Sahip |
| :- | :- |
| Azure AD 'de çoklu oturum açma (SSO) yapılandırmasının yaşam döngüsünü yönetme | IAM Işlemler ekibi |
| Azure AD uygulamaları için koşullu erişim ilkeleri tasarlama | InfoSec mimarisi ekibi |
| SıEM sisteminde arşiv oturum açma etkinliği | InfoSec Işlemler ekibi |
| SıEM sistemindeki risk olaylarını arşivleme | InfoSec Işlemler ekibi |
| Güvenlik raporlarını önceliklendirme ve araştır | InfoSec Işlemler ekibi |
| Risk olaylarını önceliklendirme ve araştırın | InfoSec Işlemler ekibi |
| Azure AD Kimlik Koruması risk ve güvenlik açığı raporları için işaretlenen kullanıcıları önceliklendirme ve araştır | InfoSec Işlemler ekibi |

> [!NOTE]
> Azure AD Kimlik Koruması, bir Azure AD Premium P2 lisansı gerektirir. Gereksinimlerinize uygun lisans bulmak için bkz. [Azure AD ücretsiz ve Azure AD Premium sürümlerinin genel olarak kullanılabilir özelliklerini karşılaştırma](https://azure.microsoft.com/pricing/details/active-directory/).

Listenizi gözden geçirdikten sonra, sahibi eksik olan görevler için bir sahip atamanız veya yukarıdaki önerilere göre hizalanmamış olan sahiplerin sahipliğini ayarlamanız gerekebilir.

#### <a name="owner-recommended-reading"></a>Sahibi tarafından önerilen okuma

- [Azure Active Directory’de yönetici rolü atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)
- [Azure’da idare](https://docs.microsoft.com/azure/security/governance-in-azure)

## <a name="credentials-management"></a>Kimlik bilgileri yönetimi

### <a name="password-policies"></a>Parola ilkeleri

Parolaların güvenli bir şekilde yönetilmesi, kimlik ve erişim yönetiminin en kritik parçalarından biridir ve genellikle saldırıların en büyük hedefleridir. Azure AD, saldırının başarılı olmasını önlemeye yardımcı olabilecek çeşitli özellikleri destekler.

Giderilmesi gereken sorunu azaltmaya yönelik önerilen çözümü bulmak için aşağıdaki tabloyu kullanın:

| Sorun | Öneri |
| :- | :- |
| Zayıf parolalara karşı koruma mekanizması yoktur | Azure AD [self servis parola sıfırlama (SSPR)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks) ve [parola korumasını](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) etkinleştirme |
| Sızdırılan parolaları algılamaya yönelik bir mekanizma yok | Öngörüler kazanmak için [Parola karması eşitlemesini](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) (PHS) etkinleştirin |
| AD FS kullanma ve yönetilen kimlik doğrulamasına taşınamıyor | [AD FS Extranet akıllı kilitleme](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection) ve/veya [Azure AD akıllı kilitleme](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-smart-lockout) 'yi etkinleştirme |
| Parola ilkesi, uzunluk, birden çok karakter kümesi veya süre sonu gibi karmaşıklık tabanlı kurallar kullanır | [Önerilen Microsoft uygulamaları](https://aka.ms/passwordguidance) için göz önünde bulundurun ve parola yönetimine yaklaşımınızı geçirin ve [Azure AD parola korumasını](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad)dağıtın. |
| Kullanıcılar Multi-Factor Authentication (MFA) kullanmak üzere kayıtlı değil | Kullanıcının kimliğini parolasıyla birlikte doğrulama mekanizması olarak kullanılabilmesi için [tüm kullanıcıların güvenlik bilgilerini kaydedin](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-mfa-policy) |
| Kullanıcı riskini temel alan parolaların iptali yoktur | SSPR kullanarak sızdırılan kimlik bilgilerinde parola değişikliklerini zorlamak için Azure AD [kimlik koruması Kullanıcı risk ilkelerini](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy) dağıtma |
| Tanınan IP adreslerinden gelen kötü amaçlı kimlik doğrulamasını korumak için akıllı kilitleme mekanizması yoktur | Parola karması eşitleme veya [geçişli kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) (PTA) ile bulut tarafından yönetilen kimlik doğrulaması dağıtma |

#### <a name="password-policies-recommended-reading"></a>Parola ilkelerinin okunması önerilir

- [Azure AD ve AD FS en iyi uygulamalar: parola spreyi saldırılarına karşı savunma-Enterprise Mobility + Security](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/05/azure-ad-and-adfs-best-practices-defending-against-password-spray-attacks/)

### <a name="enable-self-service-password-reset-and-password-protection"></a>Self servis parola sıfırlama ve parola korumasını etkinleştirme

Parolalarını değiştirmesi veya sıfırlaması gereken kullanıcılar, en büyük birim kaynaklarından ve yardım masası çağrılarının maliyetinden biridir. Maliyete ek olarak, Kullanıcı riskini azaltmak için parolayı bir araç olarak değiştirmek, kuruluşunuzun güvenlik duruşunu geliştirmekte olan temel bir adımdır.

En azından Azure AD [self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks) (SSPR) ve şirket içi [parola korumasını](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-deploy) dağıtmanız önerilir:

- Yardım Masası çağrılarını erteleme.
- Geçici parolaların kullanımını değiştirin.
- Şirket içi bir çözüme dayanan mevcut bir self servis parola yönetimi çözümünü değiştirin.
- Kuruluşunuzdaki [zayıf parolaları kaldırın](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad) .

> [!NOTE]
> Azure AD Premium P2 aboneliği olan kuruluşlar için SSPR 'nin dağıtılması ve [kimlik koruması Kullanıcı risk ilkesinin](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)bir parçası olarak kullanılması önerilir.

### <a name="strong-credential-management"></a>Güçlü kimlik bilgisi yönetimi

Kötü aktörlerin ortamınıza erişim kazanmasını engellemek için kendilerine göre parolalar yeterince güvenli değildir. En azından, ayrıcalıklı hesabı olan tüm kullanıcıların Multi-Factor Authentication (MFA) için etkinleştirilmesi gerekir. İdeal olarak, [Birleşik kayıt](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined) [deneyimini](https://docs.microsoft.com/azure/active-directory/user-help/user-help-security-info-overview)kullanarak tüm kullanıcıların MFA ve SSPR 'ye kaydolması gerekir. Sonuç olarak, öngörülemeyen koşullar nedeniyle kilitleme riskini azaltmak için [esnekliği sağlamak](https://docs.microsoft.com/azure/active-directory/authentication/concept-resilient-controls) üzere bir strateji benimsemenizi öneririz.

![Birleşik Kullanıcı deneyimi akışı](./media/active-directory-ops-guide/active-directory-ops-img4.png)

### <a name="on-premises-outage-authentication-resiliency"></a>Şirket içi kesinti kimlik doğrulama dayanıklılığı

Basitlik avantajlarının yanı sıra, Azure AD Parola karması eşitleme (PHS) ve Azure MFA, kullanıcıların SaaS uygulamalarına ve Office 365 ' de [Notpetya gibi Sife](https://www.microsoft.com/security/blog/2018/02/05/overview-of-petya-a-rapid-cyberattack/)nedeniyle Şirket içi kesintiler artma erişmesini sağlar. Ayrıca, Federasyonla birlikte da PHS 'yi etkinleştirmek mümkündür. Fes 'nin etkinleştirilmesi, Federasyon Hizmetleri kullanılabilir olmadığında kimlik doğrulamanın geri yüklenmesine izin verir.

Şirket içi kuruluşunuzda bir kesinti dayanıklılık stratejisi yoksa veya Azure AD ile tümleştirilmiş olmayan bir sorun varsa, Azure AD PHS 'yi dağıtmanız ve PHS içeren bir olağanüstü durum kurtarma planı tanımlamanız gerekir. Azure AD PHS 'in etkinleştirilmesi, kullanıcıların Azure AD 'de kimlik doğrulamasından geçmesini sağlar ve şirket içi Active Directory kullanılamaz hale gelmelidir.

![Parola karması eşitleme akışı](./media/active-directory-ops-guide/active-directory-ops-img5.png)

Kimlik doğrulama seçeneklerinizi daha iyi anlamak için bkz. [Azure Active Directory hibrit kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçme](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).

### <a name="programmatic-usage-of-credentials"></a>Kimlik bilgilerinin programlı kullanımı

Graph API kullanarak PowerShell veya uygulamaları kullanan Azure AD betikleri güvenli kimlik doğrulaması gerektirir. Bu komut dosyalarını ve araçları yürüten zayıf kimlik bilgisi yönetimi, kimlik bilgilerinin hırsızlık riskini artırır. Sabit kodlanmış parolalara veya parola istemlerine dayalı betikler veya uygulamalar kullanıyorsanız, öncelikle yapılandırma dosyalarındaki veya kaynak kodundaki parolaları gözden geçirmeniz, ardından bu bağımlılıkları değiştirmeniz ve mümkün olduğunda Azure Yönetilen kimlikler, tümleşik-Windows kimlik doğrulaması veya [Sertifikalar](https://docs.microsoft.com/azure/active-directory/reports-monitoring/tutorial-access-api-with-certificates) kullanmanız gerekir. Önceki çözümlerin mümkün olmadığı uygulamalar için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)kullanmayı göz önünde bulundurun.

Parola kimlik bilgileri ile hizmet sorumluları olduğunu tespit ederseniz ve bu parola kimlik bilgilerinin betikler veya uygulamalar tarafından nasıl sağlandığı hakkında emin değilseniz, kullanım düzenlerini daha iyi anlamak için uygulamanın sahibine başvurun.

Microsoft ayrıca, parola kimlik bilgilerine sahip hizmet sorumluları varsa kullanım düzenlerini anlamak için uygulama sahiplerine başvurmanızı da önerir.

## <a name="authentication-experience"></a>Kimlik doğrulama deneyimi

### <a name="on-premises-authentication"></a>Şirket içi kimlik doğrulaması

Tümleşik Windows kimlik doğrulaması (ıWA) veya kesintisiz çoklu oturum açma (SSO) ile yönetilen kimlik doğrulaması ile, şirket ağının içinde bulunan en iyi kullanıcı deneyimi Şirket içi etki alanı denetleyicilerine görüş satırı. Kimlik bilgisi istemi FAG 'sini en aza indirir ve kullanıcıların kimlik avı saldırılarına karşı daha fazla erişme riskini azaltır. Daha önce PHS veya PTA ile bulut tarafından yönetilen kimlik doğrulaması kullanıyorsanız, ancak kullanıcıların şirket içinde kimlik doğrulaması yaparken parolalarını yazmaları gerekiyorsa, [sorunsuz SSO](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso)'yu hemen dağıtmanız gerekir. Diğer taraftan, şu anda, son olarak bulut tarafından yönetilen kimlik doğrulamasına geçirmeye yönelik planlarınız varsa, geçiş projesinin bir parçası olarak sorunsuz SSO uygulamanız gerekir.

### <a name="device-trust-access-policies"></a>Cihaz güven erişimi ilkeleri

Kuruluşunuzdaki bir kullanıcı gibi, bir cihaz da korumak istediğiniz çekirdek kimliktir. Kaynaklarınızı istediğiniz zaman ve herhangi bir konumdan korumak için bir cihazın kimliğini kullanabilirsiniz. Dağıtım türü için cihazın ve hesaplama kimlik doğrulaması, güvenlik sonrası ve kullanışlarınızı şu şekilde geliştirir:

- Örneğin, cihaz güvenildiği zaman MFA ile, örneğin, uçuşmaktan kaçınma
- Güvenilmeyen cihazlardan erişimi engelleme
- Windows 10 cihazlarında, [Şirket içi kaynaklarda çoklu oturum açmayı sorunsuz bir şekilde](https://docs.microsoft.com/azure/active-directory/devices/azuread-join-sso)sağlayın.

Aşağıdaki yöntemlerden birini kullanarak cihaz kimliklerini ve bunları Azure AD 'de yöneterek bu hedefi gerçekleştirebilirsiniz:

- Kuruluşlar, cihazı yönetmek ve uyumluluk ilkelerini zorlamak, cihaz durumunu doğrulamak ve cihazın uyumlu olup olmadığına bağlı olarak koşullu erişim ilkeleri ayarlamak için [Microsoft Intune](https://docs.microsoft.com/intune/what-is-intune) kullanabilir. Microsoft Intune iOS cihazlarını, Mac masaüstlerini (JAMF tümleştirmesi aracılığıyla), Windows Masaüstü 'nü (Windows 10 için mobil cihaz yönetimini ve Microsoft Endpoint Manager/System Center Configuration Manager ile birlikte ortak yönetimi kullanarak) ve Android mobil cihazlarını yönetebilir.
- [Hibrit Azure AD JOIN](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-managed-domains) , etki alanına katılmış bilgisayarlar Active Directory bir ortamda grup ilkeleri, System Center Configuration Manager veya Microsoft Uç Nokta Yöneticisi ile yönetim sağlar. Kuruluşlar, sorunsuz SSO ile PHS ya da PTA aracılığıyla yönetilen bir ortam dağıtabilir. Cihazlarınızı Azure AD 'ye getirmek, bulut ve şirket içi kaynaklarınız üzerindeki SSO aracılığıyla Kullanıcı üretkenliğini en üst düzeye çıkarır ve [koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) aynı anda bulut ve şirket içi kaynaklarınız için erişim sağlamanıza olanak tanır.

Bulutta kayıtlı olmayan, ancak koşullu erişim ilkeleri olmadan etki alanına katılmış Windows cihazlarına veya bulutta kayıtlı olan, etki alanına katılmış Windows cihazlarınız varsa, kayıtsız cihazları kaydetmeniz ve her iki durumda da, koşullu erişim ilkeleriniz üzerinde [bir denetim olarak karma Azure AD JOIN 'i kullanmanız](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) gerekir.

![Karma cihaz gerektiren koşullu erişim ilkesinde verme 'nin ekran görüntüsü](./media/active-directory-ops-guide/active-directory-ops-img6.png)

Cihazları MDM veya Microsoft Intune ile yönetiyorsanız, ancak koşullu erişim ilkelerinizin cihaz denetimlerini kullanmıyorsanız, cihazın bu ilkelerde denetim olarak [uyumlu olarak Işaretlenmesini gerektir](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices#require-device-to-be-marked-as-compliant) seçeneğini kullanmanızı öneririz.

![Koşullu erişim ilkesinde cihaz uyumluluğu gerektiren verme 'nin ekran görüntüsü](./media/active-directory-ops-guide/active-directory-ops-img7.png)

#### <a name="device-trust-access-policies-recommended-reading"></a>Okuma için önerilen cihaz güven erişimi ilkeleri

- [Nasıl yapılır: karma Azure Active Directory JOIN Uygulamanızı planlayın](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan)
- [Kimlik ve cihaz erişim yapılandırmaları](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

### <a name="windows-hello-for-business"></a>İş İçin Windows Hello

Windows 10 ' da, [iş Için Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) , bilgisayarlarda güçlü iki öğeli kimlik doğrulama ile parolaları değiştirir. Iş için Windows Hello, kullanıcılar için daha kolay bir MFA deneyimi sunar ve parolalarınızın parolasını azaltır. Windows 10 cihazlarını kullanıma almadıysanız veya yalnızca kısmen dağıttıysanız, Windows 10 ' a yükseltmenizi ve tüm cihazlarda [iş Için Windows Hello 'yu etkinleştirmenizi](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-manage-in-organization) öneririz.

Parolasız kimlik doğrulama hakkında daha fazla bilgi edinmek istiyorsanız, [Azure Active Directory ile parolalar olmadan bir dünya](https://aka.ms/passwordlessdoc)bölümüne bakın.

## <a name="application-authentication-and-assignment"></a>Uygulama kimlik doğrulaması ve atama

### <a name="single-sign-on-for-apps"></a>Uygulamalar için çoklu oturum açma

Tüm kuruluş için standartlaştırılmış bir çoklu oturum açma mekanizması sağlamak, en iyi kullanıcı deneyimi, risk azaltma, rapor verme ve idare açısından önemlidir. Azure AD ile SSO 'yu destekleyen, ancak şu anda yerel hesapları kullanacak şekilde yapılandırılmış uygulamalar kullanıyorsanız, bu uygulamaları Azure AD ile SSO kullanacak şekilde yeniden yapılandırmanız gerekir. Benzer şekilde, Azure AD ile SSO 'yu destekleyen, ancak başka bir kimlik sağlayıcısı kullanan uygulamalar kullanıyorsanız, bu uygulamaları Azure AD ile SSO kullanacak şekilde yeniden yapılandırmanız gerekir. Federasyon protokollerini desteklemeyen ancak form tabanlı kimlik doğrulamasını destekleyen uygulamalar için, uygulamayı Azure AD Uygulama Ara Sunucusu ile [parola](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-password-vaulting) kullanımını kullanacak şekilde yapılandırmanızı öneririz.

![AppProxy parola tabanlı oturum açma](./media/active-directory-ops-guide/active-directory-ops-img8.png)

> [!NOTE]
> Kuruluşunuzda yönetilmeyen uygulamaları bulmak için bir mekanizmanız yoksa, [Microsoft Cloud App Security](https://www.microsoft.com/enterprise-mobility-security/cloud-app-security)gibi bir bulut erişim güvenlik Aracısı çözümü (casb) kullanarak bir bulma işlemi uygulamanızı öneririz.

Son olarak, Azure AD uygulama galeriniz varsa ve Azure AD ile SSO 'yu destekleyen uygulamalar kullanıyorsanız, [uygulamayı uygulama galerisinde listelemeyi](https://docs.microsoft.com/azure/active-directory/develop/howto-app-gallery-listing)öneririz.

#### <a name="single-sign-on-recommended-reading"></a>Okumanız için çoklu oturum açma önerilir

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

### <a name="migration-of-ad-fs-applications-to-azure-ad"></a>AD FS uygulamalarının Azure AD 'ye geçirilmesi

[Uygulamaları AD FS 'Den Azure AD 'ye geçirmek](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-apps-to-azure) , güvenlik, daha tutarlı yönetilebilirlik ve daha iyi işbirliği deneyimi konusunda ek yetenekler sağlar. Azure AD ile SSO 'yu destekleyen AD FS ' de yapılandırılmış uygulamalarınız varsa, bu uygulamaları Azure AD ile SSO kullanacak şekilde yeniden yapılandırmanız gerekir. Azure AD tarafından desteklenmeyen yaygın olmayan yapılandırmalarda AD FS yapılandırılmış uygulamalarınız varsa, özel yapılandırmanın uygulamanın mutlak bir gereksinimi olup olmadığını anlamak için uygulama sahiplerine başvurmanız gerekir. Gerekli değilse, uygulamayı Azure AD ile SSO kullanacak şekilde yeniden yapılandırmanız gerekir.

![Birincil kimlik sağlayıcısı olarak Azure AD](./media/active-directory-ops-guide/active-directory-ops-img9.png)

> [!NOTE]
> [ADFS için Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs) , Azure AD 'ye geçirilebilecek olabilecek her uygulamayla ilgili yapılandırma ayrıntılarını toplamak üzere kullanılabilir.

### <a name="assign-users-to-applications"></a>Uygulamalara Kullanıcı atama

[Kullanıcılara uygulamalara atama](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups) , daha fazla esneklik ve ölçeğe göre yönetme olanağı sağladığından, gruplar kullanılarak en iyi şekilde eşleştirilir. Grupları kullanmanın avantajları, [öznitelik tabanlı dinamik grup üyeliği](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership) ve [uygulama sahiplerine temsilciliğini](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-accessmanagement-managing-group-owners)içerir. Bu nedenle, zaten grupları kullanıyorsanız ve yönetiyorsanız yönetimi artırmak için aşağıdaki işlemleri yapmanızı öneririz:

- Uygulama sahiplerine Grup Yönetimi ve idare verme.
- Uygulamaya self servis erişimine izin verin.
- Kullanıcı özniteliklerinin uygulamalara erişimi tutarlı bir şekilde belirleyebilmesi halinde dinamik grupları tanımlayın.
- [Azure AD erişim gözden geçirmeleri](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)kullanarak uygulama erişimi için kullanılan gruplara kanıtlama uygulayın.

Öte yandan, bireysel kullanıcılara atama yapan uygulamalar bulursanız, bu uygulamaların etrafında [idare](https://docs.microsoft.com/azure/active-directory/governance/index) uyguladığınızdan emin olun.

#### <a name="assign-users-to-applications-recommended-reading"></a>Kullanıcıları okumak için önerilen uygulamalara atama

- [Azure Active Directory bir uygulamaya Kullanıcı ve Grup atama](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups)
- [Azure Active Directory 'de uygulama kayıt izinleri verme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-delegate-app-roles)
- [Azure Active Directory gruplar için dinamik üyelik kuralları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)

## <a name="access-policies"></a>Erişim ilkeleri

### <a name="named-locations"></a>Adlandırılmış konumlar

Azure AD 'de [adlandırılmış konumlar](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations) sayesinde, KURULUŞUNUZDA güvenilir IP adresi aralıklarını etiketleyebilir. Azure AD, aşağıdakileri yapmak için adlandırılmış konumları kullanır:

- Risk olaylarında yanlış pozitif sonuçlar önleyin. Güvenilen bir ağ konumundan oturum açmak, kullanıcının oturum açma riskini azaltır.
- [Konum tabanlı koşullu erişimi](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)yapılandırın.

![Adlandırılmış konum](./media/active-directory-ops-guide/active-directory-ops-img10.png)

Önceliğe göre, kuruluşunuzun ihtiyaçlarını en iyi şekilde karşılayan önerilen çözümü bulmak için aşağıdaki tabloyu kullanın:

| **Öncelik** | **Senaryo** | **Öneri** |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| 1 | PHS veya PTA kullanıyorsanız ve adlandırılmış konumlar tanımlanmamışsa | Risk olaylarının algılanmasını geliştirmek için adlandırılmış konumları tanımlayın |
| 2 | Federe çalışıyorsanız ve "insideCorporateNetwork" talebini kullanmıyorsanız ve adlandırılmış konumlar tanımlanmamışsa | Risk olaylarının algılanmasını geliştirmek için adlandırılmış konumları tanımlayın |
| 3 | Koşullu erişim ilkelerinde adlandırılmış konumlar kullanmıyorsanız ve koşullu erişim ilkelerinde risk veya cihaz denetimi yoksa | Koşullu erişim ilkesini adlandırılmış konumları içerecek şekilde yapılandırma |
| 4 | Federe çalışıyorsanız ve "insideCorporateNetwork" talebini kullanıyorsanız ve adlandırılmış konumlar tanımlanmamışsa | Risk olaylarının algılanmasını geliştirmek için adlandırılmış konumları tanımlayın |
| 5 | Adlandırılmış konumlar yerine MFA ile güvenilir IP adresleri kullanıyorsanız ve bunları güvenilir olarak işaretlemek için | Risk olaylarının algılanmasını geliştirmek için adlandırılmış konumları tanımlayın ve bunları güvenilir olarak işaretleyin |

### <a name="risk-based-access-policies"></a>Risk tabanlı erişim ilkeleri

Azure AD, her oturum açma ve Kullanıcı için riskleri hesaplayabilir. Erişim ilkelerinde bir ölçüt olarak riskin kullanılması daha iyi bir kullanıcı deneyimi sağlayabilir, örneğin, daha az kimlik doğrulama istemi ve daha iyi güvenlik, örneğin, yalnızca gerektiğinde kullanıcılara istem ve yanıt ve düzeltmeyi otomatikleştirin.

![Oturum açma risk ilkesi](./media/active-directory-ops-guide/active-directory-ops-img11.png)

Erişim ilkelerindeki risk kullanımını destekleyen Azure AD Premium P2 lisanslarınız zaten varsa, ancak kullanılmazlar, güvenlik duruşunuzda risk eklenmesini kesinlikle öneririz.

#### <a name="risk-based-access-policies-recommended-reading"></a>Risk tabanlı erişim ilkelerinin okunması önerilir

- [Nasıl yapılır: oturum açma risk ilkesini yapılandırma](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-sign-in-risk-policy)
- [Nasıl yapılır: Kullanıcı risk ilkesini yapılandırma](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)

### <a name="client-application-access-policies"></a>İstemci uygulama erişim ilkeleri

Microsoft Intune uygulama yönetimi (MAM), depolama şifrelemesi, PIN, uzak depolama temizleme vb. gibi veri koruma denetimlerini Outlook Mobile gibi uyumlu istemci mobil uygulamalarına iletme yeteneği sağlar. Ayrıca, onaylanan veya uyumlu uygulamalardan Exchange Online gibi bulut hizmetlerine [erişimi kısıtlamak](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) için koşullu erişim ilkeleri oluşturulabilir.

Çalışanlarınız, Exchange Online veya SharePoint Online gibi şirket kaynaklarına erişmek için Office mobil uygulamaları gibi MAM özellikli uygulamalar yüklerse ve KCG 'yi (kendi cihazını getir) desteklemeniz durumunda, yönetilecek uygulama MAM ilkelerini dağıtmanıza önerilir Kişisel cihazlarda MDM kaydı olmadan uygulama yapılandırması ve sonra koşullu erişim ilkelerinizi yalnızca MAM özellikli istemcilerden erişime izin verecek şekilde güncelleştirin.

![Koşullu erişim Izni denetimi](./media/active-directory-ops-guide/active-directory-ops-img12.png)

Çalışanların kurumsal kaynaklara karşı MAM özellikli uygulamalar yüklemesi ve erişim Intune yönetilen cihazlarda kısıtlanması gerekir, bu durumda, kişisel cihazların uygulama yapılandırmasını yönetmek için uygulama MAM ilkeleri dağıtımı yapmayı düşünmelisiniz ve Koşullu erişim ilkelerini yalnızca MAM özellikli istemcilerden erişime izin verecek şekilde güncelleştirin.

### <a name="conditional-access-implementation"></a>Koşullu erişim uygulama

Koşullu erişim, kuruluşunuzun güvenlik duruşunu iyileştirmek için gereken önemli bir araçtır. Bu nedenle, bu en iyi yöntemleri izlemeniz önemlidir:

- Tüm SaaS uygulamalarının en az bir ilkenin uygulanmış olduğundan emin olun
- Kilitleme riskini önlemek için **tüm uygulamalar** filtresini **blok** denetimiyle birleştirmemeye özen gösterin
- **Tüm kullanıcıları** filtre olarak kullanmaktan kaçının ve yanlışlıkla **konukları** ekleyin
- **Tüm "eski" ilkeleri Azure portal geçirin**
- Kullanıcılar, cihazlar ve uygulamalar için tüm ölçütleri yakala
- **Kullanıcı BAŞıNA MFA** kullanmak yerine [MFA uygulamak](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access)için koşullu erişim ilkelerini kullanın
- Birden çok uygulama için uygulanabilecek küçük bir çekirdek ilke kümesine sahiptir
- Boş özel durum gruplarını tanımlayın ve özel durum stratejisi sağlamak için bunları ilkelere ekleyin
- MFA denetimleri olmadan [kesme camı](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure#break-glass-what-to-do-in-an-emergency) hesaplarını planlayın
- Office 365 istemci uygulamalarında tutarlı bir deneyim sağlayın. Örneğin, takımlar, OneDrive Iş, Outlook vb.) Exchange Online ve SharePoint Online gibi hizmetler için aynı denetim kümesini uygulayarak
- İlkelere atama, bireyler değil, gruplar aracılığıyla uygulanmalıdır
- İlkelerde kullanılan özel durum gruplarını, kullanıcıların güvenlik sonrası çıkış süresini sınırlandırmak için düzenli olarak gözden geçirin. Azure AD P2 sahibiyseniz, işlemi otomatikleştirmek için erişim gözden geçirmeleri kullanabilirsiniz

#### <a name="conditional-access-recommended-reading"></a>Koşullu erişimin okunması önerilir

- [Azure Active Directory 'de koşullu erişim için en iyi yöntemler](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices)
- [Kimlik ve cihaz erişim yapılandırmaları](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Azure Active Directory Koşullu erişim ayarları başvurusu](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference)
- [Ortak koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common)

## <a name="access-surface-area"></a>Erişim yüzeyi alanı

### <a name="legacy-authentication"></a>Eski kimlik doğrulaması

MFA gibi güçlü kimlik bilgileri, kötü amaçlı aktörler tarafından tercih edilen saldırı vektörünü yapan eski kimlik doğrulama protokollerini kullanarak uygulamaları koruyamaz. Eski kimlik doğrulamasının kilitlenmesi, erişim güvenlik duruşunu geliştirmek için önemlidir.

Eski kimlik doğrulaması, şunun gibi uygulamalar tarafından kullanılan kimlik doğrulama protokollerine başvuran bir terimdir:

- Modern kimlik doğrulaması kullanmayan eski Office istemcileri (örneğin, Office 2010 istemcisi)
- IMAP/SMTP/POP gibi posta protokollerini kullanan istemciler

Saldırganlar bu protokolleri kesinlikle tercih ediyor-aslında yaklaşık [%100 parola spreyi saldırıları](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984) eski kimlik doğrulama protokollerini kullanır! Saldırganlar, çok faktörlü kimlik doğrulaması ve cihaz kimlik doğrulaması gibi ek güvenlik sorunları için gereken etkileşimli oturum açmayı desteklemediğinden, eski kimlik doğrulama protokollerini kullanır.

Ortamınızda eski kimlik doğrulama yaygın olarak kullanılıyorsa, eski istemcileri [modern kimlik doğrulamasını](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) destekleyen istemcilere en kısa sürede geçirmeyi planlamalısınız. Aynı belirteçte, bazı kullanıcılarınız zaten modern kimlik doğrulaması kullanıyor, ancak diğerleri eski kimlik doğrulamasını kullanmaya devam ediyorsa, eski kimlik doğrulama istemcilerini kilitlemek için aşağıdaki adımları uygulamanız gerekir:

1. Hala eski kimlik doğrulama ve plan düzeltmesini kullanan kullanıcıları tanımlamak için [oturum açma etkinlik raporlarını](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) kullanın:

   a. Etkilenen kullanıcılara modern kimlik doğrulaması özellikli istemcileri yükseltin.
   
   b. Aşağıdaki adımları uygulamak için bir tam geçişi zaman çerçevesi planlayın.
   
   c. Eski uygulamaların eski kimlik doğrulaması üzerinde ne kadar eski bir bağımlılığı olduğunu belirler. Aşağıdaki 3. adıma bakın.

2. Daha fazla pozlamayı önlemek için eski kimlik doğrulaması kullanmayan kullanıcılar için kaynaktaki eski protokolleri devre dışı bırakın (Örneğin Exchange posta kutusu).
3. Kalan hesaplar için (hizmet hesapları gibi ideal insan dışı kimlikler), eski protokollerin kimlik doğrulama sonrası olduğunu [kısıtlamak için koşullu erişim](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Conditional-Access-support-for-blocking-legacy-auth-is/ba-p/245417) kullanın.

#### <a name="legacy-authentication-recommended-reading"></a>Eski kimlik doğrulaması önerilir

- [Exchange Server 'da posta kutularına POP3 veya ıMAP4 erişimini etkinleştirme veya devre dışı bırakma](https://docs.microsoft.com/exchange/clients/pop3-and-imap4/configure-mailbox-access?view=exchserver-2019)

### <a name="consent-grants"></a>İzin verir

Bir ıllicıt onayı verme saldırısında, saldırgan, iletişim bilgileri, e-posta veya belgeler gibi verilere erişim isteyen bir Azure AD 'ye kayıtlı uygulama oluşturur. Kullanıcılar, kimlik avı saldırıları aracılığıyla kötü amaçlı uygulamalara izin verebilir veya kötü amaçlı Web sitelerindeki giriş sırasında dikkatli olmaz.

Aşağıda, Microsoft bulut hizmetleri için Scrutinize isteyebileceğiniz izinler verilmiştir:

- Uygulama veya Temsilcili \*uygulamalar. ReadWrite Izinleri
- Temsilci izinleri olan uygulamalar Kullanıcı adına e-posta okuyabilir, gönderebilir veya yönetebilir
- Aşağıdaki izinleri kullanarak verilen uygulamalar:

| Kaynak | İzin |
| -------------------------- | -------------------- |
| Office 365 Exchange Online | Olduğundan. AccessAsUser. All |
| | EWS. AccessAsUser. All |
| | Mail. Read |
| Microsoft Graph | Mail. Read |
| | Mail. Read. Shared |
| | Mail. ReadWrite |

Bu senaryoya engel olmak için, Office 365 ' de sahip olduğu tüm uygulamaları belirlemek ve düzeltmek için gerekli olandan daha fazla izin veren uygulamalar belirlemek ve düzeltmek üzere [Office ' de Illicit onay Iznini Algıla ve](https://docs.microsoft.com/office365/securitycompliance/detect-and-remediate-illicit-consent-grants) Düzelt bölümüne başvurmalısınız. Uygulama izinleri için düzenli İncelemeleri zamanlayın ve gerekli olmadığında kaldırın; veya self servis hizmetini tamamen kaldırın ve idare yordamları oluşturun.

#### <a name="consent-grants-recommended-reading"></a>Onay, önerilen okumaya izin veriyor

- [Azure Active Directory (AD) Graph API Izin kapsamları](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes)

### <a name="user-and-group-settings"></a>Kullanıcı ve grup ayarları

Aşağıda açık bir işletme gereksinimi yoksa kilitlenebilen Kullanıcı ve grup ayarları verilmiştir:

#### <a name="user-settings"></a>Kullanıcı ayarları

- **Dış kullanıcılar** -dış Işbirliği, ekipler, Power BI, SharePoint Online ve Azure Information Protection gibi hizmetlerle kurumsal olarak şirket içinde gerçekleşebilir. Kullanıcı tarafından başlatılan dış işbirliğini denetlemek için açık kısıtlamalarınız varsa, [Azure AD yetkilendirme yönetimini](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) veya yardım masanıza gibi denetimli bir işlemi kullanarak dış kullanıcıları etkinleştirmeniz önerilir. Hizmetler için organik dış işbirliğine izin vermek istemiyorsanız, [üyelerin dış kullanıcıları tamamen davet](https://docs.microsoft.com/azure/active-directory/b2b/delegate-invitations)edebilir. Alternatif olarak, dış Kullanıcı davetlerinde [belirli etki alanlarına da izin verebilir veya onları engelleyebilirsiniz](https://docs.microsoft.com/azure/active-directory/b2b/allow-deny-list) .
- **Uygulama kayıtları** -uygulama kayıtları etkinleştirildiğinde, son kullanıcılar uygulamaları kendi kendilerine ekleyebilir ve verilerine erişim verebilir. Uygulama kaydının tipik bir örneği, Outlook eklentilerini veya Alexa ve Siri gibi sesli yardımcıların kendi e-postalarını ve takvimini okumasını ya da kendi adına e-posta göndermesini sağlayan kullanıcılardır. Müşteri, uygulama kaydını kapatmaya karar verirse, INFOSEC ve ıAM ekipleri, uygulamaları yönetici hesabıyla kaydetmesi gereken özel durumların yönetimine (iş gereksinimlerine göre gereken uygulama kayıtları) dahil olmalıdır, ve büyük olasılıkla işlemi gerçekleştirmek için bir işlem tasarlamaya gerek duyar.
- **Yönetim Portalı** -kuruluşlar Azure Portal Azure AD dikey penceresini kilitleyerek, yönetici olmayan Azure Portal Azure AD yönetimine erişemez ve karıştırılır. Erişimi kısıtlamak için Azure AD yönetim portalındaki Kullanıcı ayarlarına gidin:

![Yönetim Portalı kısıtlı erişim](./media/active-directory-ops-guide/active-directory-ops-img13.png)

> [!NOTE]
> Yöneticiler olmayanlar, komut satırı ve diğer programlı arabirimler aracılığıyla Azure AD Yönetim arabirimlerine erişmeye devam edebilir.

#### <a name="group-settings"></a>Grup ayarları

**Self Servis Grup Yönetimi/kullanıcıları, güvenlik grupları/O365 grupları oluşturabilir.** Bulutta gruplar için geçerli self servis girişimi yoksa, müşteriler bu özelliği kullanmaya hazırlanana kadar devre dışı bırakma kararı verebilir.

#### <a name="groups-recommended-reading"></a>Okumanız önerilen gruplar

- [Azure Active Directory B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Uygulamaları Azure Active Directory tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)
- [Azure Active Directory uygulamalar, izinler ve onay.](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)
- [Azure Active Directory içindeki kaynaklara erişimi yönetmek için grupları kullanma](https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups)
- [Azure Active Directory içinde self servis uygulama erişim yönetimini ayarlama](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management)

### <a name="traffic-from-unexpected-locations"></a>Beklenmeyen konumlardan gelen trafik

Saldırganlar dünyanın çeşitli parçalarından geliyor. Koşul olarak konum ile koşullu erişim ilkeleri kullanarak bu riski yönetin. Bir koşullu erişim ilkesinin [konum koşulu](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition) , ' den oturum açmak için herhangi bir iş nedeni olmayan konumlara erişimi engellemenize olanak sağlar.

![Yeni bir adlandırılmış konum oluştur](./media/active-directory-ops-guide/active-directory-ops-img14.png)

Varsa, bölgeler arasında erişim düzenlerini çözümlemek ve bulmak için bir güvenlik bilgileri ve olay yönetimi (SıEM) çözümü kullanın. SıEM ürünü kullanmıyorsanız veya Azure AD 'den kimlik doğrulama bilgilerini geri almaya memişse, bölgeler arasında erişim düzenlerini belirlemek için [Azure izleyici](https://docs.microsoft.com/azure/azure-monitor/overview) kullanmanızı öneririz.

## <a name="access-usage"></a>Erişim kullanımı

### <a name="azure-ad-logs-archived-and-integrated-with-incident-response-plans"></a>Azure AD günlükleri arşivlenmiş ve olay yanıt planlarıyla tümleşiktir

Oturum açma etkinliğine erişim, Azure AD için denetimler ve risk olayları, sorun giderme, Kullanım Analizi ve adli araştırmalar açısından önemlidir. Azure AD, sınırlı saklama süresine sahip REST API 'Leri aracılığıyla bu kaynaklara erişim sağlar. Bir güvenlik bilgileri ve olay yönetimi (SıEM) sistemi veya eşdeğer arşivleme teknolojisi, denetim ve desteklenebilirlik için uzun süreli depolamaya yönelik bir anahtardır. Azure AD günlüklerinin uzun süreli depolanmasını etkinleştirmek için bunları mevcut SıEM çözümünüze eklemeniz veya [Azure izleyici](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor)'yi kullanmanız gerekir. Olay yanıt planlarınızın ve araştırmalarınızın bir parçası olarak kullanılabilen arşiv günlükleri.

#### <a name="logs-recommended-reading"></a>Günlük okuma önerilir

- [Azure Active Directory Denetim API 'SI başvurusu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference)
- [Azure Active Directory oturum açma etkinliği rapor API 'SI başvurusu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-sign-in-activity-reference)
- [Sertifikalarla Azure AD Raporlama API 'sini kullanarak veri edinme](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-with-certificates)
- [Azure Active Directory Kimlik Koruması için Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)
- [Office 365 yönetimi etkinlik API 'SI başvurusu](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference)
- [Azure Active Directory Power BI Içerik paketini kullanma](../reports-monitoring/howto-use-azure-monitor-workbooks.md)

## <a name="summary"></a>Özet

Güvenli bir kimlik altyapısının 12 yönü vardır. Bu liste, kimlik bilgilerini daha güvenli ve yönetebilir, kimlik doğrulama deneyimini tanımlamanızı, atama atamasını, kullanım ölçümünü ve kurumsal güvenlik duruşunu temel alan erişim ilkeleri tanımlamanıza yardımcı olur.

- Anahtar görevlere sahip atayın.
- Zayıf veya sızdırılan parolaları algılama, parola yönetimini ve korumayı geliştirme ve kaynaklara daha fazla güvenli Kullanıcı erişimi sağlamak için çözümler uygulayın.
- Kaynaklarınızı istediğiniz zaman ve herhangi bir konumdan korumak için cihazların kimliğini yönetin.
- Passwordless kimlik doğrulaması uygulayın.
- Kuruluş genelinde standartlaştırılmış bir çoklu oturum açma mekanizması sağlayın.
- Daha iyi güvenlik ve daha tutarlı yönetilebilirlik sağlamak için AD FS uygulamaları Azure AD 'ye geçirin.
- Daha fazla esneklik ve ölçeğe göre yönetme olanağı sağlamak için grupları kullanarak uygulamalara Kullanıcı atama.
- Risk tabanlı erişim ilkelerini yapılandırın.
- Eski kimlik doğrulama protokollerini kilitle.
- Illicıt onayını tespit edin ve düzeltin.
- Kullanıcı ve grup ayarlarını kilitle.
- Sorun giderme, Kullanım Analizi ve Forli ICS araştırmaları için Azure AD günlüklerinin uzun süreli depolanmasını etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

[Kimlik idare işlem denetimleri ve eylemleri](active-directory-ops-guide-govern.md)ile çalışmaya başlayın.
