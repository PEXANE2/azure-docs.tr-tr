---
title: Azure CLı-laboratuvarda sanal makine oluşturma ve doğrulama
description: Bu Azure CLI betiği, laboratuvarda bir sanal makine oluşturur ve bunun kullanılabilir olduğunu doğrular.
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/11/2020
ms.openlocfilehash: c7625f62d7897d61903f864b216ccf9aa13648ea
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102198430"
---
# <a name="use-azure-cli-to-create-and-verify-availability-of-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Azure CLI kullanarak Azure DevTest Labs’deki laboratuvarda bir sanal makine oluşturma ve bu sanal makinenin kullanılabilirliğini doğrulama

Bu Azure CLI betiği, laboratuvarda bir sanal makine (VM) oluşturur. Ssh kimlik doğrulaması ile market görüntüsüne göre sanal makine oluşturulur. Daha sonra betik, sanal makinenin kullanılabilir olduğunu doğrular. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/create-verify-virtual-machine-in-lab/create-verify-virtual-machine-in-lab.sh "Create and verify availability of a VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az Lab VM Create](/cli/azure/lab/vm#az-lab-vm-create) | Laboratuvardaki bir sanal makineyi (VM) oluşturur. |
| [az lab vm show](/cli/azure/lab/vm#az-lab-vm-show) | Bir laboratuvardaki sanal makinenin durumunu görüntüler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).

Ek Azure Lab Services CLI betik örnekleri, [Azure Lab Services CLI örnekleri](../samples-cli.md) içinde bulunabilir.
