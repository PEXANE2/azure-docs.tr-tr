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
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732064"
---
Node.js için Dil Anlayışı (LUIS) çalışma zamanı istemci kitaplığını şu şekilde kullanın:

* Yuvaya göre tahmin
* Versiyona Göre Tahmin

[Referans belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-runtime) | [Runtime Paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/luis_prediction.js)

## <a name="prerequisites"></a>Ön koşullar

* Dil Anlama çalışma zamanı kaynağı: [Azure portalında bir tane oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)
* Bir LUIS uygulama kimliği - genel IoT uygulama kimliğini `df67dcdb-c37d-46af-88e1-8b97951ca1c2`kullanın. Quickstart kodunda kullanılan kullanıcı sorgusu bu uygulamaya özgüdür.

## <a name="setting-up"></a>Ayarlama

### <a name="get-your-language-understanding-luis-runtime-key"></a>Dil Anlayışınızı (LUIS) çalışma zamanı anahtarınızı alın

Luis çalışma zamanı kaynağı oluşturarak [çalışma zamanı anahtarınızı](../luis-how-to-azure-subscription.md) alın. Anahtarınızı ve bir sonraki adım için anahtarın bitiş noktasını tutun.

[!INCLUDE [Set up environment variables for prediction quickstart](sdk-prediction-environment-variables.md)]

### <a name="create-a-new-javascript-nodejs-file"></a>Yeni bir JavaScript (Node.js) dosyası oluşturma

Tercih ettiğiniz düzenleyicide veya IDE'de yeni `luis_prediction.js`bir JavaScript dosyası oluşturun.

### <a name="install-the-npm-library-for-the-luis-runtime"></a>LUIS çalışma zamanı için NPM kitaplığını yükleme

Uygulama dizininde, bağımlılıkları aşağıdaki komutla yükleyin:

```console
npm install @azure/cognitiveservices-luis-runtime @azure/ms-rest-js
```

## <a name="object-model"></a>Nesne modeli

Dil Anlama (LUIS) yazma istemcisi, Azure'a doğrulayan ve yazar anahtarınızı içeren bir [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) nesnesidir.

İstemci oluşturulduktan sonra, aşağıdakiler de dahil olmak üzere işlevsellik erişmek için bu istemciyi kullanın:

* [Veya](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-) `staging` `production` yuvaya göre tahmin
* [Sürüme göre tahmin](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getversionprediction-string--string--predictionrequest--models-predictiongetversionpredictionoptionalparams-)

## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Dil Anlama (LUIS) tahmin runtime istemci kitaplığı ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Yuvaya göre tahmin](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Bağımlılıkları ekleme

Proje dizininden, tercih `luis_prediction.js` ettiğiniz düzenleyici veya IDE'deki dosyayı açın. Aşağıdaki bağımlılıkları ekleyin:

[!code-javascript [Dependencies](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Dependencies)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

1. Kendi gerekli LUIS bilgileriniz için değişkenler oluşturun:

    Tahmin anahtarınızı yönetmek için değişkenler ekleyin. `LUIS_RUNTIME_KEY` Uygulama başlatıldıktan sonra ortam değişkenini oluşturduysanız, düzenleyicinin, IDE'nin veya kabuk çalıştıran değişkene erişmek için kapatılması ve yeniden yüklenmesi gerekir. Yöntemler daha sonra oluşturulacaktır.

    Kaynak adınızı `LUIS_RUNTIME_ENDPOINT`tutmak için bir değişken oluşturun.

    [!code-javascript [Azure resource variables](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Variables)]

1. Uygulama kimliği için bir değişken oluşturun. `LUIS_APP_ID` Ortam değişkenini genel IoT **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** uygulamasına ayarlayın. Yayımlanan yuvayı `production` ayarlamak için bir değişken oluşturun.

    [!code-javascript [LUIS app variables](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=OtherVariables)]


1. Anahtarınızla bir msRest.ApiKeyCredentials nesnesi oluşturun ve bir LUIS oluşturmak için bitiş noktanızla birlikte [kullanın. LUISRuntimeİste](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) nesnesi.

    [!code-javascript [LUIS Runtime client is required to access predictions for LUIS apps](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=AuthoringCreateClient)]

## <a name="get-prediction-from-runtime"></a>Çalışma zamanından tahmin alma

İstek tahmin çalışma süresi oluşturmak için aşağıdaki yöntemi ekleyin.

Kullanıcı söyleyiş [tahminİstek](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionrequest?view=azure-node-latest) nesnesinin bir parçasıdır.

**[luisRuntimeClient.prediction.getSlotPrediction](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-)** yöntemi, isteği yerine getirmek için uygulama kimliği, yuva adı ve tahmin isteği nesnesi gibi çeşitli parametrelere ihtiyaç duyar. Ayrıntılı, tüm amaçları gösterir ve günlük isteğe bağlıdır.

[!code-javascript [LUIS prediction request and response in Node.js NPM SDK](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=predict)]

## <a name="main-code-for-the-prediction"></a>Tahmin için ana kod

Tahmin almak için değişkenleri ve yöntemleri birbirine bağlamak için aşağıdaki ana yöntemi kullanın.

[!code-javascript [Main method and main call](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Main)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama dizininizin `node luis_prediction.js` komutu yla uygulamayı çalıştırın.

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

Öngörüleriniz bittiğinde, dosyayı ve alt dizinlerini silerek bu hızlı başlatmadan çalışmayı temizleyin.
