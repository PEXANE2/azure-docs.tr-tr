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
ms.date: 02/10/2020
ms.author: spelluru
ms.openlocfilehash: a1a3e62646fedd468a02eac7b1a48d0b2d00fd74
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77591991"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Öğretici: Azure Lab Services’teki bir laboratuvarın kullanımını izleme
Bu öğreticide bir laboratuvar oluşturanın/sahibinin bir laboratuvarın kullanımını nasıl izleyebileceği gösterilmektedir.

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvarınıza kayıtlı kullanıcıları görüntüleme
> * Laboratuvarınızdaki VM'lerin kullanımını görüntüleme
> * Öğrenci VM'lerini yönetme 


## <a name="view-registered-users"></a>Kayıtlı kullanıcıları görüntüleme

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler.
3. **Laboratuvarlarım** sayfasında kullanımını izlemek istediğiniz laboratuvarı seçin. 
4. Sol menüdeki **Kullanıcılar'ı** veya **Kullanıcı** döşemesini seçin. Laboratuvarınıza kayıtlı öğrencileri görürsünüz.  

    ![Kayıtlı kullanıcılar](../media/tutorial-track-usage/registered-users.png)

    Laboratuvar için kullanıcı ekleme ve yönetme hakkında daha fazla bilgi için [bkz.](how-to-configure-student-usage.md)

## <a name="view-the-usage-of-vms"></a>VM kullanımını görüntüleme

1. Soldaki menüden **Sanal makineler**'i seçin. 
2. VM'lerin durumunu ve VM'lerin çalışma süresini gördüğünüzden emin olun. Bir laboratuvar sahibinin öğrenci VM'si için harcadığı süre, son sütunda gösterilen kullanım süresiyle ilgili değildir. 

    ![VM kullanımı](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Öğrenci VM'lerini yönetme 
Bu sayfada, **Durum** sütunundaki veya araç çubuğundaki denetimleri kullanarak öğrenci VM'lerini başlatabilir, durdurabilir veya sıfırlayabilirsiniz.

![VM eylemleri](../media/tutorial-track-usage/vm-controls.png)

Laboratuvar için sanal makine havuzunu yönetme hakkında daha fazla bilgi için sanal [makine havuzunu ayarlama ve yönetme](how-to-set-virtual-machine-passwords.md)bilgisine bakın.

> [!NOTE]
> Bir eğitimci öğrenci nin VM'sini açarsa, öğrencinin kontenjanı etkilenmez. Kullanıcı kotası, kullanıcının zamanlanan ders saati dışında kullanabileceği laboratuvar saatsayısını belirtir. Kotalar hakkında daha fazla bilgi için, [kullanıcılar için kota ları ayarla'ya](how-to-configure-student-usage.md?#set-quotas-for-users)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Sınıf laboratuvarları hakkında daha fazla bilgi edinmek için [Nasıl Yap'ın kılavuzları](how-to-manage-lab-accounts.md)altındaki makalelere bakın.
