---
ms.openlocfilehash: a165061b2f927d6302504819ef27c846d76d0f30
ms.sourcegitcommit: ac4a365a6c6ffa6b6a5fbca1b8f17fde87b4c05e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2020
ms.locfileid: "83006420"
---
## <a name="pause-reset-or-stop-the-session"></a>Oturumu duraklatma, sıfırlama veya durdurma

Oturumu geçici olarak durdurmak için komutunu çağırabilirsiniz `Stop()`. Bunun yapılması, çağırsanız `ProcessFrame()`bile tüm izleyicileri ve ortam işlemlerini durdurur. Daha sonra işlemeyi sürdürmeyi `Start()` sağlamak için öğesini çağırabilirsiniz. Devam edilirken, oturumda zaten yakalanan ortam verileri korunur.
