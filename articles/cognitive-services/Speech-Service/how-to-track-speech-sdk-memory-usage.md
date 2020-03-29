---
title: Konuşma SDK bellek kullanımı nasıl izler - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma Hizmeti SDK, konuşma-metin ve metin-konuşma dönüştürme için çok sayıda programlama dilini destekler, konuşma çevirisi ile birlikte. Bu makalede, SDK yerleşik bellek yönetimi aracı anlatılmaktadır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: rhurey
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: da5103317a2215aca68cec14ba8a0951258c9b89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75456437"
---
# <a name="how-to-track-speech-sdk-memory-usage"></a>Konuşma SDK bellek kullanımı nasıl izler

Speech SDK, bir dizi birlikte çalışabilirlik katmanı aracılığıyla birden çok programlama diline yansıtılan yerel bir kod tabanını temel almaktadır. Dile özgü her projeksiyon, nesne yaşam döngüsünü yönetmek için deyimolarak doğru özelliklere sahiptir. Ayrıca, Konuşma SDK nesne günlüğü ve nesne sınırları ile kaynak kullanımını izlemek için bellek yönetimi aracı içerir. 

## <a name="how-to-read-object-logs"></a>Nesne günlükleri nasıl okunur

[Konuşma SDK günlüğü etkinse,](how-to-use-logging.md)geçmiş nesne gözlemini etkinleştirmek için izleme etiketleri yayılır. Bu etiketler şunlardır: 

* `TrackHandle` veya `StopTracking` 
* Nesne türü
* Nesnenin türünü izlenen nesnelerin geçerli sayısı ve izlenen geçerli sayı.

İşte örnek bir günlük: 

```terminal
(284): 8604ms SPX_DBG_TRACE_VERBOSE:  handle_table.h:90 TrackHandle type=Microsoft::CognitiveServices::Speech::Impl::ISpxRecognitionResult handle=0x0x7f688401e1a0, ptr=0x0x7f688401e1a0, total=19
```

## <a name="set-a-warning-threshold"></a>Uyarı eşiği ayarlama

Bir uyarı eşiği oluşturma seçeneğiniz vardır ve bu eşik aşılırsa (günlüğe kaydetmenin etkin olduğunu varsayarsak), bir uyarı iletisi günlüğe kaydedilir. Uyarı iletisi, varolan tüm nesnelerin sayısıyla birlikte bir dökümeiçerir. Bu bilgiler sorunları daha iyi anlamak için kullanılabilir. 

Bir uyarı eşiğini etkinleştirmek için, `SpeechConfig` bir nesne üzerinde belirtilmelidir. Yeni bir tanıyıcı oluşturulduğunda bu nesne denetlenir. Aşağıdaki örneklerde, `SpeechConfig` aşağıdakiler adlı `config`bir örnek oluşturduğunuzu varsayalım:

::: zone pivot="programming-language-csharp"

```csharp
config.SetProperty("SPEECH-ObjectCountWarnThreshold", "10000");
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
config->SetProperty("SPEECH-ObjectCountWarnThreshold", "10000");
```

::: zone-end

::: zone pivot="programming-language-java"

```java
config.setProperty("SPEECH-ObjectCountWarnThreshold", "10000");
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
speech_config.set_property_by_name(“SPEECH-ObjectCountWarnThreshold", "10000")?
```

::: zone-end

::: zone pivot="programming-language-more"

```ObjectiveC
[config setPropertyTo:@"10000" byName:"SPEECH-ObjectCountWarnThreshold"];
```

::: zone-end

> [!TIP]
> Bu özellik için varsayılan değer 10.000'dir.

## <a name="set-an-error-threshold"></a>Hata eşiği ayarlama 

Konuşma SDK'sını kullanarak, belirli bir zamanda izin verilen en fazla nesne sayısını ayarlayabilirsiniz. Bu ayar etkinleştirilirse, en büyük sayı vurulduğunda, yeni tanıyan nesneler oluşturma girişimleri başarısız olur. Varolan nesneler çalışmaya devam eder.

Aşağıda bir örnek hatası ve bir hata ver:

```terminal
Runtime error: The maximum object count of 500 has been exceeded.
The threshold can be adjusted by setting the SPEECH-ObjectCountErrorThreshold property on the SpeechConfig object.
See http://https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-object-tracking-speech-sdk for more detailed information.
Handle table dump by ojbect type:
class Microsoft::CognitiveServices::Speech::Impl::ISpxRecognitionResult 0
class Microsoft::CognitiveServices::Speech::Impl::ISpxRecognizer 0
class Microsoft::CognitiveServices::Speech::Impl::ISpxAudioConfig 0
class Microsoft::CognitiveServices::Speech::Impl::ISpxSpeechConfig 0
```

Bir hata eşiğini etkinleştirmek için, `SpeechConfig` bir nesne üzerinde belirtilmelidir. Yeni bir tanıyıcı oluşturulduğunda bu nesne denetlenir. Aşağıdaki örneklerde, `SpeechConfig` aşağıdakiler adlı `config`bir örnek oluşturduğunuzu varsayalım:

::: zone pivot="programming-language-csharp"

```csharp
config.SetProperty("SPEECH-ObjectCountErrorThreshold", "10000");
```

::: zone-end

::: zone pivot="programming-language-cpp"

```C++
config->SetProperty("SPEECH-ObjectCountErrorThreshold", "10000");
```

::: zone-end

::: zone pivot="programming-language-java"

```java
config.setProperty("SPEECH-ObjectCountErrorThreshold", "10000");
```

::: zone-end

::: zone pivot="programming-language-python"

```Python
speech_config.set_property_by_name(“SPEECH-ObjectCountErrorThreshold", "10000")?
```

::: zone-end

::: zone pivot="programming-language-more"

```objc
[config setPropertyTo:@"10000" byName:"SPEECH-ObjectCountErrorThreshold"];
```

::: zone-end

> [!TIP]
> Bu özellik için varsayılan değer, bir `size_t` veri türü için platforma özgü maksimum değerdir. Tipik bir tanıma 7 ve 10 iç nesneleri arasında tüketir.

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma hizmeti deneme aboneliğinizi alın](get-started.md)
* [Mikrofon kullanarak konuşmayı nasıl tanıyacağınızı öğrenin](quickstarts/speech-to-text-from-microphone.md)