---
title: IP güvenlik duvarı yla Bir Azure Cosmos hesabı oluşturma
description: IP güvenlik duvarı yla Bir Azure Cosmos hesabı oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: 8b0ab04e2117108566962cc9a473556098e0e51d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71275594"
---
# <a name="create-an-azure-cosmos-account-with-ip-firewall-using-azure-cli"></a>Azure CLI'yi kullanarak IP güvenlik duvarıyla bir Azure Cosmos hesabı oluşturma

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu konu Azure CLI sürümünü 2.0.73 veya daha yeni çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

> [!NOTE]
> Bu örnek, bir SQL (Core) API hesabı nın kullanılmasını gösterir. Bu örneği diğer API'ler `ip-range-filter` için kullanmak için, `az cosmosdb account create` aşağıdaki komut dosyasındaki parametreyi API'nize özgü komut dosyanız için uygulayın.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/ipfirewall.sh "Create an Azure Cosmos account with ip firewall.")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Betik örneği çalıştırıldıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komut kullanılabilir.

```azurecli-interactive
az group delete --name $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB hesabı oluşturur. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB CLI hakkında daha fazla bilgi için [Azure Cosmos DB CLI belgelerine](/cli/azure/cosmosdb)bakın.

Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
