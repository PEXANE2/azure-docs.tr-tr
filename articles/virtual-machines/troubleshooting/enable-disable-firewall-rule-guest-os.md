---
title: Azure VM'de konuk işletim sistemi üzerinde bir güvenlik duvarı kuralını etkinleştirme veya devre dışı kılma | Microsoft Dokümanlar
description: ''
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
ms.openlocfilehash: 782240c51833fc841af9f4260860db4c03897c03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71086445"
---
# <a name="enable-or-disable-a-firewall-rule-on-an-azure-vm-guest-os"></a>Azure VM konuk işletim sisteminde güvenlik duvarı kuralını etkinleştirin veya devre dışı bırakın

Bu makalede, konuk işletim sistemi güvenlik duvarının sanal bir makinedeki (VM) kısmi trafiği filtrelediğinden şüphelendiğiniz bir durumu giderme sorunu giderme için bir başvuru sağlar. Bu, aşağıdaki nedenlerden dolayı yararlı olabilir:

*   RDP bağlantılarının başarısız olması yla ilgili güvenlik duvarında kasıtlı olarak bir değişiklik yapıldıysa, Özel Komut Dosyası Uzantısı özelliğini kullanmak sorunu çözebilir.

*   Tüm güvenlik duvarı profillerini devre dışı bırakmak, RDP'ye özgü güvenlik duvarı kuralını ayarlamaktan daha kusursuz bir sorun giderme yoludur.

## <a name="solution"></a>Çözüm

Güvenlik duvarı kurallarını nasıl yapılandırdığınız, gerekli VM'ye erişim düzeyine bağlıdır. Aşağıdaki örneklerDE RDP kuralları kullanılır. Ancak, aynı yöntemler doğru kayıt defteri anahtarıişaret ederek trafik başka tür uygulanabilir.

### <a name="online-troubleshooting"></a>Çevrimiçi sorun giderme 

#### <a name="mitigation-1-custom-script-extension"></a>Azaltma 1: Özel Komut Dosyası Uzantısı

1.  Aşağıdaki şablonu kullanarak komut dosyanızı oluşturun.

    *   Bir kuralı etkinleştirmek için:
        ```cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=yes
        ```

    *   Bir kuralı devre dışı kalmak için:
        ```cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=no
        ```

2.  Bu komut dosyasını Özel [Komut Dosyası Uzantısı](../extensions/custom-script-windows.md) özelliğini kullanarak Azure portalına yükleyin. 

#### <a name="mitigation-2-remote-powershell"></a>Azaltma 2: Uzaktan PowerShell

VM çevrimiçiyse ve aynı sanal ağdaki başka bir VM'den erişebiliyorsa, diğer VM'yi kullanarak takip azaltmalarını yapabilirsiniz.

1.  Sorun giderme VM'de bir PowerShell konsol penceresi açın.

2.  Aşağıdaki komutları uygun şekilde çalıştırın.

    *   Bir kuralı etkinleştirmek için:
        ```powershell
        Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
        Enable-NetFirewallRule -DisplayName  "RemoteDesktop-UserMode-In-TCP"
        exit
        ```

    *   Bir kuralı devre dışı kalmak için:
        ```powershell
        Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
        Disable-NetFirewallRule -DisplayName  "RemoteDesktop-UserMode-In-TCP"
        exit
        ```

#### <a name="mitigation-3-pstools-commands"></a>Azaltma 3: PSTools komutları

VM çevrimiçiyse ve aynı sanal ağdaki başka bir VM'den erişebiliyorsa, diğer VM'yi kullanarak takip azaltmalarını yapabilirsiniz.

1.  Sorun giderme VM, [PSTools](https://docs.microsoft.com/sysinternals/downloads/pstools)indirin.

2.  CmD örneğini açın ve İç IP 'si (DIP) aracılığıyla VM'ye erişin. 

    * Bir kuralı etkinleştirmek için:
        ```cmd
        psexec \\<DIP> -u <username> cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=yes
        ```

    *   Bir kuralı devre dışı kalmak için:
        ```cmd
        psexec \\<DIP> -u <username> cmd
        netsh advfirewall firewall set rule dir=in name="Remote Desktop - User Mode (TCP-In)" new enable=no
        ```

#### <a name="mitigation-4-remote-registry"></a>Azaltma 4: Uzaktan Kayıt Defteri

VM çevrimiçiyse ve aynı sanal ağdaki başka bir VM'den erişebiliyorsa, diğer VM'de [Uzaktan Kayıt Defteri'ni](https://support.microsoft.com/help/314837/how-to-manage-remote-access-to-the-registry) kullanabilirsiniz.

1.  Sorun giderme VM'de Kayıt Defteri Düzenleyicisi'ni (regedit.exe) başlatın ve ardından **Dosya** > **Bağlan Ağ Kayıt Defteri'ni**seçin.

2.  TARGET *MACHINE*\SYSTEM dalını açın ve ardından aşağıdaki değerleri belirtin:

    * Bir kuralı etkinleştirmek için aşağıdaki kayıt defteri değerini açın:
    
        *HEDEF MAKİnE*\System\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP
    
        Daha **sonra, Active=FALSE'u** **Active=TRUE** dizesinde değiştirin:

        **v2.22| Eylem=İzin Ver| Etkin=TRUE| dir=In| Protokol=6| Profil=Etki Alanı| Profil=Özel| Profil=Genel| LPort=3389| App=%SystemRoot%\system32\svchost.exe| Svc=termservice| Adı=\@FirewallAPI.dll,-28775| Desc=\@FirewallAPI.dll,-28756| EmbedCtxt=\@FirewallAPI.dll,-28752|**
    
    * Bir kuralı devre dışı düşürmek için aşağıdaki kayıt defteri değerini açın:
    
        *HEDEF MAKİnE*\System\CurrentControlSet\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP

        Sonra, **Active =TRUE** to **Active=FALSE'u**değiştirin :
        
        **v2.22| Eylem=İzin Ver| Etkin=YANLIŞ| dir=In| Protokol=6| Profil=Etki Alanı| Profil=Özel| Profil=Genel| LPort=3389| App=%SystemRoot%\system32\svchost.exe| Svc=termservice| Adı=\@FirewallAPI.dll,-28775| Desc=\@FirewallAPI.dll,-28756| EmbedCtxt=\@FirewallAPI.dll,-28752|**

3.  Değişiklikleri uygulamak için VM'yi yeniden başlatın.

### <a name="offline-troubleshooting"></a>Çevrimdışı sorun giderme 

VM'ye herhangi bir yöntemle erişemiyorsanız, Özel Komut Dosyası Uzantısı'nı kullanmak başarısız olur ve doğrudan sistem diski üzerinden çalışarak ÇEVRIMDıŞı modda çalışmanız gerekir.

Bu adımları izlemeden önce, yedek olarak etkilenen VM'nin sistem diskinin anlık görüntüsünü alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.

1.  [Sistem diskini kurtarma VM'ine takın.](troubleshoot-recovery-disks-portal-windows.md)

2.  Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın.

3.  Diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın. Ekli sistem diskine atanan sürücü mektubunun.

4.  Herhangi bir değişiklik yapmadan önce, değişikliklerin geri alınması nın gerekli olması durumunda \windows\system32\config klasörünün bir kopyasını oluşturun.

5.  Sorun giderme VM'de Kayıt Defteri Düzenleyicisi'ni (regedit.exe) başlatın.

6.  **HKEY_LOCAL_MACHINE** tuşunu vurgulayın ve menüden **Dosya** > **Yük Kovanı'nı** seçin.

    ![Regedit](./media/enable-or-disable-firewall-rule-guest-os/load-registry-hive.png)

7.  \windows\system32\config\SYSTEM dosyasını bulun ve açın. 

    > [!Note]
    > Bir ad için istenir. **BROKENSYSTEM'i**girin ve **ardından HKEY_LOCAL_MACHINE**genişletin. Şimdi **BROKENSYSTEM**adlı ek bir anahtar göreceksiniz. Bu sorun giderme için, bu sorun kovanlarını **BROKENSYSTEM**olarak monte ediyoruz.

8.  BROKENSYSTEM dalında aşağıdaki değişiklikleri yapın:

    1.  VM'nin hangi **ControlSet** kayıt defteri anahtarından başladığını denetleyin. HkLM\BROKENSYSTEM\Select\Current'da anahtar numarasını göreceksiniz.

    2.  Bir kuralı etkinleştirmek için aşağıdaki kayıt defteri değerini açın:
    
        HKLM\BROKENSYSTEM\ControlSet00X\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP
        
        Daha sonra **Active=FALSE'u** **Active=True**olarak değiştirin.
        
        **v2.22| Eylem=İzin Ver| Etkin=TRUE| dir=In| Protokol=6| Profil=Etki Alanı| Profil=Özel| Profil=Genel| LPort=3389| App=%SystemRoot%\system32\svchost.exe| Svc=termservice| Adı=\@FirewallAPI.dll,-28775| Desc=\@FirewallAPI.dll,-28756| EmbedCtxt=\@FirewallAPI.dll,-28752|**

    3.  Bir kuralı devre dışı kardırmak için şeşeşit anahtarını aç

        HKLM\BROKENSYSTEM\ControlSet00X\Services\SharedAccess\Parameters\FirewallPolicy\FirewallRules\RemoteDesktop-UserMode-In-TCP

        Sonra, **Active=True** to **Active=FALSE**'u değiştirin.
        
        **v2.22| Eylem=İzin Ver| Etkin=YANLIŞ| dir=In| Protokol=6| Profil=Etki Alanı| Profil=Özel| Profil=Genel| LPort=3389| App=%SystemRoot%\system32\svchost.exe| Svc=termservice| Adı=\@FirewallAPI.dll,-28775| Desc=\@FirewallAPI.dll,-28756| EmbedCtxt=\@FirewallAPI.dll,-28752|**

9.  **BROKENSYSTEM'i**vurgulayın ve ardından menüden **Dosya** > **Boşaltma Kovanı'nı** seçin.

10. [Sistem diskini ayırın ve VM'yi yeniden oluşturun.](troubleshoot-recovery-disks-portal-windows.md)

11. Sorunun çözülüp çözülmediğini denetleyin.
