---
title: Toplu Iş dökümü kullanma-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Toplu döküm, depolama alanında Azure Blob 'Ları gibi büyük miktarda ses eklemek istiyorsanız idealdir. Adanmış REST API kullanarak, paylaşılan erişim imzası (SAS) URI 'SI olan ses dosyalarını işaret edebilir ve zaman uyumsuz olarak alma yapabilirsiniz.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: panosper
ms.openlocfilehash: 158a99b1691e59fa58207f3c9291ca9d37a6679c
ms.sourcegitcommit: 36eb583994af0f25a04df29573ee44fbe13bd06e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74538125"
---
# <a name="why-use-batch-transcription"></a>Toplu Iş dökümü neden kullanılmalıdır?

Toplu döküm, depolama alanında Azure Blob 'Ları gibi büyük miktarda ses eklemek istiyorsanız idealdir. Adanmış REST API kullanarak, paylaşılan erişim imzası (SAS) URI 'SI olan ses dosyalarını işaret edebilir ve zaman uyumsuz olarak alma yapabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

### <a name="subscription-key"></a>Abonelik anahtarı

Konuşma hizmetinin tüm özelliklerinde olduğu gibi, [kullanmaya başlama kılavuzumuzu](get-started.md)izleyerek [Azure Portal](https://portal.azure.com) bir abonelik anahtarı oluşturursunuz. Temel modellerimizin dökümünü almayı planlıyorsanız, bir anahtar oluşturmak için yapmanız gereken tek şey vardır.

>[!NOTE]
> Toplu iş dökümünü kullanmak için, konuşma Hizmetleri için standart bir abonelik (S0) gereklidir. Ücretsiz abonelik anahtarları (F0) çalışmaz. Daha fazla bilgi için bkz. [fiyatlandırma ve sınırlar](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="custom-models"></a>Özel modeller

Akustik veya dil modellerini özelleştirmeyi planlıyorsanız, [akustik modelleri özelleştirme](how-to-customize-acoustic-models.md) ve [dil modellerini özelleştirme](how-to-customize-language-model.md)bölümündeki adımları izleyin. Toplu iş dökümlerinde oluşturulan modelleri kullanmak için model kimliklerine ihtiyacınız vardır. Bu KIMLIK, uç nokta Ayrıntıları görünümünde bulduğunuz uç nokta KIMLIĞI değil, modellerin ayrıntılarını seçerken alabileceğiniz model KIMLIĞIDIR.

## <a name="the-batch-transcription-api"></a>Toplu Iş dökümü API 'SI

Toplu Iş dökümü API 'SI, ek özelliklerle birlikte zaman uyumsuz konuşmayı metne dönüştürme Işlemi sunar. Bu, için yöntemler sunan bir REST API:

1. Toplu işleme istekleri oluşturma
1. Sorgu durumu
1. Döküm indiriliyor

> [!NOTE]
> Toplu Iş dökümü API 'SI, genellikle binlerce saatlik sesi biriktiren çağrı merkezleri için idealdir. Büyük hacimde ses kaydı yapmayı kolaylaştırır.

### <a name="supported-formats"></a>Desteklenen biçimler

Toplu Iş dökümü API 'SI aşağıdaki biçimleri destekler:

| Biçimlendir | Bileşeni | Bit hızı | Örnek hız |
|--------|-------|---------|-------------|
| WAV | PCM | 16 bit | 8 veya 16 kHz, mono, stereo |
| ÇA | PCM | 16 bit | 8 veya 16 kHz, mono, stereo |
| OGG | OPUS 'LAR | 16 bit | 8 veya 16 kHz, mono, stereo |

Stereo ses akışları için toplu Iş dökümü API 'SI, döküm sırasında sol ve sağ kanalı böler. Her biri tek bir kanaldan oluşturulan, sonuçtaki iki JSON dosyası oluşturulur. Söylenişi başına zaman damgaları, geliştiricinin sıralı bir son döküm oluşturmasını sağlar. Bu örnek istek, küfür filtrelemesinin, noktalama işaretlerinin ve sözcük düzeyi zaman damgalarının özelliklerini içerir.

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
    "ProfanityFilterMode": "Masked",
    "PunctuationMode": "DictatedAndAutomatic",
    "AddWordLevelTimestamps" : "True",
    "AddSentiment" : "True"
  }
}
```

> [!NOTE]
> Toplu Iş dökümü API 'SI, döküm istemek, durumları ve ilişkili sonuçları istemek için bir REST hizmeti kullanır. API 'yi herhangi bir dilde kullanabilirsiniz. Sonraki bölümde, API 'nin nasıl kullanıldığı açıklanmaktadır.

### <a name="configuration-properties"></a>Yapılandırma özellikleri

Dökümü yapılandırmak için bu isteğe bağlı özellikleri kullanın:

| Parametre | Açıklama |
|-----------|-------------|
| `ProfanityFilterMode` | Tanıma sonuçlarında küfür nasıl işleneceğini belirtir. Kabul edilen değerler `None`, küfür filtrelemeyi devre dışı bırakan, `masked`, sonuçtan tüm küfür kaldıran `removed` veya "küfür" etiketleri ekleyen `tags`. Varsayılan ayar `masked`. |
| `PunctuationMode` | Tanıma sonuçlarında noktalama işaretlerinin nasıl işleneceğini belirtir. Kabul edilen değerler, noktalama işaretini devre dışı bırakan `None`, `dictated` açık noktalama işareti olan `automatic`, bu da kod çözücüsünün noktalama işaretleriyle veya dikte edilen noktalama işaretlerini veya otomatik olarak veya otomatik olarak kullanıldığı `dictatedandautomatic` |
 | `AddWordLevelTimestamps` | Sözcük düzeyi tarih damgalarının çıktıya eklenip eklenmesinin gerekip gerekmediğini belirtir. Kabul edilen değerler, Word düzeyi zaman damgalarını ve `false` (varsayılan değer) devre dışı bırakmak için `true`. |
 | `AddSentiment` | Yaklaşım 'ın utterance 'e eklenmesi gerektiğini belirtir. Kabul edilen değerler, her ay için yaklaşım ve `false` (varsayılan değer) tarafından devre dışı bırakılacak `true`. |
 | `AddDiarization` | İki ses içeren mono kanalı olması beklenen girişte, seçme analizinin gerçekleştirilmesi gerektiğini belirtir. Kabul edilen değerler, devre dışı bırakmak için `true` ve `false` (varsayılan değer) sağlar. Ayrıca, `AddWordLevelTimestamps` true olarak ayarlanmasını gerektirir.|

### <a name="storage"></a>Depolama

Toplu iş dökümü, ses okumak ve depoya yazmak için [Azure Blob depolamayı](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-overview) destekler.

## <a name="speaker-separation-diarization"></a>Konuşmacı ayrımı (Diarleştirme)

Kararlama, hoparlörleri ses parçasıyla ayırma işlemidir. Batch Işlem hatmız, daha fazla seçim destekleiyor ve mono kanal kayıtlarında iki hoparlörleri tanıyor.

Ses dökümü isteğiniz için işlenmek üzere işlenmesini istemek için, yalnızca aşağıda gösterildiği gibi, HTTP isteğine ilgili parametresini eklemeniz gerekir.

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

Yukarıdaki istekteki parametreler gösterildiği gibi, sözcük düzeyi tarih damgalarının de ' açık ' olması gerekir. 

Karşılık gelen ses, bir sayıyla tanımlanan hoparlörleri içerir (Şu anda yalnızca iki ses destekliyoruz, bu nedenle konuşmacı, ' hoparlör 1 ' ve ' hoparlör 2 ' olarak tanımlanır) ve ardından döküm çıkışı gelir.

Ayrıca, stereo kayıtlarında bu özelliği kullanılamaz durumda olduğunu unutmayın. Ayrıca, tüm JSON çıktılarına konuşmacı etiketi de bulunur. Eğer kullanım kullanılmıyorsa, JSON çıktısında ' Konuşmacı: null ' gösterilir.

> [!NOTE]
> Tüm bölgelerde ve tüm yerel ayarlarda kullanılabilir.

## <a name="sentiment"></a>Duygu

Yaklaşım, toplu Iş dökümü API 'sindeki yeni bir özelliktir ve çağrı merkezi etki alanındaki önemli bir özelliktir. Müşteriler, istekleri için `AddSentiment` parametrelerini kullanabilir

1.  Müşteri memnuniyeti hakkında Öngörüler edinin
2.  Aracıların performansı hakkında öngörüler elde edin (çağrıları alan takım)
3.  Bir çağrının negatif yönde bir değer aldığı zaman içinde tam zaman noktası
4.  Pozitif yapılan olumsuz çağrıları etkinleştirirken, Pinpoint ne kadar iyi bir sorun oluştu
5.  Beğendikleri müşterileri ve bir ürün veya hizmet hakkında neleri beğendiklerinizi tanımlayın

Yaklaşım, bir ses kesiminin, utterlik (fark) başlangıcı ve bayt akışı için algılama sessizliği arasında geçen süre içinde tanımlandığı bir ses kesimi başına puanlanır. Bu kesimdeki tüm metin, yaklaşımı hesaplamak için kullanılır. Tüm çağrı veya her bir kanalın tüm konuşma için herhangi bir toplama yaklaşımı değeri hesaplamaz. Bu toplamalar daha sonra uygulamak için etki alanı sahibine bırakılır.

Yaklaşım, sözlü biçimde uygulanır.

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
Özelliği, şu anda beta sürümünde olan bir yaklaşım modeli kullanır.

## <a name="sample-code"></a>Örnek kod

Tüm örnekler, `samples/batch` alt dizininin içindeki [GitHub örnek deposunda](https://aka.ms/csspeech/samples) bulunur.

Örnek kodu Abonelik bilgileriniz, hizmet bölgesi, ses dosyasına işaret edilecek SAS URI 'SI ve özel bir akustik veya dil modeli kullanmak istiyorsanız model kimlikleri ile özelleştirmeniz gerekir.

[!code-csharp[Configuration variables for batch transcription](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchdefinition)]

Örnek kod, istemciyi ayarlar ve döküm isteğini gönderir. Daha sonra durum bilgilerini yoklayacak ve döküm ilerlemesiyle ilgili ayrıntıları yazdıracaktır.

[!code-csharp[Code to check batch transcription status](~/samples-cognitive-services-speech-sdk/samples/batch/csharp/program.cs#batchstatus)]

Önceki çağrılar hakkında tam Ayrıntılar için [Swagger belgemizi](https://westus.cris.ai/swagger/ui/index)inceleyin. Burada gösterilen tam örnek için `samples/batch` alt dizininde [GitHub](https://aka.ms/csspeech/samples) ' a gidin.

Ses ve döküm durumunu alma hakkında zaman uyumsuz Kurulum ' a göz atın. Oluşturduğunuz istemci bir .NET HTTP istemcsahiptir. Ses dosyası ayrıntılarını göndermek için bir `PostTranscriptions` yöntemi ve sonuçları almak için bir `GetTranscriptions` yöntemi vardır. `PostTranscriptions` bir tanıtıcı döndürür ve `GetTranscriptions`, döküm durumunu almak üzere bir tanıtıcı oluşturmak için onu kullanır.

Geçerli örnek kod özel bir model belirtmiyor. Hizmet, dosya veya dosyaları derleyen taban çizgisi modellerini kullanır. Modelleri belirtmek için, akustik ve dil modeli için model kimlikleriyle aynı yöntemi geçirebilirsiniz.

> [!NOTE]
> Ana hat düzenlemeleri için, taban çizgisi modellerinin KIMLIĞINI bildirmeniz gerekmez. Yalnızca bir dil modeli KIMLIĞI belirtirseniz (ve akustik model KIMLIĞI yoksa), eşleşen bir akustik model otomatik olarak seçilir. Yalnızca bir akustik model KIMLIĞI belirtirseniz, eşleşen bir dil modeli otomatik olarak seçilir.

## <a name="download-the-sample"></a>Örneği indirme

Örneği, [GitHub örnek deposundaki](https://aka.ms/csspeech/samples)`samples/batch` dizininde bulabilirsiniz.

> [!NOTE]
> Toplu iş dökümü işleri en iyi çaba temelinde zamanlanır, bir işin çalışma durumuna ne zaman değişene olacağı için bir zaman tahmini yoktur. Çalışır duruma getirildikten sonra gerçek döküm, ses gerçek saatinden daha hızlı işlenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
