---
title: Azure Cosmos DB için MongoDB API veritabanı ve koleksiyonu için RU/s'leri güncelleştirin
description: Azure Cosmos DB için MongoDB API veritabanı ve koleksiyonu için RU/s'leri güncelleştirin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: 20516a66fe664e415b97e40beacd77c34c7ccaea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71275510"
---
# <a name="update-rus-for-a-database-and-collection-for-mongodb-api-for-azure-cosmos-db-using-azure-cli"></a>Azure CLI kullanarak Azure Cosmos DB için MongoDB API veritabanı ve koleksiyonu için RU/ları güncelleştirin

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu konu Azure CLI sürümünü 2.0.73 veya daha yeni çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası, mongoDB API için Azure Cosmos DB için özel iş ortalığına sahip paylaşılan iş ve koleksiyona sahip bir veritabanı oluşturur ve ardından hem veritabanı hem de koleksiyon için iş verisini güncelleştirir.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/mongodb/throughput.sh "Update RU/s for an Azure Cosmos DB MongoDB API database and collection.")]

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
| [az cosmosdb mongodb veritabanı oluşturmak](/cli/azure/cosmosdb/mongodb/database#az-cosmosdb-mongodb-database-create) | Azure Cosmos MongoDB API veritabanı oluşturur. |
| [az cosmosdb mongodb koleksiyonu oluşturmak](/cli/azure/cosmosdb/mongodb/collection#az-cosmosdb-mongodb-collection-create) | Azure Cosmos MongoDB API koleksiyonu oluşturur. |
| [az cosmosdb mongodb veritabanı iş bilgi güncelleştirmesi](/cli/azure/cosmosdb/mongodb/database/throughput#az-cosmosdb-mongodb-database-throughput-update) | Azure Cosmos MongoDB API veritabanı için RUs'ları güncelleştirin. |
| [az cosmosdb mongodb toplama iş güncelleme](/cli/azure/cosmosdb/mongodb/collection/throughput#az-cosmosdb-mongodb-collection-throughput-update) | Azure Cosmos MongoDB API koleksiyonu için RUs'ları güncelleştirin. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB CLI hakkında daha fazla bilgi için [Azure Cosmos DB CLI belgelerine](/cli/azure/cosmosdb)bakın.

Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
