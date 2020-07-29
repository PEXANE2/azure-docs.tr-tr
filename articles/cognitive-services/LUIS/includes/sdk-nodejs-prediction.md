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
ms.openlocfilehash: 19da911cbc662f47e7bbd16aaddf8803d4109d6a
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369294"
---
Node.js için Language Understanding (LUSıS) çalışma zamanı istemci kitaplığını kullanın:

* Yuvaya göre tahmin
* Sürüme göre tahmin

[Başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-runtime)  |  [Çalışma zamanı paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime)  |  [Örnekler](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/node-sdk-authoring-prediction/luis_prediction.js)

## <a name="prerequisites"></a>Önkoşullar

* Language Understanding çalışma zamanı kaynağı: [Azure Portal bir tane oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)
* Bir LUSıS uygulama KIMLIĞI-genel IoT uygulama KIMLIĞI ' ni kullanın `df67dcdb-c37d-46af-88e1-8b97951ca1c2` . Hızlı başlangıç kodunda kullanılan Kullanıcı sorgusu bu uygulamaya özeldir.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="get-your-language-understanding-luis-runtime-key"></a>Language Understanding (LUSıS) çalışma zamanı anahtarınızı alın

Bir LUSıS çalışma zamanı kaynağı oluşturarak [çalışma zamanı anahtarınızı](../luis-how-to-azure-subscription.md) alın. Bir sonraki adımda anahtarınızı ve anahtarın uç noktasını saklayın.

### <a name="create-a-new-javascript-nodejs-file"></a>Yeni bir JavaScript (Node.js) dosyası oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de adlı yeni bir JavaScript dosyası oluşturun `luis_prediction.js` .

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

Proje dizininden `luis_prediction.js` dosyayı tercih ettiğiniz düzenleyicide veya IDE 'de açın. Aşağıdaki bağımlılıkları ekleyin:

[!code-javascript [Dependencies](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_prediction.js?name=Dependencies)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

1. Kendi gerekli LUSıS bilgileriniz için değişkenler oluşturun:

    Tahmin anahtarınızı ve uç nokta anahtarınızı yönetmek için değişken ekleyin. 
    
    [!code-javascript [Azure resource variables](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_prediction.js?name=Variables)]

1. Adlı uygulama KIMLIĞI için bir değişken oluşturun `LUIS_APP_ID` . Değişkenini genel IoT uygulaması olarak ayarlayın **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** . Yayınlanan yuvayı ayarlamak için bir değişken oluşturun `production` .

    [!code-javascript [LUIS app variables](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_prediction.js?name=OtherVariables)]


1. Anahtarınızla msRest. ApiKeyCredentials nesnesi oluşturun ve bir Lusıs oluşturmak için bunu uç noktanızla birlikte kullanın [. LUISRuntimeClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) nesnesi.

    [!code-javascript [LUIS Runtime client is required to access predictions for LUIS apps](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_prediction.js?name=AuthoringCreateClient)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin al

İstek tahmin çalışma zamanına oluşturmak için aşağıdaki yöntemi ekleyin.

Kullanıcı söylenişi, [predictionRequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionrequest?view=azure-node-latest) nesnesinin bir parçasıdır.

**[Luisruntimeclient. tahmine. Getslottahmine](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-)** metodu, isteği yerine getirmek IÇIN uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyuyor. Verbose gibi diğer seçenekler, tüm hedefleri gösterir ve günlük isteğe bağlıdır.

[!code-javascript [LUIS prediction request and response in Node.js NPM SDK](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_prediction.js?name=predict)]

## <a name="main-code-for-the-prediction"></a>Tahmin için ana kod

Tahmin sağlamak için değişkenleri ve yöntemleri birbirine bağlamak üzere aşağıdaki ana yöntemi kullanın.

[!code-javascript [Main method and main call](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_prediction.js?name=Main)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node luis_prediction.js` uygulama dizininizdeki komutla çalıştırın.

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
