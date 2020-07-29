---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 07/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: db866da43310f5407ce4daae1cade2c7512b91ea
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369286"
---
Python için Language Understanding (LUSıS) tahmini istemci kitaplığını kullanın:

* Yuvaya göre tahmin al
* Sürüme göre tahmin al

[Başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis)  |  [Tahmin çalışma zamanı paketi (Pypı)](https://pypi.org/project/azure-cognitiveservices-language-luis/)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py)

## <a name="prerequisites"></a>Önkoşullar

* Language Understanding (LUSıS) portal hesabı- [ücretsiz olarak bir tane oluşturun](https://www.luis.ai)
* [Python 3.x](https://www.python.org/)
* Bir LUSıS uygulama KIMLIĞI-genel IoT uygulama KIMLIĞI ' ni kullanın `df67dcdb-c37d-46af-88e1-8b97951ca1c2` . Hızlı başlangıç kodunda kullanılan Kullanıcı sorgusu bu uygulamaya özeldir.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="get-your-language-understanding-luis-runtime-key"></a>Language Understanding (LUSıS) çalışma zamanı anahtarınızı alın

Bir LUSıS çalışma zamanı kaynağı oluşturarak [çalışma zamanı anahtarınızı](../luis-how-to-azure-subscription.md) alın. Bir sonraki adımda anahtarınızı ve anahtarın uç noktasını saklayın.

### <a name="create-a-new-python-file"></a>Yeni bir Python dosyası oluştur

Tercih ettiğiniz düzenleyicide veya IDE 'de adlı yeni bir Python dosyası oluşturun `prediction_quickstart.py` .

### <a name="install-the-sdk"></a>SDK Yükleme

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

Proje dizininden `prediction_quickstart.py` dosyayı tercih ettiğiniz düzenleyicide veya IDE 'de açın. Aşağıdaki bağımlılıkları ekleyin:

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=Dependencies)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

1. Kendi gerekli LUSıS bilgileriniz için değişkenler oluşturun: tahmin anahtarınız ve uç noktanız.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=AuthorizationVariables)]

1. Genel IoT uygulaması olarak ayarlanan uygulama KIMLIĞI için bir değişken oluşturun **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** . Yayınlanan yuvayı ayarlamak için bir değişken oluşturun `production` .

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=OtherVariables)]


1. Anahtarınızla bir kimlik bilgileri nesnesi oluşturun ve bir [Luisruntimeclientconfiguration](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=Client)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin al

İstek tahmin çalışma zamanına oluşturmak için aşağıdaki yöntemi ekleyin.

Kullanıcı söylenişi [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python) nesnesinin bir parçasıdır.

**[Get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** yöntemi, isteği yerine getirmek IÇIN uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyuyor. Verbose gibi diğer seçenekler, tüm hedefleri gösterir ve günlük isteğe bağlıdır. İstek bir [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) nesnesi döndürüyor.

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=predict)]

## <a name="main-code-for-the-prediction"></a>Tahmin için ana kod

Tahmin sağlamak için değişkenleri ve yöntemleri birbirine bağlamak üzere aşağıdaki ana yöntemi kullanın.

```python
predict(luisAppID, luisSlotName)
```
## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `python prediction_quickstart.py` uygulama dizininizdeki komutla çalıştırın.

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
