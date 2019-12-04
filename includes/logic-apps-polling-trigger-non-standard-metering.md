---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/09/2018
ms.openlocfilehash: 89c2467843d7abc7c005804fd5263fe3beb668b6
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74793423"
---
Daha doğru tüketim maliyetlerini tahmin etmek için, yalnızca yoklama aralığındaki hesaplamalarınızı temel almak yerine, belirli bir güne ulaşacak olabilecek ileti veya olay sayısını göz önünde bulundurun. Bir olay veya ileti tetikleyici ölçütlerini karşıladığında, birçok tetikleyici tüm diğer bekleyen olayları ve ölçütlere uyan iletileri okumayı hemen dener. Bu davranış, daha uzun bir yoklama aralığı seçtiğinizde bile tetikleyici, iş akışlarını başlatmak için uygun olan bekleme olay veya ileti sayısına göre ateşlenir. Bu davranışı izleyen Tetikleyiciler Azure Service Bus ve Azure Olay Hub 'ı içerir.

Bu nedenle, örneğin, her gün bir uç noktayı denetleyen tetikleyiciyi ayarladığınızı varsayalım. Tetikleyici, uç noktayı denetlediğinde ve ölçütlere uyan 15 olay bulduğunda, tetikleyici ateşlenir ve ilgili iş akışını 15 kez çalıştırır. Tetikleyici istekleri dahil olmak üzere, bu 15 iş akışlarının gerçekleştirdiği tüm eylemleri Logic Apps ölçümleri. Olası maliyetlerinizi hesaplamak için [Azure Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)' nı deneyin.