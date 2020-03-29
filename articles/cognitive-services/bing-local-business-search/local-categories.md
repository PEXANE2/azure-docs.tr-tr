---
title: Bing Yerel İş Arama API'si için arama kategorileri
titleSuffix: Azure Cognitive Services
description: Bing Yerel İşletme arama API bitiş noktası için arama kategorilerini nasıl belirteceklerini öğrenmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: 56b94d66eb0929d2fd0ca74a1a631d229330adfa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "69906389"
---
# <a name="search-categories-for-the-bing-local-business-search-api"></a>Bing Yerel İş Arama API'si için arama kategorileri

Bing Yerel İş Arama API'si, bir kullanıcının konumunu kapatan sonuçlara öncelik vererek, çeşitli kategorilerdeki yerel işletme varlıklarını aramanızı sağlar. Bu aramaları aramalara ve `localCircularView` `localMapView` [parametrelerle](specify-geographic-search.md)birlikte ekleyebilirsiniz.


## <a name="toplevel-categories"></a>Üst Düzey Kategoriler 

Aşağıdaki türler, ana arama kategorilerini tanımlar.  `localCategories` Parametreye atanan virgülle sınırlandırılmış bir liste kullanılarak birden fazla kategori belirtilebilir.  
- EatDrink 
- SeeDo 
- Dükkanı 
- OtellerAndMotels 
- Bankalar ve Kredi Birlikleri 
- Otopark 
- Hastane 

## <a name="sub-categories"></a>Alt Kategoriler
Alt kategoriler . `localCategories` Alt kategoriler daha spesifik kategorilerdir. Aynı virgülle sınırlandırılmış listede bir C kategorisi ve onun alt kategorilerinden Biri S belirtirseniz, yalnızca C'yi belirtdiğiniz gibi aynı sonuçları alırsınız anlamında alt tantiflerdir.

### <a name="eat-drink"></a>İçki Yiyin 
|  |  |  |  |
| - | - | - | - |
| Bira FabrikalarıAndBrewPubs | Kokteyl Salonları | Afrika Restoranları |
| Amerikan Restoranları | Simit | Barbekü Restoranları |
| Taverna | Spor Barlar | Bar |
| BarlarGrillsAndPubs | Büfe Restoranlar| Belçika Restoranları | 
| İngiliz Restoranları | CafeRestoranlar | Karayip Restoranları |
| Çin Restoranları | KahveAndTea | Şarküteri | 
| Teslimat Hizmeti | Diners | İndirimli Mağazalar | 
| Donuts | Fastfood | Fransız Restoranları | 
| Dondurulmuş Yoğurt | GermanRestaurants | Süpermarket | 
| Yunan Restoranları | Bakkal | Hawaii Restoranları | 
| Macar Restoranları | IceCreamAndFrozenDesserts | IndianRestaurants | 
| İtalyan Restoranları | Japon Restoranları | Meyve suyu | 
| Kore Restoranları | Likör Mağazaları | Meksika Restoranları |
| OrtaDoğu Restoranları | Pizza | Lehçe Restoranlar | 
| Portekiz Restoranları | Simit | Restoran | 
| RusandUkrayna Restoranları | Sandviç | Deniz Ürünleri Restoranları | 
| İspanyol Restoranları | SteakhouseRestoranlar | Suşi Restoranları | 
| Paket | ThaiRestoranlar | Türk Restoranları | 
| VejetaryenAndVeganRestoranlar | VietnamRestoranları|  |
 
### <a name="see-do"></a>Bkz. Yap 
|  |  |  |
| -- | -- | -- |
| Lunaparklar | Turistik yer | Karnaval |
| Kumarhane | YerlerAndHistoricalSites | MinyatürGolf Sahaları |
| Sinema Salonları | Müze | Park |
| GeziTurları | Turist Bilgileri | Hayvanat bahçe |
 
### <a name="shop"></a>Dükkanı 
|  |  |  |
| -- | -- | -- |
| Antika Mağazaları | Kitapçılarda | CDAndRecordMağazaları |
| Çocuk GiyimMağazaları | Puro ve Tütün Dükkanları | Çizgi Roman Mağazaları |
| Büyük Mağazalar | İndirimli Mağazalar | Bit PazarlarıAndBazaars |
| Mobilya Mağazaları | AnasayfaImprovementStores | TakıAndWatchesMağazaları |
| Mutfak Mağazaları | Likör Mağazaları | Alışveriş MerkezleriAndShoppingCenters |
| MensClothingStores | Müzik Mağazaları | OutletMağazaları |
| PetShops | PetsupplyMağazaları | OkulandofficeSupplyStores |
| Ayakkabı Mağazaları | SportingGoodsMağazaları | ToyAndGameMağazaları |
| VitaminandSupplementMağazaları | Kadın GiyimMağazaları |  |


## <a name="examples-of-local-categories-search"></a>Yerel Kategoriler arama örnekleri

Aşağıdaki örnekler `localCategories` parametreye göre sonuçları Al:

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=HotelsAndMotels`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=EatDrink`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=Shop`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=Hospitals`

Aşağıdaki sorgu, 'hastane' sonuçlarının sayısını Bing Yerel İş Arama API'sinden döndürülen ilk üç le sınırlar:

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localCategories=Hospitals&count=3&offset=0`

Aşağıdaki örnek JSON yanıtı büyük Seattle bölgesinde üç hastane içerir:

```json
BingAPIs-TraceId: 68AFB51807C6485CAB8AAF20E232EFFF
BingAPIs-SessionId: F89E7B8539B34BF58AAF811485E83B20
X-MSEdge-ClientID: 1C44E64DBFAA6BCA1270EADDBE7D6A22
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 68AFB51807C6485CAB8AAF20E232EFFF Ref B: CO1EDGE0108 Ref C: 2018-10-22T18:52:28Z

{
   "_type": "SearchResponse",
   "queryContext": {
      "originalQuery": ""
   },
   "places": {
      "readLink": "https:\/\/www.bingapis.com\/api\/v7\/places\/search?q=",
      "totalEstimatedMatches": 0,
      "value": [
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.0",
            "name": "Overlake Hospital Medical Center",
            "url": "http:\/\/www.overlakehospital.org\/",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6204986572266,
               "longitude": -122.185829162598
            },
            "routablePoint": {
               "latitude": 47.6204986572266,
               "longitude": -122.185668945312
            },
            "address": {
               "streetAddress": "1035 116th Ave NE",
               "addressLocality": "Bellevue",
               "addressRegion": "WA",
               "postalCode": "98004",
               "addressCountry": "US",
               "neighborhood": "",
               "text": "1035 116th Ave NE, Bellevue, WA, 98004"
            },
            "telephone": "(425) 688-5000"
         },
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.1",
            "name": "Virginia Mason University Village Medical Center",
            "url": "https:\/\/virginiamason.org\/Seattle",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6095390319824,
               "longitude": -122.327941894531
            },
            "routablePoint": {
               "latitude": 47.6093978881836,
               "longitude": -122.328277587891
            },
            "address": {
               "streetAddress": "1100 9th Ave",
               "addressLocality": "Seattle",
               "addressRegion": "WA",
               "postalCode": "98101",
               "addressCountry": "US",
               "neighborhood": "University District",
               "text": "1100 9th Ave, Seattle, WA, 98101"
            },
            "telephone": "(206) 223-6600"
         },
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.2",
            "name": "Seattle Children’s Hospital",
            "url": "http:\/\/www.seattlechildrens.org\/",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6625061035156,
               "longitude": -122.283760070801
            },
            "routablePoint": {
               "latitude": 47.6631507873535,
               "longitude": -122.284614562988
            },
            "address": {
               "streetAddress": "4800 Sand Point Way NE",
               "addressLocality": "Seattle",
               "addressRegion": "WA",
               "postalCode": "98105",
               "addressCountry": "US",
               "neighborhood": "Laurelhurst",
               "text": "4800 Sand Point Way NE, Seattle, WA, 98105"
            },
            "telephone": "(206) 987-2000"
         }
      ],
      "searchAction": {

      }
   }
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Coğrafi arama sınırları](specify-geographic-search.md)
- [Sorgu ve yanıt](local-search-query-response.md)
- [C'de Hızlı Başlangıç #](quickstarts/local-quickstart.md)
