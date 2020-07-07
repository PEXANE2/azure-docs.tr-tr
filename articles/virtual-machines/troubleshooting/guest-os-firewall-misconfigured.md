---
title: Azure VM Konuk işletim sistemi güvenlik duvarı yanlış yapılandırılmış | Microsoft Docs
description: Uzak bir Azure VM 'de yanlış yapılandırılmış bir konuk işletim sistemi güvenlik duvarını tanılamak ve onarmak için seri konsol veya çevrimdışı yöntemi nasıl kullanacağınızı öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80422545"
---
# <a name="azure-vm-guest-os-firewall-is-misconfigured"></a>Azure VM Konuk işletim sistemi güvenlik duvarı hatalı yapılandırılmış

Bu makalede, Azure VM 'de yanlış yapılandırılmış Konuk işletim sistemi güvenlik duvarını nasıl gidereceğiniz açıklanır.

## <a name="symptoms"></a>Belirtiler

1.  Sanal makine (VM) hoş geldiniz ekranı, VM 'nin tam olarak yüklendiğini gösterir.

2.  Konuk işletim sisteminin nasıl yapılandırıldığına bağlı olarak, VM 'ye ulaşan bir ağ trafiği veya hiçbiri olabilir.

## <a name="cause"></a>Nedeni

Konuk sistem güvenlik duvarının bir yanlış yapılandırılması, VM 'ye yönelik bazı veya tüm ağ trafiği türlerini engelleyebilir.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin sistem diskinin bir anlık görüntüsünü yedek olarak alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

Bu sorunu gidermek için, sanal makinenin sistem diskini bir kurtarma VM 'sine ekleyerek seri konsolu 'Nu kullanın veya [VM 'yi çevrimdışı onarın](troubleshoot-rdp-internal-error.md#repair-the-vm-offline) .

## <a name="online-mitigations"></a>Çevrimiçi azaltmaları

[Seri konsoluna bağlanın ve ardından bir PowerShell örneği açın](serial-console-windows.md#use-cmd-or-powershell-in-serial-console). Seri konsol VM 'de etkin değilse, aşağıdaki Azure makalesinin "sanal makineyi çevrimdışı olarak Onar" bölümüne gidin:

 [Azure VM'ye Uzak Masaüstü ile bağlanmaya çalıştığınızda dahili hata oluşuyor](troubleshoot-rdp-internal-error.md#repair-the-vm-offline)

Aşağıdaki kurallar, VM 'ye erişimi etkinleştirmek için (RDP aracılığıyla) veya daha kolay bir sorun giderme deneyimi sağlamak için düzenlenebilir:

*   Uzak Masaüstü (TCP-ın): Bu, Azure 'da RDP 'ye izin vererek VM 'ye birincil erişim sağlayan standart kuraldır.

*   Windows Uzaktan Yönetimi (HTTP-ın): Bu kural, PowerShell 'i kullanarak VM 'ye bağlanmanızı sağlar. Azure 'Da bu tür bir erişim, uzaktan betik oluşturma ve sorun giderme için komut dosyası kullanımını kullanmanıza imkan sağlar.

*   Dosya ve yazıcı paylaşımı (SMB-ın): Bu kural, sorun giderme seçeneği olarak ağ paylaşımı erişimine izin vermez.

*   Dosya ve yazıcı paylaşımı (yankı Isteği-Icmpv4-ın): Bu kural VM 'ye ping yapmanızı sağlar.

Seri konsol erişimi örneğinde, güvenlik duvarı kuralının geçerli durumunu sorgulayabilirsiniz.

*   Görünen adı parametre olarak kullanarak sorgulayın:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(DisplayName.*<FIREWALL RULE NAME>)" -context 9,4 | more
    ```

*   Uygulamanın kullandığı yerel bağlantı noktasını kullanarak sorgulama:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalPort.*<APPLICATION PORT>)" -context 9,4 | more
    ```

*   Uygulamanın kullandığı yerel IP adresini kullanarak sorgulayın:

    ```cmd
    netsh advfirewall firewall show rule dir=in name=all | select-string -pattern "(LocalIP.*<CUSTOM IP>)" -context 9,4 | more
    ```

*   Kuralın devre dışı olduğunu görürseniz, aşağıdaki komutu çalıştırarak etkinleştirebilirsiniz:

    ```cmd
    netsh advfirewall firewall set rule name="<RULE NAME>" new enable=yes
    ```

*   Sorun giderme için, Güvenlik Duvarı profillerini devre dışı bırakabilirsiniz:

    ```cmd
    netsh advfirewall set allprofiles state off
    ```

    Bunu, güvenlik duvarını doğru şekilde ayarlamak için yaparsanız, sorun gidermeyi tamamladıktan sonra güvenlik duvarını yeniden etkinleştirin.

    > [!Note]
    > Bu değişikliği uygulamak için VM 'yi yeniden başlatmanız gerekmez.

*   VM 'ye RDP aracılığıyla bağlanmayı yeniden deneyin.

### <a name="offline-mitigations"></a>Çevrimdışı azaltmaları

1.  Güvenlik duvarı kurallarını etkinleştirmek veya devre dışı bırakmak için [bir Azure VM Konuk işletim sistemindeki güvenlik duvarı kuralını etkinleştirme veya devre dışı bırakma](enable-disable-firewall-rule-guest-os.md)bölümüne bakın.

2.  [Konuk işletim sistemi güvenlik duvarı gelen trafiği engelleme senaryosunda](guest-os-firewall-blocking-inbound-traffic.md)olup olmadığınızı kontrol edin.

3.  Güvenlik duvarının erişiminizi engelleyip engellemediğini hala düşünüyorsanız, [Azure VM 'de Konuk işletim sistemi güvenlik duvarını devre dışı bırakma](disable-guest-os-firewall-windows.md)bölümüne bakın ve ardından doğru kuralları kullanarak Konuk sistem güvenlik duvarını yeniden etkinleştirin.

