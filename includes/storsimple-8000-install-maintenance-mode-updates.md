---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 560c9c177bfa693580979101e5b9343fcff7fe40
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188567"
---
### <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Bakım modu güncelleştirmelerini StorSimple için Windows PowerShell aracılığıyla yüklensin

StorSimple cihaza bakım modu güncelleştirmeleri uyguladığınızda, tüm g/ç istekleri duraklatılır. Geçici olmayan rastgele erişim belleği (NVRAM) veya kümeleme hizmeti gibi hizmetler durdurulur. Bu moddan çıktığınızda veya çıkarken her iki denetleyici de yeniden başlatılır. Bu moddan çıktığınızda, tüm hizmetler sürdürülür ve sağlıklı çalışır. (Bu işlem birkaç dakika sürebilir.)

> [!IMPORTANT]
> * Bakım modunu girmeden önce, Azure portal her iki cihaz denetleyicisinin da sağlıklı olduğunu doğrulayın. Denetleyici sağlıklı değilse, sonraki adımlar için [Microsoft desteği başvurun](../articles/storsimple/storsimple-8000-contact-microsoft-support.md) .
> * Bakım modundayken, önce bir denetleyiciyi ve ardından diğer denetleyiciyi güncelleştirmeniz gerekir.

1. Seri konsoluna bağlanmak için PuTTY kullanın. [Seri konsola bağlanmak için PuTTy kullanma](../articles/storsimple/storsimple-8000-deployment-walkthrough-u2.md#use-putty-to-connect-to-the-device-serial-console) bölümündeki ayrıntılı yönergeleri izleyin. Komut isteminde **Enter** tuşuna basın. 1 seçeneğini belirleyin, **tam erişimle oturum açın**.

2. Denetleyiciyi bakım moduna yerleştirmek için şunu yazın:
    
    `Enter-HcsMaintenanceMode`

    Her iki denetleyici de bakım modunda yeniden başlatılır.

3. Bakım modu güncelleştirmelerinizi yükler. Şunu yazın:

    `Start-HcsUpdate`

    Onaylamanız istenir. Güncelleştirmeleri doğruladıktan sonra, şu anda eriştiğiniz denetleyiciye yüklenir. Güncelleştirmeler yüklendikten sonra denetleyici yeniden başlatılır.

4. Güncelleştirmelerin durumunu izleyin. Geçerli denetleyici güncellenirken ve diğer komutları işleyemediğinden, eş denetleyicisinde oturum açın. Şunu yazın:

    `Get-HcsUpdateStatus`

    `RunInProgress`İse `True` güncelleştirme hala devam etmektedir. `RunInProgress`İse `False` , güncelleştirmenin tamamlandığını gösterir.

5. Disk üretici yazılımı güncelleştirmeleri başarıyla uygulandıktan sonra ve güncelleştirilmiş denetleyici yeniden başlatıldıktan sonra, disk üretici yazılımı sürümünü doğrulayın. Güncelleştirilmiş denetleyiciye şunu yazın:

    `Get-HcsFirmwareVersion`
   
    Beklenen disk üretici yazılımı sürümleri şunlardır:`XMGJ, XGEG, KZ50, F6C2, VR08, N003, 0107`

6. Bakım modundan çıkın. Her cihaz denetleyicisi için aşağıdaki komutu yazın:

    `Exit-HcsMaintenanceMode`

    Bakım modundan çıktığınızda denetleyiciler yeniden başlatılır.

7. Azure portalına dönün. Portal, bakım modu güncelleştirmelerini 24 saat için yüklemi gösteremeyebilir.