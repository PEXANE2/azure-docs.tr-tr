---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/09/2018
ms.openlocfilehash: 89c2467843d7abc7c005804fd5263fe3beb668b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74793423"
---
Daha doğru tüketim maliyetlerini tahmin etmek için, hesaplamalarınızı yalnızca yoklama aralığına dayandırmak yerine, belirli bir günde gelebilecek olası ileti veya olay sayısını göz önünde bulundurun. Bir olay veya ileti tetikleyici ölçütlerini karşıladığında, birçok tetikleyici hemen ölçütleri karşılayan diğer bekleme olaylarını veya iletilerini okumaya çalışır. Bu davranış, daha uzun bir yoklama aralığı seçtiğinizde bile, tetikleyicinin iş akışlarını başlatmaya uygun bekleyen olayların veya iletilerin sayısına bağlı olarak devreye girdiği anlamına gelir. Bu davranışı izleyen tetikleyiciler arasında Azure Hizmet Veri Servisi ve Azure Etkinlik Hub'ı yer almaktadır.

Örneğin, her gün bir bitiş noktasını denetleyen tetikleyiciyi ayarladığınızı varsayalım. Tetikleyici bitiş noktasını denetlediği ve ölçütleri karşılayan 15 olay bulduğunda, tetikleyici ilgili iş akışını 15 kez çalıştırır ve çalıştırır. Logic Apps, tetikleyici istekleri de dahil olmak üzere bu 15 iş akışının gerçekleştirdiği tüm eylemleri ölçer. Olası maliyetlerinizi hesaplamak için [Azure fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/)deneyin.