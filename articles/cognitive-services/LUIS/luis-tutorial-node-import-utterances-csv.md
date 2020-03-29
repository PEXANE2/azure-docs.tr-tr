---
title: Node.js kullanarak ithalat söyleyişleri - LUIS
titleSuffix: Azure Cognitive Services
description: LUIS Yazma API'sini kullanarak CSV formatında önceden varolan verilerden programlı bir ŞEKILDE nasıl bir LUIS uygulaması oluşturabildiğini öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: diberry
ms.openlocfilehash: ef5f6967b7ad9500672d00d93dd8acaca99e5948
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73499456"
---
# <a name="build-a-luis-app-programmatically-using-nodejs"></a>Node.js kullanarak programlı bir LUIS uygulaması oluşturun

LUIS, [LUIS](luis-reference-regions.md) web sitesinin yaptığı her şeyi yapan programlı bir API sağlar. Bu, önceden varolan verileriniz olduğunda zamandan tasarruf edebilir ve programlı bir LUIS uygulaması oluşturmak, bilgileri elle girmekten daha hızlı olacaktır. 

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

## <a name="prerequisites"></a>Ön koşullar

* [LUIS](luis-reference-regions.md) web sitesinde oturum açın ve Hesap Ayarları'nda [yazar anahtarınızı](luis-concept-keys.md#authoring-key) bulun. Bu anahtarı Yazma API'lerini aramak için kullanırsınız.
* Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
* Bu makale, varsayımsal bir şirketin kullanıcı istekleri günlük dosyaları için bir CSV ile başlar. [Buradan](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/IoT.csv)indirin.
* NPM ile en son Düğüm.js yükleyin. [Buradan](https://nodejs.org/en/download/)indirin.
* **[Önerilen]** IntelliSense ve hata ayıklama için Visual Studio Code, ücretsiz olarak [buradan](https://code.visualstudio.com/) indirin.

Bu makaledeki kodun tümü [Azure Örnekleri Dil Anlama GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/examples/build-app-programmatically-csv)mevcuttur. 

## <a name="map-preexisting-data-to-intents-and-entities"></a>Önceden varolan verileri niyet ve varlıklarla eşle
Luis düşünülarak oluşturulmayan bir sisteminiz olsa bile, kullanıcıların yapmak istediği farklı şeyleri eşleyen metin verileri içeriyorsa, mevcut kullanıcı girişi kategorilerinden LUIS'teki niyete bir eşleme ile gelebilirsiniz. Kullanıcıların söylediklerinde önemli sözcükleri veya tümcecikleri tanımlayabiliyorsanız, bu sözcükler varlıklarla eşlenebilir.

Dosyayı [`IoT.csv`](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/IoT.csv) açın. Nasıl kategorilere alındıkları, kullanıcının ne dediği ve yararlı bilgiler içeren bazı sütunlar dahil olmak üzere varsayımsal bir ev otomasyon hizmetine kullanıcı sorgularının bir günlük içerir. 

![Önceden varolan verilerin CSV dosyası](./media/luis-tutorial-node-import-utterances-csv/csv.png) 

**İstek Türü** sütununun kasıtlı olabileceğini ve **İstek** sütununda örnek bir sözcük olduğunu görürsünüz. Diğer alanlar, söz konusu olan da olsa varlıklar olabilir. Niyetler, varlıklar ve örnek sözlerle olduğundan, basit, örnek bir uygulama için gereksinimlere sahipsiniz.

## <a name="steps-to-generate-a-luis-app-from-non-luis-data"></a>LUIS olmayan verilerden luis uygulaması oluşturma adımları
CSV dosyasından yeni bir LUIS uygulaması oluşturmak için:

* CSV dosyasındaki verileri ayrıştın:
    * Yazarlık API'sini kullanarak LUIS'e yükleyebileceğiniz bir biçime dönüştürün. 
    * Ayrıştırılan verilerden, niyetler ve varlıklar hakkında bilgi toplayın. 
* Yazma API aramaları yapma:
    * Uygulamayı oluşturun.
    * Ayrıştırılmış verilerden toplanan niyet ve varlıklar ekleyin. 
    * LUIS uygulamasını oluşturduktan sonra, ayrıştılı verilerden örnek sözlerini ekleyebilirsiniz. 

Bu program akışını `index.js` dosyanın son bölümünde görebilirsiniz. Bu kodu kopyalayın veya `index.js` [indirin](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/index.js) ve ''de kaydedin.

   [!code-javascript[Node.js code for calling the steps to build a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/index.js)]


## <a name="parse-the-csv"></a>CSV ayrıştını

CSV'deki sözcükleri içeren sütun girişlerinin, LUIS'in anlayabileceği bir JSON biçimine ayrışdırılması gerekir. Bu JSON biçimi, `intentName` söyleyiş niyetini tanımlayan bir alan içermelidir. Ayrıca, söyleyişte `entityLabels` herhangi bir varlık yoksa boş olabilecek bir alan da içermelidir. 

Örneğin, bu JSON için "ışıkları açın" haritaları için giriş:

```json
        {
            "text": "Turn on the lights",
            "intentName": "TurnOn",
            "entityLabels": [
                {
                    "entityName": "Operation",
                    "startCharIndex": 5,
                    "endCharIndex": 6
                },
                {
                    "entityName": "Device",
                    "startCharIndex": 12,
                    "endCharIndex": 17
                }
            ]
        }
```

Bu örnekte, `intentName` CSV dosyasındaki **İstek** sütunu altındaki kullanıcı isteğinden `entityName` ve anahtar bilgileri içeren diğer sütunlardan gelir. Örneğin, **Operasyon** veya **Aygıt**için bir giriş varsa ve bu dize de gerçek istekte oluşuyorsa, varlık olarak etiketlenebilir. Aşağıdaki kod bu ayrışma işlemini gösterir. Kopyalayabilir veya [indirebilir](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_parse.js) ve `_parse.js`'ye kaydedebilirsiniz.

   [!code-javascript[Node.js code for parsing a CSV file to extract intents, entities, and labeled utterances](~/samples-luis/examples/build-app-programmatically-csv/_parse.js)]



## <a name="create-the-luis-app"></a>LUIS uygulamasını oluşturun
Veriler JSON'a ayrıştıktan sonra, bir LUIS uygulamasına ekleyin. Aşağıdaki kod LUIS uygulamasını oluşturur. Kopyalayın veya [indirin](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_create.js) ve `_create.js`'ye kaydedin.

   [!code-javascript[Node.js code for creating a LUIS app](~/samples-luis/examples/build-app-programmatically-csv/_create.js)]


## <a name="add-intents"></a>Hedef ekleme
Bir uygulamanız olduğunda, uygulamanın amacına sahip olmanız gerekir. Aşağıdaki kod LUIS uygulamasını oluşturur. Kopyalayın veya [indirin](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_intents.js) ve `_intents.js`'ye kaydedin.

   [!code-javascript[Node.js code for creating a series of intents](~/samples-luis/examples/build-app-programmatically-csv/_intents.js)]


## <a name="add-entities"></a>Varlık ekleme
Aşağıdaki kod, varlıkları LUIS uygulamasına ekler. Kopyalayın veya [indirin](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_entities.js) ve `_entities.js`'ye kaydedin.

   [!code-javascript[Node.js code for creating entities](~/samples-luis/examples/build-app-programmatically-csv/_entities.js)]
   


## <a name="add-utterances"></a>Konuşma ekleme
Varlıklar ve niyetler LUIS uygulamasında tanımlandıktan sonra, söyleyiyi ekleyebilirsiniz. Aşağıdaki kod, bir seferde en fazla 100 sözcük eklemenize olanak tanıyan [Utterances_AddBatch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09) API'yi kullanır.  Kopyalayın veya [indirin](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/examples/build-app-programmatically-csv/_upload.js) ve `_upload.js`'ye kaydedin.

   [!code-javascript[Node.js code for adding utterances](~/samples-luis/examples/build-app-programmatically-csv/_upload.js)]


## <a name="run-the-code"></a>Kodu çalıştırma


### <a name="install-nodejs-dependencies"></a>Düğüm.js bağımlılıklarını yükleyin
NPM'deki Düğüm.js bağımlılıklarını terminal/komut satırına yükleyin.

```console
> npm install
```

### <a name="change-configuration-settings"></a>Yapılandırma Ayarlarını Değiştir
Bu uygulamayı kullanabilmek için index.js dosyasındaki değerleri kendi bitiş noktası anahtarınızla değiştirmeniz ve uygulamanın sahip olmasını istediğiniz adı sağlamanız gerekir. Ayrıca uygulamanın kültürünü ayarlayabilir veya sürüm numarasını değiştirebilirsiniz.

index.js dosyasını açın ve bu değerleri dosyanın üst kısmında değiştirin.


```javascript
// Change these values
const LUIS_programmaticKey = "YOUR_AUTHORING_KEY";
const LUIS_appName = "Sample App";
const LUIS_appCulture = "en-us"; 
const LUIS_versionId = "0.1";
```

### <a name="run-the-script"></a>Betiği çalıştırın
Komut dosyasını Node.js ile bir terminal/komut satırından çalıştırın.

```console
> node index.js
```

or

```console
> npm start
```

### <a name="application-progress"></a>Uygulama ilerlemesi
Uygulama çalışırken, komut satırı ilerlemeyi gösterir. Komut satırı çıktısı LUIS gelen yanıtların biçimini içerir.

```console
> node index.js
intents: ["TurnOn","TurnOff","Dim","Other"]
entities: ["Operation","Device","Room"]
parse done
JSON file should contain utterances. Next step is to create an app with the intents and entities it found.
Called createApp, created app with ID 314b306c-0033-4e09-92ab-94fe5ed158a2
Called addIntents for intent named TurnOn.
Called addIntents for intent named TurnOff.
Called addIntents for intent named Dim.
Called addIntents for intent named Other.
Results of all calls to addIntent = [{"response":"e7eaf224-8c61-44ed-a6b0-2ab4dc56f1d0"},{"response":"a8a17efd-f01c-488d-ad44-a31a818cf7d7"},{"response":"bc7c32fc-14a0-4b72-bad4-d345d807f965"},{"response":"727a8d73-cd3b-4096-bc8d-d7cfba12eb44"}]
called addEntity for entity named Operation.
called addEntity for entity named Device.
called addEntity for entity named Room.
Results of all calls to addEntity= [{"response":"6a7e914f-911d-4c6c-a5bc-377afdce4390"},{"response":"56c35237-593d-47f6-9d01-2912fa488760"},{"response":"f1dd440c-2ce3-4a20-a817-a57273f169f3"}]
retrying add examples...

Results of add utterances = [{"response":[{"value":{"UtteranceText":"turn on the lights","ExampleId":-67649},"hasError":false},{"value":{"UtteranceText":"turn the heat on","ExampleId":-69067},"hasError":false},{"value":{"UtteranceText":"switch on the kitchen fan","ExampleId":-3395901},"hasError":false},{"value":{"UtteranceText":"turn off bedroom lights","ExampleId":-85402},"hasError":false},{"value":{"UtteranceText":"turn off air conditioning","ExampleId":-8991572},"hasError":false},{"value":{"UtteranceText":"kill the lights","ExampleId":-70124},"hasError":false},{"value":{"UtteranceText":"dim the lights","ExampleId":-174358},"hasError":false},{"value":{"UtteranceText":"hi how are you","ExampleId":-143722},"hasError":false},{"value":{"UtteranceText":"answer the phone","ExampleId":-69939},"hasError":false},{"value":{"UtteranceText":"are you there","ExampleId":-149588},"hasError":false},{"value":{"UtteranceText":"help","ExampleId":-81949},"hasError":false},{"value":{"UtteranceText":"testing the circuit","ExampleId":-11548708},"hasError":false}]}]
upload done
```




## <a name="open-the-luis-app"></a>LUIS uygulamasını açın
Komut dosyası tamamlandıktan sonra [LUIS'te](luis-reference-regions.md) oturum açabilir ve **My Apps**altında oluşturduğunuz LUIS uygulamasını görebilirsiniz. **TurnOn**, **TurnOff**ve **None** niyetleri altında eklediğiniz söyleyifadeler görmek gerekir.

![TurnOn niyeti](./media/luis-tutorial-node-import-utterances-csv/imported-utterances-661.png)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [UYGULAMANIZI LUIS web sitesinde test edin ve eğitin](luis-interactive-test.md)

## <a name="additional-resources"></a>Ek kaynaklar

Bu örnek uygulama aşağıdaki LUIS API'leri kullanır:
- [uygulama oluşturma](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)
- [niyet ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0c)
- [varlıklar ekleme](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0e) 
- [söyleyiş eklemek](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09)
