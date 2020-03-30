---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 560c9c177bfa693580979101e5b9343fcff7fe40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188567"
---
### <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden bakım modu güncelleştirmelerini yükleyin

Bakım modu güncelleştirmelerini StorSimple aygıtına uyguladığınız zaman, tüm G/Ç istekleri duraklatıldı. Geçici olmayan rasgele erişim belleği (NVRAM) veya kümeleme hizmeti gibi hizmetler durdurulur. Bu moda girdiğinizde veya çıktığınızda her iki denetleyici de yeniden başlatılır. Bu moddan çıktığınızda, tüm hizmetler devam ettirilir ve sağlıklıdır. (Bu işlem birkaç dakika sürebilir.)

> [!IMPORTANT]
> * Bakım moduna girmeden önce, Azure portalında her iki aygıt denetleyicisinin de sağlıklı olduğunu doğrulayın. Denetleyici sağlıklı değilse, sonraki adımlar için [Microsoft Destek'e başvurun.](../articles/storsimple/storsimple-8000-contact-microsoft-support.md)
> * Bakım modundayken, önce bir denetleyiciyi, sonra da diğer denetleyiciyi güncelleştirmeniz gerekir.

1. Seri konsola bağlanmak için PuTTY'yi kullanın. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın. Seçenek 1'i seçin, **tam erişimle giriş yapın.**

2. Denetleyiciyi bakım moduna yerleştirmek için şunları yazın:
    
    `Enter-HcsMaintenanceMode`

    Her iki denetleyici de bakım moduna yeniden başlar.

3. Bakım modu güncelleştirmelerinizi yükleyin. Şunu yazın:

    `Start-HcsUpdate`

    Onay için istenir. Güncelleştirmeleri onayladıktan sonra, bunlar şu anda erişmekte olduğunuz denetleyiciye yüklenir. Güncelleştirmeler yüklendikten sonra denetleyici yeniden başlatılır.

4. Güncelleştirmelerin durumunu izleyin. Geçerli denetleyici güncellenirken ve diğer komutları işleyemezken eş denetleyicide oturum açın. Şunu yazın:

    `Get-HcsUpdateStatus`

    `RunInProgress` Ise, `True`güncelleştirme hala devam ediyor. `RunInProgress` Ise, `False`güncelleştirmenin tamamlandığını gösterir.

5. Disk firmware güncelleştirmeleri başarıyla uygulandıktan ve güncelleştirilmiş denetleyici yeniden başlatıldıktan sonra, disk firmware sürümünü doğrulayın. Güncelleştirilmiş denetleyicide şunları yazın:

    `Get-HcsFirmwareVersion`
   
    Beklenen disk firmware sürümleri şunlardır:`XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`

6. Bakım modundan çıkın. Her aygıt denetleyicisi için aşağıdaki komutu yazın:

    `Exit-HcsMaintenanceMode`

    Bakım modundan çıktığınızda denetleyiciler yeniden başlatılır.

7. Azure portalına dönün. Portal, bakım modu güncelleştirmelerini 24 saat boyunca yüklediğinizi göstermeyebilir.