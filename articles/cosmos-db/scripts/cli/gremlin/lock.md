---
title: Gremlin veritabanı için kaynak kilidi oluşturma ve Azure Cosmos DB için grafik oluşturma
description: Gremlin veritabanı için kaynak kilidi oluşturma ve Azure Cosmos DB için grafik oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: sample
ms.date: 06/03/2020
ms.openlocfilehash: e4aba05df23ce4048ba711680086407cb764a24a
ms.sourcegitcommit: 5504d5a88896c692303b9c676a7d2860f36394c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84514727"
---
# <a name="create-a-resource-lock-for-azure-cosmos-gremlin-api-database-and-graph-using-azure-cli"></a>Azure CLı kullanarak Azure Cosmos Gremlin API veritabanı ve grafik için bir kaynak kilidi oluşturma

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

> [!IMPORTANT]
> Kaynak kilitleri, Cosmos DB hesap ilk olarak etkinleştirilmiş özelliği ile kilitlenmediği takdirde, herhangi bir Gremlin SDK veya Azure portalını kullanarak bağlanan kullanıcılar tarafından yapılan değişiklikler için çalışmaz `disableKeyBasedMetadataWriteAccess` . Bu özelliğin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](../../../role-based-access-control.md#preventing-changes-from-cosmos-sdk).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/gremlin/lock.sh "Create a resource lock for an Azure Cosmos DB Gremlin API database and graph.")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [az kilitle oluştur](/cli/azure/lock#az-lock-create) | Bir kilit oluşturur. |
| [az Lock List](/cli/azure/lock#az-lock-list) | Kilit bilgilerini listeleyin. |
| [az Lock Show](/cli/azure/lock#az-lock-show) | Kilidin özelliklerini göster. |
| [az Lock Delete](/cli/azure/lock#az-lock-delete) | Bir kilidi siler. |

## <a name="next-steps"></a>Sonraki adımlar

-[Beklenmeyen değişiklikleri engellemek için kaynakları kilitle](../../../../azure-resource-manager/management/lock-resources.md)

-[Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

-[Azure Cosmos DB CLI GitHub deposu](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb).
