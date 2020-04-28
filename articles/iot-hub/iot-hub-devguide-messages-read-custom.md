---
title: Azure IoT Hub özel uç noktalarını anlayın | Microsoft Docs
description: Geliştirici Kılavuzu-cihazdan buluta iletileri özel uç noktalara yönlendirmek için yönlendirme sorguları kullanma.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: e5e92c40cef15e99431dc9652820c71e87935f67
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61244353"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Cihazdan buluta iletiler için ileti yollarını ve özel uç noktaları kullanın

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub [Ileti yönlendirme](iot-hub-devguide-routing-query-syntax.md) , kullanıcıların cihazdan buluta iletileri hizmete yönelik uç noktalara yönlendirmesine olanak sağlar. Yönlendirme Ayrıca, verileri uç noktalara yönlendirmeden önce verilerin filtreleneceği bir sorgulama özelliği de sağlar. Yapılandırdığınız her yönlendirme sorgusu aşağıdaki özelliklere sahiptir:

| Özellik      | Açıklama |
| ------------- | ----------- |
| **Adı**      | Sorguyu tanımlayan benzersiz ad. |
| **Kaynak**    | Üzerinde işlem yapılacak veri akışının kaynağı. Örneğin, cihaz telemetrisi. |
| **Koşul** | İleti uygulama özellikleri, sistem özellikleri, ileti gövdesi, cihaz ikizi etiketleri ve cihaz ikizi özelliklerine karşı çalıştırılan ve bu, uç nokta için bir eşleşme olup olmadığını belirleme sorgusu ifadesi. Sorgu oluşturma hakkında daha fazla bilgi için bkz. [ileti yönlendirme sorgusu sözdizimi](iot-hub-devguide-routing-query-syntax.md) |
| **Uç Nokta**  | IoT Hub sorguyla eşleşen iletileri gönderdiği bitiş noktasının adı. IoT Hub 'ınız ile aynı bölgede bir uç nokta seçmenizi öneririz. |

Tek bir ileti birden çok yönlendirme sorgusunda koşulla eşleşir, bu durumda IoT Hub iletiyi eşleşen her sorguyla ilişkili uç noktaya teslim eder. IoT Hub Ayrıca ileti teslimini otomatik olarak kaldırır, bu nedenle bir ileti aynı hedefe sahip birden çok sorguyla eşleşirse, bu hedefe yalnızca bir kez yazılır.

## <a name="endpoints-and-routing"></a>Uç noktalar ve yönlendirme

IoT Hub 'ının varsayılan bir [yerleşik uç noktası](iot-hub-devguide-messages-read-builtin.md)vardır. Aboneliğinizdeki diğer hizmetleri hub 'a bağlayarak, iletileri yönlendirmek için özel uç noktalar oluşturabilirsiniz. IoT Hub Şu anda Azure depolama kapsayıcıları, Event Hubs, Service Bus kuyruklarını ve Service Bus konularını özel uç noktalar olarak destekliyor.

Yönlendirme ve özel uç noktaları kullandığınızda, iletiler yalnızca herhangi bir sorguyla eşleşmezse yerleşik uç noktaya dağıtılır. Yerleşik uç noktaya ve özel bir uç noktaya ileti teslim etmek için, yerleşik **Olaylar** uç noktasına ileti gönderen bir rota ekleyin.

> [!NOTE]
> * IoT Hub yalnızca blob olarak Azure depolama kapsayıcılarına veri yazılmasını destekler.
> * Service Bus kuyruklar ve **Oturumlar** ve **yinelenen algılama** etkin olan konular özel uç noktalar olarak desteklenmez.

IoT Hub özel uç noktalar oluşturma hakkında daha fazla bilgi için bkz. [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md).

Özel uç noktalardan okuma hakkında daha fazla bilgi için bkz.:

* [Azure depolama kapsayıcılarından](../storage/blobs/storage-blobs-introduction.md)okuma.

* [Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)okunuyor.

* [Service Bus kuyruklarından](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)okuma.

* [Service Bus konularından](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md)okuma.

## <a name="next-steps"></a>Sonraki adımlar

* IoT Hub uç noktaları hakkında daha fazla bilgi için bkz. [IoT Hub uç noktaları](iot-hub-devguide-endpoints.md).

* Yönlendirme sorgularını tanımlamak için kullandığınız sorgu dili hakkında daha fazla bilgi için bkz. [Ileti yönlendirme sorgusu sözdizimi](iot-hub-devguide-routing-query-syntax.md).

* Yollar öğreticisini [kullanarak cihazdan buluta iletileri IoT Hub işlem](tutorial-routing.md) yönlendirme sorgularını ve özel uç noktaları nasıl kullanacağınızı gösterir.