---
title: 'Hızlı başlangıç: Python için Azure Yönetim istemci kitaplığı'
description: Bu hızlı başlangıçta, Python için Azure Yönetim istemci kitaplığı ile çalışmaya başlayın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/05/2020
ms.author: pafarley
ms.openlocfilehash: fb908cdcf3e235654effc043de29e599a48179d4
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104879653"
---
[Başvuru belgeleri](/python/api/azure-mgmt-cognitiveservices/azure.mgmt.cognitiveservices)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-mgmt-cognitiveservices)  |  [Paket (Pypı)](https://pypi.org/project/azure-mgmt-cognitiveservices/)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-mgmt-cognitiveservices/tests)

## <a name="python-prerequisites"></a>Python önkoşulları

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).
* [Python 3.x](https://www.python.org/)

[!INCLUDE [Create a service principal](./create-service-principal.md)]

[!INCLUDE [Create a resource group](./create-resource-group.md)]

## <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir Python uygulaması oluşturun ve bir konsol penceresinde projenize gidin.

### <a name="install-the-client-library"></a>İstemci kitaplığını yükler

İstemci kitaplığını ile yükleyebilirsiniz:

```console
pip install azure-mgmt-cognitiveservices
```

Visual Studio IDE kullanıyorsanız, istemci kitaplığı indirilebilir bir NuGet paketi olarak kullanılabilir.

### <a name="import-libraries"></a>Kitaplıkları içeri aktarma

Python betiğinizi açın ve aşağıdaki kitaplıkları içeri aktarın.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_imports)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Aşağıdaki alanları, betiğinizin köküne ekleyin ve oluşturduğunuz hizmet sorumlusunu ve Azure hesap bilgilerinizi kullanarak değerlerini girin.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_constants)]

Daha sonra bir **Biliveservicesmanagementclient** nesnesi oluşturmak için aşağıdaki kodu ekleyin. Bu nesne, tüm Azure yönetim işlemleri için gereklidir.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_auth)]

## <a name="create-a-cognitive-services-resource-python"></a>Bilişsel hizmetler kaynağı oluşturma (Python)

Yeni bir bilişsel hizmetler kaynağı oluşturup abone olmak için **Create** işlevini kullanın. Bu işlev, geçirdiğiniz kaynak grubuna yeni bir faturalanabilir kaynak ekler. Yeni kaynağınızı oluşturduğunuzda, kullanmak istediğiniz hizmetin "tür" değerini, fiyatlandırma katmanını (veya SKU 'SU) ve bir Azure konumunu bilmeniz gerekir. Aşağıdaki işlev bu bağımsız değişkenlerin tümünü alır ve bir kaynak oluşturur.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_create)]

### <a name="choose-a-service-and-pricing-tier"></a>Hizmet ve fiyatlandırma katmanı seçin

Yeni bir kaynak oluşturduğunuzda, kullanmak istediğiniz hizmet için "tür" i ve istediğiniz [fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/cognitive-services/) (veya SKU) birlikte bilmeniz gerekir. Kaynağı oluştururken bu ve diğer bilgileri parametre olarak kullanacaksınız. Aşağıdaki işlev, kullanılabilir bilişsel hizmeti "türlerini" listeler.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

[!INCLUDE [SKUs and pricing](./sku-pricing.md)]

## <a name="view-your-resources"></a>Kaynaklarınızı görüntüleyin

Azure hesabınızın altındaki tüm kaynakları (tüm kaynak grupları arasında) görüntülemek için aşağıdaki işlevi kullanın:

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_list)]

## <a name="delete-a-resource"></a>Kaynak silme

Aşağıdaki işlev belirtilen kaynağı verilen kaynak grubundan siler.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_delete)]

## <a name="call-management-functions"></a>Arama yönetimi işlevleri

Yukarıdaki işlevleri çağırmak için betiğinizin en altına aşağıdaki kodu ekleyin. Bu kod kullanılabilir kaynakları listeler, örnek bir kaynak oluşturur, sahip olduğunuz kaynakları listeler ve ardından örnek kaynağı siler.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_calls)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Komutu ile komut satırından uygulamanızı çalıştırın `python` .

```console
python <your-script-name>.py
```

## <a name="see-also"></a>Ayrıca bkz.

* Bilişsel hizmetlerle güvenli bir şekilde çalışmak için bkz. Azure bilişsel **[Hizmetler 'e yönelik kimlik doğrulama istekleri](../../authentication.md)** .
* Bilişsel hizmetler 'deki farklı kategorilerin listesini almak için bkz. **[Azure bilişsel hizmetler nedir?](../../what-are-cognitive-services.md)** .
* Bilişsel hizmetlerin desteklediği doğal dillerin listesini görmek için bkz. **[doğal dil desteği](../../language-support.md)** .
* Bilişsel hizmetler 'in şirket içi hizmetlerini nasıl kullanacağınızı anlamak için bkz. bilişsel **[Hizmetler 'i kapsayıcı olarak kullanma](../../cognitive-services-container-support.md)** .
* Bilişsel hizmetler 'in kullanım maliyetini tahmin etmek için bilişsel **[Hizmetler 'in maliyetlerini planlayın ve yönetin](../../plan-manage-costs.md)** bölümüne bakın.
* Yönetim SDK 'Sı hakkında daha fazla bilgi için bkz. **[Azure YÖNETIM SDK başvuru belgeleri](/python/api/azure-mgmt-cognitiveservices/azure.mgmt.cognitiveservices)** .
