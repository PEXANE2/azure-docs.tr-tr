---
title: Azure REKLAM kuruluşu için kaydolma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de belirli bir Azure Active Directory kuruluşu için kaydolma ayarlama.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 35fc4e1d64fa7df392fa878db14c0464da7dccf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78188316"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de belirli bir Azure Active Directory kuruluşu için oturum açma

Azure AD B2C'de [bir kimlik sağlayıcısı](authorization-code-flow.md) olarak bir Azure Etkin Dizin (Azure AD) kullanmak için, onu temsil eden bir uygulama oluşturmanız gerekir. Bu makalede, Azure AD B2C'deki bir kullanıcı akışını kullanarak belirli bir Azure AD kuruluşundan kullanıcılar için oturum açmayı nasıl etkinleştirdiğinizi gösterir.

## <a name="create-an-azure-ad-app"></a>Azure AD uygulaması oluşturma

Belirli bir Azure AD kuruluşundan kullanıcılar için oturum açmayı etkinleştirmek için, Azure AD B2C kiracınızla aynı olmayan kuruluş azure AD kiracısına bir uygulama kaydetmeniz gerekir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure AD kiracınızı içeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD kiracınızı içeren dizini seçin. Bu, Azure AD B2C kiracınızla aynı kiracı değildir.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından **Uygulama kayıtlarını**arayın ve seçin.
4. **Yeni kayıt**seçin.
5. Uygulamanız için bir ad girin. Örneğin, `Azure AD B2C App`.
6. Bu kuruluş **dizinindeki Hesapların** seçimini yalnızca bu uygulama için kabul edin.
7. Yeniden **Yönlendirme**URI'si için **Web**değerini kabul edin ve Azure `your-B2C-tenant-name` AD B2C kiracınızın adıyla değiştirilen tüm küçük harflere aşağıdaki URL'yi girin. Örneğin, `https://fabrikam.b2clogin.com/fabrikam.onmicrosoft.com/oauth2/authresp`:

    ```
    https://your-B2C-tenant-name.b2clogin.com/your-B2C-tenant-name.onmicrosoft.com/oauth2/authresp
    ```

    Tüm URL'ler artık [b2clogin.com](b2clogin.md)kullanmalıdır.

8. **Kaydol'u**tıklatın. Daha sonra kullanılacak **Uygulama (istemci) kimliğini** kopyalayın.
9. Uygulama menüsünde **Sertifikalar & sırları** seçin ve ardından **Yeni istemci sırrını**seçin.
10. İstemci sırrı için bir ad girin. Örneğin, `Azure AD B2C App Secret`.
11. Son kullanma süresini seçin. Bu uygulama için, **1 yıl içinde**seçim kabul edin.
12. **Ekle'yi** seçin ve daha sonra kullanılmak üzere görüntülenen yeni istemci sırrının değerini kopyalayın.

## <a name="configure-azure-ad-as-an-identity-provider"></a>Azure AD'yi kimlik sağlayıcısı olarak yapılandırma

1. Azure AD B2C kiracıiçeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. **Kimlik sağlayıcılarını**seçin ve ardından **Yeni OpenID Bağlantı sağlayıcısını**seçin.
1. Bir **Ad**girin. Örneğin, *Contoso Azure AD'yi*girin.
1. **Metadata url'si için,** `your-AD-tenant-domain` Azure AD kiracınızın alan adı ile değiştirerek aşağıdaki URL'yi girin:

    ```
    https://login.microsoftonline.com/your-AD-tenant-domain/.well-known/openid-configuration
    ```

    Örneğin, `https://login.microsoftonline.com/contoso.onmicrosoft.com/.well-known/openid-configuration`.

    **Do not** Örneğin `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`Azure AD v2.0 meta veri bitiş noktasını kullanmayın. Bunu yapmak, oturum açmaya çalışırken kine `AADB2C: A claim with id 'UserId' was not found, which is required by ClaimsTransformation 'CreateAlternativeSecurityId' with id 'CreateAlternativeSecurityId' in policy 'B2C_1_SignUpOrIn' of tenant 'contoso.onmicrosoft.com'` benzer bir hatayla sonuçlanır.

1. **İstemci Kimliği**için, daha önce kaydettiğiniz başvuru kimliğini girin.
1. **İstemci sırrı**için, daha önce kaydettiğiniz istemci sırrını girin.
1. **Kapsam,** **Yanıt türü**ve Yanıt **modu**için varsayılan değerleri bırakın.
1. (İsteğe bağlı) **Domain_hint**için bir değer girin. Örneğin, *ContosoAD*. Bu, istekteki *domain_hint* kullanarak bu kimlik sağlayıcısına atıfta bulunularak kullanılacak değerdir.
1. **Kimlik sağlayıcı talepleri eşleme**altında, aşağıdaki iddiaları eşleme değerleri girin:

    * **Kullanıcı Kimliği**: *oid*
    * **Görüntü adı**: *ad*
    * **Ad soyad**: *given_name*
    * **Ad soyad**: *family_name*
    * **E-posta**: *unique_name*

1. **Kaydet'i**seçin.
