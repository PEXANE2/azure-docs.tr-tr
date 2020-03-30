---
title: Basit sorgu söz dizimi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'da tam metin arama sorguları için kullanılan basit sorgu sözdizimine başvuru.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/10/2020
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: fc1eb1836badc3ced688750bbc7c7a164773d022
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152678"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da basit sorgu sözdizimi

Azure Bilişsel Arama iki Lucene tabanlı sorgu dili uygular: [Basit Sorgu Parser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) ve [Lucene Query Parser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Azure Bilişsel Arama'da, basit sorgu sözdizimi bulanık/slop seçeneklerini dışlar.  

> [!NOTE]
> Basit sorgu sözdizimi, [Arama Belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents) API'sinin **arama** parametresi içinde geçirilen sorgu ifadeleri için kullanılır ve bu API'nin [$filter](search-filters.md) parametresi için kullanılan [OData sözdizimi](query-odata-filter-orderby-syntax.md) ile karıştırılmamalıdır. Bu farklı sözdizimilerin sorgu oluşturmak, dizeleri kaçmak ve benzeri için kendi kuralları vardır.
>
> Azure Bilişsel Arama, **arama** parametresi'ndeki daha karmaşık sorgular için alternatif bir [tam Lucene sorgu sözdizimi](query-lucene-syntax.md) sağlar. Sorgu ayrıştırma mimarisi ve her sözdiziminin yararları hakkında daha fazla bilgi edinmek için Azure [Bilişsel Arama'da tam metin aramanın nasıl çalıştığını](search-lucene-query-architecture.md)görün.

## <a name="how-to-invoke-simple-parsing"></a>Basit ayrıştırma nasıl çağrılabilir?

Basit sözdizimi varsayılandır. Çağırma yalnızca sözdizimini tamdan basite sıfırlıyorsanız gereklidir. Sözdizimini açıkça ayarlamak için `queryType` arama parametresini kullanın. Geçerli değerler, `simple` varsayılan olarak ve `full` Lucene için içerir. `simple|full` 

## <a name="query-behavior-anomalies"></a>Davranış anormalliklerini sorgula

Bir veya daha fazla terime sahip herhangi bir metin, sorgu yürütmesi için geçerli bir başlangıç noktası olarak kabul edilir. Azure Bilişsel Arama, metnin analizi sırasında bulunan varyasyonlar da dahil olmak üzere terimlerin herhangi birini veya tümününi içeren belgeleri eşler. 

Bu kulağa ne kadar basit gelse de, Azure Bilişsel Arama'da sorgu yürütmenin beklenmeyen *sonuçlara* yol açabilecek bir yönü vardır ve giriş dizesine daha fazla terim ve işleç eklendikçe arama sonuçlarını azaltmak yerine artabilir. Bu genişletmenin gerçekten gerçekleşip gerçekleşmeyeceği, NOT işlecinin `searchMode` eklenmesine ve NOT'un VE veya OR davranışları açısından nasıl yorumlandığını belirleyen bir parametre ayarı ile birleştirilmesine bağlıdır. Varsayılan `searchMode=Any`ve NOT işleci göz önüne alındığında, işlem Seattle olmayan tüm `"New York" NOT Seattle` şehirleri döndüren bir OR eylemi olarak hesaplanır.  

Tipik olarak, daha yerleşik gezinme yapılarına sahip e-ticaret sitelerinin aksine, kullanıcıların bir sorguya bir operatör ekleme olasılığının daha yüksek olduğu, içerik üzerinde arama yapan uygulamalar için kullanıcı etkileşim ibdesi bu davranışları görme olasılığınız daha yüksektir. Daha fazla bilgi için [NOT işlecine](#not-operator)bakın. 

## <a name="boolean-operators-and-or-not"></a>Boolean operatörleri (VE, VEYA, Değİl) 

Eşleşen belgelerin bulunduğu zengin bir ölçüt kümesi oluşturmak için işleçleri sorgu dizesi'ne gömebilirsiniz. 

### <a name="and-operator-"></a>VE operatörü`+`

AND işleci bir artı işaretidir. Örneğin, `wifi+luxury` her ikisini ve `wifi` `luxury`.'yi içeren belgeleri arar.

### <a name="or-operator-"></a>VEYA operatörü`|`

OR işleci dikey bir çubuk veya boru karakteridir. Örneğin, `wifi | luxury` her ikisini `wifi` veya `luxury` her ikisini içeren belgeleri arar.

<a name="not-operator"></a>

### <a name="not-operator--"></a>NOT operatörü`-`

NOT işleci eksi işaretidir. Örneğin, `wifi –luxury` `wifi` terimi olan ve/veya olmayan `luxury` (ve/veya `searchMode`denetlenmeyen) belgeler aranır.

> [!NOTE]  
>  Seçenek, `searchMode` NOT işleci ile bir terimin ANDed veya ORed sorguda bir `+` veya `|` işleç yokluğunda diğer terimler ile olup olmadığını denetler. (varsayılan) veya `searchMode` `any` `all`. olarak ayarlanabilen geri çağırma Kullanırsanız, `any`daha fazla sonuç ekleyerek sorguların geri çağrılması `-` artar ve varsayılan olarak "VEYA Değİl" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` içeren veya terimi `luxury`içermeyen belgelereşleşir. Kullanırsanız, `all`daha az sonuç ekleyerek sorguların kesinliğini artırır ve varsayılan olarak "VE Değİl" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` içeren ve "lüks" terimini içermeyen belgeler eşleşir. Bu operatör için `-` tartışmasız daha sezgisel bir davranıştır. Bu nedenle, aramaları `searchMode=all` geri `searchMode=any` çağırmak yerine kesinlik için optimize etmek *and* istiyorsanız kullanmayı düşünmelisiniz `-` ve kullanıcılarınız aramalarda operatörü sık sık kullanır.

## <a name="suffix-operator"></a>Sonek işleci

Sonek işleci bir yıldız `*`işaretidir. Örneğin, `lux*` "büyük/küçük/ yoksayma" `lux`ile başlayan bir terimi olan belgeleri arar.  

## <a name="phrase-search-operator"></a>İfade arama operatörü

Tümcecik işleci, bir tümceciği tırnak işaretlerine `" "`eler. Örneğin, (tırnak işaretleri olmadan) herhangi `Roach` bir sırada `Motel` ve/veya `"Roach Motel"` herhangi bir yerde bulunan belgeleri ararken, `Roach Motel` (tırnak işaretleriyle) yalnızca tüm tüm ifadeyi birlikte ve bu sırada içeren belgelerle eşleşir (metin çözümlemesi hala geçerlidir).

## <a name="precedence-operator"></a>Öncelik işleci

Öncelik işleci dizeyi parantez içine `( )`alar. Örneğin, `motel+(wifi | luxury)` motel terimini ve ya `wifi` veya `luxury` (veya her ikisini) içeren belgeleri arayacaktır.  

## <a name="escaping-search-operators"></a>Arama operatörlerinden kaçış  

 Yukarıdaki sembolleri arama metninin gerçek bir parçası olarak kullanabilmek için, bir ters eğik çizgi ile önseerek kaçmış olmalıdır. Örneğin, `luxury\+hotel` terim `luxury+hotel`neden olur. İşleri daha tipik durumlar için basitleştirmek için, bu kuralın kaçışın gerekli olmadığı iki istisna vardır:  

- NOT işlecinin `-` yalnızca bir dönemin ortasında ysa, beyazuzaydan sonraki ilk karakter olması durumunda kaçması gerekir. Örneğin, `wi-fi` tek bir terimdir; oysa GUID'ler (örneğin) `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`tek bir belirteç olarak kabul edilir.
- Sonek işlecinin `*` yalnızca beyaz uzaydan önceki son karakter olması durumunda, bir terimin ortasında ysa kaçılması gerekir. Örneğin, `wi*fi` tek bir belirteç olarak kabul edilir.

> [!NOTE]  
>  Kaçan belirteçleri bir arada tutsa da, metin çözümlemesi çözümleme moduna bağlı olarak onları bölebilir. Ayrıntılar için [Azure Bilişsel Arama REST API &#40;&#41;Dil desteğine](index-add-language-analyzers.md) bakın.  

## <a name="see-also"></a>Ayrıca bkz.  

+ [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 
+ [Lucene sorgu söz dizimi](query-lucene-syntax.md)
+ [OData ifadesi söz dizimi](query-odata-filter-orderby-syntax.md) 
