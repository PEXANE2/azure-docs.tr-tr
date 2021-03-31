---
title: Karakter sayıları-çevirici
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure bilişsel hizmetler çeviricisinin, içeriğin nasıl alındığını anlayabilmeniz için karakterleri nasıl saydığı açıklanmaktadır.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 05/26/2020
ms.author: lajanuar
ms.openlocfilehash: 6e81736e3151c9e97a8926b1f67c0a7a0d4c2f3d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "98895892"
---
# <a name="how-the-translator-counts-characters"></a>Translator 'ın karakterleri sayısı

Çevirici her Unicode kod noktasını bir karakter olarak sayar. Bir metnin bir dile her çevirisi, isteğin tek bir API çağrısında yapılmış olsa bile birden çok dile çevrilirken ayrı bir çeviri olarak sayılır. Yanıtın uzunluğu önemi değildir.

Hangi sayımlar şunlardır:

* İsteğin gövdesinde çevirmene geçirilen metin
   * `Text` Çeviri, alfabe ve sözlük arama yöntemlerini kullanırken
   * `Text``Translation`Sözlük örnekleri yöntemi kullanılırken
* Tüm biçimlendirme: HTML, XML etiketleri, vb. istek gövdesinin metin alanı içinde. İsteği oluşturmak için kullanılan JSON gösterimi (örneğin, "metin:") sayılmaz.
* Tek bir harf
* Noktalama işaretleri
* Boşluk, sekme, biçimlendirme ve her türlü boşluk karakteri
* Unicode 'da tanımlanan her kod noktası
* Daha önce aynı metni çevirseniz bile yinelenen çeviri

Çince ve Japonca Kanji gibi ideograms tabanlı betikler için, çevirmen hizmeti, İdeogram başına bir karakter olan Unicode kod noktalarının sayısını yine de sayar. Özel durum: Unicode yedeklerin kapıları iki karakter olarak sayılır.

İstek, sözcük, bayt veya cümle sayısı karakter sayısında ilgisiz değildir.

Algılama ve Breakcümlesi yöntemlerine yapılan çağrılar, karakter tüketimine göre sayılmaz. Ancak, Algıla ve Breakcümlesi yöntemlerine yapılan çağrıların, sayılan diğer işlevlerin kullanımına yönelik makul bir ORANTA olmasını umuz. Yaptığınız algılama veya Breakcümle çağrılarının sayısı 100 kez diğer sayılan yöntemlerin sayısını aşarsa, Microsoft, Algıla ve Breakcümlesi yöntemlerinin kullanımını kısıtlama hakkını saklı tutar.

Karakter sayıları hakkında daha fazla bilgi, [ÇEVIRMEN SSS](https://www.microsoft.com/en-us/translator/faq.aspx)' de yer alır.
