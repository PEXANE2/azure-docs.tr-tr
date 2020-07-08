---
title: Statik IP nedeniyle Azure sanal makinelerinde uzak masaüstü yapılamıyor | Microsoft Docs
description: Microsoft Azure statik IP 'den kaynaklanan RDP sorunuyla ilgili sorunları nasıl giderebileceğinizi öğrenin. | Microsoft Docs
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77918198"
---
#  <a name="cannot-remote-desktop-to-azure-virtual-machines-because-of-static-ip"></a>Statik IP nedeniyle Azure sanal makinelerinde uzak masaüstü yapılamıyor

Bu makalede, VM 'de statik bir IP yapılandırıldıktan sonra Azure Windows Sanal Makineleri (VM) ile uzak masaüstü 'nü bir sorunla ilgili bir sorun açıklanmaktadır.


## <a name="symptoms"></a>Belirtiler

Azure 'da bir VM 'ye RDP bağlantısı yaptığınızda aşağıdaki hata iletisini alırsınız:

**Uzak Masaüstü şu nedenlerden biri için uzak bilgisayara bağlanamıyor:**

1. **Sunucuya uzaktan erişim etkin değil**

2. **Uzak bilgisayar kapalı**

3. **Uzak bilgisayar ağda kullanılamıyor**

**Uzak bilgisayarın açık ve ağa bağlı olduğundan ve uzaktan erişimin etkinleştirildiğinden emin olun.**

Azure portal [önyükleme tanılamasında](../troubleshooting/boot-diagnostics.md) ekran görüntüsünü DENETLEDIĞINIZDE, VM 'nin normal şekilde önyüklenmesini ve oturum açma ekranında kimlik bilgileri beklediğini görürsünüz.

## <a name="cause"></a>Nedeni

VM, Windows içindeki ağ arabiriminde tanımlı bir statik IP adresine sahiptir. Bu IP adresi, Azure portal tanımlanan adresten farklıdır.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin işletim sistemi diskinin bir yedek olarak anlık görüntüsünü alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

Bu sorunu çözmek için seri denetim 'i kullanarak VM için DHCP 'yi etkinleştirin veya [ağ arabirimini sıfırlayın](reset-network-interface.md) .

### <a name="use-serial-control"></a>Seri denetim kullan

1. [Seri konsoluna bağlanın ve cmd örneğini açın](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). VM 'niz üzerinde seri konsol etkinleştirilmemişse, bkz. [ağ arabirimini sıfırlama](reset-network-interface.md).
2. Ağ arabiriminde DHCP 'nin devre dışı olup olmadığını denetleyin:

        netsh interface ip show config
3. DHCP devre dışıysa, ağ arabiriminize ait yapılandırmayı DHCP kullanmak üzere döndürür:

        netsh interface ip set address name="<NIC Name>" source=dhc

    Örneğin, "Ethernet 2"-Work arabirim adları ise aşağıdaki komutu çalıştırın:

        netsh interface ip set address name="Ethernet 2" source=dhc

4. Ağ arabiriminin artık doğru ayarlandığından emin olmak için IP yapılandırmasını tekrar sorgulayın. Yeni IP adresi, Azure tarafından sağlanmalı bir adresle eşleşmelidir.

        netsh interface ip show config

    Bu noktada VM 'yi yeniden başlatmanız gerekmez. VM 'ye erişilecektir.

Bundan sonra, VM için statik IP 'yi yapılandırmak istiyorsanız bkz. [BIR VM için STATIK IP adreslerini yapılandırma](../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md).