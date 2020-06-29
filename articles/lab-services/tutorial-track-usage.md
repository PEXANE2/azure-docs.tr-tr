---
title: Azure Lab Services’teki bir laboratuvarın kullanımını izleme | Microsoft Docs
description: Bu öğreticide laboratuvar oluşturan/sahibi olarak laboratuvarınızın kullanımını izleyeceksiniz.
ms.topic: tutorial
ms.date: 06/26/2020
ms.openlocfilehash: a9a9b49b568decc621be1969a8578d61ac7e4861
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85445041"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Öğretici: Azure Lab Services’teki bir laboratuvarın kullanımını izleme
Bu öğreticide bir laboratuvar oluşturanın/sahibinin bir laboratuvarın kullanımını nasıl izleyebileceği gösterilmektedir.

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvarınıza kayıtlı kullanıcıları görüntüleme
> * Laboratuvarınızdaki VM'lerin kullanımını görüntüleme
> * Öğrenci VM'lerini yönetme 


## <a name="view-registered-users"></a>Kayıtlı kullanıcıları görüntüle

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler.
3. **Laboratuvarlarım** sayfasında kullanımını izlemek istediğiniz laboratuvarı seçin. 
4. Sol menüdeki veya **Kullanıcılar** kutucuğunda **Kullanıcılar** ' ı seçin. Laboratuvarınıza kayıtlı öğrencileri görürsünüz.  

    ![Kayıtlı kullanıcılar](./media/tutorial-track-usage/registered-users.png)

    Laboratuvar için Kullanıcı ekleme ve yönetme hakkında daha fazla bilgi için bkz. [Laboratuvar kullanıcıları ekleme ve yönetme](how-to-configure-student-usage.md).

## <a name="view-the-usage-of-vms"></a>VM 'lerin kullanımını görüntüleme

1. Soldaki menüden **Sanal makineler**'i seçin. 
2. VM'lerin durumunu ve VM'lerin çalışma süresini gördüğünüzden emin olun. Bir laboratuvar sahibinin bir öğrenci sanal makinesinde harcadığı süre, son sütunda gösterilen kullanım zamanına göre sayılmaz. 

    ![VM kullanımı](./media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Öğrenci VM'lerini yönetme 
Bu sayfada, **durum** sütunundaki veya araç çubuğundaki denetimleri kullanarak öğrenci VM 'lerini başlatabilir, durdurabilir veya sıfırlayabilirsiniz.

![VM eylemleri](./media/tutorial-track-usage/vm-controls.png)

Laboratuvar için sanal makine havuzunu yönetme hakkında daha fazla bilgi için bkz. [sanal makine havuzunu ayarlama ve yönetme](how-to-set-virtual-machine-passwords.md).

> [!NOTE]
> Bir eğitimci bir öğrenci sanal makinesini açtığında öğrencinin kotası etkilenmez. Bir kullanıcı için kota, kullanıcının zamanlanan sınıf süresi dışında kullanabileceği laboratuvar saatleri sayısını belirtir. Kotalar hakkında daha fazla bilgi için bkz. [kullanıcılar için kotaları ayarlama](how-to-configure-student-usage.md?#set-quotas-for-users).

## <a name="next-steps"></a>Sonraki adımlar
Sınıf laboratuvarları hakkında daha fazla bilgi edinmek için bkz. [nasıl yapılır kılavuzlarındaki](how-to-manage-lab-accounts.md)makaleler.
