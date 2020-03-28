---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "67188084"
---
## <a name="update-properties"></a>Özellikleri güncelleştirme

Bir bağlantıdaki özellikleri güncelleştirmek için `UpdateAnchorProperties()` yöntemi kullanırsınız. İki veya daha fazla aygıt aynı bağlantı noktası için özellikleri güncelleştirmeye çalışırsa, iyimser bir eşzamanlılık modeli kullanırız. Bu da ilk yazının kazanacağı anlamına geliyor.  Diğer tüm yazılarbir "Eşzamanlılık" hatası alır: yeniden denemeden önce özelliklerin yenilenmesi gerekir.
