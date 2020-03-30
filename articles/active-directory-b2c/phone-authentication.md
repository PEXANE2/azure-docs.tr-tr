---
title: Özel ilkelerle telefona kaydolma ve oturum açma (Önizleme)
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel ilkelerle uygulama kullanıcılarınızın telefonlarına kısa mesajlarla tek seferlik parolalar (OTP) gönderin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: eadac0e973b361b1fdee63dcc9cfa848a0b2bacb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183967"
---
# <a name="set-up-phone-sign-up-and-sign-in-with-custom-policies-in-azure-ad-b2c-preview"></a>Azure AD B2C'de özel ilkelerle telefon kaydolma ve oturum açma (Önizleme) ayarlama

Azure Active Directory B2C'de (Azure AD B2C) telefona kaydolma ve kaydolma, kullanıcılarınızın telefonlarına kısa mesajla gönderilen tek seferlik bir parola (OTP) kullanarak uygulamalarınıza kaydolmasını ve oturum açmalarına olanak tanır. Tek seferlik parolalar, kullanıcılarınızın parolalarını unutma veya parolalarını tehlikeye atma riskini en aza indirmeye yardımcı olabilir.

Müşterilerinizin telefonlarına gönderilen tek seferlik bir parolayı kullanarak uygulamalarınıza kaydolmalarını ve uygulamalarınızda oturum açmalarını sağlamak için bu makaledeki adımları izleyin.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="pricing"></a>Fiyatlandırma

Sms kısa mesajları kullanılarak kullanıcılarınıza tek seferlik parolalar gönderilir ve gönderilen her mesaj için ücretlendirilebilirsiniz. Fiyatlandırma bilgileri için Azure Active [Directory B2C fiyatlandırmasının](https://azure.microsoft.com/pricing/details/active-directory-b2c/) **Ayrı Ücretler** bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

OTP'yi kurmadan önce aşağıdaki kaynaklara ihtiyacınız var.

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* Kiracınızda [kayıtlı web uygulaması](tutorial-register-applications.md)
* Kiracınıza yüklenen [özel ilkeler](custom-policy-get-started.md)

## <a name="get-the-phone-sign-up--sign-in-starter-pack"></a>Kaydolma başlangıç paketini & telefona kaydolun

Azure AD B2C kiracınızla çalışmak için telefon kayıt ve oturum açma özel ilke dosyalarını güncelleyerek başlayın.

Aşağıdaki adımlar, [ön koşulları](#prerequisites) tamamladığınızı ve yerel makinenize [özel ilke başlangıç paketi][starter-pack] deposunu zaten klonladığınızı varsayar.

1. Başlangıç paketi repo'nun yerel klonunuzda [telefon kaydolma ve kaydolma özel ilke dosyalarını][starter-pack-phone] bulun veya doğrudan indirin. XML ilke dosyaları aşağıdaki dizinde bulunur:

    `active-directory-b2c-custom-policy-starterpack/scenarios/`**`phone-number-passwordless`**

1. Her dosyada, dizeyi `yourtenant` Azure AD B2C kiracınızın adıyla değiştirin. Örneğin, B2C kiracınızın adı *contosob2c*ise, tüm `yourtenant.onmicrosoft.com` örnekleri `contosob2c.onmicrosoft.com`.

1. [Azure Active Directory B2C'deki özel ilkelerle başlayın'ın](custom-policy-get-started.md) [özel ilke bölümüne uygulama ii'leri ekle](custom-policy-get-started.md#add-application-ids-to-the-custom-policy) adımlarını tamamlayın. Bu durumda, `/phone-number-passwordless/` **`Phone_Email_Base.xml`** ön koşulları tamamlarken kaydettiğiniz iki uygulamanın **Uygulama (istemci) kimliklerini,** *IdentityExperienceFramework'ü* ve *ProxyIdentityExperienceFramework'ü*güncelleyin.

## <a name="upload-the-policy-files"></a>İlke dosyalarını yükleme

1. [Azure portalında](https://portal.azure.com) oturum açın ve Azure AD B2C kiracınıza gidin.
1. **İlkeler**altında Kimlik **Deneyimi Çerçevesi'ni**seçin.
1. **Özel politika yükle'yi**seçin.
1. İlke dosyalarını aşağıdaki sırayla yükleyin:
    1. *Phone_Email_Base.xml*
    1. *SignUpOrSignInWithPhone.xml*
    1. *SignUpOrSignInWithPhoneOrEmail.xml*
    1. *ProfileEditPhoneOnly.xml*
    1. *ProfilEditPhoneEmail.xml*
    1. *ChangePhoneNumber.xml*
    1. *PasswordResetEmail.xml*

Her dosyayı yüklerken Azure önekini `B2C_1A_`ekler.

## <a name="test-the-custom-policy"></a>Özel ilkeyi test edin

1. **Özel ilkeler**altında, **B2C_1A_SignUpOrSignInWithPhone**seçin.
1. **Select uygulaması**altında, ön koşulları tamamlarken kaydettiğiniz *webapp1* uygulamasını seçin.
1. **Yanıt url'sini** `https://jwt.ms`seç , seçin.
1. **Şimdi Çalıştır'ı** seçin ve bir e-posta adresi veya telefon numarası kullanarak kaydolun.
1. **Şimdi çalıştır'ı** bir kez daha seçin ve doğru yapılandırmaya sahip olduğunuzu doğrulamak için aynı hesapla oturum açın.

## <a name="get-user-account-by-phone-number"></a>Telefon numarasına göre kullanıcı hesabı alma

Bir telefon numarasıyla kaydolan ancak kurtarma e-posta adresi sağlamayan bir kullanıcı, oturum açma adı olarak telefon numarasıyla Birlikte Azure AD B2C dizininize kaydedilir. Kullanıcı daha sonra telefon numarasını değiştirmek isterse, yardım masanızın veya destek ekibinizin önce hesaplarını bulması ve ardından telefon numarasını güncellemesi gerekir.

[Microsoft Graph'ı](manage-user-accounts-graph-api.md)kullanarak bir kullanıcıyı telefon numarasına (oturum açma adına) göre bulabilirsiniz:

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+{phone number}' and c/issuer eq '{tenant name}.onmicrosoft.com')
```

Örnek:

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+450334567890' and c/issuer eq 'contosob2c.onmicrosoft.com')
```

## <a name="next-steps"></a>Sonraki adımlar

Telefon kayıt ve oturum açma özel ilke başlangıç paketini (ve diğer başlangıç paketlerini) GitHub'da bulabilirsiniz:

[Azure-Örnekler/etkin-dizin-b2c-özel-ilke-başlangıç paketi/senaryolar/telefon-numara-şifresiz][starter-pack-phone]

Başlangıç paketi ilke dosyaları çok faktörlü kimlik doğrulama teknik profilleri ve telefon numarası talepleri dönüşümleri kullanın:

* [Azure Çok Faktörlü Kimlik Doğrulama teknik profilini tanımla](multi-factor-auth-technical-profile.md)
* [Telefon numarası talep dönüşümlerini tanımlama](phone-number-claims-transformations.md)

<!-- LINKS - External -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
[starter-pack-phone]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/phone-number-passwordless
