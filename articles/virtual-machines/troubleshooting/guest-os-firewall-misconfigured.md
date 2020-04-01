---
title: Azure VM Guest OS güvenlik duvarı yanlış yapılandırılmıştır | Microsoft Dokümanlar
description: Uzak bir Azure VM'de yanlış yapılandırılmış konuk işletim sistemi güvenlik duvarını tanılamak ve düzeltmek için Seri Konsol veya çevrimdışı yöntemi nasıl kullanacağınızı öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/22/2018
ms.author: delhan
ms.openlocfilehash: e6f42bdf462ac5261f77bc05c62e50500345fe37
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422545"
---
# <a name="azure-vm-guest-os-firewall-is-misconfigured"></a>Azure VM konuk işletim sistemi güvenlik duvarı yanlış yapılandırılmıştır

Bu makalede, Azure VM'de yanlış yapılandırılmış konuk işletim sistemi güvenlik duvarının nasıl düzeltilene açıklanmıştır.

## <a name="symptoms"></a>Belirtiler

1.  Sanal makine (VM) Karşılama ekranı VM'nin tamamen yüklendiğini gösterir.

2.  Konuk işletim sisteminin nasıl yapılandırıldığına bağlı olarak, VM'ye ulaşan ağ trafiğinin bir kısmı veya hiç olmaması olabilir.

## <a name="cause"></a>Nedeni

Konuk sistem güvenlik duvarının yanlış yapılandırılması VM'ye olan ağ trafiğinin bir kısmını veya her türlüsini engelleyebilir.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, yedek olarak etkilenen VM'nin sistem diskinin anlık görüntüsünü alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.

Bu sorunu gidermek için Seri Konsolu'nu kullanın veya [VM'nin](troubleshoot-rdp-internal-error.md#repair-the-vm-offline) sistem diskini kurtarma VM'ine takarak VM çevrimdışı onarımını tamamlayın.

## <a name="online-mitigations"></a>Çevrimiçi azaltıcı etkenler

[Seri Konsol'a bağlanın ve ardından bir PowerShell örneğini açın.](serial-console-windows.md#use-cmd-or-powershell-in-serial-console) Seri Konsol VM'de etkinleştirilmezse, aşağıdaki Azure makalesinin "VM Çevrimdışı Onar" bölümüne gidin:

 [Azure VM'ye Uzak Masaüstü ile bağlanmaya çalıştığınızda dahili hata oluşuyor](troubleshoot-rdp-internal-error.md#repair-the-vm-offline)

VM'ye erişimi etkinleştirmek (RDP aracılığıyla) veya daha kolay bir sorun giderme deneyimi sağlamak için aşağıdaki kurallar düzenlenebilir:

*   Uzak Masaüstü (TCP-In): Azure'da RDP'ye izin vererek VM'ye birincil erişim sağlayan standart kuraldır.

*   Windows Uzaktan Yönetim (HTTP-In): Bu kural, PowerShell., Azure'da bu tür bir erişim kullanarak VM'ye bağlanmanızı sağlar ve bu tür bir erişim, uzaktan komut dosyası kullanma ve sorun giderme özelliğini kullanmanızı sağlar.

*   Dosya ve Yazıcı Paylaşımı (SMB-In): Bu kural, sorun giderme seçeneği olarak ağ paylaşımı erişimine olanak tanır.

*   Dosya ve Yazıcı Paylaşımı (Yankı İsteği - ICMPv4-In): Bu kural VM ping yapmanızı sağlar.

Seri Konsol Erişimi örneğinde, güvenlik duvarı kuralının geçerli durumunu sorgulayabilirsiniz.

*   Görüntü Adı'nı parametre olarak kullanarak sorgula:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(DisplayName.*<FIREWALL RULE NAME>)" -context 9,4 | more
    ```

*   Uygulamanın kullandığı Yerel Bağlantı Noktası'nı kullanarak sorgula:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalPort.*<APPLICATION PORT>)" -context 9,4 | more
    ```

*   Uygulamanın kullandığı Yerel IP adresini kullanarak sorgula:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalIP.*<CUSTOM IP>)" -context 9,4 | more
    ```

*   Kuralın devre dışı bırakıldığını görürseniz, aşağıdaki komutu çalıştırarak kuralı etkinleştirebilirsiniz:

    ```cmd
    netsh advfirewall firewall set rule name="<RULE NAME>" new enable=yes
    ```

*   Sorun giderme için güvenlik duvarı profillerini KAPAT'a çevirebilirsiniz:

    ```cmd
    netsh advfirewall set allprofiles state off
    ```

    Güvenlik duvarını doğru ayarlamak için bunu yaparsanız, sorun giderme işleminizi tamamladıktan sonra güvenlik duvarını yeniden etkinleştirin.

    > [!Note]
    > Bu değişikliği uygulamak için VM'yi yeniden başlatmanız gerekmez.

*   RDP aracılığıyla VM'ye bağlanmak için yeniden deneyin.

### <a name="offline-mitigations"></a>Çevrimdışı Azaltıcı Etazaltmalar

1.  Güvenlik duvarı kurallarını etkinleştirmek veya devre dışı kalmak için [Azure VM Konuk İşletim Sistemi'nde güvenlik duvarı kuralını etkinleştirmeye veya devre dışı etme](enable-disable-firewall-rule-guest-os.md)kuralına bakın.

2.  [Gelen trafik senaryosunu engelleyen Konuk İşletim Sistemi güvenlik duvarında](guest-os-firewall-blocking-inbound-traffic.md)olup olmadığınızı denetleyin.

3.  Güvenlik duvarının erişiminizi engelleyip engellemediği konusunda hala şüpheniz varsa, [Azure VM'deki konuk Işletim Sistemi Güvenlik Duvarını Devre Dışı](disable-guest-os-firewall-windows.md)Bırak'a bakın ve ardından doğru kuralları kullanarak konuk sistem güvenlik duvarını yeniden etkinleştirin.

