---
title: Azure DevTest Labs'daki bir laboratuvarda VM'yi yeniden başlatın | Microsoft Dokümanlar
description: Bu makalede, Azure DevTest Labs'da sanal makineleri (VM) hızlı ve kolay bir şekilde yeniden başlatma adımları sağlanmaktadır.
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
ms.openlocfilehash: 52d3b92909483a99eb82c86b727261bbeb5f8d46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76760003"
---
# <a name="restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvarda VM'yi yeniden başlatma
Bu makaledeki adımları izleyerek DevTest Labs'da sanal bir makineyi hızlı ve kolay bir şekilde yeniden başlatabilirsiniz. VM'yi yeniden başlatmadan önce aşağıdakileri göz önünde bulundurun:

- Yeniden başlatma özelliğinin etkinolabilmesi için VM'nin çalışıyor olması gerekir.
- Bir kullanıcı yeniden başlatma gerçekleştirdiğinde çalışan bir VM'ye bağlıysa, yeniden başlatıldıktan sonra VM'ye yeniden bağlanması gerekir.
- VM'yi yeniden başlattığınızda bir yapı uygulanıyorsa, yapının uygulanamayabileceği ne kadar uyarı alırsınız.

    ![Yapıları uygularken yeniden başlatırken uyarı](./media/devtest-lab-restart-vm/devtest-lab-restart-vm-apply-artifacts.png)


   > [!NOTE]
   > Bir yapı yı uygularken VM durduysa, sorunu çözmek için olası bir yol olarak VM'yi yeniden başlat özelliğini kullanabilirsiniz.
   >
   >

## <a name="steps-to-restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvarda VM'yi yeniden başlatma adımları
1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
1. Laboratuvarlar listesinden, yeniden başlatmak istediğiniz VM'yi içeren laboratuarı seçin.
1. Sol panelde **Sanal Makinelerim'i**seçin.
1. VM'ler listesinden çalışan bir VM seçin.
1. VM yönetim bölmesinin üst kısmında **Yeniden Başlat'ı**seçin.

    ![VM düğmesini yeniden başlat](./media/devtest-lab-restart-vm/devtest-lab-restart-vm.png)

1. Pencerenin sağ üst kısmındaki **Bildirimler** simgesini seçerek yeniden başlatma durumunu izleyin.

    ![VM yeniden başlatma durumunu görüntüleme](./media/devtest-lab-restart-vm/devtest-lab-restart-notification.png)

Ayrıca, **Sanal Makinelerim**listesinde elipslerini (...) seçerek çalışan bir VM'yi yeniden başlatabilirsiniz.

![VM'yi elipslerle yeniden başlatın](./media/devtest-lab-restart-vm/devtest-lab-restart-elipses.png)

## <a name="next-steps"></a>Sonraki adımlar
* Yeniden başlatıldıktan sonra, yönetim bölmesindeki **Bağlan'ı** seçerek VM'ye yeniden bağlanabilirsiniz.
* [DevTest Labs Azure Kaynak Yöneticisi hızlıBaşlat şablon galerisini](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) keşfedin
