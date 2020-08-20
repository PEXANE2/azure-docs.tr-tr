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
ms.openlocfilehash: 743b05b38eddc80ce7462a3439613fc767d91daa
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607798"
---
[Başvuru belgeleri](https://docs.microsoft.com/python/api/azure-mgmt-cognitiveservices/azure.mgmt.cognitiveservices?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-mgmt-cognitiveservices)  |  [Paket (Pypı)](https://pypi.org/project/azure-mgmt-cognitiveservices/)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-mgmt-cognitiveservices/tests)

## <a name="prerequisites"></a>Önkoşullar

* Geçerli bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/).
* [Python 3.x](https://www.python.org/)

## <a name="create-an-azure-service-principal"></a>Azure hizmet sorumlusu oluşturma

Uygulamanızın Azure hesabınızla etkileşime geçmesini sağlamak için izinleri yönetmek üzere bir Azure hizmet sorumlusu gerekir. [Azure hizmet sorumlusu oluşturma](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps?view=azps-4.4.0&viewFallbackFrom=azps-3.3.0)bölümündeki yönergeleri izleyin.

Bir hizmet sorumlusu oluşturduğunuzda gizli bir değer, bir KIMLIK ve bir uygulama KIMLIĞI olduğunu görürsünüz. Sonraki adımlar için uygulama KIMLIĞI ve gizli anahtarı geçici bir konuma kaydedin.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bilişsel hizmetler kaynağı oluşturmadan önce, hesabınızın kaynağı içermesi için bir Azure Kaynak grubu olması gerekir. Henüz bir kaynak grubunuz yoksa [Azure Portal](https://ms.portal.azure.com/)bir tane oluşturun.

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

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

### <a name="choose-a-service-and-pricing-tier"></a>Hizmet ve fiyatlandırma katmanı seçin

Yeni bir kaynak oluşturduğunuzda, kullanmak istediğiniz hizmet için "tür" i ve istediğiniz [fiyatlandırma katmanını](https://azure.microsoft.com/pricing/details/cognitive-services/) (veya SKU) birlikte bilmeniz gerekir. Kaynağı oluştururken bu ve diğer bilgileri parametre olarak kullanacaksınız. Aşağıdaki işlev, kullanılabilir bilişsel hizmeti "türlerini" listeler.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

Aşağıdaki SKU 'ların ve fiyatlandırma bilgilerinin listesine bakın. 

#### <a name="multi-service"></a>Çoklu hizmet

| Hizmet                    | Tip                      |
|----------------------------|---------------------------|
| Birden çok hizmet. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/) sayfasına bakın.            | `CognitiveServices`     |


#### <a name="vision"></a>Görsel

| Hizmet                    | Tip                      |
|----------------------------|---------------------------|
| Görüntü İşleme            | `ComputerVision`          |
| Özel Görüntü İşleme tahmin | `CustomVision.Prediction` |
| Özel Görüntü İşleme-eğitim   | `CustomVision.Training`   |
| Yüz                       | `Face`                    |
| Form Tanıma            | `FormRecognizer`          |
| Mürekkep Tanıma             | `InkRecognizer`           |

#### <a name="search"></a>Arama

| Hizmet            | Tip                  |
|--------------------|-----------------------|
| Bing Otomatik Öneri   | `Bing.Autosuggest.v7` |
| Bing Özel Arama | `Bing.CustomSearch`   |
| Bing Varlık Arama | `Bing.EntitySearch`   |
| Bing Arama        | `Bing.Search.v7`      |
| Bing Yazım Denetimi   | `Bing.SpellCheck.v7`  |

#### <a name="speech"></a>Konuşma

| Hizmet            | Tip                 |
|--------------------|----------------------|
| Konuşma Hizmetleri    | `SpeechServices`     |
| Konuşma Tanıma | `SpeakerRecognition` |

#### <a name="language"></a>Dil

| Hizmet            | Tip                |
|--------------------|---------------------|
| Form anlama | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| Soru-Cevap Oluşturucu          | `QnAMaker`          |
| Metin Analizi     | `TextAnalytics`     |
| Metin Çevirisi   | `TextTranslation`   |

#### <a name="decision"></a>Karar

| Hizmet           | Tip               |
|-------------------|--------------------|
| Anomali Algılayıcısı  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Kişiselleştirme      | `Personalizer`     |


#### <a name="pricing-tiers-and-billing"></a>Fiyatlandırma katmanları ve faturalandırma

Fiyatlandırma katmanları (ve faturalandırılan miktar), kimlik doğrulama bilgilerinizi kullanarak göndereceğiniz işlem sayısına bağlıdır. Her fiyatlandırma katmanı şunları belirtir:
* saniye başına izin verilen en fazla işlem sayısı (TPS).
* fiyatlandırma katmanında etkinleştirilen hizmet özellikleri.
* önceden tanımlı işlem sayısı için maliyet. Bu sayının üzerine gitmek, hizmetinize ilişkin [fiyatlandırma ayrıntılarında](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) belirtilen ek ücret oluşmasına neden olur.

> [!NOTE]
> Bilişsel hizmetlerin birçoğu hizmeti denemek için kullanabileceğiniz ücretsiz bir katmana sahiptir. Ücretsiz katmanı kullanmak için, `F0` kaynağınız IÇIN SKU olarak kullanın.

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

Yeni bir bilişsel hizmetler kaynağı oluşturup abone olmak için **Create** işlevini kullanın. Bu işlev, geçirdiğiniz kaynak grubuna yeni bir faturalanabilir kaynak ekler. Yeni kaynağınızı oluşturduğunuzda, kullanmak istediğiniz hizmetin "tür" değerini, fiyatlandırma katmanını (veya SKU 'SU) ve bir Azure konumunu bilmeniz gerekir. Aşağıdaki işlev bu bağımsız değişkenlerin tümünü alır ve bir kaynak oluşturur.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_create)]

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

* [Azure Yönetim SDK 'Sı başvuru belgeleri](https://docs.microsoft.com/python/api/azure-mgmt-cognitiveservices/azure.mgmt.cognitiveservices?view=azure-python)
* [Azure bilişsel hizmetler nedir?](../../Welcome.md)
* [Azure bilişsel hizmetler 'e yönelik isteklerin kimliğini doğrulama](../../authentication.md)
* [Azure portalını kullanarak yeni kaynak oluşturma](../../cognitive-services-apis-create-account.md)