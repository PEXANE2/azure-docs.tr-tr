---
title: Azure CLı betik örneği-Batch 'te Windows havuzu
description: Bu betikte, Azure Batch’te Windows işlem düğümleri havuzu oluşturmaya ve yönetmeye yönelik Azure CLI’de kullanılabilir komutlardan bazıları gösterilir.
services: batch
documentationcenter: ''
author: ju-shim
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2019
ms.author: jushiman
ms.openlocfilehash: 2a692be38b1af610bfee755b68a5e9fc9bb96691
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029286"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>CLI örneği: Azure Batch’te bir Windows havuzu oluşturma ve yönetme

Bu betikte, Azure Batch’te Windows işlem düğümleri havuzu oluşturmaya ve yönetmeye yönelik Azure CLI’de kullanılabilir komutlardan bazıları gösterilir. Bir Windows havuzu iki şekilde yapılandırılabilir, bir Cloud Services yapılandırmasıyla veya Sanal Makine yapılandırmasıyla. Bu örnek, bir Windows havuzunu Cloud Services yapılandırmasıyla oluşturmayı gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Batch hesabını oluşturur. |
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az-batch-account-login) | Daha fazla CLI etkileşimi için belirtilen Batch hesabına karşı kimlik doğrulaması yapar. |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az-batch-pool-create) | İşlem düğümleri havuzu oluşturur.  |
| [az batch pool set](https://docs.microsoft.com/cli/azure/batch/pool#az-batch-pool-set) | Havuzun özelliklerini güncelleştirir.  |
| [az batch pool autoscale enable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-enable) | Bir havuzda otomatik ölçeklendirmeyi etkinleştirir ve bir formül uygular.  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az-batch-pool-show) | Havuzun özelliklerini görüntüler.  |
| [az batch pool autoscale disable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-disable) | Bir havuzda otomatik ölçeklendirmeyi devre dışı bırakır. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](https://docs.microsoft.com/cli/azure).
