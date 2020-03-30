---
title: Azure CLI ile Linux sorun giderme VM'i kullanma | Microsoft Dokümanlar
description: Azure CLI'yi kullanarak işletim sistemi diskini kurtarma VM'ine bağlayarak Linux VM sorunlarını nasıl gidereceklerini öğrenin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73620554"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli"></a>Azure CLI ile işletim sistemi diskini kurtarma VM'ine takarak bir Linux VM sorun giderme
Linux sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın bir örnek, VM'nin başarılı bir şekilde önyükleme yapabilmesini engelleyen geçersiz bir giriş `/etc/fstab` olacaktır. Bu makalede, sanal sabit diskinizi herhangi bir hatayı gidermek ve orijinal VM'nizi yeniden oluşturmak için sanal sabit diskinizi başka bir Linux VM'ye bağlamak için Azure CLI'yi nasıl kullanacağınızı ayrıntılı olarak açıklayabilirsiniz. 

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Etkilenen VM'yi durdurun.
1. VM işletim sistemi diskinden anlık görüntü alın.
1. İşletim sistemi disk anlık görüntüsünden bir disk oluşturun.
1. Sorun giderme amacıyla yeni işletim sistemi diskini başka bir Linux VM'ye takın ve takın.
1. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları edin veya yeni işletim sistemi diskindeki sorunları gidermek için herhangi bir araç çalıştırın.
1. Yeni işletim sistemi diskini sorun giderme VM'den sökün ve ayırın.
1. Etkilenen VM için işletim sistemi diskini değiştirin.

Bu sorun giderme adımlarını gerçekleştirmek için, en son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir.

> [!Important]
> Bu makaledeki komut dosyaları yalnızca [Yönetilen Disk'i](../linux/managed-disks-overview.md)kullanan VM'ler için geçerlidir. 

Aşağıdaki örneklerde, parametre adlarını kendi değerlerinizle `myResourceGroup` `myVM`değiştirin, örneğin .

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
VM'nizin neden doğru önyükleme yapamadığını belirlemek için seri çıktısını inceleyin. Yaygın bir örnek, geçersiz `/etc/fstab`bir giriş veya altta yatan sanal sabit disksilinen veya taşınan.

[az vm önyükleme-tanılama get-boot-log](/cli/azure/vm/boot-diagnostics)ile önyükleme günlükleri alın. Aşağıdaki örnek, adlı `myVM` `myResourceGroup`kaynak grubunda adı geçen VM'den seri çıktısını alır:

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

VM'nin neden önyüklemede başarısız olduğunu belirlemek için seri çıktısını gözden geçirin. Seri çıktısı herhangi bir gösterge sunmuyorsa, sorun giderme VM'sine bağlı sanal sabit diski bir `/var/log` kez dosyalarını gözden geçirmeniz gerekebilir.

## <a name="stop-the-vm"></a>VM’yi durdurma

Aşağıdaki örnek, adlı kaynak `myVM` grubundan adlı `myResourceGroup`VM'yi durdurur:

```azurecli
az vm stop --resource-group MyResourceGroup --name MyVm
```
## <a name="take-a-snapshot-from-the-os-disk-of-the-affected-vm"></a>Etkilenen VM'nin işletim sistemi diskinden anlık görüntü alma

Anlık görüntü, bir VHD'nin tam, salt okunur kopyasıdır. VM'ye bağlanamaz. Bir sonraki adımda, bu anlık görüntüden bir disk oluşturacağız. Aşağıdaki örnekte, VM'nin işletim sistemi diskinden 'myVM' adlı adla `mySnapshot` bir anlık görüntü oluşturulur. 

```azurecli
#Get the OS disk Id 
$osdiskid=(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)

#creates a snapshot of the disk
az snapshot create --resource-group myResourceGroupDisk --source "$osdiskid" --name mySnapshot
```
## <a name="create-a-disk-from-the-snapshot"></a>Anlık görüntüden disk oluşturma

Bu komut dosyası, adlı `myOSDisk` `mySnapshot`anlık görüntüden adı olan yönetilen bir disk oluşturur.  

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

Kaynak grubu ve kaynak anlık görüntü aynı bölgede değilse, çalıştırdığınızda `az disk create`"Kaynak bulunamadı" hatasını alırsınız. Bu durumda, diski `--location <region>` kaynak anlık görüntüyle aynı bölgeye oluşturmak için belirtmeniz gerekir.

Şimdi orijinal işletim sistemi diskinin bir kopyası var. Sorun giderme amacıyla bu yeni diski başka bir Windows VM'ye monte edebilirsiniz.

## <a name="attach-the-new-virtual-hard-disk-to-another-vm"></a>Yeni sanal sabit diski başka bir VM'ye takın
Sonraki birkaç adımda, sorun giderme amacıyla başka bir VM kullanırsınız. Diskin içeriğine göz atmak ve bunları yapmak için diski bu sorun giderme VM'sine bağlarsınız. Bu işlem, yapılandırma hatalarını düzeltmenize veya ek uygulama veya sistem günlüğü dosyalarını gözden geçirmenize olanak tanır.

Bu komut dosyası `myNewOSDisk` diski `MyTroubleshootVM`VM'ye iliştirin:

```azurecli
# Get ID of the OS disk that you just created.
$myNewOSDiskid=(az vm show -g myResourceGroupDisk -n myNewOSDisk --query "storageProfile.osDisk.managedDisk.id" -o tsv)

# Attach the disk to the troubleshooting VM
az vm disk attach --disk $diskId --resource-group MyResourceGroup --size-gb 128 --sku Standard_LRS --vm-name MyTroubleshootVM
```
## <a name="mount-the-attached-data-disk"></a>Bağlı veri diskini bağlama

> [!NOTE]
> Aşağıdaki örnekler, Ubuntu VM'de gereken adımları ayrıntılı olarak açıkla. Red Hat Enterprise Linux veya SUSE gibi farklı bir Linux dağıtım ını `mount` kullanıyorsanız, günlük dosya konumları ve komutları biraz farklı olabilir. Komutlarda uygun değişiklikler için özel dağıtım için belgelere bakın.

1. SSH uygun kimlik bilgilerini kullanarak VM sorun giderme için. Bu disk, sorun giderme VM'nize bağlı ilk veri diskiyse, disk büyük olasılıkla `/dev/sdc`. Ekli `dmesg` diskleri görüntülemek için kullanın:

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


## <a name="fix-issues-on-the-new-os-disk"></a>Yeni işletim sistemi diskindeki sorunları düzeltme
Mevcut sanal sabit disk takılı ile artık gerektiğinde herhangi bir bakım ve sorun giderme adımları gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-the-new-os-disk"></a>Yeni işletim sistemi diskini sökme ve ayırma
Hatalarınız çözüldükten sonra, varolan sanal sabit diski sorun giderme VM'nizden söküp ayırabilirsiniz. Sanal sabit diskinizi sorun giderme VM'sine iliştiren kiralama serbest bırakılına kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. SSH oturumundan sorun giderme VM'nize kadar, varolan sanal sabit diski boşaltın. Önce montaj noktanız için üst dizinindışına çık:

    ```bash
    cd /
    ```

    Şimdi varolan sanal sabit diski sökün. Aşağıdaki örnek, aygıtın aşağıdaki `/dev/sdc1`noktasında söküldiyi

    ```bash
    sudo umount /dev/sdc1
    ```

2. Şimdi sanal sabit diski VM'den ayırın. Sorun giderme VM'inize SSH oturumundan çıkın:

    ```azurecli
    az vm disk detach -g MyResourceGroup --vm-name MyTroubleShootVm --name myNewOSDisk
    ```

## <a name="change-the-os-disk-for-the-affected-vm"></a>Etkilenen VM için işletim sistemi diskini değiştirme

İşletim sistemi disklerini değiştirmek için Azure CLI'yi kullanabilirsiniz. VM'yi silmek ve yeniden oluşturmak zorunda değilsiniz.

Bu örnek, VM `myVM` adlı durur ve `myNewOSDisk` yeni işletim sistemi diski olarak adlandırılan disk atar.

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
VM'nize bağlanmada sorun yaşıyorsanız, [Bir Azure VM'ye bağlı Sorun Giderme SSH bağlantılarına](troubleshoot-ssh-connection.md)bakın. VM'nizde çalışan uygulamalara erişimle ilgili sorunlar için, [Linux VM'deki Sorun Giderme uygulaması bağlantısı sorunlarına](troubleshoot-app-connection.md)bakın.

