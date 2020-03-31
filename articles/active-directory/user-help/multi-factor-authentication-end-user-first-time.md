---
title: Ek doğrulama sayfası nedir? - Azure REKLAM
description: İki faktörlü doğrulama için Ek güvenlik doğrulama sayfasına nasıl gidilir?
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 01/15/2020
ms.author: curtand
ms.openlocfilehash: 5a7f0e10b23bf1a541fe83c3112962c38f7e1331
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77062566"
---
# <a name="what-is-the-additional-verification-page"></a>Ek doğrulama sayfası nedir?

BT ekibinizden veya patronunuzdan kuruluşunuzun hesabınıza ek bir güvenlik doğrulaması eklediğine dair bir e-posta aldınız. Bu ne anlama geliyor? Bu durum kuruluşunuzun oturum açma sırasında gerçekten söylediğiniz kişi olduğunuzdan emin olmak için ek adımları uygulamaya aldığını göstermektedir. İki faktörlü doğrulama olarak da bilinen bu ekstra doğrulama, kullanıcı adınız, parolanız ve bir mobil cihaz veya telefonun birleşimi aracılığıyla yapılır.

İki faktörlü doğrulama, iki kimlik doğrulama biçimine dayandığı için paroladan daha güvenlidir: bildiğiniz bir şey ve sizinle birlikte olduğunuz bir şey. Bildiğiniz şey parolanızdır. Sahip olduğunuz şey ise genelde yanınızda olan telefonunuz veya başka bir cihazdır. İki faktörlü doğrulama, kötü amaçlı bilgisayar korsanlarının sizin gibi davranmasını engellemeye yardımcı olabilir, çünkü parolanız olsa bile, bunların da cihazınıza sahip olmama olasılığı vardır.

>[!Important]
>Bu makale, bir çalışma veya okul hesabıyla (örneğin) alain@contoso.comiki faktörlü doğrulama kullanmaya çalışan kullanıcılar için hazırlanmıştır. Çalışanlarınız veya diğer kullanıcılar için iki faktörlü doğrulamayı nasıl açabileceğiniz hakkında bilgi arayan bir yöneticiyseniz, [Azure Etkin Dizin Kimlik Doğrulama belgelerine](https://docs.microsoft.com/azure/active-directory/authentication/)bakın.

## <a name="who-decides-if-you-use-this-feature"></a>Bu özelliği kullanıp kullanmadığınıza kim karar verir?

Hesap türünüze bağlı olarak, kuruluşunuz iki faktörlü doğrulama kullanmanız gerektiğine karar verebilir veya kendiniz karar verebilirsiniz.

- **İş veya okul hesabı.** Bir iş veya okul hesabı kullanıyorsanız (örneğin,), alain@contoso.combelirli doğrulama yöntemleriyle birlikte iki faktörlü doğrulama kullanıp kullanmanız kuruluşunuzun dır. Kuruluşunuz bu özelliği kullanmanız gerektiğine karar verdiği için, bu özelliği tek tek kapatmanız için bir yol yoktur.

- **Kişisel Microsoft hesabı.** Kişisel Microsoft hesaplarınız için iki faktörlü doğrulama ayarlamayı alain@outlook.comseçebilirsiniz (örneğin, ). İki faktörlü doğrulama ve kişisel Microsoft hesabınızla ilgili sorunlar yaşıyorsanız, [Microsoft hesabınız için iki faktörlü doğrulamayı açma veya kapatma'ya](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off)bakın. Bu özelliği kullanıp kullanmamayı seçtiğinizden, istediğiniz zaman açıp kapatabilirsiniz.

    >[!Note]
    >İki faktörlü doğrulama ve kişisel Microsoft hesaplarınızdan biriyle ilgili sorunlar danielle@outlook.comyaşıyorsanız (örneğin), [Microsoft hesabınızla iki aşamalı doğrulamayı nasıl kullanacağınız](https://support.microsoft.com/help/12408/microsoft-account-how-to-use-two-step-verification)la ilgili önerileri deneyebilirsiniz.

## <a name="access-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasına erişin

Kuruluşunuz açılıp iki faktörlü doyırım kurduktan sonra, hesabınızın güvenliğini korumasına yardımcı olmak için daha fazla bilgi sağlamışbir istem alırsınız.

![Daha fazla bilgi istemi gerekli](media/multi-factor-authentication-verification-methods/multi-factor-authentication-initial-prompt.png)

### <a name="to-access-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasına erişmek için

1. İstenilen **diğer bilgilerden** **İleri'yi** seçin.

    **Ek güvenlik doğrulama** sayfası görüntülenir.

2. Ek **güvenlik doğrulama** sayfasından, iş veya okul hesabınızda oturum açtıktan sonra kim olduğunuzu söylediğiniz kişi olduğunuzu doğrulamak için hangi iki faktörlü doğrulama yöntemini kullanacağınıza karar vermeniz gerekir. Şunları seçebilirsiniz:

    | İletişim yöntemi | Açıklama |
    | --- | --- |
    | Mobil uygulama | <ul><li>**Doğrulama için bildirimler alın.** Bu seçenek, akıllı telefonunuzdaki veya tabletinizdeki kimlik doğrulayıcı uygulamasına bir bildirim ilişir. Bildirimi görüntüleyin ve yasalsa uygulamada **Kimlik Doğrulaması'nı** seçin. İşiniz veya okulunuzun kimliği doğrulatmadan önce bir PIN girmeniz gerekebilir.</li><li>**Doğrulama kodunu kullanın.** Bu modda, kimlik doğrulayıcı uygulaması her 30 saniyede bir güncellenen bir doğrulama kodu oluşturur. Oturum açma ekranına en güncel doğrulama kodunu girin.<br>Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594) ve [iOS](https://go.microsoft.com/fwlink/?linkid=866594)için kullanılabilir.</li></ul> |
    | Kimlik doğrulama telefonu | <ul><li>**Telefon görüşmesi,** sağladığınız telefon numarasına otomatik bir sesli arama yerleştirir. Aramayı yanıtlayın ve kimlik doğrulaması için telefon tuş ucundaki pound tuşuna (#) basın.</li><li>**Metin iletisi** doğrulama kodu içeren bir metin iletisini sona erdirer. Metindeki istemi takiben, metin iletisini yanıtlayın veya oturum açma arabirimine sağlanan doğrulama kodunu girin.</li></ul> |
    | Ofis telefonu | Sağladığınız telefon numarasına otomatik sesli arama yerleştirir. Aramayı yanıtlayın ve kimlik doğrulaması için telefon tuş ucundaki pound tuşuna (#) basın. |

## <a name="next-steps"></a>Sonraki adımlar

**Ek güvenlik doğrulama** sayfasına eriştinkten sonra, iki faktörlü doğrulama yönteminizi seçmeli ve ayarlamanız gerekir:

- [Mobil cihazınızı doğrulama yönteminiz olarak ayarlama](multi-factor-authentication-setup-phone-number.md)

- [Ofis telefonunuzu doğrulama yönteminiz olarak ayarlama](multi-factor-authentication-setup-office-phone.md)

- [Microsoft Authenticator uygulamasını doğrulama yönteminiz olarak ayarlama](multi-factor-authentication-setup-auth-app.md)

## <a name="related-resources"></a>İlgili kaynaklar

- [İki faktörlü doğrulama yöntemi ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

- [Uygulama parolalarını yönetme](multi-factor-authentication-end-user-app-passwords.md)

- [İki faktörlü doğrulama kullanarak oturum açma](multi-factor-authentication-end-user-signin.md)

- [İki faktörlü doğrulama ile yardım alın](multi-factor-authentication-end-user-troubleshoot.md) 
