---
title: Azure İşlevlerine Genel Bakış
description: Azure Işlevlerinin ölçeklenebilir sunucusuz uygulamalar oluşturmaya nasıl yardımcı olabileceğini öğrenin.
author: craigshoemaker
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 11/20/2020
ms.author: cshoe
ms.custom: contperfq2
ms.openlocfilehash: 8b836ac8bd2255abf9a94e532a0599994c3ede89
ms.sourcegitcommit: 4c89d9ea4b834d1963c4818a965eaaaa288194eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96608261"
---
# <a name="introduction-to-azure-functions"></a>Azure İşlevleri’ne Giriş

Genellikle bir dizi kritik olaya tepki vermek için sistem oluşturacağız. Bir Web API 'SI oluşturuyor, veritabanı değişikliklerine yanıt verme, IoT veri akışlarını işleme veya hatta ileti kuyruklarını yönetme gibi her uygulama, bu olaylar gerçekleştiğinde bazı kodları çalıştırmak için bir yol gerektirir.

Azure Işlevleri, bu gereksinimi karşılamak için iki önemli şekilde "talep üzerine işlem" sağlar.

İlk olarak, Azure Işlevleri sisteminizin mantığını kolayca kullanılabilir kod bloklarına uygulamanıza olanak tanır. Bu kod blokları "işlevler" olarak adlandırılır. Kritik olaylara yanıt vermek için her zaman farklı işlevler çalıştırılabilir.

İkinci olarak, istekler artdıkça Azure Işlevleri gereken sayıda kaynak ve işlev örneği (yalnızca gerektiğinde) ile talebi karşılar. İstekler düşecek şekilde, ek kaynaklar ve uygulama örnekleri otomatik olarak kapatılır.

Tüm işlem kaynakları nereden geliyor? Azure Işlevleri, uygulamanızın talebini karşılamak için [gereken sayıda veya daha fazla işlem kaynağı sağlar](./functions-scale.md) .

İşlem kaynaklarını isteğe bağlı olarak sağlamak, Azure Işlevlerinde [sunucusuz bilgi işlemin](https://azure.microsoft.com/solutions/serverless/) özünü sağlar.

## <a name="scenarios"></a>Senaryolar

Çoğu durumda, bir işlev, özellik bakımından zengin uygulamalar sağlamak için [bulut hizmetleri dizisiyle tümleştirilir](./functions-triggers-bindings.md) .

Aşağıdakiler yaygın bir, _ancak Hayır anlamına gelir_, Azure işlevleri için ayrıntılı bir senaryo kümesi.

| İstiyorsanız... | sonra... |
| --- | --- |
| **Web API'si oluşturun** | [Http tetikleyicisini](./functions-bindings-http-webhook.md) kullanarak Web uygulamalarınız için bir uç nokta uygulama |
| **Dosya yüklemelerini işle** | [BLOB depolamada](./functions-bindings-storage-blob.md) bir dosya karşıya yüklendiğinde veya değiştirildiğinde kodu Çalıştır |
| **Sunucusuz iş akışı oluşturma** | [Dayanıklı işlevler](./durable/durable-functions-overview.md) kullanarak bir dizi işlevin birlikte zinciri oluşturma |
| **Veritabanı değişikliklerine yanıt verme** | [Cosmos DB](./functions-bindings-cosmosdb-v2.md) bir belge oluşturulduğunda veya güncelleştirilirken özel mantık Çalıştır |
| **Zamanlanmış görevleri Çalıştır** | Kodu [belirlenen saatlerde](./functions-bindings-timer.md) yürütme |
| **Güvenilir ileti sırası sistemleri oluşturma** | [Kuyruk depolama](./functions-bindings-storage-queue.md), [Service Bus](./functions-bindings-service-bus.md)veya [Event Hubs](./functions-bindings-event-hubs.md) kullanarak ileti kuyruklarını işleme |
| **IoT veri akışlarını çözümleme** | [IoT cihazlarındaki verileri](./functions-bindings-event-iot.md) toplayın ve işleyin |
| **Verileri gerçek zamanlı olarak işleme** | Şu anda verilere yanıt vermek için [Functions ve SignalR](./functions-bindings-signalr-service.md) kullanın |

İşlevlerinizi oluştururken aşağıdaki seçenekler ve kaynaklar kullanılabilir:

- **Tercih ettiğiniz dili kullanın**: [C#, Java, JavaScript, PowerShell veya Python](./supported-languages.md)'da işlevleri yazın ya da neredeyse tüm diğer dilleri kullanmak için [özel bir işleyici](./functions-custom-handlers.md) kullanın.

- **Dağıtımı otomatikleştirin**: dış işlem hatlarını kullanmaya yönelik araçlar tabanlı bir yaklaşımdan, bir [Grup dağıtım seçeneği](./functions-deployment-technologies.md) mevcuttur.

- **Bir Işlevin sorunlarını giderme**: uygulamalarınız hakkında öngörüler elde etmek için [izleme araçlarını](./functions-monitoring.md) ve [Test stratejilerini](./functions-test-a-function.md) kullanın.

- **Esnek fiyatlandırma seçenekleri**: [Tüketim](./pricing.md) planı ile yalnızca işlevleriniz çalışırken ödeme yaparsınız; [Premium](./pricing.md) ve [App Service](./pricing.md) planları, özel gereksinimlere yönelik özellikler sunar.

## <a name="next-steps"></a>Sonraki Adımlar

> [!div class="nextstepaction"]
> [Dersleri, örnekleri ve etkileşimli öğreticileri kullanmaya başlayın](./functions-get-started.md)
