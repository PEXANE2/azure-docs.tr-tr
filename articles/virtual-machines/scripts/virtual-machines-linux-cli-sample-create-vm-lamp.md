---
title: Azure CLı betik örneği-lamba yığınını yük dengeli bir sanal makine ölçek kümesinde dağıtma
description: LAMP Yığınını Azure’da yük dengeli bir sanal makine ölçek kümesine dağıtmak için özel bir betik uzantısı kullanın.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
ms.author: cynthn
tags: azure-service-management
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.custom: mvc, devx-track-azurecli
ms.date: 04/05/2017
ms.openlocfilehash: e71b2f940c637a8b1375be71f9b6ca95fd76628e
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87501874"
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>LAMP yığınını yük dengeli bir sanal makine ölçek kümesine dağıtma

Bu örnekte bir sanal makine ölçek kümesi oluşturulur ve LAMP yığınını ölçek kümesindeki her bir sanal makineye dağıtmak üzere özel betik çalıştıran bir uzantı uygulanır.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Bağlan

Sanal makinelerinize ve ölçek kümenize nasıl bağlanıldığını görmek için bu kodu kullanın.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, ölçek kümesini, VM’leri ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik bir kaynak grubu, sanal makine, kullanılabilirlik kümesi, yük dengeleyici ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az vmss create](/cli/azure/vmss) | Sanal makine ölçek kümesi oluşturur |
| [az network lb rule create](/cli/azure/network/lb/rule) | Yük dengeli uç nokta ekler |
| [az vmss extension set](/cli/azure/vmss/extension) | VM dağıtımında özel betik çalıştıran uzantıyı oluşturur |
| [az vmss update-instances](/cli/azure/vmss) | Ölçek kümesine uzantı uygulanmadan önce dağıtılmış VM örneklerinde özel betiği çalıştırır. |
| [az vmss scale](/cli/azure/vmss) | Daha fazla VM örneği ekleyerek ölçek kümesinin ölçeğini artırır. Özel betik, dağıtıldıklarında bu örnekler üzerinde çalıştırılır. |
| [az network public-ip list](/cli/azure/network/public-ip) | Örnek tarafından oluşturulan VM’lerin IP adreslerini alır. |
| [az network lb show](/cli/azure/network/lb) | Yük dengeleyici tarafından kullanılan ön uç ve arka uç bağlantı noktalarını alır. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
