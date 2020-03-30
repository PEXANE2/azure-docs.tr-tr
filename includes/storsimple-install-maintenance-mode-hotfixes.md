---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188401"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell üzerinden Bakım modu düzeltmeleri yüklemek için
> [!IMPORTANT]
> Bakım modunda, düzeltmeyi önce bir denetleyiciye, sonra da diğer denetleyiciye uygulamanız gerekir.
> 
> 

1. Cihazı Bakım moduna yerleştirin. Bkz. Adım 2: Bakım moduna nasıl girilir e-talimatları için [Bakım moduna girin.](../articles/storsimple/storsimple-update-device.md#step2)
2. Düzeltmeyi uygulamak için şunları yazın:
   
     `Start-HcsHotfix` 
3. İstendiğinde, yolu düzeltme dosyalarını içeren ağ paylaşılan klasörüne sağlar.
4. Onayınız istenir. Düzeltme yüklemesine devam etmek için **Y** yazın.
5. Düzeltmeyi bir kumandaya uyguladıktan sonra, diğer denetleyicide oturum açın. Önceki denetleyici için yaptığınız düzeltmeyi uygulayın.
6. Düzeltmeler uygulandıktan sonra Bakım modundan çıkın. Bkz. Adım 4: Talimatlar için [Çıkış Bakım modu.](../articles/storsimple/storsimple-update-device.md#step4)

