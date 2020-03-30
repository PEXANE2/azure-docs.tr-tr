---
title: Bir Azure VM'ye bağlandığınızda Uzak Masaüstü lisans sunucusu kullanılamıyor | Microsoft Dokümanlar
description: Uzak Masaüstü lisans sunucusu bulunmadığından RDP'nin başarısız sorunlarını nasıl gidereceklerini öğrenin | Microsoft Dokümanlar
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 154160f9a3fbd485ee6383bf3d5ff1c291520a75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71088516"
---
# <a name="remote-desktop-license-server-isnt-available-when-you-connect-to-an-azure-vm"></a>Bir Azure VM'ye bağlandığınızda Uzak Masaüstü lisans sunucusu kullanılamıyor

Bu makale, bir Azure sanal makinesine (VM) bağlanamadığınızda sorunu gidermeye yardımcı olur, çünkü lisans sağlamak için uzak masaüstü lisans sunucusu bulunmaz.

## <a name="symptoms"></a>Belirtiler

Sanal bir makineye (VM) bağlanmaya çalıştığınızda aşağıdaki senaryoları yaşarsınız:

- VM ekran görüntüsü, işletim sisteminin tamamen yüklü olduğunu ve kimlik bilgilerini beklediğini gösterir.
- Microsoft Uzak Masaüstü Protokolü (RDP) bağlantısı yapmaya çalıştığınızda aşağıdaki hata iletilerini alırsınız:

  - Lisans sağlamak için kullanılabilir Uzak Masaüstü lisans sunucusu bulunmadığından uzak oturum kesildi.

  - Uzak Masaüstü lisans sunucusu yok. Bu bilgisayar yetkisiz kullanım süresini geçmiş ve en azından geçerli bir Windows Server 2008 lisans sunucusuyla bağlantı kurmadığından Uzak Masaüstü Hizmetleri çalışmayı durdurur. Lisans Tanısını kullanmak için RD Session Host Server Configuration'ı açmak için bu iletiyi seçin.

Ancak, yönetim oturumu kullanarak vm'ye normal olarak bağlanabilirsiniz:

```
mstsc /v:<Server>[:<Port>] /admin
```

## <a name="cause"></a>Nedeni

Uzak Masaüstü lisans sunucusu uzak bir oturum başlatmak için lisans sağlamak için kullanılamıyorsa, bu sorun oluşur. VM'de Uzak Masaüstü Oturum Ana Bilgisayar rolü ayarlanmış olsa bile, çeşitli senaryolardan kaynaklanabilir:

- Çevrede hiçbir zaman Uzak Masaüstü lisanslama rolü olmadı ve yetkisiz kullanım süresi, 180 gün, bitti.
- Ortama Uzak Masaüstü lisansı yüklendi, ancak hiçbir zaman etkinleştirilmedi.
- Ortamdaki Uzak Masaüstü lisansı, bağlantıyı kurmak için istemci erişim lisanslarına (CA'lar) enjekte edilmemektedir.
- Ortama Uzak Masaüstü lisansı yüklendi. Kullanılabilir CAL'ler vardır, ancak düzgün şekilde yapılandırılmamıştır.
- Uzak Masaüstü lisansında KAL'lar vardır ve etkinleştirildi. Ancak, Uzak Masaüstü lisans sunucusundaki diğer bazı sorunlar, bu sunucunun ortamda lisans sağlamasını engeller.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için [işletim sistemi diskini yedekle](../windows/snapshot-copy-managed-disk.md) ve aşağıdaki adımları izleyin:

1. Yönetim oturumu kullanarak VM'ye bağlanın:

   ```
   mstsc /v:<Server>[:<Port>] /admin
   ```

    Yönetimsel bir oturum kullanarak VM'ye bağlanamıyorsanız, VM'ye erişmek için [Azure'daki Sanal Makine Seri](serial-console-windows.md) Konsolu'nu aşağıdaki gibi kullanabilirsiniz:

    1. **Seri konsolu (Preview) & Sorun Giderme** > Sorunu'nu seçerek Seri**Konsoluna**erişin. Özellik VM'de etkinse, VM'yi başarıyla bağlayabilirsiniz.

    2. CMD örneği için yeni bir kanal oluşturun. Kanalı başlatmak ve kanal adını almak için **CMD** girin.

    3. CMD örneğini çalıştıran kanala geçin. Bu durumda, kanal 1 olmalıdır:

       ```
       ch -si 1
       ```

    4. VM için yeniden **Enter'u** seçin ve geçerli bir kullanıcı adı ve parola, yerel veya etki alanı kimliği girin.

2. VM'de Uzak Masaüstü Oturum Ana Bilgisayar rolünün etkin olup olmadığını kontrol edin. Rol etkinleştirilirse, düzgün çalıştığından emin olun. Yükseltilmiş bir CMD örneğini açın ve aşağıdaki adımları izleyin:

    1. Uzak Masaüstü Oturum Ana Bilgisayar rolünün durumunu denetlemek için aşağıdaki komutu kullanın:

       ```
        reg query "HKLM\SOFTWARE\Microsoft\ServerManager\ServicingStorage\ServerComponentCache\RDS-RD-Server" /v InstallState
        ```

        Bu komut 0 değeri döndürürse, rolün devre dışı bırakıldığı ve 3 adıma gidebileceğiniz anlamına gelir.

    2. İlkeleri denetlemek ve gerektiğinde yeniden yapılandırmak için aşağıdaki komutu kullanın:

       ```
        reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode reg query "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers
       ```

        **LicensingMode** değeri kullanıcı başına 4'ten başka bir değere ayarlanmışsa, değeri 4 olarak ayarlayın:

         ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode /t REG_DWORD /d 4
        ```

       **SpecifiedLicenseServers** değeri yoksa veya yanlış lisans sunucusu bilgileri varsa, aşağıdaki gibi değiştirin:

       ```
        reg add "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers /t REG_MULTI_SZ /d "<FQDN / IP License server>"
       ```

    3. Kayıt defterinde herhangi bir değişiklik yaptıktan sonra VM'yi yeniden başlatın.

    4. CA'nız yoksa, Uzak Masaüstü Oturum Ana Bilgisayar rolünü kaldırın. O zaman RDP normale dönecek. VM'ye yalnızca iki eşzamanlı RDP bağlantısı sağlar:

        ```
       dism /ONLINE /Disable-feature /FeatureName:Remote-Desktop-Services
        ```

        VM Uzak Masaüstü lisanslama rolüne sahipse ve kullanılmamışsa, bu rolü de kaldırabilirsiniz:

       ```
        dism /ONLINE /Disable-feature /FeatureName:Licensing
       ```

    5. VM'nin Uzak Masaüstü lisans sunucusuna bağlanadığından emin olun. VM ve lisans sunucusu arasındaki bağlantı noktası 135'e bağlantınızı sınayabilirsiniz: 

       ```
       telnet <FQDN / IP License Server> 135
       ```

3. Ortamda Uzak Masaüstü lisans sunucusu yoksa ve bir sunucu istiyorsanız, [Uzak Masaüstü lisanslama rol hizmeti yükleyebilirsiniz.](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731765(v=ws.11)) Ardından [RDS lisansını yapılandırın.](https://techcommunity.microsoft.com/t5/Ask-The-Performance-Team/RD-Licensing-Configuration-on-Windows-Server-2012/ba-p/375383)

4. Uzak Masaüstü lisans sunucusu yapılandırılmış ve sağlıklıysa, Uzak Masaüstü lisans sunucusunun CA'larla etkinleştirildiğinden emin olun.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa, sorunuzun çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
