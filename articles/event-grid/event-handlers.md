---
title: Azure Event Grid olay işleyicileri
description: Azure Event Grid için desteklenen olay işleyicilerini açıklar. Azure Otomasyonu, Işlevler, Event Hubs, Karma Bağlantılar, Logic Apps, Service Bus, kuyruk depolama, Web kancaları.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: c29526bdee5ad70b3acb23117ee003877f52a55e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83592466"
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
