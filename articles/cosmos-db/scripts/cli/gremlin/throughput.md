---
title: Azure Cosmos DB Gremlin API kaynakları için üretilen iş (RU/s) işlemleri için Azure CLı betikleri
description: Azure Cosmos DB Gremlin API kaynakları için üretilen iş (RU/s) işlemleri için Azure CLı betikleri
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 10/07/2020
ms.openlocfilehash: 3e4b912d086065f28c56fd4af309d373b811a8ec
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91838645"
---
# <a name="throughput-rus-operations-with-azure-cli-for-a-database-or-graph-for-azure-cosmos-db---gremlin-api"></a>Azure Cosmos DB-Gremlin API 'SI için bir veritabanı veya grafik için Azure CLı ile üretilen iş (RU/s) işlemleri

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.12.1 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu betik, paylaşılan aktarım hızı ve bir Gremlin grafiğine sahip bir Gremlin veritabanını, ayrılmış aktarım hızı ile oluşturur ve ardından hem veritabanı hem de grafik için üretilen işi günceller. Komut dosyası daha sonra standart 'den otomatik ölçeklendirme aktarım hızına geçirilir ve sonra otomatik ölçeklendirme üretilen işinin değerini geçiş yapıldıktan sonra okur.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/gremlin/throughput.sh "Throughput operations for a Gremlin database and graph.")]

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
| [az cosmosdb Gremlin veritabanı oluşturma](/cli/azure/cosmosdb/gremlin/database#az-cosmosdb-gremlin-database-create) | Azure Cosmos Gremlin veritabanı oluşturur. |
| [az cosmosdb Gremlin Graf Create](/cli/azure/cosmosdb/gremlin/graph#az-cosmosdb-gremlin-graph-create) | Azure Cosmos Gremlin grafiği oluşturur. |
| [az cosmosdb Gremlin veritabanı aktarım hızı güncelleştirmesi](/cli/azure/cosmosdb/gremlin/database/throughput#az-cosmosdb-gremlin-database-throughput-update) | Azure Cosmos Gremlin veritabanı için RU/sn 'yi güncelleştirin. |
| [az cosmosdb Gremlin grafik aktarım hızı güncelleştirmesi](/cli/azure/cosmosdb/gremlin/graph/throughput#az-cosmosdb-gremlin-graph-throughput-update) | Azure Cosmos Gremlin grafiğinde RU/s 'yi güncelleştirin. |
| [az cosmosdb Gremlin veritabanı aktarım hızı geçişi](/cli/azure/cosmosdb/gremlin/database/throughput#az_cosmosdb_gremlin_database_throughput_migrate) | Azure Cosmos Gremlin veritabanı için aktarım hızını geçirin. |
| [az cosmosdb Gremlin grafik işleme geçişi](/cli/azure/cosmosdb/gremlin/graph/throughput#az_cosmosdb_gremlin_graph_throughput_migrate) | Azure Cosmos Gremlin grafiğinde aktarım hızını geçirin. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

CLı Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
