---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: c1b56cfb85595b8a17dc18f69a0b162d504c04ec
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2020
ms.locfileid: "95560343"
---
Cihazınızı sıfırlamak için veri diskinde ve cihazınızın önyükleme diskinde bulunan tüm verileri güvenli bir şekilde silmeniz gerekir. 

`Reset-HcsAppliance`Cmdlet 'ini kullanarak hem veri disklerini hem de önyükleme diskini veya yalnızca veri disklerini silebilirsiniz. `ClearData`Ve `BootDisk` anahtarları, veri disklerini ve önyükleme diskini sırasıyla temizlemenize olanak tanır.

`BootDisk`Anahtar, önyükleme diskini temizler ve cihazın kullanılamamasına neden olur. Yalnızca cihazın Microsoft’a iade edilmesi gerektiğinde kullanılması gerekir. Daha fazla bilgi için bkz. [cihazı Microsoft 'A döndürme](../articles/databox-online/azure-stack-edge-return-device.md).

Yerel web kullanıcı arabiriminde cihaz sıfırlama özelliğini kullanırsanız, yalnızca veri diskleri güvenli bir şekilde silinir ve önyükleme diskinde işlem yapılmaz. Önyükleme diski cihaz yapılandırmasını içerir.

1. [PowerShell arabirimine bağlanın](#connect-to-the-powershell-interface).
2. Komut istemine şunları yazın:

    `Reset-HcsAppliance -ClearData -BootDisk`

    Aşağıdaki örnek, bu cmdlet 'in nasıl kullanılacağını gösterir:

    ```powershell
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk

    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```