---
title: Azure CLI Script Örneği - Azure Monitor ile Linux VM oluşturma
description: Azure CLI Script Örneği - Azure Monitor ile Linux VM oluşturma
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 211d81b2ceec985844ed7abf7b8fc4d3e30dcca5
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459942"
---
# <a name="monitor-a-vm-with-azure-monitor"></a>Azure Monitör ile Bir VM'yi izleme

Bu betik bir Azure Sanal Makinesi oluşturur, Log Analytics aracısını yükler ve sistemi bir Log Analytics çalışma alanına kaydeder. Betik çalıştıktan sonra sanal makine konsolda görünür.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, VM’yi ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, bir kaynak grubu, sanal makine ve tüm ilgili kaynakları oluşturmak için aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | Sanal makine oluşturur ve ağ kartına, sanal ağa, alt ağa ve NSG’ye bağlar. Bu komut ayrıca kullanılacak sanal makine görüntüsünü ve yönetici kimlik bilgilerini belirtir.  |
| [azure vm extension set](https://docs.microsoft.com/cli/azure/vm/extension) | Bir sanal makineye karşı VM uzantısı çalıştırır. Bu durumda, Azure Monitor aracısı uzantısı Log Analytics aracısını yüklemek ve VM'yi bir Log Analytics çalışma alanına kaydetmek için kullanılır. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).

Ek sanal makine CLI betiği örnekleri, [Azure Linux VM belgeleri](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) içinde bulunabilir.
