---
title: Telefon kaydı ve özel ilkelerle oturum açma
titleSuffix: Azure AD B2C
description: Metin iletilerinde, Azure Active Directory B2C özel ilkelerle Kullanıcı telefonlarına tek seferlik parolalar gönderme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8cb0340d9e04db2bfbf088bce9505351d7588cd9
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840341"
---
# <a name="set-up-phone-sign-up-and-sign-in-with-custom-policies-in-azure-ad-b2c"></a>Azure AD B2C özel ilkelerle telefon kayıt ve oturum açma ayarlama

Azure Active Directory B2C (Azure AD B2C) telefon kaydı ve oturum açma, kullanıcılarınızın telefonunuza bir kısa mesajdan gönderilen bir kerelik parola (OTP) kullanarak uygulamalarınıza kaydolmalarını ve oturum açmasını sağlar. Bir kerelik parolalar, kullanıcılarınızın parolalarının tehlikeye düşmesi veya bu uygulamaların güvenliğinin aşılmasına neden olması riskini en aza indirmenize yardımcı olabilir.

Müşterilerinizin, telefonunuza bir kerelik parola kullanarak uygulamalarınıza kaydolup oturum açmasını sağlamak üzere özel ilkeleri kullanmak için bu makaledeki adımları izleyin.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Azure AD B2C kiracı](tutorial-create-tenant.md)
* Kiracınızda [kayıtlı Web uygulaması](tutorial-register-applications.md)
* Kiracınıza yüklenen [özel ilkeler](custom-policy-get-started.md)

## <a name="get-the-phone-sign-up--sign-in-starter-pack"></a>Telefon kaydolma & oturum açma başlangıç paketini alın

Telefon kayıt ve oturum açma özel ilke dosyalarını güncelleştirerek Azure AD B2C kiracınızla çalışacak şekilde çalışmaya başlayın.

Aşağıdaki adımlarda [önkoşulları](#prerequisites) tamamladığınız ve [özel ilke Başlatıcı paketi][starter-pack] deposunu zaten yerel makinenize Klonladığınız varsayılır.

1. Başlangıç paketi deposunun yerel kopyanızda [Telefon kayıt ve oturum açma özel ilke dosyalarını][starter-pack-phone] bulun veya doğrudan indirin. XML ilke dosyaları şu dizinde bulunur:

    `active-directory-b2c-custom-policy-starterpack/scenarios/`**`phone-number-passwordless`**

1. Her dosyada, dize `yourtenant` Azure AD B2C kiracınızın adıyla değiştirin. Örneğin, B2C kiracınızın adı *contosob2c*ise `yourtenant.onmicrosoft.com` tüm örnekleri `contosob2c.onmicrosoft.com`olur.

1. [Azure Active Directory B2C özel ilkeleri kullanmaya başlama](custom-policy-get-started.md)konusunun [özel Ilkeye uygulama kimlikleri ekleme](custom-policy-get-started.md#add-application-ids-to-the-custom-policy) bölümündeki adımları uygulayın. Bu durumda, *IdentityExperienceFramework* ve *ProxyIdentityExperienceFramework*önkoşullarını tamamlarken kaydettiğiniz iki uygulamanın **uygulama (istemci) kimlikleriyle** `/phone-number-passwordless/` **`Phone_Email_Base.xml`** güncelleştirin.

## <a name="upload-the-policy-files"></a>İlke dosyalarını karşıya yükle

1. [Azure Portal](https://portal.azure.com) oturum açın ve Azure AD B2C kiracınıza gidin.
1. **İlkeler**altında **kimlik deneyimi çerçevesi**' ni seçin.
1. **Özel Ilkeyi karşıya yükle**' yi seçin.
1. İlke dosyalarını aşağıdaki sırada karşıya yükleyin:
    1. *Phone_Email_Base. xml*
    1. *Signuporsignınwithphone. xml*
    1. *SignUpOrSignInWithPhoneOrEmail. xml*
    1. *ProfileEditPhoneOnly. xml*
    1. *ProfileEditPhoneEmail. xml*
    1. *ChangePhoneNumber. xml*
    1. *PasswordResetEmail. xml*

Her dosyayı karşıya yüklerken Azure `B2C_1A_`ön eki ekler.

## <a name="test-the-custom-policy"></a>Özel ilkeyi test etme

1. **Özel ilkeler**altında **B2C_1A_SignUpOrSignInWithPhone**' yi seçin.
1. **Uygulama Seç**altında önkoşulları tamamlarken kaydettiğiniz *WebApp1* uygulamasını seçin.
1. **Yanıt URL 'Si Seç**için `https://jwt.ms`seçin.
1. **Şimdi Çalıştır** ' ı seçin ve bir e-posta adresi veya telefon numarası kullanarak kaydolun.
1. **Şimdi Çalıştır** ' ı bir kez daha seçin ve doğru yapılandırmaya sahip olduğunu onaylamak için aynı hesapla oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

GitHub 'da telefon kayıt ve oturum açma özel İlkesi başlangıç paketini (ve diğer başlangıç paketleri) bulabilirsiniz:

[Azure-Samples/Active-Directory-B2C-Custom-Policy-starterpack/senaryolar/Phone-Number-passwordless][starter-pack-phone]

Başlangıç paketi ilke dosyaları Multi-Factor Authentication teknik profillerini ve telefon numarası talep dönüşümlerini kullanır:

* [Azure Multi-Factor Authentication teknik profili tanımlama](multi-factor-auth-technical-profile.md)
* [Telefon numarası talep dönüşümlerini tanımlayın](phone-number-claims-transformations.md)

<!-- LINKS - External -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
[starter-pack-phone]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/phone-number-passwordless
