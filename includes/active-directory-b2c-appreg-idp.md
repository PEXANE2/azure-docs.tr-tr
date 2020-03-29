---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 80e5166775b0cf5acbfce32e61d91c0889e3b086
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186377"
---
Azure AD B2C kiracınızda bir uygulama kaydettirmek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulamalar'ı**seçin ve sonra **Ekle'yi**seçin.
1. Uygulama için bir ad girin. Örneğin, *testapp1*.
1. **Web Uygulaması / Web API için** **Evet'i**seçin.
1. **Yanıtla URL için**, girin`https://jwt.ms`
1. **Oluştur'u**seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *testapp1*.
1. **Herhangi bir kuruluş dizininde veya herhangi bir kimlik sağlayıcısında Hesapları**seçin.
1. **Uri'yi Yönlendirme**altında **Web'i**seçin ve ardından URL metin kutusuna girin. `https://jwt.ms`
1. **İzinler**altında, *openid ve offline_access izinleri* onay kutusunu açmak için Grant yöneticisi onayını seçin.
1. **Kaydol**’u seçin.

Başvuru kaydı tamamlandıktan sonra, örtülü hibe akışını etkinleştirin:

1. **Yönet**altında, **Kimlik Doğrulama'yı**seçin.
1. **Yeni deneyimi deneyin (gösteriliyorsa)** seçin.
1. **Örtülü hibe**altında, hem **Erişim belirteçleri** hem de **kimlik belirteçleri** onay kutularını seçin.
1. **Kaydet'i**seçin.