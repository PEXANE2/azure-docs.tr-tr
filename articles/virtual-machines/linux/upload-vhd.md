---
title: Azure CLI ile özel bir Linux VM yükleme veya kopyalama
description: Kaynak Yöneticisi dağıtım modelini ve Azure CLI'yi kullanarak özelleştirilmiş bir sanal makineyükleme veya kopyalama
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/10/2019
ms.author: cynthn
ms.openlocfilehash: b5063c8037a763c1919d2172a81c8abbbd406ace
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060158"
---
# <a name="create-a-linux-vm-from-a-custom-disk-with-the-azure-cli"></a>Azure CLI ile özel bir diskten Linux VM oluşturma

<!-- rename to create-vm-specialized -->

Bu makalede, özelleştirilmiş bir sanal sabit diskin (VHD) nasıl yüklendiği ve Azure'da varolan bir VHD'nin nasıl kopyalanış edildiği gösterilmektedir. Yeni oluşturulan VHD sonra yeni Linux sanal makineleri (VMs) oluşturmak için kullanılır. Bir Linux dağıtımını gereksinimlerinize göre yükleyebilir ve yapılandırabilir ve ardından bu VHD'yi yeni bir Azure sanal makinesi oluşturmak için kullanabilirsiniz.

Özelleştirilmiş diskinizden birden fazla VM oluşturmak için önce VM veya VHD'nizden bir görüntü oluşturun. Daha fazla bilgi için [CLI'yi kullanarak Azure VM'nin özel bir görüntüsünü oluştur'a](tutorial-custom-images.md)bakın.

Özel bir disk oluşturmak için iki seçeneğiniz vardır:
* VHD’yi karşıya yükleme
* Varolan bir Azure VM'yi kopyalama


## <a name="requirements"></a>Gereksinimler
Aşağıdaki adımları tamamlamak için şunları yapmanız gerekir:

- Azure'da kullanılmak üzere hazırlanmış bir Linux sanal makinesi. Bu [makalenin VM'yi Hazırlama](#prepare-the-vm) bölümü, SSH ile bir VM'ye bağlanmanız için gereken Azure Linux Aracısını (waagent) yüklemeyle ilgili dağıtıma özgü bilgilerin nasıl bulunabileceğinizi kapsar.
- VHD dosyası, [Mevcut Bir Azure onaylı Linux dağıtımından](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (veya [onaylanmayan dağıtımlara yönelik bilgilere](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın) VHD biçimindeki bir sanal diske. VM ve VHD oluşturmak için birden çok araç vardır:
  - [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) veya [KVM'yi](https://www.linux-kvm.org/page/RunningKVM)yükleyin ve yapılandırın, görüntü biçiminiz olarak VHD'yi kullanmaya özen. Gerekirse, bir [görüntüyü](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) `qemu-img convert`.
  - Hyper-V'yi [Windows 10'da](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) veya [Windows Server 2012/2012 R2'de](https://technet.microsoft.com/library/hh846766.aspx)de kullanabilirsiniz.

> [!NOTE]
> Azure'da yeni VHDX biçimi desteklenmez. Bir VM oluşturduğunuzda, biçim olarak VHD belirtin. Gerekirse, [Qemu-img convert](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) veya [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet ile VHDX diskleri VHD'ye dönüştürebilirsiniz. Azure dinamik VHD'lerin yüklenmesini desteklemez, bu nedenle yüklemeden önce bu tür diskleri statik VHD'lere dönüştürmeniz gerekir. Dinamik diskleri Azure'a yükleme işlemi sırasında dönüştürmek [için GO için Azure VHD Yardımcı Programları](https://github.com/Microsoft/azure-vhd-utils-for-go) gibi araçları kullanabilirsiniz.
> 
> 


- En son [Azure CLI](/cli/azure/install-az-cli2) yüklü olduğundan ve az girişi olan bir Azure hesabında oturum açtığınızdan emin olun. [az login](/cli/azure/reference-index#az-login)

Aşağıdaki örneklerde, örnek parametre adlarını `myResourceGroup`kendi değerlerinizle `mystorageaccount`değiştirin, , , ve `mydisks`.

<a id="prepimage"> </a>

## <a name="prepare-the-vm"></a>VM’yi hazırlama

Azure çeşitli Linux dağıtımlarını destekler [(bkz.](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Aşağıdaki makaleler, Azure'da desteklenen çeşitli Linux dağıtımlarının nasıl hazırlanacağını açıklayınız:

* [CentOS Tabanlı Dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES ve openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Diğerleri: Onaylanmayan Dağıtımlar](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Ayrıca, Azure için Linux görüntüleri hazırlamayla ilgili daha genel ipuçları için [Linux Yükleme Notları'na](create-upload-generic.md#general-linux-installation-notes) bakın.

> [!NOTE]
> [Azure platformu SLA,](https://azure.microsoft.com/support/legal/sla/virtual-machines/) Linux'u çalıştıran VM'ler için yalnızca onaylanan dağıtımlardan biri [Azure Onaylı Dağıtımlarda Linux'ta](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)"Desteklenen Sürümler" altında belirtildiği şekilde yapılandırma ayrıntılarıyla kullanıldığında geçerlidir.
> 
> 

## <a name="option-1-upload-a-vhd"></a>Seçenek 1: VHD yükleme

Artık VHD'yi doğrudan yönetilen bir diske yükleyebilirsiniz. Talimatlar için Azure [CLI kullanarak Azure'a VHD Yükle](disks-upload-vhd-to-managed-disk-cli.md)'ye bakın.

## <a name="option-2-copy-an-existing-vm"></a>Seçenek 2: Varolan bir VM'yi kopyalama

Ayrıca Azure'da özelleştirilmiş bir VM oluşturabilir ve ardından işletim sistemi diskini kopyalayıp başka bir kopya oluşturmak için yeni bir VM'ye ekleyebilirsiniz. Bu sınama için iyidir, ancak birden çok yeni VM modeli olarak varolan bir Azure VM'yi kullanmak istiyorsanız, bunun yerine bir *resim* oluşturun. Varolan bir Azure VM'den görüntü oluşturma hakkında daha fazla bilgi için [cli'yi kullanarak Azure VM'nin özel bir görüntüsünü oluştur'a](tutorial-custom-images.md)bakın.

Varolan bir VM'yi başka bir bölgeye kopyalamak istiyorsanız, [başka bir bölgedeki bir diskin kopyasını taklit](disks-upload-vhd-to-managed-disk-cli.md#copy-a-managed-disk)etmek için azcopy kullanmak isteyebilirsiniz. 

Aksi takdirde, VM bir anlık almak ve daha sonra anlık görüntü yeni bir işletim sistemi VHD oluşturmanız gerekir.

### <a name="create-a-snapshot"></a>Anlık görüntü oluşturma

Bu örnek, kaynak grubu *myResourceGroup'ta myVM* adlı bir VM anlık görüntüsünü oluşturur ve *osDiskSnapshot*adında bir anlık görüntü oluşturur. *myResourceGroup*

```azurecli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a>Yönetilen diski oluşturma

Anlık görüntüden yeni yönetilen bir disk oluşturun.

Anlık görüntünün kimliğini alın. Bu örnekte, anlık görüntü *osDiskSnapshot* olarak adlandırılır ve *myResourceGroup* kaynak grubundadır.

```azurecli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

Yönetilen diski oluşturun. Bu örnekte, diskin standart depolama alanında olduğu ve 128 GB boyutunda olduğu anlık *görüntümüzdemyManagedDisk* adında yönetilen bir disk oluştururuz.

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a>Sanal makine oluşturma

Az [vm ile](/cli/azure/vm#az-vm-create) VM'nizi oluşturun ve yönetilen diski işletim sistemi diski olarak takın (--ekle-os-disk) oluşturun. Aşağıdaki örnek, yüklenen VHD'nizden oluşturduğunuz yönetilen diski kullanarak *myNewVM* adında bir VM oluşturur:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

Kaynak VM kimlik bilgileri ile VM içine SSH gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
Özel sanal diskinizi hazırlayıp yükledikten [sonra, Kaynak Yöneticisi ve şablonları kullanma](../../azure-resource-manager/management/overview.md)hakkında daha fazla bilgi edinebilirsiniz. Ayrıca, yeni Sanal M'lerinize [bir veri diski eklemek](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) isteyebilirsiniz. VM'lerinizde erişmeniz gereken uygulamalar çalışıyorsa, [bağlantı noktalarını ve bitiş noktalarını açtığınızdan](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)emin olun.
