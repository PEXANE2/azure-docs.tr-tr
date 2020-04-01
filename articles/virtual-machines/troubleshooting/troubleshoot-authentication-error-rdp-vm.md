---
title: Azure VM'ye bağlanmak için RDP kullandığınızda kimlik doğrulama hatalarını giderme | Microsoft Dokümanlar
description: Bir Azure sanal makinesine (VM) bağlanmak için Uzak Masaüstü Protokolü'nü (RDP) kullandığınızda oluşan kimlik doğrulama hatalarını nasıl gidereceklerini öğrenin.
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
ms.date: 11/01/2018
ms.author: delhan
ms.openlocfilehash: 03356c0b4a93f4befdbc529523e58642137a8887
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420811"
---
# <a name="troubleshoot-authentication-errors-when-you-use-rdp-to-connect-to-azure-vm"></a>Azure VM’ye RDP kullanarak bağlandığınızda karşılaştığınız kimlik doğrulaması hatalarını giderme

Bu makale, bir Azure sanal makinesine (VM) bağlanmak için Uzak Masaüstü Protokolü (RDP) bağlantısını kullandığınızda ortaya çıkan kimlik doğrulama hatalarını gidermenize yardımcı olabilir.

## <a name="symptoms"></a>Belirtiler

Hoş Geldiniz ekranını gösteren ve işletim sisteminin çalıştığını gösteren bir Azure VM ekran görüntüsünü yakalarsınız. Ancak, Uzak Masaüstü Bağlantısı'nı kullanarak VM'ye bağlanmaya çalıştığınızda, aşağıdaki hata iletilerinden birini alırsınız.

### <a name="error-message-1"></a>Hata iletisi 1

**Kimlik doğrulama hatası oluştu. Yerel Güvenlik Otoritesi ile bağlantı kuramaz.**

### <a name="error-message-2"></a>Hata iletisi 2

**Bağlanmaya çalıştığınız uzak bilgisayar ağ düzeyinde kimlik doğrulaması (NLA) gerektirir, ancak Windows etki alanı denetleyicinize NLA gerçekleştirmek için başvurulamaz. Uzak bilgisayarda yöneticiyseniz, Sistem Özellikleri iletişim kutusunun Uzak sekmesindeki seçenekleri kullanarak NLA'yı devre dışı kullanabilirsiniz.**

### <a name="error-message-3-generic-connection-error"></a>Hata iletisi 3 (genel bağlantı hatası)

**Bu bilgisayar uzak bilgisayara bağlanamıyor. Sorun devam ederse, uzaktan bilgisayarın sahibine veya ağ yöneticinize başvurun.**

## <a name="cause"></a>Nedeni

NLA'nın RDP'nin Bir VM'ye erişimini engellemesinin birden çok nedeni vardır.

### <a name="cause-1"></a>Neden 1

VM etki alanı denetleyicisi (DC) ile iletişim kuramaz. Bu sorun, etki alanı kimlik bilgilerini kullanarak bir RDP oturumunun VM'ye erişmesini engelleyebilir. Ancak, Yerel Yönetici kimlik bilgilerini kullanarak oturum açmaya devam edebilirsiniz. Bu sorun aşağıdaki durumlarda oluşabilir:

1. Bu VM ve DC arasındaki Active Directory Security Channel bozuldu.

2. VM hesap parolasının eski bir kopyasına ve DC'nin yeni bir kopyasıvardır.

3. Bu VM'nin bağlandığı DC sağlıksız.

### <a name="cause-2"></a>Neden 2

VM'nin şifreleme düzeyi istemci bilgisayar tarafından kullanılandan daha yüksektir.

### <a name="cause-3"></a>Neden 3

TLS 1.0, 1.1 veya 1.2 (sunucu) protokolleri VM'de devre dışı bırakılır.

### <a name="cause-4"></a>Neden 4

VM, etki alanı kimlik bilgilerini kullanarak oturum açmayı devre dışı bırakacak şekilde ayarlandı ve Yerel Güvenlik Otoritesi (LSA) yanlış olarak ayarlandı.

### <a name="cause-5"></a>Neden 5

VM yalnızca Federal Bilgi İşlem Standardı (FIPS) uyumlu algoritma bağlantılarını kabul edecek şekilde ayarlandı. Bu genellikle Active Directory ilkesi kullanılarak yapılır. Bu nadir bir yapılandırmadır, ancak FIPS yalnızca Uzak Masaüstü bağlantıları için zorlanabilir.

## <a name="before-you-troubleshoot"></a>Sorun gidermeden önce

### <a name="create-a-backup-snapshot"></a>Yedek anlık görüntü oluşturma

Yedek anlık görüntü oluşturmak için, [Anlık Görüntü bir disk](../windows/snapshot-copy-managed-disk.md)teki adımları izleyin.

### <a name="connect-to-the-vm-remotely"></a>VM'ye uzaktan bağlanma

VM'ye uzaktan bağlanmak için, [Azure VM sorunlarını gidermek için uzak araçları kullanma](remote-tools-troubleshoot-azure-vm-issues.md)yöntemlerinden birini kullanın.

### <a name="group-policy-client-service"></a>Grup ilkesi istemci hizmeti

Bu etki alanı birleştirilmiş bir VM ise, önce Herhangi bir Etkin Dizin İlkesi değişiklikleri üzerine yazmak önlemek için Grup İlkesi İstemci hizmeti durdurun. Bunu yapmak için aşağıdaki komutu çalıştırın:

```cmd
REM Disable the member server to retrieve the latest GPO from the domain upon start
REG add "HKLM\SYSTEM\CurrentControlSet\Services\gpsvc" /v Start /t REG_DWORD /d 4 /f
```

Sorun giderildikten sonra, bu VM'nin etki alanından en son GPO'yu almak için etki alanına başvurma yeteneğini geri yükleyin. Bunu yapmak için aşağıdaki komutları çalıştırın:

```cmd
sc config gpsvc start= auto
sc start gpsvc

gpupdate /force
```

Değişiklik geri döndürüldüyse, soruna Etkin Dizin ilkesi nin neden olduğu anlamına gelir. 

### <a name="workaround"></a>Geçici çözüm

Bu sorunu aşmak için, NLA'yı devre dışı kıçevirmek için komut penceresinde aşağıdaki komutları çalıştırın:

```cmd
REM Disable the Network Level Authentication
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 0
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 0
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 0
```

Ardından VM'yi yeniden başlatın.

NLA'yı yeniden etkinleştirmek için aşağıdaki komutu çalıştırın ve VM'yi yeniden başlatın:

```cmd
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v disabledomaincreds /t REG_DWORD /d 0 /f

REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f
```

## <a name="troubleshooting"></a>Sorun giderme

### <a name="for-domain-joined-vms"></a>Etki alanına katılan VM'ler için

Bu sorunu gidermek için, önce VM'nin DC'ye bağlanıp bağlanamayacağını ve DC'nin "sağlıklı" bir durumu olup olmadığını ve VM'den gelen istekleri işleyip işlemeyeceğini kontrol edin.

>[!Note] 
>DC sistem durumunu sınamak için, aynı oturum açma sunucusunu paylaşan aynı VNET ve Subnet'te başka bir VM kullanabilirsiniz.

"VM'ye uzaktan bağlan" bölümündeki adımlara göre Seri konsol, uzak CMD veya uzak PowerShell kullanarak sorun yaratan VM'ye bağlanın.

VM'nin hangi DC'ye bağlandığına karar vermek için konsolda aşağıdaki komutu çalıştırın: 

```cmd
set | find /i "LOGONSERVER"
```

Ardından, VM ve DC arasındaki güvenli kanalın durumunu kontrol edin. Bunu yapmak için, yükseltilmiş bir PowerShell örneğinde aşağıdaki komutu çalıştırın. Bu komut, güvenli kanalın canlı olup olmadığını belirten bir Boolean bayrağı döndürür:

```powershell
Test-ComputerSecureChannel -verbose
```

Kanal bozuksa, onarmak için aşağıdaki komutu çalıştırın:

```powershell
Test-ComputerSecureChannel -repair
```

Active Directory'deki bilgisayar hesabı parolasının VM ve DC'de güncelleştirdiğinden emin olun:

```powershell
Reset-ComputerMachinePassword -Server "<COMPUTERNAME>" -Credential <DOMAIN CREDENTIAL WITH DOMAIN ADMIN LEVEL>
```

DC ve VM arasındaki iletişim iyiyse, ancak DC bir RDP oturumunu açacak kadar sağlıklı değilse, DC'yi yeniden başlatmayı deneyebilirsiniz.

Önceki komutlar etki alanına iletişim sorununu gidermediyse, bu VM'yi etki alanına yeniden katılabilirsiniz. Bunu yapmak için şu adımları uygulayın:

1. Aşağıdaki içeriği kullanarak Unjoin.ps1 adında bir komut dosyası oluşturun ve ardından komut dosyasını Azure portalında Özel Komut Dosyası Uzantısı olarak dağıtın:

    ```cmd
    cmd /c "netdom remove <<MachineName>> /domain:<<DomainName>> /userD:<<DomainAdminhere>> /passwordD:<<PasswordHere>> /reboot:10 /Force"
    ```
    
    Bu komut dosyası VM'yi zorla etki alanından çıkarır ve 10 saniye sonra yeniden başlatır. Ardından, etki alanı tarafındaki Bilgisayar nesnesini temizlemeniz gerekir.

2.  Temizleme bittikten sonra, bu VM'yi etki alanına yeniden katılın. Bunu yapmak için, aşağıdaki içeriği kullanarak JoinDomain.ps1 adında bir komut dosyası oluşturun ve ardından komut dosyasını Azure portalında Özel Komut Dosyası Uzantısı olarak dağıtın: 

    ```cmd
    cmd /c "netdom join <<MachineName>> /domain:<<DomainName>> /userD:<<DomainAdminhere>> /passwordD:<<PasswordHere>> /reboot:10"
    ```

    >[!Note] 
    >Bu, belirtilen kimlik bilgilerini kullanarak etki alanında VM'ye katılır.

Etkin Dizin kanalı sağlıklıysa, bilgisayar parolası güncelleştirilir ve etki alanı denetleyicisi beklendiği gibi çalışıyorsa, aşağıdaki adımları deneyin.

Sorun devam ederse, etki alanı kimlik bilgisidevre dışı olup olmadığını denetleyin. Bunu yapmak için, yükseltilmiş bir Komut İstem penceresi açın ve vm'de oturum açmak için etki alanı hesaplarını devre dışı bırakabilecek şekilde ayarlanıp ayarlanmadığını belirlemek için aşağıdaki komutu çalıştırın:

```cmd
REG query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v disabledomaincreds
```

Anahtar **1**olarak ayarlanmışsa, bu sunucunun etki alanı kimlik bilgilerine izin vermeyecek şekilde ayarlandığını anlamına gelir. Bu anahtarı **0**olarak değiştirin.

### <a name="for-standalone-vms"></a>Bağımsız VM'ler için

#### <a name="check-minencryptionlevel"></a>MinEncryptionLevel'i kontrol et

CMD örneğinde, **MinEncryptionLevel** kayıt defteri değerini sorgulamak için aşağıdaki komutu çalıştırın:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel
```

Kayıt defteri değerine bağlı olarak aşağıdaki adımları izleyin:

* 4 (FIPS): [FIPs uyumlu algoritmabağlantılarını](#fips-compliant)denetle'ye gidin.

* 3 (128-bit şifreleme): Aşağıdaki komutu çalıştırarak önem derecesini **2** olarak ayarlayın:

    ```cmd
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 2 /f
    ```

* 2 (İstemci tarafından dikte edilen en yüksek şifreleme): Aşağıdaki komutu çalıştırarak şifrelemeyi en az **1** değerine ayarlamayı deneyebilirsiniz:

    ```cmd
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 1 /f
    ```
    
Kayıt defterindeki değişikliklerin etkili olması için VM'yi yeniden başlatın.

#### <a name="tls-version"></a>TLS sürümü

Sisteme bağlı olarak RDP TLS 1.0, 1.1 veya 1.2 (sunucu) protokolünü kullanır. Bu protokollerin VM'de nasıl ayarlandırılacağını sorgulamak için bir CMD örneğini açın ve ardından aşağıdaki komutları çalıştırın:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled
```

Döndürülen değerler **1'in**tümü değilse, bu protokolün devre dışı bırakıldığı anlamına gelir. Bu protokolleri etkinleştirmek için aşağıdaki komutları çalıştırın:

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f
```

Diğer protokol sürümleri için aşağıdaki komutları çalıştırabilirsiniz:

<pre lang="bat">
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS <i>x.x</i>\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS <i>x.x</i>\Server" /v Enabled
</pre>

> [!Note]
> SCHANNEL hatalarındaki Konuk İşletim Sistemi Günlüklerinden SSH/TLS x.x sürümünü alın.

#### <a name="check-fips-compliant-algorithms-connections"></a><a name="fips-compliant"></a>FIPs uyumlu algoritmabağlantılarını denetleyin

Uzak masaüstü yalnızca FIPs uyumlu algoritma bağlantılarını kullanmak için zorlanabilir. Bu bir kayıt defteri anahtarı kullanılarak ayarlanabilir. Bunu yapmak için, yükseltilmiş komut istemi penceresini açın ve ardından aşağıdaki tuşları sorgulayın:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy" /v Enabled
```

Komut **1'i**döndürürse, kayıt defteri değerini **0**olarak değiştirin.

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy" /v Enabled /t REG_DWORD /d 0
```

VM'deki geçerli MinEncryptionLevel'in hangisi olduğunu kontrol edin:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel
```

Komut **4'u**döndürürse, kayıt defteri değerini **2** olarak değiştirin

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 2
```

Kayıt defterindeki değişikliklerin etkili olması için VM'yi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

[Win32_TSGeneralSetting sınıfının SetEncryptionLevel yöntemi](https://docs.microsoft.com/windows/desktop/TermServ/win32-tsgeneralsetting-setencryptionlevel)

[Sunucu Kimlik Doğrulama ve Şifreleme Düzeylerini Yapılandırma](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770833(v=ws.11))

[Win32_TSGeneralSetting sınıfı](https://docs.microsoft.com/windows/desktop/TermServ/win32-tsgeneralsetting)
