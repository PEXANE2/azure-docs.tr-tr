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
ms.date: 04/20/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 5b21fcd2d3ec5560b01352b112e9ed1bb2404766
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678054"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C'de belirli bir Azure Active Directory kuruluşu için oturum açma

Azure AD B2C'de [bir kimlik sağlayıcısı](authorization-code-flow.md) olarak bir Azure Etkin Dizin (Azure AD) kullanmak için, onu temsil eden bir uygulama oluşturmanız gerekir. Bu makalede, Azure AD B2C'deki bir kullanıcı akışını kullanarak belirli bir Azure AD kuruluşundan kullanıcılar için oturum açmayı nasıl etkinleştirdiğinizi gösterir.

[!INCLUDE [active-directory-b2c-identity-provider-azure-ad](../../includes/active-directory-b2c-identity-provider-azure-ad.md)]

## <a name="configure-azure-ad-as-an-identity-provider"></a>Azure AD'yi kimlik sağlayıcısı olarak yapılandırma

1. Azure AD B2C kiracıiçeren dizini kullandığınızdan emin olun. Üst menüdeki **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. **Kimlik sağlayıcılarını**seçin ve ardından **Yeni OpenID Bağlantı sağlayıcısını**seçin.
1. Bir **Ad**girin. Örneğin, *Contoso Azure AD'yi*girin.
1. **Metadata url'si için,** `{tenant}` Azure AD kiracınızın alan adı ile değiştirerek aşağıdaki URL'yi girin:

    ```
    https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
    ```

    Örneğin, `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`.

1. **İstemci Kimliği**için, daha önce kaydettiğiniz başvuru kimliğini girin.
1. **İstemci sırrı**için, daha önce kaydettiğiniz istemci sırrını girin.
1. **Kapsam**için , `openid profile`.
1. **Yanıt türü**ve **Yanıt modu**için varsayılan değerleri bırakın.
1. (İsteğe bağlı) Etki **Alanı ipucu** `contoso.com`için , girin . Daha fazla bilgi için bkz: [Azure Active Directory B2C'yi kullanarak doğrudan oturum açma'yı ayarla.](direct-signin.md#redirect-sign-in-to-a-social-provider)
1. **Kimlik sağlayıcı talepleri eşleme**altında, aşağıdaki iddiaları seçin:

    * **Kullanıcı Kimliği**: *oid*
    * **Görüntü adı**: *ad*
    * **Ad soyad**: *given_name*
    * **Ad soyad**: *family_name*
    * **E-posta**: *unique_name*

1. **Kaydet**’i seçin.
