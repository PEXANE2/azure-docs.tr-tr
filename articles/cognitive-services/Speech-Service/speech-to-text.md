---
title: Konuşmadan metne - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne özellik, ses akışlarının metin içine gerçek zamanlı olarak transkripsiyonu yapılmasını sağlar. Uygulamalarınız, araçlarınız veya aygıtlarınız bu metin girişinde tüketebilir, görüntüleyebilir ve işlem yapabilir. Bu hizmet metinden konuşmaya (konuşma sentezi) ve konuşma çeviri özellikleriyle sorunsuz bir şekilde çalışır.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/12/2020
ms.author: trbye
ms.openlocfilehash: e0aea7e55381e9571b156701699f5f45315bb384
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399688"
---
# <a name="what-is-speech-to-text"></a>Konuşmayı metne dönüştürme nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Konuşma tanıma olarak da bilinen Konuşma hizmetinden metinden metne metin, ses akışlarının metin içine gerçek zamanlı olarak transkripsiyonu yapılmasını sağlar. Uygulamalarınız, araçlarınız veya aygıtlarınız bu metin üzerinde komut girişi olarak tüketebilir, görüntüleyebilir ve işlem yapabilir. Bu hizmet, Microsoft'un Cortana ve Office ürünleri için kullandığı tanıma teknolojisiyle desteklenmektedir. <a href="./speech-translation.md" target="_blank">Çeviri <span class="docon docon-navigate-external x-hidden-focus"></span> </a> ve <a href="./text-to-speech.md" target="_blank">metinden konuşmaya <span class="docon docon-navigate-external x-hidden-focus"></span> </a> hizmet teklifleri ile sorunsuz bir şekilde çalışır. Kullanılabilir konuşmadan metine dillerin tam listesi için [desteklenen dillere](language-support.md#speech-to-text)bakın.

Konuşmadan metne hizmet, Evrensel dil modelini kullanmaktan varsayılan olarak gelir. Bu model Microsoft'a ait veriler kullanılarak eğitildi ve bulutta dağıtıldı. Konuşma ve dikte senaryoları için en uygun uyracak. Benzersiz bir ortamda tanıma ve transkripsiyon için metinden metne sözcük kullanırken, özel akustik, dil ve telaffuz modelleri oluşturabilir ve eğitebilirsiniz. Özelleştirme, ortam gürültüsünü veya sektöre özgü kelime dağarcığını ele almak için yararlıdır.

> [!NOTE]
> Bing Konuşması 15 Ekim 2019 tarihinde kullanımdan kaldırıldı. Uygulamalarınız, araçlarınız veya ürünleriniz Bing Konuşma API'lerini kullanıyorsa, Konuşma hizmetine geçiş inizi yardımcı olacak kılavuzlar oluşturduk.
> - [Bing Konuşmasından Konuşma hizmetine geçiş](how-to-migrate-from-bing-speech.md)

## <a name="get-started-with-speech-to-text"></a>Konuşmadan metne kadar işe başlayın

Konuşma-metin hizmeti [Konuşma SDK](speech-sdk.md)üzerinden kullanılabilir. Çeşitli dillerde ve platformlarda hızlı başlangıçlar olarak kullanılabilen birkaç yaygın senaryo vardır:

 - [Quickstart: Mikrofon girişi yle konuşmayı tanıma](quickstarts/speech-to-text-from-microphone.md)
 - [Hızlı başlatma: Dosyadaki konuşmayı tanıma](quickstarts/speech-to-text-from-file.md)
 - [Quickstart: Blob depolamada depolanan konuşmayı tanıma](quickstarts/from-blob.md)

Konuşmadan metne REST hizmetini kullanmayı tercih ediyorsanız, [BKZ.](rest-speech-to-text.md)

## <a name="tutorials-and-sample-code"></a>Öğreticiler ve örnek kod

Konuşma hizmetini kullanma fırsatınız olduktan sonra, Konuşma SDK ve LUIS'i kullanarak konuşma nın amacını nasıl tanıyabileceğinizi öğreten öğreticimizi deneyin.

- [Öğretici: C kullanarak Konuşma SDK ve LUIS ile konuşma niyetleri tanıyın #](how-to-recognize-intents-from-speech-csharp.md)

Konuşma SDK için örnek kod GitHub mevcuttur. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek çekim tanıma ve özel modeller ile çalışma gibi yaygın senaryoları kapsar.

- [Konuşma-metin örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Toplu transkripsiyon örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>Özelleştirme

Standart Konuşma hizmeti modeline ek olarak, özel modeller oluşturabilirsiniz. Özelleştirme konuşma stili, kelime ve arka plan gürültüsü gibi konuşma tanıma engelleri aşmak için yardımcı olur, [Özel Konuşma](how-to-custom-speech.md)bakın. Özelleştirme seçenekleri dile/yerel ere göre değişir, desteği doğrulamak için [desteklenen dillere](supported-languages.md) bakın.

[!INCLUDE [speech-reference-doc-links](includes/speech-reference-doc-links.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)
- [Konuşma SDK alın](speech-sdk.md)
