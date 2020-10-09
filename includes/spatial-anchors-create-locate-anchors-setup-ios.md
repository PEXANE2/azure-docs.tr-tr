---
ms.openlocfilehash: e7e3a31663e6c166b912ba20166ba0f0f769008a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "83006405"
---
## <a name="setting-up-the-library"></a>Kitaplığı ayarlama

`Start()`Oturumunuzun ortam verilerini işlemesini sağlamak için öğesini çağırın.

Oturumunuz tarafından oluşturulan olayları işlemek için, `delegate` oturumunuzun özelliğini görünümü gibi bir nesne olarak ayarlayın. Bu nesnenin protokolü uygulaması gerekir `SSCCloudSpatialAnchorSessionDelegate` .
