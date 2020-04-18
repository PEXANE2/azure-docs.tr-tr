---
title: Azure Laboratuvar Hizmetlerinde VM'lere erişen eğitmen
description: Bu makalede, eğitmenlerin öğrenci VM'lerine eğitmen görünümünden nasıl erişebildiği gösterilmektedir. Örneğin, bir öğretim görevlisi bir sınıf için bir eğitmen, diğer sınıflar için bir öğrenci olabilir.
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
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641946"
---
# <a name="access-virtual-machines-as-a-student-from-the-instructor-view"></a>Öğrenci olarak sanal makinelere eğitmen görünümünden erişin
Bu makalede, eğitmenlerin öğrenci olarak katıldıkları sınıflar için VM'lerine nasıl erişebilecekleri gösterilmektedir. 

Burada bu özelliğiyardımcı olacak bir senaryodur. Bir öğretim görevlisi bir sınıf için bir eğitmen ama diğer sınıflarda bir öğrencidir. Ve, öğretim eğitmeni görüntülemek ve kendi laboratuvarları gösteren eğitmen görünümünden öğrenci VM'ler erişmek istiyor. 

## <a name="access-vms-from-instructor-view"></a>Eğitmen görünümünden VM'lere erişin

1. Azure Laboratuvar [Hizmetleri web sitesinde](https://labs.azure.com)oturum açın. Sahip olduğun laboratuvarları görüyorsun. Bu laboratuvarlar, kendi oluşturduğunuz laboratuvarlar veya yöneticinin size sahip olarak atadığı laboratuvarlar olabilir. Daha fazla bilgi için, [varolan bir laboratuvara nasıl ek sahipler ekleyeceğiniz](how-to-add-user-lab-owner.md)
2. Öğrenci olarak katıldığınız sınıflar için VM'lere erişmek için sağ üst köşedeki bilgisayar simgesini seçin. Öğrenci olarak erişebileceğiniz VM'leri gördüğünüzü doğrulayın. Aşağıdaki örnekte, kullanıcı Python laboratuvarı için bir öğretim görevlisi, ancak Java laboratuvarının bir öğrencisidir. Yani, kullanıcı açılan listede Java laboratuvarından VM görür. Kullanıcı VM'yi başlatAbilir ve ona bağlanabilir. 
    
    ![Öğrenci VM'lerini erişin](../media/instructors-access-virtual-machines/access-student-virtual-machines.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir VM’ye bağlanma](how-to-use-classroom-lab.md#connect-to-the-vm)
- [Mac'te RDP kullanarak VM'ye bağlanma](connect-virtual-machine-mac-rdp.md)
- [Linux sanal makineleri için uzak masaüstü nü kullan](how-to-use-remote-desktop-linux-student.md)
