---
title: Uzak Masaüstü Azure sanal makineler için statik IP nedeniyle olamaz | Microsoft Docs
description: Microsoft azure'da statik IP kaynaklanan RDP sorun gidermeyi öğrenin. | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/08/2018
ms.author: genli
ms.openlocfilehash: 92ad33fbc759605ae901c3bcf09283c8e0b1c4b5
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77918198"
---
#  <a name="cannot-remote-desktop-to-azure-virtual-machines-because-of-static-ip"></a>Uzak Masaüstü Azure sanal makineler için statik IP nedeniyle olamaz

Bu makalede bir sorun sanal Makineye statik IP yapılandırdıktan sonra Uzak Masaüstü Azure Windows sanal makinelerin (VM'ler) olamaz.


## <a name="symptoms"></a>Belirtiler

Azure'da VM ile RDP bağlantısı değişiklik yaptığınızda, aşağıdaki hata iletisini alıyorsunuz:

**Uzak Masaüstü şu nedenlerden biri için uzak bilgisayara bağlanamıyor:**

1. **Sunucuya uzaktan erişim etkin değil**

2. **Uzak bilgisayar kapalı**

3. **Uzak bilgisayar ağda kullanılamıyor**

**Uzak bilgisayarın açık ve ağa bağlı olduğundan ve uzaktan erişimin etkinleştirildiğinden emin olun.**

Azure portal [önyükleme tanılamasında](../troubleshooting/boot-diagnostics.md) ekran görüntüsünü DENETLEDIĞINIZDE, VM 'nin normal şekilde önyüklenmesini ve oturum açma ekranında kimlik bilgileri beklediğini görürsünüz.

## <a name="cause"></a>Nedeni

Sanal Makinenin ağ arabiriminde Windows içinde tanımlanmış statik bir IP adresi vardır. Bu IP adresi Azure Portalı'nda tanımlanan adresi farklıdır.

## <a name="solution"></a>Çözüm

Bu adımları gerçekleştirmeden önce etkilenen makinenin işletim sistemi diskinin anlık yedekleyin. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

Bu sorunu çözmek için seri denetim 'i kullanarak VM için DHCP 'yi etkinleştirin veya [ağ arabirimini sıfırlayın](reset-network-interface.md) .

### <a name="use-serial-control"></a>Seri denetimini kullanma

1. [Seri konsoluna bağlanın ve cmd örneğini açın](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). VM 'niz üzerinde seri konsol etkinleştirilmemişse, bkz. [ağ arabirimini sıfırlama](reset-network-interface.md).
2. DHCP ağ arabiriminde devre dışı bırakılıp bırakılmadığını kontrol edin:

        netsh interface ip show config
3. DHCP devre dışıysa, DHCP kullanacak şekilde ağ arabiriminin yapılandırmasını geri döndür:

        netsh interface ip set address name="<NIC Name>" source=dhc

    Örneğin, "Ethernet 2" birlikte işlemek arabirimi adları ise aşağıdaki komutu çalıştırın:

        netsh interface ip set address name="Ethernet 2" source=dhc

4. IP yapılandırması yeniden ağ arabirimi artık doğru şekilde ayarlandığından emin olmak için sorgu. Yeni IP adresini, Azure tarafından sağlanan hesapla eşleşmelidir.

        netsh interface ip show config

    Bu noktada VM yeniden başlatma gerekmez. VM'yi geri erişilebilir olacaktır.

Bundan sonra, VM için statik IP 'yi yapılandırmak istiyorsanız bkz. [BIR VM için STATIK IP adreslerini yapılandırma](../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md).