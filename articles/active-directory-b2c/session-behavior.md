---
title: Oturum davranışını Yapılandırma-Azure Active Directory B2C | Microsoft Docs
description: Azure Active Directory B2C 'de oturum davranışını yapılandırmayı öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/12/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 091704fabb7b50a0c83625c6ae46d9a807f01ffc
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91961043"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Azure Active Directory B2C 'de oturum davranışını yapılandırma

Azure Active Directory B2C (Azure AD B2C) içindeki [Çoklu oturum açma (SSO) oturum](session-overview.md) yönetimi, kullanıcının kimliği doğrulandıktan sonra bir kullanıcının Kullanıcı ile etkileşimini denetlemesine olanak sağlar. Örneğin, yönetici, kimlik sağlayıcılarının seçiminin görüntülenip görüntülenmediğini veya hesap ayrıntılarının yeniden girilmesi gerekip gerekmediğini denetleyebilir. Bu makalede Azure AD B2C için SSO ayarlarının nasıl yapılandırılacağı açıklanır.

## <a name="session-behavior-properties"></a>Oturum davranışı özellikleri

Web uygulaması oturumlarını yönetmek için aşağıdaki özellikleri kullanabilirsiniz:

- **Web uygulaması oturumu ömrü (dakika)** -başarılı kimlik doğrulaması sırasında kullanıcının tarayıcısında depolanan Azure AD B2C's oturum tanımlama bilgisinin ömrü.
    - Varsayılan = 1440 dakika.
    - En düşük (kapsamlı) = 15 dakika.
    - Maksimum (dahil) = 1440 dakika.
- **Web uygulaması oturumu zaman aşımı** - [oturum süre sonu türü](session-overview.md#session-expiry-type), *yuvarlama*veya *mutlak*. 
- **Çoklu oturum açma yapılandırması** -çoklu oturum açma (SSO) davranışının, Azure AD B2C kiracınızdaki birden çok uygulama ve Kullanıcı akışı üzerindeki [oturum kapsamı](session-overview.md#session-scope) .


## <a name="configure-the-properties"></a>Özellikleri yapılandırma

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip Azure AD B2C kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kullanıcı akışları ' nı**seçin.
5. Daha önce oluşturduğunuz kullanıcı akışını açın.
6. **Özellikler**’i seçin.
7. **Web uygulaması oturumu yaşam süresi (dakika)**, **Web uygulaması oturumu zaman aşımı**, **Çoklu oturum açma yapılandırması**yapılandırın ve gerektiğinde **oturum kapatma isteklerinde kimlik belirteci gerektir** .

    ![Azure portal oturum davranışı özelliği ayarları](./media/session-behavior/session-behavior.png)

8. **Kaydet**’e tıklayın.

## <a name="configure-sign-out-behavior"></a>Oturum kapatma davranışını yapılandırma

### <a name="secure-your-logout-redirect"></a>Logout yeniden yönlendirmenizi güvenli hale getirin

Oturum kapatıldıktan sonra, `post_logout_redirect_uri` uygulama için belirtilen yanıt URL 'lerinden bağımsız olarak, Kullanıcı parametresinde BELIRTILEN URI 'ye yönlendirilir. Ancak, geçerli bir `id_token_hint` değer geçirilirse ve **oturum kapatma Isteklerinde kimlik belirteci iste** açıksa Azure AD B2C, öğesinin `post_logout_redirect_uri` yeniden yönlendirmeyi gerçekleştirmeden önce uygulamanın yapılandırılmış yeniden yönlendirme URI 'lerinden biriyle eşleştiğini doğrular. Uygulama için eşleşen bir yanıt URL 'SI yapılandırılmamışsa, bir hata iletisi görüntülenir ve Kullanıcı yeniden yönlendirilmez. Oturum kapatma isteklerinde KIMLIK belirteci istemek için:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip Azure AD B2C kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Kullanıcı akışları ' nı**seçin.
1. Daha önce oluşturduğunuz kullanıcı akışını açın.
1. **Özellikler**’i seçin.
1. **Oturum kapatma ISTEKLERINDE kimlik belirteci gerektir '** i etkinleştirin.
1. **Azure AD B2C**'e geri dönün.
1. **Uygulama kayıtları**' yi seçin ve ardından uygulamanızı seçin.
1. **Kimlik Doğrulaması**'nı seçin.
1. **Logout URL 'si** metin kutusunda, post Logout yeniden yönlendirme URI 'sini yazın ve ardından **Kaydet**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2C oturum](session-overview.md)hakkında daha fazla bilgi edinin.
