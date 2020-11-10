---
title: Tek sayfalı uygulamayı yapılandırma-Microsoft Identity platform | Mavisi
description: Tek sayfalı bir uygulama (uygulamanın kod yapılandırması) oluşturmayı öğrenin
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 02/11/2020
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 30a9b710ffbf98ebc523217a3b8a7fd9a2640c49
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94443050"
---
# <a name="single-page-application-code-configuration"></a>Tek sayfalı uygulama: kod yapılandırma

Tek sayfalı uygulamanız (SPA) için kodu yapılandırmayı öğrenin.

## <a name="msal-libraries-for-spas-and-supported-authentication-flows"></a>Maça ve desteklenen kimlik doğrulama akışları için MSAL kitaplıkları

Microsoft Identity platformu, sektörde önerilen güvenlik uygulamalarını kullanarak PKCE ile örtük akış ve yetkilendirme kodu akışını desteklemek için JavaScript (MSAL.js) için aşağıdaki Microsoft kimlik doğrulama kitaplığını sağlar:

| MSAL kitaplığı | Akış | Açıklama |
|--------------|------|-------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js (2. x)](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) | Yetkilendirme kodu akışı (PKCE) | JavaScript veya SPA çerçeveleri aracılığıyla oluşturulmuş herhangi bir istemci tarafı Web uygulamasında kullanmak için, angular, Vue.js ve React.js gibi basit JavaScript kitaplığı. |
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js (1. x)](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-core) | Örtük akış | JavaScript veya SPA çerçeveleri aracılığıyla oluşturulmuş herhangi bir istemci tarafı Web uygulamasında kullanmak için, angular, Vue.js ve React.js gibi basit JavaScript kitaplığı. |
| ![MSAL angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Örtük akış | Angular çerçevesi aracılığıyla oluşturulan tek sayfalı uygulamalarda kullanımı basitleştirecek çekirdek MSAL.js kitaplığı sarmalayıcısı. |

## <a name="application-code-configuration"></a>Uygulama kodu yapılandırması

Bir MSAL kitaplığında, kitaplık başlatma sırasında uygulama kayıt bilgileri yapılandırma olarak geçirilir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_client_id'
    }
};

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```

Yapılandırılabilir seçenekler hakkında daha fazla bilgi için bkz. [MSAL.jsuygulama başlatma ](msal-js-initializing-client-applications.md).

# <a name="angular"></a>[Angular](#tab/angular)

```javascript
// App.module.ts
import { MsalModule } from '@azure/msal-angular';

@NgModule({
    imports: [
        MsalModule.forRoot({
            auth: {
                clientId: 'your_app_id'
            }
        })
    ]
})

export class AppModule { }
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryodaki bir sonraki makaleye geçin, [oturum açın ve oturumu kapatın](scenario-spa-sign-in.md).
