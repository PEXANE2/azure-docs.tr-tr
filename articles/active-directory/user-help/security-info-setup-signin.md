---
title: Oturum açma isteminizden güvenlik bilgilerini (Önizleme) ayarlama-Azure Active Directory | Microsoft Docs
description: Kuruluşunuzun oturum açma sayfasından istenirse iş veya okul hesabınız için güvenlik bilgilerini ayarlama.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: overview
ms.date: 02/13/2019
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: c20256fb712d9381ba2adc90e2e68ce4fdc8911a
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68382784"
---
# <a name="set-up-your-security-info-preview-from-the-sign-in-page-prompt"></a>Oturum açma sayfası isteminden güvenlik bilgilerinizi (Önizleme) ayarlama

İş veya okul hesabınızda oturum açtıktan hemen sonra güvenlik bilgilerinizi ayarlamanız istenirse, bu adımları izleyebilirsiniz.

Bu istemi yalnızca kuruluşunuz için gereken güvenlik bilgilerini ayarlamadıysanız görürsünüz. Daha önce güvenlik bilgilerinizi ayarladıysanız ancak değişiklik yapmak istiyorsanız, çeşitli Yöntem tabanlı nasıl yapılır makalelerindeki adımları izleyebilirsiniz. Daha fazla bilgi için bkz. [güvenlik bilgilerine genel bakış ekleme veya güncelleştirme](security-info-add-update-methods-overview.md).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

## <a name="sign-in-to-your-work-or-school-account"></a>İş veya okul hesabınızda oturum açın

İş veya okul hesabınızda oturum açtıktan sonra hesabınıza erişmenize izin vermeden önce daha fazla bilgi sağlamanızı isteyen bir istem görürsünüz.

![Daha fazla bilgi istesin](media/security-info/securityinfo-prompt.png)

## <a name="set-up-your-security-info-using-the-wizard"></a>Sihirbazı kullanarak güvenlik bilgilerinizi ayarlama

İş veya okul hesabınızla ilgili güvenlik bilgilerinizi istemden ayarlamak için bu adımları izleyin.

>[!Important]
>Bu işlem yalnızca bir örnektir. Kuruluşunuzun gereksinimlerine bağlı olarak, yöneticiniz bu işlem sırasında ayarlamanız gereken farklı doğrulama yöntemleri ayarlamış olabilir. Bu örnekte, Microsoft Authenticator uygulaması ve doğrulama çağrıları veya SMS iletileri için bir cep telefonu numarası olmak üzere iki yöntem gerekli.

1. İstemde **İleri** ' yi seçtikten sonra, yönetici ve kuruluşunuzun ayarlamanız gereken ilk yöntemi gösteren **Hesabınızı güvenli tut Sihirbazı** görüntülenir. Bu örnek için Microsoft Authenticator uygulamasıdır.

   > [!Note]
   > Microsoft Authenticator uygulaması dışında bir Authenticator uygulaması kullanmak istiyorsanız, **farklı bir Authenticator uygulama bağlantısı kullanmak istiyorum** bağlantısını seçin.
   >
   > Kuruluşunuz, doğrulayıcı uygulamasının yanı sıra farklı bir yöntem seçmenizi sağlar, **farklı bir yöntem bağlantısı kurmak istiyorum**' u seçebilirsiniz.

    ![Kimlik doğrulama uygulaması indirme sayfasını gösteren hesap güvenli Sihirbazı 'nı koruyun](media/security-info/securityinfo-prompt-get-auth-app.png)

2. Mobil cihazınıza Microsoft Authenticator uygulamayı indirip yüklemek için **Şimdi İndir** ' i seçin ve ardından **İleri**' yi seçin. Uygulamanın nasıl indirileceği ve yükleneceği hakkında daha fazla bilgi için, bkz. [Microsoft Authenticator uygulamasını indirme ve yükleme](user-help-auth-app-download-install.md).

    ![Hesap sayfanızı ayarlama kimlik doğrulamasını gösteren hesap güvenli Sihirbazı 'nı koruyun](media/security-info/securityinfo-prompt-auth-app-setup-acct.png)

3. Mobil cihazınızda Microsoft Authenticator uygulamayı ayarlarken **hesabınızı ayarlama** sayfasında kalır.

4. Microsoft Authenticator uygulamasını açın, bildirimlere izin ver ' i seçin (istenirse), sağ üst köşedeki **Özelleştir ve denetim** simgesinden **Hesap Ekle** ' yi seçin ve sonra **iş veya okul hesabı**' nı seçin.

5. Bilgisayarınızda **hesabınızı ayarlama** sayfasına dönün ve sonra **İleri**' yi seçin.

    **QR kodunu Tara** sayfası görüntülenir.

    ![Doğrulayıcı uygulamasını kullanarak QR kodunu tarama](media/security-info/securityinfo-prompt-auth-app-qrcode.png)

6. 5\. adımda iş veya okul hesabınızı oluşturduktan sonra mobil cihazınızda görünen Microsoft Authenticator App QR kod okuyucusu ile belirtilen kodu tarayın.

    Kimlik doğrulayıcı uygulaması, size herhangi bir ek bilgi gerekmeden iş veya okul hesabınızı başarıyla eklememelidir. Ancak, QR kod okuyucusu kodu okuyaamazsa **QR kodu bağlantısını tarayamaz** ve kodu ve URL 'yi Microsoft Authenticator uygulamasına el ile girebilirsiniz. Kodu el ile ekleme hakkında daha fazla bilgi için bkz. [uygulamaya el ile hesap ekleme](user-help-auth-app-add-account-manual.md).

7. Bilgisayarınızdaki **QR kodunu Tara** sayfasında **İleri ' yi** seçin.

    Hesabınızı test etmek için mobil cihazınızdaki Microsoft Authenticator uygulamasına bir bildirim gönderilir.

    ![Kimlik doğrulayıcı uygulamasıyla hesabınızı test etme](media/security-info/securityinfo-prompt-test-app.png)

8. Microsoft Authenticator uygulamasındaki bildirimi onaylayın ve ardından **İleri**' yi seçin.

    ![Başarı bildirimi, uygulamayı ve hesabınızı bağlama](media/security-info/securityinfo-prompt-auth-app-success.png).

    Güvenlik bilgileriniz, iki adımlı doğrulama veya parola sıfırlama kullanırken kimliğinizi doğrulamak için varsayılan olarak Microsoft Authenticator uygulamasını kullanacak şekilde güncelleştirilir.

9. **Telefon** kurulum sayfasında, kısa mesaj veya telefon araması almak isteyip istemediğinizi seçin ve ardından **İleri**' yi seçin. Bu örneğin, metin iletilerini kullanacağız, bu nedenle metin iletilerini kabul edebilecek bir cihaz için telefon numarası kullanmanız gerekir.

    ![SMS mesajlaşması için telefon numaranızı ayarlamaya başlayın](media/security-info/securityinfo-prompt-text-msg.png)

    Telefon numaranızı bir SMS mesajı gönderilir. Telefon araması yapmayı tercih ediyorsanız, işlem aynıdır. Bununla birlikte, kısa mesaj yerine yönergeler içeren bir telefon araması alırsınız.

10. Mobil cihazınıza gönderilen SMS iletisinin verdiği kodu girin ve ardından **İleri**' yi seçin.

    ![Kısa mesajla hesabınızı test edin](media/security-info/securityinfo-prompt-text-msg-enter-code.png)

11. Başarı bildirimini gözden geçirin ve **bitti**' yi seçin.

    ![Başarı bildirimi](media/security-info/securityinfo-prompt-call-answered-success.png)

    Güvenlik bilgileriniz, iki adımlı doğrulama veya parola sıfırlama kullanılırken kimliğinizi doğrulamak için bir yedekleme yöntemi olarak metin iletilerini kullanacak şekilde güncelleştirilir.

12. Güvenlik bilgileriniz için hem Microsoft Authenticator uygulamasını hem de telefon (kısa mesaj veya telefon araması) yöntemini başarıyla ayarladığınızı doğrulamak için **başarı** sayfasını gözden geçirin ve **bitti**' yi seçin.

    ![Sihirbaz başarıyla tamamlandı sayfası](media/security-info/securityinfo-prompt-setup-success.png)

## <a name="next-steps"></a>Sonraki adımlar

- Varsayılan güvenlik bilgisi yöntemlerini değiştirmek, silmek veya güncelleştirmek için bkz.:

    - [Bir doğrulayıcı uygulaması için güvenlik bilgilerini ayarlayın](security-info-setup-auth-app.md).

    - [SMS mesajlaşma için güvenlik bilgilerini ayarlayın](security-info-setup-text-msg.md).

    - [Telefon aramalarını kullanmak için güvenlik bilgilerini ayarlayın](security-info-setup-phone-number.md).

    - [E-posta kullanmak için güvenlik bilgilerini ayarlayın](security-info-setup-email.md).

    - [Önceden tanımlanmış güvenlik sorularını kullanmak için güvenlik bilgilerini ayarlayın](security-info-setup-questions.md).

- Belirtilen yöntemi kullanarak oturum açma hakkında daha fazla bilgi için bkz. [oturum açma](user-help-sign-in.md).

- Parola [sıfırlama portalından](https://passwordreset.microsoftonline.com/) kayıp veya unuttuysanız parolanızı sıfırlayın veya [iş veya okul parolanızı sıfırlama](user-help-reset-password.md) makalesindeki adımları izleyin.

- [Microsoft hesabı makalesinde oturum açma](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) sorunları için sorun giderme ipuçları ve yardım alın.
