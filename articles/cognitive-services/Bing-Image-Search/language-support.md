---
title: Dil desteği - Bing Resim Arama API
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API'si tarafından hangi ülkelerin/bölgelerin ve dillerin desteklenerek desteklenerek desteklenerek öğrenilir.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: ca3821b6088e45730334d1b0971e270b1d86dfce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68881915"
---
# <a name="language-and-region-support-for-the-bing-image-search-api"></a>Bing Resim Arama API'si için dil ve bölge desteği

Bing Resim Arama API'si, çoğu birden fazla dili olan üç düzineden fazla ülkeyi/bölgeyi destekler. Sorgulu bir ülke/bölge belirtmek, öncelikle arama sonuçlarını o ülkedeki/bölgedeki ilgi alanlarına göre hassaslaştırmak için hizmet eder. Ayrıca, sonuçlar Bing'e bağlantılar içerebilir ve bu bağlantılar Bing kullanıcı deneyimini belirtilen ülkeye/bölgelere veya dile göre yerelleştirebilir.

Ülke/bölge ve dili belirtmek için `mkt` aşağıdaki **Piyasalar** tablosundan (market) sorgu parametresini bir koda ayarlayın. Pazar hem bir ülke / bölge ve dil belirtir. Kullanıcı ekran metnini farklı bir dilde görmeyi `setLang` tercih ederse, sorgu parametresini uygun dil koduna ayarlayın.

Alternatif olarak, sorgu parametresini `cc` kullanarak ülke/bölge belirtebilirsiniz. Bir ülke/bölge belirtirseniz, HTTP üstbilgisini `Accept-Language` kullanarak bir veya daha fazla dil kodu da belirtmeniz gerekir. Desteklenen diller ülkeye/bölgeye göre değişir; piyasalar tablosundaki her ülke/bölge için verilir.

> [!NOTE]
> Trending Images API şu anda yalnızca aşağıdaki pazarları destekler:
> - tr-US (İngilizce, Amerika Birleşik Devletleri)
> - tr-CA (İngilizce, Kanada)
> - tr-AU (İngilizce, Avustralya)
> - zh-CN (Çince, Çin)

## <a name="countriesregions"></a>Ülkeler / Bölgeler

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
Bing Haberler Arama bitiş noktaları hakkında daha fazla bilgi için, [Bkz. Haberler Resim Arama API v7 referans.](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
