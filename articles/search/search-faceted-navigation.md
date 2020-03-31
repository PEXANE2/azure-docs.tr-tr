---
title: Yönlü gezinti kategorisi hiyerarşisi ekleme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama ile tümleşen arama uygulamalarında kendi kendini yöneten filtreleme için yönlü gezinme ekleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 5f4435ca213584fff84f3ddad9bda6f7e06628a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283166"
---
# <a name="how-to-implement-faceted-navigation-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da yönlü gezinme nasıl uygulanır?

Yönlü gezinme, arama uygulamalarında kendi kendine yönlendirilen ayrıntılı gezinme sağlayan bir filtreleme mekanizmasıdır. 'Yönlü navigasyon' terimi yabancı olabilir, ancak muhtemelen daha önce kullanmışsınızdır. Aşağıdaki örnekte de görüldüğü gibi, yönlü gezinme, sonuçları filtrelemek için kullanılan kategorilerden başka bir şey değildir.

 ![Azure Bilişsel Arama İş Portalı Demo](media/search-faceted-navigation/azure-search-faceting-example.png "Azure Bilişsel Arama İş Portalı Demo")

Yönlü navigasyon aramak için alternatif bir giriş noktasıdır. Karmaşık arama ifadelerini elle yazmak için kullanışlı bir alternatif sunar. Facets, aradığınızı bulmanıza yardımcı olurken, sıfır sonuç almamanızı sağlar. Bir geliştirici olarak, facets arama dizini gezinmek için en yararlı arama ölçütleri ortaya çıkarmanızı sağlar. Çevrimiçi perakende uygulamalarında, yönlü navigasyon genellikle markalar, departmanlar (çocuk ayakkabıları), boyut, fiyat, popülerlik ve derecelendirmeler üzerine kuruludur. 

Yönlü gezinmenin uygulanması, arama teknolojilerinde farklılık gösterir. Azure Bilişsel Arama'da, daha önce şemanızda atfettiğiniz alanları kullanarak sorgu sırasında yüzle gezinme oluşturulur.

-   Uygulamanızın oluşturduğu sorgularda, söz leziz belge sonuç kümesi için kullanılabilir faset filtre değerlerini almak için bir sorgunun *facet sorgu parametrelerini* göndermesi gerekir.

-   Belge sonuç kümesini gerçekten kırpmak için `$filter` uygulamanın bir ifade de uygulaması gerekir.

Uygulama geliştirmenizde, sorguları oluşturan kod yazmak işin büyük kısmını oluşturur. Yönlü gezinmeden beklediğiniz uygulama davranışlarının çoğu, aralıkları tanımlamak ve farklı sonuçlar için sayımlar almak için yerleşik destek de dahil olmak üzere hizmet tarafından sağlanır. Hizmet ayrıca hantal gezinme yapıları önlemek yardımcı mantıklı varsayılanlar içerir. 

## <a name="sample-code-and-demo"></a>Örnek kod ve demo
Bu makalede, örnek olarak bir iş arama portalı kullanır. Örnek, ASP.NET Bir MVC uygulaması olarak uygulanır.

- Azure Bilişsel Arama İş [Portalı Demo'da](https://aka.ms/azjobsdemo)çalışan demoyu çevrimiçi görün ve test edin.

- [Kodu GitHub'daki Azure Örnekleri reposundan](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)indirin.

## <a name="get-started"></a>Kullanmaya başlayın
Eğer geliştirme aramak için yeni iseniz, yönlü navigasyon düşünmek için en iyi yolu, kendi kendine yönlendirilmiş arama için olasılıkları gösterir olmasıdır. Arama sonuçlarını nokta ve tıklatma eylemleri yoluyla hızla daraltma için kullanılan önceden tanımlanmış filtrelere dayalı bir arama deneyimi türüdür. 

### <a name="interaction-model"></a>Etkileşim modeli

Yönlü gezinme için arama deneyimi yinelemeli, bu nedenle kullanıcı eylemlerine yanıt olarak ortaya çıkan sorgular dizisi olarak anlayarak başlayalım.

Başlangıç noktası, genellikle çevreye yerleştirilen, yönlü gezinme sağlayan bir uygulama sayfasıdır. Yönlü gezinme genellikle her değer için onay kutuları veya tıklatılabilir metin içeren bir ağaç yapısıdır. 

1. Azure Bilişsel Arama'ya gönderilen bir sorgu, bir veya daha fazla yönlü sorgu parametresi aracılığıyla yönlü gezinme yapısını belirtir. Örneğin, `facet=Rating`sorgu, sunuyu daha `:values` da `:sort` hassaslaştırmak için belki bir veya seçenek içerebilir.
2. Sunu katmanı, istekte belirtilen yönleri kullanarak, yönlü gezinme sağlayan bir arama sayfası işler.
3. Derecelendirme içeren bir yönlü navigasyon yapısı göz önüne alındığında, yalnızca 4 veya daha yüksek derecelendirmeye sahip ürünlerin gösterilmesi gerektiğini belirtmek için "4"ü tıklatın. 
4. Buna karşılık, uygulama içeren bir sorgu gönderir`$filter=Rating ge 4` 
5. Sunu katmanı, yalnızca yeni ölçütleri karşılayan öğeleri içeren azaltılmış bir sonuç kümesini göstererek sayfayı güncelleştirir (bu durumda, 4 ve yukarı derecelendirilmiş ürünler).

Faset bir sorgu parametresitir, ancak sorgu girişiyle karıştırmayın. Bir sorguda seçim ölçütleri olarak asla kullanılmaz. Bunun yerine, facet sorgu parametrelerini yanıtta geri gelen gezinti yapısına giriş olarak düşünün. Azure Bilişsel Arama, sağladığınız her bir yönlü sorgu parametresi için, her bir faset değeri için kısmi sonuçlarda kaç belge olduğunu değerlendirir.

Adım `$filter` 4'e dikkat edin. Filtre, yönlü gezinmenin önemli bir yönüdür. Fatektler ve filtreler API'de bağımsız olsa da, istediğiniz deneyimi sunmak için her ikisine de ihtiyacınız vardır. 

### <a name="app-design-pattern"></a>Uygulama tasarım deseni

Uygulama kodunda, desen fason sonuçlarla birlikte yönlü gezinme yapısını ve $filter ifadesini döndürmek için facet sorgu parametrelerini kullanmaktır.  Filtre ifadesi, tıklama olayını fason değerinde işler. İfadeyi, `$filter` sunu katmanına döndürülen arama sonuçlarının gerçek kırpmasının arkasındaki kod olarak düşünün. Renkler façesi göz önüne alındığında, Kırmızı `$filter` renk tıklatma yalnızca kırmızı renk temalı öğeleri seçen bir ifade aracılığıyla uygulanır. 

### <a name="query-basics"></a>Sorgu temelleri

Azure Bilişsel Arama'da, bir veya daha fazla sorgu parametresi aracılığıyla bir istek belirtilir (her birinin açıklaması için [Arama Belgeleri'ne](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) bakın). Sorgu parametrelerinin hiçbiri gerekmez, ancak bir sorgunun geçerli olabilmesi için en az bir tane niz olmalıdır.

Alakasız isabetleri filtreleme yeteneği olarak anlaşılan kesinlik, aşağıdaki ifadelerden biri veya her ikisi aracılığıyla elde edilir:

-   **arama=**  
    Bu parametrenin değeri arama ifadesini oluşturur. Tek bir metin parçası veya birden çok terim ve işleç içeren karmaşık bir arama ifadesi olabilir. Sunucuda, tam metin arama için bir arama ifadesi kullanılır, terimleri eşleştirmek için dizindeki aranabilir alanları sorgular, sıralama sırasına göre sonuçları döndürün. Null ayarlarsanız, `search` sorgu yürütme tüm dizin (yani, `search=*`üzerindedir). Bu durumda, `$filter` sorgunun a veya puanlama profili gibi diğer öğeleri, hangi belgelerin `($filter`döndürülen birincil`scoringProfile` faktörlerdir ) ve hangi sırada ( veya ). `$orderby`

-   **$filter=**  
    Filtre, belirli belge özniteliklerinin değerlerini temel alan arama sonuçlarının boyutunu sınırlamak için güçlü bir mekanizmadır. A `$filter` önce değerlendirilir, ardından her değer için kullanılabilir değerleri ve karşılık gelen sayıları oluşturan yönlü leştirme mantığı

Karmaşık arama ifadeleri sorgunun performansını azaltır. Mümkün olduğunda, hassasiyeti artırmak ve sorgu performansını artırmak için iyi oluşturulmuş filtre ifadeleri kullanın.

Filtrenin nasıl daha fazla kesinlik ekladığını daha iyi anlamak için, karmaşık bir arama ifadesini filtre ifadesi içeren ifadeyle karşılaştırın:

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq 'Seattle' and Parking and Type ne 'motel'`

Her iki sorgu da geçerlidir, ancak Seattle'da park yeri olan motel olmayan ları arıyorsanız ikincisi daha üstündür.
-   İlk sorgu, Ad, Açıklama ve aranabilir veri içeren diğer alanlar gibi dize alanlarında bahsedilen veya belirtilmeyen belirli sözcüklere dayanır.
-   İkinci sorgu yapılandırılmış veriler üzerinde kesin eşleşmeler arar ve çok daha doğru olması muhtemeldir.

Yönlü gezinme içeren uygulamalarda, yönlü gezinti yapısı üzerindeki her kullanıcı eylemine arama sonuçlarının daraldığından emin olun. Sonuçları daraltmak için bir filtre ifadesi kullanın.

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>Yönlü bir navigasyon uygulaması oluşturun
Arama isteğini oluşturan uygulama kodunuzda Azure Bilişsel Arama ile yönlü gezinme uygularsınız. Yönlü gezinme, şemanızdaki daha önce tanımladığınız öğelere dayanır.

Arama dizininizde önceden `Facetable [true|false]` tanımlanmış, yönlü bir gezinti yapısında kullanımlarını etkinleştirmek veya devre dışı etmek için seçili alanlara ayarlanmış dizin özniteliğidir. Olmadan, `"Facetable" = true`bir alan fason navigasyon kullanılamaz.

Kodunuzdaki sunu katmanı kullanıcı deneyimi sağlar. Etiket, değerler, onay kutuları ve sayım gibi yönlü gezintinin kurucu bölümlerini listelemelidir. Azure Bilişsel Arama REST API platform agnostik, bu nedenle istediğiniz dil ve platformu kullanın. Önemli olan, her ek fakiz seçildiğinde güncelleştirilmiş UI durumuyla artımlı yenilemeyi destekleyen UI öğelerini eklemektir. 

Sorgu sırasında, uygulama kodunuz, alanı `facet=[string]`facet'e sağlayan bir istek parametresi içeren bir istek oluşturur. Bir sorgu, her biri bir `&facet=color&facet=category&facet=rating`ampersand (&) karakteri yle ayrılmış gibi birden çok yönü olabilir.

Uygulama kodu da `$filter` yönlü gezinme tıklama olayları işlemek için bir ifade oluşturmanız gerekir. A, `$filter` filtre ölçütleri olarak fason değerini kullanarak arama sonuçlarını azaltır.

Azure Bilişsel Arama, girdiğiniz bir veya daha fazla terime göre arama sonuçlarını ve yüzü gezen gezinti yapısındaki güncelleştirmeleri döndürür. Azure Bilişsel Arama'da, yönlü gezinme, farklı değerlere ve her biri için kaç sonuç bulunduğuna sahip tek düzeyli bir yapıdır.

Aşağıdaki bölümlerde, her bir parçanın nasıl inşa edilene daha yakından bakacağız.

<a name="buildindex"></a>

## <a name="build-the-index"></a>Dizini oluşturma
Faceting bu dizin özniteliği ile, dizin bir alan-by-field bazında etkindir: `"Facetable": true`.  
Yönlü gezinmede kullanılabilecek tüm alan türleri `Facetable` varsayılan olarak. Bu tür `Edm.String`alan `Edm.DateTimeOffset`türleri , , ve tüm sayısal alan türleri (aslında, `Edm.GeographyPoint`tüm alan türleri dışında facetable vardır , hangi yönlü navigasyon kullanılamaz). 

Bir dizin oluşturmakta, yönlü gezinme için en iyi uygulama, hiçbir zaman bir fason olarak kullanılmaması gereken alanlar için yüz çevirmeyi açıkça kapatmaktır.  Özellikle, kimlik veya ürün adı gibi singleton değerleri için dize `"Facetable": false` alanları, bunların yüzlü gezinmede yanlışlıkla (ve etkisiz) kullanımını önlemek için ayarlanmalıdır. İhtiyacınız olmayan yerlerde ki yüzlemi kapatmak, dizinin boyutunu küçük tutmaya yardımcı olur ve genellikle performansı artırır.

Aşağıdaki İş Portalı Demo örnek uygulaması için şema bir parçasıdır, boyutu azaltmak için bazı öznitelikleri kesilmiş:

```json
{
  ...
  "name": "nycjobs",
  "fields": [
    { "name": "id",                 "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { "name": "job_id",             "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { "name": "agency",              "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "posting_type",        "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "num_of_positions",    "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { "name": "business_title",      "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "civil_service_title", "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "title_code_no",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "level",               "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "salary_range_from",   "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { "name": "salary_range_to",     "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { "name": "salary_frequency",    "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { "name": "work_location",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
…
    { "name": "geo_location",        "type": "Edm.GeographyPoint",     "searchable": false, "filterable": true, ...  "facetable": false, ... },
    { "name": "tags",                "type": "Collection(Edm.String)", "searchable": true,  "filterable": true, ...  "facetable": true, ...  }
  ],
…
}
```

Örnek şemada da görebileceğiniz `Facetable` gibi, kimlik değerleri gibi fatekt olarak kullanılmaması gereken dize alanları için kapatılır. İhtiyacınız olmayan yerlerde ki yüzlemi kapatmak, dizinin boyutunu küçük tutmaya yardımcı olur ve genellikle performansı artırır.

> [!TIP]
> En iyi yöntem olarak, her alan için dizin özniteliklerinin tam kümesini içerir. `Facetable` Hemen hemen tüm alanlar için varsayılan olarak olsa da, her öznitelik bilerek ayarlanması, her şema kararının sonuçlarını düşünmenize yardımcı olabilir. 

<a name="checkdata"></a>

## <a name="check-the-data"></a>Verileri kontrol edin
Verilerinizin kalitesi, yüzle gelen navigasyon yapısının beklediğiniz gibi gerçekleşip gerçekleşmediği üzerinde doğrudan bir etkiye sahiptir. Ayrıca, sonuç kümesini azaltmak için filtreleroluşturma kolaylığını da etkiler.

Marka veya Fiyat'a göre karşınıza çıkmak istiyorsanız, her belge, filtre seçeneği olarak geçerli, tutarlı ve üretken *BrandName* ve *ProductPrice* değerleri içermelidir.

Burada ne için fırçalamak için birkaç hatırlatmalar şunlardır:

* Yüz yüze gelmek istediğiniz her alan için, kendi kendini yönlendiren aramada filtre olarak uygun değerler içerip içermediğini kendinize sorun. Değerler, rakip seçenekler arasında net bir seçim sunmak için kısa, açıklayıcı ve yeterince farklı olmalıdır.
* Yazım hataları veya neredeyse eşleşen değerler. Renk'te yüz ler sayılsa ve alan değerleri Turuncu ve Ornage (yazım hatası) içeriyorsa, Renk alanına dayalı bir faset her ikisini de alır.
* Karışık durum metni de turuncu ve Turuncu iki farklı değer olarak görünen, yönlü navigasyon hasara yol açabilir. 
* Aynı değerdeki tek ve çoğul sürümler, her biri için ayrı bir yönün ortaya çıkabilir.

Tahmin edebileceğiniz gibi, veri hazırlama da çalışkanlık etkili yönlü navigasyon önemli bir yönüdür.

<a name="presentationlayer"></a>

## <a name="build-the-ui"></a>Kullanıcı Arabirimini oluşturma
Sunu katmanından geri çalışmak, aksi takdirde gözden kılabilecek gereksinimleri ortaya çıkarabilirsiniz ve arama deneyimi için hangi özelliklerin gerekli olduğunu anlamanıza yardımcı olabilir.

Yönlü gezinme açısından, web veya uygulama sayfanız yönlü gezinme yapısını görüntüler, sayfadaki kullanıcı girişini algılar ve değiştirilen öğeleri ekler. 

Web uygulamaları için, AJAX, artımlı değişiklikleri yenilemenize olanak sağladığından sunum katmanında yaygın olarak kullanılır. Ayrıca, ASP.NET MVC'yi veya http üzerinden bir Azure Bilişsel Arama hizmetine bağlanabilen başka bir görselleştirme platformu da kullanabilirsiniz. Bu makale boyunca başvurulan örnek uygulama **-Azure Bilişsel Arama İş Portalı Demosu-** ASP.NET bir MVC uygulamasıdır.

Örnekte, yönlü gezinme arama sonuçları sayfasında yerleşiktir. Örnek uygulamanın `index.cshtml` dosyasından alınan aşağıdaki örnek, arama sonuçları sayfasında yönlü gezinmeyi görüntülemek için statik HTML yapısını gösterir. Bir arama terimi gönderdiğinde veya bir fatürü seçtiğinizde veya temizlediğinizde, yüzlistesi dinamik olarak oluşturulur veya yeniden oluşturulur.

```html
<div class="widget sidebar-widget jobs-filter-widget">
  <h5 class="widget-title">Filter Results</h5>
    <p id="filterReset"></p>
    <div class="widget-content">

      <h6 id="businessTitleFacetTitle">Business Title</h6>
      <ul class="filter-list" id="business_title_facets">
      </ul>

      <h6>Location</h6>
      <ul class="filter-list" id="posting_type_facets">
      </ul>

      <h6>Posting Type</h6>
      <ul class="filter-list" id="posting_type_facets"></ul>

      <h6>Minimum Salary</h6>
      <ul class="filter-list" id="salary_range_facets">
      </ul>

  </div>
</div>
```

Sayfadan `index.cshtml` aşağıdaki kod parçacığı, ilk yönü olan İş Başlığı'nı görüntülemek için HTML'yi dinamik olarak oluşturur. Benzer işlevler diğer yönlerin HTML'sini dinamik olarak oluşturur. Her faft, bu fason sonucu için bulunan öğe sayısını görüntüleyen bir etiket eki ve bir sayıma sahiptir.

```js
function UpdateBusinessTitleFacets(data) {
  var facetResultsHTML = '';
  for (var i = 0; i < data.length; i++) {
    facetResultsHTML += '<li><a href="javascript:void(0)" onclick="ChooseBusinessTitleFacet(\'' + data[i].Value + '\');">' + data[i].Value + ' (' + data[i].Count + ')</span></a></li>';
  }

  $("#business_title_facets").html(facetResultsHTML);
}
```

> [!TIP]
> Arama sonuçları sayfasını tasarlarken, yönleri temizlemek için bir mekanizma eklemeyi unutmayın. Onay kutuları eklerseniz, filtreleri nasıl temizleyeceğinizi kolayca görebilirsiniz. Diğer düzenler için, bir kırıntı deseni veya başka bir yaratıcı yaklaşım gerekebilir. Örneğin, İş Arama Portalı örnek uygulamasında, fası temizlemek için seçili bir faktan sonrasını `[X]` tıklatabilirsiniz.

<a name="buildquery"></a>

## <a name="build-the-query"></a>Sorguyu oluşturma
Sorgu oluşturma için yazdığınız kod, bir isteği formüle etmek için kullanılan arama ifadeleri, fatektler, filtreler, puanlama profilleri dahil olmak üzere geçerli bir sorgunun tüm bölümlerini belirtmelidir. Bu bölümde, yüzlerin bir sorguya nerede uyduğunu ve filtrelerin daha düşük bir sonuç kümesi sunmak için fasetlerle nasıl kullanıldığını inceliyoruz.

Bu örnek uygulamada yüzlerin ayrılmaz bir parçası olduğuna dikkat edin. İş Portalı Demo'daki arama deneyimi, yönlü navigasyon ve filtreler etrafında tasarlanmıştır. Sayfada yönlü gezinmenin belirgin yerleşimi bunun önemini göstermektedir. 

Bir örnek genellikle başlamak için iyi bir yerdir. `JobsSearch.cs` Dosyadan alınan aşağıdaki örnek, İşletme Başlığı, Konum, Deftere Nakil Türü ve Asgari Ücret'e dayalı olarak fason gezinme oluşturan bir istek oluşturur. 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

Bir faset sorgu parametresi bir alana ayarlanır ve veri türüne bağlı olarak, virgül-sınırlı `count:<integer>`liste `sort:<>` `interval:<integer>`içeren `values:<list>`, , , ve . Aralıkları ayarlarken sayısal veriler için değerler listesi desteklenir. Kullanım ayrıntıları için [Arama Belgeleri'ne (Azure Bilişsel Arama API' si)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) bakın.

Yönlerinizin yanı sıra, uygulamanız tarafından formüle edilen istek, bir faset değer seçimine dayalı aday belgeleri kümesini daraltmak için filtreler de oluşturmalıdır. Bir bisiklet mağazası için, yönlü navigasyon *hangi renkler, üreticiler ve bisiklet türleri*mevcuttur gibi sorulara ipuçları sağlar? Filtreleme bu *fiyat aralığında hangi tam bisiklet kırmızı, dağ bisikletleri gibi*soruları yanıtlar? . Yalnızca Kırmızı ürünlerin gösterilmesi gerektiğini belirtmek için "Kırmızı"yı tıklattığınızda, `$filter=Color eq 'Red'`uygulamanın gönderdiği bir sonraki sorguyu içerir.

`JobsSearch.cs` İşletme Başlığı fatondan bir değer seçerseniz, sayfadan aşağıdaki kod parçacığı seçili İşletme Başlığını filtreye ekler.

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>İpuçları ve en iyi yöntemler

### <a name="indexing-tips"></a>İpuçlarını dizine ekin leme
**Arama kutusu kullanmıyorsanız dizin verimliliğini artırın**

Uygulamanız yalnızca yönlü gezinme kullanıyorsa (yani arama kutusu yok), `searchable=false` `facetable=true` alanı daha kompakt bir dizin oluşturmak için olarak işaretleyebilirsiniz. Buna ek olarak, dizin oluşturma, çok sözcüklü bir değerin bileşen bölümlerinin sözcük sonu veya dizine gerek olmadığı için yalnızca tüm fason değerlerinde gerçekleşir.

**Hangi alanların façe olarak kullanılabileceğini belirtin**

Dizin şemasının hangi alanların bir fason olarak kullanılabildiğini belirlediğini hatırlayın. Bir alanın yüz yüze olduğunu varsayarsak, sorgu hangi alanların yüz yüze olduğunu belirtir. Yüzlediğiniz alan, etiketin altında görünen değerleri sağlar. 

Her etiketin altında görünen değerler dizinden alınır. Örneğin, faset alanı *Renk*ise, ek filtreleme için kullanılabilir değerler bu alanın değerleridir - Kırmızı, Siyah ve benzeri.

Yalnızca Sayısal ve DateTime değerleri için, facet alanında (örneğin,) `facet=Rating,values:1|2|3|4|5`değerleri açıkça ayarlayabilirsiniz. Bu alan türlerinin, fason sonuçların bitişik aralıklara (sayısal değerlere veya dönemlere dayalı aralıklara) ayrılmasını basitleştirmesine izin verilir. 

**Varsayılan olarak yalnızca bir yönlü gezinme düzeyine sahip olabilirsiniz** 

Belirtildiği gibi, bir hiyerarşi içinde iç içe yönleri için doğrudan bir destek yoktur. Varsayılan olarak, Azure Bilişsel Arama'da yüzlü gezinme yalnızca bir filtre düzeyini destekler. Ancak, geçici çözüm var. Hiyerarşi başına bir `Collection(Edm.String)` giriş noktası olan hiyerarşik bir yönün yapısını kodlayabilirsiniz. Bu geçici çözüm uygulama bu makalenin kapsamı dışındadır. 

### <a name="querying-tips"></a>İpuçlarını sorgulama
**Alanları doğrulama**

Facets listesini güvenilmeyen kullanıcı girişine göre dinamik olarak oluşturursanız, yüzü olan alanların adlarının geçerli olduğunu doğrulayın. Veya `Uri.EscapeDataString()` .NET'te veya seçtiğiniz platformdaki eşdeğeri kullanarak URL'ler inşa ederken adlardan kaçın.

### <a name="filtering-tips"></a>Filtreleme ipuçları
**Filtrelerle arama hassasiyetini artırın**

Filtreler kullanın. Yalnızca arama ifadelerine yalnızca güveniyorsanız, kökten çıkarma, alanlarının hiçbirinde kesin yönü olmayan bir belgenin döndürülmesine neden olabilir.

**Filtrelerle arama performansını artırın**

Filtreler, arama için aday belge kümesini daraltır ve sıralamadan hariç tutar. Büyük bir belge setiniz varsa, seçici bir yönlü detaylandırma kullanmak genellikle size daha iyi performans sağlar.
  
**Yalnızca fason alanları filtreleyin**

Yönlü delinme dedahil inde, genellikle yalnızca belirli bir (yönlü) alana, tüm aranabilir alanlarda hiçbir yerde olmayan bir alanda yönlü değere sahip belgeleri eklemek istersiniz. Filtre eklemek, hizmeti yalnızca yönlü alanda eşleşen bir değer aramaya yönlendirerek hedef alanı güçlendirir.

**Daha fazla filtreyle yüz yüze sonuçları kırpın**

Fason sonuçlar, arama sonuçlarında bulunan ve bir fason terimle eşleşen belgelerdir. Aşağıdaki örnekte, *bulut bilgi işlem*için arama sonuçlarında, 254 öğe de içerik türü olarak *dahili belirtimvardır.* Öğeler mutlaka birbirini dışlayan değildir. Bir madde her iki filtrenin ölçütlerini karşılıyorsa, her birinde sayılır. Bu yineleme, genellikle belge etiketlemeuygulamak `Collection(Edm.String)` için kullanılan alanlara yüz yüze geldiğinde mümkündür.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Genel olarak, fason sonuçların sürekli olarak çok büyük olduğunu fark ederseniz, kullanıcılara aramayı daraltmak için daha fazla seçenek sunmak için daha fazla filtre eklemenizi öneririz.

### <a name="tips-about-result-count"></a>Sonuç sayısı yla ilgili ipuçları

**Fason gezintideki öğe sayısını sınırlama**

Gezinti ağacındaki her yönlü alan için varsayılan 10 değer sınırı vardır. Değerler listesini yönetilebilir bir boyuta tuttuğundan, bu varsayılan yapı gezinti yapıları için anlamlıdır. Saymak için bir değer atayarak varsayılan geçersiz kılınabilir.

* `&facet=city,count:5`sadece ilk beş şehir üst sırada yer alan sonuçlar bir fason sonucu olarak döndürülür olduğunu belirtir. "Havaalanı" ve 32 eşleşmeli bir arama terimi içeren örnek bir sorgu düşünün. Sorgu da belirtiliyorsa, `&facet=city,count:5`arama sonuçlarında en çok belgeye sahip yalnızca ilk beş benzersiz şehir, facet sonuçlarına dahil edilir.

Fason sonuçlar ve arama sonuçları arasındaki ayrıma dikkat edin. Arama sonuçları, sorguyla eşleşen tüm belgelerdir. Fason sonuçlar, her fason değeri için eşleşmelerdir. Örnekte, arama sonuçları, fason sınıflandırma listesinde olmayan Şehir adlarını içerir (örneğimizde 5). Yönlü gezinme yoluyla filtrelenen sonuçlar, yüzleri temizlediğinizde veya City dışında başka yönler seçtiğinizde görünür hale gelir. 

> [!NOTE]
> Birden `count` fazla tür olduğunda tartışmak kafa karıştırıcı olabilir. Aşağıdaki tablo, terimin Azure Bilişsel Arama API'sinde, örnek kodda ve belgelerde nasıl kullanıldığının kısa bir özetini sunar. 

* `@colorFacet.count`<br/>
  Sunu kodunda, faset sonuçlarının sayısını görüntülemek için kullanılan bir sayma parametresi görmeniz gerekir. Fason sonuçlarda, sayım, fason terim veya aralıkta eşleşen belge sayısını gösterir.
* `&facet=City,count:12`<br/>
  Bir facet sorgusunda, saymayı bir değere ayarlayabilirsiniz.  Varsayılan değer 10'dur, ancak daha yüksek veya daha düşük olarak ayarlayabilirsiniz. Ayar, `count:12` belge sayısına göre faiyer sonuçlarda ilk 12 eşleşmeyi alır.
* "`@odata.count`"<br/>
  Sorgu yanıtında, bu değer arama sonuçlarında eşleşen öğelerin sayısını gösterir. Ortalama olarak, arama terimiyle eşleşen, ancak fatür değeri eşleşmesi olmayan öğelerin varlığı nedeniyle, birleştirilmiş tüm fason sonuçların toplamından daha büyüktür.

**Fason sonuçlarda sayımları alma**

Bir faceted sorguya filtre eklediğinizde, fason deyimini (örneğin,) `facet=Rating&$filter=Rating ge 4`korumak isteyebilirsiniz. Teknik olarak, fatür=Derecelendirme gerekmez, ancak onu tutmak 4 ve daha yüksek derecelendirmeler için fatür değerlerinin sayısını döndürür. Örneğin, "4"e tıklarsanız ve sorguda "4"ün üzerinde veya daha büyük bir filtre varsa, 4 ve daha yüksek olan her derecelendirme için sayımlar döndürülür.  

**Doğru fason sayıları aldığınızdan emin olun**

Belirli koşullar altında, yüz sayımları sonuç kümeleri ile eşleşmeyebilir (Bkz. [Azure Bilişsel Arama'da (forum gönderisinde) Faceted navigasyonu).](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)

Yüz sayıları, parçalama mimarisi nedeniyle yanlış olabilir. Her arama dizininde birden çok parça vardır ve her parça belge sayısına göre en üstteki N yönlerini raporlar ve bu da tek bir sonuca ayrılır. Bazı kırıkların çok sayıda eşleşen değeri varsa, bazılarının ise daha az eşleşme değeri varsa, bazı fason değerlerinin eksik veya sonuçlarda az sayıldığını görebilirsiniz.

Bu davranış herhangi bir zamanda değişebilir, bugün bu davranışla karşılaşırsanız, sayısı yapay olarak\<şişirerek bu davranışın etrafında nüzerinde çalışabilirsiniz: her parçadan tam raporlama yı zorlamak için çok sayıda>. Sayım değeri: alandaki benzersiz değer sayısından büyük veya eşitse, doğru sonuçlar garanti edilir. Ancak, belge sayıları yüksek olduğunda, bir performans cezası vardır, bu nedenle bu seçeneği akıllıca kullanın.

### <a name="user-interface-tips"></a>Kullanıcı arabirimi ipuçları
**Fason gezinmede her alan için etiket ekleme**

Etiketler genellikle HTML veya formda (örnek`index.cshtml` uygulamada) tanımlanır. Azure Bilişsel Arama'da, yönlü gezinme etiketleri veya diğer meta veriler için API yoktur.

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>Bir diziye göre filtreleme
Değer aralıkları üzerinde yüz yüze ortak bir arama uygulaması gereksinimidir. Aralıklar sayısal veriler ve DateTime değerleri için desteklenir. [Arama Belgeleri'nde (Azure Bilişsel Arama API)](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)her yaklaşım hakkında daha fazla bilgi edinebilirsiniz.

Azure Bilişsel Arama, bir aralığı hesaplamak için iki yaklaşım sağlayarak aralık yapımını kolaylaştırır. Azure Bilişsel Arama, her iki yaklaşım için de sağladığınız girdiler göz önüne alındığında uygun aralıkları oluşturur. Örneğin, 10|20|30 aralık değerleri belirtirseniz, otomatik olarak 0-10, 10-20, 20-30 aralıkları oluşturur. Uygulamanız isteğe bağlı olarak boş olan aralıkları kaldırabilir. 

**Yaklaşım 1: Aralık parametresini kullanın**  
Fiyat yönlerini 10 TL'lik artışlarla ayarlamak için şunları belirtirsiniz:`&facet=price,interval:10`

**Yaklaşım 2: Değerler listesi kullanma**  
Sayısal veriler için bir değerler listesi kullanabilirsiniz.  Aşağıdaki gibi işlenen bir `listPrice` alan için fason aralığını göz önünde bulundurun:

  ![Örnek değerler listesi](media/search-faceted-navigation/Facet-5-Prices.PNG "Örnek değerler listesi")

Önceki ekran görüntüsündeki gibi bir fason aralığı belirtmek için değerler listesi kullanın:

    facet=listPrice,values:10|25|100|500|1000|2500

Her aralık başlangıç noktası olarak 0, listeden bir son nokta olarak bir değer kullanılarak oluşturulur ve sonra ayrı aralıklar oluşturmak için önceki aralığın kırpılmış. Azure Bilişsel Arama, bunları yönlü gezinmenin bir parçası olarak yapar. Her aralığı yapılandırmak için kod yazmak zorunda kalmadığınız.

### <a name="build-a-filter-for-a-range"></a>Aralık için filtre oluşturma
Seçtiğiniz bir aralığı temel alan belgeleri filtrelemek `"ge"` `"lt"` için, ve filtre işleçleri aralığın uç noktalarını tanımlayan iki parçalı bir ifade de kullanabilirsiniz. Örneğin, bir `listPrice` alan için 10-25 aralığını seçerseniz, `$filter=listPrice ge 10 and listPrice lt 25`filtre . Örnek kodda, filtre ifadesi uç noktaları ayarlamak için **priceFrom** ve **priceTo** parametrelerini kullanır. 

  ![Bir dizi değer için sorgu](media/search-faceted-navigation/Facet-6-buildfilter.PNG "Bir dizi değer için sorgu")

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>Mesafeye göre filtreleme
Geçerli konumunuza yakınlığına bağlı olarak bir mağaza, restoran veya hedef seçmenize yardımcı olan filtreleri görmek yaygındır. Bu tür bir filtre yönlü gezinme gibi görünse de, bu sadece bir filtredir. Biz burada özellikle bu özel tasarım sorunu için uygulama tavsiye arıyoruz sizin için söz.

Azure Bilişsel Arama'da iki Jeouzamsal işlev vardır: **geo.distance** ve **geo.intersects**.

* **Geo.distance** fonksiyonu iki nokta arasındaki kilometre mesafesini döndürür. Bir nokta bir alan dır ve diğeri filtrenin bir parçası olarak geçirilen sabittir. 
* Belirli bir nokta belirli bir çokgen içinde ise **geo.intersects** fonksiyonu doğru döner. Nokta bir alandır ve çokgen filtrenin bir parçası olarak geçirilen koordinatların sabit bir listesi olarak belirtilir.

Filtre örneklerini [OData ifade sözdiziminde (Azure Bilişsel Arama)](query-odata-filter-orderby-syntax.md)bulabilirsiniz.

<a name="tryitout"></a>

## <a name="try-the-demo"></a>Demoyu deneyin
Azure Bilişsel Arama İş Portalı Demosu, bu makalede atıfta bulunulan örnekleri içerir.

-   Azure Bilişsel Arama İş [Portalı Demo'da](https://aka.ms/azjobsdemo)çalışan demoyu çevrimiçi görün ve test edin.

-   [Kodu GitHub'daki Azure Örnekleri reposundan](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs)indirin.

Arama sonuçlarıyla çalışırken, sorgu yapısındaki değişiklikler için URL'yi izleyin. Bu uygulama, her birini seçtiğinizde URI'ye yüzler ekler.

1. Demo uygulamasının eşleme işlevini kullanmak için [Bing Maps Geliştirme Merkezi'nden](https://www.bingmapsportal.com/)bing haritaları anahtarı alın. `index.cshtml` Sayfadaki varolan anahtarın üzerine yapıştırın. Dosyadaki `BingApiKey` `Web.config` ayar kullanılmaz. 

2. Uygulamayı çalıştırın. İsteğe bağlı turu alın veya iletişim kutusunu kapatın.
   
3. "Analist" gibi bir arama terimi girin ve Arama simgesini tıklatın. Sorgu hızlı bir şekilde yürütülür.
   
   Bir yönlü navigasyon yapısı da arama sonuçları ile döndürülür. Arama sonucu sayfasında, yönlü gezinti yapısı her yönlü sonuç için sayımlar içerir. Hiçbir yönü seçilir, bu nedenle eşleşen tüm sonuçlar döndürülür.
   
   ![Fatektleri seçmeden önce sonuçları arama](media/search-faceted-navigation/faceted-search-before-facets.png "Fatektleri seçmeden önce sonuçları arama")

4. İşletme Unvanı, Konumu veya Asgari Ücret'i tıklatın. Fasetler ilk aramada geçersizdi, ancak değerleri ele aldıklarında, arama sonuçları artık eşleşmeyan öğelerin kırpılır.
   
   ![Fatektleri seçtikten sonra arama sonuçları](media/search-faceted-navigation/faceted-search-after-facets.png "Fatektleri seçtikten sonra arama sonuçları")

5. Farklı sorgu davranışlarını denemek için faceted sorguyu temizlemek `[X]` için, yüzleri temizlemek için seçili faktan sonrasını tıklatın.
   
<a name="nextstep"></a>

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Azure Bilişsel Arama Derin Dalış](https://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410)izle . Saat 45:25'te, yönlerin nasıl uygulanacağı yla ilgili bir demo vardır.

Yönlü gezinme için tasarım ilkeleri hakkında daha fazla bilgi için aşağıdaki bağlantıları öneririz:

* [Tasarım Desenleri: Yönlü Navigasyon](https://alistapart.com/article/design-patterns-faceted-navigation)
* [Yönlü Arama Uygularken Ön Uç Endişeleri – Bölüm 1](https://articles.uie.com/faceted_search2/)

