---
title: NIC devre dışı olduğundan, Azure sanal makineler için uzaktan bağlanamıyor | Microsoft Docs
description: RDP başarısız olduğu içinde NIC Azure sanal Makinesinde devre dışı bırakıldığından bir sorun gidermeyi öğrenin | Microsoft Docs
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
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77918249"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>Bir VM'ye Uzak Masaüstü Ağ arabirimini devre dışı bırakıldığından olamaz

Bu makalede, ağ arabirimini devre dışı bırakılırsa, Uzak Masaüstü Bağlantısı Azure Windows sanal makinelerine (VM'ler) yapamazsınız bir sorunun nasıl giderileceği açıklanmaktadır.


## <a name="symptoms"></a>Belirtiler

Sanal makine içindeki ağ arabirimini devre dışı olduğundan azure'da bir VM ile RDP bağlantısı veya başka türde bir bağlantı diğer bağlantı noktalarına yapamazsınız.

## <a name="solution"></a>Çözüm

Bu adımları gerçekleştirmeden önce etkilenen makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

VM 'nin arabirimini etkinleştirmek için, seri denetim veya VM için [ağ arabirimini sıfırlama](#reset-network-interface) ' yı kullanın.

### <a name="use-serial-control"></a>Seri denetimini kullanma

1. [Seri konsoluna bağlanın ve cmd örneğini açın](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). VM 'niz üzerinde seri konsol etkinleştirilmemişse, bkz. [ağ arabirimini sıfırlama](#reset-network-interface).
2. Ağ arabirimi durumunu kontrol edin:

        netsh interface show interface

    Devre dışı ağ arabirimi adını not edin.

3. Ağ arabirimi etkinleştirin:

        netsh interface set interface name="interface Name" admin=enabled

    Örneğin, birlikte işlemek arabirimi "Ethernet 2" adında, aşağıdaki komutu çalıştırın:

        netsh interface set interface name="Ethernet 2" admin=enabled

4.  Yeniden ağ arabiriminin etkin olduğundan emin olmak için ağ arabirimin durumunu denetleyin.

        netsh interface show interface

    Bu noktada VM yeniden başlatma gerekmez. VM'yi geri erişilebilir olacaktır.

5.  VM'ye bağlanın ve sorunun çözülüp olup olmadığını görebilirsiniz.

## <a name="reset-network-interface"></a>Ağ arabirimini sıfırlayın

Ağ arabirimi sıfırlamak için alt ağdaki kullanılabilir başka bir IP adresi IP adresini değiştirin. Bunu yapmak için Azure portal veya Azure PowerShell kullanın. Daha fazla bilgi için bkz. [ağ arabirimini sıfırlama](reset-network-interface.md).
