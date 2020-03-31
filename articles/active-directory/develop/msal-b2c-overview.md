---
title: Azure Active Directory B2CLearn ile MSAL'ı Kullanın | Azure
titleSuffix: Microsoft identity platform
description: Microsoft Kimlik Doğrulama Kitaplığı (MSAL), uygulamaların Azure AD B2C ile birlikte çalışmasını ve güvenli Web API'lerini aramak için belirteçler edinmesini sağlar. Bu web API'leri Microsoft Graph, diğer Microsoft API'leri, başkalarından gelen web API'leri veya kendi web API'niz olabilir.
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
ms.openlocfilehash: e25564e64410701754390024a5bcfd39321343e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696461"
---
# <a name="use-microsoft-authentication-library-to-interoperate-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile birlikte çalışmak için Microsoft Kimlik Doğrulama Kitaplığını kullanma

Microsoft Kimlik Doğrulama Kitaplığı (MSAL), uygulama geliştiricilerin [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/)kullanarak kullanıcıların sosyal ve yerel kimliklerini doğrulamalarını sağlar. Azure AD B2C bir kimlik yönetimi hizmetidir. Bunu kullanarak, müşterilerin uygulamalarınızı kullanırken profillerini nasıl kaydolduklarını, kaydolabileceğini ve yönetebileceğini özelleştirebilir ve denetleyebilirsiniz.

Azure AD B2C, müşterilerinize sorunsuz bir deneyim sunmak için uygulamalarınızın kullanıcı arabirimi'ni markalamanızı ve özelleştirmenizi de sağlar.

Bu öğretici, Azure AD B2C ile birlikte çalışmak için MSAL'ın nasıl kullanılacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar

Kendi [Azure AD B2C kiracınızı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant)henüz oluşturmadıysanız, şimdi bir tane oluşturun. Ayrıca varolan bir Azure AD B2C kiracısı da kullanabilirsiniz.

## <a name="javascript"></a>JavaScript

Aşağıdaki adımlar, tek sayfalı bir uygulamanın kaydolmak, oturum açmak ve korumalı web API'sini aramak için Azure AD B2C'yi nasıl kullanabileceğini gösterir.

### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme

Kimlik doğrulamasını uygulamak için öncelikle uygulamanızı kaydetmeniz gerekir. Bkz. Ayrıntılı adımlar için [başvurunuzu kaydedin.](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#step-4-register-your-own-web-application-with-azure-ad-b2c)

### <a name="step-2-download-the-sample-application"></a>Adım 2: Örnek uygulamayı indirin

Örneği zip dosyası olarak indirin veya GitHub'dan klonla:

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>Adım 3: Kimlik doğrulamasını yapılandırma

1. Örnekteki **index.html** dosyasını açın.

1. Uygulamanızı kaydederken örneği daha önce kaydettiğiniz istemci kimliği ve anahtarla yapılandırın. Değerleri dizininizin ve API'lerin adlarıyla değiştirerek aşağıdaki kod satırlarını değiştirin:

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

Bu öğreticide [kullanıcı akışının](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) adı **B2C_1_signupsignin1.** Farklı bir kullanıcı akışı adı kullanıyorsanız, **yetki** değerini bu ada ayarlayın.

### <a name="step-4-configure-your-application-to-use-b2clogincom"></a>Adım 4: Uygulamanızı kullanmak üzere yapılandırın`b2clogin.com`

Yönlendirme `b2clogin.com` URL'si `login.microsoftonline.com` olarak yerine kullanabilirsiniz. Kaydolmak ve kaydolmak için bir kimlik sağlayıcısı ayarladığınızda Azure AD B2C uygulamanızda bunu yaparsınız.

`b2clogin.com` Bağlamında kullanımı aşağıdaki `https://your-tenant-name.b2clogin.com/your-tenant-guid` etkilere sahiptir:

- Microsoft hizmetleri çerez üstbilgisinde daha az yer tüketir.
- URL'leriniz artık Microsoft'a başvuruda bulunmaz. Örneğin, Azure AD B2C uygulamanız `login.microsoftonline.com`büyük olasılıkla .

 Kullanmak `b2clogin.com`için, uygulamanızın yapılandırmasını güncelleştirmeniz gerekir.  

- **DoğrulamaYetkisi** özelliğini `false`, yeniden `b2clogin.com` yönlendirmelerin oluşabilmesi için ayarlayın.

Aşağıdaki örnek, özelliği nasıl ayarlayabileceğinizi gösterir:

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
> Azure AD B2C uygulamanız `login.microsoftonline.com` büyük olasılıkla kullanıcı akışı başvurularınız ve belirteç uç noktalarınız gibi çeşitli yerlerde başvurur. Yetkilendirme bitiş noktanızın, belirteç bitiş noktanızın ve vereninin kullanmak `your-tenant-name.b2clogin.com`üzere güncelleştirdiğinden emin olun.

JavaScript (MSAL.js) için MSAL Preview'un nasıl kullanılacağıyla ilgili [bu MSAL JavaScript örneğini](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#single-page-application-built-on-msaljs-with-azure-ad-b2c) izleyin. Örnek bir erişim belirteci alır ve Azure AD B2C tarafından güvenli bir API çağırır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Kullanıcı arabirimini özelleştirme](https://docs.microsoft.com/azure/active-directory-b2c/customize-ui-overview)