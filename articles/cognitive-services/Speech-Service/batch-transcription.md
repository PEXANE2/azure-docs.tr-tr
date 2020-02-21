---
title: Toplu iş dökümü kullanma-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Batch transkripsiyonu, depolama, Azure BLOB'ları gibi ses büyük bir miktarını konuşmaların istiyorsanız idealdir. Adanmış REST API'sini kullanarak bir paylaşılan erişim imzası (SAS) URI ses dosyalarının üzerine gelin ve döküm zaman uyumsuz olarak alır.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: panosper
ms.openlocfilehash: 6d5ec5f798617d03072ec5931b0d1d3623df3d42
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77500007"
---
# <a name="how-to-use-batch-transcription"></a>Toplu iş dökümünü kullanma

Toplu iş dökümü, depolamada büyük miktarda ses dökümünü yapmak için idealdir. Adanmış REST API kullanarak, paylaşılan erişim imzası (SAS) URI 'SI olan ses dosyalarını işaret edebilir ve zaman uyumsuz olarak alma sonuçlarını alabilirsiniz.

API, zaman uyumsuz konuşmayı metne dönüştürme ve diğer özellikleri sunar. Yöntemleri şunları yapmak için REST API kullanabilirsiniz:

- Toplu işleme istekleri oluşturma
- Durumu Sorgula
- Döküm sonuçlarını indir
- Hizmetten döküm bilgilerini sil

Ayrıntılı API, `Custom Speech transcriptions`başlığı altında [Swagger belgesi](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A)olarak kullanılabilir.

Toplu iş dökümü işleri en iyi çaba temelinde zamanlanır. Şu anda bir işin çalışma durumuna ne zaman değişene ilişkin bir tahmin yoktur. Normal sistem yükü altında, dakikalar içinde gerçekleşmelidir. Çalışma durumunda, gerçek döküm gerçek zamanlı olarak daha hızlı işlenir.

Kullanımı kolay API 'nin yanında, Özel uç noktaları dağıtmanız gerekmez ve gözlemlemeye yönelik eşzamanlılık gereksinimleriniz yoktur.

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscription-key"></a>Abonelik anahtarı

Konuşma hizmetinin tüm özelliklerinde olduğu gibi, [kullanmaya başlama kılavuzumuzu](get-started.md)izleyerek [Azure Portal](https://portal.azure.com) bir abonelik anahtarı oluşturursunuz.

>[!NOTE]
> Toplu iş dökümünü kullanmak için, konuşma hizmeti için standart bir abonelik (S0) gereklidir. Ücretsiz Abonelik anahtarları (F0) işe yaramaz. Daha fazla bilgi için bkz. [fiyatlandırma ve sınırlar](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Özel modeller

Akustik veya dil modellerini özelleştirmeyi planlıyorsanız, [akustik modelleri özelleştirme](how-to-customize-acoustic-models.md) ve [Özelleştirme dili modellerini tasarlama](how-to-customize-language-model.md)' daki adımları izleyin. Toplu iş dökümlerinde oluşturulan modelleri kullanmak için model kimliklerine ihtiyacınız vardır. Modelin ayrıntılarını inceleyebileceğiniz zaman model KIMLIĞINI alabilirsiniz. Toplu iş dökümü hizmeti için dağıtılan özel bir uç nokta gerekli değildir.

## <a name="the-batch-transcription-api"></a>Batch tanıma API'si

### <a name="supported-formats"></a>Desteklenen biçimler

Batch tanıma API'si, aşağıdaki biçimlerde destekler:

| Biçimlendir | Codec bileşeni | Bit hızı | Örnek hızı |
|--------|-------|---------|-------------|
| WAV | PCM | 16-bit | 8 kHz veya 16 kHz, mono veya stereo |
| MP3 | PCM | 16-bit | 8 kHz veya 16 kHz, mono veya stereo |
| OGG | GEÇERLİ | 16-bit | 8 kHz veya 16 kHz, mono veya stereo |

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
      **Parametresinin**
   :::column-end:::
   :::column span="2":::
      **Açıklama**
:::row-end:::
:::row:::
   :::column span="1":::
      `ProfanityFilterMode`
   :::column-end:::
   :::column span="2":::
      Tanıma sonuçları küfür nasıl ele alınacağını belirtir. Kabul edilen değerler, küfür filtrelemesini devre dışı bırakmak için `None`, küfür ile `Masked`, sonuçtan tüm küfür kaldırmak için `Removed` veya "küfür" etiketleri eklemek için `Tags`. Varsayılan ayar `Masked`.
:::row-end:::
:::row:::
   :::column span="1":::
      `PunctuationMode`
   :::column-end:::
   :::column span="2":::
      Noktalama işaretleri tanıma sonuçları nasıl ele alınacağını belirtir. Kabul edilen değerler, noktalama işaretlerini devre dışı bırakmak için `None`, Açık (konuşulan) noktalama işareti `Dictated`, kod çözücüsünün noktalama ile başa çıkmasına izin vermek için `Automatic` veya dikte ve otomatik noktalama kullanımı `DictatedAndAutomatic`. Varsayılan ayar `DictatedAndAutomatic`.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddWordLevelTimestamps`
   :::column-end:::
   :::column span="2":::
      Sözcük düzeyi tarih damgalarının çıktıya eklenip eklenmesinin gerekip gerekmediğini belirtir. Kabul edilen değerler, Word düzeyi zaman damgalarını etkinleştirmek için `true` ve devre dışı bırakmak için `false` (varsayılan değer).
:::row-end:::
:::row:::
   :::column span="1":::
      `AddSentiment`
   :::column-end:::
   :::column span="2":::
      Yaklaşım 'ın utterance 'e eklenmesi gerektiğini belirtir. Kabul edilen değerler, her ay için yaklaşım ve `false` (varsayılan değer) `true` devre dışı bırakmak için kullanılır.
:::row-end:::
:::row:::
   :::column span="1":::
      `AddDiarization`
   :::column-end:::
   :::column span="2":::
      İki ses içeren mono kanalı olması beklenen girişte, seçme analizinin gerçekleştirilmesi gerektiğini belirtir. Kabul edilen değerler, devre dışı bırakmak için `true` ve `false` (varsayılan değer) etkinleştirir. Ayrıca, `AddWordLevelTimestamps` true olarak ayarlanmasını gerektirir.
:::row-end:::
:::row:::
   :::column span="1":::
      `TranscriptionResultsContainerUrl`
   :::column-end:::
   :::column span="2":::
      Azure 'da yazılabilir bir kapsayıcıya [HIZMET SAS](../../storage/common/storage-sas-overview.md) ile Isteğe bağlı URL. Sonuç bu kapsayıcıda depolanacak.
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

|Biçim|İçerik|
|-|-|
|`Lexical`|Tanınan gerçek sözcükler.
|`ITN`|Tanınan metnin ters metin normalleştirilmiş biçimi. Kısaltmalar ("Doctor Smith"-"Dr Smith"), telefon numaraları ve diğer dönüşümler uygulanır.
|`MaskedITN`|Küfür maskeleme uygulanmış ıTYPEFORM.
|`Display`|Tanınan metnin görüntüleme formu. Buna eklenen noktalama işaretleri ve büyük harfleri dahildir.

## <a name="speaker-separation-diarization"></a>Konuşmacı ayrımı (Diarleştirme)

Kararlama, hoparlörleri ses parçasıyla ayırma işlemidir. Batch Işlem hatmız, daha fazla seçim destekleiyor ve mono kanal kayıtlarında iki hoparlörleri tanıyor. Özellik, stereo kayıtlarında kullanılamaz.

Tüm döküm çıktısı bir `SpeakerId`içerir. Eğer kullanım kullanılmıyorsa, JSON çıktısında `"SpeakerId": null` gösterir. Bu iki sesi destekliyoruz, böylece hoparlörler `"1"` veya `"2"`olarak tanımlanır.

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

Yaklaşım özelliği, ses içinde ifade edilen yaklaşımı tahmin eder. Yaklaşım, `Negative`, `Neutral`ve `Positive` yaklaşımı için 0 ile 1 arasında bir değere göre ifade edilir. Örneğin, yaklaşım Analizi çağrı merkezi senaryolarında kullanılabilir:

- Müşteri memnuniyeti hakkında Öngörüler alın
- Aracıların performansı hakkında öngörüler elde edin (çağrıları alan takım)
- Bir çağrının negatif yönde bir değer aldığı zaman içindeki kesin noktayı bulma
- Negatif bir çağrıyı pozitif bir yöne çevirip iyi ne oldu?
- Beğendikleri müşterileri ve bir ürün veya hizmet hakkında neleri beğendiklerinizi tanımlayın

Yaklaşım, sözcük temelli biçime göre ses kesimine göre puanlanır. Bu ses segmentinin içindeki metnin tamamı, yaklaşımı hesaplamak için kullanılır. Tüm döküm için bir toplama yaklaşımı hesaplanmıyor.

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

Döküm hizmeti, çok sayıda gönderilen dökümü işleyebilir. Döküm [yöntemindeki](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/GetTranscriptions)bir `GET` ile döküm durumunu sorgulayabilirsiniz. `take` parametresini (birkaç yüz) belirterek, döndürülen bilgileri makul bir boyuta tutun. Sonuçları aldıktan sonra hizmetten düzenli olarak döküm [silme](https://westus.cris.ai/swagger/ui/index#/Custom%20Speech%20transcriptions%3A/DeleteTranscription) . Bu, döküm yönetimi çağrılarındaki Hızlı yanıtları garanti eder.

## <a name="sample-code"></a>Örnek kod

Tüm örnekler, `samples/batch` alt dizininin içindeki [GitHub örnek deposunda](https://aka.ms/csspeech/samples) bulunur.

Örnek kodu Abonelik bilgileriniz, hizmet bölgesi, ses dosyasına işaret edilecek SAS URI 'SI ve özel bir akustik veya dil modeli kullanmak istiyorsanız model kimlikleri ile özelleştirmeniz gerekir.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Örnek kod, istemciyi ayarlar ve döküm isteğini gönderir. Daha sonra durum bilgilerini yoklayacak ve döküm ilerlemesiyle ilgili ayrıntıları yazdıracaktır.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Önceki çağrılar hakkında tam Ayrıntılar için [Swagger belgemizi](https://westus.cris.ai/swagger/ui/index)inceleyin. Burada gösterilen tam örnek için `samples/batch` alt dizininde [GitHub](https://aka.ms/csspeech/samples) ' a gidin.

Ses gönderme ve döküm durumu almak için zaman uyumsuz Kurulum not alın. Oluşturduğunuz .NET HTTP istemci istemcisidir. Ses dosyası ayrıntılarını göndermek için bir `PostTranscriptions` yöntemi ve sonuçları almak için bir `GetTranscriptions` yöntemi vardır. `PostTranscriptions` bir tanıtıcı döndürür ve `GetTranscriptions`, döküm durumunu almak üzere bir tanıtıcı oluşturmak için onu kullanır.

Geçerli örnek kod, özel bir model belirtmez. Hizmet, dosya veya dosyalar fotoğrafını için temel modelleri kullanır. Modelleri belirtmek için model kimliklerini akustik ve dil modeli için aynı yönteme geçirebilirsiniz.

> [!NOTE]
> Ana hat düzenlemeleri için, taban çizgisi modellerinin KIMLIĞINI bildirmeniz gerekmez. Yalnızca bir dil modeli KIMLIĞI belirtirseniz (ve akustik model KIMLIĞI yoksa), eşleşen bir akustik model otomatik olarak seçilir. Yalnızca bir akustik model KIMLIĞI belirtirseniz, eşleşen bir dil modeli otomatik olarak seçilir.

## <a name="download-the-sample"></a>Örneği indirme

Örneği, [GitHub örnek deposundaki](https://aka.ms/csspeech/samples)`samples/batch` dizininde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
