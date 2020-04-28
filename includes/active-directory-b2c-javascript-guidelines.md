---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/11/2020
ms.author: mimart
ms.openlocfilehash: ef08f1adc15475cd5dd38548ed39dc57532c0ef5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78185853"
---
## <a name="guidelines-for-using-javascript"></a>JavaScript kullanımı için yönergeler

JavaScript kullanarak uygulamanızın arabirimini özelleştirirken aşağıdaki yönergeleri izleyin:

- `<a>` HTML öğelerine bir tıklama olayı bağlamayın.
- Azure AD B2C koduna veya açıklamalara bağımlılık almaz.
- Azure AD B2C HTML öğelerinin sırasını veya hiyerarşisini değiştirmeyin. Kullanıcı arabirimi öğelerinin sırasını denetlemek için bir Azure AD B2C İlkesi kullanın.
- Şu noktalara sahip herhangi bir yeniden kullanılabilir hizmeti çağırabilirsiniz:
    - İstemci tarafı HTTP çağrılarına izin vermek için, Restınservıce CORS 'nizi ayarlamanız gerekebilir.
    - Yeniden yaptığınız hizmetin güvende olduğundan ve yalnızca HTTPS protokolünü kullandığından emin olun.
    - Azure AD B2C uç noktaları çağırmak için JavaScript 'ı doğrudan kullanmayın.
- JavaScript kodunuzu ekleyebilir veya dış JavaScript dosyalarına bağlanabilirsiniz. Bir dış JavaScript dosyası kullanırken göreli URL değil, mutlak URL 'yi kullandığınızdan emin olun.
- JavaScript çerçeveleri:
    - Azure AD B2C, jQuery 'in belirli bir sürümünü kullanır. JQuery 'in başka bir sürümünü eklemeyin. Aynı sayfada birden fazla sürüm kullanılması sorunlara neden olur.
    - RequireJS kullanmak desteklenmez.
    - Çoğu JavaScript çerçevesi Azure AD B2C tarafından desteklenmez.
- Azure AD B2C ayarları, `window.CONTENT` geçerli kullanıcı arabirimi dili `window.SETTINGS`gibi nesneler çağırarak okunabilir. Bu nesnelerin değerini değiştirmeyin.
- Azure AD B2C hata iletisini özelleştirmek için, bir ilkede yerelleştirme kullanın.
- Bir ilke kullanılarak herhangi bir şey elde edilebildiği takdirde, genellikle önerilen yoldur.
