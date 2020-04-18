---
title: iOS'ta sertifika tabanlı kimlik doğrulama - Azure Active Directory
description: desteklenen senaryolar ve iOS aygıtlarıyla çözümlerde Azure Active Directory için sertifika tabanlı kimlik doğrulamasını yapılandırma gereksinimleri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/17/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76c5e18a0bf84e96476eafd7ff35398049f1a492
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81639623"
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>iOS'ta Azure Active Directory sertifikası tabanlı kimlik doğrulama

Güvenliği artırmak için iOS aygıtları, aşağıdaki uygulamalara veya hizmetlere bağlanırken aygıtlarında istemci sertifikası kullanarak Azure Etkin Dizinin (Azure AD) kimlik doğrulaması (Azure AD) kimlik doğrulaması için sertifika tabanlı kimlik doğrulama (CBA) kullanabilir:

* Microsoft Outlook ve Microsoft Word gibi Office mobil uygulamaları
* Exchange ActiveSync (EAS) istemcileri

Sertifikaları kullanmak, mobil cihazınızdaki belirli postalara ve Microsoft Office uygulamalarına kullanıcı adı ve parola kombinasyonu girme gereksinimini ortadan kaldırır.

Bu makalede, bir iOS aygıtında CBA yapılandırmak için gereksinimleri ve desteklenen senaryolar ayrıntıları. iOS için CBA, Azure genel bulutları, Microsoft Public Cloud, Microsoft Cloud Germany ve Microsoft Azure China 21Vianet'te kullanılabilir.

## <a name="microsoft-mobile-applications-support"></a>Microsoft mobil uygulamaları desteği

| Uygulamalar | Destek |
| --- | --- |
| Azure Bilgi Koruması uygulaması |![Bu uygulama için desteği simgeleyen işaret][1] |
| Intune Şirket Portalı |![Bu uygulama için desteği simgeleyen işaret][1] |
| Microsoft Teams |![Bu uygulama için desteği simgeleyen işaret][1] |
| OneNote |![Bu uygulama için desteği simgeleyen işaret][1] |
| OneDrive |![Bu uygulama için desteği simgeleyen işaret][1] |
| Outlook |![Bu uygulama için desteği simgeleyen işaret][1] |
| Power BI |![Bu uygulama için desteği simgeleyen işaret][1] |
| Skype Kurumsal |![Bu uygulama için desteği simgeleyen işaret][1] |
| Word / Excel / PowerPoint |![Bu uygulama için desteği simgeleyen işaret][1] |
| Yammer |![Bu uygulama için desteği simgeleyen işaret][1] |

## <a name="requirements"></a>Gereksinimler

iOS ile CBA kullanmak için aşağıdaki gereksinimler ve hususlar geçerlidir:

* Aygıt işletim sistemi sürümü iOS 9 veya üzerinde olmalıdır.
* iOS'taki Office uygulamaları için Microsoft Kimlik Doğrulayıcı gereklidir.
* MacOS Anahtarlığı'nda ADFS sunucusunun kimlik doğrulama URL'sini içeren bir kimlik tercihi oluşturulmalıdır. Daha fazla bilgi için [bkz.](https://support.apple.com/guide/keychain-access/create-an-identity-preference-kyca6343b6c9/mac)

Aşağıdaki Active Directory Federation Services (ADFS) gereksinimleri ve hususlar geçerlidir:

* Sertifika kimlik doğrulaması için ADFS sunucusu etkinleştirilmeli ve federe kimlik doğrulaması kullanılmalıdır.
* Sertifikanın Gelişmiş Anahtar Kullanımı (EKU) kullanması ve *Konu Alternatif Adı'ndaki (NT Asıl Adı)* kullanıcının UPN'sini içermesi gerekir.

## <a name="configure-adfs"></a>ADFS'yi yapılandırma

Azure AD'nin istemci sertifikasını iptal edebilmek için ADFS belirteci aşağıdaki taleplere sahip olmalıdır. Azure AD, ADFS belirtecinde (veya başka bir SAML belirtecinde) kullanılabilirse bu talepleri yenileme belirtecine ekler. Yenileme belirteci nin doğrulanması gerektiğinde, bu bilgiler iptali denetlemek için kullanılır:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`- müşteri sertifikanızın seri numarasını ekleyin
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`- istemci sertifikanızın vereni için dize ekleyin

En iyi yöntem olarak, kuruluşunuzun ADFS hata sayfalarını aşağıdaki bilgilerle güncelleştirmeniz gerekir:

* Microsoft Authenticator'ı iOS'a yükleme gereksinimi.
* Kullanıcı sertifikası alma yla ilgili talimatlar.

Daha fazla bilgi için bkz: [AD FS oturum açını sayfaya özelleştirme.](https://technet.microsoft.com/library/dn280950.aspx)

## <a name="use-modern-authentication-with-office-apps"></a>Office uygulamalarıyla modern kimlik doğrulamayı kullanma

Modern kimlik doğrulaması etkin `prompt=login` olan bazı Office uygulamaları, istekleri nde Azure AD'ye gönderir. Varsayılan olarak, Azure `prompt=login` AD isteği ADFS'ye çevirir `wauth=usernamepassworduri` (ADFS'den U/P `wfresh=0` Auth yapmasını ister) ve (ADFS'den SSO durumunu yoksaymasını ve yeni bir kimlik doğrulaması yapmasını ister). Bu uygulamalar için sertifika tabanlı kimlik doğrulamasını etkinleştirmek istiyorsanız, varsayılan Azure REKLAM davranışını değiştirin.

Varsayılan davranışı güncelleştirmek için, federe etki alanı ayarlarınızdaki '*PromptLoginBehavior*' 'u *Devre Dışı Bırakılmış*olarak ayarlayın. Aşağıdaki örnekte gösterildiği gibi, bu görevi gerçekleştirmek için [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet'i kullanabilirsiniz:

```powershell
Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled
```

## <a name="support-for-exchange-activesync-clients"></a>Exchange ActiveSync istemcileri için destek

iOS 9 veya sonraki adreste, yerel iOS posta istemcisi desteklenir. Bu özelliğin diğer tüm Exchange ActiveSync uygulamaları için desteklenip desteklenmeyip desteklenmeyip desteklenmeyip desteklenmedisini belirlemek için uygulama geliştiricinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda sertifika tabanlı kimlik doğrulaması yapılandırmak [için](active-directory-certificate-based-authentication-get-started.md) bkz.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
