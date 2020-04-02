---
title: Azure Active Directory Authentication management işlemleri başvuru kılavuzu
description: Bu işlemler başvuru kılavuzu, kimlik doğrulama yönetimini güvence altına almak için çekmeniz gereken denetimleri ve eylemleri açıklar
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
ms.openlocfilehash: 8b4ec003888d75a582d25feef8ed2ce010fa7996
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546243"
---
# <a name="azure-active-directory-authentication-management-operations-reference-guide"></a>Azure Active Directory Authentication management işlemleri başvuru kılavuzu

Azure AD [işlemleri başvuru kılavuzunun](active-directory-ops-guide-intro.md) bu bölümünde, kimlik bilgilerini güvenli hale getirmek ve yönetmek, kimlik doğrulama deneyimini tanımlamak, temsilci ataması yapmak, kullanımı ölçmek ve kurumsal güvenlik duruşuna dayalı erişim ilkelerini tanımlamak için almanız gereken denetimleri ve eylemleri açıklar.

> [!NOTE]
> Bu öneriler yayımlama tarihi itibariyle geçerlidir, ancak zaman içinde değişebilir. Kuruluşlar, Microsoft ürünleri ve hizmetleri zaman içinde geliştikçe kimlik uygulamalarını sürekli olarak değerlendirmelidir.

## <a name="key-operational-processes"></a>Temel operasyonel süreçler

### <a name="assign-owners-to-key-tasks"></a>Anahtar görevlere sahipleri atama

Azure Active Directory'nin yönetimi, kullanıma sunulması projesinin parçası olmayabilecek önemli operasyonel görevlerin ve süreçlerin sürekli olarak yürütülmesini gerektirir. Ortamınızı en iyi duruma getirmek için bu görevleri ayarlamanız yine de önemlidir. Önemli görevler ve önerilen sahipleri şunlardır:

| Görev | Sahip |
| :- | :- |
| Azure AD'de tek oturum açma (SSO) yapılandırmasının yaşam döngüsünü yönetme | IAM Operasyon Ekibi |
| Azure AD uygulamaları için koşullu erişim ilkeleri tasarlama | InfoSec Mimarlık Ekibi |
| SIEM sisteminde arşiv oturum açma etkinliği | InfoSec Operasyon Ekibi |
| SIEM sistemindeki risk olaylarını arşivleme | InfoSec Operasyon Ekibi |
| Güvenlik raporlarını nida etmek ve araştırmak | InfoSec Operasyon Ekibi |
| Risk olaylarını nida etmek ve araştırmak | InfoSec Operasyon Ekibi |
| Azure AD Kimlik Koruması'ndan risk ve güvenlik açığı raporları için işaretlenmiş kullanıcıları üçleme ve araştırma | InfoSec Operasyon Ekibi |

> [!NOTE]
> Azure AD Kimlik Koruması için Azure AD Premium P2 lisansı gerekir. Gereksinimleriniz için doğru lisansı bulmak için bkz. [Azure AD Free ve Azure AD Premium sürümlerinin genel olarak kullanılabilir özelliklerini karşılaştırma.](https://azure.microsoft.com/pricing/details/active-directory/)

Listenizi gözden geçirerken, sahibi eksik olan görevler için bir sahip atamanız veya yukarıdaki önerilerle uyumlu olmayan sahiplerle olan görevlerin sahipliğini ayarlamanız gerekebilir.

#### <a name="owner-recommended-reading"></a>Sahibi okuma tavsiye

- [Azure Active Directory’de yönetici rolü atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)
- [Azure’da idare](https://docs.microsoft.com/azure/security/governance-in-azure)

## <a name="credentials-management"></a>Kimlik bilgileri yönetimi

### <a name="password-policies"></a>Parola ilkeleri

Parolaları güvenli bir şekilde yönetmek, kimlik ve erişim yönetiminin en kritik parçalarından biridir ve genellikle saldırıların en büyük hedefidir. Azure AD, bir saldırının başarılı olmasını önlemeye yardımcı olabilecek çeşitli özellikleri destekler.

Ele alınması gereken sorunu hafifletmek için önerilen çözümü bulmak için aşağıdaki tabloyu kullanın:

| Sorun | Öneri |
| :- | :- |
| Zayıf parolalara karşı koruma mekanizması yok | Azure AD [self servis parola sıfırlama (SSPR)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks) ve parola [korumayı](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) etkinleştirme |
| Sızdırılan parolaları algılatacak mekanizma yok | Öngörüler elde etmek için [parola karma eşitleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) (PHS) etkinleştirme |
| AD FS kullanma ve yönetilen kimlik doğrulamasına geçemiyor | [AD FS Extranet Akıllı Kilitleme](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection) ve/veya Azure AD Akıllı [Kilitleme'yi](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-smart-lockout) etkinleştirme |
| Parola ilkesi uzunluk, birden çok karakter kümesi veya son kullanma gibi karmaşıklık tabanlı kurallar kullanır | [Microsoft Önerilen Uygulamalar](https://aka.ms/passwordguidance) lehine yeniden düşünün ve parola yönetimine yaklaşımınızı değiştirin ve Azure AD parola [koruması](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad)dağıtın. |
| Kullanıcılar çok faktörlü kimlik doğrulaması (MFA) kullanmak için kayıtlı değil | Parolaları ile birlikte kullanıcının kimliğini doğrulamak için bir mekanizma olarak kullanılabilen [tüm kullanıcının güvenlik bilgilerini kaydedin](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-mfa-policy) |
| Kullanıcı riskine dayalı parolaların iptali yoktur | SSPR kullanarak sızdırılan kimlik bilgilerinde parola değişikliklerini zorlamak için Azure AD [Identity Protection kullanıcı risk ilkelerini](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy) dağıtma |
| Tanımlanan IP adreslerinden gelen kötü aktörlere karşı kötü kimlik doğrulaması korumak için akıllı kilitleme mekanizması yoktur | Bulut tarafından yönetilen kimlik doğrulamayı parola karma eşitleme veya [geçiş kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) (PTA) ile dağıtma |

#### <a name="password-policies-recommended-reading"></a>Parola ilkeleri okuma önerilir

- [Azure AD ve AD FS en iyi uygulamaları: Parola püskürtme saldırılarına karşı savunma - Kurumsal Mobilite + Güvenlik](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/05/azure-ad-and-adfs-best-practices-defending-against-password-spray-attacks/)

### <a name="enable-self-service-password-reset-and-password-protection"></a>Self servis parola sıfırlama ve parola korumayı etkinleştirme

Parolalarını değiştirmeleri veya sıfırlamaları gereken kullanıcılar, yardım masası aramalarının en büyük hacim ve maliyet kaynaklarından biridir. Maliyete ek olarak, parolayı kullanıcı riskini azaltmak için bir araç olarak değiştirmek, kuruluşunuzun güvenlik duruşunu iyileştirmede temel bir adımdır.

En azından gerçekleştirmek için Azure AD [self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks) (SSPR) ve şirket içi parola [koruması](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-deploy) dağıtmanız önerilir:

- Yardım masası aramalarını saptırın.
- Geçici parolaların kullanımını değiştirin.
- Şirket içi çözüme dayanan varolan herhangi bir self servis parola yönetimi çözümleme.
- Kuruluşunuzdaki [zayıf parolaları ortadan kaldırın.](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad)

> [!NOTE]
> Azure AD Premium P2 aboneliği olan kuruluşlar için, SSPR'yi dağıtmaları ve [kimlik koruma kullanıcı risk politikasının](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)bir parçası olarak kullanmaları önerilir.

### <a name="strong-credential-management"></a>Güçlü kimlik bilgisi yönetimi

Parolalar tek başlarına kötü aktörlerin ortamınıza erişmesini engelleyecek kadar güvenli değildir. En azından, çok faktörlü kimlik doğrulaması (MFA) için ayrıcalıklı hesabı olan herhangi bir kullanıcı etkinleştirilmelidir. İdeal olarak, [kombine kaydı](https://docs.microsoft.com/azure/active-directory/authentication/concept-registration-mfa-sspr-combined) etkinleştirmeli ve tüm kullanıcıların kombine [kayıt deneyimini](https://docs.microsoft.com/azure/active-directory/user-help/user-help-security-info-overview)kullanarak MFA ve SSPR'ye kaydolmasını zorunlu kılmalıdır. Sonuç olarak, öngörülemeyen koşullar nedeniyle lokavt riskini azaltmak için [esneklik sağlamak](https://docs.microsoft.com/azure/active-directory/authentication/concept-resilient-controls) için bir strateji benimsemenizi öneririz.

![Birleşik kullanıcı deneyimi akışı](./media/active-directory-ops-guide/active-directory-ops-img4.png)

### <a name="on-premises-outage-authentication-resiliency"></a>Şirket içi çıkış kimlik doğrulama esnekliği

Azure AD Password Hash Sync (PHS) ve Azure MFA, basitliğin ve sızdırılan kimlik bilgisi algılamasının avantajlarına ek olarak, [kullanıcıların NotPetya](https://www.microsoft.com/security/blog/2018/02/05/overview-of-petya-a-rapid-cyberattack/)gibi siber saldırılar nedeniyle şirket içi kesintilere rağmen SaaS uygulamalarına ve Office 365'e erişmesine olanak tanır. Federasyon ile birlikte phs'i etkinleştirmek de mümkündür. PHS'yi etkinleştirmek, federasyon hizmetleri kullanılamadığında kimlik doğrulamanın geri dönmesine izin verir.

Şirket içi kuruluşunuzda kesinti esnekliği stratejisi yoksa veya Azure AD ile tümleşik olmayan bir stratejivarsa, Azure AD PHS'yi dağıtmalı ve PHS içeren bir olağanüstü durum kurtarma planı tanımlamalısınız. Azure AD PHS'nin etkinleştirilmesi, şirket içi Active Directory'nizin kullanılamaması halinde kullanıcıların Azure AD'ye karşı kimlik doğrulaması yapmalarını sağlar.

![şifre karma eşitleme akışı](./media/active-directory-ops-guide/active-directory-ops-img5.png)

Kimlik doğrulama seçeneklerinizi daha iyi anlamak [için](https://docs.microsoft.com/azure/active-directory/hybrid/choose-ad-authn)bkz.

### <a name="programmatic-usage-of-credentials"></a>Kimlik bilgilerinin programlı kullanımı

PowerShell kullanan Azure AD komut dosyaları veya Microsoft Graph API'sını kullanan uygulamalar güvenli kimlik doğrulaması gerektirir. Bu komut dosyalarını ve araçları çalıştıran kötü kimlik bilgisi yönetimi kimlik bilgisi hırsızlığı riskini artırır. Sabit kodlu parolalara veya parola istemlerine dayanan komut dosyaları veya uygulamalar kullanıyorsanız, önce config dosyalarındaki veya kaynak kodundaki parolaları gözden geçirmeniz, ardından bu bağımlılıkları değiştirmeniz ve mümkün olduğunda Azure Yönetilen [Kimlikler,](https://docs.microsoft.com/azure/active-directory/reports-monitoring/tutorial-access-api-with-certificates) Tümleşik Windows Kimlik Doğrulaması veya sertifikaları kullanmanız gerekir. Önceki çözümlerin mümkün olmadığı uygulamalarda Azure [Key Vault'u](https://azure.microsoft.com/services/key-vault/)kullanmayı düşünün.

Parola kimlik bilgilerine sahip hizmet ilkelerinin olduğunu belirlerseniz ve bu parola kimlik bilgilerinin komut dosyaları veya uygulamalar tarafından nasıl güvence altına alındığınızdan emin değilseniz, kullanım modellerini daha iyi anlamak için uygulamanın sahibine başvurun.

Microsoft ayrıca, parola kimlik bilgilerine sahip hizmet ilkeleri varsa kullanım modellerini anlamak için uygulama sahipleriyle iletişim kurmanızı önerir.

## <a name="authentication-experience"></a>Kimlik doğrulama deneyimi

### <a name="on-premises-authentication"></a>Şirket içi kimlik doğrulama

Tümleşik Windows Kimlik Doğrulama (IWA) veya Sorunsuz Tek Oturum Açma (SSO) ile yönetilen kimlik doğrulama, parola karma eşitleme veya geçiş kimlik doğrulaması ile yönetilen kimlik doğrulama, şirket ağında, şirket içi etki alanı denetleyicilerine görünürken en iyi kullanıcı deneyimidir. Kimlik bilgisi istemi yorgunluğunu en aza indirir ve kullanıcıların kimlik avı saldırılarına kurban düşme riskini azaltır. Zaten PHS veya PTA ile bulut tarafından yönetilen kimlik doğrulaması kullanıyorsanız, ancak kullanıcıların şirket içinde kimlik doğrulaması yaparken parolalarını yazmaları gerekiyorsa, hemen [Kesintisiz SSO'yu dağıtmanız](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso)gerekir. Diğer taraftan, şu anda bulut tarafından yönetilen kimlik doğrulamasına geçiş planları yla doluysanız, geçiş projesinin bir parçası olarak Kesintisiz SSO uygulamanız gerekir.

### <a name="device-trust-access-policies"></a>Aygıt güven erişim ilkeleri

Kuruluşunuzdaki bir kullanıcı gibi, aygıt da korumak istediğiniz temel bir kimliktir. Kaynaklarınızı istediğiniz zaman ve herhangi bir konumdan korumak için aygıtın kimliğini kullanabilirsiniz.Aygıtın doğrulanması ve güven türüne göre muhasebeleştirilmesi, güvenlik duruşunuzu ve kullanılabilirliğinizi şu şekilde artırır:

- Örneğin, Cihaz güvenilir olduğunda MFA ile sürtünmeden kaçınma
- Güvenilmeyen aygıtlardan erişimi engelleme
- Windows 10 aygıtları için şirket [içi kaynakları sorunsuz bir şekilde tek](https://docs.microsoft.com/azure/active-directory/devices/azuread-join-sso)oturum aç't'a sağlayın.

Aşağıdaki yöntemlerden birini kullanarak aygıt kimliklerini getirerek ve Azure AD'de yöneterek bu hedefi gerçekleştirebilirsiniz:

- Kuruluşlar, aygıtı yönetmek ve uyumluluk ilkelerini uygulamak, aygıt durumunu kanıtlamak ve aygıtın uyumlu olup olmadığına bağlı koşullu erişim ilkeleri ayarlamak için [Microsoft Intune'u](https://docs.microsoft.com/intune/what-is-intune) kullanabilir. Microsoft Intune iOS aygıtlarını, Mac masaüstü bilgisayarlarını (JAMF tümleştirme yoluyla), Windows masaüstü bilgisayarlarını (windows 10 için mobil aygıt yönetimini ve Microsoft Endpoint Configuration Manager ile birlikte yönetimi kullanarak) ve Android mobil cihazları yönetebilir.
- [Karma Azure AD join,](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-managed-domains) Active Directory etki alanı yla birleştirilmiş bilgisayar aygıtlarının olduğu bir ortamda Grup İlkeleri veya Microsoft Endpoint Configuration Manager ile yönetim sağlar. Kuruluşlar, Sorunsuz SSO ile PHS veya PTA aracılığıyla yönetilen bir ortamı dağıtabilir. Cihazlarınızı Azure AD'ye getirmek, bulutve şirket içi kaynaklarınız üzerinden SSO aracılığıyla kullanıcı verimliliğini en üst düzeye çıkarırken, aynı zamanda [Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) ile bulutlarınıza ve şirket içi kaynaklarınıza erişimi güvence altına almanızı sağlar.

Bulutta kayıtlı olmayan etki alanına katılan Windows aygıtlarınız veya bulutta kayıtlı ancak koşullu erişim ilkeleri olmayan etki alanına katılan Windows aygıtlarınız varsa, kaydedilmemiş aygıtları kaydetmeniz ve her iki durumda da koşullu erişim ilkelerinizde [denetim olarak Karma Azure AD birleştirme'yi kullanmanız](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices) gerekir.

![Karma aygıt gerektiren koşullu erişim ilkesinde hibenin ekran görüntüsü](./media/active-directory-ops-guide/active-directory-ops-img6.png)

MDM veya Microsoft Intune'a sahip aygıtları yönetiyor, ancak koşullu erişim ilkelerinizde aygıt denetimlerini kullanmıyorsanız, bu ilkelerde denetim [olarak uyumlu olarak işaretlenecek şekilde Aygıt İste'yi](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices#require-device-to-be-marked-as-compliant) kullanmanızı öneririz.

![Cihaz uyumluluğu gerektiren koşullu erişim ilkesinde hibenin ekran görüntüsü](./media/active-directory-ops-guide/active-directory-ops-img7.png)

#### <a name="device-trust-access-policies-recommended-reading"></a>Aygıt güven erişim ilkeleri önerilen okuma

- [Nasıl Olunması: Karma Azure Etkin Dizininizi uygulamayla birleştirin](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan)
- [Kimlik ve cihaz erişim yapılandırmaları](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

### <a name="windows-hello-for-business"></a>İş İçin Windows Hello

Windows 10'da, [Windows Hello for Business,](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) parolaları bilgisayarlarda güçlü iki faktörlü kimlik doğrulamayla değiştirir. Windows Hello for Business, kullanıcılar için daha kolay laştırılmış bir MFA deneyimi sağlar ve parolalara olan bağımlılığınızı azaltır. Windows 10 aygıtlarını kullanıma başlamadıysanız veya bunları yalnızca kısmen dağıtmadıysanız, Windows 10'a yükseltmenizi ve tüm cihazlarda [Windows Hello for Business'ı etkinleştirmenizi](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-manage-in-organization) öneririz.

Parolasız kimlik doğrulama hakkında daha fazla bilgi edinmek istiyorsanız, [Azure Active Directory ile parolasız bir dünya](https://aka.ms/passwordlessdoc)'ya bakın.

## <a name="application-authentication-and-assignment"></a>Uygulama kimlik doğrulaması ve atama

### <a name="single-sign-on-for-apps"></a>Uygulamalar için tek oturum açma

Tüm kuruluşa standartlaştırılmış tek oturum açma mekanizması sağlamak, en iyi kullanıcı deneyimi, riskin azaltılması, rapor verme yeteneği ve yönetişim için çok önemlidir. Azure AD ile SSO'yu destekleyen ancak şu anda yerel hesapları kullanacak şekilde yapılandırılan uygulamalar kullanıyorsanız, bu uygulamaları Azure AD ile SSO kullanacak şekilde yeniden yapılandırmanız gerekir. Aynı şekilde, Azure AD ile SSO'yu destekleyen ancak başka bir Kimlik Sağlayıcısı kullanıyorsanız, bu uygulamaları Azure AD ile de SSO kullanacak şekilde yeniden yapılandırmanız gerekir. Federasyon protokollerini desteklemeyen ancak form tabanlı kimlik doğrulamayı destekleyen uygulamalar için, uygulamayı Azure AD Application Proxy ile [parola atlama](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-password-vaulting) yı kullanacak şekilde yapılandırmanızı öneririz.

![AppProxy Şifre Tabanlı Oturum Açma](./media/active-directory-ops-guide/active-directory-ops-img8.png)

> [!NOTE]
> Kuruluşunuzdaki yönetilmeyen uygulamaları keşfedecek bir mekanizmanız yoksa, [Microsoft Cloud App Security](https://www.microsoft.com/enterprise-mobility-security/cloud-app-security)gibi bir bulut erişimi güvenlik aracısı çözümü (CASB) kullanarak bir bulma işlemi uygulamanızı öneririz.

Son olarak, bir Azure AD uygulama galeriniz varsa ve Azure AD ile SSO'yu destekleyen uygulamalar kullanıyorsanız, [uygulamayı uygulama galerisinde listelemenizi](https://docs.microsoft.com/azure/active-directory/develop/howto-app-gallery-listing)öneririz.

#### <a name="single-sign-on-recommended-reading"></a>Tek oturum açma önerilen okuma

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

### <a name="migration-of-ad-fs-applications-to-azure-ad"></a>AD FS uygulamalarının Azure AD'ye geçişi

[Uygulamaları AD FS'den Azure AD'ye geçirmek,](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-apps-to-azure) güvenlik, daha tutarlı yönetilebilirlik ve daha iyi bir işbirliği deneyimi konusunda ek özellikler sağlar. SSO'yu Azure AD ile destekleyen AD FS'de yapılandırılmış uygulamalarınız varsa, bu uygulamaları Azure AD ile SSO kullanacak şekilde yeniden yapılandırmanız gerekir. AD FS'de Azure AD tarafından desteklenmeyen yaygın yapılandırmalarla yapılandırılan uygulamalarınız varsa, özel yapılandırmanın uygulamanın mutlak bir gereksinimi olup olmadığını anlamak için uygulama sahipleriyle iletişime geçmeniz gerekir. Gerekli değilse, uygulamayı Azure AD ile SSO kullanacak şekilde yeniden yapılandırmanız gerekir.

![Birincil kimlik sağlayıcısı olarak Azure AD](./media/active-directory-ops-guide/active-directory-ops-img9.png)

> [!NOTE]
> [ADFS için Azure AD Connect Health,](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs) Azure AD'ye geçirilebilen her uygulama yla ilgili yapılandırma ayrıntılarını toplamak için kullanılabilir.

### <a name="assign-users-to-applications"></a>Kullanıcıları uygulamalara atama

[Kullanıcıları uygulamalara atamak,](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups) daha fazla esneklik ve ölçekte yönetme olanağı sağladıklarından, gruplar kullanılarak en iyi şekilde eşlenir. Grupları kullanmanın yararları arasında [öznitelik tabanlı dinamik grup üyeliği](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership) ve uygulama [sahiplerine delegasyon](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-accessmanagement-managing-group-owners)yer alır. Bu nedenle, grupları zaten kullanıyor ve yönetiyorsanız, ölçekte yönetimi geliştirmek için aşağıdaki eylemleri yapmanızı öneririz:

- Grup yönetimini ve yönetimini uygulama sahiplerine devredin.
- Uygulamaya self servis erişimine izin verin.
- Kullanıcı öznitelikleri sürekli olarak uygulamalara erişimi belirleyebiliyorsa dinamik gruplar tanımlayın.
- [Azure AD erişim incelemelerini](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)kullanarak uygulama erişimi için kullanılan gruplara attestation uygulayın.

Diğer taraftan, tek tek kullanıcılara atama sı bulunan uygulamalar bulursanız, bu uygulamalar etrafında [yönetişim](https://docs.microsoft.com/azure/active-directory/governance/index) uyguladığından emin olun.

#### <a name="assign-users-to-applications-recommended-reading"></a>Önerilen okuma uygulamalarına kullanıcıları atama

- [Azure Etkin Dizini'ndeki bir uygulamaya kullanıcı ve gruplar atama](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-assigning-users-and-groups)
- [Azure Active Directory'de uygulama kayıt izinlerini temsilciolarak](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-delegate-app-roles)
- [Azure Etkin Dizini'ndeki gruplar için dinamik üyelik kuralları](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)

## <a name="access-policies"></a>Erişim ilkeleri

### <a name="named-locations"></a>Adlandırılmış konumlar

Azure AD'de [adlandırılmış konumlarla,](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations) kuruluşunuzdaki güvenilir IP adresi aralıklarını etiketleyebilirsiniz. Azure AD, aşağıdakileri yapmak için adlandırılmış konumları kullanır:

- Risk olaylarında yanlış pozitifleri önleyin. Güvenilir bir ağ konumundan oturum açma, kullanıcının oturum açma riskini azaltır.
- Konum [tabanlı Koşullu Erişimi](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)yapılandırın.

![Adlandırılmış konum](./media/active-directory-ops-guide/active-directory-ops-img10.png)

Önceliğe bağlı olarak, kuruluşunuzun gereksinimlerini en iyi karşılayan önerilen çözümü bulmak için aşağıdaki tabloyu kullanın:

| **Öncelik** | **Senaryo** | **Öneri** |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| 1 | PHS veya PTA kullanıyorsanız ve adlandırılmış konumlar tanımlanmamışsa | Risk olaylarının algılanmasını iyileştirmek için adlandırılmış konumları tanımlama |
| 2 | Federe iseniz ve "insideCorporateNetwork" iddiasını kullanmıyorsanız ve adlandırılmış konumlar tanımlanmamışsa | Risk olaylarının algılanmasını iyileştirmek için adlandırılmış konumları tanımlama |
| 3 | Koşullu erişim ilkelerinde adlandırılmış konumları kullanmıyorsanız ve koşullu erişim ilkelerinde risk veya aygıt denetimleri yoksa | Koşullu erişim ilkesini adlandırılmış konumları içerecek şekilde yapılandırma |
| 4 | Eğer federe iseniz ve "insideCorporateNetwork" iddia ve adlandırılmış konumlar tanımlanmamış kullanmak | Risk olaylarının algılanmasını iyileştirmek için adlandırılmış konumları tanımlama |
| 5 | Adlandırılmış konumlar yerine MFA ile güvenilir IP adresleri kullanıyorsanız ve bunları güvenilir olarak işaretlediyseniz | Adlandırılmış konumları tanımlayın ve risk olaylarının algılanmasını iyileştirmek için bunları güvenilir olarak işaretleyin |

### <a name="risk-based-access-policies"></a>Risk tabanlı erişim ilkeleri

Azure AD, her oturum açma ve her kullanıcı için riski hesaplayabilir. Erişim ilkelerinde riski ölçüt olarak kullanmak, daha iyi bir kullanıcı deneyimi ( örneğin, daha az kimlik doğrulama istemleri ve daha iyi güvenlik ( örneğin, kullanıcılara yalnızca ihtiyaç duyulduğunda istem de ve yanıtı ve düzeltmeyi otomatikleştirin) sağlayabilir.

![Oturum açma risk ilkesi](./media/active-directory-ops-guide/active-directory-ops-img11.png)

Erişim ilkelerinde risk kullanmayı destekleyen Azure AD Premium P2 lisanslarınıza zaten sahipseniz, ancak bunlar kullanılamıyorsa, güvenlik duruşunuza risk eklemenizi şiddetle öneririz.

#### <a name="risk-based-access-policies-recommended-reading"></a>Risk tabanlı erişim ilkeleri önerilen okuma

- [Nasıl Yapılandırılır: Oturum açma risk ilkesini yapılandırın](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-sign-in-risk-policy)
- [Nasıl Yapılandırılır: Kullanıcı risk ilkesini yapılandırın](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-user-risk-policy)

### <a name="client-application-access-policies"></a>İstemci uygulama erişim ilkeleri

Microsoft Intune Application Management (MAM), Outlook Mobile gibi uyumlu istemci mobil uygulamalarına depolama şifrelemesi, PIN, uzaktan depolama temizleme vb. veri koruma denetimlerini itme olanağı sağlar. Ayrıca, onaylı veya uyumlu uygulamalardan Exchange Online gibi bulut hizmetlerine [erişimi kısıtlamak](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) için koşullu erişim ilkeleri oluşturulabilir.

Çalışanlarınız Exchange Online veya SharePoint Online gibi kurumsal kaynaklara erişmek için Office mobil uygulamaları gibi MAM özellikli uygulamalar yüklerse ve byOD'yi de destekliyorsanız (kendi aygıtınızı getirin), UYGULAMA yapılandırmasını MDM kaydı olmayan kişisel cihazlarda yönetmek için uygulama MAM ilkelerini dağıtmanızı ve ardından yalnızca MAM özellikli istemcilerden erişime izin vermek için koşullu erişim ilkelerinizi güncelleştirmenizi öneririz.

![Koşullu Erişim Hibe kontrolü](./media/active-directory-ops-guide/active-directory-ops-img12.png)

Çalışanların KURUMSAL kaynaklara karşı MAM özellikli uygulamalar yüklemesi ve Intune Yönetilen cihazlarda erişim kısıtlanırsa, kişisel aygıtlar için uygulama yapılandırmasını yönetmek için uygulama MAM ilkelerini dağıtmayı ve Koşullu Erişim ilkelerini yalnızca MAM özellikli istemcilerden erişime izin verecek şekilde güncelleştirmeyi düşünmelisiniz.

### <a name="conditional-access-implementation"></a>Koşullu Erişim uygulaması

Koşullu Erişim, kuruluşunuzun güvenlik duruşunu iyileştirmek için önemli bir araçtır. Bu nedenle, bu en iyi uygulamaları takip etmek önemlidir:

- Tüm SaaS uygulamalarının en az bir ilke uygulandığından emin olun
- Kilitleme riskini önlemek için **Tüm uygulamalar** filtresini **blok** kontrolüyle birleştirmekten kaçının
- **Tüm kullanıcıları** filtre olarak kullanmaktan ve yanlışlıkla **Misafirler** eklemekten kaçının
- **Tüm "eski" ilkeleri Azure portalına geçirin**
- Kullanıcılar, aygıtlar ve uygulamalar için tüm ölçütleri yakalayın
- **Kullanıcı başına MFA** kullanmak yerine [MFA'yı uygulamak](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access)için Koşullu Erişim ilkelerini kullanma
- Birden çok uygulamaya uygulanabilecek küçük bir temel ilke kümesine sahip olması
- Boş özel durum gruplarını tanımlayın ve özel durum stratejisi ne olacaksa ilkelere ekleyin
- MFA kontrolleri olmadan cam hesapları [kırmak](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-admin-roles-secure#break-glass-what-to-do-in-an-emergency) için plan
- Office 365 istemci uygulamaları arasında tutarlı bir deneyim sağlayın (örneğin, Takımlar, İşletmeler için OneDrive, Outlook, vb.) Exchange Online ve Sharepoint Online gibi hizmetler için aynı denetim kümesini uygulayarak
- İlkelere atama bireyler değil, gruplar aracılığıyla uygulanmalıdır
- Kullanıcıların güvenlik duruşudışında kalma süresini sınırlamak için ilkelerde kullanılan özel durum gruplarının düzenli olarak gözden yorumlarını yapın. Azure AD P2'ye sahipseniz, işlemi otomatikleştirmek için erişim değerlendirmelerini kullanabilirsiniz

#### <a name="conditional-access-recommended-reading"></a>Koşullu Erişim önerilen okuma

- [Azure Etkin Dizini'nde Koşullu Erişim için en iyi uygulamalar](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices)
- [Kimlik ve cihaz erişim yapılandırmaları](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)
- [Azure Active Directory Koşullu Erişim ayarları başvurusu](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference)
- [Sık kullanılan Koşullu Erişim İlkeleri](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common)

## <a name="access-surface-area"></a>Erişim yüzey alanı

### <a name="legacy-authentication"></a>Eski kimlik doğrulaması

MFA gibi güçlü kimlik bilgileri, kötü amaçlı aktörler tarafından tercih edilen saldırı vektörü yapan eski kimlik doğrulama protokollerini kullanarak uygulamaları koruyamaz. Eski kimlik doğrulamasını kilitlemek, erişim güvenliği duruşunu iyileştirmek için çok önemlidir.

Eski kimlik doğrulaması, şu gibi uygulamalar tarafından kullanılan kimlik doğrulama protokollerini ifade eden bir terimdir:

- Modern kimlik doğrulaması kullanmayan eski Office istemcileri (örneğin, Office 2010 istemcisi)
- IMAP/SMTP/POP gibi posta protokollerini kullanan istemciler

Saldırganlar bu protokolleri şiddetle tercih eder - aslında parola püskürtme saldırılarının yaklaşık [%100'ü](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984) eski kimlik doğrulama protokollerini kullanır! Bilgisayar korsanları, çok faktörlü kimlik doğrulama ve aygıt kimlik doğrulama gibi ek güvenlik sorunları için gerekli olan etkileşimli oturum açmayı desteklemedikleri için eski kimlik doğrulama protokollerini kullanır.

Eski kimlik doğrulaması ortamınızda yaygın olarak kullanılıyorsa, eski istemcileri en kısa sürede [modern kimlik doğrulamasını](https://docs.microsoft.com/office365/enterprise/modern-auth-for-office-2013-and-2016) destekleyen istemcilere geçirmeyi planlamanız gerekir. Aynı şekilde, bazı kullanıcılar zaten modern kimlik doğrulaması kullanıyorsa, ancak hala eski kimlik doğrulaması kullanan lar varsa, eski kimlik doğrulama istemcilerini kilitlemek için aşağıdaki adımları uygulamanız gerekir:

1. Eski kimlik doğrulaması ve plan düzeltmesini hala kullanan kullanıcıları tanımlamak için [Oturum Açma Etkinliği raporlarını](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) kullanın:

   a. Etkilenen kullanıcılara modern kimlik doğrulama yeteneğine sahip istemcilere yükseltin.
   
   b. Aşağıdaki adım başına kilitlemek için bir kesme zaman çerçevesi planlayın.
   
   c. Hangi eski uygulamaların eski kimlik doğrulamaya sıkı bir bağımlılığı olduğunu belirleyin. Aşağıdaki adım 3'e bakın.

2. Daha fazla maruz kalmayı önlemek için eski auth kullanmayan kullanıcılar için kaynaktaki eski protokolleri (örneğin Exchange Mailbox) devre dışı bırakabilir.
3. Kalan hesaplar için (hizmet hesapları gibi ideal olarak insan olmayan kimlikler), kimlik doğrulama sonrası [eski protokolleri kısıtlamak için koşullu erişimi](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Conditional-Access-support-for-blocking-legacy-auth-is/ba-p/245417) kullanın.

#### <a name="legacy-authentication-recommended-reading"></a>Eski kimlik doğrulaması önerilen okuma

- [Exchange Server'daki posta kutularına POP3 veya IMAP4 erişimini etkinleştirme veya devre dışı etme](https://docs.microsoft.com/exchange/clients/pop3-and-imap4/configure-mailbox-access?view=exchserver-2019)

### <a name="consent-grants"></a>İzin hibeleri

Yasadışı bir rıza verme saldırısında saldırgan, kişi bilgileri, e-posta veya belgeler gibi verilere erişim isteyen Azure AD tarafından kayıtlı bir uygulama oluşturur. Kullanıcılar, kötü amaçlı web sitelerine inerken kimlik avı saldırıları yoluyla kötü amaçlı uygulamalara izin vermiş olabilir.

Aşağıda, Microsoft bulut hizmetleri için incelemek isteyebileceğiniz izinlere sahip uygulamaların listesi verilmiştir:

- Uygulama lı veya \*temsilcili uygulamalar. OkumaYazma İzinleri
- Yetki nettiği uygulamalar, kullanıcı adına e- postaokuyabilir, gönderebilir veya yönleyebilir
- Aşağıdaki izinleri kullanarak verilen uygulamalar:

| Kaynak | İzin |
| :- | :- |
| Office 365 Exchange Online | Eas. AccessasUser.All |
| | Ews. AccessasUser.All |
| | Mail.Oku |
| Microsoft Graph API'si | Mail.Oku |
| | Mail.Read.Shared |
| | Mail.ReadWrite |

- Uygulamalar, oturum açmış kullanıcının tam kullanıcı kimliğine bürünmesini sağlar. Örnek:

|Kaynak | İzin |
| :- | :- |
| Microsoft Graph API'si| Dizin.AccessAsUser.All |
| Azure REST API | user_impersonation |

Bu senaryoyu önlemek için, gerektiğinden daha fazla hibe alan yasadışı hibeler veya uygulamalariçeren uygulamaları belirlemek ve düzeltmek için [Office 365'teki yasadışı izin bağışlarını algılamak](https://docs.microsoft.com/office365/securitycompliance/detect-and-remediate-illicit-consent-grants) ve düzeltmek için başvurmanız gerekir. Daha sonra, [self-servis tamamen kaldırın](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-user-consent) ve [yönetim prosedürleri kurmak.](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow) Son olarak, uygulama izinlerinin düzenli incelemelerini zamanlayın ve gerekli olmadığında kaldırın.

#### <a name="consent-grants-recommended-reading"></a>Rıza hibeler önerilen okuma

- [Microsoft Graph API izinleri](https://docs.microsoft.com/graph/permissions-reference)

### <a name="user-and-group-settings"></a>Kullanıcı ve grup ayarları

Aşağıda, açık bir iş gereksinimi yoksa kilitlenebilen kullanıcı ve grup ayarları verilmiştir:

#### <a name="user-settings"></a>Kullanıcı ayarları

- **Dış Kullanıcılar** - Ekipler, Power BI, Sharepoint Online ve Azure Bilgi Koruması gibi hizmetlerle kuruluşta organik olarak dış işbirliği gerçekleşebilir. Kullanıcı tarafından başlatılan dış işbirliğini denetlemek için açık kısıtlamalarınız varsa, Azure [REKLAM Yetkilendirme yönetimini](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) veya yardım masanız aracılığıyla gibi kontrollü bir işlemi kullanarak harici kullanıcıları etkinleştirmeniz önerilir. Hizmetler için organik dış işbirliğine izin vermek istemiyorsanız, [üyelerin dış kullanıcıları tamamen davet etmesini engelleyebilirsiniz.](https://docs.microsoft.com/azure/active-directory/b2b/delegate-invitations) Alternatif olarak, dış kullanıcı davetlerinde belirli etki alanlarına da izin verebilir [veya engelleyebilirsiniz.](https://docs.microsoft.com/azure/active-directory/b2b/allow-deny-list)
- **Uygulama Kayıtları** - Uygulama kayıtları etkinleştirildiğinde, son kullanıcılar uygulamalara kendileri dahil edebilir ve verilerine erişim izni verebilir. Uygulama kaydının tipik bir örneği, Outlook eklentilerinin veya Alexa ve Siri gibi sesli asistanların e-postalarını ve takvimlerini okumalarını veya onların adına e-posta göndermelerini sağlayan kullanıcılardır. Müşteri Uygulama kaydını kapatmaya karar verirse, InfoSec ve IAM ekipleri, uygulamaları bir yönetici hesabına kaydetmeleri gerektiğinden ve büyük olasılıkla işlemi işlevsel hale getirmek için bir işlem tasarlamak zorunda kalacakları için özel durumların (iş gereksinimlerine göre gerekli olan uygulama kayıtları) yönetimine dahil olmalıdır.
- **Yönetim Portalı** - kuruluşlar Azure portalındaki Azure AD bıçaklarını kilitleyebilir, böylece yönetici olmayanlar Azure portalında Azure REKLAM yönetimine erişemez ve kafaları karışabilir. Erişimi kısıtlamak için Azure AD yönetim portalındaki kullanıcı ayarlarına gidin:

![Yönetim portalı sınırlı erişim](./media/active-directory-ops-guide/active-directory-ops-img13.png)

> [!NOTE]
> Yönetici olmayanlar komut satırı ve diğer programlı arabirimler aracılığıyla Azure AD yönetim arabirimlerine erişmeye devam edebilir.

#### <a name="group-settings"></a>Grup ayarları

**Self-Servis Grup Yönetimi / Kullanıcılar Güvenlik grupları / O365 grupları oluşturabilirsiniz.** Buluttaki gruplar için geçerli bir self servis girişimi yoksa, müşteriler bu özelliği kullanmaya hazır olana kadar kapatmaya karar verebilir.

#### <a name="groups-recommended-reading"></a>Önerilen gruplar okuma

- [Azure Active Directory B2B işbirliği nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Uygulamaları Azure Etkin Dizini ile Tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)
- [Azure Etkin Dizini'nde uygulamalar, izinler ve onay.](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)
- [Azure Etkin Dizini'ndeki kaynaklara erişimi yönetmek için grupları kullanma](https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups)
- [Azure Active Directory'de self servis uygulama erişim yönetimi kurma](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management)

### <a name="traffic-from-unexpected-locations"></a>Beklenmeyen konumlardan gelen trafik

Saldırganlar dünyanın çeşitli yerlerinden kaynaklanır. Koşullu erişim ilkelerini koşullu olarak konumla birlikte kullanarak bu riski yönetin. Koşullu Erişim ilkesinin [konum koşulu,](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition) oturum açmanın iş nedeni olmadığı konumlara erişimi engellemenize olanak tanır.

![Adlandırılmış yeni bir konum oluşturma](./media/active-directory-ops-guide/active-directory-ops-img14.png)

Varsa, bölgeler arası erişim modellerini analiz etmek ve bulmak için bir güvenlik bilgisi ve olay yönetimi (SIEM) çözümü kullanın. Bir SIEM ürünü kullanmıyorsanız veya Azure AD'den kimlik doğrulama bilgilerini almıyorsa, bölgeler arası erişim modellerini belirlemek için [Azure Monitor'u](https://docs.microsoft.com/azure/azure-monitor/overview) kullanmanızı öneririz.

## <a name="access-usage"></a>Erişim kullanımı

### <a name="azure-ad-logs-archived-and-integrated-with-incident-response-plans"></a>Azure AD, olay yanıt planlarıyla arşivlenmiş ve tümleştirilmiş günlükleri

Azure AD için oturum açma etkinliklerine, denetimlere ve risk olaylarına erişim sağlamak, sorun giderme, kullanım analitiği ve adli tıp soruşturmaları için çok önemlidir. Azure AD, sınırlı bir saklama süresiolan REST API'leri aracılığıyla bu kaynaklara erişim sağlar. Bir güvenlik bilgi ve olay yönetimi (SIEM) sistemi veya eşdeğer arşiv teknolojisi, denetimlerin uzun süreli depolanması ve desteklenebilirlik için anahtardır. Azure REKLAM Günlüklerinin uzun süreli depolanmasını etkinleştirmek için bunları mevcut SIEM çözümünüze eklemeniz veya [Azure Monitor'u](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor)kullanmanız gerekir. Olay yanıt planlarınız ve soruşturmalarınızın bir parçası olarak kullanılabilecek arşiv günlükleri.

#### <a name="logs-recommended-reading"></a>Günlükleri okuma önerilir

- [Azure Active Directory denetim API başvurusu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference)
- [Azure Etkin Dizin oturum açma etkinlik raporu API başvurusu](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-sign-in-activity-reference)
- [Sertifikalarla Azure AD Raporlama API’sini kullanarak veri alma](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-with-certificates)
- [Azure Active Directory Identity Protection için Microsoft Grafiği](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)
- [Office 365 Yönetim Etkinliği API başvurusu](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference)
- [Azure Active Directory Power BI İçerik Paketi'ni kullanma](../reports-monitoring/howto-use-azure-monitor-workbooks.md)

## <a name="summary"></a>Özet

Güvenli bir kimlik altyapısının 12 yönü vardır. Bu liste, kimlik bilgilerini daha fazla güvenli hale almanıza ve yönetmenize, kimlik doğrulama deneyimini tanımlamanıza, temsilci ataması tanımlamanıza, kullanımı ölçmenize ve kurumsal güvenlik duruşuna dayalı erişim ilkelerini tanımlamanıza yardımcı olur.

- Önemli görevlere sahipleri atayın.
- Zayıf veya sızdırılan parolaları algılamak, parola yönetimini ve korumayı geliştirmek ve kullanıcının kaynaklara erişimini daha da güvenli hale getirmek için çözümler uygulayın.
- Kaynaklarınızı her zaman ve herhangi bir konumdan korumak için aygıtların kimliğini yönetin.
- Parolasız kimlik doğrulaması uygulayın.
- Kuruluş genelinde standartlaştırılmış tek oturum açma mekanizması sağlayın.
- Daha iyi güvenlik ve daha tutarlı yönetilebilirlik sağlamak için uygulamaları AD FS'den Azure AD'ye geçirin.
- Daha fazla esneklik ve ölçekte yönetme olanağı sağlamak için grupları kullanarak kullanıcıları uygulamalara atayın.
- Risk tabanlı erişim ilkelerini yapılandırın.
- Eski kimlik doğrulama protokollerini kilitleyin.
- Yasadışı izin bağışlarını tespit edin ve düzeltin.
- Kullanıcı ve grup ayarlarını kilitleyin.
- Sorun giderme, kullanım analitiği ve adli tıp araştırmaları için Azure AD günlüklerinin uzun süreli depolanmasını etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Kimlik yönetimi [operasyonel kontroller ve eylemler](active-directory-ops-guide-govern.md)ile başlayın.
