---
title: Azure'a yüklemek için bir Windows VHD hazırlama
description: Azure'a yüklemek için windows VHD veya VHDX nasıl hazırlayacağınızı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/11/2019
ms.author: genli
ms.openlocfilehash: 719a1985aeb0db7b0cf7f55a10762bf3ebb3e045
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250198"
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Azure’a yüklemek için Windows VHD veya VHDX’i hazırlama

Şirket içinden Azure'a bir Windows sanal makinesi (VM) yüklemeden önce sanal sabit diski (VHD veya VHDX) hazırlamanız gerekir. Azure, VHD dosya biçiminde olan ve sabit boyutlu diski olan nesil 1 ve nesil 2 VM'leri destekler. VHD için izin verilen maksimum boyut 1.023 GB'dır. 

Bir nesil 1 VM, VHDX dosya sistemini VHD'ye dönüştürebilirsiniz. Dinamik olarak genişleyen bir diski sabit boyutlu bir diske de dönüştürebilirsiniz. Ama bir VM'in neslini değiştiremezsin. Daha fazla bilgi için [bkz.](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) [Azure support for generation 2 VMs (preview)](generation-2.md)

Azure VM'ler için destek ilkesi hakkında bilgi için [Azure VM'ler için Microsoft sunucu yazılım desteğine](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)bakın.

> [!NOTE]
> Bu makaledeki talimatlar aşağıdakiler için geçerlidir:
>1. Windows Server 2008 R2 ve daha sonra Windows Server işletim sistemlerinin 64 bit sürümü. Azure'da 32 bit işletim sistemi çalıştırma hakkında daha fazla bilgi için [Azure VM'lerinde 32 bit işletim sistemleri için Destek'e](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines)bakın.
>2. Azure Site Kurtarma veya Azure Geçişi gibi iş yükünü geçirmek için herhangi bir Olağanüstü Durum Kurtarma aracı kullanılacaksa, bu işlemin geçişten önce görüntüyü hazırlamak için Konuk İşletim Sistemi'nde yapılması ve izlenmesi gerekir.

## <a name="system-file-checker-sfc-command"></a>Sistem Dosya Denetleyicisi (SFC) komutu

### <a name="run-windows-system-file-checker-utility-run-sfc-scannow-on-os-prior-to-generalization-step-of-creating-customer-os-image"></a>Müşteri işletim sistemi görüntüsü oluşturma genelleme adımından önce Işletim Sistemi'nde Windows System File Checker yardımcı programını çalıştırma (sfc/scannow çalıştır)

Sistem Dosya Denetleyicisi (SFC) komutu, Windows sistem dosyalarını doğrulamak ve değiştirmek için kullanılır.

SFC komutunu çalıştırmak için:

1. Yönetici olarak yükseltilmiş bir CMD istemiaçın.
1. Enter `sfc /scannow` yazın **Enter**ve seçin.

    ![ Sistem Dosyası Denetleyicisi](media/prepare-for-upload-vhd-image/system-file-checker.png)


SFC tamacı tamamlandıktan sonra, Windows Güncelleştirmeleri'ni yüklemeyi ve bilgisayarı yeniden başlatmayı deneyin.

## <a name="convert-the-virtual-disk-to-a-fixed-size-and-to-vhd"></a>Sanal diski sabit bir boyuta ve VHD'ye dönüştürme

Sanal diskinizi Azure için gerekli biçime dönüştürmeniz gerekiyorsa, bu bölümdeki yöntemlerden birini kullanın:

1. Sanal disk dönüştürme işlemini çalıştırmadan önce VM'yi yedekle.

1. Windows VHD'nin yerel sunucuda doğru çalıştığından emin olun. Azure'a dönüştürmeyi veya yüklemeyi denemeden önce VM'nin kendisindeki hataları giderin.

1. VHD boyutu ile ilgili olarak:

   1. Azure'daki tüm VHD'lerin 1MB'ye hizalanmış bir sanal boyutu olmalıdır. Ham diskten VHD'ye dönüştürürken, dönüştürmeden önce ham disk boyutunun 1 MB'ın katları olduğundan emin olmalısınız. Yüklenen VHD'den görüntü oluştururken megabaytın kesirleri hatalara neden olur.

   2. OS VHD için izin verilen maksimum boyut 2 TB'dir.


Diski dönüştürdükten sonra, diski kullanan bir VM oluşturun. Yüklemeye hazırlamayı bitirmek için VM'ye başlayın ve oturum açın.

### <a name="use-hyper-v-manager-to-convert-the-disk"></a>Diski dönüştürmek için Hyper-V Manager'ı kullanma 
1. Hyper-V Manager'ı açın ve soldaki yerel bilgisayarınızı seçin. Bilgisayar listesinin üstündeki menüde,**Diski Edit'i** **seçin.** > 
2. Sanal **Sabit Disk i Bul** sayfasında sanal diskinizi seçin.
3. Eylem **Seç** sayfasında >  **İleri'yi Dönüştür'ü**seçin.**Next**
4. VHDX'ten dönüştürmeniz gerekiyorsa,**Sonraki** **VHD'yi** > seçin.
5. Dinamik olarak genişleyen bir diskten dönüştürmeniz gerekiyorsa, >  **İleri'deki Sabit boyutu'ni**seçin.**Next**
6. Yeni VHD dosyasını kaydetmek için bir yol bulun ve seçin.
7. **Bitiş'i**seçin.

> [!NOTE]
> Bu makaledeki komutları çalıştırmak için yükseltilmiş bir PowerShell oturumu kullanın.

### <a name="use-powershell-to-convert-the-disk"></a>Diski dönüştürmek için PowerShell'i kullanma 
Windows PowerShell'deki [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) komutunu kullanarak sanal diski dönüştürebilirsiniz. PowerShell'i başlattığınızda **yönetici olarak Çalıştır'ı** seçin. 

Aşağıdaki örnek komut, diski VHDX'ten VHD'ye dönüştürür. Komut ayrıca diski dinamik olarak genişleyen bir diskten sabit boyutlu bir diske dönüştürür.

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

Bu komutta, dönüştürmek `-Path` istediğiniz sanal sabit diske giden yol ile değeri değiştirin. Değeri yeni `-DestinationPath` yol ve dönüştürülen diskin adıyla değiştirin.

### <a name="convert-from-vmware-vmdk-disk-format"></a>VMware VMDK disk biçiminden dönüştürme
[VMDK dosya biçiminde](https://en.wikipedia.org/wiki/VMDK)bir Windows VM resminiz varsa, VHD biçimine dönüştürmek için [Microsoft Virtual Machine Converter'ı](https://www.microsoft.com/download/details.aspx?id=42497) kullanın. Daha fazla bilgi için [vmware VMDK'yı Hyper-V VHD'ye nasıl dönüştüreceğime](https://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx)bakın.

## <a name="set-windows-configurations-for-azure"></a>Azure için Windows yapılandırmalarını ayarlama

> [!NOTE]
> Genelleştirilmiş bir görüntüden Windows VM oluşturulduğunda Azure platformu BIR ISO dosyasını DVD-ROM'a bağlar.
> Bu nedenle, DVD-ROM genelleştirilmiş görüntü işletim sistemi etkin olmalıdır. Devre dışı bırakılırsa, Windows VM OOBE'de takılıp kalır.

Azure'a yüklemeyi planladığınız VM'de, yükseltilmiş komut [istemi penceresinden](https://technet.microsoft.com/library/cc947813.aspx)aşağıdaki komutları çalıştırın:

1. Yönlendirme tablosundaki statik kalıcı rotayı kaldırın:
   
   * Rota tablosunu görüntülemek `route print` için komut isteminde çalıştırın.
   * `Persistence Routes` Bölümleri kontrol et. Kalıcı bir rota varsa, kaldırmak `route delete` için komutu kullanın.
2. WinHTTP proxy'sini kaldırın:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```

    VM'nin belirli bir proxy ile çalışması gerekiyorsa, VM'nin Azure'a bağlanabilmesi için Azure IP adresine[(168.63.129.16)](https://blogs.msdn.microsoft.com/mast/2015/05/18/what-is-the-ip-address-168-63-129-16/
)bir proxy özel durumu ekleyin:
    ```
    $proxyAddress="<your proxy server>"
    $proxyBypassList="<your list of bypasses>;168.63.129.16"

    netsh winhttp set proxy $proxyAddress $proxyBypassList
    ```

3. Disk SAN ilkesini [`Onlineall`](https://technet.microsoft.com/library/gg252636.aspx)şu şekilde ayarlayın:
   
    ```PowerShell
    diskpart 
    ```
    Açık komut istemi penceresinde aşağıdaki komutları yazın:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Windows için Eşgüdümlü Evrensel Saat (UTC) saatini ayarlayın. Ayrıca Windows zaman hizmetinin başlangıç`w32time`türünü `Automatic`( ) ayarlamak:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -Name "RealTimeIsUniversal" -Value 1 -Type DWord -Force

    Set-Service -Name w32time -StartupType Automatic
    ```
5. Güç profilini yüksek performansa ayarlayın:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```
6. Çevresel değişkenlerin `TEMP` ve `TMP` varsayılan değerlerine ayarlandıklarından emin olun:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name "TEMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -Name "TMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -Force
    ```

## <a name="check-the-windows-services"></a>Windows hizmetlerini denetleme
Aşağıdaki Windows hizmetlerinin her birinin Windows varsayılan değerlerine ayarlı olduğundan emin olun. Bu hizmetler, VM bağlantısı sağlamak için ayarlanan minimum hizmetlerdir. Başlangıç ayarlarını sıfırlamak için aşağıdaki komutları çalıştırın:
   
```PowerShell
Get-Service -Name bfe | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name dhcp | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name dnscache | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name IKEEXT | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name iphlpsvc | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name netlogon | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name netman | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name nsi | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name TermService | Where-Object { $_.StartType -ne 'Manual' } | Set-Service -StartupType 'Manual'
Get-Service -Name MpsSvc | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
Get-Service -Name RemoteRegistry | Where-Object { $_.StartType -ne 'Automatic' } | Set-Service -StartupType 'Automatic'
```
## <a name="update-remote-desktop-registry-settings"></a>Uzak masaüstü kayıt defteri ayarlarını güncelleştirme
Uzaktan erişim için aşağıdaki ayarların doğru şekilde yapılandırıldığından emin olun:

>[!NOTE] 
>Çalıştırdığınızda bir hata iletisi `Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services -Name <object name> -Value <value>`alabilirsiniz. Bu iletiyi güvenle yoksayabilirsiniz. Yalnızca etki alanının bu yapılandırmayı bir Grup İlkesi Nesnesi üzerinden itmediği anlamına gelir.

1. Uzak Masaüstü Protokolü (RDP) etkinleştirilir:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "fDenyTSConnections" -Value 0 -Type DWord -Force
    ```
   
2. RDP bağlantı noktası doğru şekilde ayarlandı. Varsayılan bağlantı noktası 3389'dur:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "PortNumber" -Value 3389 -Type DWord -Force
    ```
    Bir VM dağıttığınızda, varsayılan kurallar bağlantı noktası 3389'a karşı oluşturulur. Bağlantı noktası numarasını değiştirmek istiyorsanız, VM Azure'da dağıtıldıktan sonra bunu yapın.

3. Dinleyici her ağ arabiriminde dinliyor:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "LanAdapter" -Value 0 -Type DWord -Force
   ```
4. RDP bağlantıları için ağ düzeyinde kimlik doğrulama (NLA) modunu yapılandırın:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "UserAuthentication" -Value 1 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "SecurityLayer" -Value 1 -Type DWord -Force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "fAllowSecProtocolNegotiation" -Value 1 -Type DWord -Force
     ```

5. Canlı tutma değerini ayarlayın:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "KeepAliveEnable" -Value 1  -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "KeepAliveInterval" -Value 1  -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "KeepAliveTimeout" -Value 1 -Type DWord -Force
    ```
6. Yeniden bağlanma:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -Name "fDisableAutoReconnect" -Value 0 -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "fInheritReconnectSame" -Value 1 -Type DWord -Force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "fReconnectSame" -Value 0 -Type DWord -Force
    ```
7. Eşzamanlı bağlantı sayısını sınırlandırın:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -Name "MaxInstanceCount" -Value 4294967295 -Type DWord -Force
    ```
8. RDP dinleyicisine bağlı kendi imzalı sertifikaları kaldırın:
    
    ```PowerShell
    if ((Get-Item -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp').Property -contains "SSLCertificateSHA1Hash")
    {
        Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -Name "SSLCertificateSHA1Hash" -Force
    }
    ```
    Bu kod, VM'yi dağıttığınızda başlangıçta bağlanabilmesini sağlar. Bunu daha sonra gözden geçirmeniz gerekirse, VM Azure'da dağıtıldıktan sonra bunu yapabilirsiniz.

9. VM bir etki alanının parçası olacaksa, eski ayarların geri alındığından emin olmak için aşağıdaki ilkeleri kontrol edin. 
    
    | Hedef                                     | İlke                                                                                                                                                       | Değer                                                                                    |
    |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
    | RDP etkin                           | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Bileşenler\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayar\Bağlantılar         | Uzak Masaüstü'nü kullanarak kullanıcıların uzaktan bağlanmasına izin verme                                  |
    | NLA grup ilkesi                         | Ayarlar\Yönetim Şablonları\Bileşenler\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Barındırma\Güvenlik                                                    | NLA kullanarak uzaktan erişim için kullanıcı kimlik doğrulaması gerektirme |
    | Canlı tutma ayarları                      | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Windows Bileşenleri\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayar\Bağlantılar | Canlı tutma bağlantı aralığını yapılandırma                                                 |
    | Ayarları yeniden bağlama                       | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Windows Bileşenleri\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayar\Bağlantılar | Otomatik olarak yeniden bağlanma                                                                   |
    | Sınırlı sayıda bağlantı ayarı | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Windows Bileşenleri\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayar\Bağlantılar | Bağlantı sayısını sınırlandırın                                                              |

## <a name="configure-windows-firewall-rules"></a>Windows Güvenlik Duvarı kurallarını yapılandırma
1. Üç profilde (etki alanı, standart ve genel) Windows Güvenlik Duvarı'nı açın:

   ```PowerShell
    Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
   ```

2. WinRM'in üç güvenlik duvarı profilinde (etki alanı, özel ve genel) geçmesine izin vermek ve PowerShell uzaktan hizmetini etkinleştirmek için PowerShell'de aşağıdaki komutu çalıştırın:
   
   ```PowerShell
    Enable-PSRemoting -Force

    Set-NetFirewallRule -DisplayName "Windows Remote Management (HTTP-In)" -Enabled True
   ```
3. RDP trafiğine izin vermek için aşağıdaki güvenlik duvarı kurallarını etkinleştirin:

   ```PowerShell
    Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Enabled True
   ```   
4. VM'nin sanal ağ içindeki ping komutuna yanıt verebilmesi için dosya ve yazıcı paylaşımı kuralını etkinleştirin:

   ```PowerShell
   Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -Enabled True
   ``` 
5. Azure platform ağı için bir kural oluşturun:

   ```PowerShell
    New-NetFirewallRule -DisplayName "AzurePlatform" -Direction Inbound -RemoteAddress 168.63.129.16 -Profile Any -Action Allow -EdgeTraversalPolicy Allow
    New-NetFirewallRule -DisplayName "AzurePlatform" -Direction Outbound -RemoteAddress 168.63.129.16 -Profile Any -Action Allow
   ``` 
6. VM bir etki alanının parçası olacaksa, eski ayarların geri döndürülmediğinden emin olmak için aşağıdaki Azure REKLAM ilkelerini kontrol edin. 

    | Hedef                                 | İlke                                                                                                                                                  | Değer                                   |
    |--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
    | Windows Güvenlik Duvarı profillerini etkinleştirme | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Ağ\Ağ Bağlantısı\Windows Güvenlik Duvarı\Etki Alanı Profili\Windows Güvenlik Duvarı   | Tüm ağ bağlantılarını koruma         |
    | RDP'yi etkinleştir                           | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Ağ\Ağ Bağlantısı\Windows Güvenlik Duvarı\Etki Alanı Profili\Windows Güvenlik Duvarı   | Gelen Uzak Masaüstü özel durumlarına izin ver |
    |                                      | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Ağ\Ağ Bağlantısı\Windows Güvenlik Duvarı\Standart Profil\Windows Güvenlik Duvarı | Gelen Uzak Masaüstü özel durumlarına izin ver |
    | ICMP-V4'ü etkinleştirme                       | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Ağ\Ağ Bağlantısı\Windows Güvenlik Duvarı\Etki Alanı Profili\Windows Güvenlik Duvarı   | ICMP özel durumlara izin ver                   |
    |                                      | Bilgisayar Yapılandırması\İlkeler\Windows Ayarları\Yönetim Şablonları\Ağ\Ağ Bağlantısı\Windows Güvenlik Duvarı\Standart Profil\Windows Güvenlik Duvarı | ICMP özel durumlara izin ver                   |

## <a name="verify-the-vm"></a>VM'yi doğrulayın 

VM'nin sağlıklı, güvenli ve RDP'ye erişilebilir olduğundan emin olun: 

1. Diskin sağlıklı ve tutarlı olduğundan emin olmak için, bir sonraki VM yeniden başlatmada diski denetleyin:

    ```PowerShell
    Chkdsk /f
    ```
    Raporun temiz ve sağlıklı bir disk gösterdiğinden emin olun.

2. Önyükleme Yapılandırma Verileri (BCD) ayarlarını ayarlayın. 

    > [!NOTE]
    > Bu komutları çalıştırmak için yükseltilmiş bir PowerShell penceresi kullanın.
   
   ```powershell
    bcdedit /set "{bootmgr}" integrityservices enable
    bcdedit /set "{default}" device partition=C:
    bcdedit /set "{default}" integrityservices enable
    bcdedit /set "{default}" recoveryenabled Off
    bcdedit /set "{default}" osdevice partition=C:
    bcdedit /set "{default}" bootstatuspolicy IgnoreAllFailures

    #Enable Serial Console Feature
    bcdedit /set "{bootmgr}" displaybootmenu yes
    bcdedit /set "{bootmgr}" timeout 5
    bcdedit /set "{bootmgr}" bootems yes
    bcdedit /ems "{current}" ON
    bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200
   ```
3. Döküm günlüğü Windows kilitlenme sorunları sorun giderme de yararlı olabilir. Döküm günlük toplamaetkinleştirin:

    ```powershell
    # Set up the guest OS to collect a kernel dump on an OS crash event
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name CrashDumpEnabled -Type DWord -Force -Value 2
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name DumpFile -Type ExpandString -Force -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -Name NMICrashDump -Type DWord -Force -Value 1

    # Set up the guest OS to collect user mode dumps on a service crash event
    $key = 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    if ((Test-Path -Path $key) -eq $false) {(New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting' -Name LocalDumps)}
    New-ItemProperty -Path $key -Name DumpFolder -Type ExpandString -Force -Value "c:\CrashDumps"
    New-ItemProperty -Path $key -Name CrashCount -Type DWord -Force -Value 10
    New-ItemProperty -Path $key -Name DumpType -Type DWord -Force -Value 2
    Set-Service -Name WerSvc -StartupType Manual
    ```
4. Windows Yönetim Araçları (WMI) deposunun tutarlı olduğunu doğrulayın:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Depo bozuksa, [bkz.](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not)

5. Başka hiçbir uygulamanın 3389 numaralı bağlantı noktasını kullanmadığından emin olun. Bu bağlantı noktası Azure'daki RDP hizmeti için kullanılır. VM'de hangi bağlantı noktalarının `netstat -anob`kullanıldığını görmek için çalıştırın:

    ```PowerShell
    netstat -anob
    ```

6. Etki alanı denetleyicisi olan bir Windows VHD yüklemek için:

   * Diski hazırlamak için [aşağıdaki ek adımları](https://support.microsoft.com/kb/2904015) izleyin.

   * Bir noktada DSRM'de VM'yi başlatmanız gerekirse Dizin Hizmetleri Geri Yükleme Modu (DSRM) parolasını bildiğinizden emin olun. Daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx)

7. Yerleşik yönetici hesabını ve parolasını bildiğinizden emin olun. Geçerli yerel yönetici parolasını sıfırlamak ve RDP bağlantısı üzerinden Windows'da oturum açmak için bu hesabı kullanabileceğinize emin olmak isteyebilirsiniz. Bu erişim izni ,"Uzak Masaüstü Hizmetleri üzerinden oturum açmaya izin ver" Grup İlkesi Nesnesi tarafından denetlenir. Yerel Grup İlkesi Düzenleyicisi'nde bu nesneyi görüntüleyin:

    Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Atama

8. RDP veya ağ üzerinden RDP erişiminizi engellemediğinizden emin olmak için aşağıdaki Azure REKLAM ilkelerini denetleyin:

    - Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Ataması\Ağdan bu bilgisayara erişimi reddetme

    - Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Ataması\Uzak Masaüstü Hizmetleri üzerinden oturum açma


9. Gerekli erişim hesaplarından hiçbirini kaldırmadığınızdan emin olmak için aşağıdaki Azure REKLAM ilkesini denetleyin:

   - Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Ataması\Bu bilgisayara ağdan eriş

   İlke aşağıdaki grupları listelemelidir:

   - Yöneticiler

   - Yedekleme İşleçleri

   - Herkes

   - Kullanıcılar

10. Windows'un hala sağlıklı olduğundan ve RDP bağlantısı üzerinden ulaşıladığından emin olmak için VM'yi yeniden başlatın. Bu noktada, VM'nin tamamen başladığından emin olmak için yerel Hyper-V'nizde bir VM oluşturmak isteyebilirsiniz. Daha sonra RDP ile VM'ye ulaşabildiğinizden emin olmak için test edin.

11. Herhangi bir ekstra Aktarım Sürücüsü Arabirimi (TDI) filtresini kaldırın. Örneğin, TCP paketlerini veya ek güvenlik duvarlarını analiz eden yazılımı kaldırın. Bunu daha sonra gözden geçirmeniz gerekirse, VM Azure'da dağıtıldıktan sonra bunu yapabilirsiniz.

12. Fiziksel bileşenlerveya diğer sanallaştırma teknolojisiyle ilgili diğer üçüncü taraf yazılımları veya sürücüyü kaldırın.

### <a name="install-windows-updates"></a>Windows güncelleştirmelerini yükleme
İdeal olarak, *makineyi yama düzeyinde*güncel tutmalısınız. Bu mümkün değilse, aşağıdaki güncelleştirmelerin yüklü olduğundan emin olun. En son güncelleştirmeleri almak için Windows güncelleme geçmişi sayfalarına bakın: [Windows 10 ve Windows Server 2019,](https://support.microsoft.com/help/4000825) [Windows 8.1 ve Windows Server 2012 R2](https://support.microsoft.com/help/4009470) ve [Windows 7 SP1 ve Windows Server 2008 R2 SP1](https://support.microsoft.com/help/4009469).

| Bileşen               | İkili         | Windows 7 SP1, Windows Server 2008 R2 SP1 | Windows 8, Windows Server 2012               | Windows 8.1, Windows Server 2012 R2 | Windows 10 v1607, Windows Server 2016 v1607 | Windows 10 v1703    | Windows 10 v1709, Windows Server 2016 v1709 | Windows 10 v1803, Windows Server 2016 v1803 |
|-------------------------|----------------|-------------------------------------------|---------------------------------------------|------------------------------------|---------------------------------------------------------|----------------------------|-------------------------------------------------|-------------------------------------------------|
| Depolama                 | disk.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061         | -                                                       | -                          | -                                               | -                                               |
|                         | Storport.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.332             | -                                               | -                                               |
|                         | ntfs.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | İologmsg.dll   | 6.1.7601.23403 - KB3125574                | 6.2.9200.16384 - KB2995387                  | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Classpnp.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614         | 10.0.14393.953 - KB4022715                              | -                          | -                                               | -                                               |
|                         | Volsnap.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384         | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | partmgr.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850         | 10.0.14393.953 - KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | volmgr.sys     |                                           |                                             |                                    |                                                         | 10.0.15063.0               | -                                               | -                                               |
|                         | Volmgrx.sys    | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | Msiscsi.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726         | 10.0.14393.1066 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Msdsm.sys      | 6.1.7601.23403 - KB3125574                | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726         | -                                                       | -                          | -                                               | -                                               |
|                         | Mpio.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726         | 10.0.14393.1198 - KB4022715                             | -                          | -                                               | -                                               |
|                         | vmstorfl.sys   | 6.3.9600.18907 - KB4072650                | 6.3.9600.18080 - KB3063109                  | 6.3.9600.18907 - KB4072650         | 10.0.14393.2007 - KB4345418                             | 10.0.15063.850 - KB4345419 | 10.0.16299.371 - KB4345420                      | -                                               |
|                         | Fveapi.dll     | 6.1.7601.23311 - KB3125574                | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614         | 10.0.14393.576 - KB4022715                              | -                          | -                                               | -                                               |
|                         | Fveapibase.dll | 6.1.7601.23403 - KB3125574                | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614         | 10.0.14393.206 - KB4022715                              | -                          | -                                               | -                                               |
| Ağ                 | netvsc.sys     | -                                         | -                                           | -                                  | 10.0.14393.1198 - KB4022715                             | 10.0.15063.250 - KB4020001 | -                                               | -                                               |
|                         | mrxsmb10.sys   | 6.1.7601.23816 - KB4022722                | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726         | 10.0.14393.479 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb20.sys   | 6.1.7601.23816 - KB4022722                | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726         | 10.0.14393.953 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb.sys     | 6.1.7601.23816 - KB4022722                | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726         | 10.0.14393.953 - KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | Tcpip.sys      | 6.1.7601.23761 - KB4022722                | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | http.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726         | 10.0.14393.251 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | vmswitch.sys   | 6.1.7601.23727 - KB4022719                | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.138             | -                                               | -                                               |
| Çekirdek                    | Ntoskrnl.exe   | 6.1.7601.23807 - KB4022719                | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.483             | -                                               | -                                               |
| Uzak Masaüstü Hizmetleri | rdpcorets.dll  | 6.2.9200.21506 - KB4022719                | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.0               | -                                               | -                                               |
|                         | termsrv.dll    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850         | 10.0.14393.0 - KB4022715                                | 10.0.15063.0               | -                                               | -                                               |
|                         | termdd.sys     | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | win32k.sys     | 6.1.7601.23807 - KB4022719                | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726         | 10.0.14393.594 - KB4022715                              | -                          | -                                               | -                                               |
|                         | rdpdd.dll      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | rdpwd.sys      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
| Güvenlik                | MS17-010 Arası       | KB4012212                                 | KB4012213                                   | KB4012213                          | KB4012606                                               | KB4012606                  | -                                               | -                                               |
|                         |                |                                           | KB4012216                                   |                                    | KB4013198                                               | KB4013198                  | -                                               | -                                               |
|                         |                | KB4012215                                 | KB4012214                                   | KB4012216                          | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         |                |                                           | KB4012217                                   |                                    | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         | CVE-2018-0886  | KB4103718               | KB4103730                | KB4103725       | KB4103723                                               | KB4103731                  | KB4103727                                       | KB4103721                                       |
|                         |                | KB4103712          | KB4103726          | KB4103715|                                                         |                            |                                                 |                                                 |
       
> [!NOTE]
> VM sağlama sırasında yanlışlıkla yeniden başlatılmasını önlemek için, tüm Windows Update yüklemelerinin tamamlandığından ve güncelleştirmebeklemeden emin olmamızı öneririz. Bunu yapmanın bir yolu, Sysprep komutunu çalıştırmadan önce tüm olası Windows güncelleştirmelerini yüklemek ve bir kez yeniden başlatmaktır.

### <a name="determine-when-to-use-sysprep"></a>Sysprep'in ne zaman kullanılacağını belirleyin<a id="step23"></a>    

Sistem Hazırlama Aracı (Sysprep), Windows yüklemesini sıfırlamak için çalıştırabileceğiniz bir işlemdir. Sysprep, tüm kişisel verileri kaldırarak ve çeşitli bileşenleri sıfırlayarak bir "kutunun dışında" deneyimi sağlar. 

Belirli bir yapılandırmaya sahip birkaç başka VM dağıtabileceğiniz bir şablon oluşturmak için genellikle Sysprep çalıştırın. Şablona *genelleştirilmiş görüntü*denir.

Bir diskten yalnızca bir VM oluşturmak istiyorsanız, Sysprep'i kullanmanız gerekmez. Bunun yerine, özel bir *görüntüden*VM oluşturabilirsiniz. Özel bir diskten VM oluşturma hakkında bilgi için bkz:

- [Özelleştirilmiş diskten VM oluşturma](create-vm-specialized.md)
- [Özel bir VHD diskten VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal?branch=master)

Genelleştirilmiş bir görüntü oluşturmak istiyorsanız, Sysprep'i çalıştırmanız gerekir. Daha fazla bilgi için Bkz. [Sysprep: Giriş](https://technet.microsoft.com/library/bb457073.aspx). 

Windows tabanlı bir bilgisayara yüklenen her rol veya uygulama genelleştirilmiş görüntüleri desteklemez. Bu yordamı çalıştırmadan önce, Sysprep'in bilgisayarın rolünü desteklediğinden emin olun. Daha fazla bilgi [için sunucu rolleri için Sysprep desteğine](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)bakın.

### <a name="generalize-a-vhd"></a>Bir VHD genelleme

>[!NOTE]
> Aşağıdaki adımlarda çalıştırdıktan `sysprep.exe` sonra VM'yi kapatın. Azure'da bir resim oluşturana kadar yeniden açmayın.

1. Windows VM'de oturum açın.
1. **Komut İstem'i** yönetici olarak çalıştırın. 
1. Dizini ' `%windir%\system32\sysprep`yi ' olarak değiştirin Ardından `sysprep.exe` komutunu çalıştırın.
1. **Sistem Hazırlama Aracı** iletişim kutusunda ** Sistem İlk Çalıştırma Deneyimi (OOBE) Moduna Gir**'i seçin ve **Genelleştir** onay kutusunun seçili olduğundan emin olun.

    ![Sistem Hazırlama Aracı](media/prepare-for-upload-vhd-image/syspre.png)
1. **Kapatma Seçenekleri'nde** **Kapatma'yı**seçin.
1. **Tamam'ı**seçin.
1. Sysprep bittiğinde VM'yi kapatın. VM'yi kapatmak için **Yeniden Başlat'ı** kullanmayın.

Şimdi VHD yüklenmeye hazır. Genelleştirilmiş bir diskten VM oluşturma hakkında daha fazla bilgi için [genelleştirilmiş bir VHD Yükle'ye bakın ve Azure'da yeni bir VM oluşturmak için kullanın.](sa-upload-generalized.md)


>[!NOTE]
> Özel *bir unattend.xml* dosyası desteklenmez. `additionalUnattendContent` Özelliği desteklememize rağmen, Azure sağlama aracısının kullandığı *unattend.xml* dosyasına [microsoft-windows-shell-kurulum](https://docs.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup) seçenekleri eklemek için yalnızca sınırlı destek sağlar. Örneğin, FirstLogonKomutları ve LogonKomutları eklemek için [ek UnattendContent'i](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.additionalunattendcontent?view=azure-dotnet) kullanabilirsiniz. Daha fazla bilgi için [ekUnattendContent FirstLogonCommands örneğine](https://github.com/Azure/azure-quickstart-templates/issues/1407)bakın.


## <a name="complete-the-recommended-configurations"></a>Önerilen yapılandırmaları tamamlayın
Aşağıdaki ayarlar VHD yüklemeyi etkilemez. Ancak, bunları yapılandırmanızı şiddetle öneririz.

* Azure [Sanal Makine Aracısını](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)yükleyin. Ardından VM uzantılarını etkinleştirebilirsiniz. VM uzantıları, VM'lerinizle kullanmak isteyebileceğin kritik işlevlerin çoğunu uygular. Örneğin, parolaları sıfırlamak veya RDP'yi yapılandırmak için uzantılara ihtiyacınız vardır. Daha fazla bilgi için Azure [Sanal Makine Aracısı'na genel bakış](../extensions/agent-windows.md)bilgisi için bkz.
* Azure'da VM'yi oluşturduktan sonra, performansı artırmak için sayfa dosyasını *geçici sürücü hacmine* koymanızı öneririz. Dosya yerleşimini aşağıdaki gibi ayarlayabilirsiniz:

   ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -Name "PagingFiles" -Value "D:\pagefile.sys" -Type MultiString -Force
   ```
  VM'ye bir veri diski bağlıysa, zamansal sürücü hacminin harfi genellikle *D'dir.* Bu atama, ayarlarınıza ve kullanılabilir sürücü sayısına bağlı olarak farklı olabilir.
  * Virüsten koruma yazılımı tarafından sağlanabilecek komut dosyası engelleyicilerini devre dışı bırakmanızı öneririz. Resminizden yeni bir VM dağıttığınızda çalıştırılan Windows Provisioning Agent komut dosyalarını etkileyebilir ve engelleyebilirler.
  
## <a name="next-steps"></a>Sonraki adımlar
* [Kaynak Yöneticisi dağıtımları için Azure'a Windows VM görüntüsü yükleme](upload-generalized-managed.md)
* [Azure Windows VM etkinleştirme sorunlarını giderme](troubleshoot-activation-problems.md)

