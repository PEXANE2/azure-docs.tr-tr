---
title: Dil desteği - Bing Haberler Arama API
titleSuffix: Azure Cognitive Services
description: Bing Haber Arama API'sı tarafından desteklenen doğal dillerin, ülkelerin ve bölgelerin listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: d15058126f43fff328acfc563ffd081164a69a90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220198"
---
# <a name="language-and-region-support-for-the-bing-news-search-api"></a>Bing Haberler Arama API'si için dil ve bölge desteği

Bing Haberler Arama API'si, çoğu birden fazla dili olan çok sayıda ülkeyi/bölgeyi destekler. Sorgulu bir ülke/bölge belirtmek, öncelikle arama sonuçlarını o ülkedeki/bölgedeki ilgi alanlarına göre hassaslaştırmak için hizmet eder. Ayrıca, sonuçlar Bing'e bağlantılar içerebilir ve bu bağlantılar Bing kullanıcı deneyimini belirtilen ülkeye/bölgeye veya dile göre yerelleştirebilir.

Sorgu parametresini `cc` kullanarak bir ülke/bölge belirtebilirsiniz. Bir ülke/bölge belirtirseniz, HTTP üstbilgisini `Accept-Language` kullanarak bir veya daha fazla dil kodu da belirtmeniz gerekir. Desteklenen diller countr/region'a göre değişir; piyasalar tablosundaki her ülke/bölge için verilir.

Alternatif olarak, sorgu parametresini `mkt` ve **Piyasalar** tablosundan bir kodu kullanarak piyasayı belirtebilirsiniz. Bir pazar belirtilmesi aynı anda bir ülke/ bölge ve tercih edilen bir dil belirtir. Sorgu `setLang` parametresi bu durumda bir dil koduolarak ayarlanabilir; genellikle bu, kullanıcı Bing'i başka bir dilde görmeyi tercih etmediği sürece aynı `mkt` dildir.

## <a name="supported-markets-for-news-search-endpoint"></a>Haber arama bitiş noktası için desteklenen pazarlar

`/news/search` Bitiş noktası için aşağıdaki tablo, sorgu parametresini `mkt` belirtmek için kullanabileceğiniz piyasa kodu değerlerini listeler. Bing yalnızca bu pazarlar için içeriği döndürür. Liste değiştirilebilir.  

`cc` Sorgu parametresinde belirtebileceğiniz ülke/bölge kodları listesi için Ülke [Kodları'na](#countrycodes)bakın.  

|Ülke/bölge|Dil|Piyasa kodu|  
|---------------------|--------------|-----------------|
|Danimarka|Danca|da-DK|
|Avusturya|Almanca|de-AT|
|İsviçre|Almanca|de-CH|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|en-AU|
|Kanada|Türkçe|tr-CA|
|Birleşik Krallık|Türkçe|tr-TR|
|Endonezya|Türkçe|tr-ID|
|İrlanda|Türkçe|en-IE|
|Hindistan|Türkçe|en-IN|
|Malezya|Türkçe|en-MY|
|Yeni Zelanda|Türkçe|en-NZ|
|Filipinler Cumhuriyeti|Türkçe|en-PH|
|Singapur|Türkçe|en-SG|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Türkçe|genel|en-WW|
|Türkçe|genel|en-XA|
|Güney Afrika|Türkçe|en-ZA|
|Arjantin|İspanyolca|es-AR|
|Şili|İspanyolca|es-CL|
|İspanya|İspanyolca|es-ES|
|Meksika|İspanyolca|es-MX|
|Amerika Birleşik Devletleri|İspanyolca|es-ABD|
|İspanyolca|genel|es-XL|
|Finlandiya|Fince|fi-FI|  
|Fransa|Fransızca|fr-BE|
|Kanada|Fransızca|fr-CA|
|Belçika|Felemenkçe|nl-BE|
|İsviçre|Fransızca|fr-CH|
|Fransa|Fransızca|fr-FR|  
|İtalya|İtalyanca|it-IT|
|Hong Kong ÖİB|Geleneksel Çince|zh-HK|  
|Tayvan|Geleneksel Çince|zh-TW|
|Japonya|Japonca|ja-JP|  
|Güney Kore|Korece|ko-KR|  
|Hollanda|Felemenkçe|nl-NL|  
|Çin Halk Cumhuriyeti|Çince|zh-CN|  
|Brezilya|Portekizce|pt-BR|
|Rusya|Rusça|ru-RU|  
|İsveç|İsveççe|sv-SE|  
|Türkiye|Türkçe|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Haber bitiş noktası için desteklenen pazarlar
`/news` Bitiş noktası için aşağıdaki tablo, sorgu parametresini `mkt` belirtmek için kullanabileceğiniz piyasa kodu değerlerini listeler. Bing yalnızca bu pazarlar için içeriği döndürür. Liste değiştirilebilir.  

`cc` Sorgu parametresinde belirtebileceğiniz ülke/bölge kodları listesi için Ülke [Kodları'na](#countrycodes)bakın.  

|Ülke/bölge|Dil|Piyasa kodu|  
|---------------------|--------------|-----------------|
|Danimarka|Danca|da-DK|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|en-AU|
|Birleşik Krallık|Türkçe|tr-TR|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Türkçe|genel|en-WW|
|Şili|İspanyolca|es-CL|
|Meksika|İspanyolca|es-MX|
|Amerika Birleşik Devletleri|İspanyolca|es-ABD|
|Finlandiya|Fince|fi-FI|  
|Kanada|Fransızca|fr-CA|
|Fransa|Fransızca|fr-FR|  
|İtalya|İtalyanca|it-IT|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Haber trendi uç noktası için desteklenen piyasalar
`/news/trendingtopics` Bitiş noktası için aşağıdaki tablo, sorgu parametresini `mkt` belirtmek için kullanabileceğiniz piyasa kodu değerlerini listeler. Bing yalnızca bu pazarlar için içeriği döndürür. Liste değiştirilebilir.  

`cc` Sorgu parametresinde belirtebileceğiniz ülke/bölge kodları listesi için Ülke [Kodları'na](#countrycodes)bakın.  

|Ülke/bölge|Dil|Piyasa kodu|  
|---------------------|--------------|-----------------|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|en-AU|
|Birleşik Krallık|Türkçe|tr-TR|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Kanada|Türkçe|tr-CA|
|Hindistan|Türkçe|en-IN|
|Fransa|Fransızca|fr-FR|
|Kanada|Fransızca|fr-CA|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Ülke kodları  

Sorgu parametresinde belirtebileceğin ülke/bölge `cc` kodları aşağıda veda edilebistir. Liste değiştirilebilir.  

|Ülke/bölge|Ülke kodu|  
|---------------------|------------------|  
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
|Çin Halk Cumhuriyeti|CN|  
|Polonya|PL|  
|Portekiz|PT|  
|Filipinler Cumhuriyeti|PH|  
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

## <a name="next-steps"></a>Sonraki adımlar
Bing Haber Arama bitiş noktaları hakkında daha fazla bilgi [için, Haber Arama API v7 referans](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference)bakın.
