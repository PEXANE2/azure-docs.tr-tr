---
title: Basit sorgu söz dizimi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de tam metin arama sorguları için kullanılan basit sorgu söz dizimi başvurusu.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 5b585a903267386358552154228705c1921df619
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85255339"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama basit sorgu söz dizimi

Azure Bilişsel Arama iki adet Lucene tabanlı sorgu dili uygular: [basit sorgu ayrıştırıcısı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) ve [Lucene sorgu ayrıştırıcısı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

Basit ayrıştırıcı daha esnektir ve kusursuz bir şekilde oluşturulmuş olmasa bile bir isteği yorumlamaya çalışır. Bu esneklik nedeniyle, Azure Bilişsel Arama 'de sorgular için varsayılan değer budur. 

Basit sözdizimi, `search` aynı arama BELGELERI API 'sinin [$Filter ifadeler](search-filters.md) parametresi için kullanılan [OData sözdizimi](query-odata-filter-orderby-syntax.md) ile karıştırılmamalıdır, [arama belgelerinin](https://docs.microsoft.com/rest/api/searchservice/search-documents)parametresi için geçirilen sorgu ifadeleri için kullanılır. `search`Ve `$filter` parametrelerinin farklı sözdizimi vardır ve sorgu oluşturma, kaçış dizeleri vb. için kendi kuralları vardır.

Basit ayrıştırıcı [Apache Lucene basit sorgu ayrıştırıcısı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) sınıfına dayansa da, Azure bilişsel arama uygulama belirsiz aramayı dışlar. [Benzer arama](search-query-fuzzy.md) veya başka gelişmiş sorgu formlarına ihtiyacınız varsa, bunun yerine alternatif [tam Lucene sorgu söz dizimini](query-lucene-syntax.md) göz önünde bulundurun.

## <a name="invoke-simple-parsing"></a>Basit ayrıştırma çağır

Basit sözdizimi varsayılandır. Çağırma yalnızca sözdizimini tam değerinden basit olarak sıfırlarsanız gereklidir. Sözdizimini açıkça ayarlamak için `queryType` Search parametresini kullanın. Geçerli değerler `queryType=simple` veya içerir `queryType=full` , burada `simple` varsayılandır ve `full` daha gelişmiş sorgular için [tam Lucene sorgu ayrıştırıcısını](query-lucene-syntax.md) çağırır. 

## <a name="syntax-fundamentals"></a>Sözdizimi temelleri

Bir veya daha fazla terim içeren herhangi bir metin, sorgu yürütmesi için geçerli bir başlangıç noktası olarak kabul edilir. Azure Bilişsel Arama, metnin çözümlenmesi sırasında bulunan tüm Çeşitlemeler dahil olmak üzere, koşulların herhangi birini veya tümünü içeren belgelerle eşleşir.

Bu seslerin yanı sıra, Azure Bilişsel Arama sorgu yürütmesinin bir yönü vardır. Bu işlem, giriş dizesine daha fazla terim ve işleç eklendikçe, arama sonuçlarını azaltmak yerine artan sonuçlara neden *olabilir* . Bu genişletmenin gerçekten gerçekleşmediği, bir NOT işlecinin eklenmesine bağlıdır, AND veya OR davranışları açısından nasıl yorumlanmadığını belirleyen bir **searchMode** parametresi ayarıyla birleştirilir. Daha fazla bilgi için, bkz. [işleç](#not-operator).

### <a name="precedence-operators-grouping"></a>Öncelik işleçleri (gruplama)

Parantez, parantez içinde işleç dahil olmak üzere alt sorgular oluşturmak için kullanabilirsiniz. Örneğin, `motel+(wifi|luxury)` "Motel" terimini ve "WiFi" veya "merkezlerini" (ya da her ikisi) içeren belgeleri arar.

Alan gruplama benzerdir, ancak gruplamayı tek bir alanla kapsamlara sahiptir. Örneğin, " `hotelAmenities:(gym+(wifi|pool))` hotelAmenities" alanını "Gym" ve "WiFi" ya da "Gym" ve "havuz" olarak arar.  

### <a name="escaping-search-operators"></a>Kaçış arama işleçleri  

Basit sözdiziminde, arama işleçleri şu karakterleri içerir:`+ | " ( ) ' \`  

Bu karakterlerden herhangi biri dizindeki bir belirtecin parçasıysa, sorguda tek bir ters eğik çizgi () ekleyerek onu kaçış `\` . Örneğin, tüm terim simgeleştirme için özel bir çözümleyici kullandığınızı varsayın ve dizininiz "merkezlerini + otel" dizesini içerir. Bu belirteçle tam eşleşme almak için bir kaçış karakteri ekleyin:  `search=luxury\+hotel` . 

Daha tipik durumlar için şeyleri basit hale getirmek amacıyla, bu kural için kaçışın gerekli olmadığı iki özel durum vardır:  

+ NOT işlecinin `-` yalnızca bir boşluktan sonraki ilk karakter olması durumunda kaçış olması gerekir. `-`Ortasında görünürse (örneğin, içinde `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD` ), kaçış işlemini atlayabilirsiniz.

+ Sonek işlecinin `*` yalnızca bir boşluktan önceki son karakter olması durumunda kaçışması gerekir. `*`Ortasında görünürse (örneğin, içinde `4*4=16` ), kaçış gerekmez.

> [!NOTE]  
> Standart çözümleyici, varsayılan olarak, sözcük [temelli analiz](search-lucene-query-architecture.md#stage-2-lexical-analysis)sırasında kısa çizgilerden, boşluklardan, ve diğer karakterlere ait sözcükleri siler ve keser. Sorgu dizesinde kalması için özel karakterler gerekiyorsa, bunları dizinde koruyan bir çözümleyici gerekebilir. Bazı seçimler, hecelenmiş kelimeleri koruyan Microsoft doğal [dil Çözümleyicileri](index-add-language-analyzers.md)veya daha karmaşık desenler için özel bir çözümleyici içerir. Daha fazla bilgi için bkz. [kısmi terimler, desenler ve özel karakterler](search-query-partial-matching.md).

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>URL 'lerde güvenli olmayan ve ayrılmış karakterleri kodlama

Lütfen tüm güvenli olmayan ve ayrılmış karakterlerin bir URL 'de kodlandığını doğrulayın. Örneğin, URL 'deki bir parça/bağlantı tanımlayıcısı olduğundan, ' # ' güvenli olmayan bir karakterdir. URL 'de kullanılıyorsa, karakterin kodlanmalıdır `%23` . ' & ' ve ' = ', parametreleri sınırlandıran ve Azure Bilişsel Arama değerlerini belirten ayrılmış karakter örnekleridir. Daha fazla ayrıntı için lütfen bkz. [rfc1738: Uniform Resource Konumlandırıcıları (URL)](https://www.ietf.org/rfc/rfc1738.txt) .

Güvenli olmayan karakterler ``" ` < > # % { } | \ ^ ~ [ ]`` . Ayrılan karakterler şunlardır `; / ? : @ = + &` .

### <a name="querying-for-special-characters"></a>Özel karakterleri sorgulama

Bazı durumlarda, ' ❤ ' emoji veya ' € ' işareti gibi özel bir karakter aramak isteyebilirsiniz. Bu durumda, kullandığınız çözümleyicinin bu karakterleri filtrelemez olduğundan emin olun.  Standart çözümleyici, özel karakterlerin çoğunu yok saydığı için dizininizdeki belirteçleri olmaz.

Bu nedenle ilk adım, bu öğe belirteçlerini göz önünde bulundurmanız gereken bir çözümleyici kullandığınızdan emin olmak olacaktır. Örneğin, "Whitespace" Çözümleyicisi, boşluk ile ayrılmış karakter dizilerini belirteç olarak kabul eder, bu nedenle "❤" dizesi belirteç olarak değerlendirilir. Ayrıca, Microsoft Ingilizce Çözümleyicisi ("en. Microsoft") gibi bir çözümleyici, "€" dizesini belirteç olarak dikkate alır. Belirli bir sorgu için ürettiği belirteçleri görmek üzere [bir Çözümleyicisi test](https://docs.microsoft.com/rest/api/searchservice/test-analyzer) edebilirsiniz.

Unicode karakterler kullanılırken, simgenin sorgu URL 'sinde doğru bir şekilde atlanacağından emin olun (örneğin, "❤" için çıkış sırasını kullanır `%E2%9D%A4+` ). Postman bu çeviriyi otomatik olarak yapar.

###  <a name="query-size-limits"></a><a name="bkmk_querysizelimits"></a>Sorgu boyutu sınırları

 Azure Bilişsel Arama 'e gönderebilmeniz için sorguların boyutuyla ilgili bir sınır vardır. Özellikle, en fazla 1024 yan tümce (ve, veya ile ayrılmış ifadeler) olabilir. Ayrıca, bir sorgudaki her bir terimin boyutunda yaklaşık 32 KB 'lik bir sınır vardır. Uygulamanız program aracılığıyla arama sorguları oluşturursa, bu şekilde, sınırsız boyut sorguları oluşturmamasını sağlayan bir şekilde tasarlamayı öneririz.  

## <a name="boolean-search"></a>Boole arama

Eşleşen belgelerin bulunduğu bir dizi zengin ölçüt kümesi oluşturmak için bir sorgu dizesinde Boole işleçleri (ve, veya DEĞIL) ekleyebilirsiniz. 

### <a name="and-operator-"></a>AND işleci`+`

AND işleci bir artı işareti. Örneğin, `wifi + luxury` hem hem de içeren belgeler için arama `wifi` yapılır `luxury` .

### <a name="or-operator-"></a>OR işleci`|`

OR işleci dikey bir çubuk veya boru karakterdir. Örneğin, ya `wifi | luxury` da ya da içeren belgeleri arar `wifi` `luxury` .

<a name="not-operator"></a>

### <a name="not-operator--"></a>NOT işleci`-`

NOT işleci eksi işareti. Örneğin, `wifi –luxury` ve/veya olmayan belgeler için arama yapılır `wifi` `luxury` .

Sorgu isteğindeki **searchMode** PARAMETRESI, Not işleci olan bir terimin, sorgudaki diğer koşullara sahip olup olmadığını denetler ( `+` diğer koşullarda hiçbir veya işleci olmadığı varsayılarak `|` ). Geçerli değerler `any` veya içerir `all` .

`searchMode=any`sorgu geri çekmeyi daha fazla sonuç ekleyerek artırır ve varsayılan `-` olarak "veya Not" olarak yorumlanır. Örneğin, `wifi -luxury` terimi ya da terimi içermeyen belgelerle eşleşir `wifi` `luxury` .

`searchMode=all`sorguların hassasiyetini daha az sonuç ekleyerek artırır ve varsayılan olarak "ve NOT" olarak yorumlanır. Örneğin, `wifi -luxury` terimi içeren belgelerle eşleştirecektir `wifi` ve "merkezlerini" terimini içermemelidir. Bu, operatör için daha sezgisel bir davranış ile yapılır `-` . Bu nedenle, `searchMode=all` `searchMode=any` aramalarını geri çağırmak yerine duyarlık için optimize etmek istiyorsanız yerine kullanmanız gerekir *ve* kullanıcılarınız, `-` aramalardaki işleci sıklıkla kullanır.

Bir **searchMode** ayarı üzerinde karar verirken, çeşitli uygulamalardaki sorgular için Kullanıcı etkileşimi düzenlerini göz önünde bulundurun. Bilgi arayan kullanıcıların, daha fazla yerleşik gezinti yapılarına sahip olan e-ticaret sitelerinin aksine, bir sorguya işleç ekleme olasılığı yüksektir.

<a name="prefix-search"></a>

## <a name="wildcard-prefix-matching--"></a>Joker karakter eşleştirme (*,?)

"İle başlar" sorgularıyla, bir terimin geri kalanı için yer tutucu olarak bir sonek işleci ekleyin. `*`Birden çok karakter veya tek karakter için yıldız işareti kullanın `?` . Örneğin, `lingui*` "diltarihi" veya "Linguini" ile eşleşir, büyük/küçük harf gözardı edilir. 

Filtrelere benzer şekilde, ön ek sorgusu tam eşleşme arar. Bu nedenle, hiçbir ilgi Puanlama yoktur (tüm sonuçlar 1,0 arama puanı alır). Ön ek sorgularının yavaş, özellikle de dizin büyükse ve ön ek az sayıda karakterden oluşuyorsa emin olun. Edge n-gram simgeleştirme gibi alternatif bir metodolojide daha hızlı işlem yapabilirsiniz.

Sonek veya bir terimin sonuna veya ortasına karşı eşleştirme gibi diğer joker karakter çeşitleri için, [joker karakter arama için tam Lucene sözdizimini](query-lucene-syntax.md#bkmk_wildcard)kullanın.

## <a name="phrase-search-"></a>Tümcecik arama`"`

Terim arama bir veya daha fazla terim için, koşulların herhangi birinin eşleşme olarak kabul edildiği bir sorgudur. Tümcecik araması, tırnak işaretleri içine alınmış tam bir tümceciktir `" "` . Örneğin, `Roach Motel` (tırnak işareti olmadan) `Roach` herhangi bir sırada ve/veya herhangi bir yerde bulunan belgeleri arar `Motel` , `"Roach Motel"` (tırnak işareti içeren) yalnızca bu tümceciği içeren belgelerle ve bu sırayla eşleşir (sözcük temelli analiz hala geçerlidir).

## <a name="see-also"></a>Ayrıca bkz.  

+ [Azure Bilişsel Arama’da tam metin araması nasıl çalışır?](search-lucene-query-architecture.md)
+ [Basit arama için sorgu örnekleri](search-query-simple-examples.md)
+ [Tam Lucene Search için sorgu örnekleri](search-query-lucene-examples.md)
+ [Belgelerde Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Lucene sorgu söz dizimi](query-lucene-syntax.md)
+ [OData ifadesi söz dizimi](query-odata-filter-orderby-syntax.md) 
