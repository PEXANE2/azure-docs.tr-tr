---
title: Azure Agent olmadan yerel Windows parolasını sıfırlama | Microsoft Docs
description: Azure Konuk Aracısı yüklü olmadığında veya bir VM 'de çalışmadığı zaman yerel bir Windows Kullanıcı hesabının parolasını sıfırlama
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: gwallace
editor: ''
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: genli
ms.openlocfilehash: 5354ebc8c25125f86a0208382d176c84372cadc1
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68369871"
---
# <a name="reset-local-windows-password-for-azure-vm-offline"></a>Azure VM için yerel Windows parolasını çevrimdışına sıfırlama
Azure 'da bir sanal makinenin yerel Windows parolasını [Azure Portal veya](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure Konuk aracısının yüklü olduğu Azure PowerShell ' i kullanarak sıfırlayabilirsiniz. Bu yöntem, bir Azure VM için parola sıfırlamanın birincil yoludur. Azure Konuk Aracısı yanıt vermeyen veya özel bir görüntü yükledikten sonra yüklenemediğinden sorunlarla karşılaşırsanız, bir Windows parolasını el ile sıfırlayabilirsiniz. Bu makalede, kaynak işletim sistemi sanal diskini başka bir sanal makineye ekleyerek yerel hesap parolasının nasıl sıfırlanacağı açıklanır. Bu makalede açıklanan adımlar Windows etki alanı denetleyicileri için geçerlidir. 

> [!WARNING]
> Bu işlemi yalnızca son çare olarak kullanın. [Azure Portal veya önce Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak bir parolayı sıfırlamayı deneyin.

## <a name="overview-of-the-process"></a>İşleme genel bakış
Azure Konuk aracısına erişim olmadığında Azure 'da bir Windows sanal makinesi için yerel parola sıfırlama işlemi gerçekleştirmeye yönelik temel adımlar aşağıdaki gibidir:

1. Kaynak VM 'yi silin. Sanal diskler korunur.

2. Kaynak VM 'nin işletim sistemi diskini Azure aboneliğinizdeki aynı konumdaki başka bir sanal makineye ekleyin. Bu VM, sorun giderme VM 'si olarak adlandırılır.

3. Sorun giderme sanal makinesini kullanarak, kaynak VM 'nin işletim sistemi diskinde bazı yapılandırma dosyaları oluşturun.

4. VM 'nin işletim sistemi diskini sorun giderme VM 'sinden ayırın.

5. Özgün sanal diski kullanarak bir VM oluşturmak için Kaynak Yöneticisi şablonu kullanın.

6. Yeni VM önyüklendiğinde, oluşturduğunuz yapılandırma dosyaları gerekli kullanıcının parolasını güncelleştirir.

> [!NOTE]
> Aşağıdaki işlemleri otomatik hale getirebilirsiniz:
>
> - Sorun giderme sanal makinesi oluşturma
> - İşletim sistemi diski iliştirme
> - Özgün VM 'yi yeniden oluşturma
> 
> Bunu yapmak için [Azure VM kurtarma betikleri](https://github.com/Azure/azure-support-scripts/blob/master/VMRecovery/ResourceManager/README.md)' ni kullanın. Azure VM kurtarma betiklerini kullanmayı seçerseniz, "ayrıntılı adımlar" bölümünde aşağıdaki işlemi kullanabilirsiniz:
> 1. Etkilenen VM 'nin işletim sistemi diskini bir kurtarma sanal makinesine eklemek için betikleri kullanarak 1. ve 2. adımları atlayın.
> 2. Azaltıcı etkenleri uygulamak için 3 – 6 arasındaki adımları uygulayın.
> 3. VM 'yi yeniden derlemek için betikleri kullanarak 7 – 9 arasındaki adımları atlayın.
> 4. 10 ve 11. adımları izleyin.

## <a name="detailed-steps-for-resource-manager"></a>Kaynak Yöneticisi için ayrıntılı adımlar

> [!NOTE]
> Adımlar Windows etki alanı denetleyicileri için geçerlidir. Yalnızca tek başına sunucuda veya bir etki alanının üyesi olan bir sunucuda çalışmaktadır.

Aşağıdaki adımları denemeden önce [Azure Portal veya Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) kullanarak her zaman bir parolayı sıfırlamayı deneyin. Başlamadan önce VM 'nizin yedeğine sahip olduğunuzdan emin olun. 

1. Azure portal etkilenen sanal makineyi silin. VM 'nin silinmesi yalnızca meta verileri, Azure 'daki VM 'nin başvurusunu siler. Sanal diskler, VM silindiğinde tutulur:
   
   * Azure portal VM 'yi seçin, *Sil*' e tıklayın:
     
     ![Mevcut VM 'yi Sil](./media/reset-local-password-without-agent/delete-vm.png)

2. Kaynak VM 'nin işletim sistemi diskini sorun giderme VM 'sine bağlayın. Sorun giderme sanal makinesi, kaynak VM 'nin işletim sistemi diski ( `West US`gibi) ile aynı bölgede olmalıdır:
   
   1. Azure portal sorun giderme sanal makinesini seçin. *Disklere* | *Ekle*' ye tıklayın:
     
     ![Var olan bir diski ekle](./media/reset-local-password-without-agent/disks-attach-existing.png)
     
   2. *VHD dosyası* ' nı seçin ve ardından kaynak sanal makineyi içeren depolama hesabını seçin:
     
     ![Depolama hesabı seçin](./media/reset-local-password-without-agent/disks-select-storage-account.png)
     
   3. Kaynak kapsayıcısını seçin. Kaynak kapsayıcısı genellikle *VHD*'ler:
     
     ![Depolama kapsayıcısını seçin](./media/reset-local-password-without-agent/disks-select-container.png)
     
   4. İliştirilecek işletim sistemi VHD 'sini seçin. İşlemi gerçekleştirmek için *Seç* ' e tıklayın:
     
     ![Kaynak sanal disk seçin](./media/reset-local-password-without-agent/disks-select-source-vhd.png)

3. Uzak Masaüstü kullanarak sorun giderme sanal makinesine bağlanın ve kaynak VM 'nin işletim sistemi diskinin görünür olduğundan emin olun:
   
   1. Azure portal sorun giderme sanal makinesini seçip *Bağlan*' a tıklayın.

   2. İndirilen RDP dosyasını açın. Sorun giderme sanal makinesinin Kullanıcı adını ve parolasını girin.

   3. Dosya Gezgini 'nde, eklediğiniz veri diskini bulun. Kaynak VM 'nin VHD 'SI, sorun giderme sanal makinesine bağlı tek veri diskdeyse, bu, F: sürücüsü olmalıdır:
     
     ![Bağlı veri diskini görüntüleme](./media/reset-local-password-without-agent/troubleshooting-vm-file-explorer.png)

4. Kaynak sanal makinenin sürücüsünde oluşturun `gpt.ini` (GPT. ini varsa, GPT. ini. bak olarak yeniden adlandırın): `\Windows\System32\GroupPolicy`
   
   > [!WARNING]
   > Sorun giderme sanal makinesi için işletim sistemi sürücüsü olan C:\Windows ' da aşağıdaki dosyaları yanlışlıkla oluşturduğunuzdan emin olun. Kaynak VM 'niz için bir veri diski olarak bağlı olan işletim sistemi sürücüsünde aşağıdaki dosyaları oluşturun.
   
   * Aşağıdaki satırları `gpt.ini` oluşturduğunuz dosyaya ekleyin:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![GPT. ini oluştur](./media/reset-local-password-without-agent/create-gpt-ini.png)

5. `scripts.ini` İçinde`\Windows\System32\GroupPolicy\Machines\Scripts\`oluşturun. Gizli klasörlerin gösterildiğinden emin olun. Gerekirse, `Machine` veya `Scripts` klasörlerini oluşturun.
   
   * Aşağıdaki satırları `scripts.ini` oluşturduğunuz dosyaya ekleyin:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts. ini oluşturma](./media/reset-local-password-without-agent/create-scripts-ini.png)

6. `FixAzureVM.cmd` `<username>` Ve değerlerini`<newpassword>` kendi değerlerinizle değiştirerek aşağıdaki `\Windows\System32` içeriklerle oluşturun:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![Create FixAzureVM.cmd](./media/reset-local-password-without-agent/create-fixazure-cmd.png)
   
    Yeni parolayı tanımlarken VM 'niz için yapılandırılmış parola karmaşıklığı gereksinimlerini karşılamanız gerekir.

7. Azure portal, diski sorun giderme VM 'sinden ayırın:
   
   1. Azure portal sorun giderme sanal makinesini seçin, *diskler*' e tıklayın.

   2. 2\. adımda eklenen veri diskini seçin ve *Ayır*'a tıklayın:
     
     ![Diski kullanımdan çıkar](./media/reset-local-password-without-agent/detach-disk.png)

8. Bir VM oluşturmadan önce, URI 'yi kaynak işletim sistemi diskine alın:
   
   1. Azure portal depolama hesabını seçin, Bloblar ' a tıklayın.

   2. Kapsayıcıyı seçin. Kaynak kapsayıcısı genellikle *VHD*'ler:
     
     ![Depolama hesabı blobu seçin](./media/reset-local-password-without-agent/select-storage-details.png)
     
   3. Kaynak VM işletim sistemi VHD 'nizi seçin ve *URL* adının yanındaki *Kopyala* düğmesine tıklayın:
     
     ![Disk URI 'sini Kopyala](./media/reset-local-password-without-agent/copy-source-vhd-uri.png)

9. Kaynak VM 'nin işletim sistemi diskinden bir VM oluşturun:
   
   1. Özelleştirilmiş bir VHD 'den VM oluşturmak için [bu Azure Resource Manager şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet) kullanın. Azure Portal, sizin için doldurulan şablonlu ayrıntılarla birlikte açmak için düğmeyetıklayın.`Deploy to Azure`

   2. VM 'nin tüm önceki ayarlarını sürdürmek istiyorsanız, var olan VNet, alt ağ, ağ bağdaştırıcısı veya genel IP 'nizi sağlamak için *Şablonu Düzenle* ' yi seçin.

   3. `OSDISKVHDURI` Parametre metin kutusunda, önceki adımda kaynak VHD alma 'nizin URI 'sini yapıştırın:
     
     ![Şablondan VM oluşturma](./media/reset-local-password-without-agent/create-new-vm-from-template.png)

10. Yeni VM çalışmaya başladıktan sonra, `FixAzureVM.cmd` komut dosyasında belirttiğiniz yeni parolayla uzak masaüstü 'nü kullanarak VM 'ye bağlanın.

11. Uzak oturumınızdan yeni VM 'ye kadar, ortamı temizlemek için aşağıdaki dosyaları kaldırın:
    
    * %Windir%\System32 adresinden
      * remove FixAzureVM.cmd
    * %Windir%\system32\groupilkemachıne\ KomutDosyaları
      * Scripts. ini dosyasını Kaldır
    * %Windir%\System32\GroupPolicy öğesinden
      * GPT. ini dosyasını kaldırın (GPT. ini daha önce vardı ve GPT. ini. bak olarak yeniden adlandırdıysanız,. bak dosyasını tekrar GPT. ini olarak yeniden adlandırın)

## <a name="detailed-steps-for-classic-vm"></a>Klasik VM için ayrıntılı adımlar

> [!NOTE]
> Adımlar Windows etki alanı denetleyicileri için geçerlidir. Yalnızca tek başına sunucuda veya bir etki alanının üyesi olan bir sunucuda çalışmaktadır.

Aşağıdaki adımları denemeden önce [Azure Portal veya Azure PowerShell](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/reset-rdp) kullanarak her zaman bir parolayı sıfırlamayı deneyin. Başlamadan önce VM 'nizin yedeğine sahip olduğunuzdan emin olun. 

1. Azure portal etkilenen sanal makineyi silin. VM 'nin silinmesi yalnızca meta verileri, Azure 'daki VM 'nin başvurusunu siler. Sanal diskler, VM silindiğinde tutulur:
   
   * Azure portal VM 'yi seçip *Sil*' e tıklayın:
     
     ![Mevcut VM 'yi Sil](./media/reset-local-password-without-agent/delete-vm-classic.png)

2. Kaynak VM 'nin işletim sistemi diskini sorun giderme VM 'sine bağlayın. Sorun giderme sanal makinesi, kaynak VM 'nin işletim sistemi diski ( `West US`gibi) ile aynı bölgede olmalıdır:
   
   1. Azure portal sorun giderme sanal makinesini seçin. *Disklere* | *Ekle*' ye tıklayın:
     
      ![Var olan bir diski ekle](./media/reset-local-password-without-agent/disks-attach-existing-classic.png)
     
   2. *VHD dosyası* ' nı seçin ve ardından kaynak sanal makineyi içeren depolama hesabını seçin:
     
      ![Depolama hesabı seçin](./media/reset-local-password-without-agent/disks-select-storage-account-classic.png)
     
   3. *Klasik depolama hesaplarını göster*işaretli kutuyu işaretleyin ve ardından kaynak kapsayıcısını seçin. Kaynak kapsayıcısı genellikle *VHD*'ler:
     
      ![Depolama kapsayıcısını seçin](./media/reset-local-password-without-agent/disks-select-container-classic.png)

      ![Depolama kapsayıcısını seçin](./media/reset-local-password-without-agent/disks-select-container-vhds-classic.png)
     
   4. İliştirilecek işletim sistemi VHD 'sini seçin. İşlemi gerçekleştirmek için *Seç* ' e tıklayın:
     
      ![Kaynak sanal disk seçin](./media/reset-local-password-without-agent/disks-select-source-vhd-classic.png)

   5. Diski eklemek için Tamam 'a tıklayın

      ![Var olan bir diski ekle](./media/reset-local-password-without-agent/disks-attach-okay-classic.png)

3. Uzak Masaüstü kullanarak sorun giderme sanal makinesine bağlanın ve kaynak VM 'nin işletim sistemi diskinin görünür olduğundan emin olun:

   1. Azure portal sorun giderme sanal makinesini seçip *Bağlan*' a tıklayın.

   2. İndirilen RDP dosyasını açın. Sorun giderme sanal makinesinin Kullanıcı adını ve parolasını girin.

   3. Dosya Gezgini 'nde, eklediğiniz veri diskini bulun. Kaynak VM 'nin VHD 'SI, sorun giderme sanal makinesine bağlı tek veri diskdeyse, bu, F: sürücüsü olmalıdır:
     
      ![Bağlı veri diskini görüntüleme](./media/reset-local-password-without-agent/troubleshooting-vm-file-explorer-classic.png)

4. Kaynak sanal makinenin sürücüsünde oluşturun `gpt.ini` `gpt.ini.bak`(varsa, yeniden adlandırın): `gpt.ini` `\Windows\System32\GroupPolicy`
   
   > [!WARNING]
   > Sorun giderme sanal makinesi için işletim sistemi sürücüsünde aşağıdaki dosyaları `C:\Windows`yanlışlıkla oluşturmadığından emin olun. Kaynak VM 'niz için bir veri diski olarak bağlı olan işletim sistemi sürücüsünde aşağıdaki dosyaları oluşturun.
   
   * Aşağıdaki satırları `gpt.ini` oluşturduğunuz dosyaya ekleyin:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![GPT. ini oluştur](./media/reset-local-password-without-agent/create-gpt-ini-classic.png)

5. `scripts.ini` İçinde`\Windows\System32\GroupPolicy\Machines\Scripts\`oluşturun. Gizli klasörlerin gösterildiğinden emin olun. Gerekirse, `Machine` veya `Scripts` klasörlerini oluşturun.
   
   * Aşağıdaki satırları `scripts.ini` oluşturduğunuz dosyaya ekleyin:

     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts. ini oluşturma](./media/reset-local-password-without-agent/create-scripts-ini-classic.png)

6. `FixAzureVM.cmd` `<username>` Ve değerlerini`<newpassword>` kendi değerlerinizle değiştirerek aşağıdaki `\Windows\System32` içeriklerle oluşturun:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add
    ```

    ![Create FixAzureVM.cmd](./media/reset-local-password-without-agent/create-fixazure-cmd-classic.png)
   
    Yeni parolayı tanımlarken VM 'niz için yapılandırılmış parola karmaşıklığı gereksinimlerini karşılamanız gerekir.

7. Azure portal, diski sorun giderme VM 'sinden ayırın:
   
   1. Azure portal sorun giderme sanal makinesini seçin, *diskler*' e tıklayın.
   
   2. 2\. adımda eklenen veri diskini seçin, *Ayır*' a ve ardından *Tamam*' a tıklayın.

     ![Diski kullanımdan çıkar](./media/reset-local-password-without-agent/data-disks-classic.png)
     
     ![Diski kullanımdan çıkar](./media/reset-local-password-without-agent/detach-disk-classic.png)

8. Kaynak VM 'nin işletim sistemi diskinden bir VM oluşturun:
   
     ![Şablondan VM oluşturma](./media/reset-local-password-without-agent/create-new-vm-from-template-classic.png)

     ![Şablondan VM oluşturma](./media/reset-local-password-without-agent/choose-subscription-classic.png)

     ![Şablondan VM oluşturma](./media/reset-local-password-without-agent/create-vm-classic.png)

## <a name="complete-the-create-virtual-machine-experience"></a>Sanal makine oluşturma deneyimini doldurun

1. Yeni VM çalışmaya başladıktan sonra, `FixAzureVM.cmd` komut dosyasında belirttiğiniz yeni parolayla uzak masaüstü 'nü kullanarak VM 'ye bağlanın.

2. Uzak oturumınızdan yeni VM 'ye kadar, ortamı temizlemek için aşağıdaki dosyaları kaldırın:
    
    * Kaynak`%windir%\System32`
      * temizlenmesine`FixAzureVM.cmd`
    * Kaynak`%windir%\System32\GroupPolicy\Machine\Scripts`
      * temizlenmesine`scripts.ini`
    * Kaynak`%windir%\System32\GroupPolicy`
      * kaldırın `gpt.ini` (daha `gpt.ini` önce varsa `gpt.ini.bak`ve yeniden adlandırdıysanız, `.bak` dosyayı `gpt.ini`olarak yeniden adlandırın)

## <a name="next-steps"></a>Sonraki adımlar
Hala uzak masaüstü 'Nü kullanarak bağlanamıyorsanız, [RDP sorun giderme kılavuzu](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)' na bakın. [AYRıNTıLı RDP sorun giderme kılavuzu](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , belirli adımlar yerine sorun giderme yöntemlerine bakar. Ayrıca, uygulamalı yardım için [bir Azure destek isteği açabilirsiniz](https://azure.microsoft.com/support/options/) .
