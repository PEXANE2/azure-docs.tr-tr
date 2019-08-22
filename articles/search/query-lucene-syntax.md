---
title: Lucene sorgu söz dizimi-Azure Search
description: Azure Search için kullanılan Full Lucene sözdiziminin başvurusu.
services: search
ms.service: search
ms.topic: conceptual
ms.date: 08/08/2019
author: brjohnstmsft
ms.author: brjohnst
manager: nitinme
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
ms.openlocfilehash: d667588cea5902700c225dd7b597d8f03d93d200
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69650056"
---
# <a name="lucene-query-syntax-in-azure-search"></a>Azure Search Lucene sorgu söz dizimi
Özelleştirilmiş sorgu formları için zengin [Lucene sorgu ayrıştırıcı](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) sözdizimine göre Azure Search karşı sorgular yazabilirsiniz: joker karakter, benzer arama, yakınlık araması, normal ifadeler birkaç örnektir. Azure Search, Azure Search ifadeler aracılığıyla `$filter` oluşturulan *Aralık aramaları* dışında, Lucene sorgu ayrıştırıcısı sözdiziminin büyük bir bölümü [olarak uygulanır](search-lucene-query-architecture.md). 

## <a name="how-to-invoke-full-parsing"></a>Tam ayrıştırma çağırma

Hangi ayrıştırıcısının kullanılacağını belirtmek için aramaparametresiniayarlayın.`queryType` Geçerli değerler `simple|full` `simple` , varsayılan olarak ve `full` Lucene için içerir. 

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

Daha fazla örnek için bkz. [Azure Search ' de sorgu oluşturma Için Lucene sorgu söz dizimi örnekleri](search-query-lucene-examples.md). Sorgu parametrelerinin tam olarak belirlenmesi hakkında daha fazla bilgi için bkz. [arama belgeleri &#40;Azure Search hizmeti REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

> [!NOTE]  
>  Azure Search Ayrıca basit [sorgu söz dizimini](query-simple-syntax.md)destekler, basit ve güçlü bir sorgu dili basit ve sağlam bir sorgu dilidir.  

##  <a name="bkmk_syntax"></a>Sözdizimi temelleri  
 Aşağıdaki sözdizimi temelleri, Lucene sözdizimini kullanan tüm sorgular için geçerlidir.  

### <a name="operator-evaluation-in-context"></a>Bağlamda işleç değerlendirmesi

Yerleştirme, bir simgenin bir operatör ya da bir dizedeki yalnızca başka bir karakter olarak yorumlanıp yorumlanmadığını belirler.

Örneğin, Lucene Full sözdiziminde, hem belirsiz arama hem de yakınlık araması için tilde (~) kullanılır. Tırnak içine alınmış bir tümcecikden sonra yerleştirildiğinde,, yakınlık aramasını çağırır. Bir terimin sonuna yerleştirildiğinde, belirsiz aramayı çağırır.

"İş ~ analist" gibi bir dönem içinde, karakter bir işleç olarak değerlendirilmez. Bu durumda, sorgunun bir terim veya tümcecik sorgusu olduğu varsayılırsa, [sözcük temelli Analize](search-lucene-query-architecture.md#stage-2-lexical-analysis) sahip [tam metin araması](search-lucene-query-architecture.md) ,, ve "Business ~ analist" TERIMINI iki: iş veya analist olarak keser.

Yukarıdaki örnek, tilde (~), ancak aynı prensibi her operatör için geçerlidir.

### <a name="escaping-special-characters"></a>Özel karakterleri kaçış

 Arama metninin bir parçası olarak kullanılması için özel karakterlerin kaçışılması gerekir. Üzerlerine ters eğik çizgi (\\) ekleyerek onları kaçış yapabilirsiniz. Kaçışılması gereken özel karakterler şunlardır:  
`+ - && || ! ( ) { } [ ] ^ " ~ * ? : \ /`  

 Örneğin, bir joker karakterle çıkmak için kullanın \\. \*

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>URL 'lerde güvenli olmayan ve ayrılmış karakterleri kodlama

 Lütfen tüm güvenli olmayan ve ayrılmış karakterlerin bir URL 'de kodlandığını doğrulayın. Örneğin, URL 'deki bir değer/bağlayıcı tanımlayıcısı olduğundan, ' # ' güvenli olmayan bir karakterdir. URL 'de kullanılıyorsa, karakterin kodlanmalıdır `%23` . ' & ' ve ' = ', parametreleri sınırlandıran ve Azure Search değerlerini belirten ayrılmış karakter örnekleridir. Lütfen bkz [. rfc1738: Daha fazla ayrıntı için Tekdüzen Kaynak](https://www.ietf.org/rfc/rfc1738.txt) Konum Belirleyicisi (URL).

 Güvenli olmayan karakterler ``" ` < > # % { } | \ ^ ~ [ ]``. Ayrılan karakterler şunlardır `; / ? : @ = + &`.

### <a name="precedence-operators-grouping-and-field-grouping"></a>Öncelik işleçleri: gruplandırma ve alan gruplama  
 Parantez, parantez içinde işleç dahil olmak üzere alt sorgular oluşturmak için kullanabilirsiniz. Örneğin, `motel+(wifi||luxury)` "Motel" terimini ve "WiFi" veya "merkezlerini" (ya da her ikisi) içeren belgeleri arar.

Alan gruplama benzerdir, ancak gruplamayı tek bir alanla kapsamlara sahiptir. Örneğin, `hotelAmenities:(gym+(wifi||pool))` "hotelAmenities" alanını "Gym" ve "WiFi" ya da "Gym" ve "havuz" olarak arar.  

### <a name="searchmode-parameter-considerations"></a>SearchMode parametresi konuları  
 `searchMode` [Azure Search basit sorgu söz dizimi](query-simple-syntax.md)bölümünde açıklandığı gibi sorguların etkileri, Lucene sorgu söz dizimine eşit olarak uygulanır. Bu şekilde `searchMode` , Not işleçleriyle birlikte, parametresini nasıl ayarlayacaı üzerinde hiçbir görünmemeye açık olmadığınız takdirde olağan dışı görünebilir olabilecek sorgu sonuçları ortaya çıkabilir. Varsayılan `searchMode=any`,, ve Not işleci kullanıyorsanız, işlem bir veya eylem olarak hesaplanır; örneğin "New York" Not "Seattle", Seattle olmayan tüm şehirleri döndürür.  

##  <a name="bkmk_boolean"></a>Boole işleçleri (ve, veya DEĞIL) 
 Her zaman tüm büyük harf metin Boole işleçlerini (ve veya, DEĞIL) belirtin.  

### <a name="or-operator-or-or-"></a>Or işleci `OR` veya`||`

OR işleci dikey bir çubuk veya boru karakterdir. Örneğin: `wifi || luxury` "WiFi" veya "merkezlerini" ya da her ikisini de içeren belgeleri arar. Ya da varsayılan bir `wifi luxury` `wifi || luxuery`bağlantılı operatör olduğundan, buna eşdeğer olan gibi da bırakabilirsiniz.

### <a name="and-operator-and--or-"></a>AND işleci `AND`veya `&&``+`

AND işleci bir ve işareti ya da artı işareti. Örneğin: `wifi && luxury` "WiFi" ve "merkezlerini" içeren belgeler için arama yapılır. Plus karakteri (+) gerekli şartlar için kullanılır. Örneğin, `+wifi +luxury` her iki terimi tek bir belge alanında bir yerde gözükmelidir.


### <a name="not-operator-not--or--"></a>Not işleci `NOT`veya `!``-`

NOT işleci bir ünlem işareti veya eksi işareti. Örneğin: `wifi !luxury` "WiFi" teriminin bulunduğu ve/veya "merkezlerini" olmayan belgeler için arama yapılır. `searchMode` Seçeneği, Not işleci olan bir terimin, bir + veya | | işleci yokluğunda, sorgudaki diğer koşullara göre mi yoksa ORed mi olduğunu denetler. , (Varsayılan) veya `all`olarak ayarlanabilir. `any` `searchMode`

Kullanarak `searchMode=any` , daha fazla sonuç ekleyerek sorguların geri çağrılması artar ve varsayılan olarak "veya Not" olarak yorumlanır. Örneğin, `wifi -luxury` *WiFi* terimini içeren belgelerle veya merkezlerini terimini içermeyen belgelerle eşleştirecektir *.*

Kullanımı `searchMode=all` , sorguların hassasiyetini daha az sonuç ekleyerek artırır ve varsayılan olarak "ve Not" olarak yorumlanır. Örneğin, `wifi -luxury` terimi `wifi` içeren belgelerle eşleştirecektir ve terimi `luxury`içermez. Bu,-işleci için daha sezgisel bir davranıştır. Bu nedenle, aramaları geri çağırmak `searchMode=all` yerine `searchMode=any` duyarlık için iyileştirmek istiyorsanız `-` *veya* kullanıcılarınız aramadaki işleci sıklıkla kullanıyorsa, üzerinde seçim yapmayı düşünmelisiniz.

##  <a name="bkmk_querysizelimits"></a>Sorgu boyutu sınırlamaları  
 Azure Search için gönderebilmeniz gereken sorguların boyutu için bir sınır vardır. Özellikle, en fazla 1024 yan tümce (ve, veya ile ayrılmış ifadeler) olabilir. Ayrıca, bir sorgudaki her bir terimin boyutunda yaklaşık 32 KB 'lik bir sınır vardır. Uygulamanız program aracılığıyla arama sorguları oluşturursa, bu şekilde, sınırsız boyut sorguları oluşturmamasını sağlayan bir şekilde tasarlamayı öneririz.  

##  <a name="bkmk_searchscoreforwildcardandregexqueries"></a>Puanlama joker karakteri ve Regex sorguları
 Azure Search metin sorguları için sıklık tabanlı Puanlama ([tf-ıDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)) kullanır. Bununla birlikte, koşulların kapsamını büyük olasılıkla geniş olabilecek bir joker karakter ve Regex sorguları için, derecelendirmenin, rarer terimleriyle eşleşmelerin eşleşmesini engellemek için sıklık faktörü yok sayılır. Tüm eşleşmeler joker ve Regex aramaları için eşit olarak değerlendirilir.

##  <a name="bkmk_fields"></a>Parçalı arama  
Arama ifadesinin tek bir sözcük veya tümcecik ya da parantez `fieldName:searchExpression` içinde daha karmaşık bir ifade olabilecek, isteğe bağlı olarak Boolean işleçleriyle, bir ara değer arama işlemini sözdizimi ile tanımlayabilirsiniz. Bazı örnekler şunlardır:  

- Tarz: canot geçmiş  

- Sanatçılar:("mil Davis" "John Coltrane")

Her iki dizenin de tek bir varlık olarak değerlendirilmesini istiyorsanız, bu durumda `artists` alanda iki ayrı sanatçı aramak istiyorsanız, tırnak işaretleri içine birden çok dize yerleştirdiğinizden emin olun.  

İçinde `fieldName:searchExpression` belirtilen alanın bir `searchable` alan olması gerekir.  Dizin özniteliklerinin alan tanımlarında nasıl kullanıldığına ilişkin ayrıntılar için bkz. [Dizin oluşturma](https://docs.microsoft.com/rest/api/searchservice/create-index) .  

> [!NOTE]
> Kullanılabilir arama ifadelerini kullanırken, her bir alan arama ifadesinde açık olarak belirtilmiş bir `searchFields` alan adı olduğundan, parametresini kullanmanız gerekmez. Ancak, bazı parçaların belirli bir alan `searchFields` kapsamında bulunduğu bir sorgu çalıştırmak istiyorsanız parametresini kullanmaya devam edebilirsiniz ve REST birçok alana uygulanabilir. `search=genre:jazz NOT history&searchFields=description` Örneğin, sorgu `NOT history` yalnızca alanla`genre`eşleşirkenalanlaeşleşir. `jazz` `description` Her zaman ' `fieldName:searchExpression` de belirtilen alan adı, bu örnekte bu nedenle parametreye dahil `searchFields` `genre` `searchFields` etmemiz gerekmediğimiz parametreye göre öncelik alır.

##  <a name="bkmk_fuzzy"></a>Benzer arama  
 Benzer bir arama, benzer bir yapıya sahip olan hükümlerle eşleşmeleri bulur. Her [Lucene belgesi](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)için, belirsiz aramalar, [dumerau-Pavenshtein mesafesini](https://en.wikipedia.org/wiki/Damerau%E2%80%93Levenshtein_distance)temel alır. Benzer aramalar, uzaklık ölçütlerine uyan en fazla 50 terim için bir dönemi genişletebilir. 

 Benzer bir arama yapmak için, tek bir sözcüğün sonundaki tilde "~" sembolünü, isteğe bağlı bir parametreyle, 0 ile 2 (varsayılan) arasında, düzenleme uzaklığını belirten bir sayı kullanın. Örneğin, "mavi ~" veya "mavi ~ 1", "mavi", "maves" ve "tutkalla" döndürür.

 Benzer arama yalnızca koşullara uygulanabilir, tümceciklere uygulanamaz, ancak her terime bir çok parçalı ad veya ifade içinde her bir terime ekleyebilirsiniz. Örneğin, "Unviersty ~ of ~" Wshington ~ "," University of Washington "ile eşleşir.
 

##  <a name="bkmk_proximity"></a>Yakınlık araması  
 Yakınlık aramaları, bir belgedeki birbirini yakın terimleri bulmak için kullanılır. Bir ifadenin sonuna, ardından yakınlık sınırını oluşturan sözcüklerin sayısını içeren bir tilde "~" simgesi ekleyin. Örneğin, `"hotel airport"~5` "otel" ve "Havaalanı" terimlerini bir belgedeki birbirini 5 sözcükten bulur.  


##  <a name="bkmk_termboost"></a>Terim artırma  
 Terim artırma, bir belgeyi, süresi içermeyen belgelere göre, daha yüksek bir dönem içeriyorsa bir belgenin derecelendirdiğini ifade eder. Bu, Puanlama profillerindeki Puanlama profillerinin belirli koşullar yerine belirli alanları arttırma açısından farklılık gösterir.  

Aşağıdaki örnek, farkları göstermeye yardımcı olur. Belirli bir alanda eşleşen bir Puanlama profili olduğunu, [müzik \ dizin örneğinde](index-add-scoring-profiles.md#bkmk_ex)de *tarzı* söylediğini varsayalım. Belirli arama terimlerini diğerlerinden daha fazla artırmak için kullanım süresi kullanılabilir. Örneğin, `rock^2 electronic` tarzdaki arama terimlerini içeren belgeleri, dizin içindeki diğer aranabilir alanlardan daha yüksek olacak şekilde artırır. Ayrıca, arama terimini içeren belgeler, diğer arama teriminden daha yüksek olarak derecelendirilir (2 ).  

 Bir terimi artırmak için, aradığınız terimin sonundaki "^" giriş işaretini, bir artırma faktörü (bir sayı) ile simge kullanın. Ayrıca tümcecikleri de kullanabilirsiniz. Yükseltme faktörü arttıkça, terim diğer arama koşullarına göre daha ilgili olacaktır. Varsayılan olarak, Boost faktörü 1 ' dir. Boost faktörü pozitif olmalıdır, ancak 1 ' den az olabilir (örneğin, 0,20).  

##  <a name="bkmk_regex"></a>Normal ifade arama  
 Normal ifade araması, [RegExp sınıfında](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html)belgelendiği gibi eğik çizgi "/" arasındaki içeriğe dayalı bir eşleşme bulur.  

 Örneğin, "Motel" veya "otel" içeren belgeleri bulmak için, öğesini belirtin `/[mh]otel/`.  Normal ifade aramaları tek sözcüklerle eşleştirilir.   

##  <a name="bkmk_wildcard"></a>Joker karakter arama  
 Birden çok (*) veya tek (?) karakterli joker karakter aramaları için genellikle tanınan sözdizimini kullanabilirsiniz. Lucene sorgu ayrıştırıcısının, bu sembollerin tek bir terim ve tümcecik değil, kullanımını desteklediği unutulmamalıdır.  

 Örneğin, "Not" ("Not defteri" veya "Not defteri" gibi "Not" önekiyle birlikte kelimeleri içeren belgeleri bulmak için "Not *" değerini belirtin.  

> [!NOTE]  
>  \* Veya? kullanamazsınız bir aramanın ilk karakteri olarak sembol.  
>  Joker karakter arama sorgularında metin analizi yapılmaz. Sorgu zamanında, joker karakter sorgu terimleri arama dizininde analiz edilen koşullara göre karşılaştırılır ve genişletilir.

## <a name="see-also"></a>Ayrıca bkz.  

+ [Belgelerde ara](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Filtreler ve sıralama için OData ifade sözdizimi](query-odata-filter-orderby-syntax.md)   
+ [Azure Search basit sorgu söz dizimi](query-simple-syntax.md)   
