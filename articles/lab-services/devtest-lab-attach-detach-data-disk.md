---
title: Azure DevTest Labs'da bir sanal makineye veri diski ekleme veya ayırma
description: Azure DevTest Labs'da bir veri diskini sanal makineye nasıl eklayacağınızı veya ayıracaklarınız öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78198869"
---
# <a name="attach-or-detach-a-data-disk-to-a-virtual-machine-in-azure-devtest-labs"></a>Azure DevTest Labs'da bir sanal makineye veri diski ekleme veya ayırma
[Azure Yönetilen Diskler,](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) sanal makine veri diskleriyle ilişkili depolama hesaplarını yönetir. Kullanıcı vm'ye yeni bir veri diski bağlar, diskin türünü ve boyutunu belirtir ve Azure diski otomatik olarak oluşturur ve yönetir. Veri diski daha sonra VM'den ayrılabilir ve daha sonra aynı VM'ye yeniden eklenebilir veya aynı kullanıcıya ait farklı bir VM'ye eklenebilir.

Bu işlevsellik, her bir sanal makine dışında depolama veya yazılım yönetmek için kullanışlıdır. Depolama veya yazılım zaten bir veri diskinin içinde varsa, kolayca eklenebilir, ayrılabilir ve bu veri diskinin sahibi olan kullanıcıya ait herhangi bir VM'ye yeniden eklenebilir.

## <a name="attach-a-data-disk"></a>Veri diski ekleme
VM'ye bir veri diski eklemeden önce şu ipuçlarını inceleyin:

- VM'nin boyutu, kaç veri diski ekebileceğinizi denetler. Ayrıntılar için [sanal makineler için Boyutlar'a](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)bakın.
- Yalnızca çalışan bir VM'ye bir veri diski ekleyebilirsiniz. Veri diski eklemeye çalışmadan önce VM'nin çalıştığından emin olun.

### <a name="attach-a-new-disk"></a>Yeni bir disk ekleme
Azure DevTest Labs'da yeni yönetilen bir veri diski oluşturmak ve VM'ye eklemek için aşağıdaki adımları izleyin.

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin. 
1. **Sanal makinelerim**listesinden çalışan bir VM seçin.
1. Soldaki menüden **Diskler'i**seçin.
1. Yeni bir veri diski oluşturmak ve VM'ye eklemek için **Yeni Ekle'yi** seçin.

    ![Sanal makineye yeni veri diski ekleme](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new.png)
1. Veri diski adı, türü ve boyutu girerek yeni disk bölmesini **ekle'yi** tamamlayın.

    !["Yeni disk ekle" formunu doldurun](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-new-form.png)
1. **Tamam'ı**seçin.

Birkaç dakika sonra, yeni veri diski oluşturulur ve VM'ye eklenir ve bu VM için **Veri diskleri** listesinde görünür.

### <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme
Varolan kullanılabilir bir veri diskini çalışan bir VM'ye yeniden eklemek için aşağıdaki adımları izleyin. 

1. Bir veri diskini yeniden eklemek istediğiniz çalışan bir VM seçin.
1. Soldaki menüden **Diskler'i**seçin.
1. VM'ye kullanılabilir bir veri diski eklemek için **Ekle'yi** seçin.

    ![Varolan veri diskini sanal makineye ekleme](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing-button.png)

1. **Varolan disk** bölmesini ekle'den Tamam'ı seçin.

    ![Varolan veri diskini sanal makineye ekleme](./media/devtest-lab-attach-detach-data-disk/devtest-lab-attach-existing.png)

Birkaç dakika sonra, veri diski VM'ye eklenir ve bu VM için **Veri diskleri** listesinde görünür.

## <a name="detach-a-data-disk"></a>Veri diski çıkarma
VM'ye bağlı bir veri diskine artık ihtiyacınız olmadığında, diski kolayca ayırabilirsiniz. Ayırma, diski VM'den kaldırır, ancak daha sonra kullanılmak üzere depolama da tutar.

Diskteki varolan verileri yeniden kullanmak istiyorsanız, aynı sanal makineye veya başka bir makineye yeniden ekleyebilirsiniz.

### <a name="detach-from-the-vms-management-pane"></a>VM'nin yönetim bölmesinden ayırın
1. Sanal makineler listenizden, veri diski eklenmiş bir VM seçin.
1. Soldaki menüden **Diskler'i**seçin.
1. **Veri diskleri**listesinden ayırmak istediğiniz veri diskini seçin.

    ![Sanal bir makine için veri diskleri seçin](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-button.png) 
1. Diskin ayrıntılar bölmesinin üstünden **ayır'ı** seçin.

    ![Veri diski çıkarma](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk2.png)
1. Veri diskini ayırmak istediğinizi doğrulamak için **Evet'i** seçin.

Disk ayrılmıştır ve başka bir VM'ye takılabilir. 
### <a name="detach-from-the-labs-main-pane"></a>Laboratuvarın ana bölmesinden ayırın
1. Laboratuvarınızın ana bölmesine veri **disklerim'i**seçin.
1. Ayırmak istediğiniz veri diskine sağ tıklayın – veya elipsini seçin (**...**) – ve **Ayır'ı**seçin.

    ![Veri diski çıkarma](./media/devtest-lab-attach-detach-data-disk/devtest-lab-detach-data-disk.png)
1. Ayırmak istediğinizi doğrulamak için **Evet'i** seçin.

   > [!NOTE]
   > Bir veri diski zaten ayrılmışsa, **Sil'i**seçerek kullanılabilir veri diskleri listenizden kaldırmayı seçebilirsiniz.
   >
   >

## <a name="upgrade-an-unmanaged-data-disk"></a>Yönetilmeyen bir veri diskini yükseltme
Yönetilmeyen veri diskleri kullanan varolan bir VM'iniz varsa, VM'yi yönetilen diskleri kullanmak için kolayca dönüştürebilirsiniz. Bu işlem hem işletim sistemi diskini hem de ekteki tüm veri disklerini dönüştürür.

Yönetilmeyen bir veri diskini yükseltmek için, [veri diskini](#detach-a-data-disk) yönetilmeyen bir VM'den ayırmak için bu makalede özetlenen adımları izleyin. Ardından, veri [diskini](#attach-an-existing-disk) yönetilmeyenden yönetilene otomatik olarak yükseltmek için diski yönetilen bir VM'ye yeniden bağlayın.

## <a name="next-steps"></a>Sonraki adımlar
[Talep edilebilir sanal makineler](devtest-lab-add-claimable-vm.md#unclaim-a-vm)için veri disklerini nasıl yöneteceklerini öğrenin.

