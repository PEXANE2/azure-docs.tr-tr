---
title: Puanlama profillerini kullanarak arama sıralamasını artırma
titleSuffix: Azure Cognitive Search
description: Puanlama profilleri ekleyerek Azure Bilişsel Arama sonuçları için arama sıralaması puanlarını artırın.
manager: nitinme
author: Brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/28/2019
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
ms.openlocfilehash: 516637b812afece1966006ce6d894dd1e32e6293
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245466"
---
# <a name="add-scoring-profiles-to-an-azure-cognitive-search-index"></a>Azure Bilişsel Arama dizinine puanlama profilleri ekleme

  Puanlama, arama sonuçlarında döndürülen her öğe için bir *arama puanının* hesaplanması anlamına gelir. Puan bir öğenin geçerli arama işlemi bağlamıyla ne kadar ilgili olduğunun göstergesidir. Puan ne kadar yüksekse öğe o kadar ilgilidir. Arama sonuçlarında, öğeler her öğe için hesaplanan arama puanlarına göre yüksekten dibe doğru sıralanır.  

 Azure Bilişsel Arama, ilk puanı hesaplamak için varsayılan puanlama kullanır, ancak hesaplamayı *bir puanlama profili*üzerinden özelleştirebilirsiniz. Puanlama profilleri, arama sonuçlarındaki öğelerin sıralaması üzerinde daha fazla denetim sağlar. Örneğin, öğeleri gelir potansiyeline göre artırmak, yeni maddeleri tanıtmak veya stokta çok uzun süredir bulunan maddeleri öne çıkarmak isteyebilirsiniz.  

 Puanlama profili, ağırlıklı alanlar, işlevler ve parametrelerden oluşan dizin tanımının bir parçasıdır.  

 Puanlama profilinin nasıl göründüğü hakkında bir fikir vermek için aşağıdaki örnekte 'geo' adlı basit bir profil gösterilmektedir. Bu, **hotelName** alanında arama terimi olan öğeleri artırır. Ayrıca, geçerli `distance` konuma on kilometre içinde öğeleri lehine işlevi kullanır. Birisi 'han' terimini ararsa ve 'han' otel adının bir parçası ysa, geçerli konumun 10 KM yarıçapı içinde 'han' bulunan otelleri içeren belgeler arama sonuçlarında daha yüksek görünür.  


```json
"scoringProfiles": [
  {  
    "name":"geo",
    "text": {  
      "weights": {  
        "hotelName": 5
      }                              
    },
    "functions": [
      {  
        "type": "distance",
        "boost": 5,
        "fieldName": "location",
        "interpolation": "logarithmic",
        "distance": {
          "referencePointParameter": "currentLocation",
          "boostingDistance": 10
        }                        
      }                                      
    ]                     
  }            
]
```  


 Bu puanlama profilini kullanmak için, sorgu dizenizdeki profili belirtmek için sorgunuz formüle edilir. Aşağıdaki sorguda, istekteki sorgu `scoringProfile=geo` parametresini fark edin.  

```  
GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2019-05-06 
```  

 Bu sorgu 'han' teriminde arama lar ve geçerli konumdan geçer. Bu sorgunun `scoringParameter`. Sorgu parametreleri [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)açıklanmıştır.  

 Puanlama profilinin daha ayrıntılı bir örneğini gözden geçirmek için [Örnek'i](#bkmk_ex) tıklatın.  

## <a name="what-is-default-scoring"></a>Varsayılan puanlama nedir?  
 Puanlama, sıralı sonuç kümesindeki her öğe için bir arama puanı hesaplar. Arama sonuç kümesindeki her öğeye bir arama puanı atanır ve ardından en yüksekila en düşük puana sıralanır. Daha yüksek puana sahip öğeler uygulamaya döndürülür. Varsayılan olarak, en iyi 50 döndürülür, `$top` ancak daha küçük veya daha fazla sayıda öğe (tek bir yanıtta 1000'e kadar) döndürmek için parametreyi kullanabilirsiniz.  

Arama puanı, verilerin ve sorgunun istatistiksel özelliklerine göre hesaplanır. Azure Bilişsel Arama, sorgu dizesinde arama terimlerini içeren belgeleri `searchMode`(bağlı olarak bazı veya tümü), arama teriminin birçok örneğini içeren belgeleri lehine bulur. Terim veri dizininde nadir bulunursa, ancak belge içinde yaygınsa, arama puanı daha da yükselir. Bilgi işlem alaka bu yaklaşımın temeli [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) veya dönem frekans-ters belge sıklığı olarak bilinir.  

 Özel sıralama olmadığı varsayılsa, sonuçlar arama uygulamasına döndürülmeden önce arama puanına göre sıralanır. $top belirtilmemişse, en yüksek arama puanına sahip 50 öğe döndürülür.  

 Arama puanı değerleri bir sonuç kümesi boyunca tekrarlanabilir. Örneğin, 1,2 puana sahip 10 öğeniz, 1,0 puanlı 20 öğeniz ve 0,5 puanlı 20 öğeniz olabilir. Birden çok isabet aynı arama puanına sahipse, aynı puanlı öğelerin sıralanması tanımlı değildir ve kararlı değildir. Sorguyu yeniden çalıştırDığınızda öğelerin konumunu değiştirin. Aynı puana sahip iki öğe göz önüne alındığında, hangisinin ilk göründüğünün garantisi yoktur.  

## <a name="when-to-use-custom-scoring"></a>Özel puanlama ne zaman kullanılır?  
 Varsayılan sıralama davranışı işletme hedeflerinizi karşılamada yeterince ileri gitmediğinde bir veya daha fazla puanlama profili oluşturmanız gerekir. Örneğin, arama alaka düzeyinin yeni eklenen öğelerlehine karar verebilirsiniz. Aynı şekilde, kar marjı veya gelir potansiyelini gösteren başka bir alan içeren bir alanınız da olabilir. İşletmenize fayda sağlayan isabetleri artırmak, puanlama profillerini kullanmaya karar vermede önemli bir faktör olabilir.  

 Alaka bazlı sıralama da puanlama profilleri aracılığıyla uygulanır. Geçmişte kullandığınız arama sonuçları sayfalarını göz önünde bulundurarak fiyat, tarih, derecelendirme veya alaka düzeyine göre sıralamanızı sağlar. Azure Bilişsel Arama'da, puanlama profilleri 'alaka düzeyi' seçeneğini kullanır. Alaka düzeyinin tanımı, iş hedeflerine ve sunmak istediğiniz arama deneyimi türüne bağlı olarak sizin yeriniz tarafından denetlenir.  

##  <a name="example"></a><a name="bkmk_ex"></a>Örnek  
 Daha önce belirtildiği gibi, özelleştirilmiş puanlama bir dizin şemasında tanımlanan bir veya daha fazla puanlama profilleri aracılığıyla uygulanır.  

 Bu örnek, iki puanlama profili olan bir dizinin`boostGenre` `newAndHighlyRated`şemasını gösterir ( , ). Sorgu parametresi olarak profili içeren bu dizine karşı yapılan tüm sorgular, sonuç kümesini puanlamak için profili kullanır.  

```json
{  
  "name": "musicstoreindex",  
  "fields": [  
    { "name": "key", "type": "Edm.String", "key": true },  
    { "name": "albumTitle", "type": "Edm.String" },  
    { "name": "albumUrl", "type": "Edm.String", "filterable": false },  
    { "name": "genre", "type": "Edm.String" },  
    { "name": "genreDescription", "type": "Edm.String", "filterable": false },  
    { "name": "artistName", "type": "Edm.String" },  
    { "name": "orderableOnline", "type": "Edm.Boolean" },  
    { "name": "rating", "type": "Edm.Int32" },  
    { "name": "tags", "type": "Collection(Edm.String)" },  
    { "name": "price", "type": "Edm.Double", "filterable": false },  
    { "name": "margin", "type": "Edm.Int32", "retrievable": false },  
    { "name": "inventory", "type": "Edm.Int32" },  
    { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }  
  ],  
  "scoringProfiles": [  
    {  
      "name": "boostGenre",  
      "text": {  
        "weights": {  
          "albumTitle": 1.5,  
          "genre": 5,  
          "artistName": 2  
        }  
      }  
    },  
    {  
      "name": "newAndHighlyRated",  
      "functions": [  
        {  
          "type": "freshness",  
          "fieldName": "lastUpdated",  
          "boost": 10,  
          "interpolation": "quadratic",  
          "freshness": {  
            "boostingDuration": "P365D"  
          }  
        },  
        {
          "type": "magnitude",  
          "fieldName": "rating",  
          "boost": 10,  
          "interpolation": "linear",  
          "magnitude": {  
            "boostingRangeStart": 1,  
            "boostingRangeEnd": 5,  
            "constantBoostBeyondRange": false  
          }  
        }  
      ]  
    }  
  ],  
  "suggesters": [  
    {  
      "name": "sg",  
      "searchMode": "analyzingInfixMatching",  
      "sourceFields": [ "albumTitle", "artistName" ]  
    }  
  ]   
}  
```  

## <a name="workflow"></a>İş akışı  
 Özel puanlama davranışı uygulamak için, dizini tanımlayan şemaya bir puanlama profili ekleyin. Bir dizin içinde en fazla 100 puanlama profiliniz olabilir [(Hizmet Sınırları'na](search-limits-quotas-capacity.md)bakın), ancak herhangi bir sorguda aynı anda yalnızca bir profil belirtebilirsiniz.  

 Bu konuda sağlanan [Şablon](#bkmk_template) ile başlayın.  

 Bir ad sağlayın. Puanlama profilleri isteğe bağlıdır, ancak bir tane eklerseniz, ad gereklidir. Alanlar için adlandırma kurallarıtakip emin olun (bir harf ile başlar, özel karakterler ve ayrılmış sözcükleri önler). Tam liste için [Azure Bilişsel Arama&#41;&#40;Adlandırma kurallarına](https://docs.microsoft.com/rest/api/searchservice/naming-rules) bakın.  

 Puanlama profilinin gövdesi ağırlıklı alanlardan ve işlevlerden oluşturulur.  

|||  
|-|-|  
|**Ağırlık**|Bir alana göreli bir ağırlık atayabilen ad değeri çiftleri belirtin. [Örnekte,](#bkmk_ex)albumTitle, tür ve artistName alanları sırasıyla 1,5, 5 ve 2 artırılır. Neden tür diğerlerinden çok daha yüksek artırılır? Arama biraz homojen veri üzerinden yapılırsa ('tür' ile olduğu `musicstoreindex`gibi), göreli ağırlıklarda daha büyük bir varyans gerekebilir. Örneğin, `musicstoreindex`'rock' hem bir tür olarak hem de aynı şekilde ifade edilen tür tanımlarında görünür. Tür tanımına ağır basmasını istiyorsanız, tür alanının çok daha yüksek bir göreli ağırliğe ihtiyacı vardır.|  
|**Işlev**|Belirli bağlamlar için ek hesaplamalar gerektiğinde kullanılır. Geçerli değerler `freshness` `magnitude`, `distance`, `tag`, ve . Her işlevin kendine özgü parametreleri vardır.<br /><br /> -   `freshness`öğenin ne kadar yeni veya eski olduğunu artırmak istediğinizde kullanılmalıdır. Bu işlev yalnızca alanlar `datetime` (edm) ile kullanılabilir. DataTimeOffset). Özniteliğin `boostingDuration` yalnızca işlevle `freshness` birlikte kullanıldığına dikkat edin.<br />-   `magnitude`sayısal değerin ne kadar yüksek veya düşük olduğuna bağlı olarak artırmak istediğinizde kullanılmalıdır. Bu işlev için çağrıda bekleyen senaryolar kar marjı, en yüksek fiyat, en düşük fiyat veya indirme sayısı ile artırma içerir. Bu işlev yalnızca çift ve tamsayı alanları ile kullanılabilir.<br />     `magnitude` İşlev için, ters desen istiyorsanız (örneğin, daha yüksek fiyatlı öğelerden daha fazla düşük fiyatlı öğeleri artırmak için) yüksekten düşüke aralığı tersine çevirebilirsiniz. 100 $ ile 1 $ arasında fiyatlar bir `boostingRangeStart` dizi göz `boostingRangeEnd` önüne alındığında, 100 ve 1 düşük fiyatlı öğeleri artırmak için ayarlamak istiyorsunuz.<br />-   `distance`yakınlık veya coğrafi konuma göre artırmak istediğinizde kullanılmalıdır. Bu işlev yalnızca alanlar `Edm.GeographyPoint` ile kullanılabilir.<br />-   `tag`belgeler ve arama sorguları arasında ortak etiketlere göre artırmak istediğinizde kullanılmalıdır. Bu işlev yalnızca `Edm.String` `Collection(Edm.String)` ve alanları ile kullanılabilir.<br /><br /> **Fonksiyonları kullanma kuralları**<br /><br /> İşlev`freshness`türü `magnitude` `distance`( `tag` , , , ) küçük harf olmalıdır.<br /><br /> İşlevler boş veya boş değerleri içeremez. Özellikle, alan adını eklerseniz, bir şeye ayarlamanız gerekir.<br /><br /> İşlevler yalnızca filtrelenebilir alanlara uygulanabilir. Filtrelenebilir alanlar hakkında daha fazla bilgi için Azure [Bilişsel Arama REST API&#41;&#40;Dizin Oluştur'a](https://docs.microsoft.com/rest/api/searchservice/create-index) bakın.<br /><br /> İşlevler yalnızca bir dizin alan koleksiyonunda tanımlanan alanlara uygulanabilir.|  

 Dizin tanımlandıktan sonra, dizin şema sını yükleyerek dizin oluşturun ve ardından belgeler. Azure Bilişsel Arama REST API&#41;&#40;[Ekle, Güncelleme veya Silme Belgeleri &#40;Azure Bilişsel Arama REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) &#40;Bu işlemlerle ilgili talimatlar için [Dizin Oluştur'a](https://docs.microsoft.com/rest/api/searchservice/create-index) bakın. Dizin oluşturulundan sonra, arama verilerinizle çalışan işlevsel bir puanlama profiliniz olmalıdır.  

##  <a name="template"></a><a name="bkmk_template"></a>Şablon  
 Bu bölümde, puanlama profilleri için sözdizimi ve şablon gösterilmektedir. Özniteliklerin açıklamaları için sonraki bölümde [Dizin öznitelikleri başvurubakın.](#bkmk_indexref)  

```  
. . .   
"scoringProfiles": [  
  {   
    "name": "name of scoring profile",   
    "text": (optional, only applies to searchable fields) {   
      "weights": {   
        "searchable_field_name": relative_weight_value (positive #'s),   
        ...   
      }   
    },   
    "functions": (optional) [  
      {   
        "type": "magnitude | freshness | distance | tag",   
        "boost": # (positive number used as multiplier for raw score != 1),   
        "fieldName": "...",   
        "interpolation": "constant | linear (default) | quadratic | logarithmic",   

        "magnitude": {
          "boostingRangeStart": #,   
          "boostingRangeEnd": #,   
          "constantBoostBeyondRange": true | false (default)
        }  

        // ( - or -)  

        "freshness": {
          "boostingDuration": "..." (value representing timespan over which boosting occurs)   
        }  

        // ( - or -)  

        "distance": {
          "referencePointParameter": "...", (parameter to be passed in queries to use as reference location)   
          "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)   
        }   

        // ( - or -)  

        "tag": {
          "tagsParameter":  "..."(parameter to be passed in queries to specify a list of tags to compare against target field)   
        }
      }
    ],   
    "functionAggregation": (optional, applies only when functions are specified) "sum (default) | average | minimum | maximum | firstMatching"   
  }   
],   
"defaultScoringProfile": (optional) "...",   
. . .  
```  

##  <a name="index-attributes-reference"></a><a name="bkmk_indexref"></a>Dizin öznitelikleri başvurusu  

> [!NOTE]  
>  Puanlama işlevi yalnızca filtrelenebilir alanlara uygulanabilir.  

|Öznitelik|Açıklama|  
|---------------|-----------------|  
|`name`|Gereklidir. Bu puanlama profilinin adıdır. Bir alanın aynı adlandırma kurallarını izler. Bir harfle başlamalı, nokta, üst üste veya @ sembolü içermemeli ve 'azureSearch' (büyük/küçük harf duyarlı) ifadesiyle başlayamaz.|  
|`text`|Ağırlıközelliğini içerir.|  
|`weights`|İsteğe bağlı. Her biri bir alan adı ve göreli ağırlık belirten ad değeri çiftleri içerir. Göreli ağırlık pozitif bir tamsayı veya kayan nokta numarası olmalıdır.<br /><br /> Ağırlıklar, bir aranabilir alanın diğerine göre önemini belirtmek için kullanılır.|  
|`functions`|İsteğe bağlı. Puanlama işlevi yalnızca filtrelenebilir alanlara uygulanabilir.|  
|`type`|Puanlama işlevleri için gereklidir. Kullanılacak işlev türünü gösterir. Geçerli değerler büyüklük, tazelik, mesafe ve etiket içerir. Her puanlama profiline birden fazla işlev ekleyebilirsiniz. İşlev adı küçük harf olmalıdır.|  
|`boost`|Puanlama işlevleri için gereklidir. Ham puan için çarpan olarak kullanılan pozitif bir sayı. 1'e eşit olamaz.|  
|`fieldname`|Puanlama işlevleri için gereklidir. Puanlama işlevi yalnızca dizin alan koleksiyonunun bir parçası olan ve filtrelenebilir alanlara uygulanabilir. Buna ek olarak, her işlev türü ek kısıtlamalar getirir (tarih alanlarıyla tazelik, tamsayı veya çift alanlı büyüklük ve konum alanlarıyla mesafe) kullanılır. İşlev tanımı başına yalnızca tek bir alan belirtebilirsiniz. Örneğin, aynı profilde iki kez büyüklük kullanmak için, her alan için bir tane olmak üzere iki tanım büyüklüğü eklemeniz gerekir.|  
|`interpolation`|Puanlama işlevleri için gereklidir. Aralığın başlangıcından aralığın sonuna kadar puan artırmanın arttığı eğimi tanımlar. Geçerli değerler Doğrusal (varsayılan), Sabit, Kuadratik ve Logaritmik içerir. Ayrıntılar için [enterpolasyonları ayarla'ya](#bkmk_interpolation) bakın.|  
|`magnitude`|Büyüklük puanlama işlevi, sayısal bir alanın değer aralığına göre sıralamaları değiştirmek için kullanılır. Bunun en yaygın kullanım örneklerinden bazıları şunlardır:<br /><br /> -   **Yıldız derecelendirmeleri:** "Yıldız Derecelendirmesi" alanındaki değere göre puanlamayı değiştirin. İki öğe alakalı olduğunda, önce daha yüksek derecelendirmeye sahip öğe görüntülenir.<br />-   **Kenar Boşluğu:** İki belge alakalı olduğunda, bir perakendeci önce daha yüksek marjlara sahip belgeleri öne çıkarmak isteyebilir.<br />-   **Tıklama sayıları:** Eylemlerden ürünlere veya sayfalara yapılan tıklamaları izleyen uygulamalar için, en fazla trafik alma eğiliminde olan öğeleri artırmak için büyüklük kullanabilirsiniz.<br />-   **İndirme sayıları:** İndirmeleri izleyen uygulamalariçin büyüklük işlevi, en çok indiren öğeleri öne çıkarmanızı sağlar.|  
|`magnitude`&#124;`boostingRangeStart`|Büyüklüğün puanlandığı aralığın başlangıç değerini ayarlar. Değer bir veya kayan nokta numarası olmalıdır. 1'den 4'e kadar olan yıldız reytingleri için bu 1 olur. %50'nin üzerindeki marjlar için bu 50 olur.|  
|`magnitude`&#124;`boostingRangeEnd`|Büyüklüğün puanlandığı aralığın son değerini ayarlar. Değer bir veya kayan nokta numarası olmalıdır. 1'den 4'e kadar olan yıldız reytingleri için bu 4 olur.|  
|`magnitude`&#124;`constantBoostBeyondRange`|Geçerli değerler doğru veya yanlıştır (varsayılan). Tam destek doğru ayarlandığında, hedef alan için değeri aralığın üst ucundan daha yüksek olan belgelere uygulanmaya devam eder. Yanlışsa, bu işlevin öne sürülme alanı aralığının dışına düşen hedef alan için değeri olan belgelere uygulanmaz.|  
|`freshness`|Tazelik puanlama işlevi, alanlardaki `DateTimeOffset` değerlere göre öğelerin sıralama puanlarını değiştirmek için kullanılır. Örneğin, daha yeni bir tarihi olan bir öğe, eski öğelerden daha yüksek sıralanabilir.<br /><br /> Takvim olayları gibi öğeleri, şimdiki zamana yakın öğelerin gelecekte öğelerden daha yüksek sıralanabileceği gibi, gelecekteki tarihleri yle sıralamak da mümkündür.<br /><br /> Geçerli hizmet sürümünde, aralığın bir ucu geçerli saate sabitlenir. Diğer uç geçmişte bir zaman dayalı `boostingDuration`. Gelecekte bir dizi kez artırmak için negatif `boostingDuration`kullanın.<br /><br /> Maksimum ve minimum aralıktaki artış hızı, puanlama profiline uygulanan Enterpolasyon tarafından belirlenir (aşağıdaki şemaya bakın). Uygulanan artırma faktörünün tersine çevirmek için 1'den küçük bir artırma faktörü seçin.|  
|`freshness`&#124;`boostingDuration`|Belirli bir belge için artırmanın duracağı bir son kullanma süresi ayarlar. Sözdizimi ve örnekler için aşağıdaki bölümde [BoostingSet'e](#bkmk_boostdur) bakın.|  
|`distance`|Mesafe puanlama işlevi, belgelerin puanını referans coğrafi konuma göre ne kadar yakın veya uzak olduğuna göre etkilemek için kullanılır. Başvuru konumu, bir parametredeki sorgunun bir parçası `scoringParameterquery` olarak (dize seçeneğini kullanarak) lon,lat bağımsız değişkeni olarak verilir.|  
|`distance`&#124;`referencePointParameter`|Başvuru yeri olarak kullanılacak sorgularda geçirilecek bir parametre. `scoringParameter`bir sorgu parametresidir. Sorgu parametrelerinin açıklamaları için [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;'na](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) bakın.|  
|`distance`&#124;`boostingDistance`|Artırma aralığının sona erdiği referans konumundan kilometrelerce uzaktaki mesafeyi gösteren bir sayı.|  
|`tag`|Etiket puanlama işlevi, belgelerdeki ve arama sorgularında etiketlere dayalı belgelerin puanını etkilemek için kullanılır. Arama sorgusuyla ortak etiketleri olan belgeler artırılır. Arama sorgusunun etiketleri, her arama isteğinde `scoringParameterquery` (dize seçeneğini kullanarak) puanlama parametresi olarak sağlanır.|  
|`tag`&#124;`tagsParameter`|Belirli bir istek için etiketleri belirtmek için sorgularda geçirilecek bir parametre. `scoringParameter`bir sorgu parametresidir. Sorgu parametrelerinin açıklamaları için [Arama Belgeleri &#40;Azure Bilişsel Arama REST API&#41;'na](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) bakın.|  
|`functionAggregation`|İsteğe bağlı. Yalnızca işlevler belirtildiğinde geçerlidir. Geçerli değerler şunlardır: toplam (varsayılan), ortalama, minimum, maksimum ve ilk Eşleştirme. Arama puanı, birden çok işlev de dahil olmak üzere birden çok değişkenden hesaplanan tek değerdir. Bu öznitelik, tüm işlevlerin öne çıkarmalarının temel belge puanına uygulanacak tek bir toplu destekte nasıl birleştirilmediğini gösterir. Taban puan, belgeden ve arama sorgusundan hesaplanan [tf-idf](http://www.tfidf.com/) değerine dayanır.|  
|`defaultScoringProfile`|Bir arama isteği yürütüldüğünde, puanlama profili belirtilmemişse, varsayılan puanlama kullanılır (yalnızca[tf-idf).](http://www.tfidf.com/)<br /><br /> Varsayılan bir puanlama profil adı burada ayarlanabilir ve bu da Azure Bilişsel Arama'nın arama isteğinde belirli bir profil verilmediğinde bu profili kullanmasına neden olur.|  

##  <a name="set-interpolations"></a><a name="bkmk_interpolation"></a>Enterpolasyonları ayarlama  
 Enterpolasyonlar puanlama için kullanılan eğimin şeklini ayarlamanızı sağlar. Puanlama yüksekten düşüke doğru olduğundan, eğim her zaman azalmaktadır, ancak enterpolasyon aşağı doğru eğimin eğrisini belirler. Aşağıdaki enterpolasyonlar kullanılabilir:  

|||  
|-|-|  
|`linear`|Maksimum ve min aralığında olan maddeler için, maddeye uygulanan destek sürekli azalan bir miktarda yapılır. Doğrusal bir puanlama profili için varsayılan enterpolasyondur.|  
|`constant`|Başlangıç ve bitiş aralığında ki öğeler için sıralama sonuçlarına sabit bir destek uygulanır.|  
|`quadratic`|Sürekli azalan bir destek olan Doğrusal enterpolasyon ile karşılaştırıldığında, Quadratic başlangıçta daha küçük bir hızda azalacak ve daha sonra bitiş aralığına yaklaştıkça, çok daha yüksek bir aralıkla azalır. Bu enterpolasyon seçeneği etiket puanlama işlevlerine izin verilmez.|  
|`logarithmic`|Sürekli azalan bir destek olan Doğrusal enterpolasyon ile karşılaştırıldığında, Logaritmik başlangıçta daha yüksek hızda azalacak ve bitiş aralığına yaklaştıkça çok daha küçük bir aralıkta azalır. Bu enterpolasyon seçeneği etiket puanlama işlevlerine izin verilmez.|  

 ![Grafikte sabit, doğrusal, kuadratik, log10 çizgileri](media/scoring-profiles/azuresearch_scorefunctioninterpolationgrapht.png "AzureSearch_ScoreFunctionInterpolationGrapht")  

##  <a name="set-boostingduration"></a><a name="bkmk_boostdur"></a>BoostingSüresi ayarlayın  
 `boostingDuration`fonksiyonun `freshness` bir özelliğidir. Belirli bir belge için artırmanın duracağı bir son kullanma süresi ayarlamak için kullanırsınız. Örneğin, bir ürün hattını veya markasını 10 günlük bir promosyon dönemi için artırmak için, 10 günlük süreyi bu belgeler için "P10D" olarak belirtirsiniz.  

 `boostingDuration`XSD "dayTimeDuration" değeri (ISO 8601 süre değerinin sınırlı bir alt kümesi) olarak biçimlendirilmelidir. Bunun deseni şudur: "P[nD][T][nH][nM][nS]]".  

 Aşağıdaki tablobirkaç örnek sağlar.  

|Süre|artırmaDuration|  
|--------------|----------------------|  
|1 gün|"P1D"|  
|2 gün 12 saat|"P2DT12H"|  
|15 dakika|"PT15M"|  
|30 gün, 5 saat, 10 dakika ve 6.334 saniye|"P30DT5H10M6.334S"|  

 Daha fazla örnek için [Bkz. XML Şema: Datatypes (W3.org web sitesi)](https://www.w3.org/TR/xmlschema11-2/#dayTimeDuration).  

## <a name="see-also"></a>Ayrıca bkz.  
 [Azure Bilişsel Arama REST](https://docs.microsoft.com/rest/api/searchservice/)   
 [Azure Bilişsel Arama REST API &#40;Dizin oluşturma&#41;](https://docs.microsoft.com/rest/api/searchservice/create-index)   
 [Azure Bilişsel Arama .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/search?view=azure-dotnet)  
