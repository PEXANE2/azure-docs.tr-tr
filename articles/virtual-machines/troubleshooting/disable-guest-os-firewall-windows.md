---
title: Azure VM'de konuk Işletim Sistemi Güvenlik Duvarını devre dışı söyler | Microsoft Dokümanlar
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
ms.openlocfilehash: dbb9b0f865c7ec5d9d29e2310ae41abbec287bd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79464971"
---
# <a name="disable-the-guest-os-firewall-in-azure-vm"></a>Azure VM'de konuk işletim sistemi Güvenlik Duvarını devre dışı bırakma

Bu makalede, konuk işletim sistemi güvenlik duvarının sanal bir makineye (VM) kısmi veya tam trafiği filtrelediğinden şüphelendiğiniz durumlar için bir başvuru sağlar. RdP bağlantılarının başarısız olması nedeniyle güvenlik duvarında kasıtlı olarak değişiklikler yapılırsa bu durum oluşabilir.

## <a name="solution"></a>Çözüm

Bu makalede açıklanan işlem, güvenlik duvarı kurallarını doğru şekilde ayarlama nın nasıl olduğu yla gerçek sorununuzu düzeltmeye odaklanabilmeniz için geçici çözüm olarak kullanılmak üzere tasarlanmıştır. Windows Güvenlik Duvarı bileşeninin etkinleştirilmesi microsoft en iyi uygulamadır. Güvenlik duvarı kurallarını nasıl yapılandırdığınız, gerekli VM'ye erişim düzeyine bağlıdır.

### <a name="online-solutions"></a>Online Çözümler 

VM çevrimiçiyse ve aynı sanal ağdaki başka bir VM'den erişebiliyorsa, bu azaltıcı etkenleri diğer VM'yi kullanarak yapabilirsiniz.

#### <a name="mitigation-1-custom-script-extension-or-run-command-feature"></a>Azaltma 1: Özel Komut Uzantısı veya Run Komutu özelliği

Çalışan bir Azure aracınız varsa, aşağıdaki komut dosyalarını uzaktan çalıştırmak için [Özel Komut Uzantısı](../extensions/custom-script-windows.md) veya Çalıştır [Komutları](../windows/run-command.md) özelliğini (yalnızca Kaynak Yöneticisi VM'leri) kullanabilirsiniz.

> [!Note]
> * Güvenlik duvarı yerel olarak ayarlanmışsa, aşağıdaki komut dosyasını çalıştırın:
>   ```
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\Standardprofile' -name "EnableFirewall" -Value 0 
>   Restart-Service -Name mpssvc
>   ```
> * Güvenlik duvarı Etkin Dizin ilkesi yle ayarlanmışsa, geçici erişim için aşağıdaki komut dosyasını çalıştır'ı kullanabilirsiniz. 
>   ```
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\DomainProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\PublicProfile' -name "EnableFirewall" -Value 0
>   Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\WindowsFirewall\StandardProfile' name "EnableFirewall" -Value 0
>   Restart-Service -Name mpssvc
>   ```
>   Ancak, ilke yeniden uygulanır uygulanmaz, uzak oturumdan atılırsınız. Bu sorunun kalıcı düzeltmesi, bu bilgisayarda uygulanan ilkeyi değiştirmektir.

#### <a name="mitigation-2-remote-powershell"></a>Azaltma 2: Uzaktan PowerShell

1.  RDP bağlantısını kullanarak ulaşamayacağınız VM ile aynı sanal ağda bulunan bir VM'ye bağlanın.

2.  PowerShell konsol pencereyi açın.

3.  Aşağıdaki komutları çalıştırın:

    ```powershell
    Enter-PSSession (New-PSSession -ComputerName "<HOSTNAME>" -Credential (Get-Credential) -SessionOption (New-PSSessionOption -SkipCACheck -SkipCNCheck)) 
    netsh advfirewall set allprofiles state off
    Restart-Service -Name mpssvc 
    exit
    ```

> [!Note]
> Güvenlik duvarı Bir Grup İlkesi Nesnesi üzerinden ayarlanmışsa, bu komut yalnızca yerel kayıt defteri girişlerini değiştirdiğinden bu yöntem çalışmayabilir. Bir ilke varsa, bu değişikliği geçersiz kılar. 

#### <a name="mitigation-3-pstools-commands"></a>Azaltma 3: PSTools komutları

1.  Sorun giderme VM, [PSTools](https://docs.microsoft.com/sysinternals/downloads/pstools)indirin.

2.  CmD örneğini açın ve ardından DY'si aracılığıyla VM'ye erişin.

3.  Aşağıdaki komutları çalıştırın:

    ```cmd
    psexec \\<DIP> -u <username> cmd
    netsh advfirewall set allprofiles state off
    psservice restart mpssvc
    ```

#### <a name="mitigation-4-remote-registry"></a>Azaltma 4: Uzaktan Kayıt Defteri 

[Uzaktan Kayıt Defteri'ni](https://support.microsoft.com/help/314837/how-to-manage-remote-access-to-the-registry)kullanmak için aşağıdaki adımları izleyin.

1.  Sorun giderme VM'de kayıt defteri düzenleyicisini başlatın ve ardından **File** > **Connect Ağ Kayıt Defteri'ne**gidin.

2.  *TARGET MACHINE*\SYSTEM dalını açın ve aşağıdaki değerleri belirtin:

    ```
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile\EnableFirewall           -->        0 
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile\EnableFirewall           -->        0 
    <TARGET MACHINE>\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile\EnableFirewall         -->        0
    ```

3.  Hizmeti yeniden başlatın. Bunu uzak kayıt defterini kullanarak yapamayacağınız için Uzaktan Hizmet Konsolu'nu kullanmanız gerekir.

4.  **Services.msc**bir örneğini açın.

5.  **Hizmetler (Yerel) seçeneğini**tıklatın.

6.  **Başka bir bilgisayara bağlan'ı**seçin.

7.  Sorun VM'nin **Özel IP Adresini (DIP)** girin.

8.  Yerel güvenlik duvarı ilkesini yeniden başlatın.

9.  Yerel bilgisayarınızdan RDP aracılığıyla VM'ye yeniden bağlanmayı deneyin.

### <a name="offline-solutions"></a>Çevrimdışı Çözümler 

Herhangi bir yöntemle VM'ye erişemediğiniz bir durum varsa, Özel Komut Dosyası Uzantısı başarısız olur ve doğrudan sistem diski üzerinden çalışarak ÇEVRIMDıŞı modda çalışmak zorunda kaldığınız zaman. Bunu yapmak için şu adımları uygulayın:

1.  [Sistem diskini kurtarma VM'ine takın.](troubleshoot-recovery-disks-portal-windows.md)

2.  Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın.

3.  Diskin Disk Yönetimi konsolunda Çevrimiçi olarak işaretlendiğini unutmayın. Ekli sistem diskine atanan sürücü mektubuna dikkat edin.

4.  Herhangi bir değişiklik yapmadan önce, değişikliklerin geri alınması nın gerekli olması durumunda \windows\system32\config klasörünün bir kopyasını oluşturun.

5.  Sorun giderme VM'de kayıt defteri düzenleyicisini (regedit.exe) başlatın. 

6.  Bu sorun giderme işlemi için kovanları BROKENSYSTEM ve BROKENSOFTWARE olarak monte ediyoruz.

7.  HKEY_LOCAL_MACHINE tuşunu vurgulayın ve menüden Dosya > Yük Kovanı'nı seçin.

8.  \windows\system32\config\SYSTEM dosyasını ekli sistem diskinde bulun.

9.  Yükseltilmiş bir PowerShell örneğini açın ve ardından aşağıdaki komutları çalıştırın:

    ```cmd
    # Load the hives - If your attached disk is not F, replace the letter assignment here
    reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM
    reg load HKLM\BROKENSOFTWARE f:\windows\system32\config\SOFTWARE 
    # Disable the firewall on the local policy
    $ControlSet = (get-ItemProperty -Path 'HKLM:\BROKENSYSTEM\Select' -name "Current").Current
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'BROKENSYSTEM\ControlSet00'+$ControlSet+'\services\SharedAccess\Parameters\FirewallPolicy\StandardProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    # To ensure the firewall is not set through AD policy, check if the following registry entries exist and if they do, then check if the following entries exist:
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\DomainProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\PublicProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    $key = 'HKLM:\BROKENSOFTWARE\Policies\Microsoft\WindowsFirewall\StandardProfile'
    Set-ItemProperty -Path $key -name 'EnableFirewall' -Value 0 -Type Dword -force
    # Unload the hives
    reg unload HKLM\BROKENSYSTEM
    reg unload HKLM\BROKENSOFTWARE
    ```

10. [Sistem diskini ayırın ve VM'yi yeniden oluşturun.](troubleshoot-recovery-disks-portal-windows.md)

11. Sorunun çözülüp çözülmediğini denetleyin.
