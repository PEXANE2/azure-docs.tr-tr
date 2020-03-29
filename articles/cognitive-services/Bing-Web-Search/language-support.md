---
title: Dil desteği - Bing Web Arama API
titleSuffix: Azure Cognitive Services
description: Bing Haber Arama API'sı tarafından desteklenen doğal dillerin, ülkelerin ve bölgelerin listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: 9425de6e75a9a46d71ff85ce49b0650c8e7a9a16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68882681"
---
# <a name="language-and-region-support-for-the-bing-web-search-api"></a>Bing Web Arama API'sı için dil ve bölge desteği

Bing Web Arama API'si, çoğu birden fazla dili olan üç düzineden fazla ülkeyi veya bölgeyi destekler. Sorgulu bir ülke veya bölge belirtmek, arama sonuçlarını o ülke veya bölgelerin ilgi alanlarına göre hassaslaştırmaya yardımcı olur. Sonuçlar Bing'e bağlantılar içerebilir ve bu bağlantılar Bing kullanıcı deneyimini belirtilen ülkeye/bölgeye veya dile göre yerelleştirebilir.

Sorgu parametresini `cc` kullanarak bir ülke veya bölge belirtebilirsiniz. Bir ülke veya bölge belirtildiğinde, [ `Accept-Language` üstbilgiyle](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#headers)bir veya daha fazla dil kodu belirtmeniz gerekir. Her pazarda desteklenen dillerin listesi için [Piyasalar tablosunu](#markets) kullanın.

Alternatif olarak, `mkt` sorgu parametresi ve **Piyasalar** tablosundan bir kod ile piyasabelirtebilirsiniz. Bir pazar belirtilmesi aynı anda bir ülke veya bölge ve tercih edilen bir dil belirtir. Sorgu parametresi ile `setLang` dili açıkça ayarlayabilirsiniz.

## <a name="countriesregions"></a>Ülkeler/ bölgeler

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
|Endonezya|Kimlik|
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
|Amerika Birleşik Devletleri|ABD|

## <a name="markets"></a>Piyasa

|Ülke/bölge|Dil|Piyasa Kodu|
|-------|--------|-----------|
|Arjantin|İspanyolca|es-AR|
|Avustralya|Türkçe|en-AU|
|Avusturya|Almanca|de-AT|
|Belçika|Felemenkçe|nl-BE|
|Belçika|Fransızca|fr-BE|
|Brezilya|Portekizce|pt-BR|
|Kanada|Türkçe|tr-CA|
|Kanada|Fransızca|fr-CA|
|Şili|İspanyolca|es-CL|
|Danimarka|Danca|da-DK|
|Finlandiya|Fince|fi-FI|
|Fransa|Fransızca|fr-FR|
|Almanya|Almanca|de-DE|
|Hong Kong ÖİB|Geleneksel Çince|zh-HK|
|Hindistan|Türkçe|en-IN|
|Endonezya|Türkçe|tr-ID|
|İtalya|İtalyanca|it-IT|
|Japonya|Japonca|ja-JP|
|Güney Kore|Korece|ko-KR|
|Malezya|Türkçe|en-MY|
|Meksika|İspanyolca|es-MX|
|Hollanda|Felemenkçe|nl-NL|
|Yeni Zelanda|Türkçe|en-NZ|
|Norveç|Norveççe|Hayır|
|Çin|Çince|zh-CN|
|Polonya|Lehçe|pl-PL|
|Portekiz|Portekizce|pt-PT|
|Filipinler|Türkçe|en-PH|
|Rusya|Rusça|ru-RU|
|Suudi Arabistan|Arapça|ar-SA|
|Güney Afrika|Türkçe|en-ZA|
|İspanya|İspanyolca|es-ES|
|İsveç|İsveççe|sv-SE|
|İsviçre|Fransızca|fr-CH|
|İsviçre|Almanca|de-CH|
|Tayvan|Geleneksel Çince|zh-TW|
|Türkiye|Türkçe|tr-TR|
|Birleşik Krallık|Türkçe|tr-TR|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Amerika Birleşik Devletleri|İspanyolca|es-ABD|

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Resim Arama API’si başvurusu](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
