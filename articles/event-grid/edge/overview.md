---
title: Kenarda etkinlik odaklı mimariler — IoT Edge'deki Azure Olay Izgarası
description: Modüller, kenar aygıtları ve bulut arasındaki olayları iletmek için IoT Edge'de modül olarak Azure Olay Izgarasını kullanın.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: overview
ms.date: 10/22/2019
ms.author: babanisa
ms.openlocfilehash: feac5891734731e6f7377750127958a40a815036
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76844674"
---
# <a name="what-is-azure-event-grid-on-azure-iot-edge"></a>Azure IoT Edge'deki Azure Etkinlik Izgarası nedir?
IoT Edge'deki Olay Izgarası, Azure Olay Izgarasının gücünü ve esnekliğini öne çıkarır. Aynı aygıtta, diğer kenar aygıtlarında veya buluttaki hizmetlerde modül olsunlar, konular oluşturun, olayları yayımlayın ve birden çok noktaya abone olun.

Bulutta olduğu gibi, IoT Edge modülündeki Olay Izgarası, olayların ölçekte yönlendirme, filtreleme ve güvenilir bir şekilde teslimini işler. Gelişmiş dize, sayısal ve boolean filtreleri kullanarak yalnızca alakalı olayların farklı olay işleyicilerine gönderilmesini sağlamak için olayları filtreleyin. Yeniden deneme mantığı, yayımlama sırasında kullanılamasa bile olayın hedef hedefe ulaşmasını sağlar. IoT Edge'deki Event Grid'i güçlü bir mağaza ve ileri mekanizması olarak kullanmanıza olanak tanır.

IoT Edge'deki Olay Grid, hem CloudEvents v1.0 hem de özel etkinlik şemalarını destekler. Ayrıca, kolay birlikte çalışabilirlik için buluttaki Event Grid ile aynı Pub/Sub semantikini de destekler.

Bu makalede, IoT Edge'deki Azure Olay Izgarası'na genel bir bakış sağlanmaktadır. Bu modülü kenarda kullanmak için adım adım talimatlar için [bkz.](pub-sub-events-webhook-local.md) 

![Kaynakların ve işleyicilerin IoT Edge modelinde Olay Izgara](../media/edge-overview/functional-model.png)

Bu resim, IoT Edge'de Olay Izgarasını kullanmanın bazı yollarını gösterir ve desteklenen işlevselliğin kapsamlı bir listesi değildir.

## <a name="when-to-use-event-grid-on-iot-edge"></a>IoT Edge'de Olay Izgarası ne zaman kullanılır?

IoT Edge'deki Olay Izgarası, kenar ve bulut arasında kullanımı kolay ve güvenilir bir olay modeli sağlar.

IoT Edge'deki Olay Grid, Azure bulut hizmetine yönelik simetrik çalışma zamanı yüzey alanıyla oluşturulmuştur, böylece aynı olayları ve API çağrılarını ihtiyacınız olan her yerde kullanabilirsiniz. İster bulutta, ister kenarda, ister ikisi arasında pub/alt yapın, IoT Edge'deki Event Grid artık tek çözümünüzüz olabilir.

Modüller arasındaki basit iş akışlarını tetiklemek için IoT Edge'deki Olay Izgarasını kullanın. Örneğin, bir konu oluşturun ve depolama modülünüzden konuya "depolama blob oluşturuldu" olaylarını yayımlayın. Artık bu konulara bir veya birkaç fonksiyon veya özel modül abone olabilirsiniz.

İşlevlerinizi kenar aygıtları arasında genişletin. Blob modülü olaylarını yayımlıyorsanız ve kenarlara yakın birden çok aygıtın hesaplama gücünü kullanmak istiyorsanız, cihazlar arası abonelikler oluşturun.

Son olarak, buluta bağlanın. Blob modülü etkinlikleriniz bulutla düzenli olarak eşitlenecekse, bulutta bulunan daha fazla bilgi işlem kullanın veya işlenmiş verileri yukarı gönderin, ek bulut hizmeti abonelikleri oluşturun.

IoT Edge'deki Olay Izgarası esnek ve güvenilir ayrılmış bir olay mimarisi sağlar.

## <a name="event-sources"></a>Olay kaynakları

Bulutta olduğu gibi, IoT Edge'deki Event Grid de etkinlik odaklı mimariler oluşturmak için modüller arasında doğrudan tümleştirme sağlar. Şu anda, olaylar IoT Edge'deki Olay Izgarasına şu andan gönderilebilir:

* IoT Edge üzerinde Azure Blob Depolama
* CloudEvents kaynakları
* HTTP POST üzerinden konteyner& özel modüller

## <a name="event-handlers"></a>Olay işleyicileri

IoT Edge'deki Olay Izgarası, olayları istediğiniz yere göndermek için oluşturulmuştur. Şu anda, aşağıdaki hedefler desteklenir:

* IoT Hub, fonksiyonlar ve özel modüller dahil olmak üzere diğer modüller
* Diğer kenar aygıtları
* WebHooks
* Azure Olay Izgara bulut hizmeti
* Event Hubs
* Service Bus Kuyrukları
* Service Bus Konuları
* Depolama Kuyrukları

## <a name="supported-environments"></a>Desteklenen ortamlar
Şu anda, Windows 64-bit, Linux 64-bit ve ARM 32-bit ortamları desteklenir.

## <a name="concepts"></a>Kavramlar

Azure Etkinlik Adabı'nda başlamanızı sağlayan beş kavram vardır:

* **Olaylar** — Ne oldu.
* **Olay kaynakları** — Olayın gerçekleştiği yer.
* **Konular** — Yayıncıların etkinlik gönderdiği bitiş noktası.
* **Olay abonelikleri** — Olayları bazen birden fazla işleyiciye yönlendirmek için bitiş noktası veya yerleşik mekanizma. Abonelikler ayrıca işleyiciler tarafından gelen olayları akıllıca filtrelemek için de kullanılır.
* **Olay işleyicileri** — Olaya tepki veren uygulama veya hizmet.

## <a name="cost"></a>Maliyet

IoT Edge'deki Olay Izgarası genel önizleme sırasında ücretsizdir.

## <a name="issues"></a>Sorunlar
IoT Edge'de Event Grid'i [https://github.com/Azure/event-grid-iot-edge/issues](https://github.com/Azure/event-grid-iot-edge/issues)kullanmayla ilgili sorunları şu anda bildirin.

## <a name="next-steps"></a>Sonraki adımlar

* [Yayımla, etkinliklere yerel abone olun](pub-sub-events-webhook-local.md)
* [Buluttaki etkinlikleri yayımla, abone ol](pub-sub-events-webhook-cloud.md)
* [Olayları Olay Izgara bulutuna iletme](forward-events-event-grid-cloud.md)
* [Olayları IoTHub'a iletme](forward-events-iothub.md)
* [Blob Depolama olaylarına yerel olarak tepki verme](react-blob-storage-events-locally.md)