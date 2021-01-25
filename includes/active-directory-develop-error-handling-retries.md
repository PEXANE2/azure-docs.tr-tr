---
author: mmacy
ms.author: marsma
ms.date: 11/25/2020
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.openlocfilehash: d0377e3e918a8431c35c1f5b15a1c4b54eeae3b4
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "98760641"
---
## <a name="retrying-after-errors-and-exceptions"></a>Hatalar ve özel durumlar sonrasında yeniden deneniyor

MSAL çağrılırken kendi yeniden deneme ilkelerinizi uygulamanız bekleniyor. MSAL, Azure AD hizmetine HTTP çağrıları yapar ve bazen hatalara neden olabilir. Örneğin, ağ kapatılabilir veya sunucu aşırı yüklenmiş olabilir.  

### <a name="http-429"></a>HTTP 429

Hizmet belirteci sunucusu (STS) çok fazla istek ile aşırı yüklendiğinde, yanıt alanında yeniden deneyebilmeniz için ne kadar süreyle bir ipucu ile HTTP hatası 429 döndürür `Retry-After` .