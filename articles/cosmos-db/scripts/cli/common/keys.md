---
title: Azure Cosmos hesabı için hesap anahtarlarıyla ve bağlantı dizeleriyle çalışma
description: Azure Cosmos hesabı için hesap anahtarlarıyla ve bağlantı dizeleriyle çalışma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/29/2020
ms.openlocfilehash: af5f92168e6bcc23345e3a11727eefe76615422c
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94563265"
---
# <a name="work-with-account-keys-and-connection-strings-for-an-azure-cosmos-account-using-azure-cli"></a>Azure CLı kullanarak bir Azure Cosmos hesabı için hesap anahtarlarıyla ve bağlantı dizeleriyle çalışma
[!INCLUDE[appliesto-all-apis](../../../includes/appliesto-all-apis.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../../includes/azure-cli-prepare-your-environment.md)]

- Bu makale, Azure CLı 'nin sürüm 2.9.1 veya üstünü gerektirir. Azure Cloud Shell kullanılıyorsa, en son sürüm zaten yüklüdür.

## <a name="sample-script"></a>Örnek betik

Bu betik dört işlemi gösterir.

- Tüm hesap anahtarlarını Listele
- Salt okuma hesap anahtarlarını Listele
- Bağlantı dizelerini listeleme
- Hesap anahtarlarını yeniden oluşturma

> [!NOTE]
> Bu örnekte bir SQL (Core) API hesabı kullanılması gösterilmektedir, ancak hesap anahtarı ve bağlantı dizesi işlemleri Cosmos DB içindeki tüm veritabanı API 'Lerinde aynıdır.

[!code-azurecli-interactive[main](../../../../../cli_scripts/cosmosdb/common/keys.sh "Keys and connection string operations for Cosmos DB.")]

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
| [az cosmosdb anahtarları listesi](/cli/azure/cosmosdb/keys#az-cosmosdb-keys-list) | Azure Cosmos DB hesabın anahtarlarını listeleyin. |
| [az cosmosdb List-salt-Read-Keys](/cli/azure/cosmosdb#az-cosmosdb-list-read-only-keys) | Azure Cosmos DB hesabının salt okuma anahtarlarını listeleyin. |
| [az cosmosdb list-connection-strings](/cli/azure/cosmosdb#az-cosmosdb-list-connection-strings) | Azure Cosmos DB hesabın bağlantı dizelerini listeleyin. |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb#az-cosmosdb-regenerate-key) | Azure Cosmos DB hesap için anahtarları yeniden oluşturun. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

CLı Azure Cosmos DB hakkında daha fazla bilgi için bkz. [Azure Cosmos DB CLI belgeleri](/cli/azure/cosmosdb).

Tüm Azure Cosmos DB CLı betiği örnekleri [Azure Cosmos DB CLI GitHub deposunda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
