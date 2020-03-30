---
title: Cassandra keyspace için RU/s'leri ve Azure Cosmos DB için tabloyu güncelleştirin
description: Cassandra için RU/s'yi Azure Cosmos DB için bir anahtar alanı ve tabloolarak güncelleştirin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: 3615157543d826050b8adf0e6ae59bf62ad9cb1b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71827312"
---
# <a name="update-rus-for-a-cassandra-keyspace-and-table-using-azure-cli"></a>Azure CLI'yi kullanarak Cassandra anahtar alanı ve tablo için RU/lar'ı güncelleştirin

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu konu Azure CLI sürümünü 2.0.73 veya daha yeni çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası paylaşılan iş tamına sahip bir Cassandra anahtar alanı ve özel iş paylaşımına sahip bir Cassandra tablosu oluşturur, ardından hem anahtar alanı hem de tablo için iş bilgisini günceller.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/cassandra/throughput.sh "Update RU/s for Cassandra keyspace and table.")]

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
| [az cosmosdb cassandra keyspace oluşturmak](/cli/azure/cosmosdb/cassandra/keyspace#az-cosmosdb-cassandra-keyspace-create) | Azure Cosmos Cassandra anahtar alanı oluşturur. |
| [az cosmosdb cassandra tablo oluşturmak](/cli/azure/cosmosdb/cassandra/table#az-cosmosdb-cassandra-table-create) | Azure Cosmos Cassandra tablosu oluşturur. |
| [az cosmosdb cassandra keyspace iş yeri güncelleme](/cli/azure/cosmosdb/cassandra/keyspace/throughput?view=azure-cli-latest#az-cosmosdb-cassandra-keyspace-throughput-update) | Azure Cosmos Cassandra anahtar alanı için RU/s'yi güncelleştirin. |
| [az cosmosdb cassandra tablo iş güncelleme](/cli/azure/cosmosdb/cassandra/table/throughput#az-cosmosdb-cassandra-table-throughput-update) | Azure Cosmos Cassandra tablosu için RU/s'yi güncelleştirin. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB CLI hakkında daha fazla bilgi için [Azure Cosmos DB CLI belgelerine](/cli/azure/cosmosdb)bakın.

Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
