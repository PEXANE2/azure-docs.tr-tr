---
title: Azure DevTest Labs'de laboratuvara erişme | Microsoft Docs
description: Bu öğreticide, Azure DevTest Labs kullanılarak oluşturulan laboratuvara erişir, sanal makineler talep eder, sanal makineleri kullanır ve sanal makine talebini geri alırsınız.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/18/2019
ms.author: spelluru
ms.openlocfilehash: ee9a68df685095244fc9471b7d4ab0f6cee0642d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68360268"
---
# <a name="tutorial-access-a-lab-in-azure-devtest-labs"></a>Öğretici: Azure DevTest Labs’de bir laboratuvara erişme
Bu öğreticide, [Öğretici: Azure DevTest Labs’de laboratuvar oluşturma](tutorial-create-custom-lab.md) başlıklı öğreticide oluşturulan laboratuvarı kullanırsınız.

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvarda sanal makine (VM) talep etme
> * VM’ye bağlanma
> * Sanal makine talebini geri alma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="access-the-lab"></a>Laboratuvara erişme

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Soldaki menüden **Tüm kaynaklar**’ı seçin. 
3. Kaynak türü için **DevTest Labs**’i seçin. 
4. Laboratuvarı seçin. 

    ![Laboratuvarı seçme](./media/tutorial-use-custom-lab/search-for-select-custom-lab.png)

## <a name="claim-a-vm"></a>Sanal makine talep etme

1. **Talep edilebilir sanal makineler** listesinde **...** (üç nokta) ve sonra **Makineyi talep et**’i seçin.

    ![Sanal makineyi talep etme](./media/tutorial-use-custom-lab/claim-virtual-machine.png)
1. **Sanal makinelerim** listesinde sanal makineyi gördüğünüzü onaylayın.

    ![Sanal makinem](./media/tutorial-use-custom-lab/my-virtual-machines.png)

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

1. Listeden sanal makinenizi seçin. Sanal makineniz için **Sanal Makine sayfasını** görürsünüz. Araç çubuğunda **Bağlan**’ı seçin.

    ![Sanal makineye bağlanma](./media/tutorial-use-custom-lab/connect-button.png)
2. İndirilen **RDP** dosyasını sabit diskinize kaydedin ve sanal makineye bağlanmak için bunu kullanın. Önceki bölümde sanal makine oluşturulurken belirttiğiniz kullanıcı adını ve parolayı belirtin. 

    Bir Linux VM'ye bağlanmak için VM’nin SSH ve/veya RDP erişiminin etkinleştirilmesi gerekir. RDP üzerinden Linux VM’ye bağlanma adımları için bkz. [Azure’da Linux VM’ye bağlanmak için Uzak Masaüstü’nü yükleme ve yapılandırma](../virtual-machines/linux/use-remote-desktop.md). 

    > [!NOTE]
    > VM'iniz için Sanal Makine sayfasına girmenin başka yolları da vardır. Bunlardan bazıları şunlardır: 
    > 
    > 1. Aboneliğinizdeki tüm VM'leri arayın. **Sanal Makine** sayfasına ulaşmak için VM'ler listesinde VM'nizi seçin.
    > 2. Kaynak grubu için **Kaynak Grubu** sayfasına gidin. Ardından, **Sanal Makine** sayfasına ulaşmak için kaynak grubundaki kaynaklar listesinden VM'nizi seçin. 
    >
    > Bu seçenekleri kullanarak aldığınız **Sanal Makine** sayfasındaki araç çubuğundaki **Bağlan** düğmesini kullanmayın. Bunun yerine, bu makalede gösterildiği gibi **DevTest Labs** sayfasından **Sanal Makine** sayfasına gidin ve ardından araç çubuğundaki **Bağlan** düğmesini kullanın.


## <a name="unclaim-the-vm"></a>Sanal makine talebini geri alma
Sanal makineyi kullandıktan sonra şu adımları izleyerek sanal makine talebini geri alın: 

1. Sanal makine sayfasında, araç çubuğundan **Talebi Geri Al**’ı seçin. 

    ![Sanal makine talebini geri alma](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. Sanal makine talebi geri alınmadan önce sanal makine kapatılır. Bildirimlerde bu işlemin durumunu görebilirsiniz.  
3. Üstteki ekmek kırıntısı menüsünde laboratuvar adınızı tıklatarak DevTest Lab sayfasına geri gidin. 
    
    ![Laboratuvara geri dön](./media/tutorial-use-custom-lab/breadcrumb-to-lab.png)
1. VM'yi en alttaki **Talep Edilebilir sanal makineler** listesinde gördüğünüzü doğrulayın.

    
## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure DevTest Labs kullanılarak oluşturulan bir laboratuvara nasıl erişeceğiniz ve bu laboratuvarı nasıl kullanacağınız gösterildi. Laboratuvardaki sanal makinelere erişme ve bu sanal makineleri kullanma hakkında daha fazla bilgi için bkz. 

> [!div class="nextstepaction"]
> [Nasıl yapılır: Laboratuvardaki sanal makineleri kullanma](devtest-lab-add-vm.md)

