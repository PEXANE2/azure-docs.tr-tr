---
title: PowerShell'i kullanarak Azure Cosmos DB oluşturma ve yönetme
description: Azure Powershell'i kullanarak Azure Cosmos hesaplarınızı, veritabanlarınızı, kapsayıcılarınızı ve iş lerinizi yönetin.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/26/2020
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: c8e833a4ba18520d8e354398cfd0d00525594d15
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365755"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>PowerShell'i kullanarak Azure Cosmos DB SQL API kaynaklarını yönetme

Aşağıdaki kılavuzda hesap, veritabanı, kapsayıcı ve aktarım hızı gibi Azure Cosmos DB kaynaklarının yönetimine yönelik betik oluşturmak ve yönetimini otomatikleştirmek için PowerShell’in nasıl kullanılacağı açıklanır.

> [!NOTE]
> Bu makaledeki `Get-AzResource` örnekler, Azure kaynak işlemlerinin yanı sıra `Set-AzResource` [Az.CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) yönetim cmdletleri için Powershell cmdlets kullanır. `Az.CosmosDB`cmdlets hala önizleme ve genel olarak kullanılabilir önce değişebilir. Komutlarda yapılan güncellemeler için [Az.CosmosDB](https://docs.microsoft.com/powershell/module/az.cosmosdb) API başvuru sayfasına bakın.

PowerShell cmdlets kullanılarak `Get-Resource` / `Set-AzResource` yönetilebilen tüm özellikleri görüntülemek için [Azure Cosmos DB kaynak sağlayıcısı şemasına](/azure/templates/microsoft.documentdb/allversions) bakın

Azure Cosmos DB'nin çapraz platform yönetimi `Az` için, `Az.CosmosDB` [platformlar arası Powershell'in](https://docs.microsoft.com/powershell/scripting/install/installing-powershell)yanı sıra [Azure CLI](manage-with-cli.md), [REST API][rp-rest-api]veya [Azure portalı](create-sql-api-dotnet.md#create-account)ile cmdlet'leri kullanabilirsiniz.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Başlarken

Azure [PowerShell'i yükleme ve yapılandırma][powershell-install-configure] ve yükleme ve Powershell'deki Azure hesabınızda oturum açma yönergelerini izleyin.

* `Set-AzureResource`aşağıda kullanılır. Kullanıcı onayı isteyecektir.  Kullanıcı onayı gerektirmeden yürütmeyi tercih ederseniz, `-Force` bayrağı komutana tamamla.

## <a name="azure-cosmos-accounts"></a>Azure Cosmos hesapları

Aşağıdaki bölümler, Aşağıdakiler dahil olmak üzere Azure Cosmos hesabının nasıl yönetilenini gösterir:

* [Azure Cosmos hesabı oluşturma](#create-account)
* [Azure Cosmos hesabını güncelleştirme](#update-account)
* [Abonelikteki tüm Azure Cosmos hesaplarını listelama](#list-accounts)
* [Azure Cosmos hesabı alın](#get-account)
* [Azure Cosmos hesabını silme](#delete-account)
* [Azure Cosmos hesabı için etiketleri güncelleştirme](#update-tags)
* [Azure Cosmos hesabı için liste anahtarları](#list-keys)
* [Azure Cosmos hesabının anahtarlarını yeniden oluşturma](#regenerate-keys)
* [Azure Cosmos hesabı için bağlantı dizelerini listele](#list-connection-strings)
* [Azure Cosmos hesabı için hata önceliğideğiştirme](#modify-failover-priority)
* [Azure Cosmos hesabı için el ile başarısızlamayı tetikleme](#trigger-manual-failover)

### <a name="create-an-azure-cosmos-account"></a><a id="create-account"></a>Azure Cosmos hesabı oluşturma

Bu komut, [birden çok bölge][distribute-data-globally], otomatik hata [ve](how-to-manage-database-account.md#automatic-failover) sınırlı bayatlık [tutarlılık ilkesine](consistency-levels.md)sahip bir Azure Cosmos DB veritabanı hesabı oluşturur.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$locations = @("West US 2", "East US 2")
$accountName = "mycosmosaccount"
$apiKind = "GlobalDocumentDB"
$consistencyLevel = "BoundedStaleness"
$maxStalenessInterval = 300
$maxStalenessPrefix = 100000

New-AzCosmosDBAccount -ResourceGroupName $resourceGroupName `
    -Location $locations -Name $accountName `
    -ApiKind $apiKind -EnableAutomaticFailover:$true `
    -DefaultConsistencyLevel $consistencyLevel `
    -MaxStalenessIntervalInSeconds $maxStalenessInterval `
    -MaxStalenessPrefix $maxStalenessPrefix
```

* `$resourceGroupName`Cosmos hesabını dağıtacak Azure kaynak grubu. Zaten var olmalı.
* `$locations`Veritabanı hesabı için bölgeler, yazma bölgesinden başlayarak ve başarısız öncelik tarafından sıralanır.
* `$accountName`Azure Cosmos hesabının adı. Benzersiz, küçük, yalnızca alfasayısal ve '-' karakterleri içermeli ve 3 ila 31 karakter uzunluğunda olmalıdır.
* `$apiKind`Oluşturulacak Cosmos hesabı türü. Daha fazla bilgi için [Cosmos DB'deki API'ler'e](introduction.md#develop-applications-on-cosmos-db-using-popular-open-source-software-oss-apis)bakın.
* `$consistencyPolicy`, `$maxStalenessInterval`ve `$maxStalenessPrefix` Azure Cosmos hesabının varsayılan tutarlılık düzeyi ve ayarları. Daha fazla bilgi için [Azure Cosmos DB'deki Tutarlılık Düzeyleri'ne](consistency-levels.md)bakın.

Azure Cosmos hesapları IP Güvenlik Duvarı, Sanal Ağ hizmet bitiş noktaları ve özel uç noktalarıyla yapılandırılabilir. Azure Cosmos DB için IP Güvenlik Duvarı'nın nasıl yapılandırılabildiğini öğrenmek için ip [güvenlik duvarını yapılandırın.](how-to-configure-firewall.md) Azure Cosmos DB için hizmet bitiş noktalarını etkinleştirme hakkında bilgi [için](how-to-configure-vnet-service-endpoint.md)bkz. Azure Cosmos DB için özel uç noktaları etkinleştirme hakkında bilgi [için](how-to-configure-private-endpoints.md)bkz.

### <a name="list-all-azure-cosmos-accounts-in-a-resource-group"></a><a id="list-accounts"></a>Kaynak Grubundaki tüm Azure Cosmos hesaplarını listelama

Bu komut, Kaynak Grubu'ndaki tüm Azure Cosmos hesaplarını listeler.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"

Get-AzCosmosDBAccount -ResourceGroupName $resourceGroupName
```

### <a name="get-the-properties-of-an-azure-cosmos-account"></a><a id="get-account"></a>Azure Cosmos hesabının özelliklerini alma

Bu komut, varolan bir Azure Cosmos hesabının özelliklerini almanızı sağlar.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccount -ResourceGroupName $resourceGroupName -Name $accountName
```

### <a name="update-an-azure-cosmos-account"></a><a id="update-account"></a>Azure Cosmos hesabını güncelleştirme

Bu komut, Azure Cosmos DB veritabanı hesap özelliklerinizi güncelleştirmenize olanak tanır. Güncelleştirilebilen özellikler şunlardır:

* Bölge ekleme veya kaldırma
* Varsayılan tutarlılık ilkesini değiştirme
* IP Aralığı Filtresini Değiştirme
* Sanal Ağ yapılandırmalarını değiştirme
* Çoklu Master'ı etkinleştirme

> [!NOTE]
> Bir Azure Cosmos hesabı `locations` için aynı anda bölgeler ekleyemez, kaldıramaz ve diğer özellikleri değiştiremezsiniz. Değiştirerek bölgeler, hesaptaki diğer değişikliklerden ayrı bir işlem olarak gerçekleştirilmelidir.
> [!NOTE]
> Bu komut, bölgeler eklemenize ve kaldırmanıza izin verir, ancak hata yerine etme önceliklerini değiştirmenize veya el ile başarısız olmayı tetiklemenize izin vermez. Bkz. [Değiştirme failover önceliği](#modify-failover-priority) ve [Tetikleme kılavuzu failover](#trigger-manual-failover).

```azurepowershell-interactive
# Create account with two regions
$resourceGroupName = "myResourceGroup"
$locations = @("West US 2", "East US 2")
$accountName = "mycosmosaccount"
$apiKind = "GlobalDocumentDB"
$consistencyLevel = "Session"
$enableAutomaticFailover = $true

New-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Location $locations -Name $accountName `
    -ApiKind $apiKind -EnableAutomaticFailover:$enableAutomaticFailover `
    -DefaultConsistencyLevel $consistencyLevel

# Region operations
$resourceType = "Microsoft.DocumentDb/databaseAccounts"
$apiVersion = "2020-03-01"

# Add a region to the account
$locations2 = @("West US 2", "East US 2", "South Central US")
$locationObjects2 = @()
$i = 0
ForEach ($location in $locations2) { $locationObjects2 += @{ locationName = "$location"; failoverPriority = $i++ } }
$accountProperties = @{
    databaseAccountOfferType = "Standard";
    locations = $locationObjects2;
    enableAutomaticFailover = $enableAutomaticFailover;
}

Set-AzResource -ResourceType $resourceType `
    -ResourceGroupName $resourceGroupName `
    -ApiVersion $apiVersion -Name $accountName `
    -PropertyObject $accountProperties

Write-Host "Set-AzResource returns before the region update is complete."
Write-Host "Check account in Azure portal or using Get-AzCosmosDBAccount for region status."
Write-Host "When region was added, press any key to continue."
$HOST.UI.RawUI.ReadKey("NoEcho,IncludeKeyDown") | OUT-NULL
$HOST.UI.RawUI.Flushinputbuffer()

# Remove a region from the account
$locations3 = @("West US 2", "South Central US")
$locationObjects3 = @()
$i = 0
ForEach ($location in $locations3) { $locationObjects3 += @{ locationName = "$location"; failoverPriority = $i++ } }
$accountProperties = @{
    databaseAccountOfferType = "Standard";
    locations = $locationObjects3;
    enableAutomaticFailover = $enableAutomaticFailover;
}

Set-AzResource -ResourceType $resourceType `
    -ResourceGroupName $resourceGroupName `
    -ApiVersion $apiVersion -Name $accountName `
    -PropertyObject $accountProperties

Write-Host "Set-AzResource returns before the region update is complete."
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

Bu komut, varolan bir Azure Cosmos hesabını siler.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName -PassThru
```

### <a name="update-tags-of-an-azure-cosmos-account"></a><a id="update-tags"></a>Azure Cosmos hesabının Etiketlerini Güncelleştirme

Bu komut, Azure Cosmos hesabı için [Azure kaynak etiketlerini][azure-resource-tags] ayarlar. Etiketler hem hesap oluşturma da `New-AzCosmosDBAccount` kullanarak hem de `Update-AzCosmosDBAccount`hesap güncelleştirmesi kullanılarak ayarlanabilir.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$tags = @{dept = "Finance"; environment = "Production";}

Update-AzCosmosDBAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName -Tag $tags
```

### <a name="list-account-keys"></a><a id="list-keys"></a>Hesap Anahtarlarını Listele

Bir Azure Cosmos hesabı oluşturduğunuzda, hizmet Azure Cosmos hesabına erişildiğinde kimlik doğrulama için kullanılabilecek iki ana erişim anahtarı oluşturur. Salt okunur işlemleri doğrulamak için salt okunur tuşları da oluşturulur.
Azure Cosmos DB, iki erişim anahtarı sağlayarak Azure Cosmos hesabınızda kesintiye uğramadan bir tuşu aynı anda yeniden oluşturmanızı ve döndürmenizi sağlar.
Cosmos DB hesaplarında iki okuma-yazma anahtarı (birincil ve ikincil) ve iki salt okunur anahtarı (birincil ve ikincil) bulunur.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName -Type "Keys"
```

### <a name="list-connection-strings"></a><a id="list-connection-strings"></a>Bağlantı Dizelerini Listele

Aşağıdaki komut, uygulamaları Cosmos DB hesabına bağlamak için bağlantı dizelerini alır.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName -Type "ConnectionStrings"
```

### <a name="regenerate-account-keys"></a><a id="regenerate-keys"></a>Hesap Anahtarlarını Yeniden Oluştur

Bir Azure Cosmos hesabının erişim anahtarları, bağlantıların güvenliğini sağlamaya yardımcı olmak için düzenli aralıklarla yeniden oluşturulmalıdır. Hesaba birincil ve ikincil erişim anahtarları atanır. Bu, istemcilerin her seferinde bir anahtar yeniden oluşturulurken erişimi korumasını sağlar.
Bir Azure Cosmos hesabı için dört tür anahtar vardır (Birincil, İkincil, Birincil Okuma ve İkincil Readonly)

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup" # Resource Group must already exist
$accountName = "mycosmosaccount" # Must be all lower case
$keyKind = "primary" # Other key kinds: secondary, primaryReadOnly, secondaryReadOnly

New-AzCosmosDBAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName -KeyKind $keyKind
```

### <a name="enable-automatic-failover"></a><a id="enable-automatic-failover"></a>Otomatik arızayı etkinleştirme

Aşağıdaki komut, birincil bölge kullanılamaz hale gelmesi halinde bir Cosmos DB hesabını ikincil bölgesine otomatik olarak geçememesini ayarlar.

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

### <a name="modify-failover-priority"></a><a id="modify-failover-priority"></a>Failover Önceliğini Değiştir

Otomatik Failover ile yapılandırılan hesaplar için, birincil kullanılamaz hale gelmesi durumunda Cosmos birincil ikincil yinelemeler teşvik edecek sırasını değiştirebilirsiniz.

Aşağıdaki örnekte, geçerli başarısız lık `West US 2 = 0`önceliğinin `South Central US = 2`, , `East US 2 = 1`. Komut bunu `West US 2 = 0`, . `South Central US = 1` `East US 2 = 2`

> [!CAUTION]
> Konumu değiştirmek, `failoverPriority=0` Azure Cosmos hesabı için el ile başarısızlığa neden olur. Diğer öncelik değişiklikleri bir başarısızlık tetiklemez.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$locations = @("West US 2", "South Central US", "East US 2") # Regions ordered by UPDATED failover priority

Update-AzCosmosDBAccountFailoverPriority `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -FailoverPolicy $locations
```

### <a name="trigger-manual-failover"></a><a id="trigger-manual-failover"></a>Tetik Manuel Failover

Manuel Failover ile yapılandırılan hesaplar için, üzerinde başarısız olabilir ve değiştirerek birincil herhangi bir ikincil yineleme teşvik `failoverPriority=0`. Bu işlem, olağanüstü durum kurtarma planlamasını test etmek için bir olağanüstü durum kurtarma matkabı başlatmak için kullanılabilir.

Aşağıdaki örnekte, hesabın geçerli bir başarısızlık `West US 2 = 0` önceliği `East US 2 = 1` olduğunu varsayalım ve bölgeleri çevirin.

> [!CAUTION]
> `locationName` Değiştirme, `failoverPriority=0` Azure Cosmos hesabı için el ile başarısızlığa neden olur. Diğer öncelik değişikliği bir başarısızlık tetiklemez.

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$locations = @("East US 2", "West US 2") # Regions ordered by UPDATED failover priority

Update-AzCosmosDBAccountFailoverPriority `
    -ResourceGroupName $resourceGroupName `
    -Name $accountName `
    -FailoverPolicy $locations
```

## <a name="azure-cosmos-db-database"></a>Azure Cosmos DB Veritabanı

Aşağıdaki bölümler, aşağıdakiler de dahil olmak üzere Azure Cosmos DB veritabanının nasıl yönetilenini gösterir:

* [Azure Cosmos DB veritabanı oluşturma](#create-db)
* [Paylaşılan iş ortası ile bir Azure Cosmos DB veritabanı oluşturma](#create-db-ru)
* [Azure Cosmos DB veritabanının iş verisini edinin](#get-db-ru)
* [Tüm Azure Cosmos DB veritabanlarını bir hesapta listele](#list-db)
* [Tek bir Azure Cosmos DB veritabanı edinin](#get-db)
* [Azure Cosmos DB veritabanını silme](#delete-db)

### <a name="create-an-azure-cosmos-db-database"></a><a id="create-db"></a>Azure Cosmos DB veritabanı oluşturma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Set-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="create-an-azure-cosmos-db-database-with-shared-throughput"></a><a id="create-db-ru"></a>Paylaşılan iş ortası ile bir Azure Cosmos DB veritabanı oluşturma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$databaseRUs = 400

Set-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName `
    -Throughput $databaseRUs
```

### <a name="get-the-throughput-of-an-azure-cosmos-db-database"></a><a id="get-db-ru"></a>Azure Cosmos DB veritabanının iş verisini edinin

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"

Get-AzCosmosDBSqlDatabaseThroughput `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -Name $databaseName
```

### <a name="get-all-azure-cosmos-db-databases-in-an-account"></a><a id="list-db"></a>Tüm Azure Cosmos DB veritabanlarını bir hesapta edinin

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzCosmosDBSqlDatabase `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName
```

### <a name="get-a-single-azure-cosmos-db-database"></a><a id="get-db"></a>Tek bir Azure Cosmos DB veritabanı edinin

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

## <a name="azure-cosmos-db-container"></a>Azure Cosmos DB Konteyner

Aşağıdaki bölümler, Aşağıdakiler dahil olmak üzere Azure Cosmos DB kapsayıcısının nasıl yönetilenini gösterir:

* [Azure Cosmos DB kapsayıcısı oluşturma](#create-container)
* [Büyük bölme anahtarına sahip bir Azure Cosmos DB kapsayıcısı oluşturma](#create-container-big-pk)
* [Azure Cosmos DB kapsayıcısının ipini alın](#get-container-ru)
* [Özel dizin oluşturma ile bir Azure Cosmos DB kapsayıcısı oluşturma](#create-container-custom-index)
* [Dizin oluşturma kapalı bir Azure Cosmos DB kapsayıcısı oluşturma](#create-container-no-index)
* [Benzersiz anahtar ve TTL ile bir Azure Cosmos DB kapsayıcı sı](#create-container-unique-key-ttl)
* [Çakışma çözümlemesi olan bir Azure Cosmos DB kapsayıcısı oluşturma](#create-container-lww)
* [Tüm Azure Cosmos DB kapsayıcılarını bir veritabanında listele](#list-containers)
* [Veritabanında tek bir Azure Cosmos DB kapsayıcısı edinin](#get-container)
* [Azure Cosmos DB kapsayıcısı silme](#delete-container)

### <a name="create-an-azure-cosmos-db-container"></a><a id="create-container"></a>Azure Cosmos DB kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos DB container with default indexes and throughput at 400 RU
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

Set-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath
```

### <a name="create-an-azure-cosmos-db-container-with-a-large-partition-key-size"></a><a id="create-container-big-pk"></a>Büyük bölüm anahtar boyutuna sahip bir Azure Cosmos DB kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos DB container with a large partition key value (version = 2)
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

Set-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -PartitionKeyVersion 2
```

### <a name="get-the-throughput-of-an-azure-cosmos-db-container"></a><a id="get-container-ru"></a>Azure Cosmos DB kapsayıcısının ipini alın

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

Set-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -IndexingPolicy $indexingPolicy
```

### <a name="create-an-azure-cosmos-db-container-with-indexing-turned-off"></a><a id="create-container-no-index"></a>Dizin oluşturma kapalı bir Azure Cosmos DB kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos DB container with no indexing
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "myDatabase"
$containerName = "myContainer"
$partitionKeyPath = "/myPartitionKey"

$indexingPolicy = New-AzCosmosDBSqlIndexingPolicy `
    -IndexingMode None

Set-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -IndexingPolicy $indexingPolicy
```

### <a name="create-an-azure-cosmos-db-container-with-unique-key-policy-and-ttl"></a><a id="create-container-unique-key-ttl"></a>Benzersiz anahtar ilkesi ve TTL ile bir Azure Cosmos DB kapsayıcısı oluşturun

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

Set-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -UniqueKeyPolicy $uniqueKeyPolicy `
    -TtlInSeconds $ttlInSeconds
```

### <a name="create-an-azure-cosmos-db-container-with-conflict-resolution"></a><a id="create-container-lww"></a>Çakışma çözümlemesi olan bir Azure Cosmos DB kapsayıcısı oluşturma

Depolanan yordamı kullanmak için çakışma `"mode"="custom"` çözümlemesi ilkesi oluşturmak için çözüm yolunu `"conflictResolutionPath"="myResolverStoredProcedure"`depolanan yordamın adı olarak ayarlayın ve ayarlayın. Tüm çakışmaları ConflictsFeed'e yazmak ve `"mode"="custom"` ayrı ayrı işlemek için,`"conflictResolutionPath"=""`

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

Set-AzCosmosDBSqlContainer `
    -ResourceGroupName $resourceGroupName `
    -AccountName $accountName `
    -DatabaseName $databaseName `
    -Name $containerName `
    -PartitionKeyKind Hash `
    -PartitionKeyPath $partitionKeyPath `
    -ConflictResolutionPolicy $conflictResolutionPolicy
```

### <a name="list-all-azure-cosmos-db-containers-in-a-database"></a><a id="list-containers"></a>Tüm Azure Cosmos DB kapsayıcılarını bir veritabanında listele

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

### <a name="get-a-single-azure-cosmos-db-container-in-a-database"></a><a id="get-container"></a>Veritabanında tek bir Azure Cosmos DB kapsayıcısı edinin

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

### <a name="delete-an-azure-cosmos-db-container"></a><a id="delete-container"></a>Azure Cosmos DB kapsayıcısı silme

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

## <a name="next-steps"></a>Sonraki adımlar

* [Tüm PowerShell Örnekleri](powershell-samples.md)
* [Azure Cosmos hesabı nasıl yönetilir?](how-to-manage-database-account.md)
* [Azure Cosmos DB kapsayıcısı oluşturma](how-to-create-container.md)
* [Azure Cosmos DB'de canlı zamanı yapılandırma](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
