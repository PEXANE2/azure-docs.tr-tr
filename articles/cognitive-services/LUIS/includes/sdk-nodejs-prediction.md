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
ms.openlocfilehash: 9c15e4217c5331346c5a95329bae7e2a4f0e0841
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81732064"
---
Node. js için Language Understanding (LUSıS) çalışma zamanı istemci kitaplığını kullanarak şunları yapın:

* Yuvaya göre tahmin
* Sürüme göre tahmin

[Başvuru belge](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest) | [kitaplığı kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-runtime) | [çalışma zamanı paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) | [örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/luis_prediction.js)

## <a name="prerequisites"></a>Ön koşullar

* Language Understanding çalışma zamanı kaynağı: [Azure Portal bir tane oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)
* Bir LUSıS uygulama KIMLIĞI-genel IoT uygulama KIMLIĞI ' ni kullanın `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. Hızlı başlangıç kodunda kullanılan Kullanıcı sorgusu bu uygulamaya özeldir.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="get-your-language-understanding-luis-runtime-key"></a>Language Understanding (LUSıS) çalışma zamanı anahtarınızı alın

Bir LUSıS çalışma zamanı kaynağı oluşturarak [çalışma zamanı anahtarınızı](../luis-how-to-azure-subscription.md) alın. Bir sonraki adımda anahtarınızı ve anahtarın uç noktasını saklayın.

[!INCLUDE [Set up environment variables for prediction quickstart](sdk-prediction-environment-variables.md)]

### <a name="create-a-new-javascript-nodejs-file"></a>Yeni bir JavaScript (node. js) dosyası oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de adlı `luis_prediction.js`yeni bir JavaScript dosyası oluşturun.

### <a name="install-the-npm-library-for-the-luis-runtime"></a>LUSıS çalışma zamanı için NPM kitaplığını yükler

Uygulama dizini içinde aşağıdaki komutla bağımlılıkları yüklemelisiniz:

```console
npm install @azure/cognitiveservices-luis-runtime @azure/ms-rest-js
```

## <a name="object-model"></a>Nesne modeli

Language Understanding (LUSıS) yazma istemcisi, yazma anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisauthoringclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) nesnesidir.

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* [Tahmin](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-) `staging` veya `production` yuva
* [Sürüme göre tahmin](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getversionprediction-string--string--predictionrequest--models-predictiongetversionpredictionoptionalparams-)

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları Language Understanding (LUSıS) tahmini çalışma zamanı istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını gösterir:

* [Yuvaya göre tahmin](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden `luis_prediction.js` dosyayı tercih ettiğiniz DÜZENLEYICIDE veya IDE 'de açın. Aşağıdaki bağımlılıkları ekleyin:

[!code-javascript [Dependencies](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Dependencies)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

1. Kendi gerekli LUSıS bilgileriniz için değişkenler oluşturun:

    Adlı `LUIS_RUNTIME_KEY`bir ortam değişkeninden çekilen tahmin anahtarınızı yönetmek için değişken ekleyin. Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, bu değişkeni çalıştıran düzenleyici, IDE veya kabuğun kapatılıp yeniden yüklenmesi gerekir. Yöntemler daha sonra oluşturulacak.

    Kaynak adınızı `LUIS_RUNTIME_ENDPOINT`tutacak bir değişken oluşturun.

    [!code-javascript [Azure resource variables](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Variables)]

1. Uygulama KIMLIĞI için adlı `LUIS_APP_ID`bir ortam değişkeni olarak bir değişken oluşturun. Ortam değişkenini genel IoT uygulaması olarak ayarlayın **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** . `production` Yayınlanan yuvayı ayarlamak için bir değişken oluşturun.

    [!code-javascript [LUIS app variables](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=OtherVariables)]


1. Anahtarınızla msRest. ApiKeyCredentials nesnesi oluşturun ve bir Lusıs oluşturmak için bunu uç noktanızla birlikte kullanın [. LUISRuntimeClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) nesnesi.

    [!code-javascript [LUIS Runtime client is required to access predictions for LUIS apps](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=AuthoringCreateClient)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin al

İstek tahmin çalışma zamanına oluşturmak için aşağıdaki yöntemi ekleyin.

Kullanıcı söylenişi, [predictionRequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionrequest?view=azure-node-latest) nesnesinin bir parçasıdır.

**[Luisruntimeclient. tahmine. Getslottahmine](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-)** metodu, isteği yerine getirmek IÇIN uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyuyor. Verbose gibi diğer seçenekler, tüm hedefleri gösterir ve günlük isteğe bağlıdır.

[!code-javascript [LUIS prediction request and response in Node.js NPM SDK](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=predict)]

## <a name="main-code-for-the-prediction"></a>Tahmin için ana kod

Tahmin sağlamak için değişkenleri ve yöntemleri birbirine bağlamak üzere aşağıdaki ana yöntemi kullanın.

[!code-javascript [Main method and main call](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Main)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı uygulama dizininizdeki `node luis_prediction.js` komutla çalıştırın.

```console
node luis_prediction.js
```

Tahmin sonucu bir JSON nesnesi döndürür:

```console
{
   "query":"turn on all lights",
   "prediction":{
      "topIntent":"HomeAutomation.TurnOn",
      "intents":{
         "HomeAutomation.TurnOn":{
            "score":0.5375382
         },
         "None":{
            "score":0.08687421
         },
         "HomeAutomation.TurnOff":{
            "score":0.0207554
         }
      },
      "entities":{
         "HomeAutomation.Operation":[
            "on"
         ],
         "$instance":{
            "HomeAutomation.Operation":[
               {
                  "type":"HomeAutomation.Operation",
                  "text":"on",
                  "startIndex":5,
                  "length":2,
                  "score":0.724984169,
                  "modelTypeId":-1,
                  "modelType":"Unknown",
                  "recognitionSources":[
                     "model"
                  ]
               }
            ]
         }
      }
   }
}
```


## <a name="clean-up-resources"></a>Kaynakları temizleme

Tahminlerinizi tamamladığınızda, dosyayı ve alt dizinlerini silerek bu hızlı başlangıçta işi temizleyin.
