---
title: Azure Active Directory kuruluş için oturum açma ayarlama-Azure Active Directory B2C
description: Azure Active Directory B2C belirli bir Azure Active Directory organizasyonu için oturum açma ayarlayın.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 58c6d1b032f5b492c5641ff51da80426124069b1
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68716777"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C içinde belirli bir Azure Active Directory kuruluş için oturum açma ayarlayın

>[!NOTE]
> Bu özellik genel önizleme aşamasındadır. Özelliği üretim ortamlarında kullanmayın.

Azure AD B2C bir [kimlik sağlayıcısı](active-directory-b2c-reference-oauth-code.md) olarak Azure Active Directory (Azure AD) kullanmak için, onu temsil eden bir uygulama oluşturmanız gerekir. Bu makalede, Azure AD B2C bir Kullanıcı akışı kullanarak belirli bir Azure AD kuruluştan kullanıcılar için oturum açmayı etkinleştirme konusu gösterilmektedir.

## <a name="create-an-azure-ad-app"></a>Azure AD uygulaması oluşturma

Kullanıcıların belirli bir Azure AD kuruluştan oturum açmasını etkinleştirmek için, Azure AD B2C kiracınızla aynı olmayan kurumsal Azure AD kiracısında bir uygulamayı kaydetmeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüden **Dizin ve abonelik filtresi** ' ni seçin ve Azure AD kiracınızı içeren dizini seçin. Bu, Azure AD B2C kiracınız ile aynı kiracı değildir.
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

1. Azure AD B2C kiracı içeren dizini kullandığınızdan emin olun. Üst menüden **Dizin ve abonelik filtresi** ' ni seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
2. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
3. Seçin **kimlik sağlayıcıları**ve ardından **Ekle**.
4. Bir **ad**girin. Örneğin, `Contoso Azure AD` girin.
5. **Kimlik sağlayıcısı türünü**seçin, **OpenID Connect (Önizleme)** öğesini seçin ve ardından **Tamam**' a tıklayın.
6. **Bu kimlik sağlayıcısını ayarla** ' yı seçin
7. **Meta veri URL 'si**için, Azure AD kiracınızın etki alanı adıyla `your-AD-tenant-domain` birlikte aşağıdaki URL 'yi girin. Örneğin `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

8. **ISTEMCI kimliği**için, daha önce KAYDETTIĞINIZ uygulama kimliğini ve **istemci gizli**anahtarını girip, daha önce kaydettiğiniz istemci gizli anahtarını girin.
9. İsteğe bağlı olarak, **Domain_hint**için bir değer girin. Örneğin: `ContosoAD`. Bu, istekte *domain_hint* kullanarak bu kimlik sağlayıcısına başvururken kullanılacak değerdir.
10.           **Tamam**'ı tıklatın.
11. **Bu kimlik sağlayıcısının taleplerini eşle** ve aşağıdaki talepleri ayarla ' yı seçin:

    - **Kullanıcı kimliği**için girin `oid`.
    - **Görünen ad**için girin `name`.
    - **Verilen ad**için girin `given_name`.
    - **Soyadı**için girin `family_name`.
    - **E-posta**için `unique_name`girin.

12. **Tamam**' a tıklayın ve sonra yapılandırmanızı kaydetmek için **Oluştur** ' a tıklayın.
