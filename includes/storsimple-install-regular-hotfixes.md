---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8cc5dbb907c342b766cebe6da36cf580ddac5e2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188403"
---
#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden düzenli düzeltmeler yüklemek için
1. Aygıt seri konsoluna bağlanın. Daha fazla bilgi için [Bkz. Adım 1: Seri konsoluna bağlanın.](../articles/storsimple/storsimple-update-device.md#step1)
2. Seri konsol menüsünde, seçenek 1'i seçin, **tam erişimle giriş yapın.** Parolayı yazın. Varsayılan parola **Password1'dir.**
3. Komut istemine şunları yazın:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Bu komut yalnızca normal düzeltmeler için geçerlidir. Bu komutu yalnızca bir denetleyicide çalıştırın, ancak her iki denetleyici de güncelleştirilir.
    > Güncelleştirme işlemi sırasında bir denetleyicinin başarısız olduğunu fark edebilirsiniz; ancak, failover sistem kullanılabilirliğini veya çalışmasını etkilemez.

4. İstendiğinde, yolu düzeltme dosyalarını içeren ağ paylaşılan klasörüne sağlar.
5. Onayınız istenir. Düzeltme yüklemesine devam etmek için **Y** yazın.

