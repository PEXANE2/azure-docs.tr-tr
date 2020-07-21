---
title: Bir ortama olay gönderme-Azure Time Series Insights | Microsoft Docs
description: Bir olay hub 'ını yapılandırmayı, örnek bir uygulamayı çalıştırmayı ve Azure Time Series Insights ortamınıza olay göndermenizi öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/30/2020
ms.custom: seodec18
ms.openlocfilehash: 589dd411e3d340eb8a0bf84b21a306cabd4bb362
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86495083"
---
# <a name="send-events-to-a-azure-time-series-insights-gen1-environment-by-using-an-event-hub"></a>Olay Hub 'ı kullanarak Azure Time Series Insights Gen1 ortamına olay gönderme

Bu makalede, Azure Event Hubs 'da bir olay hub 'ı oluşturma ve yapılandırma açıklanmaktadır. Ayrıca, olayları Event Hubs Azure Time Series Insights göndermek için örnek bir uygulamanın nasıl çalıştırılacağını açıklar. JSON biçiminde olaylar içeren bir olay hub 'ınız varsa, bu öğreticiyi atlayıp [Azure Time Series Insights](./time-series-insights-update-create-environment.md)ortamınızı görüntüleyin.

## <a name="configure-an-event-hub"></a>Olay hub’ını yapılandırma

1. Bir olay hub 'ı oluşturmayı öğrenmek için [Event Hubs belgelerini](https://docs.microsoft.com/azure/event-hubs/)okuyun.
1. Arama kutusunda **Event Hubs**aratın. Döndürülen listede **Event Hubs**' yi seçin.
1. Olay Hub 'ınızı seçin.
1. Bir olay hub 'ı oluşturduğunuzda, bir olay hub 'ı ad alanı oluşturuyorsunuz. Ad alanı içinde henüz bir olay hub 'ı oluşturmadıysanız, menüde, **varlıklar**altında bir olay hub 'ı oluşturun.  

    [![Olay Hub 'ları listesi](media/send-events/tsi-connect-event-hub-namespace.png)](media/send-events/tsi-connect-event-hub-namespace.png#lightbox)

1. Bir olay hub 'ı oluşturduktan sonra, Olay Hub 'ları listesinden bunu seçin.
1. Menüsünde, **varlıklar**altında **Event Hubs**' yi seçin.
1. Yapılandırılacak Olay Hub 'ının adını seçin.
1. **Genel bakış**altında **tüketici grupları**' nı ve ardından **Tüketici grubu**' nu seçin.

    [![Tüketici grubu oluşturma](media/send-events/add-event-hub-consumer-group.png)](media/send-events/add-event-hub-consumer-group.png#lightbox)

1. Yalnızca Azure Time Series Insights olay kaynağınız tarafından kullanılan bir tüketici grubu oluşturduğunuzdan emin olun.

    > [!IMPORTANT]
    > Bu tüketici grubunun Azure Stream Analytics iş veya başka bir Azure Time Series Insights ortamı gibi başka bir hizmet tarafından kullanılmadığından emin olun. Tüketici grubu başka hizmetler tarafından kullanılıyorsa, bu ortam ve diğer hizmetler için okuma işlemleri olumsuz etkilenir. Tüketici grubu olarak **$Default** kullanıyorsanız, diğer okuyucular tüketici grubunuzu kullanabilir.

1. Menüde, **Ayarlar**' ın altında, **paylaşılan erişim ilkeleri**' ni seçin ve ardından **Ekle**' yi seçin.

    [![Paylaşılan erişim ilkeleri ' ni seçin ve ardından Ekle düğmesini seçin.](media/send-events/add-shared-access-policy.png)](media/send-events/add-shared-access-policy.png#lightbox)

1. **Yeni paylaşılan erişim Ilkesi Ekle** bölmesinde, **mysendpolicy**adlı bir paylaşılan erişim oluşturun. Bu paylaşılan erişim ilkesini, bu makalenin ilerleyen kısımlarında bulunan C# örneklerine olay göndermek için kullanırsınız.

    [![Ilke adı kutusuna MySendPolicy yazın.](media/send-events/configure-shared-access-policy-confirm.png)](media/send-events/configure-shared-access-policy-confirm.png#lightbox)

1. **Talep**altında **Gönder** onay kutusunu seçin.

## <a name="add-an-azure-time-series-insights-instance"></a>Azure Time Series Insights örneği ekleme

Azure Time Series Insights Gen 2 ' de, zaman serisi modelini (TSM) kullanarak gelen telemetrisine bağlamsal veriler ekleyebilirsiniz. TSD 'de, etiketlerinizin veya sinyallerinizin *örnekleri* olarak başvurduğu ve bağlamsal verileri *örnek alanlarında* depolayabilmeniz gerekir. Veriler, **zaman SERISI kimliği**kullanılarak sorgu zamanına birleştirilir. Bu makalede daha sonra kullandığımız örnek **wınıdmills projesi Için zaman serisi kimliği** `id` . Örnek alanlarında veri depolama hakkında daha fazla bilgi edinmek için [zaman serisi modeline](./concepts-model-overview.md) genel bakış makalesini okuyun.

### <a name="create-a-azure-time-series-insights-event-source"></a>Azure Time Series Insights olay kaynağı oluşturma

1. Bir olay kaynağı oluşturmadıysanız, [bir olay kaynağı oluşturma](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub)adımlarını izleyin.

1. İçin bir değer ayarlayın `timeSeriesId` . **Zaman SERISI kimliği**hakkında daha fazla bilgi edinmek Için [zaman serisi modellerini](./concepts-model-overview.md)okuyun.

### <a name="push-events-to-windmills-sample"></a>Olayları, wınte milfrels örneğine gönder

1. Arama çubuğunda **Event Hubs**aratın. Döndürülen listede **Event Hubs**' yi seçin.

1. Olay Hub örneğinizi seçin.

1. **Paylaşılan erişim ilkeleri**  >  **mysendpolicy**bölümüne gidin. **Bağlantı dizesi-birincil anahtar**değerini kopyalayın.

    [![Birincil anahtar bağlantı dizesinin değerini kopyalayın](media/send-events/configure-sample-code-connection-string.png)](media/send-events/configure-sample-code-connection-string.png#lightbox)

1. https://tsiclientsample.azurewebsites.net/windFarmGen.html öğesine gidin. URL, benzetimli wındmill cihazları oluşturur ve çalıştırır.
1. Web sayfasındaki **Olay Hub 'ı bağlantı dizesi** kutusunda, [wındmill giriş alanına](#push-events-to-windmills-sample)kopyaladığınız bağlantı dizesini yapıştırın.
  
    [![Birincil anahtar bağlantı dizesini Olay Hub 'ı bağlantı dizesi kutusuna yapıştırın](media/send-events/configure-wind-mill-sim.png)](media/send-events/configure-wind-mill-sim.png#lightbox)

1. **Başlamak Için tıklayın ' ı**seçin. 

    > [!TIP]
    > Windmill simülatörü Ayrıca [Azure TIME SERIES INSIGHTS GA sorgu API 'leri](https://docs.microsoft.com/rest/api/time-series-insights/ga-query)ile yük olarak kullanabileceğiniz JSON oluşturur.

    > [!NOTE]
    > Tarayıcı sekmesi kapatılana kadar simülatör verileri gönderilmeye devam edecektir.

1. Azure portal olay hub 'ınıza geri dönün. **Genel bakış** sayfasında, Olay Hub 'ı tarafından alınan yeni olaylar görüntülenir.

    [![Olay Hub 'ı için ölçümleri gösteren bir olay hub 'ı genel bakış sayfası](media/send-events/review-windmill-telemetry.png)](media/send-events/review-windmill-telemetry.png#lightbox)

## <a name="supported-json-shapes"></a>Desteklenen JSON şekilleri

### <a name="example-one"></a>Örnek bir

* **Giriş**: basıt bir JSON nesnesi.

    ```JSON
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    }
    ```

* **Çıkış**: bir olay.

    |kimlik|timestamp|
    |--------|---------------|
    |cihaz1|2016-01-08T01:08:00Z|

### <a name="example-two"></a>Örnek iki

* **Giriş**: iki JSON nesnesi olan bir JSON dizisi. Her JSON nesnesi bir olaya dönüştürülür.

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

* **Çıkış**: iki olay.

    |kimlik|timestamp|
    |--------|---------------|
    |cihaz1|2016-01-08T01:08:00Z|
    |cihaz2|2016-01-08T01:17:00Z|

### <a name="example-three"></a>Örnek üç

* **Giriş**: iki JSON nesnesi içeren Iç içe JSON dizisine sahıp bir JSON nesnesi.

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

* **Çıkış**: iki olay. Özellik **konumu** her olaya kopyalanır.

    |location|events.id|events.timestamp|
    |--------|---------------|----------------------|
    |WestUs|cihaz1|2016-01-08T01:08:00Z|
    |WestUs|cihaz2|2016-01-08T01:17:00Z|

### <a name="example-four"></a>Örnek dört

* **Giriş**: iki JSON nesnesi içeren Iç içe JSON dizisine sahıp bir JSON nesnesi. Bu giriş, genel özelliklerin karmaşık JSON nesnesiyle temsil edilebilir olduğunu gösterir.

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

* **Çıkış**: iki olay.

    |location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
    |---|---|---|---|---|---|---|---|
    |WestUs|üretici1|EastUs|cihaz1|2016-01-08T01:08:00Z|basınç|psi|108.09|
    |WestUs|üretici1|EastUs|cihaz2|2016-01-08T01:17:00Z|titreşim|abs G|217.09|

## <a name="next-steps"></a>Sonraki adımlar

- Azure Time Series Insights Gezgini ' nde [ortamınızı görüntüleyin](https://insights.timeseries.azure.com) .

- [IoT Hub cihaz iletileri](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-construct) hakkında daha fazla bilgi edinin
