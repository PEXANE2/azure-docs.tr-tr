---
title: Azure Cosmos DB Core (SQL) API veritabanı ve kapsayıcısı için kaynak kilidi oluşturma
description: Azure Cosmos DB Core (SQL) API veritabanı ve kapsayıcısı için kaynak kilidi oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: 21e20def0cf1e0890909fc86e361544a95791c18
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93102147"
---
# <a name="create-resource-lock-for-a-azure-cosmos-db-core-sql-api-database-and-container-using-azure-cli"></a>Azure CLı kullanarak Azure Cosmos DB Core (SQL) API veritabanı ve kapsayıcısı için kaynak kilidi oluşturma
[!INCLUDE[appliesto-sql-api](../../../includes/appliesto-sql-api.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.9.1 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

> [!IMPORTANT]
> Kaynak kilitleri, Cosmos DB hesabı etkinleştirilmiş özellik ile ilk kez kilitlenmediği takdirde, herhangi bir Cosmos DB SDK 'yı, hesap anahtarları aracılığıyla bağlanan araçları veya Azure portalını kullanarak bağlanan kullanıcılar tarafından yapılan değişiklikler için çalışmaz `disableKeyBasedMetadataWriteAccess` . Bu özelliğin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/sql/lock.sh "Create a resource lock for an Azure Cosmos DB Core (SQL) API database and container.")]

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
