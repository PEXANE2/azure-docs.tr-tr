---
title: Azure Lab Services’teki bir laboratuvarın kullanımını izleme | Microsoft Docs
description: Bu öğreticide laboratuvar oluşturan/sahibi olarak laboratuvarınızın kullanımını izleyeceksiniz.
services: lab-services
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
ms.date: 10/18/2019
ms.author: spelluru
ms.openlocfilehash: 842392ab425628a1c82a39e25a65066064747211
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675787"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Öğretici: Azure Lab Services’teki bir laboratuvarın kullanımını izleme
Bu öğreticide bir laboratuvar oluşturanın/sahibinin bir laboratuvarın kullanımını nasıl izleyebileceği gösterilmektedir.

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvarınıza kayıtlı kullanıcıları görüntüleme
> * Laboratuvarınızdaki VM'lerin kullanımını görüntüleme
> * Öğrenci VM'lerini yönetme 


## <a name="view-users-registered-with-the-lab"></a>Laboratuvara kayıtlı kullanıcıları görüntüleme

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler.
3. **Laboratuvarlarım** sayfasında kullanımını izlemek istediğiniz laboratuvarı seçin. 
4. Sol menüdeki veya **Kullanıcılar** kutucuğunda **Kullanıcılar** ' ı seçin. Laboratuvarınıza kayıtlı öğrencileri görürsünüz. **Kayıt bağlantısı**'nı seçin, bağlantıyı kopyalayın ve henüz laboratuvarınıza kaydolmamış yeni öğrencilere gönderin. 

    ![Kayıtlı kullanıcılar](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Laboratuvarınızdaki VM'lerin kullanımını görüntüleme 

1. Soldaki menüden **Sanal makineler**'i seçin. 
2. VM'lerin durumunu ve VM'lerin çalışma süresini gördüğünüzden emin olun. Bir laboratuvar sahibinin bir öğrenci sanal makinesinde harcadığı süre, son sütunda gösterilen kullanım zamanına göre sayılmaz. 

    ![VM kullanımı](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Öğrenci VM'lerini yönetme 
Bu sayfada, araç çubuğundaki **durum** sütunundaki veya düğmelerdeki açılan listeden birini kullanarak öğrenci sanal makinelerini başlatabilir, durdurabilir veya sıfırlayabilirsiniz. 

![VM denetimleri](../media/tutorial-track-usage/vm-controls.png)

Ayrıca, bir sanal makineyi başlatmak, durdurmak veya silmek için araç çubuğu düğmelerini kullanabilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
Sınıf laboratuvarları hakkında daha fazla bilgi edinmek için bkz. [nasıl yapılır kılavuzlarındaki](how-to-manage-lab-accounts.md)makaleler.
