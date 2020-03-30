---
title: Azure DevTest Labs'daki bir laboratuvarda bir laboratuvarı veya VM'yi silme
description: Bu makalede, Azure portalını (Azure DevTest Labs) kullanarak bir laboratuarı nasıl sildiğinizi veya bir laboratuvardaki VM'yi nasıl silebilirsiniz gösterilmektedir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: 28fb464714f464a4c0a8f5eaf304dcdd5d603c90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270816"
---
# <a name="delete-a-lab-or-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvarda bir laboratuvarı veya VM'yi silme
Bu makalede, bir laboratuarda bir laboratuar veya VM nasıl silinir gösterir.

## <a name="delete-a-lab"></a>Bir laboratuarı silme
Bir kaynak grubundan Bir DevTest Labs örneğini sildiğinizde, DevTest Labs hizmeti aşağıdaki işlemleri gerçekleştirir: 

- Laboratuvar oluşturulduğu sırada otomatik olarak oluşturulan tüm kaynaklar otomatik olarak silinir. Kaynak grubunun kendisi silinmez. Bu kaynak grubundaki kaynakları el ile oluşturduysanız, hizmet bunları silmez. 
- Laboratuvardaki tüm VM'ler ve bu VM'lerle ilişkili kaynak grupları otomatik olarak silinir. Bir laboratuvarda bir VM oluşturduğunuzda, hizmet ayrı bir kaynak grubunda VM için kaynaklar (disk, ağ arabirimi, genel IP adresi, vb.) oluşturur. Ancak, bu kaynak gruplarında el ile ek kaynak oluşturursanız, DevTest Labs hizmeti bu kaynakları ve kaynak grubunu silmez. 

Bir laboratuarı silmek için aşağıdaki işlemleri yapın: 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Soldaki menüden **Tüm kaynağı** seçin, hizmet türü için **DevTest Labs'ı** seçin ve laboratuarı seçin.

    ![Laboratuvarınızı seçin](media/devtest-lab-delete-lab-vm/select-lab.png)
3. **DevTest Lab** sayfasında, araç çubuğunda **Sil'i** tıklatın. 

    ![Sil düğmesi](media/devtest-lab-delete-lab-vm/delete-button.png)
4. **Onay** sayfasında, laboratuvarınızın **adını** girin ve **Sil'i**seçin. 

    ![Confirm](media/devtest-lab-delete-lab-vm/confirm-delete.png)
5. İşlemin durumunu görmek için **Bildirimler** simgesini (Bell) seçin. 

    ![Bildirimler](media/devtest-lab-delete-lab-vm/delete-status.png)

 
## <a name="delete-a-vm-in-a-lab"></a>Laboratuvarda VM silme
Bir laboratuarda bir VM silersem, laboratuvar oluşturulduğu sırada oluşturulan bazı kaynaklar (tümü değil) silinir. Aşağıdaki kaynaklar silinmez: 

-   Ana kaynak grubunda anahtar kasası
-   Kullanılabilirlik kümesi, yük dengeleyicisi, VM kaynak grubunda genel IP adresi. Bu kaynaklar, kaynak grubundaki birden çok VM tarafından paylaşılır. 

Sanal makine, ağ arabirimi ve VM ile ilişkili disk silinir. 

Bir laboratuvardaki Bir VM'yi silmek için aşağıdaki işlemleri yapın: 

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Soldaki menüden **Tüm kaynağı** seçin, hizmet türü için **DevTest Labs'ı** seçin ve laboratuarı seçin.

    ![Laboratuvarınızı seçin](media/devtest-lab-delete-lab-vm/select-lab.png)
3. Seçin **... (elips)** VM listesindeki VM için sil'i **seçin.** 

    ![Menüde VM silme](media/devtest-lab-delete-lab-vm/delete-vm-menu-in-list.png)
4. **Onay** iletişim kutusunda **Tamam'ı**seçin. 
5. İşlemin durumunu görmek için **Bildirimler** simgesini (Bell) seçin. 

**Sanal Makine sayfasından**bir VM'i silmek için, aşağıdaki resimde gösterildiği gibi araç çubuğundan **Sil'i** seçin:

![VM sayfasından VM silme](media/devtest-lab-delete-lab-vm/delete-from-vm-page.png) 


## <a name="next-steps"></a>Sonraki adımlar
Bir laboratuvar oluşturmak istiyorsanız, aşağıdaki makalelere bakın: 

- [Laboratuvar oluşturma](devtest-lab-create-lab.md)
- [Laboratuvara bir sanal makine ekleme](devtest-lab-add-vm.md)