---
title: Lucene sorgu söz dizimi
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama joker karakter, belirsiz arama, RegEx ve diğer gelişmiş sorgu yapıları için kullanılan tam Lucene sorgu söz dizimi için başvuru.
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
ms.openlocfilehash: f4c3330b23b8b724cdbf5d7e09eec8a8dd5b8cfa
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81258992"
---
# <a name="lucene-query-syntax-in-azure-cognitive-search"></a>Azure Bilişsel Arama Lucene sorgu söz dizimi

Özelleştirilmiş sorgu formları için zengin [Lucene sorgu ayrıştırıcı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) sözdizimine göre Azure bilişsel arama karşı sorgular yazabilirsiniz: joker karakter, benzer arama, yakınlık araması, normal ifadeler birkaç örnektir. Lucene sorgu ayrıştırıcısı sözdiziminin büyük bir bölümü [azure bilişsel arama ' de](search-lucene-query-architecture.md), deyimler aracılığıyla `$filter` Azure bilişsel arama oluşturulan *Aralık aramaları* dışında bir şekilde uygulanır. 

> [!NOTE]
> Full Lucene sözdizimi, bu API 'nin [$Filter](search-filters.md) parametresi Için kullanılan [OData sözdizimiyle](query-odata-filter-orderby-syntax.md) karıştırılmamalıdır, [arama belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents) API 'sinin **arama** parametresinde geçirilen sorgu ifadeleri için kullanılır. Bu farklı sözdizimlerinin sorgu oluşturma, kaçış dizeleri vb. oluşturmak için kendi kuralları vardır.

## <a name="invoke-full-parsing"></a>Tam ayrıştırmayı çağır

Hangi ayrıştırıcısının kullanılacağını belirtmek için `queryType` arama parametresini ayarlayın. Geçerli değerler, `simple|full`varsayılan `simple` olarak ve `full` Lucene için içerir. 

<a name="bkmk_example"></a> 

### <a name="example-showing-full-syntax"></a>Tam sözdizimini gösteren örnek

Aşağıdaki örnek, dizinde bulunan ve Lucene sorgu söz dizimini `queryType=full` kullanarak dizindeki belgeleri bulur. Bu sorgu, Kategori alanının "bütçe" terimini ve "son randevu" ifadesini içeren tüm aranabilir alanları içermesi halinde oteller döndürür. "Son randevu" ifadesini içeren belgeler, artırma değeri (3) sonucu olarak daha yüksektir.  

`searchMode=all` Parametresi bu örnekle ilgilidir. Her operatör sorgu üzerinde olduğunda, genellikle ölçütlerin *tümünün* eşleştiğinden emin `searchMode=all` olmak için ayarlamanız gerekir.

```
GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2019-05-06&querytype=full
```

 Alternatif olarak, POST 'u kullanın:  

```
POST /indexes/hotels/docs/search?api-version=2019-05-06
{
  "search": "category:budget AND \"recently renovated\"^3",
  "queryType": "full",
  "searchMode": "all"
}
```

Daha fazla örnek için bkz. [Lucene sorgu söz dizimi örnekleri Azure bilişsel arama 'de sorgu oluşturma](search-query-lucene-examples.md). Sorgu parametrelerinin tam olarak belirlenmesi hakkında daha fazla bilgi için bkz. [arama belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

> [!NOTE]  
>  Azure Bilişsel Arama [basit sorgu söz dizimini](query-simple-syntax.md)da destekler, basit ve güçlü bir sorgu dili, doğrudan anahtar sözcük araması için kullanılabilir.  

##  <a name="syntax-fundamentals"></a><a name="bkmk_syntax"></a>Sözdizimi temelleri  

Aşağıdaki sözdizimi temelleri, Lucene sözdizimini kullanan tüm sorgular için geçerlidir.  

### <a name="operator-evaluation-in-context"></a>Bağlamda işleç değerlendirmesi

Yerleştirme, bir simgenin bir operatör ya da bir dizedeki yalnızca başka bir karakter olarak yorumlanıp yorumlanmadığını belirler.

Örneğin, Lucene Full sözdiziminde, hem belirsiz arama hem de yakınlık araması için tilde (~) kullanılır. Tırnak içine alınmış bir tümcecikden sonra yerleştirildiğinde,, yakınlık aramasını çağırır. Bir terimin sonuna yerleştirildiğinde, belirsiz aramayı çağırır.

"İş ~ analist" gibi bir dönem içinde, karakter bir işleç olarak değerlendirilmez. Bu durumda, sorgunun bir terim veya tümcecik sorgusu olduğu varsayılırsa, [sözcük temelli Analize](search-lucene-query-architecture.md#stage-2-lexical-analysis) sahip [tam metin araması](search-lucene-query-architecture.md) ,, ve "Business ~ analist" TERIMINI iki: iş veya analist olarak keser.

Yukarıdaki örnek, tilde (~), ancak aynı prensibi her operatör için geçerlidir.

### <a name="escaping-special-characters"></a>Özel karakterleri kaçış

Arama metinlerinin bir parçası olarak arama işleçlerinden herhangi birini kullanmak için, karakteri tek bir ters eğik çizgiyle (`\`) sonuna ekleyerek karakteri kaçış. Örneğin, bir joker karakter araması için `https://`, burada `://` sorgu dizesinin bir parçası olan ' i belirtmeniz `search=https\:\/\/*`gerekir. Benzer şekilde, kaçan bir telefon numarası, şöyle görünebilir `\+1 \(800\) 642\-7676`.

Kaçışın gerekli olduğu özel karakterler şunlardır:  
`+ - & | ! ( ) { } [ ] ^ " ~ * ? : \ /`  

> [!NOTE]  
> Kaçış belirteçleri birlikte tutar, ancak dizin oluşturma sırasında [sözcük temelli analizler](search-lucene-query-architecture.md#stage-2-lexical-analysis) bunları açabilir. Örneğin, standart Lucene çözümleyici, sözcükleri kısa çizgilerden, boşluklardan ve diğer karakterlere göre keser. Sorgu dizesinde özel karakterlere ihtiyacınız varsa, bunları dizinde koruyan bir çözümleyici gerekebilir. Bazı seçimler, hecelenmiş kelimeleri koruyan Microsoft doğal [dil Çözümleyicileri](index-add-language-analyzers.md)veya daha karmaşık desenler için özel bir çözümleyici içerir. Daha fazla bilgi için bkz. [kısmi terimler, desenler ve özel karakterler](search-query-partial-matching.md).

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>URL 'lerde güvenli olmayan ve ayrılmış karakterleri kodlama

Lütfen tüm güvenli olmayan ve ayrılmış karakterlerin bir URL 'de kodlandığını doğrulayın. Örneğin, URL 'deki bir parça/bağlantı tanımlayıcısı olduğundan, ' # ' güvenli olmayan bir karakterdir. URL 'de kullanılıyorsa, karakterin kodlanmalıdır `%23` . ' & ' ve ' = ', parametreleri sınırlandıran ve Azure Bilişsel Arama değerlerini belirten ayrılmış karakter örnekleridir. Daha fazla ayrıntı için lütfen bkz. [rfc1738: Uniform Resource Konumlandırıcıları (URL)](https://www.ietf.org/rfc/rfc1738.txt) .

Güvenli olmayan karakterler ``" ` < > # % { } | \ ^ ~ [ ]``. Ayrılan karakterler şunlardır `; / ? : @ = + &`.

###  <a name="query-size-limits"></a><a name="bkmk_querysizelimits"></a>Sorgu boyutu sınırları

 Azure Bilişsel Arama 'e gönderebilmeniz için sorguların boyutuyla ilgili bir sınır vardır. Özellikle, en fazla 1024 yan tümce (ve, veya ile ayrılmış ifadeler) olabilir. Ayrıca, bir sorgudaki her bir terimin boyutunda yaklaşık 32 KB 'lik bir sınır vardır. Uygulamanız program aracılığıyla arama sorguları oluşturursa, bu şekilde, sınırsız boyut sorguları oluşturmamasını sağlayan bir şekilde tasarlamayı öneririz.  

### <a name="precedence-operators-grouping"></a>Öncelik işleçleri (gruplama)

 Parantez, parantez içinde işleç dahil olmak üzere alt sorgular oluşturmak için kullanabilirsiniz. Örneğin, `motel+(wifi||luxury)` "Motel" terimini ve "WiFi" veya "merkezlerini" (ya da her ikisi) içeren belgeleri arar.

Alan gruplama benzerdir, ancak gruplamayı tek bir alanla kapsamlara sahiptir. Örneğin, `hotelAmenities:(gym+(wifi||pool))` "hotelAmenities" alanını "Gym" ve "WiFi" ya da "Gym" ve "havuz" olarak arar.  

##  <a name="boolean-search"></a><a name="bkmk_boolean"></a>Boole arama

 Her zaman tüm büyük harf metin Boole işleçlerini (ve veya, DEĞIL) belirtin.  

### <a name="or-operator-or-or-"></a>OR işleci `OR` veya`||`

OR işleci dikey bir çubuk veya boru karakterdir. Örneğin: `wifi || luxury` "WiFi" veya "merkezlerini" ya da her ikisini de içeren belgeleri arar. YA da varsayılan bir `wifi luxury` `wifi || luxury`bağlantılı operatör olduğundan, buna eşdeğer olan gibi da bırakabilirsiniz.

### <a name="and-operator-and--or-"></a>AND işleci `AND` `&&` veya`+`

AND işleci bir ve işareti ya da artı işareti. Örneğin: `wifi && luxury` "WiFi" ve "merkezlerini" içeren belgeler için arama yapılır. Plus karakteri (+) gerekli şartlar için kullanılır. Örneğin, `+wifi +luxury` her iki terimi tek bir belge alanında bir yerde gözükmelidir.

### <a name="not-operator-not--or--"></a>Not işleci `NOT` `!` veya`-`

NOT işleci eksi işareti. Örneğin, `wifi –luxury` ve/veya olmayan belgeler `wifi` için arama yapılır. `luxury`

Sorgu isteğindeki **searchMode** PARAMETRESI, Not işleci olan bir terimin, sorgudaki diğer koşullara sahip olup olmadığını denetler (diğer koşullarda hiçbir `+` veya `|` işleci olmadığı varsayılarak). Geçerli değerler veya `any` `all`içerir.

`searchMode=any`sorgu geri çekmeyi daha fazla sonuç ekleyerek artırır ve varsayılan `-` olarak "veya Not" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` ya da terimi `luxury`içermeyen belgelerle eşleşir.

`searchMode=all`sorguların hassasiyetini daha az sonuç ekleyerek artırır ve varsayılan olarak "ve NOT" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` içeren belgelerle eşleştirecektir ve "merkezlerini" terimini içermemelidir. Bu, `-` operatör için daha sezgisel bir davranış ile yapılır. Bu nedenle, aramalarını geri çağırmak `searchMode=all` yerine duyarlık `searchMode=any` için optimize etmek istiyorsanız yerine kullanmanız gerekir *ve* kullanıcılarınız, aramalardaki `-` işleci sıklıkla kullanır.

Bir **searchMode** ayarı üzerinde karar verirken, çeşitli uygulamalardaki sorgular için Kullanıcı etkileşimi düzenlerini göz önünde bulundurun. Bilgi arayan kullanıcıların, daha fazla yerleşik gezinti yapılarına sahip olan e-ticaret sitelerinin aksine, bir sorguya işleç ekleme olasılığı yüksektir.

##  <a name="fielded-search"></a><a name="bkmk_fields"></a>Parçalı arama

Arama ifadesinin tek bir sözcük veya tümcecik ya da parantez `fieldName:searchExpression` içinde daha karmaşık bir ifade olabilecek, isteğe bağlı olarak Boolean işleçleriyle, bir ara değer arama işlemini sözdizimi ile tanımlayabilirsiniz. Bazı örnekler şunlardır:  

- Tarz: canot geçmiş  

- Sanatçılar:("mil Davis" "John Coltrane")

Her iki dizenin de tek bir varlık olarak değerlendirilmesini istiyorsanız, bu durumda `artists` alanda iki ayrı sanatçı aramak istiyorsanız, tırnak işaretleri içine birden çok dize yerleştirdiğinizden emin olun.  

İçinde `fieldName:searchExpression` belirtilen alanın bir `searchable` alan olması gerekir.  Dizin özniteliklerinin alan tanımlarında nasıl kullanıldığına ilişkin ayrıntılar için bkz. [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) .  

> [!NOTE]
> Kullanılabilir arama ifadelerini kullanırken, her bir alan arama ifadesinde açık olarak belirtilmiş bir `searchFields` alan adı olduğundan, parametresini kullanmanız gerekmez. Ancak, bazı parçaların belirli bir alan `searchFields` kapsamında bulunduğu bir sorgu çalıştırmak istiyorsanız parametresini kullanmaya devam edebilirsiniz ve REST birçok alana uygulanabilir. Örneğin, `search=genre:jazz NOT history&searchFields=description` sorgu `jazz` `genre` yalnızca alanla eşleşirken `NOT history` `description` alanla eşleşir. `fieldName:searchExpression` Her zaman ' de belirtilen alan adı, bu örnekte `searchFields` bu nedenle parametreye dahil `genre` `searchFields` etmemiz gerekmediğimiz parametreye göre öncelik alır.

##  <a name="fuzzy-search"></a><a name="bkmk_fuzzy"></a>Benzer arama

Benzer bir arama, benzer bir yapıya sahip hükümlerde eşleşmeleri bulur ve iki veya daha az uzaklık ölçütlerine uyan en fazla 50 terim için bir terim genişletiyor. Daha fazla bilgi için bkz. [belirsiz arama](search-query-fuzzy.md).

 Benzer bir arama yapmak için, tek bir sözcüğün sonundaki tilde "~" sembolünü, isteğe bağlı bir parametreyle, 0 ile 2 (varsayılan) arasında, düzenleme uzaklığını belirten bir sayı kullanın. Örneğin, "mavi ~" veya "mavi ~ 1", "mavi", "maves" ve "tutkalla" döndürür.

 Benzer arama yalnızca koşullara uygulanabilir, tümceciklere uygulanamaz, ancak her terime bir çok parçalı ad veya ifade içinde her bir terime ekleyebilirsiniz. Örneğin, "Unviersty ~ of ~" Wshington ~ "," University of Washington "ile eşleşir.
 
##  <a name="proximity-search"></a><a name="bkmk_proximity"></a>Yakınlık araması

Yakınlık aramaları, bir belgedeki birbirini yakın terimleri bulmak için kullanılır. Bir ifadenin sonuna, ardından yakınlık sınırını oluşturan sözcüklerin sayısını içeren bir tilde "~" simgesi ekleyin. Örneğin, `"hotel airport"~5` "otel" ve "Havaalanı" terimlerini bir belgedeki birbirini 5 sözcükten bulur.  


##  <a name="term-boosting"></a><a name="bkmk_termboost"></a>Terim artırma

Terim artırma, bir belgeyi, süresi içermeyen belgelere göre, daha yüksek bir dönem içeriyorsa bir belgenin derecelendirdiğini ifade eder. Bu, Puanlama profillerindeki Puanlama profillerinin belirli koşullar yerine belirli alanları arttırma açısından farklılık gösterir.  

Aşağıdaki örnek, farkları göstermeye yardımcı olur. Belirli bir alanda eşleşen bir Puanlama profili olduğunu, [müzik \ dizin örneğinde](index-add-scoring-profiles.md#bkmk_ex)de *tarzı* söylediğini varsayalım. Belirli arama terimlerini diğerlerinden daha fazla artırmak için kullanım süresi kullanılabilir. Örneğin, `rock^2 electronic` tarzdaki arama terimlerini içeren belgeleri, dizin içindeki diğer aranabilir alanlardan daha yüksek olacak şekilde artırır. Ayrıca, arama terimini içeren belgeler, diğer arama teriminden daha *yüksek olarak* *derecelendirilir (* 2).  

 Bir terimi artırmak için, aradığınız terimin sonundaki "^" giriş işaretini, bir artırma faktörü (bir sayı) ile simge kullanın. Ayrıca tümcecikleri de kullanabilirsiniz. Yükseltme faktörü arttıkça, terim diğer arama koşullarına göre daha ilgili olacaktır. Varsayılan olarak, Boost faktörü 1 ' dir. Boost faktörü pozitif olmalıdır, ancak 1 ' den az olabilir (örneğin, 0,20).  

##  <a name="regular-expression-search"></a><a name="bkmk_regex"></a>Normal ifade arama  
 Normal ifade araması, [RegExp sınıfında](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html)belgelendiği gibi eğik çizgi "/" arasındaki içeriğe dayalı bir eşleşme bulur.  

 Örneğin, "Motel" veya "otel" içeren belgeleri bulmak için, öğesini belirtin `/[mh]otel/`. Normal ifade aramaları tek sözcüklerle eşleştirilir.

Bazı araçlar ve diller, ek kaçış karakter gereksinimleri de vardır. JSON için, eğik çizgi içeren dizelerin ters eğik çizgiyle kaçışması vardır: "microsoft.com/azure/", normal `search=/.*microsoft.com\/azure\/.*/` ifadenin `search=/.* <string-placeholder>.*/` ayarlandığı ve `microsoft.com\/azure\/` kaçış eğik çizgiyle kaçış olan dize olur.

##  <a name="wildcard-search"></a><a name="bkmk_wildcard"></a>Joker karakter arama  

Birden çok (*) veya tek (?) karakterli joker karakter aramaları için genellikle tanınan sözdizimini kullanabilirsiniz. Lucene sorgu ayrıştırıcısının, bu sembollerin tek bir terim ve tümcecik değil, kullanımını desteklediği unutulmamalıdır.

Ön ek arama, yıldız işareti (`*`) karakterini de kullanır. Örneğin, bir sorgu ifadesi "Not `search=note*` defteri" veya "Notepad" döndürür. Önek arama için tam Lucene sözdizimi gerekli değildir. Basit sözdizimi bu senaryoyu destekler.

Sonek araması, dizenin `*` nerede `?` veya önünde olduğu, Full Lucene söz dizimi ve normal bir ifade gerektirir (* veya kullanamazsınız. bir aramanın ilk karakteri olarak simge). "Alfasayısal" terimi verildiğinde, (`search=/.*numeric.*/`) öğesinin bir sorgu ifadesi eşleşmeyi bulur.

> [!NOTE]  
> Sorgu ayrıştırma sırasında, önek, sonek, joker karakter veya normal ifadeler olarak ifade edilen sorgular, [sözcük temelli analizleri](search-lucene-query-architecture.md#stage-2-lexical-analysis)atlayarak sorgu ağacına olarak geçirilir. Eşleşmeler yalnızca, sorgunun belirttiği biçimdeki dizeleri içermesi halinde bulunur. Çoğu durumda, kısmi terim ve kalıp eşleştirme başarılı olması için dize bütünlüğünü koruyan dizin oluşturma sırasında alternatif bir çözümleyiciye ihtiyacınız olacaktır. Daha fazla bilgi için bkz. [Azure bilişsel arama sorgularda kısmi terim arama](search-query-partial-matching.md).

##  <a name="scoring-wildcard-and-regex-queries"></a><a name="bkmk_searchscoreforwildcardandregexqueries"></a>Puanlama joker karakteri ve Regex sorguları

Azure Bilişsel Arama metin sorguları için sıklık tabanlı Puanlama ([tf-ıDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)) kullanır. Bununla birlikte, koşulların kapsamını büyük olasılıkla geniş olabilecek bir joker karakter ve Regex sorguları için, derecelendirmenin, rarer terimleriyle eşleşmelerin eşleşmesini engellemek için sıklık faktörü yok sayılır. Tüm eşleşmeler joker ve Regex aramaları için eşit olarak değerlendirilir.

## <a name="see-also"></a>Ayrıca bkz.

+ [Basit arama için sorgu örnekleri](search-query-simple-examples.md)
+ [Tam Lucene Search için sorgu örnekleri](search-query-lucene-examples.md)
+ [Belgelerde ara](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Filtreler ve sıralama için OData ifade sözdizimi](query-odata-filter-orderby-syntax.md)   
+ [Azure Bilişsel Arama basit sorgu söz dizimi](query-simple-syntax.md)   
