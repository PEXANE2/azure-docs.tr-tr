---
title: Azure Lab Services 'de Linux için Uzak Masaüstü 'nü etkinleştirme | Microsoft Docs
description: Azure Lab Services 'deki bir laboratuvarda Linux sanal makineleri için Uzak Masaüstü 'nü etkinleştirmeyi öğrenin.
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
ms.date: 04/30/2020
ms.author: spelluru
ms.openlocfilehash: a40575340fc5b1c202be6b001807085954439f03
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588165"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Azure Lab Services bir laboratuvardaki Linux sanal makineleri için Uzak Masaüstü 'nü etkinleştirme
Bu makalede aşağıdaki görevlerin nasıl yapılacağı gösterilmektedir:

- Linux VM için Uzak Masaüstü 'nü etkinleştirme
- Bir eğitimci, şablon VM 'ye Uzak Masaüstü Bağlantısı (RDP) aracılığıyla bağlanabilir.

## <a name="enable-remote-desktop-for-linux-vm"></a>Linux VM için Uzak Masaüstü 'nü etkinleştirme
Laboratuvar oluşturma sırasında eğitimciler, **Linux** görüntüleri için **Uzak Masaüstü bağlantısı** 'nı etkinleştirebilir. Şablon için bir Linux görüntüsü seçildiğinde **etkinleştir Uzak Masaüstü bağlantısı** seçeneği görüntülenir. Bu seçenek etkinleştirildiğinde, eğitimciler RDP (Uzak Masaüstü) aracılığıyla şablon VM ve öğrenci VM 'lerine bağlanabilir. 

![Linux görüntüsü için Uzak Masaüstü bağlantısını etkinleştirme](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

**Uzak Masaüstü bağlantısı etkinleştirme** Iletisi kutusunda **Uzak Masaüstü ile devam et**' i seçin. 

![Linux görüntüsü için Uzak Masaüstü bağlantısını etkinleştirme](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> **Uzak Masaüstü bağlantısını** etkinleştirmek yalnızca Linux makinelerde **RDP** bağlantı noktasını açar. RDP, sanal makine görüntüsünde zaten yüklü ve yapılandırılmışsa, siz/öğrenciler ek adımları izleyerek RDP aracılığıyla VM 'lere bağlanabilir.
> 
> VM görüntüsünün RDP yüklemesi ve yapılandırması yoksa, Linux makinesine ilk kez SSH kullanarak bağlanmanız ve RDP ve GUI paketlerini yüklemeniz gerekir, böylelikle/öğrenciler daha sonra RDP kullanarak Linux makinesine bağlanabilir. Daha fazla bilgi için bkz. [Azure 'da bir LINUX VM 'sine bağlanmak Için uzak masaüstü 'Nü yüklemek ve yapılandırmak](../../virtual-machines/linux/use-remote-desktop.md). Daha sonra, öğrencilerin öğrenci Linux VM 'lerinde RDP kullanabilmesi için görüntüyü yayımlayın. 

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Şu anda, Uzak Masaüstü bağlantısı aşağıdaki işletim sistemleri için desteklenir:

- openSUSE artık 42,3
- CentOS tabanlı 7,5
- 9 "uzat"
- Ubuntu Server 16,04 LTS

## <a name="connect-to-the-template-vm"></a>Şablon VM'ye bağlanma 
Eğitimciler, önce SSH kullanarak şablon VM 'ye bağlanmalıdır ve buna RDP ve GUI paketleri yükler. Daha sonra eğitimciler, şablon VM 'ye bağlanmak için RDP kullanabilir: 

1. Araç çubuğunda **şablonu Özelleştir** ' i görürseniz, bunu seçin. Ardından, **şablonu Özelleştir** Iletişim kutusunda **devam** ' ı seçin. Bu eylem, şablon VM 'yi başlatır.  

    ![Şablonu Özelleştir](../media/how-to-enable-remote-desktop-linux/customize-template.png)
2. Şablon VM 'si başlatıldıktan sonra, **şablonu bağla** ' yı seçip araç çubuğunda **SSH aracılığıyla bağlanabilirsiniz** . 

    ![Laboratuvar oluşturulduktan sonra RDP aracılığıyla şablona bağlanma](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 
3. Aşağıdaki **Sanal makinenize Bağlan** iletişim kutusunu görürsünüz. Panoya kopyalamak için metin kutusunun yanındaki **Kopyala** düğmesini seçin. SSH bağlantı dizesini kaydedin. Sanal makineye bağlanmak için bir SSH terminalinde ( [Putty](https://www.putty.org/)gibi) Bu bağlantı dizesini kullanın.
 
    ![SSH bağlantı dizesi](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)
4. RDP ve GUI paketlerini, daha sonra RDP kullanarak Linux makinesine bağlanabilmeleri için, RDP ve GUI paketleri yükleyebilirsiniz. Daha fazla bilgi için bkz. [Azure 'da bir LINUX VM 'sine bağlanmak Için uzak masaüstü 'Nü yüklemek ve yapılandırmak](../../virtual-machines/linux/use-remote-desktop.md). Daha sonra, öğrencilerin öğrenci Linux VM 'lerinde RDP kullanabilmesi için görüntüyü yayımlayın.
5. Bu paketler yüklendikten sonra, araç çubuğundaki **şablona Bağlan şablonunu** kullanabilir ve sonra RDP aracılığıyla şablon VM 'sine bağlanmak için **RDP aracılığıyla Bağlan** ' ı seçebilirsiniz. RDP dosyasını kaydedin ve RDP aracılığıyla şablon VM 'sine bağlanmak için kullanın. 

## <a name="next-steps"></a>Sonraki adımlar
Bir eğitmen Uzak Masaüstü bağlantısı özelliğini etkinleştirdikten sonra, öğrenciler RDP/SSH aracılığıyla VM 'lerine bağlanabilir. Daha fazla bilgi için bkz. [sınıf laboratuvarında Linux için Uzak Masaüstü VM 'Leri kullanma](how-to-use-remote-desktop-linux-student.md). 