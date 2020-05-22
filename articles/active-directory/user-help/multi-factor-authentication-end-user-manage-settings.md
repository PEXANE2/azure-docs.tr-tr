---
title: İki öğeli doğrulama yönteminizi ve ayarlarını değiştirin-Azure Active Directory
description: Ek güvenlik doğrulama sayfasından iş veya okul hesabınız için güvenlik doğrulama yöntemi ve ayarlarını değiştirmeyi öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 02/20/2020
ms.author: curtand
ms.openlocfilehash: c9304f4a958360d01d404d9b510feb5659e5436d
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83746741"
---
# <a name="change-your-two-factor-verification-method-and-settings"></a>İki öğeli doğrulama yönteminizi ve ayarlarını değiştirin

İş veya okul hesabınız için güvenlik doğrulama yöntemlerinizi ayarladıktan sonra, aşağıdakiler dahil olmak üzere ilgili ayrıntıların birini güncelleştirebilirsiniz:

- Varsayılan güvenlik doğrulama yöntemi

- Telefon numaranız gibi güvenlik doğrulama yöntemi ayrıntıları

- Doğrulayıcı uygulama kurulumu veya bir cihazı Authenticator uygulamasından silme

## <a name="using-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasını kullanma

Kuruluşunuz, iki öğeli doğrulamanızı açma ve yönetme hakkında belirli adımlar sağladıysa, önce bu yönergeleri izlemelisiniz. Aksi takdirde, güvenlik doğrulama yöntemi ayarlarınıza [ek güvenlik doğrulama](https://docs.microsoft.com/azure/active-directory/user-help/multi-factor-authentication-end-user-first-time) sayfasından ulaşabilirsiniz.

>[!Note]
>Ekranınızda gördüğünüz özellikler, bu makalede ele alınanlara uymuyor, yöneticinizin **güvenlik bilgileri (Önizleme)** deneyimini kapatmış olması veya kuruluşunuzun kendi özel portalınızı sağlamasıdır. Yeni güvenlik bilgileri deneyimi hakkında daha fazla bilgi için bkz. [güvenlik bilgisi (Önizleme) genel bakış](user-help-security-info-overview.md). Kuruluşunuzun özel portalı hakkında daha fazla bilgi için kuruluşunuzun yardım masasına başvurmanız gerekir.

### <a name="to-get-to-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasına ulaşmak için

Bu bağlantıyı, [ek güvenlik doğrulama sayfasına](https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1)izleyebilirsiniz.

![Ek güvenlik doğrulama sayfası, kullanılabilir güvenlik doğrulama yöntemi ayrıntıları](./media/multi-factor-authentication-end-user-manage-settings/mfa-security-verification-page.png)

Ayrıca, aşağıdaki adımları izleyerek **ek güvenlik doğrulama** sayfasına da ulaşabilirsiniz:

1. [https://myapps.microsoft.com](https://myapps.microsoft.com) adresinde oturum açın.

1. Sağ üst köşedeki hesap adınızı seçip **profil**' i seçin.

1. **Ek güvenlik doğrulaması**' nı seçin.  

    ![Uygulamalarım ek güvenlik doğrulama sayfasına bağlanır](./media/multi-factor-authentication-end-user-manage-settings/mfa-myapps-link.png)

>[!Note]
>**Ek güvenlik doğrulama** sayfasının **Uygulama parolaları** bölümünü kullanma hakkında bilgi için bkz. [iki öğeli doğrulama için uygulama parolalarını yönetme](multi-factor-authentication-end-user-app-passwords.md). Uygulama parolaları yalnızca iki öğeli doğrulamayı desteklemeyen uygulamalar için kullanılmalıdır.

## <a name="change-your-default-security-verification-method"></a>Varsayılan güvenlik doğrulama yönteminizi değiştirme

Kullanıcı adınız ve parolanızla iş veya okul hesabınızda oturum açtıktan sonra otomatik olarak seçtiğiniz güvenlik doğrulama yöntemi sunulur. Kuruluşunuzun gereksinimlerine bağlı olarak, bu bir doğrulayıcı uygulaması, bir SMS mesajı veya telefon araması aracılığıyla bir bildirim veya doğrulama kodu olabilir.

Kullanmakta olduğunuz varsayılan güvenlik doğrulama yöntemini değiştirmek istediğinize karar verirseniz, buradan yapabilirsiniz.

### <a name="to-change-your-default-security-verification-method"></a>Varsayılan güvenlik doğrulama yönteminizi değiştirmek için

1. **Ek güvenlik doğrulaması** sayfasında, **tercih ettiğiniz seçenek** listesinden kullanılacak yöntemi seçin. Tüm seçenekleri görürsünüz ancak kuruluşunuz tarafından yalnızca sizin tarafınızdan kullanılabilir hale getirilenler arasından seçim yapabilirsiniz.

    - **Uygulama aracılığıyla bana bildir**: kimlik doğrulayıcı uygulamanız aracılığıyla, bekleyen bir doğrulama istemi olduğunu bilgilendirirsiniz.

    - **Kimlik doğrulama telefonumu çağır**: mobil cihazınızda bilgilerinizi doğrulamanızı isteyen bir telefon araması alırsınız.

    - **Kimlik doğrulama telefonum Için metin kodu**: bir doğrulama kodunu, mobil cihazınızda bir SMS mesajının parçası olarak alırsınız. Bu kodu iş veya okul hesabınızın doğrulama istemine girmeniz gerekir.

    - **Ofis telefonumu arayın**: Office telefonunuzda, bilgilerinizi doğrulamanızı isteyen bir telefon araması alırsınız.

    - **Uygulamadan doğrulama kodunu kullan**: kimlik doğrulayıcı uygulamanızı, iş veya okul hesabınızdan gelen isteme yazdığınız bir doğrulama kodu almak için kullanacaksınız.

2. **Kaydet**’i seçin.

## <a name="add-or-change-your-phone-number"></a>Telefon numaranızı ekleme veya değiştirme

**Ek güvenlik doğrulama** sayfasından yeni telefon numaraları ekleyebilir veya mevcut numaraları güncelleştirebilirsiniz.

>[!Important]
>Birincil telefonunuz kaybolur veya çalınırsa ya da yeni bir telefon alırsanız ve artık özgün, birincil telefon numaranız yoksa, hesabınız dışında bırakılmasını önlemeye yardımcı olmak için bir ikincil telefon numarası eklemenizi önemle öneririz.

### <a name="to-change-your-phone-numbers"></a>Telefon numaralarınızı değiştirmek için

1. **Ek güvenlik doğrulama** sayfasının **nasıl yanıt vermesini istersiniz?** bölümünde, **kimlik doğrulama telefonunuzun** telefon numarası bilgilerini (birincil mobil cihazınız) ve **ofis telefonunuzu**güncelleştirin.

1. **Alternatif kimlik doğrulama telefonu** seçeneğinin yanındaki kutuyu işaretleyin ve ardından birincil cihazınıza erişemiyorsanız metin iletilerini veya telefon görüşmelerini alacağınız bir ikincil telefon numarası yazın.

1. **Kaydet**’i seçin.

## <a name="add-a-new-account-to-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasına yeni bir hesap ekleme

İş veya okul hesabınızı [Android](https://play.google.com/store/apps/details?id=com.azure.authenticator) veya [iOS](https://apps.apple.com/app/microsoft-authenticator/id983156458)için Microsoft Authenticator uygulamasında ayarlayabilirsiniz.

Microsoft Authenticator uygulamasında iş veya okul hesabınızı zaten ayarladıysanız, bunu tekrar yapmanız gerekmez.

1. **Ek güvenlik doğrulama** sayfasının **nasıl yanıt vermesini istiyorsunuz?** bölümünde, **kimlik doğrulayıcı uygulaması ayarla**' yı seçin.

    ![Microsoft Authenticator uygulamasında iş veya okul hesabınızı ayarlama](./media/multi-factor-authentication-end-user-manage-settings/mfa-security-verification-page-auth-app.png)

1. QR kodunu taramak için mobil cihazınızı kullanma da dahil olmak üzere ekrandaki yönergeleri izleyin ve sonra **İleri**' yi seçin.

    Bilgilerinizi doğrulamak için Microsoft Authenticator uygulama aracılığıyla bir bildirimi onaylamanız istenecektir.

1. **Kaydet**’i seçin.

## <a name="delete-your-account-or-device-from-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasından hesabınızı veya cihazınızı silme

Microsoft Authenticator uygulamasından hesabınızı silebilir ve cihazınızı iş veya okul hesabınızdan silebilirsiniz. Genellikle cihazınızı, kayıp, çalınmış veya eski bir cihazı hesabınızdan kalıcı olarak kaldıracak şekilde siler ve bazı bağlantı sorunlarını gidermeyi denemek veya yeni bir Kullanıcı adı gibi bir hesap değişikliğini gidermek için hesabınızı silersiniz.

### <a name="to-delete-your-device-from-your-work-or-school-account"></a>Cihazınızı iş veya okul hesabınızdan silmek için

1. **Ek güvenlik doğrulama** sayfasının **nasıl yanıt vermesini istiyorsunuz?** bölümünde, **kimlik doğrulayıcı uygulaması ayarla** düğmesini seçin.

1. **Kaydet**’i seçin.

### <a name="to-delete-your-account-from-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasından hesabınızı silmek için

Microsoft Authenticator uygulamadan, silmek istediğiniz cihazın yanındaki **Sil** düğmesini seçin.

## <a name="turn-on-two-factor-verification-prompts-on-a-trusted-device"></a>Güvenilen bir cihazda iki öğeli doğrulama istemlerini açma

Kuruluşunuzun ayarlarına bağlı olarak, tarayıcınızda iki öğeli doğrulama gerçekleştirirken **X gün boyunca tekrar sorma** seçeneğini belirten bir onay kutusu görebilirsiniz. İki öğeli doğrulama istemini durdurmak için bu seçeneği belirlediyseniz ve cihazınızı kaybederseniz veya cihazınız potansiyel olarak tehlikeye girerse, hesabınızı korumaya yardımcı olması için iki öğeli doğrulama istemini yeniden açmanız gerekir. Tüm cihazlarınızın tüm cihazlarınızı aynı anda açmanız gerekir. Ne yazık ki, yalnızca belirli bir cihaz için istemleri yeniden açamazsınız.

### <a name="to-turn-two-factor-verification-prompts-back-on-for-your-devices"></a>Cihazlarınız için iki öğeli doğrulama istemini yeniden etkinleştirmek için

[ **Ek güvenlik doğrulaması** sayfasında](#to-get-to-the-additional-security-verification-page), **önceden güvenilen cihazlarda Multi-Factor Authentication 'ı geri yükle**' yi seçin. Herhangi bir cihazda bir sonraki oturum açışınızda iki öğeli doğrulama gerçekleştirmeniz istenir.

## <a name="next-steps"></a>Sonraki adımlar

İki öğeli doğrulama ayarlarınızı ekledikten veya güncelleştirdikten sonra, uygulama parolalarınızı yönetebilir, oturum açabilir veya bazı yaygın iki faktörlü doğrulamayla ilgili bazı sorunlarda yardım alabilirsiniz.

- İki öğeli doğrulamayı desteklemeyen uygulamalar için [iki öğeli doğrulama için uygulama parolalarını yönetin](multi-factor-authentication-end-user-app-passwords.md) .

- [İki öğeli doğrulama kullanarak oturum açma](multi-factor-authentication-end-user-signin.md)

- [İki öğeli doğrulamayla ilgili yaygın sorunları çözün](multi-factor-authentication-end-user-troubleshoot.md)
