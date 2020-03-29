---
title: Extact metin eşleşen varlıklar - LUIS
titleSuffix: Azure Cognitive Services
description: Luis'in bir sözcük veya tümcecik varyasyonlarını etiketlemesine yardımcı olmak için liste varlığı eklemeyi öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: diberry
ms.openlocfilehash: f3c99856eaffc454754618a1eac34630b985a77e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73499485"
---
# <a name="use-a-list-entity-to-increase-entity-detection"></a>Varlık algılamayı artırmak için liste varlığı kullanma 
Bu makalede, varlık algılamaartırmak için bir [liste varlık](luis-concept-entity-types.md) kullanımını göstermektedir. Liste varlıklarının terimlerin tam uyumlu olması nedeniyle etiketlenmeleri gerekmez.  

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Liste varlığı oluşturma 
> * Normalleştirilmiş değerler ve eşanlamlılar ekleme
> * Geliştirilmiş varlık tanımlamayı doğrulama

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Son [Düğüm.js](https://nodejs.org)
> * [AnasayfaAutomation LUIS uygulaması](luis-get-started-create-app.md). Ev Otomasyonu uygulaması oluşturulmuyorsa, yeni bir uygulama oluşturun ve Önceden Oluşturulmuş Alan Adı Ana **Otomasyonu'nu**ekleyin. Uygulamayı eğitin ve yayımlayın. 
> * [AuthoringKey](luis-concept-keys.md#authoring-key), [EndpointKey](luis-concept-keys.md#endpoint-key) (birçok kez sorgulanıyorsanız), uygulama kimliği, sürüm kimliği ve LUIS uygulaması için [bölge.](luis-reference-regions.md)

> [!Tip]
> Zaten bir aboneliğiniz yoksa, ücretsiz bir [hesaba](https://azure.microsoft.com/free/)kaydolabilirsiniz.

Bu makaledeki kodun tümü [Azure Örnekleri GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-list-entity)mevcuttur. 

## <a name="use-homeautomation-app"></a>HomeAutomation uygulamasını kullanma
HomeAutomation uygulaması, ışıklar, eğlence sistemleri ve ısıtma ve soğutma gibi ortam kontrolleri gibi cihazların kontrolünü sağlar. Bu sistemler, Üretici adları, takma adlar, kısaltmalar ve argo içerebilir birkaç farklı adları vardır. 

Farklı kültürler ve demografik birçok isim vardır bir sistem termostat olduğunu. Bir termostat bir ev veya bina için hem soğutma hem de ısıtma sistemlerini kontrol edebilir.

İdeal olarak aşağıdaki söyleyerek Önceden oluşturulmuş varlık **HomeAutomation.Device**çözmek gerekir:

|#|Söyleyiş|tanımlanan varlık|puan|
|--|--|--|--|
|1|ac açmak|HomeAutomation.Device - "ac"|0.8748562|
|2|ısı açmak|HomeAutomation.Device - "ısı"|0.784990132|
|3|daha soğuk yapmak|||

İlk iki söyleyiş farklı aygıtlarla eşlenir. Üçüncü söz, "daha soğuk yapmak", bir cihaz için harita değil, bunun yerine bir sonuç istiyor. LUIS, "daha soğuk" teriminin termostatın istenen cihaz olduğunu bilmiyor. İdeal olarak, LUIS aynı cihaza tüm bu söyleyiş çözmek gerekir. 

## <a name="use-a-list-entity"></a>Liste varlığı kullanma
HomeAutomation.Device varlığı az sayıda aygıt veya birkaç ad varyasyonu yla mükemmeldir. Bir ofis binası veya kampüs için cihaz adları HomeAutomation.Device varlığının kullanışlılığının ötesinde büyür. 

Bir bina veya kampüsteki bir aygıtın terimler kümesi büyük bir küme olsa bile, **liste varlığı** bu senaryo için iyi bir seçimdir. Bir liste varlığı kullanarak, LUIS termostat için kümedeki olası herhangi bir değeri alabilir ve bunu sadece tek bir cihazolan "termostat" olarak çözebilir. 

Bu makale, termostat ile bir varlık listesi oluşturmak için gidiyor. Bu makalede bir termostat için alternatif isimler şunlardır: 

|termostat için alternatif isimler|
|--|
| Ac |
| Klima|
| a-c|
|ısıtıcı|
|Kısayol|
|Sıcak|
|Soğuk|
|Soğuk|

LUIS sık sık yeni bir alternatif belirlemek gerekiyorsa, o zaman bir [ifade listesi](luis-concept-feature.md#how-to-use-phrase-lists) daha iyi bir cevaptır.

## <a name="create-a-list-entity"></a>Liste varlığı oluşturma
Bir Düğüm.js dosyası oluşturun ve aşağıdaki kodu kopyalayın. AuthoringKey, appId, versionId ve bölge değerlerini değiştirin.

   [!code-javascript[Create DevicesList List Entity](~/samples-luis/documentation-samples/tutorial-list-entity/add-entity-list.js "Create DevicesList List Entity")]

NPM bağımlılıklarını yüklemek ve liste varlığını oluşturmak için kodu çalıştırmak için aşağıdaki komutu kullanın:

```console
npm install && node add-entity-list.js
```

Çalıştırmanın çıktısı liste varlığının kimliğidir:

```console
026e92b3-4834-484f-8608-6114a83b03a6
```

## <a name="train-the-model"></a>Modeli eğitme
Yeni listenin sorgu sonuçlarını etkilemesi için LUIS'i eğitin. Eğitim, eğitimin yapIlip yapılmadığını kontrol etmek için iki bölümden oluşan bir eğitim sürecidir. Birçok modele sahip bir uygulamanın eğitilen birkaç dakika sürebilir. Aşağıdaki kod uygulamayı eğitir ve eğitim başarılı olana kadar bekler. Kod, 429 "Çok fazla istek" hatasını önlemek için bir bekleme ve yeniden deneme stratejisi kullanır. 

Bir Düğüm.js dosyası oluşturun ve aşağıdaki kodu kopyalayın. AuthoringKey, appId, versionId ve bölge değerlerini değiştirin.

   [!code-javascript[Train LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/train.js "Train LUIS")]

Uygulamayı eğitmek için kodu çalıştırmak için aşağıdaki komutu kullanın:

```console
node train.js
```

Çalıştırmanın çıktısı, LUIS modellerinin eğitiminin her yinelemesinin durumudur. Aşağıdaki yürütme eğitim sadece bir kontrol gerekli:

```console
1 trained = true
[ { modelId: '2c549f95-867a-4189-9c35-44b95c78b70f',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '5530e900-571d-40ec-9c78-63e66b50c7d4',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '519faa39-ae1a-4d98-965c-abff6f743fe6',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9671a485-36a9-46d5-aacd-b16d05115415',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9ef7d891-54ab-48bf-8112-c34dcd75d5e2',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '8e16a660-8781-4abf-bf3d-f296ebe1bf2d',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } } ]

```
## <a name="publish-the-model"></a>Modeli yayımlama
Liste varlığının bitiş noktasından edinilebilecek şekilde yayımlayın.

Bir Düğüm.js dosyası oluşturun ve aşağıdaki kodu kopyalayın. EndpointKey, appId ve bölge değerlerini değiştirin. Bu dosyayı kota sınırınızın ötesinde aramayı planlamıyorsanız authoringKey'inizi kullanabilirsiniz.

   [!code-javascript[Publish LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/publish.js "Publish LUIS")]

Uygulamayı sorgulamak için kodu çalıştırmak için aşağıdaki komutu kullanın:

```console
node publish.js
```

Aşağıdaki çıktı, tüm sorgular için bitiş noktası url'sini içerir. Gerçek JSON sonuçları gerçek appID içerecektir. 

```json
{ 
  versionId: null,
  isStaging: false,
  endpointUrl: 'https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>',
  region: null,
  assignedEndpointKey: null,
  endpointRegion: 'westus',
  publishedDateTime: '2018-01-29T22:17:38Z' }
}
```

## <a name="query-the-app"></a>Uygulamayı sorgula 
Liste varlığının LUIS'in aygıt türünü belirlemesine yardımcı olduğunu kanıtlamak için uygulamayı bitiş noktasından sorgulayın.

Bir Düğüm.js dosyası oluşturun ve aşağıdaki kodu kopyalayın. EndpointKey, appId ve bölge değerlerini değiştirin. Bu dosyayı kota sınırınızın ötesinde aramayı planlamıyorsanız authoringKey'inizi kullanabilirsiniz.

   [!code-javascript[Query LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/query.js "Query LUIS")]

Kodu çalıştırmak ve uygulamayı sorgulamak için aşağıdaki komutu kullanın:

```console
node train.js
```

Çıktı sorgu sonuçlarıdır. Kod sorgu dizesine **ayrıntılı** ad/değer çiftini eklediklerinden, çıktı tüm amaçları ve puanlarını içerir:

```json
{
  "query": "turn up the heat",
  "topScoringIntent": {
    "intent": "HomeAutomation.TurnOn",
    "score": 0.139018849
  },
  "intents": [
    {
      "intent": "HomeAutomation.TurnOn",
      "score": 0.139018849
    },
    {
      "intent": "None",
      "score": 0.120624863
    },
    {
      "intent": "HomeAutomation.TurnOff",
      "score": 0.06746891
    }
  ],
  "entities": [
    {
      "entity": "heat",
      "type": "HomeAutomation.Device",
      "startIndex": 12,
      "endIndex": 15,
      "score": 0.784990132
    },
    {
      "entity": "heat",
      "type": "DevicesList",
      "startIndex": 12,
      "endIndex": 15,
      "resolution": {
        "values": [
          "Thermostat"
        ]
      }
    }
  ]
}
```

**Termostat'ın** özel cihazı, "ısıyı aç" sorgusu yla tanımlanır. Orijinal HomeAutomation.Device varlığı hala uygulamada olduğundan, sonuçlarını da görebilirsiniz. 

Onlar da bir termostat olarak iade olduğunu görmek için diğer iki söyleyiş deneyin. 

|#|Söyleyiş|varlık|type|value|
|--|--|--|--|--|
|1|ac açmak| Ac | Cihazlar Listesi | Termostat|
|2|ısı açmak|ısı| Cihazlar Listesi |Termostat|
|3|daha soğuk yapmak|Soğuk|Cihazlar Listesi|Termostat|

## <a name="next-steps"></a>Sonraki adımlar

Aygıt konumlarını odalara, zeminlere veya binalara genişletmek için başka bir Liste varlığı oluşturabilirsiniz. 
