---
title: Azure DevTest Labs'daki bir laboratuvarda VM'yi yeniden boyutlandırma
description: Cpu, ağ veya disk performansı gereksinimlerinize bağlı olarak Azure DevTest Labs'da sanal makinenin (VM) boyutunu nasıl değiştireceğinizi öğrenin.
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
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: bf7c425766a97aaa1d143133f04502a0aa3c36cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76756186"
---
# <a name="resize-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvarda VM'yi yeniden boyutlandırma
Azure sanal makinelerinin önemli özelliklerinden biri, CPU, ağ veya disk performansı gereksinimlerinize göre sanal makine (VM) boyutunu değiştirmenize olanak tanır. Azure DevTest Labs artık bir laboratuvarda VM'ler için bu özelliği destekler. Yeniden boyutlandırma özelliği, laboratuvarda izin verilen VM boyutları için laboratuvar ilkesine uyar. Diğer bir deyişle, bir VM'nin boyutunu yalnızca laboratuarda izin verilen boyutlarla değiştirebilirsiniz. 


## <a name="steps-to-resize-a-vm-in-a-lab"></a>Laboratuvarda VM'yi yeniden boyutlandırma adımları 
Azure DevTest Labs'daki bir laboratuvarda bir VM'yi yeniden boyutlandırmak için aşağıdaki adımları izleyin: 

> [!NOTE]
> Uzak bir masaüstü oturumu (RDP) aracılığıyla VM'ye bağlıysanız, çalışmanızı kaydedin ve yeniden boyutlandırmadan önce VM ile bağlantısını kesin.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden, yeniden boyutlandırmak istediğiniz VM'yi içeren laboratuarı seçin.  
4. Sol panelde **Sanal Makinelerim'i**seçin. 
5. VM'ler listesinden bir VM seçin.
6. VM çalışıyorsa araç çubuğunda **Durdur'u** seçin. **Bildirimler** penceresinde ki işlemin durumunu denetleyin. VM durdurulana kadar bekleyin ve ardından **Bildirimler** penceresini kapatın. 

    ![VM’yi durdurma](media/devtest-lab-resize-vm/stop-vm.png)
1. VM'niz için Sanal Makine sayfasında sol menüde **AYARLAR** altında **Boyut'u** seçin.

    ![Boyut menüsü](media/devtest-lab-resize-vm/size-menu.png)
1. Boyut **seç** penceresinde, VM'niz için bir boyuta göz atın ve seçin ve **Seç'i**tıklatın.     
1. **Bildirimler** penceresinde yeniden boyutlandırma işleminin durumunu denetleyin.

    ![Durumu yeniden boyutlandırma](media/devtest-lab-resize-vm/resize-status.png)
10. Yeniden boyutlandırma işlemi başarılı olduktan sonra **Bildirimler** penceresini kapatın. 
11. Sol menüde **Genel Bakış'ı** seçin ve VM'yi yeniden başlatmak için araç çubuğunda **Yeniden Başlat'ı** seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Azure sanal makineleri tarafından desteklenen yeniden boyutlandırma özelliği hakkında ayrıntılı bilgi [için](https://azure.microsoft.com/blog/resize-virtual-machines/)bkz.


