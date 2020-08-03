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
ms.custom: include file, devx-track-javascript
ms.author: diberry
ms.openlocfilehash: 88a9c2f18237974a0188f6cf387bf98ebfa2b211
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87425121"
---
Node.js için Language Understanding (LUSıS) yazma istemci kitaplığını kullanın:

* Uygulama oluşturun.
* Amaç, varlık ve örnek ekleme.
* Tümcecik listesi gibi özellikler ekleme.
* Bir uygulamayı eğitme ve yayımlama.
* Uygulamayı Sil

[Başvuru belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/?view=azure-node-latest)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-authoring)  |  [Yazma paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-authoring), [çalışma zamanı paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime)  |  [örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js)

## <a name="prerequisites"></a>Ön koşullar

* Language Understanding yazma kaynağı: [Azure Portal bir tane oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)

## <a name="setting-up"></a>Ayarlanıyor

### <a name="get-your-language-understanding-luis-starter-key"></a>Language Understanding (LUSıS) başlangıç anahtarınızı alın

Bir LUSıS yazma kaynağı oluşturarak [yazma anahtarınızı](../luis-how-to-azure-subscription.md) alın. Anahtarınızı ve anahtarın uç noktasını saklayın, kod dosyasının en üstünde dizeleri eklemeniz gerekir.

### <a name="install-the-npm-library-for-luis-authoring"></a>LUSıS yazma için NPM kitaplığını yükler

Uygulama dizini içinde aşağıdaki komutla bağımlılıkları yüklemelisiniz:

```console
npm install @azure/cognitiveservices-luis-authoring @azure/ms-rest-js
```

## <a name="object-model"></a>Nesne modeli

Language Understanding (LUSıS) yazma istemcisi, yazma anahtarınızı içeren Azure 'da kimlik doğrulayan bir [Luisauthoringclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient?view=azure-node-latest) nesnesidir.

İstemci oluşturulduktan sonra aşağıdaki işlevlere erişmek için bu istemciyi kullanın:

* Uygulamalar- [ekleme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#add-applicationcreateobject--msrest-requestoptionsbase-), [silme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#deletemethod-string--models-appsdeletemethodoptionalparams-), [Yayımlama](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#publish-string--applicationpublishobject--msrest-requestoptionsbase-)
* Örnek araslar- [Batch ile ekleme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#batch-string--string--examplelabelobject----msrest-requestoptionsbase-), [kimliğe göre silme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#deletemethod-string--string--number--msrest-requestoptionsbase-)
* Özellikler- [tümcecik listelerini](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/features?view=azure-node-latest#addphraselist-string--string--phraselistcreateobject--msrest-requestoptionsbase-) yönetme
* Model- [amaçları](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) ve varlıkları yönetme
* Desen yönetme [desenleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/pattern?view=azure-node-latest#addpattern-string--string--patternrulecreateobject--msrest-requestoptionsbase-)
* Eğitim- [uygulamayı](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#trainversion-string--string--msrest-requestoptionsbase-) ve yoklama [durumunu](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#getstatus-string--string--msrest-requestoptionsbase-) eğitme
* [Sürümler](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/versions?view=azure-node-latest) -kopyalama, dışarı aktarma ve silme ile yönetme


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için Language Understanding (LUSıS) yazma istemci kitaplığı ile aşağıdakilerin nasıl yapılacağını göstermektedir:

* [Uygulama oluşturun](#create-a-luis-app)
* [Varlık ekleme](#create-entities-for-the-app)
* [Hedef ekleme](#create-intent-for-the-app)
* [Örnek konuşmalar ekleme](#add-example-utterance-to-intent)
* [Uygulamayı eğitme](#train-the-app)
* [Uygulamayı yayımlama](#publish-a-language-understanding-app)
* [Uygulamayı silme](#delete-a-language-understanding-app)
* [Uygulamaları Listele](#list-language-understanding-apps)

## <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE 'de adlı yeni bir metin dosyası oluşturun `luis_authoring_quickstart.js` . Ardından aşağıdaki bağımlılıkları ekleyin.

[!code-javascript[Create a new application in your preferred editor or IDE.](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=Dependencies)]

[!code-javascript[Create variables for your resource's Azure endpoint and key.](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=Variables)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrulama

Anahtarınızla bir [Biliveservicescredentials](https://docs.microsoft.com/javascript/api/@azure/ms-rest-js/apikeycredentials?view=azure-node-latest) nesnesi oluşturun ve bir [Luisauthoringclient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient?view=azure-node-latest) nesnesi oluşturmak için bunu uç noktanızla birlikte kullanın.

[!code-javascript[Create LUIS client object](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

1. Hedefleri, varlıkları ve örnek kullanımları tutan doğal dil işleme (NLP) modelini içeren bir LUO uygulaması oluşturun.

1. Uygulamayı oluşturmak için bir [AppsOperation](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest) nesnesinin [Add](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest) metodunu oluşturun. Ad ve dil kültürü gerekli özelliklerdir.

    [!code-javascript[Create LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringCreateApplication&highlight=9-11)]


## <a name="create-intent-for-the-app"></a>Uygulama için amaç oluştur
Bir LUıN uygulamasının modelindeki birincil nesne, amaç ' dır. Amaç, Kullanıcı utterlerinin bir gruplandırması ile _hizalanır._ Bir Kullanıcı soru sorabilir veya bir bot 'tan (veya başka bir istemci uygulamasından) belirli bir _amaçlanan_ yanıtı bulmak için bir ifade oluşturabilir. Bir uçuşmaya örnek olarak, hedef şehirdeki hava durumu hakkında bilgi isteyerek ve müşteri hizmetleri için iletişim bilgilerini soruyor.

[Model. add_intent](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) yöntemini benzersiz bir amaç adıyla kullanın, ardından uygulama kimliği, sürüm kimliği ve yeni amaç adını geçirin.

[!code-javascript[Create intent](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringAddIntents&highlight=2-6)]

## <a name="create-entities-for-the-app"></a>Uygulama için varlık oluşturma

Varlıklar gerekli olmasa da, çoğu uygulama içinde bulunur. Varlık, kullanıcının amaç bilgisini almak için gerekli olan bilgileri Kullanıcı aracılığıyla ayıklar. Her biri kendi veri dönüştürme nesnesi (DTO) modelleriyle birlikte, çok sayıda [önceden oluşturulmuş](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addcustomprebuiltentity-string--string--prebuiltdomainmodelcreateobject--msrest-requestoptionsbase-) ve özel varlık türü vardır.  Uygulamanıza eklenecek ortak önceden oluşturulmuş varlıklar [Number](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md), [Ordinal](../luis-reference-prebuilt-ordinal.md)içerir.

Bu **add_entities** yöntemi bir basit varlık `Location` , basit bir varlık, bileşik bir varlık `Class` `Flight` ve çok sayıda önceden oluşturulmuş varlık oluşturdu.

Varlıkların bir amaç ile işaretlenmediğini bilmek önemlidir. Bunlar, genellikle birçok amaç için uygulanabilir. Yalnızca belirli bir amaç için örnek Kullanıcı utbotları işaretlenir.

Varlıklar için oluşturma yöntemleri [model](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest) sınıfının bir parçasıdır. Her varlık türünün kendi veri dönüştürme nesnesi (DTO) modeli vardır.

[!code-javascript[Create entities for the app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringAddEntities&highlight=2-6)]

## <a name="add-example-utterance-to-intent"></a>Amaca göre örnek ekleme

Bir utterance 'in amacı ve ayıklama varlıklarını tespit etmek için, uygulama için örneklere örnek gerekir. Örneklerin belirli, tek bir amacı hedeflemesi ve tüm özel varlıkları işaretlemesi gerekir. Önceden oluşturulmuş varlıkların işaretlenmesi gerekmez.

Her örnek için tek bir nesne olan [Examplelabelobject](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examplelabelobject?view=azure-node-latest) nesnelerinin bir listesini oluşturarak örnek bir parametre ekleyin. Her örnek, varlık adı ve varlık değerinin ad/değer çiftleri sözlüğüne sahip tüm varlıkları işaretlemelidir. Varlık değeri, örnek utterine 'nın metninde göründüğünden tam olarak olmalıdır.

Uygulama KIMLIĞI, sürüm KIMLIĞI ve örneklerin listesi ile [examples.batch](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#batch-string--string--examplelabelobject----msrest-requestoptionsbase-) çağrısı yapın. Çağrı, bir sonuç listesiyle yanıt verir. Modele başarıyla eklendiğinden emin olmak için her bir örneğin sonucunu denetlemeniz gerekir.

[!code-javascript[Add example utterance to intent](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringBatchAddUtterancesForIntent&highlight=52-56)]

## <a name="train-the-app"></a>Uygulamayı eğitme

Model oluşturulduktan sonra, bu modelin bu sürümü için LUıN uygulamasının eğitililmesi gerekir. Eğitilen bir model [kapsayıcıda](../luis-container-howto.md)kullanılabilir veya hazırlama veya ürün yuvalarında [yayımlanabilir](../luis-how-to-publish-app.md) .

[Eğit. Traınversion](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#trainversion-string--string--msrest-requestoptionsbase-) YÖNTEMI uygulama kimliği ve sürüm kimliği gerektirir.

Bu hızlı başlangıç gibi çok küçük bir model, çok hızlı bir şekilde eğitecektir. Üretim düzeyinde uygulamalar için, uygulamanın eğitim ne zaman ne zaman veya ne zaman başarılı olduğunu anlamak için [get_Status](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#getstatus-string--string--msrest-requestoptionsbase-) yöntemine bir yoklama çağrısı içermesi gerekir. Yanıt, her bir nesne için ayrı bir durum içeren [Modeltrainingınfo](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/modeltraininginfo?view=azure-node-latest) nesnelerinin bir listesidir. Eğitimin tamamlandı olarak kabul edilmesi için tüm nesnelerin başarılı olması gerekir.

[!code-javascript[Train LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringTrainVersion&highlight=2-5)]

Eğitim tüm modeller zaman alır. Eğitim durumunu denetlemek için operationResult 'ı kullanın.

[!code-javascript[Get training status](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringWaitForOperation&highlight=11-14)]

## <a name="publish-a-language-understanding-app"></a>Language Understanding uygulaması yayımlama

[App. Publish](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#publish-string--applicationpublishobject--msrest-requestoptionsbase-) metodunu kullanarak lusıs uygulamasını yayımlayın. Bu, geçerli eğitilen sürümü uç noktada belirtilen yuvaya yayımlar. İstemci uygulamanız bu uç noktayı, amaç ve varlık ayıklama amacıyla Kullanıcı utbotları göndermek için kullanır.

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringPublishVersion&highlight=2-5)]

## <a name="delete-a-language-understanding-app"></a>Language Understanding uygulamasını silme

[App. deleteMethod](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#deletemethod-string--models-appsdeletemethodoptionalparams-) metodunu kullanarak Luo uygulamasını silin. Bu, geçerli uygulamayı siler.

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringDeleteApp&highlight=2)]

## <a name="list-language-understanding-apps"></a>Liste dili uygulamaları anlama

Dil anlama anahtarıyla ilişkili uygulamaların listesini alın

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringListApps)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulamayı `node luis_authoring_quickstart.js` hızlı başlangıç dosyanızdaki komutla çalıştırın.

```console
node luis_authoring_quickstart.js
```

Uygulamanın komut satırı çıkışı:

```console
Created LUIS app with ID e137a439-b3e0-4e16-a7a8-a9746e0715f7
Entity Destination created.
Entity Class created.
Entity Flight created.
Intent FindFlights added.
Created 3 entity labels.
Created 3 entity labels.
Created 3 entity labels.
Example utterances added.
Waiting for train operation to finish...
Current model status: ["Queued"]
Current model status: ["InProgress"]
Current model status: ["InProgress"]
Current model status: ["InProgress"]
Current model status: ["Success"]
Application published. Endpoint URL: https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/e137a439-b3e0-4e16-a7a8-a9746e0715f7
Application with ID e137a439-b3e0-4e16-a7a8-a9746e0715f7 deleted. Operation result: Operation Successful
```
