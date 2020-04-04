---
title: Lucene sorgu söz dizimi
titleSuffix: Azure Cognitive Search
description: Joker karakter, bulanık arama, RegEx ve diğer gelişmiş sorgu yapıları için Azure Bilişsel Arama'da kullanılan tam Lucene sorgu sözdizimi için başvuru.
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
ms.openlocfilehash: 1392f69bea09996e46ad4c112474f9067ff5a63d
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656907"
---
# <a name="lucene-query-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da Lucene sorgu sözdizimi

Özelleştirilmiş sorgu formları için zengin [Lucene Query Parser](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) sözdizimine dayalı olarak Azure Bilişsel Arama'ya karşı sorgular yazabilirsiniz: joker karakter, bulanık arama, yakınlık araması, normal ifadeler birkaç örnektir. Lucene Query Parser sözdiziminin çoğu, ifadeler aracılığıyla `$filter` Azure Bilişsel Arama'da yapılan aralık *aramaları* dışında Azure Bilişsel [Arama'da bozulmadan uygulanır.](search-lucene-query-architecture.md) 

> [!NOTE]
> Tam Lucene sözdizimi, [Arama Belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents) API'sinin **arama** parametresi içinde geçirilen sorgu ifadeleri için kullanılır ve bu API'nin [$filter](search-filters.md) parametresi için kullanılan [OData sözdizimi](query-odata-filter-orderby-syntax.md) ile karıştırılmamalıdır. Bu farklı sözdizimilerin sorgu oluşturmak, dizeleri kaçmak ve benzeri için kendi kuralları vardır.

## <a name="how-to-invoke-full-parsing"></a>Tam ayrıştırma nasıl çağrılabilir?

Hangi `queryType` parser'ın kullanılacağını belirtmek için arama parametresini ayarlayın. Geçerli değerler, `simple` varsayılan olarak ve `full` Lucene için içerir. `simple|full` 

<a name="bkmk_example"></a> 

### <a name="example-showing-full-syntax"></a>Tam sözdizimini gösteren örnek

Aşağıdaki örnek, `queryType=full` parametrede belirgin olan Lucene sorgu sözdizimini kullanarak dizindeki belgeleri bulur. Bu sorgu, kategori alanının "bütçe" terimini içerdiği otelleri ve "yakın zamanda yenilenmiş" ifadesini içeren tüm aranabilir alanları döndürür. "Yakın zamanda yenilenmiş" ifadesini içeren belgeler, artış değeri teriminin bir sonucu olarak daha yüksek sırada yer alatır (3).  

Parametre `searchMode=all` bu örnekte alakalıdır. İşleçler sorguda olduğunda, genellikle `searchMode=all` *tüm* ölçütlerin eşleştiğinden emin olmak için ayarlamanız gerekir.

```
GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2019-05-06&querytype=full
```

 Alternatif olarak, POST kullanın:  

```
POST /indexes/hotels/docs/search?api-version=2019-05-06
{
  "search": "category:budget AND \"recently renovated\"^3",
  "queryType": "full",
  "searchMode": "all"
}
```

Ek örnekler için, [Azure Bilişsel Arama'da sorgu oluşturmak için Lucene sorgu sözdizimi örneklerine](search-query-lucene-examples.md)bakın. Sorgu parametrelerinin tam birliğini belirtme hakkında ayrıntılı bilgi için, [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;' ](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)e bakın.

> [!NOTE]  
>  Azure Bilişsel Arama, basit anahtar kelime araması için kullanılabilecek basit ve sağlam bir sorgu dili olan [Basit Sorgu Sözdizimini](query-simple-syntax.md)de destekler.  

##  <a name="syntax-fundamentals"></a><a name="bkmk_syntax"></a>Sözdizimi temelleri  
 Aşağıdaki sözdizimi temelleri Lucene sözdizimini kullanan tüm sorgular için geçerlidir.  

### <a name="operator-evaluation-in-context"></a>Bağlam içinde operatör değerlendirmesi

Yerleşim, bir sembolün işleç olarak mı yoksa dizedeki başka bir karakter olarak mı yorumlandığını belirler.

Örneğin, Lucene tam sözdiziminde tilde (~) hem bulanık arama hem de yakınlık araması için kullanılır. Alıntı yapılan bir ifadeden sonra yerleştirildiğinde, ~ yakınlık araması çağırır. Bir dönemin sonuna yerleştirildiğinde, ~ bulanık arama çağırır.

"Business~analyst" gibi bir dönem içinde karakter operatör olarak değerlendirilmez. Bu durumda, sorgunun bir terim veya tümcecik sorgusu olduğunu varsayarsak, [sözlük analizi](search-lucene-query-architecture.md#stage-2-lexical-analysis) ile tam [metin arama](search-lucene-query-architecture.md) ~ dışarı şeritler ve iki "business ~ analist" terimini tatili: iş VEYA analisti.

Yukarıdaki örnek tilde (~), ancak aynı ilke her işleç için geçerlidir.

### <a name="escaping-special-characters"></a>Özel karakterlerden kaçış

 Arama metninin bir parçası olarak kullanılmak üzere özel karakterler kaçmış olmalıdır. Onları ters eğik çizgi ile öne\\koyarak onlardan kurtulabilirsiniz ( ). Kaçması gereken özel karakterler şunlardır:  
`+ - && || ! ( ) { } [ ] ^ " ~ * ? : \ /`  

 Örneğin, joker karakterden \\ \*kaçmak için .

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>URL'lerde güvenli olmayan ve ayrılmış karakterleri kodlama

 Lütfen tüm güvenli olmayan ve rezerve edilmiş karakterlerin bir URL'de şifrelendirildiğından emin olun. Örneğin, '#' güvenli olmayan bir karakterdir, çünkü bir URL'deki bir fragement/anchor tanımlayıcısIdır. Bir URL'de `%23` kullanılıyorsa karakter kodlanmalıdır. '&' ve '=' parametreleri sınırlandırDıkları ve Azure Bilişsel Arama'da değerleri belirtirken ayrılmış karakterlere örnektir. Daha fazla bilgi için lütfen [RFC1738: Uniform Resource Locators (URL)](https://www.ietf.org/rfc/rfc1738.txt) adresine bakın.

 Güvensiz karakterler. ``" ` < > # % { } | \ ^ ~ [ ]`` Ayrılmış karakterler `; / ? : @ = + &`.

### <a name="precedence-operators-grouping-and-field-grouping"></a>Öncelik işleçleri: gruplandırma ve alan gruplandırma  
 Parantez içindeki işleçler de dahil olmak üzere alt sorgular oluşturmak için parantez ler kullanabilirsiniz. Örneğin, `motel+(wifi||luxury)` "motel" terimini ve "wifi" veya "lüks" (veya her ikisini) içeren belgeleri arayacaktır.

Alan gruplandırması benzerdir, ancak gruplandırmayı tek bir alana doğru kapsamlıdır. Örneğin, `hotelAmenities:(gym+(wifi||pool))` "gym" ve "wifi" veya "gym" ve "pool" için "hotelAmenities" alanını arar.  

### <a name="searchmode-parameter-considerations"></a>SearchMode parametre hususları  
 Azure Bilişsel `searchMode` Arama'da Basit [sorgu sözdiziminde](query-simple-syntax.md)açıklandığı gibi sorgular üzerindeki etkisi, Lucene sorgu sözdizimi için de geçerlidir. Yani, `searchMode` NOT operatörleri ile birlikte parametre ayarlamak nasıl etkileri açık değilseniz olağandışı görünebilir sorgu sonuçları neden olabilir. Varsayılanı `searchMode=any`korursanız ve NOT işleci kullanırsanız, işlem "New York" NOT "Seattle" Seattle olmayan tüm şehirleri döndürür gibi bir OR eylemi olarak hesaplanır.  

##  <a name="boolean-operators-and-or-not"></a><a name="bkmk_boolean"></a>Boolean operatörleri (VE, VEYA, Değİl) 
 Her zaman tüm büyük harflerle metin boolean işleçleri (VE, VEYA, NOT) belirtin.  

### <a name="or-operator-or-or-"></a>VEYA `OR` operatörü veya`||`

OR işleci dikey bir çubuk veya boru karakteridir. Örneğin: `wifi || luxury` "wifi" veya "lüks" veya her ikisini de içeren belgeleri arar. OR varsayılan bağlaç işleci olduğundan, aynı zamanda `wifi luxury` , bu `wifi || luxuery`eşdeğerdir, dışında bırakabilirsiniz .

### <a name="and-operator-and--or-"></a>VE `AND`operatörü `&&` , veya`+`

AND işleci bir ampersand veya artı işaretidir. Örneğin: `wifi && luxury` hem "wifi" hem de "lüks" içeren belgeleri arar. Artı karakter (+) gerekli terimler için kullanılır. Örneğin, `+wifi +luxury` her iki terimin de tek bir belge alanında bir yerde görünmesi gerekir.


### <a name="not-operator-not--or--"></a>NOT `NOT`operatörü `!` , veya`-`

NOT işleci bir ünlem işareti veya eksi işaretidir. Örneğin: `wifi !luxury` "wifi" terimine sahip ve/veya "lüks" olmayan belgeleri arayacaktır. Seçenek, `searchMode` NOT işleci ile bir terimin BIR + veya || yokluğunda sorgudaki diğer terimlerle birlikte ANDed veya ORed olup olmadığını denetler. Işleç. (varsayılan) veya `searchMode` `any` `all`. olarak ayarlanabilen geri çağırma

Daha `searchMode=any` fazla sonuç ekleyerek sorguların geri çağrılması artar ve varsayılan olarak " VEYA Değİl" olarak yorumlanır. Örneğin, `wifi -luxury` *wifi* terimini içeren veya lüks terimini içermeyen belgelerle *eşleşir.*

Kullanmak, `searchMode=all` daha az sonuç ekleyerek sorguların kesinliğini artırır ve varsayılan olarak "VE Değİl" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` içeren ve terimi `luxury`içermeyen belgeler eşleşir. Bu tartışmalı için daha sezgisel bir davranıştır - operatör. Bu nedenle, aramaları `searchMode=all` geri `searchMode=any` çağırmak yerine kesinlik için optimize etmek istiyorsanız *ve* kullanıcılarınız aramalarda `-` operatörü sık sık kullanıyorsa, yerine seçim yapmayı düşünmelisiniz.

##  <a name="query-size-limitations"></a><a name="bkmk_querysizelimits"></a>Sorgu boyutu sınırlamaları  
 Azure Bilişsel Arama'ya gönderebileceğiniz sorguların boyutunda bir sınır vardır. Özellikle, en fazla 1024 yan tümcesi (VE, OR vb. tarafından ayrılmış ifadeler) olabilir. Ayrıca bir sorguda herhangi bir tek terimin boyutu yaklaşık 32 KB sınırı vardır. Uygulamanız arama sorgularını programlı bir şekilde oluşturuyorsa, bu sorguyu sınırsız boyutta sorgu oluşturmayacak şekilde tasarlamanızı öneririz.  

##  <a name="scoring-wildcard-and-regex-queries"></a><a name="bkmk_searchscoreforwildcardandregexqueries"></a>Joker karakter ve regex sorgularını puanlama
 Azure Bilişsel Arama, metin sorguları için sıklık tabanlı puanlama[(TF-IDF)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)kullanır. Ancak, terimlerin kapsamının geniş olabileceği joker karakter ve regex sorguları için, sıralamanın eşleşmelere daha nadir gelen terimlere karşı önyargılı olmasını önlemek için sıklık faktörü göz ardı edilir. Tüm eşleşmeler joker karakter ve regex aramaları için eşit olarak ele alınır.

##  <a name="fielded-search"></a><a name="bkmk_fields"></a>Alanlı arama  
Arama deyiminin `fieldName:searchExpression` tek bir sözcük veya tümcecik olabileceği sözdizimi veya parantez içinde isteğe bağlı olarak Boolean işleçleri ile daha karmaşık bir ifade yle alanlı bir arama işlemi tanımlayabilirsiniz. Bazı örnekler şunlardır:  

- tür:caz Değİl tarih  

- sanatçılar:("Miles Davis" "John Coltrane")

Her iki dizenin de tek bir varlık olarak değerlendirilmesini istiyorsanız, bu durumda `artists` alanında iki farklı sanatçıyı ararken, tırnak işaretlerinin içine birden çok dize koyduğunuzdan emin olun.  

Belirtilen alan `fieldName:searchExpression` bir `searchable` alan olmalıdır.  Alan tanımlarında dizin özniteliklerinin nasıl kullanıldığına ilişkin ayrıntılar için [Dizin Oluştur'a](https://docs.microsoft.com/rest/api/searchservice/create-index) bakın.  

> [!NOTE]
> Alanlı arama ifadeleri kullanırken, her alanlı `searchFields` arama ifadesi açıkça belirtilen bir alan adı olduğundan parametre kullanmanız gerekmez. Ancak, bazı parçaların `searchFields` belirli bir alana kapsamının bulunduğu ve geri kalanının çeşitli alanlara uygulanabileceği bir sorgu çalıştırmak istiyorsanız parametreyi kullanmaya devam edebilirsiniz. `search=genre:jazz NOT history&searchFields=description` Örneğin, sorgu yalnızca `jazz` `genre` alanla eşleşirken, `NOT history` `description` alanla eşleşir. Verilen alan adı `fieldName:searchExpression` her zaman `searchFields` parametreden önce gelir, bu nedenle bu örnekte `genre` `searchFields` parametreye eklememiz gerekmez.

##  <a name="fuzzy-search"></a><a name="bkmk_fuzzy"></a>Bulanık arama  
 Bulanık bir arama, benzer bir yapıya sahip terimlerle eşleşmeler bulur. [Lucene belgeleri](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)başına, bulanık aramalar [Damerau-Levenshtein Mesafe](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance)dayanmaktadır. Bulanık aramalar, bir terimi mesafe ölçütlerini karşılayan en fazla 50 terime kadar genişletebilir. 

 Bulanık bir arama yapmak için, tek bir sözcüğün sonunda, 0 ile 2 (varsayılan) arasında bir sayı olan ve düzenlenen mesafeyi belirten "~" simgesini kullanın. Örneğin, "blue~" veya "blue~1" "blue", "blues" ve "glue" döndürülecek.

 Bulanık arama yalnızca terimlere uygulanabilir, tümceciklere değil, ancak tilde'yi çok parçalı bir ad veya tümcecikte ayrı ayrı ekleyebilirsiniz. Örneğin, "Unviersty~ ~ "Wshington ~" "University of Washington" maç olacaktır.
 

##  <a name="proximity-search"></a><a name="bkmk_proximity"></a>Yakınlık araması  
 Yakınlık aramaları, belgede birbirine yakın terimleri bulmak için kullanılır. Yakınlık sınırını oluşturan sözcük sayısını izleyen bir tümceciğin sonuna bir tilde "~" simgesi ekleyin. Örneğin, `"hotel airport"~5` bir belgede birbirinden 5 kelime içinde "otel" ve "havaalanı" terimlerini bulabilirsiniz.  


##  <a name="term-boosting"></a><a name="bkmk_termboost"></a>Terim artırma  
 Terim artırma, bir belgeyi, terimi içermeyen belgelere göre artırılmış terimi içeriyorsa daha yüksek sıralamaanlamına gelir. Bu, puanlama profillerinin belirli terimler yerine belirli alanları artırmasındaki puanlama profillerinden farklıdır.  

Aşağıdaki örnek, farklılıkların ortaya önüne gibi yardımcı olur. Belirli bir alandaki eşleşmeleri artıran bir puanlama profili olduğunu varsayalım, [örneğin musicstoreindex örneğinde](index-add-scoring-profiles.md#bkmk_ex) *tür* deyin. Terim artırma, belirli arama terimlerini diğerlerinden daha yüksek bir şekilde artırmak için kullanılabilir. Örneğin, `rock^2 electronic` tür alanındaki arama terimlerini içeren belgeleri dizindeki diğer aranabilir alanlardan daha yüksek artırır. Ayrıca, *kaya* arama terimini içeren belgeler, artış değeri terimi sonucunda diğer arama terimi *elektronikten* daha yüksek sıralanır (2).  

 Bir terimi artırmak için aradığınız dönemin sonunda bir artırma faktörü (bir sayı) içeren "^" simgesi olan "^", "^". İfadeleri de artırabilirsiniz. Artırma faktörü ne kadar yüksekse, terim diğer arama terimlerine göre o kadar alakalı olur. Varsayılan olarak, artırma faktörü 1'dir. Artırma faktörü pozitif olsa da, 1'den küçük olabilir (örneğin, 0,20).  

##  <a name="regular-expression-search"></a><a name="bkmk_regex"></a>Düzenli ifade araması  
 Normal bir ifade [araması, RegExp sınıfında](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html)belgelenen "/" ileri eğik çizgileri arasındaki içeriği temel alan bir eşleşme bulur.  

 Örneğin, "motel" veya "otel" içeren belgeleri `/[mh]otel/`bulmak için, belirtin. Normal ifade aramaları tek sözcüklerle eşleşir.

##  <a name="wildcard-search"></a><a name="bkmk_wildcard"></a>Joker karakter arama  
 Birden çok (*) veya tek (?) karakter joker karakter aramaları için genel olarak tanınan sözdizimini kullanabilirsiniz. Lucene sorgu arayıcının bu sembollerin bir tümcecikle değil, tek bir terimle kullanımını desteklediğini unutmayın.

Önek arama da yıldız (`*`) karakterini kullanır. Örneğin, "not defteri" veya "not defteri" döndürür `search=note*` bir sorgu ifadesi. Önek araması için tam Lucene sözdizimi gerekli değildir. Basit sözdizimi bu senaryoyu destekler.

Soneki arama, `*` `?` nerede veya dize önce, tam Lucene sözdizimi ve düzenli bir ifade gerektirir (bir * veya kullanamazsınız ? bir aramanın ilk karakteri olarak sembol). "Alfanümerik" terimi göz önüne alındığında,`search=/.*numeric.*/`bir sorgu ifadesi ( ) eşleşmeyi bulur.

> [!NOTE]  
> Sorgu ayrıştırma sırasında, önek, sonek, joker karakter veya normal ifadeler olarak formüle edilen sorgular, [sözlü çözümlemesi](search-lucene-query-architecture.md#stage-2-lexical-analysis)atlayarak sorgu ağacına olduğu gibi aktarılır. Eşleşmeler yalnızca dizin, sorgunuzun belirttiği biçimdeki dizeleri içeriyorsa bulunur. Çoğu durumda, kısmi terim ve desen eşleştirme başarılı olacak şekilde dize bütünlüğünü koruyan dizinleme sırasında alternatif bir çözümleyici gerekir. Daha fazla bilgi için Azure [Bilişsel Arama sorgularında Kısmi süreli arama'ya](search-query-partial-matching.md)bakın.

## <a name="see-also"></a>Ayrıca bkz.  

+ [Belgeleri Ara](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Filtreler ve sıralama için OData ifade sözdizimi](query-odata-filter-orderby-syntax.md)   
+ [Azure Bilişsel Arama'da basit sorgu sözdizimi](query-simple-syntax.md)   
