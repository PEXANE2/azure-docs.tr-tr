---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d35f0ef783a2c48f8211657bc8829635c19495aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188398"
---
#### <a name="to-enter-maintenance-mode"></a>Bakım moduna girmek için
1. Seri konsol menüsünde, seçenek 1 seçin, **tam erişim ile giriş yapın.**
2. Parolayı yazın. Varsayılan parola **Password1'dir.**
3. Komut isteminde,
   
     `Enter-HcsMaintenanceMode`
4. Bakım modunun tüm G/Ç isteklerini bozacağını ve Azure klasik portalına olan bağlantıyı keseceğini belirten bir uyarı iletisi görürsünüz ve onay için istenirsiniz. Bakım moduna girmek için **Y** yazın.
   
    Her iki denetleyici de yeniden başlatılacaktır. Yeniden başlatma tamamlandığında, aygıtın Bakım modunda olduğunu belirten başka bir ileti görüntülenir.

