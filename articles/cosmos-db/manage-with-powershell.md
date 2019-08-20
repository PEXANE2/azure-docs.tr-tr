---
title: PowerShell kullanarak Azure Cosmos DB oluşturma ve yönetme
description: Azure Cosmos DB hesaplarınızı, veritabanlarınızı, Kapsayıcılarınızı ve aktarım hızını yönetmek için Azure PowerShell 'i kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 08/05/2019
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: e8f943ebaa5dfc06e0bfb04dc1097d6794ec6d05
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69616824"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>PowerShell kullanarak Azure Cosmos DB SQL API kaynaklarını yönetme

Aşağıdaki kılavuzda hesap, veritabanı, kapsayıcı ve aktarım hızı dahil olmak üzere Azure Cosmos DB kaynaklarının yönetimi ve otomatik hale getirmek için PowerShell 'in nasıl kullanılacağı açıklanmaktadır. Azure Cosmos DB yönetimi, AzResource cmdlet 'i aracılığıyla doğrudan Azure Cosmos DB kaynak sağlayıcısına işlenir. Azure Cosmos DB kaynak sağlayıcısı için PowerShell kullanılarak yönetilebilen tüm özellikleri görüntülemek için, bkz. [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)

Azure Cosmos DB platformlar arası yönetimi için [Azure CLI](manage-with-cli.md), [REST API][rp-rest-api]veya [Azure Portal](create-sql-api-dotnet.md#create-account)kullanabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Başlarken

PowerShell 'de Azure hesabınızda yüklemek ve oturum açmak için [Azure PowerShell yükleyip yapılandırma][powershell-install-configure] konusundaki yönergeleri izleyin.

* Kullanıcı onayı gerekmeden aşağıdaki komutları yürütün, eklemek istediğiniz, `-Force` bayrağı komutu.
* Aşağıdaki komutlar zaman uyumlu.

## <a name="azure-cosmos-accounts"></a>Azure Cosmos hesapları

Aşağıdaki bölümlerde aşağıdakiler dahil olmak üzere Azure Cosmos hesabının nasıl yönetileceği gösterilmektedir:

* [Azure Cosmos hesabı oluşturma](#create-account)
* [Azure Cosmos hesabını güncelleştirme](#update-account)
* [Bir abonelikteki tüm Azure Cosmos hesaplarını listeleme](#list-accounts)
* [Azure Cosmos hesabı edinme](#get-account)
* [Azure Cosmos hesabını silme](#delete-account)
* [Azure Cosmos hesabının etiketlerini güncelleştirme](#update-tags)
* [Azure Cosmos hesabının anahtarlarını listeleyin](#list-keys)
* [Azure Cosmos hesabı için anahtarları yeniden oluşturun](#regenerate-keys)
* [Azure Cosmos hesabı için bağlantı dizelerini listeleme](#list-connection-strings)
* [Azure Cosmos hesabı için yük devretme önceliğini değiştirme](#modify-failover-priority)

### <a id="create-account"></a>Azure Cosmos hesabı oluşturma

Bu komut, [birden çok bölgede][distribute-data-globally], sınırlanmış bir [tutarlılık Ilkesiyle](consistency-levels.md)Azure Cosmos veritabanı hesabı oluşturur.

```azurepowershell-interactive
# Create an Azure Cosmos Account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US 2"
$accountName = "mycosmosaccount" # must be lowercase and < 31 characters .

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0 },
    @{ "locationName"="East US 2"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="false"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

* `$accountName`Azure Cosmos hesabının adı. Küçük harf olmalı, alfasayısal ve '-' karakteri ve 3 ila 31 karakter kabul eder.
* `$location`Azure Cosmos hesabı kaynağının konumu.
* `$locations`Veritabanı hesabının çoğaltma bölgeleri. Yük devretme önceliği değeri 0 olan veritabanı hesabı başına bir yazma bölgesi olmalıdır.
* `$consistencyPolicy`Azure Cosmos hesabının varsayılan tutarlılık düzeyi. Daha fazla bilgi için [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md).
* `$CosmosDBProperties`Hesabı sağlamak için Cosmos DB Azure Resource Manager sağlayıcısına geçirilen özellik değerleri.

Azure Cosmos hesapları, IP güvenlik duvarı ve sanal ağ hizmeti uç noktaları ile yapılandırılabilir. Azure Cosmos DB için IP güvenlik duvarını yapılandırma hakkında daha fazla bilgi için bkz. [IP güvenlik duvarını yapılandırma](how-to-configure-firewall.md).  Azure Cosmos DB için hizmet uç noktalarını etkinleştirme hakkında daha fazla bilgi için bkz. [sanal ağlardan erişimi yapılandırma](how-to-configure-vnet-service-endpoint.md).

### <a id="list-accounts"></a>Bir abonelikteki tüm Azure Cosmos hesaplarını listeleme

Bu komut, bir abonelikteki tüm Azure Cosmos hesaplarını listeetmenize olanak tanır.

```azurepowershell-interactive
# List Azure Cosmos Accounts

Get-AzResource -ResourceType Microsoft.DocumentDb/databaseAccounts | ft
```

### <a id="get-account"></a>Azure Cosmos hesabının özelliklerini al

Bu komut, mevcut bir Azure Cosmos hesabının özelliklerini almanızı sağlar.

```azurepowershell-interactive
# Get the properties of an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName | Select-Object Properties
```

### <a id="update-account"></a>Azure Cosmos hesabını güncelleştirme

Bu komut, Azure Cosmos veritabanı hesabı özelliklerinizi güncelleştirmenize olanak tanır. Güncelleştirilebilen özellikler şunları içerir:

* Bölge ekleme veya kaldırma
* Varsayılan tutarlılık ilkesini değiştirme
* IP aralığı filtresini değiştirme
* Sanal ağ yapılandırmasını değiştirme
* Çoklu yönetici etkinleştiriliyor

> [!NOTE]
> Bu komut, bölgeleri eklemenize ve kaldırmanıza olanak tanır ancak yük devretme önceliklerini değiştirmenize veya bölgeyi ile `failoverPriority=0`değiştirmenize izin vermez. Yük devretme önceliğini değiştirmek için bkz. [Azure Cosmos hesabı için yük devretme önceliğini değiştirme](#modify-failover-priority).

```azurepowershell-interactive
# Get an Azure Cosmos Account (assume it has two regions currently West US 2 and East US 2) and add a third region

$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0 },
    @{ "locationName"="East US 2"; "failoverPriority"=1 },
    @{ "locationName"="South Central US"; "failoverPriority"=2 }
)

$account.Properties.locations = $locations
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="delete-account"></a>Azure Cosmos hesabını silme

Bu komut, var olan bir Azure Cosmos hesabını silmenizi sağlar.

```azurepowershell-interactive
# Delete an Azure Cosmos Account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName
```

### <a id="update-tags"></a>Azure Cosmos hesabının etiketlerini güncelleştirme

Aşağıdaki örnekte, bir Azure Cosmos hesabı için [Azure Kaynak etiketlerinin][azure-resource-tags] nasıl ayarlanacağı açıklanır.

> [!NOTE]
> Bu komut oluşturma veya güncelleştirme komutlarla ekleyerek birleştirilebilir `-Tags` bayrağıyla karşılık gelen parametre.

```azurepowershell-interactive
# Update tags for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$tags = @{
    "dept" = "Finance";
    "environment" = "Production"
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -Tags $tags
```

### <a id="list-keys"></a> Hesap anahtarlarını Listele

Azure Cosmos DB hesabı oluşturduğunuzda, hizmet, Azure Cosmos DB hesabına erişim sağlandığında kimlik doğrulaması için kullanılan iki ana erişim anahtarları oluşturur. İki erişim tuşu sağlayarak, Azure Cosmos DB, kesinti olmadan Azure Cosmos DB hesabınız için anahtarları yeniden sağlar. Salt okunur anahtarlar, salt okunur işlemler kimlik doğrulaması için de kullanılabilir. Var. iki okuma-yazma anahtarları (birincil ve ikincil) ve iki salt okunur anahtarlar (birincil ve ikincil)

```azurepowershell-interactive
# List keys for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listKeys `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="list-connection-strings"></a> Liste bağlantı dizeleri

MongoDB uygulamanızı hesabınıza veritabanı hesabına bağlanacak bağlantı dizesini, MongoDB hesabı için aşağıdaki komutu kullanarak alınabilir.

```azurepowershell-interactive
# List connection strings for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listConnectionStrings `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="regenerate-keys"></a>Hesap anahtarlarını yeniden üret

Bağlantıları daha güvenli tutmaya yardımcı olmak için bir Azure Cosmos hesabına erişim anahtarlarının düzenli olarak yeniden oluşturulması gerekir. Hesaba birincil ve ikincil erişim anahtarları atanır. Bu, diğer yeniden üretilirken istemcilerin erişimi korumasını sağlar. Azure Cosmos hesabı için dört tür anahtar vardır (birincil, Ikincil, PrimaryReadonly ve SecondaryReadonly)

```azurepowershell-interactive
# Regenerate the primary key for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keyKind = @{ "keyKind"="Primary" }

$keys = Invoke-AzResourceAction -Action regenerateKey `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $keyKind

Select-Object $keys
```

### <a id="modify-failover-priority"></a>Yük devretme önceliğini değiştirme

Çok bölgeli veritabanı hesaplarında, Cosmos hesabının ikincil okuma çoğaltmalarını yükseltebileceği sırayı değiştirerek birincil yazma çoğaltmasında bölgesel bir yük devretme gerçekleşmelidir. Ayrıca `failoverPriority=0` , olağanüstü durum kurtarma planlamasının test bir olağanüstü durum kurtarma detayına karşı değiştirme de kullanılabilir.

Aşağıdaki örnekte, hesabının geçerli bir yük devretme önceliğine `West US 2 = 0` `East US 2 = 1` sahip olduğunu ve bölgeleri çevireceğini varsayalım.

> [!CAUTION]
> `locationName` İçin`failoverPriority=0` değiştirmek, bir Azure Cosmos hesabı için el ile yük devretmeyi tetikler. Başka herhangi bir öncelik değişikliği, yük devretmeyi tetiklemez.

```azurepowershell-interactive
# Change the failover priority for an Azure Cosmos Account
# Assume existing priority is "West US 2" = 0 and "East US 2" = 1

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$failoverRegions = @(
    @{ "locationName"="East US 2"; "failoverPriority"=0 },
    @{ "locationName"="West US 2"; "failoverPriority"=1 }
)

$failoverPolicies = @{
    "failoverPolicies"= $failoverRegions
}

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a name="azure-cosmos-database"></a>Azure Cosmos veritabanı

Aşağıdaki bölümlerde aşağıdakiler dahil olmak üzere Azure Cosmos veritabanının nasıl yönetileceği gösterilmektedir:

* [Azure Cosmos veritabanı oluşturma](#create-db)
* [Paylaşılan aktarım hızı ile bir Azure Cosmos veritabanı oluşturma](#create-db-ru)
* [Azure Cosmos veritabanının verimini alın](#get-db-ru)
* [Bir hesaptaki tüm Azure Cosmos veritabanlarını listeleme](#list-db)
* [Tek bir Azure Cosmos veritabanı edinme](#get-db)
* [Azure Cosmos veritabanını silme](#delete-db)

### <a id="create-db"></a>Azure Cosmos veritabanı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{"id"=$databaseName}
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="create-db-ru"></a>Paylaşılan aktarım hızı ile bir Azure Cosmos veritabanı oluşturma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database2"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="get-db-ru"></a>Azure Cosmos veritabanının verimini alın

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$databaseThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/settings"
$databaseThroughputResourceName = $accountName + "/sql/" + $databaseName + "/throughput"

Get-AzResource -ResourceType $databaseThroughputResourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseThroughputResourceName  | Select-Object Properties
```

### <a id="list-db"></a>Tüm Azure Cosmos veritabanlarını bir hesapta al

```azurepowershell-interactive
# Get all databases in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$resourceName = $accountName + "/sql/"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName  | Select-Object Properties
```

### <a id="get-db"></a>Tek bir Azure Cosmos veritabanı edinme

```azurepowershell-interactive
# Get a single database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-db"></a>Azure Cosmos veritabanını silme

```azurepowershell-interactive
# Delete a database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName
Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="azure-cosmos-container"></a>Azure Cosmos kapsayıcısı

Aşağıdaki bölümlerde aşağıdakiler de dahil olmak üzere Azure Cosmos kapsayıcısının nasıl yönetileceği gösterilmektedir:

* [Azure Cosmos kapsayıcısı oluşturma](#create-container)
* [Büyük bölüm anahtarı ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-big-pk)
* [Azure Cosmos kapsayıcısının verimini alın](#get-container-ru)
* [Paylaşılan verimlilik ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-ru)
* [Özel dizin oluşturma ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-custom-index)
* [Dizin oluşturma kapalıyken bir Azure Cosmos kapsayıcısı oluşturma](#create-container-no-index)
* [Benzersiz anahtar ve TTL ile Azure Cosmos kapsayıcısı oluşturma](#create-container-unique-key-ttl)
* [Çakışma çözümü ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-lww)
* [Bir veritabanındaki tüm Azure Cosmos kapsayıcılarını listeleme](#list-containers)
* [Veritabanında tek bir Azure Cosmos kapsayıcısı edinme](#get-container)
* [Azure Cosmos kapsayıcısını silme](#delete-container)

### <a id="create-container"></a>Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos container with default indexes and throughput at 400 RU
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-big-pk"></a>Büyük bölüm anahtarı boyutuyla Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos container with a large partition key value (version = 2)
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash";
            "version" = 2
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="get-container-ru"></a>Azure Cosmos kapsayıcısının verimini alın

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$containerThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers/settings"
$containerThroughputResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName + "/throughput"

Get-AzResource -ResourceType $containerThroughputResourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $containerThroughputResourceName  | Select-Object Properties
```

### <a id="create-container-ru"></a>Paylaşılan verimlilik ile bir Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container2"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{}
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties 
```

### <a id="create-container-custom-index"></a>Özel dizin ilkesiyle Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create a container with a custom indexing policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container3"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent";
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @()
            });
            "excludedPaths"= @(@{
                "path"="/myPathToNotIndex/*"
            })
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-no-index"></a>Dizin oluşturma kapalıyken bir Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos container with no indexing
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container4"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        };
        "indexingPolicy"=@{
            "indexingMode"="none"
        }
    };
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-unique-key-ttl"></a>Benzersiz anahtar ilkesi ve TTL ile Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create a container with a unique key policy and TTL
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent";
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @()
            });
            "excludedPaths"= @()
        };
        "uniqueKeyPolicy"= @{
            "uniqueKeys"= @(@{
                "paths"= @(
                    "/myUniqueKey1";
                    "/myUniqueKey2"
                )
            })
        };
        "defaultTtl"= 100;
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-lww"></a>Çakışma çözümü ile bir Azure Cosmos kapsayıcısı oluşturma

Saklı yordam kullanmak üzere bir çakışma çözümleme ilkesi oluşturmak için, çözümleme yolunu `"mode"="custom"` saklı `"conflictResolutionPath"="myResolverStoredProcedure"`yordamın adı olarak ayarlayın ve ayarlayın. Conflictsfeed 'e yönelik tüm çakışmaları yazmak ve ayrı olarak işlemek için, `"mode"="custom"` ve`"conflictResolutionPath"=""`

```azurepowershell-interactive
# Create container with last-writer-wins conflict resolution policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container6"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        };
        "conflictResolutionPolicy"=@{
            "mode"="lastWriterWins";
            "conflictResolutionPath"="/myResolutionPath"
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="list-containers"></a>Bir veritabanındaki tüm Azure Cosmos kapsayıcılarını listeleme

```azurepowershell-interactive
# List all Azure Cosmos containers in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="get-container"></a>Veritabanında tek bir Azure Cosmos kapsayıcısı edinme

```azurepowershell-interactive
# Get a single Azure Cosmos container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-container"></a>Azure Cosmos kapsayıcısını silme

```azurepowershell-interactive
# Delete an Azure Cosmos container
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tüm PowerShell örnekleri](powershell-samples.md)
* [Azure Cosmos hesabını yönetme](how-to-manage-database-account.md)
* [Azure Cosmos kapsayıcısı oluşturma](how-to-create-container.md)
* [Azure Cosmos DB yaşam süresi yapılandırma](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
