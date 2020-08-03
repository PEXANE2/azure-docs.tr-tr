---
title: Azure CLı betik örneği-iç ve dış NSG ile iki VM oluşturma
description: Azure CLI Betik Örneği - İç ve dış NSG ile iki VM oluşturma
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: gwallace
tags: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 0d0e3dd53c592c991e342240b9c973e1677969d6
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87498387"
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Sanal makineler arasındaki ağ trafiğinin güvenliğini sağlama

Bu betik iki sanal makine oluşturur ve her ikisine de gelen trafiğin güvenliğini sağlar. Bir sanal makineye İnternet üzerinden erişilebilir ve sanal makinenin 3389 ve 80 numaralı bağlantı noktaları üzerinden trafiğe izin verecek şekilde yapılandırılmış bir ağ güvenlik gurubu (NSG) vardır. İkinci sanal makine Internet üzerinden erişilebilir değildir ve yalnızca birinci sanal makineden gelen trafiğe izin verecek şekilde yapılandırılmış bir NSG’ye sahiptir. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az network vnet create](/cli/azure/network/vnet) | Bir Azure sanal ağı ve alt ağ oluşturur. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet) | Bir alt ağ oluşturur. |
| [az vm create](/cli/azure/vm) | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve NSG’ye bağlar. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir.  |
| [az network nsg rule update](/cli/azure/network/nsg/rule) | Bir NSG kuralını güncelleştirir. Bu örnekte arka uç kuralı, trafiği yalnızca ön uç alt ağından geçirecek şekilde güncelleştirilir. |
| [az network nsg rule list](/cli/azure/network/nsg/rule) | Bir ağ güvenlik grubu kuralı hakkındaki bilgileri döndürür. Bu örnekte kural adı, daha sonra betikte kullanılmak üzere bir değişkende depolanır. |
| [az group delete](/cli/azure/vm/extension) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Windows VM belgeleri](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) içinde bulunabilir.
