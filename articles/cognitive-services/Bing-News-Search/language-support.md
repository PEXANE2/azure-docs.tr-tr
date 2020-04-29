---
title: Dil desteği-Bing Haber Arama API'si
titleSuffix: Azure Cognitive Services
description: Bing Haber Arama API'si tarafından desteklenen doğal dillerin, ülkelerin ve bölgelerin listesi.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: aahi
ms.openlocfilehash: d15058126f43fff328acfc563ffd081164a69a90
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79220198"
---
# <a name="language-and-region-support-for-the-bing-news-search-api"></a>Bing Haber Arama API'si için dil ve bölge desteği

Bing Haber Arama API'si, birden fazla dili olan çok sayıda ülkeyi/bölgeyi destekler. Bir ülkeyi/bölgeyi bir sorgu ile belirtmek, birincil olarak bu ülkenin/bölgedeki ilgi alanları temelinde arama sonuçlarını iyileştirmesini sağlar. Ayrıca, sonuçlar Bing bağlantıları içerebilir ve bu bağlantılar, belirtilen ülkeye/bölgeye veya dile göre Bing Kullanıcı deneyimini yerelleştiremeyebilir.

`cc` Sorgu parametresini kullanarak bir ülke/bölge belirtebilirsiniz. Bir ülke/bölge belirtirseniz, `Accept-Language` http üstbilgisini kullanarak bir veya daha fazla dil kodu da belirtmeniz gerekir. Desteklenen diller, countr/bölgeye göre farklılık gösterir. Bunlar, pazarlar tablosundaki her bir ülke/bölge için verilir.

Alternatif olarak, `mkt` sorgu parametresini ve **pazarlar** tablosundan bir kodu kullanarak pazarı de belirtebilirsiniz. Bir pazar aynı anda belirtildiğinde bir ülke/bölge ve tercih edilen dil belirtilir. `setLang` Sorgu parametresi bu durumda bir dil koduna ayarlanabilir. Genellikle, Kullanıcı Bing 'yi başka bir dilde `mkt` görmeyi tercih etmediği müddetçe, bu dil tarafından belirtilen dildir.

## <a name="supported-markets-for-news-search-endpoint"></a>Haber arama uç noktası için desteklenen pazarlar

`/news/search` Uç nokta için aşağıdaki tabloda, `mkt` sorgu parametresini belirtmek için kullanabileceğiniz piyasa kodu değerleri listelenmektedir. Bing yalnızca bu pazarlar için içerik döndürür. Liste değişebilir.  

`cc` Sorgu parametresinde belirtebileceğiniz ülke/bölge kodlarının bir listesi için bkz. [Ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------|
|Danimarka|Danca|da-DK|
|Avusturya|Almanca|de|
|İsviçre|Almanca|devre dışı|
|Almanya|Almanca|de-DE|
|Avustralya|İngilizce|En-AU|
|Kanada|İngilizce|en-CA|
|Birleşik Krallık|İngilizce|en-GB|
|Endonezya|İngilizce|En-KIMLIĞI|
|İrlanda|İngilizce|en-IE|
|Hindistan|İngilizce|En-ın|
|Malezya|İngilizce|En-MY|
|Yeni Zelanda|İngilizce|En-NZ|
|Filipinler Cumhuriyeti|İngilizce|En-PH|
|Singapur|İngilizce|En-SG|
|Amerika Birleşik Devletleri|İngilizce|tr-TR|
|İngilizce|genel|En-WW|
|İngilizce|genel|En-XA|
|Güney Afrika|İngilizce|En-ZA|
|Arjantin|İspanyolca|es-AR|
|Şili|İspanyolca|es-CL|
|İspanya|İspanyolca|es-ES|
|Meksika|İspanyolca|es-MX|
|Amerika Birleşik Devletleri|İspanyolca|es-US|
|İspanyolca|genel|es-XL|
|Finlandiya|Fince|fi-FI|  
|Fransa|Fransızca|fr-of|
|Kanada|Fransızca|fr-CA|
|Belçika|Felemenkçe|nl-|
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

## <a name="supported-markets-for-news-endpoint"></a>Haber uç noktası için desteklenen pazarlar
`/news` Uç nokta için aşağıdaki tabloda, `mkt` sorgu parametresini belirtmek için kullanabileceğiniz piyasa kodu değerleri listelenmektedir. Bing yalnızca bu pazarlar için içerik döndürür. Liste değişebilir.  

`cc` Sorgu parametresinde belirtebileceğiniz ülke/bölge kodlarının bir listesi için bkz. [Ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------|
|Danimarka|Danca|da-DK|
|Almanya|Almanca|de-DE|
|Avustralya|İngilizce|En-AU|
|Birleşik Krallık|İngilizce|en-GB|
|Amerika Birleşik Devletleri|İngilizce|tr-TR|
|İngilizce|genel|En-WW|
|Şili|İspanyolca|es-CL|
|Meksika|İspanyolca|es-MX|
|Amerika Birleşik Devletleri|İspanyolca|es-US|
|Finlandiya|Fince|fi-FI|  
|Kanada|Fransızca|fr-CA|
|Fransa|Fransızca|fr-FR|  
|İtalya|İtalyanca|it-IT|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Haber eğilim uç noktası için desteklenen pazarlar
`/news/trendingtopics` Uç nokta için aşağıdaki tabloda, `mkt` sorgu parametresini belirtmek için kullanabileceğiniz piyasa kodu değerleri listelenmektedir. Bing yalnızca bu pazarlar için içerik döndürür. Liste değişebilir.  

`cc` Sorgu parametresinde belirtebileceğiniz ülke/bölge kodlarının bir listesi için bkz. [Ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------|
|Almanya|Almanca|de-DE|
|Avustralya|İngilizce|En-AU|
|Birleşik Krallık|İngilizce|en-GB|
|Amerika Birleşik Devletleri|İngilizce|tr-TR|
|Kanada|İngilizce|en-CA|
|Hindistan|İngilizce|En-ın|
|Fransa|Fransızca|fr-FR|
|Kanada|Fransızca|fr-CA|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Ülke kodları  

`cc` Sorgu parametresinde belirtebileceğiniz ülke/bölge kodları aşağıda verilmiştir. Liste değişebilir.  

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
Bing Haber Arama uç noktaları hakkında daha fazla bilgi için bkz. [Haber Arama API v7 Reference](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-news-api-v7-reference).
