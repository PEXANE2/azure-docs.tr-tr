---
title: Uygulamalarım portalı ile ilgili yardım alın-Azure Active Directory | Microsoft Docs
description: Uygulamalarım portalında oturum açma ve ortak görevleri gerçekleştirme konusunda yardım alın.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 03/21/2019
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: c5d1b08944417493d175de23c7738db58f1c7bd3
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88798015"
---
# <a name="troubleshoot-problems-with-the-my-apps-portal"></a>Uygulamalarım portalındaki sorunları giderme

**Uygulamalarım** portalı 'nda oturum açma veya kullanma ile ilgili sorun yaşıyorsanız yardım için yardım masasına veya yöneticinize tıklamadan önce bu sorun giderme ipuçlarını deneyin.

## <a name="im-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Uygulamalarım güvenli oturum açma uzantısını yüklerken sorun yaşıyorum

Uygulamalarım güvenli oturum açma uzantısını yüklerken sorun yaşıyorsanız:

- Aşağıdakiler de dahil olmak üzere desteklenen bir tarayıcı kullandığınızdan emin olun:

    - **Microsoft Edge.** Windows 10 yıldönümü sürümünde veya sonraki sürümlerde çalışır.

    - **Google Chrome.** Windows 7 veya sonraki sürümlerde ve Mac OS X veya üzeri sürümlerde çalışır.

    - **Mozilla Firefox 26,0 veya üzeri.** Windows XP SP2 veya sonraki sürümlerde ve Mac OS X 10,6 veya üzeri sürümlerde çalışır.

    - **Internet Explorer 11.** Windows 7 veya sonraki sürümlerde (sınırlı destek) çalışıyor.

- Tarayıcı uzantısı ayarlarınızın açık olduğundan emin olun.

- Tarayıcınızı yeniden başlatmayı ve **uygulamalar** portalında yeniden oturum açmayı deneyin.

- Tarayıcınızın tanımlama bilgilerini temizlemeyi deneyin ve sonra yeniden başlatın ve **uygulamalar** portalında yeniden oturum açın.

## <a name="i-cant-sign-in-to-the-my-apps-portal"></a>**Uygulamalarım** portalında oturum açamıyorum

**Uygulamalarım** portalında oturum açarken sorun yaşıyorsanız aşağıdakileri deneyebilirsiniz:

- Doğru URL 'YI kullandığınızdan emin olun. https://myapps.microsoft.comKuruluşunuz için veya gibi özelleştirilmiş bir sayfa olmalıdır https://myapps.microsoft.com/contoso.com .

- Parolanızın doğru olduğundan ve süresinin dolmadığından emin olun. Daha fazla bilgi için bkz. [iş veya okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md).

- Doğrulama bilgilerinizin güncel ve doğru olduğundan emin olun. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication benim Için ne anlama geliyor?](./multi-factor-authentication-end-user-first-time.md) veya [güvenlik bilgileri yöntemlerinizi ile bilgilerinizi değiştirme](./security-info-setup-auth-app.md).

- **Uygulama** portalı URL 'Sini **Internet özellikleri > güvenlik > güvenilen siteler** ayarına ekleyin.

- Tarayıcınızın önbelleğini temizleyin ve yeniden oturum açmayı deneyin.

## <a name="my-password-isnt-working"></a>Parolam çalışmıyor

Parolanızı unuttuysanız, kuruluşunuzda hiç bir tane almadıysanız, hesabınız dışında bir hesap yoksa veya parolanızı değiştirmek istiyorsanız yardım 'a bakın. [Azure AD Parolamı unuttum](active-directory-passwords-update-your-own-password.md).

## <a name="i-want-to-be-able-to-reset-my-own-password"></a>Kendi parolamı sıfırlayabilmek istiyorum

Kendi parolanızı sıfırlayabilmeniz için, yöneticinizin öncelikle kuruluşunuzun özelliğini açmanız ve ardından gerekli doğrulama yöntemlerinizi güncelleştirmeniz ve doğrulamanız gerekir. Doğrulama yöntemlerinizi güncelleştirme hakkında daha fazla bilgi için bkz. [self servis parola sıfırlama Için kaydolma](active-directory-passwords-reset-register.md).

## <a name="im-getting-an-access-denied-message-when-i-start-an-app"></a>Bir uygulamayı başlattığımda erişim engellendi iletisi alıyorum

**Uygulama** portalından bir uygulama başlattıktan sonra **erişim reddedildi** iletisi alıyorsanız, aşağıdakileri deneyebilirsiniz:

- [Uygulamalarım güvenli oturum açma uzantısını](my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension) yüklediğinizden ve [desteklenen bir tarayıcı](my-apps-portal-end-user-access.md#supported-browsers)kullandığınızdan emin olun.

- Uygulamanın doğru URL 'sini kullandığınızdan ve URL 'nin **Internet özellikleri > güvenlik > güvenilen siteler** listesinde olduğundan emin olun.

- Parolanızın doğru olduğundan ve süresinin dolmadığından emin olun. Daha fazla bilgi için bkz. [iş veya okul parolanızı sıfırlama](active-directory-passwords-update-your-own-password.md).

- Doğrulama bilgilerinizin güncel ve doğru olduğundan emin olun. Daha fazla bilgi için bkz. [Azure Multi-Factor Authentication benim Için ne anlama geliyor?](./multi-factor-authentication-end-user-first-time.md) veya [güvenlik bilgileri yöntemlerinizi ile bilgilerinizi değiştirme](./security-info-setup-auth-app.md).

- Tarayıcınızın önbelleğini temizleyin ve yeniden oturum açmayı deneyin.

Uygulamanıza hala erişemiyorsanız, yardım almak için kuruluşunuzun yardım masasına başvurmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

**Uygulamalarım** portalında oturum açtıktan sonra profil ve hesap bilgilerinizi, Grup bilgilerinizi ve erişim gözden geçirme bilgilerini (izniniz varsa) güncelleştirebilirsiniz.

- [Uygulamalarım portalındaki uygulamalara erişin ve bunları kullanın](my-apps-portal-end-user-access.md).

- [Profil bilgilerinizi değiştirin](my-apps-portal-end-user-update-profile.md).

- [Gruplarla ilgili bilgilerinizi görüntüleyin ve güncelleştirin](my-apps-portal-end-user-groups.md).

- [Kendi erişim incelemelerinizi gerçekleştirin](my-apps-portal-end-user-access-reviews.md).