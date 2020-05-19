---
title: Azure Event Grid olaylar için olay işleyicisi olarak depolama kuyruğu
description: Azure depolama kuyruklarını Azure Event Grid olayları için olay işleyicileri olarak nasıl kullanabileceğinizi açıklar.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: spelluru
ms.openlocfilehash: 9b767caa1041f865d8e15cd57796b186f7a4a6bb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598545"
---
# <a name="storage-queue-as-an-event-handler-for-azure-event-grid-events"></a>Azure Event Grid olaylar için olay işleyicisi olarak depolama kuyruğu
Olay işleyicisi, olayın gönderildiği yerdir. İşleyici, olayı işlemek için başka bir eylem gerçekleştirir. Birkaç Azure hizmeti, olayları işleyecek şekilde otomatik olarak yapılandırılır ve **Azure kuyruk depolaması** bunlardan biridir. 

Çekilmesi gereken olayları almak için **kuyruk depolama alanını** kullanın. Yanıt vermeyi çok uzun süren uzun süre çalışan bir işleminiz varsa kuyruk depolama alanını kullanabilirsiniz. Uygulama kuyruk depolamaya olay göndererek olayları kendi zamanlamaya göre çekebilir ve işleyebilir.

## <a name="tutorials"></a>Öğreticiler
Kuyruk depolama alanını olay işleyicisi olarak kullanmanın bir örneği için aşağıdaki öğreticiye bakın. 

|Başlık  |Açıklama  |
|---------|---------|
| [Hızlı başlangıç: Azure CLı ve Event Grid ile Azure kuyruk depolama 'ya özel olayları yönlendirme](custom-event-to-queue-storage.md) | Bir kuyruk depolamasına özel olayların nasıl gönderileceğini açıklar. |

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen olay işleyicilerinin bir listesi için bkz. [olay işleyicileri](event-handlers.md) makalesi. 
