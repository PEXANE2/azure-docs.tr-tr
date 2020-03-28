---
author: IEvangelist
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 01/27/2020
ms.topic: include
ms.author: dapine
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: 724f52317ce2afda023ae0514a330da0032e8710
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78924733"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

* <a href="~/articles/cognitive-services/Speech-Service/quickstarts/setup-platform.md" target="_blank">Geliştirme ortamınız için<span class="docon docon-navigate-external x-hidden-focus"></span>Konuşma SDK'sını yükleyin ve boş bir örnek proje oluşturun.</a>

## <a name="create-a-luis-app-for-intent-recognition"></a>Niyet tanıma için bir LUIS uygulaması oluşturma

[!INCLUDE [Create a LUIS app for intent recognition](../luis-sign-up.md)]

## <a name="open-your-project"></a>Projenizi açın

1. Tercih ettiğiniz IDE'yi açın.
2. Yeni bir proje oluşturun `quickstart.py`ve dosya oluşturun, sonra açın.

## <a name="start-with-some-boilerplate-code"></a>Bazı ortak kodile başlayın

Projemiz için iskelet görevi gören bazı kodlar ekleyelim.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-7)]

## <a name="create-a-speech-configuration"></a>Konuşma yapılandırması oluşturma

Bir `IntentRecognizer` nesneyi başlatmadan önce, LUIS tahmin kaynağınızın anahtarını ve konumunu kullanan bir yapılandırma oluşturmanız gerekir.

Bu kodu `quickstart.py`ekle. Bu değerleri güncelleştirdiğinden emin olun:

* LUIS `"YourLanguageUnderstandingSubscriptionKey"` tahmin anahtarınızla değiştirin.
* LUIS `"YourLanguageUnderstandingServiceRegion"` konumunuzla değiştirin. [Bölgeden](https://aka.ms/speech/sdkregion) **Bölge tanımlayıcısı** kullanma

>[!TIP]
> Bu değerleri bulmak için yardıma ihtiyacınız varsa, [bkz.](#create-a-luis-app-for-intent-recognition)

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=12)]

Bu örnek, `SpeechConfig` LUIS tuşu ve bölge kullanarak nesneyi yapır. Kullanılabilir yöntemlerin tam listesi için [SpeechConfig Class'a](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig)bakın.

Konuşma SDK dil için en-us kullanarak tanıma varsayılan olacaktır, kaynak dil seçimi hakkında bilgi [için metin konuşma için kaynak dil belirtin](../../../../how-to-specify-source-language.md) bakın.

## <a name="initialize-an-intentrecognizer"></a>Bir IntentRecognizer'ı Başlatma

Şimdi, bir. `IntentRecognizer` Bu kodu Konuşma yapılandırmanızın hemen altına ekleyin.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=15)]

## <a name="add-a-languageunderstandingmodel-and-intents"></a>Bir LanguageUnderstandingModel ve Niyetler ekleyin

Bir amacı `LanguageUnderstandingModel` tanıyan la ilişkilendirmeniz ve tanınmak istediğiniz amaçları eklemeniz gerekir. Ev otomasyonu için önceden oluşturulmuş alan dan gelen niyetleri kullanacağız.

Bu kodu aşağıdaki `IntentRecognizer`kodu ekleyin. LUIS uygulama kimliğinizle değiştirdiğinizden `"YourLanguageUnderstandingAppId"` emin olun. 

>[!TIP]
> Bu değeri bulmak için yardıma ihtiyacınız varsa, [bkz.](#create-a-luis-app-for-intent-recognition)

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=19-27)]

## <a name="recognize-an-intent"></a>Bir niyeti tanıma

Nesneden, `IntentRecognizer` `recognize_once()` yöntemi aramalısınız. Bu yöntem, Konuşma hizmetinin tanınmak için tek bir ifade gönderdiğinizi ve ifade tanımlandıktan sonra konuşmayı tanımayı durdurmak için tanımladığını bilmesini sağlar.

Bu kodu modelinizin altına ekleyin.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=35)]

## <a name="display-the-recognition-results-or-errors"></a>Tanıma sonuçlarını (veya hataları) görüntüleme

Tanıma sonucu Konuşma hizmeti tarafından döndürüldüğünde, bununla ilgili bir şey yapmak istersiniz. Basit tutacağız ve sonucu konsola yazdıracağız.

Çağrınızın altında `recognize_once()`, bu kodu ekleyin.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=38-47)]

## <a name="check-your-code"></a>Kodunuzu kontrol edin

Bu noktada, kodunuz bu gibi görünmelidir.

> [!NOTE]
> Bu sürüme bazı yorumlar ekledik.

[!code-python[](~/samples-cognitive-services-speech-sdk/quickstart/python/intent-recognition/quickstart.py?range=5-47)]

## <a name="build-and-run-your-app"></a>Uygulamanızı oluşturun ve çalıştırın

Örneği konsoldan veya IDE'nizden çalıştırın:

```
python quickstart.py
```

Mikrofonunuzdan yapılan sonraki 15 saniyelik konuşma girişi tanınır ve konsol penceresinde günlüğe kaydedilir.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]
