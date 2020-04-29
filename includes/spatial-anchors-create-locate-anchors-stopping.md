---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188092"
---
## <a name="pause-reset-or-stop-the-session"></a>Oturumu duraklatma, sıfırlama veya durdurma

Oturumu geçici olarak durdurmak için komutunu çağırabilirsiniz `Stop()`. Bunun yapılması, ProcessFrame () çağırsanız bile tüm izleyicileri ve ortam işlemlerini durdurur. Daha sonra işlemeyi sürdürmeyi `Start()` sağlamak için öğesini çağırabilirsiniz. Devam edilirken, oturumda zaten yakalanan ortam verileri korunur.
