---
title: Azure DevTest Labs laboratuarda laboratuvar veya VM silme
description: Bu makalede, Azure DevTest Labs Azure portal kullanarak laboratuvarda bir laboratuvarı silme veya bir VM silme işlemlerinin nasıl yapılacağı gösterilir.
ms.topic: article
ms.date: 01/24/2020
ms.openlocfilehash: 495fb98f3da41a47d316dd64554ba616ede0af47
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85481230"
---
# <a name="delete-a-lab-or-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuarda laboratuvar veya VM silme
Bu makalede, laboratuvarda bir laboratuvarın veya VM 'nin nasıl silineceği gösterilir.

## <a name="delete-a-lab"></a>Laboratuvarı silme
Bir kaynak grubundan bir DevTest Labs örneğini sildiğinizde, DevTest Labs hizmeti aşağıdaki eylemleri gerçekleştirir: 

- Laboratuvar oluşturma sırasında otomatik olarak oluşturulan tüm kaynaklar otomatik olarak silinir. Kaynak grubunun kendisi silinmez. Bu kaynak grubundaki kaynakları el ile oluşturduysanız, hizmet onları silmez. 
- Laboratuvar ve bu VM 'lerle ilişkili kaynak gruplarındaki tüm VM 'Ler otomatik olarak silinir. Laboratuvarda bir sanal makine oluşturduğunuzda, hizmet ayrı bir kaynak grubundaki VM için kaynaklar (disk, ağ arabirimi, genel IP adresi vb.) oluşturur. Ancak, bu kaynak gruplarında el ile ek kaynaklar oluşturursanız, DevTest Labs hizmeti bu kaynakları ve kaynak grubunu silmez. 

Bir laboratuvarı silmek için aşağıdaki işlemleri yapın: 

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Sol taraftaki menüden **tüm kaynak** ' ı seçin, hizmet türü Için **DevTest Labs** ' i seçin ve Laboratuvarı seçin.

    ![Laboratuvarınızı seçin](media/devtest-lab-delete-lab-vm/select-lab.png)
3. **DevTest Lab** sayfasında, araç çubuğunda **Sil** ' e tıklayın. 

    ![Sil düğmesi](media/devtest-lab-delete-lab-vm/delete-button.png)
4. **Onay** sayfasında, laboratuvarınızın **adını** girin ve **Sil**' i seçin. 

    ![Confirm](media/devtest-lab-delete-lab-vm/confirm-delete.png)
5. İşlemin durumunu görmek için **Bildirimler** simgesini (zil) seçin. 

    ![Bildirimler](media/devtest-lab-delete-lab-vm/delete-status.png)

 
## <a name="delete-a-vm-in-a-lab"></a>Laboratuvarda VM silme
Laboratuvardaki bir VM 'yi silersem, Laboratuvar oluşturma sırasında oluşturulan kaynaklardan bazıları (tümü değil) silinir. Aşağıdaki kaynaklar silinmez: 

-   Ana kaynak grubundaki Anahtar Kasası
-   VM kaynak grubundaki kullanılabilirlik kümesi, yük dengeleyici, genel IP adresi. Bu kaynaklar bir kaynak grubundaki birden çok VM tarafından paylaşılır. 

Sanal makine, ağ arabirimi ve VM ile ilişkili disk silinir. 

Laboratuvara bir sanal makineyi silmek için aşağıdaki işlemleri yapın: 

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Sol taraftaki menüden **tüm kaynak** ' ı seçin, hizmet türü Için **DevTest Labs** ' i seçin ve Laboratuvarı seçin.

    ![Laboratuvarınızı seçin](media/devtest-lab-delete-lab-vm/select-lab.png)
3. Seç **... (üç nokta)** sanal makine listesindeki VM için **Sil**' i seçin. 

    ![Menüdeki VM 'yi Sil](media/devtest-lab-delete-lab-vm/delete-vm-menu-in-list.png)
4. **Onay** Iletişim kutusunda **Tamam**' ı seçin. 
5. İşlemin durumunu görmek için **Bildirimler** simgesini (zil) seçin. 

**Sanal makine sayfasından**bir VM 'yi silmek için, aşağıdaki görüntüde gösterildiği gibi araç çubuğundan **Sil** ' i seçin:

![VM 'yi VM 'den silme sayfası](media/devtest-lab-delete-lab-vm/delete-from-vm-page.png) 


## <a name="next-steps"></a>Sonraki adımlar
Laboratuvar oluşturmak istiyorsanız aşağıdaki makalelere bakın: 

- [Laboratuvar oluşturma](devtest-lab-create-lab.md)
- [Laboratuvara bir sanal makine ekleme](devtest-lab-add-vm.md)