---
title: Olay Kimliğine göre Azure VM RDP bağlantı sorunlarını giderme | Microsoft Dokümanlar
description: Uzak Masaüstü protokolü (RDP) bağlantısının Azure Sanal Makinesi'ne (VM) bağlanmasını engelleyen çeşitli sorunları gidermek için olay iI'lerini kullanın.
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
ms.openlocfilehash: 2073d5f91b26cd2ae53e3291a6d1dad4d711b66d
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437058"
---
# <a name="troubleshoot-azure-vm-rdp-connection-issues-by-event-id"></a>Azure VM RDP bağlantı sorunlarını olay kimliğine göre giderme 

Bu makalede, Bir Azure Sanal Makine (VM) uzak masaüstü protokolü (RDP) bağlantısını engelleyen sorunları gidermek için olay iAnd'lerinin nasıl kullanılacağı açıklanmaktadır.

## <a name="symptoms"></a>Belirtiler

Azure VM'ye bağlanmak için Uzak Masaüstü protokolü (RDP) oturumu kullanmaya çalışırsınız. Kimlik bilgilerinizi girdikten sonra bağlantı başarısız olur ve aşağıdaki hata iletisini alırsınız:

**Bu bilgisayar uzak bilgisayara bağlanamıyor. Sorun devam ederse, uzaktan bilgisayarın sahibine veya ağ yöneticinize başvurun.**

Bu sorunu gidermek için VM'deki olay günlüklerini gözden geçirin ve ardından aşağıdaki senaryolara bakın.

## <a name="before-you-troubleshoot"></a>Sorun gidermeden önce

### <a name="create-a-backup-snapshot"></a>Yedek anlık görüntü oluşturma

Yedek anlık görüntü oluşturmak için, [Anlık Görüntü bir disk](../windows/snapshot-copy-managed-disk.md)teki adımları izleyin.

### <a name="connect-to-the-vm-remotely"></a>VM'ye uzaktan bağlanma

VM'ye uzaktan bağlanmak için, [Azure VM sorunlarını gidermek için uzak araçları kullanma](remote-tools-troubleshoot-azure-vm-issues.md)yöntemlerinden birini kullanın.

## <a name="scenario-1"></a>Senaryo 1

### <a name="event-logs"></a>Olay günlükleri

CMD örneğinde, olay 1058 veya olay 1057'nin son 24 saat içinde Sistem günlüğüne günlüğe kaydedilip günlüğe kaydedilip günlüğe kaydedilip kaydedilmediğine kontrol etmek için aşağıdaki komutları çalıştırın:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-TerminalServices-RemoteConnectionManager'] and EventID=1058 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-TerminalServices-RemoteConnectionManager'] and EventID=1057 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Günlük Adı:**      Sistem <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Tarih:**          *saat* <br />
**Etkinlik Id:** 1058 <br />
**Görev Kategorisi:** Hiçbiri <br />
**Düzey:**         Hata <br />
**Anahtar Kelimeler:**      Klasik <br />
**Kullanıcı:**          Yok <br />
**Bilgisayar:**      *bilgisayar* <br />
**Açıklama:** RD Session Host Server, TLS bağlantılarında RD Session Host Server kimlik doğrulaması için kullanılan süresi dolmuş kendi imzalı sertifikasını değiştirmeyi başaramadı. İlgili durum kodu Access reddedildi.

**Günlük Adı:**      Sistem <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Tarih:**          *saat* <br />
**Etkinlik Id:** 1058 <br />
**Görev Kategorisi:** Hiçbiri <br />
**Düzey:**         Hata <br />
**Anahtar Kelimeler:**      Klasik <br />
**Kullanıcı:**          Yok <br />
**Bilgisayar:**      *bilgisayar* <br />
**Açıklama:** RD Session host sunucusu TLS bağlantılarında RD Session ana bilgisayar sunucu kimlik doğrulaması için kullanılmak üzere yeni bir kendi imzalı sertifika oluşturmak için başarısız oldu, ilgili durum kodu nesne zaten var oldu.

**Günlük Adı:**      Sistem <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Tarih:**          *saat* <br />
**Etkinlik Id:** 1057 <br />
**Görev Kategorisi:** Hiçbiri <br />
**Düzey:**         Hata <br />
**Anahtar Kelimeler:**      Klasik <br />
**Kullanıcı:**          Yok <br />
**Bilgisayar:**      *bilgisayar* <br />
**Açıklama:** RD Session Host Server, TLS bağlantılarında RD Session Host Server kimlik doğrulaması için kullanılmak üzere kendi imzalı yeni bir sertifika oluşturamadı. İlgili durum kodu Keyset yok oldu

Ayrıca aşağıdaki komutları çalıştırarak SCHANNEL hata olayları 36872 ve 36870 için kontrol edebilirsiniz:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36870 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36872 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Günlük Adı:**      Sistem <br />
**Kaynak:**        Schannel <br />
**Tarih:** — <br />
**Etkinlik Id:** 36870 <br />
**Görev Kategorisi:** Hiçbiri <br />
**Düzey:**         Hata <br />
**Anahtar kelime -ler:**       <br />
**Kullanıcı:**          Sistem <br />
**Bilgisayar:**      *bilgisayar* <br />
**Açıklama:** TLS sunucu kimlik bilgisi özel anahtarına erişmeye çalışırken önemli bir hata oluştu. Şifreleme modülünden döndürülen hata kodu 0x8009030D'dir.  <br />
İç hata durumu 10001'dir.

### <a name="cause"></a>Nedeni
VM'deki MachineKeys klasöründeki yerel RSA şifreleme anahtarlarına erişilemedığından bu sorun oluşur. Bu sorun aşağıdaki nedenlerden biri için oluşabilir:

1. Machinekeys klasöründe veya RSA dosyalarında yanlış izin yapılandırması.

2. Bozuk veya eksik RSA anahtarı.

### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için, bu adımları kullanarak RDP Sertifikası'nda doğru izinleri ayarlamanız gerekir.

#### <a name="grant-permission-to-the-machinekeys-folder"></a>MachineKeys klasörüne izin verme

1. Aşağıdaki içeriği kullanarak bir komut dosyası oluşturun:

   ```powershell
   remove-module psreadline 
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt
   takeown /f "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt
   Restart-Service TermService -Force
   ```

2.  MachineKey klasörünün izinlerini sıfırlamak ve RSA dosyalarını varsayılan değerlere sıfırlamak için bu komut dosyasını çalıştırın.

3.  VM'ye tekrar erişmeye çalışın.

Komut dosyasını çalıştırdıktan sonra, izin sorunlarıyla karşılaşan aşağıdaki dosyaları denetleyebilirsiniz:

* c:\temp\BeforeScript_permissions.txt
* c:\temp\AfterScript_permissions.txt

#### <a name="renew-rdp-self-signed-certificate"></a>RDP kendi imzalı sertifikasını yenileme

Sorun devam ederse, RDP kendi imzalı sertifikasının yenilendiğinden emin olmak için aşağıdaki komut dosyasını çalıştırın:

```powershell
Import-Module PKI
Set-Location Cert:\LocalMachine
$RdpCertThumbprint = 'Cert:\LocalMachine\Remote Desktop\'+((Get-ChildItem -Path 'Cert:\LocalMachine\Remote Desktop\').thumbprint)
Remove-Item -Path $RdpCertThumbprint
Stop-Service -Name "SessionEnv"
Start-Service -Name "SessionEnv"
```

Sertifikayı yenileyemiyorsanız, sertifikayı silmeyi denemek için aşağıdaki adımları izleyin:

1. Aynı VNET'teki başka bir VM'de **Çalıştır** kutusunu açın, **mmc**yazın ve **ardından Tamam tuşuna**basın. 

2. **Dosya** menüsünde **Ekle/Kaldır'ı**seçin.

3. Kullanılabilir **ekler** **listesinde, Sertifikalar'ı**seçin ve sonra **Ekle'yi**seçin.

4. **Bilgisayar hesabını**seçin ve sonra **İleri'yi**seçin.

5. **Başka bir bilgisayar**seçin ve ardından sorunları olan VM'nin IP adresini ekleyin.
   >[!Note]
   >Sanal bir IP adresi kullanmaktan kaçınmak için dahili ağı kullanmaya çalışın.

6. **Bitiş'i**seçin ve ardından **Tamam'ı**seçin.

   ![Bilgisayar Seçin](./media/event-id-troubleshoot-vm-rdp-connecton/select-computer.png)

7. Sertifikaları genişletin, Uzak Masaüstü\Sertifikalar klasörüne gidin, sertifikayı sağ tıklatın ve sonra **Sil'i**seçin.

8. Uzak Masaüstü Yapılandırma hizmetini yeniden başlatın:

   ```cmd
   net stop SessionEnv
   net start SessionEnv
   ```

   >[!Note]
   >Bu noktada, mağazayı mmc'den yenilerseniz, sertifika yeniden görüntülenir. 

RDP'yi tekrar kullanarak VM'ye erişmeye çalışın.

#### <a name="update-tlsssl-certificate"></a>TLS/SSL sertifikasını güncelleştir

VM'yi TLS/SSL sertifikası kullanacak şekilde ayarlarsanız, parmak izini almak için aşağıdaki komutu çalıştırın. Ardından sertifikanın parmak izi ile aynı olup olmadığını kontrol edin:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash
```

Değilse, parmak izini değiştirin:

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash /t REG_BINARY /d <CERTIFICATE THUMBPRINT>
```

RdP'nin RDP için kendi imzaladığı sertifikayı kullanması için anahtarı silmeyi de deneyebilirsiniz:

```cmd
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash
```

## <a name="scenario-2"></a>Senaryo 2

### <a name="event-log"></a>Olay günlüğü

CMD örneğinde, SCHANNEL hata olayı 36871'in son 24 saat içinde Sistem günlüğüne kaydedilip günlüğe kaydedilip günlüğe kaydedilip kaydedilemediğini kontrol etmek için aşağıdaki komutları çalıştırın:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36871 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Günlük Adı:**      Sistem <br />
**Kaynak:**        Schannel <br />
**Tarih:** — <br />
**Etkinlik Numarası:** 36871 <br />
**Görev Kategorisi:** Hiçbiri <br />
**Düzey:**         Hata <br />
**Anahtar kelime -ler:**       <br />
**Kullanıcı:**          Sistem <br />
**Bilgisayar:**      *bilgisayar* <br />
**Açıklama:** TLS sunucu kimlik bilgisi oluşturulurken önemli bir hata oluştu. İç hata durumu 10013'dür.
 
### <a name="cause"></a>Nedeni

Bu sorun, güvenlik ilkeleri neden olur. TLS'nin (1,0 gibi) eski sürümleri devre dışı bırakıldığında, RDP erişimi başarısız olur.

### <a name="resolution"></a>Çözüm

RDP varsayılan protokol olarak TLS 1.0 kullanır. Ancak, protokol yeni standart olan TLS 1.1 olarak değiştirilebilir.

Bu sorunu gidermek için, [Azure VM'ye bağlanmak için RDP kullandığınızda sorun giderme kimlik doğrulama hatalarına](troubleshoot-authentication-error-rdp-vm.md#tls-version)bakın.

## <a name="scenario-3"></a>3. Senaryo

Uzak Masaüstü Bağlantı **Aracısı** rolünü VM'ye yüklediyseniz, son 24 saat içinde 2056 etkinliği veya olay 1296 olup olmadığını kontrol edin. CMD örneğinde, aşağıdaki komutları çalıştırın: 

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name=' Microsoft-Windows-TerminalServices-SessionBroker '] and EventID=2056 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name=' Microsoft-Windows-TerminalServices-SessionBroker-Client '] and EventID=1296 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Günlük Adı:**      Microsoft-Windows-TerminalServices-SessionBroker/Operasyonel <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-SessionBroker <br />
**Tarih:**          *saat* <br />
**Etkinlik ID:** 2056 <br />
**Görev Kategorisi:** (109) <br />
**Düzey:**         Hata <br />
**Anahtar kelime -ler:**       <br />
**Kullanıcı:**          AĞ HİzMETİ <br />
**Bilgisayar:**      *bilgisayar fqdn* <br />
**Açıklama:** Kaynak microsoft-Windows-TerminalServices-SessionBroker olay kimliği 2056 açıklaması bulunamıyor. Bu olayı yükselten bileşen yerel bilgisayarınıza yüklenmez veya yükleme bozuk. Bileşeni yerel bilgisayara yükleyebilir veya onarabilirsiniz. <br />
Olay başka bir bilgisayarda kaynaklanıyorsa, görüntü bilgilerinin olayla birlikte kaydedilmesi gerekir. <br />
Aşağıdaki bilgiler etkinliğe eklenmiştir: <br />
NULL <br />
NULL <br />
Veritabanına giriş başarısız oldu.

**Günlük Adı:**      Microsoft-Windows-TerminalServices-SessionBroker-Client/Operasyonel <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-SessionBroker-Client <br />
**Tarih:**          *saat* <br />
**Etkinlik Id:** 1296 <br />
**Görev Kategorisi:** (104) <br />
**Düzey:**         Hata <br />
**Anahtar kelime -ler:**       <br />
**Kullanıcı:**          AĞ HİzMETİ <br />
**Bilgisayar:**      *bilgisayar fqdn* <br />
**Açıklama:** Kaynakmicrosoft-Windows-TerminalServices-SessionBroker-Client'dan Olay Kimliği 1296 açıklaması bulunamıyor. Bu olayı yükselten bileşen yerel bilgisayarınıza yüklenmez veya yükleme bozuk. Bileşeni yerel bilgisayara yükleyebilir veya onarabilirsiniz.
Olay başka bir bilgisayarda kaynaklanıyorsa, görüntü bilgilerinin olayla birlikte kaydedilmesi gerekir.
Aşağıdaki bilgiler etkinliğe eklenmiştir:  <br />
*Metin* <br />
*Metin* <br />
Uzak Masaüstü Bağlantı Aracısı RPC iletişimi için hazır değildir.

### <a name="cause"></a>Nedeni

Uzak Masaüstü Bağlantı Aracısı sunucusunun ana bilgisayar adı değiştirildi, bu da desteklenen bir değişiklik değildir, çünkü bu sorun oluşur. 

Ana bilgisayar adı, çalışabilmek için Uzak Masaüstü Hizmeti çiftliği tarafından gerekli olan Windows İç Veritabanı'na girişler ve bağımlılıklar sahiptir. Çiftlik zaten inşa edildikten sonra ana bilgisayar adının değiştirilmesi birçok hataya neden olur ve aracı sunucunun çalışmayı durdurmasına neden olabilir.

### <a name="resolution"></a>Çözüm 

Bu sorunu gidermek için Uzak Masaüstü Bağlantı Aracısı rolü ve Windows İç Veritabanı yeniden yüklenmesi gerekir.

## <a name="next-steps"></a>Sonraki Adımlar

[Kanal Etkinlikleri](https://technet.microsoft.com/library/dn786445(v=ws.11).aspx)

[Schannel SSP Teknik Genel Bakış](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx)

[RDP Olay Kimliği 1058 & Olay 36870 Uzak Masaüstü Oturum Host Sertifikası & SSL İletişim ile Fails](https://blogs.technet.microsoft.com/askperf/2014/10/22/rdp-fails-with-event-id-1058-event-36870-with-remote-desktop-session-host-certificate-ssl-communication/)

[Bir Etki Alanı Denetleyicisi üzerinde Schannel 36872 veya Schannel 36870](https://blogs.technet.microsoft.com/instan/2009/01/05/schannel-36872-or-schannel-36870-on-a-domain-controller/)

[Olay Kimliği 1058 — Uzak Masaüstü Hizmetleri Kimlik Doğrulama ve Şifreleme](https://technet.microsoft.com/library/ee890862(v=ws.10).aspx)

