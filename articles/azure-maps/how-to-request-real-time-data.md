---
title: Gerçek zamanlı genel geçiş verileri iste | Microsoft Azure haritaları
description: Microsoft Azure haritaları Mobility hizmetini kullanarak gerçek zamanlı genel aktarım verileri isteyin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 4743fbe84f5d41b4659e13d96868d2f64a473e4b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82086086"
---
# <a name="request-real-time-public-transit-data-using-the-azure-maps-mobility-service"></a>Azure haritalar Mobility hizmetini kullanarak gerçek zamanlı genel aktarım verileri isteyin

Bu makalede, Azure Maps [Mobility hizmetini](https://aka.ms/AzureMapsMobilityService) kullanarak gerçek zamanlı genel aktarım verileri isteme hakkında yönergeler verilmektedir.

Bu makalede, belirli bir dura ulaşan tüm satırlar için sonraki gerçek zamanlı varışları isteme hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure Maps ortak Aktarma API 'Lerine herhangi bir çağrı yapmak için önce bir Azure Maps hesabına ve bir abonelik anahtarına sahip olmanız gerekir. Daha fazla bilgi için, [Hesap oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps) ' daki yönergeleri Izleyerek Azure Maps hesabı oluşturun. Hesabınız için birincil anahtarı almak üzere [birincil anahtar al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).

Bu makale, REST çağrıları oluşturmak için [Postman uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.

## <a name="request-real-time-arrivals-for-a-stop"></a>Durdurma için gerçek zamanlı varış süresi iste

Belirli bir genel yoldaki gerçek zamanlı varış verileri istemek için, Azure Maps [Mobility hizmetinin](https://aka.ms/AzureMapsMobilityService) [gerçek zamanlı varış API](https://aka.ms/AzureMapsMobilityRealTimeArrivals) 'sine istek yapmanız gerekir. İsteği tamamlayabilmeniz için **metroID** ve **stopid** gerekir. Bu parametreleri isteme hakkında daha fazla bilgi edinmek için bkz. [genel geçiş rotaları isteme](https://aka.ms/AMapsHowToGuidePublicTransitRouting)Kılavuzu.

"Seattle – Tacoma – Bellevue, WA" alanı için Metro kimliği olan "522" öğesini Metro KIMLIĞINIZLE kullanalım. Durma KIMLIĞI olarak "522---2060603" kullanın, bu veri yolu durağı "ne 24 th St & 162. Ave, Bellevue WA" olur. Sonraki beş gerçek zamanlı varış verilerini istemek için, bu durdurmakta olan tüm sonraki canlı varış için aşağıdaki adımları izleyin:

1. Postman uygulamasını açın ve istekleri depolamak için bir koleksiyon oluşturalım. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **koleksiyon**' ı seçin.  Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin.

2. İsteği oluşturmak için **Yeni** ' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin. İstek için bir **istek adı** girin. Önceki adımda oluşturduğunuz koleksiyonu, isteğin kaydedileceği konum olarak seçin. Ardından **Kaydet**’i seçin.

    ![Postman 'da istek oluşturma](./media/how-to-request-transit-data/postman-new.png)

3. Oluşturucu sekmesinde http **Al** metodunu seçin ve bir get isteği oluşturmak için aşağıdaki URL 'yi girin. Azure `{subscription-key}`haritalar birincil anahtarınızla değiştirin.

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

## <a name="next-steps"></a>Sonraki adımlar

Mobility hizmetini kullanarak geçiş verileri isteme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Geçiş verileri isteme](how-to-request-transit-data.md)

Azure Maps Mobility hizmeti API 'SI belgelerini inceleyin:

> [!div class="nextstepaction"]
> [Mobility hizmeti API 'SI belgeleri](https://aka.ms/AzureMapsMobilityService)
