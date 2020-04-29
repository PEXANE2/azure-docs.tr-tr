---
title: NIC devre dışı bırakıldığından Azure sanal makinelerine uzaktan bağlanılamıyor | Microsoft Docs
description: Azure VM 'de NIC devre dışı bırakıldığından, RDP 'nin başarısız olduğu bir sorunu nasıl giderebileceğinizi öğrenin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/12/2018
ms.author: genli
ms.openlocfilehash: 315974e4995630eb3af055ac0e1c44f7d8dd0737
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77918249"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>Ağ arabirimi devre dışı bırakıldığından bir VM 'ye uzak masaüstü yapılamıyor

Bu makalede, ağ arabirimi devre dışı bırakılmışsa Azure Windows Sanal Makineleri (VM 'Ler) ile uzak masaüstü bağlantısı yapma konusunda bir sorunu nasıl giderebileceğiniz açıklanır.


## <a name="symptoms"></a>Belirtiler

VM 'deki ağ arabirimi devre dışı bırakıldığından, bir RDP bağlantısı veya diğer bağlantı noktalarından başka bir bağlantı noktası için Azure 'daki bir VM 'ye herhangi bir türde bağlantı yapamazsınız.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin işletim sistemi diskinin bir yedek olarak anlık görüntüsünü alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

VM 'nin arabirimini etkinleştirmek için, seri denetim veya VM için [ağ arabirimini sıfırlama](#reset-network-interface) ' yı kullanın.

### <a name="use-serial-control"></a>Seri denetim kullan

1. [Seri konsoluna bağlanın ve cmd örneğini açın](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). VM 'niz üzerinde seri konsol etkinleştirilmemişse, bkz. [ağ arabirimini sıfırlama](#reset-network-interface).
2. Ağ arabiriminin durumunu kontrol edin:

        netsh interface show interface

    Devre dışı bırakılan ağ arabiriminin adını aklınızda edin.

3. Ağ arabirimini etkinleştir:

        netsh interface set interface name="interface Name" admin=enabled

    Örneğin, INTERWORK arabirimi "Ethernet 2" olarak adlandırılmışsa, aşağıdaki komutu çalıştırın:

        netsh interface set interface name="Ethernet 2" admin=enabled

4.  Ağ arabiriminin etkinleştirildiğinden emin olmak için ağ arabiriminin durumunu yeniden kontrol edin.

        netsh interface show interface

    Bu noktada VM 'yi yeniden başlatmanız gerekmez. VM 'ye erişilecektir.

5.  VM 'ye bağlanın ve sorunun giderilmiş olup olmadığını görün.

## <a name="reset-network-interface"></a>Ağ arabirimini Sıfırla

Ağ arabirimini sıfırlamak için IP adresini alt ağda kullanılabilir olan başka bir IP adresi ile değiştirin. Bunu yapmak için Azure portal veya Azure PowerShell kullanın. Daha fazla bilgi için bkz. [ağ arabirimini sıfırlama](reset-network-interface.md).
