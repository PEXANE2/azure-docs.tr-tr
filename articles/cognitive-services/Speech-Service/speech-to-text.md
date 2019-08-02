---
title: Konuşmayı metne dönüştürme-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti 'nde konuşma-metin özelliği olarak da bilinen konuşmadan metne özelliği, ses akışlarının gerçek zamanlı olarak kullanımını, uygulamalar, araçlar ya da cihazlarınızın komut girişi olarak tüketebileceği, görüntüleyeceği ve üzerinde işlem gerçekleştirebildiği metne olanak sağlar. Bu hizmet, Microsoft 'un Cortana ve Office ürünleri için kullandığı aynı tanıma teknolojisi tarafından desteklenir ve çeviri ve metinden konuşmaya sorunsuz bir şekilde çalışır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 435de6e6516256d350ad93b121dd5a38d1512ddc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68558845"
---
# <a name="what-is-speech-to-text"></a>Konuşmayı metne dönüştürme nedir?

Azure konuşma hizmetlerinden konuşmaya metin olarak da bilinen konuşmadan metne dönüştürme, ses akışlarının gerçek zamanlı olarak kullanımını, uygulamalar, araçlar ya da cihazlarınızın komut girişi olarak tüketebileceği, görüntülemesi ve eylem yapması için kullanabileceği bir metne olanak sağlar. Bu hizmet, Microsoft 'un Cortana ve Office ürünleri için kullandığı aynı tanıma teknolojisi tarafından desteklenir ve çeviri ve metinden konuşmaya sorunsuz bir şekilde çalışır.  Kullanılabilir konuşmadan metne yönelik dillerin tam listesi için bkz. [desteklenen diller](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#speech-to-text).

Varsayılan olarak, konuşma-metin hizmeti, evrensel dil modelini kullanır. Bu model, Microsoft 'a ait veriler kullanılarak eğitildi ve buluta dağıtıldı. Konuşma ve dikte senaryoları için idealdir. Benzersiz bir ortamda tanıma ve döküm için konuşmaya metin kullanıyorsanız, çevresel gürültü veya sektöre özgü sözlük için özel akustik, dil ve telaffuz modeller oluşturup eğitebilirsiniz.

Konuşmayı bir mikrofondan kolayca yakalayabilir, bir akıştan okuyabilir veya ses dosyalarına konuşma SDK 'Sı ve REST API 'Leri ile erişebilirsiniz. Konuşma SDK 'Sı, konuşma tanıma için WAV/PCM 16 bit, 16 kHz/8 kHz, tek kanallı ses ' i destekler. Ek ses biçimleri, [konuşmadan metne REST uç noktası](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) veya [toplu iş dökümü hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats)kullanılarak desteklenir.

## <a name="core-features"></a>Temel Özellikler

Konuşma SDK 'Sı ve REST API 'Leri aracılığıyla kullanılabilen özellikler şunlardır:

| Kullanım örneği | SDK | REST |
|----------|-----|------|
| Kısa mesafeli (15 saniye <). Yalnızca son döküm sonucunu destekler. | Evet | Evet |
| Uzun ifade ve akış sesinin sürekli olarak dökümünü alma (> 15 saniye). , Geçici ve son döküm sonuçlarını destekler. | Evet | Hayır |
| Lusıs ile tanıma sonuçlarından hedefleri [](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis)türetebilirsiniz. | Evet | Yok\* |
| Ses dosyalarının zaman uyumsuz olarak toplu olarak dökümünü alma. | Hayır | Yes\** |
| Konuşma modellerini oluşturun ve yönetin. | Hayır | Yes\** |
| Özel model dağıtımları oluşturun ve yönetin. | Hayır | Yes\** |
| Özel modellere karşı taban çizgisi modelinin doğruluğunu ölçmek için doğruluk testleri oluşturun. | Hayır | Yes\** |
| Abonelikleri yönetin. | Hayır | Yes\** |

\* *LUIS amaç ve varlıkları kullanarak ayrı bir LUIS aboneliği türetilebilir. Bu abonelikle, SDK BASıS 'yi çağırabilir ve varlık ve amaç sonuçları sağlayabilir. REST API ile LUIS çağırabilirsiniz kendiniz amaç ve varlıkları LUIS aboneliğinizle türetmek için.*

\** *Bu hizmetler cris.ai uç noktası kullanılarak kullanılabilir. Bkz. [Swagger başvurusu](https://westus.cris.ai/swagger/ui/index).*

## <a name="get-started-with-speech-to-text"></a>Konuşmaya metne Başlarken

Her biri 10 dakikadan kısa bir süre içinde kod çalıştırmak için tasarlanan en popüler programlama dillerinde hızlı başlangıçları sunuyoruz. [Bu tablo](https://aka.ms/csspeech#5-minute-quickstarts) , bir konuşma SDK 'sı hızlı başlangıç listesini, platfrom ve Language tarafından düzenlenmiş bir şekilde içerir.  Ayrıca, API başvurusu [burada](https://aka.ms/csspeech#reference)bulunabilir.

Konuşmayı metne REST hizmetini kullanmayı tercih ediyorsanız bkz. [REST API 'leri](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

## <a name="tutorials-and-sample-code"></a>Öğreticiler ve örnek kod

Konuşma hizmetlerini kullanma şansınız olduktan sonra, konuşma SDK 'sını ve LUO kullanarak konuşmayı nasıl anlayacağınızı öğreten Öğreticimizi deneyin.

* [Öğretici: Konuşma SDK 'Sı ve LUSıS ile konuşmadan amaçları tanımaC#](how-to-recognize-intents-from-speech-csharp.md)

Konuşma SDK 'Sı için örnek kod GitHub ' da kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek kararlı bir tanıma ve özel modellerle çalışma gibi yaygın senaryoları kapsar.

* [Konuşmaya metin örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Toplu iş dökümü örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>Özelleştirme

Konuşma Hizmetleri tarafından kullanılan evrensel modele ek olarak, deneyiminiz için özel akustik, dil ve telaffuz modeller de oluşturabilirsiniz. Özelleştirme seçeneklerinin bir listesi aşağıda verilmiştir:

| Model | Açıklama |
|-------|-------------|
| [Akustik model](how-to-customize-acoustic-models.md) | Uygulamanızın, araçlarınızın veya cihazlarınızın belirli bir ortamda belirli bir kayıt koşullarına sahip bir araç veya fabrika gibi kullanılması durumunda özel bir akustik model oluşturma işlemi yararlı olur. Örnek olarak aksanlı konuşma, belirli arka plan görüntüleri veya kayıt için belirli bir mikrofonun kullanılması verilebilir. |
| [Dil modeli](how-to-customize-language-model.md) | Sektöre özgü sözlük ve dilbilgisinde tıp terminolojisi ya da It jarggibi bir tür dökümü geliştirmek için özel bir dil modeli oluşturun. |
| [Söyleniş modeli](how-to-customize-pronunciation.md) | Özel bir telaffuz modeliyle, bir sözcük veya terimin fonetik biçimini tanımlayabilir ve görüntüleyebilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş koşullarını işlemek için kullanışlıdır. Başlamak için ihtiyacınız olan telaffuz dosya--basit .txt dosyası. |

> [!NOTE]
> Özelleştirme seçenekleri dile/yerel ayara göre değişir (bkz. [desteklenen diller](supported-languages.md)).

## <a name="migration-guides"></a>Geçiş Kılavuzu

> [!WARNING]
> Bing Konuşma, 15 Ekim 2019 ' de kullanımdan kaldırılacaktır.

Uygulamalarınız, araçlar veya ürünleriniz Bing Konuşma API 'Leri veya Özel Konuşma Tanıma kullanıyorsa, konuşma hizmetlerine geçiş yapmanıza yardımcı olacak kılavuzlar oluşturduk.

* [Bing Konuşma konuşma hizmetlerine geçirme](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-bing-speech)
* [Özel Konuşma Tanıma konuşma hizmetlerine geçirme](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-custom-speech-service)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](https://aka.ms/csspeech)
* [Konuşma cihazları SDK 'Sı](speech-devices-sdk.md)
* [REST API: Konuşmayı metne dönüştürme](rest-speech-to-text.md)
* [REST API: Metin okuma](rest-text-to-speech.md)
* [REST API: Toplu iş dökümü ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

* [Ücretsiz bir konuşma Hizmetleri abonelik anahtarı alın](get-started.md)
* [Konuşma SDK 'sını alın](speech-sdk.md)
