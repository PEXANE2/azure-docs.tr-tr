---
title: Azure Medya Hizmetleri v3 API'ye bağlanın - Düğüm.js
description: Bu makalede, Node.js ile Media Services v3 API'ye nasıl bağlanılacağın gösterildiği gösterilmiştir.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74896109"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>Medya Hizmetlerine bağlan v3 API - Düğüm.js

Bu makalede, hizmet ana oturum açma yöntemini kullanarak Azure Media Services v3 düğümü.js SDK'ya nasıl bağlanabileceğinizi gösterir.

## <a name="prerequisites"></a>Ön koşullar

- [Node.js](https://nodejs.org/en/download/)yükleyin.
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md) Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun.

> [!IMPORTANT]
> Adlandırma kurallarını gözden [geçirin.](media-services-apis-overview.md#naming-conventions)

## <a name="create-packagejson"></a>package.json oluştur

1. En sevdiğiniz düzenleyiciyi kullanarak bir package.json dosyası oluşturun.
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
|`azure-arm-mediaservices`|Azure Medya Hizmetleri SDK. <br/>En son Azure Medya Hizmetleri paketini kullandığınızdan emin olmak için [NPM'nin azure-arm-mediaservices yükle'sini](https://www.npmjs.com/package/azure-arm-mediaservices/)kontrol edin.|
|`azure-storage`|Depolama SDK. Varlıklara dosya yüklerken kullanılır.|
|`ms-rest-azure`| Oturum vermek için kullanılır.|

En son paketi kullandığınızdan emin olmak için aşağıdaki komutu çalıştırabilirsiniz:

```
npm install azure-arm-mediaservices
```

## <a name="connect-to-nodejs-client"></a>Düğüm.js istemcisine bağlan

1. En sevdiğiniz düzenleyiciyi kullanarak bir .js dosyası oluşturun.
1. Dosyayı açın ve aşağıdaki kodu yapıştırın.
1. "Uç nokta config" bölümündeki değerleri [erişim API'lerinden](access-api-cli-how-to.md)aldığınız değerlere ayarlayın.

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

Bir komut istemi açın. Örneğin dizinine göz atın ve aşağıdaki komutları uygulayın:

```
npm install 
node index.js
```

## <a name="see-also"></a>Ayrıca bkz.

- [Medya Hizmetleri kavramları](concepts-overview.md)
- [NPM azure-arm-mediaservices yüklemesi](https://www.npmjs.com/package/azure-arm-mediaservices/)

## <a name="next-steps"></a>Sonraki adımlar

Medya Hizmetleri [Node.js ref](/javascript/api/overview/azure/mediaservices/management) belgelerini keşfedin ve Media Services API'nin node.js ile nasıl kullanılacağını gösteren [örneklere](https://github.com/Azure-Samples/media-services-v3-node-tutorials) göz atın.

