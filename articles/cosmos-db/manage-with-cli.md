---
title: Azure CLI'yi kullanarak Azure Cosmos DB kaynaklarını yönetme
description: Azure Cosmos DB hesabınızı, veritabanınızı ve kapsayıcılarınızı yönetmek için Azure CLI'yi kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: mjbrown
ms.openlocfilehash: 3f86468bcafe3d7ce78827aba761bb4e1bf920fa
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273639"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Azure CLI'yi kullanarak Azure Cosmos kaynaklarını yönetme

Aşağıdaki kılavuzda Azure Cosmos DB hesaplarınız, veritabanlarınız ve kapsayıcılarınızı yönetme işleminin Azure CLI kullanılarak otomatikleştirilmesini sağlayan yaygın komutlar açıklanır. Tüm Azure Cosmos DB CLI komutları için başvuru sayfaları [Azure CLI Başvurusu'nda](https://docs.microsoft.com/cli/azure/cosmosdb)mevcuttur. Ayrıca Azure [Cosmos DB için Azure CLI örneklerinde](cli-samples.md)(Cosmos DB hesaplarının nasıl oluşturulup yönetilene kadar) mongodb, Gremlin, Cassandra ve Table API için veritabanları ve kapsayıcılar gibi daha fazla örnek bulabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

SQL API, Batı ABD 2 ve Doğu ABD 2 bölgelerinde Oturum tutarlılığı içeren bir Azure Cosmos DB hesabı oluşturun:

> [!IMPORTANT]
> Azure Cosmos hesap adı küçük ve 31 karakterden az olmalıdır.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount' #needs to be lower case and less than 31 characters

az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --default-consistency-level Session \
    --locations regionName='West US 2' failoverPriority=0 isZoneRedundant=False \
    --locations regionName='East US 2' failoverPriority=1 isZoneRedundant=False
```

## <a name="add-or-remove-regions"></a>Bölge ekleme veya kaldırma

İki bölgeden bir Azure Cosmos hesabı oluşturun, bir bölge ekleyin ve bir bölgeyi kaldırın.

> [!NOTE]
> Bir Azure Cosmos hesabı `locations` için aynı anda bölgeler ekleyemez, kaldıramaz ve diğer özellikleri değiştiremezsiniz. Bölgeleri değiştirme, hesap kaynağındaki diğer değişikliklerden ayrı bir işlem olarak gerçekleştirilmelidir.
> [!NOTE]
> Bu komut, bölgeler eklemenize ve kaldırmanıza izin verir, ancak hata yerine etme önceliklerini değiştirmenize veya el ile başarısız olmayı tetiklemenize izin vermez. Bkz. [Set failover önceliği](#set-failover-priority) ve [Tetikleme kılavuzu failover.](#trigger-manual-failover)

```azurecli-interactive
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount' # must be lower case and <31 characters

# Create an account with 2 regions
az cosmosdb create --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False

# Add a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False \
    --locations regionName="South Central US" failoverPriority=2 isZoneRedundant=False

# Remove a region
az cosmosdb update --name $accountName --resource-group $resourceGroupName \
    --locations regionName="West US 2" failoverPriority=0 isZoneRedundant=False \
    --locations regionName="East US 2" failoverPriority=1 isZoneRedundant=False
```

## <a name="enable-multiple-write-regions"></a>Birden çok yazma bölgesini etkinleştirme

Cosmos hesabı için çoklu master'ı etkinleştirme

```azurecli-interactive
# Update an Azure Cosmos account from single to multi-master
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

az cosmosdb update --ids $accountId --enable-multiple-write-locations true
```

## <a name="set-failover-priority"></a>Başarısız önceliği ayarlama

Otomatik hata için yapılandırılan bir Azure Cosmos hesabının başarısız önceliğini ayarlama

```azurecli-interactive
# Assume region order is initially 'West US 2'=0 'East US 2'=1 'South Central US'=2 for account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

# Make South Central US the next region to fail over to instead of East US 2
az cosmosdb failover-priority-change --ids $accountId \
    --failover-policies 'West US 2'=0 'South Central US'=1 'East US 2'=2
```

## <a name="enable-automatic-failover"></a>Otomatik arızayı etkinleştirme

```azurecli-interactive
# Enable automatic failover on an existing account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

az cosmosdb update --ids $accountId --enable-automatic-failover true
```

## <a name="trigger-manual-failover"></a>Tetikleme kılavuzu failover

> [!CAUTION]
> Önceliği = 0 olan bölgeyi değiştirmek, Azure Cosmos hesabı için el ile başarısızlığa neden olur. Diğer öncelik değişikliği bir başarısızlık tetiklemez.

```azurecli-interactive
# Assume region order is initially 'West US 2'=0 'East US 2'=1 'South Central US'=2 for account
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'

# Get the account resource id for an existing account
accountId=$(az cosmosdb show -g $resourceGroupName -n $accountName --query id -o tsv)

# Trigger a manual failover to promote East US 2 as new write region
az cosmosdb failover-priority-change --ids $accountId \
    --failover-policies 'East US 2'=0 'South Central US'=1 'West US 2'=2
```

## <a name="list-all-account-keys"></a><a id="list-account-keys"></a>Tüm hesap anahtarlarını listele

Cosmos hesabının tüm anahtarlarını al.

```azurecli-interactive
# List all account keys
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
   -n $accountName \
   -g $resourceGroupName
```

## <a name="list-read-only-account-keys"></a>Salt okunur hesap anahtarlarını listele

Cosmos hesabı için salt okunur anahtarları alın.

```azurecli-interactive
# List read-only account keys
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type read-only-keys
```

## <a name="list-connection-strings"></a>Bağlantı dizelerini listele

Cosmos hesabının bağlantı dizelerini alın.

```azurecli-interactive
# List connection strings
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'

az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type connection-strings
```

## <a name="regenerate-account-key"></a>Hesap anahtarını yeniden oluşturma

Cosmos hesabı için yeni bir anahtar oluşturun.

```azurecli-interactive
# Regenerate secondary account keys
# key-kind values: primary, primaryReadonly, secondary, secondaryReadonly
az cosmosdb keys regenerate \
    -n $accountName \
    -g $resourceGroupName \
    --key-kind secondary
```

## <a name="create-a-database"></a>Veritabanı oluşturma

Bir Cosmos veritabanı oluşturun.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'

az cosmosdb sql database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName
```

## <a name="create-a-database-with-shared-throughput"></a>Paylaşılan iş tamına sahip bir veritabanı oluşturma

Paylaşılan iş veri tabanına sahip bir Cosmos veritabanı oluşturun.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
throughput=400

az cosmosdb sql database create \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $throughput
```

## <a name="change-the-throughput-of-a-database"></a>Veritabanının iş verisini değiştirme

Bir Cosmos veritabanının veri tabanının veri veri tabanının veri veri tabanının veri sini 1000 RU/s artırın.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
newRU=1000

# Get minimum throughput to make sure newRU is not lower than minRU
minRU=$(az cosmosdb sql database throughput show \
    -g $resourceGroupName -a $accountName -n $databaseName \
    --query resource.minimumThroughput -o tsv)

if [ $minRU -gt $newRU ]; then
    newRU=$minRU
fi

az cosmosdb sql database throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -n $databaseName \
    --throughput $newRU
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Varsayılan dizin ilkesi, bölüm tuşu ve 400 RU/s içeren bir Cosmos kapsayıcısı oluşturun.

```azurecli-interactive
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
throughput=400

az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --throughput $throughput
```

## <a name="create-a-container-with-ttl"></a>TTL ile kapsayıcı oluşturma

TTL etkin leştirilmiş bir Cosmos kapsayıcıoluşturun.

```azurecli-interactive
# Create an Azure Cosmos container with TTL of one day
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'

az cosmosdb sql container update \
    -g $resourceGroupName \
    -a $accountName \
    -d $databaseName \
    -n $containerName \
    --ttl=86400
```

## <a name="create-a-container-with-a-custom-index-policy"></a>Özel dizin ilkesi olan bir kapsayıcı oluşturma

Özel dizin ilkesi, uzamsal dizin, bileşik dizin, bir bölüm anahtarı ve 400 RU/s içeren bir Cosmos kapsayıcısı oluşturun.

```azurecli-interactive
# Create a SQL API container
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
partitionKey='/myPartitionKey'
throughput=400

# Generate a unique 10 character alphanumeric string to ensure unique resource names
uniqueId=$(env LC_CTYPE=C tr -dc 'a-z0-9' < /dev/urandom | fold -w 10 | head -n 1)

# Define the index policy for the container, include spatial and composite indexes
idxpolicy=$(cat << EOF
{
    "indexingMode": "consistent",
    "includedPaths": [
        {"path": "/*"}
    ],
    "excludedPaths": [
        { "path": "/headquarters/employees/?"}
    ],
    "spatialIndexes": [
        {"path": "/*", "types": ["Point"]}
    ],
    "compositeIndexes":[
        [
            { "path":"/name", "order":"ascending" },
            { "path":"/age", "order":"descending" }
        ]
    ]
}
EOF
)
# Persist index policy to json file
echo "$idxpolicy" > "idxpolicy-$uniqueId.json"


az cosmosdb sql container create \
    -a $accountName -g $resourceGroupName \
    -d $databaseName -n $containerName \
    -p $partitionKey --throughput $throughput \
    --idx @idxpolicy-$uniqueId.json

# Clean up temporary index policy file
rm -f "idxpolicy-$uniqueId.json"
```

## <a name="change-the-throughput-of-a-container"></a>Bir kapsayıcının iş kısmını değiştirme

Bir Cosmos konteynerinin üretim ini 1000 RU/s artırın.

```azurecli-interactive
resourceGroupName='MyResourceGroup'
accountName='mycosmosaccount'
databaseName='database1'
containerName='container1'
newRU=1000

# Get minimum throughput to make sure newRU is not lower than minRU
minRU=$(az cosmosdb sql container throughput show \
    -g $resourceGroupName -a $accountName -d $databaseName \
    -n $containerName --query resource.minimumThroughput -o tsv)

if [ $minRU -gt $newRU ]; then
    newRU=$minRU
fi

az cosmosdb sql container throughput update \
    -a $accountName \
    -g $resourceGroupName \
    -d $databaseName \
    -n $containerName \
    --throughput $newRU
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI hakkında daha fazla bilgi için bkz:

- [Azure CLI'yi yükle](/cli/azure/install-azure-cli)
- [Azure CLI Başvurusu](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Azure Cosmos DB için ek Azure CLI örnekleri](cli-samples.md)
