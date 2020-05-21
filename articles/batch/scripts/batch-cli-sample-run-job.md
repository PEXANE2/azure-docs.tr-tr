---
title: Azure CLı betik örneği-Batch işi çalıştırma
description: Bu betik, bir Batch işi oluşturur ve bu işe bir dizi görev ekler. Ayrıca bir işi ve görevlerini izlemeyi gösterir.
ms.topic: sample
ms.date: 12/12/2019
ms.openlocfilehash: ce3c06b4631095df54371ca2417dc45d493a437d
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83681832"
---
# <a name="cli-example-run-a-job-and-tasks-with-azure-batch"></a>CLI örneği: Azure Batch ile bir işi ve görevleri çalıştırma

Bu betik, bir Batch işi oluşturur ve bu işe bir dizi görev ekler. Ayrıca bir işi ve görevlerini izlemeyi gösterir. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch/run-job/run-job.sh "Run Job")]

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
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Daha fazla CLI etkileşimi için belirtilen Batch hesabına karşı kimlik doğrulaması yapar.  |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az-batch-pool-create) | İşlem düğümleri havuzu oluşturur.  |
| [az batch job create](https://docs.microsoft.com/cli/azure/batch/job#az-batch-job-create) | Batch işi oluşturur.  |
| [az batch task create](https://docs.microsoft.com/cli/azure/batch/task#az-batch-task-create) | Belirtilen Batch işine bir görev ekler.  |
| [az batch job set](https://docs.microsoft.com/cli/azure/batch/job#az-batch-job-set) | Bir Batch işinin özelliklerini güncelleştirir.  |
| [az batch job show](https://docs.microsoft.com/cli/azure/batch/job#az-batch-job-show) | Belirtilen Batch işinin ayrıntılarını alır.  |
| [az batch task show](https://docs.microsoft.com/cli/azure/batch/task#az-batch-task-show) | Belirtilen Batch işinden bir görevin ayrıntılarını alır.  |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).