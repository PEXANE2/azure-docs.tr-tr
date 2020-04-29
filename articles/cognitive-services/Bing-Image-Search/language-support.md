---
title: Dil desteği-Bing Resim Arama API'si
titleSuffix: Azure Cognitive Services
description: Bing Resim Arama API'si hangi ülkelerin/bölgelerin ve dillerin desteklendiğini öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: ca3821b6088e45730334d1b0971e270b1d86dfce
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "68881915"
---
# <a name="language-and-region-support-for-the-bing-image-search-api"></a>Bing Resim Arama API'si için dil ve bölge desteği

Bing Resim Arama API'si, birden fazla dilden çok sayıda ülkeyi/bölgeyi destekler. Bir ülkeyi/bölgeyi bir sorgu ile belirtmek, birincil olarak bu ülkenin/bölgedeki ilgi alanları temelinde arama sonuçlarını iyileştirmesini sağlar. Ayrıca, sonuçlar Bing bağlantıları içerebilir ve bu bağlantılar, belirtilen ülkeye/bölgelere veya dile göre Bing Kullanıcı deneyimini yerelleştiremeyebilir.

Ülke/bölge ve dil belirtmek için, `mkt` (Pazar) sorgu parametresini aşağıdaki **pazarlar** tablosundan bir koda ayarlayın. Pazar, hem ülke/bölge hem de dili belirtir. Kullanıcı farklı bir dilde görüntüleme metnini görmeyi tercih ediyorsa, sorgu parametresini uygun dil `setLang` koduna ayarlayın.

Alternatif olarak, ülke/bölge ' yi `cc` sorgu parametresini kullanarak belirtebilirsiniz. Bir ülke/bölge belirtirseniz, `Accept-Language` http üstbilgisini kullanarak bir veya daha fazla dil kodu da belirtmeniz gerekir. Desteklenen diller ülkeye/bölgeye göre farklılık gösterir. Bunlar, pazarlar tablosundaki her bir ülke/bölge için verilir.

> [!NOTE]
> Popüler resimler API 'SI Şu anda yalnızca aşağıdaki pazarları desteklemektedir:
> - en-US (Ingilizce, Birleşik Devletler)
> - en-CA (Ingilizce, Kanada)
> - En-AU (Ingilizce, Avustralya)
> - zh-CN (Çince, Çin)

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
|Amerika Birleşik Devletleri|İngilizce|tr-TR|
|Amerika Birleşik Devletleri|İspanyolca|es-US|

## <a name="next-steps"></a>Sonraki adımlar
Bing Haber Arama uç noktaları hakkında daha fazla bilgi için bkz. [News resım arama API v7 Reference](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference).
