---
title: Dil desteği-Bing Web Araması API'si
titleSuffix: Azure Cognitive Services
description: Bing Web Araması API'si tarafından desteklenen doğal dillerin, ülkelerin ve bölgelerin listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: 998e18f8901dda3430d5289e0590ef8099b6fb8c
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93095467"
---
# <a name="language-and-region-support-for-the-bing-web-search-api"></a>Bing Web Araması API'si için dil ve bölge desteği

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](https://aka.ms/cogsvcs/bingmove)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](https://aka.ms/cogsvcs/bingmigration).

Bing Web Araması API'si, üç düzine ülke veya bölgeyi destekler ve birden çok dili vardır. Bir ülkeyi veya bölgeyi bir sorgu ile belirtmek, bu ülkeye veya bölgelere ilgi alanları temelinde arama sonuçlarının iyileştirmesine yardımcı olur. Sonuçlar Bing bağlantıları içerebilir ve bu bağlantılar, belirtilen ülkeye/bölgeye veya dile göre Bing Kullanıcı deneyimini yerelleştiremeyebilir.

Sorgu parametresini kullanarak bir ülke veya bölge belirtebilirsiniz `cc` . Ülke veya bölge belirtildiğinde, [ `Accept-Language` üst](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#headers)bilgiyle bir veya daha fazla dil kodu belirtmeniz gerekir. Her pazarda desteklenen dillerin bir listesi için [pazarlar tablosunu](#markets) kullanın.

Alternatif olarak, bir Pazar, `mkt` sorgu parametresi ve **pazarlar** tablosundan bir kod ile de belirtebilirsiniz. Bir pazar aynı anda belirtildiğinde bir ülke veya bölgeyi ve tercih edilen dili belirtir. Dili `setLang` sorgu parametresiyle açıkça ayarlayabilirsiniz.

## <a name="countriesregions"></a>Ülkeler/bölgeler

|Ülke/bölge|Kod|
|-------|----|
|Arjantin|AR|
|Avustralya|AU|
|Avusturya|AT|
|Belçika|BE|
|Brezilya|BR|
|Kanada|CA|
|Şili|CL|
|Danimarka|DK|
|Finlandiya|FI|
|Fransa|GS|
|Almanya|DE|
|Hong Kong ÖİB|HK|
|Hindistan|IN|
|Endonezya|ID|
|İtalya|BT|
|Japonya|JP|
|Güney Kore|KR|
|Malezya|MY|
|Meksika|MX|
|Hollanda|NL|
|Yeni Zelanda|NZ|
|Norveç|NO|
|Çin|CN|
|Polonya|PL|
|Portekiz|PT|
|Filipinler|PH|
|Rusya|RU|
|Suudi Arabistan|SA|
|Güney Afrika|ZA|
|İspanya|ES|
|İsveç|SE|
|İsviçre|CH|
|Tayvan|TW|
|Türkiye|TR|
|Birleşik Krallık|GB|
|Birleşik Devletler|ABD|

## <a name="markets"></a>Pazar

|Ülke/bölge|Dil|Pazar kodu|
|-------|--------|-----------|
|Arjantin|İspanyolca|es-AR|
|Avustralya|İngilizce|En-AU|
|Avusturya|Almanca|de|
|Belçika|Felemenkçe|nl-|
|Belçika|Fransızca|fr-of|
|Brezilya|Portekizce|pt-BR|
|Kanada|İngilizce|en-CA|
|Kanada|Fransızca|fr-CA|
|Şili|İspanyolca|es-CL|
|Danimarka|Danca|da-DK|
|Finlandiya|Fince|fi-FI|
|Fransa|Fransızca|fr-FR|
|Almanya|Almanca|de-DE|
|Hong Kong ÖİB|Geleneksel Çince|zh-HK|
|Hindistan|İngilizce|En-ın|
|Endonezya|İngilizce|En-KIMLIĞI|
|İtalya|İtalyanca|it-IT|
|Japonya|Japonca|ja-JP|
|Güney Kore|Korece|ko-KR|
|Malezya|İngilizce|En-MY|
|Meksika|İspanyolca|es-MX|
|Hollanda|Felemenkçe|nl-NL|
|Yeni Zelanda|İngilizce|En-NZ|
|Norveç|Norveççe|Hayır-Hayır|
|Çin|Çince|zh-CN|
|Polonya|Lehçe|pl-PL|
|Portekiz|Portekizce|pt-PT|
|Filipinler|İngilizce|En-PH|
|Rusya|Rusça|ru-RU|
|Suudi Arabistan|Arapça|ar-SA|
|Güney Afrika|İngilizce|En-ZA|
|İspanya|İspanyolca|es-ES|
|İsveç|İsveççe|sv-SE|
|İsviçre|Fransızca|fr-CH|
|İsviçre|Almanca|devre dışı|
|Tayvan|Geleneksel Çince|zh-TW|
|Türkiye|Türkçe|tr-TR|
|Birleşik Krallık|İngilizce|en-GB|
|Birleşik Devletler|İngilizce|en-US|
|Birleşik Devletler|İspanyolca|es-US|

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Resim Arama API’si başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
