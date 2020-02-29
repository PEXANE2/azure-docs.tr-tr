---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 80e5166775b0cf5acbfce32e61d91c0889e3b086
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78186377"
---
Bir uygulamayı Azure AD B2C kiracınıza kaydetmek için, geçerli **uygulamalar** deneyimini veya yeni Birleşik **uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin](https://aka.ms/b2cappregintro).

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
1. Uygulama için bir ad girin. Örneğin, *testapp1*.
1. **Web uygulaması/Web API 'si**için **Evet**' i seçin.
1. **Yanıt URL 'si**için `https://jwt.ms` girin
1. **Oluştur**’u seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. **Uygulama kayıtları (Önizleme)** öğesini seçin ve ardından **Yeni kayıt**' ı seçin.
1. Uygulama için bir **ad** girin. Örneğin, *testapp1*.
1. **Herhangi bir kuruluş dizininde veya herhangi bir kimlik sağlayıcısında hesaplar '** ı seçin.
1. **Yeniden yönlendirme URI 'si**altında **Web**' i seçin ve ardından URL metin kutusuna `https://jwt.ms` yazın.
1. **İzinler**altında, *openıd ve offline_access izinleri Için yönetici izni ver* onay kutusunu seçin.
1. **Kaydol**’u seçin.

Uygulama kaydı tamamlandıktan sonra, örtük verme akışını etkinleştirin:

1. **Yönet**altında **kimlik doğrulaması**' nı seçin.
1. **Yeni deneyimi deneyin** (gösteriliyorsa) seçeneğini belirleyin.
1. **Örtük izin**' ın altında, **erişim belirteçleri** ve **Kimlik belirteçleri** onay kutularını seçin.
1. **Kaydet**’i seçin.