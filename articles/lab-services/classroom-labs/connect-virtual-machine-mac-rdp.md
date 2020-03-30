---
title: Mac'ten Azure Lab Hizmetleri VM'sine bağlanma | Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79503094"
---
# <a name="connect-to-a-vm-using-rdp-on-a-mac"></a>Mac'te RDP kullanarak VM'ye bağlanma
Bu bölüm, bir öğrencinin RDP kullanarak Mac'ten sınıf laboratuvarı VM'ye nasıl bağlanabileceğini gösterir.

## <a name="install-microsoft-remote-desktop-on-a-mac"></a>Microsoft Uzak Masaüstü'nü Mac'e Yükleme
1. Mac'inizde App Store'u açın ve **Microsoft Uzak Masaüstü'nü**arayın.

    ![Microsoft Uzak Masaüstü](../media/how-to-use-classroom-lab/install-ms-remote-desktop.png)
1. Microsoft Uzak Masaüstü'nün en son sürümünü yükleyin. 

## <a name="access-the-vm-from-your-mac-using-rdp"></a>RDP'yi kullanarak Mac'inizden VM'ye erişin
1. **Microsoft Remote Desktop** yüklü olarak bilgisayarınızda indirilen **RDP** dosyasını açın. VM'ye bağlanmaya başlamalıdır. 

    ![VM’ye bağlanma](../media/how-to-use-classroom-lab/connect-linux-vm.png)
1. Aşağıdaki uyarıyı alırsanız **Devam'ı** seçin. 

    ![Sertifika uyarısı](../media/how-to-use-classroom-lab/certificate-error.png)
1. VM'i görmelisin. 

    > [!NOTE]
    > Aşağıdaki örnek bir CentOS Linux VM içindir. 

    ![VM](../media/how-to-use-classroom-lab/vm-ui.png)


## <a name="next-steps"></a>Sonraki adımlar
RDP kullanarak Linux VM'lerine nasıl bağlanışkullanılacağını öğrenmek [için](how-to-use-remote-desktop-linux-student.md) bkz.


