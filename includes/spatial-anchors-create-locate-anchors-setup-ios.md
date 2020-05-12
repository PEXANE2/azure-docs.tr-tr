---
ms.openlocfilehash: e7e3a31663e6c166b912ba20166ba0f0f769008a
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006405"
---
## <a name="setting-up-the-library"></a>Kitaplığı ayarlama

Oturumunuzun `Start()` ortam verilerini işlemesini sağlamak için öğesini çağırın.

Oturumunuz tarafından oluşturulan olayları işlemek için, oturumunuzun `delegate` özelliğini görünümü gibi bir nesne olarak ayarlayın. Bu nesnenin `SSCCloudSpatialAnchorSessionDelegate` protokolü uygulaması gerekir.
