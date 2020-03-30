---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: mimart
ms.openlocfilehash: 31d4b65c6cd6a3dfe3cb779022329f338436c69b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78184376"
---
Azure AD B2C kiracınızda bir uygulama kaydettirmek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure portalında Azure Etkin **Dizini'ni**arayın ve seçin.
1. **Yönet**altında, **Uygulama kayıtlarını (Eski)** seçin.
1. **Yeni uygulama kaydı**’nı seçin.
1. Uygulama için bir ad girin. Örneğin, *yönetimapp1*.
1. **Uygulama türü için**Web uygulamasını / **API'yi**seçin.
1. **Oturum Açma URL'sinde**geçerli bir URL girin. Örneğin, `https://localhost`. Bitiş noktasına ulaşılabilmesi gerekmez, ancak geçerli bir URL olmalıdır.
1. **Oluştur'u**seçin.
1. Kayıtlı **uygulamaya** genel bakış sayfasında görünen Uygulama Kimliğini **kaydedin.** Bu değeri daha sonraki bir adımda kullanırsınız.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Azure **portalında, Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *yönetimapp1*.
1. **Yalnızca bu kuruluş dizinindeki Hesapları**seçin.
1. **İzinler**altında, *Grant yöneticisinin openid ve offline_access izinleri* onay kutusunu açma iznini temizleyin.
1. **Kaydol**’u seçin.
1. Uygulamaya genel bakış sayfasında görünen **Uygulama (istemci) kimliğini** kaydedin. Bu değeri daha sonraki bir adımda kullanırsınız.