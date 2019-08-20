---
title: Azure CLı kullanarak Azure Cosmos DB kaynaklarını yönetme
description: Azure Cosmos DB hesabınızı, veritabanınızı ve Kapsayıcılarınızı yönetmek için Azure CLı 'yi kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: f9d8bf9161343e4b36a3c16209873962b69d8af5
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69615215"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Azure CLı kullanarak Azure Cosmos kaynaklarını yönetme

Aşağıdaki kılavuzda, Azure CLı kullanarak Azure Cosmos DB hesaplarınızın, veritabanlarınızın ve kapsayıcılarınızın yönetimini otomatik hale getirmek için kullanılan komutlar açıklanmaktadır. Tüm Azure Cosmos DB CLI komutlarına ait başvuru sayfalarına [Azure CLI Başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb) sayfasından erişilebilir. Ayrıca, MongoDB, Gremlin, Cassandra ve Tablo API'si için Cosmos DB hesapları, veritabanları ve kapsayıcıları oluşturma ve yönetme dahil olmak üzere [Azure Cosmos DB Için Azure CLI örneklerinde](cli-samples.md)daha fazla örnek bulabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu, Azure CLI 2.0 veya sonraki bir sürümünü kullanmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Doğu ABD ve Batı ABD bölgelerinde SQL API 'SI, oturum tutarlılığı Azure Cosmos DB bir hesap oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb create \
   --name mycosmosdbaccount # must be lowercase and < 31 characters \
   --resource-group myResourceGroup \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations regionName=EastUS failoverPriority=0 isZoneRedundant=False \
   --locations regionName=WestUS failoverPriority=1 isZoneRedundant=False \
   --enable-multiple-write-locations false
```

> [!IMPORTANT]
> Azure Cosmos hesap adı küçük harfle yazılmalıdır.

## <a name="create-a-database"></a>Veritabanı oluşturma

Cosmos veritabanı oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az cosmosdb database create \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

400 RU/s ve bölüm anahtarı ile Cosmos kapsayıcısı oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --partition-key-path /myPartitionKey \
   --throughput 400
```

## <a name="change-the-throughput-of-a-container"></a>Kapsayıcının verimini değiştirme

Cosmos kapsayıcısının verimini 1000 RU/sn olarak değiştirmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --throughput 1000
```

## <a name="list-account-keys"></a>Hesap anahtarlarını listeleme

Cosmos hesabınıza ait anahtarları almak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# List account keys
az cosmosdb keys list \
   --name  mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="list-connection-strings"></a>Bağlantı dizelerini listeleme

Cosmos hesabınız için bağlantı dizelerini almak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="regenerate-account-key"></a>Hesap anahtarını yeniden oluştur

Cosmos hesabınız için yeni bir birincil anahtar oluşturmak üzere aşağıdaki komutu çalıştırın:

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --key-kind primary
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı hakkında daha fazla bilgi için bkz.

- [Azure CLI'yı yükleme](/cli/azure/install-azure-cli)
- [Azure CLI Başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Azure Cosmos DB için ek Azure CLı örnekleri](cli-samples.md)
