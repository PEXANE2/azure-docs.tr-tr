---
title: Azure DevTest Labs bir laboratuvarda sanal makineyi yeniden başlatma | Microsoft Docs
description: Bu makalede, Azure DevTest Labs ' de sanal makineleri (VM) hızlı ve kolay bir şekilde yeniden başlatma adımları sağlanmaktadır.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76760003"
---
# <a name="restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs bir laboratuvarda sanal makineyi yeniden başlatma
Bu makaledeki adımları izleyerek DevTest Labs 'de bir sanal makineyi hızlı ve kolay bir şekilde yeniden başlatabilirsiniz. Bir VM 'yi yeniden başlatmadan önce aşağıdakileri göz önünde bulundurun:

- Yeniden başlatma özelliğinin etkinleştirilmesi için VM 'nin çalışıyor olması gerekir.
- Bir Kullanıcı yeniden başlatma gerçekleştirirken çalışan bir sanal makineye bağlıysa, yedekleme başladıktan sonra sanal makineye yeniden bağlanmaları gerekir.
- VM 'yi yeniden başlattığınızda bir yapıt uygulanmışsa, yapıtın uygulanamadığını belirten bir uyarı alırsınız.

    ![Yapıtlar uygulanırken yeniden başlatılırken uyarı oluştu](./media/devtest-lab-restart-vm/devtest-lab-restart-vm-apply-artifacts.png)


   > [!NOTE]
   > VM bir yapıt uygulanırken durdurulmuş ise, sorunu çözmek için olası bir yol olarak VM 'yi yeniden Başlat özelliğini kullanabilirsiniz.
   >
   >

## <a name="steps-to-restart-a-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs bir laboratuvarda sanal makineyi yeniden başlatma adımları
1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
1. Laboratuvarlar listesinden, yeniden başlatmak istediğiniz VM 'yi içeren Laboratuvarı seçin.
1. Sol bölmede **sanal makinelerim**' ı seçin.
1. VM 'Ler listesinden çalışan bir VM seçin.
1. VM yönetimi bölmesinin en üstünde **Yeniden Başlat**' ı seçin.

    ![VM 'yi yeniden Başlat düğmesi](./media/devtest-lab-restart-vm/devtest-lab-restart-vm.png)

1. Pencerenin sağ üst köşesindeki **Bildirimler** simgesini seçerek yeniden başlatmanın durumunu izleyin.

    ![VM yeniden başlatma durumunu görüntüleme](./media/devtest-lab-restart-vm/devtest-lab-restart-notification.png)

Çalışan bir VM 'yi, **sanal makinelerim**listesinde üç nokta (...) seçerek de yeniden başlatabilirsiniz.

![Üç nokta ile VM 'yi yeniden başlatma](./media/devtest-lab-restart-vm/devtest-lab-restart-elipses.png)

## <a name="next-steps"></a>Sonraki adımlar
* Yeniden başlatıldıktan sonra, Yönetim bölmesinde **Bağlan** ' ı seçerek VM 'ye yeniden bağlanabilirsiniz.
* [DevTest Labs Azure Resource Manager hızlı başlangıç şablonu galerisini](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) keşfet
