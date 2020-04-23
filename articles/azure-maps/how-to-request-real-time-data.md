---
title: Gerçek zamanlı toplu taşıma verileri isteyin | Microsoft Azure Haritaları
description: Microsoft Azure Haritalar Mobilite Hizmeti'ni kullanarak gerçek zamanlı toplu taşıma verileri isteyin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 09/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 4743fbe84f5d41b4659e13d96868d2f64a473e4b
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086086"
---
# <a name="request-real-time-public-transit-data-using-the-azure-maps-mobility-service"></a>Azure Haritalar Mobilite Hizmeti'ni kullanarak gerçek zamanlı toplu taşıma verileri isteyin

Bu makalede, gerçek zamanlı toplu taşıma verileri istemek için Azure Haritalar [Mobilite Hizmeti'ni](https://aka.ms/AzureMapsMobilityService) nasıl kullanacağınızı gösterilmektedir.

Bu makalede, belirli bir durakta gelen tüm hatlar için bir sonraki gerçek zamanlı varışları nasıl isteyeceğinizi öğreneceksiniz

## <a name="prerequisites"></a>Önkoşullar

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

## <a name="next-steps"></a>Sonraki adımlar

Mobilite Hizmetini kullanarak transit verilerini nasıl isteyeceğizi öğrenin:

> [!div class="nextstepaction"]
> [Aktarım verileri nasıl istenir?](how-to-request-transit-data.md)

Azure Haritalar Mobilite Hizmeti API belgelerini keşfedin:

> [!div class="nextstepaction"]
> [Mobilite Hizmeti API belgeleri](https://aka.ms/AzureMapsMobilityService)
