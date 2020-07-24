---
title: Azure Cosmos DB için Cassandra anahtar alanı ve tablo için kaynak kilidi oluşturma
description: Azure Cosmos DB için Cassandra anahtar alanı ve tablo için kaynak kilidi oluşturma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: sample
ms.date: 06/03/2020
ms.openlocfilehash: 5683eece3f6dc1c63be27ce3c98664e972b8d7c6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87086753"
---
# <a name="create-a-resource-lock-for-azure-cosmos-cassandra-api-keyspace-and-table-using-azure-cli"></a>Azure CLI kullanarak Azure Cosmos Cassandra API anahtar alanı ve tablo için bir kaynak kilidi oluşturma

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.6.0 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

> [!IMPORTANT]
> Kaynak kilitleri, hiçbir Cassandra SDK 'Sı, CQL kabuğu veya Azure portalını kullanarak bağlanan kullanıcılar tarafından yapılan değişiklikler için çalışmaz, çünkü Cosmos DB hesap ilk olarak etkin özellik ile kilitlenmez `disableKeyBasedMetadataWriteAccess` . Bu özelliğin nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [SDK 'lardan değişiklikleri engellemeyi](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/cassandra/lock.sh "Create a resource lock for an Azure Cosmos DB Cassandra API keyspace, and table.")]

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
