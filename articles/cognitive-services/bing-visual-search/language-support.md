---
title: Dil desteği-Bing Görsel Arama API'si
titleSuffix: Azure Cognitive Services
description: Bing Görsel Arama API'si tarafından desteklenen doğal dillerin, ülkelerin ve bölgelerin listesi. Bing Görsel Arama API'si, birden fazla dilden çok sayıda ülkeyi/bölgeyi destekler.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: scottwhi
ms.openlocfilehash: 6d93b6d024fc966f3b260602f4798a2d77669a35
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96501167"
---
# <a name="language-and-region-support-for-the-bing-visual-search-api"></a>Bing Görsel Arama API'si için dil ve bölge desteği

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing Görsel Arama API'si, birden fazla dilden çok sayıda ülkeyi/bölgeyi destekler. Her istek, kullanıcının ülke/bölge ve tercih edilen dilini içermelidir. Kullanıcının pazarını bilmek, Bing 'in uygun sonuçlara dönüşmesini sağlar. Bir ülke/bölge ve dil belirtmezseniz, Bing kullanıcının ülke/bölge ve dilini belirlemek için en iyi çabayı sağlar. Sonuçlar Bing bağlantıları içerebileceğinden, Kullanıcı Bing bağlantılarına tıkladığında ülke/bölge ve dil tercih edilen yerelleştirilmiş Bing Kullanıcı deneyimi sağlayabilir.

Ülke/bölge ve dil belirtmek için, `mkt` (Pazar) sorgu parametresini aşağıdaki **pazarlar** tablosundan bir koda ayarlayın. Pazar, hem ülke/bölge hem de dili belirtir. Kullanıcı farklı bir dilde görüntüleme metnini görmeyi tercih ediyorsa, `setLang` sorgu parametresini uygun dil koduna ayarlayın.

Alternatif olarak, ülke/bölge ' yi `cc` sorgu parametresini kullanarak belirtebilirsiniz. Bir ülke/bölge belirtirseniz, http üstbilgisini kullanarak bir veya daha fazla dil kodu da belirtmeniz gerekir `Accept-Language` . Desteklenen diller ülkeye/bölgeye göre farklılık gösterir. Bunlar, pazarlar tablosundaki her bir ülke için verilir.



> [!NOTE]
> Aşağıdaki Pazar kısıtlamaları geçerlidir:
>
> - Görüntü tanıma ek açıklamaları yalnızca Ingilizce olarak kullanılabilir.
> - İçgörüler, alışveriş ve sayfalar dahil Öngörüler yalnızca en-US pazarında sunulmaktadır.


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