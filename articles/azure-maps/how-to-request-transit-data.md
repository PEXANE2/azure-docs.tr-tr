---
title: Geçiş verilerini isteme | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar Mobilite Hizmeti'ni kullanarak toplu taşıma verilerini nasıl isteyeceğinizi öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: f60b66790342874620971c8f15a1e8ace9a3c7cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335464"
---
# <a name="request-public-transit-data-using-the-azure-maps-mobility-service"></a>Azure Haritalar Mobilite Hizmeti'ni kullanarak toplu taşıma verilerini isteyin 

Bu makalede, toplu taşıma verileri istemek için Azure Haritalar [Mobilite Hizmeti'ni](https://aka.ms/AzureMapsMobilityService) nasıl kullanacağınızı gösterilmektedir. Geçiş verileri transit durakları, rota bilgilerini ve seyahat süresi tahminlerini içerir.

Bu makalede öğreneceksiniz, nasıl:

* [Get Metro Area API'yi](https://aka.ms/AzureMapsMobilityMetro) kullanarak metro alanı kimliği alın
* Yakındaki Transit hizmeti [ne](https://aka.ms/AzureMapsMobilityNearbyTransit) zaman dursa onu talep edin.
* Toplu taşımayı kullanarak bir rota planlamak için [Transit Rotalar API'sini sorgula.](https://aka.ms/AzureMapsMobilityTransitRoute)
* Transit Güzergah ı API'sini kullanarak rota için geçiş rotası [geometrisi](https://aka.ms/https://azure.microsoft.com/services/azure-maps/)ve ayrıntılı zamanlama isteyin.


## <a name="prerequisites"></a>Ön koşullar

Azure Haritalar toplu taşıma API'lerine herhangi bir arama yapmak için öncelikle bir Azure Haritalar hesabınız ve abonelik anahtarınız olması gerekir. Daha fazla bilgi için, Azure Haritalar hesabı oluşturmak için [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin. Hesabınızın birincil anahtarını almak için [birincil anahtarı al](quick-demo-map-app.md#get-the-primary-key-for-your-account) adımlarını izleyin. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.


Bu makalede, REST aramaları oluşturmak için [Postacı uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.


## <a name="get-a-metro-area-id"></a>Metro alanı kimliği alın

Belirli bir metropol alanı için transit bilgilerini istemek için, `metroId` o alanın gerekir. [Metro Alanı Al API,](https://aka.ms/AzureMapsMobilityMetro) Azure Haritalar Mobilite Hizmetinin kullanılabildiği metro alanlarını talep etmenizi sağlar. `metroId`Yanıt, `metroName`metro alanı geometrisinin GeoJSON formatında temsili ve temsili gibi ayrıntıları içerir.

Seattle-Tacoma metro bölgesi kimliği için metro bölgesini almak için bir istekte bulunalım. Metro alanı için kimlik istemek için aşağıdaki adımları tamamlayın:

1. Postacı uygulamasını açın ve istekleri depolamak için bir koleksiyon oluşturalım. Postacı uygulamasının üst kısmında **Yeni'yi**seçin. Yeni **Oluştur** penceresinde **Koleksiyon'u**seçin.  Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin.

2. İstek oluşturmak için Yeniden **Yeni'yi** seçin. Yeni **Oluştur** penceresinde **İstek'i**seçin. İstek için bir **İstek adı** girin. İsteği kaydetmek için önceki adımda oluşturduğunuz koleksiyonu konum olarak seçin. Ardından **Kaydet**’i seçin.
    
    ![Postacı'da istek oluşturma](./media/how-to-request-transit-data/postman-new.png)

3. Oluşturucu sekmesinde **HTTP'yi AL** yöntemini seçin ve GET isteği oluşturmak için aşağıdaki URL'yi girin. Azure `{subscription-key}`Haritalar birincil anahtarınızla değiştirin.

    ```HTTP
    https://atlas.microsoft.com/mobility/metroArea/id/json?subscription-key={subscription-key}&api-version=1.0&query=47.63096,-122.126
    ```

4. Başarılı bir istekten sonra aşağıdaki yanıtı alırsınız:

    ```JSON
    {
        "results": [
            {
                "metroId": 522,
                "metroName": "Seattle–Tacoma–Bellevue, WA",
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 48.5853,
                        "longitude": -124.80934
                    },
                    "btmRightPoint": {
                        "latitude": 46.90534,
                        "longitude": -121.55032
                    }
                },
                "geometry": {
                    "type": "Polygon",
                    "coordinates": [
                        [
                            [
                                -121.99604,
                                47.16147
                            ],
                            [
                                -121.97051,
                                47.17222
                            ],
                            [
                                -121.96308,
                                47.17671
                            ],
                            ...,
                            ...,
                            ...,
                            [
                                -122.01525,
                                47.16008
                            ],
                            [
                                -122.00553,
                                47.15919
                            ],
                            [
                                -121.99604,
                                47.16147
                            ]
                        ]
                    ]
                }
            }
        ]
    }
    ```

5. Kopyalayın `metroId`, biz daha sonra kullanmak gerekir.

## <a name="request-nearby-transit-stops"></a>Yakındaki transit duraklarını talep etme

Azure Haritalar [Yakın Geçiş](https://aka.ms/AzureMapsMobilityNearbyTransit) olsun hizmeti, geçiş nesnelerinde arama yapmanızı sağlar.  API, toplu taşıma durakları ve belirli bir konumun etrafında paylaşılan bisikletler gibi geçiş nesnesi ayrıntılarını döndürür. Daha sonra, verilen yerin etrafındaki 300 metre yarıçap içinde yakındaki toplu taşıma duraklarını aramak için servise bir istekte bulunacağız. İstek, biz daha önce `metroId` alınan eklememiz gerekir.

[Yakındaki Geçişi Al'a](https://aka.ms/AzureMapsMobilityNearbyTransit)bir istekte bulunmak için aşağıdaki adımları izleyin:

1. Postacı'da **Yeni İstek** | **GET isteğini** tıklatın ve yakındaki **durakları alın**adını alın.

2. Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin, API bitiş noktanız için aşağıdaki istek URL'sini girin ve **Gönder'i**tıklatın.

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=47.63096,-122.126&radius=300&objectType=stop
    ```

3. Başarılı bir istekten sonra yanıt yapısı aşağıdaki gibi görünmelidir:

    ```JSON
    {
        "results": [
            {
                "id": "522---2060603",
                "type": "stop",
                "objectDetails": {
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631504,
                    "longitude": -122.125275
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632413,
                        "longitude": -122.12659
                    },
                    "btmRightPoint": {
                        "latitude": 47.630594,
                        "longitude": -122.123959
                    }
                }
            },
            {
                "id": "522---2061020",
                "type": "stop",
                "objectDetails": {
                    "stopKey": "68372",
                    "stopName": "NE 24th St & 160th Ave NE",
                    "stopCode": "68372",
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631409,
                    "longitude": -122.127136
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632318,
                        "longitude": -122.128451
                    },
                    "btmRightPoint": {
                        "latitude": 47.630499,
                        "longitude": -122.12582
                    }
                }
            },
            {
                "id": "522---2060604",
                "type": "stop",
                "objectDetails": {
                    "stopKey": "71310",
                    "stopName": "NE 24th St & 160th Ave NE",
                    "stopCode": "71310",
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                },
                "position": {
                    "latitude": 47.631565,
                    "longitude": -122.127808
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 47.632474,
                        "longitude": -122.129124
                    },
                    "btmRightPoint": {
                        "latitude": 47.630655,
                        "longitude": -122.126492
                    }
                }
            }
        ]
    }   
    ```

Yanıt yapısını dikkatle gözlemlerseniz, her aktarım nesnesi için parametreler içerdiğini görürsünüz. Her `id`aktarım nesnesinin, `type` `stopName`, `mainTransitType` `mainAgencyName`, , , ve konum, koordinatlar, nesnenin gibi parametreleri vardır.

Öğrenmek için, bir sonraki bölümde `id` rotamız için, başlangıç olarak bir otobüs durağı kullanacağız.  


## <a name="request-a-transit-route"></a>Transit rota isteme

Azure Haritalar [Geçiş Rotaları API'si](https://aka.ms/AzureMapsMobilityTransitRoute) yolculuk planlamasına izin verir. Bir kaynaktan hedefe mümkün olan en iyi rota seçeneklerini döndürür. Hizmet, yürüyüş, bisiklet ve toplu taşıma gibi farklı türde seyahat modları sunmaktadır. Sonra, seattle'daki Space Needle kulesine en yakın otobüs durağından bir rota yı arayacağız.

### <a name="get-location-coordinates-for-destination"></a>Hedef için konum koordinatlarını alma

Space Needle kulesinin konum koordinatlarını elde etmek için Azure Haritalar [Bulanık Arama Hizmeti'ni](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)kullanalım.

Bulanık arama hizmetine istekte bulunmak için aşağıdaki adımları izleyin:

1. Postacı'da **Yeni İstek** | **GET isteğini** tıklatın ve konum **koordinatlarını alın**adını alın.

2.  Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin, aşağıdaki istek URL'sini girin ve **Gönder'i**tıklatın.
 
    ```HTTP
    https://atlas.microsoft.com/search/fuzzy/json?subscription-key={subscription-key}&api-version=1.0&query=space needle
    ```
    
3. Yanıta dikkatlice bakarsanız, Space Needle aramasının sonuçlarında birden çok konum içerir. Her sonuç **konumu**altında konum koordinatları içerir. İlk `lat` sonucun `lon` **konumunu** ve altında kopyalayın.
    
   ```JSON
   {
        "summary": {
            "query": "space needle",
            "queryType": "NON_NEAR",
            "queryTime": 35,
            "numResults": 8,
            "offset": 0,
            "totalResults": 11,
            "fuzzyLevel": 1
        },
        "results": [
            {
                "type": "POI",
                "id": "US/POI/p0/6993440",
                "score": 4.67369,
                "info": "search:ta:840539001406144-US",
                "poi": {
                    "name": "Space Needle",
                    "phone": "+(1)-(206)-9052100",
                    "categorySet": [
                        {
                            "id": 7376009
                        }
                    ],
                    "url": "www.spaceneedle.com",
                    "categories": [
                        "important tourist attraction",
                        "tower"
                    ],
                    "classifications": [
                        {
                            "code": "IMPORTANT_TOURIST_ATTRACTION",
                            "names": [
                                {
                                    "nameLocale": "en-US",
                                    "name": "important tourist attraction"
                                },
                                {
                                    "nameLocale": "en-US",
                                    "name": "tower"
                                }
                            ]
                        }
                    ]
                },
                "address": {
                    "streetNumber": "400",
                    "streetName": "Broad St",
                    "municipalitySubdivision": "South Lake Union, Seattle, Lower Queen Anne",
                    "municipality": "Seattle",
                    "countrySecondarySubdivision": "King",
                    "countryTertiarySubdivision": "Seattle",
                    "countrySubdivision": "WA",
                    "postalCode": "98109",
                    "countryCode": "US",
                    "country": "United States",
                    "countryCodeISO3": "USA",
                    "freeformAddress": "400 Broad St, Seattle, WA 98109",
                    "localName": "Seattle",
                    "countrySubdivisionName": "Washington"
                },
                "position": {
                    "lat": 47.62039,
                    "lon": -122.34928
                },
                "viewport": {
                    "topLeftPoint": {
                        "lat": 47.62129,
                        "lon": -122.35061
                    },
                    "btmRightPoint": {
                        "lat": 47.61949,
                        "lon": -122.34795
                    }
                },
                "entryPoints": [
                    {
                        "type": "main",
                        "position": {
                            "lat": 47.61982,
                            "lon": -122.34886
                        }
                    }
                ]
            },
            ...,
            ...,
            ...
        ]
    }
    ``` 
    

### <a name="request-route"></a>İstek rotası

Rota isteğinde bulunmak için aşağıdaki adımları tamamlayın:

1. Postacı'da, **Yeni İstek** | **GET isteğini** tıklatın ve rota **bilgilerini alın**adını alın.

2. Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin, API bitiş noktanız için aşağıdaki istek URL'sini girin ve **Gönder'i**tıklatın.

    Parametreleri `modeType` ve parametreleri `transitType` belirterek otobüs için toplu taşıma güzergahları talep edeceğiz. İstek URL'si, önceki bölümlerde alınan konumları içerir. `originType`Artık bir **stopid**imiz var. Ve bunun `destionationType`için, **biz pozisyon**var .

    Transit [Rotaları](https://aka.ms/AzureMapsMobilityTransitRoute#uri-parameters) AL API'ye isteğinizde kullanabileceğiniz URI [parametrelerinin](https://aka.ms/AzureMapsMobilityTransitRoute)listesine bakın. 
  
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/route/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&originType=stopId&origin=522---2060603&destionationType=position&destination=47.62039,-122.34928&modeType=publicTransit&transitType=bus
    ```

3. Başarılı bir istek üzerine, yanıt yapısı aşağıdaki gibi görünmelidir:

    ```JSON
    {
        "results": [
            {
                "itineraryId": "cb6b6b6f-5cda-451e-b68d-2e97971dd60c---20190906BBBEC4D2219A436A9D794224978C9BBF:0---522",
                "departureTime": "2019-09-07T01:01:50Z",
                "arrivalTime": "2019-09-07T02:16:33Z",
                "travelTimeInSeconds": 4483,
                "numberOfLegs": 8,
                "legs": [
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:01:50Z",
                        "legEndTime": "2019-09-07T01:01:50Z",
                        "caption": "249"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T01:01:50Z",
                        "legEndTime": "2019-09-07T01:26:00Z",
                        "caption": "249",
                        "lengthInMeters": 9139
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:26:00Z",
                        "legEndTime": "2019-09-07T01:28:00Z",
                        "caption": "255"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T01:28:00Z",
                        "legEndTime": "2019-09-07T01:57:21Z",
                        "caption": "255",
                        "lengthInMeters": 13136
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-09-07T01:57:22Z",
                        "legEndTime": "2019-09-07T02:01:27Z",
                        "caption": "Denny Way",
                        "lengthInMeters": 308
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T02:01:27Z",
                        "legEndTime": "2019-09-07T02:06:33Z",
                        "caption": "8"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T02:06:33Z",
                        "legEndTime": "2019-09-07T02:12:41Z",
                        "caption": "8",
                        "lengthInMeters": 1060
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-09-07T02:12:42Z",
                        "legEndTime": "2019-09-07T02:16:33Z",
                        "lengthInMeters": 251
                    }
                ]
            },
            ...,
            {
                "itineraryId": "cb6b6b6f-5cda-451e-b68d-2e97971dd60c---20190906BBBEC4D2219A436A9D794224978C9BBF:2---522",
                "departureTime": "2019-09-07T00:49:32Z",
                "arrivalTime": "2019-09-07T02:20:06Z",
                "travelTimeInSeconds": 5434,
                "numberOfLegs": 10,
                "legs": [
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T00:49:32Z",
                        "legEndTime": "2019-09-07T00:49:32Z",
                        "caption": "226"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T00:49:32Z",
                        "legEndTime": "2019-09-07T01:15:00Z",
                        "caption": "226",
                        "lengthInMeters": 6792
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:15:00Z",
                        "legEndTime": "2019-09-07T01:20:00Z",
                        "caption": "241"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T01:20:00Z",
                        "legEndTime": "2019-09-07T01:28:00Z",
                        "caption": "241",
                        "lengthInMeters": 3397
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:28:00Z",
                        "legEndTime": "2019-09-07T01:33:00Z",
                        "caption": "550"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T01:33:00Z",
                        "legEndTime": "2019-09-07T01:58:00Z",
                        "caption": "550",
                        "lengthInMeters": 12899
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-09-07T01:58:01Z",
                        "legEndTime": "2019-09-07T01:59:21Z",
                        "caption": "4th Avenue South",
                        "lengthInMeters": 99
                    },
                    {
                        "legType": "Wait",
                        "legStartTime": "2019-09-07T01:59:21Z",
                        "legEndTime": "2019-09-07T02:01:00Z",
                        "caption": "33"
                    },
                    {
                        "legType": "Bus",
                        "legStartTime": "2019-09-07T02:01:00Z",
                        "legEndTime": "2019-09-07T02:13:29Z",
                        "caption": "33,24",
                        "lengthInMeters": 2447
                    },
                    {
                        "legType": "Walk",
                        "legStartTime": "2019-09-07T02:13:30Z",
                        "legEndTime": "2019-09-07T02:20:06Z",
                        "lengthInMeters": 457
                    }
                ]
            }
        ]
    }
    ```

4. Dikkatlice gözlemlerseniz, yanıtta birden fazla **otobüs** güzergahı vardır. Her rotanın benzersiz bir **güzergah kimliği** ve rotanın her bacağını açıklayan bir özeti vardır. Rota bacağı, rotanın iki durak yol noktası arasındaki bölümüdür. Ardından, yanıtı kullanarak `itineraryId` en hızlı rota için ayrıntılar isteyeceğiz.

## <a name="request-fastest-route-itinerary"></a>En hızlı rota güzergahı nı isteyin

Azure Haritalar [Transit Güzergah](https://aka.ms/AzureMapsMobilityTransitItinerary) al hizmeti, Geçiş Yolları [API](https://aka.ms/AzureMapsMobilityTransitRoute) hizmeti tarafından döndürülen rotanın **güzergah kimliğini** kullanarak belirli bir rota için veri istemenize olanak tanır. Bir istekte bulunmak için aşağıdaki adımları tamamlayın:

1. Postacı'da, **Yeni İstek** | **GET isteğini** tıklatın ve geçiş **bilgilerini alın**adını alın.

2. Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin. API bitiş noktanız için aşağıdaki istek URL'sini girin ve **Gönder'i**tıklatın.

    Yanıtın toplu `detailType` taşıma için dur bilgilerini ve rotanın yürüyüş ve bisiklet ayakları için turn-by-turn navigasyon bilgilerini içerecek şekilde parametreyi **geometriye** ayarlayacağız.

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/itinerary/json?api-version=1.0&subscription-key={subscription-key}&query={itineraryId}&detailType=geometry
    ```
    
3. Başarılı bir istek üzerine, yanıt yapısı aşağıdaki gibi görünmelidir:

    ```JSON
    {
        "departureTime": "2019-09-07T01:01:50Z",
        "arrivalTime": "2019-09-07T02:16:33Z",
        "legs": [
            {
                "legType": "Wait",
                "legStartTime": "2019-09-07T01:01:50Z",
                "legEndTime": "2019-09-07T01:01:50Z",
                "lineGroup": {
                    "lineGroupId": "522---666077",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "caption1": "Overlake TC - South Bellevue P&R",
                    "caption2": "249 Overlake TC - South Bellevue P&R",
                    "color": "347E5D",
                    "transitType": "Bus"
                },
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R"
                },
                "stops": [
                    {
                        "stopId": "522---2060603",
                        "stopKey": "71300",
                        "stopName": "NE 24th St & 162nd Ave NE",
                        "stopCode": "71300",
                        "position": {
                            "latitude": 47.631504,
                            "longitude": -122.125275
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    },
                    {
                        "stopId": "522---2061703",
                        "stopKey": "74450",
                        "stopName": "South Kirkland P&R & 108th Ave NE",
                        "stopCode": "74450",
                        "position": {
                            "latitude": 47.643852,
                            "longitude": -122.196693
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    }
                ],
                "waitOnVehicle": false
            },
            {
                "legType": "Bus",
                "legStartTime": "2019-09-07T01:01:50Z",
                "legEndTime": "2019-09-07T01:26:00Z",
                "lineGroup": {
                    "lineGroupId": "522---666077",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "caption1": "Overlake TC - South Bellevue P&R",
                    "caption2": "249 Overlake TC - South Bellevue P&R",
                    "color": "347E5D",
                    "transitType": "Bus"
                },
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R"
                },
                "stops": [
                    {
                        "stopId": "522---2060603",
                        "stopKey": "71300",
                        "stopName": "NE 24th St & 162nd Ave NE",
                        "stopCode": "71300",
                        "position": {
                            "latitude": 47.631504,
                            "longitude": -122.125275
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    },
                    {
                        "stopId": "522---2060604",
                        "stopKey": "71310",
                        "stopName": "NE 24th St & 160th Ave NE",
                        "stopCode": "71310",
                        "position": {
                            "latitude": 47.631565,
                            "longitude": -122.127808
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    },
                    ...,
                    ...,
                    {
                        "stopId": "522---2061704",
                        "stopKey": "74451",
                        "stopName": "Northup Way & NE 33rd Pl",
                        "stopCode": "74451",
                        "position": {
                            "latitude": 47.640911,
                            "longitude": -122.194443
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    },
                    {
                        "stopId": "522---2061703",
                        "stopKey": "74450",
                        "stopName": "South Kirkland P&R & 108th Ave NE",
                        "stopCode": "74450",
                        "position": {
                            "latitude": 47.643852,
                            "longitude": -122.196693
                        },
                        "mainTransitType": "Bus",
                        "mainAgencyId": "522---5872"
                    }
                ],
                "geometry": {
                    "type": "LineString",
                    "coordinates": [
                        [
                            -122.12527,
                            47.63143
                        ],
                        [
                            -122.12529,
                            47.63143
                        ],
                        [
                            -122.12561,
                            47.63144
                        ],
                        [
                            -122.12701,
                            47.63148
                        ],
                        ...,
                        ...,
                        ...,
                        [
                            -122.19601,
                            47.64304
                        ],
                        [
                            -122.19584,
                            47.64315
                        ],
                        [
                            -122.19677,
                            47.6438
                        ]
                    ]
                }
            },
            ...,
            ...,
            ...,
            {
                "legType": "Walk",
                "legStartTime": "2019-09-07T02:12:42Z",
                "legEndTime": "2019-09-07T02:16:33Z",
                "steps": [
                    {
                        "direction": {
                            "relativeDirection": "depart"
                        },
                        "streetName": "Denny Way"
                    },
                    {
                        "direction": {
                            "relativeDirection": "right"
                        },
                        "streetName": "4th Avenue North"
                    },
                    {
                        "direction": {
                            "relativeDirection": "right"
                        },
                        "streetName": "Broad Street"
                    }
                ],
                "origin": {
                    "position": {
                        "latitude": 47.618578,
                        "longitude": -122.348058
                    }
                },
                "destination": {
                    "position": {
                        "latitude": 47.62039,
                        "longitude": -122.34928
                    }
                },
                "geometry": {
                    "type": "LineString",
                    "coordinates": [
                        [
                            -122.34806,
                            47.61857
                        ],
                        [
                            -122.3481,
                            47.61857
                        ],
                        [
                            -122.34894,
                            47.61858
                        ],
                        [
                            -122.34892,
                            47.61964
                        ],
                        [
                            -122.34877,
                            47.61975
                        ],
                        [
                            -122.3492,
                            47.62001
                        ],
                        [
                            -122.34918,
                            47.62003
                        ],
                        [
                            -122.34917,
                            47.62006
                        ],
                        [
                            -122.34916,
                            47.62008
                        ],
                        [
                            -122.34916,
                            47.62008
                        ],
                        [
                            -122.34916,
                            47.62008
                        ],
                        [
                            -122.34916,
                            47.62008
                        ],
                        [
                            -122.34928,
                            47.62039
                        ]
                    ]
                }
            }
        ]
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

Mobilite Hizmeti'ni kullanarak gerçek zamanlı verileri nasıl isteyeceğizi öğrenin:

> [!div class="nextstepaction"]
> [Gerçek zamanlı veri isteme](how-to-request-real-time-data.md)

Azure Haritalar Mobilite Hizmeti API belgelerini keşfedin

> [!div class="nextstepaction"]
> [Mobilite Hizmeti API belgeleri](https://aka.ms/AzureMapsMobilityService)

