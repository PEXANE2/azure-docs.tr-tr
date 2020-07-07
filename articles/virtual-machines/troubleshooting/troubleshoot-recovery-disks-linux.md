---
title: Azure CLı ile Linux sorun giderme VM kullanma | Microsoft Docs
description: Azure CLı kullanarak işletim sistemi diskini bir kurtarma VM 'sine bağlayarak Linux VM sorunlarını giderme hakkında bilgi edinin
services: virtual-machines-linux
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: genli
ms.openlocfilehash: 1b91a39e1297d8952da67a4f8d3b8568cefe04ce
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "73620554"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli"></a>Azure CLı ile işletim sistemi diskini bir kurtarma sanal makinesine ekleyerek bir Linux sanal makinesi sorunlarını giderme
Linux sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, sanal sabit diskin kendisi üzerinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Ortak bir örnek `/etc/fstab` , VM 'nin başarıyla önyükleme yapabilmesini engelleyen ' de geçersiz bir giriş olabilir. Bu makalede, herhangi bir hatayı onarmak üzere sanal sabit diskinizi başka bir Linux VM 'sine bağlamak için Azure CLı 'nın nasıl kullanılacağı açıklanır ve ardından özgün VM 'nizi yeniden oluşturabilirsiniz. 

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen VM 'yi durdurun.
1. VM 'nin işletim sistemi diskinden bir anlık görüntü alın.
1. İşletim sistemi diski anlık görüntüsünden bir disk oluşturun.
1. Sorun giderme amacıyla yeni işletim sistemi diskini başka bir Linux VM 'sine ekleyin ve bağlayın.
1. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Yeni işletim sistemi diskindeki sorunları gidermek için dosyaları düzenleyin veya herhangi bir araç çalıştırın.
1. Yeni işletim sistemi diskini sorun giderme VM 'sinden çıkarın ve ayırın.
1. Etkilenen VM için işletim sistemi diskini değiştirin.

Bu sorun giderme adımlarını gerçekleştirmek için, [az Login](/cli/azure/reference-index)kullanılarak en son [Azure CLI](/cli/azure/install-az-cli2) 'Nın yüklü ve bir Azure hesabında oturum açmış olmanız gerekir.

> [!Important]
> Bu makaledeki betikler yalnızca [yönetilen disk](../linux/managed-disks-overview.md)kullanan VM 'ler için geçerlidir. 

Aşağıdaki örneklerde, ve gibi parametre adlarını kendi değerlerinizle değiştirin `myResourceGroup` `myVM` .

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
SANAL makinenizin neden doğru şekilde önyüklenemediğini anlamak için seri çıktıyı inceleyin. Ortak bir örnek, içinde geçersiz bir giriştir `/etc/fstab` veya silinmekte veya taşınmakta olan temeldeki sanal sabit disk.

[Az VM Boot-Diagnostics Get-boot-log](/cli/azure/vm/boot-diagnostics)ile önyükleme günlüklerini alın. Aşağıdaki örnek, adlı kaynak grubunda adlı VM 'den alınan seri çıktıyı alır `myVM` `myResourceGroup` :

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

VM 'nin neden önyükleme başarısız olduğunu anlamak için seri çıktıyı gözden geçirin. Seri çıkış herhangi bir bildirim sağlamadıysa, `/var/log` sanal sabit diski bir sorun GIDERME sanal makinesine bağlıyken günlük dosyalarını gözden geçirmeniz gerekebilir.

## <a name="stop-the-vm"></a>VM’yi durdurma

Aşağıdaki örnek, adlı kaynak grubundan adlı VM 'yi durduruyor `myVM` `myResourceGroup` :

```azurecli
az vm stop --resource-group MyResourceGroup --name MyVm
```
## <a name="take-a-snapshot-from-the-os-disk-of-the-affected-vm"></a>Etkilenen VM 'nin işletim sistemi diskinden bir anlık görüntü alın

Anlık görüntü, bir VHD 'nin tam ve salt okunurdur kopyasıdır. Bir sanal makineye bağlanamaz. Bir sonraki adımda, bu anlık görüntüden bir disk oluşturacağız. Aşağıdaki örnek, `mySnapshot` ' myVM ' ADLı VM 'nin işletim sistemi diskinden adında bir anlık görüntü oluşturur. 

```azurecli
#Get the OS disk Id 
$osdiskid=(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)

#creates a snapshot of the disk
az snapshot create --resource-group myResourceGroupDisk --source "$osdiskid" --name mySnapshot
```
## <a name="create-a-disk-from-the-snapshot"></a>Anlık görüntüden disk oluşturma

Bu betik adlı anlık görüntüden adında bir yönetilen disk oluşturur `myOSDisk` `mySnapshot` .  

```azurecli
#Provide the name of your resource group
$resourceGroup=myResourceGroup

#Provide the name of the snapshot that will be used to create Managed Disks
$snapshot=mySnapshot

#Provide the name of the Managed Disk
$osDisk=myNewOSDisk

#Provide the size of the disks in GB. It should be greater than the VHD file size.
$diskSize=128

#Provide the storage type for Managed Disk. Premium_LRS or Standard_LRS.
$storageType=Premium_LRS

#Provide the OS type
$osType=linux

#Provide the name of the virtual machine
$virtualMachine=myVM

#Get the snapshot Id 
$snapshotId=(az snapshot show --name $snapshot --resource-group $resourceGroup --query [id] -o tsv)

# Create a new Managed Disks using the snapshot Id.

az disk create --resource-group $resourceGroup --name $osDisk --sku $storageType --size-gb $diskSize --source $snapshotId

```

Kaynak grubu ve kaynak anlık görüntüsü aynı bölgede değilse, çalıştırdığınızda "kaynak bulunamadı" hatasını alırsınız `az disk create` . Bu durumda, `--location <region>` diski kaynak anlık görüntüsü ile aynı bölgede oluşturmayı belirtmeniz gerekir.

Artık özgün işletim sistemi diskinin bir kopyasına sahipsiniz. Sorun giderme amacıyla bu yeni diski başka bir Windows sanal makinesine bağlayabilirsiniz.

## <a name="attach-the-new-virtual-hard-disk-to-another-vm"></a>Yeni sanal sabit diski başka bir VM 'ye iliştirme
Sonraki birkaç adımda, sorun giderme amacıyla başka bir VM kullanırsınız. Diskin içeriğini taramak ve düzenlemek için bu sorun giderme sanal makinesine diski iliştirin. Bu işlem, yapılandırma hatalarını düzeltmenize veya ek uygulama veya sistem günlük dosyalarını incelemenizi sağlar.

Bu betik, diski `myNewOSDisk` VM 'ye ekler `MyTroubleshootVM` :

```azurecli
# Get ID of the OS disk that you just created.
$myNewOSDiskid=(az vm show -g myResourceGroupDisk -n myNewOSDisk --query "storageProfile.osDisk.managedDisk.id" -o tsv)

# Attach the disk to the troubleshooting VM
az vm disk attach --disk $diskId --resource-group MyResourceGroup --size-gb 128 --sku Standard_LRS --vm-name MyTroubleshootVM
```
## <a name="mount-the-attached-data-disk"></a>Bağlı veri diskini bağlama

> [!NOTE]
> Aşağıdaki örneklerde bir Ubuntu VM 'de gereken adımlar ayrıntılı olarak verilmiştir. Red Hat Enterprise Linux veya SUSE gibi farklı bir Linux kaldırı kullanıyorsanız, günlük dosyası konumları ve `mount` komutları biraz farklı olabilir. Komutlarınıza ilişkin uygun değişiklikler için özel olarak verilen belgelere başvurun.

1. Uygun kimlik bilgilerini kullanarak sorun giderme sanal makinesine SSH. Bu disk, sorun giderme sanal makinesine bağlı ilk veri diskidir, disk muhtemelen öğesine bağlıdır `/dev/sdc` . `dmesg`Eklenen diskleri görüntülemek için kullanın:

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

    Yukarıdaki örnekte, işletim sistemi diski `/dev/sda` ve her sanal makine için belirtilen geçici disk ' dir `/dev/sdb` . Birden çok veri diskine sahipseniz,, vb. olmaları gerekir `/dev/sdd` `/dev/sde` .

2. Var olan sanal sabit diskinizi bağlamak için bir dizin oluşturun. Aşağıdaki örnek adlı bir dizin oluşturur `troubleshootingdisk` :

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Var olan sanal sabit diskinizde birden çok bölüm varsa, gerekli bölümü bağlayın. Aşağıdaki örnek, konumundaki ilk birincil bölümü bağlar `/dev/sdc1` :

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > En iyi yöntem, sanal sabit diskin evrensel benzersiz tanımlayıcısını (UUID) kullanarak Azure 'daki VM 'Lere veri disklerini bağlamasıdır. Bu kısa sorun giderme senaryosunda, UUID 'yi kullanarak sanal sabit diski bağlama gerekli değildir. Ancak, normal kullanım altında, `/etc/fstab` sanal sabit DISKLERIN UUID yerine cihaz adı kullanılarak bağlanması için düzenlemenin başarısız olması VM 'nin önyüklenememesine neden olabilir.


## <a name="fix-issues-on-the-new-os-disk"></a>Yeni işletim sistemi diskinde sorunları çözme
Var olan sanal sabit disk takılı olduğunda, artık gereken tüm bakım ve sorun giderme adımlarını gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-the-new-os-disk"></a>Yeni işletim sistemi diskini çıkarma ve ayırma
Hatalar çözümlendikten sonra, var olan sanal sabit diski sorun giderme sanal makinenizin bağlantısını çıkarırın. Sanal sabit diski, sorun giderme VM 'sine iliştirene kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. SSH oturumundan sorun giderme sanal makinesine var olan sanal sabit diski çıkarın. İlk olarak bağlama noktanız için üst dizinden geçiş yapın:

    ```bash
    cd /
    ```

    Şimdi var olan sanal sabit diski çıkarın. Aşağıdaki örnek, cihazı şu adreste kaldırır `/dev/sdc1` :

    ```bash
    sudo umount /dev/sdc1
    ```

2. Artık sanal sabit diski VM 'den ayırın. SSH oturumundan sorun giderme sanal makinesine çıkın:

    ```azurecli
    az vm disk detach -g MyResourceGroup --vm-name MyTroubleShootVm --name myNewOSDisk
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>Etkilenen VM için işletim sistemi diskini değiştirme

İşletim sistemi disklerini değiştirmek için Azure CLı kullanabilirsiniz. VM 'yi silip yeniden oluşturmanız gerekmez.

Bu örnek, adlı VM 'yi durdurup `myVM` `myNewOSDisk` yeni işletim sistemi diski olarak adlandırılan diski atar.

```azurecli
# Stop the affected VM
az vm stop -n myVM -g myResourceGroup

# Get ID of the OS disk that is repaired.
$myNewOSDiskid=(az vm show -g myResourceGroupDisk -n myNewOSDisk --query "storageProfile.osDisk.managedDisk.id" -o tsv)

# Change the OS disk of the affected VM to "myNewOSDisk"
az vm update -g myResourceGroup -n myVM --os-disk $myNewOSDiskid

# Start the VM
az vm start -n myVM -g myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure VM Ile SSH bağlantılarında sorun giderme](troubleshoot-ssh-connection.md). VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [bir LINUX sanal makinesinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).

