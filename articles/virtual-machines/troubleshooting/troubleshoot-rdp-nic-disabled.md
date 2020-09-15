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
ms.openlocfilehash: 0e79efc9de43fc0a3044e9ae1e3959f63bb6e69f
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90090285"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>Ağ arabirimi devre dışı bırakıldığından bir VM 'ye uzak masaüstü yapılamıyor

Bu makalede, ağ arabirimi devre dışı bırakılmışsa Azure Windows Sanal Makineleri (VM 'Ler) ile uzak masaüstü bağlantısı yapma konusunda bir sorunu nasıl giderebileceğiniz açıklanır.


## <a name="symptoms"></a>Belirtiler

VM 'deki ağ arabirimi devre dışı bırakıldığından, bir RDP bağlantısı veya diğer bağlantı noktalarından başka bir bağlantı noktası için Azure 'daki bir VM 'ye herhangi bir türde bağlantı yapamazsınız.

![Ağ arabiriminin bağlantısının kesildiği bir VM 'yi gösteren ekran görüntüsü.](./media/troubleshoot-rdp-nic-disabled/disconnected.png)

![Ağ arabiriminin devre dışı bırakıldığı bir VM 'yi gösteren ekran görüntüsü.](./media/troubleshoot-rdp-nic-disabled/disabled.png)


## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin işletim sistemi diskinin bir yedek olarak anlık görüntüsünü alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

VM 'nin arabirimini etkinleştirmek için, seri denetim veya VM için [ağ arabirimini sıfırlama](#reset-network-interface) ' yı kullanın.

### <a name="use-serial-control"></a>Seri denetim kullan

1. [Seri konsoluna bağlanın ve cmd örneğini açın](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console). VM 'niz üzerinde seri konsol etkinleştirilmemişse, bkz. [ağ arabirimini sıfırlama](#reset-network-interface).
2. Ağ arabiriminin durumunu kontrol edin:

    ```console
    netsh interface show interface
    ```

    Devre dışı bırakılan ağ arabiriminin adını aklınızda edin.

3. Ağ arabirimini etkinleştir:

    ```console
    netsh interface set interface name="interface Name" admin=enabled
    ```

    Örneğin, INTERWORK arabirimi "Ethernet 2" olarak adlandırılmışsa, aşağıdaki komutu çalıştırın:

    ```console
    netsh interface set interface name="Ethernet 2" admin=enabled
    ```

4.  Ağ arabiriminin etkinleştirildiğinden emin olmak için ağ arabiriminin durumunu yeniden kontrol edin.

    ```console
    netsh interface show interface
    ```

    Bu noktada VM 'yi yeniden başlatmanız gerekmez. VM 'ye erişilecektir.

5.  VM 'ye bağlanın ve sorunun giderilmiş olup olmadığını görün.

## <a name="reset-network-interface"></a>Ağ arabirimini Sıfırla

Ağ arabirimini sıfırlamak için IP adresini alt ağda kullanılabilir olan başka bir IP adresi ile değiştirin. Bunu yapmak için Azure portal veya Azure PowerShell kullanın. Daha fazla bilgi için bkz. [ağ arabirimini sıfırlama](reset-network-interface.md).
