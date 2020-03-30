---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: dc50f94ae9b207961a71480c2fc172e88db79cf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188404"
---
#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden düzenli güncelleştirmeler yüklemek için
1. Cihaz seri konsolu açın ve seçenek 1 seçin, **tam erişim ile giriş**yapın. Parolayı yazın. Varsayılan parola *Password1'dir.* 
2. Komut istemine şunları yazın:
   
     `Get-HcsUpdateAvailability`
   
    Güncelleştirmelerin kullanılabilir olup olmadığı ve güncelleştirmelerin kesintiye uğrayıp veya kesintiye uğramayacağını size bildirilir.
3. Komut istemine şunları yazın:
   
     `Start-HcsUpdate`
   
    Güncelleştirme işlemi başlayacaktır.

> [!IMPORTANT]
> * Bu komut yalnızca normal güncelleştirmeler için geçerlidir. Bu komutu yalnızca bir denetleyicide çalıştırın, ancak her iki denetleyici de güncelleştirilir. 
> * Güncelleştirme işlemi sırasında bir denetleyicinin başarısız olduğunu fark edebilirsiniz; ancak, failover sistem kullanılabilirliğini veya çalışmasını etkilemez.
> 
> 

