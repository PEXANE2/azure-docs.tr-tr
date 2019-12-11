---
title: Tek sayfalı uygulamayı yapılandırma-Microsoft Identity platform | Mavisi
description: Tek sayfalı bir uygulama (uygulamanın kod yapılandırması) oluşturmayı öğrenin
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
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 111f8d45fced1ed0e9293c874d7a56c64e9fb0e4
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965424"
---
# <a name="single-page-application-code-configuration"></a>Tek sayfalı uygulama: kod yapılandırma

Tek sayfalı uygulamanız (SPA) için kodu yapılandırmayı öğrenin.

## <a name="msal-libraries-that-support-implicit-flow"></a>Örtük akışı destekleyen MSAL kitaplıkları

Microsoft Identity platformu, sektörde önerilen güvenlik uygulamalarını kullanarak örtük akışı desteklemek için aşağıdaki Microsoft kimlik doğrulama kitaplığı (MSAL) kitaplıklarını sağlar:  

| MSAL kitaplığı | Açıklama |
|--------------|--------------|
| ![MSAL. js](media/sample-v2-code/logo_js.png) <br/> [MSAL. js](https://github.com/AzureAD/microsoft-authentication-library-for-js)  | JavaScript veya angular, Vue. js ve yanıt verme. js gibi SPA çerçeveleri aracılığıyla oluşturulmuş herhangi bir istemci tarafı Web uygulamasında kullanmak için düz JavaScript kitaplığı. |
| ![MSAL angular](media/sample-v2-code/logo_angular.png) <br/> [MSAL angular](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular/README.md) | Angular çerçevesi aracılığıyla oluşturulan tek sayfalı uygulamalarda kullanımı basitleştirecek Core MSAL. js kitaplığı sarmalayıcısı. Bu kitaplık önizlemededir ve belirli angular sürümlerinde ve tarayıcılarında [bilinen sorunlar](https://github.com/AzureAD/microsoft-authentication-library-for-js/issues?q=is%3Aopen+is%3Aissue+label%3Aangular) vardır. |

## <a name="application-code-configuration"></a>Uygulama kodu yapılandırması

Bir MSAL kitaplığında, kitaplık başlatma sırasında uygulama kayıt bilgileri yapılandırma olarak geçirilir.

### <a name="javascript"></a>JavaScript

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
Yapılandırılabilir seçenekler hakkında daha fazla bilgi için bkz. [msal. js ile uygulamayı başlatma](msal-js-initializing-client-applications.md).

### <a name="angular"></a>Angular

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Oturum açma ve oturum kapatma](scenario-spa-sign-in.md)
