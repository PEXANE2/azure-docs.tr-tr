---
title: MongoDB işlemlerine yönelik Azure Cosmos DB API için hız sınırlaması hatalarını engelleyin.
description: MongoDB işlemlerine yönelik Azure Cosmos DB API 'nizin SSR (sunucu tarafı yeniden deneme) özelliği ile hataları sınırlandırma oranını nasıl önleyeceğinizi öğrenin.
author: gahl-levy
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 01/13/2021
ms.author: gahllevy
ms.openlocfilehash: 73c2aba3028f42621f241bd8f295e83e0ef96e68
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540737"
---
# <a name="prevent-rate-limiting-errors-for-azure-cosmos-db-api-for-mongodb-operations"></a>MongoDB işlemlerine yönelik Azure Cosmos DB API 'sinin hız sınırlaması hatalarını engelleyin
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

MongoDB işlemlerine yönelik Azure Cosmos DB API 'SI, bir koleksiyonun aktarım hızı sınırını (ru) aşarsa hız sınırlaması (16500/429) hatalarıyla başarısız olabilir. 

Sunucu tarafı yeniden deneme (SSR) özelliğini etkinleştirebilir ve sunucunun bu işlemleri otomatik olarak yeniden denemesini sağlayabilirsiniz. İstek, hesabınızdaki tüm koleksiyonlar için kısa bir gecikmeden sonra yeniden denenir. Bu özellik, istemci uygulamasındaki hız sınırlaması hatalarını işlemeye yönelik kullanışlı bir alternatiftir.


## <a name="use-the-azure-portal"></a>Azure portalını kullanma

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. MongoDB hesabı için Azure Cosmos DB API 'nize gidin.

1. **Ayarlar** bölümünün altındaki **Özellikler** bölmesine gidin.

1. **Sunucu tarafında yeniden dene**' yi seçin.

1. Hesabınızdaki tüm koleksiyonlar için bu özelliği etkinleştirmek üzere **Etkinleştir** ' e tıklayın.

:::image type="content" source="./media/prevent-rate-limiting-errors/portal-features-server-side-retry.png" alt-text="MongoDB için Azure Cosmos DB API 'sinin sunucu tarafı yeniden deneme özelliğinin ekran görüntüsü":::


## <a name="next-steps"></a>Sonraki adımlar

Yaygın hatalarda sorun giderme hakkında daha fazla bilgi için şu makaleye bakın:

* [MongoDB için Azure Cosmos DB API 'sindeki yaygın sorunları giderme](mongodb-troubleshoot.md)
