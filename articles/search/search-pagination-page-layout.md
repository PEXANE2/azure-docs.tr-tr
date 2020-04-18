---
title: Arama sonuçlarıyla çalışma
titleSuffix: Azure Cognitive Search
description: Arama sonuçlarını yapılayın ve sıralayın, belge sayısı alın ve Azure Bilişsel Arama'daki arama sonuçlarına içerik gezintisi ekleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 0f815003449f0600bce1cb8927b92b85b51b09a1
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641614"
---
# <a name="how-to-work-with-search-results-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da arama sonuçlarıyla çalışma

Bu makalede, eşleşen belgelerin toplam sayısı, paginated sonuçlar, sıralanmış sonuçlar ve isabet vurgulanan terimler ile geri gelen bir sorgu yanıtı almak için nasıl açıklanır.

Yanıtın yapısı sorgudaki parametrelere göre belirlenir: REST API'deki [Arama Belgesi](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) veya .NET SDK'daki [DocumentSearchResult Class.](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1)

## <a name="result-composition"></a>Sonuç kompozisyonu

Bir arama belgesi çok sayıda alandan oluşsa da, sonuç kümesindeki her belgeyi temsil etmek için genellikle yalnızca birkaç tane gerekir. Sorgu isteğinde, yanıtta hangi alanların görünün ğünü belirtmek için ekin. `$select=<field list>` Bir alanın bir sonuca dahil edilebilmek için dizinde **Alınabilen** olarak atfedilmesi gerekir. 

En iyi çalışan alanlar arasında, kullanıcı adına bir tıklama yanıtı davet etmek için yeterli bilgi sağlayan, belgeler arasında kontrast ve farklılaşma bulunan alanlar yer almaktadır. Bir e-ticaret sitesinde, bir ürün adı, açıklama, marka, renk, boyut, fiyat ve derecelendirme olabilir. Otellerin örnek endeksi yerleşik örneği için aşağıdaki örnekteki alanlar olabilir:

```http
POST /indexes/hotels-sample-index/docs/search?api-version=2019-05-06 
    {  
      "search": "sandy beaches",
      "select": "HotelId, HotelName, Description, Rating, Address/City"
      "count": true
    }
```

> [!NOTE]
> Resim dosyalarını ürün fotoğrafı veya logo gibi bir sonuca eklemek istiyorsanız, bunları Azure Bilişsel Arama'nın dışında saklayın, ancak arama belgesindeki resim URL'sine başvurmak için dizininize bir alan ekleyin. Sonuçlarda görüntüleri destekleyen örnek dizinler **emlak-örnek-us** demo, bu [quickstart](search-create-app-portal.md)özellikli ve [New York City Jobs demo uygulaması](https://aka.ms/azjobsdemo)içerir.

## <a name="paging-results"></a>Disk belleği sonuçları

Varsayılan olarak, arama motoru, sorgu tam metin arama ysa veya tam eşleme sorguları için rasgele bir sırada arama puanına göre belirlendiği gibi ilk 50 eşleşmeye kadar geri döner.

Farklı sayıda eşleşen belge döndürmek `$top` için, sorgu isteğine parametre ekleyin ve `$skip` parametreler ekleyin. Aşağıdaki liste mantığı açıklar.

+ Bir `$count=true` dizin içindeki eşleşen belgelerin toplam sayısının sayısını almak için ekleyin.

+ 15 eşleşen belgenin ilk kümesini ve toplam eşleşme sayısını döndürün:`GET /indexes/<INDEX-NAME>/docs?search=<QUERY STRING>&$top=15&$skip=0&$count=true`

+ Sonraki 15 almak için ilk 15 atlayarak, `$top=15&$skip=15`ikinci seti döndürün: . 15 üçüncü seti için aynı şeyi yapın:`$top=15&$skip=30`

Altta yatan dizin değişiyorsa, paginated sorguların sonuçlarının kararlı olacağı garanti edilmez. Sayfalama her sayfanın `$skip` değerini değiştirir, ancak her sorgu bağımsızdır ve sorgu sırasında dizinde var olduğu için verilerin geçerli görünümünde çalışır (diğer bir deyişle, genel amaçlı bir veritabanında bulunanlar gibi sonuçların önbelleğe alma veya anlık görüntüsü yoktur).
 
Aşağıda, yinelemeleri nasıl alabileceğinize bir örnek verilmiştir. Dört belgeiçeren bir dizin varsayalım:

    { "id": "1", "rating": 5 }
    { "id": "2", "rating": 3 }
    { "id": "3", "rating": 2 }
    { "id": "4", "rating": 1 }
 
Şimdi, sonuçların derecelendirmeye göre sıralanmış olarak bir defada ikişer döndürülmesini istediğinizi varsayalım. Sonuçların ilk sayfasını almak için bu sorguyu yürütebilirsin: `$top=2&$skip=0&$orderby=rating desc`, aşağıdaki sonuçları üreterek:

    { "id": "1", "rating": 5 }
    { "id": "2", "rating": 3 }
 
Hizmette, sorgu çağrıları arasında dizine beşinci bir belge `{ "id": "5", "rating": 4 }`nin eklenmiştir varsayalım: .  Kısa bir süre sonra, ikinci sayfayı almak `$top=2&$skip=2&$orderby=rating desc`için bir sorgu yürütmek: , ve aşağıdaki sonuçları almak:

    { "id": "2", "rating": 3 }
    { "id": "3", "rating": 2 }
 
Belge 2'nin iki kez getirili olduğuna dikkat edin. Bunun nedeni, yeni belge 5'in derecelendirme için daha büyük bir değere sahip olmasıdır, bu nedenle belge 2'den önce sıralanır ve ilk sayfaya iner. Bu davranış beklenmeyen olsa da, arama motorunun nasıl davrandığının tipik bir sonucuvardır.

## <a name="ordering-results"></a>Sonuçları sıralama

Tam metin arama sorguları için sonuçlar, bir belgedeki terim sıklığı ve yakınlığına göre hesaplanan bir arama puanına göre otomatik olarak sıralanır ve daha yüksek puanlar bir arama teriminde daha fazla veya daha güçlü eşleşmelere sahip belgelere giderek sıralanır. 

Arama puanları, aynı sonuç kümesindeki diğer belgelerle karşılaştırıldığında eşleşmenin gücünü yansıtan genel alaka düzeyi duygusunu taşır. Puanlar her zaman bir sorgudan diğerine tutarlı değildir, bu nedenle sorgularla çalışırken, arama belgelerinin nasıl sıralandırılına İlişkin küçük tutarsızlıklar fark edebilirsiniz. Bunun neden oluşabileceğinin çeşitli açıklamaları vardır.

| Nedeni | Açıklama |
|-----------|-------------|
| Veri volatilitesi | Dizin içeriği, belgeleri ekledikçe, değiştirdikçe veya silerken değişir. Dizin güncelleştirmeleri zaman içinde işlendikçe dönem frekansları değişir ve eşleşen belgelerin arama puanlarını etkiler. |
| Birden çok yineleme | Birden çok yineleme kullanan hizmetler için sorgular her yinelemeye paralel olarak verilir. Arama puanını hesaplamak için kullanılan dizin istatistikleri, yineleme başına hesaplanır ve sonuçlar sorgu yanıtında birleştirilir ve sıralanır. Yinelemeler çoğunlukla birbirlerinin aynalarıdır, ancak durumdaki küçük farklılıklar nedeniyle istatistikler farklılık gösterir. Örneğin, bir yineleme, istatistiklerine katkıda bulunan ve diğer yinelemelerden birleştirilen belgeleri silmiş olabilir. Genellikle, çoğaltma başına istatistikteki farklılıklar daha küçük dizinlerde daha belirgindir. |
| Aynı skorlar | Birden çok belge aynı puana sahipse, bunlardan herhangi biri önce görünebilir.  |

### <a name="consistent-ordering"></a>Tutarlı sıralama

Sonuç sıralamasındaki esneklik göz önüne alındığında, tutarlılık bir uygulama gereksinimiyse diğer seçenekleri de keşfetmek isteyebilirsiniz. En kolay yaklaşım, derecelendirme veya tarih gibi bir alan değerine göre sıralamadır. Derecelendirme veya tarih gibi belirli bir alana göre sıralamak istediğiniz senaryolar için, Sıralanabilir olarak dizine eklenmiş herhangi bir alana uygulanabilecek bir [ `$orderby` ifadeyi](query-odata-filter-orderby-syntax.md)açıkça **tanımlayabilirsiniz.**

Başka bir seçenek özel bir [puanlama profili](index-add-scoring-profiles.md)kullanıyor. Puanlama profilleri, belirli alanlarda bulunan eşleşmeleri artırma olanağıyla, arama sonuçlarındaki öğelerin sıralaması üzerinde daha fazla denetim sağlar. Ek puanlama mantığı, her belgenin arama puanları birbirinden daha uzak olduğundan, yinelemeler arasındaki küçük farklılıkları geçersiz kılmaya yardımcı olabilir. Bu yaklaşım için [sıralama algoritmasını](index-ranking-similarity.md) öneririz.

## <a name="hit-highlighting"></a>İsabet vurgulama

Hit vurgulama, sonuç olarak eşleşen terime uygulanan metin biçimlendirme (kalın veya sarı vurgular gibi) anlamına gelir ve bu da eşleşmeyi fark etmeyi kolaylaştırır. Hit vurgulama yönergeleri sorgu [isteğinde](https://docs.microsoft.com/rest/api/searchservice/search-documents)sağlanır. Arama motoru eşleşen terimi etiketlere dahil `highlightPreTag` `highlightPostTag`eder ve kodunuz yanıtı işler (örneğin, kalın bir yazı tipi uygular).

Biçimlendirme tüm terim sorgularına uygulanır. Aşağıdaki örnekte, Açıklama alanı içinde bulunan "kumlu", "kum", "plajlar", "plaj" terimleri vurgulamak için etiketlenir. Bulanık ve joker karakter araması gibi motorda sorgu genişletmesini tetikleyen sorguların isabet vurgulama desteği sınırlıdır.

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

### <a name="new-behavior-starting-july-15"></a>Yeni davranış (15 Temmuz'dan itibaren)

15 Temmuz 2020'den sonra oluşturulan hizmetler farklı bir vurgulama deneyimi sağlayacaktır. Bu tarihten önce oluşturulan hizmetler, vurgulama davranışlarında değişmez. 

Yeni davranış ile:

* Yalnızca tam tümcecik sorgusuyla eşleşen tümcecikler döndürülür. Sorgu "super bowl" bu gibi olayları dönecektir:

    ```html
    '<em>super bowl</em> is super awesome with a bowl of chips'
    ```
  Tam ifadeyle eşleşmediği için talaş ların terim *kasesi* herhangi bir vurgulama yok.
  
* Vurgu için döndürülen parça boyutunu belirtmek mümkün olacaktır. Parça boyutu karakter sayısı olarak belirtilir (en fazla 1000 karakter).

Isabet vurgulamayı uygulayan istemci kodu yazarken, bu değişikliğin farkında olun. Tamamen yeni bir arama hizmeti oluşturmadığınız sürece bunun sizi etkilemeyeceğini unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

İstemciniz için hızlı bir arama sayfası oluşturmak için şu seçenekleri göz önünde bulundurun:

+ [Uygulama Jeneratör](search-create-app-portal.md), portalda, bir arama çubuğu, yönlü navigasyon ve görüntüleri içeren sonuç alanı ile bir HTML sayfası oluşturur.
+ [C# ilk uygulama oluşturun](tutorial-csharp-create-first-app.md) fonksiyonel bir istemci oluşturur bir öğretici. Örnek kod paginated sorguları, hit vurgulama ve sıralama gösterir.

Çeşitli kod örnekleri burada bulabilirsiniz bir web ön uç arayüzü, şunlardır: [New York City Jobs demo uygulaması](https://aka.ms/azjobsdemo), Canlı bir demo sitesi ile [JavaScript örnek kodu](https://github.com/liamca/azure-search-javascript-samples), ve [CognitiveSearchFrontEnd](https://github.com/LuisCabrer/CognitiveSearchFrontEnd).
