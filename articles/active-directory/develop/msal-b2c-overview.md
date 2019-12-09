---
title: Azure Active Directory B2CLearn ile MSAL kullanma | Mavisi
titleSuffix: Microsoft identity platform
description: Microsoft kimlik doğrulama kitaplığı (MSAL), uygulamaların, güvenli Web API 'Lerini çağırmak için Azure AD B2C birlikte çalışabilmesine ve belirteçleri almasına olanak sağlar. Bu Web API 'Leri Microsoft Graph, diğer Microsoft API 'Leri, başkalarından Web API 'Leri veya kendi Web API 'niz olabilir.
services: active-directory
author: negoe
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/16/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f080c14cd0aa20bd312b4be8d9eacd8d901b7cef
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74917038"
---
# <a name="use-microsoft-authentication-library-to-interoperate-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile birlikte çalışmak için Microsoft kimlik doğrulama kitaplığı 'nı kullanın

Microsoft kimlik doğrulama kitaplığı (MSAL), uygulama geliştiricilerinin [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/)kullanarak sosyal ve yerel kimliklere sahip kullanıcıların kimliğini doğrulamasını sağlar. Azure AD B2C bir kimlik yönetimi hizmetidir. Bu uygulamayı kullanarak, uygulamalarınızı kullandıklarında müşterilerin nasıl kaydolup oturum açmasını ve profillerini nasıl yönetebileceğinizi denetleyebilir.

Azure AD B2C Ayrıca, müşterilerinize sorunsuz bir deneyim sunmak için uygulamalarınızın Kullanıcı arabirimini markalamanıza ve özelleştirmenize olanak sağlar.

Bu öğreticide, Azure AD B2C ile birlikte çalışmak için MSAL kullanımı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Kendi [Azure AD B2C kiracınızı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant)önceden oluşturmadıysanız, şimdi bir tane oluşturun. Ayrıca, mevcut bir Azure AD B2C kiracısını de kullanabilirsiniz.

## <a name="javascript"></a>JavaScript

Aşağıdaki adımlarda, tek sayfalı bir uygulamanın, oturum açmak, oturum açmak ve korumalı bir Web API 'SI çağırmak için Azure AD B2C nasıl kullanabileceği gösterilmektedir.

### <a name="step-1-register-your-application"></a>1\. Adım: Uygulamanızı kaydetme

Kimlik doğrulaması uygulamak için öncelikle uygulamanızı kaydetmeniz gerekir. Bkz. ayrıntılı adımlar için [Uygulamanızı kaydetme](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#step-4-register-your-own-web-application-with-azure-ad-b2c) .

### <a name="step-2-download-the-sample-application"></a>2\. Adım: örnek uygulamayı Indirme

Örneği bir zip dosyası olarak indirin veya GitHub 'dan kopyalayın:

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>3\. Adım: kimlik doğrulamasını yapılandırma

1. Örnekteki **index. html** dosyasını açın.

1. Uygulamanızı kaydederken daha önce kaydettiğiniz istemci KIMLIĞI ve anahtarı ile örneği yapılandırın. Aşağıdaki kod satırlarını, değerlerini dizininizin ve API 'lerinizin adlarıyla değiştirerek değiştirin:

   ```javascript
   // The current application coordinates were pre-registered in a B2C tenant.

    var appConfig = {
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"],
        webApi: "https://fabrikamb2chello.azurewebsites.net/hello"
    };

    const msalConfig = {
        auth: {
            clientId: "e760cab2-b9a1-4c0d-86fb-ff7084abd902" //This is your client/application ID
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_susi", //This is your tenant info
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage",
            storeAuthStateInCookie: true
        }
    };
    // create UserAgentApplication instance
    const myMSALObj = new Msal.UserAgentApplication(msalConfig);

   ```

Bu öğreticideki [Kullanıcı akışının](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) adı **B2C_1_signupsignin1**. Farklı bir Kullanıcı akış adı kullanıyorsanız, **yetkili** değerini bu ad olarak ayarlayın.

### <a name="step-4-configure-your-application-to-use-b2clogincom"></a>4\. Adım: Uygulamanızı kullanacak şekilde yapılandırma `b2clogin.com`

`b2clogin.com` yeniden yönlendirme URL 'SI olarak `login.microsoftonline.com` yerine kullanabilirsiniz. Kaydolma ve oturum açma için bir kimlik sağlayıcısı ayarlarken Azure AD B2C uygulamanızda bunu yapabilirsiniz.

`https://your-tenant-name.b2clogin.com/your-tenant-guid` bağlamında `b2clogin.com` kullanımı aşağıdaki etkilere sahiptir:

- Microsoft Hizmetleri, tanımlama bilgisi üstbilgisinde daha az alan tüketir.
- URL 'niz artık Microsoft 'a bir başvuru içermiyor. Örneğin, Azure AD B2C uygulamanız muhtemelen `login.microsoftonline.com`anlamına gelir.

 `b2clogin.com`kullanmak için uygulamanızın yapılandırmasını güncelleştirmeniz gerekir.  

- `b2clogin.com` kullanarak yeniden yönlendirmeler gerçekleşebilmesi için **Validateauthority** özelliğini `false`olarak ayarlayın.

Aşağıdaki örnek, özelliği nasıl ayarlayabileceğini göstermektedir:

```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
        validateAuthority: false;

};
// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

> [!NOTE]
> Azure AD B2C uygulamanız muhtemelen Kullanıcı akışı başvuruları ve belirteç uç noktaları gibi birkaç yerde `login.microsoftonline.com` anlamına gelir. Yetkilendirme uç noktanızın, belirteç uç noktasının ve verenin `your-tenant-name.b2clogin.com`kullanacak şekilde güncelleştirildiğinden emin olun.

JavaScript (MSAL. js) için MSAL önizlemesini nasıl kullanacağınızı öğrenmek için [Bu msal JavaScript örneğini](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#single-page-application-built-on-msaljs-with-azure-ad-b2c) izleyin. Örnek, bir erişim belirteci alır ve Azure AD B2C tarafından güvenliği sağlanmış bir API çağırır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Kullanıcı arabirimini özelleştirme](https://docs.microsoft.com/azure/active-directory-b2c/customize-ui-overview)