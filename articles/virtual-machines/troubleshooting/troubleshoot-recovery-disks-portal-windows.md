---
title: Azure portalında Windows sorun giderme VM'i kullanma | Microsoft Dokümanlar
description: Azure portalını kullanarak işletim sistemi diskini kurtarma VM'ine bağlayarak Azure'daki Windows sanal makine sorunlarını nasıl gidereceklerini öğrenin
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
ms.openlocfilehash: e76fc2da8da2325a8bb0cda47c4405c9eb03c8f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250003"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Azure portalını kullanarak işletim sistemi diskini kurtarma VM'ine takarak windows VM sorun giderme
Azure'daki Windows sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın bir örnek, VM'nin başarılı bir şekilde önyükleme yapabilmesini engelleyen başarısız bir uygulama güncelleştirmesi olacaktır. Bu makalede, sanal sabit diskinizi herhangi bir hatayı gidermek ve orijinal VM'nizi yeniden oluşturmak için sanal sabit diskinizi başka bir Windows VM'ye bağlamak için Azure portalının nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır. 

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen VM'yi durdurun.
1. VM işletim sistemi diski için anlık görüntü oluşturun.
1. Anlık görüntüden sanal bir sabit disk oluşturun.
1. Sorun giderme amacıyla sanal sabit diski başka bir Windows VM'ye takın ve monte edin.
1. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Özgün sanal sabit diskteki sorunları gidermek için dosyaları edin veya herhangi bir araç çalıştırın.
1. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
1. Os diskini VM ile değiştirin.

> [!NOTE]
> Bu makale, yönetilmeyen diske sahip VM için geçerli değildir.

## <a name="take-a-snapshot-of-the-os-disk"></a>Os Diskinin anlık görüntüsünü alın
Anlık görüntü, sanal bir sabit diskin (VHD) tam, salt okunur kopyasıdır. Devam eden işlemleri temizlemek için anlık görüntü almadan önce VM'yi temiz bir şekilde kapatmanızı öneririz. Bir işletim sistemi diskinin anlık görüntüsünü almak için aşağıdaki adımları izleyin:

1. Azure [portalına](https://portal.azure.com)gidin. Kenar çubuğundan **Sanal makineleri** seçin ve ardından sorun olan VM'yi seçin.
1. Sol bölmede **Diskler'i**seçin ve ardından işletim sistemi diskinin adını seçin.
    ![Os diskinin adı hakkında resim](./media/troubleshoot-recovery-disks-portal-windows/select-osdisk.png)
1. İşletim sistemi diskinin **Genel Bakış** sayfasında ve ardından **anlık görüntü oluştur'u**seçin.
1. İşletim sistemi diski ile aynı konumda anlık görüntü oluşturun.

## <a name="create-a-disk-from-the-snapshot"></a>Anlık görüntüden disk oluşturma
Anlık görüntüden bir disk oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalından **Cloud Shell'i** seçin.

    ![Open Cloud Shell hakkında resim](./media/troubleshoot-recovery-disks-portal-windows/cloud-shell.png)
1. Anlık görüntüden yönetilen bir disk oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın. Bu örnek adları uygun adlarla değiştirmelisiniz.

    ```powershell
    #Provide the name of your resource group
    $resourceGroupName ='myResourceGroup'
    
    #Provide the name of the snapshot that will be used to create Managed Disks
    $snapshotName = 'mySnapshot' 
    
    #Provide the name of theManaged Disk
    $diskName = 'newOSDisk'
    
    #Provide the size of the disks in GB. It should be greater than the VHD file size. In this sample, the size of the snapshot is 127 GB. So we set the disk size to 128 GB.
    $diskSize = '128'
    
    #Provide the storage type for Managed Disk.  Premium_LRS or Standard_LRS.
    $storageType = 'Standard_LRS'
    
    #Provide the Azure region (e.g. westus) where Managed Disks will be located.
    #This location should be same as the snapshot location
    #Get all the Azure location using command below:
    #Get-AzLocation
    $location = 'westus'
    
    $snapshot = Get-AzSnapshot -ResourceGroupName $resourceGroupName -SnapshotName $snapshotName 
     
    $diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Copy -SourceResourceId $snapshot.Id
     
    New-AzDisk -Disk $diskConfig -ResourceGroupName $resourceGroupName -DiskName $diskName
    ```
3. Komutlar başarıyla çalıştırılırsa, sağladığınız kaynak grubunda yeni diski görürsünüz.

## <a name="attach-the-disk-to-another-vm"></a>Diski başka bir VM'ye takın
Sonraki birkaç adımda, sorun giderme amacıyla başka bir VM kullanırsınız. Diski sorun giderme VM'ine taktıktan sonra diskin içeriğine göz atabilir ve edinebilirsiniz. Bu işlem, yapılandırma hatalarını düzeltmenize veya ek uygulama veya sistem günlüğü dosyalarını gözden geçirmenize olanak tanır. Diski başka bir VM'ye eklemek için aşağıdaki adımları izleyin:

1. Portaldan kaynak grubunuzu seçin ve sorun giderme VM'nizi seçin. **Diskler'i**seçin, **Edit'i**seçin ve ardından **veri diski ekle'yi**tıklatın:

    ![Portalda varolan diski ekleme](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. Veri **diskleri** listesinde, tanımladığınız VM'nin işletim sistemi diskini seçin. İşletim sistemi diskini görmüyorsanız, sorun giderme VM ve işletim sistemi diskinin aynı bölgede (konumda) olduğundan emin olun. 
3. Değişiklikleri uygulamak için **Kaydet'i** seçin.

## <a name="mount-the-attached-data-disk-to-the-vm"></a>Bağlı veri diskini VM'ye monte etme

1. Sorun giderme VM'sine Uzak Masaüstü bağlantısı açın. 
2. Sorun giderme VM, **Open Server Manager,** sonra **Dosya ve Depolama Hizmetleri**seçin. 

    ![Server Manager'da Dosya ve Depolama Hizmetlerini Seçin](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. Veri diski otomatik olarak algılanır ve eklenir. Bağlı disklerin listesini görmek için **Diskler'i**seçin. Sürücü harfi de dahil olmak üzere ses bilgilerini görüntülemek için veri diskinizi seçebilirsiniz. Aşağıdaki örnek, bağlı ve F kullanarak veri diski **gösterir:**:

    ![Server Manager'da disk ekli ve ses bilgileri](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskteki sorunları düzeltme
Mevcut sanal sabit disk takılı ile artık gerektiğinde herhangi bir bakım ve sorun giderme adımları gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.

## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Özgün sanal sabit diski sökme ve ayırma
Hatalarınız çözüldükten sonra, varolan sanal sabit diski sorun giderme VM'nizden ayırın. Sanal sabit diskinizi sorun giderme VM'sine iliştiren kiralama serbest bırakılına kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. RDP oturumundan VM'nize, **Sunucu Yöneticisi'ni**açın, ardından **Dosya ve Depolama Hizmetleri'ni**seçin:

    ![Sunucu Yöneticisi'nde Dosya ve Depolama Hizmetlerini Seçin](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. **Diskler'i** seçin ve ardından veri diskinizi seçin. Veri diskinize sağ tıklayın ve **Çevrimdışı Kaldır'ı**seçin:

    ![Server Manager'da veri diskini çevrimdışı olarak ayarlama](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Şimdi sanal sabit diski VM'den ayırın. Azure portalında VM'nizi seçin ve **Diskler'i**tıklatın. 
4. **Edit'i**seçin, iliştirdiğiniz işletim sistemi diskini seçin ve ardından **Ayır'ı**tıklatın:

    ![Varolan sanal sabit diski ayırma](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    Devam etmeden önce VM'nin veri diskini başarıyla ayırmasını bekleyin.

## <a name="swap-the-os-disk-for-the-vm"></a>İşletim sistemi diskini VM ile değiştirme

Azure portalı artık VM'nin işletim sistemi diskini değiştirmeyi destekler. Bunu yapmak için şu adımları uygulayın:

1. Azure [portalına](https://portal.azure.com)gidin. Kenar çubuğundan **Sanal makineleri** seçin ve ardından sorun olan VM'yi seçin.
1. Sol bölmede **Diskler'i**seçin ve ardından **Os diskini değiştir'i**seçin.
        ![Azure portalında Takas Os diski ile ilgili görüntü](./media/troubleshoot-recovery-disks-portal-windows/swap-os-ui.png)

1. Onardığınız yeni diski seçin ve değişikliği onaylamak için VM'nin adını yazın. Listedeki diski görmüyorsanız, diski sorun giderme VM'den çıkardıktan sonra 10 ~ 15 dakika bekleyin. Ayrıca diskin VM ile aynı konumda olduğundan emin olun.
1. Tamam'ı seçin.

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlanmada sorun yaşıyorsanız, [bir Azure VM'ye bağlı SORUN Giderme RDP bağlantılarına](troubleshoot-rdp-connection.md)bakın. VM'nizde çalışan uygulamalara erişimle ilgili sorunlar için [Windows VM'deki Sorun Giderme uygulaması bağlantısı sorunlarına](troubleshoot-app-connection.md)bakın.

Kaynak Yöneticisi'ni kullanma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)bilgisine bakın.


