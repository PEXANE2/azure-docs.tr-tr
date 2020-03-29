---
author: cherylmc
ms.date: 12/06/2019
ms.service: expressroute
ms.topic: include
ms.author: cherylmc
ms.openlocfilehash: f04861d55c9cea3c79f4983f7be2e1f5a3c6d864
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74926236"
---
Azure Hizmet Yönetimi (SM) PowerShell modüllerinin ve ExpressRoute modülünün en son sürümlerini yükleyin. SM modüllerini çalıştırmak için Azure CloudShell ortamını kullanamazsınız.

1. Azure [Hizmet Yönetimi Modül'üne](/powershell/azure/servicemanagement/install-azure-ps) yüklemek için Hizmet Yönetimi modülü ne sini yükleyin makalesindeki yönergeleri kullanın. Az veya RM modülzaten yüklü varsa, '-AllowClobber' kullandığınızdan emin olun.
2. Yüklü modülleri içe aktarın. Aşağıdaki örneği kullanırken, yüklü PowerShell modüllerinizin konumunu ve sürümünü yansıtacak şekilde yolu ayarlayın.

   ```powershell
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\Azure.psd1'
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\ExpressRoute\ExpressRoute.psd1'
   ```
3. Azure hesabınızda oturum açmak için PowerShell konsolunuzu yüksek haklara sahip olarak açın ve hesabınıza bağlanın. Hizmet Yönetimi modülünü kullanarak bağlanmanıza yardımcı olmak için aşağıdaki örneği kullanın:

   ```powershell
   Add-AzureAccount
   ```