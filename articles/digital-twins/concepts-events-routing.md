---
title: Olayları ve mesajları yönlendirme - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins ile hizmet bitiş noktalarına yönlendirme olaylarına ve iletilere genel bakış
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: 65b760eaf28d907fab3654ed92f960be7556b0d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75862365"
---
# <a name="routing-iot-events-and-messages"></a>Yönlendirme IoT olayları ve mesajları

Nesnelerin İnterneti çözümleri genellikle depolama, analitik ve daha fazlasını içeren birkaç güçlü hizmeti birleştirir. Bu makalede, Azure Digital Twins uygulamalarının Azure analitiği, ai ve depolama hizmetlerine nasıl bağlanışla bağlanarak daha derin bilgiler ve işlevler sunabileceğiniz açıklanmaktadır.

## <a name="route-types"></a>Rota türleri  

Azure Digital Twins, IoT etkinliklerini diğer Azure hizmetleri veya iş uygulamalarıyla ilişkilendirmenin iki yolunu sunar:

* **Azure Dijital İkizler olaylarını yönlendirme**: Uzamsal grafikte değişen bir nesne, alınan telemetri verileri veya önceden tanımlanmış koşullara dayalı bir bildirim oluşturan kullanıcı tanımlı bir işlev Azure Digital Twins olaylarını tetikleyebilir. Kullanıcılar bu etkinlikleri daha fazla işlem için [Azure Etkinlik Hub'larına,](https://azure.microsoft.com/services/event-hubs/) [Azure Hizmet Veri Gönderi konularına](https://azure.microsoft.com/services/service-bus/)veya [Azure Olay Ağıt'ına](https://azure.microsoft.com/services/event-grid/) gönderebilir.

* **Yönlendirme cihazı telemetrisi**: Yönlendirme etkinliklerine ek olarak, Azure Digital Twins daha fazla bilgi ve analiz için ham cihaz telemetri mesajlarını Etkinlik Hub'larına yönlendirebilir. Bu tür iletiler Azure Digital Twins tarafından işlenmez. Ve sadece etkinlik merkezine iletilirler.

Kullanıcılar, etkinlik göndermek veya ileti leri iletmek için bir veya daha fazla çıkış uç noktası belirtebilir. Olaylar ve iletiler, önceden tanımlanmış yönlendirme tercihlerine göre uç noktalara gönderilir. Başka bir deyişle, kullanıcılar grafik işlem olaylarını almak için belirli bir bitiş noktası, aygıt telemetri sebebleri almak için başka bir nokta ve benzeri belirtebilirsiniz.

[![Azure Digital Twins etkinlikleri yönlendirme](media/concepts/digital-twins-events-routing.png)](media/concepts/digital-twins-events-routing.png#lightbox)

Olay Hub'larına yönlendirme, telemetri iletilerinin gönderilme sırasını korur. Bu yüzden bitiş noktasına ilk aldıkları sırayla ulaşırlar. 

Olay Izgara ve Servis Veri Servisi, uç noktalarının olayları oluştukları sırada alacağını garanti etmez. Ancak, olay şeması, olaylar bitiş noktasına ulaştıktan sonra sırayı tanımlamak için kullanılabilecek bir zaman damgası içerir.

## <a name="route-implementation"></a>Rota uygulaması

Azure Digital Twins hizmeti şu anda aşağıdaki **EndpointTypes**destekler:

* **EventHub,** Event Hub'ların bağlantı dize uç noktasıdır.
* **ServiceBus,** Service Bus bağlantı dizesi bitiş noktasıdır.
* **EventGrid** Olay Izgara bağlantı dize uç noktasıdır.

Azure Digital Twins şu anda seçilen bitiş noktasına gönderilecek aşağıdaki **EventTypes'ı** destekler:

* **DeviceMessages,** kullanıcıların aygıtlarından gönderilen ve sistem tarafından iletilen telemetri iletileridir.
* **Topolojiİşlem** grafiğin grafiğini veya meta verilerini değiştiren bir işlemdir. Bir örnek, boşluk gibi bir varlık eklemek veya siler.
* **SpaceChange,** bir aygıtın telemetri iletisinden kaynaklanan bir alanın hesaplanan değerindeki bir değişikliktir.
* **SensorChange,** bir cihazın telemetri iletisinden kaynaklanan bir sensörün hesaplanmış değerindeki bir değişikliktir.
* **UdfCustom,** kullanıcı tanımlı bir işlevden gelen özel bir bildirimdir.

> [!IMPORTANT]  
> Tüm **EndpointTypes** tüm **EventTypes**destekler.
> Her **EndpointType**için izin verilen **EventTypes** için aşağıdaki tabloyu gözden geçirin.

|             | Aygıt Mesajları | TopolojiOperasyonu | SpaceChange | Sensör Değişimi | UdfCustom |
| ----------- | -------------- | ----------------- | ----------- | ------------ | --------- |
| EventHub|     X          |         X         |     X       |      X       |   X       |
| ServiceBus|              |         X         |     X       |      X       |   X       |
| EventGrid|               |         X         |     X       |      X       |   X       |

>[!NOTE]  
>Son noktaları ve olayların şema örnekleri oluşturmak için nasıl daha fazla bilgi için, [Çıkış ve bitiş noktaları](how-to-egress-endpoints.md)okuyun.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Digital Twins önizleme sınırları hakkında bilgi edinmek için [Genel önizleme hizmeti sınırlarını](concepts-service-limits.md)okuyun.

- Azure Digital Twins örneğini denemek [için, kullanılabilir odaları bulmak için hızlı başlat'ı](quickstart-view-occupancy-dotnet.md)okuyun.