---
title: Azure VM|'nde Uzak Masaüstü sık sık keser| Microsoft Dokümanlar
description: Azure VM'de Uzak Masaüstü'nün sık sık kopukluklarını nasıl gidereceklerini öğrenin.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/24/2018
ms.author: genli
ms.openlocfilehash: c22a401a6b25f7bb2c27a10e52214fa42ac6089b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77918232"
---
# <a name="remote-desktop-disconnects-frequently-in-azure-vm"></a>Azure VM ile Uzak Masaüstü bağlantısı sık sık kesiliyor

Bu makalede, Uzak Masaüstü Protokolü RDP aracılığıyla bir Azure sanal makine (VM) için sık sık kopuklukları nasıl giderebilirsiniz açıklanmaktadır.


## <a name="symptom"></a>Belirti

Oturumlarınız sırasında aralıklı RDP bağlantısı sorunlarıyla karşı karşıya kaldığınız zaman. Başlangıçta VM'ye bağlanabilirsiniz, ancak sonra bağlantı düşer.

## <a name="cause"></a>Nedeni

RDP Dinleyicisi yanlış yapılandırılırsa bu sorun oluşabilir. Genellikle, bu sorun özel bir görüntü kullanan bir VM oluşur.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen [VM'nin işletim sistemi diskinin](../windows/snapshot-copy-managed-disk.md) bir anlık görüntüsünü yedek olarak alın. 

Bu sorunu gidermek için, VM'nin işletim sistemi diskini kurtarma VM'ine ekleyerek Seri denetimini kullanın veya [VM çevrimdışı onarın.](#repair-the-vm-offline)

### <a name="serial-control"></a>Seri kontrol

1. Seri [Konsola bağlanın ve CMD örneğini açın.](./serial-console-windows.md) Ardından, RDP yapılandırmalarını sıfırlamak için aşağıdaki komutları çalıştırın. VM'nizde Seri Konsol etkinleştirilemezse, bir sonraki adıma geçin.
2. RDP Güvenlik Katmanını 0'a indirin. Bu ayarda, sunucu ve istemci arasındaki iletişim yerel RDP şifrelemesini kullanır.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
3. Eski RDP istemcilerinin bağlanmasına izin vermek için şifreleme düzeyini en aza indirin.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
4. İstemci bilgisayarın kullanıcı yapılandırmasını yüklemek için RDP'yi ayarlayın.

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
5. RDP Canlı Tutma denetimini etkinleştirin:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
6. RDP Yeniden Bağlanma denetimini ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f
7. RDP Oturum Süresi denetimini ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
8. RDP Bağlantı Dansı Süresi denetimini ayarlayın: 

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f
        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
9. RDP Bağlantı Süresi denetimini ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
10. RDP Oturum Boşta Zaman denetimini ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxIdleTime' /t REG_DWORD /d 0 /f
11. "Maksimum eşzamanlı bağlantıları sınırlandır" denetimini ayarlayın:

        REG ADD "HKLM\SYSTEM\CurrentControlSet\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d 4294967295 /f

12. VM'yi yeniden başlatın ve RDP kullanarak ona yeniden bağlanmayı deneyin.

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onar

1. [Os diskini kurtarma VM'sine takın.](../windows/troubleshoot-recovery-disks-portal.md)
2. İşletim VM'sine işletim sistemi diski bağlandıktan sonra, diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın. Ekli işletim sistemi diskine atanan sürücü mektubuna dikkat edin.
3. Eklediğiniz işletim sistemi diskinde **\windows\system32\config** klasörüne gidin. Geri alma gerekirse, bu klasördeki tüm dosyaları yedek olarak kopyalayın.
4. Kayıt Defteri Düzenleyicisi'ni başlatın (regedit.exe).
5. **HKEY_LOCAL_MACHINE** tuşunu seçin. Menüde **Dosya** > **Yük Kovanı'nı**seçin:
6. Eklediğiniz işletim sistemi diskindeki **\windows\system32\config\SYSTEM** klasörüne göz atın. Kovanın adı için **BROKENSYSTEM'i**girin. Yeni kayıt kovanı **HKEY_LOCAL_MACHINE** anahtarının altında görüntülenir. Sonra yazılım kovan **\windows\system32\config\SOFTWARE** **HKEY_LOCAL_MACHINE** anahtarı altında yükleyin. Kovan yazılımının adı için **BROKENSOFTWARE'i**girin. 
7. Yükseltilmiş komut istemi penceresini açın **(Yönetici olarak çalıştırın)** ve RDP yapılandırmalarını sıfırlamak için kalan adımlarda komutları çalıştırın. 
8. Sunucu ve istemci arasındaki iletişimin yerel RDP Şifrelemesini kullanması için RDP Güvenlik Katmanı'nı 0'a düşürün:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'SecurityLayer' /t REG_DWORD /d 0 /f
9. Eski RDP istemcilerinin bağlanmasına izin vermek için şifreleme düzeyini en aza indirin:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MinEncryptionLevel' /t REG_DWORD /d 1 /f
10. İstemci makinesinin kullanıcı yapılandırmasını yüklemek için RDP'yi ayarlayın.

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fQueryUserConfigFromLocalMachine' /t REG_DWORD /d 1 /f
11. RDP Canlı Tutma denetimini etkinleştirin:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'KeepAliveTimeout' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveEnable' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'KeepAliveInterval' /t REG_DWORD /d 1 /f
12. RDP Yeniden Bağlanma denetimini ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 
        
        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritReconnectSame' /t REG_DWORD /d 0 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fReconnectSame' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v 'fDisableAutoReconnect' /t REG_DWORD /d 0 /f

13. RDP Oturum Süresi denetimini ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxSessionTime' /t REG_DWORD /d 1 /f
14. RDP Bağlantı Dansı Süresi denetimini ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxDisconnectionTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxDisconnectionTime' /t REG_DWORD /d 0 /f
15. RDP Bağlantı Süresi denetimini ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxConnectionTime' /t REG_DWORD /d 0 /f
16. RDP Oturum Boşta Zaman denetimini ayarlayın: REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'fInheritMaxIdleTime' /t REG_DWORD /d 1 /f 

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v ' MaxIdleTime' /t REG_DWORD /d 0 /f
17. "Maksimum eşzamanlı bağlantıları sınırlandır" denetimini ayarlayın:

        REG ADD "HKLM\BROKENSYSTEM\ControlSet001\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f

        REG ADD "HKLM\BROKENSYSTEM\ControlSet002\control\Terminal Server\Winstations\RDP-Tcp" /v 'MaxInstanceCount' /t REG_DWORD /d ffffffff /f
18. VM'yi yeniden başlatın ve RDP kullanarak ona yeniden bağlanmayı deneyin.

## <a name="need-help"></a>Yardıma mı ihtiyacınız var? 
Desteğe başvurun. Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).





