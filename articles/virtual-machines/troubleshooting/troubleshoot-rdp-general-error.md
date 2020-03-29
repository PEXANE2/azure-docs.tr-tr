---
title: Azure'daki bir Windows VM'de RDP genel hatasını giderme | Microsoft Dokümanlar
description: Azure'daki bir Windows VM'de RDP genel hatasını nasıl gidereceklerini öğrenin | Microsoft Dokümanlar
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 7fc0fbf3362d18284ad6a80afa6396b6be1270a9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058010"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Azure VM'de bir RDP genel hatalarını giderme

Bu makalede, Azure'daki bir Windows Sanal Makine'ye (VM) Uzak Masaüstü Protokolü (RDP) bağlantısı yaptığınızda karşılaşabileceğiniz genel bir hata açıklanmaktadır.

## <a name="symptom"></a>Belirti

Azure'da bir Window VM'ye RDP bağlantısı yaptığınızda, aşağıdaki genel hata iletisini alabilirsiniz:

**Uzak Masaüstü aşağıdaki nedenlerden dolayı uzak bilgisayara bağlanamaz:**

1. **Sunucuya uzaktan erişim etkinleştirildi**

2. **Uzak Bilgisayar kapatıldı**

3. **Uzak bilgisayar ağda kullanılamıyor**

**Uzak bilgisayarın açık olduğundan ve ağa bağlı olduğundan ve uzaktan erişimin etkin olduğundan emin olun.**

## <a name="cause"></a>Nedeni

Aşağıdaki nedenlerden dolayı bu sorun oluşabilir:

### <a name="cause-1"></a>Neden 1

RDP bileşeni aşağıdaki gibi devre dışı bırakılır:

- Bileşen düzeyinde
- Dinleyici düzeyinde
- Terminal sunucusunda
- Uzak Masaüstü Oturum Ana Bilgisayar rolünde

### <a name="cause-2"></a>Neden 2

Uzak Masaüstü Hizmetleri (TermService) çalışmıyor.

### <a name="cause-3"></a>Neden 3

RDP dinleyicisi yanlış yapılandırılmıştır.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek [için, işletim sistemi diskini yedekle](../windows/snapshot-copy-managed-disk.md)ve [işletim sistemi diskini kurtarma VM'ine takın](troubleshoot-recovery-disks-portal-windows.md)ve ardından adımları izleyin.

### <a name="serial-console"></a>Seri Konsol

#### <a name="step-1-open-cmd-instance-in-serial-console"></a>Adım 1: Seri konsolda CMD örneğini açın

1. Seri [konsolu](serial-console-windows.md) & **Sorun Giderme** > **Seri konsolu (Önizleme)** seçerek Seri Konsoluna erişin. Özellik VM'de etkinse, VM'yi başarıyla bağlayabilirsiniz.

2. CMD örneği için yeni bir kanal oluşturun. Kanal adını almak için kanalı başlatmak için **CMD** yazın.

3. CMD örneğini çalıştıran kanala geçin, bu durumda kanal 1 olmalıdır.

   ```
   ch -si 1
   ```

#### <a name="step-2-check-the-values-of-rdp-registry-keys"></a>Adım 2: RDP kayıt defteri anahtarlarının değerlerini kontrol edin:

1. RDP'nin polis tarafından devre dışı bırakıldığını kontrol edin.

      ```
      REM Get the local policy 
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server " /v fDenyTSConnections

      REM Get the domain policy if any
      reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
      ```

      - Etki alanı ilkesi varsa, yerel ilke deki kurulum üzerine yazılır.
      - Etki alanı ilkesi RDP'nin devre dışı bırakıldığını belirtiyorsa (1), ardından AD ilkesini etki alanı denetleyicisinden güncelleştirin.
      - Etki alanı ilkesi RDP'nin etkin olduğunu (0) belirtiyorsa, güncelleştirme gerekmez.
      - Etki alanı ilkesi yoksa ve yerel ilke RDP'nin devre dışı bırakıldığını belirtirse (1), aşağıdaki komutu kullanarak RDP'yi etkinleştirin: 
      
            reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
                  

2. Terminal sunucusunun geçerli yapılandırmasını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
      ```

      Komut 0 döndürürse, terminal sunucusu devre dışı bırakılır. Ardından terminal sunucusunu aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

3. Sunucu bir terminal sunucu çiftliğinde (RDS veya Citrix) ise Terminal Server modülü boşaltma moduna ayarlanır. Terminal Sunucusu modülünün geçerli modunu kontrol edin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

      Komut 1 döndürürse, Terminal Server modülü boşaltma moduna ayarlanır. Ardından, modülü aşağıdaki gibi çalışma moduna ayarlayın:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

4. Terminal sunucusuna bağlanıp bağlanamayacağınızı kontrol edin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

      Komut 1 döndürürse, terminal sunucusuna bağlanamazsınız. Ardından, bağlantıyı aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```
5. RDP dinleyicisinin geçerli yapılandırmasını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

      Komut 0 döndürürse, RDP dinleyicisi devre dışı bırakılır. Daha sonra dinleyiciyi aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

6. RDP dinleyicisine bağlanıp bağlanamayacağınızı kontrol edin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   Komut 1'i döndürürse, RDP dinleyicisine bağlanamazsınız. Ardından, bağlantıyı aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

7. VM’yi yeniden başlatın.

8. CMD örneğinden yazarak `exit`çıkın ve **ardından** enter tuşuna iki kez basın.

9. Yazarak `restart`VM'yi yeniden başlatın ve vm'ye bağlanın.

Sorun hala olursa, adım 2'ye geçin.

#### <a name="step-2-enable-remote-desktop-services"></a>Adım 2: Uzak masaüstü hizmetlerini etkinleştirme

Daha fazla bilgi için bkz. [Uzak Masaüstü Hizmetleri Azure VM'de başlatılmıyor.](troubleshoot-remote-desktop-services-issues.md)

#### <a name="step-3-reset-rdp-listener"></a>Adım 3: RDP dinleyicisini sıfırlama

Daha fazla bilgi için Azure [VM'de Uzak Masaüstü bağlantı larının sık sık kesilmesine](troubleshoot-rdp-intermittent-connectivity.md)bakın.

### <a name="offline-repair"></a>Çevrimdışı onarım

#### <a name="step-1-turn-on-remote-desktop"></a>Adım 1: Uzak Masaüstünü Aç

1. [Os diskini kurtarma VM'sine takın.](../windows/troubleshoot-recovery-disks-portal.md)
2. Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın.
3. Diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın. Ekli işletim sistemi diskine atanan sürücü mektubuna dikkat edin.
4. Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın.
5. Yükseltilmiş bir komut istemi oturumu açın (**Yönetici olarak çalıştırın).** Aşağıdaki komut dosyalarını çalıştırın. Bu komut dosyasında, ekli işletim sistemi diskine atanan sürücü harfinin F olduğunu varsayıyoruz.

      ```
      reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv 
      reg load HKLM\BROKENSOFTWARE F:\windows\system32\config\SOFTWARE.hiv 
 
      REM Ensure that Terminal Server is enabled 

      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f 

      REM Ensure Terminal Service is not set to Drain mode 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f 

      REM Ensure Terminal Service has logon enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f 

      REM Ensure the RDP Listener is not disabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f 

      REM Ensure the RDP Listener accepts logons 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f 

      REM RDP component is enabled 
      reg add "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
      reg add "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f 

      reg unload HKLM\BROKENSYSTEM 
      reg unload HKLM\BROKENSOFTWARE 
      ```

6. VM etki alanı birleştirilmişse, RDP'yi devre dışı edecek bir grup ilkesi olup olmadığını görmek için aşağıdaki kayıt defteri anahtarını denetleyin. 

      ```
      HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
      ```

      Bu anahtar değeri 1 olarak ayarlanmışsa, bu RDP'nin ilke tarafından devre dışı bırakıldığı anlamına gelir. GPO ilkesi aracılığıyla Uzak Masaüstü'nü etkinleştirmek için aşağıdaki ilkeyi etki alanı denetleyicisinden değiştirin:

   
      **Bilgisayar Yapılandırması\İlkeler\Yönetim Şablonları:**

      İlke tanımları\Windows Bileşenleri\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayar\Bağlantılar\Uzak Masaüstü Hizmetlerini kullanarak kullanıcıların uzaktan bağlanmasına izin ver
  
1. Diski kurtarma VM'sinden ayırın.
1. [Diskten yeni bir VM oluşturun.](../windows/create-vm-specialized.md)

Sorun hala olursa, adım 2'ye geçin.

#### <a name="step-2-enable-remote-desktop-services"></a>Adım 2: Uzak masaüstü hizmetlerini etkinleştirme

Daha fazla bilgi için bkz. [Uzak Masaüstü Hizmetleri Azure VM'de başlatılmıyor.](troubleshoot-remote-desktop-services-issues.md)

#### <a name="step-3-reset-rdp-listener"></a>Adım 3: RDP dinleyicisini sıfırlama

Daha fazla bilgi için Azure [VM'de Uzak Masaüstü bağlantı larının sık sık kesilmesine](troubleshoot-rdp-intermittent-connectivity.md)bakın.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
