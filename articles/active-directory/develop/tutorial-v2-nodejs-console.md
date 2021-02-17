---
title: 'Öğretici: Node.js konsol uygulamasında Microsoft Graph çağırma | Mavisi'
titleSuffix: Microsoft identity platform
description: Bu öğreticide, bir Node.js konsol uygulamasına Microsoft Graph çağırmak için bir konsol uygulaması oluşturacaksınız.
services: active-directory
author: derisen
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.date: 01/12/2021
ms.author: v-doeris
ms.openlocfilehash: 3d4211acbf6b65ef8f04d00b3936d70bb930ed9e
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100562158"
---
# <a name="tutorial-call-the-microsoft-graph-api-in-a-nodejs-console-app"></a>Öğretici: Node.js konsol uygulamasında Microsoft Graph API 'sini çağırma

Bu öğreticide, kendi kimliğini kullanarak Microsoft Graph API çağıran bir konsol uygulaması oluşturacaksınız. Oluşturduğunuz konsol uygulaması, [Node.jsIçin Microsoft kimlik doğrulama kitaplığı 'nı (msal) ](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-node)kullanır.

Bu öğreticideki adımları izleyerek şunları yapın:

> [!div class="checklist"]
> - Uygulamayı Azure portal kaydetme
> - Node.js konsol uygulaması projesi oluşturma
> - Uygulamanıza kimlik doğrulama mantığı ekleyin
> - Uygulama Kayıt ayrıntıları ekleme
> - Web API 'SI çağırmak için bir yöntem ekleme
> - Uygulamayı test etme

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://nodejs.org/en/download/)
- [Visual Studio Code](https://code.visualstudio.com/download) veya başka bir kod Düzenleyicisi

## <a name="register-the-application"></a>Uygulamayı kaydetme

İlk olarak, uygulamanızı kaydetmek için [Microsoft Identity platformu ile uygulama kaydetme](quickstart-register-app.md) bölümündeki adımları uygulayın.

Uygulama kaydınız için aşağıdaki ayarları kullanın:

- Ad: `NodeConsoleApp` (önerilen)
- Desteklenen hesap türleri: **yalnızca bu kuruluş dizinindeki hesaplar**
- API izinleri:   >    >  **Uygulama izinleri** Microsoft Graph Microsoft API 'leri > `User.Read.All`
- İstemci parolası: `*********` (Bu değeri sonraki bir adımda kullanmak üzere Kaydet-yalnızca bir kez gösteriliyor)

## <a name="create-the-project"></a>Proje oluşturma

Uygulamanızı barındırmak için bir klasör oluşturun, örneğin *Nodeconsoleapp*.

1. İlk olarak, terminalinizdeki proje dizininize geçin ve aşağıdaki NPM komutlarını çalıştırın:

```console
    npm init -y
    npm install --save dotenv yargs axios @azure/msal-node
```

2. Sonra, *bin* adlı bir klasör oluşturun. Ardından, bu klasörün içinde *index.js* adlı dosya oluşturun ve aşağıdaki kodu ekleyin:

```JavaScript
#!/usr/bin/env node

// read in env settings
require('dotenv').config();

const yargs = require('yargs');

const fetch = require('./fetch');
const auth = require('./auth');

const options = yargs
    .usage('Usage: --op <operation_name>')
    .option('op', { alias: 'operation', describe: 'operation name', type: 'string', demandOption: true })
    .argv;

async function main() {
    console.log(`You have selected: ${options.op}`);

    switch (yargs.argv['op']) {
        case 'getUsers':

            try {
                // here we get an access token
                const authResponse = await auth.getToken(auth.tokenRequest);

                // call the web API with the access token
                const users = await fetch.callApi(auth.apiConfig.uri, authResponse.accessToken);

                // display result
                console.log(users);
            } catch (error) {
                console.log(error);
            }

            break;
        default:
            console.log('Select a Graph operation first');
            break;
    }
};

main();
```

Bu dosya, diğer iki düğüm modüllerine başvurur: erişim belirteçlerini almak için MSAL düğümünün bir uygulamasını içeren *auth.js* ve bir ERIŞIM belirteciyle API Microsoft Graph IÇIN bir http isteği oluşturmak üzere bir yöntemi içeren *fetch.js* . Öğreticinin geri kalanını tamamladıktan sonra, projenizin dosya ve klasör yapısı şuna benzer olmalıdır:

```
NodeConsoleApp/
├── bin
│   ├── auth.js
│   ├── fetch.js
│   ├── index.js
├── package.json
└── .env
```

## <a name="add-authentication-logic"></a>Kimlik doğrulama mantığı Ekle

*Bin* klasörünün içinde, *auth.js* adlı başka bir dosya oluşturun ve Microsoft Graph API çağrılırken sunmak üzere bir erişim belirteci edinmek için aşağıdaki kodu ekleyin.

```JavaScript
const msal = require('@azure/msal-node');

/**
 * Configuration object to be passed to MSAL instance on creation. 
 * For a full list of MSAL Node configuration parameters, visit:
 * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/configuration.md 
 */
const msalConfig = {
    auth: {
        clientId: process.env.CLIENT_ID,
        authority: process.env.AAD_ENDPOINT + process.env.TENANT_ID,
        clientSecret: process.env.CLIENT_SECRET,
    }
};

/**
 * With client credentials flows permissions need to be granted in the portal by a tenant administrator.
 * The scope is always in the format '<resource>/.default'. For more, visit: 
 * https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow 
 */
const tokenRequest = {
    scopes: [process.env.GRAPH_ENDPOINT + '.default'],
};

const apiConfig = {
    uri: process.env.GRAPH_ENDPOINT + 'v1.0/users',
};

/**
 * Initialize a confidential client application. For more info, visit:
 * https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md
 */
const cca = new msal.ConfidentialClientApplication(msalConfig);

/**
 * Acquires token with client credentials.
 * @param {object} tokenRequest 
 */
async function getToken(tokenRequest) {
    return await cca.acquireTokenByClientCredential(tokenRequest);
}

module.exports = {
    apiConfig: apiConfig,
    tokenRequest: tokenRequest,
    getToken: getToken
};
```

Yukarıdaki kod parçacığında, ilk olarak bir yapılandırma nesnesi (*Msalconfig*) OLUŞTURACAĞıZ ve msal [ConfidentialClientApplication](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md)başlatmak üzere geçiyoruz. Daha sonra, **istemci kimlik bilgileri** aracılığıyla belirteçleri almak için bir yöntem oluşturacağız ve son olarak bu modüle *main.js* tarafından erişilmek üzere kullanıma sunun. Bu modüldeki yapılandırma parametreleri bir sonraki adımda oluşturduğumuz bir ortam dosyasından çizilir.

## <a name="add-app-registration-details"></a>Uygulama Kayıt ayrıntıları ekleme

Belirteçleri alırken kullanılacak uygulama kaydı ayrıntılarını depolamak için bir ortam dosyası oluşturun. Bunu yapmak için, örneğin kök klasöründe (*Nodeconsoleapp*) *. env* adlı bir dosya oluşturun ve aşağıdaki kodu ekleyin:

```
# Credentials
TENANT_ID=Enter_the_Tenant_Id_Here
CLIENT_ID=Enter_the_Application_Id_Here
CLIENT_SECRET=Enter_the_Client_Secret_Here

# Endpoints
AAD_ENDPOINT=Enter_the_Cloud_Instance_Id_Here
GRAPH_ENDPOINT=Enter_the_Graph_Endpoint_Here
```

Bu ayrıntıları Azure uygulama kayıt portalından elde ettiğiniz değerlerle birlikte girin:

- `Enter_the_Tenant_Id_here` aşağıdakilerden biri olmalıdır:
  - Uygulamanız *bu kuruluş dizinindeki hesapları* destekliyorsa, bu DEĞERI **Kiracı kimliği** veya **kiracı adı** ile değiştirin. Örneğin, `contoso.microsoft.com`.
  - Uygulamanız *herhangi bir kuruluş dizinindeki hesapları* destekliyorsa, bu değeri ile değiştirin `organizations` .
  - Uygulamanız *herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesapları* destekliyorsa, bu değeri ile değiştirin `common` .
  - *Yalnızca kişisel Microsoft hesaplarına* yönelik desteği kısıtlamak için bu değeri ile değiştirin `consumers` .
- `Enter_the_Application_Id_Here`: Kaydettiğiniz uygulamanın **uygulama (istemci) kimliği** .
- `Enter_the_Cloud_Instance_Id_Here`: Uygulamanızın kaydedildiği Azure bulut örneği.
  - Ana (veya *küresel*) Azure bulutu için girin `https://login.microsoftonline.com` .
  - **Ulusal** bulutlar için (örneğin, Çin), [Ulusal bulutlarda](authentication-national-cloud.md)uygun değerleri bulabilirsiniz.
- `Enter_the_Graph_Endpoint_Here` , uygulamanın iletişim kurması gereken Microsoft Graph API örneğidir.
  - **Genel** Microsoft Graph API uç noktası için bu dizenin her iki örneğini ile değiştirin `https://graph.microsoft.com` .
  - **Ulusal** bulut dağıtımlarındaki uç noktalar için Microsoft Graph belgelerindeki [Ulusal bulut dağıtımları](/graph/deployments) bölümüne bakın.

## <a name="add-a-method-to-call-a-web-api"></a>Web API 'SI çağırmak için bir yöntem ekleme

*Bin* klasörünün içinde, *fetch.js* adlı başka bir dosya oluşturun ve Microsoft Graph API 'sine Rest çağrıları yapmak için aşağıdaki kodu ekleyin:

```javascript
const axios = require('axios');

/**
 * Calls the endpoint with authorization bearer token.
 * @param {string} endpoint 
 * @param {string} accessToken 
 */
async function callApi(endpoint, accessToken) {

    const options = {
        headers: {
            Authorization: `Bearer ${accessToken}`
        }
    };

    console.log('request made to web API at: ' + new Date().toString());

    try {
        const response = await axios.default.get(endpoint, options);
        return response.data;
    } catch (error) {
        console.log(error)
        return error;
    }
};

module.exports = {
    callApi: callApi
};
```

Burada, `callApi` yöntemi, `GET` erişim belirteci gerektiren korumalı bir kaynağa karşı http isteği oluşturmak için kullanılır. İstek daha sonra içeriği çağırana döndürür. Bu yöntem, alınan belirteci *http yetkilendirme üst bilgisine* ekler. Burada korunan kaynak, bu uygulamanın kaydedildiği Kiracıdaki kullanıcıları görüntüleyen Microsoft Graph API [kullanıcıları uç noktasıdır](https://docs.microsoft.com/graph/api/user-list) .

## <a name="test-the-app"></a>Uygulamayı test etme

Uygulamanın oluşturulmasını tamamladınız ve artık uygulamanın işlevlerini test etmeye hazır.

Proje klasörünüzün kökünün içinden aşağıdaki komutu çalıştırarak Node.js konsol uygulamasını başlatın:

```console
node . --op getUsers
```

Bu, Microsoft Graph API 'den bazı JSON yanıtına neden olmalıdır ve konsolda bir kullanıcı nesneleri dizisi görmeniz gerekir:

```console
You have selected: getUsers
request made to web API at: Fri Jan 22 2021 09:31:52 GMT-0800 (Pacific Standard Time)
{
    '@odata.context': 'https://graph.microsoft.com/v1.0/$metadata#users',
    value: [
        {
            displayName: 'Adele Vance'
            givenName: 'Adele',
            jobTitle: 'Retail Manager',
            mail: 'AdeleV@msaltestingjs.onmicrosoft.com',
            mobilePhone: null,
            officeLocation: '18/2111',
            preferredLanguage: 'en-US',
            surname: 'Vance',
            userPrincipalName: 'AdeleV@msaltestingjs.onmicrosoft.com',
            id: 'a6a218a5-f5ae-462a-acd3-581af4bcca00'
        }
    ]
}
```
:::image type="content" source="media/tutorial-v2-nodejs-console/screenshot.png" alt-text="Graph yanıtını görüntüleyen komut satırı arabirimi":::

## <a name="how-the-application-works"></a>Uygulamanın nasıl çalıştığı

Bu uygulama, [OAuth 2,0 istemci kimlik bilgileri verme](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)kullanır. Kullanıcının anında etkileşime geçmesi gerekmeyen ve arka planda çalışması gereken sunucular arası etkileşimler için genellikle bu izin türü kullanılır. Kimlik bilgileri verme akışı, bir Web hizmetinin (gizli istemci), bir kullanıcının kimliğine bürünmek yerine, başka bir Web hizmetini çağırırken kimlik doğrulaması yerine kendi kimlik bilgilerini kullanmasına izin verir. Bu kimlik doğrulama modeliyle desteklenen uygulamaların türü genellikle **Daemon 'ları** veya **hizmet hesaplarıdır**.

İstemci kimlik bilgisi akışı için istenen kapsam, kaynağın ve sonrasında gelen addır `/.default` . Bu gösterim Azure Active Directory (Azure AD) uygulama kaydı sırasında statik olarak belirtilen uygulama düzeyi izinleri kullanmasını söyler. Ayrıca, bu API izinlerinin bir **Kiracı Yöneticisi** tarafından verilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Identity platformunda Node.js konsol uygulaması geliştirmeyi daha ayrıntılı bir şekilde incelemek istiyorsanız, bkz. çok parçalı senaryo serimiz:

> [!div class="nextstepaction"]
> [Senaryo: Daemon uygulaması](scenario-daemon-overview.md)
