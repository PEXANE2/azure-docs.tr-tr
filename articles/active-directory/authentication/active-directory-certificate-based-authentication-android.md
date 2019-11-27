---
title: Android sertifika tabanlı kimlik doğrulaması-Azure Active Directory
description: Android cihazlarıyla çözümlerinde desteklenen senaryolar ve sertifika tabanlı kimlik doğrulaması yapılandırma gereksinimleri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: annaba
ms.collection: M365-identity-device-management
ms.openlocfilehash: effa03f042b44890fccd474128e75bd1c0f782a3
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74381981"
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Android 'de sertifika tabanlı kimlik doğrulamayı Azure Active Directory

Android cihazlar, bağlantısı sırasında cihazlarındaki istemci sertifikası kullanarak Azure Active Directory kimlik doğrulaması yapmak için sertifika tabanlı kimlik doğrulaması (CBA) kullanabilir:

* Microsoft Outlook ve Microsoft Word gibi Office mobil uygulamaları
* Exchange ActiveSync (EAS) istemcileri

Bu özelliği yapılandırmak, mobil cihazınızda belirli e-posta ve Microsoft Office uygulamalarına bir Kullanıcı adı ve parola birleşimi girme gereksinimini ortadan kaldırır.

Bu konu, Office 365 Kurumsal, Iş, eğitim, ABD Kamu, Çin ve Almanya planlarındaki kiracılar kullanıcılarına yönelik bir iOS (Android) cihazında CBA 'yı yapılandırmaya yönelik gereksinimler ve desteklenen senaryolar sağlar.

Bu özellik, Office 365 ABD kamu savunması ve Federal planlarında önizlemede sunulmaktadır.

## <a name="microsoft-mobile-applications-support"></a>Microsoft mobil uygulamalar desteği

| Uygulamalar | Destek |
| --- | --- |
| Azure Information Protection uygulaması |![Bu uygulama için desteği belirtir onay işareti][1] |
| Intune Şirket Portalı |![Bu uygulama için desteği belirtir onay işareti][1] |
| Microsoft Teams |![Bu uygulama için desteği belirtir onay işareti][1] |
| OneNote |![Bu uygulama için desteği belirtir onay işareti][1] |
| OneDrive |![Bu uygulama için desteği belirtir onay işareti][1] |
| Outlook |![Bu uygulama için desteği belirtir onay işareti][1] |
| Power BI |![Bu uygulama için desteği belirtir onay işareti][1] |
| Skype Kurumsal |![Bu uygulama için desteği belirtir onay işareti][1] |
| Word/Excel/PowerPoint |![Bu uygulama için desteği belirtir onay işareti][1] |
| Yammer |![Bu uygulama için desteği belirtir onay işareti][1] |

### <a name="implementation-requirements"></a>Uygulama gereksinimleri

Cihazın işletim sistemi sürümü, Android 5,0 (Lollipop) ve üzeri olmalıdır.

Federasyon sunucusunun yapılandırılması gerekir.

İstemci sertifikasını iptal etmek için Azure Active Directory için, ADFS belirtecinin aşağıdaki talepleri olması gerekir:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>` (istemci sertifikasının seri numarası)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>` (istemci sertifikasını veren için dize)

Azure Active Directory, bu talepleri ADFS belirtecinde (veya başka bir SAML belirteci) varsa yenileme belirtecine ekler. Yenileme belirtecinin doğrulanması gerektiğinde, bu bilgiler iptali denetlemek için kullanılır.

En iyi uygulama olarak, kuruluşunuzun ADFS hata sayfalarını aşağıdaki bilgilerle güncelleştirmeniz gerekir:

* Android 'e Microsoft Authenticator yükleme gereksinimi.
* Kullanıcı sertifikası alma yönergeleri.

Daha fazla bilgi için bkz. [AD FS oturum açma sayfalarını özelleştirme](https://technet.microsoft.com/library/dn280950.aspx).

Bazı Office uygulamaları (modern kimlik doğrulaması etkinleştirilmiş), istekleri için Azure AD 'ye '*Prompt = Login*' gönderin. Varsayılan olarak, Azure AD '*wauth = usernamepassworduri*' olarak ADFS 'e istekte bulunan '*Prompt = Login*' değerini çevirir (ADFS 'Nin U/P kimlik doğrulamasını yapması ister) ve '*wyeni = 0*' (ADFS 'in SSO durumunu yok saymasını ve yeni bir kimlik doğrulaması yapması istenir). Bu uygulamalar için sertifika tabanlı kimlik doğrulamasını etkinleştirmek istiyorsanız, varsayılan Azure AD davranışını değiştirmeniz gerekir. Federal etki alanı ayarlarınızda '*Promptloginbehavior*' ayarını '*Disabled*' olarak ayarlayın.
Bu görevi gerçekleştirmek için [Msoldomainfederationsettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet 'ini kullanabilirsiniz:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync istemcileri desteği

Android 5,0 (Lollipop) veya sonraki sürümlerde belirli Exchange ActiveSync uygulamaları desteklenir. E-posta uygulamanızın bu özelliği desteklediğini öğrenmek için uygulama geliştiricinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda sertifika tabanlı kimlik doğrulaması yapılandırmak istiyorsanız, yönergeler için bkz. [Android 'de sertifika tabanlı kimlik doğrulamayı kullanmaya başlama](active-directory-certificate-based-authentication-get-started.md) .

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
