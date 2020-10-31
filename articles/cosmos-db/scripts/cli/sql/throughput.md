---
title: Azure Cosmos DB Core (SQL) API kaynakları için üretilen iş (RU/s) işlemleri için Azure CLı betikleri
description: Azure Cosmos DB Core (SQL) API kaynakları için üretilen iş (RU/s) işlemleri için Azure CLı betikleri
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 10/07/2020
ms.openlocfilehash: e48a167327a83483b44d88fc73a0254df6f85c55
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93087358"
---
# <a name="throughput-rus-operations-with-azure-cli-for-a-database-or-container-for-azure-cosmos-db-core-sql-api"></a>Azure Cosmos DB Core (SQL) API 'sine yönelik bir veritabanı veya kapsayıcı için Azure CLı ile üretilen iş (RU/s) işlemleri
[!INCLUDE[appliesto-sql-api](../../../includes/appliesto-sql-api.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.12.1 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu betik, paylaşılan aktarım hızı ile bir çekirdek (SQL) API veritabanını ve adanmış işleme sahip bir çekirdek (SQL) API kapsayıcısını oluşturur, ardından hem veritabanı hem de kapsayıcı için aktarım hızını günceller. Komut dosyası daha sonra standart 'den otomatik ölçeklendirme aktarım hızına geçirilir ve sonra otomatik ölçeklendirme üretilen işinin değerini geçiş yapıldıktan sonra okur.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/sql/throughput.sh "Throughput operations for a SQL database and container.")]

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
| [az cosmosdb SQL veritabanı oluşturma](/cli/azure/cosmosdb/sql/database#az-cosmosdb-sql-database-create) | Azure Cosmos Core (SQL) veritabanı oluşturur. |
| [az cosmosdb SQL kapsayıcı oluşturma](/cli/azure/cosmosdb/sql/container#az-cosmosdb-sql-container-create) | Bir Azure Cosmos Core (SQL) kapsayıcısı oluşturur. |
| [az cosmosdb SQL veritabanı aktarım hızı güncelleştirmesi](/cli/azure/cosmosdb/sql/database/throughput#az-cosmosdb-sql-database-throughput-update) | Azure Cosmos Core (SQL) veritabanı için üretilen işi güncelleştirin. |
| [az cosmosdb SQL Container üretilen iş güncelleştirmesi](/cli/azure/cosmosdb/sql/container/throughput#az-cosmosdb-sql-container-throughput-update) | Azure Cosmos Core (SQL) kapsayıcısının aktarım hızını güncelleştirin. |
| [az cosmosdb SQL veritabanı aktarım hızı geçişi](/cli/azure/cosmosdb/sql/database/throughput#az-cosmosdb-sql-database-throughput-migrate) | Azure Cosmos Core (SQL) veritabanı için aktarım hızını geçirin. |
| [az cosmosdb SQL Container verimlilik geçişi](/cli/azure/cosmosdb/sql/container/throughput#az-cosmosdb-sql-container-throughput-migrate) | Azure Cosmos Core (SQL) kapsayıcısı için aktarım hızını geçirin. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

CLı Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
