---
title: PowerShell kullanarak Azure Cosmos DB oluşturma ve yönetme
description: Azure Cosmos hesaplarınızı, veritabanlarınızı, Kapsayıcılarınızı ve aktarım hızını yönetmek Azure PowerShell kullanın.
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/13/2020
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: 1e43cc48a6c4684326a152adedabcd00a44657a6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85390848"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>PowerShell kullanarak Azure Cosmos DB SQL API kaynaklarını yönetme

Aşağıdaki kılavuzda hesap, veritabanı, kapsayıcı ve aktarım hızı gibi Azure Cosmos DB kaynaklarının yönetimine yönelik betik oluşturmak ve yönetimini otomatikleştirmek için PowerShell’in nasıl kullanılacağı açıklanır.

> [!NOTE]
> Bu makaledeki örneklerde [az. CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) yönetim cmdlet 'leri kullanılır. En son değişiklikler için [az. CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) API başvuru sayfasına bakın.

Azure Cosmos DB platformlar arası yönetimi için, `Az` ve `Az.CosmosDB` cmdlet 'lerini, [platformlar arası PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell)Ile birlikte ve [Azure CLI](manage-with-cli.md), [REST API][rp-rest-api]veya [Azure Portal](create-sql-api-dotnet.md#create-account)birlikte kullanabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Kullanmaya Başlama

PowerShell 'de Azure hesabınızda yüklemek ve oturum açmak için [Azure PowerShell yükleyip yapılandırma][powershell-install-configure] konusundaki yönergeleri izleyin.

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
* [Azure Cosmos hesabı için el ile yük devretme tetikleyin](#trigger-manual-failover)
* [Azure Cosmos DB hesabındaki kaynak kilitlerini listeleme](#list-account-locks)

### <a name="create-an-azure-cosmos-account"></a><a id="create-account"></a>Azure Cosmos hesabı oluşturma

Bu komut, [birden çok bölge][distribute-data-globally], [otomatik yük devretme](how-to-manage-database-account.md#automatic-failover) ve sınırlanmış stalet [tutarlılık ilkesiyle](consistency-levels.md)bir Azure Cosmos DB veritabanı hesabı oluşturur.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$locations = @("West US 2", "East US 2")
$accountName = "mycosmosaccount"
$apiKind = "Sql"
$consistencyLevel = "BoundedStaleness"
$maxStalenessInterval = 300
$maxStalenessPrefix = 100000

New-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Location $locations `
    -Name $accountName `
    -ApiKind $apiKind `
    -EnableAutomaticFailover:$true `
    -DefaultConsistencyLevel $consistencyLevel `
    -MaxStalenessIntervalInSeconds $maxStalenessInterval `
    -MaxStalenessPrefix $maxStalenessPrefix
```

* `$resourceGroupName`Cosmos hesabının dağıtılacağı Azure Kaynak grubu. Zaten var olmalıdır.
* `$locations`Veritabanı hesabına ait bölgeler, yazma bölgesiyle başlar ve yük devretme önceliğine göre sıralanır.
* `$accountName`Azure Cosmos hesabının adı. Benzersiz, küçük harf, yalnızca alfasayısal ve '-' karakter içermeli ve 3 ila 31 karakter uzunluğunda olmalıdır.
* `$apiKind`Oluşturulacak Cosmos hesabının türü. Daha fazla bilgi için bkz. [Cosmos DB API 'leri](introduction.md#develop-applications-on-cosmos-db-using-popular-open-source-software-oss-apis).
* `$consistencyPolicy`, `$maxStalenessInterval` , ve `$maxStalenessPrefix` Azure Cosmos hesabının varsayılan tutarlılık düzeyini ve ayarlarını yapın. Daha fazla bilgi için bkz. [Azure Cosmos DB tutarlılık düzeyleri](consistency-levels.md).

Azure Cosmos hesapları IP güvenlik duvarı, sanal ağ hizmeti uç noktaları ve özel uç noktalarla yapılandırılabilir. Azure Cosmos DB için IP güvenlik duvarını yapılandırma hakkında daha fazla bilgi için bkz. [IP güvenlik duvarını yapılandırma](how-to-configure-firewall.md). Azure Cosmos DB için hizmet uç noktalarını etkinleştirme hakkında daha fazla bilgi için bkz. [sanal ağlardan erişimi yapılandırma](how-to-configure-vnet-service-endpoint.md). Azure Cosmos DB için özel uç noktaları etkinleştirme hakkında daha fazla bilgi için bkz. [Özel uç noktalardan erişimi yapılandırma](how-to-configure-private-endpoints.md).

### <a name="list-all-azure-cosmos-accounts-in-a-resource-group"></a><a id="list-accounts"></a>Bir kaynak grubundaki tüm Azure Cosmos hesaplarını listeleme

Bu komut, bir kaynak grubundaki tüm Azure Cosmos hesaplarını listeler.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"

Get-AzCosmosDBAccount -ResourceGroupName $resourceGroupName
```

### <a name="get-the-properties-of-an-azure-cosmos-account"></a><a id="get-account"></a>Azure Cosmos hesabının özelliklerini al

Bu komut, mevcut bir Azure Cosmos hesabının özelliklerini almanızı sağlar.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccount -ResourceGroupName $resourceGroupName -Name $accountName
```

### <a name="update-an-azure-cosmos-account"></a><a id="update-account"></a>Azure Cosmos hesabını güncelleştirme

Bu komut Azure Cosmos DB veritabanı hesabı özelliklerinizi güncelleştirmenize olanak tanır. Güncelleştirilebilen özellikler şunları içerir:

* Bölge ekleme veya kaldırma
* Varsayılan tutarlılık ilkesini değiştirme
* IP aralığı filtresini değiştirme
* Sanal ağ yapılandırmasını değiştirme
* Çoklu yönetici etkinleştiriliyor

> [!NOTE]
> Azure Cosmos hesabı için aynı anda bölge ekleyemez veya kaldıramazsınız ( `locations` ) ve diğer özellikleri değiştiremezsiniz. Bölgeleri değiştirmek, hesapta başka herhangi bir değişiklikten ayrı bir işlem olarak gerçekleştirilmelidir.
> [!NOTE]
> Bu komut, bölge eklemenize ve kaldırmanıza izin verir ancak yük devretme önceliklerini değiştirmenize veya el ile yük devretme tetiklemesine izin vermez. Bkz. [Yük devretme önceliğini değiştirme](#modify-failover-priority) ve [El Ile yük devretmeyi tetikleme](#trigger-manual-failover).

```azurepowershell-interactive
# Create account with two regions
$resourceGroupName = "myResourceGroup"
$locations = @("West US 2", "East US 2")
$accountName = "mycosmosaccount"
$apiKind = "Sql"
$consistencyLevel = "Session"
$enableAutomaticFailover = $true

# Create the Cosmos DB account
New-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Location $locations `
    -Name $accountName `
    -ApiKind $apiKind `
    -EnableAutomaticFailover:$enableAutomaticFailover `
    -DefaultConsistencyLevel $consistencyLevel

# Add a region to the account
$locations2 = @("West US 2", "East US 2", "South Central US")
$locationObjects2 = @()
$i = 0
ForEach ($location in $locations2) {
    $locationObjects2 += @{ locationName = "$location"; failoverPriority = $i++ }
}

Update-AzCosmosDBAccountRegion `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -LocationObject $locationObjects2

Write-Host "Update-AzCosmosDBAccountRegion returns before the region update is complete."
Write-Host "Check account in Azure portal or using Get-AzCosmosDBAccount for region status."
Write-Host "When region was added, press any key to continue."
$HOST.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown") | OUT-NULL
$HOST.UI.RawUI.Flushinputbuffer()

# Remove a region from the account
$locations3 = @("West US 2", "South Central US")
$locationObjects3 = @()
$i = 0
ForEach ($location in $locations3) {
    $locationObjects3 += @{ locationName = "$location"; failoverPriority = $i++ }
}

Update-AzCosmosDBAccountRegion `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -LocationObject $locationObjects3

Write-Host "Update-AzCosmosDBAccountRegion returns before the region update is complete."
Write-Host "Check account in Azure portal or using Get-AzCosmosDBAccount for region status."
```
### <a name="enable-multiple-write-regions-for-an-azure-cosmos-account"></a><a id="multi-master"></a>Azure Cosmos hesabı için birden çok yazma bölgesini etkinleştirme

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$enableAutomaticFailover = $false
$enableMultiMaster = $true

# First disable automatic failover - cannot have both automatic
# failover and multi-master on an account
Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -EnableAutomaticFailover:$enableAutomaticFailover

# Now enable multi-master
Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -EnableMultipleWriteLocations:$enableMultiMaster
```

### <a name="delete-an-azure-cosmos-account"></a><a id="delete-account"></a>Azure Cosmos hesabını silme

Bu komut, var olan bir Azure Cosmos hesabını siler.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -PassThru:$true
```

### <a name="update-tags-of-an-azure-cosmos-account"></a><a id="update-tags"></a>Azure Cosmos hesabının etiketlerini güncelleştirme

Bu komut, bir Azure Cosmos hesabının [Azure Kaynak etiketlerini][azure-resource-tags] ayarlar. Etiketler, kullanarak hesap güncelleştirme ' de kullanılarak hem hesap oluşturma sırasında hem de ayarlanabilir `New-AzCosmosDBAccount` `Update-AzCosmosDBAccount` .

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$tags = @{dept = "Finance"; environment = "Production";}

Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -Tag $tags
```

### <a name="list-account-keys"></a><a id="list-keys"></a>Hesap anahtarlarını listeleme

Azure Cosmos hesabı oluşturduğunuzda, hizmet Azure Cosmos hesabına erişildiğinde kimlik doğrulaması için kullanılabilecek iki ana erişim anahtarı oluşturur. Salt okuma işlemlerine yönelik kimlik doğrulama için salt okuma anahtarları da oluşturulur.
Azure Cosmos DB iki erişim anahtarı sunarak, Azure Cosmos hesabınızda bir kesinti olmadan bir anahtarı tek seferde yeniden oluşturup döndürmenizi sağlar.
Cosmos DB hesapların iki okuma-yazma anahtarı (birincil ve ikincil) ve iki salt okuma anahtarı vardır (birincil ve ikincil).

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -Type "Keys"
```

### <a name="list-connection-strings"></a><a id="list-connection-strings"></a>Bağlantı dizelerini listeleme

Aşağıdaki komut, Cosmos DB hesabına uygulama bağlamak için bağlantı dizelerini alır.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -Type "ConnectionStrings"
```

### <a name="regenerate-account-keys"></a><a id="regenerate-keys"></a>Hesap anahtarlarını yeniden üret

Bağlantıları güvenli tutmaya yardımcı olmak için Azure Cosmos hesabına yönelik erişim anahtarlarının düzenli olarak yeniden oluşturulması gerekir. Hesaba birincil ve ikincil erişim anahtarları atanır. Bu, bir seferde bir anahtar yeniden üretilirken istemcilerin erişimi korumasını sağlar.
Azure Cosmos hesabı için dört tür anahtar vardır (birincil, Ikincil, PrimaryReadonly ve SecondaryReadonly)

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "mycosmosaccount" # Must be all lower case
$keyKind = "primary" # Other key kinds: secondary, primaryReadOnly, secondaryReadOnly

New-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -KeyKind $keyKind
```

### <a name="enable-automatic-failover"></a><a id="enable-automatic-failover"></a>Otomatik yük devretmeyi etkinleştir

Aşağıdaki komut, birincil bölge kullanılamaz hale gelmesi için bir Cosmos DB hesabını otomatik olarak ikincil bölgesine devretmek üzere ayarlar.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$enableAutomaticFailover = $true
$enableMultiMaster = $false

# First disable multi-master - cannot have both automatic
# failover and multi-master on an account
Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -EnableMultipleWriteLocations:$enableMultiMaster

# Now enable automatic failover
Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -EnableAutomaticFailover:$enableAutomaticFailover
```

### <a name="modify-failover-priority"></a><a id="modify-failover-priority"></a>Yük devretme önceliğini değiştirme

Otomatik yük devretme ile yapılandırılan hesaplar için, Cosmos 'nin ikincil çoğaltmaları birincil olarak yükseltebileceği sırayı değiştirebilirsiniz.

Aşağıdaki örnekte, geçerli yük devretme önceliğin, `West US 2 = 0` `East US 2 = 1` ,, olduğunu varsayalım `South Central US = 2` . Komut bunu `West US 2 = 0` ,,, olarak değiştirir `South Central US = 1` `East US 2 = 2` .

> [!CAUTION]
> Konumunun değiştirilmesi, `failoverPriority=0` bir Azure Cosmos hesabı için el ile yük devretmeyi tetikler. Başka herhangi bir öncelik değişikliği, yük devretmeyi tetiklemez.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$locations = @("West US 2", "South Central US", "East US 2") # Regions ordered by UPDATED failover priority

Update-AzCosmosDBAccountFailoverPriority `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -FailoverPolicy $locations
```

### <a name="trigger-manual-failover"></a><a id="trigger-manual-failover"></a>El Ile yük devretmeyi Tetikle

El Ile yük devretme ile yapılandırılan hesaplar için, üzerinde değişiklik yaparak ikincil çoğaltmanın yükünü devreder ve birincil olarak yükseltebilirsiniz `failoverPriority=0` . Bu işlem olağanüstü durum kurtarma planlamasını test etmek için bir olağanüstü durum kurtarma detayına başlamayı başlatmak için kullanılabilir.

Aşağıdaki örnekte, hesabının geçerli bir yük devretme önceliğine sahip olduğunu ve `West US 2 = 0` `East US 2 = 1` bölgeleri çevireceğini varsayalım.

> [!CAUTION]
> `locationName`İçin değiştirmek `failoverPriority=0` , bir Azure Cosmos hesabı için el ile yük devretmeyi tetikler. Diğer herhangi bir öncelik değişikliği, yük devretmeyi tetiklemez.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$locations = @("East US 2", "West US 2") # Regions ordered by UPDATED failover priority

Update-AzCosmosDBAccountFailoverPriority `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -FailoverPolicy $locations
```

### <a name="list-resource-locks-on-an-azure-cosmos-db-account"></a><a id="list-account-locks"></a>Azure Cosmos DB hesabındaki kaynak kilitlerini listeleme

Kaynak kilitleri, veritabanları ve koleksiyonlar dahil olmak üzere Azure Cosmos DB kaynaklara yerleştirilebilir. Aşağıdaki örnekte, bir Azure Cosmos DB hesabındaki tüm Azure Kaynak kilitlerinin nasıl listelediği gösterilmektedir.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$resourceTypeAccount = "Microsoft.DocumentDB/databaseAccounts"
$accountName = "mycosmosaccount"

Get-AzResourceLock `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceTypeAccount `
    -ResourceName $accountName
```

## <a name="azure-cosmos-db-database"></a>Azure Cosmos DB veritabanı

Aşağıdaki bölümlerde aşağıdakiler de dahil olmak üzere Azure Cosmos DB veritabanının nasıl yönetileceği gösterilmektedir:

* [Azure Cosmos DB veritabanı oluşturma](#create-db)
* [Paylaşılan verimlilik ile Azure Cosmos DB veritabanı oluşturma](#create-db-ru)
* [Azure Cosmos DB veritabanının verimini al](#get-db-ru)
* [Bir hesaptaki tüm Azure Cosmos DB veritabanlarını listeleme](#list-db)
* [Tek bir Azure Cosmos DB veritabanı al](#get-db)
* [Azure Cosmos DB veritabanını silme](#delete-db)
* [Silmeyi engellemek için Azure Cosmos DB veritabanında bir kaynak kilidi oluşturun](#create-db-lock)
* [Azure Cosmos DB veritabanında kaynak kilidini kaldırma](#remove-db-lock)

### <a name="create-an-azure-cosmos-db-database"></a><a id="create-db"></a>Azure Cosmos DB veritabanı oluşturma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

New-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="create-an-azure-cosmos-db-database-with-shared-throughput"></a><a id="create-db-ru"></a>Paylaşılan verimlilik ile Azure Cosmos DB veritabanı oluşturma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$databaseRUs = 400

New-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName `
    -Throughput $databaseRUs
```

### <a name="get-the-throughput-of-an-azure-cosmos-db-database"></a><a id="get-db-ru"></a>Azure Cosmos DB veritabanının verimini al

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Get-AzCosmosDBSqlDatabaseThroughput `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="get-all-azure-cosmos-db-databases-in-an-account"></a><a id="list-db"></a>Bir hesaptaki tüm Azure Cosmos DB veritabanlarını al

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName
```

### <a name="get-a-single-azure-cosmos-db-database"></a><a id="get-db"></a>Tek bir Azure Cosmos DB veritabanı al

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Get-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="delete-an-azure-cosmos-db-database"></a><a id="delete-db"></a>Azure Cosmos DB veritabanını silme

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Remove-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="create-a-resource-lock-on-an-azure-cosmos-db-database-to-prevent-delete"></a><a id="create-db-lock"></a>Silmeyi engellemek için Azure Cosmos DB veritabanında bir kaynak kilidi oluşturun

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$resourceType = "Microsoft.DocumentDB/databaseAccounts/sqlDatabases"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$resourceName = "$accountName/$databaseName"
$lockName = "myResourceLock"
$lockLevel = "CanNotDelete"

New-AzResourceLock `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceType `
    -ResourceName $resourceName `
    -LockName $lockName `
    -LockLevel $lockLevel
```

### <a name="remove-a-resource-lock-on-an-azure-cosmos-db-database"></a><a id="remove-db-lock"></a>Azure Cosmos DB veritabanında kaynak kilidini kaldırma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$resourceType = "Microsoft.DocumentDB/databaseAccounts/sqlDatabases"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$resourceName = "$accountName/$databaseName"
$lockName = "myResourceLock"

Remove-AzResourceLock `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceType `
    -ResourceName $resourceName `
    -LockName $lockName
```

## <a name="azure-cosmos-db-container"></a>Azure Cosmos DB kapsayıcı

Aşağıdaki bölümlerde aşağıdakiler dahil Azure Cosmos DB kapsayıcısının nasıl yönetileceği gösterilmektedir:

* [Azure Cosmos DB kapsayıcısı oluşturma](#create-container)
* [Büyük bölüm anahtarı ile Azure Cosmos DB kapsayıcısı oluşturma](#create-container-big-pk)
* [Azure Cosmos DB kapsayıcısının verimini al](#get-container-ru)
* [Özel dizin oluşturma ile Azure Cosmos DB kapsayıcısı oluşturma](#create-container-custom-index)
* [Dizin oluşturma kapalıyken Azure Cosmos DB kapsayıcısı oluşturma](#create-container-no-index)
* [Benzersiz anahtar ve TTL ile Azure Cosmos DB kapsayıcısı oluşturma](#create-container-unique-key-ttl)
* [Çakışma çözümüne sahip bir Azure Cosmos DB kapsayıcısı oluşturma](#create-container-lww)
* [Bir veritabanındaki tüm Azure Cosmos DB kapsayıcıları listeleme](#list-containers)
* [Veritabanında tek bir Azure Cosmos DB kapsayıcısı edinme](#get-container)
* [Azure Cosmos DB kapsayıcısını silme](#delete-container)
* [Silmeyi engellemek için Azure Cosmos DB kapsayıcısı üzerinde bir kaynak kilidi oluşturun](#create-container-lock)
* [Azure Cosmos DB kapsayıcısında kaynak kilidini kaldırma](#remove-container-lock)

### <a name="create-an-azure-cosmos-db-container"></a><a id="create-container"></a>Azure Cosmos DB kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos DB container with default indexes and throughput at 400 RU
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath
```

### <a name="create-an-azure-cosmos-db-container-with-a-large-partition-key-size"></a><a id="create-container-big-pk"></a>Büyük bölüm anahtarı boyutuyla Azure Cosmos DB kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos DB container with a large partition key value (version = 2)
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -PartitionKeyVersion 2
```

### <a name="get-the-throughput-of-an-azure-cosmos-db-container"></a><a id="get-container-ru"></a>Azure Cosmos DB kapsayıcısının verimini al

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"

Get-AzCosmosDBSqlContainerThroughput `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName
```

### <a name="create-an-azure-cosmos-db-container-with-custom-index-policy"></a><a id="create-container-custom-index"></a>Özel dizin ilkesiyle bir Azure Cosmos DB kapsayıcısı oluşturma

```azurepowershell-interactive
# Create a container with a custom indexing policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"
$indexPathIncluded = "/*"
$indexPathExcluded = "/myExcludedPath/*"

$includedPathIndex = New-AzCosmosDBSqlIncludedPathIndex -DataType String -Kind Range
$includedPath = New-AzCosmosDBSqlIncludedPath -Path $indexPathIncluded -Index $includedPathIndex

$indexingPolicy = New-AzCosmosDBSqlIndexingPolicy `
    -IncludedPath $includedPath `
    -ExcludedPath $indexPathExcluded `
    -IndexingMode Consistent `
    -Automatic $true

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -IndexingPolicy $indexingPolicy
```

### <a name="create-an-azure-cosmos-db-container-with-indexing-turned-off"></a><a id="create-container-no-index"></a>Dizin oluşturma kapalıyken Azure Cosmos DB kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos DB container with no indexing
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

$indexingPolicy = New-AzCosmosDBSqlIndexingPolicy `
    -IndexingMode None

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -IndexingPolicy $indexingPolicy
```

### <a name="create-an-azure-cosmos-db-container-with-unique-key-policy-and-ttl"></a><a id="create-container-unique-key-ttl"></a>Benzersiz anahtar ilkesi ve TTL ile Azure Cosmos DB kapsayıcısı oluşturma

```azurepowershell-interactive
# Create a container with a unique key policy and TTL of one day
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"
$uniqueKeyPath = "/myUniqueKeyPath"
$ttlInSeconds = 86400 # Set this to -1 (or don't use it at all) to never expire

$uniqueKey = New-AzCosmosDBSqlUniqueKey `
    -Path $uniqueKeyPath

$uniqueKeyPolicy = New-AzCosmosDBSqlUniqueKeyPolicy `
    -UniqueKey $uniqueKey

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -UniqueKeyPolicy $uniqueKeyPolicy `
    -TtlInSeconds $ttlInSeconds
```

### <a name="create-an-azure-cosmos-db-container-with-conflict-resolution"></a><a id="create-container-lww"></a>Çakışma çözümüne sahip bir Azure Cosmos DB kapsayıcısı oluşturma

ConflictsFeed 'e yönelik tüm çakışmaları yazmak ve ayrı olarak işlemek için geçirin `-Type "Custom" -Path ""` .

```azurepowershell-interactive
# Create container with last-writer-wins conflict resolution policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"
$conflictResolutionPath = "/myResolutionPath"

$conflictResolutionPolicy = New-AzCosmosDBSqlConflictResolutionPolicy `
    -Type LastWriterWins `
    -Path $conflictResolutionPath

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -ConflictResolutionPolicy $conflictResolutionPolicy
```

Saklı yordam kullanmak üzere bir çakışma çözümleme ilkesi oluşturmak için, parametreleri çağırın `New-AzCosmosDBSqlConflictResolutionPolicy` ve geçirin `-Type` `-ConflictResolutionProcedure` .

```azurepowershell-interactive
# Create container with custom conflict resolution policy using a stored procedure
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"
$conflictResolutionSprocName = "mysproc"

$conflictResolutionSproc = "/dbs/$databaseName/colls/$containerName/sprocs/$conflictResolutionSprocName"

$conflictResolutionPolicy = New-AzCosmosDBSqlConflictResolutionPolicy `
    -Type Custom `
    -ConflictResolutionProcedure $conflictResolutionSproc

New-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -ConflictResolutionPolicy $conflictResolutionPolicy
```


### <a name="list-all-azure-cosmos-db-containers-in-a-database"></a><a id="list-containers"></a>Bir veritabanındaki tüm Azure Cosmos DB kapsayıcıları listeleme

```azurepowershell-interactive
# List all Azure Cosmos DB containers in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Get-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName
```

### <a name="get-a-single-azure-cosmos-db-container-in-a-database"></a><a id="get-container"></a>Veritabanında tek bir Azure Cosmos DB kapsayıcısı edinme

```azurepowershell-interactive
# Get a single Azure Cosmos DB container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"

Get-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName
```

### <a name="delete-an-azure-cosmos-db-container"></a><a id="delete-container"></a>Azure Cosmos DB kapsayıcısını silme

```azurepowershell-interactive
# Delete an Azure Cosmos DB container
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"

Remove-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName
```
### <a name="create-a-resource-lock-on-an-azure-cosmos-db-container-to-prevent-delete"></a><a id="create-container-lock"></a>Silmeyi engellemek için Azure Cosmos DB kapsayıcısı üzerinde bir kaynak kilidi oluşturun

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$resourceType = "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$resourceName = "$accountName/$databaseName/$containerName"
$lockName = "myResourceLock"
$lockLevel = "CanNotDelete"

New-AzResourceLock `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceType `
    -ResourceName $resourceName `
    -LockName $lockName `
    -LockLevel $lockLevel
```

### <a name="remove-a-resource-lock-on-an-azure-cosmos-db-container"></a><a id="remove-container-lock"></a>Azure Cosmos DB kapsayıcısında kaynak kilidini kaldırma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$resourceType = "Microsoft.DocumentDB/databaseAccounts/sqlDatabases/containers"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$resourceName = "$accountName/$databaseName/$containerName"
$lockName = "myResourceLock"

Remove-AzResourceLock `
    -ResourceGroupName $resourceGroupName `
    -ResourceType $resourceType `
    -ResourceName $resourceName `
    -LockName $lockName
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tüm PowerShell örnekleri](powershell-samples.md)
* [Azure Cosmos hesabını yönetme](how-to-manage-database-account.md)
* [Azure Cosmos DB kapsayıcısı oluşturma](how-to-create-container.md)
* [Azure Cosmos DB yaşam süresi yapılandırma](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: /rest/api/cosmos-db-resource-provider/
