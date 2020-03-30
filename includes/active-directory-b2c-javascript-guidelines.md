---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/11/2020
ms.author: mimart
ms.openlocfilehash: ef08f1adc15475cd5dd38548ed39dc57532c0ef5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78185853"
---
## <a name="guidelines-for-using-javascript"></a>JavaScript kullanma yönergeleri

JavaScript kullanarak uygulamanızın arabirimini özelleştirdiğinizde aşağıdaki yönergeleri izleyin:

- `<a>` HTML öğelerine bir tıklama olayı bağlamayın.
- Azure AD B2C koduna veya açıklamalarına bağımlı olun.
- Azure AD B2C HTML öğelerinin sırasını veya hiyerarşisini değiştirmeyin. UI öğelerinin sırasını denetlemek için bir Azure AD B2C ilkesi kullanın.
- Herhangi bir RESTful hizmeti şu hususlarla arayabilirsiniz:
    - Müşteri tarafı HTTP çağrılarına izin vermek için restful service CORS'unuzu ayarlamanız gerekebilir.
    - RESTful hizmetinizin güvenli olduğundan ve yalnızca HTTPS protokolünü kullandığından emin olun.
    - Azure AD B2C uç noktalarını aramak için doğrudan JavaScript'i kullanmayın.
- JavaScript'inizi gömebilir veya harici JavaScript dosyalarına bağlanabilirsiniz. Harici bir JavaScript dosyası kullanırken, göreceli bir URL değil, mutlak URL kullandığınızdan emin olun.
- JavaScript çerçeveleri:
    - Azure AD B2C jQuery'nin belirli bir sürümünü kullanır. jQuery'nin başka bir sürümünü eklemeyin. Aynı sayfada birden fazla sürümün kullanılması sorunlara neden olur.
    - RequireJS kullanımı desteklenmez.
    - Çoğu JavaScript çerçevesi Azure AD B2C tarafından desteklenmez.
- Azure AD B2C ayarları, `window.SETTINGS`geçerli `window.CONTENT` UI dili gibi nesneleri çağırarak okunabilir. Bu nesnelerin değerini değiştirmeyin.
- Azure AD B2C hata iletisini özelleştirmek için bir ilkede yerelleştirme yi kullanın.
- Bir ilke kullanılarak bir şey elde edilebilirse, genellikle önerilen yoldur.
