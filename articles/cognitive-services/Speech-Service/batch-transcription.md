---
title: Toplu transkripsiyon nedir - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Azure Blobs gibi büyük miktarda sesin depoda transkripsiyonu yapmak istiyorsanız toplu transkripsiyon idealdir. Özel REST API'sini kullanarak, paylaşılan erişim imzası (SAS) URI ile ses dosyalarını işaret edebilir ve eşitsiz bir şekilde transkripsiyonları alabilirsiniz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: wolfma
ms.openlocfilehash: fb39f1ec83416ee8ab2a33b514971110db0c0b17
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80668847"
---
# <a name="what-is-batch-transcription"></a>Toplu transkripsiyon nedir?

Toplu transkripsiyon, depolama alanında büyük miktarda ses transkripsiyonu yapmanızı sağlayan bir dizi REST API işlemidir. Paylaşılan erişim imzası (SAS) URI ile ses dosyalarını işaret edebilir ve eşitsiz bir şekilde transkripsiyon sonuçlarını alabilirsiniz.

Asynchronous konuşma-metin transkripsiyon özelliklerinden sadece biridir. Aşağıdaki yöntemleri aramak için toplu transkripsiyon REST API'lerini kullanabilirsiniz:



|    Toplu Transkripsiyon Operasyonu                                             |    Yöntem    |    REST API Çağrı                                   |
|------------------------------------------------------------------------------|--------------|----------------------------------------------------|
|    Yeni bir transkripsiyon oluşturur.                                              |    POST      |    api/speechtotext/v2.0/transkripsiyonları            |
|    Kimlik doğrulaması yapılan abonelik için transkripsiyonların listesini alır.    |    GET       |    api/speechtotext/v2.0/transkripsiyonları            |
|    Çevrimdışı transkripsiyonlar için desteklenen yerel leştirilmiş yerel lerin listesini alır.              |    GET       |    api/speechtotext/v2.0/transkripsiyonlar/yerel    |
|    Kimliğiyle tanımlanan transkripsiyonun mutable ayrıntılarını güncelleştirir.    |    Yama     |    api/speechtotext/v2.0/transkripsiyonlar/{id}       |
|    Belirtilen transkripsiyon görevini siler.                                 |    DELETE    |    api/speechtotext/v2.0/transkripsiyonlar/{id}       |
|    Verilen kimlikle tanımlanan transkripsiyonu alır.                        |    GET       |    api/speechtotext/v2.0/transkripsiyonlar/{id}       |




[Swagger belgesi](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A)olarak kullanılabilen ayrıntılı API'yi gözden `Custom Speech transcriptions`geçirip test edebilirsiniz.

Toplu transkripsiyon işleri en iyi çaba temelinde zamanlanır. Şu anda bir işin çalışan duruma ne zaman değdiyi ne zaman değiştiş tir diye bir tahmin bulunmamaktadır. Normal sistem yükü altında, birkaç dakika içinde gerçekleşmesi gerekir. Bir kez çalışan durumda, gerçek transkripsiyon ses gerçek zamanlı daha hızlı işlenir.

Kullanımı kolay API'nin yanında, özel uç noktaları dağıtmanız gerekmez ve gözlemlemek için eşzamanlılık gereksinimleriniz yoktur.

## <a name="prerequisites"></a>Ön koşullar

### <a name="subscription-key"></a>Abonelik Anahtarı

Konuşma hizmetinin tüm özelliklerinde olduğu gibi, [Başlat kılavuzumuzu](get-started.md)takip ederek [Azure portalından](https://portal.azure.com) bir abonelik anahtarı oluşturursunuz.

>[!NOTE]
> Toplu transkripsiyon kullanmak için Konuşma hizmeti için standart bir abonelik (S0) gereklidir. Ücretsiz abonelik anahtarları (F0) çalışmıyor. Daha fazla bilgi için [fiyatlandırma ve limitler](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)e bakın.

### <a name="custom-models"></a>Özel modeller

Akustik veya dil modellerini özelleştirmeyi planlıyorsanız, [akustik modelleri özelleştir](how-to-customize-acoustic-models.md) ve [özelleştirme dil modellerini](how-to-customize-language-model.md)tasarla'daki adımları izleyin. Oluşturulan modelleri toplu transkripsiyonda kullanmak için, onların model kimliklerini kullanmanız gerekir. Modelin ayrıntılarını incelerken model kimliğini alabilirsiniz. Toplu transkripsiyon hizmeti için dağıtılan özel bir bitiş noktası gerekmez.

## <a name="the-batch-transcription-api"></a>Toplu Transkripsiyon API

### <a name="supported-formats"></a>Desteklenen biçimler

Toplu Transkripsiyon API aşağıdaki biçimleri destekler:

| Biçimlendir | Codec | Bitrate | Örnek Oranı                     |
|--------|-------|---------|---------------------------------|
| WAV    | PCM   | 16 bit  | 8 kHz veya 16 kHz, mono veya stereo |
| Mp3    | PCM   | 16 bit  | 8 kHz veya 16 kHz, mono veya stereo |
| Ogg    | Opus  | 16 bit  | 8 kHz veya 16 kHz, mono veya stereo |

Stereo ses akışları için, transkripsiyon sırasında sol ve sağ kanallar ayrılır. Her kanal için bir JSON sonuç dosyası oluşturuluyor. Söyleyiş başına oluşturulan zaman damgaları, geliştiricinin sıralı bir son transkript oluşturmasına olanak tanır.

### <a name="configuration"></a>Yapılandırma

Yapılandırma parametreleri JSON olarak sağlanır:

```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to use, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "ProfanityFilterMode": "None | Removed | Tags | Masked",
    "PunctuationMode": "None | Dictated | Automatic | DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True | False",
    "AddSentiment" : "True | False",
    "AddDiarization" : "True | False",
    "TranscriptionResultsContainerUrl" : "<service SAS URI to Azure container to store results into (write permission required)>"
  }
}
```

### <a name="configuration-properties"></a>Yapılandırma özellikleri

Transkripsiyon yapılandırmak için bu isteğe bağlı özellikleri kullanın:

:::row:::
   :::column span="1":::
      **Parametre**
   :::column-end:::
   :::column span="2":::
      **Açıklama**
:::row-end:::
:::row:::
   :::column span="1":::
      `ProfanityFilterMode`
   :::column-end:::
   :::column span="2":::
      Tanıma sonuçlarında küfürle nasıl başa çıkılacağını belirtir. Kabul edilen `None` değerler küfür filtrelemi `Masked` devre dışı, küfür leri yıldız yıldızlarıyla değiştirmek, `Removed` sonuçtan tüm küfürleri kaldırmak veya `Tags` "küfür" etiketleri eklemektir. Varsayılan ayar `Masked` değeridir.
:::row-end:::
:::row:::
   :::column span="1":::
      `PunctuationMode`
   :::column-end:::
   :::column span="2":::
      Tanıma sonuçlarında noktalama işaretlerinin nasıl işleyeceğini belirtir. Kabul edilen `None` değerler noktalama işaretlerini devre `Dictated` dışı bırakmak, açık (konuşulan) `Automatic` noktalama işaretini ima etmek, kod `DictatedAndAutomatic` çözücünün noktalama işaretleriyle başa çıkmasına izin vermek veya dikte edilmiş ve otomatik noktalama işaretini kullanmaktır. Varsayılan ayar `DictatedAndAutomatic` değeridir.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddWordLevelTimestamps`
   :::column-end:::
   :::column span="2":::
      Çıktıya sözcük düzeyi zaman damgalarının eklenmesi gerekip gerekeceğini belirtir. Kabul edilen `true` değerler, sözcük düzeyi `false` zaman damgalarını ve (varsayılan değer) devre dışı düşürecek şekilde etkinleştirilir.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddSentiment`
   :::column-end:::
   :::column span="2":::
      Duygu analizinin söyleyeme uygulanıp uygulanamaması gerektiğini belirtir. Kabul edilen `true` değerler etkinleştirmek ve `false` (varsayılan değer) devre dışı bırakılır.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddDiarization`
   :::column-end:::
   :::column span="2":::
      İki ses içeren mono kanal olması beklenen girişte ishal analizi yapılması gerektiğini belirtir. Kabul edilen `true` değerler, isisization `false` ve (varsayılan değer) devre dışı bırakılır. Aynı zamanda `AddWordLevelTimestamps` doğru ayarlanmalıdır.
:::row-end:::
:::row:::
   :::column span="1":::
      `TranscriptionResultsContainerUrl`
   :::column-end:::
   :::column span="2":::
      Azure'da yazılabilir bir kapsayıcıya [hizmet SAS](../../storage/common/storage-sas-overview.md) ile isteğe bağlı URL. Sonuç bu kapsayıcıda saklanır.
:::row-end:::

### <a name="storage"></a>Depolama

Toplu transkripsiyon, ses okuma ve depolamaya transkripsiyon yazma için [Azure Blob depolamasını](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) destekler.

## <a name="the-batch-transcription-result"></a>Toplu transkripsiyon sonucu

Mono giriş ses için bir transkripsiyon sonuç dosyası oluşturuluyor. Stereo giriş ses için iki transkripsiyon sonuç dosyası oluşturulmaktadır. Her biri bu yapıya sahiptir:

```json
{
  "AudioFileResults":[
    {
      "AudioFileName": "Channel.0.wav | Channel.1.wav"      'maximum of 2 channels supported'
      "AudioFileUrl": null                                  'always null'
      "AudioLengthInSeconds": number                        'Real number. Two decimal places'
      "CombinedResults": [
        {
          "ChannelNumber": null                             'always null'
          "Lexical": string
          "ITN": string
          "MaskedITN": string
          "Display": string
        }
      ]
      SegmentResults:[                                      'for each individual segment'
        {
          "RecognitionStatus": "Success | Failure"
          "ChannelNumber": null
          "SpeakerId": null | "1 | 2"                       'null if no diarization
                                                             or stereo input file, the
                                                             speakerId as a string if
                                                             diarization requested for
                                                             mono audio file'
          "Offset": number                                  'time in ticks (1 tick is 100 nanosec)'
          "Duration": number                                'time in ticks (1 tick is 100 nanosec)'
          "OffsetInSeconds" : number                        'Real number. Two decimal places'
          "DurationInSeconds" : number                      'Real number. Two decimal places'
          "NBest": [
            {
              "Confidence": number                          'between 0 and 1'
              "Lexical": string
              "ITN": string
              "MaskedITN": string
              "Display": string
              "Sentiment":
                {                                           'this is omitted if sentiment is
                                                             not requested'
                  "Negative": number                        'between 0 and 1'
                  "Neutral": number                         'between 0 and 1'
                  "Positive": number                        'between 0 and 1'
                }
              "Words": [
                {
                  "Word": string
                  "Offset": number                          'time in ticks (1 tick is 100 nanosec)'
                  "Duration": number                        'time in ticks (1 tick is 100 nanosec)'
                  "OffsetInSeconds": number                 'Real number. Two decimal places'
                  "DurationInSeconds": number               'Real number. Two decimal places'
                  "Confidence": number                      'between 0 and 1'
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

Sonuç şu formları içerir:

| Form        | İçerik                                                                                                                                                  |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Lexical`   | Gerçek kelimeler tanındı.                                                                                                                             |
| `ITN`       | Tanınan metnin ters metin normalleştirilmiş biçimi. Kısaltmalar ("doctor smith" to "dr smith"), telefon numaraları ve diğer dönüşümler uygulanır. |
| `MaskedITN` | Küfür maskeleme ile ITN formu uygulanır.                                                                                                             |
| `Display`   | Tanınan metnin görüntü formu. Eklenen noktalama işaretleri ve büyük harf dahildir.                                                             |

## <a name="speaker-separation-diarization"></a>Hoparlör ayrımı (Isarizasyon)

Ishal, hoparlörleri bir ses parçasında ayırma işlemidir. Toplu işlem hattımız isisizasyonu destekler ve mono kanal kayıtlarında iki hoparlörü tanıyabilir. Özellik stereo kayıtlarında kullanılamaz.

Tüm transkripsiyon `SpeakerId`çıktısı bir . İshal kullanılmazsa, JSON çıkışında gösterir. `"SpeakerId": null` Ishal için iki sesi destekliyoruz, bu `"1"` nedenle `"2"`hoparlörler veya .

Ishal talebinde bulunmak için, http isteğinde aşağıda gösterildiği gibi ilgili parametreyi eklemeniz yeterlidir.

 ```json
{
  "recordingsUrl": "<URL to the Azure blob to transcribe>",
  "models": [{"Id":"<optional acoustic model ID>"},{"Id":"<optional language model ID>"}],
  "locale": "<locale to us, for example en-US>",
  "name": "<user defined name of the transcription batch>",
  "description": "<optional description of the transcription>",
  "properties": {
    "AddWordLevelTimestamps" : "True",
    "AddDiarization" : "True"
  }
}
```

Yukarıdaki istekteki parametrelerin gösterdiği gibi sözcük düzeyinde ki zaman damgalarının da 'açık' olması gerekir.

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Duygu özelliği ses ifade duyguları tahmin ediyor. Duygu için `Negative`0 ve 1 arasında bir `Neutral`değer `Positive` ile ifade edilir , ve duygu. Örneğin, duygu analizi çağrı merkezi senaryolarında kullanılabilir:

- Müşteri memnuniyeti hakkında bilgi edinin
- Acentelerin performansı hakkında bilgi edinin (çağrıları alan ekip)
- Bir aramanın negatif yönde dönüş yaptığında tam zamanı bulma
- Negatif bir çağrıyı olumlu bir yöne dönüştürürken ne iyi gitti?
- Müşterilerin bir ürün veya hizmet hakkında nelerden hoşlanıp hoşlanmadıklarını belirleme

Duygusallık sözlü formuna göre ses segmenti başına puanlanır. Bu ses segmentindeki tüm metin duyarlılığı hesaplamak için kullanılır. Transkripsiyon un tamamı için hiçbir toplu duyarlılık hesaplanmıyor. Şu anda duygu analizi sadece İngilizce dil için kullanılabilir.

Bir JSON çıktı örneği aşağıdaki gibi görünür:

```json
{
  "AudioFileResults": [
    {
      "AudioFileName": "Channel.0.wav",
      "AudioFileUrl": null,
      "SegmentResults": [
        {
          "RecognitionStatus": "Success",
          "ChannelNumber": null,
          "Offset": 400000,
          "Duration": 13300000,
          "NBest": [
            {
              "Confidence": 0.976174,
              "Lexical": "what's the weather like",
              "ITN": "what's the weather like",
              "MaskedITN": "what's the weather like",
              "Display": "What's the weather like?",
              "Words": null,
              "Sentiment": {
                "Negative": 0.206194,
                "Neutral": 0.793785,
                "Positive": 0.0
              }
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="best-practices"></a>En iyi uygulamalar

Transkripsiyon hizmeti gönderilen transkripsiyonların çok sayıda işleyebilir. Transkripsiyonlarınızın `GET` durumunu [transkripsiyon yöntemi](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/GetTranscriptions)ile sorgulayabilirsiniz. `take` Parametreyi (birkaç yüz) belirterek bilgileri makul bir boyuta döndürür. Sonuçları aldıktan sonra [transkripsiyonları](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/DeleteTranscription) hizmetten düzenli olarak silin. Bu, transkripsiyon yönetimi aramalarından gelen hızlı yanıtları garanti eder.

## <a name="sample-code"></a>Örnek kod

Tam örnekler alt dizinin içindeki [GitHub örnek deposunda](https://aka.ms/csspeech/samples) `samples/batch` mevcuttur.

Örnek kodu abonelik bilgileriniz, hizmet bölgeniz, yazıya uygun ses dosyasını işaret eden SAS URI ve özel bir akustik veya dil modeli kullanmak istediğinizde model iyelikleri ile özelleştirmeniz gerekir.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Örnek kod istemciyi ayarlar ve transkripsiyon isteğini gönderir. Daha sonra durum bilgileri için anketler ve transkripsiyon ilerleme hakkında ayrıntıları yazdırmak.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Önceki aramalar hakkında ayrıntılı bilgi için [Swagger belgemize](https://westus.cris.ai/swagger/ui/index)bakın. Burada gösterilen tam örnek için `samples/batch` alt dizinde [GitHub'a](https://aka.ms/csspeech/samples) gidin.

Ses deftere nakletmek ve transkripsiyon durumu almak için eşzamanlı kuruluma dikkat edin. Oluşturduğunuz istemci bir .NET HTTP istemcisidir. Ses dosyası `PostTranscriptions` ayrıntılarını göndermek için bir yöntem `GetTranscriptions` ve sonuçları almak için bir yöntem vardır. `PostTranscriptions`bir tutamaç `GetTranscriptions` döndürür ve transkripsiyon durumunu almak için bir tutamaç oluşturmak için kullanır.

Geçerli örnek kod özel bir model belirtmez. Hizmet, dosyayı veya dosyaları çevirmek için temel modelleri kullanır. Modelleri belirtmek için, akustik ve dil modeli için model disleri ile aynı yöntemi aktarabilirsiniz.

> [!NOTE]
> Temel transkripsiyonlar için, temel modeller için kimliği bildirmeniz gerekmez. Yalnızca bir dil modeli kimliği (ve akustik model kimliği) belirtirseniz, eşleşen bir akustik model otomatik olarak seçilir. Yalnızca akustik model kimliği belirtirseniz, eşleşen bir dil modeli otomatik olarak seçilir.

## <a name="download-the-sample"></a>Örneği indirme

Örneği `samples/batch` [GitHub örnek deposunda](https://aka.ms/csspeech/samples)dizinde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
