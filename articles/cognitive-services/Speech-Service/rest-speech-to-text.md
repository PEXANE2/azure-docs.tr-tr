---
title: Konuşma-metin API başvurusu (REST) - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne REST API'sini nasıl kullanacağınızı öğrenin. Bu makalede, yetkilendirme seçenekleri, sorgu seçenekleri, bir isteği yapılandırma ve yanıt alma hakkında bilgi edineceksiniz.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: dapine
ms.openlocfilehash: 759ea697e4093da5bfc1c082c886c6dfda636f42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474807"
---
# <a name="speech-to-text-rest-api"></a>Konuşmayı metne dönüştürme REST API'si

[Konuşma SDK'ya](speech-sdk.md)alternatif olarak, Konuşma hizmeti, REST API'sını kullanarak konuşmaya konuşmaya dönüştürmenizi sağlar. Erişilebilir her uç nokta bir bölgeyle ilişkilidir. Uygulamanız, kullanmayı planladığınız bitiş noktası için bir abonelik anahtarı gerektirir. REST API çok sınırlıdır ve sadece [Konuşma SDK](speech-sdk.md) olabilir durumlarda kullanılmalıdır.

Konuşma-metin REST API kullanmadan önce, anlayın:

* REST API'yi kullanan ve doğrudan ses aktaran istekler yalnızca 60 saniyeye kadar ses içerebilir.
* Konuşma-to-metin REST API yalnızca nihai sonuçları döndürür. Kısmi sonuçlar sağlanmaz.

Daha uzun ses göndermek uygulamanız için bir gereklilikse, [Toplu transkripsiyon](batch-transcription.md)gibi [Konuşma SDK'sını](speech-sdk.md) veya dosya tabanlı REST API'yi kullanmayı düşünün.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="regions-and-endpoints"></a>Bölgeler ve uç noktalar

REST API için bitiş noktası şu biçime sahiptir:

```
https://<REGION_IDENTIFIER>.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1
```

Aboneliğinizin bulunduğu bölgeye uyan tanımlayıcıyı bu tablodan değiştirin: `<REGION_IDENTIFIER>`

[!INCLUDE [](../../../includes/cognitive-services-speech-service-region-identifier.md)]

> [!NOTE]
> 4xx HTTP hatası almamak için dil parametresi URL'ye eklenmelidir. Örneğin, Batı ABD bitiş noktasını kullanarak ABD İngilizcesi olarak ayarlanan dil: `https://westus.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.

## <a name="query-parameters"></a>Sorgu parametreleri

Bu parametreler REST isteğinin sorgu dizesinde dahil edilebilir.

| Parametre | Açıklama | Gerekli / İsteğe Bağlı |
|-----------|-------------|---------------------|
| `language` | Tanınmakta olan konuşulan dili tanımlar. Bkz. [Desteklenen diller.](language-support.md#speech-to-text) | Gerekli |
| `format` | Sonuç biçimini belirtir. Kabul edilen `simple` değerler `detailed`ve . Basit sonuçlar `RecognitionStatus` `DisplayText`, `Offset`, `Duration`, ve . Ayrıntılı yanıtlar, güven değerleri ve dört farklı gösterimi olan birden çok sonuç içerir. Varsayılan ayar `simple` değeridir. | İsteğe bağlı |
| `profanity` | Tanıma sonuçlarında küfürle nasıl başa çıkılacağını belirtir. Kabul edilen `masked`değerler, küfür yerine yıldız işaretleri, `removed`sonuçtan tüm küfür kaldırır, ya da `raw`, sonuç küfür içerir. Varsayılan ayar `masked` değeridir. | İsteğe bağlı |
| `cid` | Özel modeller oluşturmak için [Özel Konuşma portalını](how-to-custom-speech.md) kullanırken, **Dağıtım** sayfasında bulunan Uç **Nokta Kimliği** aracılığıyla özel modelleri kullanabilirsiniz. Sorgu dizesi parametresine bağımsız değişken olarak **Endpoint Kimliğini** kullanın. `cid` | İsteğe bağlı |

## <a name="request-headers"></a>İstek üst bilgileri

Bu tablo, konuşmadan metne istekler için gerekli ve isteğe bağlı üstleri listeler.

|Üst bilgi| Açıklama | Gerekli / İsteğe Bağlı |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Konuşma hizmeti abonelik anahtarınız. | Bu üstbilgi `Authorization` veya gereklidir. |
| `Authorization` | Sözcüğün `Bearer`önünde ki bir yetkilendirme belirteci. Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). | Bu üstbilgi `Ocp-Apim-Subscription-Key` veya gereklidir. |
| `Content-type` | Sağlanan ses verilerinin biçimini ve codec'ini açıklar. Kabul edilen `audio/wav; codecs=audio/pcm; samplerate=16000` değerler `audio/ogg; codecs=opus`ve . | Gerekli |
| `Transfer-Encoding` | Tek bir dosya yerine yığınlı ses verilerinin gönderildiğini belirtir. Bu üstbilgiyalnızca ses verilerini öttüst ederken kullanın. | İsteğe bağlı |
| `Expect` | Parçalı aktarım kullanıyorsanız, gönder. `Expect: 100-continue` Konuşma hizmeti ilk isteği kabul eder ve ek veri bekler.| Parçalanmış ses verileri gönderiyorsanız gereklidir. |
| `Accept` | Eğer sağlanırsa, `application/json`öyle olmalı. Konuşma hizmeti JSON sonuçları sağlar. Bazı istek çerçeveleri uyumsuz bir varsayılan değer sağlar. Her zaman dahil `Accept`etmek iyi bir uygulamadır. | İsteğe bağlı, ancak önerilir. |

## <a name="audio-formats"></a>Ses biçimleri

Ses, HTTP `POST` isteğinin gövdesine gönderilir. Bu tablodaki biçimlerden birinde olmalıdır:

| Biçimlendir | Codec | Bitrate | Örnek Oranı  |
|--------|-------|---------|--------------|
| WAV    | PCM   | 16 bit  | 16 kHz, mono |
| Ogg    | Opus  | 16 bit  | 16 kHz, mono |

>[!NOTE]
>Yukarıdaki biçimler Konuşma hizmetinde REST API ve WebSocket aracılığıyla desteklenir. [Konuşma SDK](speech-sdk.md) şu anda PCM codec yanı sıra [diğer biçimleri](how-to-use-codec-compressed-audio-input-streams.md)ile WAV biçimini destekler.

## <a name="sample-request"></a>Örnek istek

Aşağıdaki örnek, ana bilgisayar adını ve gerekli üstbilgiyi içerir. Hizmetin, bu örnekte yer almayan ses verilerini de beklediğini unutmayın. Daha önce de belirtildiği gibi, chunking önerilir, ancak, gerekli değildir.

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

Her yanıt ın HTTP durum kodu başarıyı veya yaygın hataları gösterir.

| HTTP durum kodu | Açıklama | Olası neden |
|------------------|-------------|-----------------|
| `100` | Devam et | İlk istek kabul edildi. Verilerin geri kalanını göndermeye devam edin. (Öbek transferi ile kullanılır) |
| `200` | Tamam | İstek başarılı oldu; yanıt gövdesi bir JSON nesnesidir. |
| `400` | Kötü istek | Dil kodu sağlanmadı, desteklenen bir dil, geçersiz ses dosyası, vb. |
| `401` | Yetkisiz | Abonelik anahtarı veya yetkilendirme belirteci belirtilen bölgede geçersizdir veya geçersiz bitiş noktasıdır. |
| `403` | Yasak | Eksik abonelik anahtarı veya yetkilendirme belirteci. |

## <a name="chunked-transfer"></a>Chunked transferi

Chunked aktarım (`Transfer-Encoding: chunked`) tanıma gecikmesüresini azaltmaya yardımcı olabilir. Konuşma hizmetinin aktarılırken ses dosyasını işlemeye başlamasını sağlar. REST API'si kısmi veya ara sonuçlar sağlamaz.

Bu kod örneği, sesin parçalar halinde nasıl gönderilebildiğini gösterir. Yalnızca ilk yığın ses dosyasının üstbilgisini içermelidir. `request`uygun `HttpWebRequest` REST bitiş noktasına bağlı bir nesnedir. `audioFile`diskteki bir ses dosyasına giden yoldur.

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

Sonuçlar JSON olarak sağlanmaktadır. `simple` Biçim, bu üst düzey alanları içerir.

| Parametre | Açıklama  |
|-----------|--------------|
|`RecognitionStatus`|Başarılı tanıma `Success` gibi durum. Sonraki masaya bak.|
|`DisplayText`|Büyük harf, noktalama işareti, ters metin normalleştirme (konuşulan metnin "iki yüz" için 200 veya "Doctor Smith" için "doktor smith" gibi daha kısa formlara dönüştürülmesi) ve küfür maskelemeden sonra tanınan metin. Sadece başarı ile sun.|
|`Offset`|Tanınan konuşmanın ses akışında başladığı saat (100 nanosaniyelik ünitelerde).|
|`Duration`|Ses akışında tanınan konuşmanın süresi (100 nanosaniyelik ünitelerde).|

Alan `RecognitionStatus` şu değerleri içerebilir:

| Durum | Açıklama |
|--------|-------------|
| `Success` | Tanıma başarılı oldu `DisplayText` ve alan mevcut. |
| `NoMatch` | Ses akışında konuşma algılandı, ancak hedef dildeki hiçbir sözcük eşleştirildi. Genellikle tanıma dilinin kullanıcının konuştuğu dilden farklı bir dil olduğu anlamına gelir. |
| `InitialSilenceTimeout` | Ses akışının başlangıcında yalnızca sessizlik ve hizmet konuşma için bekleme süresi doldu. |
| `BabbleTimeout` | Ses akışının başlangıcında yalnızca gürültü bulunurve hizmet konuşma beklerken zaman doldu. |
| `Error` | Tanıma hizmeti bir iç hatayla karşılaştı ve devam edemedi. Mümkünse tekrar deneyin. |

> [!NOTE]
> Ses yalnızca küfürden oluşuyorsa ve `profanity` sorgu parametresi `remove`ayarlanırsa, hizmet bir konuşma sonucu döndürmez.

Biçim, `detailed` `simple` biçimiyle `NBest`aynı verileri ve aynı tanıma sonucunun alternatif yorumlarının listesini içerir. Bu sonuçlar büyük olasılıkla en düşük olasılıkla sıralanır. İlk giriş, ana tanıma sonucuyla aynıdır.  `detailed` Biçimi kullanırken, `DisplayText` `NBest` listedeki `Display` her sonuç için sağlanır.

Listedeki `NBest` her nesne şunları içerir:

| Parametre | Açıklama |
|-----------|-------------|
| `Confidence` | Girişin güven puanı 0,0 'dan (güven yok) 1,0'a (tam güven) |
| `Lexical` | Tanınan metnin sözlü biçimi: tanınan gerçek sözcükler. |
| `ITN` | Telefon numaraları, sayılar, kısaltmalar ("doctor smith" to "dr smith") ve uygulanan diğer dönüşümlerle tanınan metnin ters metin normalleştirilmiş ("kanonik") biçimi. |
| `MaskedITN` | İstenirse küfür maskeleme içeren ITN formu uygulanır. |
| `Display` | Noktalama işaretleri ve büyük harf eklenmi ile tanınan metnin görüntü formu. Bu parametre, biçim `DisplayText` `simple`' e ayarlandığında sağlanan parametreyle aynıdır. |

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
