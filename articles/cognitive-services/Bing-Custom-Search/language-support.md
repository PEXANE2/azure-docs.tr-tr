---
title: Dil desteği-Bing Özel Arama API'si
titleSuffix: Azure Cognitive Services
description: Bing Özel Arama API'si için desteklenen dillerin ve bölgelerin listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 09/25/2018
ms.author: aahi
ms.openlocfilehash: 004bd973651d5903db4254a8883be2c8a83d9b38
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84310589"
---
# <a name="language-and-region-support-for-the-bing-custom-search-api"></a>Bing Özel Arama API'si için dil ve bölge desteği

Bing Özel Arama API'si, birden fazla dilden çok sayıda ülkeyi/bölgeyi destekler.

İsteğe bağlı olsa da, isteğin, sonuçların gelmesini istediğiniz pazarı tanımlayan [Mkt](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#mkt) sorgu parametresini belirtmesi gerekir. İsteğe bağlı sorgu parametrelerinin bir listesi için bkz. [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters)

Sorgu parametresini kullanarak bir ülke/bölge belirtebilirsiniz `cc` . Bir ülke/bölge belirtirseniz, üstbilgiyi kullanarak bir veya daha fazla dil kodu da belirtmeniz gerekir `Accept-Language` . Desteklenen diller ülkeye/bölgeye göre farklılık gösterir. Bunlar, **pazarlar** tablosundaki her bir ülke/bölge için verilir.

`Accept-Language`Üst bilgi ve `setLang` sorgu parametresi birbirini dışlıyor; ikisini de belirtmeyin. Ayrıntılar için bkz. [Accept-Language](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#acceptlanguage).

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
|Birleşik Devletler|İngilizce|tr-TR|
|Birleşik Devletler|İspanyolca|es-US|
