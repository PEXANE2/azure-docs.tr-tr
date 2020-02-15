---
title: İstek aktarma verileri | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları Mobility hizmetini kullanarak genel aktarım verileri isteme hakkında bilgi edineceksiniz.
author: farah-alyasari
ms.author: v-faalya
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 17fbc78b34237c6f5e1e688a88c68bb0a321884f
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77209877"
---
# <a name="request-public-transit-data-using-the-azure-maps-mobility-service"></a>Azure haritalar Mobility hizmetini kullanarak genel aktarım verileri isteme 

Bu makalede, Azure Maps [Mobility hizmetini](https://aka.ms/AzureMapsMobilityService) kullanarak genel aktarım verileri isteme hakkında yönergeler verilmektedir. Transit verileri aktarım duraklarını, rota bilgilerini ve seyahat süresi tahminleri içerir.

Bu makalede şunları öğreneceksiniz:

* [Metro alanını al API](https://aka.ms/AzureMapsMobilityMetro) 'sini kullanarak bir metro alanı kimliği alın
* Yakın transit isteği [Al](https://aka.ms/AzureMapsMobilityNearbyTransit) hizmetini kullanarak yakında geçiş durduruluyor.
* Ortak aktarım kullanarak bir rota planlamak için [Get transit yollarını sorgula API](https://aka.ms/AzureMapsMobilityTransitRoute) .
* [Get transit programı API](https://aka.ms/https://azure.microsoft.com/services/azure-maps/)'sini kullanarak rota için geçiş rotası geometrisi ve ayrıntılı zamanlama isteyin.


## <a name="prerequisites"></a>Önkoşullar

Azure Maps ortak Aktarma API 'Lerine herhangi bir çağrı yapmak için önce bir Azure Maps hesabına ve bir abonelik anahtarına sahip olmanız gerekir. Daha fazla bilgi için, [Hesap oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps) ' daki yönergeleri Izleyerek Azure Maps hesabı oluşturun. Hesabınız için birincil anahtarı almak üzere [birincil anahtar al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).


Bu makale, REST çağrıları oluşturmak için [Postman uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.


## <a name="get-a-metro-area-id"></a>Metro alanı KIMLIĞI al

Belirli bir metropol alanı için aktarım bilgilerini istemek amacıyla bu alanın `metroId` gerekir. [Metro alanını al API 'si](https://aka.ms/AzureMapsMobilityMetro) , Azure Maps Mobility hizmetinin kullanılabildiği Metro alanlarına izin verir. Yanıt, `metroId`, `metroName`ve GeoJSON biçimindeki Metro alanı geometrisinin temsili gibi ayrıntıları içerir.

Seattle-Tacoma Metro alanı KIMLIĞI için Metro alanını almak için bir istek oluşturalım. Bir metro alanı için KIMLIK istemek için aşağıdaki adımları izleyin:

1. Postman uygulamasını açın ve istekleri depolamak için bir koleksiyon oluşturalım. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **koleksiyon**' ı seçin.  Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin.

2. İsteği oluşturmak için **Yeni** ' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin. İstek için bir **istek adı** girin. Önceki adımda oluşturduğunuz koleksiyonu, isteğin kaydedileceği konum olarak seçin. Ardından **Kaydet**’i seçin.
    
    ![Postman 'da istek oluşturma](./media/how-to-request-transit-data/postman-new.png)

3. Oluşturucu sekmesinde http **Al** metodunu seçin ve bir get isteği oluşturmak için aşağıdaki URL 'yi girin. `{subscription-key}`, Azure Maps birincil anahtarınızla değiştirin.

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

5. `metroId`kopyalayın, daha sonra kullanmanız gerekir.

## <a name="request-nearby-transit-stops"></a>Yakın ulaşım isteği durduruluyor

Azure haritalar [yakında ulaşım hizmeti edinme](https://aka.ms/AzureMapsMobilityNearbyTransit) , geçiş nesnelerinde arama yapmanıza olanak tanır.  API, genel aktarım ve belirli bir konum etrafında paylaşılan bisiklet gibi aktarım nesnesi ayrıntılarını döndürür. Daha sonra, belirli bir konum etrafında 300 ölçüm yarıçapı dahilinde, yakın ortak aktarım için arama yapmak üzere hizmete bir istek göndereceğiz. İstekte, daha önce alınan `metroId` dahil etmemiz gerekir.

[Yakında yer almaya](https://aka.ms/AzureMapsMobilityNearbyTransit)yönelik bir istek oluşturmak için aşağıdaki adımları izleyin:

1. Postman 'da, **yeni istek** | **isteği al** ' a tıklayın ve **yakın duraklara ulaşın**.

2. Oluşturucu sekmesinde http **Al** metodunu SEÇIN, API uç noktanız için AŞAĞıDAKI istek URL 'sini girin ve **Gönder**' e tıklayın.

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=47.63096,-122.126&radius=300&objectType=stop
    ```

3. Başarılı bir istekten sonra, yanıt yapısı aşağıdaki gibi görünmelidir:

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

Yanıt yapısını dikkatle gözlemlerseniz, her bir aktarma nesnesi için parametreler içerdiğini görürsünüz. Her transit nesnesi `id`, `type`, `stopName`, `mainTransitType`, `mainAgencyName`ve nesnenin koordinatlarındaki konumu gibi parametrelere sahiptir.

Öğrenme amacıyla, sonraki bölümde yer aldığı Rotamız için bir veri yolu `id` kaynak olarak duraklar.  


## <a name="request-a-transit-route"></a>Geçiş rotası isteme

Azure haritalar [Get transit rotaları API 'si](https://aka.ms/AzureMapsMobilityTransitRoute) Seyahat planlamasına izin verir. Bir kaynaktan hedefe en iyi olası yol seçeneklerini döndürür. Hizmet, yürüyen, biking ve genel aktarım gibi farklı türde seyahat modları sağlar. Daha sonra, en yakın veri yolundan bir yol ile Seattle 'daki Iğne kulede

### <a name="get-location-coordinates-for-destination"></a>Hedefin Konum koordinatlarını al

Alan Iğne kulelerinin Konum koordinatlarını elde etmek için Azure Maps 'un [belirsiz arama hizmeti](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)kullanmasına izin verir.

Benzer arama hizmetine bir istek yapmak için aşağıdaki adımları izleyin:

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **Konum koordinatlarını alın**.

2.  Oluşturucu sekmesinde, http **Al** metodunu seçin, AŞAĞıDAKI istek URL 'sini girin ve **Gönder**' e tıklayın.
 
    ```HTTP
    https://atlas.microsoft.com/search/fuzzy/json?subscription-key={subscription-key}&api-version=1.0&query=space needle
    ```
    
3. Yanıta dikkatle bakarsanız, alan Iğne aramasının sonuçlarında birden çok konum bulunur. Her sonuç **, konum altındaki**Konum koordinatlarını içerir. `lat` ve `lon` ilk sonucun **konumunun** altına kopyalayın.
    
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
    

### <a name="request-route"></a>İstek yolu

Bir yol isteği oluşturmak için aşağıdaki adımları uygulayın:

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **yol bilgilerini alın**.

2. Oluşturucu sekmesinde http **Al** metodunu SEÇIN, API uç noktanız için AŞAĞıDAKI istek URL 'sini girin ve **Gönder**' e tıklayın.

    `modeType` ve `transitType` parametrelerini belirterek, bir veri yolu için ortak geçiş rotaları isteyeceğiz. İstek URL 'SI, önceki bölümlerde alınan konumları içerir. `originType`için artık bir **Stopid**sunuyoruz. `destionationType`için de **konum**vardır.

    İsteğiniz içinde kullanabileceğiniz [URI parametrelerinin listesine](https://aka.ms/AzureMapsMobilityTransitRoute#uri-parameters) bkz. [transit yolları al API 'si](https://aka.ms/AzureMapsMobilityTransitRoute). 
  
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/route/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&originType=stopId&origin=522---2060603&destionationType=position&destination=47.62039,-122.34928&modeType=publicTransit&transitType=bus
    ```

3. Başarılı bir istek olduğunda, yanıt yapısı aşağıdaki gibi görünmelidir:

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

4. Dikkatle gözlemlerseniz, yanıtta birden çok **veri yolu** rotası vardır. Her rotada benzersiz bir **gezı kimliği** ve yolun her bir bayı açıklayan bir Özet vardır. Rota baları iki dur waypoints arasındaki yolun parçasıdır. Daha sonra, yanıttaki `itineraryId` kullanarak en hızlı rota için Ayrıntılar isteyeceğiz.

## <a name="request-fastest-route-itinerary"></a>En hızlı rota programı isteyin

Azure haritalar [Get aktarma programı](https://aka.ms/AzureMapsMobilityTransitItinerary) hizmeti, belirli bir rota için verileri, [Get transit rotaları API](https://aka.ms/AzureMapsMobilityTransitRoute) hizmeti tarafından döndürülen yolun **gezi kimliğini** kullanarak isteyemenize olanak tanır. Bir istek yapmak için aşağıdaki adımları uygulayın:

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **Aktarım bilgilerini alın**.

2. Oluşturucu sekmesinde http **Al** yöntemini seçin. API uç noktanız için aşağıdaki istek URL 'sini girin ve **Gönder**' e tıklayın.

    Yanıtın, ortak geçiş için durma bilgilerini içermesi ve yolun ilerleme ve bisiklet bisikletleri için açık gezinmesi için `detailType` parametresini **geometri** olarak ayarlayacağız.

    ```HTTP
    https://atlas.microsoft.com/mobility/transit/itinerary/json?api-version=1.0&subscription-key={subscription-key}&query={itineraryId}&detailType=geometry
    ```
    
3. Başarılı bir istek olduğunda, yanıt yapısı aşağıdaki gibi görünmelidir:

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

Mobility hizmetini kullanarak gerçek zamanlı veriler isteme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Gerçek zamanlı veriler isteme](how-to-request-real-time-data.md)

Azure haritalar Mobility hizmeti API 'SI belgelerini inceleyin

> [!div class="nextstepaction"]
> [Mobility hizmeti API 'SI belgeleri](https://aka.ms/AzureMapsMobilityService)

