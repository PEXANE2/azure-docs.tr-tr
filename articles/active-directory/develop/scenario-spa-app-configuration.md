---
title: Tek sayfalı uygulamayı yapılandırma - Microsoft kimlik platformu | Azure
description: Tek sayfalık bir uygulamayı (uygulamanın kod yapılandırması) nasıl oluşturabilirsiniz öğrenin
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/11/2020
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f1e0bf44515aab18019b19b4f0a6f84183e5aac3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77160092"
---
# <a name="single-page-application-code-configuration"></a>Tek sayfauygulaması: Kod yapılandırması

Tek sayfalı uygulamanızın (SPA) kodunu nasıl yapılandırıştırılayarıştırılayarıştırılayarıştırılayarıştırabilirsiniz.

## <a name="msal-libraries-that-support-implicit-flow"></a>Örtük akışı destekleyen MSAL kitaplıkları

Microsoft kimlik platformu, endüstri tarafından önerilen güvenlik uygulamalarını kullanarak örtülü akışı desteklemek için aşağıdaki Microsoft Kimlik Doğrulama Kitaplığı (MSAL) kitaplıklarını sağlar:  

| MSAL kütüphanesi | Açıklama |
|--------------|--------------|
| ![MSAL.js](media/sample-v2-code/logo_js.png) <br/> [MSAL.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | JavaScript veya Açısal, Vue.js ve React.js gibi SPA çerçeveleri aracılığıyla oluşturulmuş istemci tarafındaki herhangi bir web uygulamasında kullanılmak üzere Düz JavaScript kitaplığı. |
| ![MSAL Açısal](media/sample-v2-code/logo_angular.png) <br/> [MSAL Açısal](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Açısal çerçeve üzerinden oluşturulmuş tek sayfalı uygulamalarda kullanımı kolaylaştırmak için çekirdek MSAL.js kitaplığın sarıcı. Bu kitaplık önizleme de ve bazı Açısal sürümleri ve tarayıcıları ile [bilinen sorunları](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) vardır. |

## <a name="application-code-configuration"></a>Uygulama kodu yapılandırması

Bir MSAL kitaplığında, uygulama kayıt bilgileri kitaplık başlatma sırasında yapılandırma olarak geçirilir.

# <a name="javascript"></a>[Javascript](#tab/javascript)

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
//In app.module.ts
import { MsalModule } from '@azure/msal-angular';

@NgModule({
  imports: [ MsalModule.forRoot({
                clientID: 'your_app_id'
            })]
         })

  export class AppModule { }
```

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oturum açma ve oturumu kapatma](scenario-spa-sign-in.md)
