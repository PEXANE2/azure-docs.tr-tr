---
title: Mac 'ten bir Azure Lab Services VM 'sine bağlanma | Microsoft Docs
description: Bu makale
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/17/2020
ms.author: spelluru
ms.openlocfilehash: 0d3484c1008e00bcfde3adb399e925c0e054f49e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79503094"
---
# <a name="connect-to-a-vm-using-rdp-on-a-mac"></a>Mac üzerinde RDP kullanarak VM 'ye bağlanma
Bu bölümde, bir öğrencinin RDP kullanarak Mac 'ten bir derslik laboratuvar VM 'sine nasıl bağlanabildiği gösterilmektedir.

## <a name="install-microsoft-remote-desktop-on-a-mac"></a>Mac 'e Microsoft Uzak Masaüstü yüklemesi
1. Mac 'inizde App Store ' u açın ve **Microsoft Uzak Masaüstü**arayın.

    ![Microsoft Uzak Masaüstü](../media/how-to-use-classroom-lab/install-ms-remote-desktop.png)
1. En son Microsoft Uzak Masaüstü sürümünü yükler. 

## <a name="access-the-vm-from-your-mac-using-rdp"></a>RDP kullanarak Mac 'Inizden VM 'ye erişme
1. **Microsoft Uzak Masaüstü** yüklü olan bilgisayarınızda indirilen **RDP** dosyasını açın. VM 'ye bağlanmaya başlamalıdır. 

    ![VM’ye bağlanma](../media/how-to-use-classroom-lab/connect-linux-vm.png)
1. Aşağıdaki uyarıyı alırsanız **devam** ' ı seçin. 

    ![Sertifika uyarısı](../media/how-to-use-classroom-lab/certificate-error.png)
1. VM 'yi görmeniz gerekir. 

    > [!NOTE]
    > Aşağıdaki örnek bir CentOS Linux sanal makinesi içindir. 

    ![VM](../media/how-to-use-classroom-lab/vm-ui.png)


## <a name="next-steps"></a>Sonraki adımlar
RDP kullanarak Linux VM 'lerine nasıl bağlanacağınızı öğrenmek için bkz. [Linux sanal makineleri için Uzak Masaüstü kullanma](how-to-use-remote-desktop-linux-student.md)


