---
title: Basit sorgu söz dizimi-Azure Search
description: Azure Search içindeki tam metin arama sorguları için kullanılan basit sorgu söz dizimi başvurusu.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 08/08/2019
author: brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
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
ms.openlocfilehash: 41a9c87731dcb6a2cb31e9120a0170b892c58b6f
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68884086"
---
# <a name="simple-query-syntax-in-azure-search"></a>Azure Search basit sorgu söz dizimi
Azure Search iki adet Lucene tabanlı sorgu dili uygular: [Basit sorgu ayrıştırıcısı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) ve [Lucene sorgu ayrıştırıcısı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Azure Search, basit sorgu söz dizimi, benzer/slop seçeneklerini dışlar.  

> [!NOTE]  
>  Azure Search daha karmaşık sorgular için alternatif bir [Lucene sorgu söz dizimi](query-lucene-syntax.md) sağlar. Her bir sözdiziminin sorgu ayrıştırma mimarisi ve avantajları hakkında daha fazla bilgi edinmek için, [tam metin aramasının Azure Search nasıl çalıştığını](search-lucene-query-architecture.md)görün.

## <a name="how-to-invoke-simple-parsing"></a>Basit ayrıştırma çağırma

Basit sözdizimi varsayılandır. Çağırma yalnızca sözdizimini tam değerinden basit olarak sıfırlarsanız gereklidir. Sözdizimini açıkça ayarlamak için `queryType` Search parametresini kullanın. Geçerli değerler `simple|full` `simple` , varsayılan olarak ve `full` Lucene için içerir. 

## <a name="query-behavior-anomalies"></a>Sorgu davranışı bozuklukları

Bir veya daha fazla terim içeren herhangi bir metin, sorgu yürütmesi için geçerli bir başlangıç noktası olarak kabul edilir. Azure Search, metnin çözümlenmesi sırasında bulunan tüm Çeşitlemeler dahil olmak üzere, koşulların herhangi birini veya tümünü içeren belgelerle eşleşir. 

Bu seslerin yanı sıra, giriş dizesine daha fazla terim ve işleç eklendikçe, arama sonuçlarını azaltmak yerine, Azure Search sorgu yürütmesinin bir yönü vardır. Bu genişletmenin gerçekten gerçekleşmediği, bir not işlecinin eklenmesine bağlıdır, ve ya da veya `searchMode` davranışları açısından nasıl yorumlanmadığını belirleyen bir parametre ayarıyla birleştirilir. Varsayılan, `searchMode=Any`, ve Not işleci verildiğinde, işlem, Seattle olmayan tüm şehirleri `"New York" NOT Seattle` döndüren bir veya eylem olarak hesaplanır.  

Genellikle, kullanıcıların daha fazla yerleşik gezinti yapılarına sahip olan e-ticaret sitelerinin aksine, içerik üzerinde arama yapan uygulamalar için Kullanıcı etkileşimi desenlerinde bu davranışları görmeniz daha olasıdır. Daha fazla bilgi için, bkz. [işleç](#not-operator). 

## <a name="boolean-operators-and-or-not"></a>Boole işleçleri (ve, veya DEĞIL) 

Eşleşen belgelerin bulunduğu bir dizi zengin ölçüt kümesi oluşturmak için işleçleri bir sorgu dizesine ekleyebilirsiniz. 

### <a name="and-operator-"></a>AND işleci`+`

AND işleci bir artı işareti. Örneğin, `wifi+luxury` hemhem`wifi` de içeren belgeler için arama yapılır. `luxury`

### <a name="or-operator-"></a>OR işleci`|`

OR işleci dikey bir çubuk veya boru karakterdir. Örneğin, `wifi | luxury` ya daya`wifi` da içerenbelgeleriarar.`luxury`

<a name="not-operator"></a>

### <a name="not-operator--"></a>NOT işleci`-`

NOT işleci eksi işareti. Örneğin, `wifi –luxury` ve/veya sahibi `luxury` olmayan (ve/ `wifi` veya denetimindeki `searchMode`) belgeleri aramak için arama yapılır.

> [!NOTE]  
>  Seçeneği, Not işleci olan bir terimin, `+` veya `|` işlecinin yokluğunda, sorgudaki diğer koşullara sahip olup olmadığını denetler. `searchMode` , (Varsayılan) veya `all`olarak ayarlanabilir. `any` `searchMode` Kullanırsanız `any`, daha fazla sonuç ekleyerek sorguların geri çağrılması artar ve varsayılan `-` olarak "veya Not" olarak yorumlanacaktır. Örneğin, `wifi -luxury` terimi `wifi` ya da terimi `luxury`içermeyen belgelerle eşleşir. Kullanırsanız `all`, daha az sonuç ekleyerek sorguların duyarlığını artırır ve varsayılan olarak "ve Not" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` içeren belgelerle eşleştirecektir ve "merkezlerini" terimini içermemelidir. Bu, `-` operatör için daha sezgisel bir davranış ile yapılır. Bu nedenle, aramalarını geri çağırmak `searchMode=all` yerine duyarlık `searchMode=any` için optimize etmek istiyorsanız yerine kullanmanız gerekir `-` *ve* kullanıcılarınız, aramalardaki işleci sıklıkla kullanır.

## <a name="suffix-operator"></a>Sonek işleci

Sonek işleci bir yıldız işaretidir `*`. Örneğin, `lux*` ile `lux`başlayan, büyük/küçük harf gözardı eden bir terimi olan belgeleri arar.  

## <a name="phrase-search-operator"></a>Tümcecik arama işleci

Tümcecik işleci, tırnak işaretleri `" "`içinde bir tümceciği barındırır. Örneğin, `Roach Motel` (tırnak işareti olmadan) herhangi bir sırada `Roach` ve/veya `Motel` herhangi bir yerde bulunan belgeleri arar, `"Roach Motel"` (tırnak işareti içeren) yalnızca bu tümceciği içeren belgelerle ve bu Düzen (metin analizi hala geçerlidir).

## <a name="precedence-operator"></a>Öncelik operatörü

Öncelik işleci, dizeyi parantez `( )`içinde barındırır. Örneğin, `motel+(wifi | luxury)` Motel terimi ve ya `luxury` da `wifi` (ya da her ikisi) içeren belgeleri arar.  

## <a name="escaping-search-operators"></a>Kaçış arama işleçleri  

 Yukarıdaki sembolleri, arama metninin gerçek parçası olarak kullanmak için, bir ters eğik çizgiyle önek olarak önüne alınmalıdır. Örneğin, `luxury\+hotel` terime `luxury+hotel`neden olur. Daha tipik durumlarda bu kurala göre basit hale getirmek için, bu kuralın kaçışın gerekli olmadığı iki özel durum vardır:  

- Not işlecinin `-` , bir terimin ortasında değil, boşluk 'tan sonra ilk karakter olması durumunda yalnızca kaçış olması gerekir. Örneğin, `wi-fi` tek bir terimdir; ancak GUID 'ler ( `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD`gibi) tek bir belirteç olarak değerlendirilir.
- Sonek işlecinin `*` , bir terimin ortasında değil, yalnızca boşluktan önceki son karakter olması durumunda kaçışması gerekir. Örneğin, `wi*fi` tek bir belirteç olarak değerlendirilir.

> [!NOTE]  
>  Kaçış belirteçleri birlikte tutar, ancak çözümleme moduna bağlı olarak metin analizi bunları bölebilir. Ayrıntılar için bkz. [dil &#40;desteği&#41; Azure Search hizmeti REST API](index-add-language-analyzers.md) .  

## <a name="see-also"></a>Ayrıca bkz.  

+ [Belgeleri &#40;Azure Search arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) 
+ [Lucene sorgu söz dizimi](query-lucene-syntax.md)
+ [OData ifadesi söz dizimi](query-odata-filter-orderby-syntax.md) 
