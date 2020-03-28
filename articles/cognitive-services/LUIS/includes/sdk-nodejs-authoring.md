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
ms.openlocfilehash: 070de0f65e890c38acd5075286b349e95cd19f3b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77372266"
---
Node.js için istemci kitaplığını yazan Dil Anlayışını (LUIS) şu şekilde kullanın:

* Bir uygulama oluşturun.
* Niyetler, varlıklar ve örnek söz lerle ekleyin.
* İfade listesi gibi özellikler ekleyin.
* Bir uygulamayı eğitin ve yayınlayın.
* Uygulamayı silme

[Referans belgeleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/?view=azure-node-latest) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-authoring) | [Yazma Paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-authoring), [Çalışma Zamanı Paketi (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) | [Örnekleri](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/luis_authoring_quickstart.js)

## <a name="prerequisites"></a>Ön koşullar

* Dil Anlama yazma kaynağı: [Azure portalında bir tane oluşturun](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)

## <a name="setting-up"></a>Ayarlama

### <a name="get-your-language-understanding-luis-starter-key"></a>Dil Anlayışınızı (LUIS) başlangıç anahtarınızı alın

Bir LUIS yazma kaynağı oluşturarak [başlangıç anahtarınızı](../luis-how-to-azure-subscription.md#starter-key) alın. Anahtarınızı ve bir sonraki adım için anahtarın bitiş noktasını tutun.

### <a name="create-an-environment-variable"></a>Bir ortam değişkeni oluşturma

Anahtarınızı ve anahtar için bölgeyi kullanarak kimlik doğrulaması için iki ortam değişkeni oluşturun:

* `LUIS_AUTHORING_KEY`- İsteklerinizi doğrulamak için kaynak anahtarı.
* `LUIS_AUTHORING_ENDPOINT`- Anahtarınızla ilişkili bitiş noktası.

İşletim sisteminiziçin yönergeleri kullanın.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx LUIS_AUTHORING_KEY <replace-with-your-luis-authoring-key
setx LUIS_AUTHORING_ENDPOINT <replace-with-your-luis-authoring-endpoint>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export LUIS_AUTHORING_KEY=<replace-with-your-luis-authoring-key>
export LUIS_AUTHORING_ENDPOINT=<replace-with-your-luis-authoring-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[Macos](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export LUIS_AUTHORING_KEY=<replace-with-your-luis-authoring-key>
export LUIS_AUTHORING_ENDPOINT=<replace-with-your-luis-authoring-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.
***

### <a name="install-the-npm-library-for-luis-authoring"></a>LUIS yazma için NPM kitaplığını yükleme

Uygulama dizininde, bağımlılıkları aşağıdaki komutla yükleyin:

```console
npm install @azure/cognitiveservices-luis-authoring @azure/ms-rest-js
```

## <a name="object-model"></a>Nesne modeli

Dil Anlama (LUIS) yazma istemcisi, Azure'a doğrulayan ve yazar anahtarınızı içeren bir [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient?view=azure-node-latest) nesnesidir.

İstemci oluşturulduktan sonra, aşağıdakiler de dahil olmak üzere işlevsellik erişmek için bu istemciyi kullanın:

* Uygulamalar - [eklemek,](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#add-applicationcreateobject--msrest-requestoptionsbase-) [sil,](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#deletemethod-string--models-appsdeletemethodoptionalparams-) [yayınlamak](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#publish-string--applicationpublishobject--msrest-requestoptionsbase-)
* Örnek söyleyinçler - [toplu iş ekle](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#batch-string--string--examplelabelobject----msrest-requestoptionsbase-), [ID'ye göre sil](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#deletemethod-string--string--number--msrest-requestoptionsbase-)
* Özellikler - [ifade listelerini](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/features?view=azure-node-latest#addphraselist-string--string--phraselistcreateobject--msrest-requestoptionsbase-) yönetme
* Model - [niyetleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) ve varlıkları yönetme
* Desen - [desenleri](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/pattern?view=azure-node-latest#addpattern-string--string--patternrulecreateobject--msrest-requestoptionsbase-) yönetme
* Tren - [eğitim durumu](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#getstatus-string--string--msrest-requestoptionsbase-) için uygulamayı ve anketi [eğitin](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#trainversion-string--string--msrest-requestoptionsbase-)
* [sürümler](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/versions?view=azure-node-latest) - klonla yönetin, dışa aktarma ve silme


## <a name="code-examples"></a>Kod örnekleri

Bu kod parçacıkları, Node.js için istemci kitaplığı yazan Dil Anlayışı (LUIS) ile aşağıdakileri nasıl yapacağınızı gösterir:

* [Uygulama oluşturma](#create-a-luis-app)
* [Varlık ekleme](#create-entities-for-the-app)
* [Hedef ekleme](#create-intent-for-the-app)
* [Örnek konuşmalar ekleme](#add-example-utterance-to-intent)
* [Uygulamayı eğitme](#train-the-app)
* [Uygulamayı yayımlama](#publish-a-language-understanding-app)
* [Uygulamayı silme](#delete-a-language-understanding-app)
* [Uygulamaları listele](#list-language-understanding-apps)

## <a name="create-a-new-nodejs-application"></a>Yeni bir Node.js uygulaması oluşturma

Tercih ettiğiniz düzenleyicide veya IDE adlı `luis_authoring_quickstart.js`yeni bir metin dosyası oluşturun. Ardından aşağıdaki bağımlılıkları ekleyin.

[!code-javascript[Create a new application in your preferred editor or IDE.](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=Dependencies)]

Kaynağınızın Azure bitiş noktası ve anahtarı için değişkenler oluşturun. Uygulamayı başlattıktan sonra ortam değişkenini oluşturduysanız, değişkene erişmek için düzenleyiciyi, IDE'yi veya bu uygulamayı çalıştıran kabuğu kapatmanız ve yeniden açmanız gerekir.

[!code-javascript[Create variables for your resource's Azure endpoint and key.](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=Variables)]

## <a name="authenticate-the-client"></a>İstemcinin kimliğini doğrula

Anahtarınızla bir [CognitiveServicesCredentials](https://docs.microsoft.com/javascript/api/@azure/ms-rest-js/apikeycredentials?view=azure-node-latest) nesnesi oluşturun ve bir [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient?view=azure-node-latest) nesnesi oluşturmak için bitiş noktanızla birlikte kullanın.

[!code-javascript[Create LUIS client object](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>LUIS uygulaması oluşturma

1. Amaçları, varlıkları ve örnek söyleyiyi tutan doğal dil işleme (NLP) modelini içeren bir LUIS uygulaması oluşturun.

1. Uygulamayı oluşturmak için [AppsOperation](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest) nesnesinin [ekleme](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest) yöntemini oluşturun. Ad ve dil kültürü gerekli özellikleri vardır.

    [!code-javascript[Create LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringCreateApplication&highlight=9-11)]


## <a name="create-intent-for-the-app"></a>Uygulama için niyet oluşturma
LUIS uygulamasının modelindeki birincil nesne amaçtır. Niyet kullanıcı söyleyiş _niyetleri_bir gruplama ile hizalar. Bir kullanıcı bir soru sorabilir veya bir bottan (veya başka bir istemci uygulamasından) belirli bir _yanıt_ arayan bir açıklama yapabilir. Bir uçuş rezervasyonu, varış şehrindeki hava durumu hakkında soru sormak ve müşteri hizmetleri için iletişim bilgilerini sormak gibi niyetörnekleri ne verilebilir:

Benzersiz amaç adı ile [model.add_intent](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) yöntemini kullanın ve ardından uygulama kimliğini, sürüm kimliğini ve yeni niyet adını geçirin.

[!code-javascript[Create intent](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringAddIntents&highlight=2-6)]

## <a name="create-entities-for-the-app"></a>Uygulama için varlıklar oluşturma

Varlıklar gerekli olmasa da, çoğu uygulamada bulunur. Varlık, kullanıcının niyetini tamolarak dosyalamak için gerekli olan kullanıcı sözcüklerinden bilgi ayıklar. Önceden [oluşturulmuş](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addcustomprebuiltentity-string--string--prebuiltdomainmodelcreateobject--msrest-requestoptionsbase-) ve özel varlıklar, her biri kendi veri dönüştürme nesnesi (DTO) modelleri ile çeşitli türleri vardır.  Uygulamanıza eklemek için ortak önceden oluşturulmuş varlıklar [numarası,](../luis-reference-prebuilt-number.md) [datetimeV2,](../luis-reference-prebuilt-datetimev2.md) [geographyV2](../luis-reference-prebuilt-geographyv2.md), [ordinal](../luis-reference-prebuilt-ordinal.md)içerir.

Bu **add_entities** yöntemi `Location` iki rol, basit `Class` bir varlık, bileşik varlık ve birkaç önceden oluşturulmuş varlıklar ekler basit bir `Flight` varlık oluşturdu.

Varlıkların bir niyetle işaretlenmediğini bilmek önemlidir. Onlar ve genellikle birçok niyet için geçerli dir. Yalnızca örnek kullanıcı sözcükleri belirli, tek bir amaç için işaretlenir.

Varlıklar için oluşturma yöntemleri [Model](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest) sınıfının bir parçasıdır. Her varlık türünün kendi veri dönüştürme nesnesi (DTO) modeli vardır.

[!code-javascript[Create entities for the app](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringAddEntities&highlight=2-6)]

## <a name="add-example-utterance-to-intent"></a>Niyete örnek söyleyiş ekleme

Bir söyleyiş niyetini belirlemek ve varlıkları ayıklamak için, uygulamanın söyleyiş örneklerine ihtiyacı vardır. Örneklerin belirli, tek bir amacı hedeflemesi ve tüm özel varlıkları işaretlemesi gerekir. Önceden oluşturulmuş varlıkların işaretlemesi gerekmez.

[Örnek Etiket Nesnesi](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examplelabelobject?view=azure-node-latest) nesnelerinin bir listesini oluşturarak örnek söyleyerek, her örnek için bir nesne ekleyin. Her örnek, varlık adı ve varlık değerinin ad/değer çiftleri sözlüğü ile tüm varlıkları işaretlemelidir. Varlık değeri tam olarak örnek söyleyiş metninde göründüğü gibi olmalıdır.

[Arama örnekleri.batch](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#batch-string--string--examplelabelobject----msrest-requestoptionsbase-) uygulama kimliği, sürüm kimliği ve örneklerin listesi ile. Arama, bir sonuç listesiyle yanıt verir. Modele başarıyla eklenmiştir emin olmak için her örneğin sonucunu denetlemeniz gerekir.

[!code-javascript[Add example utterance to intent](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringBatchAddUtterancesForIntent&highlight=52-56)]

## <a name="train-the-app"></a>Uygulamayı eğitme

Model oluşturulduktan sonra, LUIS uygulamasının modelin bu sürümü için eğitilmesi gerekir. Eğitimli bir model bir [kapta](../luis-container-howto.md)kullanılabilir veya evreleme veya ürün yuvalarında [yayınlanabilir.](../luis-how-to-publish-app.md)

[train.trainVersion](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#trainversion-string--string--msrest-requestoptionsbase-) yönteminin uygulama kimliği ne ve sürüm kimliğine ihtiyacı vardır.

Çok küçük bir model, bu quickstart gösterir gibi, çok hızlı bir şekilde tren olacaktır. Üretim düzeyindeki uygulamalar için, uygulamanın eğitimi, eğitimin ne zaman veya ne zaman başarılı olduğunu belirlemek için [get_status](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#getstatus-string--string--msrest-requestoptionsbase-) yöntemine bir yoklama çağrısı içermelidir. Yanıt, her nesne için ayrı bir durum olan [ModelTrainingInfo](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/modeltraininginfo?view=azure-node-latest) nesnelerinin listesidir. Eğitimin tamamlanmış sayılması için tüm nesnelerin başarılı olması gerekir.

[!code-javascript[Train LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringTrainVersion&highlight=2-5)]

Tüm modellerin eğitimi zaman alır. Eğitim durumunu kontrol etmek için operasyon Sonucu'nu kullanın.

[!code-javascript[Get training status](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringWaitForOperation&highlight=11-14)]

## <a name="publish-a-language-understanding-app"></a>Dil Anlama uygulaması yayınlama

App.publish yöntemini kullanarak LUIS uygulamasını [yayımlayın.](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#publish-string--applicationpublishobject--msrest-requestoptionsbase-) Bu, geçerli eğitilmiş sürümü bitiş noktasında belirtilen yuvaya yayımlar. İstemci uygulamanız, amaç ve varlık çıkarma tahmini için kullanıcı yla ilgili söyleyişler göndermek için bu bitiş noktasını kullanır.

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringPublishVersion&highlight=2-5)]

## <a name="delete-a-language-understanding-app"></a>Dil Anlama uygulamasını silme

[App.deleteMethod](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#deletemethod-string--models-appsdeletemethodoptionalparams-) yöntemini kullanarak LUIS uygulamasını silin. Bu, geçerli uygulamayı siler.

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringDeleteApp&highlight=2)]

## <a name="list-language-understanding-apps"></a>Dil anlama uygulamalarını listele

Dil anlama anahtarıyla ilişkili uygulamaların listesini alma

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/luis_authoring_quickstart.js?name=AuthoringListApps)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Hızlı başlatma dosyanızdaki `node luis_authoring_quickstart.js` komutla uygulamayı çalıştırın.

```console
node luis_authoring_quickstart.js
```

Uygulamanın komut satırı çıktısı:

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
