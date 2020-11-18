---
title: Azure portal bir Windows sanal makinesi sorunlarını giderme | Microsoft Docs
description: Azure portal aracılığıyla işletim sistemi diskini bir kurtarma VM 'sine bağlayarak Azure 'da Windows sanal makine sorunlarını nasıl giderebileceğinizi öğrenin.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/19/2018
ms.author: genli
ms.openlocfilehash: 04adf3903cd925f4507e94347251c762a576ff93
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94735237"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-through-the-azure-portal"></a>Azure portal aracılığıyla işletim sistemi diskini bir kurtarma VM 'sine ekleyerek bir Windows sanal makinesi sorunlarını giderme
Azure 'daki Windows sanal makineniz (VM) bir başlangıç veya disk hatasıyla karşılaşırsa, sanal sabit diskte (VHD) sorun giderme adımları gerçekleştirmeniz gerekebilir. Ortak bir örnek, VM 'nin başarıyla başlamasını önleyen, başarısız bir uygulama güncelleştirmesidir. Bu makalede, tüm hataları onarmak için sanal sabit diskinizi başka bir Windows sanal makineye bağlamak üzere Azure portal kullanımı ve ardından özgün VM 'nizi yeniden oluşturmanız ayrıntılı olarak açıklanır. 

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen VM 'yi durdurun.
1. VM 'nin işletim sistemi diski için bir anlık görüntü oluşturun.
1. Anlık görüntüden bir sanal sabit disk oluşturun.
1. Sorun giderme amacıyla sanal sabit diski başka bir Windows sanal makinesine ekleyin ve bağlayın.
1. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Özgün sanal sabit diskte bulunan sorunları gidermeye yönelik dosyaları düzenleyin veya herhangi bir araç çalıştırın.
1. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
1. VM için işletim sistemi diskini değiştirin.

> [!NOTE]
> Bu makale, yönetilmeyen disklere sahip VM 'Ler için geçerlidir.

## <a name="take-a-snapshot-of-the-os-disk"></a>İşletim sistemi diskinin anlık görüntüsünü alın
Anlık görüntü, bir sanal sabit diskin tam ve Salt okunabilir bir kopyasıdır. Sürmekte olan tüm işlemlerin seçimini kaldırmak için bir anlık görüntüyü almadan önce VM 'yi düzgün bir şekilde kapatmanız önerilir. Bir işletim sistemi diskinin anlık görüntüsünü almak için şu adımları izleyin:

1. [Azure portalına](https://portal.azure.com) gidin. Kenar çubuğundan **sanal makineler** ' i seçin ve ardından sorunu olan VM 'yi seçin.
1. Sol bölmede, **diskler**' i seçin ve ardından işletim sistemi diskinin adını seçin.
    ![Disk ayarlarındaki işletim sistemi diskinin adını gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/select-osdisk.png)
1. İşletim sistemi diskinin **genel bakış** sayfasında **anlık görüntü oluştur**' u seçin.
1. İşletim sistemi diskiyle aynı konumda bir anlık görüntü oluşturun.

## <a name="create-a-disk-from-the-snapshot"></a>Anlık görüntüden disk oluşturma
Anlık görüntüden bir disk oluşturmak için aşağıdaki adımları izleyin:

1. Azure portal **Cloud Shell** seçin.

    ![Azure Cloud Shell açma düğmesini gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/cloud-shell.png)
1. Anlık görüntüden yönetilen disk oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın. Örnek adları uygun adlarla değiştirin.

    ```powershell
    #Provide the name of your resource group.
    $resourceGroupName ='myResourceGroup'
    
    #Provide the name of the snapshot that will be used to create managed disks.
    $snapshotName = 'mySnapshot' 
    
    #Provide the name of the managed disk.
    $diskName = 'newOSDisk'
    
    #Provide the size of the disks in gigabytes. It should be greater than the VHD file size. In this example, the size of the snapshot is 127 GB. So we set the disk size to 128 GB.
    $diskSize = '128'
    
    #Provide the storage type for the managed disk: Premium_LRS or Standard_LRS.
    $storageType = 'Standard_LRS'
    
    #Provide the Azure region (for example, westus) where the managed disks will be located.
    #This location should be the same as the snapshot location.
    #Get all the Azure locations by using this command:
    #Get-AzLocation
    $location = 'westus'
    
    $snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
     
    $diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
     
    New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
    ```
3. Komutlar başarıyla çalışıyorsa, belirttiğiniz kaynak grubunda Yeni diski görürsünüz.

## <a name="attach-the-disk-to-another-vm"></a>Diski başka bir VM 'ye iliştirme
Sonraki birkaç adımda, sorun giderme amacıyla başka bir VM kullanırsınız. Diski sorun giderme VM 'sine iliştirdikten sonra, diskin içeriğine gözatıp düzenleyebilirsiniz. Bu işlem, yapılandırma hatalarını düzeltmenize veya ek uygulama veya sistem günlük dosyalarını incelemenizi sağlar. Diski başka bir VM 'ye eklemek için şu adımları izleyin:

1. Portaldan kaynak grubunuzu seçin ve ardından sorun giderme sanal makinesini seçin. **Disk** seçin  >  **düzenleme**  >  **veri diski** seçin.

    ![Portalda var olan bir diski ekleme seçimlerini gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. **Veri diskleri** listesinde, tanımladığınız VM 'nin işletim sistemi diskini seçin. İşletim sistemi diskini görmüyorsanız, sorun giderme sanal makinesinin ve işletim sistemi diskinin aynı bölgede (konum) bulunduğundan emin olun. 
3. Değişiklikleri uygulamak için **Kaydet** ' i seçin.

## <a name="mount-the-attached-data-disk-to-the-vm"></a>Bağlı veri diskini VM 'ye bağlama

1. Sorun giderme VM 'sine bir Uzak Masaüstü bağlantısı açın. 
2. Sorun giderme sanal makinesinde **Sunucu Yöneticisi** açın ve ardından **dosya ve depolama hizmetleri**' ni seçin. 

    ![Sunucu Yöneticisi içinde dosya ve depolama hizmetleri seçmeyi gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. Veri diski otomatik olarak algılanır ve eklenir. Bağlı disklerin bir listesini görmek için **diskler**' i seçin. Sürücü harfi dahil olmak üzere birim bilgilerini görüntülemek için veri diskinizi seçebilirsiniz. Aşağıdaki örnek, ve **F** sürücüsünü kullanarak bağlı veri diskini gösterir.

    ![Sunucu Yöneticisi eklenen bir disk ve birim bilgilerini gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)

## <a name="fix-problems-on-the-original-virtual-hard-disk"></a>Özgün sanal sabit diskteki sorunları çözme
Var olan sanal sabit disk takılı olduğunda, artık gereken tüm bakım ve sorun giderme adımlarını gerçekleştirebilirsiniz. Tüm hataları çözdükten sonra, aşağıdaki adımlarla devam edin.

## <a name="unmount-and-detach-the-original-virtual-hard-disk"></a>Orijinal sanal sabit diski çıkarma
Var olan sanal sabit diski sorun giderme sanal makinenizin dışında ayırın. Sanal sabit diski, sorun giderme sanal makinesine bağlayan kira serbest bırakılana kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. Uzak Masaüstü oturumundan sanal makinenize **Sunucu Yöneticisi** açın ve ardından **dosya ve depolama hizmetleri**' ni seçin.

    ![Sunucu Yöneticisi 'de dosya ve depolama hizmetleri seçmeyi gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. **Diskler**' i seçin, veri diskinize sağ tıklayın ve ardından **Çevrimdışına Al**' ı seçin.

    ![Sunucu Yöneticisi 'de veri diskinin çevrimdışı olarak ayarlanmasını gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Sanal sabit diski VM 'den ayırın. Azure portal VM 'nizi seçin ve ardından **diskler**' i seçin. 
4. **Düzenle**' yi seçin, eklediğiniz işletim sistemi diskini seçin ve **Sil**' i seçin.

    ![Var olan bir sanal sabit diski ayırmak için seçimleri gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Devam etmeden önce, sanal makinede veri diski başarıyla silinene kadar bekleyin.

## <a name="swap-the-os-disk-for-the-vm"></a>VM için işletim sistemi diskini değiştirme

Azure portal artık VM 'nin işletim sistemi diskini değiştirmeyi destekler. Bunu yapmak için şu adımları uygulayın:

1. [Azure portalına](https://portal.azure.com) gidin. Kenar çubuğundan **sanal makineler** ' i seçin ve ardından sorunu olan VM 'yi seçin.
1. Sol bölmede, **diskler**' i seçin ve ardından **Işletim sistemi diski Değiştir**' i seçin.
   
    ![Azure portal bir işletim sistemi diskini değiştirme düğmesini gösteren ekran görüntüsü.](./media/troubleshoot-recovery-disks-portal-windows/swap-os-ui.png)

1. Onarılmakta olduğunuz yeni diski seçin ve ardından değişikliği onaylamak için VM 'nin adını girin. Diski listede görmüyorsanız, diski sorun giderme VM 'sinden ayırdıktan sonra 10 ila 15 dakika bekleyin. Ayrıca, diskin VM ile aynı konumda olduğundan emin olun.
1. **Tamam**’ı seçin.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure VM Ile uzak masaüstü bağlantılarında sorun giderme](troubleshoot-rdp-connection.md). VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [WINDOWS VM 'de uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).

Azure Resource Manager kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](../../azure-resource-manager/management/overview.md).


