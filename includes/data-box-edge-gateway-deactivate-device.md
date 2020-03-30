---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 21c19027d21a87e199d74644cfc5c8f3cd52ba4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79128568"
---
Cihazınızı sıfırlamak için, veri diskindeki ve cihazınızın önyükleme diskindeki tüm verileri güvenli bir şekilde silmeniz gerekir. 

Hem `Reset-HcsAppliance` veri disklerini hem de önyükleme diskini veya yalnızca veri disklerini silmek için cmdlet'i kullanın. Ve `ClearData` `BootDisk` anahtarları sırasıyla veri diskleri ve önyükleme diski silmenize olanak sağlar.

Anahtar `BootDisk` önyükleme diskini siler ve aygıtı kullanılamaz hale getirir. Yalnızca aygıtın Microsoft'a döndürülmesi gerektiğinde kullanılmalıdır. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/databox-online/data-box-edge-return-device)

Aygıtı yerel web arama sürümünde sıfırlarsanız, yalnızca veri diskleri güvenli bir şekilde silinir, ancak önyükleme diski bozulmadan tutulur. Önyükleme diski aygıt yapılandırmasını içerir.

1. [PowerShell arabirimine bağlanın.](#connect-to-the-powershell-interface)
2. Komut istemine şunları yazın:

    `Reset-HcsAppliance -ClearData -BootDisk`

    Aşağıdaki örnek, bu cmdlet'in nasıl kullanılacağını gösterir:

    ```powershell
    [10.128.24.33]: PS>Reset-HcsAppliance -ClearData -BootDisk

    Confirm
    Are you sure you want to perform this action?
    Performing the operation "Reset-HcsAppliance" on target "ShouldProcess appliance".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"): N
    ```
