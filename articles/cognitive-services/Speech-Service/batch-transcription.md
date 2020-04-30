---
title: Batch dökümünü alma-konuşma hizmeti nedir?
titleSuffix: Azure Cognitive Services
description: Toplu döküm, depolama alanında Azure Blob 'Ları gibi büyük miktarda ses eklemek istiyorsanız idealdir. Adanmış REST API kullanarak, paylaşılan erişim imzası (SAS) URI 'SI olan ses dosyalarını işaret edebilir ve zaman uyumsuz olarak alma yapabilirsiniz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: wolfma
ms.openlocfilehash: 46bfabfb2ccf091fd5dc0fcf0e9b447bad7c34d1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82208627"
---
# <a name="what-is-batch-transcription"></a>Toplu iş dökümü nedir?

Batch dökümü, depolamada büyük miktarda ses eklemenizi sağlayan bir REST API işlemleri kümesidir. Paylaşılan erişim imzası (SAS) URI 'SI olan ses dosyalarını işaret edebilir ve zaman uyumsuz olarak alma sonuçlarıyla karşılaşabilirsiniz.

Zaman uyumsuz konuşmayı metne dönüştürme, özelliklerden yalnızca biridir. Aşağıdaki yöntemleri çağırmak için Batch transcripts REST API 'Lerini kullanabilirsiniz:



|    Toplu iş dökümü Işlemi                                             |    Yöntem    |    REST API çağrısı                                   |
|------------------------------------------------------------------------------|--------------|----------------------------------------------------|
|    Yeni bir döküm oluşturur.                                              |    POST      |    API/speechtotext/v 2.0/döküm            |
|    Kimliği doğrulanmış aboneliğin dökümlerini listesini alır.    |    GET       |    API/speechtotext/v 2.0/döküm            |
|    Çevrimdışı döküm için desteklenen yerel ayarların bir listesini alır.              |    GET       |    api/speechtotext/v 2.0/transcripts/yerel ayarlar    |
|    KIMLIĞI tarafından tanımlanan dökümün değişebilir ayrıntılarını günceller.    |    DÜZELTMESI     |    api/speechtotext/v 2.0/transcripts/{id}       |
|    Belirtilen döküm görevini siler.                                 |    DELETE    |    api/speechtotext/v 2.0/transcripts/{id}       |
|    Verilen KIMLIK tarafından tanımlanan dökümü alır.                        |    GET       |    api/speechtotext/v 2.0/transcripts/{id}       |




Başlık `Custom Speech transcriptions`altında [Swagger belgesi](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A)olarak kullanılabilen ayrıntılı API 'yi gözden geçirebilir ve test edebilirsiniz.

Toplu iş dökümü işleri en iyi çaba temelinde zamanlanır. Şu anda bir işin çalışma durumuna ne zaman değiştiği hakkında bir tahmin yoktur. Normal sistem yükü altında, dakikalar içinde gerçekleşmelidir. Çalışma durumunda, gerçek döküm gerçek zamanlı olarak daha hızlı işlenir.

Kullanımı kolay API 'nin yanında, Özel uç noktaları dağıtmanız gerekmez ve gözlemlemeye yönelik eşzamanlılık gereksinimleriniz yoktur.

## <a name="prerequisites"></a>Ön koşullar

### <a name="subscription-key"></a>Abonelik Anahtarı

Konuşma hizmetinin tüm özelliklerinde olduğu gibi, [kullanmaya başlama kılavuzumuzu](get-started.md)izleyerek [Azure Portal](https://portal.azure.com) bir abonelik anahtarı oluşturursunuz.

>[!NOTE]
> Toplu iş dökümünü kullanmak için, konuşma hizmeti için standart bir abonelik (S0) gereklidir. Ücretsiz abonelik anahtarları (F0) çalışmıyor. Daha fazla bilgi için bkz. [fiyatlandırma ve sınırlar](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Özel modeller

Akustik veya dil modellerini özelleştirmeyi planlıyorsanız, [akustik modelleri özelleştirme](how-to-customize-acoustic-models.md) ve [Özelleştirme dili modellerini tasarlama](how-to-customize-language-model.md)' daki adımları izleyin. Toplu iş dökümlerinde oluşturulan modelleri kullanmak için model kimliklerine ihtiyacınız vardır. Modelin ayrıntılarını inceleyebileceğiniz zaman model KIMLIĞINI alabilirsiniz. Toplu iş dökümü hizmeti için dağıtılan özel bir uç nokta gerekli değildir.

## <a name="the-batch-transcription-api"></a>Toplu Iş dökümü API 'SI

### <a name="supported-formats"></a>Desteklenen biçimler

Toplu Iş dökümü API 'SI aşağıdaki biçimleri destekler:

| Biçimlendir | Bileşeni | Bit hızı | Örnek hız                     |
|--------|-------|---------|---------------------------------|
| WAV    | PCM   | 16 bit  | 8 kHz veya 16 kHz, mono veya stereo |
| ÇA    | PCM   | 16 bit  | 8 kHz veya 16 kHz, mono veya stereo |
| OGG    | OPUS 'LAR  | 16 bit  | 8 kHz veya 16 kHz, mono veya stereo |

Stereo ses akışları için, sol ve sağ kanallar, döküm sırasında bölünür. Her kanal için bir JSON sonuç dosyası oluşturuluyor. Söylenişi başına oluşturulan zaman damgaları, geliştiricinin sıralı bir son döküm oluşturmasını sağlar.

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

Dökümü yapılandırmak için bu isteğe bağlı özellikleri kullanın:

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
      Tanıma sonuçlarında küfür nasıl işleneceğini belirtir. Kabul edilen değerler `None` , küfür filtrelemeyi `Masked` devre dışı bırakmak, küfür ile `Removed` tüm küfür kaldırmak veya `Tags` "küfür" etiketleri eklemek için. Varsayılan ayar `Masked` değeridir.
:::row-end:::
:::row:::
   :::column span="1":::
      `PunctuationMode`
   :::column-end:::
   :::column span="2":::
      Tanıma sonuçlarında noktalama işaretlerinin nasıl işleneceğini belirtir. Kabul edilen değerler `None` , `Dictated` açık (konuşulan) noktalama `Automatic` işareti, kod çözücüsünün noktalama ile uğraşmasına veya `DictatedAndAutomatic` dikte ve otomatik noktalama kullanımına izin vermek için noktalama işaretlerini devre dışı bırakır. Varsayılan ayar `DictatedAndAutomatic` değeridir.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddWordLevelTimestamps`
   :::column-end:::
   :::column span="2":::
      Sözcük düzeyi tarih damgalarının çıktıya eklenip eklenmesinin gerekip gerekmediğini belirtir. Kabul edilen değerler `true` , devre dışı bırakmak için sözcük `false` düzeyi zaman damgalarını ve (varsayılan değer) etkin hale gelir.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddSentiment`
   :::column-end:::
   :::column span="2":::
      Yaklaşım analizinin, utterance 'e uygulanması gerekip gerekmediğini belirtir. Kabul edilen değerler `true` etkinleştirilir ve `false` devre dışı bırakmak için (varsayılan değer). Daha fazla ayrıntı için bkz. [yaklaşım Analizi](#sentiment-analysis) .
:::row-end:::
:::row:::
   :::column span="1":::
      `AddDiarization`
   :::column-end:::
   :::column span="2":::
      Giriş analizinin, iki ses içeren mono kanalı olması beklenen girişte gerçekleştirilmesi gerektiğini belirtir. Kabul edilen değerler `true` devre dışı bırakılır ve `false` devre dışı bırakmak için (varsayılan değer). Ayrıca, true `AddWordLevelTimestamps` olarak ayarlanması gerekir.
:::row-end:::
:::row:::
   :::column span="1":::
      `TranscriptionResultsContainerUrl`
   :::column-end:::
   :::column span="2":::
      Azure 'da yazılabilir bir kapsayıcıya [HIZMET SAS](../../storage/common/storage-sas-overview.md) ile Isteğe bağlı URL. Sonuç bu kapsayıcıda saklanır.
:::row-end:::

### <a name="storage"></a>Depolama

Toplu iş dökümü, ses okumak ve depoya yazmak için [Azure Blob depolamayı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) destekler.

## <a name="the-batch-transcription-result"></a>Toplu iş dökümü sonucu

Mono giriş sesi için bir döküm sonuç dosyası oluşturuluyor. Stereo giriş sesi için iki döküm sonuç dosyası oluşturuluyor. Her birinin bu yapıya sahiptir:

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

:::row:::
   :::column span="1":::
      **Form**
   :::column-end:::
   :::column span="2":::
      **İçerik**
:::row-end:::
:::row:::
   :::column span="1":::
      `Lexical`
   :::column-end:::
   :::column span="2":::
      Tanınan gerçek sözcükler.
:::row-end:::
:::row:::
   :::column span="1":::
      `ITN`
   :::column-end:::
   :::column span="2":::
      Tanınan metnin ters metin normalleştirilmiş biçimi. Kısaltmalar ("Doctor Smith"-"Dr Smith"), telefon numaraları ve diğer dönüşümler uygulanır.
:::row-end:::
:::row:::
   :::column span="1":::
      `MaskedITN`
   :::column-end:::
   :::column span="2":::
      Küfür maskeleme uygulanmış ıTYPEFORM.
:::row-end:::
:::row:::
   :::column span="1":::
      `Display`
   :::column-end:::
   :::column span="2":::
      Tanınan metnin görüntüleme formu. Eklenen noktalama işaretleri ve büyük harfler dahil edilir.
:::row-end:::

## <a name="speaker-separation-diarization"></a>Konuşmacı ayrımı (Diarleştirme)

Kararlama, hoparlörleri ses parçasıyla ayırma işlemidir. Batch Işlem hatmız, daha fazla seçim destekleiyor ve mono kanal kayıtlarında iki hoparlörleri tanıyor. Özellik, stereo kayıtlarında kullanılamaz.

Tüm döküm çıktısı bir `SpeakerId`içerir. Kullanılması kullanılmazsa JSON çıktısında gösterilir `"SpeakerId": null` . İki sesi destekliyoruz, böylece hoparlörler veya `"1"` `"2"`olarak tanımlanır.

Daha fazla bilgi almak için, aşağıda gösterildiği gibi, HTTP isteğine ilgili parametresini eklemeniz yeterlidir.

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

Yukarıdaki istekteki parametreler gösterildiği gibi, sözcük düzeyi zaman damgalarının de ' açık ' olması gerekir.

## <a name="sentiment-analysis"></a>Yaklaşım analizi

Yaklaşım özelliği, ses içinde ifade edilen yaklaşımı tahmin eder. Yaklaşım,, ve `Negative` `Neutral` `Positive` yaklaşım için 0 ile 1 arasında bir değere göre ifade edilir. Örneğin, yaklaşım Analizi çağrı merkezi senaryolarında kullanılabilir:

- Müşteri memnuniyeti hakkında Öngörüler alın
- Aracıların performansı hakkında öngörüler elde edin (çağrıları alan takım)
- Bir çağrının negatif yönde bir değer aldığı zaman içindeki kesin noktayı bulma
- Negatif bir çağrıyı pozitif bir yöne çevirip iyi ne oldu?
- Beğendikleri müşterileri ve bir ürün veya hizmet hakkında neleri beğendiklerinizi tanımlayın

Yaklaşım, sözcük temelli biçime göre ses kesimine göre puanlanır. Bu ses segmentinin içindeki metnin tamamı, yaklaşımı hesaplamak için kullanılır. Tüm döküm için bir toplama yaklaşımı hesaplanmıyor. Yaklaşım Analizi Şu anda yalnızca Ingilizce dilinde kullanılabilir.

> [!NOTE]
> Bunun yerine Microsoft Metin Analizi API'si kullanmanızı öneririz. Anahtar tümceciği ayıklama, otomatik dil algılama ve daha fazlası gibi yaklaşım analizinin ötesinde daha gelişmiş özellikler sunar. [Metin analizi belgelerindeki](https://azure.microsoft.com/services/cognitive-services/text-analytics/)bilgileri ve örnekleri bulabilirsiniz.
>

Bir JSON çıkış örneği aşağıdaki gibi görünür:

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

Döküm hizmeti, çok sayıda gönderilen dökümü işleyebilir. Döküm `GET` [yönteminde](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/GetTranscriptions)bir ile döküm durumunu sorgulayabilirsiniz. Parametresi (birkaç yüz) belirterek, `take` bilgileri makul bir boyuta geri döndürür. Sonuçları aldıktan sonra hizmetten düzenli olarak döküm [silme](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/DeleteTranscription) . Bu, döküm yönetimi çağrılarındaki Hızlı yanıtları garanti eder.

## <a name="sample-code"></a>Örnek kod

Tüm örnekler, `samples/batch` alt dizinin içindeki [GitHub örnek deposunda](https://aka.ms/csspeech/samples) bulunur.

Örnek kodu Abonelik bilgileriniz, hizmet bölgesi, ses dosyasına işaret edilecek SAS URI 'SI ve özel bir akustik veya dil modeli kullanmak istiyorsanız model kimlikleri ile özelleştirmeniz gerekir.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Örnek kod, istemciyi ayarlar ve döküm isteğini gönderir. Ardından durum bilgilerini yoklar ve döküm ilerleme durumuyla ilgili ayrıntıları yazdırır.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Önceki çağrılar hakkında tam Ayrıntılar için [Swagger belgemizi](https://westus.cris.ai/swagger/ui/index)inceleyin. Burada gösterilen tam örnek için `samples/batch` alt dizinde [GitHub](https://aka.ms/csspeech/samples) ' a gidin.

Ses ve döküm durumunu alma hakkında zaman uyumsuz Kurulum ' a göz atın. Oluşturduğunuz istemci bir .NET HTTP istemcsahiptir. Ses dosyası ayrıntılarını `PostTranscriptions` ve sonuçları almak için bir `GetTranscriptions` yöntemi göndermek için bir yöntem vardır. `PostTranscriptions`bir tanıtıcı döndürür ve `GetTranscriptions` bunu, döküm durumunu almak için bir tanıtıcı oluşturmak üzere kullanır.

Geçerli örnek kod özel bir model belirtmiyor. Hizmet, dosya veya dosyaları derleyen taban çizgisi modellerini kullanır. Modelleri belirtmek için, akustik ve dil modeli için model kimlikleriyle aynı yöntemi geçirebilirsiniz.

> [!NOTE]
> Ana hat düzenlemeleri için, taban çizgisi modellerinin KIMLIĞINI bildirmeniz gerekmez. Yalnızca bir dil modeli KIMLIĞI belirtirseniz (ve akustik model KIMLIĞI yoksa), eşleşen bir akustik model otomatik olarak seçilir. Yalnızca bir akustik model KIMLIĞI belirtirseniz, eşleşen bir dil modeli otomatik olarak seçilir.

## <a name="download-the-sample"></a>Örneği indirme

Örneği `samples/batch` [GitHub örnek deposundaki](https://aka.ms/csspeech/samples)dizinde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
