---
title: Azure Sanal Makineleri' ne RDP bağlantısı yaptığınızda iç hata oluşur | Microsoft Dokümanlar
description: Microsoft Azure'da RDP iç hatalarının nasıl giderilmeye başlandığını öğrenin.| Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266929"
---
#  <a name="an-internal-error-occurs-when-you-try-to-connect-to-an-azure-vm-through-remote-desktop"></a>Azure VM'ye Uzak Masaüstü ile bağlanmaya çalıştığınızda dahili hata oluşuyor

Bu makalede, Microsoft Azure'da sanal bir makineye (VM) bağlanmaya çalıştığınızda karşılaşabileceğiniz bir hata açıklanmaktadır.


## <a name="symptoms"></a>Belirtiler

Uzak masaüstü iletişim kuralını (RDP) kullanarak Azure VM'ye bağlanamazsınız. Bağlantı "Uzaktan Yapılandırma" bölümüne takılıp kalır veya aşağıdaki hata iletisini alırsınız:

- RDP iç hatası
- İç hata oluştu
- Bu bilgisayar uzak bilgisayara bağlanamıyor. Yeniden bağlanmayı deneyin. Sorun devam ederse, uzak bilgisayarın sahibine veya ağ yöneticinize başvurun


## <a name="cause"></a>Nedeni

Bu sorun aşağıdaki nedenlerle oluşabilir:

- Yerel RSA şifreleme anahtarlarına erişilemiyor.
- TLS protokolü devre dışı bırakıldı.
- Sertifika bozuk veya süresi dolmuş.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM'nin işletim sistemi diskinin bir anlık görüntüsünü yedek olarak alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.

Bu sorunu gidermek için Seri Konsolu'nu kullanın veya [VM'nin](#repair-the-vm-offline) işletim sistemi diskini kurtarma VM'ine takarak VM çevrimdışı onarımını tamamla.


### <a name="use-serial-control"></a>Seri denetimi kullanma

Seri [Konsola bağlanın ve PowerShell örneğini açın.](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
) VM'nizde Seri Konsol etkinleştirilemiyorsa, VM çevrimdışı bölümünü [onarmaya](#repair-the-vm-offline) gidin.

#### <a name="step-1-check-the-rdp-port"></a>Adım: 1 RDP bağlantı noktasını kontrol edin

1. PowerShell örneğinde, port 8080'in diğer uygulamalar tarafından kullanılıp kullanılmadığını kontrol etmek için [NETSTAT'ı](https://docs.microsoft.com/windows-server/administration/windows-commands/netstat
) kullanın:

        Netstat -anob |more
2. Termservice.exe 8080 bağlantı noktası kullanıyorsa, adım 2'ye gidin. Termservice.exe dışındaki başka bir hizmet veya uygulama 8080 bağlantı noktası kullanıyorsa, aşağıdaki adımları izleyin:

    1. 3389 hizmetini kullanan uygulama için hizmeti durdurun:

            Stop-Service -Name <ServiceName> -Force

    2. Terminal hizmetini başlatın:

            Start-Service -Name Termservice

2. Uygulama durdurulamıyorsa veya bu yöntem sizin için geçerli değilse, RDP bağlantı noktasını değiştirin:

    1. Bağlantı noktasını değiştirin:

            Set-ItemProperty -Path 'HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name PortNumber -value <Hexportnumber>

            Stop-Service -Name Termservice -Force
            
            Start-Service -Name Termservice 

    2. Yeni bağlantı noktası için güvenlik duvarını ayarlayın:

            Set-NetFirewallRule -Name "RemoteDesktop-UserMode-In-TCP" -LocalPort <NEW PORT (decimal)>

    3. Azure portalı RDP [bağlantı noktasındaki yeni bağlantı noktası için ağ güvenlik grubunu güncelleştirin.](../../virtual-network/security-overview.md)

#### <a name="step-2-set-correct-permissions-on-the-rdp-self-signed-certificate"></a>Adım 2: RDP kendi imzalı sertifikasında doğru izinleri ayarlama

1.  PowerShell örneğinde, RDP kendi imzalı sertifikasını yenilemek için aşağıdaki komutları birer birer çalıştırın:

        Import-Module PKI 
    
        Set-Location Cert:\LocalMachine 
        
        $RdpCertThumbprint = 'Cert:\LocalMachine\Remote Desktop\'+((Get-ChildItem -Path 'Cert:\LocalMachine\Remote Desktop\').thumbprint) 
        
        Remove-Item -Path $RdpCertThumbprint

        Stop-Service -Name "SessionEnv"

        Start-Service -Name "SessionEnv"

2. Bu yöntemi kullanarak sertifikayı yenileyemiyorsanız, RDP kendi imzalı sertifikayı uzaktan yenilemeyi deneyin:

    1. Sorunlarla karşılaşan VM bağlantısı olan çalışan bir VM'den, Microsoft Yönetim Konsolu'nu açmak için **Çalıştır** kutusuna **mmc** yazın.
    2. **Dosya** menüsünde **Ekle/Kaldır,** **Sertifikalar'ı**seçin ve ardından **Ekle'yi**seçin.
    3. **Bilgisayar hesaplarını**seçin, Başka **Bir Bilgisayar'ı**seçin ve ardından sorun VM'nin IP adresini ekleyin.
    4. **Uzak Masaüstü\Sertifikalar** klasörüne gidin, sertifikayı sağ tıklatın ve sonra **Sil'i**seçin.
    5. Seri Konsol'dan bir PowerShell örneğinde, Uzak Masaüstü Yapılandırma hizmetini yeniden başlatın:

            Stop-Service -Name "SessionEnv"

            Start-Service -Name "SessionEnv"
3. MachineKeys klasörü için izni sıfırla.

        remove-module psreadline icacls

        md c:\temp

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt 
        
        takeown /f "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"

        icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt 
        
        Restart-Service TermService -Force

4. VM'yi yeniden başlatın ve ardından VM'ye Uzak Masaüstü bağlantısını başlat'ı deneyin. Hata hala oluşursa, bir sonraki adıma geçin.

#### <a name="step-3-enable-all-supported-tls-versions"></a>Adım 3: Desteklenen tüm TLS sürümlerini etkinleştirme

RDP istemcisi varsayılan protokol olarak TLS 1.0 kullanır. Ancak bu durum, yeni standart haline gelen TLS 1.1 olarak değiştirilebilir. TLS 1.1 VM'de devre dışı bırakılırsa, bağlantı başarısız olur.
1.  CMD örneğinde TLS protokolünü etkinleştirin:

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f
2.  AD ilkesinin değişiklikleri üzerine yazmasını önlemek için grup ilkesi güncelleştirmesini geçici olarak durdurun:

        REG add "HKLM\SYSTEM\CurrentControlSet\Services\gpsvc" /v Start /t REG_DWORD /d 4 /f
3.  Değişikliklerin etkili olması için VM'yi yeniden başlatın. Sorun çözülürse, grup ilkesini yeniden etkinleştirmek için aşağıdaki komutu çalıştırın:

        sc config gpsvc start= auto sc start gpsvc

        gpupdate /force
    Değişiklik geri döndürüldüyse, şirket etki alanınızda bir Etkin Dizin ilkesi olduğu anlamına gelir. Bu sorunun yeniden oluşmasını önlemek için bu ilkeyi değiştirmeniz gerekir.

### <a name="repair-the-vm-offline"></a>VM Çevrimdışı Onarım

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Os diskini kurtarma VM'ine takın

1. [Os diskini kurtarma VM'sine takın.](../windows/troubleshoot-recovery-disks-portal.md)
2. İşletim VM'sine işletim sistemi diski bağlandıktan sonra, diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın. Ekli işletim sistemi diskine atanan sürücü mektubuna dikkat edin.
3. Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın.

#### <a name="enable-dump-log-and-serial-console"></a>Döküm günlük ve Seri Konsol etkinleştirme

Döküm günlüğü ve Seri Konsol'u etkinleştirmek için aşağıdaki komut dosyasını çalıştırın.

1. Yükseltilmiş bir komut istemi oturumu açın (**Yönetici olarak çalıştırın).**
2. Şu betiği çalıştırın:

    Bu komut dosyasında, ekli işletim sistemi diskine atanan sürücü harfinin F olduğunu varsayıyoruz.

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

#### <a name="reset-the-permission-for-machinekeys-folder"></a>MachineKeys klasörü için izni sıfırlama

1. Yükseltilmiş bir komut istemi oturumu açın (**Yönetici olarak çalıştırın).**
2. Aşağıdaki komut dosyasını çalıştırın. Bu komut dosyasında, ekli işletim sistemi diskine atanan sürücü harfinin F olduğunu varsayıyoruz.

        Md F:\temp

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt
        
        takeown /f "F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"

        icacls F:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt

#### <a name="enable-all-supported-tls-versions"></a>Desteklenen tüm TLS sürümlerini etkinleştirme

1.  Yükseltilmiş bir komut istemi oturumu açın **(Yönetici olarak çalıştırın**) ve aşağıdaki komutları çalıştırın. Aşağıdaki komut dosyası, sürücü mektubunun bağlı işletim sistemi diskine atandığını varsayar F. Bu sürücü harfini VM'iniz için uygun değerle değiştirin.
2.  Hangi TLS'nin etkin olduğunu kontrol edin:

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWO

3.  Anahtar yoksa veya değeri **0**ise, aşağıdaki komut dosyalarını çalıştırarak protokolü etkinleştirin:

        REM Enable TLS 1.0, TLS 1.1 and TLS 1.2

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f

4.  NLA'yı etkinleştir:

        REM Enable NLA

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f reg unload HKLM\BROKENSYSTEM
5.  [İşletim sistemi diskini ayırın ve VM'yi yeniden oluşturun](../windows/troubleshoot-recovery-disks-portal.md)ve sorunun çözülüp çözülmediğini denetleyin.





