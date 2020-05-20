---
title: Mac 'ten bir Azure Lab Services VM 'sine bağlanma | Microsoft Docs
description: Azure Lab Services bir Mac 'ten bir sanal makineye nasıl bağlanacağınızı öğrenin.
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
ms.openlocfilehash: 9087e4fb7989f89c19a70afd9ee8f061f9061166
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83704467"
---
# <a name="connect-to-a-vm-using-remote-desktop-protocol-on-a-mac"></a>Mac üzerinde Uzak Masaüstü Protokolü kullanarak bir VM 'ye bağlanma
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


