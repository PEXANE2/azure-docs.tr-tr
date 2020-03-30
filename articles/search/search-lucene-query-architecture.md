---
title: Tam metin sorgusu ve dizin oluşturma motoru mimarisi (Lucene)
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama ile ilgili olarak Lucene sorgu işleme ve belge alma kavramlarını tam metin arama için inceler.
manager: nitinme
author: yahnoosh
ms.author: jlembicz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d46d0309b3d2ffb638016e88ba022e49009eedf2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282945"
---
# <a name="how-full-text-search-works-in-azure-cognitive-search"></a>Azure Bilişsel Arama’da tam metin araması nasıl çalışır?

Bu makale, Lucene tam metin aramasının Azure Bilişsel Arama'da nasıl çalıştığını daha iyi anlamanız gereken geliştiriciler içindir. Metin sorguları için, Azure Bilişsel Arama çoğu senaryoda beklenen sonuçları hiç sorunsuz verecektir ama bazen biraz "konu dışı" görünen sonuçlar alabilirsiniz. Böyle durumlarda Lucene sorgu yürütmesinin dört aşaması (sorgu ayrıştırma, sözcük analizi, belge eşleştirme, puanlama) hakkında arka plan bilgilerine sahip olmak, istenen sonucu verecek belirli sorgu parametresi değişikliklerini ve dizin yapılandırmasını belirlemenize yardımcı olabilir. 

> [!Note] 
> Azure Bilişsel Arama tam metin arama için Lucene kullanır, ancak Lucene tümleştirmesi kapsamlı değildir. Azure Bilişsel Arama için önemli senaryoları etkinleştirmek için Lucene işlevini seçici olarak ortaya çıkarır ve genişletiriz. 

## <a name="architecture-overview-and-diagram"></a>Mimariye genel bakış ve diyagram

Tam metin arama sorgusunu işleme, arama terimlerini ayıklamak için sorgu metnini ayrıştırma ile başlar. Arama motoru eşleşen terimlere sahip belgeleri almak için bir dizin kullanır. Bireysel sorgu terimleri bazen bölünüp, potansiyel eşleşme olarak kabul edilebilenler üzerinde daha geniş bir ağ oluşturmak için yeni formlara dönüştürülur. Sonuç kümesi daha sonra her eşleşen belgeye atanan alaka düzeyi puanına göre sıralanır. Sıralanan listenin en üstündekiler arama uygulamasına döndürülür.

Yeniden ifade edilir, sorgu yürütme dört aşaması vardır: 

1. Sorgu ayrıştMa 
2. Sözlü analiz 
3. Belge alma 
4. Puanlama 

Aşağıdaki diyagram, bir arama isteğini işlemek için kullanılan bileşenleri göstermektedir. 

 ![Azure Bilişsel Arama'da Lucene sorgu mimarisi diyagramı][1]


| Başlıca bileşenler | Fonksiyonel açıklama | 
|----------------|------------------------|
|**Sorgu parsers** | Sorgu terimlerini sorgu işleçlerinden ayırın ve arama motoruna gönderilecek sorgu yapısını (sorgu ağacı) oluşturun. |
|**Çözümleyiciler** | Sorgu terimlerinde sözlü çözümleme gerçekleştirin. Bu işlem, sorgu terimlerinin dönüştürülmesini, kaldırılmasını veya genişletilmesini içerebilir. |
|**Dizin oluşturma** | Dizinlenmiş belgelerden ayıklanan aranabilir terimleri depolamak ve düzenlemek için kullanılan verimli bir veri yapısı. |
|**Arama motoru** | Ters gelen dizinin içeriğine göre belgeleri eşleştiren alır ve puanlar. |

## <a name="anatomy-of-a-search-request"></a>Arama isteğinin anatomisi

Arama isteği, bir sonuç kümesinde döndürülmesi gerekenlerin tam bir belirtimidir. En basit haliyle, herhangi bir tkriteri olmayan boş bir sorgudur. Daha gerçekçi bir örnek, büyük olasılıkla bir filtre ifadesi ve sipariş kurallarıyla, belirli alanlara kapsam dahilinde olan parametreleri, birkaç sorgu terimini içerir.  

Aşağıdaki örnek, [REST API'sini](https://docs.microsoft.com/rest/api/searchservice/search-documents)kullanarak Azure Bilişsel Arama'ya gönderilebilirsiniz.  

~~~~
POST /indexes/hotels/docs/search?api-version=2019-05-06
{
    "search": "Spacious, air-condition* +\"Ocean view\"",
    "searchFields": "description, title",
    "searchMode": "any",
    "filter": "price ge 60 and price lt 300",
    "orderby": "geo.distance(location, geography'POINT(-159.476235 22.227659)')", 
    "queryType": "full" 
}
~~~~

Bu istek için, arama motoru aşağıdakileri yapar:

1. Fiyatın en az 60 TL ve 300 TL'den az olduğu belgeleri filtreler.
2. Sorguyu yürütür. Bu örnekte, arama sorgusu tümceciklerden `"Spacious, air-condition* +\"Ocean view\""` ve terimlerden oluşur: (kullanıcılar genellikle noktalama işaretlerini girmez, ancak örnekte dahil olmak üzere çözümleyicilerin bunu nasıl ele alabildiğini açıklamamıza olanak tanır). Bu sorgu için, arama motoru "Okyanus görünümü" içeren belgeler `searchFields` için belirtilen açıklama ve başlık alanlarını ve ayrıca "geniş" terimini veya "klima" önekiyle başlayan terimleri tarar. `searchMode` Parametre, bir terimin açıkça gerekli olmadığı durumlarda herhangi bir terim (varsayılan) veya`+`bunların tümüyle eşleşecek şekilde kullanılır.
3. Ortaya çıkan otel kümesini belirli bir coğrafya konumuna yakınlık la sipariş verir ve ardından arama uygulamasına geri döner. 

Bu makalenin çoğunluğu *arama sorgusunun*işlenmesi `"Spacious, air-condition* +\"Ocean view\""`hakkında: . Filtreleme ve sipariş kapsamı dışındadır. Daha fazla bilgi için [Arama API başvuru belgelerine](https://docs.microsoft.com/rest/api/searchservice/search-documents)bakın.

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a>Aşama 1: Sorgu ayrıştma 

Belirtildiği gibi, sorgu dizesi isteğin ilk satırıdır: 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

Sorgu ayırıcısı işleçleri `*` (örneğin ve `+` örnekte) arama terimlerinden ayırır ve arama sorgusunu desteklenen bir türün *alt sorgularına* dönüştürür: 

+ bağımsız terimler için *terim sorgusu* (geniş gibi)
+ alıntı terimler için *ifade sorgusu* (okyanus görünümü gibi)
+ önek işlecinin `*` izlediği terimler için *önek sorgusu* (klima gibi)

Desteklenen sorgu türlerinin tam listesi için [bkz: Lucene sorgu sözdizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

Bir alt sorguyla ilişkili işleçler, bir belgenin eşleşme olarak kabul edilebilmesi için sorgunun "olması" mı yoksa "olması" mı gerektiğini belirler. Örneğin, `+"Ocean view"` `+` işleç nedeniyle "gerekir" dir. 

Sorgu arayıcı, alt sorguları arama motoruna geçtiği bir *sorgu ağacına* (sorguyu temsil eden iç yapı) yeniden yapılandırır. Sorgu ayrıştının ilk aşamasında, sorgu ağacı aşağıdaki gibi görünür.  

 ![Boolean sorgu searchmode herhangi][2]

### <a name="supported-parsers-simple-and-full-lucene"></a>Desteklenen ayrışdırıcılar: Basit ve Tam Lucene 

 Azure Bilişsel Arama iki farklı `simple` sorgu dili `full`ortaya çıkarır, (varsayılan) ve . Arama isteğinizle parametreyi `queryType` ayarlayarak, işleçleri ve sözdizimini nasıl yorumlayabildiğini bilmesi için sorgu aracını seçeceğiniz sorgu dilini söylersiniz. [Basit sorgu dili](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) sezgisel ve sağlamdır, genellikle kullanıcı girişini istemci tarafı işleme olmadan olduğu gibi yorumlamak için uygundur. Web arama motorlarından tanıdık sorgu operatörlerini destekler. Ayarlayarak `queryType=full`elde ettiğiniz [Tam Lucene sorgu dili,](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)joker karakter, bulanık, regex ve alan kapsamı teşliki gibi daha fazla işleç ve sorgu türü için destek ekleyerek varsayılan Basit sorgu dilini genişletir. Örneğin, Basit sorgu sözdiziminde gönderilen normal bir ifade bir ifade olarak değil, sorgu dizesi olarak yorumlanır. Bu makaledeki örnek istek Tam Lucene sorgu dilini kullanır.

### <a name="impact-of-searchmode-on-the-parser"></a>searchMode'un parser üzerindeki etkisi 

Ayrıştırma etkileyen başka bir arama `searchMode` isteği parametresi parametredir. Boolean sorguları için varsayılan işleci denetler: herhangi bir (varsayılan) veya tüm.  

Varsayılan `searchMode=any`olan, geniş ve klima arasındaki boşluk sınırlayıcı OR (`||`olduğunda, örnek sorgu metnini aşağıdakilere eşdeğer hale getirir: 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

Boolean sorgu `+` yapısında açık işleçler `+"Ocean view"`(terim *eşleşmelidir)* açık bir şekilde kullanılır. Daha az belirgin nasıl kalan terimleri yorumlamak için: geniş ve klima. Arama motoru okyanus manzarası *ve* geniş *ve* klima üzerinde maçlar bulmak gerekir mi? Ya da okyanus manzarası artı kalan *terimlerden birini* bulmalı mı? 

Varsayılan olarak`searchMode=any`( ), arama motoru daha geniş yorumu varsayar. Her iki alan da "veya" anlambilimini yansıtarak *eşleştirilmelidir.* Daha önce gösterilen ilk sorgu ağacı, iki "should" işlemiyle varsayılanı gösterir.  

Şimdi ayarladığımızı `searchMode=all`varsayalım. Bu durumda, boşluk bir "ve" işlemi olarak yorumlanır. Eşleşme olarak nitelendirilebilmek için kalan şartların her ikisinin de belgede bulunması gerekir. Elde edilen örnek sorgu aşağıdaki gibi yorumlanır: 

~~~~
+Spacious,+air-condition*+"Ocean view"
~~~~

Bu sorgu için değiştirilmiş bir sorgu ağacı aşağıdaki gibi olur, eşleşen bir belge üç alt sorgunun da kesişimidir: 

 ![Boolean sorgu searchmode tüm][3]

> [!Note] 
> `searchMode=any` Üzerinde `searchMode=all` seçim en iyi temsilci sorguları çalıştırarak geldi bir karardır. İşleçleri ekleme olasılığı yüksek olan kullanıcılar (belge depolarında `searchMode=all` arama yaparken yaygın olarak kullanılan) boolean sorgu yapılarını bildirirse sonuçları daha sezgisel bulabilirler. Operatörler arasındaki `searchMode` etkileşim hakkında daha fazla şey için [Basit sorgu sözdizimine](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)bakın.

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a>Aşama 2: Sözlü analiz 

Sözlük çözümleyiciler sorgu ağacı yapılandırıldıktan sonra *dönem sorgularını* ve *tümcecik sorgularını* işler. Çözümleyici, arayıcı tarafından kendisine verilen metin girişlerini kabul eder, metni işler ve sorgu ağacına dahil edilecek belirteçli terimleri geri gönderir. 

Sözlü analizin en yaygın biçimi, sorgu terimlerini belirli bir dile özgü kurallara göre dönüştüren *dilbilimsel analizdir:* 

* Sorgu terimini sözcüğün kök biçimine indirgin 
* Temel olmayan sözcüklerin kaldırılması (İngilizce'de "the" veya "ve" gibi stopwords) 
* Bileşik sözcüğü bileşen parçalarına bölme 
* Alt kasa bir büyük harf sözcüğü 

Tüm bu işlemler, kullanıcı tarafından sağlanan metin girişi ile dizinde depolanan terimler arasındaki farkları silme eğilimindedir. Bu tür işlemler metin işlemenin ötesine geçer ve dilin kendisi hakkında derinlemesine bilgi gerektirir. Azure Bilişsel Arama, bu dil seli farkındalığı katmanını eklemek için hem Lucene hem de Microsoft'tan uzun bir [dil çözümleyicileri](https://docs.microsoft.com/rest/api/searchservice/language-support) listesini destekler.

> [!Note]
> Analiz gereksinimleri, senaryonuza bağlı olarak en az ayrıntıya kadar değişebilir. Önceden tanımlanmış çözümleyicilerden birini seçerek veya kendi [özel çözümleyicinizi](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search)oluşturarak sözlü analizin karmaşıklığını kontrol edebilirsiniz. Çözümleyiciler aranabilir alanlara kapsamı vardır ve alan tanımının bir parçası olarak belirtilir. Bu, her alan için ayrı ayrı sözlü çözümleme yapmanızı sağlar. Belirtilmemiş, *standart* Lucene çözümleyici kullanılır.

Örneğimizde, analizden önce, ilk sorgu ağacında "Geniş" terimi, büyük harfli "S" ve sorgu arayıcının sorgu teriminin bir parçası olarak yorumladığı virgül bulunur (virgül sorgu dili işleci olarak kabul edilmez).  

Varsayılan çözümleyici terimi işlediğinde, "okyanus görünümü" ve "geniş" öğesini küçük düşürür ve virgül karakterini kaldırır. Değiştirilen sorgu ağacı aşağıdaki gibi görünecektir: 

 ![Çözümlenmiş terimlerle Boolean sorgusu][4]

### <a name="testing-analyzer-behaviors"></a>Çözümleyici davranışlarını test etme 

Çözümleyicinin davranışı [Analiz API'si](https://docs.microsoft.com/rest/api/searchservice/test-analyzer)kullanılarak test edilebilir. Çözümleyiciye verilen terimlerin hangi terimleri oluşturacağını görmek için çözümlemek istediğiniz metni sağlayın. Örneğin, standart çözümleyicinin "klima" metnini nasıl işleyeceklerini görmek için aşağıdaki isteği iletebilirsiniz:

~~~~
{
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

Standart çözümleyici, giriş metnini aşağıdaki iki belirteçe ayırır ve bunları başlangıç ve bitiş uzaklıkları (isabet vurgulama için kullanılır) ve konumları (ifade eşleştirme için kullanılır) gibi özniteliklerle açıklama lar:

~~~~
{
  "tokens": [
    {
      "token": "air",
      "startOffset": 0,
      "endOffset": 3,
      "position": 0
    },
    {
      "token": "condition",
      "startOffset": 4,
      "endOffset": 13,
      "position": 1
    }
  ]
}
~~~~

<a name="exceptions"></a>

### <a name="exceptions-to-lexical-analysis"></a>Sözlü analizin istisnaları 

Sözlü çözümleme yalnızca tam terimler gerektiren sorgu türleri için geçerlidir – terim sorgusu veya tümcecik sorgusu. Eksik terimleri olan sorgu türleri için geçerli değildir – önek sorgusu, joker karakter sorgusu, regex sorgusu – veya bulanık bir sorgu için. Örneğimizde terimiçeren `air-condition*` önek sorgusu da dahil olmak üzere bu sorgu türleri, çözümleme aşamasını atlayarak doğrudan sorgu ağacına eklenir. Bu tür sorgu terimlerinde gerçekleştirilen tek dönüşüm, düşürmedir.

<a name="stage3"></a>

## <a name="stage-3-document-retrieval"></a>3. Aşama: Belge alma 

Belge alma, dizindeki eşleşen terimlere sahip belgeleri bulma anlamına gelir. Bu aşama en iyi bir örnek le anlaşılır. Aşağıdaki basit şemaya sahip bir otel endeksi ile başlayalım: 

~~~~
{
    "name": "hotels",
    "fields": [
        { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
        { "name": "title", "type": "Edm.String", "searchable": true },
        { "name": "description", "type": "Edm.String", "searchable": true }
    ] 
} 
~~~~

Ayrıca bu dizinin aşağıdaki dört belgeyi içerdiğini varsayalım: 

~~~~
{
    "value": [
        {
            "id": "1",
            "title": "Hotel Atman",
            "description": "Spacious rooms, ocean view, walking distance to the beach."
        },
        {
            "id": "2",
            "title": "Beach Resort",
            "description": "Located on the north shore of the island of Kauaʻi. Ocean view."
        },
        {
            "id": "3",
            "title": "Playa Hotel",
            "description": "Comfortable, air-conditioned rooms with ocean view."
        },
        {
            "id": "4",
            "title": "Ocean Retreat",
            "description": "Quiet and secluded"
        }
    ]
}
~~~~

**Terimler nasıl dizine alınır?**

Alma anlamak için, dizin oluşturma hakkında birkaç temel bilmek yardımcı olur. Depolama birimi, her aranabilir alan için bir tane olan ters bir dizindir. Ters bir dizin içinde tüm belgelerden tüm terimlerin sıralanmış bir listesidir. Her terim, aşağıdaki örnekte açıkça görüldüğü gibi, içinde bulunduğu belgelerin listesine eşler.

Terimleri ters dizinde üretmek için, arama motoru sorgu işleme sırasında olanlara benzer şekilde belgelerin içeriği üzerinde sözlü analiz yapar:

1. *Metin girişleri* çözümleyici yapılandırmasına bağlı olarak bir çözümleyiciye aktarılır, küçük harfli, noktalama işaretli ve benzeri şekilde. 
2. *Belirteçler* metin çözümlemesi çıktısI.
3. *Terimler* dizin eklenir.

Sorgu terimlerinin dizin içindeki terimlere daha çok benzemesi için arama ve dizin oluşturma işlemleri için aynı çözümleyicileri kullanmak yaygındır, ancak gerekli değildir.

> [!Note]
> Azure Bilişsel Arama, ek `indexAnalyzer` ve `searchAnalyzer` alan parametreleri aracılığıyla dizin oluşturma ve arama için farklı çözümleyiciler belirtmenize olanak tanır. Belirtilmemişse, `analyzer` özellik ile çözümleyici kümesi hem dizin oluşturma ve arama için kullanılır.  

**Örneğin belgeler için ters dizin**

Örneğimize dönersek, **başlık** alanı için ters dizini şu şekilde görünür:

| Sözleşme Dönemi | Belge listesi |
|------|---------------|
| atman | 1 |
| Beach | 2 |
| Hotel | 1, 3 |
| Okyanus | 4  |
| Playa | 3 |
| Resort | 3 |
| Geri çekilme | 4 |

Başlık alanında, sadece *otel* iki belge içinde gösterir: 1, 3.

**Açıklama** alanı için dizin aşağıdaki gibidir:

| Sözleşme Dönemi | Belge listesi |
|------|---------------|
| Hava | 3
| ve | 4
| Beach | 1
| Şartlı | 3
| Rahat | 3
| Mesafe | 1
| Adası | 2
| kauaʻi | 2
| Yer almaktadır | 2
| north | 2
| Okyanus | 1, 2, 3
| / | 2
| on |2
| Sessiz | 4
| Oda -larında  | 1, 3
| Tenha | 4
| Shore | 2
| Geniş | 1
| şunu | 1, 2
| - | 1
| görüntüle | 1, 2, 3
| Yürüyüş | 1
| örneklerini şununla değiştirin: | 3


**Sorgu terimlerini dizine eklenmiş terimlerle eşleştirme**

Yukarıdaki ters endeksler göz önüne alındığında, örnek sorguya dönelim ve örnek sorgumuzda eşleşen belgelerin nasıl bulunduğunu görelim. Son sorgu ağacının aşağıdaki gibi göründüğünü hatırlayın: 

 ![Çözümlenmiş terimlerle Boolean sorgusu][4]

Sorgu yürütme sırasında, tek tek sorgular bağımsız olarak aranabilir alanlara karşı yürütülür. 

+ TermQuery, "geniş", belge 1 (Hotel Atman) eşleşir. 

+ PrefixQuery, "klima*", herhangi bir belgeyle eşleşmiyor. 

  Bu, bazen geliştiricilerin kafasını karıştıran bir davranıştır. Klimalı terim belgede bulunmasına rağmen, varsayılan çözümleyici tarafından iki terime bölünür. Kısmi terimler içeren önek sorgularının çözümlenmediğini hatırlayın. Bu nedenle önek "klima" ile terimler ters indeks baktı ve bulunamadı.

+ PhraseQuery, "okyanus görünümü", terimleri "okyanus" ve "görünüm" bakar ve orijinal belgede terimlerin yakınlığını denetler. 1, 2 ve 3 belgeleri açıklama alanında bu sorguyla eşleşir. Bildirim belge 4 başlıkta okyanus terimi vardır ama bir maç olarak kabul etmez, biz tek tek kelimeler yerine "okyanus görünümü" ifade arıyoruz gibi. 

> [!Note]
> Örnek arama isteğinde gösterildiği gibi `searchFields` parametreyle ayarlanan alanları sınırlamadığınız sürece, arama sorgusu Azure Bilişsel Arama dizinindeki tüm aranabilir alanlara karşı bağımsız olarak yürütülür. Seçili alanlardan herhangi birinde eşleşen belgeler döndürülür. 

Bütün olarak, söz konusu sorgu için eşleşen belgeler 1, 2, 3'tür. 

## <a name="stage-4-scoring"></a>Aşama 4: Puanlama  

Arama sonuç kümesindeki her belgeye alaka düzeyi puanı atanır. Alaka düzeyi puanının işlevi, arama sorgusunda ifade edilen kullanıcı sorusunu en iyi yanıtlayan bu belgeleri daha yüksek bir şekilde sıralamaktır. Puan, eşleşen terimlerin istatistiksel özelliklerine göre hesaplanır. Puanlama formülünün özünde [TF/IDF (dönem frekans-ters belge frekansı)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)yer alır. Nadir ve yaygın terimler içeren sorgularda, TF/IDF nadir terimi içeren sonuçları teşvik eder. Örneğin, tüm Vikipedi makaleleri ile varsayımsal bir dizinde, sorgu *başkan*eşleşen belgelerden , *başkan* eşleşen belgeler daha alakalı olarak kabul edilir *.*


### <a name="scoring-example"></a>Puanlama örneği

Örnek sorgumuzla eşleşen üç belgeyi hatırlayın:
~~~~
search=Spacious, air-condition* +"Ocean view"  
~~~~
~~~~
{
  "value": [
    {
      "@search.score": 0.25610128,
      "id": "1",
      "title": "Hotel Atman",
      "description": "Spacious rooms, ocean view, walking distance to the beach."
    },
    {
      "@search.score": 0.08951007,
      "id": "3",
      "title": "Playa Hotel",
      "description": "Comfortable, air-conditioned rooms with ocean view."
    },
    {
      "@search.score": 0.05967338,
      "id": "2",
      "title": "Ocean Resort",
      "description": "Located on a cliff on the north shore of the island of Kauai. Ocean view."
    }
  ]
}
~~~~

Belge 1 sorguyla en iyi eşleşen şekilde eşleşir, çünkü hem *geniş* terim hem de gerekli ifade *okyanus görünümü* açıklama alanında oluşur. Sonraki iki belge yalnızca *ifade okyanus görünümü*eşleşir. Belge 2 ve 3'ün alaka düzeyi puanının, sorguyla aynı şekilde eşleşseler bile farklı olması şaşırtıcı olabilir. Çünkü puanlama formülü sadece TF/IDF'den daha fazla bileşene sahiptir. Bu durumda, tanımı daha kısa olduğundan belge 3 biraz daha yüksek bir puan atanır. Alan uzunluğu nun ve diğer faktörlerin alaka düzeyini nasıl etkileyebileceğini anlamak için [Lucene'nin Pratik Puanlama Formülü](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) hakkında bilgi edinin.

Bazı sorgu türleri (joker karakter, önek, regex) her zaman genel belge puanına sabit bir puan ekler. Bu, sorgu genişletme yoluyla bulunan eşleşmelerin sonuçlara dahil edilmesine, ancak sıralamayı etkilemeden yapılmasına olanak tanır. 

Bunun neden önemli olduğunu gösteren bir örnek. Önek aramaları da dahil olmak üzere joker karakter aramaları tanım olarak belirsizdir, çünkü giriş çok sayıda farklı terimde potansiyel eşleşmelere sahip kısmi bir dizedir ("tur", "tourettes" ve " üzerinde bulunan eşleşmelerle "tur*" girdisini göz önünde bulundurun ve " tourmaline"). Bu sonuçların doğası göz önüne alındığında, hangi terimlerin diğerlerinden daha değerli olduğu konusunda makul bir çıkarı elde etmenin bir yolu yoktur. Bu nedenle, joker karakter, önek ve regex türlerinin sorgularında sonuç alırken terim sıklıklarını yoksak. Kısmi ve tam terimleri içeren çok parçalı bir arama isteğinde, kısmi girişten elde edilen sonuçlar, beklenmeyen olası eşleşmelere karşı önyargıyı önlemek için sabit bir skorla birleştirilir.

### <a name="score-tuning"></a>Puan alamı

Azure Bilişsel Arama'da alaka düzeyi puanlarını ayarlamanın iki yolu vardır:

1. **Puanlama profilleri,** bir dizi kurala dayalı olarak sıralanan sonuçlar listesindeki belgeleri teşvik eder. Örneğimizde, başlık alanında eşleşen belgeleri, açıklama alanında eşleşen belgelerden daha alakalı olarak düşünebiliriz. Ayrıca, bizim endeksi her otel için bir fiyat alanı olsaydı, biz daha düşük fiyat ile belgeleri teşvik edebilir. [Arama dizinine Puanlama Profilleri](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) nasıl ekleyeceğinizi daha fazla öğrenin.
2. **Terim artırma** (yalnızca Full Lucene sorgu sözdiziminde kullanılabilir) sorgu ağacının herhangi bir bölümüne uygulanabilecek bir artırma işleci `^` sağlar. Örneğimizde, önek *klima*\*üzerinde arama yapmak yerine, tam terim *klima* veya öneki aranabilir, ancak tam terimle eşleşen belgeler, terim sorgusuna destek uygulanarak daha yüksek sıralanır: *klima^2|| klima**. [Dönem artırma](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost)hakkında daha fazla bilgi edinin.


### <a name="scoring-in-a-distributed-index"></a>Dağıtılmış dizinde puanlama

Azure Bilişsel Arama'daki tüm dizinler otomatik olarak birden çok parçaya bölünür ve bu da hizmet ölçeğinde veya küçültülme sırasında dizini birden çok düğüm arasında hızla dağıtmamıza olanak tanır. Bir arama isteği yayımlandığında, bağımsız olarak her parçaya karşı verilir. Her parçanın sonuçları daha sonra birleştirilir ve skora göre sıralanır (başka bir sıralama tanımlanmamışsa). Puanlama işlevinin, tüm kırıklar arasında değil, parça içindeki tüm belgelerde ters belge sıklığına karşı dönem frekansını sorguladığını bilmek önemlidir!

Bu, farklı kırıklarda ikamet eden aynı belgeler için alaka düzeyi puanının farklı *olabileceği* anlamına gelir. Neyse ki, dizindeki belge sayısı daha eşit dönem dağılımı nedeniyle arttıkça bu tür farklılıklar kaybolma eğilimindedir. Herhangi bir belgenin hangi parçaya yerleştirileceğini varsaymak mümkün değildir. Ancak, bir belge anahtarının değişmediğini varsayarsak, her zaman aynı parçaya atanır.

Genel olarak, sipariş kararlılığı önemliyse belge siparişi için en iyi özellik belge puanı değildir. Örneğin, aynı puana sahip iki belge göz önüne alındığında, aynı sorgunun sonraki çalıştırmalarında hangisinin ilk olarak görüntülendiğine dair bir garanti yoktur. Belge skoru yalnızca belirlenen sonuçlardaki diğer belgelere göre genel bir belge alaka düzeyi hissi vermelidir.

## <a name="conclusion"></a>Sonuç

Internet arama motorlarının başarısı özel veriler üzerinden tam metin arama için beklentileri yükseltti. Hemen hemen her türlü arama deneyimi için, terimler yanlış yazılmış veya eksik olsa bile, artık motorun niyetimizi anlamasını bekliyoruz. Hatta neredeyse eşdeğer terimlere veya aslında hiç belirtmediğimiz eş anlamlıkelimelere dayalı eşleşmeler bekleyebiliriz.

Teknik açıdan bakıldığında, tam metin arama son derece karmaşıktır, karmaşık dilbilimsel analiz ve ilgili bir sonuç sunmak için sorgu terimleri damıtma, genişletmek ve dönüştürmek yollarla işleme sistematik bir yaklaşım gerektiren. Doğal karmaşıklıklar göz önüne alındığında, bir sorgunun sonucunu etkileyebilecek birçok etken vardır. Bu nedenle, tam metin arama mekaniği anlamak için zaman yatırım beklenmeyen sonuçlar üzerinden çalışmaya çalışırken somut faydalar sağlar.  

Bu makalede, Azure Bilişsel Arama bağlamında tam metin arama araştırılmıştır. Bunun, olası sorgu sorunlarını gidermek için olası nedenleri ve çözümleri tanımak için yeterli arka plan sağladığını umuyoruz. 

## <a name="next-steps"></a>Sonraki adımlar

+ Örnek dizini oluşturun, farklı sorguları deneyin ve sonuçları gözden geçirin. Yönergeler için [bkz.](search-get-started-portal.md#query-index)

+ [Arama Belgeleri](https://docs.microsoft.com/rest/api/searchservice/search-documents#bkmk_examples) örnek bölümünden veya portaldaki Arama gezginindeki [Basit sorgu sözdiziminden](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) ek sorgu sözdizimini deneyin.

+ Arama uygulamanızda sıralamayı ayarlamak istiyorsanız [puanlama profillerini](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) gözden geçirin.

+ Dile özgü [sözlü analizörlerin](https://docs.microsoft.com/rest/api/searchservice/language-support)nasıl uygulanacağı öğrenin.

+ Belirli alanlarda en az işleme veya özel işleme için [özel çözümleyicileri yapılandırın.](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search)

## <a name="see-also"></a>Ayrıca bkz.

[Belgelerde Arama REST API'si](https://docs.microsoft.com/rest/api/searchservice/search-documents) 

[Basit sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 

[Tam Lucene sorgu söz dizimi](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 

[Arama sonuçlarını işleme](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
