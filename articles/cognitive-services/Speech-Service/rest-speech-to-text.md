---
title: Konuşmadan metne API başvurusu (REST)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmayı metne REST API nasıl kullanacağınızı öğrenin. Bu makalede, sorgu seçenekleri, yetkilendirme seçenekleri hakkında bilgi edineceksiniz yapısı bir istek ve yanıt.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: erhopf
ms.openlocfilehash: 873898ce321100edbaa800d2436d0413c06ce175
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78255664"
---
# <a name="speech-to-text-rest-api"></a>Konuşmayı metne dönüştürme REST API'si

Konuşma [SDK 'sına](speech-sdk.md)alternatif olarak, konuşma hizmeti bir REST API kullanarak konuşmayı metne dönüştürmenize olanak tanır. Her erişilebilen bir uç noktaya bir bölge ile ilişkilidir. Uygulamanızı kullanmayı planladığınız uç nokta için bir abonelik anahtarı gerektirir.

Konuşmayı metne REST API kullanmadan önce, şunu anlayın:

* REST API ve doğrudan ses iletimi kullanan istekler yalnızca en fazla 60 saniyelik ses içerebilir.
* Konuşmayı metne REST API, yalnızca son sonuçları döndürür. Kısmi sonuçlar sağlanmaz.

Daha uzun bir ses gönderdiğinizde uygulamanız için bir gereklilik varsa, [konuşma SDK 'sını](speech-sdk.md) veya [toplu iş dökümü](batch-transcription.md)gibi dosya tabanlı REST API kullanmayı düşünün.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Bölgeler ve uç noktaları

REST API uç noktası şu biçimdedir:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

`<REGION_IDENTIFIER>`, bu tablodaki aboneliğinizin bölgesiyle eşleşen tanımlayıcıyla değiştirin:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> Bir 4xx HTTP hatası almamak için dil parametresi URL 'ye eklenmelidir. Örneğin, Batı ABD uç noktası kullanılarak ABD Ingilizcesi olarak ayarlanan dil: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.

## <a name="query-parameters"></a>Sorgu parametreleri

Bu parametreleri REST isteğinin sorgu dizesinde eklenebilir.

| Parametre | Açıklama | Gerekli / isteğe bağlı |
|-----------|-------------|---------------------|
| `language` | Tanınan konuşulan dil tanımlar. [Desteklenen diller](language-support.md#speech-to-text)bölümüne bakın. | Gerekli |
| `format` | Sonuç biçimi belirtir. Kabul edilen değerler `simple` ve `detailed`. Basit sonuçlar `RecognitionStatus`, `DisplayText`, `Offset`ve `Duration`içerir. Ayrıntılı yanıtlar, birden çok sonuçla güvenle değerleri ve dört farklı temsilleri içerir. Varsayılan ayar `simple`. | İsteğe bağlı |
| `profanity` | Tanıma sonuçları küfür nasıl ele alınacağını belirtir. Kabul edilen değerler `masked`, küfür ile `removed`, sonuçtaki tüm küfür kaldıran ve `raw`sonuçtaki küfür içeren. Varsayılan ayar `masked`. | İsteğe bağlı |
| `cid` | Özel modeller oluşturmak için [özel konuşma tanıma portalını](how-to-custom-speech.md) kullanırken, **dağıtım** SAYFASıNDA bulunan **uç nokta kimlikleri** aracılığıyla özel modeller kullanabilirsiniz. `cid` sorgu dizesi parametresinin bağımsız değişkeni olarak **uç nokta kimliğini** kullanın. | İsteğe bağlı |

## <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, Konuşmayı metne istekler için gerekli ve isteğe bağlı üst bilgileri listeler.

|Üst bilgi| Açıklama | Gerekli / isteğe bağlı |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Konuşma hizmeti abonelik anahtarınız. | Bu üstbilgi veya `Authorization` gereklidir. |
| `Authorization` | Öncesinde `Bearer`bir yetkilendirme belirteci. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Bu üstbilgi veya `Ocp-Apim-Subscription-Key` gereklidir. |
| `Content-type` | Sağlanan ses verisi codec ve biçim açıklar. Kabul edilen değerler `audio/wav; codecs=audio/pcm; samplerate=16000` ve `audio/ogg; codecs=opus`. | Gerekli |
| `Transfer-Encoding` | Öbekli ses, tek bir dosya yerine gönderilen veri olduğunu belirtir. Yalnızca ses verileri varsa bu üstbilgiyi kullanır. | İsteğe bağlı |
| `Expect` | Öbekli aktarım kullanılıyorsa `Expect: 100-continue`gönderin. Konuşma hizmeti, ilk isteği ve bekleek verileri onaylar.| Öbekli ses veri gönderen gereklidir. |
| `Accept` | Sağlanmışsa, `application/json`olması gerekir. Konuşma hizmeti, sonuçları JSON ile sağlar. Bazı istek çerçeveleri uyumsuz bir varsayılan değer sağlar. `Accept`her zaman dahil etmek iyi bir uygulamadır. | İsteğe bağlı, ancak önerilir. |

## <a name="audio-formats"></a>Ses biçimleri

Ses HTTP `POST` isteği gövdesinde gönderilir. Bu tabloda biçimlerden birinde olmalıdır:

| Biçimlendir | Codec bileşeni | Bit hızı | Örnek hızı  |
|--------|-------|---------|--------------|
| WAV    | PCM   | 16-bit  | 16 kHz, mono |
| OGG    | GEÇERLİ  | 16-bit  | 16 kHz, mono |

>[!NOTE]
>Yukarıdaki biçimler, konuşma hizmetindeki REST API ve WebSocket aracılığıyla desteklenir. [Konuşma SDK 'sı](speech-sdk.md) Şu anda, PCM codec ve [DIĞER biçimlere](how-to-use-codec-compressed-audio-input-streams.md)sahip WAV biçimini desteklemektedir.

## <a name="sample-request"></a>Örnek istek

Aşağıdaki örnek, ana bilgisayar adı ve gerekli üst bilgileri içerir. Hizmet bu örnekte yer almayan ses veri girmeniz gerektiğini unutmayın. Belirtildiği gibi daha önce parçalama, ancak gerekli değildir önerilir.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

## <a name="http-status-codes"></a>HTTP durum kodları

Her yanıt için HTTP durum kodu, başarı veya sık karşılaşılan hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| `100` | Devam | İlk istek kabul edildi. Kalan verileri göndermek ile devam edin. (Öbekli aktarımlı olarak kullanılır) |
| `200` | Tamam | İstek başarılı oldu; yanıt gövdesi bir JSON nesnesidir. |
| `400` | Hatalı istek | Dil kodu sağlanmadı, desteklenen bir dil değil, geçersiz ses dosyası, vb. |
| `401` | Yetkisiz | Abonelik anahtarı veya yetkilendirme belirteci, belirtilen bölge veya geçersiz uç nokta geçersiz. |
| `403` | Yasak | Eksik abonelik anahtarı veya yetkilendirme belirteci. |

## <a name="chunked-transfer"></a>Öbekli aktarım

Öbekli aktarım (`Transfer-Encoding: chunked`), tanınma gecikmesini azaltmaya yardımcı olabilir. Konuşma hizmetinin, aktarım sırasında ses dosyasını işlemeye başlamasını sağlar. REST API, kısmi veya Ara sonuçlar sağlamaz.

Bu kod örneği, nasıl öbekler halinde ses gönderileceğini gösterir. Yalnızca ilk öbekte ses dosyanın üst bilgisi içermelidir. `request`, uygun REST uç noktasına bağlı bir `HttpWebRequest` nesnesidir. `audioFile`, diskteki bir ses dosyasının yoludur.

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

Sonuçları JSON olarak sağlanır. `simple` biçimi bu üst düzey alanları içerir.

| Parametre | Açıklama  |
|-----------|--------------|
|`RecognitionStatus`|Başarılı tanıma için `Success` gibi durum. Sonraki tabloya bakın.|
|`DisplayText`|Büyük harfler, noktalama, ters metin normalleştirmesinin ardından tanınan metin ("Doctor Smith" için "200" veya "Dr. Smith" için 200 gibi daha kısa formlara dönüştürme) ve küfür maskeleme. Yalnızca başarı sunar.|
|`Offset`|Tanınan konuşma tanıma ses akışı başlar süre (100 nanosaniyelik birimleri).|
|`Duration`|Ses akışı olarak tanınan konuşma süresi (100 nanosaniyelik birimlerindeki).|

`RecognitionStatus` alanı şu değerleri içerebilir:

| Durum | Açıklama |
|--------|-------------|
| `Success` | Tanıma başarılı oldu ve `DisplayText` alanı var. |
| `NoMatch` | Konuşma Tanıma Ses akışında algılandı, ancak hiçbir hedef dil sözcükleri eşleştirilmiş olan. Genellikle kullanıcı Konuşmayı olandan farklı bir dil tanıma dilidir anlamına gelir. |
| `InitialSilenceTimeout` | Ses akışı başlangıcını yalnızca sessizlik ve konuşma için beklerken zaman aşımına hizmetini içeriyordu. |
| `BabbleTimeout` | Ses akışı başlangıcını yalnızca gürültü ve konuşma için beklerken zaman aşımına hizmetini içeriyordu. |
| `Error` | Tanıma hizmeti bir iç hatayla karşılaştı ve çalışmaya devam edemedi. Mümkün olduğunda yeniden deneyin. |

> [!NOTE]
> Ses yalnızca küfür içeriyorsa ve `profanity` sorgu parametresi `remove`olarak ayarlanırsa, hizmet bir konuşma sonucu döndürmez.

`detailed` biçimi `simple` biçimiyle aynı verileri, aynı tanınma sonucunun alternatif yorumlarının bir listesini `NBest`ile birlikte içerir. Bu sonuçlar en büyük olasılıkla en az büyük olasılıkla derecelendirilir. İlk giriş, ana tanıma sonucuyla aynıdır.  `detailed` biçimi kullanılırken, `DisplayText` `NBest` listesindeki her sonuç için `Display` olarak sağlanır.

`NBest` listesindeki her nesne şunları içerir:

| Parametre | Açıklama |
|-----------|-------------|
| `Confidence` | Güvenilirlik puanı 1.0 (tam güven) girişinin 0,0 (güven yok) |
| `Lexical` | Sözcük şeklinde tanınan metin: Gerçek sözcüklerin tanınır. |
| `ITN` | Ters metin normalleştirilmiş ("") kurallı tanınan metinle telefon numaraları, sayılar, kısaltmaları ("doktor smith" için "dr smith") ve uygulanan diğer dönüşümler. |
| `MaskedITN` | Edemezsiniz formun, istenmesi halinde uygulanan küfür maskeleme ile. |
| `Display` | Noktalama işaretleri ve eklenen büyük/küçük harf ile tanınan metin görüntüleme formu. Biçim `simple`olarak ayarlandığında, bu parametre `DisplayText` sağlandığı ile aynıdır. |

## <a name="sample-responses"></a>Örnek yanıt

`simple` tanıma için tipik bir yanıt:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

`detailed` tanıma için tipik bir yanıt:

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
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
