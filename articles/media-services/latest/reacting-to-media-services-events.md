---
title: Azure Medya Hizmetleri olaylarına tepki verme | Microsoft Dokümanlar
description: Bu makalede, Medya Hizmetleri etkinliklerine abone olmak için Azure Olay Ağıt'ın nasıl kullanılacağı açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/08/2019
ms.author: juliako
ms.openlocfilehash: e24bacb0ea7ab406442022915872fc77e9cc1a5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74887893"
---
# <a name="handling-event-grid-events"></a>Event Grid olaylarını işleme

Medya Hizmetleri olayları, uygulamaların modern sunucusuz mimariler kullanarak farklı olaylara (örneğin, iş durumu değişikliği olayı) tepki göstermesine olanak sağlar. Bunu karmaşık kod veya pahalı ve verimsiz yoklama hizmetlerine gerek kalmadan yapar. Bunun yerine, etkinlikler [Azure İşlevleri](https://azure.microsoft.com/services/functions/), Azure Mantıksal [Uygulamaları](https://azure.microsoft.com/services/logic-apps/)ve hatta kendi Web hook'unuz gibi etkinlik işleyicilerine [Azure Olay Izgarası](https://azure.microsoft.com/services/event-grid/) üzerinden itilir ve yalnızca kullandığınız kadar ını ödenersiniz. Fiyatlandırma hakkında daha fazla bilgi için [Olay Ağı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/event-grid/)bakın.

Ortam Hizmetleri etkinlikleri için kullanılabilirlik Olay Izgara [kullanılabilirliğine](../../event-grid/overview.md) bağlıdır ve Olay Izgara'nın yaptığı gibi diğer bölgelerde kullanılabilir hale gelir.  

## <a name="media-services-events-and-schemas"></a>Medya Hizmetleri etkinlikleri ve şemalar

Olay [ızgarası,](../../event-grid/concepts.md#event-subscriptions) olay iletilerini abonelere yönlendirmek için olay aboneliklerini kullanır. Medya Hizmetleri olayları, verilerinizdeki değişikliklere yanıt vermek için gereken tüm bilgileri içerir. EventType özelliği "Microsoft.Media" ile başladığı için bir Medya Hizmetleri olayını tanımlayabilirsiniz.

Daha fazla bilgi için Medya [Hizmetleri etkinlik şemalarına](media-services-event-schemas.md)bakın.

## <a name="practices-for-consuming-events"></a>Etkinlikleri tüketme uygulamaları

Medya Hizmetleri olaylarını işleyen uygulamalar, önerilen birkaç uygulamayı izlemelidir:

* Birden çok abonelik olayları aynı olay işleyicisine yönlendirecek şekilde yapılandırılabildiği için, olayların belirli bir kaynaktan geldiğini varsaymak değil, beklediğiniz depolama hesabından geldiğinden emin olmak için iletinin konusunu denetlemek önemlidir.
* Benzer şekilde, eventType'ın işlemeye hazır olup olmadığını denetleyin ve aldığınız tüm olayların beklediğiniz türler olacağını varsaymayın.
* Anlamadığınız alanları yoksay.  Bu uygulama, gelecekte eklenebilir yeni özelliklere karşı esnek tutmanıza yardımcı olacaktır.
* Olayları belirli bir olayla sınırlamak için "özne" öneki ve sonek eşleşmelerini kullanın.

> [!NOTE]
> Olaylar, Olay Izgara [Hizmet Düzeyi Sözleşmesi'ne (SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/)tabidir. API'leri kullanarak olay bildirimleri almak istiyorsanız, [.NET SDK](https://github.com/Azure-Samples/media-services-v3-dotnet) veya [Java SDK](https://github.com/Azure-Samples/media-services-v3-java)ile olayların nasıl tüketilene ilişkin örneklere bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Olayları izleyin - portal](monitor-events-portal-how-to.md)
* [Olayları izleme - CLI](job-state-events-cli-how-to.md)
