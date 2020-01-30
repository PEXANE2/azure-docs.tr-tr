---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 01/23/2020
ms.author: diberry
ms.openlocfilehash: 31280b1efa3db22e8008f401a25ca627dda4153f
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76772445"
---
Python için Language Understanding (LUSıS) tahmini istemci kitaplığını kullanın:

* Yuvaya göre tahmin al
* Sürüme göre tahmin al

[Başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python) | [kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis) | [tahmin çalışma zamanı paketi (Pypı)](https://pypi.org/project/azure-cognitiveservices-language-luis/) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/LUIS)

## <a name="prerequisites"></a>Ön koşullar

* Language Understanding (LUSıS) portal hesabı- [ücretsiz olarak bir tane oluşturun](https://www.luis.ai)
* [Python 3. x](https://www.python.org/)

## <a name="setting-up"></a>Ayarlanıyor

### <a name="get-your-language-understanding-luis-runtime-key"></a>Language Understanding (LUSıS) çalışma zamanı anahtarınızı alın

Bir LUSıS çalışma zamanı kaynağı oluşturarak [çalışma zamanı anahtarınızı](../luis-how-to-azure-subscription.md) alın. Bir sonraki adımda anahtarınızı ve anahtarın uç noktasını saklayın.

[!INCLUDE [Set up environment variables for prediction quickstart](sdk-prediction-environment-variables.md)]

### <a name="create-a-new-python-file"></a>Yeni bir Python dosyası oluştur

`prediction_quickstart.py`adlı tercih ettiğiniz düzenleyicide veya IDE 'de yeni bir Python dosyası oluşturun.

### <a name="install-the-sdk"></a>SDK yükle

Uygulama dizini içinde, aşağıdaki komutla Python için Language Understanding (LUSıS) tahmin çalışma zamanı istemci Kitaplığı ' nı yükler:

```python
python -m pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>Nesne modeli

Language Understanding (LUSıS) tahmini çalışma zamanı istemcisi, kaynak anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisruntimeclient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python) nesnesidir.

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* [Hazırlama veya üretim yuvasına](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-) göre tahmin
* [Sürüme](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-) göre tahmin

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Python için Language Understanding (LUSıS) tahmin çalışma zamanı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [Yuvaya göre tahmin](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, `prediction_quickstart.py` dosyasını tercih ettiğiniz düzenleyicide veya IDE 'de açın. Aşağıdaki bağımlılıkları ekleyin:

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=Dependencies)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

1. Kendi gerekli LUSıS bilgileriniz için değişkenler oluşturun:

    Tahmin anahtarınızı, `LUIS_RUNTIME_KEY`adlı bir ortam değişkeninden çekilen şekilde yönetmek için değişken ekleyin. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, bu değişkeni çalıştıran düzenleyici, IDE veya kabuğun kapatılıp yeniden yüklenmesi gerekir. Yöntemler daha sonra oluşturulacak.

    `LUIS_RUNTIME_ENDPOINT`kaynak adınızı tutacak bir değişken oluşturun.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=AuthorizationVariables)]

1. Uygulama KIMLIĞI için `LUIS_APP_ID`adlı bir ortam değişkeni olarak bir değişken oluşturun. Ortam değişkenini genel IoT uygulaması, **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** olarak ayarlayın. Yayımlanan `production` yuvasını ayarlamak için bir değişken oluşturun.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=OtherVariables)]


1. Anahtarınızla bir kimlik bilgileri nesnesi oluşturun ve bir [LUISRuntimeClientConfiguration]https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python() nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=Client)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin al

İstek tahmin çalışma zamanına oluşturmak için aşağıdaki yöntemi ekleyin.

Kullanıcı söylenişi [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python) nesnesinin bir parçasıdır.

**[Get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** yöntemi, isteği yerine getirmek IÇIN uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyuyor. Verbose gibi diğer seçenekler, tüm hedefleri gösterir ve günlük isteğe bağlıdır. İstek bir [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) nesnesi döndürüyor.

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=predict)]

## <a name="main-code-for-the-prediction"></a>Tahmin için ana kod

Tahmin sağlamak için değişkenleri ve yöntemleri birbirine bağlamak üzere aşağıdaki ana yöntemi kullanın.

```python
predict(luisAppID, luisSlotName)
```
## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama dizininizdeki `python prediction_quickstart.py` komutuyla çalıştırın.

```console
python prediction_quickstart.py
```

Hızlı başlangıç konsolu çıktıyı görüntüler:

```console
Top intent: HomeAutomation.TurnOn
Sentiment: None
Intents:
        "HomeAutomation.TurnOn"
Entities: {'HomeAutomation.Operation': ['on']}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Tahminlerinizi tamamladığınızda, dosyayı ve alt dizinlerini silerek bu hızlı başlangıçta işi temizleyin.
