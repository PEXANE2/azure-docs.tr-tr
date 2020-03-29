---
title: Azure DevTest Labs'daki bir laboratuvarda VM'yi yeniden dağıtma | Microsoft Dokümanlar
description: Azure DevTest Labs'da sanal bir makineyi yeniden dağıtmayı (bir Azure düğümünden diğerine taşıma) öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60561633"
---
# <a name="redeploy-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvarda VM'yi yeniden dağıtma
Uzak bir masaüstü bağlantısı üzerinden bir laboratuvardaki sanal makineye (VM) bağlanamıyorsanız, VM'yi yeniden dağıtın ve yeniden bağlanmayı deneyin. Bir VM'yi yeniden dağıttığınızda, DevTest Labs VM'yi üzerinde çalışan düğümden Azure altyapısı nda yeni bir düğüme taşır. Daha sonra tüm yapılandırma seçenekleri ve ilişkili kaynakları korurken VM başlar. Bu özellik, uzak masaüstü bağlantınızı veya uygulama erişiminizi laboratuarda Windows tabanlı VM'lere gidermek için harcadığınız zamandan tasarruf etmenizi sağlar. 

## <a name="steps-to-redeploy-a-vm-in-a-lab"></a>Bir VM'yi laboratuvarda yeniden dağıtma adımları 
Bir VM'yi Azure DevTest Labs'daki bir laboratuvarda yeniden dağıtmak için aşağıdaki adımları izleyin: 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden, yeniden dağıtmak istediğiniz VM'yi içeren laboratuarı seçin.  
4. Sol panelde **Sanal Makinelerim'i**seçin. 
5. VM'ler listesinden bir VM seçin.
6. VM'niz in Sanal Makine sayfasında, sol menüde **OPERATIONS** altında **Yeniden Dağıt'ı** seçin.

    ![Yeniden dağıtım](media/devtest-lab-redeploy-vm/redeploy.png)
7. Sayfadaki bilgileri okuyun ve **Yeniden Dağıt düğmesini** seçin. 9. **Bildirimler** penceresinde yeniden dağıtma işleminin durumunu denetleyin.

    ![Durumu yeniden dağıtma](media/devtest-lab-redeploy-vm/redeploy-status.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure DevTest Labs'da bir VM'i nasıl yeniden [boyutlandırıracağız](devtest-lab-resize-vm.md)öğrenin, bkz.


