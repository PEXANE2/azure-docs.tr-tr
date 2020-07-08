---
title: Azure DevTest Labs laboratuvardaki bir VM 'yi yeniden boyutlandırma
description: Azure DevTest Labs bir sanal makinenin (VM) boyutunu CPU, ağ veya disk performansı gereksinimlerinize göre değiştirme hakkında bilgi edinin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 592be0862bc33ead86fe43d37753fb72fde120c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482029"
---
# <a name="resize-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvardaki bir VM 'yi yeniden boyutlandırma
Azure sanal makinelerinin önemli özelliklerinden biri, bir sanal makinenin (VM) boyutunu CPU, ağ veya disk performansı gereksinimlerinize göre değiştirmenize olanak sağlar. Azure DevTest Labs, şimdi bir laboratuvardaki VM 'Ler için bu özelliği destekler. Yeniden boyutlandır özelliği, laboratuvardaki izin verilen VM boyutları için laboratuvar ilkesine uyar. Diğer bir deyişle, bir sanal makinenin boyutunu laboratuvardaki yalnızca izin verilen boyutlarla değiştirebilirsiniz. 


## <a name="steps-to-resize-a-vm-in-a-lab"></a>Laboratuvarda bir VM 'yi yeniden boyutlandırma adımları 
Azure DevTest Labs bir laboratuvardaki bir sanal makineyi yeniden boyutlandırmak için aşağıdaki adımları uygulayın: 

> [!NOTE]
> VM 'ye Uzak Masaüstü oturumu (RDP) aracılığıyla bağlıysanız, çalışmanızı kaydedin ve yeniden boyutlandırmadan önce VM bağlantısını kesin.

1. [Azure portalında](https://portal.azure.com) oturum açın.
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


