---
title: Uygulama parolaları nasıl yönetilir - Azure Active Directory | Microsoft Dokümanlar
description: Uygulama parolaları ve iki aşamalı doğrulama yla ilgili olarak ne için kullanıldıkları hakkında bilgi edinin.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: richagi
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: curtand
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 5f81181ac3107307a352cdbcd0b5cc4a555deacb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253227"
---
# <a name="manage-app-passwords-for-two-step-verification"></a>İki aşamalı doğrulama için uygulama parolalarını yönetme

>[!Important]
>Yöneticiniz uygulama parolalarını kullanmanıza izin vermeyebilir. **Uygulama parolalarını** bir seçenek olarak görmüyorsanız, bunlar kuruluşunuzda kullanılamaz.

Uygulama parolalarını kullanırken şunları unutmamak önemlidir:

- Uygulama parolaları otomatik olarak oluşturulur ve uygulama başına bir kez oluşturulup girilmelidir.

- Kullanıcı başına 40 parola sınırı vardır. Bu sınırdan sonra bir tane oluşturmaya çalışırsanız, yenisini oluşturmasına izin verilmeden önce varolan bir parolayı silmeniz istenir.

    >[!Note]
    >Office 2013 istemcileri (Outlook dahil) yeni kimlik doğrulama protokollerini destekler ve iki adımlı doğrulamayla kullanılabilir. Bu destek, iki aşamalı doğrulama açık olduktan sonra Office 2013 istemcileri için artık uygulama parolalarına ihtiyacınız olmayacağı anlamına gelir. Daha fazla bilgi için [Office 2013 ve Office 2016 istemci uygulamaları makalesi için modern kimlik doğrulamanın nasıl çalıştığına](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) bakın.

## <a name="create-new-app-passwords"></a>Yeni uygulama parolaları oluşturma

İlk iki faktörlü doğrulama kayıt işleminiz sırasında size tek bir uygulama şifresi verilir. Birden fazla sınaihtiyacınız varsa, bunları kendiniz oluşturmanız gerekir. Kuruluşunuzda iki faktörlü doğrulamanın nasıl ayarlıştığına bağlı olarak birden çok alandan uygulama parolaları oluşturabilirsiniz. İşletmenizde veya okul hesabınızla iki faktörlü doğrulama kullanmak için kayıt hakkında daha fazla bilgi için, [iki faktörlü doğrulama ya](multi-factor-authentication-end-user-first-time.md) da iş veya okul hesabınız ve ilgili makaleler için Genel Bakış'a bakın.

### <a name="where-to-create-and-delete-your-app-passwords"></a>Uygulama parolalarınızı nerede oluşturup silebilirsiniz?

İki faktörlü doğrulamayı nasıl kullandığınıza bağlı olarak uygulama parolaları oluşturabilir ve silebilirsiniz:

- **Kuruluşunuz iki faktörlü doğrulama ve Ek güvenlik doğrulama sayfasını kullanır.** İş veya okul hesabınızı (örneğin) alain@contoso.comkuruluşunuzdaki iki faktörlü doğrulamayla kullanıyorsanız, uygulama parolalarınızı [Ek güvenlik doğrulama sayfasından](https://account.activedirectory.windowsazure.com/Proofup.aspx)yönetebilirsiniz. Ayrıntılı talimatlar için, bu [makaledeki Ek güvenlik doğrulama sayfasını kullanarak uygulama parolalarını oluştur ve sil](#create-and-delete-app-passwords-from-the-additional-security-verification-page) sayfasına bakın.

- **Kuruluşunuz iki faktörlü doğrulama ve Office 365 portalı kullanır.** İş veya okul hesabınızı (örneğin, alain@contoso.comkuruluşunuzdaki Office 365 uygulamaları) kullanıyorsanız, uygulama parolalarınızı Office [365 portal sayfasından](https://www.office.com)yönetebilirsiniz. Ayrıntılı talimatlar için, bu [makaledeki Office 365 portalını kullanarak uygulama parolaları oluştur ve sil](#create-and-delete-app-passwords-using-the-office-365-portal) mesuliyye bakın.

- **Kişisel bir Microsoft hesabıyla iki faktörlü doğrulama kullanıyorsunuz.** İki faktörlü doğrulamayla kişisel bir Microsoft alain@outlook.comhesabı (örneğin) kullanıyorsanız, uygulama parolalarınızı [Güvenlik temelleri sayfasından](https://account.microsoft.com/security/)yönetebilirsiniz. Ayrıntılı talimatlar için bkz: [Uygulama parolalarını iki aşamalı doğrulamayı desteklemeyen uygulamalarla kullanma.](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-and-two-step-verification)

## <a name="create-and-delete-app-passwords-from-the-additional-security-verification-page"></a>Ek güvenlik doğrulama sayfasından uygulama parolaları oluşturma ve silme

İş veya okul hesabınız için **Ek güvenlik doğrulama** sayfasından uygulama parolaları oluşturabilir ve silebilirsiniz.

1. Ek güvenlik [doğrulama sayfasında](https://account.activedirectory.windowsazure.com/Proofup.aspx)oturum açın ve ardından **Uygulama parolalarını**seçin.

    ![Uygulama parolaları sayfası, Uygulama şifreleri sekmesi vurgulanmış](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page.png)

2. **Oluştur'u**seçin , uygulama parolasını gerektiren uygulamanın adını yazın ve sonra **İleri'yi**seçin.

    ![Parola ya da parola gerektiren uygulama adı ile uygulama parolaları sayfası oluşturma](media/multi-factor-authentication-end-user-app-passwords/mfa-create-app-password-page.png)

3. **Uygulamanızın parola** sayfasından parolayı kopyalayın ve ardından **Kapat'ı**seçin.

    ![Belirtilen uygulamanızın parolasını içeren uygulama şifre sayfanız](media/multi-factor-authentication-end-user-app-passwords/mfa-your-app-password-page.png)

4. Uygulama **parolaları** sayfasından uygulamanızın listelenmiş olduğundan emin olun.

     ![Listede gösterilen yeni uygulama ile uygulama şifreleri sayfası](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-with-new-password.png)  

5. Uygulama parolasını oluşturduğunuz uygulamayı açın (örneğin, Outlook 2010) ve istendiğinde uygulama parolasını yapıştırın. Bunu uygulama başına yalnızca bir kez yapmanız gerekir.

### <a name="to-delete-an-app-password-using-the-app-passwords-page"></a>Uygulama parolaları sayfasını kullanarak uygulama parolasını silmek için

1. Uygulama **parolaları** sayfasından, silmek istediğiniz uygulama parolasının yanındaki **Sil'i** seçin.

   ![Uygulama parolası silme](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-delete.png)

2. Parolayı silmek istediğinizi onaylamak için **Evet'i** seçin ve ardından **Kapat'ı**seçin.

    Uygulama parolası başarıyla silinir.

## <a name="create-and-delete-app-passwords-using-the-office-365-portal"></a>Office 365 portalını kullanarak uygulama parolaları oluşturma ve silme

İş veya okul hesabınız ve Office 365 uygulamalarınızla iki aşamalı doğrulama kullanıyorsanız, Office 365 portalını kullanarak uygulama parolalarınızı oluşturabilir ve silebilirsiniz.

### <a name="to-create-app-passwords-using-the-office-365-portal"></a>Office 365 portalını kullanarak uygulama parolaları oluşturmak için

1. Office 365'te oturum açın ve ardından [Hesabım sayfasına](https://portal.office.com)gidin, **Güvenlik & gizliliği'ni**seçin ve ek **güvenlik doğrulamasını genişletin.**

    ![Genişletilmiş ek güvenlik doğrulama alanını gösteren office portalı](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-o365-my-account-page.png)

2. **Uygulama parolaları** sayfasını açmak için uygulama **parolaları oluştur ve yönet** yazan metni seçin.

    ![Uygulama parolaları sayfası, Uygulama şifreleri sekmesi vurgulanmış](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page.png)

3. **Oluştur'u**seçin , uygulama parolasını gerektiren uygulamanın adını yazın ve sonra **İleri'yi**seçin.

    ![Parola ya da parola gerektiren uygulama adı ile uygulama parolaları sayfası oluşturma](media/multi-factor-authentication-end-user-app-passwords/mfa-create-app-password-page.png)

4. **Uygulamanızın parola** sayfasından parolayı kopyalayın ve ardından **Kapat'ı**seçin.

    ![Belirtilen uygulamanızın parolasını içeren uygulama şifre sayfanız](media/multi-factor-authentication-end-user-app-passwords/mfa-your-app-password-page.png)

5. Uygulama **parolaları** sayfasından uygulamanızın listelenmiş olduğundan emin olun.

     ![Listede gösterilen yeni uygulama ile uygulama şifreleri sayfası](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-with-new-password.png)  

6. Uygulama parolasını oluşturduğunuz uygulamayı açın (örneğin, Outlook 2010) ve istendiğinde uygulama parolasını yapıştırın. Bunu uygulama başına yalnızca bir kez yapmanız gerekir.

### <a name="to-delete-app-passwords-using-the-app-passwords-page"></a>Uygulama parolalarını kullanarak uygulama parolalarını silmek için

1. Uygulama **parolaları** sayfasından, silmek istediğiniz uygulama parolasının yanındaki **Sil'i** seçin.

   ![Uygulama parolası silme](media/multi-factor-authentication-end-user-app-passwords/mfa-app-passwords-page-delete.png)

2. Onay kutusunda **Evet'i** seçin ve ardından **Kapat'ı**seçin.

    Uygulama parolası başarıyla silinir.

## <a name="if-your-app-passwords-arent-working-properly"></a>Uygulama parolalarınız düzgün çalışmıyorsa

Parolanızı doğru yazdığınızdan emin olun. Parolanızı doğru girdiğinizden eminseniz, yeniden oturum açmayı ve yeni bir uygulama parolası oluşturmayı deneyebilirsiniz. Bu seçeneklerden hiçbiri sorununuzu çözmezse, mevcut uygulama parolalarınızı silmek ve yepyeni parolalar oluşturmanıza izin vermek için kuruluşunuzun Yardım masasına başvurun.

## <a name="next-steps"></a>Sonraki adımlar

- [İki aşamalı doğrulama ayarlarınızı yönetme](multi-factor-authentication-end-user-manage-settings.md)

- Oturum açma işlemlerinizi metin veya çağrı almak yerine uygulama bildirimleriyle doğrulamak için [Microsoft Authenticator uygulamasını](user-help-auth-app-download-install.md) deneyin.
