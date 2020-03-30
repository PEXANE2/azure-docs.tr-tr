---
title: İki faktörlü doğrulama yönteminiz olarak bir mobil cihaz ayarlama - Azure Active Directory | Microsoft Dokümanlar
description: İki faktörlü doğrulama yönteminiz olarak mobil cihazı nasıl ayarlayatıyarı nızı öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: overview
ms.date: 08/12/2019
ms.author: curtand
ms.openlocfilehash: b0bfaa87c77ba9fff9f6605c1989e48ffbc3fb35
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77062515"
---
# <a name="set-up-a-mobile-device-as-your-two-factor-verification-method"></a>İki faktörlü doğrulama yönteminiz olarak bir mobil cihaz ayarlama

Mobil cihazınızı iki faktörlü doğrulama yönteminiz olarak hareket edebilirsiniz. Cep telefonunuz doğrulama kodu içeren bir kısa mesaj veya telefon görüşmesi alabilir.

>[!Note]
> Kimlik doğrulama telefonu seçeneği soluksa, kuruluşunuzun doğrulama için bir telefon numarası veya kısa mesaj kullanmanıza izin vermemiş olabilir. Bu durumda, daha fazla yardım için başka bir yöntem seçmeniz veya yöneticinize başvurmanız gerekir.

## <a name="set-up-your-mobile-device-to-use-a-text-message-as-your-verification-method"></a>Doğrulama yönteminiz olarak kısa mesaj kullanacak şekilde mobil cihazınızı ayarlama

1. Ek **güvenlik doğrulama** sayfasında, Adım 1'den **Kimlik Doğrulama telefonunu** **seçin: Sizinle nasıl iletişim kurmalıyız,** açılan listeden ülkenizi veya bölgenizi seçin ve ardından cep telefonunuzun telefon numarasını yazın.

2. Yöntem alanından **kısa mesajla bana kod gönder'i** seçin ve ardından **İleri'yi**seçin. **Method**

    ![Kimlik doğrulama telefon ve metin mesajı ile ek güvenlik doğrulama sayfası,](media/multi-factor-authentication-verification-methods/multi-factor-authentication-text-message.png)

3. Doğrulama kodunu Microsoft'tan gönderilen kısa mesajdan **Adım 2'ye yazın: Telefon alanınıza bir kısa mesaj gönderdik** ve ardından **Doğrula'yı**seçin.

    ![Kimlik doğrulama telefon ve metin mesajı ile ek güvenlik doğrulama sayfası,](media/multi-factor-authentication-verification-methods/multi-factor-authentication-text-message-test.png)

4. Adım **3'ten: Mevcut uygulama alanınızı kullanmaya devam edin,** sağlanan uygulama parolasını kopyalayın ve güvenli bir yere yapıştırın.

    ![Ek güvenlik doğrulama sayfasının uygulama parolaları alanı](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Eski uygulamalarınızla uygulama parolasını nasıl kullanacağınız hakkında bilgi için uygulama [parolalarını yönet'e](multi-factor-authentication-end-user-app-passwords.md)bakın. Yalnızca iki faktörlü doğrulamayı desteklemeyen eski uygulamaları kullanmaya devam ediyorsanız uygulama parolalarını kullanmanız gerekir.

5. **Done** (Bitti) öğesini seçin.

## <a name="set-up-your-mobile-device-to-receive-a-phone-call"></a>Mobil cihazınızı telefon görüşmesi alacak şekilde ayarlama

1. Ek **güvenlik doğrulama** sayfasında, Adım 1'den **Kimlik Doğrulama telefonunu** **seçin: Sizinle nasıl iletişim kurmalıyız,** açılan listeden ülkenizi veya bölgenizi seçin ve ardından cep telefonunuzun telefon numarasını yazın.

2. **Yöntem** alanından **beni ara'yı** seçin ve sonra **İleri'yi**seçin.

    ![Kimlik doğrulama telefon ve telefon görüşmesi ile ek güvenlik doğrulama sayfası](media/multi-factor-authentication-verification-methods/multi-factor-authentication-phone-call.png)

3. Microsoft'tan, kimliğinizi doğrulamak için mobil cihazınızdaki pound (#) işaretine basmanızı isteyen bir telefon alırsınız.

    ![Belirtilen telefon numarasını test etme](media/multi-factor-authentication-verification-methods/multi-factor-authentication-phone-call-test.png)

4. Adım **3'ten: Mevcut uygulama alanınızı kullanmaya devam edin,** sağlanan uygulama parolasını kopyalayın ve güvenli bir yere yapıştırın.

    ![Ek güvenlik doğrulama sayfasının uygulama parolaları alanı](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Eski uygulamalarınızla uygulama parolasını nasıl kullanacağınız hakkında bilgi için uygulama [parolalarını yönet'e](multi-factor-authentication-end-user-app-passwords.md)bakın. Yalnızca iki faktörlü doğrulamayı desteklemeyen eski uygulamaları kullanmaya devam ediyorsanız uygulama parolalarını kullanmanız gerekir.

5. **Done** (Bitti) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

İki faktörlü doğrulama yönteminizi ayarladıktan sonra ek yöntemler ekleyebilir, ayarlarınızı ve uygulama parolalarınızı yönetebilir, oturum açabilir veya iki faktörlü doğrulamayla ilgili bazı sorunlarla ilgili yardım alabilirsiniz.

- [İki faktörlü doğrulama yöntemi ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

- [Uygulama parolalarını yönetme](multi-factor-authentication-end-user-app-passwords.md)

- [İki faktörlü doğrulama kullanarak oturum açma](multi-factor-authentication-end-user-signin.md)

- [İki faktörlü doğrulama ile yardım alın](multi-factor-authentication-end-user-troubleshoot.md)
