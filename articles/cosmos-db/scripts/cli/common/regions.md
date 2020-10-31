---
title: Bölge ekleyin, yük devretme önceliğini değiştirin, bir Azure Cosmos hesabı için yük devretmeyi tetikleyin
description: Bölge ekleyin, yük devretme önceliğini değiştirin, bir Azure Cosmos hesabı için yük devretmeyi tetikleyin
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: c8c8cbd81be07070ec5434ee49e42d81641bc969
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93083945"
---
# <a name="add-regions-change-failover-priority-trigger-failover-for-an-azure-cosmos-account-using-azure-cli"></a>Azure CLı kullanarak bölge ekleme, yük devretme önceliğini değiştirme, Azure Cosmos hesabı için yük devretmeyi tetikleme
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı altında, Azure CLı sürüm 2.9.1 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu betik üç işlemi gösterir.

- Mevcut bir Azure Cosmos hesabına bölge ekleyin.
- Bölgesel yük devretme önceliğini Değiştir (otomatik yük devretme kullanan hesaplar için geçerlidir)
- Birincil bilgisayardan ikincil bölgelere el ile yük devretme tetikleyin (el ile yük devretme içeren hesaplar için geçerlidir)

> [!NOTE]
> Diğer özellikleri değiştirirken Cosmos hesabında bölge ekleme ve kaldırma işlemleri yapılamaz.

> [!NOTE]
> Bu örnek, bir SQL (Core) API hesabı kullanmayı gösterir, ancak bu işlemler Cosmos DB tüm veritabanı API 'Lerinde aynıdır.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/regions.sh "Regional operations for Cosmos DB.")]

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
| [az cosmosdb update](/cli/azure/cosmosdb#az-cosmosdb-update) | Bir Azure Cosmos DB hesabını güncelleştirir (bölge Ekle veya Kaldır). |
| [az cosmosdb yük devretme-öncelik-değiştirme](/cli/azure/cosmosdb#az-cosmosdb-failover-priority-change) | Azure Cosmos DB hesapta yük devretme önceliğini güncelleştirin veya yük devretmeyi tetikleyin. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

CLı Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
