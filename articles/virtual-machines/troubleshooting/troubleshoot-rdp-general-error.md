---
title: Azure 'da bir Windows sanal makinesinde RDP genel hatası giderme | Microsoft Docs
description: Azure 'da bir Windows sanal makinesinde RDP genel hatası giderme hakkında bilgi edinin | Microsoft Docs
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
ms.openlocfilehash: df318fea4960601dcbfa84149fdc47bedc9104a2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079834"
---
# <a name="troubleshoot-an-rdp-general-error-in-azure-vm"></a>Azure VM 'de RDP genel hatasıyla ilgili sorunları giderme

Bu makalede, Azure 'da bir Windows sanal makinesine (VM) bir Uzak Masaüstü Protokolü (RDP) bağlantısı yaptığınızda karşılaşabileceğiniz genel bir hata açıklanır.

## <a name="symptom"></a>Belirti

Azure 'da bir Windows VM 'sine RDP bağlantısı yaptığınızda aşağıdaki genel hata iletisini alabilirsiniz:

**Uzak Masaüstü şu nedenlerden biri için uzak bilgisayara bağlanamıyor:**

1. **Sunucuya uzaktan erişim etkin değil**

2. **Uzak bilgisayar kapalı**

3. **Uzak bilgisayar ağda kullanılamıyor**

**Uzak bilgisayarın açık ve ağa bağlı olduğundan ve uzaktan erişimin etkinleştirildiğinden emin olun.**

## <a name="cause"></a>Nedeni

Bu sorun aşağıdaki nedenlerle oluşabilir:

### <a name="cause-1"></a>Neden 1

RDP bileşeni aşağıdaki şekilde devre dışıdır:

- Bileşen düzeyinde
- Dinleyici düzeyinde
- Terminal sunucusunda
- Uzak Masaüstü Oturumu Ana Bilgisayarı rolünde

### <a name="cause-2"></a>Neden 2

Uzak Masaüstü Hizmetleri (TermService) çalışmıyor.

### <a name="cause-3"></a>Neden 3

RDP dinleyicisi yanlış yapılandırılmış.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin işletim sistemi diskinin bir yedek olarak anlık görüntüsünü alın. Bu sorunu çözmek için seri denetim kullanın veya VM 'yi çevrimdışı onarın.

### <a name="serial-console"></a>Seri Konsol

#### <a name="step-1-open-cmd-instance-in-serial-console"></a>1. Adım: Seri konsol CMD örneğini açın

1. **Destek & sorun giderme** [Serial Console](serial-console-windows.md)  >  **seri konsol (Önizleme)** öğesini seçerek seri konsoluna erişin. Özellik VM 'de etkinse VM 'yi başarıyla bağlayabilirsiniz.

2. Bir CMD örneği için yeni bir kanal oluşturun. Kanal adını almak için kanalı başlatmak üzere **cmd** yazın.

3. CMD örneğini çalıştıran kanala geçiş yapın, bu durumda Kanal 1 olmalıdır.

   ```
   ch -si 1
   ```

#### <a name="step-2-check-the-values-of-rdp-registry-keys"></a>2. Adım: RDP kayıt defteri anahtarlarının değerlerini denetleyin:

1. RDP 'nin Grup ilkeleri tarafından devre dışı bırakılıp bırakılmadığını kontrol edin.

    ```
    REM Get the group policy 
    reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections
    ```
    Grup İlkesi RDP 'nin devre dışı bırakıldığını belirtir (fDenyTSConnections değeri 0x1), TermService hizmetini etkinleştirmek için aşağıdaki komutu çalıştırın. Kayıt defteri anahtarı bulunamazsa, RDP 'yi devre dışı bırakmak için yapılandırılmış bir grup ilkesi yoktur. Sonraki adıma geçebilirsiniz.

    ```
    REM update the fDenyTSConnections value to enable TermService service
    reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fDenyTSConnections /t REG_DWORD /d 0 /f
    ```
    > [!NOTE]
    > Bu adım, TermService hizmetini geçici olarak sunar. Grup İlkesi ayarları yenilendiğinde değişiklik sıfırlanır. Bu sorunu çözmek için, TermService hizmetinin yerel Grup İlkesi veya etki alanı Grup İlkesi tarafından devre dışı bırakılıp bırakılmadığını denetlemeniz ve ardından ilke ayarlarını karşılık olarak güncelleştirmeniz gerekir.
    
2. Geçerli uzak bağlantı yapılandırmasını denetleyin.
    ```
    REM Get the local remote connection setting
    reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections
    ```
    Komut 0x1 döndürürse, sanal makine uzak bağlantıya izin vermez. Ardından, aşağıdaki komutu kullanarak uzak bağlantıya izin verin:
     ```
     reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
     ```
    
1. Terminal sunucusunun geçerli yapılandırmasını denetleyin.

    ```
    REM Get the local remote connection setting
    reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled
    ```

      Komut 0 döndürürse, Terminal sunucusu devre dışıdır. Ardından, Terminal sunucusunu şu şekilde etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSEnabled /t REG_DWORD /d 1 /f
      ```

3. Sunucu bir Terminal sunucusu çiftliğinde (RDS veya Citrix), Terminal sunucusu modülü boşalt moduna ayarlanır. Terminal sunucusu modülünün geçerli modunu kontrol edin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode
      ```

      Komut 1 döndürürse, Terminal sunucusu modülü boşalt moduna ayarlanır. Ardından, modülü çalışma moduna aşağıdaki şekilde ayarlayın:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSServerDrainMode /t REG_DWORD /d 0 /f
      ```

4. Terminal sunucusuna bağlanıp bağlanamaıp bağlanamayacağını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled
      ```

      Komut 1 döndürürse, Terminal sunucusuna bağlanamazsınız. Ardından, bağlantıyı aşağıdaki şekilde etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v TSUserEnabled /t REG_DWORD /d 0 /f
      ```
5. RDP dinleyicisinin geçerli yapılandırmasını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation
      ```

      Komut 0 döndürürse, RDP dinleyicisi devre dışıdır. Ardından, dinleyiciyi aşağıdaki gibi etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fEnableWinStation /t REG_DWORD /d 1 /f
      ```

6. RDP dinleyicisine bağlanıp bağlanamayacağını denetleyin.

      ```
      reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled
      ```

   Komut 1 döndürürse, RDP dinleyicisine bağlanamazsınız. Ardından, bağlantıyı aşağıdaki şekilde etkinleştirin:

      ```
      reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v fLogonDisabled /t REG_DWORD /d 0 /f
      ```

7. VM’yi yeniden başlatın.

8. Yazarak CMD örneğinden çıkın `exit` ve sonra iki kez **ENTER** tuşuna basın.

9. Yazarak VM 'yi yeniden başlatın `restart` ve VM 'ye bağlanın.

Sorun devam ediyorsa 2. adıma geçin.

#### <a name="step-2-enable-remote-desktop-services"></a>2. Adım: Uzak Masaüstü Hizmetleri 'ni etkinleştirme

Daha fazla bilgi için bkz. [Uzak Masaüstü Hizmetleri bir Azure VM üzerinde başlamıyor](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>3. Adım: RDP dinleyicisini sıfırlama

Daha fazla bilgi için bkz. [Azure VM 'de Uzak Masaüstü bağlantısı kesiliyor](troubleshoot-rdp-intermittent-connectivity.md).

### <a name="offline-repair"></a>Çevrimdışı onarma

#### <a name="step-1-turn-on-remote-desktop"></a>1. Adım: Uzak Masaüstü 'Nü etkinleştirme

1. [İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin](./troubleshoot-recovery-disks-portal-windows.md).
2. Kurtarma VM 'sine bir Uzak Masaüstü bağlantısı başlatın.
3. Diskin Disk Yönetimi konsolunda **çevrimiçi** olarak işaretlendiğinden emin olun. Bağlı işletim sistemi diskine atanan sürücü harfini unutmayın.
4. Kurtarma VM 'sine bir Uzak Masaüstü bağlantısı başlatın.
5. Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**). Aşağıdaki komut dosyalarını çalıştırın. Bu betikte, bağlı işletim sistemi diskine atanan sürücü harfinin F olduğunu varsaytık. Bu sürücü harfini VM 'niz için uygun değerle değiştirin.

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

6. VM etki alanına katılmış ise, RDP 'yi devre dışı bırakamayacak bir grup ilkesi olup olmadığını görmek için aşağıdaki kayıt defteri anahtarını kontrol edin. 

      ```
      HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services\fDenyTSConnectionS
      ```

      Bu anahtar değer 1 olarak ayarlanırsa, RDP ilke tarafından devre dışı bırakılır. GPO ilkesi aracılığıyla uzak masaüstü 'Nü etkinleştirmek için, etki alanı denetleyicisinden aşağıdaki ilkeyi değiştirin:

   
      **Bilgisayar Yapılandırması \ şablonlar:**

      İlke definitions\Windows uzak masaüstü Hizmetleri\uzak Masaüstü oturumu Host\connections\kullanıcıların Uzak Masaüstü Hizmetleri kullanarak uzaktan bağlanmasına izin ver
  
1. Diski Kurtarma VM 'sinden ayırın.
1. [Diskten yeni BIR VM oluşturun](../windows/create-vm-specialized.md).

Sorun devam ediyorsa 2. adıma geçin.

#### <a name="step-2-enable-remote-desktop-services"></a>2. Adım: Uzak Masaüstü Hizmetleri 'ni etkinleştirme

Daha fazla bilgi için bkz. [Uzak Masaüstü Hizmetleri bir Azure VM üzerinde başlamıyor](troubleshoot-remote-desktop-services-issues.md).

#### <a name="step-3-reset-rdp-listener"></a>3. Adım: RDP dinleyicisini sıfırlama

Daha fazla bilgi için bkz. [Azure VM 'de Uzak Masaüstü bağlantısı kesiliyor](troubleshoot-rdp-intermittent-connectivity.md).

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
