---
title: Dil desteği - Bing Görsel Arama API'si
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si tarafından desteklenen doğal dillerin, ülkelerin ve bölgelerin listesi. Bing Görsel Arama API'si, çoğu birden fazla dili olan üç düzineden fazla ülkeyi/bölgeyi destekler.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: scottwhi
ms.openlocfilehash: b17341bc234ff3dfecc2c6dcd84ef77116a95d61
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68883542"
---
# <a name="language-and-region-support-for-the-bing-visual-search-api"></a>Bing Görsel Arama API'si için dil ve bölge desteği

Bing Görsel Arama API'si, çoğu birden fazla dili olan üç düzineden fazla ülkeyi/bölgeyi destekler. Her istek, kullanıcının ülkesini/bölgesini ve tercih edilen dilini içermelidir. Kullanıcının pazarını bilmek Bing'in uygun sonuçları geri döndürmesini sağlar. Bir ülke/bölge ve dil belirtmezseniz, Bing kullanıcının ülkesini/bölgesini ve dilini belirlemek için en iyi çabayı gösterir. Sonuçlar Bing'e bağlantılar içerebileceğinden, ülke/bölge ve dili bilmek, kullanıcı Bing bağlantılarını tıklattığında tercih edilen yerelleştirilmiş Bing kullanıcı deneyimi sağlayabilir.

Ülke/bölge ve dili belirtmek için `mkt` aşağıdaki **Piyasalar** tablosundan (market) sorgu parametresini bir koda ayarlayın. Pazar hem bir ülke / bölge ve dil belirtir. Kullanıcı ekran metnini farklı bir dilde görmeyi `setLang` tercih ederse, sorgu parametresini uygun dil koduna ayarlayın.

Alternatif olarak, sorgu parametresini `cc` kullanarak ülke/bölge belirtebilirsiniz. Bir ülke/bölge belirtirseniz, HTTP üstbilgisini `Accept-Language` kullanarak bir veya daha fazla dil kodu da belirtmeniz gerekir. Desteklenen diller ülkeye/bölgeye göre değişir; piyasalar tablosunda her ülke için verilir.



> [!NOTE]
> Aşağıdaki pazar kısıtlamaları geçerlidir:
>
> - Görüntü tanıma ek açıklamaları yalnızca İngilizce olarak kullanılabilir.
> - Tarif, alışveriş ve sayfa dahil içgörüler sadece en-ABD pazarında mevcuttur.


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
