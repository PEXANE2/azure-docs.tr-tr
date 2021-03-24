---
title: 'Hızlı başlangıç: Node.js için Azure Yönetim istemci kitaplığı'
description: Bu hızlı başlangıçta Node.js için Azure Yönetim istemci kitaplığı 'nı kullanmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 3/22/2021
ms.author: pafarley
ms.openlocfilehash: 41f6c8e260968eacd04249b3f887d4865907df0d
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104879575"
---
[Başvuru belgeleri](/javascript/api/@azure/arm-cognitiveservices/)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices)  |  [Paket (NPM)](https://www.npmjs.com/package/@azure/arm-cognitiveservices)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/arm-cognitiveservices#sample-code)

## <a name="javascript-prerequisites"></a>JavaScript önkoşulları

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

## <a name="create-a-cognitive-services-resource-nodejs"></a>Bilişsel hizmetler kaynağı oluşturma (Node.js)

Yeni bir bilişsel hizmetler kaynağı oluşturup abone olmak için **Create** işlevini kullanın. Bu işlev, geçirdiğiniz kaynak grubuna yeni bir faturalanabilir kaynak ekler. Yeni kaynağınızı oluşturduğunuzda, kullanmak istediğiniz hizmetin "tür" değerini, fiyatlandırma katmanını (veya SKU 'SU) ve bir Azure konumunu bilmeniz gerekir. Aşağıdaki işlev bu bağımsız değişkenlerin tümünü alır ve bir kaynak oluşturur.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_create)]

### <a name="choose-a-service-and-pricing-tier"></a>Hizmet ve fiyatlandırma katmanı seçin

Yeni bir kaynak oluşturduğunuzda, kullanmak istediğiniz hizmet için "tür" i ve istediğiniz [fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/cognitive-services/) (veya SKU) birlikte bilmeniz gerekir. Kaynağı oluştururken bu ve diğer bilgileri parametre olarak kullanacaksınız. Aşağıdaki işlev, kullanılabilir bilişsel hizmeti "türlerini" listeler.

[!code-javascript[](~/cognitive-services-quickstart-code/javascript/azure_management_service/create_delete_resource.js?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

[!INCLUDE [SKUs and pricing](./sku-pricing.md)]

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

* Bilişsel hizmetlerle güvenli bir şekilde çalışmak için bkz. Azure bilişsel **[Hizmetler 'e yönelik kimlik doğrulama istekleri](../../authentication.md)** .
* Bilişsel hizmetler 'deki farklı kategorilerin listesini almak için bkz. **[Azure bilişsel hizmetler nedir?](../../what-are-cognitive-services.md)** .
* Bilişsel hizmetlerin desteklediği doğal dillerin listesini görmek için bkz. **[doğal dil desteği](../../language-support.md)** .
* Bilişsel hizmetler 'in şirket içi hizmetlerini nasıl kullanacağınızı anlamak için bkz. bilişsel **[Hizmetler 'i kapsayıcı olarak kullanma](../../cognitive-services-container-support.md)** .
* Bilişsel hizmetler 'in kullanım maliyetini tahmin etmek için bilişsel **[Hizmetler 'in maliyetlerini planlayın ve yönetin](../../plan-manage-costs.md)** bölümüne bakın.
* Yönetim SDK 'Sı hakkında daha fazla bilgi için bkz. **[Azure YÖNETIM SDK başvuru belgeleri](/javascript/api/@azure/arm-cognitiveservices/)** .
