---
title: Azure Windows VM'lerinde CMD ve PowerShell | Microsoft Dokümanlar
description: Azure Windows VM'lerinde SAC içinde CMD ve PowerShell komutları nasıl kullanılır?
services: virtual-machines-windows
documentationcenter: ''
author: alsin
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 493340764f507c4fa364a5000f65cc232630b243
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77167038"
---
# <a name="windows-commands---cmd-and-powershell"></a>Windows Komutları - CMD ve PowerShell

Bu bölüm, Windows VM'nize erişmek için SAC kullanmanız gerekebileceği senaryolarda (örneğin RDP bağlantı hatalarını gidermeniz gerektiğinde) ortak görevleri gerçekleştirmek için örnek komutlar içerir.

SAC, Windows Server 2003'ten beri Windows'un tüm sürümlerine dahil edilmiştir, ancak varsayılan olarak devre dışı bırakılır. SAC çekirdek `sacdrv.sys` sürücüsüne, `Special Administration Console Helper` hizmete (`sacsvr`) ve `sacsess.exe` işleme dayanır. Daha fazla bilgi için Acil [Durum Yönetim Hizmetleri Araçları ve Ayarları'na](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc787940(v%3dws.10))bakın.

SAC, seri bağlantı noktası üzerinden çalışan işletim sisteminize bağlanmanızı sağlar. CMD'yi SAC'dan `sacsess.exe` başlattığınızda, çalışan işletim sisteminizde başlatılabilirsiniz. `cmd.exe` Aynı anda VM'nize RDP'niz varsa, seri konsol özelliği üzerinden SAC'a bağlı olduğunuzu Task Manager'da görebilirsiniz. SAC üzerinden erişebildiğiniz CMD, RDP ile bağlandığında kullandığınız cmd ile aynıdır. `cmd.exe` PowerShell'i bu CMD örneğinden fırlatma olanağı da dahil olmak üzere, aynı komutlar ve araçlar mevcuttur. Bu SAC ve Windows Kurtarma Ortamı (WinRE) arasında büyük bir fark bu SAC farklı, minimal işletim sistemi içine WinRE önyüklemeler çalışan işletim sistemi, yönetmenize izin vermektedir. Azure VM'ler seri konsol özelliğiyle WinRE'ye erişme özelliğini desteklemese de, Azure VM'leri SAC üzerinden yönetilebilir.

SAC, geri kaydırma olmadan 80x24 ekran arabelleğiyle sınırlı olduğundan, çıktıyı tek er tek sayfa görüntülemek için komutlara ekleyin. `| more` Sonraki `<spacebar>` sayfayı veya `<enter>` sonraki satırı görmek için kullanın.

`SHIFT+INSERT`seri konsol penceresi için yapıştırılakısayol'dur.

SAC'ın sınırlı ekran arabelleği nedeniyle, daha uzun komutları yerel bir metin düzenleyicisine yazıp SAC'ye yapıştırmak daha kolay olabilir.

## <a name="view-and-edit-windows-registry-settings"></a>Windows Kayıt Defteri Ayarlarını Görüntüleme ve Değiştirme
### <a name="verify-rdp-is-enabled"></a>RDP'nin etkin olduğunu doğrula
`reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections`

`reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections`

İkinci anahtar (\İlkeler altında) yalnızca ilgili grup ilkesi ayarı yapılandırıldığında bulunur.

### <a name="enable-rdp"></a>RDP'yi etkinleştir
`reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0`

`reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0`

İkinci anahtar (\İlkeler altında) yalnızca ilgili grup ilkesi ayarı yapılandırıldıysa gerekli olacaktır. Değer, grup ilkesinde yapılandırılırsa bir sonraki grup ilkesi yenilemesinde yeniden yazılır.

## <a name="manage-windows-services"></a>Windows Hizmetlerini Yönetme

### <a name="view-service-state"></a>Hizmet durumunu görüntüleme
`sc query termservice`
###  <a name="view-service-logon-account"></a>Hizmet oturum açma hesabını görüntüleme
`sc qc termservice`
### <a name="set-service-logon-account"></a>Hizmet oturum açma hesabını ayarlama
`sc config termservice obj= "NT Authority\NetworkService"`

Eşitler işaretinden sonra bir boşluk gereklidir.
### <a name="set-service-start-type"></a>Hizmet başlangıç türünü ayarlama
`sc config termservice start= demand`

Eşitler işaretinden sonra bir boşluk gereklidir. Olası başlangıç `boot`değerleri `system` `auto`, `demand` `disabled`, `delayed-auto`, , , .
### <a name="set-service-dependencies"></a>Hizmet bağımlılıklarını ayarlama
`sc config termservice depend= RPCSS`

Eşitler işaretinden sonra bir boşluk gereklidir.
### <a name="start-service"></a>Hizmeti başlatın
`net start termservice`

or

`sc start termservice`
### <a name="stop-service"></a>Hizmeti durdurun
`net stop termservice`

or

`sc stop termservice`
## <a name="manage-networking-features"></a>Ağ Özelliklerini Yönetme
### <a name="show-nic-properties"></a>NIC özelliklerini göster
`netsh interface show interface`
### <a name="show-ip-properties"></a>IP özelliklerini göster
`netsh interface ip show config`
### <a name="show-ipsec-configuration"></a>IPSec yapılandırmayı göster
`netsh nap client show configuration`
### <a name="enable-nic"></a>NIC'i etkinleştir
`netsh interface set interface name="<interface name>" admin=enabled`
### <a name="set-nic-to-use-dhcp"></a>DHCP kullanmak için NIC'yi ayarlama
`netsh interface ip set address name="<interface name>" source=dhcp`

Hakkında `netsh`daha fazla bilgi için, [buraya tıklayın.](https://docs.microsoft.com/windows-server/networking/technologies/netsh/netsh-contexts)

Azure VM'ler her zaman bir IP adresi elde etmek için DHCP kullanmak üzere konuk işletim sistemi üzerinde yapılandırılmalıdır. Azure statik IP ayarı, statik IP'yi VM'ye vermek için hala DHCP'yi kullanır.
### <a name="ping"></a>Ping
`ping 8.8.8.8`
### <a name="port-ping"></a>Bağlantı noktası pingi
Telnet istemcisini yükleme

`dism /online /Enable-Feature /FeatureName:TelnetClient`

Bağlantıyı test etme

`telnet bing.com 80`

Telnet istemcisini kaldırmak için

`dism /online /Disable-Feature /FeatureName:TelnetClient`

Varsayılan olarak Windows'da kullanılabilen yöntemlerle sınırlı olduğunda, PowerShell bağlantı noktası bağlantısını test etmek için daha iyi bir yaklaşım olabilir. Örnekler için aşağıdaki PowerShell bölümüne bakın.
### <a name="test-dns-name-resolution"></a>Test DNS ad çözümlemesi
`nslookup bing.com`
### <a name="show-windows-firewall-rule"></a>Windows Güvenlik Duvarı kuralını göster
`netsh advfirewall firewall show rule name="Remote Desktop - User Mode (TCP-In)"`
### <a name="disable-windows-firewall"></a>Windows Güvenlik Duvarını devre düşün
`netsh advfirewall set allprofiles state off`

Windows Güvenlik Duvarı'nı geçici olarak elemek için sorun giderme sırasında bu komutu kullanabilirsiniz. Bir sonraki yeniden başlatmada veya aşağıdaki komutu kullanarak etkinleştirdiğinizde etkinleştirilecektir. Windows Güvenlik Duvarı hizmetini (MPSSVC) veya Temel Filtreleme Motoru (BFE) hizmetini, Windows Güvenlik Duvarı'nı ekarte etmenin bir yolu olarak durdurmayın. MPSSVC veya BFE'nin durdurulması tüm bağlantının engellenmesine neden olur.
### <a name="enable-windows-firewall"></a>Windows Güvenlik Duvarı’nı Etkinleştir
`netsh advfirewall set allprofiles state on`
## <a name="manage-users-and-groups"></a>Kullanıcıları ve Grupları Yönetme
### <a name="create-local-user-account"></a>Yerel kullanıcı hesabı oluşturma
`net user /add <username> <password>`
### <a name="add-local-user-to-local-group"></a>Yerel kullanıcıyı yerel gruba ekleme
`net localgroup Administrators <username> /add`
### <a name="verify-user-account-is-enabled"></a>Kullanıcı hesabının etkin olduğunu doğrulayın
`net user <username> | find /i "active"`

Genelleştirilmiş görüntüden oluşturulan Azure VM'ler, yerel yönetici hesabının VM sağlama sırasında belirtilen adla yeniden adlandırını sağlar. Bu yüzden genellikle `Administrator`olmayacaktır .
### <a name="enable-user-account"></a>Kullanıcı hesabını etkinleştirme
`net user <username> /active:yes`
### <a name="view-user-account-properties"></a>Kullanıcı hesabı özelliklerini görüntüleme
`net user <username>`

Yerel bir yönetici hesabından örnek ilgi satırları:

`Account active Yes`

`Account expires Never`

`Password expires Never`

`Workstations allowed All`

`Logon hours allowed All`

`Local Group Memberships *Administrators`

### <a name="view-local-groups"></a>Yerel grupları görüntüleme
`net localgroup`
## <a name="manage-the-windows-event-log"></a>Windows Olay Günlüğü'nün yönetilmesi
### <a name="query-event-log-errors"></a>Olay günlüğü hatalarını sorgula
`wevtutil qe system /c:10 /f:text /q:"Event[System[Level=2]]" | more`

Döndürülecek istenilen sayıda olay adedine geçin `/c:10` veya filtreyle eşleşen tüm olayları döndürmek için onu taşıyın.
### <a name="query-event-log-by-event-id"></a>Olay Kimliğine göre olay günlüğü sorgula
`wevtutil qe system /c:1 /f:text /q:"Event[System[EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider"></a>Olay Kimliği ve Sağlayıcıya göre olay günlüğü sorgula
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Olay kimliği ve Sağlayıcı tarafından son 24 saat içinde sorguolay günlüğü
`wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

24 saat yerine 7 gün geriye bakmak için kullanın. `604800000`
### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Olay kimliği, Sağlayıcı ve EventData tarafından son 7 gün içinde sorguolay günlüğü
`wevtutil qe security /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
## <a name="view-or-remove-installed-applications"></a>Yüklü Uygulamaları Görüntüleme veya Kaldırma
### <a name="list-installed-applications"></a>Yüklü uygulamaları listele
`wmic product get Name,InstallDate | sort /r | more`

Son `sort /r` yüklenenleri görmeyi kolaylaştırmak için yükleme tarihine kadar azalan sıralamalar. Çıktının bir sonraki sayfasına ilerlemek veya `<spacebar>` `<enter>` bir satırı ilerletmek için kullanın.
### <a name="uninstall-an-application"></a>Uygulamayı kaldırma
`wmic path win32_product where name="<name>" call uninstall`

Kaldırmak `<name>` istediğiniz uygulama için yukarıdaki komutta döndürülen adla değiştirin.

## <a name="file-system-management"></a>Dosya Sistemi Yönetimi
### <a name="get-file-version"></a>Dosya sürümünü alma
`wmic datafile where "drive='C:' and path='\\windows\\system32\\drivers\\' and filename like 'netvsc%'" get version /format:list`

Bu örnek, Windows sürümüne bağlı olarak netvsc.sys, netvsc63.sys veya netvsc60.sys sanal NIC sürücüsünün dosya sürümünü döndürür.
### <a name="scan-for-system-file-corruption"></a>Sistem dosyası bozulması için taramaya
`sfc /scannow`

Ayrıca bakınız [Bir Windows Resmini onarın.](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image)
### <a name="scan-for-system-file-corruption"></a>Sistem dosyası bozulması için taramaya
`dism /online /cleanup-image /scanhealth`

Ayrıca bakınız [Bir Windows Resmini onarın.](https://docs.microsoft.com/windows-hardware/manufacture/desktop/repair-a-windows-image)
### <a name="export-file-permissions-to-text-file"></a>Dosya izinlerini metin dosyasına aktarma
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /t /c > %temp%\MachineKeys_permissions_before.txt`
### <a name="save-file-permissions-to-acl-file"></a>Dosya izinlerini ACL dosyasına kaydetme
`icacls %programdata%\Microsoft\Crypto\RSA\MachineKeys /save %temp%\MachineKeys_permissions_before.aclfile /t`
### <a name="restore-file-permissions-from-acl-file"></a>ACL dosyasından dosya izinlerini geri yükleme
`icacls %programdata%\Microsoft\Crypto\RSA /save %temp%\MachineKeys_permissions_before.aclfile /t`

Kullanırken `/restore` yol, kullanırken `/save`belirttiğiniz klasörün üst klasörü olmalıdır. Bu örnekte, `\RSA` yukarıdaki `\MachineKeys` `/save` örnekte belirtilen klasörün üst öğesidir.
### <a name="take-ntfs-ownership-of-a-folder"></a>Bir klasörün NTFS sahipliğini alma
`takeown /f %programdata%\Microsoft\Crypto\RSA\MachineKeys /a /r`
### <a name="grant-ntfs-permissions-to-a-folder-recursively"></a>NTFS izinlerini bir klasöre özyinelemeyle verme
`icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"`
## <a name="manage-devices"></a>Cihazları Yönetme
### <a name="remove-non-present-pnp-devices"></a>Mevcut olmayan PNP aygıtlarını kaldırma
`%windir%\System32\RUNDLL32.exe %windir%\System32\pnpclean.dll,RunDLL_PnpClean /Devices /Maxclean`
## <a name="manage-group-policy"></a>Grup İlkesi'ni yönetme
### <a name="force-group-policy-update"></a>Kuvvet grubu ilkesi güncelleştirmesi
`gpupdate /force /wait:-1`
## <a name="miscellaneous-tasks"></a>Çeşitli Görevler
### <a name="show-os-version"></a>İşletim sistemi sürümünü göster
`ver`

or

`wmic os get caption,version,buildnumber /format:list`

or

`systeminfo  find /i "os name"`

`systeminfo | findstr /i /r "os.*version.*build"`
### <a name="view-os-install-date"></a>İşletim sistemi yükleme tarihini görüntüleme
`systeminfo | find /i "original"`

or

`wmic os get installdate`
### <a name="view-last-boot-time"></a>Son önyükleme saatini görüntüleme
`systeminfo | find /i "system boot time"`
### <a name="view-time-zone"></a>Saat dilimini görüntüleme
`systeminfo | find /i "time zone"`

or

`wmic timezone get caption,standardname /format:list`
### <a name="restart-windows"></a>Windows'u yeniden başlatın
`shutdown /r /t 0`

Ekleme, `/f` kullanıcıları uyarmadan çalışan uygulamaları kapatmaya zorlar.
### <a name="detect-safe-mode-boot"></a>Güvenli Mod önyüklemeyi algıla
`bcdedit /enum | find /i "safeboot"`

## <a name="windows-commands---powershell"></a>Windows Komutları - PowerShell

PowerShell'i SAC'da çalıştırmak için CMD istemine ulaştıktan sonra şunları yazın:

`powershell <enter>`

> [!CAUTION]
> Diğer PowerShell komutlarını çalıştırmadan önce PSReadLine modüllerini PowerShell oturumundan çıkarın. PSReadLine SAC'daki PowerShell oturumunda çalışıyorsa, panodan yapıştırılan metinde fazladan karakterlerin getirilebileceği bilinen bir sorun vardır.

PsReadLine yüklü olup olmadığını ilk kontrol edin. Windows Server 2016, Windows 10 ve windows'un sonraki sürümlerinde varsayılan olarak yüklenir. Yalnızca el ile yüklenmiş olsaydı önceki Windows sürümlerinde mevcut olurdu.

Bu komut çıktısız bir istem'e dönerse, modül yüklenmedi ve POWERShell oturumunu SAC'da normal şekilde kullanmaya devam edebilirsiniz.

`get-module psreadline`

Yukarıdaki komut PSReadLine modül sürümünü döndürürse, boşaltmak için aşağıdaki komutu çalıştırın. Bu komut modülü silmez veya kaldırmaz, yalnızca geçerli PowerShell oturumundan boşaltır.

`remove-module psreadline`

## <a name="view-and-edit-windows-registry-settings"></a>Windows Kayıt Defteri Ayarlarını Görüntüleme ve Değiştirme
### <a name="verify-rdp-is-enabled"></a>RDP'nin etkin olduğunu doğrula
`get-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections'`

`get-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections'`

İkinci anahtar (\İlkeler altında) yalnızca ilgili grup ilkesi ayarı yapılandırıldığında bulunur.
### <a name="enable-rdp"></a>RDP'yi etkinleştir
`set-itemproperty -path 'hklm:\system\curRentcontrolset\control\terminal server' -name 'fdenytsconNections' 0 -type dword`

`set-itemproperty -path 'hklm:\software\policies\microsoft\windows nt\terminal services' -name 'fdenytsconNections' 0 -type dword`

İkinci anahtar (\İlkeler altında) yalnızca ilgili grup ilkesi ayarı yapılandırıldıysa gerekli olacaktır. Değer, grup ilkesinde yapılandırılırsa bir sonraki grup ilkesi yenilemesinde yeniden yazılır.
## <a name="manage-windows-services"></a>Windows Hizmetlerini Yönetme
### <a name="view-service-details"></a>Hizmet ayrıntılarını görüntüleme
`get-wmiobject win32_service -filter "name='termservice'" |  format-list Name,DisplayName,State,StartMode,StartName,PathName,ServiceType,Status,ExitCode,ServiceSpecificExitCode,ProcessId`

`Get-Service`kullanılabilir, ancak hizmet oturum açma hesabını içermez. `Get-WmiObject win32-service`yapar.
### <a name="set-service-logon-account"></a>Hizmet oturum açma hesabını ayarlama
`(get-wmiobject win32_service -filter "name='termservice'").Change($null,$null,$null,$null,$null,$false,'NT Authority\NetworkService')`

`NT AUTHORITY\LocalService`, , veya `LocalSystem`, `NT AUTHORITY\NetworkService`dışında bir hizmet hesabı kullanırken hesap adından sonra son (sekizinci) bağımsız değişken olarak hesap parolası belirtin.
### <a name="set-service-startup-type"></a>Hizmet başlangıç türünü ayarlama
`set-service termservice -startuptype Manual`

`Set-service`kabul `Automatic`eder, `Manual` `Disabled` veya başlangıç türü için.
### <a name="set-service-dependencies"></a>Hizmet bağımlılıklarını ayarlama
`Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Services\TermService' -Name DependOnService -Value @('RPCSS','TermDD')`
### <a name="start-service"></a>Hizmeti başlatın
`start-service termservice`
### <a name="stop-service"></a>Hizmeti durdurun
`stop-service termservice`
## <a name="manage-networking-features"></a>Ağ Özelliklerini Yönetme
### <a name="show-nic-properties"></a>NIC özelliklerini göster
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} |  format-list status,name,ifdesc,macadDresS,driverversion,MediaConNectState,MediaDuplexState`

or

`get-wmiobject win32_networkadapter -filter "servicename='netvsc'" |  format-list netenabled,name,macaddress`

`Get-NetAdapter`2008R2 kullanım `Get-WmiObject`için 2012+'da mevcuttur.
### <a name="show-ip-properties"></a>IP özelliklerini göster
`get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'" |  format-list DNSHostName,IPAddress,DHCPEnabled,IPSubnet,DefaultIPGateway,MACAddress,DHCPServer,DNSServerSearchOrder`
### <a name="enable-nic"></a>NIC'i etkinleştir
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | enable-netadapter`

or

`(get-wmiobject win32_networkadapter -filter "servicename='netvsc'").enable()`

`Get-NetAdapter`2008R2 kullanım `Get-WmiObject`için 2012+'da mevcuttur.
### <a name="set-nic-to-use-dhcp"></a>DHCP kullanmak için NIC'yi ayarlama
`get-netadapter | where {$_.ifdesc.startswith('Microsoft Hyper-V Network Adapter')} | Set-NetIPInterface -DHCP Enabled`

`(get-wmiobject Win32_NetworkAdapterConfiguration -filter "ServiceName='netvsc'").EnableDHCP()`

`Get-NetAdapter`2012+'da mevcuttur. 2008R2 kullanımı `Get-WmiObject`için. Azure VM'ler her zaman bir IP adresi elde etmek için DHCP kullanmak üzere konuk işletim sistemi üzerinde yapılandırılmalıdır. Azure statik IP ayarı, IP'yi VM'ye vermek için hala DHCP'yi kullanır.
### <a name="ping"></a>Ping
`test-netconnection`

> [!NOTE]
> Yazma-İlerleme cmdlet bu komutile çalışmayabilir. Azaltma olarak, `$ProgressPreference = "SilentlyContinue"` ilerleme çubuğunu devre dışı kılabilir.

or

`get-wmiobject Win32_PingStatus -Filter 'Address="8.8.8.8"' | format-table -autosize IPV4Address,ReplySize,ResponseTime`

`Test-Netconnection`herhangi bir parametre `internetbeacon.msedge.net`olmadan ping çalışacağız. 2012+'da satışa sunulmuştur. 2008R2 için `Get-WmiObject` ikinci örnekte olduğu gibi kullanın.
### <a name="port-ping"></a>Bağlantı Noktası Pingi
`test-netconnection -ComputerName bing.com -Port 80`

or

`(new-object Net.Sockets.TcpClient).BeginConnect('bing.com','80',$null,$null).AsyncWaitHandle.WaitOne(300)`

`Test-NetConnection`2012+'da mevcuttur. 2008R2 kullanımı için`Net.Sockets.TcpClient`
### <a name="test-dns-name-resolution"></a>Test DNS ad çözümlemesi
`resolve-dnsname bing.com`

or

`[System.Net.Dns]::GetHostAddresses('bing.com')`

`Resolve-DnsName`2012+'da mevcuttur. 2008R2 kullanımı `System.Net.DNS`için.
### <a name="show-windows-firewall-rule-by-name"></a>Windows güvenlik duvarı kuralını ada göre göster
`get-netfirewallrule -name RemoteDesktop-UserMode-In-TCP`
### <a name="show-windows-firewall-rule-by-port"></a>Windows güvenlik duvarı kuralını bağlantı noktasına göre göster
`get-netfirewallportfilter | where {$_.localport -eq 3389} | foreach {Get-NetFirewallRule -Name $_.InstanceId} | format-list Name,Enabled,Profile,Direction,Action`

or

`(new-object -ComObject hnetcfg.fwpolicy2).rules | where {$_.localports -eq 3389 -and $_.direction -eq 1} | format-table Name,Enabled`

`Get-NetFirewallPortFilter`2012+'da mevcuttur. 2008R2 için `hnetcfg.fwpolicy2` COM nesnesini kullanın.
### <a name="disable-windows-firewall"></a>Windows güvenlik duvarını devre t
`Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled False`

`Set-NetFirewallProfile`2012+'da mevcuttur. Yukarıdaki CMD bölümünde atıfta `netsh advfirewall` bulunulan 2008R2 kullanımı için.
## <a name="manage-users-and-groups"></a>Kullanıcıları ve Grupları Yönetme
### <a name="create-local-user-account"></a>Yerel kullanıcı hesabı oluşturma
`new-localuser <name>`
### <a name="verify-user-account-is-enabled"></a>Kullanıcı hesabının etkin olduğunu doğrulayın
`(get-localuser | where {$_.SID -like "S-1-5-21-*-500"}).Enabled`

or

`(get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'").Disabled`

`Get-LocalUser`2012+'da mevcuttur. 2008R2 kullanımı `Get-WmiObject`için. Bu örnek, her zaman SID'si `S-1-5-21-*-500`olan yerleşik yerel yönetici hesabını gösterir. Genelleştirilmiş görüntüden oluşturulan Azure VM'ler, yerel yönetici hesabının VM sağlama sırasında belirtilen adla yeniden adlandırını sağlar. Bu yüzden genellikle `Administrator`olmayacaktır .
### <a name="add-local-user-to-local-group"></a>Yerel kullanıcıyı yerel gruba ekleme
`add-localgroupmember -group Administrators -member <username>`
### <a name="enable-local-user-account"></a>Yerel kullanıcı hesabını etkinleştirme
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | enable-localuser`

Bu örnek, her zaman SID'si `S-1-5-21-*-500`olan yerleşik yerel yönetici hesabını etkinleştirir. Genelleştirilmiş görüntüden oluşturulan Azure VM'ler, yerel yönetici hesabının VM sağlama sırasında belirtilen adla yeniden adlandırını sağlar. Bu yüzden genellikle `Administrator`olmayacaktır .
### <a name="view-user-account-properties"></a>Kullanıcı hesabı özelliklerini görüntüleme
`get-localuser | where {$_.SID -like "S-1-5-21-*-500"} | format-list *`

or

`get-wmiobject Win32_UserAccount -Namespace "root\cimv2" -Filter "SID like 'S-1-5-%-500'" |  format-list Name,Disabled,Status,Lockout,Description,SID`

`Get-LocalUser`2012+'da mevcuttur. 2008R2 kullanımı `Get-WmiObject`için. Bu örnek, her zaman SID'si `S-1-5-21-*-500`olan yerleşik yerel yönetici hesabını gösterir.
### <a name="view-local-groups"></a>Yerel grupları görüntüleme
`(get-localgroup).name | sort` `(get-wmiobject win32_group).Name | sort`

`Get-LocalUser`2012+'da mevcuttur. 2008R2 kullanımı `Get-WmiObject`için.
## <a name="manage-the-windows-event-log"></a>Windows Olay Günlüğü'nün yönetilmesi
### <a name="query-event-log-errors"></a>Olay günlüğü hatalarını sorgula
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Level=2]]" | more`

Döndürülecek istenilen sayıda olay adedine geçin `/c:10` veya filtreyle eşleşen tüm olayları döndürmek için onu taşıyın.
### <a name="query-event-log-by-event-id"></a>Olay Kimliğine göre olay günlüğü sorgula
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider"></a>Olay Kimliği ve Sağlayıcıya göre olay günlüğü sorgula
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11]]" | more`
### <a name="query-event-log-by-event-id-and-provider-for-the-last-24-hours"></a>Olay kimliği ve Sağlayıcı tarafından son 24 saat içinde sorguolay günlüğü
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Hyper-V-Netvsc'] and EventID=11 and TimeCreated[timediff(@SystemTime) <= 86400000]]]"`

24 saat yerine 7 gün geriye bakmak için kullanın. `604800000` |
### <a name="query-event-log-by-event-id-provider-and-eventdata-in-the-last-7-days"></a>Olay kimliği, Sağlayıcı ve EventData tarafından son 7 gün içinde sorguolay günlüğü
`get-winevent -logname system -maxevents 1 -filterxpath "*[System[Provider[@Name='Microsoft-Windows-Security-Auditing'] and EventID=4624 and TimeCreated[timediff(@SystemTime) <= 604800000]] and EventData[Data[@Name='TargetUserName']='<username>']]" | more`
## <a name="view-or-remove-installed-applications"></a>Yüklü Uygulamaları Görüntüleme veya Kaldırma
### <a name="list-installed-software"></a>Yüklü yazılımları listele
`get-wmiobject win32_product | select installdate,name | sort installdate -descending | more`
### <a name="uninstall-software"></a>Yazılımı kaldırma
`(get-wmiobject win32_product -filter "Name='<name>'").Uninstall()`
## <a name="file-system-management"></a>Dosya Sistemi Yönetimi
### <a name="get-file-version"></a>Dosya sürümünü alma
`(get-childitem $env:windir\system32\drivers\netvsc*.sys).VersionInfo.FileVersion`

Bu örnek, Windows sürümüne bağlı olarak netvsc.sys, netvsc63.sys veya netvsc60.sys adlı sanal NIC sürücüsünün dosya sürümünü döndürür.
### <a name="download-and-extract-file"></a>Dosyayı karşıdan yükleme ve ayıklama
`$path='c:\bin';md $path;cd $path;(new-object net.webclient).downloadfile( ('htTp:/'+'/download.sysinternals.com/files/SysinternalsSuite.zip'),"$path\SysinternalsSuite.zip");(new-object -com shelL.apPlication).namespace($path).CopyHere( (new-object -com shelL.apPlication).namespace("$path\SysinternalsSuite.zip").Items(),16)`

Bu örnek bir `c:\bin` klasör oluşturur, sonra karşıdan yükler ve sysinternals araç paketini `c:\bin`.
## <a name="miscellaneous-tasks"></a>Çeşitli Görevler
### <a name="show-os-version"></a>İşletim sistemi sürümünü göster
`get-wmiobject win32_operatingsystem | format-list caption,version,buildnumber`
### <a name="view-os-install-date"></a>İşletim sistemi yükleme tarihini görüntüleme
`(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).installdate)`
### <a name="view-last-boot-time"></a>Son önyükleme saatini görüntüleme
`(get-wmiobject win32_operatingsystem).lastbootuptime`
### <a name="view-windows-uptime"></a>Windows çalışma süresini görüntüleme
`"{0:dd}:{0:hh}:{0:mm}:{0:ss}.{0:ff}" -f ((get-date)-(get-wmiobject win32_operatingsystem).converttodatetime((get-wmiobject win32_operatingsystem).lastbootuptime))`

Çalışma süresini `<days>:<hours>:<minutes>:<seconds>:<milliseconds>`, örneğin `49:16:48:00.00`.
### <a name="restart-windows"></a>Windows'u yeniden başlatın
`restart-computer`

Ekleme, `-force` kullanıcıları uyarmadan çalışan uygulamaları kapatmaya zorlar.
## <a name="instance-metadata"></a>Örnek Meta veriler

osType, Location, vmSize, vmId, ad, resourceGroupName, subscriptionId, privateIpAddress ve publicIpAddress gibi ayrıntıları görüntülemek için Azure Örnek meta verilerini Azure VM'nizden sorgulayabilirsiniz.

Örnek meta verilerini sorgulamak, Azure ana bilgisayar aracılığıyla örnek meta veri hizmetine REST çağrısı yaptığından, sağlıklı konuk ağ bağlantısı gerektirir. Bu nedenle, örnek meta verileri sorgulayabiliyorsanız, bu size konuğun ağ üzerinden Azure tarafından barındırılan bir hizmetle iletişim kurabildiğini gösterir.

Daha fazla bilgi için Azure [Örneği Meta veri hizmetine](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)bakın.

### <a name="instance-metadata"></a>Örnek meta veriler
`$im = invoke-restmethod -headers @{"metadata"="true"} -uri http://169.254.169.254/metadata/instance?api-version=2017-08-01 -method get`

`$im | convertto-json`
### <a name="os-type-instance-metadata"></a>İşletim Sistemi Türü (Örnek Meta data)
`$im.Compute.osType`
### <a name="location-instance-metadata"></a>Konum (Örnek Meta veri)
`$im.Compute.Location`
### <a name="size-instance-metadata"></a>Boyut (Örnek Meta veri)
`$im.Compute.vmSize`
### <a name="vm-id-instance-metadata"></a>VM ID (Örnek Meta veriler)
`$im.Compute.vmId`
### <a name="vm-name-instance-metadata"></a>VM Adı (Örnek Metadata)
`$im.Compute.name`
### <a name="resource-group-name-instance-metadata"></a>Kaynak Grup Adı (Örnek Meta data)
`$im.Compute.resourceGroupName`
### <a name="subscription-id-instance-metadata"></a>Abonelik Kimliği (Örnek Metadata)
`$im.Compute.subscriptionId`
### <a name="tags-instance-metadata"></a>Etiketler (Örnek Metadata)
`$im.Compute.tags`
### <a name="placement-group-id-instance-metadata"></a>Yerleşim Grubu Kimliği (Örnek Metadata)
`$im.Compute.placementGroupId`
### <a name="platform-fault-domain-instance-metadata"></a>Platform Hata Etki Alanı (Örnek Metadata)
`$im.Compute.platformFaultDomain`
### <a name="platform-update-domain-instance-metadata"></a>Platform Güncelleme Etki Alanı (Örnek Metadata)
`$im.Compute.platformUpdateDomain`
### <a name="ipv4-private-ip-address-instance-metadata"></a>IPv4 Özel IP Adresi (Örnek Metadata)
`$im.network.interface.ipv4.ipAddress.privateIpAddress`
### <a name="ipv4-public-ip-address-instance-metadata"></a>IPv4 Genel IP Adresi (Örnek Metadata)
`$im.network.interface.ipv4.ipAddress.publicIpAddress`
### <a name="ipv4-subnet-address--prefix-instance-metadata"></a>IPv4 Alt Ağ Adresi / Önek (Örnek Meta data)
`$im.network.interface.ipv4.subnet.address`

`$im.network.interface.ipv4.subnet.prefix`
### <a name="ipv6-ip-address-instance-metadata"></a>IPv6 IP Adresi (Örnek Metadata)
`$im.network.interface.ipv6.ipAddress`
### <a name="mac-address-instance-metadata"></a>MAC Adresi (Örnek Meta data)
`$im.network.interface.macAddress`

## <a name="next-steps"></a>Sonraki adımlar
* Ana seri konsolu Windows dokümantasyon sayfası [burada](serial-console-windows.md)yer almaktadır.
* Seri konsol [Linux](serial-console-linux.md) VM'ler için de kullanılabilir.
* [Önyükleme tanılama](boot-diagnostics.md)hakkında daha fazla bilgi edinin.
