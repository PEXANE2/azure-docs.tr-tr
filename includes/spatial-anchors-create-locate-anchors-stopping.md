---
ms.openlocfilehash: a165061b2f927d6302504819ef27c846d76d0f30
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "83006420"
---
## <a name="pause-reset-or-stop-the-session"></a>Oturumu duraklatma, sıfırlama veya durdurma

Oturumu geçici olarak durdurmak için komutunu çağırabilirsiniz `Stop()` . Bunun yapılması, çağırsanız bile tüm izleyicileri ve ortam işlemlerini durdurur `ProcessFrame()` . Daha sonra `Start()` işlemeyi sürdürmeyi sağlamak için öğesini çağırabilirsiniz. Devam edilirken, oturumda zaten yakalanan ortam verileri korunur.
