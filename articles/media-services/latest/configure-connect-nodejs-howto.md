---
title: Azure Media Services v3 API 'sine bağlanma-Node.js
description: Bu makalede, Node.js ile Media Services v3 API 'sine nasıl bağlanacağı gösterilmektedir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 02/17/2021
ms.author: inhenkel
ms.custom: devx-track-js
ms.openlocfilehash: 9628e46281e267bd1c1fd8277a3a975bc338c6dc
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "101096055"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>Media Services v3 API 'sine bağlanma-Node.js

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makalede hizmet sorumlusu oturum açma yöntemi kullanılarak Azure Media Services v3 node.js SDK 'sına nasıl bağlanabilmeniz gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

- [Node.js](https://nodejs.org/en/download/)'i yükler.
- [TypeScript](https://www.typescriptlang.org/download)'i yükler.
- [Media Services hesabı oluşturun](./create-account-howto.md). Kaynak grubu adını ve Media Services hesap adını unutduğunuzdan emin olun.

> [!IMPORTANT]
> Varlıklarda önemli adlandırma kısıtlamalarını anlamak için Azure Media Services [adlandırma kurallarını](media-services-apis-overview.md#naming-conventions) gözden geçirin. 

## <a name="reference-documentation-for-azurearm-mediaservices"></a>İçin başvuru belgeleri @Azure/arm-mediaservices
- [Node.jsiçin Azure Media Services modülleriyle ilgili başvuru belgeleri ](https://docs.microsoft.com/javascript/api/overview/azure/media-services?view=azure-node-latest)

## <a name="more-developer-documentation-for-nodejs-on-azure"></a>Azure 'da Node.js için daha fazla geliştirici belgesi
- [JavaScript & için Azure Node.js geliştiriciler](https://docs.microsoft.com/azure/developer/javascript/?view=azure-node-latest)
- [Git hub deposunda Media Services kaynak kodu @azure/azure-sdk-for-js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/mediaservices/arm-mediaservices)
- [Node.js geliştiricileri için Azure paket belgeleri](https://docs.microsoft.com/javascript/api/overview/azure/?view=azure-node-latest)

## <a name="install-the-packages"></a>Paketleri yükler

1. En sevdiğiniz düzenleyiciyi kullanarak dosyada package.jsoluşturun.
1. Dosyayı açın ve aşağıdaki kodu yapıştırın:

   [JavaScript Için Azudüzeltici Aservices SDK 'sının](https://www.npmjs.com/package/@azure/arm-mediaservices)en son sürümünü aldığınızdan emin olun.

```json
{
  "name": "media-services-node-sample",
  "version": "0.1.0",
  "description": "",
  "main": "./index.ts",
  "dependencies": {
    "@azure/arm-mediaservices": "^8.0.0",
    "@azure/abort-controller": "^1.0.2",
    "@azure/ms-rest-nodeauth": "^3.0.6",
    "@azure/storage-blob": "^12.4.0",
  }
}
```

Aşağıdaki paketler belirtilmelidir:

|Paket|Description|
|---|---|
|`@azure/arm-mediaservices`|SDK Azure Media Services. <br/>En son Azure Media Services paketini kullandığınızdan emin olmak için [NPM yüklemesini @azure/arm-mediaservices ](https://www.npmjs.com/package/@azure/arm-mediaservices)denetleyin.|
|`@azure/ms-rest-nodeauth` | Hizmet sorumlusu veya yönetilen kimlik kullanılarak AAD kimlik doğrulaması için gereklidir|
|`@azure/storage-blob`|Depolama SDK 'Sı. Dosyalar varlıklara yüklenirken kullanılır.|
|`@azure/ms-rest-js`| Oturum açmak için kullanılır.|
|`@azure/storage-blob` | Kodlama için Azure Media Services içindeki varlıklara dosya yüklemek ve bunları indirmek için kullanılır.|
|`@azure/abort-controller`| Uzun süre çalışan indirme işlemleri için depolama istemcisiyle birlikte kullanılır|


En son paketi kullandığınızdan emin olmak için aşağıdaki komutu çalıştırabilirsiniz:

### <a name="install-azurearm-mediaservices"></a>Yükleyeceğiniz @azure/arm-mediaservices
```
npm install @azure/arm-mediaservices
```

### <a name="install-azurems-rest-nodeauth"></a>Yükleyeceğiniz @azure/ms-rest-nodeauth

Lütfen en düşük sürüm olan " @azure/ms-rest-nodeauth ": "^ 3.0.0" sürümünü yüklemelisiniz.

```
npm install @azure/ms-rest-nodeauth@"^3.0.0"
```

## <a name="connect-to-nodejs-client-using-typescript"></a>TypeScript kullanarak Node.js istemciye bağlanma

1. En sevdiğiniz düzenleyiciyi kullanarak bir TypeScript. ts dosyası oluşturun.
1. Dosyayı açın ve aşağıdaki kodu yapıştırın.
1. Bir. env dosyası oluşturun ve Azure portal ayrıntıları doldurun. Bkz. [erişim API 'leri](./access-api-howto.md).

### <a name="sample-env-file"></a>Sample. env dosyası
```
# copy the content of this file to a file named ".env". It should be stored at the root of the repo.
# The values can be obtained from the API Access page for your Media Services account in the portal.
AZURE_CLIENT_ID=""
AZURE_CLIENT_SECRET= ""
AZURE_TENANT_ID= ""

# Change this to match your AAD Tenant domain name. 
AAD_TENANT_DOMAIN = "microsoft.onmicrosoft.com"

# Set this to your Media Services Account name, resource group it is contained in, and location
AZURE_MEDIA_ACCOUNT_NAME = ""
AZURE_LOCATION= ""
AZURE_RESOURCE_GROUP= ""

# Set this to your Azure Subscription ID
AZURE_SUBSCRIPTION_ID= ""

# You must change these if you are using Gov Cloud, China, or other non standard cloud regions
AZURE_ARM_AUDIENCE= "https://management.core.windows.net"
AZURE_ARM_ENDPOINT="https://management.azure.com"

# DRM Testing
DRM_SYMMETRIC_KEY="add random base 64 encoded string here"
```

## <a name="typescript---hello-world---list-assets"></a>TypeScript-Merhaba Dünya listesi varlıkları
Bu örnek, bir hizmet sorumlusu ile Media Services istemcisine bağlanmayı ve hesaptaki varlıkları listelemenizi gösterir. Yeni bir hesap kullanıyorsanız, liste tekrar boş olur. Sonuçları görmek için portalda birkaç varlığı karşıya yükleyebilirsiniz.

```ts
import * as msRestNodeAuth from "@azure/ms-rest-nodeauth";
import { AzureMediaServices } from '@azure/arm-mediaservices';
import { AzureMediaServicesOptions } from "@azure/arm-mediaservices/esm/models";
// Load the .env file if it exists
import * as dotenv from "dotenv";
dotenv.config();

export async function main() {
  // Copy the samples.env file and rename it to .env first, then populate it's values with the values obtained 
  // from your Media Services account's API Access page in the Azure portal.
  const clientId = process.env.AZURE_CLIENT_ID as string;
  const secret = process.env.AZURE_CLIENT_SECRET as string;
  const tenantDomain = process.env.AAD_TENANT_DOMAIN as string;
  const subscriptionId = process.env.AZURE_SUBSCRIPTION_ID as string;
  const resourceGroup = process.env.AZURE_RESOURCE_GROUP as string;
  const accountName = process.env.AZURE_MEDIA_ACCOUNT_NAME as string;

  let clientOptions: AzureMediaServicesOptions = {
    longRunningOperationRetryTimeout: 5, // set the time out for retries to 5 seconds
    noRetryPolicy: false // use the default retry policy.
  }

  const creds = await msRestNodeAuth.loginWithServicePrincipalSecret(clientId, secret, tenantDomain);
  const mediaClient = new AzureMediaServices(creds, subscriptionId, clientOptions);

  // List Assets in Account
  console.log("Listing Assets Names in account:")
  var assets = await mediaClient.assets.list(resourceGroup, accountName);

  assets.forEach(asset => {
    console.log(asset.name);
  });

  if (assets.odatanextLink) {
    console.log("There are more than 1000 assets in this account, use the assets.listNext() method to continue listing more assets if needed")
    console.log("For example:  assets = await mediaClient.assets.listNext(assets.odatanextLink)");
  }
}

main().catch((err) => {
  console.error("Error running sample:", err.message);
});
```

## <a name="run-the-sample-application-helloworld-listassets"></a>Örnek uygulamayı çalıştırın HelloWorld-ListAssets

Node.js örnekleri için depoyu kopyalayın

```git
git clone https://github.com/Azure-Samples/media-services-v3-node-tutorials.git
```

Dizini AMSv3Samples klasörüne değiştirme
```bash
cd AMSv3Samples
```

Üzerinde packages.jskullanılan paketleri yükler
```
npm install 
```

Dizini HelloWorld-ListAssets klasöre Değiştir
```bash
cd HelloWorld-ListAssets
```

AMSv3Samples klasöründen Visual Studio Code başlatın. Bu, ". vscode" klasörünün ve dosyalardaki tsconfig.jsbulunduğu klasörden başlatmak için gereklidir
```dotnetcli
cd ..
code .
```

HelloWorld-ListAssets klasörünü açın ve index. TS dosyasını Visual Studio Code düzenleyicisinde açın.
İndex. TS dosyasında, hata ayıklayıcıyı başlatmak için F5 tuşuna basın. Hesapta zaten bir varlık varsa, görünen varlıkların bir listesini görmeniz gerekir. Hesap boşsa boş bir liste görürsünüz.  Sonuçları görmek için portalda birkaç varlığı karşıya yükleyin.

## <a name="more-samples"></a>Daha fazla örnek

[Depoda](https://github.com/Azure-Samples/media-services-v3-node-tutorials) aşağıdaki örnekler mevcuttur

|Proje adı|Kullanım Örneği|
|---|---|
|Canlı/dizin. TS| Temel canlı akış örneği. **Uyarı**, canlı kullanırken tüm kaynakların temizlenip artık portalda faturalandırılırsınız emin olun|
|Streamfilesörnek/dizin. TS| Kaynak URL 'den yerel bir dosya veya kodlama yüklemeye yönelik temel örnek. Örnek, içerik indirmek için depolama SDK 'sının nasıl kullanılacağını gösterir ve bir oyuncunun nasıl akışa alınacağını gösterir |
|StreamFilesWithDRMSample/index. TS| Widevine ve PlayReady DRM kullanarak kodlama ve akışın nasıl yapılacağını gösterir |
|VideoIndexerSample/index. TS| Video ve ses çözümleyici ön ayarlarını kullanarak video veya ses dosyasından meta veriler ve Öngörüler oluşturma örneği |

## <a name="see-also"></a>Ayrıca bkz.

- [Media Services kavramlar](concepts-overview.md)
- [NPM yüklemesi @azure/arm-mediaservices](https://www.npmjs.com/package/@azure/arm-mediaservices)
- [JavaScript & için Azure Node.js geliştiriciler](https://docs.microsoft.com/azure/developer/javascript/?view=azure-node-latest)
- [Depodaki Media Services kaynak kodu @azure/azure-sdk-for-js](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/mediaservices/arm-mediaservices)

## <a name="next-steps"></a>Sonraki adımlar

Media Services [Node.js başvuru](/javascript/api/overview/azure/mediaservices/management) belgelerini inceleyin ve node.js ile Media Services API 'sinin nasıl kullanılacağını gösteren [örneklere](https://github.com/Azure-Samples/media-services-v3-node-tutorials) göz atın.
