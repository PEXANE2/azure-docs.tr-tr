---
title: Gerçek zamanlı transit verileri isteme | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar Mobilite Hizmetini kullanarak gerçek zamanlı veri isteyin.
author: philmea
ms.author: philmea
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: d3145181a863bf8188dd0b0bb52cd2efc662ce2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335478"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Azure Haritalar Mobilite Hizmetini kullanarak gerçek zamanlı veri isteyin

Bu makalede, gerçek zamanlı aktarım verileri istemek için Azure Haritalar [Mobilite Hizmeti'ni](https://aka.ms/AzureMapsMobilityService) nasıl kullanacağınızı gösterilmektedir.

Bu makalede nasıl öğreneceksiniz:


 * Belirli bir durakta gelen tüm hatlar için sonraki gerçek zamanlı varışları talep edin
 * Belirli bir bisiklet yerleştirme istasyonu için gerçek zamanlı bilgi isteyin.


## <a name="prerequisites"></a>Ön koşullar

Azure Haritalar toplu taşıma API'lerine herhangi bir arama yapmak için öncelikle bir Azure Haritalar hesabınız ve abonelik anahtarınız olması gerekir. Daha fazla bilgi için, Azure Haritalar hesabı oluşturmak için [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin. Hesabınızın birincil anahtarını almak için [birincil anahtarı al](quick-demo-map-app.md#get-the-primary-key-for-your-account) adımlarını izleyin. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.


Bu makalede, REST aramaları oluşturmak için [Postacı uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.


## <a name="request-real-time-arrivals-for-a-stop"></a>Bir durak için gerçek zamanlı varış talep

Belirli bir toplu taşıma durağının gerçek zamanlı varış verilerini istemek için, Azure Haritalar [Mobilite Hizmetinin](https://aka.ms/AzureMapsMobilityService) [Gerçek Zamanlı Gelen Yolcu API'sine](https://aka.ms/AzureMapsMobilityRealTimeArrivals) talepte bulunmanız gerekir. Talebi tamamlamak için **metroID** ve **stopID'ye** ihtiyacınız olacak. Bu parametreleri nasıl talep edebilirsiniz hakkında daha fazla bilgi edinmek için [toplu taşıma rotalarını](https://aka.ms/AMapsHowToGuidePublicTransitRouting)nasıl talep edebilirsiniz kılavuzumuza bakın. 

"Seattle-Tacoma-Bellevue, WA" alanının metro kimliği olan metro kimliğimiz olarak "522"yi kullanalım. "522---2060603" stop id olarak kullanın, bu otobüs durağı "Ne 24 St & 162 Ave Ne, Bellevue WA" yer almaktadır. Sonraki beş gerçek zamanlı varış verisini istemek için, bu duraktaki tüm canlı varışlar için aşağıdaki adımları tamamlayın:

1. Postacı uygulamasını açın ve istekleri depolamak için bir koleksiyon oluşturalım. Postacı uygulamasının üst kısmında **Yeni'yi**seçin. Yeni **Oluştur** penceresinde **Koleksiyon'u**seçin.  Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin.

2. İstek oluşturmak için Yeniden **Yeni'yi** seçin. Yeni **Oluştur** penceresinde **İstek'i**seçin. İstek için bir **İstek adı** girin. İsteğin kaydedildiği konum olarak önceki adımda oluşturduğunuz koleksiyonu seçin. Ardından **Kaydet**’i seçin.

    ![Postacı'da istek oluşturma](./media/how-to-request-transit-data/postman-new.png)

3. Oluşturucu sekmesinde **HTTP'yi AL** yöntemini seçin ve GET isteği oluşturmak için aşağıdaki URL'yi girin. Azure `{subscription-key}`Haritalar birincil anahtarınızla değiştirin.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

4. Başarılı bir istekten sonra aşağıdaki yanıtı alırsınız.  'scheduleType' parametresinin tahmini varış süresinin gerçek zamanlı veya statik verilere mi dayandığını tanımladığına dikkat edin.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```


## <a name="real-time-data-for-bike-docking-station"></a>Bisiklet yerleştirme istasyonu için gerçek zamanlı veriler

[Transit Dock Info API'si alın,](https://aka.ms/AzureMapsMobilityTransitDock) kullanıcıların statik ve gerçek zamanlı bilgi istemesine olanak tanır. Örneğin, kullanıcılar bir bisiklet veya scooter yerleştirme istasyonu için kullanılabilirlik ve boş alan bilgileri isteyebilir. [Toplu Taşıma Dock Bilgi API'si](https://aka.ms/AzureMapsMobilityTransitDock) de Azure Haritalar [Mobilite Hizmeti'nin](https://aka.ms/AzureMapsMobilityService)bir parçasıdır.

[Transit Dock Bilgi API'sine bir](https://aka.ms/AzureMapsMobilityTransitDock)istekte bulunmak için, bu istasyon için **dockId** gerekir. "bikeDock" adı verilen **objectType** parametresi ile [Yakındaki Transit API'ye](https://aka.ms/AzureMapsMobilityNearbyTransit) arama isteğinde bulunarak dock kimliğini alabilirsiniz. Bisikletler için bir yerleştirme istasyonunun gerçek zamanlı verilerini almak için aşağıdaki adımları izleyin.


### <a name="get-dock-id"></a>Dock kimliğini alın

**DockID**almak için, Yakındaki Transit API'ye istekte bulunmak için aşağıdaki adımları izleyin:

1. Postacı'da, **Yeni İstek** | **GET isteğini** tıklatın ve dock **kimliğini alın**adını alın.

2.  Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin, aşağıdaki istek URL'sini girin ve **Gönder'i**tıklatın.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. Başarılı bir istekten sonra aşağıdaki yanıtı alırsınız. Artık yanıtta, daha sonra Geçiş Dock Bilgi API'si alın isteğinde sorgu parametresi olarak kullanılabilecek **bir id'e** sahip olduğumuza dikkat edin.

    ```JSON
    {
        "results": [
            {
                "id": "121---4640799",
                "type": "bikeDock",
                "objectDetails": {
                    "availableVehicles": 0,
                    "vacantLocations": 31,
                    "lastUpdated": "2019-09-07T00:55:19Z",
                    "operatorInfo": {
                        "id": "121---80",
                        "name": "Citi Bike"
                    }
                },
                "position": {
                    "latitude": 40.767128,
                    "longitude": -73.962243
                },
                "viewport": {
                    "topLeftPoint": {
                        "latitude": 40.768039,
                        "longitude": -73.963413
                    },
                    "btmRightPoint": {
                        "latitude": 40.766216,
                        "longitude": -73.961072
                    }
                }
            }
        ]
    }
    ```


### <a name="get-real-time-bike-dock-status"></a>Gerçek zamanlı bisiklet dock durumu alın

Seçili dock için gerçek zamanlı veri almak için Transit Dock Bilgi API'sine istekte bulunmak için aşağıdaki adımları izleyin.

1. Postacı'da, **Yeni İstek** | **GET isteğini** tıklatın ve gerçek zamanlı dock **verilerini alın**adını verin.

2.  Oluşturucu sekmesinde, **HTTP'yi AL** yöntemini seçin, aşağıdaki istek URL'sini girin ve **Gönder'i**tıklatın.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. Başarılı bir istekten sonra, aşağıdaki yapının yanıtını alırsınız:

    ```JSON
    {
        "availableVehicles": 0,
        "vacantLocations": 31,
        "position": {
            "latitude": 40.767128,
            "longitude": -73.962246
        },
        "lastUpdated": "2019-09-07T00:55:19Z",
        "operatorInfo": {
            "id": "121---80",
            "name": "Citi Bike"
        }
    }
    ```


## <a name="next-steps"></a>Sonraki adımlar

Mobilite Hizmetini kullanarak transit verilerini nasıl isteyeceğizi öğrenin:

> [!div class="nextstepaction"]
> [Aktarım verileri nasıl istenir?](how-to-request-transit-data.md)

Azure Haritalar Mobilite Hizmeti API belgelerini keşfedin:

> [!div class="nextstepaction"]
> [Mobilite Hizmeti API belgeleri](https://aka.ms/AzureMapsMobilityService)
