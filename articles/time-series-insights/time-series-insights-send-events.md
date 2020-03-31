---
title: Etkinlikleri ortama gönderme - Azure Time Series Öngörüleri | Microsoft Dokümanlar
description: Bir etkinlik merkezini nasıl yapılandıracak, örnek bir uygulama çalıştırın ve etkinlikleri Azure Zaman Serisi Öngörüleri ortamınıza nasıl göndereceğinizi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/11/2020
ms.custom: seodec18
ms.openlocfilehash: c3c7f59ecb3a06d80012917e2da4425a899859d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254254"
---
# <a name="send-events-to-a-time-series-insights-environment-by-using-an-event-hub"></a>Etkinlik hub'ı kullanarak olayları Time Series Insights ortamına gönderme

Bu makalede, Azure Etkinlik Hub'larında bir olay hub'ı nasıl oluşturulup yapılandırılabilenler açıklanmaktadır. Ayrıca, etkinlikleri Etkinlik Hub'larından Azure Time Series Öngörülerine itmek için örnek bir uygulamanın nasıl çalıştırılabildiğini de açıklar. JSON formatında etkinlikleriçeren varolan bir etkinlik hub'ınız varsa, bu öğreticiyi atlayın ve [Azure Zaman Serisi Öngörüleri'nde](./time-series-insights-update-create-environment.md)ortamınızı görüntüleyin.

## <a name="configure-an-event-hub"></a>Olay hub’ını yapılandırma

1. Olay hub'ı oluşturmayı öğrenmek için [Olay Hub'ları belgelerini](https://docs.microsoft.com/azure/event-hubs/)okuyun.
1. Arama kutusunda, Olay **Hub'larını**arayın. Döndürülen listede **Olay Hub'ları'nı**seçin.
1. Etkinlik merkezinizi seçin.
1. Bir olay hub'ı oluşturduğunuzda, bir olay merkezi ad alanı oluşturursunuz. Ad alanı içinde henüz bir olay hub'ı oluşturmadıysanız, menüde, **Varlıklar**altında bir olay hub'ı oluşturun.  

    [![Etkinlik merkezleri listesi](media/send-events/tsi-connect-event-hub-namespace.png)](media/send-events/tsi-connect-event-hub-namespace.png#lightbox)

1. Bir olay hub'ı oluşturduktan sonra, olay hub'ları listesinde seçin.
1. Menüde, **Varlıklar**altında **Olay Hub'ları'nı**seçin.
1. Yapılandırmak için olay merkezinin adını seçin.
1. **Genel Bakış**altında, **Tüketici gruplarını**seçin ve ardından Tüketici **Grubu'nun**seçin.

    [![Tüketici grubu oluşturma](media/send-events/add-event-hub-consumer-group.png)](media/send-events/add-event-hub-consumer-group.png#lightbox)

1. Yalnızca Time Series Insights etkinlik kaynağınız tarafından kullanılan bir tüketici grubu oluşturduğunuzdan emin olun.

    > [!IMPORTANT]
    > Bu tüketici grubunun Azure Akışı Analizi işi veya başka bir Zaman Serisi Öngörüleri ortamı gibi başka bir hizmet tarafından kullanılmadığından emin olun. Tüketici grubu diğer hizmetler tarafından kullanılıyorsa, okuma işlemleri hem bu ortam hem de diğer hizmetler için olumsuz etkilenir. **$Default** tüketici grubu olarak kullanıyorsanız, diğer okuyucular tüketici grubunuzu yeniden kullanabilir.

1. Menüde Ayarlar **altında** **Paylaşılan erişim ilkelerini**seçin ve ardından **Ekle'yi**seçin.

    [![Paylaşılan erişim ilkelerini seçin ve sonra Ekle düğmesini seçin](media/send-events/add-shared-access-policy.png)](media/send-events/add-shared-access-policy.png#lightbox)

1. Yeni **paylaşılan erişim ilkesi ekle** bölmesinde **MySendPolicy**adında paylaşılan bir erişim oluşturun. Bu makalenin ilerleyen saatlerinde C# örneklerinde olay göndermek için bu paylaşılan erişim ilkesini kullanırsınız.

    [![İlke ad kutusuna MySendPolicy'yi girin](media/send-events/configure-shared-access-policy-confirm.png)](media/send-events/configure-shared-access-policy-confirm.png#lightbox)

1. **Talep**altında, **Gönder** onay kutusunu seçin.

## <a name="add-a-time-series-insights-instance"></a>Zaman Serisi Öngörüleri örneği ekleme

Zaman Serisi Öngörüleri güncelleştirmesi, gelen telemetri verilerine bağlamsal veri eklemek için örnekleri kullanır. Veriler, **zaman serisi kimliği**kullanılarak sorgu sırasında birleştirilir. Bu makalede daha sonra kullandığımız örnek yel değirmenleri projesi `id`için Zaman Serisi **Kimliği.** Time Series Insight örnekleri ve **Time Series ID**hakkında daha fazla bilgi edinmek için [Time Series Modellerini](./time-series-insights-update-tsm.md)okuyun.

### <a name="create-a-time-series-insights-event-source"></a>Zaman Serisi Öngörüleri etkinlik kaynağı oluşturma

1. Bir olay kaynağı oluşturmadıysanız, [olay kaynağı oluşturmak](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub)için adımları tamamlayın.

1. '' için `timeSeriesId`bir değer ayarlama Time Series **ID**hakkında daha fazla bilgi edinmek için [Time Series Modelleri'ni](./time-series-insights-update-tsm.md)okuyun.

### <a name="push-events-to-windmills-sample"></a>Olayları yel değirmenleri örneğine itme

1. Arama çubuğunda Olay **Hub'larını**arayın. Döndürülen listede **Olay Hub'ları'nı**seçin.

1. Olay merkezi örneğini seçin.

1. Paylaşılan **Erişim İlkeleri** > **MySendPolicy'ye**gidin. **Bağlantı dizesi birincil anahtarının**değerini kopyalayın.

    [![Birincil anahtar bağlantı dizesi değerini kopyalama](media/send-events/configure-sample-code-connection-string.png)](media/send-events/configure-sample-code-connection-string.png#lightbox)

1. https://tsiclientsample.azurewebsites.net/windFarmGen.html kısmına gidin. URL, simüle edilmiş yel değirmeni aygıtları oluşturur ve çalıştırAr.
1. Web sayfasındaki **Event Hub Bağlantı String** kutusuna, yel [değirmeni giriş alanında](#push-events-to-windmills-sample)kopyaladığınız bağlantı dizesini yapıştırın.
  
    [![Olay Hub Bağlantısı String kutusuna birincil anahtar bağlantı dizesini yapıştırın](media/send-events/configure-wind-mill-sim.png)](media/send-events/configure-wind-mill-sim.png#lightbox)

1. **Başlamak için Tıklayın'ı**seçin. 

    > [!TIP]
    > Yel değirmeni simülatörü de [Zaman Serisi Insights GA Sorgu API'leri](https://docs.microsoft.com/rest/api/time-series-insights/ga-query)ile bir yük olarak kullanabileceğiniz JSON oluşturur.

    > [!NOTE]
    > Simülatör, tarayıcı sekmesi kapatılana kadar veri göndermeye devam edecektir.

1. Azure portalındaki etkinlik merkezinize geri dön. Genel **Bakış** sayfasında, olay merkezi tarafından alınan yeni olaylar görüntülenir.

    [![Olay merkezinin ölçümlerini gösteren bir olay merkezi Genel Bakış sayfası](media/send-events/review-windmill-telemetry.png)](media/send-events/review-windmill-telemetry.png#lightbox)

## <a name="supported-json-shapes"></a>Desteklenen JSON şekilleri

### <a name="example-one"></a>Örnek bir

* **Giriş**: Basit bir JSON nesnesi.

    ```JSON
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    }
    ```

* **Çıktı**: Bir olay.

    |id|timestamp|
    |--------|---------------|
    |cihaz1|2016-01-08T01:08:00Z|

### <a name="example-two"></a>Örnek iki

* **Giriş**: İki JSON nesnesi olan bir JSON dizisi. Her JSON nesnesi bir olaya dönüştürülür.

    ```JSON
    [
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
    ```

* **Çıktı**: İki olay.

    |id|timestamp|
    |--------|---------------|
    |cihaz1|2016-01-08T01:08:00Z|
    |cihaz2|2016-01-08T01:17:00Z|

### <a name="example-three"></a>Örnek üç

* **Giriş**: İki JSON nesnesi içeren iç içe json dizili bir JSON nesnesi.

    ```JSON
    {
        "location":"WestUs",
        "events":[
            {
                "id":"device1",
                "timestamp":"2016-01-08T01:08:00Z"
            },
            {
                "id":"device2",
                "timestamp":"2016-01-17T01:17:00Z"
            }
        ]
    }
    ```

* **Çıktı**: İki olay. Özellik **konumu** her olay için kopyalanır.

    |location|events.id|events.timestamp|
    |--------|---------------|----------------------|
    |WestUs|cihaz1|2016-01-08T01:08:00Z|
    |WestUs|cihaz2|2016-01-08T01:17:00Z|

### <a name="example-four"></a>Örnek dört

* **Giriş**: İki JSON nesnesi içeren iç içe json dizili bir JSON nesnesi. Bu giriş, genel özelliklerin karmaşık JSON nesnesi tarafından temsil edilebilmiş olabileceğini gösterir.

    ```JSON
    {
        "location":"WestUs",
        "manufacturer":{
            "name":"manufacturer1",
            "location":"EastUs"
        },
        "events":[
            {
                "id":"device1",
                "timestamp":"2016-01-08T01:08:00Z",
                "data":{
                    "type":"pressure",
                    "units":"psi",
                    "value":108.09
                }
            },
            {
                "id":"device2",
                "timestamp":"2016-01-17T01:17:00Z",
                "data":{
                    "type":"vibration",
                    "units":"abs G",
                    "value":217.09
                }
            }
        ]
    }
    ```

* **Çıktı**: İki olay.

    |location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
    |---|---|---|---|---|---|---|---|
    |WestUs|üretici1|EastUs|cihaz1|2016-01-08T01:08:00Z|basınç|psi|108.09|
    |WestUs|üretici1|EastUs|cihaz2|2016-01-08T01:17:00Z|titreşim|abs G|217.09|

## <a name="next-steps"></a>Sonraki adımlar

- Zaman Serisi Öngörüler kaşifinde [ortamınızı görüntüleyin.](https://insights.timeseries.azure.com)

- [IoT Hub aygıt iletileri](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct) hakkında daha fazla bilgi edinin
