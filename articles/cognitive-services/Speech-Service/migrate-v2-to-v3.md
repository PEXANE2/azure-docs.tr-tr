---
title: V2 'den v3 REST API-konuşma hizmetine geçiş
titleSuffix: Azure Cognitive Services
description: V2 'ye kıyasla yeni API sürümü V3, daha küçük bir son değişiklik kümesi içerir. Bu belge, yeni ana sürüme zaman içinde geçiş yapmanıza yardımcı olur.
services: cognitive-services
author: bexxx
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: rbeckers
ms.custom: devx-track-csharp
ms.openlocfilehash: dd1dae963781cc0caacc25938e700a4c70a1f51a
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2020
ms.locfileid: "96738029"
---
# <a name="migration-from-v20-to-v30-of-speech-to-text-rest-api"></a>V 2.0 'dan v 3.0'a konuşmadan metne geçiş REST API

Konuşma REST API 'nin v3 sürümü, güvenilirlik ve kullanım kolaylığı açısından önceki API sürümünü geliştirir. API düzeni diğer Azure veya Bilişsel Hizmetler API'si daha yakından hizalanır. Bu, konuşma API 'imizi kullanırken mevcut becerilerinizi uygulamanıza yardımcı olur.

API 'ye genel bakış, [Swagger belgesi](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)olarak kullanılabilir. Bu, size API 'ye genel bakış sağlamak ve yeni API 'yi test etmek için idealdir.

C# ve Python için örnekler sağlıyoruz. Toplu iş dökümü için, alt dizinin içindeki [GitHub örnek deposundaki](https://aka.ms/csspeech/samples) örnekleri bulacaksınız `samples/batch` .

## <a name="forward-compatibility"></a>İleriye dönük uyumluluk

V3 'e kesintisiz geçiş sağlamak için, v2 'deki tüm varlıkların aynı kimliğin altındaki v3 API 'sinde de bulunabilir. Bir sonuç şeması değişikliği varsa (örneğin, döküm), API 'nin v3 sürümünde GET yanıtları v3 şemasında olur. API 'nin v2 sürümünde Al işlemini gerçekleştirirseniz, sonuç şeması v2 biçiminde olur. V3 üzerinde yeni oluşturulan varlıklar v2 **'de kullanılamaz.**

## <a name="breaking-changes"></a>Yeni değişiklikler

Son değişikliklerin listesi, uyarlanabilmesi için gereken değişikliklerin büyüklüğüne göre sıralanmıştır. Çağıran kodda önemsiz olmayan bir değişiklik gerektiren birkaç değişiklik vardır. Çoğu değişiklik basit yeniden adlandırma gerektirir. Ekiplerin v2 'den birkaç saat arasında birkaç güne kadar bir saat arasında geçiş yapmak için geçen süre. Ancak, daha fazla kararlılık, daha basit kod ve daha hızlı yanıtların sürekliliği, yatırımın hızla kaymasını sağlar. 

### <a name="host-name-changes"></a>Ana bilgisayar adı değişiklikleri

Ana bilgisayar adları {Region}. crsıs. AI, {Region}. api. bilişsel. Microsoft. com ' a değişti. Bu değişiklik içinde, ana bilgisayar adının parçası olduğundan, yollar artık "api/" içermez. Bölgelerin ve yolların tam açıklaması için [Swagger belgesine](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0) bakın.

### <a name="identity-of-an-entity"></a>Bir varlığın kimliği

Özelliği `id` ile değiştirilmiştir `self` . V2 sürümünde bir API kullanıcısının, API 'deki yollarımızı nasıl oluşturduğumuzu öğrenmiş olması gerekiyordu. Bu, Genişletilebilir olmayan ve Kullanıcı tarafından gerekli olan gereksiz çalışmamıştı. Özelliği ( `id` UUID), `self` VARLıĞıN (URL) konumu olan (dize) ile değiştirilmiştir. Değer, tüm varlıklarınız arasında hala benzersizdir. `id`Kodunuzda bir dize olarak depolanıyorsa, yeni şemayı desteklemek için basit bir yeniden adlandırma yeterlidir. Artık, `self` varlığınız IÇIN Rest çağrılarınızın (Get, Patch, Delete) URL olarak içeriğini kullanabilirsiniz.

Varlığın diğer yollar altında kullanılabilir ek işlevleri varsa, bunlar altında listelenir `links` . İyi bir örnek, döküm içeriğine ayrı bir yönteme sahip olan bir dökümdir `GET` .

v2 dökümü:

```json
{
    "id": "9891c965-bb32-4880-b14b-6d44efb158f3",
    "createdDateTime": "2019-01-07T11:34:12Z",
    "lastActionDateTime": "2019-01-07T11:36:07Z",
    "status": "Succeeded",
    "locale": "en-US",
    "name": "Transcription using locale en-US"
}
```

v3 dökümü:

```json
{
    "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions/9891c965-bb32-4880-b14b-6d44efb158f3",
    "createdDateTime": "2019-01-07T11:34:12Z",
    "lastActionDateTime": "2019-01-07T11:36:07Z",
    "status": "Succeeded",
    "locale": "en-US", 
    "displayName": "Transcription using locale en-US",
    "links": {
      "files": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions/9891c965-bb32-4880-b14b-6d44efb158f3/files"
    }
}
```

İstemci uygulamanıza bağlı olarak, özelliğin yeniden adlandırılması yeterli olmayabilir. Döndürülen değerlerini `self` ve `links` rest aramalarınızın hedef URL 'lerini kullanarak, istemcinizdeki yolları oluşturmamanızı öneririz. Döndürülen URL 'Leri kullanarak, yollardaki gelecekteki değişikliklerin istemci kodunuzu bozmadığından emin olabilirsiniz.

### <a name="working-with-collections-of-entities"></a>Varlık koleksiyonlarıyla çalışma

Daha önce v2 API 'SI, bir yanıttaki tüm kullanılabilir varlıkları döndürdü. Beklenen yanıt boyutu üzerinde daha ayrıntılı bir denetime izin vermek için, tüm koleksiyonların yanıtlarının sayfalandırılmış olması gerekir. Döndürülen varlıkların sayısı ve sayfanın kayması üzerinde denetiminiz vardır. Bu davranış, yanıt işlemcisinin çalışma zamanının tahmin edilmesine ve diğer Azure API 'Leriyle tutarlı hale gelmesini kolaylaştırır.

Yanıtın temel şekli tüm koleksiyonlar için aynıdır:

```json
{
    "values": [
        {     
        }
    ],
    "@nextLink": "https://{region}.api.cognitive.microsoft.com/speechtotext/v3.0/{collection}?skip=100&top=100"
}
```

Özelliği, `values` kullanılabilir koleksiyon varlıklarının bir alt kümesini içerir. Sayı ve konum sorgu parametreleri ve kullanılarak denetlenebilir `skip` `top` . `@nextLink`Null olmadığında, daha fazla veri mevcuttur ve bir sonraki veri grubu BIR get işlemi yaparak alınabilir `$.@nextLink` .

Bu değişiklik, `GET` tüm öğeler döndürülünceye kadar bir döngüde koleksiyon için çağrılmasını gerektirir.

### <a name="creating-transcriptions"></a>Döküm oluşturma

Kayıt oluşturma hakkında ayrıntılı bir açıklama, [toplu iş dökümü nasıl yapılır](./batch-transcription.md)bölümünde bulunabilir.

Belirli bir döküm seçeneklerini açıkça ayarlamayı etkinleştirmek için, v3 'de döküm oluşturma değiştirilmiştir. Tüm (isteğe bağlı) yapılandırma özellikleri artık `properties` özellikte ayarlanabilir.
Ayrıca, v3 sürümü artık birden çok giriş dosyasını destekler ve bu nedenle v2 'nin gerektirdiği şekilde tek bir URL 'YI değil, URL 'lerin bir listesini gerektirir. Özellik adı iken olarak yeniden adlandırıldı `recordingsUrl` `contentUrls` . Analbetiklerde yaklaşım çözümleme işlevselliği v3 üzerinde kaldırılmıştır. Bunun yerine Microsoft bilişsel hizmet [metin analizini](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/) kullanmanızı öneririz.

Altındaki yeni özelliği `timeToLive` , `properties` mevcut tamamlanmış varlıkların ayıklalamaya yardımcı olabilir. , `timeToLive` Tamamlanan bir varlığın otomatik olarak silineceği süreyi belirtir. `PT12H`Varlıkların sürekli olarak izlendiği, tüketildiği ve silindiği ve bu nedenle genellikle 12 saat geçmeden önce uzun süre işlendiği bir yüksek değere ayarlayın (örneğin).

v2 istek SONRASı isteği gövdesi:

```json
{
  "locale": "en-US",
  "name": "Transcription using locale en-US",
  "recordingsUrl": "https://contoso.com/mystoragelocation",
  "properties": {
    "AddDiarization": "False",
    "AddWordLevelTimestamps": "False",
    "PunctuationMode": "DictatedAndAutomatic",
    "ProfanityFilterMode": "Masked"
  }
}
```

v3 döküm istek gövdesi:

```json
{
  "locale": "en-US",
  "displayName": "Transcription using locale en-US",
  "contentUrls": [
    "https://contoso.com/mystoragelocation",
    "https://contoso.com/myotherstoragelocation"
  ],
  "properties": {
    "diarizationEnabled": false,
    "wordLevelTimestampsEnabled": false,
    "punctuationMode": "DictatedAndAutomatic",
    "profanityFilterMode": "Masked"
  }
}
```

### <a name="format-of-v3-transcription-results"></a>V3 dökümü sonuçlarının biçimi

Döküm sonuçlarının şeması, gerçek zamanlı uç noktalar tarafından oluşturulan dökümlerle hizalanacak şekilde biraz değiştirilmiştir. Yeni biçimin ayrıntılı bir açıklaması [toplu iş dökümü Ile nasıl yapılır?](./batch-transcription.md)bölümünde bulunabilir. Sonucun şeması, altında [GitHub örnek depomızda](https://aka.ms/csspeech/samples) yayımlanır `samples/batch/transcriptionresult_v3.schema.json` .

Özellik adları artık Camel-cased ve kanal ve konuşmacı değerleri tamsayı türlerini kullanıyor. Diğer Azure API 'Leriyle süreler biçimini hizalamak için, artık ISO 8601 ' de açıklandığı gibi biçimlendirilir.

Bir v3 dökümü sonucunun örneği. Farklar açıklamalarda açıklanmıştır.

```json
{
  "source": "...",                      // (new in v3) was AudioFileName / AudioFileUrl
  "timestamp": "2020-06-16T09:30:21Z",  // (new in v3)
  "durationInTicks": 41200000,          // (new in v3) was AudioLengthInSeconds
  "duration": "PT4.12S",                // (new in v3)
  "combinedRecognizedPhrases": [        // (new in v3) was CombinedResults
    {
      "channel": 0,                     // (new in v3) was ChannelNumber
      "lexical": "hello world",
      "itn": "hello world",
      "maskedITN": "hello world",
      "display": "Hello world."
    }
  ],
  "recognizedPhrases": [                // (new in v3) was SegmentResults
    {
      "recognitionStatus": "Success",   // 
      "channel": 0,                     // (new in v3) was ChannelNumber
      "offset": "PT0.07S",              // (new in v3) new format, was OffsetInSeconds
      "duration": "PT1.59S",            // (new in v3) new format, was DurationInSeconds
      "offsetInTicks": 700000.0,        // (new in v3) was Offset
      "durationInTicks": 15900000.0,    // (new in v3) was Duration

      // possible transcriptions of the current phrase with confidences
      "nBest": [
        {
          "confidence": 0.898652852,phrase
          "speaker": 1,
          "lexical": "hello world",
          "itn": "hello world",
          "maskedITN": "hello world",
          "display": "Hello world.",

          "words": [
            {
              "word": "hello",
              "offset": "PT0.09S",
              "duration": "PT0.48S",
              "offsetInTicks": 900000.0,
              "durationInTicks": 4800000.0,
              "confidence": 0.987572
            },
            {
              "word": "world",
              "offset": "PT0.59S",
              "duration": "PT0.16S",
              "offsetInTicks": 5900000.0,
              "durationInTicks": 1600000.0,
              "confidence": 0.906032
            }
          ]
        }
      ]
    }
  ]
}
```

### <a name="getting-the-content-of-entities-and-the-results"></a>Varlıkların içeriğini ve sonuçlarını alma

V2 'de, giriş veya sonuç dosyalarının bağlantıları, varlık meta verileri geri kalanı ile birlikte satır içine alındı. V3 'de bir geliştirme olarak, varlık meta verileri arasında bir GET on ile döndürülen `$.self` ve sonuç dosyalarına erişmek için Ayrıntılar ve kimlik bilgileri arasında net bir ayrım vardır. Bu ayrım, müşteri verilerinin korunmasına yardımcı olur ve kimlik bilgilerinin geçerlilik süresi boyunca ince denetim sağlar.

V3 'de, `files` varlık verileri (veri kümeleri, döküm, uç noktalar, değerlendirmeler) kullanıma sunabilmeniz durumunda bağlantılar altında çağrılan bir özellik vardır. Bir GET on, `$.links.files` her dosyanın içeriğine erişmek için dosya ve SAS URL 'sinin bir listesini döndürür. SAS URL 'Lerinin geçerlilik süresini denetlemek için sorgu parametresi, `sasValidityInSeconds` yaşam süresini belirtmek için kullanılabilir.

v2 dökümü:

```json
{
  "id": "9891c965-bb32-4880-b14b-6d44efb158f3",
  "status": "Succeeded",
  "reportFileUrl": "https://contoso.com/report.txt?st=2018-02-09T18%3A07%3A00Z&se=2018-02-10T18%3A07%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=6c044930-3926-4be4-be76-f728327c53b5",
  "resultsUrls": {
    "channel_0": "https://contoso.com/audiofile1.wav?st=2018-02-09T18%3A07%3A00Z&se=2018-02-10T18%3A07%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=6c044930-3926-4be4-be76-f72832e6600c",
    "channel_1": "https://contoso.com/audiofile2.wav?st=2018-02-09T18%3A07%3A00Z&se=2018-02-10T18%3A07%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=3e0163f1-0029-4d4a-988d-3fba7d7c53b5"
  }
}
```

v3 dökümü:

```json
{
    "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions/9891c965-bb32-4880-b14b-6d44efb158f3",
    "links": {
      "files": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions/9891c965-bb32-4880-b14b-6d44efb158f3/files"
    } 
}
```

Bundan sonra bir GET ile `$.links.files` sonuçlanır:

```json
{
  "values": [
    {
      "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions/9891c965-bb32-4880-b14b-6d44efb158f3/files/f23e54f5-ed74-4c31-9730-2f1a3ef83ce8",
      "name": "Name",
      "kind": "Transcription",
      "properties": {
        "size": 200
      },
      "createdDateTime": "2020-01-13T08:00:00Z",
      "links": {
        "contentUrl": "https://customspeech-usw.blob.core.windows.net/artifacts/mywavefile1.wav.json?st=2018-02-09T18%3A07%3A00Z&se=2018-02-10T18%3A07%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=e05d8d56-9675-448b-820c-4318ae64c8d5"
      }
    },
    {
      "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions/9891c965-bb32-4880-b14b-6d44efb158f3/files/28bc946b-c251-4a86-84f6-ea0f0a2373ef",
      "name": "Name",
      "kind": "TranscriptionReport",
      "properties": {
        "size": 200
      },
      "createdDateTime": "2020-01-13T08:00:00Z",
      "links": {
        "contentUrl": "https://customspeech-usw.blob.core.windows.net/artifacts/report.json?st=2018-02-09T18%3A07%3A00Z&se=2018-02-10T18%3A07%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=e05d8d56-9675-448b-820c-4318ae64c8d5"
      }
    }
  ],
  "@nextLink": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions/9891c965-bb32-4880-b14b-6d44efb158f3/files?skip=2&top=2"
}
```

, `kind` Dosyanın içerik biçimini gösterir. Döküm için, tür dosyaları `TranscriptionReport` işin özetidir ve tür dosyaları `Transcription` işin kendisinin sonucudur.

### <a name="customizing-models"></a>Modelleri özelleştirme

V3 'den önce, bir model eğitilirken "akustik model" ve "dil modeli" arasında bir ayrım vardı. Bu ayrım, uç noktalar veya döküm oluştururken birden çok model belirtme gereksiniminden sonuçlandı. Bu işlemi bir arayan için basitleştirmek amacıyla, farkları kaldırdık ve model eğitimi için kullanılmakta olan veri kümelerinin içeriğine bağımlı hale getirildik. Bu değişiklik ile model oluşturma artık karışık veri kümelerini (dil verileri ve akustik veriler) desteklemektedir. Uç noktalar ve döküm artık yalnızca bir model gerektirir.

Bu değişiklik ile, GÖNDERIYLE ilgili bir a gereksinimi `kind` kaldırılmıştır ve `datasets[]` artık aynı veya karışık türde birden çok veri kümesi içerebilir.

Eğitilen bir modelin sonuçlarını geliştirmek için, akustik veriler otomatik olarak dil eğitimi için kullanılır. Genel olarak, v3 API 'SI ile oluşturulan modeller v2 API 'siyle oluşturulan modellerden daha doğru sonuçlar sunar.

### <a name="retrieving-base-and-custom-models"></a>Temel ve özel modelleri alma

V3, kullanılabilir modellerin alınmasını kolaylaştırmak için "temel modeller" koleksiyonlarını müşterinin "özelleştirilmiş modeller" sahipliğinde ayırmıştır. İki yol artık `GET /speechtotext/v3.0/models/base` ve ' dir `GET /speechtotext/v3.0/models/` .

Daha önce tüm modeller tek bir yanıtta birlikte döndürülür.

### <a name="name-of-an-entity"></a>Bir varlığın adı

Özelliğin adı `name` olarak yeniden adlandırılır `displayName` . Bu, kimlik özelliklerini belirtmemelidir diğer Azure API 'Lerine hizalanır. Bu özelliğin değeri benzersiz olmamalı ve, ile varlık oluşturulduktan sonra değiştirilebilir `PATCH` .

v2 dökümü:

```json
{
    "name": "Transcription using locale en-US"
}
```

v3 dökümü:

```json
{
    "displayName": "Transcription using locale en-US"
}
```

### <a name="accessing-referenced-entities"></a>Başvurulan varlıklara erişme

V2 'ye Başvurulmuş varlıklar, örneğin bir uç noktanın kullanılan modelleri gibi her zaman satır içine alınmış olmalıdır. Varlıkların iç içe geçirilmesi büyük yanıtlar ve tüketiciler tarafından nadiren iç içe geçmiş içeriğe neden olarak tüketildi. Yanıt boyutunu küçültmek ve tüm API kullanıcıları için performansı artırmak üzere, başvurulan varlıklar yanıtta artık satır içine alınmaz. Bunun yerine, doğrudan kullanılabilecek olan diğer varlığa yönelik bir başvuru kullanılır. Bu başvuru, `GET` bağlantı ile aynı kalıbı takip eden bir sonraki (URL de BIR URL) için kullanılabilir `self` .

v2 dökümü:

```json
{
  "id": "9891c965-bb32-4880-b14b-6d44efb158f3",
  "models": [
    {
      "id": "827712a5-f942-4997-91c3-7c6cde35600b",
      "modelKind": "Language",
      "lastActionDateTime": "2019-01-07T11:36:07Z",
      "status": "Running",
      "createdDateTime": "2019-01-07T11:34:12Z",
      "locale": "en-US",
      "name": "Acoustic model",
      "description": "Example for an acoustic model",
      "datasets": [
        {
          "id": "702d913a-8ba6-4f66-ad5c-897400b081fb",
          "dataImportKind": "Language",
          "lastActionDateTime": "2019-01-07T11:36:07Z",
          "status": "Succeeded",
          "createdDateTime": "2019-01-07T11:34:12Z",
          "locale": "en-US",
          "name": "Language dataset",
          
        }
      ]
    },
  ]
}
```

v3 dökümü:

```json
{
  "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions/9891c965-bb32-4880-b14b-6d44efb158f3",
  "model": {
    "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/models/021a72d0-54c4-43d3-8254-27336ead9037"
  }
}
```

Yukarıdaki örnekte gösterildiği gibi, başvurulan bir modelin ayrıntılarını kullanmanız gerekirse, GET on ile ilgili konuyu kolaylaştırın `$.model.self` .

### <a name="retrieving-endpoint-logs"></a>Uç nokta günlüklerini alma

Hizmetin sürüm v2, uç noktaların yanıtlarının günlüğe kaydedilmesini destekliyordu. Bir uç noktanın sonuçlarını v2 ile almak için, birinin bir zaman aralığı tarafından tanımlanan sonuçların anlık görüntüsünü temsil eden bir "veri dışarı aktarma" oluşturması gerekiyordu. Veri yığınlarının dışarı aktarılması işlemi, esnek hale getirilir. V3 API her bir dosyaya erişim sağlar ve bunlara yineleme sağlar.

V3 uç noktası başarıyla çalıştırılıyor:

```json
{
  "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/endpoints/afa0669c-a01e-4693-ae3a-93baf40f26d6",
  "links": {
    "logs": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/endpoints/afa0669c-a01e-4693-ae3a-93baf40f26d6/files/logs" 
  }
}
```

GET yanıtı `$.links.logs` :

```json
{
  "values": [
    {
      "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/endpoints/6d72ad7e-f286-4a6f-b81b-a0532ca6bcaa/files/logs/2019-09-20_080000_3b5f4628-e225-439d-bd27-8804f9eed13f.wav",
      "name": "2019-09-20_080000_3b5f4628-e225-439d-bd27-8804f9eed13f.wav",
      "kind": "Audio",
      "properties": {
        "size": 12345
      },
      "createdDateTime": "2020-01-13T08:00:00Z",
      "links": {
        "contentUrl": "https://customspeech-usw.blob.core.windows.net/artifacts/2019-09-20_080000_3b5f4628-e225-439d-bd27-8804f9eed13f.wav?st=2018-02-09T18%3A07%3A00Z&se=2018-02-10T18%3A07%3A00Z&sp=rl&sv=2017-04-17&sr=b&sig=e05d8d56-9675-448b-820c-4318ae64c8d5"
      }
    }    
  ],
  "@nextLink": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/endpoints/afa0669c-a01e-4693-ae3a-93baf40f26d6/files/logs?top=2&SkipToken=2!188!MDAwMDk1ITZhMjhiMDllLTg0MDYtNDViMi1hMGRkLWFlNzRlOGRhZWJkNi8yMDIwLTA0LTAxLzEyNDY0M182MzI5NGRkMi1mZGYzLTRhZmEtOTA0NC1mODU5ZTcxOWJiYzYud2F2ITAwMDAyOCE5OTk5LTEyLTMxVDIzOjU5OjU5Ljk5OTk5OTlaIQ--"
}
```

Uç nokta günlükleri için sayfalandırma, hiçbir uzaklığa izin belirtilmedikçe diğer tüm koleksiyonlara benzer şekilde işler. Büyük miktarda kullanılabilir veri nedeniyle, sunucu odaklı sayfalandırma uygulanmalıdır.

V3 'de, her bir uç nokta günlüğü, bir dosyanın üzerinde bir SILME göndererek `self` veya silme açık kullanılarak tek tek silinebilir `$.links.logs` . Son verileri belirtmek için, isteğe sorgu parametresi `endDate` eklenebilir.

### <a name="using-custom-properties"></a>"Özel" özellikleri kullanma

İsteğe bağlı yapılandırma özelliklerinden özel özellikleri ayırmak için, açıkça adlandırılmış tüm özellikler artık `properties` özellikte bulunur ve çağıranlar tanımlanmış tüm özellikler artık `customProperties` özellikte bulunur.

v2 dökümü varlığı

```json
{
  "properties": {
    "customerDefinedKey": "value",
    "diarizationEnabled": "False",
    "wordLevelTimestampsEnabled": "False"
  }
}
```

v3 dökümü varlığı

```json
{
  "properties": {
    "diarizationEnabled": false,
    "wordLevelTimestampsEnabled": false 
  },
  "customProperties": {
    "customerDefinedKey": "value"
  }
}
```

Bu değişiklik Ayrıca `properties` (dize yerine bool gibi) tüm açıkça adlandırılmış özellikler üzerinde doğru türlerin kullanımını da etkinleştirdi.

### <a name="response-headers"></a>Yanıt üst bilgileri

V3, `Operation-Location` POST istekleri üzerine üstbilgiye ek olarak artık üstbilgiyi döndürmez `Location` . Her iki üst bilgilerin de tam olarak aynı olması için kullanılan değer. Şimdi yalnızca `Location` döndürülüyor.

Yeni API sürümü artık Azure API Management (APıM) tarafından yönetildiğinden, ilgili üst bilgiler üzerinde kısıtlama, `X-RateLimit-Limit` `X-RateLimit-Remaining` ve `X-RateLimit-Reset` yanıt üst bilgilerinde yer almayan bir.

### <a name="accuracy-tests"></a>Doğruluk testleri

Yeni ad, ne kadar iyi temsil ettiğini açıkladığı için doğruluk testleri değerlendirmelere yeniden adlandırıldı. Haber yolları "https://{Region}. api. bilişsel. Microsoft. com/speechtotext/v 3.0/değerlendirmeleri" gibi bir örnektir.
