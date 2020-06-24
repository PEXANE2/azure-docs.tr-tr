---
title: Azure DevTest Labs bir sanal makineye bir veri diski iliştirme veya ayırma
description: Azure DevTest Labs bir sanal makineye bir veri diski eklemeyi veya kullanımdan çıkarmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 9616bf38-7db8-4915-a32a-e4f40a7a56ad
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2020
ms.author: spelluru
ms.openlocfilehash: 3f18425408e6526904db85eae1c3a4db41d11a58
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84896588"
---
# <a name="attach-or-detach-a-data-disk-to-a-virtual-machine-in-azure-devtest-labs"></a>Azure DevTest Labs bir sanal makineye bir veri diski iliştirme veya ayırma
[Azure yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) , sanal makine veri diskleriyle ilişkili depolama hesaplarını yönetir. Bir Kullanıcı bir VM 'ye yeni bir veri diski iliştirir, gereken disk türünü ve boyutunu belirtir ve Azure, diski otomatik olarak oluşturur ve yönetir. Veri diski, VM 'den ayrılabilir ve daha sonra aynı VM 'ye yeniden eklenebilir ya da aynı kullanıcıya ait farklı bir VM 'ye iliştirilebilir.

Bu işlevsellik, her bir sanal makinenin dışında depolama veya yazılım yönetimi için kullanışlıdır. Depolama veya yazılım bir veri diskinde zaten mevcutsa, bu veri diskine sahip olan kullanıcı tarafından sahip olunan herhangi bir VM 'ye kolayca eklenebilir, ayrılabilir ve yeniden eklenebilir.

## <a name="attach-a-data-disk"></a>Veri diski ekleme
Bir sanal makineye bir veri diski iliştirmadan önce şu ipuçlarını gözden geçirin:

- VM 'nin boyutu, ekleyebileceğiniz veri disklerinin sayısını denetler. Ayrıntılar için bkz. [sanal makineler Için boyutlar](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).
- Yalnızca çalıştıran bir VM 'ye veri diski ekleyebilirsiniz. Bir veri diski eklemeyi denemeden önce VM 'nin çalıştığından emin olun.

### <a name="attach-a-new-disk"></a>Yeni Disk Ekle
Azure DevTest Labs ' de bir VM 'ye yeni bir yönetilen veri diski oluşturup eklemek için aşağıdaki adımları izleyin.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
1. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin. 
1. **Sanal makinelerimin**listesinden çalışan bir VM seçin.
1. Soldaki menüden **diskler**' i seçin.
1. Yeni bir veri diski oluşturmak ve VM 'ye eklemek için **Yeni Ekle** ' yi seçin.

    ![Yeni veri diskini bir sanal makineye Ekle](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new.png)
1. Bir veri diski adı, türü ve boyutu girerek **Yeni Disk Ekle** bölmesini doldurun.

    !["Yeni disk Ekle" formunu doldurun](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new-form.png)
1. **Tamam**’ı seçin.

Birkaç dakika sonra, yeni veri diski oluşturulur ve VM 'ye iliştirilir ve bu VM için **veri diskleri** listesinde görünür.

### <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme
Mevcut bir veri diskinin çalışan bir VM 'ye yeniden iliştirmeniz için aşağıdaki adımları izleyin. 

1. Bir veri diskini yeniden iliştirmek istediğiniz çalışan bir VM seçin.
1. Soldaki menüden **diskler**' i seçin.
1. VM 'ye kullanılabilir bir veri diski eklemek için **Varolanı Ekle** ' yi seçin.

    ![Var olan veri diskini bir sanal makineye Ekle](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing-button.png)

1. **Varolan diski Ekle** bölmesinde Tamam ' ı seçin.

    ![Var olan veri diskini bir sanal makineye Ekle](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing.png)

Birkaç dakika sonra, veri diski sanal makineye iliştirilir ve bu VM 'nin **veri diskleri** listesinde görünür.

## <a name="detach-a-data-disk"></a>Veri diski çıkarma
Artık bir VM 'ye bağlı bir veri diskine ihtiyacınız kalmadığında, kolayca ayırabilirsiniz. Ayırma, diski sanal makineden kaldırır, ancak daha sonra kullanmak üzere depolama alanında tutar.

Diskte var olan verileri yeniden kullanmak istiyorsanız, onu aynı sanal makineye veya başka bir bilgisayara yeniden ekleyebilirsiniz.

### <a name="detach-from-the-vms-management-pane"></a>VM 'nin Yönetim bölmesinden ayır
1. Sanal makineler listenizden, bağlı bir veri diskine sahip bir VM seçin.
1. Soldaki menüden **diskler**' i seçin.
1. **Veri diskleri**listesinden, ayırmak istediğiniz veri diskini seçin.

    ![Bir sanal makine için veri disklerini seçin](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-button.png) 
1. Diskin ayrıntılar bölmesinin en üstünden **Ayır** ' ı seçin.

    ![Veri diski çıkarma](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk2.png)
1. Veri diskini ayırmak istediğinizi onaylamak için **Evet** ' i seçin.

Disk ayrılır ve başka bir VM 'ye iliştirilemiyor. 
### <a name="detach-from-the-labs-main-pane"></a>Laboratuvarın ana bölmesinden ayır
1. Laboratuvarınızın ana bölmesinde **veri disklerim**' i seçin.
1. Ayırmak istediğiniz veri diskine sağ tıklayın veya üç nokta (**...**) simgesini seçip **Ayır**' ı seçin.

    ![Veri diski çıkarma](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk.png)
1. Ayırmak istediğinizi onaylamak için **Evet** ' i seçin.

   > [!NOTE]
   > Bir veri diski zaten ayrılmışsa, **Sil**' i seçerek onu kullanılabilir veri diskleri listenizden kaldırmayı seçebilirsiniz.
   >
   >

## <a name="upgrade-an-unmanaged-data-disk"></a>Yönetilmeyen bir veri diskini yükseltme
Yönetilmeyen veri disklerini kullanan mevcut bir VM varsa, VM 'yi yönetilen diskleri kullanacak şekilde kolayca dönüştürebilirsiniz. Bu işlem hem işletim sistemi diskini hem de bağlı veri disklerini dönüştürür.

Yönetilmeyen bir veri diskini yükseltmek için, bu makalede açıklanan adımları izleyerek [veri diskini](#detach-a-data-disk) YÖNETILMEYEN bir VM 'den ayırın. Ardından, veri diskini Yönetilmeyenden yönetilene otomatik olarak yükseltmek için diski yönetilen bir sanal makineye yeniden [bağlayın](#attach-an-existing-disk) .

## <a name="next-steps"></a>Sonraki adımlar
Çakışan [sanal makineler](devtest-lab-add-claimable-vm.md#unclaim-a-vm)için veri disklerini yönetmeyi öğrenin.

