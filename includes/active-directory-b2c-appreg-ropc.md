---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: cea3245176e6c38137d68e3ad4b47477bedc78be
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529178"
---
Azure AD B2C kiracınızda bir uygulama kaydettirmek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulamalar'ı**seçin ve sonra **Ekle'yi**seçin.
1. Uygulama için bir ad girin. Örneğin, *ROPC_Auth_app.*
1. **Yerel istemci**için **Evet'i**seçin.
1. Diğer değerleri olduğu gibi bırakın ve ardından **Oluştur'u**seçin.
1. Uygulama **kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *ROPC_Auth_app.*
1. Diğer değerleri olduğu gibi bırakın ve ardından **Kaydol'u**seçin.
1. Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.
1. **Yönet**altında, **Kimlik Doğrulama'yı**seçin.
1. **Yeni deneyimi deneyin (gösteriliyorsa)** seçin.
1. **Varsayılan istemci türü**altında, uygulamayı ortak istemci olarak ele almak için **Evet'i** seçin. Bu ayar ROPC akışı için gereklidir.
1. **Kaydet'i**seçin.
1. Sol menüde, bildirim düzenleyicisini açmak için **Bildirim'i** seçin. 
1. **Oauth2AllowImplicitFlow** özniteliğini *doğru*olarak ayarlayın:
    ```json
    "oauth2AllowImplicitFlow": true,
    ```
1. **Kaydet'i**seçin.