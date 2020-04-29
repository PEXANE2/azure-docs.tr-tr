---
title: Azure Lab Services sanal makinelere erişen eğitmen
description: Bu makalede, Eğitmenler 'in öğrenci VM 'lerine eğitmen görünümünden nasıl erişebileceği gösterilmektedir. Örneğin, bir eğitim Yardımcısı, bir sınıf için bir eğitmen, diğer sınıflar için de öğrenci olabilir.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2020
ms.author: spelluru
ms.openlocfilehash: c048a2f159784a5bd38f185af29cec314b31824f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81641946"
---
# <a name="access-virtual-machines-as-a-student-from-the-instructor-view"></a>Sanal makinelere eğitmen görünümünden bir öğrenci olarak erişin
Bu makalede, Eğitmenler, öğrenciler olarak katıldıkları sınıflar için VM 'lerine nasıl erişebilecekleri gösterilmektedir. 

Bu özelliğin yardımcı olacağı bir senaryo aşağıda verilmiştir. Eğitim Yardımcısı, bir sınıf için bir eğitmendir, diğer sınıflarda bir öğrenciye sahiptir. Eğitim eğitmeni, kendi sahip oldukları laboratuvarları gösteren eğitmen görünümünden öğrenci VM 'lerini görüntülemek ve bunlara erişmek istiyor. 

## <a name="access-vms-from-instructor-view"></a>Sanal makinelere eğitmen görünümünden erişin

1. [Azure Lab Services Web sitesinde](https://labs.azure.com)oturum açın. Sahip olduğunuz laboratuvarları görürsünüz. Bu laboratuvarlar sizin oluşturduğunuz laboratuvarlar veya yönetici tarafından size sahip olarak atanan laboratuvarlar olabilir. Daha fazla bilgi için bkz. [var olan laboratuvara ek sahipler ekleme](how-to-add-user-lab-owner.md)
2. Öğrenci olarak katılan sınıfların VM 'lerine erişmek için sağ üst köşedeki bilgisayar simgesini seçin. Öğrenci olarak erişebileceğiniz VM 'Leri görtığınızdan emin olun. Aşağıdaki örnekte, Kullanıcı Python Laboratuvarı için bir öğretim Yardımcısı, ancak Java laboratuvarının bir öğrencisi. Bu nedenle, Kullanıcı sanal makineyi açılan listede Java laboratuvarında görür. Kullanıcı sanal makineyi başlatabilir ve ona bağlanabilir. 
    
    ![Öğrenci VM 'lerine erişin](../media/instructors-access-virtual-machines/access-student-virtual-machines.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir VM’ye bağlanma](how-to-use-classroom-lab.md#connect-to-the-vm)
- [Mac üzerinde RDP kullanarak VM 'ye bağlanma](connect-virtual-machine-mac-rdp.md)
- [Linux sanal makineleri için Uzak Masaüstü 'nü kullanma](how-to-use-remote-desktop-linux-student.md)
