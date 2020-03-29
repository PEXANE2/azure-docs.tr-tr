---
title: Azure portalında Linux sorun giderme VM'i kullanma | Microsoft Dokümanlar
description: Azure portalını kullanarak işletim sistemi diskini kurtarma VM'ine bağlayarak Linux sanal makine sorunlarını nasıl gidereceklerini öğrenin
services: virtual-machines-linux
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/19/2019
ms.author: genli
ms.openlocfilehash: e45de5c12f0d93645a0b1253acf8300527cafdbc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75374650"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Azure portalını kullanarak işletim sistemi diskini kurtarma VM'ine takarak bir Linux VM sorun giderme
Linux sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın bir örnek, VM'nin başarılı bir şekilde önyükleme yapabilmesini engelleyen geçersiz bir giriş `/etc/fstab` olacaktır. Bu makalede, sanal sabit diskinizi herhangi bir hatayı gidermek ve orijinal VM'nizi yeniden oluşturmak için sanal sabit diskinizi başka bir Linux VM'ye bağlamak için Azure portalının nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır.

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen VM'yi durdurun.
1. VM işletim sistemi diski için anlık görüntü alın.
1. Anlık görüntüden sanal bir sabit disk oluşturun.
1. Sorun giderme amacıyla sanal sabit diski başka bir Windows VM'ye takın ve monte edin.
1. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Özgün sanal sabit diskteki sorunları gidermek için dosyaları edin veya herhangi bir araç çalıştırın.
1. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
1. Os diskini VM ile değiştirin.

> [!NOTE]
> Bu makale, yönetilmeyen diske sahip VM için geçerli değildir.

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
VM'nizin neden doğru önyükleme yapamadığını belirlemek için önyükleme tanılama ve VM ekran görüntüsünü inceleyin. Yaygın bir `/etc/fstab`örnek, geçersiz bir giriş veya altta yatan bir sanal sabit disksilinen veya taşınan olacaktır.

Portalda VM'nizi seçin ve ardından **Destek + Sorun Giderme** bölümüne gidin. VM'inizden aktarılan konsol iletilerini görüntülemek için **Önyükleme tanılamasını** tıklatın. VM'nin neden bir sorunla karşılaştığını belirlemek için konsol günlüklerini gözden geçirin. Aşağıdaki örnek, el ile etkileşim gerektiren bakım modunda sıkışmış bir VM'yi gösterir:

![VM önyükleme tanılama konsol günlüklerini görüntüleme](./media/troubleshoot-recovery-disks-portal-linux/boot-diagnostics-error.png)

VM ekran görüntüsünü indirmek için önyükleme tanılama günlüğünün üst kısmındaki **Ekran Görüntüsü'ne** de tıklayabilirsiniz.

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
    
    #Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
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

## <a name="attach-disk-to-another-vm"></a>Diski başka bir VM'ye ekleme
Sonraki birkaç adımda, sorun giderme amacıyla başka bir VM kullanırsınız. Diski sorun giderme VM'ine taktıktan sonra diskin içeriğine göz atabilir ve edinebilirsiniz. Bu işlem, yapılandırma hatalarını düzeltmenize veya ek uygulama veya sistem günlüğü dosyalarını gözden geçirmenize olanak tanır. Diski başka bir VM'ye eklemek için aşağıdaki adımları izleyin:

1. Portaldan kaynak grubunuzu seçin ve sorun giderme VM'nizi seçin. **Diskler'i**seçin, **Edit'i**seçin ve ardından **veri diski ekle'yi**tıklatın:

    ![Portalda varolan diski ekleme](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. Veri **diskleri** listesinde, tanımladığınız VM'nin işletim sistemi diskini seçin. İşletim sistemi diskini görmüyorsanız, sorun giderme VM ve işletim sistemi diskinin aynı bölgede (konumda) olduğundan emin olun. 
3. Değişiklikleri uygulamak için **Kaydet'i** seçin.

## <a name="mount-the-attached-data-disk"></a>Bağlı veri diskini bağlama

> [!NOTE]
> Aşağıdaki örnekler, Ubuntu VM'de gereken adımları ayrıntılı olarak açıkla. Red Hat Enterprise Linux veya SUSE gibi farklı bir Linux dağıtım ını `mount` kullanıyorsanız, günlük dosya konumları ve komutları biraz farklı olabilir. Komutlarda uygun değişiklikler için özel dağıtım için belgelere bakın.

1. SSH uygun kimlik bilgilerini kullanarak VM sorun giderme için. Bu disk, sorun giderme VM'nize bağlı ilk veri diskiyse, büyük olasılıkla `/dev/sdc`. Ekli `dmseg` diskleri listelemek için kullanın:

    ```bash
    dmesg | grep SCSI
    ```
    Çıktı aşağıdaki örneğe benzer:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    Önceki örnekte, işletim sistemi diski ve `/dev/sda` her VM için `/dev/sdb`sağlanan geçici disk . Birden çok veri diskininvarsa, `/dev/sdd`bunlar `/dev/sde`, , ve benzeri olmalıdır.

2. Varolan sanal sabit diskinizi monte etmek için bir dizin oluşturun. Aşağıdaki örnek adlı `troubleshootingdisk`bir dizin oluşturur:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Varolan sanal sabit diskinizde birden çok bölüm varsa, gerekli bölümü monte edin. Aşağıdaki örnek, ilk birincil bölümü `/dev/sdc1`bağlar:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > En iyi yöntem, sanal sabit diskin evrensel benzersiz tanımlayıcısını (UUID) kullanarak Azure'daki VM'lere veri diskleri monte etmektir. Bu kısa sorun giderme senaryosu için UUID kullanarak sanal sabit diske montaj gerekli değildir. Ancak, normal kullanım `/etc/fstab` altında, UUID yerine aygıt adını kullanarak sanal sabit diskleri takmak için düzenleme VM önyükleme başarısız neden olabilir.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskteki sorunları düzeltme
Mevcut sanal sabit disk takılı ile artık gerektiğinde herhangi bir bakım ve sorun giderme adımları gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.

## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Özgün sanal sabit diski sökme ve ayırma
Hatalarınız çözüldükten sonra, varolan sanal sabit diski sorun giderme VM'nizden ayırın. Sanal sabit diskinizi sorun giderme VM'sine iliştiren kiralama serbest bırakılına kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. SSH oturumundan sorun giderme VM'nize kadar, varolan sanal sabit diski boşaltın. Önce montaj noktanız için üst dizinindışına çık:

    ```bash
    cd /
    ```

    Şimdi varolan sanal sabit diski sökün. Aşağıdaki örnek, aygıtın aşağıdaki `/dev/sdc1`noktasında söküldiyi

    ```bash
    sudo umount /dev/sdc1
    ```

2. Şimdi sanal sabit diski VM'den ayırın. Portalda VM'nizi seçin ve **Diskler'i**tıklatın. Varolan sanal sabit diskinizi seçin ve sonra **Ayır'ı**tıklatın:

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
VM'nize bağlanmada sorun yaşıyorsanız, [Bir Azure VM'ye bağlı Sorun Giderme SSH bağlantılarına](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın. VM'nizde çalışan uygulamalara erişimle ilgili sorunlar için, [Linux VM'deki Sorun Giderme uygulaması bağlantısı sorunlarına](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.

Kaynak Yöneticisi'ni kullanma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bilgisine bakın.
