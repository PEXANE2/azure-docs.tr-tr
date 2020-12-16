---
title: Basit sorgu söz dizimi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'de tam metin arama sorguları için kullanılan basit sorgu söz dizimi başvurusu.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: f679d6fbab57bcbcccc09b722f6b2f670df49eb2
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97516580"
---
# <a name="simple-query-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama basit sorgu söz dizimi

Azure Bilişsel Arama iki adet Lucene tabanlı sorgu dili uygular: [basit sorgu ayrıştırıcısı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) ve [Lucene sorgu ayrıştırıcısı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html). Basit ayrıştırıcı daha esnektir ve kusursuz bir şekilde oluşturulmuş olmasa bile bir isteği yorumlamaya çalışır. Bu esneklik nedeniyle, Azure Bilişsel Arama 'de sorgular için varsayılan değer budur.

Basit sözdizimi, **`search`** bir [arama belgeleri (REST API)](/rest/api/searchservice/search-documents) isteğinin parametresinde geçirilen sorgu ifadeleri için kullanılır, aynı istekteki ve ifadelerinde kullanılan [OData sözdizimiyle](query-odata-filter-orderby-syntax.md) karıştırılmamalıdır [**`$filter`**](search-filters.md) [**`$orderby`**](search-query-odata-orderby.md) . OData parametrelerinin sorgu oluşturma, kaçış dizeleri vb. için farklı sözdizimi ve kuralları vardır.

Basit ayrıştırıcı [Apache Lucene basit sorgu ayrıştırıcı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/simple/SimpleQueryParser.html) sınıfına dayansa da, bilişsel arama uygulamasında uygulama belirsiz aramayı dışlar. [Benzer arama](search-query-fuzzy.md)gerekiyorsa bunun yerine alternatif [tam Lucene sorgu söz dizimini](query-lucene-syntax.md) göz önünde bulundurun.

## <a name="example-simple-syntax"></a>Örnek (basit sözdizimi)

**`queryType`**, Aşağıdaki gibi ayarlanmış olsa da, alternatif bir türden geri döndürmediğiniz takdirde varsayılan değer atlanabilir. Aşağıdaki örnek, tüm eşleşen belgelerin "havuz" içermesi gereksinimini içeren bağımsız koşullara göre arama örneğidir.

```http
POST https://{{service-name}}.search.windows.net/indexes/hotel-rooms-sample/docs/search?api-version=2020-06-30
{
  "queryType": "simple",
  "search": "budget hotel +pool",
  "searchMode": "all"
}
```

**`searchMode`** Parametresi bu örnekle ilgilidir. Sorguda Boole işleçleri olduğunda, genellikle `searchMode=all` ölçütlerin *tümünün* eşleştiğinden emin olmak için ayarlamanız gerekir. Aksi takdirde, `searchMode=any` duyarlık üzerinde geri çekmeyi tercih eden varsayılan değer de kullanabilirsiniz.

Daha fazla örnek için bkz. [basit sorgu söz dizimi örnekleri](search-query-simple-examples.md). Sorgu isteği ve parametreleri hakkında daha fazla bilgi için bkz. [arama belgeleri (REST API)](/rest/api/searchservice/Search-Documents).

## <a name="keyword-search-on-terms-and-phrases"></a>Hüküm ve tümceciklerinde anahtar sözcük arama

Parametreye geçirilen dizeler **`search`** , desteklenen herhangi bir dilde terimleri veya tümceleri, Boole işleçlerini, öncelik işleçlerini, joker karakter veya önek karakterlerini içerebilir "sorgular, kaçış karakterleri ve URL kodlama karakterleri olabilir. **`search`** Parametresi isteğe bağlıdır. Belirtilmemiş, ara ( `search=*` veya `search=" "` ), ilk 50 belgeyi rastgele (derecelendirildi) sırada döndürür.

+ *Terim arama* bir veya daha fazla terim sorgusuyla, koşulların herhangi birinde eşleşme olduğu kabul edilir.

+ *Tümcecik araması* , tırnak işaretleri içine alınmış tam bir tümceciktir `" "` . Örneğin, ```Roach Motel``` (tırnak işareti olmadan) ```Roach``` herhangi bir sırada ve/veya herhangi bir yerde bulunan belgeleri arar ```Motel``` , ```"Roach Motel"``` (tırnak işareti içeren) yalnızca bu tümceciği içeren belgelerle ve bu sırayla eşleşir (sözcük temelli analiz hala geçerlidir). 

  Arama istemcinizden bağlı olarak, tümcecik aramasında tırnak işaretlerine kaçış yapmanız gerekebilir. Örneğin, bir POST isteğindeki Postman 'da, istek gövdesinde açık bir ifade arama `"Roach Motel"` olarak belirtilir `"\"Roach Motel\""` .

Varsayılan olarak, parametreye geçirilen tüm hüküm veya tümcecikler, **`search`** sözlü Analize sahiptir. Kullanmakta olduğunuz çözümleyicinin simgeleştirme davranışını anladığınızdan emin olun. Genellikle sorgu sonuçları beklenmiyorsa, nedeni, sorgu zamanında terimlerin simgeleştirilmiş olarak izlenebilir.

Bir veya daha fazla terim içeren herhangi bir metin, sorgu yürütmesi için geçerli bir başlangıç noktası olarak kabul edilir. Azure Bilişsel Arama, metnin çözümlenmesi sırasında bulunan tüm Çeşitlemeler dahil olmak üzere, koşulların herhangi birini veya tümünü içeren belgelerle eşleşir.

Bu seslerin yanı sıra, Azure Bilişsel Arama sorgu yürütmesinin bir yönü vardır. Bu işlem, giriş dizesine daha fazla terim ve işleç eklendikçe, arama sonuçlarını azaltmak yerine artan sonuçlara neden *olabilir* . Bu genişletmenin gerçekten gerçekleşmediği, bir NOT işlecinin eklenmesine bağlıdır, ve ya da veya **`searchMode`** davranışları açısından nasıl yorumlanmadığını belirleyen bir parametre ayarıyla birleştirilir. Daha fazla bilgi için bkz. [Boolean işleçleri](#boolean-operators)altında not işleci.

## <a name="boolean-operators"></a>Boole işleçleri

Bir eşleşmenin hassasiyetini artırmak için bir sorgu dizesine Boole işleçleri ekleyebilirsiniz. Basit sözdiziminde, Boole işleçleri karakter tabanlıdır. Word ve gibi metin işleçleri desteklenmez.

| Karakter | Örnek | Kullanım |
|----------- |--------|-------|
| `+` | `pool + ocean` | VE işlemi. Örneğin, `pool + ocean` bir belgenin her iki terimi de içermesi gerektiğini spucu.|
| `|` | `pool | ocean` | YA da bir işlem, her iki terim bulunduğunda bir eşleşme bulur. Örnekte, sorgu altyapısı veya ya da her ikisini içeren belgelerde eşleşme döndürür `pool` `ocean` . YA da varsayılan bir bağlantılı operatör olduğundan, buna eşdeğer olan gibi da bırakabilirsiniz `pool ocean`  `pool | ocean` .|
| `-` | `pool – ocean` | NOT işlemi, terimi hariç tutmayan belgelerde eşleşmeler döndürüyor. <br/><br/>NOT ifadesinde beklenen davranışı almak için, istek üzerine ayarlamayı göz önünde bulundurun **`searchMode=all`** . Aksi takdirde, varsayılan ' ın altında, ve ' ın **`searchMode=any`** `pool` içermediği tüm belgeler üzerinde eşleşmeler elde edersiniz, bu da `ocean` çok fazla belge olabilir. **`searchMode`** Bir sorgu isteğindeki parametresi, Not işleci olan bir terimin, sorgudaki diğer koşullara sahip veya ORed olup olmadığını denetler ( `+` diğer koşullarda hiçbir veya işleci olmadığı varsayılarak `|` ). Kullanımı **`searchMode=all`** , sorguların hassasiyetini daha az sonuç ekleyerek artırır ve varsayılan olarak "ve Not" olarak yorumlanır. <br/><br/>Bir ayar üzerinde karar verirken **`searchMode`** , çeşitli uygulamalardaki sorgular için Kullanıcı etkileşimi düzenlerini göz önünde bulundurun. Bilgi arayan kullanıcıların, daha fazla yerleşik gezinti yapılarına sahip olan e-ticaret sitelerinin aksine, bir sorguya işleç ekleme olasılığı yüksektir. |

<a name="prefix-search"></a>

## <a name="prefix-queries"></a>Ön ek sorguları

"İle başlar" sorgularıyla, `*` bir terimin geri kalanı için yer tutucu olarak bir sonek işleci () ekleyin. Sonek işlecini ekleyebilmeniz için ön ek sorgusunun en az bir alfasayısal karakterle başlaması gerekir.

| Karakter | Örnek | Kullanım |
|----------- |--------|-------|
| `*` | `lingui*` "dilsel" veya "Linguini" ile eşleşir | Yıldız işareti ( `*` ) rastgele uzunluktaki bir veya daha fazla karakteri temsil eder, büyük/küçük harf yok sayılıyor.  |

Filtrelere benzer şekilde, ön ek sorgusu tam eşleşme arar. Bu nedenle, hiçbir ilgi Puanlama yoktur (tüm sonuçlar 1,0 arama puanı alır). Ön ek sorgularının yavaş, özellikle de dizin büyükse ve ön ek az sayıda karakterden oluşuyorsa emin olun. Edge n-gram simgeleştirme gibi alternatif bir metodolojide daha hızlı işlem yapabilirsiniz.

Basit sözdizimi yalnızca önek eşleştirmeyi destekler. Bir terimin sonuna veya ortasına karşılık gelen sonek veya düzeltilmesi için, [joker karakter araması için tam Lucene sözdizimini](query-lucene-syntax.md#bkmk_wildcard)kullanın.

## <a name="escaping-search-operators"></a>Kaçış arama işleçleri  

Basit sözdiziminde, arama işleçleri şu karakterleri içerir: `+ | " ( ) ' \`  

Bu karakterlerden herhangi biri dizindeki bir belirtecin parçasıysa, sorguda tek bir ters eğik çizgi () ekleyerek onu kaçış `\` . Örneğin, tüm terim simgeleştirme için özel bir çözümleyici kullandığınızı varsayın ve dizininiz "merkezlerini + otel" dizesini içerir. Bu belirteçle tam eşleşme almak için bir kaçış karakteri ekleyin: `search=luxury\+hotel` .

Daha tipik durumlar için şeyleri basit hale getirmek amacıyla, bu kural için kaçışın gerekli olmadığı iki özel durum vardır:  

+ NOT işlecinin `-` yalnızca bir boşluktan sonraki ilk karakter olması durumunda kaçış olması gerekir. `-`Ortasında görünürse (örneğin, içinde `3352CDD0-EF30-4A2E-A512-3B30AF40F3FD` ), kaçış işlemini atlayabilirsiniz.

+ Sonek işlecinin `*` yalnızca bir boşluktan önceki son karakter olması durumunda kaçışması gerekir. `*`Ortasında görünürse (örneğin, içinde `4*4=16` ), kaçış gerekmez.

> [!NOTE]  
> Standart çözümleyici, varsayılan olarak, sözcük [temelli analiz](search-lucene-query-architecture.md#stage-2-lexical-analysis)sırasında kısa çizgilerden, boşluklardan, ve diğer karakterlere ait sözcükleri siler ve keser. Sorgu dizesinde kalması için özel karakterler gerekiyorsa, bunları dizinde koruyan bir çözümleyici gerekebilir. Bazı seçimler, hecelenmiş kelimeleri koruyan Microsoft doğal [dil Çözümleyicileri](index-add-language-analyzers.md)veya daha karmaşık desenler için özel bir çözümleyici içerir. Daha fazla bilgi için bkz. [kısmi terimler, desenler ve özel karakterler](search-query-partial-matching.md).

## <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>URL 'lerde güvenli olmayan ve ayrılmış karakterleri kodlama

Tüm güvenli olmayan ve ayrılmış karakterlerin bir URL 'de kodlandığını doğrulayın. Örneğin, URL 'deki bir parça/bağlantı tanımlayıcısı olduğundan, ' # ' güvenli olmayan bir karakterdir. URL 'de kullanılıyorsa, karakterin kodlanmalıdır `%23` . ' & ' ve ' = ', parametreleri sınırlandıran ve Azure Bilişsel Arama değerlerini belirten ayrılmış karakter örnekleridir. Daha fazla bilgi için bkz. [rfc1738: Uniform Resource Konumlandırıcıları (URL)](https://www.ietf.org/rfc/rfc1738.txt).

Güvenli olmayan karakterler ``" ` < > # % { } | \ ^ ~ [ ]`` . Ayrılan karakterler şunlardır `; / ? : @ = + &` .

## <a name="special-characters"></a>Özel karakterler

Bazı durumlarda, bir ' ❤ ' emoji veya ' € ' işareti gibi özel bir karakter aramak isteyebilirsiniz. Bu gibi durumlarda, kullandığınız çözümleyicinin bu karakterleri filtrelemez olduğundan emin olun. Standart çözümleyici, dizininizden hariç olmak üzere birçok özel karakteri atlar.

Özel karakterleri simgeleştirmek için kullanılan çözümleyiciler "Whitespace" çözümleyicisini içerir. Bu, "❤" dizesi belirteç olarak kabul edilir. Ayrıca, Microsoft Ingilizce Çözümleyicisi ("en. Microsoft") gibi bir dil Çözümleyicisi, "€" dizesini belirteç olarak alır. Belirli bir sorgu için ürettiği belirteçleri görmek üzere [bir Çözümleyicisi test](/rest/api/searchservice/test-analyzer) edebilirsiniz.

Unicode karakterler kullanılırken, simgenin sorgu URL 'sinde doğru bir şekilde atlanacağından emin olun (örneğin, "❤" için çıkış sırasını kullanır `%E2%9D%A4+` ). Postman bu çeviriyi otomatik olarak yapar.  

## <a name="precedence-grouping"></a>Öncelik (gruplama)

Parantez, parantez içinde işleç dahil olmak üzere alt sorgular oluşturmak için kullanabilirsiniz. Örneğin, `motel+(wifi|luxury)` "Motel" terimini ve "WiFi" veya "merkezlerini" (ya da her ikisi) içeren belgeleri arar.

## <a name="query-size-limits"></a>Sorgu boyutu sınırları

Uygulamanız program aracılığıyla arama sorguları oluşturursa, bu şekilde, sınırsız boyut sorguları oluşturmamasını sağlayan bir şekilde tasarlamayı öneririz. 

+ GET için, URL 'nin uzunluğu 8 KB 'yi aşamaz.

+ İstek gövdesinin ve gibi diğer parametrelerin bulunduğu POST (ve diğer tüm istekler) için `search` `filter` `orderby` en büyük boyut 16 MB 'tır. burada, içindeki en fazla sayıda yan tümce `search` (ve ile ayrılmış ifadeler) 1024 ' dir. Ayrıca, bir sorgudaki her bir terimin boyutunda yaklaşık 32 KB 'lik bir sınır vardır. Daha fazla bilgi için bkz. [API isteği sınırları](search-limits-quotas-capacity.md#api-request-limits).

## <a name="next-steps"></a>Sonraki adımlar

Sorgu işleme aşamalarını ve metin analizinin etkilerini anlamak için [Azure bilişsel arama 'de tam metin aramasını](search-lucene-query-architecture.md) gözden geçirin.

Sorgu oluşturma hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri de inceleyebilirsiniz:

+ [Basit arama için sorgu örnekleri](search-query-simple-examples.md)
+ [Tam Lucene Search için sorgu örnekleri](search-query-lucene-examples.md)
+ [Belgelerde Arama REST API'si](/rest/api/searchservice/Search-Documents)
+ [Lucene sorgu söz dizimi](query-lucene-syntax.md)
+ [Filtre Uygula ve Seç (OData) ifade sözdizimi](query-odata-filter-orderby-syntax.md)