---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: dc50f94ae9b207961a71480c2fc172e88db79cf4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "67188404"
---
#### <a name="to-install-regular-updates-via-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell aracılığıyla düzenli güncelleştirmeleri yüklemek için
1. Cihaz seri konsolunu açın ve seçenek 1 ' i seçin, **tam erişimle oturum açın**. Parolayı yazın. Varsayılan parola *Parola1*' dir. 
2. Komut istemine şunları yazın:
   
     `Get-HcsUpdateAvailability`
   
    Güncelleştirmeler varsa ve güncelleştirmelerin karışıklığa veya kesintiye uğramamış olup olmadığı hakkında bilgi verilecektir.
3. Komut istemine şunları yazın:
   
     `Start-HcsUpdate`
   
    Güncelleştirme işlemi başlayacaktır.

> [!IMPORTANT]
> * Bu komut yalnızca normal güncelleştirmeler için geçerlidir. Bu komutu yalnızca bir denetleyicide çalıştırırsınız, ancak her iki denetleyici de güncelleştirilir. 
> * Güncelleştirme işlemi sırasında denetleyicinin yük devretmesini fark edebilirsiniz; Ancak, yük devretme sistem kullanılabilirliğini veya işlemini etkilemez.
> 
> 

