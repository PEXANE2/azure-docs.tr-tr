---
title: Azure Lab Hizmetlerinde Linux için uzak masaüstünü etkinleştirin | Microsoft Dokümanlar
description: Azure Lab Hizmetleri'ndeki bir laboratuvarda Linux sanal makineleri için uzak masaüstünü nasıl etkinleştirin öğrenin.
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
ms.date: 10/31/2019
ms.author: spelluru
ms.openlocfilehash: cb9a3e2b9ddcd0f74bfa4978f0bc3f4eb0688257
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270868"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Azure Lab Hizmetleri'ndeki bir laboratuvarda Linux sanal makineleri için uzak masaüstünü etkinleştirme
Bu makalede, aşağıdaki görevleri nasıl yapacağınızı gösterir:

- Linux VM için uzak masaüstünü etkinleştirme
- Öğretmen uzak masaüstü bağlantısı (RDP) üzerinden şablon VM'ye nasıl bağlanabilir?

## <a name="enable-remote-desktop-for-linux-vm"></a>Linux VM için uzak masaüstünü etkinleştirme
Laboratuvar oluşturma sırasında, öğretmenler **Linux** görüntüleri için **uzak masaüstü bağlantısını** etkinleştirebilir. Şablon için bir Linux görüntüsü seçildiğinde **Uzak Masaüstü Bağlantısını Etkinleştir** seçeneği gösterilir. Bu seçenek etkinleştirildiğinde, öğretmenler ŞABLON VM'ye ve öğrenci VM'lerine RDP (Uzak Masaüstü) aracılığıyla bağlanabilir. 

![Linux görüntüsü için uzak masaüstü bağlantısını etkinleştirme](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

**Etkinle Uzak Masaüstü Bağlantısı** ileti kutusunda, **Uzak Masaüstüyle Devam Et'i**seçin. 

![Linux görüntüsü için uzak masaüstü bağlantısını etkinleştirme](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> Uzak **masaüstü bağlantısını** etkinleştirmek yalnızca Linux makinelerinde **RDP** bağlantı noktasını açar. RDP zaten sanal makine görüntüsüne yüklenmiş ve yapılandırılırsa (örneğin: Ubuntu Data Science Virtual Machine görüntüsü), siz/öğrenciler herhangi bir ek adımı takip etmeden RDP üzerinden VM'lere bağlayabilirsiniz.
> 
> VM görüntüsüNDE RDP yüklü ve yapılandırılmış değilse, ilk kez SSH kullanarak Linux makinesine bağlanmanız ve RDP ve GUI paketlerini yüklemeniz gerekir, böylece daha sonra RDP kullanarak Linux makinesine bağlanabilirsiniz. Daha fazla bilgi için [Azure'daki bir Linux VM'ye bağlanmak için Uzak Masaüstü'nü Yükle ve yapılandırma 'ya](../../virtual-machines/linux/use-remote-desktop.md)bakın. Daha sonra, öğrencilerin öğrenci Linux VM'lerine RDP yapabilmeleri için görüntüyü yayımlarsınız. 

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Şu anda, uzak masaüstü bağlantısı aşağıdaki işletim sistemleri için desteklenir:

- openSUSE Sıçrama42.3
- CentOS tabanlı 7.5
- Debian 9 "Stretch"
- Ubuntu Server 16.04 LTS

## <a name="connect-to-the-template-vm"></a>Şablon VM'ye bağlanma 
Öğretmenler önce SSH kullanarak VM şablonuna bağlanmalı ve üzerine RDP ve GUI paketleri yüklemelidir. Ardından, öğretmenler Şablon VM'ye bağlanmak için RDP'yi kullanabilirler: 

1. Araç çubuğunda **Şablonu Özelleştir'i** görürseniz, şablonu seçin. Ardından, **Şablonu Özelleştir** iletişim kutusunda **Devam** et'i seçin. Bu eylem, şablon VM'yi başlatır.  

    ![Şablonu özelleştir](../media/how-to-enable-remote-desktop-linux/customize-template.png)
2. Şablon VM başlatıldıktan sonra Connect **şablonu'nu** seçebilir ve araç çubuğunda **SSH üzerinden bağlanabilirsiniz.** 

    ![Laboratuvar oluşturulduktan sonra RDP ile şablona bağlanma](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 
3. Aşağıdaki Connect **to sanal makine** iletişim kutusunuz için ilerler. Metin kutusunun yanındaki **Kopyala** düğmesini seçerek panoya kopyalayın. SSH bağlantı dizesini kaydedin. Sanal makineye bağlanmak için bir SSH terminalindeki [(Macun](https://www.putty.org/)gibi) bu bağlantı dizesini kullanın.
 
    ![SSH bağlantı dizesi](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)
4. RDP ve GUI paketlerini yükleyin, böylece/öğrenciler daha sonra RDP kullanarak Linux makinesine bağlanabilirsiniz. Daha fazla bilgi için [Azure'daki bir Linux VM'ye bağlanmak için Uzak Masaüstü'nü Yükle ve yapılandırma 'ya](../../virtual-machines/linux/use-remote-desktop.md)bakın. Daha sonra, öğrencilerin öğrenci Linux VM'lerine RDP yapabilmeleri için görüntüyü yayımlarsınız.
5. Bu paketler yüklendikten sonra araç çubuğundaki **şablona Bağlan'ı** kullanabilir ve rdp aracılığıyla vm şablonuna bağlanmak için **RDP ile Bağlan'ı** seçebilirsiniz. RDP dosyasını kaydedin ve RDP aracılığıyla VM şablonuna bağlanmak için kullanın. 

## <a name="next-steps"></a>Sonraki adımlar
Bir eğitmen uzak masaüstü bağlantı özelliğini etkinleştirdikten sonra, öğrenciler RDP/SSH üzerinden VM'lerine bağlanabilirler. Daha fazla bilgi için [bkz.](how-to-use-remote-desktop-linux-student.md) 