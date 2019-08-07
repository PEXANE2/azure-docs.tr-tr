---
title: Uygulamaların Microsoft kimlik doğrulama kitaplığı 'nı kullanarak Azure AD B2C ile nasıl birlikte çalışabilme hakkında bilgi edinin
description: Microsoft kimlik doğrulama kitaplığı (MSAL), uygulamaların, güvenli Web API 'Lerini çağırmak için Azure AD B2C birlikte çalışabilmesine ve belirteçleri almasına olanak sağlar. Bu Web API 'Leri Microsoft Graph, diğer Microsoft API 'Leri, başkalarından Web API 'Leri veya kendi Web API 'niz olabilir.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 784ec507027d6ec0ac1b5288c101e2a76cab436e
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835061"
---
# <a name="use-microsoft-authentication-library-to-interoperate-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile birlikte çalışmak için Microsoft kimlik doğrulama kitaplığı 'nı kullanın

Microsoft kimlik doğrulama kitaplığı (MSAL), uygulama geliştiricilerinin [Azure Active Directory B2C (Azure AD B2C)](https://docs.microsoft.com/azure/active-directory-b2c/)kullanarak sosyal ve yerel kimliklere sahip kullanıcıların kimliğini doğrulamasını sağlar. Azure AD B2C bir kimlik yönetimi hizmetidir. Bu uygulamayı kullanarak, uygulamalarınızı kullandıklarında müşterilerin nasıl kaydolup oturum açmasını ve profillerini nasıl yönetebileceğinizi denetleyebilir.

Azure AD B2C Ayrıca, müşterilerinize sorunsuz bir deneyim sunmak için uygulamalarınızın Kullanıcı arabirimini markalamanıza ve özelleştirmenize olanak sağlar.

Bu öğreticide, Azure AD B2C ile birlikte çalışmak için MSAL kullanımı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Kendi [Azure AD B2C kiracınızı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant)önceden oluşturmadıysanız, şimdi bir tane oluşturun. Ayrıca, mevcut bir Azure AD B2C kiracısını de kullanabilirsiniz.

## <a name="javascript"></a>JavaScript

Aşağıdaki adımlarda, tek sayfalı bir uygulamanın, oturum açmak, oturum açmak ve korumalı bir Web API 'SI çağırmak için Azure AD B2C nasıl kullanabileceği gösterilmektedir.

### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydedin

Kimlik doğrulaması uygulamak için öncelikle uygulamanızı kaydetmeniz gerekir. Bkz. ayrıntılı adımlar için [Uygulamanızı kaydetme](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#step-4-register-your-own-web-application-with-azure-ad-b2c) .

### <a name="step-2-download-the-sample-application"></a>2\. adım: Örnek uygulamayı indirin:

Örneği bir zip dosyası olarak indirin veya GitHub 'dan kopyalayın:

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git
```

### <a name="step-3-configure-authentication"></a>3\. adım: Kimlik doğrulamasını Yapılandır

1. Örnekteki **index. html** dosyasını açın.

1. Uygulamanızı kaydederken daha önce kaydettiğiniz uygulama KIMLIĞI ve anahtarı ile örneği yapılandırın. Aşağıdaki kod satırlarını, değerlerini dizininizin ve API 'lerinizin adlarıyla değiştirerek değiştirin:

   ```javascript
   // The current application coordinates were pre-registered in a B2C directory.

   const msalConfig = {
       auth:{
           clientId: "Enter_the_Application_Id_here",
           authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>",
           b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/hello/demo.read"],
           webApi: 'http://localhost:5000/hello',
     };

   // create UserAgentApplication instance
   const myMSALObj = new UserAgentApplication(msalConfig);
   ```

Bu öğreticideki [Kullanıcı akışının](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) adı **B2C_1_signupsignin1**' dir. Farklı bir Kullanıcı akış adı kullanıyorsanız, **yetkili** değerini bu ad olarak ayarlayın.

### <a name="step-4-configure-your-application-to-use-b2clogincom"></a>4\. Adım: Uygulamanızı kullanacak şekilde yapılandırma`b2clogin.com`

' In `b2clogin.com` `login.microsoftonline.com` yerine yeniden yönlendirme URL 'si olarak kullanabilirsiniz. Kaydolma ve oturum açma için bir kimlik sağlayıcısı ayarlarken Azure AD B2C uygulamanızda bunu yapabilirsiniz.

Bağlamında öğesinin `b2clogin.com` kullanımı aşağıdaki etkilere sahiptir:`https://your-tenant-name.b2clogin.com/your-tenant-guid`

- Microsoft Hizmetleri, tanımlama bilgisi üstbilgisinde daha az alan tüketir.
- URL 'niz artık Microsoft 'a bir başvuru içermiyor. Örneğin, Azure AD B2C uygulamanız muhtemelen öğesine `login.microsoftonline.com`başvurur.

 Kullanmak `b2clogin.com`için uygulamanızın yapılandırmasını güncelleştirmeniz gerekir.  

- Kullanarak `false` yenidenyönlendirmelergerçekleşebilmesiiçinvalidateauthorityözelliğiniolarak`b2clogin.com` ayarlayın.

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
> Azure AD B2C uygulamanız muhtemelen Kullanıcı akışı başvuruları `login.microsoftonline.com` ve belirteç uç noktaları gibi birkaç yerde anlamına gelir. Yetkilendirme uç noktanızın, belirtecin bitiş noktasının ve verenin kullanılmak `your-tenant-name.b2clogin.com`üzere güncelleştirildiğinden emin olun.

JavaScript (MSAL. js) için MSAL önizlemesini nasıl kullanacağınızı öğrenmek için [Bu msal JavaScript örneğini](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#single-page-application-built-on-msaljs-with-azure-ad-b2c) izleyin. Örnek, bir erişim belirteci alır ve Azure AD B2C tarafından güvenliği sağlanmış bir API çağırır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi:

- [Özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Kullanıcı arabirimi özelleştirmesi](https://docs.microsoft.com/azure/active-directory-b2c/customize-ui-overview)