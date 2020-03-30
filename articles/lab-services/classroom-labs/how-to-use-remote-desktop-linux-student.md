---
title: Azure Lab Hizmetlerinde Linux için uzak masaüstünü kullanın | Microsoft Dokümanlar
description: Azure Lab Hizmetleri'ndeki bir laboratuvarda Linux sanal makineleri için uzak masaüstünü nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: 0f45af2730b05998fc82212c63778c89bb16b6ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73585094"
---
# <a name="use-remote-desktop-for-linux-virtual-machines-in-a-classroom-lab-of-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nin sınıf laboratuvarında Linux sanal makineleri için uzak masaüstünü kullanın
Bu makalede, öğrencilerin RDP/SSH kullanarak bir laboratuvardaki Linux sanal makinesine (VM) nasıl bağlanabilecekleri gösterilmektedir. 

Öğrencilerin sınıf laboratuvarının VM'sine bağlanabilmesi için bir eğitmenin uzak masaüstü bağlantı özelliğini etkinleştirmesi gerekir. Bir eğitmenin uzak masaüstü bağlantı özelliğini nasıl etkinleştirebileceğiyle ilgili talimatlar [için, Linux sanal makineleri için uzak masaüstünü etkinleştir](how-to-enable-remote-desktop-linux.md)mesuliyye bakın.

> [!IMPORTANT] 
> Uzak **masaüstü bağlantısını** etkinleştirmek yalnızca Linux makinelerinde **RDP** bağlantı noktasını açar. Bir eğitmen SSH'yi kullanarak Linux makinesine ilk kez bağlanabilir ve RDP ve GUI paketlerini yükleyebilir, böylece daha sonra RDP kullanarak Linux makinesine bağlanabilirsiniz. 

## <a name="connect-to-the-student-vm"></a>Öğrenci VM'sine bağlanın
Laboratuvar sahibi (öğretmen/profesör) makineye yüklenen RDP ve GUI paketlerinin yüklü vm şablonlarını **yayımladıktan** sonra öğrenciler Linux VM'lerine rdp'lerini ekleyebilirler. Adımlar aşağıdaki gibidir: 

1. Bir öğrenci doğrudan Labs portalına`https://labs.azure.com`giriş yaptığında () veya`https://labs.azure.com/register/<registrationCode>`bir kayıt bağlantısı kullanarak ( ), öğrencinin erişediği her laboratuvar için bir döşeme görüntülenir. 
2. Döşemede, durdurulmuş durumdaysa VM'yi başlatmak için düğmeyi titretin. 
3. **Bağlan**’ı seçin. VM'ye bağlanmak için iki seçenek görürsünüz: **SSH** ve **Uzak Masaüstü.**

    ![Öğrenci VM - bağlantı seçenekleri](../media/how-to-enable-remote-desktop-linux/student-vm-connect-options.png)

## <a name="connect-using-ssh-or-rdp"></a>SSH veya RDP kullanarak bağlanma
**SSH** seçeneğini seçerseniz, sanal makine iletişim **kutunuza aşağıdaki Connect'i** görürsünüz:  

![SSH bağlantı dizesi](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Metin kutusunun yanındaki **Kopyala** düğmesini seçerek panoya kopyalayın. SSH bağlantı dizesini kaydedin. Sanal makineye bağlanmak için bir SSH terminalindeki [(Macun](https://www.putty.org/)gibi) bu bağlantı dizesini kullanın.

**RDP** seçeneğini seçerseniz, makinenize bir RDP dosyası indirilir. Kaydedin ve makineye bağlanmak için açın. 

## <a name="next-steps"></a>Sonraki adımlar
Bir sınıf laboratuvarında Linux VM'ler için uzak masaüstü bağlantı özelliğini nasıl etkinleştireceğiz öğrenmek [için](how-to-enable-remote-desktop-linux.md)bkz. 

