---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188084"
---
## <a name="update-properties"></a>Güncelleştirme özellikleri

Bir bağlayıcının özelliklerini güncelleştirmek için `UpdateAnchorProperties()` yöntemini kullanırsınız. İki veya daha fazla cihaz aynı anda aynı bağlantı için özellikleri güncelleştirmeye çalışıyorsa, iyimser bir eşzamanlılık modeli kullanıyoruz. Bu, ilk yazmanın kazanacağı anlamına gelir.  Diğer tüm yazma işlemleri bir "eşzamanlılık" hatası alır: yeniden denenmeye başlamadan önce özelliklerin yenilenmesi gerekiyor.
