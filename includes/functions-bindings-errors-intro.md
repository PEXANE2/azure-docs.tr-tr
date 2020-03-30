---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 629de079f7cc7d95d10f8ff951a47b8b8fc62dad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77474124"
---
Azure İşlevlerinde ortaya çıkan hatalar aşağıdaki kökenlerden herhangi birinden gelebilir:

- Yerleşik Azure Fonksiyonlarının kullanımı [tetikleyiciler ve bağlamalar](..\articles\azure-functions\functions-triggers-bindings.md)
- Temel Azure hizmetlerinin API'lerine çağrılar
- REST uç noktalarına çağrılar
- İstemci kitaplıklarına, paketlere veya üçüncü taraf API'lerine yapılan aramalar

Katı hata işleme uygulamalarını takip etmek, veri kaybını veya cevapsız iletileri önlemek için önemlidir. Önerilen hata işleme uygulamaları aşağıdaki eylemleri içerir:

- [Application Insights'ı etkinleştirme](../articles/azure-functions/functions-monitoring.md)
- [Yapılandırılmış hata işleme yi kullanma](#use-structured-error-handling)
- [Idempotency için tasarım](../articles/azure-functions/functions-idempotent.md)
- [Yeniden deneme ilkelerini uygulayın](../articles/azure-functions/functions-reliable-event-processing.md) (uygun olduğunda)

### <a name="use-structured-error-handling"></a>Yapılandırılmış hata işleme yi kullanma

Hataları yakalamak ve yayımlamak, uygulamanızın durumunu izlemek için çok önemlidir. Herhangi bir işlev kodunun en üst düzey düzeyi bir try/catch bloğu içermelidir. Catch bloğunda hataları yakalayabilir ve yayımlayabilirsiniz.

### <a name="retry-support"></a>Desteği yeniden deneyin

Aşağıdaki tetikleyiciler yerleşik yeniden deneme desteğine sahiptir:

* [Azure Blob depolama](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure Sıra depolama](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Servis Veri Servisi (sıra/konu)](../articles/azure-functions/functions-bindings-service-bus.md)

Varsayılan olarak, bu tetikleyiciler istekleri en fazla beş kez yeniden dener. Beşinci yeniden denemeden sonra, hem Azure Kuyruk depolama alanı hem de Azure Hizmet Veri Servisi tetikleyicileri [zehirli kuyruğa](..\articles\azure-functions\functions-bindings-storage-queue-trigger.md#poison-messages)bir ileti yazar.

Diğer tetikleyiciler veya bağlama türleri için yeniden deneme ilkelerini el ile uygulamanız gerekir. El ile [uygulamalar, bir zehirli ileti kuyruğuna](..\articles\azure-functions\functions-bindings-storage-blob-trigger.md#poison-blobs)hata bilgisi yazmayı içerebilir. Zehirli bir sıraya yazarak, işlemleri daha sonra yeniden deneme fırsatına sahip siniz. Bu yaklaşım, Blob depolama tetikleyicisi tarafından kullanılan la aynıdır.
