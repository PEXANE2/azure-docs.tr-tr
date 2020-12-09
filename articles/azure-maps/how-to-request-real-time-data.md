---
title: Microsoft Azure Maps Mobility Services ile gerçek zamanlı genel aktarım verileri isteme (Önizleme)
description: Bir geçiş durgde gibi gerçek zamanlı genel aktarım verileri isteme hakkında bilgi edinin. Bu amaçla Azure Maps Mobility hizmetlerini (Önizleme) nasıl kullanacağınızı öğrenin.
author: anastasia-ms
ms.author: v-stharr
ms.date: 12/07/2020
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: d3e3dc4b0e3bc64a38856da8344583b744ea62b6
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96906055"
---
# <a name="request-real-time-public-transit-data-using-the-azure-maps-mobility-services-preview"></a>Azure haritalar Mobility hizmetlerini (Önizleme) kullanarak gerçek zamanlı genel aktarım verileri isteyin 

> [!IMPORTANT]
> Azure haritalar Mobility Hizmetleri şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Bu makalede, Azure Maps [Mobility hizmetlerinin](/rest/api/maps/mobility) gerçek zamanlı genel aktarım verileri istemek için nasıl kullanılacağı gösterilmektedir.

Bu makalede, belirli bir dura ulaşan tüm satırlar için sonraki gerçek zamanlı varışları isteme hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Azure Maps ortak Aktarma API 'Lerine herhangi bir çağrı yapmak için önce bir Azure Maps hesabına ve bir abonelik anahtarına sahip olmanız gerekir. Daha fazla bilgi için, [Hesap oluşturma](quick-demo-map-app.md#create-an-azure-maps-account) ' daki yönergeleri Izleyerek Azure Maps hesabı oluşturun. Hesabınız için birincil anahtarı almak üzere [birincil anahtar al](quick-demo-map-app.md#get-the-primary-key-for-your-account) bölümündeki adımları izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).

Bu makale, REST çağrıları oluşturmak için [Postman uygulamasını](https://www.getpostman.com/apps) kullanır. Tercih ettiğiniz herhangi bir API geliştirme ortamını kullanabilirsiniz.

## <a name="request-real-time-arrivals-for-a-stop"></a>Durdurma için gerçek zamanlı varış süresi iste

Belirli bir genel yoldaki gerçek zamanlı varış verileri istemek için, Azure Maps [Mobility hizmeti 'nin (Önizleme)](/rest/api/maps/mobility) [gerçek zamanlı varış API](/rest/api/maps/mobility/getrealtimearrivalspreview) 'sine istek yapmanız gerekir. İsteği tamamlayabilmeniz için **metroID** ve **stopid** gerekir. Bu parametreleri isteme hakkında daha fazla bilgi edinmek için bkz. [genel geçiş rotaları isteme](./how-to-request-transit-data.md)Kılavuzu.

"Seattle – Tacoma – Bellevue, WA" alanı için Metro kimliği olan "522" öğesini Metro KIMLIĞINIZLE kullanalım. Durma KIMLIĞI olarak "522---2060603" kullanın, bu veri yolu durağı "ne 24 th St & 162. Ave, Bellevue WA" olur. Sonraki beş gerçek zamanlı varış verilerini istemek için, bu durdurmakta olan tüm sonraki canlı varış için aşağıdaki adımları izleyin:

1. Postman uygulamasını açın ve istekleri depolamak için bir koleksiyon oluşturalım. Postman uygulamasının üst kısmında **Yeni**' yi seçin. **Yeni oluştur** penceresinde **koleksiyon**' ı seçin.  Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin.

2. İsteği oluşturmak için **Yeni** ' yi seçin. **Yeni oluştur** penceresinde **istek**' ı seçin. İstek için bir **istek adı** girin. Önceki adımda oluşturduğunuz koleksiyonu, isteğin kaydedileceği konum olarak seçin. Sonra **Kaydet**' i seçin.

    ![Postman 'da istek oluşturma](./media/how-to-request-transit-data/postman-new.png)

3. Oluşturucu sekmesinde http **Al** metodunu seçin ve bir get isteği oluşturmak için aşağıdaki URL 'yi girin. `{subscription-key}`Azure haritalar birincil anahtarınızla değiştirin.

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

Mobility hizmetlerini (Önizleme) kullanarak geçiş verileri isteme hakkında bilgi edinin:

> [!div class="nextstepaction"]
> [Geçiş verileri isteme](how-to-request-transit-data.md)

Azure haritalar Mobility Hizmetleri (Önizleme) API belgelerini inceleyin:

> [!div class="nextstepaction"]
> [Mobility Hizmetleri API 'SI belgeleri](/rest/api/maps/mobility)