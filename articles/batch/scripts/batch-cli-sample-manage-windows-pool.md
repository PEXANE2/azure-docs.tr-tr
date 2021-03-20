---
title: Azure CLı betik örneği-Batch 'te Windows havuzu
description: Bu betikte, Azure Batch’te Windows işlem düğümleri havuzu oluşturmaya ve yönetmeye yönelik Azure CLI’de kullanılabilir komutlardan bazıları gösterilir.
ms.topic: sample
ms.date: 12/12/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: fb18f9d8777c17d31a3ab246603df0d9fa162467
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93100948"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>CLI örneği: Azure Batch’te bir Windows havuzu oluşturma ve yönetme

Bu betikte, Azure Batch’te Windows işlem düğümleri havuzu oluşturmaya ve yönetmeye yönelik Azure CLI’de kullanılabilir komutlardan bazıları gösterilir. Bir Windows havuzu iki şekilde yapılandırılabilir, bir Cloud Services yapılandırmasıyla veya Sanal Makine yapılandırmasıyla. Bu örnek, bir Windows havuzunu Cloud Services yapılandırmasıyla oluşturmayı gösterir.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Bu öğretici, Azure CLı 'nin sürüm 2.0.20 veya üstünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür. 

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
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Daha fazla CLI etkileşimi için belirtilen Batch hesabına karşı kimlik doğrulaması yapar. |
| [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create) | İşlem düğümleri havuzu oluşturur.  |
| [az batch pool set](/cli/azure/batch/pool#az-batch-pool-set) | Havuzun özelliklerini güncelleştirir.  |
| [az batch pool autoscale enable](/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-enable) | Bir havuzda otomatik ölçeklendirmeyi etkinleştirir ve bir formül uygular.  |
| [az batch pool show](/cli/azure/batch/pool#az-batch-pool-show) | Havuzun özelliklerini görüntüler.  |
| [az batch pool autoscale disable](/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-disable) | Bir havuzda otomatik ölçeklendirmeyi devre dışı bırakır. |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |


## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
