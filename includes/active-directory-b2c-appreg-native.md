---
author: mmacy
ms.service: active-directory-b2c
ms.topic: include
ms.date: 09/27/2019
ms.author: marsma
ms.openlocfilehash: b83a6b9185eb4ef127da1dd3b55aba4e8b2945f9
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326291"
---
1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüden **Dizin + abonelik** filtresi ' ni seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. Sol menüden **Azure AD B2C**' yi seçin. Ya da **tüm hizmetler** ' i seçin ve **Azure AD B2C**seçin.
1. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
1. Uygulama için bir ad girin. Örneğin, *nativeapp1*.
1. **Yerel istemci**için **Evet**' i seçin.
1. Benzersiz bir şemaya sahip **özel bir yeniden yönlendirme URI 'si** girin. Örneğin, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`. Yeniden yönlendirme URI 'SI seçerken dikkat etmeniz gereken iki önemli nokta vardır:
    1. **Benzersiz**: Yeniden yönlendirme URI 'sinin şeması her uygulama için benzersiz olmalıdır. Örnekte `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect` ,`com.onmicrosoft.contosob2c.exampleapp` şemadır. Bu düzenin izlenmesi gerekir. İki uygulama aynı düzeni paylaşıyorsa, kullanıcıya bir uygulama seçme seçeneği verilir. Kullanıcı yanlış seçerse, oturum açma başarısız olur.
    1. **Tamam**: Yeniden yönlendirme URI 'SI bir şemaya ve bir yola sahip olmalıdır. Yol, etki alanından sonra en az bir eğik çizgi içermelidir. Örneğin, `//oauth/` `//oauth` başarısız olsa da işe yarar. URI 'de, alt çizgi gibi özel karakterler eklemeyin.
1. **Oluştur**’u seçin.
