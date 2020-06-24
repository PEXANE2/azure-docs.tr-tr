---
title: Azure DevTest Labs bir laboratuvarda sanal makineyi yeniden dağıtma | Microsoft Docs
description: Azure DevTest Labs bir sanal makineyi yeniden dağıtmayı öğrenin (bir Azure düğümünden diğerine geçiş).
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: spelluru
ms.openlocfilehash: 4500fcfa6fbfb346a6e5c7fd045ba0046a901b91
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84898727"
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


