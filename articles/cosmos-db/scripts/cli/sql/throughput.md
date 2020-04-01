---
title: SQL (Core) API veritabanı ve Azure Cosmos DB için kapsayıcı için RU/ları güncelleştirme
description: SQL (Core) API veritabanı ve Azure Cosmos DB için kapsayıcı için RU/ları güncelleştirme
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: cde0615a5312372992d2604f32809a983c248a2b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71275398"
---
# <a name="update-rus-for-a-sql-core-api-database-and-container-for-azure-cosmos-db-using-azure-cli"></a>Azure CLI kullanarak SQL (Core) API veritabanı ve Azure Cosmos DB için kapsayıcı için RU/lar'ı güncelleştirin

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu konu Azure CLI sürümünü 2.0.73 veya daha yeni çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası, paylaşılan iş tamına sahip bir SQL (Core) API veritabanı ve özel iş başına sahip bir SQL (Core) API kapsayıcısı oluşturur ve ardından hem veritabanı hem de kapsayıcı için iş verisini güncelleştirir.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/sql/throughput.sh "Update RU/s for a SQL database and container.")]

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
| [az cosmosdb sql veritabanı oluşturmak](/cli/azure/cosmosdb/sql/database#az-cosmosdb-sql-database-create) | Azure Cosmos SQL (Core) veritabanı oluşturur. |
| [az cosmosdb sql konteyner oluşturmak](/cli/azure/cosmosdb/sql/container#az-cosmosdb-sql-container-create) | Bir Azure Cosmos SQL (Core) kapsayıcısı oluşturur. |
| [az cosmosdb sql veritabanı iş bilgi güncelleme](/cli/azure/cosmosdb/sql/database/throughput#az-cosmosdb-sql-database-throughput-update) | Azure Cosmos SQL (Core) veritabanı için RU/s'yi güncelleştirin. |
| [az cosmosdb sql konteyner iş tartışımı](/cli/azure/cosmosdb/sql/container/throughput#az-cosmosdb-sql-container-throughput-update) | Azure Cosmos SQL (Core) kapsayıcısı için RU/s'yi güncelleştirin. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB CLI hakkında daha fazla bilgi için [Azure Cosmos DB CLI belgelerine](/cli/azure/cosmosdb)bakın.

Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
