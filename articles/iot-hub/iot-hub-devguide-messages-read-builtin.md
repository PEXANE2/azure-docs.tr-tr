---
title: Azure IoT Hub'ını yerleşik bitiş noktasını anlayın | Microsoft Dokümanlar
description: Geliştirici kılavuzu - aygıttan buluta iletileri okumak için yerleşik, Event Hub uyumlu bitiş noktasının nasıl kullanılacağını açıklar.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2019
ms.openlocfilehash: e7b8f8a33b741a8dcf2d1a68ae3cf86d6e3687eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284609"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Cihazdan buluta iletilerini yerleşik uç noktadan okuma

Varsayılan olarak, iletiler [Olay Hub'larıyla](https://azure.microsoft.com/documentation/services/event-hubs/)uyumlu yerleşik hizmete bakan bitiş noktasına **(iletiler/olaylar)** yönlendirilir. Bu bitiş noktası şu anda yalnızca bağlantı noktası 5671'deki [AMQP](https://www.amqp.org/) protokolü kullanılarak açıktadır. Bir IoT hub'ı, yerleşik Event Hub uyumlu ileti uç nokta **iletilerini/olaylarını**denetlemenizi sağlamak için aşağıdaki özellikleri ortaya çıkarır.

| Özellik            | Açıklama |
| ------------------- | ----------- |
| **Bölüm sayısı** | Aygıttan buluta olay veya alma için [bölüm](../event-hubs/event-hubs-features.md#partitions) sayısını tanımlamak için bu özelliği oluşturma da ayarlayın. |
| **Bekletme süresi**  | Bu özellik, iletilerin IoT Hub tarafından günler içinde ne kadar süreyle tutulduğunu belirtir. Varsayılan değer bir gündür, ancak yedi güne çıkarılabilir. |

IoT Hub, yerleşik Olay Hub'larında en fazla 7 gün süreyle veri saklamaya izin verir. IoT Hub'ınızın oluşturulması sırasında bekletme süresini ayarlayabilirsiniz. IoT Hub'daki veri saklama süresi, IoT hub katmanınıza ve birim türünüze bağlıdır. Yerleşik Olay Hub'ları, boyut açısından en az 24 saate kadar en fazla ileti boyutundaki iletileri saklayabilir. Örneğin, 1 S1 birimi ioT Hub için her biri 4k boyutunda en az 400K iletileri tutmak için yeterli depolama alanı sağlar. Aygıtlarınız daha küçük iletiler gönderiyorsa, ne kadar depolama alanı tüketildiklerine bağlı olarak daha uzun süre (7 güne kadar) saklanabilir. Belirtilen bekletme süresi için verilerin minimum olarak saklanması garanti ediyoruz.

IoT Hub, yerleşik aygıt-bulut alma bitiş noktasında ki tüketici gruplarını yönetmenize de olanak tanır. Her IoT Hub'ı için en fazla 20 tüketici grubuna sahip olabilirsiniz.

[İleti yönlendirme](iot-hub-devguide-messages-d2c.md) kullanıyorsanız ve geri [dönüş rotası](iot-hub-devguide-messages-d2c.md#fallback-route) etkinse, herhangi bir rotadaki sorguyla eşleşmeyan tüm iletiler yerleşik bitiş noktasına gider. Bu geri dönüş rotasını devre dışı bırakırsan, sorguyla eşleşmeyan iletiler bırakılır.

[IoT Hub kaynak sağlayıcısı REST API'lerini](/rest/api/iothub/iothubresource)kullanarak veya [Azure portalı](https://portal.azure.com)ile bekletme süresini programlı olarak değiştirebilirsiniz.

IoT Hub, arka uç hizmetlerinizin hub'ınız tarafından alınan aygıttan buluta iletileri okuması için **iletileri/olayları** yerleşik bitiş noktasını ortaya çıkarır. Bu bitiş noktası, Olay Hub'ları hizmetinin iletileri okumak için desteklediği mekanizmalardan herhangi birini kullanmanıza olanak tanıyan Event Hub uyumludur.

## <a name="read-from-the-built-in-endpoint"></a>Yerleşik bitiş noktasından okuyun

Bazı ürün tümleştirmeleri ve Olay Hub'ları SDK'ları IoT Hub'ının farkındadır ve yerleşik bitiş noktasına bağlanmak için IoT hub hizmet bağlantı dizenizi kullanmanıza izin verir.

IoT Hub'ından habersiz Olan Event Hub'ları SDK'larını veya ürün tümleştirmelerini kullandığınızda, Olay Hub'ı uyumlu bir bitiş noktası ve Olay Hub'ı uyumlu bir ada ihtiyacınız vardır. Bu değerleri portaldan aşağıdaki gibi alabilirsiniz:

1. [Azure portalında](https://portal.azure.com) oturum açın ve IoT hub'ınıza gidin.

2. **Yerleşik uç noktaları**tıklatın.

3. **Olaylar** bölümü aşağıdaki değerleri içerir: **Bölümler,** **Olay Hub uyumlu ad,** **Event Hub uyumlu bitiş noktası,** **bekletme süresi**ve Tüketici **grupları.**

    ![Aygıt-bulut ayarları](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

Portalda, Event Hub uyumlu uç nokta alanı gibi görünen tam bir Olay Hub'ları bağlantı dizesi içerir: **Endpoint=sb://abcd1234namespace.servicebus.windows.net/; SharedAccessKeyName=iothubowner; SharedAccessKey=keykeykeykeykey=; EntityPath=iothub-ehub-abcd-1234-123456**. Kullandığınız SDK başka değerler gerektiriyorsa, bunlar aşağıdakileri olacaktır:

| Adı | Değer |
| ---- | ----- |
| Uç Nokta | sb://abcd1234namespace.servicebus.windows.net/ |
| Ana Bilgisayar Adı | abcd1234namespace.servicebus.windows.net |
| Ad Alanı | abcd1234namespace |

Daha sonra belirtilen Olay Hub'ına bağlanmak için **ServiceConnect** izinlerine sahip paylaşılan erişim ilkesini kullanabilirsiniz.

IoT Hub'ın ortaya çıkardığı yerleşik Event Hub uyumlu uç noktasına bağlanmak için kullanabileceğiniz SDK'lar:

| Dil | SDK | Örnek | Notlar |
| -------- | --- | ------ | ----- |
| .NET | https://github.com/Azure/azure-event-hubs-dotnet | [Quickstart](quickstart-send-telemetry-dotnet.md) | Etkinlik Hub'ları uyumlu bilgileri kullanır |
 Java | https://github.com/Azure/azure-event-hubs-java | [Quickstart](quickstart-send-telemetry-java.md) | Etkinlik Hub'ları uyumlu bilgileri kullanır |
| Node.js | https://github.com/Azure/azure-event-hubs-node | [Quickstart](quickstart-send-telemetry-node.md) | IoT Hub bağlantı dizelerini kullanır |
| Python | https://github.com/Azure/azure-event-hubs-python | https://github.com/Azure/azure-event-hubs-python/blob/master/examples/iothub_recv.py | IoT Hub bağlantı dizelerini kullanır |

IoT Hub'ın ortaya çıkardığı yerleşik Event Hub uyumlu uç noktasıyla kullanabileceğiniz ürün entegrasyonları şunlardır:

* [Azure Fonksiyonları](https://docs.microsoft.com/azure/azure-functions/). Bkz. [Azure İşlevleriyle IoT Hub'ından veri işleme.](https://azure.microsoft.com/resources/samples/functions-js-iot-hub-processing/)
* [Azure Akış Analizi](https://docs.microsoft.com/azure/stream-analytics/). [Akış verilerini Akış Analizi'ne giriş olarak](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-iot-hub)görün.
* [Zaman Serisi Insights](https://docs.microsoft.com/azure/time-series-insights/). Bkz. [Time Series Insights ortamınıza bir IoT hub olay kaynağı ekleyin.](../time-series-insights/time-series-insights-how-to-add-an-event-source-iothub.md)
* [Apaçi Fırtına emzik](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). GitHub'da [spout kaynağını](https://github.com/apache/storm/tree/master/external/storm-eventhubs) görüntüleyebilirsiniz.
* [Apache Kıvılcım entegrasyonu](../hdinsight/spark/apache-spark-eventhub-streaming.md).
* [Azure Veri Tuğlaları](https://docs.microsoft.com/azure/azure-databricks/).

## <a name="next-steps"></a>Sonraki adımlar

* IoT Hub uç noktaları hakkında daha fazla bilgi için [IoT Hub uç noktalarına](iot-hub-devguide-endpoints.md)bakın.

* [Hızlı Başlangıçlar,](quickstart-send-telemetry-node.md) simüle edilmiş aygıtlardan aygıttan buluta iletileri nasıl göndereceğinizi ve yerleşik bitiş noktasından gelen iletileri nasıl okuyabileceğinizi gösterir. 

Daha fazla ayrıntı [için, rotalar öğreticisini kullanarak İşlem IoT Hub aygıttan buluta iletilere](tutorial-routing.md) bakın.

* Aygıttan buluta iletilerinizi özel uç noktalara yönlendirmek istiyorsanız, [bkz.](iot-hub-devguide-messages-read-custom.md)
