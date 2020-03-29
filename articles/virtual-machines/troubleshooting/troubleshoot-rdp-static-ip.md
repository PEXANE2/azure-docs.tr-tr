---
title: Statik IP| Microsoft Dokümanlar
description: Microsoft Azure'daki statik IP'nin neden olduğu RDP sorununu nasıl gidereceklerini öğrenin.| Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77918198"
---
#  <a name="cannot-remote-desktop-to-azure-virtual-machines-because-of-static-ip"></a>Statik IP nedeniyle masaüstünü Azure Sanal Makineleri'ne uzaklamıyor

Bu makalede, statik bir IP VM'de yapılandırıldıktan sonra masaüstünü Azure Windows Sanal Makineleri'ne (VM) uzaklatamayabileceğiniz bir sorun açıklanmaktadır.


## <a name="symptoms"></a>Belirtiler

Azure'da bir VM'ye RDP bağlantısı yaptığınızda aşağıdaki hata iletisini alırsınız:

**Uzak Masaüstü aşağıdaki nedenlerden dolayı uzak bilgisayara bağlanamaz:**

1. **Sunucuya uzaktan erişim etkinleştirildi**

2. **Uzak Bilgisayar kapatıldı**

3. **Uzak bilgisayar ağda kullanılamıyor**

**Uzak bilgisayarın açık olduğundan ve ağa bağlı olduğundan ve uzaktan erişimin etkin olduğundan emin olun.**

Azure portalındaki [Önyükleme tanılama](../troubleshooting/boot-diagnostics.md) bölümündeekran görüntüsünü kontrol ettiğinizde, VM önyüklemelerini normal olarak görürsünüz ve giriş ekranında kimlik bilgilerini beklersiniz.

## <a name="cause"></a>Nedeni

VM, Windows içindeki ağ arabiriminde tanımlanan statik bir IP adresine sahiptir. Bu IP adresi, Azure portalında tanımlanan adresten farklıdır.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM'nin işletim sistemi diskinin bir anlık görüntüsünü yedek olarak alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.

Bu sorunu gidermek için, DHCP'yi etkinleştirmek için Seri denetimini kullanın veya VM için [ağ arabirimini sıfırlayın.](reset-network-interface.md)

### <a name="use-serial-control"></a>Seri denetimi kullanma

1. Seri [Konsola bağlanın ve CMD örneğini açın.](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
) Seri Konsol VM'nizde etkinleştirilemiyorsa, [ağ arabirimini sıfırla'ya](reset-network-interface.md)bakın.
2. DHCP'nin ağ arabiriminde devre dışı bırakıldığını denetleyin:

        netsh interface ip show config
3. DHCP devre dışı bırakılırsa, DHCP kullanmak için ağ arabiriminizin yapılandırmasını geri çevirin:

        netsh interface ip set address name="<NIC Name>" source=dhc

    Örneğin, ara birim arabirim "Ethernet 2" adlarını adlandırırsa, aşağıdaki komutu çalıştırın:

        netsh interface ip set address name="Ethernet 2" source=dhc

4. Ağ arabiriminin artık doğru şekilde ayarlandığından emin olmak için IP yapılandırmasını yeniden sorgula. Yeni IP adresi, Azure tarafından sağlanan adresle eşleşmelidir.

        netsh interface ip show config

    Bu noktada VM yeniden başlatmak zorunda değilsiniz. VM'ye tekrar ulaşılabilir olacak.

Bundan sonra, VM için statik IP yapılandırmak istiyorsanız, [bir VM için statik IP adreslerini yapılandırma](../../virtual-network/virtual-networks-static-private-ip-arm-pportal.md)kınızda.