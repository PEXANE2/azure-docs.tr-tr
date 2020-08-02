---
title: Azure CLı betik örneği-Batch 'te Linux havuzu
description: Bu betik, Azure Batch ' de bir Linux işlem düğümü havuzu oluşturmak ve yönetmek için Azure CLı 'de mevcut olan komutlardan bazılarını gösterir.
ms.topic: sample
ms.date: 01/29/2018
ms.custom: devx-track-azurecli
ms.openlocfilehash: 7aba6e64fdf39a069eb010ef032fcc5391fd47ee
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87494400"
---
# <a name="cli-example-create-and-manage-a-linux-pool-in-azure-batch"></a>CLI örneği: Azure Batch’te bir Linux havuzu oluşturma ve yönetme

Bu betik, Azure Batch ' de bir Linux işlem düğümü havuzu oluşturmak ve yönetmek için Azure CLı 'de mevcut olan komutlardan bazılarını gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.20 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-linux.sh "Manage Linux Virtual Machine Pool")]

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
| [az batch pool node-agent-skus list](../batch-linux-nodes.md#list-of-virtual-machine-images) | Kullanılabilir düğüm aracısı SKU’larını ve görüntü bilgilerini listeler.  |
| [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create) | İşlem düğümleri havuzu oluşturur.  |
| [az batch pool resize](/cli/azure/batch/pool#az-batch-pool-resize) | Belirtilen havuzda çalışan VM’lerin sayısını yeniden boyutlandırır.  |
| [az batch pool show](/cli/azure/batch/pool#az-batch-pool-show) | Havuzun özelliklerini görüntüler.  |
| [az batch node list](/cli/azure/batch/node#az-batch-node-list) | Belirtilen havuzdaki işlem düğümlerinin tümünü listeler.  |
| [az batch node reboot](/cli/azure/batch/node#az-batch-node-reboot) | Belirtilen işlem düğümünü yeniden başlatır.  |
| [az batch node delete](/cli/azure/batch/node#az-batch-node-delete) | Listelenen düğümleri belirtilen havuzdan siler.  |
| [az group delete](/cli/azure/group#az-group-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz. [Azure CLI belgeleri](/cli/azure).
