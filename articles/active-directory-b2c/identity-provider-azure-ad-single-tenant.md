---
title: Azure AD kuruluşu için oturum açma ayarlama
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C belirli bir Azure Active Directory organizasyonu için oturum açma ayarlayın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 04/20/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: e4fa4b64c6519df90d5883e8c5760b3ed2ce0337
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90004468"
---
# <a name="set-up-sign-in-for-a-specific-azure-active-directory-organization-in-azure-active-directory-b2c"></a>Azure Active Directory B2C içinde belirli bir Azure Active Directory kuruluş için oturum açma ayarlayın

Azure AD B2C bir [kimlik sağlayıcısı](authorization-code-flow.md) olarak Azure Active Directory (Azure AD) kullanmak için, onu temsil eden bir uygulama oluşturmanız gerekir. Bu makalede, Azure AD B2C bir Kullanıcı akışı kullanarak belirli bir Azure AD kuruluştan kullanıcılar için oturum açmayı etkinleştirme konusu gösterilmektedir.

[!INCLUDE [active-directory-b2c-identity-provider-azure-ad](../../includes/active-directory-b2c-identity-provider-azure-ad.md)]

## <a name="configure-azure-ad-as-an-identity-provider"></a>Azure AD 'yi kimlik sağlayıcısı olarak yapılandırma

1. Azure AD B2C kiracı içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Kimlik sağlayıcıları**' nı seçin ve ardından **Yeni OpenID Connect sağlayıcısı**' nı seçin.
1. Bir **ad**girin. Örneğin, *contoso Azure AD*yazın.
1. **Meta veri URL 'si**Için, `{tenant}` Azure AD kiracınızın etki alanı adıyla birlikte aşağıdaki URL 'yi girin:

    ```
    https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
    ```

    Örneğin, `https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0/.well-known/openid-configuration`.
    Örneğin, `https://login.microsoftonline.com/contoso.com/v2.0/.well-known/openid-configuration`.

1. **ISTEMCI kimliği**için, daha önce KAYDETTIĞINIZ uygulama kimliğini girin.
1. **İstemci parolası**için, daha önce kaydettiğiniz istemci gizli anahtarını girin.
1. **Kapsam**için, girin `openid profile` .
1. **Yanıt türü**ve **Yanıt modu**için varsayılan değerleri bırakın.
1. Seçim **Etki alanı ipucu**için girin `contoso.com` . Daha fazla bilgi için bkz. [Azure Active Directory B2C kullanarak doğrudan oturum açma ayarlama](direct-signin.md#redirect-sign-in-to-a-social-provider).
1. **Kimlik sağlayıcısı talep eşlemesi**altında aşağıdaki talepleri seçin:

    * **Kullanıcı kimliği**: *OID*
    * **Görünen ad**: *ad*
    * **Verilen ad**: *given_name*
    * **Soyadı**: *family_name*
    * **E-posta**: *unique_name*

1. **Kaydet**’i seçin.
