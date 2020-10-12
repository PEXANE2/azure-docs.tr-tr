---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67188401"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Bakım modu düzeltmelerini StorSimple için Windows PowerShell aracılığıyla yüklemek için
> [!IMPORTANT]
> Bakım modunda, düzeltmeyi önce bir denetleyicide, sonra da diğer denetleyicide uygulamanız gerekir.
> 
> 

1. Cihazı bakım moduna koyun. Bakım modunun nasıl kullanılacağına ilişkin yönergeler için bkz. [2. Adım: bakım modunu girin](../articles/storsimple/storsimple-update-device.md#step2) .
2. Düzeltmeyi uygulamak için şunu yazın:
   
     `Start-HcsHotfix` 
3. İstendiğinde, düzeltme dosyalarını içeren ağ paylaşılan klasörünün yolunu sağlayın.
4. Onayınız istenir. Düzeltme yüklemesine devam etmek için **Y** yazın.
5. Düzeltmeyi bir denetleyicide uyguladıktan sonra, diğer denetleyiciye oturum açın. Önceki denetleyicide yaptığınız gibi düzeltmeyi uygulayın.
6. Düzeltmeler uygulandıktan sonra bakım modundan çıkın. Yönergeler için bkz. [4. Adım: bakım modundan çıkma](../articles/storsimple/storsimple-update-device.md#step4) .

