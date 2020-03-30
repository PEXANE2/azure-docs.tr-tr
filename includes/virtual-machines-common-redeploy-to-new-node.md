---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 1c3996c3f40da496af0cd795d0873864667a1f19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188376"
---
## <a name="use-the-azure-portal"></a>Azure portalı kullanma
1. Yeniden dağıtmak istediğiniz VM'yi seçin ve *ardından Ayarlar* bıçağındaki *Yeniden Dağıt* düğmesini seçin. Aşağıdaki örnekte olduğu gibi 'Yeniden Dağıt' düğmesini içeren **Destek ve Sorun Giderme** bölümünü görmek için aşağı kaydırmanız gerekebilir:
   
    ![Azure VM bıçak](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. İşlemi onaylamak için *Yeniden Dağıt* düğmesini seçin:
   
    ![VM bıçağını yeniden dağıtın](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. VM aşağıdaki örnekte gösterildiği gibi yeniden dağıtmaya hazırlanırken VM'nin **durumu** *Güncelleştirme'de* değişir:
   
    ![VM güncelleme](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. **Durum** daha sonra aşağıdaki örnekte gösterildiği gibi, vm yeni bir Azure ana bilgisayar üzerinde önyükleme olarak *Başlangıç* olarak değişir:
   
    ![VM başlangıç](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. VM önyükleme işlemini tamamladıktan **sonra, Durum** daha sonra *Çalıştırma'ya*geri döner ve VM'nin başarıyla yeniden dağıtıldığını gösterir:
   
    ![VM çalışıyor](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

