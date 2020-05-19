---
title: Azure Lab Services sanal makinelere erişme eğitimci
description: Bu makalede, eğitimciler 'in öğrenci VM 'lerine eğitimci görünümünden nasıl erişebileceği gösterilmektedir. Örneğin, bir eğitim Yardımcısı tek bir sınıf için bir eğitimci olabilir, diğer sınıflar için de öğrenciye sahip olabilir.
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
ms.date: 05/15/2020
ms.author: spelluru
ms.openlocfilehash: 449ff8aafd6dec1e9c0ff16dc407155949fd1313
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83586176"
---
# <a name="access-virtual-machines-as-a-student-from-the-educator-view"></a>Sanal makinelere eğitimci görünümünden bir öğrenci olarak erişin
Bu makalede, eğitimciler 'in, öğrenci olarak katılabilecekleri sınıflar için VM 'lerine nasıl erişebileceği gösterilmektedir. 

Bu özelliğin yardımcı olacağı bir senaryo aşağıda verilmiştir. Eğitim Yardımcısı, bir sınıf için bir eğitimci, diğer sınıflarda bir öğrenciye sahiptir. Eğitim Yardımcısı, kendi sahip oldukları laboratuvarları gösteren eğitimci görünümündeki öğrenci VM 'lerini görüntülemek ve bunlara erişmek istiyor. 

## <a name="access-vms-from-educator-view"></a>Eğitimci görünümünden VM 'Lere erişin

1. [Azure Lab Services Web sitesinde](https://labs.azure.com)oturum açın. Sahip olduğunuz laboratuvarları görürsünüz. Bu laboratuvarlar sizin oluşturduğunuz laboratuvarlar veya yönetici tarafından size sahip olarak atanan laboratuvarlar olabilir. Daha fazla bilgi için bkz. [var olan laboratuvara ek sahipler ekleme](how-to-add-user-lab-owner.md)
2. Öğrenci olarak katılan sınıfların VM 'lerine erişmek için sağ üst köşedeki bilgisayar simgesini seçin. Öğrenci olarak erişebileceğiniz VM 'Leri görtığınızdan emin olun. Aşağıdaki örnekte, Kullanıcı Python Laboratuvarı için bir öğretim Yardımcısı, ancak Java laboratuvarının bir öğrencisi. Bu nedenle, Kullanıcı sanal makineyi açılan listede Java laboratuvarında görür. Kullanıcı sanal makineyi başlatabilir ve ona bağlanabilir. 
    
    ![Öğrenci VM 'lerine erişin](../media/instructors-access-virtual-machines/access-student-virtual-machines.png)

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir VM’ye bağlanma](how-to-use-classroom-lab.md#connect-to-the-vm)
- [Mac üzerinde RDP kullanarak VM 'ye bağlanma](connect-virtual-machine-mac-rdp.md)
- [Linux sanal makineleri için Uzak Masaüstü 'nü kullanma](how-to-use-remote-desktop-linux-student.md)
