---
title: İOS Azure Active Directory Sertifika tabanlı kimlik doğrulaması
description: İOS cihazlarıyla çözümlerinde Azure Active Directory için sertifika tabanlı kimlik doğrulaması yapılandırma gereksinimleri ve desteklenen senaryolar hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/17/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5ede7ddb81bae69d92983e787e779ee9d410bd87
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82144078"
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>İOS 'ta sertifika tabanlı kimlik doğrulamasını Azure Active Directory

İOS cihazları güvenliği artırmak için sertifika tabanlı kimlik doğrulaması (CBA) kullanarak, aşağıdaki uygulamalara veya hizmetlere bağlanırken cihazlarındaki istemci sertifikası kullanarak Azure Active Directory (Azure AD) kimlik doğrulaması yapabilir:

* Microsoft Outlook ve Microsoft Word gibi Office mobil uygulamaları
* Exchange ActiveSync (EAS) istemcileri

Sertifikaları kullanmak, mobil cihazınızda belirli e-posta ve Microsoft Office uygulamalarına bir Kullanıcı adı ve parola birleşimi girme gereksinimini ortadan kaldırır.

Bu makalede, iOS cihazında CBA 'yi yapılandırmaya yönelik gereksinimler ve desteklenen senaryolar ayrıntılı olarak anlatılmaktadır. İOS için CBA, Azure genel bulutları, Microsoft kamu bulutu, Microsoft Bulut Almanya ve Microsoft Azure Çin 21Vianet genelinde sunulmaktadır.

## <a name="microsoft-mobile-applications-support"></a>Microsoft mobil uygulamalar desteği

| Uygulamalar | Destek |
| --- | --- |
| Azure Information Protection uygulaması |![Bu uygulama için desteği belirtir onay işareti][1] |
| Intune Şirket Portalı |![Bu uygulama için desteği belirtir onay işareti][1] |
| Microsoft Teams |![Bu uygulama için desteği belirtir onay işareti][1] |
| Office (mobil) |![Bu uygulama için desteği belirtir onay işareti][1] |
| OneNote |![Bu uygulama için desteği belirtir onay işareti][1] |
| OneDrive |![Bu uygulama için desteği belirtir onay işareti][1] |
| Outlook |![Bu uygulama için desteği belirtir onay işareti][1] |
| Power BI |![Bu uygulama için desteği belirtir onay işareti][1] |
| Skype Kurumsal |![Bu uygulama için desteği belirtir onay işareti][1] |
| Word/Excel/PowerPoint |![Bu uygulama için desteği belirtir onay işareti][1] |
| Yammer |![Bu uygulama için desteği belirtir onay işareti][1] |

## <a name="requirements"></a>Gereksinimler

CBA 'yı iOS ile kullanmak için aşağıdaki gereksinimler ve önemli noktalar geçerlidir:

* Cihazın işletim sistemi sürümü iOS 9 veya üzeri olmalıdır.
* İOS üzerinde Office uygulamaları için Microsoft Authenticator gereklidir.
* ADFS sunucusunun kimlik doğrulama URL 'sini içeren macOS Anahtarlığınızdan bir kimlik tercihi oluşturulmalıdır. Daha fazla bilgi için bkz. [Mac 'Te Anahtarlık erişimi için kimlik tercihi oluşturma](https://support.apple.com/guide/keychain-access/create-an-identity-preference-kyca6343b6c9/mac).

Aşağıdaki Active Directory Federasyon Hizmetleri (AD FS) (ADFS) gereksinimleri ve konuları geçerlidir:

* ADFS sunucusunun sertifika kimlik doğrulaması için etkinleştirilmesi ve federal kimlik doğrulaması kullanılması gerekir.
* Sertifikanın, gelişmiş anahtar kullanımı (EKU) kullanması ve *konu alternatif adında (NT asıl adı)* kullanıcının UPN 'sini içermesi gerekir.

## <a name="configure-adfs"></a>ADFS 'yi yapılandırma

Azure AD 'nin bir istemci sertifikasını iptal etmek için, ADFS belirtecinin aşağıdaki talepleri olması gerekir. Azure AD, ADFS belirtecinde (veya başka bir SAML belirteci) kullanılabiliyorsa bu talepleri yenileme belirtecine ekler. Yenileme belirtecinin doğrulanması gerektiğinde, bu bilgiler iptali denetlemek için kullanılır:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`-istemci sertifikanızın seri numarasını ekleyin
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`-İstemci sertifikanızı veren için dizeyi ekleyin

En iyi uygulama olarak, kuruluşunuzun ADFS hata sayfalarını aşağıdaki bilgilerle de güncelleştirmeniz gerekir:

* Microsoft Authenticator iOS üzerinde yükleme gereksinimi.
* Kullanıcı sertifikası alma yönergeleri.

Daha fazla bilgi için bkz. [AD FS oturum açma sayfasını özelleştirme](https://technet.microsoft.com/library/dn280950.aspx).

## <a name="use-modern-authentication-with-office-apps"></a>Office uygulamalarıyla modern kimlik doğrulaması kullanma

Modern kimlik doğrulaması etkinleştirilmiş bazı Office uygulamaları `prompt=login` , istekleri Içinde Azure AD 'ye gönderilir. Varsayılan olarak, Azure AD `prompt=login` , ISTEğI ADFS 'ye dönüştürür `wauth=usernamepassworduri` (ADFS 'nin U/P auth 'yi yapması istenir) ve `wfresh=0` (ADFS 'nin SSO durumunu yok saymasını ve yeni bir kimlik doğrulaması aramasını ister). Bu uygulamalar için sertifika tabanlı kimlik doğrulamasını etkinleştirmek istiyorsanız, varsayılan Azure AD davranışını değiştirin.

Varsayılan davranışı güncelleştirmek için, Federasyon etki alanı ayarlarınızda '*Promptloginbehavior*' ayarını *devre dışı*olarak ayarlayın. Bu görevi gerçekleştirmek için aşağıdaki örnekte gösterildiği gibi [Msoldomainfederationsettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet 'ini kullanabilirsiniz:

```powershell
Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled
```

## <a name="support-for-exchange-activesync-clients"></a>Exchange ActiveSync istemcileri için destek

İOS 9 veya sonraki sürümlerde, Yerel iOS posta istemcisi desteklenir. Bu özelliğin diğer tüm Exchange ActiveSync uygulamaları için desteklenip desteklenmediğini öğrenmek için uygulama geliştiricinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda sertifika tabanlı kimlik doğrulamasını yapılandırmak için, bkz. yönergeler için [sertifika tabanlı kimlik doğrulaması ile çalışmaya başlama](active-directory-certificate-based-authentication-get-started.md) .

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
