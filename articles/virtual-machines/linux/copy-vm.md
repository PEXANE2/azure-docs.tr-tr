---
title: Azure CLı kullanarak bir Linux VM kopyalama
description: Azure CLı ve yönetilen diskleri kullanarak Azure Linux sanal makinenizin bir kopyasını oluşturmayı öğrenin.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 10/17/2018
ms.author: cynthn
ms.custom: legacy
ms.openlocfilehash: 406eda6902ae451c7fdf79e1bd1215c035a66750
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82792233"
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-and-managed-disks"></a>Azure CLı ve yönetilen diskleri kullanarak bir Linux VM 'nin kopyasını oluşturma

Bu makalede, Azure CLı kullanarak Linux çalıştıran Azure sanal makinenizin (VM) bir kopyasının nasıl oluşturulacağı gösterilmektedir. VM görüntülerini kopyalamak, oluşturmak, depolamak ve ölçeklendirmek için bkz. [paylaşılan görüntü galerileri](shared-images.md).

Ayrıca, bir [VHD 'den BIR VM 'yi karşıya yükleyebilir ve oluşturabilirsiniz](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Ön koşullar

-   [Azure CLI](/cli/azure/install-az-cli2)'yı yükler.

-   [Az Login](/cli/azure/reference-index#az-login)komutuyla bir Azure hesabında oturum açın.

-   Kopyanız için kaynak olarak kullanılacak bir Azure VM 'niz olmalıdır.

## <a name="stop-the-source-vm"></a>Kaynak VM 'yi durdur

[Az VM serbest bırakma](/cli/azure/vm#az-vm-deallocate)kullanarak kaynak VM 'yi serbest bırakın.
Aşağıdaki örnek, *Myresourcegroup*kaynak grubundaki *MYVM* adlı VM 'yi kaldırır:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

## <a name="copy-the-source-vm"></a>Kaynak VM 'yi kopyalama

Bir VM 'yi kopyalamak için, temeldeki sanal sabit diskin bir kopyasını oluşturursunuz. Bu işlem, kaynak VM ile aynı yapılandırma ve ayarları içeren bir yönetilen disk olarak özelleştirilmiş bir sanal sabit disk (VHD) oluşturur.

Azure Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../windows/managed-disks-overview.md). 

1.  Her VM 'yi ve işletim sistemi diskinin adını [az VM List](/cli/azure/vm#az-vm-list)ile listeleyin. Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubundaki tüm VM 'leri listeler:
    
    ```azurecli
    az vm list -g myResourceGroup \
         --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' \
         --output table
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  Yeni bir yönetilen disk oluşturarak ve [az disk Create](/cli/azure/disk#az-disk-create)kullanarak diski kopyalayın. Aşağıdaki örnek, *mydisk*adlı yönetilen diskten *Mycopieddisk* adlı bir disk oluşturur:

    ```azurecli
    az disk create --resource-group myResourceGroup \
         --name myCopiedDisk --source myDisk
    ``` 

1.  Kaynak grubunuzda [az disk List](/cli/azure/disk#az-disk-list)kullanarak yönetilen diskleri şimdi doğrulayın. Aşağıdaki örnek, *Myresourcegroup*adlı kaynak grubundaki yönetilen diskleri listeler:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```


## <a name="set-up-a-virtual-network"></a>Sanal ağ ayarlama

Aşağıdaki isteğe bağlı adımlar yeni bir sanal ağ, alt ağ, genel IP adresi ve sanal ağ arabirim kartı (NIC) oluşturur.

Bir VM 'yi sorun giderme amacıyla veya ek dağıtımlar için kopyalıyorsanız, var olan bir sanal ağda VM kullanmak istemeyebilirsiniz.

Kopyalanmış VM 'niz için bir sanal ağ altyapısı oluşturmak istiyorsanız, sonraki birkaç adımı izleyin. Sanal ağ oluşturmak istemiyorsanız, [VM oluşturmak](#create-a-vm)için atlayın.

1.  [Az Network VNET Create](/cli/azure/network/vnet#az-network-vnet-create)kullanarak sanal ağ oluşturun. Aşağıdaki örnek, *Myvnet* adlı bir sanal ağ ve *mysubnet*adlı bir alt ağ oluşturur:

    ```azurecli
    az network vnet create --resource-group myResourceGroup \
        --location eastus --name myVnet \
        --address-prefix 192.168.0.0/16 \
        --subnet-name mySubnet \
        --subnet-prefix 192.168.1.0/24
    ```

1.  [Az Network public-IP Create](/cli/azure/network/public-ip#az-network-public-ip-create)kullanarak genel IP oluşturun. Aşağıdaki örnek, mypublicdns DNS adıyla *Mypublicıp* adlı BIR genel *mypublicdns*IP oluşturur. (DNS adı benzersiz olmalıdır, benzersiz bir ad sağlayın.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup \
        --location eastus --name myPublicIP --dns-name mypublicdns \
        --allocation-method static --idle-timeout 4
    ```

1.  [Az Network NIC Create](/cli/azure/network/nic#az-network-nic-create)kullanarak NIC 'yi oluşturun.
    Aşağıdaki örnek, *Mysubnet* alt ağına bağlı *MYNıC* adlı bir NIC oluşturur:

    ```azurecli
    az network nic create --resource-group myResourceGroup \
        --location eastus --name myNic \
        --vnet-name myVnet --subnet mySubnet \
        --public-ip-address myPublicIP
    ```

## <a name="create-a-vm"></a>VM oluşturma

[Az VM Create](/cli/azure/vm#az-vm-create)kullanarak bir VM oluşturun.

İşletim sistemi diski () olarak kullanılacak kopyalanmış yönetilen diski `--attach-os-disk` aşağıdaki gibi belirtin:

```azurecli
az vm create --resource-group myResourceGroup \
    --name myCopiedVM --nics myNic \
    --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Sonraki adımlar

VM görüntülerini yönetmek için [paylaşılan görüntü galerisini](shared-images.md) nasıl kullanacağınızı öğrenmek için.
