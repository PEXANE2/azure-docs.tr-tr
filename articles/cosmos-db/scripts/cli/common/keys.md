---
title: Azure Cosmos hesabı için hesap anahtarları ve bağlantı dizeleri ile çalışma
description: Azure Cosmos hesabı için hesap anahtarları ve bağlantı dizeleri ile çalışma
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 9/25/2019
ms.openlocfilehash: e766f52d729a4f916eefd2b148d926d929b4f540
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71275559"
---
# <a name="work-with-account-keys-and-connection-strings-for-an-azure-cosmos-account-using-azure-cli"></a>Azure CLI'yi kullanarak Bir Azure Cosmos hesabı için hesap anahtarları ve bağlantı dizeleri ile çalışma

[!INCLUDE [cloud-shell-try-it.md](../../../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu konu Azure CLI sürümünü 2.0.73 veya daha yeni çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="sample-script"></a>Örnek betik

Bu komut dosyası dört işlemi gösterir.

- Tüm hesap anahtarlarını listele
- Yalnızca hesap anahtarlarını okuma listesi
- Bağlantı dizelerini listele
- Hesap anahtarlarını yeniden oluşturma

> [!NOTE]
> Bu örnek bir SQL (Core) API hesabı kullanılarak gösterilir, ancak hesap anahtarı ve bağlantı dize işlemleri Cosmos DB'deki tüm veritabanı API'lerinde aynıdır.

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
| [az cosmosdb tuşları listesi](/cli/azure/cosmosdb/keys#az-cosmosdb-keys-list) | Azure Cosmos DB hesabının anahtarlarını listele. |
| [az cosmosdb listesi-salt okunur-tuşları](/cli/azure/cosmosdb#az-cosmosdb-list-read-only-keys) | Azure Cosmos DB hesabıiçin yalnızca okunan anahtarları listele. |
| [az cosmosdb list-connection-strings](/cli/azure/cosmosdb#az-cosmosdb-list-connection-strings) | Azure Cosmos DB hesabının bağlantı dizelerini listele. |
| [az cosmosdb regenerate-key](/cli/azure/cosmosdb#az-cosmosdb-regenerate-key) | Azure Cosmos DB hesabının anahtarlarını yeniden oluşturun. |
| [az group delete](/cli/azure/resource#az-resource-delete) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB CLI hakkında daha fazla bilgi için [Azure Cosmos DB CLI belgelerine](/cli/azure/cosmosdb)bakın.

Tüm Azure Cosmos DB CLI komut dosyası örnekleri [Azure Cosmos DB CLI GitHub Deposu'nda](https://github.com/Azure-Samples/azure-cli-samples/tree/master/cosmosdb)bulunabilir.
