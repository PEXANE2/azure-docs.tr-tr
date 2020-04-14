---
title: Basit sorgu söz dizimi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da tam metin arama sorguları için kullanılan basit sorgu sözdizimine başvuru.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/12/2020
ms.openlocfilehash: 066190ff6b735d30db351ff90c0b6e5173b7f583
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81258873"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da basit sorgu sözdizimi

Azure Bilişsel Arama iki Lucene tabanlı sorgu dili uygular: [Basit Sorgu Parser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) ve [Lucene Query Parser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). 

Azure Bilişsel Arama'da, basit sorgu sözdizimi bulanık arama işlemlerini dışlar. Bunun yerine, [bulanık arama](search-query-fuzzy.md)için tam Lucene sözdizimini kullanın.

> [!NOTE]
> Basit sorgu sözdizimi, [Arama Belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents) API'sinin **arama** parametresi içinde geçirilen sorgu ifadeleri için kullanılır ve bu API'nin [$filter](search-filters.md) parametresi için kullanılan [OData sözdizimi](query-odata-filter-orderby-syntax.md) ile karıştırılmamalıdır. Bu farklı sözdizimilerin sorgu oluşturmak, dizeleri kaçmak ve benzeri için kendi kuralları vardır.
>
> Azure Bilişsel Arama, **arama** parametresi'ndeki daha karmaşık sorgular için alternatif bir [tam Lucene sorgu sözdizimi](query-lucene-syntax.md) sağlar. Sorgu ayrıştırma mimarisi ve her sözdiziminin yararları hakkında daha fazla bilgi edinmek için Azure [Bilişsel Arama'da tam metin aramanın nasıl çalıştığını](search-lucene-query-architecture.md)görün.

## <a name="invoke-simple-parsing"></a>Basit ayrıştırma çağırma

Basit sözdizimi varsayılandır. Çağırma yalnızca sözdizimini tamdan basite sıfırlıyorsanız gereklidir. Sözdizimini açıkça ayarlamak için `queryType` arama parametresini kullanın. Geçerli değerler `queryType=simple` `queryType=full`içerir `simple` veya, varsayılan `full` nerede ve daha gelişmiş sorgular için [tam Lucene sorgu arayıcısı](query-lucene-syntax.md) çağırır. 

## <a name="syntax-fundamentals"></a>Sözdizimi temelleri

Bir veya daha fazla terime sahip herhangi bir metin, sorgu yürütmesi için geçerli bir başlangıç noktası olarak kabul edilir. Azure Bilişsel Arama, metnin analizi sırasında bulunan varyasyonlar da dahil olmak üzere terimlerin herhangi birini veya tümününi içeren belgeleri eşler.

Bu kulağa ne kadar basit gelse de, Azure Bilişsel Arama'da sorgu yürütmenin beklenmeyen *sonuçlara* yol açabilecek bir yönü vardır ve giriş dizesine daha fazla terim ve işleç eklendikçe arama sonuçlarını azaltmak yerine artabilir. Bu genişletmenin gerçekten gerçekleşip gerçekleşmeyeceği, NOT işlecinin dahil edilmesine ve NOT'un VE veya OR davranışları açısından nasıl yorumlandığını belirleyen bir **searchMode** parametre ayarı ile birleştirilmesine bağlıdır. Daha fazla bilgi için [NOT işlecine](#not-operator)bakın.

### <a name="precedence-operators-grouping"></a>Öncelik işleçleri (gruplandırma)

Parantez içindeki işleçler de dahil olmak üzere alt sorgular oluşturmak için parantez ler kullanabilirsiniz. Örneğin, `motel+(wifi||luxury)` "motel" terimini ve "wifi" veya "lüks" (veya her ikisini) içeren belgeleri arayacaktır.

Alan gruplandırması benzerdir, ancak gruplandırmayı tek bir alana doğru kapsamlıdır. Örneğin, `hotelAmenities:(gym+(wifi||pool))` "gym" ve "wifi" veya "gym" ve "pool" için "hotelAmenities" alanını arar.  

### <a name="escaping-search-operators"></a>Arama operatörlerinden kaçış  

Arama işleçlerinden herhangi birini arama metninin bir parçası olarak kullanmak için, karakteri tek`\`bir ters eğik çizgi ile öne atarak kaçmak ( ). Örneğin, sorgu dizesinin `https://`bir `://` parçası olan joker karakter araması `search=https\:\/\/*`için . Benzer şekilde, kaçan bir telefon numarası `\+1 \(800\) 642\-7676`deseni de buna benzer.

Kaçmayı gerektiren özel karakterler şunlardır:`- * ? \ /`  

İşleri daha tipik durumlar için basitleştirmek için, bu kuralın kaçışın gerekli olmadığı iki istisna vardır:  

+ NOT işlecinin `-` yalnızca bir dönemin ortasında ysa, beyazuzaydan sonraki ilk karakter olması durumunda kaçması gerekir. Örneğin, aşağıdaki GUID kaçış karakteri olmadan `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`geçerlidir: .

+ Sonek işlecinin `*` yalnızca beyaz uzaydan önceki son karakter olması durumunda, bir terimin ortasında ysa kaçılması gerekir. Örneğin, `4*4=16` bir ters çizgi gerektirmez.

> [!NOTE]  
> Kaçan belirteçleri bir arada tutsa da, dizin oluşturma sırasında [yapılan sözlü analizler](search-lucene-query-architecture.md#stage-2-lexical-analysis) onları silebilir. Örneğin, standart Lucene çözümleyicisi tire, boşluk ve diğer karakterlerdeki sözcükleri siler ve kırar. Sorgu dizesinde özel karakterlere ihtiyacınız varsa, bunları dizinde koruyan bir çözümleyici gerekebilir. Bazı seçenekler arasında tireli sözcükleri koruyan Microsoft doğal [dil çözümleyicileri](index-add-language-analyzers.md)veya daha karmaşık desenler için özel bir çözümleyici yer almaktadır. Daha fazla bilgi için [Kısmi terimlere, desenlere ve özel karakterlere](search-query-partial-matching.md)bakın.

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>URL'lerde güvenli olmayan ve ayrılmış karakterleri kodlama

Lütfen tüm güvenli olmayan ve rezerve edilmiş karakterlerin bir URL'de şifrelendirildiğından emin olun. Örneğin, '#' güvenli olmayan bir karakterdir, çünkü url'deki bir parça/bağlantı tanımlayıcısıdır. Bir URL'de `%23` kullanılıyorsa karakter kodlanmalıdır. '&' ve '=' parametreleri sınırlandırDıkları ve Azure Bilişsel Arama'da değerleri belirtirken ayrılmış karakterlere örnektir. Daha fazla bilgi için lütfen [RFC1738: Uniform Resource Locators (URL)](https://www.ietf.org/rfc/rfc1738.txt) adresine bakın.

Güvensiz karakterler. ``" ` < > # % { } | \ ^ ~ [ ]`` Ayrılmış karakterler `; / ? : @ = + &`.

###  <a name="query-size-limits"></a><a name="bkmk_querysizelimits"></a>Sorgu boyutu sınırları

 Azure Bilişsel Arama'ya gönderebileceğiniz sorguların boyutunda bir sınır vardır. Özellikle, en fazla 1024 yan tümcesi (VE, OR vb. tarafından ayrılmış ifadeler) olabilir. Ayrıca bir sorguda herhangi bir tek terimin boyutu yaklaşık 32 KB sınırı vardır. Uygulamanız arama sorgularını programlı bir şekilde oluşturuyorsa, bu sorguyu sınırsız boyutta sorgu oluşturmayacak şekilde tasarlamanızı öneririz.  

## <a name="boolean-search"></a>Boolean arama

Eşleşen belgelerin bulunduğu zengin bir ölçüt kümesi oluşturmak için Boolean işleçlerini (VE VEYA, DEĞİL) sorgu dizesine gömebilirsiniz. 

### <a name="and-operator-"></a>VE operatörü`+`

AND işleci bir artı işaretidir. Örneğin, `wifi+luxury` her ikisini ve `wifi` `luxury`.'yi içeren belgeleri arar.

### <a name="or-operator-"></a>VEYA operatörü`|`

OR işleci dikey bir çubuk veya boru karakteridir. Örneğin, `wifi | luxury` her ikisini `wifi` veya `luxury` her ikisini içeren belgeleri arar.

<a name="not-operator"></a>

### <a name="not-operator--"></a>NOT operatörü`-`

NOT işleci eksi işaretidir. Örneğin, `wifi –luxury` `wifi` terimi olan ve/veya ' ı olmayan `luxury`belgeleri arayacaktır.

Sorgu isteğindeki **searchMode** parametresi, NOT işleciyle birlikte bir terimin sorgudaki diğer terimlerle birlikte `+` `|` ANDed veya ORed olup olmadığını denetler (diğer terimlerde operatör veya işleç olmadığını varsayarsak). Geçerli değerler `any` `all`içerir veya .

`searchMode=any`daha fazla sonuç ekleyerek sorguların geri çağrılması artar ve varsayılan olarak `-` "VEYA Değİl" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` içeren veya terimi `luxury`içermeyen belgelereşleşir.

`searchMode=all`daha az sonuç ekleyerek sorguların kesinliğini artırır ve varsayılan olarak "VE Değİl" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` içeren ve "lüks" terimini içermeyen belgeler eşleşir. Bu operatör için `-` tartışmasız daha sezgisel bir davranıştır. Bu nedenle, aramaları `searchMode=all` geri `searchMode=any` çağırmak yerine kesinlik için optimize etmek *and* istiyorsanız kullanmayı düşünmelisiniz `-` ve kullanıcılarınız aramalarda operatörü sık sık kullanır.

**SearchMode** ayarına karar verirken, çeşitli uygulamalardaki sorgular için kullanıcı etkileşim ideleşİm modellerini göz önünde bulundurun. Daha fazla yerleşik navigasyon yapısına sahip e-ticaret sitelerinin aksine, bilgi arayan kullanıcıların bir sorguya bir operatör ekleme olasılığı daha yüksektir.

<a name="prefix-search"></a>

## <a name="prefix-search"></a>Önek arama

Sonek işleci bir yıldız `*`işaretidir. Örneğin, `lingui*` "dilsel" veya "linguini" bulacaksınız, durum göz ardı. 

Filtrelere benzer şekilde, önek sorgusu tam bir eşleşme arar. Bu nedenle, alaka düzeyi puanlama yoktur (tüm sonuçlar 1.0 arama puanı alır). Önek sorguları, özellikle dizin büyükse ve önek az sayıda karakterden oluşuyorsa, yavaş olabilir. 

Dize son bölümünde eşleşen bir sonek sorgusu yürütmek istiyorsanız, bir [joker karakter arama](query-lucene-syntax.md#bkmk_wildcard) ve tam Lucene sözdizimi kullanın.

## <a name="phrase-search-"></a>İfade arama`"`

Terim araması, terimlerden herhangi birinin eşleştiği bir veya daha fazla terimiçin yapılan bir sorgudur. Tümcecik araması, tırnak işaretlerine `" "`ekteki tam bir ifadedir. Örneğin, (tırnak işaretleri olmadan) herhangi `Roach` bir sırada `Motel` ve/veya `"Roach Motel"` herhangi bir yerde bulunan belgeleri ararken, `Roach Motel` (tırnak işaretleriyle) yalnızca tüm tüm ifadeyi birlikte ve bu sırada içeren belgelerle eşleşir (metin çözümlemesi hala geçerlidir).

## <a name="see-also"></a>Ayrıca bkz.  

+ [Basit arama için sorgu örnekleri](search-query-simple-examples.md)
+ [Tam Lucene araması için sorgu örnekleri](search-query-lucene-examples.md)
+ [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Lucene sorgu söz dizimi](query-lucene-syntax.md)
+ [OData ifadesi söz dizimi](query-odata-filter-orderby-syntax.md) 
