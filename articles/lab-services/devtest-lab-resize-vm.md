---
title: Azure DevTest Labs laboratuvardaki bir VM 'yi yeniden boyutlandırma
description: Azure DevTest Labs bir sanal makinenin (VM) boyutunu CPU, ağ veya disk performansı gereksinimlerinize göre değiştirme hakkında bilgi edinin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76756186"
---
# <a name="resize-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvardaki bir VM 'yi yeniden boyutlandırma
Azure sanal makinelerinin önemli özelliklerinden biri, bir sanal makinenin (VM) boyutunu CPU, ağ veya disk performansı gereksinimlerinize göre değiştirmenize olanak sağlar. Azure DevTest Labs, şimdi bir laboratuvardaki VM 'Ler için bu özelliği destekler. Yeniden boyutlandır özelliği, laboratuvardaki izin verilen VM boyutları için laboratuvar ilkesine uyar. Diğer bir deyişle, bir sanal makinenin boyutunu laboratuvardaki yalnızca izin verilen boyutlarla değiştirebilirsiniz. 


## <a name="steps-to-resize-a-vm-in-a-lab"></a>Laboratuvarda bir VM 'yi yeniden boyutlandırma adımları 
Azure DevTest Labs bir laboratuvardaki bir sanal makineyi yeniden boyutlandırmak için aşağıdaki adımları uygulayın: 

> [!NOTE]
> VM 'ye Uzak Masaüstü oturumu (RDP) aracılığıyla bağlıysanız, çalışmanızı kaydedin ve yeniden boyutlandırmadan önce VM bağlantısını kesin.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden, yeniden boyutlandırmak istediğiniz VM 'yi içeren Laboratuvarı seçin.  
4. Sol bölmede **sanal makinelerim**' ı seçin. 
5. VM 'Ler listesinden bir VM seçin.
6. VM çalışıyorsa, araç çubuğunda **Durdur** ' u seçin. **Bildirimler** penceresinde işlemin durumunu kontrol edin. VM durduruluncaya kadar bekleyin ve ardından **Bildirimler** penceresini kapatın. 

    ![VM’yi durdurma](media/devtest-lab-resize-vm/stop-vm.png)
1. VM 'nizin sanal makine sayfasında, Sol menüdeki **Ayarlar** altında **Boyut** ' u seçin.

    ![Boyut menüsü](media/devtest-lab-resize-vm/size-menu.png)
1. **Boyut seçin** PENCERESINDE, VM 'niz için bir boyut seçin ve seçin ve **Seç**' e tıklayın.     
1. **Bildirimler** penceresindeki yeniden boyutlandırma işleminin durumunu denetleyin.

    ![Durumu yeniden boyutlandır](media/devtest-lab-resize-vm/resize-status.png)
10. Yeniden boyutlandırma işlemi başarılı olduktan sonra **Bildirimler** penceresini kapatın. 
11. Sol menüden **genel bakış** ' ı seçin ve sanal makineyi yeniden başlatmak için araç çubuğundan **Yeniden Başlat** ' ı seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Azure sanal makineleri tarafından desteklenen yeniden boyutlandırma özelliği hakkında ayrıntılı bilgi için bkz. [sanal makineleri yeniden boyutlandırma](https://azure.microsoft.com/blog/resize-virtual-machines/).


