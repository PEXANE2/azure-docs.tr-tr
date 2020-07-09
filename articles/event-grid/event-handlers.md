---
title: Azure Event Grid olay işleyicileri
description: Azure Event Grid için desteklenen olay işleyicilerini açıklar. Azure Otomasyonu, Işlevler, Event Hubs, Karma Bağlantılar, Logic Apps, Service Bus, kuyruk depolama, Web kancaları.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 76f7fff3e0a3e1c0c554e452987bf944503358b8
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86117024"
---
# <a name="event-handlers-in-azure-event-grid"></a>Azure Event Grid içindeki olay işleyicileri
Olay işleyicisi, olayın gönderildiği yerdir. İşleyici, olayı işlemek için başka bir eylem gerçekleştirir. Çeşitli Azure Hizmetleri, olayları işleyecek şekilde otomatik olarak yapılandırılır. Olayları işlemek için herhangi bir Web kancasını da kullanabilirsiniz. Web kancasının olayları işlemek için Azure 'da barındırılması gerekmez. Event Grid yalnızca HTTPS Web kancası uç noktalarını destekler.

## <a name="supported-event-handlers"></a>Desteklenen olay işleyicileri
Desteklenen olay işleyicileri aşağıda verilmiştir: 

- [Web kancaları](handler-webhooks.md). Azure Otomasyonu runbook 'ları ve Logic Apps web kancaları aracılığıyla desteklenir. 
- [Azure işlevleri](handler-functions.md)
- [Olay hub’ları](handler-event-hubs.md)
- [Hibrit bağlantılarının geçişi](handler-relay-hybrid-connections.md)
- [Service Bus kuyrukları ve konuları](handler-service-bus.md)
- [Depolama kuyrukları](handler-storage-queues.md)

## <a name="next-steps"></a>Sonraki adımlar
- Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
