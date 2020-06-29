---
title: Azure DevTest Labs bir laboratuvarda sanal makineyi yeniden dağıtma | Microsoft Docs
description: Azure DevTest Labs bir sanal makineyi yeniden dağıtmayı öğrenin (bir Azure düğümünden diğerine geçiş).
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: da0edf13adaa0d7ecd84ee2c190f376c19b398db
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85480244"
---
# <a name="redeploy-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs bir laboratuvarda sanal makineyi yeniden dağıtma
Bir laboratuvarda Uzak Masaüstü bağlantısı aracılığıyla bir sanal makineye (VM) bağlanamıyorsanız, VM 'yi yeniden dağıtın ve tekrar bağlanmayı deneyin. Bir VM 'yi yeniden dağıtırken, DevTest Labs VM 'yi çalıştıran düğümden Azure altyapısı içindeki yeni bir düğüme taşıdır. Daha sonra tüm yapılandırma seçeneklerinizi ve ilişkili kaynaklarınızı korurken VM 'yi başlatır. Bu özellik, uzak masaüstü bağlantınızın giderilmesi veya laboratuvardaki Windows tabanlı VM 'lere erişim için harcanan süreyi kaydeder. 

## <a name="steps-to-redeploy-a-vm-in-a-lab"></a>Laboratuvarda bir VM 'yi yeniden dağıtma adımları 
Azure DevTest Labs bir laboratuvarda bir sanal makineyi yeniden dağıtmak için aşağıdaki adımları uygulayın: 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden, yeniden dağıtmak istediğiniz VM 'yi içeren Laboratuvarı seçin.  
4. Sol bölmede **sanal makinelerim**' ı seçin. 
5. VM 'Ler listesinden bir VM seçin.
6. VM 'nizin sanal makine sayfasında sol menüdeki **işlemler** ' in altında yeniden **Dağıt** ' ı seçin.

    ![Yeniden dağıtım](media/devtest-lab-redeploy-vm/redeploy.png)
7. Sayfadaki bilgileri okuyun ve yeniden **Dağıt** düğmesini seçin. 9. **Bildirimler** penceresindeki yeniden dağıtma işleminin durumunu denetleyin.

    ![Yeniden dağıtım durumu](media/devtest-lab-redeploy-vm/redeploy-status.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure DevTest Labs bir sanal makineyi yeniden boyutlandırmayı öğrenin, bkz. [bir VM 'Yi yeniden boyutlandırma](devtest-lab-resize-vm.md).


