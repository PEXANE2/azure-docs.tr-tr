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
ms.date: 08/20/2019
ms.author: spelluru
ms.openlocfilehash: 86ad892f514513840ff33e46921607221896db84
ms.sourcegitcommit: 9fba13cdfce9d03d202ada4a764e574a51691dcd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71315742"
---
# <a name="enable-remote-desktop-for-linux-virtual-machines-in-a-lab-in-azure-lab-services"></a>Azure Lab Services bir laboratuvardaki Linux sanal makineleri için Uzak Masaüstü 'nü etkinleştirme
Bu makalede aşağıdaki görevlerin nasıl yapılacağı gösterilmektedir:

- Linux VM için Uzak Masaüstü 'nü etkinleştirme
- Öğretmen, Uzak Masaüstü Bağlantısı (RDP) aracılığıyla şablon VM 'sine nasıl bağlanabilir.

## <a name="enable-remote-desktop-for-linux-vm"></a>Linux VM için Uzak Masaüstü 'nü etkinleştirme
Laboratuvar oluşturma sırasında, öğretmenler **Linux** görüntüleri için **Uzak Masaüstü bağlantısı** 'nı etkinleştirebilir. Şablon için bir Linux görüntüsü seçildiğinde **etkinleştir Uzak Masaüstü bağlantısı** seçeneği görüntülenir. Bu seçenek etkinleştirildiğinde, öğretmenler RDP (Uzak Masaüstü) aracılığıyla şablon VM ve öğrenci VM 'lerine bağlanabilir. 

![Linux görüntüsü için Uzak Masaüstü bağlantısını etkinleştirme](../media/how-to-enable-remote-desktop-linux/enable-rdp-option.png)

**Uzak Masaüstü bağlantısı etkinleştirme** Iletisi kutusunda **Uzak Masaüstü ile devam et**' i seçin. 

![Linux görüntüsü için Uzak Masaüstü bağlantısını etkinleştirme](../media/how-to-enable-remote-desktop-linux/enabling-remote-desktop-connection-dialog.png)

> [!IMPORTANT] 
> **Uzak Masaüstü bağlantısını** etkinleştirmek yalnızca Linux makinelerde **RDP** bağlantı noktasını açar. Zaten RDP, sanal makine görüntüsüne yüklenip yapılandırılmışsa (örneğin: Ubuntu Veri Bilimi Sanal Makinesi Image), siz/öğrenciler ek adımları izleyerek RDP aracılığıyla VM 'lere bağlanabilir.
> 
> VM görüntüsünün RDP yüklemesi ve yapılandırması yoksa, Linux makinesine ilk kez SSH kullanarak bağlanmanız ve RDP ve GUI paketlerini yüklemeniz gerekir, böylelikle/öğrenciler daha sonra RDP kullanarak Linux makinesine bağlanabilir. Daha sonra, öğrencilerin öğrenci Linux VM 'lerinde RDP kullanabilmesi için görüntüyü yayımlayın.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Şu anda, Uzak Masaüstü bağlantısı aşağıdaki işletim sistemleri için desteklenir:

- openSUSE artık 42,3
- CentOS tabanlı 7,5
- 9 "uzat"
- Ubuntu Server 16,04 LTS

## <a name="teachers-connecting-to-the-template-vm-using-rdp"></a>RDP kullanarak şablon VM 'ye bağlanan öğretmenler
Öğretmenler, önce SSH kullanarak şablon VM 'ye bağlanmalıdır ve buna RDP ve GUI paketleri yükler. Daha sonra öğretmenler, RDP kullanarak Linux VM 'lerine bağlanmak için aşağıdaki adımları kullanabilir: 

Laboratuvar oluşturma sırasında şablon VM 'ye bağlanmak için **Uzak Masaüstü** seçeneğini görürsünüz. 

![Oluşturma sırasında RDP aracılığıyla şablona bağlanma](../media/how-to-enable-remote-desktop-linux/connect-at-creation.png)

Laboratuvar oluşturulduktan ve şablon VM başlatıldıktan sonra laboratuvarın ana sayfasında **Uzak Masaüstü** seçeneğini görürsünüz. Zaten başlatılmamışsa, şablon VM 'yi başlatın. 

![Laboratuvar oluşturulduktan sonra RDP aracılığıyla şablona bağlanma](../media/how-to-enable-remote-desktop-linux/rdp-after-lab-creation.png) 

SSH veya RDP kullanarak VM 'ye bağlanma hakkında daha fazla bilgi için bkz. [SSH veya RDP kullanarak bağlanma] ((#connect-using-SSH-veya-RDP). 

## <a name="teachers-connecting-to-a-student-vm-using-rdp"></a>RDP kullanarak bir öğrenci VM 'sine bağlanan öğretmenler
Öğretmen/Mesleme, **sanal makineler** görünümüne geçerek ve **Bağlan** SIMGESINI seçerek bir öğrenci VM 'sine bağlanabilir. Bundan önce, öğretmenlerin şablon görüntüsünü üzerinde RDP ve GUI paketleri yüklü olarak **yayımlaması** gerekir. 

![Öğrenci VM 'sine bağlanan öğretmenler](../media/how-to-enable-remote-desktop-linux/teacher-connect-to-student-vm.png)

SSH veya RDP kullanarak VM 'ye bağlanma hakkında daha fazla bilgi için bkz. [SSH veya RDP kullanarak bağlanma] ((#connect-using-SSH-veya-RDP). 

## <a name="connect-using-ssh-or-rdp"></a>SSH veya RDP kullanarak bağlanma
**SSH** seçeneğini belirlerseniz, **Sanal makinenize şu şekilde Bağlan** iletişim kutusunu görürsünüz:  

![SSH bağlantı dizesi](../media/how-to-enable-remote-desktop-linux/ssh-connection-string.png)

Panoya kopyalamak için metin kutusunun yanındaki **Kopyala** düğmesini seçin. SSH bağlantı dizesini kaydedin. Sanal makineye bağlanmak için bir SSH terminalinde ( [Putty](https://www.putty.org/)gibi) Bu bağlantı dizesini kullanın.

**RDP** seçeneğini belirlerseniz, MAKINENIZDE bir RDP dosyası indirilir. Bunu kaydederek makineye bağlanmak için açın. 

## <a name="next-steps"></a>Sonraki adımlar
Bir eğitmen Uzak Masaüstü bağlantısı özelliğini etkinleştirdikten sonra, öğrenciler RDP/SSH aracılığıyla VM 'lerine bağlanabilir. Daha fazla bilgi için bkz. [sınıf laboratuvarında Linux için Uzak Masaüstü VM 'Leri kullanma](how-to-use-remote-desktop-linux-student.md). 