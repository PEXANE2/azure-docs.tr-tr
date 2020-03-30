---
title: İş veya okul hesabıyla kimlik doğrulamayı kullanarak oturum açma - Azure AD
description: Çeşitli iki faktörlü doğrulama yöntemlerini kullanarak iş veya okul hesabınızda nasıl oturum açarak oturum açarak oturum açınızın olduğunu öğrenin.
services: active-directory
author: curtand
manager: daveba
ms.assetid: b310b762-471b-4b26-887a-a321c9e81d46
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 04/02/2017
ms.author: curtand
ms.reviewer: librown
ms.custom: end-user, seo-update-azuread-jan
ms.openlocfilehash: 33cf9e284d2206ea497af7a5da7c3cf4a890cc87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064096"
---
# <a name="sign-in-to-your-work-or-school-account-using-your-two-factor-verification-method"></a>İki faktörlü doğrulama yönteminizi kullanarak iş veya okul hesabınızda oturum açın

> [!NOTE]
> Bu makalenin amacı, tipik bir oturum açma deneyimi nden geçmektir. Oturum açma veya sorunları gidermek için yardım için [bkz.](multi-factor-authentication-end-user-troubleshoot.md)

## <a name="what-will-your-sign-in-experience-be"></a>Oturum açma deneyiminiz ne olacak?
Oturum açma deneyiminiz, ikinci faktörünüz olarak ne kullanmayı seçtiğinize bağlı olarak farklılık gösterir: telefon görüşmesi, kimlik doğrulama uygulaması veya kısa mesajlar. Ne yaptığınızı en iyi açıklayan seçeneği seçin:

| Nasıl oturum açıyorsun? |
| --- |
| [Cep telefonumu veya ofis telefonumu telefonla arayarak](#signing-in-with-a-phone-call) |
| [Cep telefonuma bir mesajla](#signing-in-with-a-text-message)
| [Microsoft Authenticator uygulamasından gelen bildirimlerle](#to-sign-in-with-a-notification-from-the-microsoft-authenticator-app) |
| Microsoft Authenticator uygulamasından doğrulama kodları ile |
| [Alternatif bir yöntemle, çünkü şu anda tercih ettiğim yöntemi kullanamıyorum](#signing-in-with-an-alternate-method) |

## <a name="signing-in-with-a-phone-call"></a>Bir telefon görüşmesiyle oturum açma
Aşağıdaki bilgiler, cep telefonunuza veya ofis telefonunuza yapılan bir aramayla iki aşamalı doğrulama deneyimini açıklar.

1. Kullanıcı adınızı ve parolanızı kullanarak Office 365 gibi bir uygulamada veya hizmette oturum açın.  
2. Microsoft sizi arar.  
3. Telefonu aç ve # tuşuna bas.  

## <a name="signing-in-with-a-text-message"></a>Kısa mesajla oturum açma
Aşağıdaki bilgiler, cep telefonunuza gönderilen kısa mesajla iki aşamalı doğrulama deneyimini açıklar:

1. Kullanıcı adınızı ve parolanızı kullanarak Office 365 gibi bir uygulamada veya hizmette oturum açın.
2. Microsoft size bir numara kodu içeren bir kısa mesaj gönderir.
3. Oturum açma sayfasında sağlanan kutuya kodu girin.

## <a name="signing-in-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasıyla oturum açma
Aşağıdaki bilgiler, iki aşamalı doğrulamalar için Microsoft Authenticator uygulamasını kullanma deneyimini açıklar. Uygulamayı kullanmanın iki farklı yolu vardır. Cihazınızdan anında iletme bildirimleri alabilir veya doğrulama kodu almak için uygulamayı açabilirsiniz.

### <a name="to-sign-in-with-a-notification-from-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasından bir bildirimle oturum açma
1. Kullanıcı adınızı ve parolanızı kullanarak Office 365 gibi bir uygulamada veya hizmette oturum açın.
2. Microsoft, cihazınızdaki Microsoft Authenticator uygulamasına bir bildirim gönderir.

   ![Microsoft bildirim gönderir](./media/multi-factor-authentication-end-user-signin/notify.png)

3. Telefonunuzdaki bildirimi açın ve **Doğrula** tuşunu seçin. Şirketinizin PIN'e ihtiyacı varsa, buraya girin.
4. Şimdi oturum açmış olmalısın.

### <a name="to-sign-in-using-a-verification-code-with-the-microsoft-authenticator-app"></a>Microsoft Authenticator uygulamasıyla bir doğrulama kodu kullanarak oturum açma

Doğrulama kodları almak için Microsoft Authenticator uygulamasını kullanıyorsanız, uygulamayı açtığınızda hesap adınız altında bir numara görürsünüz. Bu numara her 30 saniyede bir değişir, böylece aynı numarayı iki kez kullanmazsınız. Doğrulama kodu istendiğinde, uygulamayı açın ve şu anda görüntülenen numarayı kullanın.

1. Kullanıcı adınızı ve parolanızı kullanarak Office 365 gibi bir uygulamada veya hizmette oturum açın.
2. Microsoft sizden bir doğrulama kodu ister.

   ![Doğrulama kodunu girin](./media/multi-factor-authentication-end-user-signin/verify3.png)

3. Telefonunuzdaki Microsoft Authenticator uygulamasını açın ve oturum açtığınız kutuya kodu girin.

## <a name="signing-in-with-an-alternate-method"></a>Alternatif bir yöntemle oturum açma
Bazen tercih ettiğiniz doğrulama yöntemi olarak ayarladığınız telefon veya cihaza sahip olmazsınız. Bu nedenle hesabınız için yedekleme yöntemleri ayarlamanızı öneririz. Aşağıdaki bölümde, birincil yönteminiz kullanılamıyorsa alternatif bir yöntemle nasıl oturum açabileceğiniz gösterilmektedir.

1. Kullanıcı adınızı ve parolanızı kullanarak Office 365 gibi bir uygulamada veya hizmette oturum açın.
2. **Farklı bir doğrulama seçeneği kullan'ı**seçin. Kurulum unuzun kaç tanesine bağlı olarak farklı doğrulama seçenekleri görürsünüz.
3. Alternatif bir yöntem seçin ve oturum açın.

   ![Alternatif yöntem kullanma](./media/multi-factor-authentication-end-user-signin/alt.png)

## <a name="next-steps"></a>Sonraki adımlar
- İki aşamalı doğrulamayla oturum açarken sorun yaşıyorsanız, [Azure Çok Faktörlü Kimlik Doğrulama ile ilgili sorun yaşıyorsanız](multi-factor-authentication-end-user-troubleshoot.md)daha fazla bilgi alın.

- İki aşamalı doğrulama ayarlarınızı nasıl [yöneteceğinizi](multi-factor-authentication-end-user-manage-settings.md)öğrenin.

- Metinler ve telefon görüşmeleri yerine oturum açabilmeniz için Bildirimleri kullanabilmeniz için [Microsoft Authenticator uygulamasıyla](user-help-auth-app-download-install.md) nasıl başlayın.
