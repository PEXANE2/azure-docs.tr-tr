---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: baf39c1fe72703d8ebc18b03bae1c963536d7ced
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73475199"
---
Bir uygulamayı Azure AD B2C kiracınıza kaydetmek için, geçerli **uygulamalar** deneyimini veya yeni Birleşik **uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Önizleme deneyimi hakkında daha fazla bilgi edinin](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure Active Directory** (Azure AD B2C*değil* ) öğesini seçin. Ya da **tüm hizmetler** ' i seçip **Azure Active Directory**arayıp seçin.
1. **Yönet**altında **uygulama kayıtları (eski)** seçeneğini belirleyin.
1. **Yeni uygulama kaydı**’nı seçin.
1. Uygulama için bir ad girin. Örneğin, *managementapp1*.
1. **Uygulama türü**için **Web uygulaması/API**' yi seçin.
1. **Oturum açma URL**'sinde GEÇERLI bir URL girin. Örneğin, `https://localhost`. Uç noktanın erişilebilir olması gerekmez, ancak geçerli bir URL olmalıdır.
1. **Oluştur**'u seçin.
1. **Kayıtlı uygulamaya** Genel Bakış sayfasında görüntülenen **uygulama kimliğini** kaydedin. Bu değeri sonraki bir adımda kullanırsınız.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. **Uygulama kayıtları (Önizleme)** öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. Uygulama için bir **ad** girin. Örneğin, *managementapp1*.
1. **Yalnızca bu kuruluş dizininde hesaplar '** ı seçin.
1. **İzinler**altında, *OpenID ve offline_access Permissions Için yönetici izni ver* onay kutusunu temizleyin.
1. **Kaydol**’u seçin.
1. Uygulamaya Genel Bakış sayfasında görüntülenen **uygulama (istemci) kimliğini** kaydedin. Bu değeri sonraki bir adımda kullanırsınız.