---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 1c3996c3f40da496af0cd795d0873864667a1f19
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188376"
---
## <a name="use-the-azure-portal"></a>Azure portalı kullanma
1. Yeniden dağıtmak istediğiniz VM 'yi seçin ve ardından *Ayarlar* dikey penceresinde yeniden *Dağıt* düğmesini seçin. Aşağıdaki örnekte gösterildiği gibi, ' yeniden Dağıt ' düğmesini içeren **destek ve sorun giderme** bölümünü görmek için aşağı kaydırmanız gerekebilir:
   
    ![Azure VM dikey penceresi](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. İşlemi onaylamak için yeniden *Dağıt* düğmesini seçin:
   
    ![VM dikey penceresini yeniden dağıtma](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. Aşağıdaki örnekte gösterildiği gibi, VM yeniden dağıtmaya hazırlanırken **VM 'Nin** *güncelleştirilmesi* için değişiklikler:
   
    ![VM güncelleştirme](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. **Durum** daha sonra, aşağıdaki örnekte gösterildiği gıbı, VM yeni bir Azure ana bilgisayarında önyüklendiği için *başlayacak* şekilde değişir:
   
    ![VM başlatılıyor](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. VM, önyükleme işlemini tamamladıktan sonra, sanal makinenin başarıyla yeniden dağıtıldığını belirten durum *çalışır* **duruma** döner:
   
    ![VM çalışıyor](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

