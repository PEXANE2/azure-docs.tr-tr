---
ms.openlocfilehash: e7e3a31663e6c166b912ba20166ba0f0f769008a
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "83006405"
---
## <a name="setting-up-the-library"></a>Kitaplığı ayarlama

`Start()`Oturumunuzun ortam verilerini işlemesini sağlamak için öğesini çağırın.

Oturumunuz tarafından oluşturulan olayları işlemek için, `delegate` oturumunuzun özelliğini görünümü gibi bir nesne olarak ayarlayın. Bu nesnenin protokolü uygulaması gerekir `SSCCloudSpatialAnchorSessionDelegate` .
