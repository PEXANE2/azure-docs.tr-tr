---
title: Gerçek zamanlı transit verileri isteme | Microsoft Azure haritaları
description: Microsoft Azure Maps Mobility hizmetini kullanarak gerçek zamanlı veriler isteyin.
author: walsehgal
ms.author: v-musehg
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 053e6c84f69e8b3d3fed0a90a8b632aa4eb311cb
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198165"
---
# <a name="request-real-time-data-using-the-azure-maps-mobility-service"></a>Azure haritalar Mobility hizmetini kullanarak gerçek zamanlı veriler isteme

Bu makalede, gerçek zamanlı transit verileri istemek için Azure Maps [Mobility hizmetini](https://aka.ms/AzureMapsMobilityService) nasıl kullanacağınız gösterilmektedir.

Bu makalede şunları yapmayı öğreneceksiniz:


 * Belirli bir dura ulaşan tüm satırlar için sonraki gerçek zamanlı varışları iste
 * Belirli bir bisiklet yerleştirme istasyonu için gerçek zamanlı bilgi isteyin.


## <a name="prerequisites"></a>Önkoşullar

Azure Maps ortak Aktarma API 'Lerine herhangi bir çağrı yapmak için önce bir Azure Maps hesabına ve bir abonelik anahtarına sahip olmanız gerekir. Daha fazla bilgi için, [Hesap oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps) ' daki yönergeleri Izleyerek Azure Maps hesabı oluşturun. Hesabınız için birincil anahtarı almak üzere [birincil anahtar al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).


Bu makale, REST çağrıları oluşturmak için [Postman uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.


## <a name="request-real-time-arrivals-for-a-stop"></a>Durdurma için gerçek zamanlı varış süresi iste

Belirli bir genel yoldaki gerçek zamanlı varış verileri istemek için, Azure Maps [Mobility hizmetinin](https://aka.ms/AzureMapsMobilityService) [gerçek zamanlı varış API](https://aka.ms/AzureMapsMobilityRealTimeArrivals) 'sine istek yapmanız gerekir. İsteği tamamlayabilmeniz için **metroID** ve **stopid** gerekir. Bu parametreleri isteme hakkında daha fazla bilgi edinmek için bkz. [genel geçiş rotaları isteme](https://aka.ms/AMapsHowToGuidePublicTransitRouting)Kılavuzu. 

"Seattle – Tacoma – Bellevue, WA" alanı için Metro kimliği olan "522" öğesini Metro KIMLIĞINIZLE kullanalım. Durma KIMLIĞI olarak "522---2060603" kullanın, bu veri yolu durağı "ne 24 th St & 162. Ave, Bellevue WA" olur. Sonraki beş gerçek zamanlı varış verilerini istemek için, bu durdurmakta olan tüm sonraki canlı varış için aşağıdaki adımları izleyin:

1. Postman uygulamasını açın ve istekleri depolamak için bir koleksiyon oluşturalım. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **koleksiyon**' ı seçin.  Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin.

2. İsteği oluşturmak için **Yeni** ' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin. İstek için bir **istek adı** girin. Önceki adımda oluşturduğunuz koleksiyonu, isteğin kaydedileceği konum olarak seçin. Ardından **Kaydet**’i seçin.

    ![Postman 'da istek oluşturma](./media/how-to-request-transit-data/postman-new.png)

3. Oluşturucu sekmesinde http **Al** metodunu seçin ve bir get isteği oluşturmak için aşağıdaki URL 'yi girin. `{subscription-key}`, Azure Maps birincil anahtarınızla değiştirin.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

4. Başarılı bir istekten sonra aşağıdaki yanıtı alırsınız.  ' ScheduleType ' parametresinin tahmini varış zamanının gerçek zamanlı veya statik verilere dayanıp dayandırmadığını tanımladığından emin olun.

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

[Get transit Dock Info API 'si](https://aka.ms/AzureMapsMobilityTransitDock) , kullanıcıların statik ve gerçek zamanlı bilgiler istemesine izin verir. Örneğin, kullanıcılar bir bisiklet veya bir ayrıntılı yerleştirme istasyonu için kullanılabilirlik ve açık pozisyon bilgileri isteyebilir. [Get transit Dock Info API 'si](https://aka.ms/AzureMapsMobilityTransitDock) de Azure Maps [Mobility hizmeti](https://aka.ms/AzureMapsMobilityService)'nin bir parçasıdır.

[Get aktarma Dock ıNFO API](https://aka.ms/AzureMapsMobilityTransitDock)'sine bir istek yapmak için bu Istasyonda **dockıd** gerekir. "BikeDock" öğesine atanmış **ObjectType** parametresi Ile [YAKıNDAKI transit alma API](https://aka.ms/AzureMapsMobilityNearbyTransit) 'sine bir arama isteği yaparak yuva kimliğini alabilirsiniz. Bisiklet için bir yerleşik istasyonın gerçek zamanlı verilerini almak için aşağıdaki adımları izleyin.


### <a name="get-dock-id"></a>Dock ID Al

**Dockıd**'yi almak için aşağıdaki adımları izleyerek yakında bulunan transit API 'sine yönelik bir istek oluşturun:

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **yuva kimliğini alın**.

2.  Oluşturucu sekmesinde, http **Al** metodunu seçin, AŞAĞıDAKI istek URL 'sini girin ve **Gönder**' e tıklayın.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/nearby/json?subscription-key={subscription-key}&api-version=1.0&metroId=121&query=40.7663753,-73.9627498&radius=100&objectType=bikeDock
    ```

3. Başarılı bir istekten sonra aşağıdaki yanıtı alırsınız. Artık yanıttaki kimliğe sahip olduğumuz, daha sonra aktarım noktası bilgi API 'sine yönelik istekte bir sorgu parametresi olarak kullanılabilecek olan **kimliği** görürsünüz.

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


### <a name="get-real-time-bike-dock-status"></a>Gerçek zamanlı Bisiklet dock durumunu al

Seçilen yerleştirme için gerçek zamanlı verileri almak üzere aktarma yerleştirme noktası bilgi API 'sine bir istek yapmak için aşağıdaki adımları izleyin.

1. Postman 'da, **Isteği al** | **yeni istek** ' a tıklayın ve **gerçek zamanlı yerleştirme verilerini alın**.

2.  Oluşturucu sekmesinde, http **Al** metodunu seçin, AŞAĞıDAKI istek URL 'sini girin ve **Gönder**' e tıklayın.
 
    ```HTTP
    https://atlas.microsoft.com/mobility/transit/dock/json?subscription-key={subscription-key}&api-version=1.0&query=121---4640799
    ```

3. Başarılı bir istekten sonra, aşağıdaki yapıya sahip bir yanıt alacaksınız:

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

Mobility hizmetini kullanarak geçiş verileri isteme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Geçiş verileri isteme](how-to-request-transit-data.md)

Azure Maps Mobility hizmeti API 'SI belgelerini inceleyin:

> [!div class="nextstepaction"]
> [Mobility hizmeti API 'SI belgeleri](https://aka.ms/AzureMapsMobilityService)
