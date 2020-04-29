---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 21c19027d21a87e199d74644cfc5c8f3cd52ba4c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79128568"
---
Cihazınızı sıfırlamak için, veri diskine ve cihazınızın önyükleme diskine ait tüm verileri güvenli bir şekilde silmek gerekir. 

`Reset-HcsAppliance` Cmdlet 'ini kullanarak hem veri disklerini hem de önyükleme diskini veya yalnızca veri disklerini silebilirsiniz. `ClearData` Ve `BootDisk` anahtarları, veri disklerini ve önyükleme diskini sırasıyla temizlemenize olanak tanır.

`BootDisk` Anahtar, önyükleme diskini temizler ve cihazın kullanılamamasına neden olur. Yalnızca cihazın Microsoft 'a döndürülmesi gerektiğinde kullanılmalıdır. Daha fazla bilgi için bkz. [cihazı Microsoft 'A döndürme](https://docs.microsoft.com/azure/databox-online/data-box-edge-return-device).

Yerel Web Kullanıcı arabiriminde cihaz sıfırlamayı kullanırsanız, yalnızca veri diskleri güvenli bir şekilde temizlenir ancak önyükleme diski bozulmadan tutulur. Önyükleme diski, cihaz yapılandırmasını içerir.

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
