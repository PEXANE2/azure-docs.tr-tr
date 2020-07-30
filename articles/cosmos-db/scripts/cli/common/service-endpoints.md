---
title: Sanal ağ hizmeti uç noktaları ile bir Azure Cosmos hesabı oluşturma
description: Sanal ağ hizmeti uç noktaları ile bir Azure Cosmos hesabı oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 4d1a56c80cab58e98121ae35c98a086d16dfe02b
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87432233"
---
# <a name="create-an-azure-cosmos-account-with-virtual-network-service-endpoints-using-azure-cli"></a>Azure CLı kullanarak sanal ağ hizmet uç noktaları ile bir Azure Cosmos hesabı oluşturma

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz, bu komut Azure CLı sürüm 2.9.1 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu örnek, ön ve arka uç alt ağı olan yeni bir sanal ağ oluşturur ve hizmet uç noktalarını etkinleştirilir `Microsoft.AzureCosmosDB` . Daha sonra bu alt ağın kaynak KIMLIĞINI alır ve Azure Cosmos hesabına uygular ve hesap için hizmet uç noktalarını sağlar.

> [!NOTE]
> Bu örnekte, çekirdek (SQL) API hesabı kullanılması gösterilmektedir. Bu örneği diğer API 'Ler için kullanmak üzere `enable-virtual-network` `virtual-network-rules` API 'ye özel betiğe aşağıdaki komut dosyasına ve parametrelerini uygulayın.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/service-endpoints.sh "Create an Azure Cosmos account with service endpoints.")]

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
| [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create) | Bir Azure sanal ağı oluşturur. |
| [az network vnet subnet create](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-create) | Azure sanal ağı için bir alt ağ oluşturur. |
| [az Network VNET subnet Show](/cli/azure/network/vnet/subnet#az-network-vnet-subnet-show) | Bir Azure sanal ağı için bir alt ağ döndürür. |
| [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) | Azure Cosmos DB hesabı oluşturur. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

CLı Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
