---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 8cc5dbb907c342b766cebe6da36cf580ddac5e2c
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67188403"
---
#### <a name="to-install-regular-hotfixes-via-windows-powershell-for-storsimple"></a>Düzenli düzeltmeleri StorSimple için Windows PowerShell aracılığıyla yüklemek için
1. Cihaz seri konsoluna bağlanın. Daha fazla bilgi için bkz. [1. Adım: seri konsoluna bağlanma](../articles/storsimple/storsimple-update-device.md#step1).
2. Seri konsol menüsünde, seçenek 1 ' i seçin, **tam erişim Ile oturum açın**. Parolayı yazın. Varsayılan parola **Parola1**' dir.
3. Komut istemine şunları yazın:
   
    ```
    Start-HcsHotfix
    ```
   
    > [!IMPORTANT]
    >
    > Bu komut yalnızca normal düzeltmeler için geçerlidir. Bu komutu yalnızca bir denetleyicide çalıştırırsınız, ancak her iki denetleyici de güncelleştirilir.
    > Güncelleştirme işlemi sırasında denetleyicinin yük devretmesini fark edebilirsiniz; Ancak, yük devretme sistem kullanılabilirliğini veya işlemini etkilemez.

4. İstendiğinde, düzeltme dosyalarını içeren ağ paylaşılan klasörünün yolunu sağlayın.
5. Onayınız istenir. Düzeltme yüklemesine devam etmek için **Y** yazın.

