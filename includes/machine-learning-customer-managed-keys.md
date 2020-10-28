---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 10/26/2020
ms.author: larryfr
ms.openlocfilehash: 1fde2947719079e411bc233bcce426feec8fa996
ms.sourcegitcommit: fb3c846de147cc2e3515cd8219d8c84790e3a442
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92631061"
---
> [!IMPORTANT]
> Cosmos DB örneği, __aboneliğinizdeki__ Microsoft tarafından yönetilen bir kaynak grubunda ve ihtiyaç duyacağı tüm kaynaklarla birlikte oluşturulur. Bu, Cosmos DB örneği için ücretlendirildiğiniz anlamına gelir. Yönetilen kaynak grubu biçiminde adlandırılır `<AML Workspace Resource Group Name><GUID>` . Azure Machine Learning çalışma alanınız özel bir uç nokta kullanıyorsa, Cosmos DB örneği için bir sanal ağ de oluşturulur. Bu sanal ağ Cosmos DB ve Azure Machine Learning arasındaki iletişimin güvenliğini sağlamak için kullanılır.
> 
> * Bu Cosmos DB örneğini içeren __kaynak grubunu__ veya bu grupta otomatik olarak oluşturulan kaynakları silmeyin. Kaynak grubunu, Cosmos DB örneğini, vb. silmeniz gerekiyorsa, onu kullanan Azure Machine Learning çalışma alanını silmeniz gerekir. Kaynak grubu, Cosmos DB örneği ve diğer otomatik oluşturulan kaynaklar ilişkili çalışma alanı silindiğinde silinir.
> * Bu Cosmos DB hesabının varsayılan [__Istek birimleri__](../articles/cosmos-db/request-units.md) __8000__ ' de ayarlanır. __Bu değerin değiştirilmesi desteklenmez__ .
> * Oluşturulan __Cosmos DB örneğiyle birlikte kullanmak için kendi VNET 'i sağlayamezsiniz__ . Ayrıca __sanal ağı__ değiştiremezsiniz. Örneğin, kullandığı IP adres aralığını değiştiremezsiniz.