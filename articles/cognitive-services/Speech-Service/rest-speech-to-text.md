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
ms.date: 05/13/2020
ms.author: yinhew
ms.openlocfilehash: e7bbedf253d6a64609179a8710fc9accd1f03818
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537978"
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

`<REGION_IDENTIFIER>`Bu tablodaki aboneliğinizin bölgesiyle eşleşen tanımlayıcıyla değiştirin:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> Bir 4xx HTTP hatası almamak için dil parametresi URL 'ye eklenmelidir. Örneğin, Batı ABD uç noktası kullanılarak dil ABD Ingilizcesi olarak ayarlanmıştır: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US` .

## <a name="query-parameters"></a>Sorgu parametreleri

Bu parametreler REST isteğinin sorgu dizesine dahil edilebilir.

| Parametre | Açıklama | Gerekli/Isteğe bağlı |
|-----------|-------------|---------------------|
| `language` | Tanınmakta olan konuşulan dili tanımlar. [Desteklenen diller](language-support.md#speech-to-text)bölümüne bakın. | Gerekli |
| `format` | Sonuç biçimini belirtir. Kabul edilen değerler `simple` şunlardır `detailed` . Basit sonuçlar,,, `RecognitionStatus` `DisplayText` ve içerir `Offset` `Duration` . Ayrıntılı yanıtlar, görüntü metninin dört farklı temsilini içerir. Varsayılan ayar `simple` değeridir. | İsteğe Bağlı |
| `profanity` | Tanıma sonuçlarında küfür nasıl işleneceğini belirtir. Kabul edilen değerler `masked` , küfür ile bir `removed` bütün küfür kaldıran, ya da `raw` sonuçtaki küfür da dahil olmak üzere yıldız işaretiyle değiştirilir. Varsayılan ayar `masked` değeridir. | İsteğe Bağlı |
| `cid` | Özel modeller oluşturmak için [özel konuşma tanıma portalını](how-to-custom-speech.md) kullanırken, **dağıtım** SAYFASıNDA bulunan **uç nokta kimlikleri** aracılığıyla özel modeller kullanabilirsiniz. Sorgu dizesi parametresinin bağımsız değişkeni olarak **uç nokta kimliğini** kullanın `cid` . | İsteğe Bağlı |

## <a name="request-headers"></a>İstek üst bilgileri

Bu tabloda, konuşma-metin istekleri için gerekli ve isteğe bağlı üstbilgiler listelenmektedir.

|Üst bilgi| Description | Gerekli/Isteğe bağlı |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Konuşma hizmeti abonelik anahtarınız. | Bu üst bilgi ya da `Authorization` gerekli. |
| `Authorization` | Bir yetkilendirme belirteci öncesinde kelimedir `Bearer` . Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Bu üst bilgi ya da `Ocp-Apim-Subscription-Key` gerekli. |
| `Pronunciation-Assessment` | Tanınma sonuçlarında telaffuz puanlarını göstermek için parametreleri belirtir; doğruluk, akıcı, tamamlanma, vb. göstergeler ile konuşma girişi için telaffuz kalitesini değerlendirir. Bu parametre, birden çok ayrıntılı parametre içeren Base64 kodlamalı JSON 'dir. Bu üstbilgiyi derlemek için [telaffuz değerlendirmesi parametrelerine](#pronunciation-assessment-parameters) bakın. | İsteğe Bağlı |
| `Content-type` | Belirtilen ses verilerinin biçimini ve codec 'ini açıklar. Kabul edilen değerler `audio/wav; codecs=audio/pcm; samplerate=16000` şunlardır `audio/ogg; codecs=opus` . | Gerekli |
| `Transfer-Encoding` | Tek bir dosya yerine, öbekli ses verilerinin gönderileceğini belirtir. Yalnızca ses verilerini parçalama durumunda bu üstbilgiyi kullanın. | İsteğe Bağlı |
| `Expect` | Öbekli aktarım kullanılıyorsa, gönderin `Expect: 100-continue` . Konuşma hizmeti, ilk isteği ve bekleek verileri onaylar.| Öbekli ses verileri gönderiyorsanız gereklidir. |
| `Accept` | Sağlanmışsa, olmalıdır `application/json` . Konuşma hizmeti, sonuçları JSON ile sağlar. Bazı istek çerçeveleri uyumsuz bir varsayılan değer sağlar. Her zaman dahil etmek iyi bir uygulamadır `Accept` . | İsteğe bağlı, ancak önerilir. |

## <a name="audio-formats"></a>Ses biçimleri

HTTP isteğinin gövdesinde ses gönderilir `POST` . Bu tablodaki biçimlerden birinde olmalıdır:

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
| GradingSystem | Puan ayarlaması için nokta sistemi. Kabul edilen değerler `FivePoint` şunlardır `HundredMark` . Varsayılan ayar `FivePoint` değeridir. | İsteğe Bağlı |
| Ayrıntı düzeyi | Değerlendirme ayrıntı düzeyi. Kabul edilen değerler, tam metin ve sözcük düzeyindeki puanı gösteren ve tam metin düzeyinde puan gösteren, `Phoneme` Word ve Fonem düzeyindeki puanı gösteren kabul edilir `Word` `FullText` . Varsayılan ayar `Phoneme` değeridir. | İsteğe Bağlı |
| Boyut | Çıkış ölçütünü tanımlar. Kabul edilen değerler `Basic` yalnızca doğruluk puanı ' nı gösterir, `Comprehensive` daha fazla boyutlara ilişkin puanları gösterir (örneğin, tam metin düzeyinde, akıcı puan ve tamamlayıcı puanı, sözcük düzeyinde hata türü). Farklı puan boyutlarının tanımlarını ve sözcük hata türlerini görmek için [yanıt parametrelerini](#response-parameters) denetleyin. Varsayılan ayar `Basic` değeridir. | İsteğe Bağlı |
| EnableMiscue | Hatalı işaret hesaplamasını etkinleştirilir. Bu etkinken, bulunan sözcükler başvuru metniyle karşılaştırılır ve karşılaştırmaya göre atlama/ekleme ile işaretlenir. Kabul edilen değerler `False` şunlardır `True` . Varsayılan ayar `False` değeridir. | İsteğe Bağlı |
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

Aşağıdaki örnek kod, üst bilgiyle telaffuz değerlendirmesi parametrelerinin nasıl oluşturulacağını gösterir `Pronunciation-Assessment` :

```csharp
var pronAssessmentParamsJson = $"{{\"ReferenceText\":\"Good morning.\",\"GradingSystem\":\"HundredMark\",\"Granularity\":\"FullText\",\"Dimension\":\"Comprehensive\"}}";
var pronAssessmentParamsBytes = Encoding.UTF8.GetBytes(pronAssessmentParamsJson);
var pronAssessmentHeader = Convert.ToBase64String(pronAssessmentParamsBytes);
```

Ses verilerinin, gecikme süresini önemli ölçüde azaltan akış (öbekli) karşıya yükleme işlemini kesinlikle öneririz. Akışı etkinleştirme hakkında bilgi için bkz. [farklı programlama dillerinde örnek kod](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/PronunciationAssessment) .

>[!NOTE]
>Telaffuz değerlendirmesi özelliği şu anda yalnızca `westus` `eastasia` ve `centralindia` bölgelerinde kullanılabilir. Bu özellik şu anda yalnızca `en-US` dilde kullanılabilir.

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

Telaffuz değerlendirmesini etkinleştirmek için aşağıdaki üst bilgiyi ekleyebilirsiniz. Bu üstbilgiyi derlemek için [telaffuz değerlendirmesi parametrelerine](#pronunciation-assessment-parameters) bakın.

```HTTP
Pronunciation-Assessment: eyJSZWZlcm...
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

Öbekli aktarım ( `Transfer-Encoding: chunked` ), tanınma gecikmesini azaltmaya yardımcı olabilir. Konuşma hizmetinin, aktarım sırasında ses dosyasını işlemeye başlamasını sağlar. REST API kısmi veya geçici sonuçlar sağlamıyor.

Bu kod örneği, öbekte nasıl ses gönderileceğini gösterir. Yalnızca ilk öbek, ses dosyasının üst bilgisini içermelidir. `request`, `HttpWebRequest` uygun REST uç noktasına bağlı bir nesnedir. `audioFile`diskteki bir ses dosyasının yoludur.

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

Sonuçlar JSON olarak sağlanır. `simple`Biçim, bu üst düzey alanları içerir.

| Parametre | Description  |
|-----------|--------------|
|`RecognitionStatus`|Başarılı tanıma gibi durum `Success` . Sonraki tabloya bakın.|
|`DisplayText`|Büyük harfler, noktalama, ters metin normalleştirmesinin ardından tanınan metin ("Doctor Smith" için "200" veya "Dr. Smith" için 200 gibi daha kısa formlara dönüştürme) ve küfür maskeleme. Yalnızca başarılı olduğunda sunun.|
|`Offset`|Tanınan konuşmanın ses akışında başladığı zaman (100-nanosaniyelik birimi).|
|`Duration`|Ses akışındaki tanınan konuşmanın süresi (100-nanosaniyelik birimi).|

`RecognitionStatus`Alan şu değerleri içerebilir:

| Durum | Description |
|--------|-------------|
| `Success` | Tanıma başarılı oldu ve `DisplayText` alan var. |
| `NoMatch` | Ses akışında konuşma algılandı, ancak hedef dilden hiçbir sözcük eşleşmedi. Genellikle, tanınma dilinin kullanıcının konuşmadan farklı bir dil olduğu anlamına gelir. |
| `InitialSilenceTimeout` | Ses akışının başlangıcı yalnızca sessizlik içeriyordu ve hizmet konuşmayı beklerken zaman aşımına uğradı. |
| `BabbleTimeout` | Ses akışının başlangıcı yalnızca gürültü içeriyordu ve hizmet konuşmayı beklerken zaman aşımına uğradı. |
| `Error` | Tanıma hizmeti bir iç hatayla karşılaştı ve devam edemedi. Mümkünse yeniden deneyin. |

> [!NOTE]
> Ses yalnızca küfür içeriyorsa ve `profanity` sorgu parametresi olarak ayarlanırsa `remove` , hizmet bir konuşma sonucu döndürmez.

`detailed`Biçim, tanınan sonuçların ek biçimlerini içerir.
`detailed`Biçimini kullanırken, `DisplayText` `Display` listedeki her sonuç için olarak olarak sağlanır `NBest` .

`NBest`Listedeki nesne şunları içerebilir:

| Parametre | Description |
|-----------|-------------|
| `Confidence` | 0,0 (güven yok) ile 1,0 arasındaki girdinin Güvenirlik puanı (tam güven) |
| `Lexical` | Tanınan metnin sözlü biçimi: tanınan gerçek sözcükler. |
| `ITN` | Tanınan metnin, telefon numarası, sayı, kısaltmalar ("Doctor Smith" ile "Dr Smith") ve uygulanan diğer dönüşümler içeren ters metin normalleştirilmiş ("kurallı") biçimi. |
| `MaskedITN` | İsteniyorsa, uygunsuz bir maskeleme uygulanmış ıTYPEFORM. |
| `Display` | Tanınan metnin noktalama ve büyük harfleri eklenmiş olan görüntüleme formu. Bu parametre, `DisplayText` biçim olarak ayarlandığında belirtilen şekilde aynıdır `simple` . |
| `AccuracyScore` | Konuşmayı telaffuz doğruluğu. Doğruluk, alfabesine 'in yerel konuşmacı söylenişi ile ne kadar yakın olduğunu gösterir. Word ve tam metin düzeyi doğruluk puanı, Fonem düzeyi doğruluk puanı ' ndan toplanır. |
| `FluencyScore` | Verilen konuşmayı akıcı olarak. Akıcı bir şekilde, konuşmanın bir yerel konuşmacının sözcükler arasında sessiz kesmeler kullanımıyla ne kadar yakın olduğunu gösterir. |
| `CompletenessScore` | Sözcük girişine başvuru metin girişi olarak bulunan sözcüklerin oranının hesaplanmasıyla belirlenen konuşma. |
| `PronScore` | Verilen konuşmayı gösteren telaffuz kalitesini belirten genel puan. Bu `AccuracyScore` , `FluencyScore` ve `CompletenessScore` ağırlığıyla toplanır. |
| `ErrorType` | Bu değer, ile karşılaştırıldığında bir sözcüğün atlanıp atlanmadığını, ekleneceğini veya hatalı bir şekilde olduğunu gösterir `ReferenceText` . Olası değerler şunlardır `None` (Yani bu sözcükte hata yok), `Omission` `Insertion` ve `Mispronunciation` . |

## <a name="sample-responses"></a>Örnek yanıtlar

Tanıma için tipik bir yanıt `simple` :

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

Tanıma için tipik bir yanıt `detailed` :

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
