---
ms.openlocfilehash: 6d6886996d926ce778600dfa1d69caaa1c7ac004
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "67188092"
---
## <a name="pause-reset-or-stop-the-session"></a>Oturumu duraklatma, sıfırlama veya durdurma

Oturumu geçici olarak durdurmak için. `Stop()` Bunu yapmak, ProcessFrame() 'yi çağırsanız bile, tüm izleyicileri ve ortam işlemeyi durdurur. Daha sonra `Start()` işleme devam etmek için çağırabilirsiniz. Devam ederken, oturumda zaten yakalanan ortam verileri korunur.
