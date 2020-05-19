---
title: Azure Lab Services 'de Linux için Uzak Masaüstü kullanma | Microsoft Docs
description: Azure Lab Services 'deki bir laboratuvarda Linux sanal makineleri için Uzak Masaüstü 'nü nasıl kullanacağınızı öğrenin.
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
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: 8ccad0698ea6560dd183cacc71f5f3a644e8220c
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588114"
---
# <a name="use-remote-desktop-for-linux-virtual-machines-in-a-classroom-lab-of-azure-lab-services"></a>Azure Lab Services sınıf laboratuvarında Linux sanal makineleri için Uzak Masaüstü 'nü kullanma
Bu makalede, öğrencilerin RDP/SSH kullanarak laboratuvara bir Linux sanal makinesine (VM) nasıl bağlanabildiği gösterilmektedir. 

Öğrenciler, öğrencilerin ders Laboratuvarı 'nın VM 'sine bağlanabilmesi için Uzak Masaüstü bağlantısı özelliğini etkinleştirmeleri gerekir. Bir eğitmenin Uzak Masaüstü bağlantısı özelliğini nasıl etkinleştirebilirim hakkında yönergeler için bkz. [Linux sanal makineleri için uzak masaüstünü etkinleştirme](how-to-enable-remote-desktop-linux.md).

> [!IMPORTANT] 
> **Uzak Masaüstü bağlantısını** etkinleştirmek yalnızca Linux makinelerde **RDP** bağlantı noktasını açar. Bir eğitmen, Linux makinesine ilk kez SSH kullanarak bağlanıp daha sonra RDP kullanarak Linux makinesine bağlanabilmeniz için RDP ve GUI paketleri yükleyebilir. 

## <a name="connect-to-the-student-vm"></a>Öğrenci VM 'sine bağlanma
Laboratuvar sahibi (eğiticator), sanal makine VM 'yi makinede yüklü olan RDP ve GUI paketleri ile **yayımladıktan** sonra, Linux VM 'Lerinde, LINUX 'ta RDP oluşturabilir. Uygulamanız gereken adımlar: 

1. Bir öğrenci, Labs portalında doğrudan ( `https://labs.azure.com` ) veya bir kayıt bağlantısı () kullanarak oturum açtığında `https://labs.azure.com/register/<registrationCode>` , öğrencinin erişimi olan her laboratuvar için bir kutucuk görüntülenir. 
2. Kutucukta, durdurulmuş durumdaysa sanal makineyi başlatmak için düğmeyi değiştirin. 
3. **Bağlan**’ı seçin. SANAL makineye bağlanmak için iki seçenek görürsünüz: **SSH** ve **Uzak Masaüstü**.

    ![Öğrenci VM-bağlantı seçenekleri](../media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)

## <a name="connect-using-ssh-or-rdp"></a>SSH veya RDP kullanarak bağlanma
**SSH** seçeneğini belirlerseniz, **Sanal makinenize şu şekilde Bağlan** iletişim kutusunu görürsünüz:  

![SSH bağlantı dizesi](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Panoya kopyalamak için metin kutusunun yanındaki **Kopyala** düğmesini seçin. SSH bağlantı dizesini kaydedin. Sanal makineye bağlanmak için bir SSH terminalinde ( [Putty](https://www.putty.org/)gibi) Bu bağlantı dizesini kullanın.

**RDP** seçeneğini belirlerseniz, MAKINENIZDE bir RDP dosyası indirilir. Bunu kaydederek makineye bağlanmak için açın. 

## <a name="next-steps"></a>Sonraki adımlar
Bir sınıf laboratuvarında Linux VM 'Leri için Uzak Masaüstü bağlantısı özelliğini etkinleştirme hakkında bilgi edinmek için bkz. [Linux sanal makineleri için uzak masaüstünü etkinleştirme](how-to-enable-remote-desktop-linux.md). 

