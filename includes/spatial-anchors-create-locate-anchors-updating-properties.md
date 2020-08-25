---
ms.openlocfilehash: 8ebb10f955be8f3004fdbdc595ea0fefc0d2b7ea
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "67188084"
---
## <a name="update-properties"></a>Güncelleştirme özellikleri

Bir bağlayıcının özelliklerini güncelleştirmek için `UpdateAnchorProperties()` yöntemini kullanırsınız. İki veya daha fazla cihaz aynı anda aynı bağlantı için özellikleri güncelleştirmeye çalışıyorsa, iyimser bir eşzamanlılık modeli kullanıyoruz. Bu, ilk yazmanın kazanacağı anlamına gelir.  Diğer tüm yazma işlemleri bir "eşzamanlılık" hatası alır: yeniden denenmeye başlamadan önce özelliklerin yenilenmesi gerekiyor.
