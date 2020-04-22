---
title: Android sertifika tabanlı kimlik doğrulama - Azure Active Directory
description: Desteklenen senaryolar ve Android cihazlarla çözümlerde sertifika tabanlı kimlik doğrulamayı yapılandırma gereksinimleri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: annaba
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9760624afec111a271ae5aa0ebbe5533d6ba8d6
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680204"
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a>Android'de Azure Active Directory sertifikası tabanlı kimlik doğrulama

Android cihazlar, aşağıdaki lere bağlanırken cihazlarındaki istemci sertifikasını kullanarak Azure Active Directory'ye kimlik doğrulaması yapmak için sertifika tabanlı kimlik doğrulama (CBA) kullanabilir:

* Microsoft Outlook ve Microsoft Word gibi Office mobil uygulamaları
* Exchange ActiveSync (EAS) istemcileri

Bu özelliğin yapılandırılması, mobil cihazınızdaki belirli postalara ve Microsoft Office uygulamalarına bir kullanıcı adı ve parola kombinasyonu girme gereksinimini ortadan kaldırır.

Bu konu, Office 365 Enterprise, Business, Education, US Government, China ve Germany planlarında kiracı kullanıcılar için Bir Android cihazda CBA yapılandırmak için gereksinimleri ve desteklenen senaryoları sağlar.

Bu özellik, Office 365 ABD Hükümeti Savunması ve Federal planlarında önizlemede kullanılabilir.

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

### <a name="implementation-requirements"></a>Uygulama gereksinimleri

Cihaz işletim sistemi sürümü Android 5.0 (Lollipop) ve üzeri olmalıdır.

Bir federasyon sunucusu yapılandırılmalıdır.

Azure Etkin Dizin'in bir istemci sertifikasını iptal edebilmek için ADFS belirteci aşağıdaki iddialara sahip olmalıdır:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`(İstemci sertifikasının seri numarası)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`(İstemci sertifikasını veren in dizesi)

Azure Etkin Dizin, ADFS belirtecinde (veya başka bir SAML belirtecinde) kullanılabilirse, bu talepleri yenileme belirtecine ekler. Yenileme belirteci nin doğrulanması gerektiğinde, bu bilgiler iptali denetlemek için kullanılır.

En iyi yöntem olarak, kuruluşunuzun ADFS hata sayfalarını aşağıdaki bilgilerle güncelleştirmeniz gerekir:

* Microsoft Authenticator'ı Android'e yükleme gereksinimi.
* Kullanıcı sertifikası alma yla ilgili talimatlar.

Daha fazla bilgi için, [AD FS Oturum Açma Sayfalarını Özelleştirme'ye](https://technet.microsoft.com/library/dn280950.aspx)bakın.

Bazı Office uygulamaları (modern kimlik doğrulaması etkinleştirilmiş) isteklerinde Azure AD'ye '*istemi=giriş*' gönderir. Varsayılan olarak, Azure AD '*prompt=login*' ia' olarak ADFS'ye *'istek=giriş*' (ADFS'den U/P Auth yapmasını ister) ve '*wfresh=0*' olarak çevirir (ADFS'den SSO durumunu yoksaymasını ve yeni bir kimlik doğrulaması yapmasını ister). Bu uygulamalar için sertifika tabanlı kimlik doğrulamasını etkinleştirmek istiyorsanız, varsayılan Azure AD davranışını değiştirmeniz gerekir. Federe etki alanı ayarlarınızdaki '*PromptLoginBehavior*' u '*Devre Dışı Bırakılmış*' olarak ayarlayın.
Bu görevi gerçekleştirmek için [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet'i kullanabilirsiniz:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync istemcileri desteği

Android 5.0 (Lollipop) veya sonraki bazı Exchange ActiveSync uygulamaları desteklenir. E-posta uygulamanızın bu özelliği destekletip desteklemediğini belirlemek için uygulama geliştiricinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda sertifika tabanlı kimlik doğrulaması yapılandırmak istiyorsanız, talimatlar için [Android'de sertifika tabanlı kimlik doğrulamasına başlayın'a](active-directory-certificate-based-authentication-get-started.md) bakın.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
