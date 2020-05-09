---
title: Konuşmadan metne API başvurusu (REST)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmayı metne REST API nasıl kullanacağınızı öğrenin. Bu makalede yetkilendirme seçenekleri, sorgu seçenekleri, bir isteği nasıl yapılandıracağınızı ve yanıt alabileceğinizi öğreneceksiniz.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/23/2020
ms.author: yinhew
ms.openlocfilehash: 2f102199c14ba9611a83e3ed3b31ebcd189624d6
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82978629"
---
# <a name="speech-to-text-rest-api"></a>Konuşmayı metne dönüştürme REST API'si

Konuşma [SDK 'sına](speech-sdk.md)alternatif olarak, konuşma hizmeti bir REST API kullanarak konuşmayı metne dönüştürmenize olanak tanır. Her erişilebilir uç nokta bir bölgeyle ilişkilendirilir. Uygulamanız, kullanmayı planladığınız uç nokta için bir abonelik anahtarı gerektirir. REST API çok sınırlıdır ve yalnızca [konuşma SDK 'sının](speech-sdk.md) olmaması durumunda kullanılmalıdır.

Konuşmayı metne REST API kullanmadan önce, şunu anlayın:

* REST API ve doğrudan ses iletimi kullanan istekler yalnızca en fazla 60 saniyelik ses içerebilir.
* Konuşmadan metne REST API yalnızca nihai sonuçları döndürür. Kısmi sonuçlar sağlanmaz.

Daha uzun bir ses gönderdiğinizde uygulamanız için bir gereklilik varsa, [konuşma SDK 'sını](speech-sdk.md) veya [toplu iş dökümü](batch-transcription.md)gibi dosya tabanlı REST API kullanmayı düşünün.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Bölgeler ve uç noktalar

REST API uç noktası şu biçimdedir:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

Bu `<REGION_IDENTIFIER>` tablodaki aboneliğinizin bölgesiyle eşleşen tanımlayıcıyla değiştirin:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> Bir 4xx HTTP hatası almamak için dil parametresi URL 'ye eklenmelidir. Örneğin, Batı ABD uç noktası kullanılarak dil ABD Ingilizcesi olarak ayarlanmıştır: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.

## <a name="query-parameters"></a>Sorgu parametreleri

Bu parametreler REST isteğinin sorgu dizesine dahil edilebilir.

| Parametre | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `language` | Tanınmakta olan konuşulan dili tanımlar. [Desteklenen diller](language-support.md#speech-to-text)bölümüne bakın. | Gerekli |
| `format` | Sonuç biçimini belirtir. Kabul edilen değerler `simple` şunlardır `detailed`. Basit sonuçlar, `RecognitionStatus`, `DisplayText`, `Offset`ve `Duration`içerir. Ayrıntılı yanıtlar, görüntü metninin dört farklı temsilini içerir. Varsayılan ayar `simple` değeridir. | İsteğe Bağlı |
| `profanity` | Tanıma sonuçlarında küfür nasıl işleneceğini belirtir. Kabul edilen değerler `masked`, küfür ile bir bütün küfür kaldıran, ya `removed` `raw`da sonuçtaki küfür da dahil olmak üzere yıldız işaretiyle değiştirilir. Varsayılan ayar `masked` değeridir. | İsteğe Bağlı |
| `pronunciationScoreParams` | Tanınma sonuçlarında telaffuz puanlarını göstermek için parametreleri belirtir; doğruluk, akıcı, tamamlanma, vb. göstergeler ile konuşma girişi için telaffuz kalitesini değerlendirir. Bu parametre, birden çok ayrıntılı parametre içeren Base64 kodlamalı JSON 'dir. Bu parametrenin nasıl oluşturulacağı için [telaffuz değerlendirmesi parametrelerine](#pronunciation-assessment-parameters) bakın. | İsteğe Bağlı |
| `cid` | Özel modeller oluşturmak için [özel konuşma tanıma portalını](how-to-custom-speech.md) kullanırken, **dağıtım** SAYFASıNDA bulunan **uç nokta kimlikleri** aracılığıyla özel modeller kullanabilirsiniz. Sorgu dizesi parametresinin bağımsız değişkeni olarak **uç nokta kimliğini** kullanın. `cid` | İsteğe Bağlı |

## <a name="request-headers"></a>İstek üst bilgileri

Bu tabloda, konuşma-metin istekleri için gerekli ve isteğe bağlı üstbilgiler listelenmektedir.

|Üst bilgi| Açıklama | Gerekli/Isteğe bağlı |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Konuşma hizmeti abonelik anahtarınız. | Bu üst bilgi ya `Authorization` da gerekli. |
| `Authorization` | Bir yetkilendirme belirteci öncesinde kelimedir `Bearer`. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Bu üst bilgi ya `Ocp-Apim-Subscription-Key` da gerekli. |
| `Content-type` | Belirtilen ses verilerinin biçimini ve codec 'ini açıklar. Kabul edilen değerler `audio/wav; codecs=audio/pcm; samplerate=16000` şunlardır `audio/ogg; codecs=opus`. | Gerekli |
| `Transfer-Encoding` | Tek bir dosya yerine, öbekli ses verilerinin gönderileceğini belirtir. Yalnızca ses verilerini parçalama durumunda bu üstbilgiyi kullanın. | İsteğe Bağlı |
| `Expect` | Öbekli aktarım kullanılıyorsa, gönderin `Expect: 100-continue`. Konuşma hizmeti, ilk isteği ve bekleek verileri onaylar.| Öbekli ses verileri gönderiyorsanız gereklidir. |
| `Accept` | Sağlanmışsa, olmalıdır `application/json`. Konuşma hizmeti, sonuçları JSON ile sağlar. Bazı istek çerçeveleri uyumsuz bir varsayılan değer sağlar. Her zaman dahil `Accept`etmek iyi bir uygulamadır. | İsteğe bağlı, ancak önerilir. |

## <a name="audio-formats"></a>Ses biçimleri

HTTP `POST` isteğinin gövdesinde ses gönderilir. Bu tablodaki biçimlerden birinde olmalıdır:

| Biçimlendir | Bileşeni | Bit hızı | Örnek hız  |
|--------|-------|----------|--------------|
| WAV    | PCM   | 256 kbps | 16 kHz, mono |
| OGG    | OPUS 'LAR  | 256 kpbs | 16 kHz, mono |

>[!NOTE]
>Yukarıdaki biçimler, konuşma hizmetindeki REST API ve WebSocket aracılığıyla desteklenir. [Konuşma SDK 'sı](speech-sdk.md) Şu anda, PCM codec ve [DIĞER biçimlere](how-to-use-codec-compressed-audio-input-streams.md)sahip WAV biçimini desteklemektedir.

## <a name="pronunciation-assessment-parameters"></a>Telaffuz değerlendirme parametreleri

Bu tabloda, telaffuz değerlendirmesi için gerekli ve isteğe bağlı parametreler listelenmektedir.

| Parametre | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| ReferenceText | Telaffuz tarafından değerlendirilecek metin. | Gerekli |
| GradingSystem | Puan ayarlaması için nokta sistemi. Kabul edilen değerler `FivePoint` şunlardır `HundredMark`. Varsayılan ayar `FivePoint` değeridir. | İsteğe Bağlı |
| Ayrıntı düzeyi | Değerlendirme ayrıntı düzeyi. Kabul edilen değerler `Phoneme`, tam metin ve sözcük düzeyindeki `Word` `FullText`puanı gösteren ve tam metin düzeyinde puan gösteren, Word ve Fonem düzeyindeki puanı gösteren kabul edilir. Varsayılan ayar `Phoneme` değeridir. | İsteğe Bağlı |
| Boyut | Çıkış ölçütünü tanımlar. Kabul edilen değerler `Basic`yalnızca doğruluk puanı ' nı gösterir, daha `Comprehensive` fazla boyutlara ilişkin puanları gösterir (örneğin, tam metin düzeyinde, akıcı puan ve tamamlayıcı puanı, sözcük düzeyinde hata türü). Farklı puan boyutlarının tanımlarını ve sözcük hata türlerini görmek için [yanıt parametrelerini](#response-parameters) denetleyin. Varsayılan ayar `Basic` değeridir. | İsteğe Bağlı |
| EnableMiscue | Hatalı işaret hesaplamasını etkinleştirilir. Bu etkinken, bulunan sözcükler başvuru metniyle karşılaştırılır ve karşılaştırmaya göre atlama/ekleme ile işaretlenir. Kabul edilen değerler `False` şunlardır `True`. Varsayılan ayar `False` değeridir. | İsteğe Bağlı |
| ScenarioId | Özelleştirilmiş bir nokta sistemini gösteren bir GUID. | İsteğe Bağlı |

Aşağıda, telaffuz değerlendirmesi parametrelerini içeren bir JSON örneği verilmiştir:

```json
{
  "ReferenceText": "Good morning.",
  "GradingSystem": "HundredMark",
  "Granularity": "FullText",
  "Dimension": "Comprehensive"
}
```

Aşağıdaki örnek kod, URL sorgu parametresine telaffuz değerlendirmesi parametrelerinin nasıl oluşturulacağını gösterir:

```csharp
var pronunciationScoreParamsJson = $"{{\"ReferenceText\":\"Good morning.\",\"GradingSystem\":\"HundredMark\",\"Granularity\":\"FullText\",\"Dimension\":\"Comprehensive\"}}";
var pronunciationScoreParamsBytes = Encoding.UTF8.GetBytes(pronunciationScoreParamsJson);
var pronunciationScoreParams = Convert.ToBase64String(pronunciationScoreParamsBytes);
```

## <a name="sample-request"></a>Örnek istek

Aşağıdaki örnekte konak adı ve gerekli üst bilgiler yer alır. Hizmetin bu örneğe dahil olmayan ses verilerini de beklediğini unutmayın. Daha önce belirtildiği gibi, öbek oluşturma önerilir, ancak gerekli değildir.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

## <a name="http-status-codes"></a>HTTP durum kodu

Her yanıt için HTTP durum kodu başarı veya genel hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| `100` | Devam et | İlk istek kabul edildi. Verilerin geri kalanını göndermeye devam edin. (Öbekli aktarımlı olarak kullanılır) |
| `200` | Tamam | İstek başarılı oldu; yanıt gövdesi bir JSON nesnesidir. |
| `400` | Hatalı istek | Dil kodu sağlanmadı, desteklenen bir dil değil, geçersiz ses dosyası, vb. |
| `401` | Yetkisiz | Belirtilen bölgede veya geçersiz uç noktada abonelik anahtarı veya yetkilendirme belirteci geçersiz. |
| `403` | Yasak | Abonelik anahtarı veya yetkilendirme belirteci eksik. |

## <a name="chunked-transfer"></a>Öbekli aktarım

Öbekli aktarım`Transfer-Encoding: chunked`(), tanınma gecikmesini azaltmaya yardımcı olabilir. Konuşma hizmetinin, aktarım sırasında ses dosyasını işlemeye başlamasını sağlar. REST API kısmi veya geçici sonuçlar sağlamıyor.

Bu kod örneği, öbekte nasıl ses gönderileceğini gösterir. Yalnızca ilk öbek, ses dosyasının üst bilgisini içermelidir. `request`, uygun `HttpWebRequest` REST uç noktasına bağlı bir nesnedir. `audioFile`diskteki bir ses dosyasının yoludur.

```csharp
var request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = host;
request.ContentType = @"audio/wav; codecs=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";
request.AllowWriteStreamBuffering = false;

using (var fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{
    // Open a request stream and write 1024 byte chunks in the stream one at a time.
    byte[] buffer = null;
    int bytesRead = 0;
    using (var requestStream = request.GetRequestStream())
    {
        // Read 1024 raw bytes from the input audio file.
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        requestStream.Flush();
    }
}
```

## <a name="response-parameters"></a>Yanıt parametreleri

Sonuçlar JSON olarak sağlanır. Biçim `simple` , bu üst düzey alanları içerir.

| Parametre | Açıklama  |
|-----------|--------------|
|`RecognitionStatus`|`Success` Başarılı tanıma gibi durum. Sonraki tabloya bakın.|
|`DisplayText`|Büyük harfler, noktalama, ters metin normalleştirmesinin ardından tanınan metin ("Doctor Smith" için "200" veya "Dr. Smith" için 200 gibi daha kısa formlara dönüştürme) ve küfür maskeleme. Yalnızca başarılı olduğunda sunun.|
|`Offset`|Tanınan konuşmanın ses akışında başladığı zaman (100-nanosaniyelik birimi).|
|`Duration`|Ses akışındaki tanınan konuşmanın süresi (100-nanosaniyelik birimi).|

`RecognitionStatus` Alan şu değerleri içerebilir:

| Durum | Açıklama |
|--------|-------------|
| `Success` | Tanıma başarılı oldu ve `DisplayText` alan var. |
| `NoMatch` | Ses akışında konuşma algılandı, ancak hedef dilden hiçbir sözcük eşleşmedi. Genellikle, tanınma dilinin kullanıcının konuşmadan farklı bir dil olduğu anlamına gelir. |
| `InitialSilenceTimeout` | Ses akışının başlangıcı yalnızca sessizlik içeriyordu ve hizmet konuşmayı beklerken zaman aşımına uğradı. |
| `BabbleTimeout` | Ses akışının başlangıcı yalnızca gürültü içeriyordu ve hizmet konuşmayı beklerken zaman aşımına uğradı. |
| `Error` | Tanıma hizmeti bir iç hatayla karşılaştı ve devam edemedi. Mümkünse yeniden deneyin. |

> [!NOTE]
> Ses yalnızca küfür içeriyorsa ve `profanity` sorgu parametresi olarak `remove`ayarlanırsa, hizmet bir konuşma sonucu döndürmez.

Biçim `detailed` , tanınan sonuçların ek biçimlerini içerir.
`detailed` Biçimini kullanırken, `DisplayText` `NBest` listedeki her sonuç için olarak `Display` olarak sağlanır.

`NBest` Listedeki nesne şunları içerebilir:

| Parametre | Açıklama |
|-----------|-------------|
| `Confidence` | 0,0 (güven yok) ile 1,0 arasındaki girdinin Güvenirlik puanı (tam güven) |
| `Lexical` | Tanınan metnin sözlü biçimi: tanınan gerçek sözcükler. |
| `ITN` | Tanınan metnin, telefon numarası, sayı, kısaltmalar ("Doctor Smith" ile "Dr Smith") ve uygulanan diğer dönüşümler içeren ters metin normalleştirilmiş ("kurallı") biçimi. |
| `MaskedITN` | İsteniyorsa, uygunsuz bir maskeleme uygulanmış ıTYPEFORM. |
| `Display` | Tanınan metnin noktalama ve büyük harfleri eklenmiş olan görüntüleme formu. Bu parametre, biçim olarak ayarlandığında `DisplayText` belirtilen şekilde aynıdır `simple`. |
| `AccuracyScore` | Verilen konuşmaya ait telaffuz doğruluğunu belirten puan. |
| `FluencyScore` | Verilen konuşmayı akıcı bir şekilde belirten puan. |
| `CompletenessScore` | Tüm girişe doğru olan sözcük oranını hesaplayarak verilen konuşmayı belirten puan. |
| `PronScore` | Verilen konuşmayı gösteren telaffuz kalitesini belirten genel puan. Bu `AccuracyScore`, `FluencyScore` ve `CompletenessScore` ağırlığıyla hesaplanır. |
| `ErrorType` | Bu değer, ile karşılaştırıldığında bir sözcüğün atlanıp atlanmadığını, ekleneceğini veya hatalı bir `ReferenceText`şekilde olduğunu gösterir. Olası değerler şunlardır `None` (Yani bu sözcükte hata yok), `Omission` `Insertion` ve `Mispronunciation`. |

## <a name="sample-responses"></a>Örnek yanıtlar

Tanıma için `simple` tipik bir yanıt:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Tanıma için `detailed` tipik bir yanıt:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      }
  ]
}
```

Telaffuz değerlendirmesi ile tipik bir yanıt:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "400000",
  "Duration": "11000000",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "good morning",
        "ITN" : "good morning",
        "MaskedITN" : "good morning",
        "Display" : "Good morning.",
        "PronScore" : 84.4,
        "AccuracyScore" : 100.0,
        "FluencyScore" : 74.0,
        "CompletenessScore" : 100.0,
        "Words": [
            {
              "Word" : "Good",
              "AccuracyScore" : 100.0,
              "ErrorType" : "None",
              "Offset" : 500000,
              "Duration" : 2700000
            },
            {
              "Word" : "morning",
              "AccuracyScore" : 100.0,
              "ErrorType" : "None",
              "Offset" : 5300000,
              "Duration" : 900000
            }
        ]
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
