---
title: Konuşmaya metne genel bakış-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmadan metne yazılım, ses akışlarının metin halinde gerçek zamanlı olarak dökümünü sağlar. Uygulamalarınız, araçlarınız veya cihazlarınız bu metin girişini kullanabilir, görüntüleyebilir ve eylem gerçekleştirebilir. Bu makale, konuşmadan metne bir hizmetin avantajları ve özelliklerine genel bir bakış sunar.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/01/2020
ms.author: trbye
ms.custom: cog-serv-seo-aug-2020
keywords: konuşmayı metne dönüştürme, konuşmayı metne dönüştürme
ms.openlocfilehash: 804acd17f3df91cc3df949d6d9ccac0450bb225f
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569229"
---
# <a name="what-is-speech-to-text"></a>Konuşmayı metne dönüştürme nedir?

Bu genel bakışta, konuşmadan metne olan hizmetin avantajları ve özellikleri hakkında bilgi edinebilirsiniz.
Konuşma tanıma olarak da bilinen konuşmayı metne dönüştürme, ses akışlarının gerçek zamanlı olarak dökümünü metne sağlar. Uygulamalarınız, araçlarınız veya cihazlarınız bu metni komut girişi olarak kullanabilir, görüntüleyebilir ve eylem gerçekleştirebilir. Bu hizmet, Microsoft 'un Cortana ve Office ürünleri için kullandığı aynı tanıma teknolojisi ile desteklenmektedir. <a href="./speech-translation.md" target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> Çeviri</a> ve <a href="./text-to-speech.md" target="_blank">metin okuma <span class="docon docon-navigate-external x-hidden-focus"></span> </a> hizmeti teklifleriyle sorunsuz bir şekilde çalışır. Kullanılabilir konuşmadan metne yönelik dillerin tam listesi için bkz. [desteklenen diller](language-support.md#speech-to-text).

Konuşmadan metne hizmet varsayılan olarak evrensel dil modelini kullanmaktır. Bu model, Microsoft 'a ait veriler kullanılarak eğitildi ve buluta dağıtıldı. Konuşma ve dikte senaryoları için idealdir. Benzersiz bir ortamda tanıma ve döküm için konuşmayı metne dönüştürme özelliğini kullanırken özel akustik, dil ve telaffuz modeller oluşturup eğitebilirsiniz. Özelleştirme, çevresel gürültü veya sektöre özgü sözlük adreslemeye yardımcı olur.

Giriş olarak ek başvuru metniyle birlikte, konuşmadan metne hizmet, konuşma söylenişini değerlendirmek ve konuşmaların doğruluğu ve akıcı olması ile konuşmacıya geri bildirimde bulunmak için [de sunar.](rest-speech-to-text.md#pronunciation-assessment-parameters) Telaffuz değerlendirmesi sayesinde, konuşma, anında geri bildirim alabilir ve telaffuz hale gelecek ve güvenle sunabilmeleri için söylenişlerini geliştirebilirler. Eğitimciler, birden çok konuşmacıdaki söylenişi gerçek zamanlı olarak değerlendirmek için özelliğini kullanabilir. Özelliği şu anda ABD Ingilizcesi 'ni destekliyor ve uzmanlar tarafından gerçekleştirilen konuşma değerlendirmelerinden yüksek oranda.

> [!NOTE]
> Bing Konuşma, 15 Ekim 2019 ' de kullanımdan çıkarıldı. Uygulamalarınız, araçlarınız veya ürünleriniz Bing Konuşma API 'Leri kullanıyorsa, konuşma hizmetine geçiş yapmanıza yardımcı olacak kılavuzlar oluşturduk.
> - [Bing Konuşma konuşma hizmetine geçirme](how-to-migrate-from-bing-speech.md)

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

## <a name="get-started"></a>başlarken

Konuşmayı metne kullanmaya başlamak için [hızlı](get-started-speech-to-text.md) başlangıca bakın. Hizmet, [konuşma SDK 'sı](speech-sdk.md), [REST API](rest-speech-to-text.md#pronunciation-assessment-parameters)ve [konuşma CLI](spx-overview.md)aracılığıyla kullanılabilir.

## <a name="sample-code"></a>Örnek kod

Konuşma SDK 'Sı için örnek kod GitHub ' da kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek kararlı bir tanıma ve özel modellerle çalışma gibi yaygın senaryoları kapsar.

- [Konuşmaya metin örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Toplu iş dökümü örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Söyleniş değerlendirmesi örnekleri (REST)](rest-speech-to-text.md#pronunciation-assessment-parameters)

## <a name="customization"></a>Özelleştirme

Standart konuşma hizmeti modelinin yanı sıra özel modeller de oluşturabilirsiniz. Özelleştirme, konuşma stili, sözlük ve arka plan gürültüsü gibi konuşma tanıma engellerinin üstesinden gelmenize yardımcı olur. [özel konuşma tanıma](how-to-custom-speech.md). Özelleştirme seçenekleri dile/yerel ayara göre farklılık gösterir. desteği doğrulamak için [desteklenen diller](supported-languages.md) bölümüne bakın.

## <a name="batch-transcription"></a>Toplu iş transkripsiyonu

Toplu iş dökümü, depolamada büyük miktarda ses aktarmanıza olanak sağlayan bir REST API işlemleri kümesidir. Paylaşılan erişim imzası (SAS) URI'si kullanarak ses dosyalarınızı işaret edebilir ve transkripsiyon sonuçlarını zaman uyumsuz bir şekilde alabilirsiniz. Toplu iş dökümü API 'sini kullanma hakkında daha fazla bilgi için bkz. [nasıl yapılır](batch-transcription.md) .

[!INCLUDE [speech-reference-doc-links](includes/speech-reference-doc-links.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Ücretsiz bir konuşma hizmeti abonelik anahtarı alın](get-started.md)
- [Konuşma SDK 'sını alın](speech-sdk.md)
