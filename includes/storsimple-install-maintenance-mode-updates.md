---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 720288aff462b0590bb9da509096a9305b9b6cc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188406"
---
#### <a name="to-install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden Bakım modu güncelleştirmelerini yüklemek için
1. Bunu daha önce yapmadıysanız, aygıt seri konsoluna erişin ve seçenek 1'i seçin, **tam erişimle giriş yapın.** 
2. Parolayı yazın. Varsayılan parola **Password1'dir.**
3. Komut istemine şunları yazın:
   
     `Get-HcsUpdateAvailability` 
4. Güncelleştirmelerin kullanılabilir olup olmadığı ve güncelleştirmelerin kesintiye uğrayıp veya kesintiye uğramayacağını size bildirilir. Rahatsız edici güncelleştirmeler uygulamak için aygıtı Bakım moduna sokmanız gerekir. Bkz. Adım 2: Talimatlar için [Bakım moduna girin.](../articles/storsimple/storsimple-update-device.md#step2)
5. Cihazınız Bakım modundayken, komut isteminde şunları yazın:`Start-HcsUpdate`
6. Onayınız istenir. Güncelleştirmeleri onayladıktan sonra, bunlar şu anda erişmekte olduğunuz denetleyiciye yüklenir. Güncelleştirmeler yüklendikten sonra denetleyici yeniden başlatılır. 
7. Güncelleştirmelerin durumunu izleyin. Şunu yazın:
   
    `Get-HcsUpdateStatus`
   
    `RunInProgress` Ise, `True`güncelleştirme hala devam ediyor. `RunInProgress` Ise, `False`güncelleştirmenin tamamlandığını gösterir.  
8. Güncelleştirme geçerli denetleyiciye yüklendiğinde ve yeniden başlatıldığında, diğer denetleyiciye bağlanın ve 1'den 6'ya kadar olan adımları gerçekleştirin.
9. Her iki denetleyici de güncellendikten sonra, Bakım modundan çıkın. Bkz. Adım 4: Talimatlar için [Çıkış Bakım modu.](../articles/storsimple/storsimple-update-device.md#step4)

