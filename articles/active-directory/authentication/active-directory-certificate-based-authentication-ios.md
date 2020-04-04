---
title: iOS'ta sertifika tabanlı kimlik doğrulama - Azure Active Directory
description: desteklenen senaryolar ve iOS aygıtlarıyla çözümlerde sertifika tabanlı kimlik doğrulamayı yapılandırma gereksinimleri hakkında bilgi edinin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: annaba
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8a042897ecbe35935c1832a53f523eb0597ebafc
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80654236"
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a>iOS'ta Azure Active Directory sertifikası tabanlı kimlik doğrulama

iOS aygıtları, aşağıdakilere bağlanırken aygıtlarında bir istemci sertifikası kullanarak Azure Active Directory'ye kimlik doğrulaması yapmak için sertifika tabanlı kimlik doğrulama (CBA) kullanabilir:

* Microsoft Outlook ve Microsoft Word gibi Office mobil uygulamaları
* Exchange ActiveSync (EAS) istemcileri

Bu özelliğin yapılandırılması, mobil cihazınızdaki belirli postalara ve Microsoft Office uygulamalarına bir kullanıcı adı ve parola kombinasyonu girme gereksinimini ortadan kaldırır.

Bu konu, Office 365 Enterprise, Business, Education, US Government, China ve Almanya planlarında kiracı kullanıcılar için cba'yı bir iOS (Android) cihazında yapılandırmak için gereksinimleri ve desteklenen senaryoları sağlar.

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

## <a name="requirements"></a>Gereksinimler

Aygıt işletim sistemi sürümü iOS 9 ve üzeri olmalıdır

Bir federasyon sunucusu yapılandırılmalıdır.

iOS'taki Office uygulamaları için Microsoft Kimlik Doğrulayıcı gereklidir.

Azure Etkin Dizin'in bir istemci sertifikasını iptal edebilmek için ADFS belirteci aşağıdaki iddialara sahip olmalıdır:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`(İstemci sertifikasının seri numarası)
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`(İstemci sertifikasını veren in dizesi)

Azure Etkin Dizin, ADFS belirtecinde (veya başka bir SAML belirtecinde) kullanılabilirse, bu talepleri yenileme belirtecine ekler. Yenileme belirteci nin doğrulanması gerektiğinde, bu bilgiler iptali denetlemek için kullanılır.

En iyi yöntem olarak, kuruluşunuzun ADFS hata sayfalarını aşağıdaki bilgilerle güncelleştirmeniz gerekir:

* Microsoft Authenticator'ı iOS'a yükleme gereksinimi
* Kullanıcı sertifikası alma yla ilgili talimatlar.

Daha fazla bilgi için, [AD FS Oturum Açma Sayfalarını Özelleştirme'ye](https://technet.microsoft.com/library/dn280950.aspx)bakın.

Bazı Office uygulamaları (modern kimlik doğrulaması etkinleştirilmiş) isteklerinde Azure AD'ye '*istemi=giriş*' gönderir. Varsayılan olarak, Azure AD '*prompt=login*' ia' olarak ADFS'ye *'istek=giriş*' (ADFS'den U/P Auth yapmasını ister) ve '*wfresh=0*' olarak çevirir (ADFS'den SSO durumunu yoksaymasını ve yeni bir kimlik doğrulaması yapmasını ister). Bu uygulamalar için sertifika tabanlı kimlik doğrulamasını etkinleştirmek istiyorsanız, varsayılan Azure AD davranışını değiştirmeniz gerekir. Federe etki alanı ayarlarınızdaki '*PromptLoginBehavior*'*'ı ' Devre Dışı Bırakılmış*' olarak ayarlamanız.
Bu görevi gerçekleştirmek için [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet'i kullanabilirsiniz:

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`

## <a name="exchange-activesync-clients-support"></a>Exchange ActiveSync istemcileri desteği

iOS 9 veya sonraki adreste, yerel iOS posta istemcisi desteklenir. Diğer tüm Exchange ActiveSync uygulamaları için, bu özelliğin desteklenip desteklenmeip desteklenmeyip desteklenmeyip desteklenmeyip desteklenmeyini belirlemek için uygulama geliştiricinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Ortamınızda sertifika tabanlı kimlik doğrulaması yapılandırmak istiyorsanız, talimatlar için [Android'de sertifika tabanlı kimlik doğrulamasına başlayın'a](../authentication/active-directory-certificate-based-authentication-get-started.md) bakın.

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
