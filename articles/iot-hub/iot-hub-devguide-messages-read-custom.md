---
title: Azure IoT Hub özel uç noktalarını anlama | Microsoft Dokümanlar
description: Geliştirici kılavuzu - aygıttan buluta iletileri özel uç noktalara yönlendirmek için yönlendirme sorgularını kullanma.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: e5e92c40cef15e99431dc9652820c71e87935f67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61244353"
---
# <a name="use-message-routes-and-custom-endpoints-for-device-to-cloud-messages"></a>Aygıttan buluta iletiler için ileti rotalarını ve özel uç noktalarını kullanma

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub [İleti yönlendirme,](iot-hub-devguide-routing-query-syntax.md) kullanıcıların aygıttan buluta iletileri hizmete bakan uç noktalara yönlendirmesini sağlar. Yönlendirme, verileri uç noktalara yönlendirmeden önce filtrelemek için bir sorgu lama yeteneği de sağlar. Yapılandırdığınız her yönlendirme sorgusuaşağıdaki özelliklere sahiptir:

| Özellik      | Açıklama |
| ------------- | ----------- |
| **Adı**      | Sorguyu tanımlayan benzersiz ad. |
| **Kaynak**    | Harekete alınacak veri akışının kaynağı. Örneğin, aygıt telemetrisi. |
| **Koşul** | İleti uygulama özellikleri, sistem özellikleri, ileti gövdesi, aygıt ikiz etiketleri ve aygıt ikiz ilerci özellikleriyle karşı çalıştırılan yönlendirme sorgusunun sorgu ifadesi, bitiş noktasının eşleşip eşleşmediğini belirler. Sorgu oluşturma hakkında daha fazla bilgi [message routing query syntax](iot-hub-devguide-routing-query-syntax.md) için bkz. |
| **Uç Nokta**  | IoT Hub'ın sorguyla eşleşen iletiler gönderdiği bitiş noktasının adı. IoT hub'ınızla aynı bölgede bir bitiş noktası seçmenizi öneririz. |

Tek bir ileti, birden çok yönlendirme sorgusundaki koşulla eşleşebilir ve bu durumda IoT Hub iletiyi eşleşen her sorguyla ilişkili bitiş noktasına teslim eder. IoT Hub ileti teslimini de otomatik olarak devre dener, bu nedenle bir ileti aynı hedefe sahip birden çok sorguyla eşleşirse, bu hedefe yalnızca bir kez yazılır.

## <a name="endpoints-and-routing"></a>Uç noktalar ve yönlendirme

Bir IoT hub varsayılan [yerleşik bitiş noktası](iot-hub-devguide-messages-read-builtin.md)vardır. Aboneliğinizdeki diğer hizmetleri hub'a bağlayarak iletileri yönlendirmek için özel uç noktalar oluşturabilirsiniz. IoT Hub şu anda Azure Depolama kapsayıcılarını, Olay Hub'larını, Servis Veri Hizmeti sıralarını ve Servis Veri Servisi veri meskenlerini özel uç nokta olarak destekler.

Yönlendirme ve özel uç noktaları kullandığınızda, iletiler yalnızca herhangi bir sorguyla eşleşmezlerse yerleşik bitiş noktasına teslim edilir. Yerleşik bitiş noktasına ve özel bir bitiş noktasına iletiler teslim etmek için yerleşik **olaylar** bitiş noktasına ileti gönderen bir rota ekleyin.

> [!NOTE]
> * IoT Hub, azure depolama kapsayıcılarına yalnızca blob olarak veri yazmayı destekler.
> * Servis Veri Servisi kuyrukları ve **Sessions** veya **Yinelenen Algılama** etkin olan konular özel uç nokta olarak desteklenmez.

IoT Hub'da özel uç noktaları oluşturma hakkında daha fazla bilgi için [Bkz. IoT Hub uç noktaları.](iot-hub-devguide-endpoints.md)

Özel uç noktalardan okuma hakkında daha fazla bilgi için bkz:

* Azure [Depolama kapsayıcılarından](../storage/blobs/storage-blobs-introduction.md)okuma.

* Etkinlik [Hub'larından](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)okuma.

* Servis [Veri Servisi kuyruklarından](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)okuma.

* Hizmet [Veri Servisi konularından](../service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions.md)okuma .

## <a name="next-steps"></a>Sonraki adımlar

* IoT Hub uç noktaları hakkında daha fazla bilgi için [IoT Hub uç noktalarına](iot-hub-devguide-endpoints.md)bakın.

* Yönlendirme sorgularını tanımlamak için kullandığınız sorgu dili hakkında daha fazla bilgi için İleti [yönlendirme sorgusu sözdizimini](iot-hub-devguide-routing-query-syntax.md)görme bkz.

* [Routes tutorial'i kullanarak Process IoT Hub aygıttan buluta iletiler](tutorial-routing.md) yönlendirme sorgularını ve özel uç noktaları nasıl kullanacağınızı gösterir.