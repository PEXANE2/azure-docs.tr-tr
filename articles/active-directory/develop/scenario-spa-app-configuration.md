---
title: Tek sayfalı uygulamayı yapılandırma - Microsoft kimlik platformu | Azure
description: Tek sayfalık bir uygulamayı (uygulamanın kod yapılandırması) nasıl oluşturabilirsiniz öğrenin
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
ms.openlocfilehash: f159105046231ba5fb4e458cdd70d930a411a920
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80882344"
---
# <a name="single-page-application-code-configuration"></a>Tek sayfauygulaması: Kod yapılandırması

Tek sayfalı uygulamanızın (SPA) kodunu nasıl yapılandırıştırılayarıştırılayarıştırılayarıştırılayarıştırabilirsiniz.

## <a name="msal-libraries-that-support-implicit-flow"></a>Örtük akışı destekleyen MSAL kitaplıkları

Microsoft kimlik platformu, endüstri tarafından önerilen güvenlik uygulamalarını kullanarak örtülü akışı desteklemek için aşağıdaki Microsoft Kimlik Doğrulama Kitaplığı (MSAL) kitaplıklarını sağlar:

| MSAL kütüphanesi | Açıklama |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | JavaScript veya Açısal, Vue.js ve React.js gibi SPA çerçeveleri aracılığıyla oluşturulmuş istemci tarafındaki herhangi bir web uygulamasında kullanılmak üzere Düz JavaScript kitaplığı. |
| ![MSAL Açısal](media/sample-v2-code/logo_angular.png) <br/> [MSAL Açısal](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Açısal çerçeve üzerinden oluşturulmuş tek sayfalı uygulamalarda kullanımı kolaylaştırmak için çekirdek MSAL.js kitaplığın sarıcı. |

## <a name="application-code-configuration"></a>Uygulama kodu yapılandırması

Bir MSAL kitaplığında, uygulama kayıt bilgileri kitaplık başlatma sırasında yapılandırma olarak geçirilir.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
// Configuration object constructed.
const config = {
    auth: {
        clientId: 'your_app_id',
        redirectUri: "your_app_redirect_uri" //defaults to application start page
    }
}

// create UserAgentApplication instance
const userAgentApplication = new UserAgentApplication(config);
```

Yapılandırılabilir seçenekler hakkında daha fazla bilgi için, [MSAL.js ile Uygulama Başlatma'ya](msal-js-initializing-client-applications.md)bakın.

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

> [!div class="nextstepaction"]
> [Oturum açma ve oturumu kapatma](scenario-spa-sign-in.md)
