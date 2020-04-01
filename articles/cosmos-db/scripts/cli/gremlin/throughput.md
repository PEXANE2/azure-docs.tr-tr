---
title: Gremlin veritabanı için RU/s'leri ve Azure Cosmos DB için grafiği güncelleştirme
description: Gremlin veritabanı için RU/s'leri ve Azure Cosmos DB için grafiği güncelleştirme
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: 0ef01a50e4827ec2ba96ce5d370bc2386e0dbfd6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71275524"
---
# <a name="update-rus-for-a-gremlin-database-and-graph-for-azure-cosmos-db-using-azure-cli"></a>Azure CLI kullanarak Gremlin veritabanı için RU/s'leri ve Azure Cosmos DB grafiğini güncelleştirin

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu konu Azure CLI sürümünü 2.0.73 veya daha yeni çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası paylaşılan iş ve özel iş girdisi ile bir Gremlin grafiği ile bir Gremlin veritabanı oluşturur, sonra veritabanı ve grafik hem de iş veri tabanı için iş verisini güncelleştirir.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/gremlin/throughput.sh "Update RU/s for a Gremlin database and graph.")]

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
| [az cosmosdb gremlin veritabanı oluşturmak](/cli/azure/cosmosdb/gremlin/database#az-cosmosdb-gremlin-database-create) | Azure Cosmos Gremlin veritabanı oluşturur. |
| [az cosmosdb gremlin grafik oluşturmak](/cli/azure/cosmosdb/gremlin/graph#az-cosmosdb-gremlin-graph-create) | Azure Cosmos Gremlin grafiği oluşturur. |
| [az cosmosdb gremlin veritabanı iş bilgi güncelleme](/cli/azure/cosmosdb/gremlin/database/throughput#az-cosmosdb-gremlin-database-throughput-update) | Azure Cosmos Gremlin veritabanı için RU/s'yi güncelleştirin. |
| [az cosmosdb gremlin grafik iğde güncelleme](/cli/azure/cosmosdb/gremlin/graph/throughput#az-cosmosdb-gremlin-graph-throughput-update) | Azure Cosmos Gremlin grafiği için RU/s'yi güncelleştirin. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB CLI hakkında daha fazla bilgi için [Azure Cosmos DB CLI belgelerine](/cli/azure/cosmosdb)bakın.

Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
