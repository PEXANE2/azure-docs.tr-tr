---
title: Azure Agent olmadan yerel Windows parolasını sıfırlama | Microsoft Docs
description: Azure Konuk Aracısı yüklü olmadığında veya bir VM 'de çalışmadığı zaman yerel bir Windows Kullanıcı hesabının parolasını sıfırlama
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
ms.openlocfilehash: 4cec8f77cacc5d3492dd6a5f8a8baa060f910763
ms.sourcegitcommit: b4f303f59bb04e3bae0739761a0eb7e974745bb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91650605"
---
# <a name="reset-local-windows-password-for-azure-vm-offline"></a>Çevrimdışı Azure VM için yerel Windows parolasını sıfırlama
Azure 'da bir sanal makinenin yerel Windows parolasını [Azure Portal veya](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure Konuk aracısının yüklü olduğu Azure PowerShell ' i kullanarak sıfırlayabilirsiniz. Bu yöntem, bir Azure VM için parola sıfırlamanın birincil yoludur. Azure Konuk Aracısı yanıt vermeyen veya özel bir görüntü yükledikten sonra yüklenemediğinden sorunlarla karşılaşırsanız, bir Windows parolasını el ile sıfırlayabilirsiniz. Bu makalede, kaynak işletim sistemi sanal diskini başka bir sanal makineye ekleyerek yerel hesap parolasının nasıl sıfırlanacağı açıklanır. Bu makalede açıklanan adımlar Windows etki alanı denetleyicileri için geçerlidir. 

> [!WARNING]
> Bu işlemi yalnızca son çare olarak kullanın. [Azure Portal veya önce Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak bir parolayı sıfırlamayı deneyin.

## <a name="overview-of-the-process"></a>İşleme genel bakış
Azure Konuk aracısına erişim olmadığında Azure 'da bir Windows sanal makinesi için yerel parola sıfırlama işlemi gerçekleştirmeye yönelik temel adımlar aşağıdaki gibidir:

1. Etkilenen VM 'yi durdurun.
1. VM 'nin işletim sistemi diski için bir anlık görüntü oluşturun.
1. Anlık görüntüden işletim sistemi diskinin bir kopyasını oluşturun.
1. Kopyalanmış işletim sistemi diskini başka bir Windows sanal makinesine ekleyip bağlayın ve ardından diskte bazı yapılandırma dosyaları oluşturun. Dosyalar, parolayı sıfırlamanıza yardımcı olur.
1. Kopyalanmış işletim sistemi diskini sorun giderme VM 'sinden çıkarın ve ayırın.
1. Etkilenen VM 'nin işletim sistemi diskini değiştirin.

## <a name="detailed-steps-for-the-vm-with-resource-manager-deployment"></a>Kaynak Yöneticisi dağıtımı ile VM için ayrıntılı adımlar

> [!NOTE]
> Adımlar Windows etki alanı denetleyicileri için geçerlidir. Yalnızca tek başına sunucuda veya bir etki alanının üyesi olan bir sunucuda çalışmaktadır.

Aşağıdaki adımları denemeden önce [Azure Portal veya Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak her zaman bir parolayı sıfırlamayı deneyin. Başlamadan önce VM 'nizin yedeğine sahip olduğunuzdan emin olun.

1. Etkilenen VM 'nin işletim sistemi diski için bir anlık görüntü alın, anlık görüntüden bir disk oluşturun ve ardından diski bir sorun giderme VM 'sine bağlayın. Daha fazla bilgi için, [Azure Portal kullanarak işletim sistemi diskini bir kurtarma sanal makinesine ekleyerek WINDOWS VM sorunlarını giderme](troubleshoot-recovery-disks-portal-windows.md)bölümüne bakın.
2. Uzak Masaüstü kullanarak sorun giderme sanal makinesine bağlanın.
3. `gpt.ini` `\Windows\System32\GroupPolicy` Kaynak sanal makinenin sürücüsünde oluşturun (gpt.ini varsa, gpt.ini. bak olarak yeniden adlandırın):
   
   > [!WARNING]
   > Sorun giderme sanal makinesi için işletim sistemi sürücüsü olan C:\Windows ' da aşağıdaki dosyaları yanlışlıkla oluşturduğunuzdan emin olun. Kaynak VM 'niz için bir veri diski olarak bağlı olan işletim sistemi sürücüsünde aşağıdaki dosyaları oluşturun.
   
   * Aşağıdaki satırları `gpt.ini` oluşturduğunuz dosyaya ekleyin:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     :::image type="content" source="./media/reset-local-password-without-agent/create-gpt-ini.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

4. `scripts.ini`İçinde oluşturun `\Windows\System32\GroupPolicy\Machine\Scripts\` . Gizli klasörlerin gösterildiğinden emin olun. Gerekirse, `Machine` veya `Scripts` klasörlerini oluşturun. 
   
   * Aşağıdaki satırları `scripts.ini` oluşturduğunuz dosyaya ekleyin:
     
     ```
     [Startup]
     0CmdLine=FixAzureVM.cmd
     0Parameters=
     ```
     
     :::image type="content" source="./media/reset-local-password-without-agent/create-scripts-ini-1.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü." <username> /add
    ```

    :::image type="content" source="./media/reset-local-password-without-agent/create-fixazure-cmd-1.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::
   
    Yeni parolayı tanımlarken VM 'niz için yapılandırılmış parola karmaşıklığı gereksinimlerini karşılamanız gerekir.

6. Azure portal, diski sorun giderme VM 'sinden ayırın.

7. [ETKILENEN VM için işletim sistemi diskini değiştirin](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm).

8. Yeni VM çalışmaya başladıktan sonra, komut dosyasında belirttiğiniz yeni parolayla uzak masaüstü 'Nü kullanarak VM 'ye bağlanın `FixAzureVM.cmd` .

9. Uzak oturumınızdan yeni VM 'ye kadar, ortamı temizlemek için aşağıdaki dosyaları kaldırın:
    
    * %Windir%\System32\GroupPolicy\Machine\Scripts\Startup konumundan
      * FixAzureVM. cmd dosyasını Kaldır
    * %Windir%\system32\groupilkemachıne\ KomutDosyaları
      * scripts.ini kaldır
    * %Windir%\System32\GroupPolicy öğesinden
      * gpt.ini kaldırın (gpt.ini daha önce varsa ve gpt.ini. bak olarak yeniden adlandırdıysanız,. bak dosyasını tekrar gpt.ini olarak yeniden adlandırın)

## <a name="detailed-steps-for-classic-vm"></a>Klasik VM için ayrıntılı adımlar

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

> [!NOTE]
> Adımlar Windows etki alanı denetleyicileri için geçerlidir. Yalnızca tek başına sunucuda veya bir etki alanının üyesi olan bir sunucuda çalışmaktadır.

Aşağıdaki adımları denemeden önce [Azure Portal veya Azure PowerShell](/previous-versions/azure/virtual-machines/windows/classic/reset-rdp) kullanarak her zaman bir parolayı sıfırlamayı deneyin. Başlamadan önce VM 'nizin yedeğine sahip olduğunuzdan emin olun. 

1. Azure portal etkilenen sanal makineyi silin. VM 'nin silinmesi yalnızca meta verileri, Azure 'daki VM 'nin başvurusunu siler. Sanal diskler, VM silindiğinde tutulur:
   
   * Azure portal VM 'yi seçip *Sil*' e tıklayın:
     
     :::image type="content" source="./media/reset-local-password-without-agent/delete-vm-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

2. Kaynak VM 'nin işletim sistemi diskini sorun giderme VM 'sine bağlayın. Sorun giderme sanal makinesi, kaynak VM 'nin işletim sistemi diski (gibi) ile aynı bölgede olmalıdır `West US` :
   
   1. Azure portal sorun giderme sanal makinesini seçin. *Disklere*  |  *Ekle*' ye tıklayın:
     
      :::image type="content" source="./media/reset-local-password-without-agent/disks-attach-existing-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::
     
   2. *VHD dosyası* ' nı seçin ve ardından kaynak sanal makineyi içeren depolama hesabını seçin:
     
      :::image type="content" source="./media/reset-local-password-without-agent/disks-select-storage-account-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::
     
   3. *Klasik depolama hesaplarını göster*işaretli kutuyu işaretleyin ve ardından kaynak kapsayıcısını seçin. Kaynak kapsayıcısı genellikle *VHD*'ler:
     
      :::image type="content" source="./media/reset-local-password-without-agent/disks-select-container-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

      :::image type="content" source="./media/reset-local-password-without-agent/disks-select-container-vhds-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::
     
   4. İliştirilecek işletim sistemi VHD 'sini seçin. İşlemi gerçekleştirmek için *Seç* ' e tıklayın:
     
      :::image type="content" source="./media/reset-local-password-without-agent/disks-select-source-vhd-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

   5. Diski eklemek için Tamam 'a tıklayın

      :::image type="content" source="./media/reset-local-password-without-agent/disks-attach-okay-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

3. Uzak Masaüstü kullanarak sorun giderme sanal makinesine bağlanın ve kaynak VM 'nin işletim sistemi diskinin görünür olduğundan emin olun:

   1. Azure portal sorun giderme sanal makinesini seçip *Bağlan*' a tıklayın.

   2. İndirilen RDP dosyasını açın. Sorun giderme sanal makinesinin Kullanıcı adını ve parolasını girin.

   3. Dosya Gezgini 'nde, eklediğiniz veri diskini bulun. Kaynak VM 'nin VHD 'SI, sorun giderme sanal makinesine bağlı tek veri diskdeyse, bu, F: sürücüsü olmalıdır:
     
      :::image type="content" source="./media/reset-local-password-without-agent/troubleshooting-vm-file-explorer-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

4. `gpt.ini` `\Windows\System32\GroupPolicy` Kaynak sanal makinenin sürücüsünde oluşturun (varsa `gpt.ini` , yeniden adlandırın `gpt.ini.bak` ):
   
   > [!WARNING]
   > `C:\Windows`Sorun giderme sanal makinesi için işletim sistemi sürücüsünde aşağıdaki dosyaları yanlışlıkla oluşturmadığından emin olun. Kaynak VM 'niz için bir veri diski olarak bağlı olan işletim sistemi sürücüsünde aşağıdaki dosyaları oluşturun.
   
   * Aşağıdaki satırları `gpt.ini` oluşturduğunuz dosyaya ekleyin:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     :::image type="content" source="./media/reset-local-password-without-agent/create-gpt-ini-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

5. `scripts.ini`İçinde oluşturun `\Windows\System32\GroupPolicy\Machine\Scripts\` . Gizli klasörlerin gösterildiğinden emin olun. Gerekirse, `Machine` veya `Scripts` klasörlerini oluşturun.
   
   * Aşağıdaki satırları `scripts.ini` oluşturduğunuz dosyaya ekleyin:

     ```
     [Startup]
     0CmdLine=FixAzureVM.cmd
     0Parameters=
     ```
     
     :::image type="content" source="./media/reset-local-password-without-agent/create-scripts-ini-classic-1.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü." <username> /add
    ```

    :::image type="content" source="./media/reset-local-password-without-agent/create-fixazure-cmd-1.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::
   
    Yeni parolayı tanımlarken VM 'niz için yapılandırılmış parola karmaşıklığı gereksinimlerini karşılamanız gerekir.

7. Azure portal, diski sorun giderme VM 'sinden ayırın:
   
   1. Azure portal sorun giderme sanal makinesini seçin, *diskler*' e tıklayın.
   
   2. 2. adımda eklenen veri diskini seçin, **Ayır**' a ve ardından **Tamam**' a tıklayın.

     :::image type="content" source="./media/reset-local-password-without-agent/data-disks-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::
     
     :::image type="content" source="./media/reset-local-password-without-agent/detach-disk-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

8. Kaynak VM 'nin işletim sistemi diskinden bir VM oluşturun:
   
     :::image type="content" source="./media/reset-local-password-without-agent/create-new-vm-from-template-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

     :::image type="content" source="./media/reset-local-password-without-agent/choose-subscription-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

     :::image type="content" source="./media/reset-local-password-without-agent/create-vm-classic.png" alt-text="gpt.ini dosyasına yapılan güncelleştirmeleri gösteren ekran görüntüsü.":::

## <a name="complete-the-create-virtual-machine-experience"></a>Sanal makine oluşturma deneyimini doldurun

1. Yeni VM çalışmaya başladıktan sonra, komut dosyasında belirttiğiniz yeni parolayla uzak masaüstü 'Nü kullanarak VM 'ye bağlanın `FixAzureVM.cmd` .

2. Uzak oturumınızdan yeni VM 'ye kadar, ortamı temizlemek için aşağıdaki dosyaları kaldırın:
    
    * Kaynak `%windir%\System32\GroupPolicy\Machine\Scripts\Startup\`
      * temizlenmesine `FixAzureVM.cmd`
    * Kaynak `%windir%\System32\GroupPolicy\Machine\Scripts`
      * temizlenmesine `scripts.ini`
    * Kaynak `%windir%\System32\GroupPolicy`
      * kaldırın `gpt.ini` (daha `gpt.ini` önce varsa ve yeniden adlandırdıysanız `gpt.ini.bak` , dosyayı olarak yeniden adlandırın `.bak` `gpt.ini` )

## <a name="next-steps"></a>Sonraki adımlar
Hala uzak masaüstü 'Nü kullanarak bağlanamıyorsanız, [RDP sorun giderme kılavuzu](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)' na bakın. [AYRıNTıLı RDP sorun giderme kılavuzu](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , belirli adımlar yerine sorun giderme yöntemlerine bakar. Ayrıca, uygulamalı yardım için [bir Azure destek isteği açabilirsiniz](https://azure.microsoft.com/support/options/) .
