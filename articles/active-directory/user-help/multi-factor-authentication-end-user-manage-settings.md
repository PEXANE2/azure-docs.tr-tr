---
title: İki faktörlü doğrulama yönteminizi ve ayarlarınızı değiştirme - Azure Active Directory
description: Ek güvenlik doğrulama sayfasından, iş veya okul hesabınız için güvenlik doğrulama yöntemini ve ayarlarını nasıl değiştireceğinizi öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: curtand
ms.openlocfilehash: 5949f04ecc28a88e340a9c2de530031793f193a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253253"
---
# <a name="change-your-two-factor-verification-method-and-settings"></a>İki faktörlü doğrulama yönteminizi ve ayarlarınızı değiştirme

İş veya okul hesabınız için güvenlik doğrulama yöntemlerinizi ayarladıktan sonra, aşağıdakiler dahil olmak üzere ilgili ayrıntılardan herhangi birini güncelleyebilirsiniz:

- Varsayılan güvenlik doğrulama yöntemi

- Telefon numaranız gibi güvenlik doğrulama yöntemi ayrıntıları

- Kimlik doğrulayıcı uygulaması kurulumu veya aygıtı kimlik doğrulayıcı uygulamasından silme

## <a name="using-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasını kullanma

Kuruluşunuz size iki faktörlü doğrulamayı nasıl açacağınız ve yönetmeniz hakkında belirli adımlar sağladıysa, önce bu yönergeleri izlemeniz gerekir. Aksi takdirde, [Ek güvenlik doğrulama](https://docs.microsoft.com/azure/active-directory/user-help/multi-factor-authentication-end-user-first-time) sayfasından güvenlik doğrulama yöntemi ayarlarınıza ulaşabilirsiniz.

>[!Note]
>Ekranınızda gördükleriniz bu makalede kapsananlarla eşleşmiyorsa, yöneticiniz **güvenlik bilgilerini (önizleme)** deneyimini açıklamış veya kuruluşunuzun kendi özel portalınızı sağladığı anlamına gelir. Yeni güvenlik bilgileri deneyimi hakkında daha fazla bilgi için [Güvenlik bilgileri (önizleme) genel bakış](user-help-security-info-overview.md)bilgisine bakın. Kuruluşunuzun özel portalı hakkında daha fazla bilgi için kuruluşunuzun Yardım masasına başvurmanız gerekir.

### <a name="to-get-to-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasına ulaşmak için

Ek [güvenlik doğrulama sayfasına](https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1)bu bağlantıyı takip edebilirsiniz.

![Kullanılabilir güvenlik doğrulama yöntemi ayrıntılarıyla ek güvenlik doğrulama sayfası](./media/multi-factor-authentication-end-user-manage-settings/mfa-security-verification-page.png)

Ek **güvenlik doğrulama** sayfasına aşağıdaki adımları izleyerek de ulaşabilirsiniz:

1. Oturum [https://myapps.microsoft.com](https://myapps.microsoft.com)aç.

1. Sağ üstteki hesap adınızı seçin, ardından **profili**seçin.

1. **Ek güvenlik doğrulamasını**seçin.  

    ![Ek güvenlik doğrulama sayfasına Uygulamalarım bağlantısı](./media/multi-factor-authentication-end-user-manage-settings/mfa-myapps-link.png)

>[!Note]
>**Ek güvenlik doğrulama** sayfasının Uygulama **parolaları** bölümünü kullanma hakkında bilgi için [iki faktörlü doğrulama için uygulama parolalarını yönet bölümüne](multi-factor-authentication-end-user-app-passwords.md)bakın. Uygulama parolaları yalnızca iki faktörlü doğrulamayı desteklemeyen uygulamalar için kullanılmalıdır.

## <a name="change-your-default-security-verification-method"></a>Varsayılan güvenlik doğrulama yönteminizi değiştirme

İş veya okul hesabınızda kullanıcı adınız ve şifrenizle oturum açmanız sonrasında, otomatik olarak seçtiğiniz güvenlik doğrulama yöntemi sunulur. Kuruluşunuzun gereksinimlerine bağlı olarak, bu bir kimlik doğrulayıcı uygulaması, kısa mesaj veya bir telefon görüşmesi aracılığıyla bir bildirim veya doğrulama kodu olabilir.

Kullanmakta olduğunuz varsayılan güvenlik doğrulama yöntemini değiştirmek istediğinize karar verirseniz, bunu buradan yapabilirsiniz.

### <a name="to-change-your-default-security-verification-method"></a>Varsayılan güvenlik doğrulama yönteminizi değiştirmek için

1. Ek **güvenlik doğrulama** sayfasından, tercih ettiğiniz **seçenek** listesinden kullanılacak yöntemi seçin. Tüm seçenekleri görürsünüz, ancak yalnızca kuruluşunuz tarafından kullanılabilir hale getirilecek seçenekleri seçebilirsiniz.

    - **Uygulama aracılığıyla bana bildirin**: Kimlik doğrulayıcı uygulamanız aracılığıyla bekleme doğrulama isteminiz olduğu size bildirilir.

    - **Kimlik doğrulama telefonumu arayın:** Mobil cihazınızdan bilgilerinizi doğrulamanızı isteyen bir telefon alırsınız.

    - **Kimlik doğrulama telefonuma metin kodu**: Mobil cihazınızda kısa mesajın bir parçası olarak bir doğrulama kodu alırsınız. Bu kodu iş veya okul hesabınız için doğrulama istemine girmeniz gerekir.

    - **Ofis telefonumu arayın:** Ofis telefonunuzdan bilgilerinizi doğrulamanızı isteyen bir telefon alırsınız.

    - **Uygulamadan doğrulama kodunu kullanın**: İş veya okul hesabınızdan istem'e yazacağınız bir doğrulama kodu almak için kimlik doğrulayıcı uygulamanızı kullanırsınız.

2. **Kaydet'i**seçin.

## <a name="add-or-change-your-phone-number"></a>Telefon numaranızı ekleme veya değiştirme

**Ek güvenlik doğrulama** sayfasından yeni telefon numaraları ekleyebilir veya varolan numaraları güncelleyebilirsiniz.

>[!Important]
>Birincil telefonunuz kaybolması veya çalınması veya yeni bir telefon alıp artık orijinal, birincil telefon numaranız yoksa hesabınızın kilitlenmemesini önlemeye yardımcı olmak için ikinci bir telefon numarası eklemenizi şiddetle öneririz.

### <a name="to-change-your-phone-numbers"></a>Telefon numaralarınızı değiştirmek için

1. **Ek güvenlik doğrulama** sayfasının nasıl yanıt vermek **istiyorsunuz?** bölümünden, Kimlik **Doğrulama telefonunuz** (birincil mobil cihazınız) ve Office **telefonunuz**için telefon numarası bilgilerini güncelleyin.

1. Alternatif kimlik doğrulama **telefon** seçeneğinin yanındaki kutuyu seçin ve birincil cihazınıza erişemiyorsanız kısa mesaj veya telefon görüşmesi alabileceğiniz ikincil bir telefon numarası yazın.

1. **Kaydet'i**seçin.

## <a name="add-a-new-account-to-the-microsoft-authenticator-app"></a>Microsoft kimlik doğrulayıcı uygulamasına yeni bir hesap ekleme

[İşinizi](https://play.google.com/store/apps/details?id=com.azure.authenticator) veya okul hesabınızı Android veya [iOS](https://apps.apple.com/app/microsoft-authenticator/id983156458)için Microsoft Authenticator uygulamasında ayarlayabilirsiniz.

Microsoft Authenticator uygulamasında çalışmanızı veya okul hesabınızı zaten ayarladıysanız, bunu yeniden yapmanız gerekmez.

1. **Ek güvenlik doğrulama** sayfasının nasıl yanıt vermek **istiyorsunuz?** bölümünde, **Kimlik Doğrulaması Ayarla uygulamasını**seçin.

    ![Microsoft Authenticator uygulamasında çalışmanızı veya okul hesabınızı ayarlama](./media/multi-factor-authentication-end-user-manage-settings/mfa-security-verification-page-auth-app.png)

1. QR kodunu taramak için mobil cihazınızı kullanmak da dahil olmak üzere ekrandaki yönergeleri izleyin ve ardından **İleri'yi**seçin.

    Bilgilerinizi doğrulamak için Microsoft Authenticator uygulaması aracılığıyla bir bildirimi onaylamanız istenir.

1. **Kaydet'i**seçin.

## <a name="delete-your-account-or-device-from-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasından hesabınızı veya cihazınızı silme

Hesabınızı Microsoft Authenticator uygulamasından ve cihazınızı iş veya okul hesabınızdan silebilirsiniz. Genellikle kaybolan, çalınan veya eski bir aygıtı hesabınızdan kalıcı olarak kaldırmak için cihazınızı silersiniz ve bazı bağlantı sorunlarını gidermek veya yeni bir kullanıcı adı gibi bir hesap değişikliğini gidermek için hesabınızı silersiniz.

### <a name="to-delete-your-device-from-your-work-or-school-account"></a>Cihazınızı iş veya okul hesabınızdan silmek için

1. **Ek güvenlik doğrulama** sayfasının nasıl yanıt vermek **istiyorsunuz?** bölümünden **Kimlik Doğrulaması Uygulaması Ayarla** düğmesini seçin.

1. **Kaydet'i**seçin.

### <a name="to-delete-your-account-from-the-microsoft-authenticator-app"></a>Hesabınızı Microsoft Authenticator uygulamasından silmek için

Microsoft Authenticator uygulamasından, silmek istediğiniz aygıtın yanındaki **Sil** düğmesini seçin.

## <a name="turn-on-two-factor-verification-prompts-on-a-trusted-device"></a>Güvenilir bir aygıtta iki faktörlü doğrulama istemlerini açma

Kuruluş ayarlarınıza bağlı olarak, tarayıcınızda iki faktörlü doğrulama yaptığınızda **X günlerini tekrar sormayın** yazan bir onay kutusu görebilirsiniz. İki faktörlü doğrulama istemlerini durdurmak için bu seçeneği seçtiyseniz ve ardından cihazınızı kaybettiyseniz veya cihazınız tehlikeye girdiyse, hesabınızın korunmasına yardımcı olmak için iki faktörlü doğrulama istemlerini yeniden açmanız gerekir. Tüm aygıtlarınız için istemleri aynı anda açmanız gerekir. Ne yazık ki, yalnızca belirli bir aygıt için istemleri geri açamam.

### <a name="to-turn-two-factor-verification-prompts-back-on-for-your-devices"></a>Aygıtlarınız için iki faktörlü doğrulama istemlerini yeniden açmak için

Ek [ **güvenlik doğrulama** sayfasından,](#to-get-to-the-additional-security-verification-page) **önceden güvenilen aygıtlarda çok faktörlü kimlik doğrulamasını geri yükle'yi**seçin. Herhangi bir cihazda bir sonraki oturum açtığınızda, iki faktörlü doğrulama gerçekleştirmeniz istenir.

## <a name="next-steps"></a>Sonraki adımlar

İki faktörlü doğrulama ayarlarınızı ekledikten veya güncelledikten sonra, uygulama parolalarınızı yönetebilir, oturum açabilir veya iki faktörlü doğrulamayla ilgili bazı ortak sorunlarla ilgili yardım alabilirsiniz.

- [İki faktörlü doğrulamayı](multi-factor-authentication-end-user-app-passwords.md) desteklemeyen uygulamalar için iki faktörlü doğrulama için uygulama parolalarını yönetin.

- [İki faktörlü doğrulamayı kullanarak oturum açma](multi-factor-authentication-end-user-signin.md)

- [İki faktörlü doğrulama ile sık karşılaşılan sorunları çözme](multi-factor-authentication-end-user-troubleshoot.md)
