---
title: Azure aracısı olmadan yerel bir Windows parolası sıfırlama | Microsoft Dokümanlar
description: Azure konuk aracı sıyüklenmediğinde veya bir VM'de çalışmıyorsa yerel bir Windows kullanıcı hesabının parolasını sıfırlama
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: genli
ms.openlocfilehash: becbf88aeda164f7d916cbc1f1ace89262cc1a3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921632"
---
# <a name="reset-local-windows-password-for-azure-vm-offline"></a>Çevrimdışı Azure VM için yerel Windows parolasını sıfırlama
Azure konuk aracısının yüklenmesi koşuluyla [Azure portalı veya Azure PowerShell'i](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak Azure'daki bir VM'nin yerel Windows parolasını sıfırlayabilirsiniz. Bu yöntem, Bir Azure VM için parolasıfırlamanın birincil yoludur. Azure konuk aracısının yanıt vermemesiyle ilgili sorunlarla karşılaşırsanız veya özel bir resim yükledikten sonra yüklemezseniz, windows parolasını el ile sıfırlayabilirsiniz. Bu makalede, kaynak işletim sistemi sanal diskini başka bir VM'ye ekleyerek yerel hesap parolasını nasıl sıfırlaylayacağız ayrıntıları. Bu makalede açıklanan adımlar Windows etki alanı denetleyicileri için geçerli değildir. 

> [!WARNING]
> Bu işlemi yalnızca son çare olarak kullanın. Önce [Azure portalını veya Azure PowerShell'i](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak parolayı sıfırlamayı deneyin.

## <a name="overview-of-the-process"></a>İşleme genel bakış
Azure konuk aracısına erişim olmadığında Azure'da bir Windows VM için yerel parola sıfırlama gerçekleştirmenin temel adımları aşağıdaki gibidir:

1. Etkilenen VM'yi durdurun.
1. VM işletim sistemi diski için anlık görüntü oluşturun.
1. Anlık görüntüden işletim sistemi diskinin bir kopyasını oluşturun.
1. Kopyalanan işletim sistemi diskini başka bir Windows VM'ye takın ve bağlayın, ardından diskte bazı config dosyaları oluşturun. Dosyalar parolayı sıfırlamanıza yardımcı olur.
1. Kopyalanan işletim sistemi diskini sorun giderme VM'den sökün ve ayırın.
1. Os diskini etkilenen VM ile değiştirin.

## <a name="detailed-steps-for-the-vm-with-resource-manager-deployment"></a>Kaynak Yöneticisi dağıtımı ile VM için ayrıntılı adımlar

> [!NOTE]
> Adımlar Windows etki alanı denetleyicileri için geçerli değildir. Yalnızca bağımsız sunucuda veya etki alanının üyesi olan bir sunucuda çalışır.

Aşağıdaki adımları denemeden önce azure [portalını veya Azure PowerShell'i](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak parolayı her zaman sıfırlamayı deneyin. Başlamadan önce VM'nizin yedeğinin olduğundan emin olun.

1. Etkilenen VM işletim sistemi diski için anlık görüntü alın, anlık görüntüden bir disk oluşturun ve ardından diski sorun giderme VM'sine takın. Daha fazla bilgi için, [Azure portalını kullanarak işletim sistemi diskini kurtarma VM'ine ekleyerek Windows VM Sorun Giderme'ye](troubleshoot-recovery-disks-portal-windows.md)bakın.
2. Uzak Masaüstü'nü kullanarak sorun giderme VM'ine bağlanın.
3. `gpt.ini` Kaynak `\Windows\System32\GroupPolicy` VM sürücü (gpt.ini varsa, gpt.ini.bak yeniden ad:
   
   > [!WARNING]
   > Sorun giderme VM için işletim sistemi sürücüsü C:\Windows'da aşağıdaki dosyaları yanlışlıkla oluşturmadığınızdan emin olun. Veri diski olarak iliştirilmiş kaynak VM'niz için işletim sistemi sürücüsünde aşağıdaki dosyaları oluşturun.
   
   * Oluşturduğunuz dosyaya `gpt.ini` aşağıdaki satırları ekleyin:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![gpt.ini oluştur](./media/reset-local-password-without-agent/create-gpt-ini.png)

4. `scripts.ini` 'de `\Windows\System32\GroupPolicy\Machines\Scripts\`oluşturun. Gizli klasörlerin gösterildiğinden emin olun. Gerekirse, veya `Machine` `Scripts` klasörleri oluşturun.
   
   * Oluşturduğunuz dosyayı `scripts.ini` aşağıdaki satırları ekleyin:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Komut dosyaları oluşturma.ini](./media/reset-local-password-without-agent/create-scripts-ini.png)

5. Aşağıdaki `FixAzureVM.cmd` `\Windows\System32` içeriklerle, değiştirerek `<username>` ve `<newpassword>` kendi değerlerinizle oluşturun:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![FixAzureVM.cmd oluştur](./media/reset-local-password-without-agent/create-fixazure-cmd.png)
   
    Yeni parolayı tanımlarken VM'niz için yapılandırılan parola karmaşıklığı gereksinimlerini karşılamanız gerekir.

6. Azure portalında, diski sorun giderme VM'sinden ayırın.

7. [Etkilenen VM için işletim sistemi diskini değiştirin.](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm)

8. Yeni VM çalışmaya başladıktan sonra, komut dosyasında belirttiğiniz yeni parolayla Uzak Masaüstü'nü kullanarak VM'ye `FixAzureVM.cmd` bağlanın.

9. Uzak oturumunuzdan yeni VM'ye, ortamı temizlemek için aşağıdaki dosyaları kaldırın:
    
    * Kaynak: %windir%\System32
      * FixAzureVM.cmd kaldırmak
    * %windir%\System32\GroupPolicy\Machine\Scripts gönderen
      * komut dosyalarını kaldırın.ini
    * Kaynak : %windir%\System32\GroupPolicy
      * gpt.ini kaldırın (gpt.ini daha önce varsa ve gpt.ini.bak olarak yeniden adlandırıldıysanız, .bak dosyasını gpt.ini olarak yeniden adlandırın)

## <a name="detailed-steps-for-classic-vm"></a>Klasik VM için ayrıntılı adımlar

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

> [!NOTE]
> Adımlar Windows etki alanı denetleyicileri için geçerli değildir. Yalnızca bağımsız sunucuda veya etki alanının üyesi olan bir sunucuda çalışır.

Aşağıdaki adımları denemeden önce azure [portalını veya Azure PowerShell'i](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/reset-rdp) kullanarak parolayı her zaman sıfırlamayı deneyin. Başlamadan önce VM'nizin yedeğinin olduğundan emin olun. 

1. Azure portalında etkilenen VM'yi silin. VM'yi silmek yalnızca Azure içindeki VM'nin başvurusu olan meta verileri siler. Sanal diskler VM silindiğinde korunur:
   
   * Azure portalında VM'yi seçin ve *sonra Sil'i*tıklatın:
     
     ![Varolan VM'yi silme](./media/reset-local-password-without-agent/delete-vm-classic.png)

2. Kaynak VM'nin işletim sistemi diskini sorun giderme VM'sine takın. Sorun giderme VM kaynak VM's OS disk (gibi) `West US`aynı bölgede olmalıdır:
   
   1. Azure portalındaki sorun giderme VM'sini seçin. *Diskler* | *Ekle'yi*tıklatın varolan :
     
      ![Varolan diski ekleme](./media/reset-local-password-without-agent/disks-attach-existing-classic.png)
     
   2. *VHD Dosya'yı* seçin ve ardından kaynak VM'nizi içeren depolama hesabını seçin:
     
      ![Depolama hesabı seçme](./media/reset-local-password-without-agent/disks-select-storage-account-classic.png)
     
   3. İşaretli kutuyu *işaretli işaretli olarak işaretleyin Klasik depolama hesaplarını göster,* ardından kaynak kapsayıcıyı seçin. Kaynak kapsayıcı genellikle *vhds:*
     
      ![Depolama kabını seçin](./media/reset-local-password-without-agent/disks-select-container-classic.png)

      ![Depolama kabını seçin](./media/reset-local-password-without-agent/disks-select-container-vhds-classic.png)
     
   4. Eklemek için OS vhd'yi seçin. İşlemi tamamlamak için *Seç'i* tıklatın:
     
      ![Kaynak sanal diski seçin](./media/reset-local-password-without-agent/disks-select-source-vhd-classic.png)

   5. Diski takmak için Tamam'ı tıklatın

      ![Varolan diski ekleme](./media/reset-local-password-without-agent/disks-attach-okay-classic.png)

3. Uzak Masaüstü'nü kullanarak sorun giderme VM'ine bağlanın ve kaynak VM'nin işletim sistemi diskinin görünür olduğundan emin olun:

   1. Azure portalında sorun giderme VM'sini seçin ve *Bağlan'ı*tıklatın.

   2. İndirilen RDP dosyasını açın. Sorun giderme VM'nin kullanıcı adını ve parolasını girin.

   3. Dosya Gezgini'nde, iliştirdiğiniz veri diskini arayın. Kaynak VM'nin VHD'si sorun giderme VM'ine bağlı tek veri diskiyse, F: sürücü olmalıdır:
     
      ![Bağlı veri diskini görüntüleme](./media/reset-local-password-without-agent/troubleshooting-vm-file-explorer-classic.png)

4. `gpt.ini` Kaynak `\Windows\System32\GroupPolicy` VM sürücüsü (varsa, `gpt.ini` yeniden `gpt.ini.bak`adlandırmak) oluşturun:
   
   > [!WARNING]
   > Sorun giderme VM için işletim `C:\Windows`sistemi sürücüsünde yanlışlıkla aşağıdaki dosyaları oluşturmadığınızdan emin olun. Veri diski olarak iliştirilmiş kaynak VM'niz için işletim sistemi sürücüsünde aşağıdaki dosyaları oluşturun.
   
   * Oluşturduğunuz dosyaya `gpt.ini` aşağıdaki satırları ekleyin:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![gpt.ini oluştur](./media/reset-local-password-without-agent/create-gpt-ini-classic.png)

5. `scripts.ini` 'de `\Windows\System32\GroupPolicy\Machines\Scripts\`oluşturun. Gizli klasörlerin gösterildiğinden emin olun. Gerekirse, veya `Machine` `Scripts` klasörleri oluşturun.
   
   * Oluşturduğunuz dosyayı `scripts.ini` aşağıdaki satırları ekleyin:

     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Komut dosyaları oluşturma.ini](./media/reset-local-password-without-agent/create-scripts-ini-classic.png)

6. Aşağıdaki `FixAzureVM.cmd` `\Windows\System32` içeriklerle, değiştirerek `<username>` ve `<newpassword>` kendi değerlerinizle oluşturun:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![FixAzureVM.cmd oluştur](./media/reset-local-password-without-agent/create-fixazure-cmd-classic.png)
   
    Yeni parolayı tanımlarken VM'niz için yapılandırılan parola karmaşıklığı gereksinimlerini karşılamanız gerekir.

7. Azure portalında, diski sorun giderme VM'sinden ayırın:
   
   1. Azure portalında sorun giderme VM'sini seçin, *Diskler'i*tıklatın.
   
   2. Adım 2'de bağlı olan veri diskini seçin, **Ayır'ı**tıklatın, ardından **Tamam'ı**tıklatın.

     ![Diski ayırma](./media/reset-local-password-without-agent/data-disks-classic.png)
     
     ![Diski ayırma](./media/reset-local-password-without-agent/detach-disk-classic.png)

8. Kaynak VM'nin işletim sistemi diskinden bir VM oluşturun:
   
     ![Şablondan VM oluşturma](./media/reset-local-password-without-agent/create-new-vm-from-template-classic.png)

     ![Şablondan VM oluşturma](./media/reset-local-password-without-agent/choose-subscription-classic.png)

     ![Şablondan VM oluşturma](./media/reset-local-password-without-agent/create-vm-classic.png)

## <a name="complete-the-create-virtual-machine-experience"></a>Sanal makine oluştur deneyimini tamamlayın

1. Yeni VM çalışmaya başladıktan sonra, komut dosyasında belirttiğiniz yeni parolayla Uzak Masaüstü'nü kullanarak VM'ye `FixAzureVM.cmd` bağlanın.

2. Uzak oturumunuzdan yeni VM'ye, ortamı temizlemek için aşağıdaki dosyaları kaldırın:
    
    * Kaynak`%windir%\System32`
      * Kaldırmak`FixAzureVM.cmd`
    * Kaynak`%windir%\System32\GroupPolicy\Machine\Scripts`
      * Kaldırmak`scripts.ini`
    * Kaynak`%windir%\System32\GroupPolicy`
      * kaldır `gpt.ini` `gpt.ini` (daha önce varsa ve dosyayı `gpt.ini.bak`yeniden `.bak` `gpt.ini`adlandırdıysanız, dosyayı yeniden adlandırın)

## <a name="next-steps"></a>Sonraki adımlar
Uzak Masaüstü'nü kullanarak hala bağlanamıyorsanız, [RDP sorun giderme kılavuzuna](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın. [Ayrıntılı RDP sorun giderme kılavuzu,](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) belirli adımlar yerine sorun giderme yöntemlerini inceliyor. Uygulamalı yardım için [bir Azure destek isteği](https://azure.microsoft.com/support/options/) de açabilirsiniz.
