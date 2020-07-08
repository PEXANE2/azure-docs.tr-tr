---
title: Azure DevTest Labs bir laboratuvarda sanal makineyi yeniden başlatma | Microsoft Docs
description: Bu makalede, Azure DevTest Labs ' de sanal makineleri (VM) hızlı ve kolay bir şekilde yeniden başlatma adımları sağlanmaktadır.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: d2bcbcbea613aa84da91789fb9f118afd7023fab
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482004"
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
1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
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
