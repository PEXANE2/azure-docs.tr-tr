---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: b609a708a987194398c53bdf83f0d6e1f281808d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188864"
---
Varsayılan olarak, Mobil Uygulamalar arka uçtaki API'ler anonim olarak çağrılabilir. Ardından, yalnızca kimlik doğrulaması verilen istemcilere erişimi kısıtlamanız gerekir.  

* **Düğüm.js arka uç (Azure portalı üzerinden)** :  

    Mobil Uygulamalar ayarlarınızda **Kolay Tablolar'ı** tıklatın ve tablonuzu seçin. **İzinleri Değiştir'i**tıklatın, yalnızca tüm izinler için **Kimlik Doğrulaması erişimi** seçin ve ardından **Kaydet'i**tıklatın.
* **.NET arka uç (C#)**:  

    Sunucu projesinde, **denetleyiciler** > **TodoItemController.cs**gidin. Aşağıdaki `[Authorize]` gibi **TodoItemController** sınıfına özniteliği ekleyin. Erişimi yalnızca belirli yöntemlere kısıtlamak için, bu özniteliği sınıf yerine yalnızca bu yöntemlere de uygulayabilirsiniz. Sunucu projesini yeniden yayımlayın.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **Düğüm.js arka uç (Node.js kodu ile)** :  

    Tablo erişimi için kimlik doğrulaması gerektirmek için, Node.js sunucu komut dosyasına aşağıdaki satırı ekleyin:

        table.access = 'authenticated';

    Daha fazla ayrıntı için [bkz: Tablolara erişmek için kimlik doğrulamasını gerektirir.](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth) Sitenizden hızlı başlangıç kodu projesini nasıl indireceklerini öğrenmek için [bkz.](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart)
