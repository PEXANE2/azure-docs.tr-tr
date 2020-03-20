---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 21c19027d21a87e199d74644cfc5c8f3cd52ba4c
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128568"
---
Cihazınızı sıfırlamak için veri diskinde ve cihazınızın önyükleme diskinde bulunan tüm verileri güvenli bir şekilde silmeniz gerekir. 

Hem veri disklerini ve önyükleme diskini veya yalnızca veri disklerini silmek için `Reset-HcsAppliance` cmdlet’ini kullanın. `ClearData` ve `BootDisk` anahtarları (sırasıyla) veri disklerini ve önyükleme diskini silmenize olanak tanır.

`BootDisk` anahtarı önyükleme diskini siler ve cihazı kullanılamaz hale getirir. Yalnızca cihazın Microsoft’a iade edilmesi gerektiğinde kullanılması gerekir. Daha fazla bilgi için bkz. [Cihazı Microsoft’a iade etme](https://docs.microsoft.com/azure/databox-online/data-box-edge-return-device).

Yerel web kullanıcı arabiriminde cihaz sıfırlama özelliğini kullanırsanız, yalnızca veri diskleri güvenli bir şekilde silinir ve önyükleme diskinde işlem yapılmaz. Önyükleme diski cihaz yapılandırmasını içerir.

1. [PowerShell arabirimine bağlanma](#connect-to-the-powershell-interface).
2. Komut istemine şunları yazın:

    `Reset-HcsAppliance -ClearData -BootDisk`

    Aşağıdaki örnek bu cmdlet’in nasıl kullanıldığını gösterir:

    ```powershell
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk

    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```
