---
title: Azure Active Directory kuruluş için oturum açma ayarlama-Azure Active Directory B2C
description: Azure Active Directory B2C belirli bir Azure Active Directory organizasyonu için oturum açma ayarlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 477b4e51c49a558aed0e5623a3821fa9b8d9eabd
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69622359"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C içinde belirli bir Azure Active Directory kuruluş için oturum açma ayarlayın

Azure AD B2C bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) olarak Azure Active Directory (Azure AD) kullanmak için, onu temsil eden bir uygulama oluşturmanız gerekir. Bu makalede, Azure AD B2C bir Kullanıcı akışı kullanarak belirli bir Azure AD kuruluştan kullanıcılar için oturum açmayı etkinleştirme konusu gösterilmektedir.

## <a name="create-an-azure-ad-app"></a>Azure AD uygulaması oluşturma

Kullanıcıların belirli bir Azure AD kuruluştan oturum açmasını etkinleştirmek için, Azure AD B2C kiracınızla aynı olmayan kurumsal Azure AD kiracısında bir uygulamayı kaydetmeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD kiracınızı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD kiracınızı içeren dizini seçin. Bu, Azure AD B2C kiracınız ile aynı kiracı değildir.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **uygulama kayıtları**' i arayıp seçin.
4. **Yeni kayıt**seçeneğini belirleyin.
5. Uygulamanız için bir ad girin. Örneğin: `Azure AD B2C App`.
6. Bu **kuruluş dizinindeki hesapların seçimini yalnızca** bu uygulama için kabul edin.
7. **Yeniden yönlendirme URI 'si**için, **Web**'in değerini kabul edin ve aşağıdaki URL 'yi tüm küçük harflerle, burada `your-B2C-tenant-name` Azure AD B2C kiracınızın adıyla birlikte girin. Örneğin `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Tüm URL 'Ler artık [b2clogin.com](b2clogin.md)kullanıyor olmalıdır.

8. **Kaydol**' a tıklayın. Daha sonra kullanılacak **uygulama (istemci) kimliğini** kopyalayın.
9. Uygulama menüsünde **sertifikalar & gizlilikler** ' ı seçin ve ardından **yeni istemci parolası**' nı seçin.
10. İstemci parolası için bir ad girin. Örneğin: `Azure AD B2C App Secret`.
11. Süre sonu dönemini seçin. Bu uygulama için, **1 yıl içinde**seçimini kabul edin.
12. **Ekle** ' yi seçin ve daha sonra kullanılmak üzere görüntülenen yeni istemci parolasının değerini kopyalayın.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Azure AD 'yi kimlik sağlayıcısı olarak yapılandırma

1. Azure AD B2C kiracı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve ardından **Yeni OpenID Connect sağlayıcısı**' nı seçin.
1. Bir **ad**girin. Örneğin, *contoso Azure AD*yazın.
1. **Meta veri URL 'si**için, Azure AD kiracınızın etki alanı adıyla `your-AD-tenant-domain` birlikte aşağıdaki URL 'yi girin:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Örneğin: `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

1. **ISTEMCI kimliği**için, daha önce KAYDETTIĞINIZ uygulama kimliğini girin.
1. **İstemci parolası**için, daha önce kaydettiğiniz istemci gizli anahtarını girin.
1. **Kapsam**, **yanıt türü**ve **Yanıt modu**için varsayılan değerleri bırakın.
1. Seçim **Domain_hint**için bir değer girin. Örneğin, *Contosoad*. Bu, istekte *domain_hint* kullanarak bu kimlik sağlayıcısına başvururken kullanılacak değerdir.
1. **Kimlik sağlayıcısı talep eşlemesi**altında, aşağıdaki talep eşleme değerlerini girin:

    * **Kullanıcı kimliği**: *OID*
    * **Görünen ad**: *ad*
    * **Verilen ad**: *given_name*
    * **Soyadı**: *family_name*
    * **E-posta**: *unique_name*

1. **Kaydet**’i seçin.
