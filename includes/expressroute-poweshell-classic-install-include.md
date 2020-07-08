---
author: cherylmc
ms.date: 12/06/2019
ms.service: expressroute
ms.topic: include
ms.author: cherylmc
ms.openlocfilehash: f04861d55c9cea3c79f4983f7be2e1f5a3c6d864
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74926236"
---
Azure hizmet yönetimi (SM) PowerShell modüllerinin ve ExpressRoute modülünün en son sürümlerini yükler. SM modüllerini çalıştırmak için Azure CloudShell ortamını kullanamazsınız.

1. Azure hizmet yönetimi modülünü yüklemek için [hizmet yönetimi modülünü yükleme](/powershell/azure/servicemanagement/install-azure-ps) makalesindeki yönergeleri kullanın. Az veya RM modülü zaten yüklüyse, '-AllowClobber ' kullandığınızdan emin olun.
2. Yüklü modülleri içeri aktarın. Aşağıdaki örneği kullanırken yolu, yüklü PowerShell modüllerinizin konumunu ve sürümünü yansıtacak şekilde ayarlayın.

   ```powershell
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\Azure.psd1'
   Import-Module 'C:\Program Files\WindowsPowerShell\Modules\Azure\5.3.0\ExpressRoute\ExpressRoute.psd1'
   ```
3. Azure hesabınızda oturum açmak için, PowerShell konsolunuzu yükseltilmiş haklarla açın ve hesabınıza bağlanın. Hizmet yönetimi modülünü kullanarak bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

   ```powershell
   Add-AzureAccount
   ```