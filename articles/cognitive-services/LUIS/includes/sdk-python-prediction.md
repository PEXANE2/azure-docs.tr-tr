---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: ff4c33aea3d3ce604f44c38e6e3856242388b0e9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77371851"
---
Python için Dil Anlama (LUIS) tahmin istemci kitaplığını kullanın:

* Yuvaya göre tahmin alma
* Sürüme göre tahmin alma

[Referans belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis) | [Tahmin çalışma zamanı Paketi (PyPi)](https://pypi.org/project/azure-cognitiveservices-language-luis/)  |  [ Örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/LUIS)

## <a name="prerequisites"></a>Ön koşullar

* Dil Anlama (LUIS) portal hesabı - [Ücretsiz bir tane oluşturun](https://www.luis.ai)
* [Python 3.x](https://www.python.org/)

## <a name="setting-up"></a>Ayarlama

### <a name="get-your-language-understanding-luis-runtime-key"></a>Dil Anlayışınızı (LUIS) çalışma zamanı anahtarınızı alın

Luis çalışma zamanı kaynağı oluşturarak [çalışma zamanı anahtarınızı](../luis-how-to-azure-subscription.md) alın. Anahtarınızı ve bir sonraki adım için anahtarın bitiş noktasını tutun.

[!INCLUDE [Set up environment variables for prediction quickstart](sdk-prediction-environment-variables.md)]

### <a name="create-a-new-python-file"></a>Yeni bir python dosyası oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de `prediction_quickstart.py`yeni bir python dosyası oluşturun.

### <a name="install-the-sdk"></a>SDK yükle

Uygulama dizininde Python için Dil Anlama (LUIS) tahmin çalışma zamanı istemci kitaplığını aşağıdaki komutla yükleyin:

```python
python -m pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>Nesne modeli

Dil Bilgisi (LUIS) tahmin çalışma zamanı istemcisi, kaynak anahtarınızı içeren Azure'a kimlik doğrulayan bir [LUISRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python) nesnesidir.

İstemci oluşturulduktan sonra, aşağıdakiler de dahil olmak üzere işlevsellik erişmek için bu istemciyi kullanın:

* [Evreleme veya üretim yuvası](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-) ile tahmin
* [Sürüme](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-) göre tahmin

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları Python için Dil Anlama (LUIS) tahmin runtime istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Yuvaya göre tahmin](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, tercih `prediction_quickstart.py` ettiğiniz düzenleyici veya IDE'deki dosyayı açın. Aşağıdaki bağımlılıkları ekleyin:

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=Dependencies)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

1. Kendi gerekli LUIS bilgileriniz için değişkenler oluşturun:

    Tahmin anahtarınızı yönetmek için değişkenler ekleyin. `LUIS_RUNTIME_KEY` Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, düzenleyicinin, IDE'nin veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir. Yöntemler daha sonra oluşturulacaktır.

    Kaynak adınızı `LUIS_RUNTIME_ENDPOINT`tutmak için bir değişken oluşturun.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=AuthorizationVariables)]

1. Uygulama kimliği için bir değişken oluşturun. `LUIS_APP_ID` Ortam değişkenini genel IoT **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** uygulamasına ayarlayın. Yayımlanan yuvayı `production` ayarlamak için bir değişken oluşturun.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=OtherVariables)]


1. Anahtarınızla bir kimlik bilgileri nesnesi oluşturun ve [LUISRuntimeClientConfiguration]https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python() nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=Client)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin alma

İstek tahmin çalışma süresi oluşturmak için aşağıdaki yöntemi ekleyin.

Kullanıcı söyleyiş [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python) nesnenin bir parçasıdır.

**[get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** yöntemi, isteği yerine getirmek için uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyar. Ayrıntılı, tüm amaçları gösterir ve günlük isteğe bağlıdır. İstek bir [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) nesnesi döndürür.

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/prediction_quickstart.py?name=predict)]

## <a name="main-code-for-the-prediction"></a>Tahmin için ana kod

Tahmin almak için değişkenleri ve yöntemleri birbirine bağlamak için aşağıdaki ana yöntemi kullanın.

```python
predict(luisAppID, luisSlotName)
```
## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizin `python prediction_quickstart.py` komutu yla uygulamayı çalıştırın.

```console
python prediction_quickstart.py
```

Quickstart konsolu çıktıyı görüntüler:

```console
Top intent: HomeAutomation.TurnOn
Sentiment: None
Intents:
        "HomeAutomation.TurnOn"
Entities: {'HomeAutomation.Operation': ['on']}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öngörüleriniz bittiğinde, dosyayı ve alt dizinlerini silerek bu hızlı başlatmadan çalışmayı temizleyin.
