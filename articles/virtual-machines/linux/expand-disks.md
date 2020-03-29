---
title: Linux VM'de sanal sabit diskleri genişletme
description: Azure CLI ile Linux VM'de sanal sabit diskleri nasıl genişleteceklerini öğrenin.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 1295c5276f0f342323acf8d86eaaf9f785af3e9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945177"
---
# <a name="expand-virtual-hard-disks-on-a-linux-vm-with-the-azure-cli"></a>Azure CLI ile Linux VM'de sanal sabit diskleri genişletme

Bu makalede, Azure CLI ile bir Linux sanal makine (VM) için yönetilen disklerin nasıl genişletilen açıklanmaktadır. Ek depolama alanı sağlamak için [veri diskleri ekleyebilir](add-disk.md) ve varolan bir veri diskini de genişletebilirsiniz. İşletim sistemi (OS) için varsayılan sanal sabit disk boyutu, Azure'daki bir Linux VM'de genellikle 30 GB'dır. 

> [!WARNING]
> Her zaman dosya sisteminizin sağlıklı bir durumda olduğundan emin olun, disk bölümü tablo türü yeni boyutu destekleyecek ve disk yeniden boyutlandırma işlemleri gerçekleştirmeden önce verilerinizin yedekolduğundan emin olun. Daha fazla bilgi için [Azure'da Linux VM'lerini Yedekle'ye](tutorial-backup-vms.md)bakın. 

## <a name="expand-an-azure-managed-disk"></a>Azure Yönetilen Diski Genişletme
En son [Azure CLI'sini](/cli/azure/install-az-cli2) yüklediğinizden ve [az giriş](/cli/azure/reference-index#az-login)kullanarak bir Azure hesabında oturum açtığından emin olun.

Bu makalede, en az bir veri diski takılı ve hazırlanmış Azure'da varolan bir VM gerekir. Zaten kullanabileceğiniz bir VM'iniz yoksa, [bkz.](tutorial-manage-disks.md#create-and-attach-disks)

Aşağıdaki örneklerde, *myResourceGroup* ve *myVM* gibi örnek parametre adlarını kendi değerlerinizle değiştirin.

1. Sanal sabit disklerde işlemler VM çalışırken gerçekleştiriciolamaz. Az [vm deallocate](/cli/azure/vm#az-vm-deallocate)ile VM'nizi bulun. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myVM* adlı VM'yi yerle bir eder:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > Sanal sabit diski genişletmek için VM'nin ayrılması gerekir. VM'yi `az vm stop` ile durdurmak, işlem kaynaklarını serbest bırakmaz. İşlem kaynaklarını serbest bırakmak `az vm deallocate`için.

1. [Az disk listesi](/cli/azure/disk#az-disk-list)olan bir kaynak grubunda yönetilen disklerin listesini görüntüleyin. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda yönetilen disklerin listesini görüntüler:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    [Az disk güncelleştirmesi](/cli/azure/disk#az-disk-update)ile gerekli diski genişletin. Aşağıdaki örnek, *myDataDisk* adlı yönetilen diski *200* GB'a genişletir:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Yönetilen bir diski genişletdiğinizde, güncelleştirilen boyut en yakın yönetilen disk boyutuna yuvarlanır. Kullanılabilir yönetilen disk boyutları ve katmanlarından oluşan bir tablo için Azure [Yönetilen Disklere Genel Bakış - Fiyatlandırma ve Faturalandırma](../windows/managed-disks-overview.md)bölümüne bakın.

1. Az [vm start](/cli/azure/vm#az-vm-start)ile VM başlatın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myVM* adlı VM'yi başlatır:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```


## <a name="expand-a-disk-partition-and-filesystem"></a>Disk bölümle meskenini ve dosya sistemini genişletme
Genişletilmiş bir disk kullanmak için, temel bölümü ve dosya sistemini genişletin.

1. Uygun kimlik bilgileri ile VM'nize SSH. [Az vm show](/cli/azure/vm#az-vm-show)ile VM'nizin genel IP adresini görebilirsiniz:

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

1. Temel bölümü ve dosya sistemini genişletin.

    a. Disk zaten monte edilmişse, aşağıdakileri sökün:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Disk `parted` bilgilerini görüntülemek ve bölümü yeniden boyutlandırmak için kullanın:

    ```bash
    sudo parted /dev/sdc
    ```

    Varolan bölüm düzeni yle `print`ilgili bilgileri görüntüle. Çıktı, altta yatan diskin 215 GB olduğunu gösteren aşağıdaki örneğe benzer:

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    c. Bölümü `resizepart`' ile genişletin. Yeni bölüm için bölüm numarası, *1*ve boyut girin:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. Çıkmak için `quit`girin.

1. Bölüm yeniden boyutlandırılanın, `e2fsck`bölüm tutarlılığını şu şekilde doğrulayın:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. Dosya sistemini `resize2fs`yeniden boyutlandırma:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. Bölümü istenilen konuma monte `/datadrive`edin:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

1. Veri diskinin yeniden boyutlandırıldığını doğrulamak için `df -h`. Aşağıdaki örnek çıktı veri sürücüsü */dev/sdc1* şimdi 200 GB olduğunu gösterir:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Sonraki adımlar
* Ek depolama alanına ihtiyacınız varsa, [Linux VM'ye veri diskleri](add-disk.md)de ekleyebilirsiniz. 
* Disk şifreleme hakkında daha fazla bilgi için [Linux VM'leri için Azure Disk Şifreleme'ye](disk-encryption-overview.md)bakın.
