---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/11/2020
ms.author: mimart
ms.openlocfilehash: ef08f1adc15475cd5dd38548ed39dc57532c0ef5
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78185853"
---
## <a name="guidelines-for-using-javascript"></a>JavaScript kullanma yönergeleri

JavaScript kullanarak uygulamanızı arabiriminin'nı özelleştirdiğinizde, aşağıdaki yönergeleri izleyin:

- `<a>` HTML öğelerine bir tıklama olayı bağlamayın.
- Azure AD B2C kod ya da yorumlarınız bağımlılık almaz.
- Sipariş veya Azure AD B2C HTML öğeleri hiyerarşisini değiştirmeyin. UI öğelerin sırasını denetlemek için bir Azure AD B2C İlkesi'ni kullanın.
- Bu konuları ile herhangi bir RESTful hizmeti çağırabilirsiniz:
    - İstemci tarafı HTTP çağrıları izin vermek için CORS'yi RESTful hizmetinizi ayarlamanız gerekebilir.
    - RESTful hizmetiniz güvendedir ve yalnızca HTTPS protokolünü kullanan emin olun.
    - JavaScript, Azure AD B2C uç noktaları doğrudan çağırmak için kullanmayın.
- Javascript'inizi ekleyebilir veya dış JavaScript dosyalarına bağlayabilirsiniz. Harici bir JavaScript dosyasını kullanırken, mutlak bir URL ve göreli bir URL değil kullanmaya dikkat edin.
- Yeni JavaScript çerçevesi:
    - Azure AD B2C, jQuery belirli bir sürümünü kullanır. JQuery başka bir sürümünü içermez. Aynı sayfa üzerinde birden fazla sürümünü kullanarak sorunlarına neden olur.
    - RequireJS kullanımı desteklenmiyor.
    - En yeni JavaScript çerçevesi, Azure AD B2C tarafından desteklenmez.
- Azure AD B2C ayarları, geçerli kullanıcı arabirimi dili gibi `window.SETTINGS`, `window.CONTENT` nesneleri çağırarak okunabilir. Bu nesneler değerini değiştirmeyin.
- Azure AD B2C hata iletisini özelleştirmek için bir ilke yerelleştirme kullanın.
- Herhangi bir ilke kullanarak gerçekleştirilebilir, genellikle bu önerilen yoludur.
