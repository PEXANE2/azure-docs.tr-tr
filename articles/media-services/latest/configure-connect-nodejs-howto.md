---
title: Azure Media Services v3 API-Node. js ' ye bağlanma
description: Bu makalede, Node. js ile Media Services v3 API 'sine nasıl bağlanacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: juliako
ms.openlocfilehash: 0381a2e2b8fd2a8b60e7cb702e0336a5678df057
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896109"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>Media Services v3 API-Node. js ' ye bağlanma

Bu makalede hizmet sorumlusu oturum açma yöntemi kullanılarak Azure Media Services v3 Node. js SDK 'sına nasıl bağlanabilmeniz gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://nodejs.org/en/download/)’yi yükleyin.
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adını ve Media Services hesap adını unutduğunuzdan emin olun.

> [!IMPORTANT]
> [Adlandırma kurallarını](media-services-apis-overview.md#naming-conventions)gözden geçirin.

## <a name="create-packagejson"></a>Package. JSON oluştur

1. En sevdiğiniz düzenleyiciyi kullanarak bir Package. JSON dosyası oluşturun.
1. Dosyayı açın ve aşağıdaki kodu yapıştırın:

```json
{
  "name": "media-services-node-sample",
  "version": "0.1.0",
  "description": "",
  "main": "./index.js",
  "dependencies": {
    "azure-arm-mediaservices": "^4.1.0",
    "azure-storage": "^2.8.0",
    "ms-rest": "^2.3.3",
    "ms-rest-azure": "^2.5.5"
  }
}
```

Aşağıdaki paketler belirtilmelidir:

|Paket|Açıklama|
|---|---|
|`azure-arm-mediaservices`|SDK Azure Media Services. <br/>En son Azure Media Services paketini kullandığınızdan emin olmak için [NPM 'yi Azure-ARM-mediaservices 'ı yüklemek](https://www.npmjs.com/package/azure-arm-mediaservices/)için denetleyin.|
|`azure-storage`|Depolama SDK 'Sı. Dosyalar varlıklara yüklenirken kullanılır.|
|`ms-rest-azure`| Oturum açmak için kullanılır.|

En son paketi kullandığınızdan emin olmak için aşağıdaki komutu çalıştırabilirsiniz:

```
npm install azure-arm-mediaservices
```

## <a name="connect-to-nodejs-client"></a>Node. js istemcisine bağlanma

1. En sevdiğiniz düzenleyiciyi kullanarak bir. js dosyası oluşturun.
1. Dosyasını açın ve aşağıdaki kodu yapıştırın.
1. "Endpoint config" bölümündeki değerleri, [erişim API 'lerinden](access-api-cli-how-to.md)aldığınız değerlere ayarlayın.

```js
'use strict';

const MediaServices = require('azure-arm-mediaservices');
const msRestAzure = require('ms-rest-azure');
const msRest = require('ms-rest');
const azureStorage = require('azure-storage');

// endpoint config
// make sure your URL values end with '/'
const armAadAudience = "";
const aadEndpoint = "";
const armEndpoint = "";
const subscriptionId = "";
const accountName = "";
const region = "";
const aadClientId = "";
const aadSecret = "";
const aadTenantId = "";
const resourceGroup = "";

let azureMediaServicesClient;

///////////////////////////////////////////
//     Entrypoint for sample script      //
///////////////////////////////////////////

msRestAzure.loginWithServicePrincipalSecret(aadClientId, aadSecret, aadTenantId, {
  environment: {
    activeDirectoryResourceId: armAadAudience,
    resourceManagerEndpointUrl: armEndpoint,
    activeDirectoryEndpointUrl: aadEndpoint
  }
}, async function(err, credentials, subscriptions) {
    if (err) return console.log(err);
    azureMediaServicesClient = new MediaServices(credentials, subscriptionId, armEndpoint, { noRetryPolicy: true });
    
    console.log("connected");

});
```

## <a name="run-your-app"></a>Uygulamanızı çalıştırma

Bir komut istemi açın. Örneğin dizinine gidin ve aşağıdaki komutları yürütün:

```
npm install 
node index.js
```

## <a name="see-also"></a>Ayrıca bkz.

- [Media Services kavramlar](concepts-overview.md)
- [NPM azure-arm-mediaservices yüklemesi](https://www.npmjs.com/package/azure-arm-mediaservices/)

## <a name="next-steps"></a>Sonraki adımlar

Media Services [Node. js başvuru](/javascript/api/overview/azure/mediaservices/management) belgelerini inceleyin ve Node. js ile Media Services API 'sinin nasıl kullanılacağını gösteren [örneklere](https://github.com/Azure-Samples/media-services-v3-node-tutorials) göz atın.

