---
title: Karakter Sayıları - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Bu makalede, Azure Bilişsel Hizmetler Çevirmeni Metin API'sinin karakterleri nasıl saydığını açıklayarak içeriği nasıl yutturabileceğini anlayabilirsiniz.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cb70b8624ac0d909511032622948f14f8764b153
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73888156"
---
# <a name="how-the-translator-text-api-counts-characters"></a>Çevirmen Metin API'si karakterleri nasıl sayar?

Çevirmen Metin API'si, giriş metninin her Unicode kod noktasını bir karakter olarak sayar. Bir metnin bir dile her çevirisi, istek tek bir API çağrısında birden çok dile çevrilmiş olsa bile ayrı bir çeviri olarak sayılır. Yanıtın uzunluğu önemli değil.

Önemli olan:

* İstek gövdesinde Çevirmen Metin API'sine geçen metin
   * `Text`Translate, Transliterate ve Dictionary Lookup yöntemlerini kullanırken
   * `Text`ve `Translation` Sözlük Örnekleri yöntemini kullanırken
* Tüm biçimlendirme: HTML, XML etiketleri, vb istek gövdesinin metin alanı içinde. İstek oluşturmak için kullanılan JSON gösterimi (örneğin "Metin:") sayılmaz.
* Tek bir mektup
* Noktalama işaretleri
* Bir boşluk, sekme, biçimlendirme ve her türlü beyaz boşluk karakteri
* Unicode'da tanımlanan her kod noktası
* Aynı metni daha önce çevirmiş olsanız bile, tekrarlanan bir çeviri

Çince ve Japonca Kanji gibi ideogramlara dayanan komut dosyaları için Çevirmen Metin API'si yine de ideogram başına bir karakter olan Unicode kod noktalarının sayısını sayacaktır. Özel Durum: Unicode vekilleri iki karakter olarak sayılır.

İstek, sözcük, bayt veya cümle sayısı karakter sayısında önemsizdir.

Algılama ve Kesme Cümlesi yöntemlerine yapılan çağrılar karakter tüketiminde sayılmaz. Ancak, Algılama ve Kesme Cümlesi yöntemlerine yapılan çağrıların sayılan diğer işlevlerin kullanımıyla makul bir orantılı olmasını bekliyoruz. Yaptığınız Algıla veya Kesme Cümlesi çağrılarının sayısı sayılan diğer yöntemlerin sayısını 100 kat aşıyorsa, Microsoft Algılama ve Kesme Cümlesi yöntemlerini kullanımınızı kısıtlama hakkını saklı tutar.


Karakter sayıları hakkında daha fazla bilgi [Microsoft Translator SSS'de](https://www.microsoft.com/en-us/translator/faq.aspx)yer almaktadır.
