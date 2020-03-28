---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: d3a323a28d92e2a5834b65316d61c7d53a42aacf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183796"
---
Azure AD B2C kiracınızda bir uygulama kaydettirmek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulamalar'ı**seçin ve sonra **Ekle'yi**seçin.
1. Uygulama için bir ad girin. Örneğin, *nativeapp1*.
1. **Yerel istemci**için **Evet'i**seçin.
1. Benzersiz bir şema ile **Özel Yönlendirme URI** girin. Örneğin, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. Yeniden yönlendirme URI'si seçerken iki önemli husus vardır:
    * **Benzersiz**: Yeniden yönlendirme URI şeması her uygulama için benzersiz olmalıdır. Örnekte, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect` `com.onmicrosoft.contosob2c.exampleapp` şema olduğunu. Bu desen takip edilmelidir. İki uygulama aynı düzeni paylaşıyorsa, kullanıcıya bir uygulama seçme seçeneği verilir. Kullanıcı yanlış seçerse, oturum açma başarısız olur.
    * **Tam**: Yeniden yönlendirme URI'nin hem bir düzeni hem de bir yolu olmalıdır. Yol etki alanından sonra en az bir ileri eğik çizgi içermelidir. Örneğin, `//oauth/` başarısız `//oauth` olurken çalışır. Örneğin URI'ye özel karakterler eklemeyin.
1. **Oluştur'u**seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *nativeapp1*.
1. **Desteklenen hesap türleri**altında, herhangi bir kuruluş **dizinindeki veya herhangi bir kimlik sağlayıcısındaki Hesapları**seçin.
1. **Redirect URI**altında, **Açılan kullanıcı/yerel (mobil & masaüstü)** seçmek için açılır aşağı kullanın.
1. Benzersiz bir şema ile yeniden yönlendirme URI girin. Örneğin, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. Yeniden yönlendirme URI'si seçerken iki önemli husus vardır:
    * **Benzersiz**: Yeniden yönlendirme URI şeması her uygulama için benzersiz olmalıdır. Örnekte, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect` `com.onmicrosoft.contosob2c.exampleapp` şema olduğunu. Bu desen takip edilmelidir. İki uygulama aynı düzeni paylaşıyorsa, kullanıcıya bir uygulama seçme seçeneği verilir. Kullanıcı yanlış seçerse, oturum açma başarısız olur.
    * **Tam**: Yeniden yönlendirme URI'nin hem bir düzeni hem de bir yolu olmalıdır. Yol etki alanından sonra en az bir ileri eğik çizgi içermelidir. Örneğin, `//oauth/` başarısız `//oauth` olurken çalışır. Örneğin URI'ye özel karakterler eklemeyin.
1. **İzinler**altında, *openid ve offline_access izinleri* onay kutusunu açmak için Grant yöneticisi onayını seçin.
1. **Kaydol**’u seçin.