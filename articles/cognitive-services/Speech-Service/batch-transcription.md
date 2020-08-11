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
ms.openlocfilehash: 3e7f310f37bd016a73c589db3c9a23e197465427
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88053925"
---
# <a name="what-is-batch-transcription"></a>Toplu iş dökümü nedir?

Batch dökümü, depolamada büyük miktarda ses eklemenizi sağlayan bir REST API işlemleri kümesidir. Paylaşılan erişim imzası (SAS) URI 'SI olan ses dosyalarını işaret edebilir ve zaman uyumsuz olarak alma sonuçlarıyla karşılaşabilirsiniz. Yeni v 3.0 API 'SI ile bir veya daha fazla ses dosyası oluşturma veya bir depolama kapsayıcısının tamamını işleme seçeneğiniz vardır.

Zaman uyumsuz konuşmayı metne dönüştürme, özelliklerden yalnızca biridir. Aşağıdaki yöntemleri çağırmak için Batch transcripts REST API 'Lerini kullanabilirsiniz:



|    Toplu iş dökümü Işlemi                                             |    Yöntem    |    REST API çağrısı                                   |
|------------------------------------------------------------------------------|--------------|----------------------------------------------------|
|    Yeni bir döküm oluşturur.                                              |    POST      |    speechtotext/v 3.0/döküm            |
|    Kimliği doğrulanmış aboneliğin dökümlerini listesini alır.    |    GET       |    speechtotext/v 3.0/döküm            |
|    Çevrimdışı döküm için desteklenen yerel ayarların bir listesini alır.              |    GET       |    speechtotext/v 3.0/transcripts/yerel ayarlar    |
|    KIMLIĞI tarafından tanımlanan dökümün değişebilir ayrıntılarını günceller.    |    DÜZELTMESI     |    speechtotext/v 3.0/Transcriptions/{id}       |
|    Belirtilen döküm görevini siler.                                 |    DELETE    |    speechtotext/v 3.0/Transcriptions/{id}       |
|    Verilen KIMLIK tarafından tanımlanan dökümü alır.                        |    GET       |    speechtotext/v 3.0/Transcriptions/{id}       |
|    Verilen KIMLIK tarafından tanımlanan döküm sonuç dosyalarını alır.    |    GET       |    speechtotext/v 3.0/Transcriptions/{id}/dosya |




[Swagger belgesi](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)olarak KULLANıLABILEN ayrıntılı API 'yi gözden geçirebilir ve test edebilirsiniz.

Toplu iş dökümü işleri en iyi çaba temelinde zamanlanır. Şu anda bir işin çalışma durumuna ne zaman değiştiği hakkında bir tahmin yoktur. Normal sistem yükü altında, dakikalar içinde gerçekleşmelidir. Çalışma durumunda, gerçek döküm gerçek zamanlı olarak daha hızlı işlenir.

Kullanımı kolay API 'nin yanında, Özel uç noktaları dağıtmanız gerekmez ve gözlemlemeye yönelik eşzamanlılık gereksinimleriniz yoktur.

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscription-key"></a>Abonelik Anahtarı

Konuşma hizmetinin tüm özelliklerinde olduğu gibi, [kullanmaya başlama kılavuzumuzu](get-started.md)izleyerek [Azure Portal](https://portal.azure.com) bir abonelik anahtarı oluşturursunuz.

>[!NOTE]
> Toplu iş dökümünü kullanmak için, konuşma hizmeti için standart bir abonelik (S0) gereklidir. Ücretsiz abonelik anahtarları (F0) çalışmıyor. Daha fazla bilgi için bkz. [fiyatlandırma ve sınırlar](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Özel modeller

Modelleri özelleştirmeyi planlıyorsanız, [akustik özelleştirme](how-to-customize-acoustic-models.md) ve [dil özelleştirme](how-to-customize-language-model.md)bölümündeki adımları izleyin. Toplu iş dökümlerinde oluşturulan modelleri kullanmak için model konumuyla ihtiyacınız vardır. Modelin ayrıntılarını inceleyebileceğiniz zaman model konumunu alabilirsiniz ( `self` özellik). Toplu iş dökümü hizmeti için dağıtılan özel bir uç nokta *gerekli değildir* .

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

Yapılandırma parametreleri JSON (bir veya daha fazla tek dosya) olarak sağlanır:

```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "displayName": "Transcription of file using default model for en-US"
}
```

Yapılandırma parametreleri JSON (bir depolama kapsayıcısının tamamına göre işlenir) olarak sağlanır:

```json
{
  "contentContainerUrl": "<SAS URL to the Azure blob container to transcribe>",
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "displayName": "Transcription of container using default model for en-US"
}
```

Toplu iş dökümlerine özel eğitilen modeller kullanmak için, aşağıda gösterildiği gibi başvurulabilir:

```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "wordLevelTimestampsEnabled": true
  },
  "locale": "en-US",
  "model": {
    "self": "https://westus.api.cognitive.microsoft.com/speechtotext/v3.0/models/{id}"
  },
  "displayName": "Transcription of file using default model for en-US"
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
      `profanityFilterMode`
   :::column-end:::
   :::column span="2":::
      Tanıma sonuçlarında küfür nasıl işleneceğini belirtir. Kabul edilen değerler, küfür `None` filtrelemeyi devre dışı bırakmak, `Masked` küfür ile `Removed` tüm küfür kaldırmak veya `Tags` "küfür" etiketleri eklemek için. Varsayılan ayar `Masked` değeridir.
:::row-end:::
:::row:::
   :::column span="1":::
      `punctuationMode`
   :::column-end:::
   :::column span="2":::
      Tanıma sonuçlarında noktalama işaretlerinin nasıl işleneceğini belirtir. Kabul edilen değerler `None` , `Dictated` Açık (konuşulan) noktalama işareti, `Automatic` kod çözücüsünün noktalama ile uğraşmasına veya `DictatedAndAutomatic` Dikte ve otomatik noktalama kullanımına izin vermek için noktalama işaretlerini devre dışı bırakır. Varsayılan ayar `DictatedAndAutomatic` değeridir.
:::row-end:::
:::row:::
   :::column span="1":::
      `wordLevelTimestampsEnabled`
   :::column-end:::
   :::column span="2":::
      Sözcük düzeyi tarih damgalarının çıktıya eklenip eklenmesinin gerekip gerekmediğini belirtir. Kabul edilen değerler, `true` devre dışı bırakmak için sözcük düzeyi zaman damgalarını ve `false` (varsayılan değer) etkin hale gelir.
:::row-end:::
:::row:::
   :::column span="1":::
      `diarizationEnabled`
   :::column-end:::
   :::column span="2":::
      Giriş analizinin, iki ses içeren mono kanalı olması beklenen girişte gerçekleştirilmesi gerektiğini belirtir. Kabul edilen değerler `true` `false` devre dışı bırakılır ve devre dışı bırakmak için (varsayılan değer). Ayrıca, `wordLevelTimestampsEnabled` true olarak ayarlanması gerekir.
:::row-end:::
:::row:::
   :::column span="1":::
      `channels`
   :::column-end:::
   :::column span="2":::
      İşlemek için isteğe bağlı kanal numaraları dizisi. Burada, ses dosyasındaki kullanılabilir kanalların bir alt kümesinin işlenmek üzere belirtilenebilir (örneğin, `0` yalnızca). Belirtilmezse, kanallar `0` `1` Varsayılan olarak yeniden oluşturulur.
:::row-end:::
:::row:::
   :::column span="1":::
      `timeToLive`
   :::column-end:::
   :::column span="2":::
      Dökümü tamamladıktan sonra otomatik olarak döküm silme işlemi için isteğe bağlı süre. , `timeToLive` Sonunda silindiklerinden emin olmak için yığın işleme dökümlerinin yararlı olması gerekir (ör. `PT12H` ). Belirtilmemişse veya olarak ayarlandıysa, döküm `PT0H` otomatik olarak silinmez.
:::row-end:::
:::row:::
   :::column span="1":::
      `destinationContainerUrl`
   :::column-end:::
   :::column span="2":::
      Azure 'da yazılabilir bir kapsayıcıya [HIZMET SAS](../../storage/common/storage-sas-overview.md) ile Isteğe bağlı URL. Sonuç bu kapsayıcıda saklanır. Belirtilmediğinde Microsoft, sonuçları Microsoft tarafından yönetilen bir depolama kapsayıcısında depolar. Döküm, [silme](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/DeleteTranscription)işlemi çağırarak silindiğinde, sonuç verileri de silinir.
:::row-end:::

### <a name="storage"></a>Depolama

Toplu iş dökümü, ses okumak ve depoya yazmak için [Azure Blob depolamayı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) destekler.

## <a name="the-batch-transcription-result"></a>Toplu iş dökümü sonucu

Her giriş sesi için, bir döküm sonuç dosyası oluşturuluyor. [Get döküm dosyalarını](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetTranscriptionFiles)çağırarak sonuç dosyalarının listesini alabilirsiniz. Bu yöntem, bu döküm için sonuç dosyalarının bir listesini döndürür. Belirli bir giriş dosyası için döküm dosyasını bulmak için, ve ile döndürülen tüm dosyaları filtreleyin `kind`  ==  `Transcription` `name`  ==  `{originalInputName.suffix}.json` .

Her döküm sonuç dosyası şu biçimdedir:

```json
{
  "source": "...",                                                 // the sas url of a given contentUrl or the path relative to the root of a given container
  "timestamp": "2020-06-16T09:30:21Z",                             // creation time of the transcription, ISO 8601 encoded timestamp, combined date and time
  "durationInTicks": 41200000,                                     // total audio duration in ticks (1 tick is 100 nanoseconds)
  "duration": "PT4.12S",                                           // total audio duration, ISO 8601 encoded duration
  "combinedRecognizedPhrases": [                                   // concatenated results for simple access in single string for each channel
    {
      "channel": 0,                                                // channel number of the concatenated results
      "lexical": "hello world",
      "itn": "hello world",
      "maskedITN": "hello world",
      "display": "Hello world."
    }
  ],
  "recognizedPhrases": [                                           // results for each phrase and each channel individually
    {
      "recognitionStatus": "Success",                              // recognition state, e.g. "Success", "Failure"
      "channel": 0,                                                // channel number of the result
      "offset": "PT0.07S",                                         // offset in audio of this phrase, ISO 8601 encoded duration 
      "duration": "PT1.59S",                                       // audio duration of this phrase, ISO 8601 encoded duration
      "offsetInTicks": 700000.0,                                   // offset in audio of this phrase in ticks (1 tick is 100 nanoseconds)
      "durationInTicks": 15900000.0,                               // audio duration of this phrase in ticks (1 tick is 100 nanoseconds)
      
      // possible transcriptions of the current phrase with confidences
      "nBest": [
        {
          "confidence": 0.898652852,                               // confidence value for the recognition of the whole phrase
          "speaker": 1,                                            // if `diarizationEnabled` is `true`, this is the identified speaker (1 or 2), otherwise this property is not present
          "lexical": "hello world",
          "itn": "hello world",
          "maskedITN": "hello world",
          "display": "Hello world.",
          
          // if wordLevelTimestampsEnabled is `true`, there will be a result for each word of the phrase, otherwise this property is not present
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
      `lexical`
   :::column-end:::
   :::column span="2":::
      Tanınan gerçek sözcükler.
:::row-end:::
:::row:::
   :::column span="1":::
      `itn`
   :::column-end:::
   :::column span="2":::
      Tanınan metnin ters metin normalleştirilmiş biçimi. Kısaltmalar ("Doctor Smith"-"Dr Smith"), telefon numaraları ve diğer dönüşümler uygulanır.
:::row-end:::
:::row:::
   :::column span="1":::
      `maskedITN`
   :::column-end:::
   :::column span="2":::
      Küfür maskeleme uygulanmış ıTYPEFORM.
:::row-end:::
:::row:::
   :::column span="1":::
      `display`
   :::column-end:::
   :::column span="2":::
      Tanınan metnin görüntüleme formu. Eklenen noktalama işaretleri ve büyük harfler dahil edilir.
:::row-end:::

## <a name="speaker-separation-diarization"></a>Konuşmacı ayrımı (Diarleştirme)

Kararlama, hoparlörleri ses parçasıyla ayırma işlemidir. Batch Işlem hatmız, daha fazla seçim destekleiyor ve mono kanal kayıtlarında iki hoparlörleri tanıyor. Özellik, stereo kayıtlarında kullanılamaz.

Diarleştirme etkin olan döküm çıkışı, her bir çıkış `Speaker` tümceciği için bir giriş içerir. Eğer Eğer bu özellik kullanılmazsa, özelliği `Speaker` JSON çıktısında yok. İki sesi destekliyoruz, böylece hoparlörler veya olarak tanımlanır `1` `2` .

Daha fazla bilgi almak için, aşağıda gösterildiği gibi, HTTP isteğine ilgili parametresini eklemeniz yeterlidir.

 ```json
{
  "contentUrls": [
    "<URL to an audio file to transcribe>",
  ],
  "properties": {
    "diarizationEnabled": true,
    "wordLevelTimestampsEnabled": true,
    "punctuationMode": "DictatedAndAutomatic",
    "profanityFilterMode": "Masked"
  },
  "locale": "en-US",
  "displayName": "Transcription of file using default model for en-US"
}
```

Yukarıdaki istekteki parametreler gösterildiği için sözcük düzeyi zaman damgalarının etkinleştirilmesi gerekir.

## <a name="best-practices"></a>En iyi uygulamalar

Döküm hizmeti, çok sayıda gönderilen dökümü işleyebilir. Bir veya daha fazla gözden geçir aracılığıyla döküm durumunu sorgulayabilirsiniz `GET` . [Get transcriptions](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetTranscriptions) Sonuçları aldıktan sonra hizmetten düzenli olarak [silme](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/DeleteTranscription) çağrısını çağırın. Bunun yerine `timeToLive` , sonuçların son silinmesini sağlamak için özelliği makul bir değere ayarlayın.

## <a name="sample-code"></a>Örnek kod

Tüm örnekler, alt dizinin içindeki [GitHub örnek deposunda](https://aka.ms/csspeech/samples) bulunur `samples/batch` .

Lütfen örnek kodu Abonelik bilgileriniz, hizmet bölgesi, geçiş için ses dosyasına işaret eden SAS URI 'SI ve özel model kullanmak istiyorsanız model konumu ile güncelleştirin.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#transcriptiondefinition)]

Örnek kod, istemciyi ayarlar ve döküm isteğini gönderir. Ardından durum bilgilerini yoklar ve döküm ilerleme durumuyla ilgili ayrıntıları yazdırır.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#transcriptionstatus)]

Önceki çağrılar hakkında tam Ayrıntılar için [Swagger belgemizi](https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0)inceleyin. Burada gösterilen tam örnek için alt dizinde [GitHub](https://aka.ms/csspeech/samples) ' a gidin `samples/batch` .

Ses ve döküm durumunu alma hakkında zaman uyumsuz Kurulum ' a göz atın. Oluşturduğunuz istemci bir .NET HTTP istemcsahiptir. `PostTranscriptions`Ses dosyası ayrıntılarını ve durumları almaya yönelik bir yöntemi göndermek için bir yöntem vardır `GetTranscriptions` . `PostTranscriptions`bir tanıtıcı döndürür ve bunu, döküm `GetTranscriptions` durumunu almak için bir tanıtıcı oluşturmak üzere kullanır.

Geçerli örnek kod özel bir model belirtmiyor. Hizmet, dosya veya dosyaları çözümlemek için temel modeli kullanır. Modeli belirtmek için, özel model için model başvurusuyla aynı yönteme geçiş yapabilirsiniz.

> [!NOTE]
> Taban çizgisi dökümü için, taban çizgisi modelinin KIMLIĞINI bildirmeniz gerekmez.

## <a name="download-the-sample"></a>Örneği indirme

Örneği `samples/batch` [GitHub örnek deposundaki](https://aka.ms/csspeech/samples)dizinde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşmaya metin v3 API başvurusu](https://centralus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/CopyModelToSubscription)
