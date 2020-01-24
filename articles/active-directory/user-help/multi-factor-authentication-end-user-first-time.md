---
title: Ek doğrulama sayfası nedir? -Azure AD
description: İki öğeli doğrulama için ek güvenlik doğrulama sayfasına nasıl ulaşırsanız.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/15/2020
ms.author: lizross
ms.openlocfilehash: d7ec884cb55d876edd93f9fe1c5cc59f5ee13295
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705301"
---
# <a name="what-is-the-additional-verification-page"></a>Ek doğrulama sayfası nedir?

BT ekibinizden veya patronunuzdan kuruluşunuzun hesabınıza ek bir güvenlik doğrulaması eklediğine dair bir e-posta aldınız. Bu ne anlama geliyor? Bu durum kuruluşunuzun oturum açma sırasında gerçekten söylediğiniz kişi olduğunuzdan emin olmak için ek adımları uygulamaya aldığını göstermektedir. İki öğeli doğrulama olarak da bilinen bu ek doğrulama, Kullanıcı adınızın, parolanızın ve bir mobil cihazın veya telefonunuzun bir birleşimi aracılığıyla yapılır.

İki öğeli doğrulama iki farklı kimlik doğrulama biçimini temel aldığından yalnızca bir paroladan daha güvenlidir: bildiğiniz bir şey ve sizinle bir şeyler. Bildiğiniz şey parolanızdır. Sahip olduğunuz şey ise genelde yanınızda olan telefonunuz veya başka bir cihazdır. İki öğeli doğrulama kötü amaçlı korsanların sizin için hazır olmasını durdurmaya yardımcı olabilir, çünkü parolankenizin olsa da, gürültü, cihazınıza sahip olmalardır.

>[!Important]
>Bu makale, bir iş veya okul hesabıyla iki öğeli doğrulama kullanmaya çalışan kullanıcılara yöneliktir (örneğin, alain@contoso.com). Çalışanlarınız veya diğer kullanıcılar için iki öğeli doğrulamanın nasıl kullanılacağına ilişkin bilgi arayan bir yöneticiyseniz, [Azure Active Directory kimlik doğrulama belgelerine](https://docs.microsoft.com/azure/active-directory/authentication/)bakın.

## <a name="who-decides-if-you-use-this-feature"></a>Bu özelliği kullanıp kullanmayacağına karar veriyor musunuz?

Kuruluşunuz, hesap türüne bağlı olarak iki öğeli doğrulama kullanmanız gerektiğine karar verebilir veya kendiniz karar veremeyebilirsiniz.

- **İş veya Okul hesabı.** Bir iş veya okul hesabı kullanıyorsanız (örneğin, alain@contoso.com), bu, belirli doğrulama yöntemleriyle birlikte iki öğeli doğrulama kullanmanız gerekip gerekmediğini kuruluşunuza göre yapılır. Kuruluşunuz bu özelliği kullanmanız gerektiğine karar verdiği için, tek yapmanız gereken bir yöntem yoktur.

- **Kişisel Microsoft hesabı.** Kişisel Microsoft hesaplarınız için iki öğeli doğrulama ayarlamayı seçebilirsiniz (örneğin, alain@outlook.com). İki öğeli doğrulamayla ve kişisel Microsoft hesabı ilgili sorun yaşıyorsanız, [Microsoft hesabı için iki öğeli doğrulamayı açma veya kapatma](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off)bölümüne bakın. Bu özelliği kullanmak isteyip istemediğinizi seçerken dilediğiniz zaman açıp kapatabilirsiniz.

    >[!Note]
    >İki öğeli doğrulamayla ve kişisel Microsoft hesaplarınızdan biriyle ilgili sorun yaşıyorsanız (örneğin, danielle@outlook.com), [Microsoft hesabı iki aşamalı doğrulamayı kullanma](https://support.microsoft.com/help/12408/microsoft-account-how-to-use-two-step-verification)önerilerini deneyebilirsiniz.

## <a name="access-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasına erişin

Kuruluşunuz, iki öğeli doğrulamayı etkinleştirdikten ve ayarladıktan sonra, hesabınızı güvende tutmaya yardımcı olmak için daha fazla bilgi sağlamanızı söyleyen bir istem alırsınız.

![Daha fazla bilgi gerekli istemi](media/multi-factor-authentication-verification-methods/multi-factor-authentication-initial-prompt.png)

### <a name="to-access-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasına erişmek için

1. **Daha fazla bilgi gerekli** isteminde **İleri ' yi** seçin.

    **Ek güvenlik doğrulama** sayfası görüntülenir.

2. **Ek güvenlik doğrulaması** sayfasında, iş veya okul hesabınızda oturum açtıktan sonra nerede olduğunuzu doğrulamak için hangi iki öğeli doğrulama yönteminin kullanılacağına karar vermelisiniz. Şunları seçebilirsiniz:

    | İletişim yöntemi | Açıklama |
    | --- | --- |
    | Mobil uygulama | <ul><li>**Doğrulama için bildirim alın.** Bu seçenek, Smartphone veya tabletinizdeki kimlik doğrulayıcı uygulamasına bir bildirim gönderir. Bildirimi görüntüleyin ve mümkünse, uygulamada **kimlik doğrulaması** ' nı seçin. Çalışmanız veya okulunuz, kimlik doğrulamasından önce bir PIN girmenizi gerektirebilir.</li><li>**Doğrulama kodunu kullanın.** Bu modda, Authenticator uygulaması 30 saniyede bir güncelleştirme yapan bir doğrulama kodu üretir. Oturum açma ekranında en güncel doğrulama kodunu girin.<br>Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594) ve [iOS](https://go.microsoft.com/fwlink/?linkid=866594)için kullanılabilir.</li></ul> |
    | Kimlik doğrulama telefonu | <ul><li>**Telefon araması** , sağladığınız telefon numarasına otomatik bir sesli çağrı koyar. Çağrıyı yanıtlayın ve kimlik doğrulaması için telefon tuş takımındaki kare tuşuna (#) basın.</li><li>**Kısa mesaj** , doğrulama kodu içeren bir kısa mesaj sonlandırır. Metinde istemden sonra, metin iletisini yanıtlayın ya da oturum açma arabirimine sunulan doğrulama kodunu girin.</li></ul> |
    | Ofis telefonu | Sağladığınız telefon numarasına otomatik bir sesli çağrı koyar. Çağrıyı yanıtlayın ve kimlik doğrulaması için telefon tuş takımındaki kare tuşuna (#) basın. |

## <a name="next-steps"></a>Sonraki adımlar

**Ek güvenlik doğrulama** sayfasına eriştiyseniz, iki öğeli doğrulama yönteminizi seçip ayarlamanız gerekir:

- [Mobil cihazınızı doğrulama yönteminiz olarak ayarlayın](multi-factor-authentication-setup-phone-number.md)

- [Office telefonunuzu doğrulama yönteminiz olarak ayarlayın](multi-factor-authentication-setup-office-phone.md)

- [Doğrulama yönteminiz olarak Microsoft Authenticator uygulamasını ayarlama](multi-factor-authentication-setup-auth-app.md)

## <a name="related-resources"></a>İlgili kaynaklar

- [İki öğeli doğrulama yöntemi ayarlarınızı yönetin](multi-factor-authentication-end-user-manage-settings.md)

- [Uygulama parolalarını yönetme](multi-factor-authentication-end-user-app-passwords.md)

- [İki öğeli doğrulama kullanarak oturum açın](multi-factor-authentication-end-user-signin.md)

- [İki öğeli doğrulamayla ilgili yardım alın](multi-factor-authentication-end-user-troubleshoot.md) 
