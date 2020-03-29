---
title: Azure CLI kullanarak Linux VM kopyalama
description: Azure CLI ve Yönetilen Diskler'i kullanarak Azure Linux VM'nizin bir kopyasını nasıl oluşturup oluşturabilirsiniz öğrenin.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 10/17/2018
ms.author: cynthn
ms.openlocfilehash: ed8574133eafe751699e90ea8cae832ee649fb00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969594"
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-and-managed-disks"></a>Azure CLI ve Yönetilen Diskler'i kullanarak Linux VM'nin kopyasını oluşturma

Bu makalede, Azure CLI ve Azure Kaynak Yöneticisi dağıtım modelini kullanarak Linux çalıştıran Azure sanal makinenizin (VM) bir kopyasını nasıl oluşturabileceğiniz gösterilmektedir. 

Ayrıca [bir VHD'den vm yükleyebilir ve oluşturabilirsiniz.](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="prerequisites"></a>Ön koşullar

-   Azure [CLI'yi](/cli/azure/install-az-cli2)yükleyin.

-   Az giriş ile bir Azure hesabında oturum [açın.](/cli/azure/reference-index#az-login)

-   Kopyanızın kaynağı olarak kullanmak üzere bir Azure VM'ine sahip olun.

## <a name="stop-the-source-vm"></a>Kaynak VM'yi durdurun

Deallocate [az vm deallocate](/cli/azure/vm#az-vm-deallocate)kullanarak kaynak VM bulun .
Aşağıdaki örnek, *myResourceGroup*kaynak grubunda *myVM* adlı VM'yi yerle bir eder:

```azurecli
az vm deallocate \
    --resource-group myResourceGroup \
    --name myVM
```

## <a name="copy-the-source-vm"></a>Kaynak VM'yi kopyalama

VM'yi kopyalamak için, altta yatan sanal sabit diskin bir kopyasını oluşturursunuz. Bu işlem, kaynak VM ile aynı yapılandırma ve ayarları içeren yönetilen bir disk olarak özelleştirilmiş bir sanal sabit disk (VHD) oluşturur.

Azure Yönetilen Diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../windows/managed-disks-overview.md). 

1.  Her VM'yi ve işletim sistemi diskinin adını [az vm listesiile listele.](/cli/azure/vm#az-vm-list) Aşağıdaki örnekte *myResourceGroup*adlı kaynak grubundaki tüm VM'ler listeleneme:
    
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

1.  Yeni yönetilen bir disk oluşturarak ve [az disk oluşturarak diski kopyalayın.](/cli/azure/disk#az-disk-create) Aşağıdaki örnek, *myDisk*adlı yönetilen diskten *myCopiedDisk* adlı bir disk oluşturur:

    ```azurecli
    az disk create --resource-group myResourceGroup \
         --name myCopiedDisk --source myDisk
    ``` 

1.  [Az disk listesini](/cli/azure/disk#az-disk-list)kullanarak kaynak grubunuzdaki yönetilen diskleri şimdi doğrulayın. Aşağıdaki örnekte *myResourceGroup*adlı kaynak grubunda yönetilen diskler listeleilmektedir:

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```


## <a name="set-up-a-virtual-network"></a>Sanal ağ kurma

Aşağıdaki isteğe bağlı adımlar yeni bir sanal ağ, alt ağ, genel IP adresi ve sanal ağ arabirimi kartı (NIC) oluşturur.

Sorun giderme amacıyla veya ek dağıtımlar için bir VM kopyalıyorsanız, varolan bir sanal ağda VM kullanmak istemeyebilirsiniz.

Kopyalanan VM'leriniz için sanal ağ altyapısı oluşturmak istiyorsanız, sonraki birkaç adımı izleyin. Sanal ağ oluşturmak istemiyorsanız, [VM oluşturmak](#create-a-vm)için atlayın.

1.  [Az ağ vnet oluşturarak](/cli/azure/network/vnet#az-network-vnet-create)sanal ağ oluşturun oluşturun. Aşağıdaki örnek, *myVnet* adında bir sanal ağ ve *mySubnet*adında bir alt ağ oluşturur:

    ```azurecli
    az network vnet create --resource-group myResourceGroup \
        --location eastus --name myVnet \
        --address-prefix 192.168.0.0/16 \
        --subnet-name mySubnet \
        --subnet-prefix 192.168.1.0/24
    ```

1.  [Az network public-ip create](/cli/azure/network/public-ip#az-network-public-ip-create)kullanarak ortak IP oluşturun. Aşağıdaki örnek, *mypublicdns*DNS adı ile *myPublicIP* adlı bir ortak IP oluşturur. (DNS adı benzersiz olması gerektiğinden, benzersiz bir ad sağlayın.)

    ```azurecli
    az network public-ip create --resource-group myResourceGroup \
        --location eastus --name myPublicIP --dns-name mypublicdns \
        --allocation-method static --idle-timeout 4
    ```

1.  Az network nic create kullanarak NIC [oluşturun.](/cli/azure/network/nic#az-network-nic-create)
    Aşağıdaki örnek, *mySubnet* alt ağına bağlı *myNic* adlı bir NIC oluşturur:

    ```azurecli
    az network nic create --resource-group myResourceGroup \
        --location eastus --name myNic \
        --vnet-name myVnet --subnet mySubnet \
        --public-ip-address myPublicIP
    ```

## <a name="create-a-vm"></a>VM oluşturma

[az vm oluştur](/cli/azure/vm#az-vm-create)kullanarak bir VM oluşturun.

Kopyalanan yönetilen diskin işletim sistemi diski olarak kullanılacağını belirtiniz (`--attach-os-disk`), aşağıdaki gibi:

```azurecli
az vm create --resource-group myResourceGroup \
    --name myCopiedVM --nics myNic \
    --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a>Sonraki adımlar

VM görüntülerini yönetmek için paylaşılan bir [resim galerisini](shared-images.md) nasıl kullanacağınızı öğrenmek için.
