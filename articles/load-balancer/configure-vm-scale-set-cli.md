---
title: Mevcut bir Azure Yük Bakiyesi ile ayarlanan sanal makine ölçeğini yapılandırın - Azure CLI
description: Varolan bir Azure Yük Dengeleyicisi ile sanal makine ölçeği kümesini nasıl yapılandıracağımız öğrenin.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: article
ms.date: 03/25/2020
ms.openlocfilehash: a7f44a21dd404c556d6f3d8444fa70583cd71c57
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349744"
---
# <a name="configure-a-virtual-machine-scale-set-with-an-existing-azure-load-balancer-using-the-azure-cli"></a>Azure CLI'yi kullanarak mevcut bir Azure Yük Dengeleyicisiyle sanal makine ölçeği kümesini yapılandırın

Bu makalede, varolan bir Azure Yük Dengeleyicisi ile ayarlanmış sanal makine ölçeğini nasıl yapılandıracağınız öğrenilir. 

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği.
- Sanal makine ölçeği kümesinin dağıtılanacağı abonelikteki mevcut standart sku yük dengeleyicisi.
- Sanal makine ölçeği kümesi için bir Azure Sanal Ağı.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

CLI'yi yerel olarak kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.28 veya daha sonra yüklenen bir sürümün olması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma

Azure'da oturum açın.

```azurecli-interactive
az login
```

## <a name="deploy-a-virtual-machine-scale-set-with-existing-load-balancer"></a>Mevcut yük dengeleyicisi ile ayarlanmış sanal makine ölçeğini dağıtma

Parantez içinde değerleri yapılandırmanızdaki kaynakların adlarıyla değiştirin.

```azurecli-interactive
az vmss create \
    --resource-group <resource-group> \
    --name <vmss-name>\
    --image <your-image> \
    --admin-username <admin-username> \
    --generate-ssh-keys  \
    --upgrade-policy-mode Automatic \
    --instance-count 3 \
    --vnet-name <virtual-network-name> \
    --subnet <subnet-name> \
    --lb <load-balancer-name> \
    --backend-pool-name <backend-pool-name>
```

Aşağıdaki örnekte, aşağıdakilerle ayarlanmış sanal bir makine ölçeği dağıtılır:

- **MyVMSS** adlı sanal makine ölçeği kümesi
- Azure Yük **DengeleyicimyLoadBalancer** adlı
- **MyBackendPool** adlı yük dengeleyici arka uç havuzu
- Azure Sanal Ağı **myVnet** adlı
- Subnet **mySubnet** adlı
- **myResourceGroup** adlı kaynak grubu
- Sanal makine ölçek kümesi için Ubuntu Server görüntüsü

```azurecli-interactive
az vmss create \
    --resource-group myResourceGroup \
    --name myVMSS \
    --image Canonical:UbuntuServer:18.04-LTS:latest \
    --admin-username adminuser \
    --generate-ssh-keys \
    --upgrade-policy-mode Automatic \
    --instance-count 3 \
    --vnet-name myVnet\
    --subnet mySubnet \
    --lb myLoadBalancer \
    --backend-pool-name myBackendPool
```
> [!NOTE]
> Ölçek kümesi oluşturulduktan sonra, arka uç bağlantı noktası yük dengeleyicisinin sistem durumu sondası tarafından kullanılan bir yük dengeleme kuralı için değiştirilemez. Bağlantı noktasını değiştirmek için, Azure sanal makine ölçeği kümesini güncelleştirerek sistem durumu sondasını kaldırabilir, bağlantı noktasını güncelleyebilir ve sistem durumu sondasını yeniden yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, varolan bir Azure Yük Dengeleyicisi ile ayarlanmış sanal bir makine ölçeği dağıttınız.  Sanal makine ölçek kümeleri ve yük dengeleyicisi hakkında daha fazla bilgi edinmek için bkz:

- [Azure Load Balancer nedir?](load-balancer-overview.md)
- [Sanal makine ölçek kümeleri nedir?](../virtual-machine-scale-sets/overview.md)
                                