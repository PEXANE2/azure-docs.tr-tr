---
title: Uygulamalarım portalı - Azure Active Directory| Microsoft Dokümanlar
description: Uygulamalarım portalında ortak görevlere kaydolma ve gerçekleştirme konusunda yardım alın.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: f72dd5595d67ae989cec5681d22def9a2f929adf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253162"
---
# <a name="troubleshoot-problems-with-the-my-apps-portal"></a>Uygulamalarım portalıyla ilgili sorun giderme

**Uygulamalar** Portalım'da oturum açma veya kullanma yla ilgili sorunlar yaşıyorsanız, yardım masasına veya yöneticinize başvurmadan önce yardım masasına veya yöneticinize başvurmadan önce bu sorun giderme ipuçlarını deneyin.

## <a name="im-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Uygulamalarım Güvenli Oturum Açma Uzantısı'nı yüklerken sorun yaşıyorum

Uygulamalarım Güvenli Oturum Açma Uzantısı'nı yüklerken sorun yaşıyorsanız:

- Şular da dahil olmak üzere desteklenen bir tarayıcı kullandığınızdan emin olun:

    - **Microsoft Edge.** Windows 10 Anniversary Edition veya sonraki sürümde çalışma.

    - **Google Chrome.** Windows 7 veya sonraki saatlerde ve Mac OS X veya daha sonra çalışma.

    - **Mozilla Firefox 26.0 veya sonrası.** Windows XP SP2 veya sonraki saatlerde ve Mac OS X 10.6 veya sonraki saatlerde çalışır.

    - **Internet Explorer 11.** Windows 7 veya sonraki saatlerde çalıştırma (sınırlı destek).

- Tarayıcı uzantısı ayarlarınızın açık olduğundan emin olun.

- Tarayıcınızı yeniden başlatmayı ve **Uygulamalarım** portalında yeniden oturum açmayı deneyin.

- Tarayıcınızın çerezlerini temizlemeyi deneyin ve ardından **Uygulamalarım** portalında yeniden başlatın ve oturum açın.

## <a name="i-cant-sign-in-to-the-my-apps-portal"></a>**Uygulamalarım** portalında oturum açamıyorum

**Uygulamalarım** portalına oturum açarken sorun yaşıyorsanız, aşağıdakileri deneyebilirsiniz:

- Doğru URL'yi kullandığınızdan emin olun. Kuruluşunuz https://myapps.microsoft.com için özelleştirilmiş bir sayfa olmalıdır. https://myapps.microsoft.com/contoso.com

- Parolanızın doğru olduğundan ve süresinin dolmadığından emin olun. Daha fazla bilgi için [bkz: İş veya okul parolanızı sıfırla.](active-directory-passwords-update-your-own-password.md)

- Doğrulama bilgilerinizin güncel ve doğru olduğundan emin olun. Daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama'nın benim için ne anlama geldiğini](multi-factor-authentication-end-user.md) görmek veya güvenlik bilgileri [yöntemlerinizi ve bilgilerinizi değiştirme](security-info-add-update-methods-overview.md).

- **Güvenlik > Güvenilen siteler** ayarı > Internet Properties'e **Uygulamam** portalı URL'sini ekleyin.

- Tarayıcınızın önbelleğini temizleyin ve yeniden oturum açmayı deneyin.

## <a name="my-password-isnt-working"></a>Şifrem çalışmıyor

Parolanızı unuttuysanız, kuruluşunuzdan hiç almadıysanız, hesabınız kilitlendiyse veya parolanızı değiştirmek istiyorsanız, [Bkz. Yardım, Azure AD şifremi unuttum.](active-directory-passwords-update-your-own-password.md)

## <a name="i-want-to-be-able-to-reset-my-own-password"></a>Kendi şifremi sıfırlamak istiyorum

Kendi parolanızı sıfırlayabilmek için yöneticinizin önce kuruluşunuz için özelliği açmanız ve ardından gerekli doğrulama yöntemlerinizi güncelleştirmeniz ve doğrulamanız gerekir. Doğrulama yöntemlerinizi nasıl güncelleştirin izlenmeye ilişkin daha fazla bilgi için self [servis parola sıfırlama için kaydolun'](active-directory-passwords-reset-register.md)a bakın.

## <a name="im-getting-an-access-denied-message-when-i-start-an-app"></a>Bir uygulamayı başlattığımda Erişim Reddedildi iletisi alıyorum

**Uygulamam** portalından bir uygulamayı başlattıktan sonra **Erişim Reddedildi** iletisi alıyorsanız, aşağıdakileri deneyebilirsiniz:

- [Uygulamalarım Güvenli Oturum Açma Uzantısı'nı](my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension) yüklediğinizden ve desteklenen bir [tarayıcı](my-apps-portal-end-user-access.md#supported-browsers)kullandığınızdan emin olun.

- Uygulama için doğru URL'yi kullandığınızdan ve URL'nin **Internet Özellikleri > Güvenlik > Güvenilen siteler** listenizde olduğundan emin olun.

- Parolanızın doğru olduğundan ve süresinin dolmadığından emin olun. Daha fazla bilgi için [bkz: İş veya okul parolanızı sıfırla.](active-directory-passwords-update-your-own-password.md)

- Doğrulama bilgilerinizin güncel ve doğru olduğundan emin olun. Daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama'nın benim için ne anlama geldiğini](multi-factor-authentication-end-user.md) görmek veya güvenlik bilgileri [yöntemlerinizi ve bilgilerinizi değiştirme](security-info-add-update-methods-overview.md).

- Tarayıcınızın önbelleğini temizleyin ve yeniden oturum açmayı deneyin.

Bunları denedikten sonra hala uygulamanıza erişemiyorsanız, yardım için kuruluşunuzun Yardım masasına başvurmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

**Uygulamalarım** portalında oturum açtıktan sonra, profil ve hesap bilgilerinizi, grup bilgilerinizi ve inceleme bilgilerine (izniniz varsa) erişebilirsiniz.

- [Uygulamalarım portalındaki uygulamalara erişin ve bunları kullanın.](my-apps-portal-end-user-access.md)

- [Profil bilgilerinizi değiştirin.](my-apps-portal-end-user-update-profile.md)

- [Gruplarla ilgili bilgilerinizi görüntüleyin ve güncelleyin.](my-apps-portal-end-user-groups.md)

- [Kendi erişim değerlendirmelerinizi gerçekleştirin.](my-apps-portal-end-user-access-reviews.md)
