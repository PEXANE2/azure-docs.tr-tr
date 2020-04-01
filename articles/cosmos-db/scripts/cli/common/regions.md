---
title: Azure Cosmos hesabı için bölge ekleme, öncelik değiştirme, başarısızlık
description: Azure Cosmos hesabı için bölge ekleme, öncelik değiştirme, başarısızlık
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: b7b6be0ce781debcb19b5c0fb7b6a4b0123ef366
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71275552"
---
# <a name="add-regions-change-failover-priority-trigger-failover-for-an-azure-cosmos-account-using-azure-cli"></a>Azure CLI kullanarak bir Azure Cosmos hesabı için bölgeleri ekleme, hata önceliği değiştirme, azure cosmos hesabı için başarısızlık tetikleme

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu konu Azure CLI sürümünü 2.0.73 veya daha yeni çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası üç işlemi gösterir.

- Varolan bir Azure Cosmos hesabına bölge ekleyin.
- Bölgesel başarısız önceliği değiştirme (otomatik hata kullanan hesaplar için geçerlidir)
- Birincil den ikincil bölgelere manuel arıza yı tetikleme (manuel başarısız olan hesaplar için geçerlidir)

> [!NOTE]
> Cosmos hesabındaki bölge işlemlerini ekleme ve kaldırma işlemi diğer özellikleri değiştirirken yapılamaz.

> [!NOTE]
> Bu örnek bir SQL (Core) API hesabı kullanarak gösterir, ancak bu işlemler Cosmos DB'deki tüm veritabanı API'lerinde aynıdır.

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
| [az cosmosdb update](/cli/azure/cosmosdb#az-cosmosdb-update) | Azure Cosmos DB hesabını güncelleştirir (bölge ekleyin veya kaldırın). |
| [az cosmosdb failover-öncelik-değişim](/cli/azure/cosmosdb#az-cosmosdb-failover-priority-change) | Azure Cosmos DB hesabında başarısız önceliği güncelleştirin veya başarısızlığı tetikleyin. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB CLI hakkında daha fazla bilgi için [Azure Cosmos DB CLI belgelerine](/cli/azure/cosmosdb)bakın.

Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
