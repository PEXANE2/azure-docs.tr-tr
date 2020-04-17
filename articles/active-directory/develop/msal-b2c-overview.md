---
title: Azure Active Directory B2CLearn ile MSAL'ı Kullanın | Azure
titleSuffix: Microsoft identity platform
description: JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js), uygulamaların Azure AD B2C ile çalışmasını ve güvenli web API'lerini aramak için belirteçler edinmesini sağlar. Bu web API'leri Microsoft Graph, diğer Microsoft API'leri, başkalarından gelen web API'leri veya kendi web API'niz olabilir.
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
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81534491"
---
# <a name="use-microsoft-authentication-library-for-javascript-to-work-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile çalışmak için JavaScript için Microsoft Kimlik Doğrulama Kitaplığını kullanma

[JavaScript için Microsoft Kimlik Doğrulama Kitaplığı (MSAL.js),](https://github.com/AzureAD/microsoft-authentication-library-for-js) JavaScript geliştiricilerinin Azure [Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/)kullanarak kullanıcıların sosyal ve yerel kimliklerini doğrulamasını sağlar. Azure AD B2C'yi kimlik yönetimi hizmeti olarak kullanarak, müşterilerin uygulamalarınızı kullanırken profillerini nasıl kaydolduklarını, kaydolabileceğini ve yönetebileceğini özelleştirebilir ve denetleyebilirsiniz.

Azure AD B2C, müşterilerinize sorunsuz bir deneyim sunmak için kimlik doğrulama işlemi sırasında uygulamalarınızın kullanıcı arabirimi'ni markalamanızı ve özelleştirmenizi de sağlar.

Bu makalede, Azure AD B2C ile çalışmak için MSAL.js'nin nasıl kullanılacağı gösterilebilir ve dikkat edilmesi gereken önemli noktaları özetleyebilirsiniz. Tam bir tartışma ve öğretici için lütfen [Azure AD B2C Belgeleri'ne](https://docs.microsoft.com/azure/active-directory-b2c/overview)başvurun.

## <a name="prerequisites"></a>Ön koşullar

Kendi [Azure AD B2C kiracınızı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant)henüz oluşturmadıysanız, şimdi bir tane oluşturmaya başlayın (zaten varsa varolan bir Azure AD B2C kiracınızı da kullanabilirsiniz).

Bu gösteri iki bölümden oluşur:

- nasıl bir web API korumak için.
- kimlik doğrulaması ve web API aramak için tek sayfalık bir uygulama nın nasıl *kaydedilen.*

## <a name="nodejs-web-api"></a>Node.js web API’si

> [!NOTE]
> Şu anda, Düğüm için MSAL.js hala geliştirme aşamasındadır [(yol haritasına](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki#roadmap)bakın). Bu arada, Microsoft tarafından geliştirilen ve desteklenen Node.js için bir kimlik doğrulama kitaplığı olan [passport-azure-ad'ı](https://github.com/AzureAD/passport-azure-ad)kullanmanızı öneririz.

Aşağıdaki adımlar, bir **web API'sinin** kendisini korumak ve seçili kapsamları istemci uygulamasına maruz bırakmak için Azure AD B2C'yi nasıl kullanabileceğini gösterir.

### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme

Web API'nizi Azure AD B2C ile korumak için öncelikle bu api'yi kaydetmeniz gerekir. Bkz. Ayrıntılı adımlar için [başvurunuzu kaydedin.](https://docs.microsoft.com/azure/active-directory-b2c/add-web-application?tabs=applications)

### <a name="step-2-download-the-sample-application"></a>Adım 2: Örnek uygulamayı indirin

Örneği zip dosyası olarak indirin veya GitHub'dan klonla:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

### <a name="step-3-configure-authentication"></a>Adım 3: Kimlik doğrulamasını yapılandırma

1. Örnekteki `config.js` dosyayı açın.

2. Örneği, başvurunuzu kaydederken daha önce elde ettiğiniz uygulama kimlik bilgileriyle yapılandırın. Değerleri müşteri kimliğiniz, ana bilgisayar, kiracı kimliği ve ilke adı adlarıyla değiştirerek aşağıdaki kod satırlarını değiştirin.

```JavaScript
const clientID = "<Application ID for your Node.js web API - found on Properties page in Azure portal e.g. 93733604-cc77-4a3c-a604-87084dd55348>";
const b2cDomainHost = "<Domain of your B2C host eg. fabrikamb2c.b2clogin.com>";
const tenantId = "<your-tenant-ID>.onmicrosoft.com"; // Alternatively, you can use your Directory (tenant) ID (GUID)
const policyName = "<Name of your sign in / sign up policy, e.g. B2C_1_signupsignin1>";
```

Daha fazla bilgi için, bu [Node.js B2C web API örnek](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)göz atın.

---

## <a name="javascript-spa"></a>JavaScript SPA

Aşağıdaki adımlar, **tek sayfalı** bir uygulamanın kaydolmak, oturum açmak ve korumalı web API'sini aramak için Azure AD B2C'yi nasıl kullanabileceğini gösterir.

### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme

Kimlik doğrulamasını uygulamak için öncelikle uygulamanızı kaydetmeniz gerekir. Bkz. Ayrıntılı adımlar için [başvurunuzu kaydedin.](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-register-applications)

### <a name="step-2-download-the-sample-application"></a>Adım 2: Örnek uygulamayı indirin

Örneği zip dosyası olarak indirin veya GitHub'dan klonla:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>Adım 3: Kimlik doğrulamasını yapılandırma

Uygulamanızı yapılandırmada iki önemli nokta vardır:

- API uç noktasını ve açıkta kalan kapsamları yapılandırma
- Kimlik doğrulama parametrelerini ve belirteç kapsamlarını yapılandırma

1. Örnekteki `apiConfig.js` dosyayı açın.

2. Web API'nizi kaydederken örneği daha önce elde ettiğiniz parametrelerle yapılandırın. Değerleri web API'nizin ve açığa çıkarılan kapsamların adresiyle değiştirerek aşağıdaki kod satırlarını değiştirin.

   ```javascript
    // The current application coordinates were pre-registered in a B2C tenant.
    const apiConfig = {
        b2cScopes: ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"], //API scopes you exposed during api registration
        webApi: "https://fabrikamb2chello.azurewebsites.net/hello"
    };
   ```

3. Örnekteki `authConfig.js` dosyayı açın.

4. Tek sayfalı uygulamanızı kaydederken örneği daha önce elde ettiğiniz parametrelerle yapılandırın. Değerleri ClientId, yetki meta verileri ve belirteç isteği kapsamlarınızla değiştirerek aşağıdaki kod satırlarını değiştirin.

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

Daha fazla bilgi için, bu [JavaScript B2C tek sayfalık uygulama örneğine](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)göz atın.

---

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:
- [Kullanıcı akışları](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-user-flows)
- [Özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-get-started)
- [UX özelleştirme](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-configure-user-input)
