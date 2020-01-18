---
title: Azure CLı-laboratuvarda sanal makine oluşturma ve doğrulama
description: Bu Azure CLI betiği, laboratuvarda bir sanal makine oluşturur ve bunun kullanılabilir olduğunu doğrular.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2020
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: 767d1f3a504e91783e37d8ff1c1b97f62816af3b
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76169270"
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
| [az Lab VM Create](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-create) | Laboratuvardaki bir sanal makineyi (VM) oluşturur. |
| [az lab vm show](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-show) | Bir laboratuvardaki sanal makinenin durumunu görüntüler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek Azure Lab Services CLI betik örnekleri, [Azure Lab Services CLI örnekleri](../samples-cli.md) içinde bulunabilir.
