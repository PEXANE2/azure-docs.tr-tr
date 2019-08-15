---
title: İki aşamalı doğrulamayı ayarlama-Azure Active Directory | Microsoft Docs
description: Şirketiniz Azure Multi-Factor Authentication 'ı yapılandırdığında, iki adımlı doğrulama için kaydolmanız istenir. Ayarlamayı öğrenin.
services: active-directory
keywords: Azure Directory 'yi, bulutta Active Directory 'yi, Active Directory öğreticisini kullanma
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: 46f83a6a-dbdd-4375-8dc4-e7ea77c16357
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75bc067bfe8a98ef2337f368243b3221be1677d6
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949902"
---
# <a name="set-up-my-account-for-two-step-verification"></a>İki adımlı doğrulama için hesabımı ayarlama
İki aşamalı doğrulama, diğer kişilerin kesintiye uğramasından daha zor hale getirerek hesabınızı korumanıza yardımcı olan ek bir güvenlik adımıdır. Bu makaleyi okuyorsanız muhtemelen çok faktörlü kimlik doğrulaması hakkında iş veya okul yöneticinizden e-posta aldınız. Ya da oturum açmaya çalıştık ve ek güvenlik doğrulaması ayarlamanızı isteyen bir ileti aldınız. Bu durumda, **otomatik kayıt işlemini tamamlayana kadar oturum açmanız gerekmez**.

Bu makale, **iş veya okul hesabınızı**ayarlamanıza yardımcı olur. Kendi kişisel Microsoft hesabı için iki aşamalı doğrulamayı etkinleştirmek istiyorsanız, [iki adımlı doğrulama hakkında](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification)bölümüne bakın.

## <a name="set-up-your-account"></a>Hesabınızı ayarlayın

Şirketinizin destek alanı iki aşamalı doğrulamayı kullanmaya başlamanız gerektirdiğinde, **Yöneticinizin bu hesabı ek güvenlik doğrulaması için ayarlamanızı**gerektirdiğini belirten bir ekran görürsünüz:

![Kurulum](./media/multi-factor-authentication-end-user-first-time/first.png)

Başlamak için **Şimdi ayarla** ' yı seçin.

Oturum açtığınızda bunun gibi bir ekran görmüyorsanız, doğrulama seçeneklerinizi yönetebileceğiniz ayarlar sayfasını bulmak için [ayarları iki adımlı doğrulama Için yönetme](multi-factor-authentication-end-user-manage-settings.md#using-the-additional-security-verification-page) bölümündeki yönergeleri izleyin.

## <a name="decide-how-you-want-to-verify-your-sign-ins"></a>Oturum açma işlemlerini nasıl doğrulamak istediğinize karar verin

Kayıt işlemindeki ilk soru sizinle iletişim kurmamızı ister. Tablodaki seçeneklere göz atın ve her yöntemin kurulum adımlarına gitmek için bağlantıları kullanın.

| İletişim yöntemi | Açıklama |
| --- | --- |
| [Mobil uygulama](#use-a-mobile-app-as-the-contact-method) |- **Doğrulama için bildirim alın.** Bu seçenek, Smartphone veya tabletinizdeki kimlik doğrulayıcı uygulamasına bir bildirim gönderir. Bildirimi görüntüleyin ve mümkünse, uygulamada **kimlik doğrulaması** ' nı seçin. Çalışmanız veya okulunuz, kimlik doğrulamasından önce bir PIN girmenizi gerektirebilir.<br>- **Doğrulama kodunu kullanın.** Bu modda, Authenticator uygulaması 30 saniyede bir güncelleştirme yapan bir doğrulama kodu üretir. Oturum açma arabirimindeki en güncel doğrulama kodunu girin.<br>Microsoft Authenticator uygulaması [Android](https://go.microsoft.com/fwlink/?linkid=866594) ve [iOS](https://go.microsoft.com/fwlink/?linkid=866594)için kullanılabilir.|
| [Cep telefonu araması veya metni](#use-your-mobile-phone-as-the-contact-method) |- **Telefon araması** , sağladığınız telefon numarasına otomatik bir sesli çağrı koyar. Numarayı yanıtlayın ve telefon tuş takımında # tuşlarına basarak kimlik doğrulaması yapın.<br>- **Kısa mesaj** , doğrulama kodu içeren bir kısa mesaj sonlandırır. Metinde istemden sonra, metin iletisini yanıtlayın ya da oturum açma arabirimine sunulan doğrulama kodunu girin. |
| [Ofis telefonu çağrısı](#use-your-office-phone-as-the-contact-method) |Sağladığınız telefon numarasına otomatik bir sesli çağrı koyar. Numarayı yanıtlayın ve telefon tuş takımında # tuşlarına bastıktan sonra kimlik doğrulaması yapın. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>İletişim yöntemi olarak bir mobil uygulama kullanma
Bu yöntemin kullanılması, telefonunuza veya tabletinize bir Authenticator uygulaması yüklemenizi gerektirir. Bu makaledeki adımlar, [Android](https://go.microsoft.com/fwlink/?Linkid=825072) ve [iOS](https://go.microsoft.com/fwlink/?Linkid=825073)için kullanılabilen Microsoft Authenticator uygulamasını temel alır.

>[!NOTE]
>Microsoft Authenticator uygulamasını kullanmanız gerekmez. Zaten başka bir doğrulayıcı uygulaması kullanıyorsanız, kullanmaya devam edebilirsiniz.

1. Açılan listeden **mobil uygulama** ' yı seçin.
2. **Doğrulama için bildirimleri al** veya **doğrulama kodunu kullan**' ı seçin ve ardından **Ayarla**' yı seçin.

   ![Ek güvenlik doğrulama ekranı](./media/multi-factor-authentication-end-user-first-time/mobileapp.png)

3. Telefonunuzda veya tabletinizde uygulamayı açın ve hesap eklemeyi seçin **+** . (Android cihazlarda üç noktayı ve ardından **Hesap Ekle**' yi seçin.)
4. İş veya okul hesabı eklemek istediğinizi belirtin. Telefonunuzdaki QR kodu tarayıcısı açılır. Kameranız düzgün çalışmıyorsa, şirket bilgilerinizi el ile girmeyi seçebilirsiniz. Daha fazla bilgi için bkz. [bir hesabı el Ile ekleme](#add-an-account-manually).  
5. Mobil uygulamayı yapılandırma ekranıyla birlikte görünen QR kod resmini tarayın.  QR kod ekranını kapatmak için **bitti** ' yi seçin.  

   ![QR kod ekranı](./media/multi-factor-authentication-end-user-first-time/scan2.png)

6. Telefonda etkinleştirme tamamlandığında **benimle Iletişim kurun**' i seçin.  Bu adım, telefonunuza bir bildirim ya da doğrulama kodu gönderir. **Doğrula**' yı seçin.  
7. Şirketiniz, oturum açma doğrulamasını onaylamak için bir PIN gerektiriyorsa, girin.

   ![PIN girme kutusu](./media/multi-factor-authentication-end-user-first-time/scan3.png)

8. PIN girişi tamamlandıktan sonra **Kapat**' ı seçin. Bu noktada doğrulamanın başarılı olması gerekir.
9. Mobil uygulamanıza erişiminizi kaybetmeniz durumunda cep telefonu numaranızı girmenizi öneririz. Ülke/bölge ' yi aşağı açılan listeden belirtin ve ülke/bölge adının yanındaki kutuya cep telefonu numaranızı girin. **İleri**’yi seçin.
10. Bu noktada, Outlook 2010 veya üzeri gibi tarayıcı olmayan uygulamalar veya Apple cihazlarda yerel e-posta uygulaması için uygulama parolaları ayarlamanız istenir. Bunun nedeni, bazı uygulamaların iki aşamalı doğrulamayı desteklememasıdır. Bu uygulamaları kullanmıyorsanız **bitti** ' ye tıklayın ve adımların geri kalanını atlayın.
11. Bu uygulamaları kullanıyorsanız, belirtilen uygulama parolasını kopyalayın ve normal parolanız yerine uygulamanıza yapıştırın. Birden çok uygulama için aynı uygulama parolasını kullanabilirsiniz. Daha fazla bilgi için [uygulama parolalarıyla ilgili yardım].
12. **Bitti**’ye tıklayın.

### <a name="add-an-account-manually"></a>Hesabı el ile ekleme
Mobil uygulamaya bir hesabı QR Okuyucusu yerine el ile eklemek istiyorsanız, aşağıdaki adımları izleyin.

1. **Hesabı el Ile gir** düğmesini seçin.  
2. Kodu ve barkodu gösteren aynı sayfada sunulan URL 'YI girin. Bu bilgi, mobil uygulamadaki **kod** ve **URL** kutularına gider.

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/barcode2.png)
3. Etkinleştirme tamamlandığında **benimle Iletişim kurun**' i seçin. Bu adım, telefonunuza bir bildirim ya da doğrulama kodu gönderir. **Doğrula**' yı seçin.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Cep telefonunuzu iletişim yöntemi olarak kullanın
1. Açılan listeden **kimlik doğrulama telefonunu** seçin.  

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/phone.png)  
2. Aşağı açılan listeden bulunduğunuz ülkeyi/bölgenizi seçin ve cep telefonu numaranızı girin.
3. Cep telefonunuz veya çağrlarınız ile kullanmayı tercih ettiğiniz yöntemi seçin.
4. Telefon numaranızı doğrulamak için **benimle Iletişim kurun** ' ı seçin. Seçtiğiniz moda bağlı olarak size bir metin göndereceğiz veya size çağrı yaptık. Ekranda belirtilen yönergeleri izleyin ve **Doğrula**' yı seçin.
5. Bu noktada, Outlook 2010 veya üzeri gibi tarayıcı olmayan uygulamalar veya Apple cihazlarda yerel e-posta uygulaması için uygulama parolaları ayarlamanız istenir. Bunun nedeni, bazı uygulamaların iki aşamalı doğrulamayı desteklememasıdır. Bu uygulamaları kullanmıyorsanız **bitti** ' ye tıklayın ve adımların geri kalanını atlayın.
6. Bu uygulamaları kullanıyorsanız, belirtilen uygulama parolasını kopyalayın ve normal parolanız yerine uygulamanıza yapıştırın. Birden çok uygulama için aynı uygulama parolasını kullanabilirsiniz. Daha fazla bilgi için [uygulama parolalarıyla ilgili yardım].
7. **Bitti**’ye tıklayın.

## <a name="use-your-office-phone-as-the-contact-method"></a>Office telefonunuzu iletişim yöntemi olarak kullanma
1. Açılan listeden **Office telefonu** ' nu seçin  

    ![Kurulum](./media/multi-factor-authentication-end-user-first-time/office.png)  
2. Telefon numarası kutusu, şirketinizin iletişim bilgileriyle otomatik olarak doldurulur. Sayı yanlış veya eksikse, yöneticinizden değişiklik yapmasını isteyin.
3. Telefon numaranızı doğrulamak için **benimle Iletişim kurun** ' i seçin ve numaranızı arayacağız. Ekranda belirtilen yönergeleri izleyin ve **Doğrula**' yı seçin.
4. Bu noktada, Outlook 2010 veya üzeri gibi tarayıcı olmayan uygulamalar veya Apple cihazlarda yerel e-posta uygulaması için uygulama parolaları ayarlamanız istenir. Bunun nedeni, bazı uygulamaların iki aşamalı doğrulamayı desteklememasıdır. Bu uygulamaları kullanmıyorsanız **bitti** ' ye tıklayın ve adımların geri kalanını atlayın.
5. Bu uygulamaları kullanıyorsanız, belirtilen uygulama parolasını kopyalayın ve normal parolanız yerine uygulamanıza yapıştırın. Birden çok uygulama için aynı uygulama parolasını kullanabilirsiniz. Daha fazla bilgi için bkz. [Uygulama parolaları nelerdir](multi-factor-authentication-end-user-app-passwords.md).
6. **Bitti**’ye tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
* Tercih ettiğiniz seçenekleri değiştirin ve [iki adımlı doğrulama için ayarlarınızı yönetin](multi-factor-authentication-end-user-manage-settings.md)
* İki aşamalı doğrulamayı desteklemeyen yerel cihaz uygulamaları için [Uygulama parolaları](multi-factor-authentication-end-user-app-passwords.md) ayarlayın.
* Hücre hizmetiniz olmadığında bile hızlı ve güvenli kimlik doğrulama için [Microsoft Authenticator uygulamasına](user-help-auth-app-download-install.md) göz atın.
