---
title: 'Hızlı başlangıç: Node.js için Azure Yönetim istemci kitaplığı'
description: Bu hızlı başlangıçta Node.js için Azure Yönetim istemci kitaplığı 'nı kullanmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: fcfefb2b6cd5c23e9b77d7d27413d1ff6d775bfe
ms.sourcegitcommit: 5ed504a9ddfbd69d4f2d256ec431e634eb38813e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89321628"
---
[Başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/arm-cognitiveservices/?view=azure-node-latest)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/arm-cognitiveservices)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices#sample-code)

## <a name="prerequisites"></a>Ön koşullar

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).
* [Node.js](https://nodejs.org/) geçerli sürümü

[!INCLUDE [Create a service principal](./create-service-principal.md)]

[!INCLUDE [Create a resource group](./create-resource-group.md)]

## <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Konsol penceresinde (cmd, PowerShell veya Bash gibi), uygulamanız için yeni bir dizin oluşturun ve bu uygulamaya gidin. 

```console
mkdir myapp && cd myapp
```

`npm init`Bir dosya ile bir düğüm uygulaması oluşturmak için komutunu çalıştırın `package.json` . 

```console
npm init
```

Devam etmeden önce _index.js_ adlı bir dosya oluşturun.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

Aşağıdaki NPM paketlerini yükler:

```console
npm install @azure/arm-cognitiveservices
npm install @azure/ms-rest-js
npm install @azure/ms-rest-nodeauth
```

Uygulamanızın `package.json` dosyası bağımlılıklarla güncelleştirilir.

### <a name="import-libraries"></a>Kitaplıkları içeri aktarma

_index.js_ betiğinizi açın ve aşağıdaki kitaplıkları içeri aktarın.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_imports)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Aşağıdaki alanları, betiğinizin köküne ekleyin ve oluşturduğunuz hizmet sorumlusunu ve Azure hesap bilgilerinizi kullanarak değerlerini girin.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_constants)]

Sonra, `quickstart` programınızın ana işini işlemek için aşağıdaki işlevi ekleyin. İlk kod bloğu, yukarıda girdiğiniz kimlik bilgisi değişkenlerini kullanarak bir **Biliveservicesmanagementclient** nesnesi oluşturur. Bu nesne, tüm Azure yönetim işlemleri için gereklidir.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_main_auth)]

## <a name="call-management-functions"></a>Arama yönetimi işlevleri

`quickstart`Kullanılabilir kaynakları listelemek için işlevinizin sonuna aşağıdaki kodu ekleyin, örnek bir kaynak oluşturun, sahip olduğunuz kaynakları listeleyin ve ardından örnek kaynağı silin. Sonraki adımlarda bu işlevleri tanımlayacaksınız.

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

### <a name="choose-a-service-and-pricing-tier"></a>Hizmet ve fiyatlandırma katmanı seçin

Yeni bir kaynak oluşturduğunuzda, kullanmak istediğiniz hizmet için "tür" i ve istediğiniz [fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/cognitive-services/) (veya SKU) birlikte bilmeniz gerekir. Kaynağı oluştururken bu ve diğer bilgileri parametre olarak kullanacaksınız. Aşağıdaki işlev, kullanılabilir bilişsel hizmeti "türlerini" listeler.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

[!INCLUDE [SKUs and pricing](./sku-pricing.md)]

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

Yeni bir bilişsel hizmetler kaynağı oluşturup abone olmak için **Create** işlevini kullanın. Bu işlev, geçirdiğiniz kaynak grubuna yeni bir faturalanabilir kaynak ekler. Yeni kaynağınızı oluşturduğunuzda, kullanmak istediğiniz hizmetin "tür" değerini, fiyatlandırma katmanını (veya SKU 'SU) ve bir Azure konumunu bilmeniz gerekir. Aşağıdaki işlev bu bağımsız değişkenlerin tümünü alır ve bir kaynak oluşturur.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_create)]

## <a name="view-your-resources"></a>Kaynaklarınızı görüntüleyin

Azure hesabınızın altındaki tüm kaynakları (tüm kaynak grupları arasında) görüntülemek için aşağıdaki işlevi kullanın:

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list)]

## <a name="delete-a-resource"></a>Kaynak silme

Aşağıdaki işlev belirtilen kaynağı verilen kaynak grubundan siler.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_delete)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Ana işlevinizi hata işleme ile çağırmak için betiğinizin altına aşağıdaki kodu ekleyin `quickstart` .

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_main)]

Ardından, konsol pencerenize komutuyla uygulamayı çalıştırın `node` .

```console
node index.js
```

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Yönetim SDK 'Sı başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/arm-cognitiveservices/?view=azure-node-latest)
* [Azure bilişsel hizmetler nedir?](../../Welcome.md)
* [Azure bilişsel hizmetler 'e yönelik isteklerin kimliğini doğrulama](../../authentication.md)
* [Azure portalını kullanarak yeni kaynak oluşturma](../../cognitive-services-apis-create-account.md)