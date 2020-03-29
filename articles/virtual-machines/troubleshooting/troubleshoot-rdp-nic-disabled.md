---
title: NIC devre dışı bırakıldığından Azure Sanal Makineleri'ne uzaktan bağlanamıyor | Microsoft Dokümanlar
description: NIC Azure VM|'nde devre dışı bırakıldığından RDP'nin başarısız olduğu bir sorunu nasıl gidereceklerini öğrenin| Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77918249"
---
#  <a name="cannot-remote-desktop-to-a-vm-because-the-network-interface-is-disabled"></a>Ağ arabirimi devre dışı bırakıldığından masaüstünü VM'ye uzaklatamam

Bu makalede, ağ arabirimi devre dışı bırakılırsa Azure Windows Sanal Makinelere (VM) Uzak Masaüstü bağlantısı kuramadığınız bir sorunun nasıl çözüleceği açıklanmaktadır.


## <a name="symptoms"></a>Belirtiler

VM'deki ağ arabirimi devre dışı olduğundan, Azure'daki bir VM'ye RDP bağlantısı veya başka bir bağlantı türü yapamazsınız.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM'nin işletim sistemi diskinin bir anlık görüntüsünü yedek olarak alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.

VM arabirimini etkinleştirmek için, VM için Seri denetimi veya [sıfırlama ağ arabirimini](#reset-network-interface) kullanın.

### <a name="use-serial-control"></a>Seri denetimi kullanma

1. Seri [Konsola bağlanın ve CMD örneğini açın.](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
) Seri Konsol VM'nizde etkinleştirilemezse, [sıfırlama ağ arabirimine](#reset-network-interface)bakın.
2. Ağ arabiriminin durumunu denetleyin:

        netsh interface show interface

    Devre dışı bırakılmış ağ arabiriminin adını not edin.

3. Ağ arabirimini etkinleştirin:

        netsh interface set interface name="interface Name" admin=enabled

    Örneğin, ara birim arabirimi "Ethernet 2" olarak adlandırılırsa, aşağıdaki komutu çalıştırın:

        netsh interface set interface name="Ethernet 2" admin=enabled

4.  Ağ arabiriminin etkin olduğundan emin olmak için ağ arabiriminin durumunu yeniden denetleyin.

        netsh interface show interface

    Bu noktada VM yeniden başlatmak zorunda değilsiniz. VM'ye tekrar ulaşılabilir olacak.

5.  VM'ye bağlanın ve sorunun çözülüp çözülmediğini görün.

## <a name="reset-network-interface"></a>Ağ arabirimini sıfırlama

Ağ arabirimini sıfırlamak için IP adresini Subnet'te bulunan başka bir IP adresiyle değiştirin. Bunu yapmak için Azure portalını veya Azure PowerShell'i kullanın. Daha fazla bilgi için [ağ arabirimini sıfırla'](reset-network-interface.md)ya bakın.
