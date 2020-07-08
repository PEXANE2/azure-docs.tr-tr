---
title: Azure sanal makinelerine RDP bağlantısı yaptığınızda bir iç hata oluşur | Microsoft Docs
description: Microsoft Azure 'de RDP iç hatalarında sorun gidermeyi öğrenin. | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/22/2018
ms.author: genli
ms.openlocfilehash: 8046e4f42db50db15c840a13b95ae1f3620a8c7f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84703800"
---
#  <a name="an-internal-error-occurs-when-you-try-to-connect-to-an-azure-vm-through-remote-desktop"></a>Azure VM'ye Uzak Masaüstü ile bağlanmaya çalıştığınızda dahili hata oluşuyor

Bu makalede, Microsoft Azure ' de bir sanal makineye (VM) bağlanmaya çalıştığınızda karşılaşabileceğiniz bir hata açıklanır.


## <a name="symptoms"></a>Belirtiler

Uzak Masaüstü Protokolü 'nü (RDP) kullanarak bir Azure VM 'sine bağlanamazsınız. Bağlantı, "uzaktan yapılandırma" bölümünde takılmış olur veya aşağıdaki hata iletisini alırsınız:

- RDP iç hatası
- Bir iç hata oluştu
- Bu bilgisayar uzak bilgisayara bağlanamıyor. Yeniden bağlanmayı deneyin. Sorun devam ederse, uzak bilgisayarın sahibine veya ağ yöneticinize başvurun


## <a name="cause"></a>Nedeni

Bu sorun aşağıdaki nedenlerden kaynaklanabilir:

- Yerel RSA şifreleme anahtarlarına erişilemiyor.
- TLS protokolü devre dışı bırakıldı.
- Sertifika bozuk veya zaman aşımına uğradı.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin işletim sistemi diskinin bir yedek olarak anlık görüntüsünü alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

Bu sorunu gidermek için, sanal makinenin işletim sistemi diskini bir kurtarma sanal makinesine ekleyerek seri konsolunu kullanın veya [VM 'yi çevrimdışı onarın](#repair-the-vm-offline) .


### <a name="use-serial-control"></a>Seri denetim kullan

[Seri konsoluna bağlanın ve PowerShell örneğini açın](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
). VM 'niz üzerinde seri konsol etkinleştirilmemişse, [sanal makineyi çevrimdışı olarak Onar](#repair-the-vm-offline) bölümüne gidin.

#### <a name="step-1-check-the-rdp-port"></a>Adım: 1 RDP bağlantı noktasını denetleyin

1. Bir PowerShell örneğinde, bağlantı noktası 8080 ' nin diğer uygulamalar tarafından kullanılıp kullanılmadığını denetlemek için [netstat](https://docs.microsoft.com/windows-server/administration/windows-commands/netstat
) kullanın:

        Netstat -anob |more
2. Termservice.exe 8080 bağlantı noktası kullanıyorsa adım 2 ' ye gidin. Termservice.exe dışında başka bir hizmet veya uygulama 8080 bağlantı noktası kullanıyorsa, aşağıdaki adımları izleyin:

    1. 3389 hizmetini kullanan uygulama için hizmeti durdurun:

            Stop-Service -Name <ServiceName> -Force

    2. Terminal hizmetini başlatın:

            Start-Service -Name Termservice

2. Uygulama durdurulamazsa veya bu yöntem size uygulanmamışsa, RDP için bağlantı noktasını değiştirin:

    1. Bağlantı noktasını değiştirin:

            Set-ItemProperty -Path 'HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name PortNumber -value <Hexportnumber>

            Stop-Service -Name Termservice -Force
            
            Start-Service -Name Termservice 

    2. Yeni bağlantı noktası için güvenlik duvarını ayarlayın:

            Set-NetFirewallRule -Name "RemoteDesktop-UserMode-In-TCP" -LocalPort <NEW PORT (decimal)>

    3. Azure portal RDP bağlantı noktasındaki [Yeni bağlantı noktası için ağ güvenlik grubunu güncelleştirin](../../virtual-network/security-overview.md) .

#### <a name="step-2-set-correct-permissions-on-the-rdp-self-signed-certificate"></a>2. Adım: RDP otomatik olarak imzalanan sertifika üzerinde doğru izinleri ayarlama

1.  Bir PowerShell örneğinde, RDP otomatik olarak imzalanan sertifikayı yenilemek için aşağıdaki komutları tek tek çalıştırın:

        Import-Module PKI 
    
        Set-Location Cert:\LocalMachine 
        
        $RdpCertThumbprint = 'Cert:\LocalMachine\Remote Desktop\'+((Get-ChildItem -Path 'Cert:\LocalMachine\Remote Desktop\').thumbprint) 
        
        Remove-Item -Path $RdpCertThumbprint

        Stop-Service -Name "SessionEnv"

        Start-Service -Name "SessionEnv"

2. Bu yöntemi kullanarak sertifikayı yenileyemezseniz, RDP otomatik imzalı sertifikasını uzaktan yenilemeyi deneyin:

    1. Sorun yaşayan VM 'ye bağlantısı olan çalışan bir VM 'den, Microsoft Yönetim Konsolu 'Nu açmak için **Çalıştır** kutusuna **MMC** yazın.
    2. **Dosya** menüsünde, **ek bileşen Ekle/Kaldır**' ı seçin, **Sertifikalar**' ı seçin ve ardından **Ekle**' yi seçin.
    3. **Bilgisayar hesapları**' nı seçin, **başka bir bilgisayar**seçin ve ardından sorun sanal makinesinin IP adresini ekleyin.
    4. **Uzak Desktop\Certificates** klasörüne gidin, sertifikaya sağ tıklayın ve **Sil**' i seçin.
    5. Seri konsolundaki bir PowerShell örneğinde, Uzak Masaüstü yapılandırma hizmetini yeniden başlatın:

            Stop-Service -Name "SessionEnv"

            Start-Service -Name "SessionEnv"
3. MachineKeys klasörünün iznini sıfırlayın.

        remove-module psreadline icacls

        md c:\temp

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt 
        
        takeown /f "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt 
        
        Restart-Service TermService -Force

4. VM 'yi yeniden başlatın ve ardından sanal makineye Uzak Masaüstü bağlantısı başlatmayı deneyin. Hata yine oluşursa, sonraki adıma gidin.

#### <a name="step-3-enable-all-supported-tls-versions"></a>3. Adım: desteklenen tüm TLS sürümlerini etkinleştirme

RDP istemcisi varsayılan protokol olarak TLS 1,0 kullanır. Ancak bu, yeni standart haline gelen TLS 1,1 olarak değiştirilebilir. VM 'de TLS 1,1 devre dışıysa bağlantı başarısız olur.
1.  Bir CMD örneğinde TLS protokolünü etkinleştirin:

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f
2.  AD ilkesinin değişikliklerin üzerine yazmasını engellemek için, Grup ilkesi güncelleştirmesini geçici olarak durdurun:

        REG add "HKLM\SYSTEM\CurrentControlSet\Services\gpsvc" /v Start /t REG_DWORD /d 4 /f
3.  Değişikliklerin etkili olması için sanal makineyi yeniden başlatın. Sorun çözümlenirse, Grup ilkesini yeniden etkinleştirmek için aşağıdaki komutu çalıştırın:

        sc config gpsvc start= auto sc start gpsvc

        gpupdate /force
    Değişiklik geri çevrilse, şirket etki alanında Active Directory bir ilke olduğu anlamına gelir. Bu sorunun yeniden oluşmasını önlemek için bu ilkeyi değiştirmeniz gerekir.

### <a name="repair-the-vm-offline"></a>VM 'yi çevrimdışı onarma

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM 'sine iliştirme

1. [İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin](../windows/troubleshoot-recovery-disks-portal.md).
2. İşletim sistemi diski kurtarma VM 'sine eklendikten sonra, diskin Disk Yönetimi konsolunda **çevrimiçi** olarak işaretlendiğinden emin olun. Bağlı işletim sistemi diskine atanan sürücü harfini unutmayın.
3. Kurtarma VM 'sine bir Uzak Masaüstü bağlantısı başlatın.

#### <a name="enable-dump-log-and-serial-console"></a>Döküm günlüğünü ve seri konsolunu etkinleştir

Döküm günlüğünü ve seri konsolunu etkinleştirmek için aşağıdaki betiği çalıştırın.

1. Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**).
2. Şu betiği çalıştırın:

    Bu betikte, bağlı işletim sistemi diskine atanan sürücü harfinin F olduğunu varsaytık. Bu sürücü harfini VM 'niz için uygun değerle değiştirin.

    ```
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

    REM Enable Serial Console
    bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
    bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
    bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
    bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

    REM Suggested configuration to enable OS Dump
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

#### <a name="reset-the-permission-for-machinekeys-folder"></a>MachineKeys klasörü için izinleri sıfırlayın

1. Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**).
2. Aşağıdaki komut dosyasını çalıştırın. Bu betikte, bağlı işletim sistemi diskine atanan sürücü harfinin F olduğunu varsaytık. Bu sürücü harfini VM 'niz için uygun değerle değiştirin.

        Md F:\temp

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt
        
        takeown /f "F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt

#### <a name="enable-all-supported-tls-versions"></a>Desteklenen tüm TLS sürümlerini etkinleştir

1.  Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**) ve aşağıdaki komutları çalıştırın. Aşağıdaki betik, sürücü harfinin bağlı işletim sistemi diskine atandığını varsayar. Bu sürücü harfini VM 'niz için uygun değerle değiştirin.
2.  Hangi TLS 'nin etkin olduğunu denetleyin:

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWO

3.  Anahtar yoksa veya değeri **0**ise, aşağıdaki komut dosyalarını çalıştırarak Protokolü etkinleştirin:

        REM Enable TLS 1.0, TLS 1.1 and TLS 1.2

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

4.  NLA 'yı etkinleştir:

        REM Enable NLA

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f reg unload HKLM\BROKENSYSTEM
5.  [İşletim sistemi diskini ayırın ve VM 'yi yeniden oluşturun](../windows/troubleshoot-recovery-disks-portal.md)ve sorunun çözümlenip çözümlenmediğini denetleyin.





