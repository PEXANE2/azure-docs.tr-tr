---
title: dosya dahil etme
description: dosya dahil etme
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 08/13/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: eac827097f892de81bc5820994f3e2a4e0696dcb
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88246155"
---
Python için Language Understanding (LUSıS) istemci kitaplıklarını kullanın:

* Uygulama oluşturun
* Makine tarafından öğrenilen bir varlık (örneğin, örnek) ile bir amaç ekleyin
* Uygulamayı eğitme ve yayımlama
* Sorgu tahmini çalışma zamanı

[Başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python)  |  [Yazma](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis/authoring) ve [tahmin](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis/runtime) kitaplığı kaynak kodu | [Paket (Pypı)](https://pypi.org/project/azure-cognitiveservices-language-luis/)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/LUIS/sdk-3x/authoring_and_predict.py)

## <a name="prerequisites"></a>Önkoşullar

* [Python 3. x](https://www.python.org/)' in geçerli sürümü. 
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services)
* Azure aboneliğiniz olduktan sonra, anahtarınızı ve uç noktanızı almak için Azure portal [bir Language Understanding yazma kaynağı oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) . Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    * Uygulamanızı Language Understanding yazmak üzere bağlamak için [oluşturduğunuz](../luis-how-to-azure-subscription.md#create-luis-resources-in-azure-portal) kaynaktaki anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz. Hizmeti denemek için ücretsiz fiyatlandırma katmanını ( `F0` ) kullanabilirsiniz.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="create-a-new-python-application"></a>Yeni Python uygulaması oluşturma

1. Konsol penceresinde, uygulamanız için yeni bir dizin oluşturun ve bu dizine taşıyın.

    ```console
    mkdir quickstart-sdk && cd quickstart-sdk
    ```

1. Python kodunuz için adlı bir dosya oluşturun `authoring_and_predict.py` . 

    ```console
    touch authoring_and_predict.py
    ```

### <a name="install-the-client-library-with-pip"></a>İstemci kitaplığını PIP ile yükler

Uygulama dizini içinde, Python için Language Understanding (LUSıS) istemci kitaplığını aşağıdaki komutla birlikte yüklersiniz:

```console
pip install azure-cognitiveservices-language-luis
```

## <a name="authoring-object-model"></a>Nesne modeli yazma

Language Understanding (LUSıS) yazma istemcisi, yazma anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisauthoringclient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-python) nesnesidir.

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* Uygulamalar- [oluşturma](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#add-application-create-object--custom-headers-none--raw-false----operation-config-), [silme](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#delete-app-id--force-false--custom-headers-none--raw-false----operation-config-), [Yayımlama](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#publish-app-id--version-id-none--is-staging-false--custom-headers-none--raw-false----operation-config-)
* Örnek araslar- [Batch ile ekleme](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#batch-app-id--version-id--example-label-object-array--custom-headers-none--raw-false----operation-config-), [kimliğe göre silme](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#delete-app-id--version-id--example-id--custom-headers-none--raw-false----operation-config-)
* Özellikler- [tümcecik listelerini](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.featuresoperations?view=azure-python#add-phrase-list-app-id--version-id--phraselist-create-object--custom-headers-none--raw-false----operation-config-) yönetme
* Model- [amaçları](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-intent-app-id--version-id--name-none--custom-headers-none--raw-false----operation-config-) ve varlıkları yönetme
* Desen yönetme [desenleri](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.patternoperations?view=azure-python#add-pattern-app-id--version-id--pattern-none--intent-none--custom-headers-none--raw-false----operation-config-)
* Eğitim- [uygulamayı](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#train-version-app-id--version-id--custom-headers-none--raw-false----operation-config-) ve yoklama [durumunu](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#get-status-app-id--version-id--custom-headers-none--raw-false----operation-config-) eğitme
* [Sürümler](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.versionsoperations?view=azure-python) -kopyalama, dışarı aktarma ve silme ile yönetme


## <a name="prediction-object-model"></a>Tahmin nesnesi modeli

Language Understanding (LUSıS) tahmini çalışma zamanı istemcisi, kaynak anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisruntimeclient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python) nesnesidir.

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* [Hazırlama veya üretim yuvasına](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-) göre tahmin
* [Sürüme](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-) göre tahmin

[!INCLUDE [Bookmark links to same article](sdk-code-examples.md)]

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

İstemci kitaplıklarını Python dosyasına ekleyin. 

[!code-python[Add python libraries to code file](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=Dependencies)]


## <a name="add-boilerplate-code"></a>Ortak kod ekleme 

1. `quickstart`Yöntemini ve çağrısını ekleyin. Bu yöntem, kalan kodların çoğunu barındırır. Bu yöntem, dosyanın sonunda çağırılır. 

    ```python
    def quickstart(): 

        # add calls here, remember to indent properly
    
    quickstart()
    ```

1. Aksi belirtilmedikçe, kalan kodu hızlı başlangıç yöntemine ekleyin. 

## <a name="create-variables-for-the-app"></a>Uygulama için değişkenler oluşturma

1. Yazma anahtarınızı ve kaynak adlarınızı tutmak için değişkenler oluşturun.

    [!code-python[Variables you need to change](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=VariablesYouChange)]

1. Uç noktalarınızı, uygulama adınızı, sürümünüzü ve amaç adınızı tutmak için değişkenler oluşturun.

    [!code-python[Variables you don't need to change](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=VariablesYouDontNeedToChangeChange)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Anahtarınızla bir [Biliveservicescredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) nesnesi oluşturun ve bir [Luisauthoringclient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-python) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!code-python[Create LUIS authoring client object](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

Bir LUSıS uygulaması, amaçlar, varlıklar ve örnek işleme dahil olmak üzere doğal dil işleme (NLP) modelini içerir.

Uygulamayı oluşturmak için bir [AppsOperation](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python) nesnesinin [Add](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#add-application-create-object--custom-headers-none--raw-false----operation-config-) metodunu oluşturun. Ad ve dil kültürü gerekli özelliklerdir.

[!code-python[Create LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringCreateApplication)]


## <a name="create-intent-for-the-app"></a>Uygulama için amaç oluştur
Bir LUıN uygulamasının modelindeki birincil nesne, amaç ' dır. Amaç, Kullanıcı utterlerinin bir gruplandırması ile _hizalanır._ Bir Kullanıcı soru sorabilir veya bir bot 'tan (veya başka bir istemci uygulamasından) belirli bir _amaçlanan_ yanıtı bulmak için bir ifade oluşturabilir. Bir uçuşmaya örnek olarak, hedef şehirdeki hava durumu hakkında bilgi isteyerek ve müşteri hizmetleri için iletişim bilgilerini soruyor.

[Model. add_intent](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-intent-app-id--version-id--name-none--custom-headers-none--raw-false----operation-config-) yöntemini benzersiz bir amaç adıyla kullanın, ardından uygulama kimliği, sürüm kimliği ve yeni amaç adını geçirin.

`intentName`Değer, `OrderPizzaIntent` [uygulama Için değişkenlerinizi oluşturma](#create-variables-for-the-app) bölümünde değişkenlerin bir parçası olarak için sabit olarak kodlanır.

[!code-python[Create intent](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AddIntent)]

## <a name="create-entities-for-the-app"></a>Uygulama için varlık oluşturma

Varlıklar gerekli olmasa da, çoğu uygulama içinde bulunur. Varlık, kullanıcının amaç bilgisini almak için gerekli olan bilgileri Kullanıcı aracılığıyla ayıklar. Her biri kendi veri dönüştürme nesnesi (DTO) modelleriyle birlikte, çok sayıda [önceden oluşturulmuş](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-prebuilt-app-id--version-id--prebuilt-extractor-names--custom-headers-none--raw-false----operation-config-) ve özel varlık türü vardır.  Uygulamanıza eklenecek ortak önceden oluşturulmuş varlıklar [Number](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md), [Ordinal](../luis-reference-prebuilt-ordinal.md)içerir.

Varlıkların bir amaç ile işaretlenmediğini bilmek önemlidir. Bunlar, genellikle birçok amaç için uygulanabilir. Yalnızca belirli bir amaç için örnek Kullanıcı utbotları işaretlenir.

Varlıklar için oluşturma yöntemleri, [ModelOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python) sınıfının bir parçasıdır. Her varlık türünün kendi veri dönüştürme nesnesi (DTO) modeli vardır.

Varlık oluşturma kodu, alt varlıklara uygulanan alt varlıklar ve özelliklerle makine öğrenimi varlığı oluşturur `Quantity` .

:::image type="content" source="../media/quickstart-sdk/machine-learned-entity.png" alt-text="Varlık oluşturma kodu, alt varlıklar ve ' miktar ' alt varlıklarına uygulanan özelliklerle makine öğrenimi varlığı oluşturur.":::

[!code-python[Create entities](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringAddEntities)]

`quickstart`Bu alt varlığa Özellikler atamak için, alt varlık kimliğini bulmak için yönteminin üzerine aşağıdaki yöntemi koyun.

[!code-python[Find subentity id](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringSortModelObject)]

## <a name="add-example-utterance-to-intent"></a>Amaca göre örnek ekleme

Bir utterance 'in amacı ve ayıklama varlıklarını tespit etmek için, uygulama için örneklere örnek gerekir. Örneklerin belirli, tek bir amacı hedeflemesi ve tüm özel varlıkları işaretlemesi gerekir. Önceden oluşturulmuş varlıkların işaretlenmesi gerekmez.

Her örnek için tek bir nesne olan [Examplelabelobject](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.models.examplelabelobject?view=azure-python) nesnelerinin bir listesini oluşturarak örnek bir parametre ekleyin. Her örnek, varlık adı ve varlık değerinin ad/değer çiftleri sözlüğüne sahip tüm varlıkları işaretlemelidir. Varlık değeri, örnek utterine 'nın metninde göründüğünden tam olarak olmalıdır.

:::image type="content" source="../media/quickstart-sdk/labeled-example-machine-learned-entity.png" alt-text="Varlık değeri, örnek utterine 'nın metninde göründüğünden tam olarak olmalıdır.":::

[Örnekleri çağırın.](https://docs.microsoft.com//python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#add-app-id--version-id--example-label-object--enable-nested-children-false--custom-headers-none--raw-false----operation-config-) uygulama kimliği, sürüm kimliği ve örnekle birlikte ekleyin. 

[!code-python[Add example utterance to a specific intent](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=AuthoringAddLabeledExamples)]

## <a name="train-the-app"></a>Uygulamayı eğitme

Model oluşturulduktan sonra, bu modelin bu sürümü için LUıN uygulamasının eğitililmesi gerekir. Eğitilen bir model [kapsayıcıda](../luis-container-howto.md)kullanılabilir veya hazırlama veya ürün yuvalarında [yayımlanabilir](../luis-how-to-publish-app.md) .

[Eğit. train_version](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#train-version-app-id--version-id--custom-headers-none--raw-false----operation-config-) YÖNTEMI uygulama kimliği ve sürüm kimliği gerektirir.

Bu hızlı başlangıç gibi çok küçük bir model, çok hızlı bir şekilde eğitecektir. Üretim düzeyinde uygulamalar için, uygulamanın eğitim ne zaman ne zaman veya ne zaman başarılı olduğunu anlamak için [get_Status](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#get-status-app-id--version-id--custom-headers-none--raw-false----operation-config-) yöntemine bir yoklama çağrısı içermesi gerekir. Yanıt, her bir nesne için ayrı bir durum içeren [Modeltrainingınfo](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.models.modeltraininginfo?view=azure-python) nesnelerinin bir listesidir. Eğitimin tamamlandı olarak kabul edilmesi için tüm nesnelerin başarılı olması gerekir.

[!code-python[Train the app's version](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=TrainAppVersion)]

## <a name="publish-a-language-understanding-app"></a>Language Understanding uygulaması yayımlama

[App. Publish](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#publish-app-id--version-id-none--is-staging-false--custom-headers-none--raw-false----operation-config-) metodunu kullanarak lusıs uygulamasını yayımlayın. Bu, geçerli eğitilen sürümü uç noktada belirtilen yuvaya yayımlar. İstemci uygulamanız bu uç noktayı, amaç ve varlık ayıklama amacıyla Kullanıcı utbotları göndermek için kullanır.

[!code-python[Publish app to production slot](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=PublishVersion)]

## <a name="authenticate-the-prediction-runtime-client"></a>Tahmin çalışma zamanı istemcisinin kimliğini doğrulama

Anahtarınızla birlikte kimlik bilgileri nesnesini kullanın ve bir [Luisruntimeclientconfiguration](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!INCLUDE [Caution about using authoring key](caution-authoring-key.md)]

[!code-python[Create LUIS runtime client object](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=PredictionCreateClient)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin al

İstek tahmin çalışma zamanına oluşturmak için aşağıdaki kodu ekleyin.

Kullanıcı söylenişi [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python) nesnesinin bir parçasıdır.

**[Get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** yöntemi, isteği yerine getirmek IÇIN uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyuyor. Verbose gibi diğer seçenekler, tüm hedefleri gösterir ve günlük isteğe bağlıdır. İstek bir [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) nesnesi döndürüyor.

[!code-python[Get prediction based on query](~/cognitive-services-quickstart-code/python/LUIS/sdk-3x/authoring_and_predict.py?name=QueryPredictionEndpoint)]

[!INCLUDE [Prediction JSON response](sdk-json.md)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `python` hızlı başlangıç dosyanızdaki komutla çalıştırın.

```console
python authoring_and_predict.py
```