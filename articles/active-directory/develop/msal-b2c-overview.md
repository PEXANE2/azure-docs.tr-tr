---
title: Azure Active Directory B2CLearn ile MSAL kullanma | Mavisi
titleSuffix: Microsoft identity platform
description: JavaScript (MSAL. js) için Microsoft kimlik doğrulama kitaplığı, uygulamaların, güvenli Web API 'Lerini çağırmak için Azure AD B2C ve belirteçleri almasına olanak sağlar. Bu Web API 'Leri Microsoft Graph, diğer Microsoft API 'Leri, başkalarından Web API 'Leri veya kendi Web API 'niz olabilir.
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
ms.openlocfilehash: 8e076dfd6670265d458eb35d8e1b3e4500009a12
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81534491"
---
# <a name="use-microsoft-authentication-library-for-javascript-to-work-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile çalışmak için JavaScript için Microsoft kimlik doğrulama kitaplığı 'nı kullanın

[JavaScript (msal. js) Için Microsoft kimlik doğrulama kitaplığı](https://github.com/AzureAD/microsoft-authentication-library-for-js) , javascript geliştiricilerinin [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/)kullanarak sosyal ve yerel kimliklere sahip kullanıcıların kimliklerini doğrulamasına olanak sağlar. Azure AD B2C bir kimlik yönetimi hizmeti olarak kullanarak, uygulamalarınızı kullandıklarında müşterilerin nasıl kaydolup oturum açmasını ve profillerini nasıl yönetebileceğinizi denetleyebilir.

Azure AD B2C Ayrıca, müşterilerinize sorunsuz bir deneyim sunmak amacıyla, kimlik doğrulama işlemi sırasında uygulamalarınızın Kullanıcı arabirimini markalamanıza ve özelleştirmenize olanak sağlar.

Bu makalede, Azure AD B2C çalışmak ve farkında olmanız gereken önemli noktaları özetleyen MSAL. js ' nin nasıl kullanılacağı gösterilmektedir. Tüm tartışma ve öğretici için lütfen [Azure AD B2C belgelerine](https://docs.microsoft.com/azure/active-directory-b2c/overview)danışın.

## <a name="prerequisites"></a>Ön koşullar

Kendi [Azure AD B2C kiracınızı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant)önceden oluşturmadıysanız, şimdi bir tane oluşturmaya başlayın (zaten varsa, var olan bir Azure AD B2C kiracısını de kullanabilirsiniz).

Bu gösterim iki bölüm içerir:

- bir Web API 'sini koruma.
- *Web API* 'sinin kimliğini doğrulamak ve çağırmak için tek sayfalı bir uygulamayı kaydetme.

## <a name="nodejs-web-api"></a>Node.js web API’si

> [!NOTE]
> Şu anda, düğüm için MSAL. js hala geliştirme aşamasındadır (bkz. [yol haritası](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)). Bu sırada, Microsoft tarafından geliştirilen ve desteklenen Node. js için bir kimlik doğrulama kitaplığı olan [Passport-Azure-AD](https://github.com/AzureAD/passport-azure-ad)' ı kullanmayı öneririz.

Aşağıdaki adımlarda, bir **Web API 'sinin** kendisini korumak ve seçilen kapsamları bir istemci uygulamasına göstermek için Azure AD B2C nasıl kullanabileceği gösterilmektedir.

### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme

Web API 'nizi Azure AD B2C korumak için önce kaydetmeniz gerekir. Bkz. ayrıntılı adımlar için [Uygulamanızı kaydetme](https://docs.microsoft.com/azure/active-directory-b2c/add-web-application?tabs=applications) .

### <a name="step-2-download-the-sample-application"></a>2. Adım: örnek uygulamayı Indirme

Örneği bir zip dosyası olarak indirin veya GitHub 'dan kopyalayın:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="step-3-configure-authentication"></a>3. Adım: kimlik doğrulamasını yapılandırma

1. Örnekteki `config.js` dosyayı açın.

2. Uygulamanızı kaydederken daha önce edindiğiniz uygulama kimlik bilgileriyle örneği yapılandırın. Değerleri ClientID, Host, Tenantıd ve ilke adı adlarıyla değiştirerek aşağıdaki kod satırlarını değiştirin.

```JavaScript
const clientID = "<Application ID for your Node.js web API - found on Properties page in Azure portal e.g. 93733604-cc77-4a3c-a604-87084dd55348>";
const b2cDomainHost = "<Domain of your B2C host eg. fabrikamb2c.b2clogin.com>";
const tenantId = "<your-tenant-ID>.onmicrosoft.com"; // Alternatively, you can use your Directory (tenant) ID (GUID)
const policyName = "<Name of your sign in / sign up policy, e.g. B2C_1_signupsignin1>";
```

Daha fazla bilgi için bu [Node. js B2C Web API örneğine](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)göz atın.

---

## <a name="javascript-spa"></a>JavaScript SPA

Aşağıdaki adımlarda, **tek sayfalı bir uygulamanın** , oturum açmak, oturum açmak ve korumalı BIR Web API 'si çağırmak için Azure AD B2C nasıl kullanabileceği gösterilmektedir.

### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme

Kimlik doğrulaması uygulamak için öncelikle uygulamanızı kaydetmeniz gerekir. Bkz. ayrıntılı adımlar için [Uygulamanızı kaydetme](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-register-applications) .

### <a name="step-2-download-the-sample-application"></a>2. Adım: örnek uygulamayı Indirme

Örneği bir zip dosyası olarak indirin veya GitHub 'dan kopyalayın:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>3. Adım: kimlik doğrulamasını yapılandırma

Uygulamanızı yapılandırmanın iki ilgisi vardır:

- API uç noktası ve sunulan Kapsamları yapılandırma
- Kimlik doğrulama parametrelerini ve belirteç kapsamlarını yapılandırma

1. Örnekteki `apiConfig.js` dosyayı açın.

2. Örneği, Web API 'nizi kaydederken daha önce edindiğiniz parametrelerle yapılandırın. Aşağıdaki kod satırlarını, değerlerini Web API 'nizin adresiyle ve sunulan kapsamlarla değiştirerek değiştirin.

   ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    const apiConfig = {
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"], //API scopes you exposed during api registration
        webApi: "https://fabrikamb2chello.azurewebsites.net/hello"
    };
   ```

3. Örnekteki `authConfig.js` dosyayı açın.

4. Örneği, daha önce tek sayfalı Uygulamanızı kaydederken elde ettiğiniz parametrelerle yapılandırın. Değerleri ClientID, yetkili meta verileri ve belirteç istek kapsamlarınız ile değiştirerek aşağıdaki kod satırlarını değiştirin.

   ```javascript
    // Config object to be passed to Msal on creation.
    const msalConfig = {
        auth: {
            clientId: "e760cab2-b9a1-4c0d-86fb-ff7084abd902",
            authority: "https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/B2C_1_signupsignin1",
            validateAuthority: false
        },
        cache: {
            cacheLocation: "localStorage", // This configures where your cache will be stored
            storeAuthStateInCookie: false // Set this to "true" to save cache in cookies
        }
    };

    // Add here scopes for id token to be used at the MS Identity Platform endpoint
    const loginRequest = {
        scopes: ["openid", "profile"],
    };
   ```

Daha fazla bilgi için bu [JAVASCRIPT B2C tek sayfalı uygulama örneğine](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)göz atın.

---

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:
- [Kullanıcı akışları](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-user-flows)
- [Özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started)
- [UX özelleştirme](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-configure-user-input)
