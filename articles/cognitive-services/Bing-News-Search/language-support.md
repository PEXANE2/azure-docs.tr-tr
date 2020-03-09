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
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78385069"
---
# <a name="language-and-region-support-for-the-bing-news-search-api"></a>Bing Haber Arama API'si için dil ve bölge desteği

Bing Haber Arama API'si, birden fazla dili olan çok sayıda ülkeyi/bölgeyi destekler. Bir ülkeyi/bölgeyi bir sorgu ile belirtmek, birincil olarak bu ülkenin/bölgedeki ilgi alanları temelinde arama sonuçlarını iyileştirmesini sağlar. Ayrıca, sonuçlar Bing bağlantıları içerebilir ve bu bağlantılar, belirtilen ülkeye/bölgeye veya dile göre Bing Kullanıcı deneyimini yerelleştiremeyebilir.

`cc` sorgu parametresini kullanarak bir ülke/bölge belirtebilirsiniz. Bir ülke/bölge belirtirseniz, `Accept-Language` HTTP üstbilgisini kullanarak bir veya daha fazla dil kodu da belirtmeniz gerekir. Desteklenen diller, countr/bölgeye göre farklılık gösterir. Bunlar, pazarlar tablosundaki her bir ülke/bölge için verilir.

Alternatif olarak, `mkt` sorgu parametresini ve **pazarlar** tablosundan bir kodu kullanarak pazara de belirtebilirsiniz. Bir pazar aynı anda belirtildiğinde bir ülke/bölge ve tercih edilen dil belirtilir. `setLang` sorgu parametresi bu durumda bir dil koduna ayarlanabilir. Bu, Kullanıcı Bing 'i başka bir dilde görmeyi tercih etmediği müddetçe, genellikle `mkt` tarafından belirtilen dildir.

## <a name="supported-markets-for-news-search-endpoint"></a>Haber arama uç noktası için desteklenen pazarlar

`/news/search` uç noktası için aşağıdaki tabloda, `mkt` sorgu parametresini belirtmek için kullanabileceğiniz Pazar kodu değerleri listelenmektedir. Bing yalnızca bu pazarlar için içerik döndürür. Liste değişebilir.  

`cc` sorgu parametresinde belirtebileceğiniz ülke/bölge kodlarının listesi için bkz. [Ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------|
|Danimarka|Danca|v-DK|
|Avusturya|Almanca|de-AT|
|İsviçre|Almanca|de-CH|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|tr-AU|
|Kanada|Türkçe|CA tr|
|Birleşik Krallık|Türkçe|en-GB|
|Endonezya|Türkçe|En-KIMLIĞI|
|İrlanda|Türkçe|IE tr|
|Hindistan|Türkçe|tr-giriş|
|Malezya|Türkçe|En-MY|
|Yeni Zelanda|Türkçe|tr NZ|
|Filipinler Cumhuriyeti|Türkçe|En-PH|
|Singapur|Türkçe|En-SG|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Türkçe|Genel|En-WW|
|Türkçe|Genel|En-XA|
|Güney Afrika|Türkçe|En-ZA|
|Arjantin|İspanyolca|es-AR|
|Şili|İspanyolca|es-CL|
|İspanya|İspanyolca|es-ES|
|Meksika|İspanyolca|es-MX|
|Amerika Birleşik Devletleri|İspanyolca|es-US|
|İspanyolca|Genel|es-XL|
|Finlandiya|Fince|FI-FI|  
|Fransa|Fransızca|fr-of|
|Kanada|Fransızca|fr-CA|
|Belçika|Felemenkçe|nl-|
|İsviçre|Fransızca|FR-CH|
|Fransa|Fransızca|fr-FR|  
|İtalya|İtalyanca|İt-IT|
|Hong Kong SAR|Geleneksel Çince|zh-HK|  
|Tayvan|Geleneksel Çince|zh-TW|
|Japonya|Japonca|ja-JP|  
|Güney Kore|Korece|ko-KR|  
|Hollanda|Felemenkçe|NL-NL|  
|Çin Halk Cumhuriyeti|Çince|zh-CN|  
|Brezilya|Portekizce|pt-BR|
|Rusya|Rusça|ru-RU|  
|İsveç|İsveççe|sv-SE|  
|Türkiye|Türkçe|tr-TR|  

## <a name="supported-markets-for-news-endpoint"></a>Haber uç noktası için desteklenen pazarlar
`/news` uç noktası için aşağıdaki tabloda, `mkt` sorgu parametresini belirtmek için kullanabileceğiniz Pazar kodu değerleri listelenmektedir. Bing yalnızca bu pazarlar için içerik döndürür. Liste değişebilir.  

`cc` sorgu parametresinde belirtebileceğiniz ülke/bölge kodlarının listesi için bkz. [Ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------|
|Danimarka|Danca|v-DK|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|tr-AU|
|Birleşik Krallık|Türkçe|en-GB|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Türkçe|Genel|En-WW|
|Şili|İspanyolca|es-CL|
|Meksika|İspanyolca|es-MX|
|Amerika Birleşik Devletleri|İspanyolca|es-US|
|Finlandiya|Fince|FI-FI|  
|Kanada|Fransızca|fr-CA|
|Fransa|Fransızca|fr-FR|  
|İtalya|İtalyanca|İt-IT|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|

## <a name="supported-markets-for-news-trending-endpoint"></a>Haber eğilim uç noktası için desteklenen pazarlar
`/news/trendingtopics` uç noktası için aşağıdaki tabloda, `mkt` sorgu parametresini belirtmek için kullanabileceğiniz Pazar kodu değerleri listelenmektedir. Bing yalnızca bu pazarlar için içerik döndürür. Liste değişebilir.  

`cc` sorgu parametresinde belirtebileceğiniz ülke/bölge kodlarının listesi için bkz. [Ülke kodları](#countrycodes).  

|Ülke/bölge|Dil|Pazar kodu|  
|---------------------|--------------|-----------------|
|Almanya|Almanca|de-DE|
|Avustralya|Türkçe|tr-AU|
|Birleşik Krallık|Türkçe|en-GB|
|Amerika Birleşik Devletleri|Türkçe|tr-TR|
|Kanada|Türkçe|CA tr|
|Hindistan|Türkçe|tr-giriş|
|Fransa|Fransızca|fr-FR|
|Kanada|Fransızca|fr-CA|
|Brezilya|Portekizce|pt-BR|
|Çin Halk Cumhuriyeti|Çince|zh-CN|


<a name="countrycodes"></a>   
### <a name="country-codes"></a>Ülke kodları  

Aşağıda, `cc` sorgu parametresinde belirtebileceğiniz ülke/bölge kodları verilmiştir. Liste değişebilir.  

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
|Hong Kong SAR|HK|  
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
