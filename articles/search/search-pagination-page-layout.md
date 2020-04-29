---
title: Arama sonuçlarıyla çalışma
titleSuffix: Azure Cognitive Search
description: Arama sonuçlarını yapı ve sıralama, bir belge sayısı edinme ve Azure Bilişsel Arama arama sonuçlarına içerik gezintisi ekleme.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 0f815003449f0600bce1cb8927b92b85b51b09a1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81641614"
---
# <a name="how-to-work-with-search-results-in-azure-cognitive-search"></a>Azure Bilişsel Arama arama sonuçlarıyla çalışma

Bu makalede, toplam eşleşen belge sayısı, sayfalandırılmış sonuçlar, sıralanmış sonuçlar ve isabet vurgulu terimler ile geri gelen bir sorgu yanıtının nasıl alınacağı açıklanmaktadır.

Bir yanıtın yapısı, sorgudaki parametrelere göre belirlenir: REST API veya .NET SDK 'da [Documentsearchresult sınıfında](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1) [Arama belgesi](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) .

## <a name="result-composition"></a>Sonuç oluşturma

Bir arama belgesi çok sayıda alandan oluşabilir, genellikle sonuç kümesindeki her bir belgeyi temsil etmek için yalnızca birkaç tane gerekir. Bir sorgu isteğinde, yanıtta hangi `$select=<field list>` alanların gösterileceğini belirtmek için sonuna ekleyin. Bir alan, bir sonuca dahil edilecek dizinde **alınabilir** olarak belirtilmelidir. 

En iyi şekilde çalışan alanlar, Kullanıcı bölümünde bir tıklama yanıtı davet etmek için yeterli bilgi sağlayan ve belgeler arasında ayrım yapan ve bunları ayırt eden alanlardır. Bir e-ticaret sitesinde bir ürün adı, açıklama, marka, renk, boyut, Fiyat ve derecelendirme olabilir. Oteller-örnek dizini yerleşik örneği için aşağıdaki örnekteki alanlar olabilir:

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2019-05-06 
    {  
      "search": "sandy beaches",
      "select": "HotelId, HotelName, Description, Rating, Address/City"
      "count": true
    }
```

> [!NOTE]
> Görüntü dosyalarını bir ürün fotoğrafı veya logosu gibi bir sonuca eklemek istiyorsanız, bunları Azure Bilişsel Arama dışında depolayın, ancak arama belgesinde görüntü URL 'sine başvurmak için dizininizdeki bir alanı ekleyin. Sonuçlarda görüntüleri destekleyen örnek dizinler, bu [hızlı](search-create-app-portal.md)başlangıçta sunulan **reatastate-Sample-US** tanıtımı ve [New York City işleri tanıtım uygulaması](https://aka.ms/azjobsdemo)' nı içerir.

## <a name="paging-results"></a>Disk belleği sonuçları

Varsayılan olarak, arama altyapısı, sorgu tam metin aramasında veya tam eşleştirme sorguları için rastgele bir düzende, arama puanı tarafından belirlendiği şekilde ilk 50 eşleştirmeye kadar geri döner.

Farklı sayıda eşleşen belge döndürmek için sorgu isteğine ve `$top` `$skip` parametreleri ekleyin. Aşağıdaki listede Logic açıklanmaktadır.

+ Bir `$count=true` dizin içindeki toplam eşleşen belge sayısının sayısını almak için ekleyin.

+ İlk 15 eşleşen belge kümesini ve toplam eşleşme sayısını döndürün:`GET /indexes/<INDEX-NAME>/docs?search=<QUERY STRING>&$top=15&$skip=0&$count=true`

+ İkinci kümeyi döndürün, sonraki 15 ' i almak için ilk 15 ' i atlayarak `$top=15&$skip=15`:. Üçüncü 15 kümesi için aynısını yapın:`$top=15&$skip=30`

Temel alınan dizin değiştirilirken sayfalandırılmış sorguların sonuçlarının kararlı olmaması garanti edilmez. Sayfalama, her bir `$skip` sayfa için değerini değiştirir, ancak her sorgu bağımsızdır ve sorgu zamanında dizinde olduğu gibi verilerin geçerli görünümü üzerinde çalışır (başka bir deyişle, bir genel amaçlı veritabanında bulunanlar gibi, sonuçların önbelleğe alınması veya anlık görüntüsü yoktur).
 
Yinelemeleri nasıl alabileceğiniz hakkında bir örnek aşağıda verilmiştir. Dört belge içeren bir dizin varsayın:

    { "id": "1", "rating": 5 }
    { "id": "2", "rating": 3 }
    { "id": "3", "rating": 2 }
    { "id": "4", "rating": 1 }
 
Şimdi sonuçların, derecelendirmeye göre sıralanmış olarak her seferinde bir kez döndürülmesini istediğinizi varsayın. Sonuçların ilk sayfasını almak için bu sorguyu yürütülecektir: `$top=2&$skip=0&$orderby=rating desc`, aşağıdaki sonuçları üretir:

    { "id": "1", "rating": 5 }
    { "id": "2", "rating": 3 }
 
Hizmette, sorgu çağrıları arasındaki dizine beşinci bir belge eklendiğini varsayalım: `{ "id": "5", "rating": 4 }`.  Kısa süre sonra, ikinci sayfayı `$top=2&$skip=2&$orderby=rating desc`getirmek için bir sorgu yürütüyoruz ve şu sonuçları elde edersiniz:

    { "id": "2", "rating": 3 }
    { "id": "3", "rating": 2 }
 
Belge 2 ' nin iki kez getirildiğine dikkat edin. Bunun nedeni, yeni belge 5 ' in derecelendirme için daha büyük olması, bu yüzden belge 2 ' den önce ve ilk sayfada yer bırakmadan önce sıralanır. Bu davranış beklenmeyen bir durum olsa da, bir arama altyapısının nasıl davrandığı normaldir.

## <a name="ordering-results"></a>Sonuçları sıralama

Tam metin arama sorguları için, sonuçlar, bir arama puanı tarafından otomatik olarak derecelendirilir, daha yüksek puanlar bir arama terimi üzerinde daha fazla veya daha fazla eşleşme elde eden belgeler. 

Arama puanları, aynı sonuç kümesindeki diğer belgelerle karşılaştırıldığında eşleşme gücünü yansıtan genel ilgi hissi elde ediyor. Puanlar bir sorgudan sonrakine her zaman tutarlı değildir. bu nedenle, sorgularla çalışırken arama belgelerinin nasıl sıralandığına ilişkin küçük tutarsızlıklar fark edebilirsiniz. Bunun neden olabileceği hakkında birkaç açıklamalar vardır.

| Nedeni | Açıklama |
|-----------|-------------|
| Veri Vola | Belge eklerken, değiştirirken veya silerken Dizin içeriği farklılık gösterir. Dizin güncelleştirmeleri zaman içinde işlenirken, eşleşen belgelerin arama puanlarını etkileyen terim frekansları değişir. |
| Birden çok çoğaltma | Birden çok çoğaltma kullanan hizmetler için sorgular her bir çoğaltmaya paralel olarak verilir. Bir arama Puanını hesaplamak için kullanılan dizin istatistikleri çoğaltma temelinde hesaplanır, sonuçlar birleştirilir ve sorgu yanıtında sıralanır. Çoğaltmalar genellikle birbirleriyle yansıtılıyordur, ancak durum bakımından küçük farklılıklar nedeniyle istatistikler farklılık gösterebilir. Örneğin, bir çoğaltma, diğer çoğaltmalardan birleştirilmiş olan, istatistiklere katkıda bulunan belgeleri silmiş olabilir. Genellikle, çoğaltma başına istatistikteki farklılıklar daha küçük dizinlerde daha belirgin olur. |
| Özdeş puanlar | Birden çok belgede aynı puan varsa, bunlardan biri önce görünebilir.  |

### <a name="consistent-ordering"></a>Tutarlı sıralama

Sonuç sıralamasına göre esneklik verildiğinde, tutarlılık bir uygulama gereksinimidir, diğer seçenekleri incelemek isteyebilirsiniz. En kolay yaklaşım, derecelendirme veya tarih gibi bir alan değerine göre sıralıyor. Derecelendirme veya tarih gibi belirli bir alana göre sıralamak istediğiniz senaryolar için, **sıralanabilir**olarak dizini oluşturulmuş herhangi bir alana uygulanabilen bir [ `$orderby` ifadeyi](query-odata-filter-orderby-syntax.md)açıkça tanımlayabilirsiniz.

Başka bir seçenek de [özel bir Puanlama profili](index-add-scoring-profiles.md)kullanıyor. Puanlama profilleri, belirli alanlarda bulunan eşleşmeleri artırma özelliği sayesinde, arama sonuçlarında öğelerin derecelendirmesi üzerinde daha fazla denetim sağlar. Ek Puanlama mantığı, her belge için arama puanları birbirinden farklı olduğundan çoğaltmalar arasındaki küçük farklılıkları geçersiz kılmaya yardımcı olabilir. Bu yaklaşım için [Derecelendirme algoritmasını](index-ranking-similarity.md) öneririz.

## <a name="hit-highlighting"></a>İsabet vurgulama

İsabet vurgulama, bir sonuçla eşleşen terime uygulanan metin biçimlendirmesine (kalın veya sarı vurgular gibi) başvurur, bu da eşleştirmeyi daha kolay hale getirir. [Sorgu isteğinde](https://docs.microsoft.com/rest/api/searchservice/search-documents)isabet vurgulama yönergeleri verilmiştir. Arama altyapısı, eşleşen terimi Etiketler ' `highlightPreTag` de, ve `highlightPostTag`kodunuz ise yanıtı işler (örneğin, kalın yazı tipi uygulama).

Biçimlendirme, tüm terim sorgularına uygulanır. Aşağıdaki örnekte, açıklama alanı içindeki "Sandy", "kum", "kirekler", "plaj" terimleri vurgulama için etiketlendi. Benzer ve joker karakter arama gibi altyapıda sorgu genişletmeyi tetikleyen sorgular, isabet vurgulama için sınırlı desteğe sahiptir.

```http
GET /indexes/hotels-sample-index/docs/search=sandy beaches&highlight=Description?api-version=2019-05-06 
```

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2019-05-06 
    {  
      "search": "sandy beaches",  
      "highlight": "Description"
    }
```

### <a name="new-behavior-starting-july-15"></a>Yeni davranış (15 Temmuz 'dan itibaren)

15 Temmuz 2020 ' den sonra oluşturulan hizmetler, farklı bir vurgulama deneyimi sağlar. Bu tarihten önce oluşturulan hizmetler, vurgulama davranışlarına göre değişmeyecektir. 

Yeni davranışla:

* Yalnızca tam tümcecik sorgusuyla eşleşen ifadeler döndürülür. "Super una" sorgusu şöyle sonuç verecektir:

    ```html
    '<em>super bowl</em> is super awesome with a bowl of chips'
    ```
  *Yongalar* teriminin, tam tümcecikle eşleşmediğinden hiçbir vurgulamaya sahip olmadığını unutmayın.
  
* Vurgu için döndürülen parça boyutunu belirtmek mümkün olacaktır. Parça boyutu karakter sayısı olarak belirtilir (en fazla 1000 karakter).

İsabet vurgulama uygulayan istemci kodu yazarken, bu değişikliği unutmayın. Tamamen yeni bir arama hizmeti oluşturmadığınız takdirde bunu etkilemeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

İstemcinizi hızlı bir şekilde bir arama sayfası oluşturmak için aşağıdaki seçenekleri göz önünde bulundurun:

+ [Uygulama Oluşturucu](search-create-app-portal.md), portalda bir arama çubuğu, çok yönlü gezinme ve görüntü içeren sonuç alanı IÇEREN bir HTML sayfası oluşturur.
+ [C# ' de ilk uygulamanızı oluşturma](tutorial-csharp-create-first-app.md) işlevsel bir istemci oluşturan bir öğreticidir. Örnek kod, sayfalandırılmış sorguları, isabet vurgulamasını ve sıralamayı gösterir.

Birçok kod örneği, burada bulabileceğiniz bir Web ön uç arabirimi içerir: [New York City işleri tanıtım uygulaması](https://aka.ms/azjobsdemo), [canlı tanıtım sitesiyle JavaScript örnek kodu](https://github.com/liamca/azure-search-javascript-samples)ve [bilivesearchön uç](https://github.com/LuisCabrer/CognitiveSearchFrontEnd).
