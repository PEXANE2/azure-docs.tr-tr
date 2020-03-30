---
title: Azure AD dağıtımı denetim listesi
description: Azure Etkin Dizin özelliği dağıtım denetim listesi
services: active-directory
ms.service: active-directory
ms.subservice: ''
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: f84226a631014b51338d47887fe3bafc969dc571
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063654"
---
# <a name="azure-active-directory-feature-deployment-guide"></a>Azure Etkin Dizin özelliği dağıtım kılavuzu

Kuruluşunuz için Azure Active Directory'yi (Azure AD) dağıtmak ve güvenli tutmak zor görünebilir. Bu makalede, müşterilerin güvenlik duruşlarını geliştirmek için 30, 60, 90 gün veya daha uzun bir süredir aşamalı olarak tamamlamaları yararlı buldukları yaygın görevler tanımlanmıştır. Azure AD'yi zaten dağıtmış kuruluşlar bile yatırımlarından en iyi şekilde yararlandıklarından emin olmak için bu kılavuzu kullanabilir.

İyi planlanmış ve çalıştırılan kimlik altyapısı, yalnızca bilinen kullanıcılar ve aygıtlar tarafından üretkenlik iş yüklerinize ve verilerinize güvenli erişimin önünü açıyor.

Ayrıca, müşteriler Microsoft'un en iyi uygulamalarıyla ne kadar uyumlu olduklarını görmek için [kimlik güvenli puanlarını](identity-secure-score.md) denetleyebilir. Sektörünüzdeki diğer kuruluşlarla ve sizin büyüklüğünüzdeki diğer kuruluşlarla karşılaştırıldığında ne kadar iyi olduğunuzu görmek için bu önerileri uygulamadan önce ve uyguladıktan sonra güvenli puanınızı kontrol edin.

## <a name="prerequisites"></a>Ön koşullar

Bu kılavuzdaki önerilerin çoğu Azure AD Free ile uygulanabilir veya hiç lisans kullanmaz. Lisansların gerekli olduğu durumlarda, görevi yerine getirmek için en az hangi lisansın gerekli olduğunu belirtiriz.

Lisanslama ile ilgili ek bilgiler aşağıdaki sayfalarda bulunabilir:

* [Azure AD lisanslama](https://azure.microsoft.com/pricing/details/active-directory/)
* [Microsoft 365 Kurumsal](https://www.microsoft.com/en-us/licensing/product-licensing/microsoft-365-enterprise)
* [Enterprise Mobility + Security](https://www.microsoft.com/en-us/licensing/product-licensing/enterprise-mobility-security)
* [Azure AD B2B lisanslama kılavuzu](../b2b/licensing-guidance.md)

## <a name="phase-1-build-a-foundation-of-security"></a>Aşama 1: Güvenlik temeli oluşturun

Bu aşamada, yöneticiler, normal kullanıcı hesaplarını almadan veya oluşturmadan önce Azure AD'de daha güvenli ve kullanımı kolay bir temel oluşturmak için temel güvenlik özelliklerini etkinleştirin. Bu temel aşama, başlangıçtan itibaren daha güvenli bir durumda olmanızı ve son kullanıcılarınızın yalnızca bir kez yeni kavramlarla tanıştırılmasını sağlar.

| Görev | Ayrıntı | Gerekli lisans |
| ---- | ------ | ---------------- |
| [Birden fazla genel yönetici atama](../users-groups-roles/directory-emergency-access.md) | Acil bir durum olduğunda kullanılmak üzere yalnızca buluta özel yalnızca kalıcı küresel yönetici hesapları atayın. Bu hesaplar günlük olarak kullanılmaz ve uzun ve karmaşık parolalara sahip olmalıdır. | Azure AD Ücretsiz |
| [Mümkün olduğunda genel olmayan yönetim rollerini kullanma](../users-groups-roles/directory-assign-admin-roles.md) | Yöneticilerinize yalnızca yalnızca erişmeleri gereken alanlara gereksinim duydukları erişimi verin. Tüm yöneticilerin genel yönetici olması gerekmez. | Azure AD Ücretsiz |
| [Yönetici rol kullanımını izlemek için Ayrıcalıklı Kimlik Yönetimini etkinleştirme](../privileged-identity-management/pim-getting-started.md) | Yönetimrol kullanımını izlemeye başlamak için Ayrıcalıklı Kimlik Yönetimi'ni etkinleştirin. | Azure AD Premium P2 |
| [Self servis parola sıfırlamayı dağıtma](../authentication/howto-sspr-deployment.md) | Yönetici denetimi olarak ilkeleri kullanarak personelin kendi parolalarını sıfırlamasına izin vererek yardım masası nın parola sıfırlama çağrılarını azaltın. | |
| [Kuruluşa özel yasaklı parola listesi oluşturma](../authentication/howto-password-ban-bad-configure.md) | Kullanıcıların kuruluşunuzdan veya alanınızdan ortak sözcükler veya ifadeler içeren parolalar oluşturmasını engelleyin. | |
| [Azure AD parola korumasıyla şirket içi tümleştirmeyi etkinleştirme](../authentication/concept-password-ban-bad-on-premises.md) | Şirket içinde ayarlanan parolaların da genel ve kiracıya özel yasaklı parola listeleriyle uyumlu olduğundan emin olmak için yasaklı parola listesini şirket içi dizininize genişletin. | Azure AD Premium P1 |
| [Microsoft'un parola kılavuzunu etkinleştirme](https://www.microsoft.com/research/publication/password-guidance/) | Kullanıcıların parolalarını belirli bir zamanlamada değiştirmelerini, karmaşıklık gereksinimlerini devre dışı bırakmalarını ve kullanıcılarınızın parolalarını hatırlamaya ve güvenli bir şey tutmaya daha yatkın olmalarını engellemeyi bırakın. | Azure AD Ücretsiz |
| [Bulut tabanlı kullanıcı hesapları için periyodik parola sıfırlamalarını devre dışı kaldırma](../authentication/concept-sspr-policy.md#set-a-password-to-never-expire) | Periyodik parola sıfırlamaları, kullanıcılarınızı varolan parolalarını kademeli olarak yapmaya teşvik eder. Microsoft'un parola kılavuzu dokümanındaki yönergeleri kullanın ve şirket içi ilkenizi yalnızca bulut kullanıcılarıyla yansıtın. | Azure AD Ücretsiz |
| [Azure Active Directory akıllı kilitlemeyi özelleştirin](../authentication/howto-password-smart-lockout.md) | Bulut tabanlı kullanıcıların şirket içi Active Directory kullanıcılarına çoğaltılmasını engelleme | |
| [AD FS için Extranet Akıllı Kilitleme'yi etkinleştirin](/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-smart-lockout-protection) | AD FS extranet kilitleme, geçerli AD FS kullanıcılarının hesaplarını kullanmaya devam etmesine izin verirken kaba kuvvet parola tahmin saldırılarına karşı koruma sağlar. | |
| [Koşullu Erişim ilkelerini kullanarak Azure AD Çok Faktörlü Kimlik Doğrulaması'nı dağıtma](../authentication/howto-mfa-getstarted.md) | Koşullu Erişim ilkelerini kullanarak hassas uygulamalara erişirken kullanıcıların iki aşamalı doğrulama gerçekleştirmesini zorunlu kılın. | Azure AD Premium P1 |
| [Azure Etkin Dizin Kimlik Korumasını Etkinleştirme](../identity-protection/overview-identity-protection.md) | Kuruluşunuzdaki kullanıcılar için riskli oturum açma ve tehlikeye atılmış kimlik bilgilerinin izlenmesini etkinleştirin. | Azure AD Premium P2 |
| [Çok faktörlü kimlik doğrulamayı ve parola değişikliklerini tetiklemek için risk algılamalarını kullanma](../authentication/tutorial-risk-based-sspr-mfa.md) | Çok faktörlü kimlik doğrulama, parola sıfırlama ve risk temeline dayalı oturum açmaların engellenmesi gibi olayları tetiklenebilen otomasyonu etkinleştirin. | Azure AD Premium P2 |
| [Self servis parola sıfırlama ve Azure AD Çok Faktörlü Kimlik Doğrulama (önizleme) için yakınsama kaydını etkinleştirme](../authentication/concept-registration-mfa-sspr-converged.md) | Kullanıcılarınızın hem Azure Çok Faktörlü Kimlik Doğrulama hem de self servis parola sıfırlama için ortak bir deneyimden kaydolmasına izin verin. | Azure AD Premium P1 |

## <a name="phase-2-import-users-enable-synchronization-and-manage-devices"></a>Aşama 2: Kullanıcıları içe aktarma, eşitlemayı etkinleştirme ve cihazları yönetme

Daha sonra, kullanıcılarımızı içe aktararak ve senkronizasyonu sağlayarak, misafir erişimini planlayarak ve ek işlevselliği desteklemeye hazırlanarak faz 1'de yer alan temele katkıda bulunacağız.

| Görev | Ayrıntı | Gerekli lisans |
| ---- | ------ | ---------------- |
| [Azure AD Connect'i yükleme](../connect/active-directory-aadconnect-select-installation.md) | Kullanıcıları mevcut şirket içi dizininizden buluta senkronize etmeye hazırlanın. | Azure AD Ücretsiz |
| [Parola Karma Eşitleme uygula](../connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md) | Parola değişikliklerinin çoğaltılması, hatalı parola algılama ve düzeltme ve sızdırılmış kimlik bilgisi raporlaması için parola işleblerini eşitle. | Azure AD Ücretsiz |
| [Parola Yazma](../authentication/howto-sspr-writeback.md) | Buluttaki parola değişikliklerinin şirket içi Windows Server Active Directory ortamına geri yazılmasına izin verin. | Azure AD Premium P1 |
| [Azure AD Bağlantı Durumu Uygulama](../connect-health/active-directory-aadconnect-health.md) | Azure AD Connect sunucularınız, AD FS sunucularınız ve etki alanı denetleyicileriniz için önemli sistem durumu istatistiklerinin izlenmesini etkinleştirin. | Azure AD Premium P1 |
| [Azure Active Directory'de grup üyeliğine göre kullanıcılara lisans atama](../users-groups-roles/licensing-groups-assign.md) | Kullanıcı başına ayar yerine grup özelliklerine göre özellikleri etkinleştiren veya devre dışı eden lisans grupları oluşturarak zamandan ve çabadan tasarruf edin. | |
| [Konuk kullanıcı erişimi için bir plan oluşturma](../b2b/what-is-b2b.md) | Kendi çalışmaları, okulları veya sosyal kimlikleri ile uygulamalarınızda ve hizmetlerinizde oturum açmalarına izin vererek konuk kullanıcılarla işbirliği yapın. | [Azure AD B2B lisanslama kılavuzu](../b2b/licensing-guidance.md) |
| [Cihaz yönetimi stratejisine karar verin](../devices/overview.md) | Kuruluşunuzun aygıtlar hakkında neye izin verdiğine karar verin. Kayıt vs katılma, Kendi Cihazı vs şirket getir sağladı. | |
| [Kuruluşunuzdaki İşletmeler için Windows Merhaba dağıtma](/windows/security/identity-protection/hello-for-business/hello-manage-in-organization) | Windows Hello'yu kullanarak parolasız kimlik doğrulamaya hazırlanın | |
| [Kullanıcılarınız için parolasız kimlik doğrulama yöntemlerini dağıtma](../authentication/concept-authentication-passwordless.md) | Kullanıcılarınıza kullanışlı parolasız kimlik doğrulama yöntemleri sağlayın | Azure AD Premium P1 |

## <a name="phase-3-manage-applications"></a>Aşama 3: Uygulamaları yönetme

Önceki aşamaları geliştirmeye devam ettikçe, Azure AD ile geçiş ve tümleştirme için aday uygulamaları belirler ve bu uygulamaların kurulumunu tamamlarız.

| Görev | Ayrıntı | Gerekli lisans |
| ---- | ------ | ---------------- |
| Uygulamalarınızı tanımlayın | Kuruluşunuzda kullanılan uygulamaları tanımlayın: şirket içi, buluttaki SaaS uygulamaları ve diğer iş yeri uygulamaları. Bu uygulamaların Azure AD ile yönetilip yönetilmeyeceğini belirleyin. | Lisans gerekmez |
| [Desteklenen SaaS uygulamalarını galeriye entegre edin](../manage-apps/add-application-portal.md) | Azure AD'nin, önceden tümleşik binlerce uygulama içeren bir galerisi vardır. Kuruluşunuzun kullandığı uygulamalardan bazıları büyük olasılıkla doğrudan Azure portalından erişilebilen galeridedir. | Azure AD Ücretsiz |
| [Şirket içi uygulamaları entegre etmek için Uygulama Proxy'sini kullanın](../manage-apps/application-proxy-add-on-premises-application.md) | Application Proxy, kullanıcıların Azure AD hesaplarıyla oturum açarak şirket içi uygulamalara erişmesini sağlar. | |

## <a name="phase-4-audit-privileged-identities-complete-an-access-review-and-manage-user-lifecycle"></a>Aşama 4: Ayrıcalıklı kimlikleri denetleme, erişim incelemesini tamamlama ve kullanıcı yaşam döngüsünü yönetme

Aşama 4, yöneticilerin yönetim için en az ayrıcalık ilkesini uyguladığını, ilk erişim incelemelerini tamamladığını ve ortak kullanıcı yaşam döngüsü görevlerinin otomasyonunu sağladığını görür.

| Görev | Ayrıntı | Gerekli lisans |
| ---- | ------ | ---------------- |
| [Ayrıcalıklı Kimlik Yönetimi kullanımını uygulamak](../privileged-identity-management/pim-security-wizard.md) | Yönetim rollerini normal günden güne kullanıcı hesaplarından kaldırın. Çok faktörlü kimlik doğrulama denetimi yaptıktan, iş gerekçesi sağladıktan veya atanan onaylayıcılardan onay istedikten sonra yönetim kullanıcılarırollerini kullanmaya uygun hale getirin. | Azure AD Premium P2 |
| [PIM'deki Azure REKLAM dizin rolleri için bir erişim incelemesi ni tamamlayın](../privileged-identity-management/pim-how-to-start-security-review.md) | Kuruluşunuzun ilkelerine dayalı olarak yönetim erişimini gözden geçirmek için bir erişim gözden geçirme ilkesi oluşturmak için güvenlik ve liderlik ekiplerinizle birlikte çalışın. | Azure AD Premium P2 |
| [Dinamik grup üyelik ilkeleri uygulama](../users-groups-roles/groups-dynamic-membership.md) | Bölümü, başlığı, bölge ve diğer öznitelikleri gibi İk'dan (veya doğruluk kaynağınızdan) bağlı olarak kullanıcıları gruplara otomatik olarak atamak için dinamik grupları kullanın. |  |
| [Grup tabanlı uygulama sağlama uygulama](../manage-apps/what-is-access-management.md) | SaaS uygulamaları için kullanıcıları otomatik olarak sağlamak için grup tabanlı erişim yönetimi sağlama yı kullanın. |  |
| [Kullanıcı sağlama ve sağlamayı otomatikleştirin](../app-provisioning/user-provisioning.md) | Yetkisiz erişimi önlemek için çalışan hesabı yaşam döngünüzden el ile adımları kaldırın. Kimlikleri doğruluk kaynağınızdan (İk Sistemi) Azure AD'ye senkronize edin. |  |

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD lisanslama ve fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/active-directory/)

[Kimlik ve cihaz erişim yapılandırmaları](https://docs.microsoft.com/microsoft-365/enterprise/microsoft-365-policies-configurations)

[Sık önerilen kimlik ve aygıt erişim ilkeleri](https://docs.microsoft.com/microsoft-365/enterprise/identity-access-policies)
