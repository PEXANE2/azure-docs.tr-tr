---
title: Konuşmayı metne dönüştürme-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne özelliği, ses akışlarının metin halinde gerçek zamanlı olarak dökümünü sağlar. Uygulamalarınız, araçlarınız veya cihazlarınız bu metin girişini kullanabilir, görüntüleyebilir ve eylem gerçekleştirebilir. Bu hizmet, metinden konuşmaya (konuşma birleştirme) ve konuşma çevirisi özellikleriyle sorunsuz bir şekilde çalışır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: erhopf
ms.openlocfilehash: 8518f92a4f4df1686d4b338783a93d969e04d219
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78388306"
---
# <a name="what-is-speech-to-text"></a>Konuşmayı metne dönüştürme nedir?

Konuşma tanıma olarak da bilinen konuşma hizmetindeki konuşma metni, ses akışlarının gerçek zamanlı olarak dökümünü metne sağlar. Uygulamalarınız, araçlarınız veya cihazlarınız bu metni komut girişi olarak kullanabilir, görüntüleyebilir ve eylem gerçekleştirebilir. Bu hizmet, Microsoft 'un Cortana ve Office ürünleri için kullandığı aynı tanıma teknolojisi ile desteklenmektedir. <a href="./speech-translation.md" target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> Çeviri</a> ve <a href="./text-to-speech.md" target="_blank">metin okuma <span class="docon docon-navigate-external x-hidden-focus"></span> </a> hizmeti teklifleriyle sorunsuz bir şekilde çalışır. Kullanılabilir konuşmadan metne yönelik dillerin tam listesi için bkz. [desteklenen diller](language-support.md#speech-to-text).

Konuşmadan metne hizmet varsayılan olarak evrensel dil modelini kullanmaktır. Bu model, Microsoft 'a ait veriler kullanılarak eğitildi ve buluta dağıtıldı. Konuşma ve dikte senaryoları için idealdir. Benzersiz bir ortamda tanıma ve döküm için konuşmayı metne dönüştürme özelliğini kullanırken özel akustik, dil ve telaffuz modeller oluşturup eğitebilirsiniz. Özelleştirme, çevresel gürültü veya sektöre özgü sözlük adreslemeye yardımcı olur.

> [!NOTE]
> Bing Konuşma, 15 Ekim 2019 ' de kullanımdan çıkarıldı. Uygulamalarınız, araçlar veya ürünleriniz Bing Konuşma API 'Leri veya Özel Konuşma Tanıma kullanıyorsa, konuşma hizmetine geçiş yapmanıza yardımcı olacak kılavuzlar oluşturduk.
> - [Bing Konuşma konuşma hizmetine geçirme](how-to-migrate-from-bing-speech.md)
> - [Özel Konuşma Tanıma konuşma hizmetine geçirme](how-to-migrate-from-custom-speech-service.md)

## <a name="get-started-with-speech-to-text"></a>Konuşmaya metne Başlarken

Konuşmaya metin hizmetine [konuşma SDK 'sı](speech-sdk.md)aracılığıyla ulaşılabilir. Hızlı başlangıçların çeşitli dillerde ve platformlarda kullanıma sunulduğu birkaç yaygın senaryo vardır:

 - [Hızlı başlangıç: mikrofon girişi ile konuşmayı tanıma](quickstarts/speech-to-text-from-microphone.md)
 - [Hızlı başlangıç: bir dosyadan konuşmayı tanıma](quickstarts/speech-to-text-from-file.md)
 - [Hızlı başlangıç: blob depolamada depolanan konuşmayı tanıma](quickstarts/from-blob.md)

Konuşmayı metne REST hizmetini kullanmayı tercih ediyorsanız bkz. [REST API 'leri](rest-speech-to-text.md).

## <a name="tutorials-and-sample-code"></a>Öğreticiler ve örnek kod

Konuşma hizmetini kullanma şansınız olduktan sonra, konuşma SDK 'sını ve LUO kullanarak konuşmayı nasıl anlayacağınızı öğreten Öğreticimizi deneyin.

- [Öğretici: konuşma SDK 'Sı ve LUVE ile konuşmayla ilgili amaçları kullanarak konuşmayı tanımaC#](how-to-recognize-intents-from-speech-csharp.md)

Konuşma SDK 'Sı için örnek kod GitHub ' da kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek kararlı bir tanıma ve özel modellerle çalışma gibi yaygın senaryoları kapsar.

- [Konuşmaya metin örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Toplu iş dökümü örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>Özelleştirme

Standart konuşma hizmeti modelinin yanı sıra özel modeller de oluşturabilirsiniz. Özelleştirme, konuşma stili, sözlük ve arka plan gürültüsü gibi konuşma tanıma engellerinin üstesinden gelmenize yardımcı olur. [özel konuşma tanıma](how-to-custom-speech.md). Özelleştirme seçenekleri dile/yerel ayara göre farklılık gösterir. desteği doğrulamak için [desteklenen diller](supported-languages.md) bölümüne bakın.

[!INCLUDE [speech-reference-doc-links](includes/speech-reference-doc-links.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
- [Konuşma SDK 'sını alın](speech-sdk.md)
