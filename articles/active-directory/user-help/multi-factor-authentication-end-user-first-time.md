---
title: Ek doğrulama sayfası nedir? -Azure AD
description: İki öğeli doğrulama için ek güvenlik doğrulama sayfasına nasıl ulaşırsanız
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 05/28/2020
ms.author: curtand
ms.openlocfilehash: fc95e988b3f89402967cdbedd06c4b945a99f99a
ms.sourcegitcommit: 309cf6876d906425a0d6f72deceb9ecd231d387c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84266262"
---
# <a name="what-is-the-additional-verification-page"></a>Ek doğrulama sayfası nedir?

Kuruluşunuz, oturum açtığınızda size söylediğinizden emin olmak için ek adımlar gerçekleştiriyorsunuz. Bu ek güvenlik doğrulaması, iki öğeli doğrulama olarak da bilinir. Kullanıcı adı, parola ve mobil cihaz ya da telefonun birleşiminden oluşur. Yapmak istediğiniz tek bir Microsoft hesabı için iki öğeli doğrulamayı devre dışı bırakmak istiyorsanız alain@outlook.com , [Microsoft hesabı için iki öğeli doğrulamayı açma veya kapatma](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off)bölümündeki yönergeleri kullanın.

<center>

![Kavramsal kimlik doğrulama yöntemleri görüntüsü](../authentication/media/concept-mfa-howitworks/methods.png)</center>

İki öğeli doğrulama iki farklı kimlik doğrulama biçimini temel aldığından yalnızca bir paroladan daha güvenlidir:

- Parolanız gibi bildiğiniz bir şey.
- Sahip olduğunuz bir telefon veya başka bir cihaz gibi.

İki öğeli doğrulama kötü amaçlı korsanların sizin için hazır olmasını durdurmaya yardımcı olabilir. Parolanız olsa bile, ODD 'ler cihazınıza de sahip olmalardır.

>[!Important]
>Çalışanlarınız veya diğer kullanıcılar için iki öğeli doğrulamanın nasıl kullanılacağına ilişkin bilgi arayan bir yöneticiyseniz, [Azure Active Directory kimlik doğrulama belgelerine](https://docs.microsoft.com/azure/active-directory/authentication/)bakın. Bu makale, bir iş veya okul hesabıyla iki öğeli doğrulama kullanmaya çalışan kullanıcılara yöneliktir (örneğin, alain@contoso.com ).

## <a name="who-decides-if-you-use-this-feature"></a>Bu özelliği kullanıp kullanmayacağına karar veriyor musunuz?

İki öğeli doğrulama kullanıp kullandığınıza karar veren kim, sahip olduğunuz hesap türüne bağlıdır:

- **İş veya okul hesabı.** Bir iş veya okul hesabı kullanıyorsanız (gibi alain@contoso.com ), bu, iki öğeli doğrulama kullanıp, belirli doğrulama yöntemleriyle birlikte kuruluşunuza göre yapılır. Kuruluşunuz bu özelliği kullanmanız gerektiğine karar verdiği için, tek yapmanız gereken bir yöntem yoktur.

- **Kişisel Microsoft hesabı.** Kişisel Microsoft hesaplarınız için iki öğeli doğrulama ayarlamayı seçebilirsiniz (örneğin, alain@outlook.com ). Microsoft hesabı dilediğiniz zaman etkinleştirebilir veya devre dışı bırakabilirsiniz. bunun [için iki öğeli doğrulamayı açma veya kapatma](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off)bölümündeki basit yönergeleri kullanın.

    >[!Note]
    >İki öğeli doğrulamayla ve kişisel Microsoft hesaplarınızdan biri ile ilgili başka sorunlarla karşılaşıyorsanız, [Microsoft hesabı iki aşamalı doğrulamanın nasıl kullanılacağına ilişkin](https://support.microsoft.com/help/12408/microsoft-account-how-to-use-two-step-verification)daha fazla öneri vardır.

## <a name="open-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasını açın

Kuruluşunuz iki öğeli doğrulamayı etkinleştirdikten sonra, her oturum açışınızda hesabınızı güvende tutmaya yardımcı olmak için daha fazla bilgi sağlamanızı söyleyen bir istem alırsınız.

![Daha fazla bilgi gerekli istemi](media/multi-factor-authentication-verification-methods/multi-factor-authentication-initial-prompt.png)

### <a name="to-access-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasına erişmek için

1. **Daha fazla bilgi gerekli** isteminde **İleri ' yi** seçin.

    **Ek güvenlik doğrulama** sayfası görüntülenir.

2. **Ek güvenlik doğrulaması** sayfasında, iş veya okul hesabınızda oturum açarken söylediğinizi doğrulamak için kullanılacak iki öğeli doğrulama yöntemini seçin. Şunları seçebilirsiniz:

    | İletişim yöntemi | Açıklama |
    | --- | --- |
    | Mobil uygulama | <ul><li>**Doğrulama için bildirim alın.** Bu seçenek, Smartphone veya tabletinizdeki kimlik doğrulayıcı uygulamasına bir bildirim gönderir. Bildirimi görüntüleyin ve mümkünse, uygulamada **kimlik doğrulaması** ' nı seçin. Çalışmanız veya okulunuz, kimlik doğrulamasından önce bir PIN girmenizi gerektirebilir.</li><li>**Doğrulama kodunu kullanın.** Bu modda uygulama, her 30 saniyede bir doğrulama kodu oluşturur. Oturum açma ekranında en güncel doğrulama kodunu girin.<br>Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594) ve [iOS](https://go.microsoft.com/fwlink/?linkid=866594)için kullanılabilir.</li></ul> |
    | Kimlik doğrulama telefonu | <ul><li>**Telefon araması** , sağladığınız telefon numarasına otomatik bir sesli çağrı koyar. Çağrıyı yanıtlayın ve kimlik doğrulaması için telefon tuş takımındaki kare tuşuna (#) basın.</li><li>**Kısa mesaj** , doğrulama kodu içeren bir kısa mesaj sonlandırır. Metinde istemden sonra, metin iletisini yanıtlayın ya da oturum açma arabirimine sunulan doğrulama kodunu girin.</li></ul> |
    | Ofis telefonu | Sağladığınız telefon numarasına otomatik bir sesli çağrı koyar. Çağrıyı yanıtlayın ve kimlik doğrulaması için telefon tuş takımındaki kare tuşuna (#) basın. |

## <a name="next-steps"></a>Sonraki adımlar

**Ek güvenlik doğrulama** sayfasında iki öğeli bir doğrulama yöntemi seçtikten sonra, onu ayarlamanız gerekir:

- [Mobil cihazınızı doğrulama yönteminiz olarak ayarlayın](multi-factor-authentication-setup-phone-number.md)

- [Office telefonunuzu doğrulama yönteminiz olarak ayarlayın](multi-factor-authentication-setup-office-phone.md)

- [Doğrulama yönteminiz olarak Microsoft Authenticator uygulamasını ayarlama](multi-factor-authentication-setup-auth-app.md)

## <a name="related-resources"></a>İlgili kaynaklar

- [İki öğeli doğrulama kullanarak oturum açın](multi-factor-authentication-end-user-signin.md)

- [İki öğeli doğrulamayla ilgili yardım alın](multi-factor-authentication-end-user-troubleshoot.md)
