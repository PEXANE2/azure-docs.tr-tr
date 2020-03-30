---
title: Azure PowerShell ile Redis için Azure Önbelleğini Yönetme
description: Azure PowerShell'i kullanarak Redis için Azure Önbelleği için yönetim görevlerini nasıl gerçekleştireceğinizi öğrenin.
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 07/13/2017
ms.author: yegu
ms.openlocfilehash: a385d3ed7ef46389f96de72c98ffc29cebf60ec4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278538"
---
# <a name="manage-azure-cache-for-redis-with-azure-powershell"></a>Azure PowerShell ile Redis için Azure Önbelleğini Yönetme
> [!div class="op_single_selector"]
> * [Powershell](cache-how-to-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
> 
> 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu konu, Redis örnekleri için Azure Önbelleği oluşturma, güncelleştirme ve ölçeklendirme gibi yaygın görevleri nasıl gerçekleştireceğiniz, erişim anahtarlarını nasıl yeniden oluşturacağınızı ve önbellekleriniz hakkındaki bilgileri nasıl görüntüleyeceğiniz gösterir. Redis PowerShell cmdlets için Azure Önbelleğinin tam listesi [için Redis cmdlets için Azure Önbelleği'ne](https://docs.microsoft.com/powershell/module/az.rediscache)bakın.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Klasik dağıtım modeli hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi ve klasik dağıtım: Dağıtım modellerini ve kaynaklarınızın durumunu anlayın.](../azure-resource-manager/management/deployment-models.md)

## <a name="prerequisites"></a>Ön koşullar
Azure PowerShell'i zaten yüklediyseniz, Azure PowerShell sürüm 1.0.0 veya daha sonra olmalıdır. Bu komutla yüklediğiniz Azure PowerShell sürümünü Azure PowerShell komut isteminde denetleyebilirsiniz.

    Get-Module Az | format-table version


Öncelikle bu komutla Azure'da oturum açmanız gerekir.

    Connect-AzAccount

Azure hesabınızın e-posta adresini ve parolasını Microsoft Azure oturum açma iletişim kutusunda belirtin.

Ardından, birden çok Azure aboneliğiniz varsa, Azure aboneliğinizi ayarlamanız gerekir. Geçerli aboneliklerinizin listesini görmek için bu komutu çalıştırın.

    Get-AzSubscription | sort SubscriptionName | Select SubscriptionName

Aboneliği belirtmek için aşağıdaki komutu çalıştırın. Aşağıdaki örnekte, abonelik `ContosoSubscription`adı.

    Select-AzSubscription -SubscriptionName ContosoSubscription

Windows PowerShell'i Azure Kaynak Yöneticisi ile kullanabilmeniz için önce aşağıdakilere ihtiyacınız vardır:

* Windows PowerShell, Sürüm 3.0 veya 4.0. Windows PowerShell sürümünü bulmak için`$PSVersionTable` yazın: ve `PSVersion` değerini doğrulamak 3.0 veya 4.0. Uyumlu bir sürümünü yüklemek için [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) veya [Windows Management Framework 4.0'a](https://www.microsoft.com/download/details.aspx?id=40855)bakın.

Bu eğitimde gördüğünüz herhangi bir cmdlet için detaylı yardım almak için Yardım Al cmdlet'i kullanın.

    Get-Help <cmdlet-name> -Detailed

Örneğin, `New-AzRedisCache` cmdlet için yardım almak için şunları yazın:

    Get-Help New-AzRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a>Diğer bulutlara nasıl bağlanır?
Varsayılan olarak Azure `AzureCloud`ortamı, genel Azure bulut örneğini temsil eder. Farklı bir örne bağlanmak `Connect-AzAccount` için, `-Environment` istenilen`EnvironmentName` ortam veya ortam adı ile veya - komut satırı anahtarı ile komutu kullanın.

Kullanılabilir ortamların listesini görmek için `Get-AzEnvironment` cmdlet'i çalıştırın.

### <a name="to-connect-to-the-azure-government-cloud"></a>Azure Devlet Bulutu'na bağlanmak için
Azure Genel Kullanıma Kullanım Bulutu'na bağlanmak için aşağıdaki komutlardan birini kullanın.

    Connect-AzAccount -EnvironmentName AzureUSGovernment

or

    Connect-AzAccount -Environment (Get-AzEnvironment -Name AzureUSGovernment)

Azure Kamu Bulutu'nda önbellek oluşturmak için aşağıdaki konumlardan birini kullanın.

* USGov Virginia
* USGov Iowa

Azure Kamu Bulutu hakkında daha fazla bilgi için [Microsoft Azure Kamu Ve](https://azure.microsoft.com/features/gov/) Microsoft Azure [Genel Geliştirici Kılavuzu'na](../azure-government-developer-guide.md)bakın.

### <a name="to-connect-to-the-azure-china-cloud"></a>Azure China Cloud'a bağlanmak için
Azure China Cloud'a bağlanmak için aşağıdaki komutlardan birini kullanın.

    Connect-AzAccount -EnvironmentName AzureChinaCloud

or

    Connect-AzAccount -Environment (Get-AzEnvironment -Name AzureChinaCloud)

Azure Çin Bulutu'nda önbellek oluşturmak için aşağıdaki konumlardan birini kullanın.

* Doğu Çin
* Kuzey Çin

Azure China Cloud hakkında daha fazla bilgi için, [Çin'de 21Vianet tarafından işletilen Azure için Azure ChinaCloud'a](https://www.windowsazure.cn/)bakın.

### <a name="to-connect-to-microsoft-azure-germany"></a>Microsoft Azure Almanya'ya bağlanmak için
Microsoft Azure Almanya'ya bağlanmak için aşağıdaki komutlardan birini kullanın.

    Connect-AzAccount -EnvironmentName AzureGermanCloud


or

    Connect-AzAccount -Environment (Get-AzEnvironment -Name AzureGermanCloud)

Microsoft Azure Almanya'da önbellek oluşturmak için aşağıdaki konumlardan birini kullanın.

* Orta Almanya
* Kuzeydoğu Almanya

Microsoft Azure Almanya hakkında daha fazla bilgi için [Microsoft Azure Almanya'ya](https://azure.microsoft.com/overview/clouds/germany/)bakın.

### <a name="properties-used-for-azure-cache-for-redis-powershell"></a>Redis PowerShell için Azure Önbelleği için kullanılan özellikler
Aşağıdaki tablo, Azure PowerShell'i kullanarak Redis örnekleri için Azure Önbelleğinizi oluştururken ve yönetirken sık kullanılan parametrelere yönelik özellikler ve açıklamalar içerir.

| Parametre | Açıklama | Varsayılan |
| --- | --- | --- |
| Adı |Önbelleğin adı | |
| Konum |Önbelleğin konumu | |
| ResourceGroupName |Önbelleği oluşturmak için kaynak grubu adı | |
| Boyut |Önbelleğin boyutu. Geçerli değerler şunlardır: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB |1 GB |
| ShardCount |Kümeleme etkin bir premium önbellek oluştururken oluşturulacak parça sayısı. Geçerli değerler şunlardır: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Önbelleğin SKU'su belirtilir. Geçerli değerler şunlardır: Temel, Standart, Premium |Standart |
| RedisConfiguration |Redis yapılandırma ayarlarını belirtir. Her ayarla ilgili ayrıntılar için aşağıdaki [RedisConfiguration özellikleri](#redisconfiguration-properties) tablosuna bakın. | |
| EnableNonSslPort |SSL olmayan bağlantı noktasının etkin olup olmadığını gösterir. |False |
| MaxMemoryPolitikası |Bu parametre amortismana kaldırıldı - bunun yerine RedisConfiguration kullanın. | |
| StatikIP |Önbelleğinizi Bir VNET'te barındırırken, önbellek için alt ağda benzersiz bir IP adresi belirtir. Sağlanmazsa, alt ağdan sizin için bir tane seçilir. | |
| Alt ağ |Önbelleğinizi Bir VNET'te barındırırken, önbelleği dağıtmak için alt netin adını belirtir. | |
| VirtualNetwork |Önbelleğinizi Bir VNET'te barındırırken, önbelleği dağıtacağınız VNET'in kaynak kimliğini belirtir. | |
| Keytype |Erişim anahtarlarını yenilerken hangi erişim anahtarının yeniden oluşturacağı nızı belirtir. Geçerli değerler şunlardır: Birincil, İkincil | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration özellikleri
| Özellik | Açıklama | Fiyatlandırma katmanları |
| --- | --- | --- |
| rdb-yedekleme etkin |[Redis veri kalıcılığı](cache-how-to-premium-persistence.md) etkin olup olmadığını |Yalnızca Premium |
| rdb-depolama-bağlantı-dize |[Redis veri kalıcılığı](cache-how-to-premium-persistence.md) için depolama hesabına bağlantı dizesi |Yalnızca Premium |
| rdb-yedekleme-frekans |[Redis veri kalıcılığı](cache-how-to-premium-persistence.md) için yedekleme frekansı |Yalnızca Premium |
| maxmemory-ayrılmış |Önbellek dışı işlemler için [ayrılmış belleği](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) yapılandırır |Standart ve Premium |
| maxmemory-ilke |Önbellek için [çıkarma ilkesini](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) yapılandırır |Tüm fiyatlandırma katmanları |
| notify-keyspace-olayları |Anahtar [alanı bildirimlerini](cache-configure.md#keyspace-notifications-advanced-settings) yapılandırma |Standart ve Premium |
| hash-max-ziplist-girişleri |Küçük toplam veri türleri için [bellek optimizasyonu](https://redis.io/topics/memory-optimization) yapılandırır |Standart ve Premium |
| hash-max-ziplist-değeri |Küçük toplam veri türleri için [bellek optimizasyonu](https://redis.io/topics/memory-optimization) yapılandırır |Standart ve Premium |
| set-max-intset-girişleri |Küçük toplam veri türleri için [bellek optimizasyonu](https://redis.io/topics/memory-optimization) yapılandırır |Standart ve Premium |
| zset-max-ziplist-girişleri |Küçük toplam veri türleri için [bellek optimizasyonu](https://redis.io/topics/memory-optimization) yapılandırır |Standart ve Premium |
| zset-max-ziplist-değeri |Küçük toplam veri türleri için [bellek optimizasyonu](https://redis.io/topics/memory-optimization) yapılandırır |Standart ve Premium |
| veritabanları |Veritabanlarının sayısını yapılandırır. Bu özellik yalnızca önbellek oluşturma da yapılandırılabilir. |Standart ve Premium |

## <a name="to-create-an-azure-cache-for-redis"></a>Redis için Bir Azure Önbelleği oluşturmak için
Redis örnekleri için yeni Azure Önbelleği, [Yeni-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/new-azrediscache) cmdlet kullanılarak oluşturulur.

> [!IMPORTANT]
> Azure portalını kullanarak bir abonelikte Redis için ilk kez bir Azure `Microsoft.Cache` Önbelleği oluşturduğunuzda, portal bu abonelik için ad alanını kaydeder. PowerShell'i kullanarak bir abonelikte Redis için ilk Azure Önbelleğini oluşturmaya çalışırsanız, öncelikle bu ad alanını aşağıdaki komutu kullanarak kaydetmeniz gerekir; aksi takdirde cmdlets gibi `New-AzRedisCache` ve `Get-AzRedisCache` başarısız.
> 
> `Register-AzResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

Kullanılabilir parametrelerin listesini ve açıklamalarını `New-AzRedisCache`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help New-AzRedisCache -detailed

    NAME
        New-AzRedisCache

    SYNOPSIS
        Creates a new Azure Cache for Redis.


    SYNTAX
        New-AzRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzRedisCache cmdlet creates a new Azure Cache for Redis.


    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the Azure Cache for Redis.

        -Location <String>
            Location in which to create the Azure Cache for Redis.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the Azure Cache for Redis. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of Azure Cache for Redis. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Cache for Redis. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the Azure Cache for Redis in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Varsayılan parametreleriçeren bir önbellek oluşturmak için aşağıdaki komutu çalıştırın.

    New-AzRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, `Location` ve gerekli parametreler, ancak geri kalanı isteğe bağlıdır ve varsayılan değerlere sahip. Önceki komutu çalıştırmak, belirtilen ad, konum ve kaynak grubu yla Redis için ssl olmayan bağlantı noktası devre dışı bırakılmış 1 GB boyutunda bir Standart SKU Azure Önbelleği oluşturur.

Premium önbellek oluşturmak için P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), P3 (26 GB - 260 GB) veya P4 (53 GB - 530 GB) boyutu belirtin. Kümelemayı etkinleştirmek için, parametreyi kullanarak bir parça sayısı belirtin. `ShardCount` Aşağıdaki örnekte, 3 parçadan ilerlene bir P1 premium önbellek oluşturulur. Bir P1 premium önbellek boyutu 6 GB, ve biz üç parça belirtildiği için toplam boyutu 18 GB (3 x 6 GB).

    New-AzRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

`RedisConfiguration` Parametre için değer belirtmek için, içindeki `{}` değerleri ' nin `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`anahtar/değer çiftleri gibi. Aşağıdaki örnek, `allkeys-random` maxmemory ilkesi ve anahtar alanı bildirimleri ile `KEA`yapılandırılan standart bir 1 GB önbellek oluşturur. Daha fazla bilgi için [Keyspace bildirimleri (gelişmiş ayarlar)](cache-configure.md#keyspace-notifications-advanced-settings) ve [Bellek ilkelerine](cache-configure.md#memory-policies)bakın.

    New-AzRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Önbellek oluşturma sırasında veritabanları ayarını yapılandırmak için
Ayar `databases` yalnızca önbellek oluşturma sırasında yapılandırılabilir. Aşağıdaki örnek, [Yeni-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/New-azRedisCache) cmdlet kullanarak 48 veritabanları ile prim P3 (26 GB) önbellek oluşturur.

    New-AzRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

`databases` Özellik hakkında daha fazla bilgi için [Redis sunucu yapılandırması için Varsayılan Azure Önbelleği'ne](cache-configure.md#default-redis-server-configuration)bakın. [Yeni-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/new-azrediscache) cmdlet kullanarak önbellek oluşturma hakkında daha fazla bilgi için önceki Redis için Bir Azure Önbelleği oluşturma bölümüne bakın.

## <a name="to-update-an-azure-cache-for-redis"></a>Redis için Azure Önbelleğini güncelleştirmek için
Redis örnekleri için Azure Önbelleği, [Set-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/Set-azRedisCache) cmdlet kullanılarak güncelleştirilir.

Kullanılabilir parametrelerin listesini ve açıklamalarını `Set-AzRedisCache`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Set-AzRedisCache -detailed

    NAME
        Set-AzRedisCache

    SYNOPSIS
        Set Azure Cache for Redis updatable parameters.

    SYNTAX
        Set-AzRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzRedisCache cmdlet sets Azure Cache for Redis parameters.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the Azure Cache for Redis. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of Azure Cache for Redis. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Cache for Redis. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Cmdlet, `Set-AzRedisCache` `Size`, , `Sku` `EnableNonSslPort`, ve değerleri gibi `RedisConfiguration` özellikleri güncelleştirmek için kullanılabilir. 

Aşağıdaki komut, MyCache adlı Redis için Azure Önbelleği için maksimum bellek ilkesini güncelleştirir.

    Set-AzRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-an-azure-cache-for-redis"></a>Redis için Azure Önbelleğini ölçeklendirmek için
`Set-AzRedisCache`, `Size`, `Sku`veya `ShardCount` özellikleri değiştirildiğinde Redis örneği için bir Azure Önbelleği ölçeklendirmek için kullanılabilir. 

> [!NOTE]
> PowerShell kullanarak önbelleği ölçekleme, Azure portalından bir önbelleği ölçeklemeyle aynı sınırlara ve yönergelere tabidir. Aşağıdaki kısıtlamalarla farklı bir fiyatlandırma katmanına ölçeklendirebilirsiniz.
> 
> * Daha yüksek bir fiyatlandırma katmanından daha düşük bir fiyatlandırma katmanına ölçeklendiremezsiniz.
> * **Premium** önbellekten **Standarda** veya **Temel** önbelleğe ölçeklendiremezsiniz.
> * **Standart** önbellekten **Temel** önbelleğe ölçeklendiremezsiniz.
> * **Temel** önbellekten **Standart** önbelleğe ölçeklendirebilirsiniz, ancak boyutu aynı anda değiştiremezsiniz. Farklı bir boyuta ihtiyacınız varsa, sonraki ölçekleme işlemini istediğiniz boyutta yapabilirsiniz.
> * **Temel** önbellekten doğrudan **Premium** önbelleğine ölçeklendiremezsiniz. Bir ölçeklendirme işleminde **Temel'den** **Standard'a,** ardından bir ölçeklendirme işleminde de **Standard'dan** **Premium'a** ölçeklendirme niz gerekir.
> * Daha büyük bir boyuttan **C0 (250 MB)** boyutuna kadar ölçeklendiremezsiniz.
> 
> Daha fazla bilgi için [Redis için Azure Önbelleğini niçin ölçeklendireceğini](cache-how-to-scale.md)öğrenin.
> 
> 

Aşağıdaki örnek, 2,5 GB `myCache` önbelleğe adlandırılmış bir önbelleğe nasıl ölçeklendirilir gösterilmektedir. Bu komutun hem Temel hem de Standart önbellek için çalıştığını unutmayın.

    Set-AzRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Bu komut verildikten sonra önbelleğin durumu döndürülür `Get-AzRedisCache`(aramaya benzer). `ProvisioningState` Olduğunu `Scaling`unutmayın.

    PS C:\> Set-AzRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Ölçekleme işlemi tamamlandığında, '' `ProvisioningState` ye `Succeeded`göre değişir. Temel'den Standard'a değiştirme ve ardından boyutu değiştirme gibi sonraki bir ölçekleme işlemi yapmanız gerekiyorsa, önceki işlemin tamamlanmasını beklemeniz veya aşağıdakilere benzer bir hata almanız gerekir.

    Set-AzRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-an-azure-cache-for-redis"></a>Redis için Azure Önbelleği hakkında bilgi almak için
[Get-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/get-azrediscache) cmdlet'i kullanarak önbellek hakkında bilgi alabilirsiniz.

Kullanılabilir parametrelerin listesini ve açıklamalarını `Get-AzRedisCache`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Get-AzRedisCache -detailed

    NAME
        Get-AzRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Geçerli abonelikteki tüm önbellekler hakkında `Get-AzRedisCache` bilgi döndürmek için herhangi bir parametre olmadan çalıştırın.

    Get-AzRedisCache

Belirli bir kaynak grubundaki tüm önbellekler `Get-AzRedisCache` hakkında `ResourceGroupName` bilgi döndürmek için parametreyle çalıştırın.

    Get-AzRedisCache -ResourceGroupName myGroup

Belirli bir önbellek hakkında bilgi `Get-AzRedisCache` döndürmek `Name` için, önbelleğin adını içeren `ResourceGroupName` parametre ve bu önbelleği içeren kaynak grubuyla parametreile çalıştırın.

    PS C:\> Get-AzRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-an-azure-cache-for-redis"></a>Redis için Azure Önbelleği için erişim anahtarlarını almak için
Önbelleğinizin erişim anahtarlarını almak için [Get-AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/Get-azRedisCacheKey) cmdlet'ini kullanabilirsiniz.

Kullanılabilir parametrelerin listesini ve açıklamalarını `Get-AzRedisCacheKey`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Get-AzRedisCacheKey -detailed

    NAME
        Get-AzRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified Azure Cache for Redis.


    SYNTAX
        Get-AzRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Önbelleğinizin anahtarlarını almak `Get-AzRedisCacheKey` için cmdlet'i arayın ve önbelleğinizin adını önbelleğinizin adını geçirin.

    PS C:\> Get-AzRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-azure-cache-for-redis"></a>Redis için Azure Önbelleğiniz için erişim anahtarlarını yeniden oluşturmak için
Önbelleğinizin erişim anahtarlarını yeniden oluşturmak için [Yeni-AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/New-azRedisCacheKey) cmdlet'ini kullanabilirsiniz.

Kullanılabilir parametrelerin listesini ve açıklamalarını `New-AzRedisCacheKey`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help New-AzRedisCacheKey -detailed

    NAME
        New-AzRedisCacheKey

    SYNOPSIS
        Regenerates the access key of an Azure Cache for Redis.

    SYNTAX
        New-AzRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzRedisCacheKey cmdlet regenerate the access key of an Azure Cache for Redis.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Önbelleğiniz için birincil veya ikincil anahtarı `New-AzRedisCacheKey` yeniden oluşturmak için cmdlet'i arayın ve `Primary` `Secondary` ad, `KeyType` kaynak grubu olarak geçirin ve parametre den birini veya parametreyi belirtin. Aşağıdaki örnekte, önbellek için ikincil erişim anahtarı yeniden oluşturulur.

    PS C:\> New-AzRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for Azure Cache for Redis 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-an-azure-cache-for-redis"></a>Redis için bir Azure Önbelleği silmek için
Redis için bir Azure Önbelleği silmek için [Kaldır-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/remove-azrediscache) cmdlet'ini kullanın.

Kullanılabilir parametrelerin listesini ve açıklamalarını `Remove-AzRedisCache`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Remove-AzRedisCache -detailed

    NAME
        Remove-AzRedisCache

    SYNOPSIS
        Remove Azure Cache for Redis if exists.

    SYNTAX
        Remove-AzRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzRedisCache cmdlet removes an Azure Cache for Redis if it exists.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Aşağıdaki örnekte, adlı `myCache` önbellek kaldırılır.

    PS C:\> Remove-AzRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove Azure Cache for Redis 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-an-azure-cache-for-redis"></a>Redis için Azure Önbelleği almak için
`Import-AzRedisCache` Cmdlet'i kullanarak verileri Redis için Bir Azure Önbelleğine aktarabilirsiniz.

> [!IMPORTANT]
> Alma/Dışa Aktarma yalnızca [premium katman](cache-premium-tier-intro.md) önbellekleri için kullanılabilir. İçe Aktarma/Dışa Aktarma hakkında daha fazla bilgi [için, Redis için Azure Önbelleğinde İçe Aktarma ve Dışa Aktarma verilerine](cache-how-to-import-export-data.md)bakın.
> 
> 

Kullanılabilir parametrelerin listesini ve açıklamalarını `Import-AzRedisCache`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Import-AzRedisCache -detailed

    NAME
        Import-AzRedisCache

    SYNOPSIS
        Import data from blobs to Azure Cache for Redis.


    SYNTAX
        Import-AzRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzRedisCache cmdlet imports data from the specified blobs into Azure Cache for Redis.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komut, SAS uri tarafından belirtilen blob'dan verileri Redis için Azure Önbelleğine aktarır.

    PS C:\>Import-AzRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-an-azure-cache-for-redis"></a>Redis için Azure Önbelleği dışa aktarmak için
Cmdlet'i kullanarak Redis için bir `Export-AzRedisCache` Azure Önbelleğinden veri dışa aktarabilirsiniz.

> [!IMPORTANT]
> Alma/Dışa Aktarma yalnızca [premium katman](cache-premium-tier-intro.md) önbellekleri için kullanılabilir. İçe Aktarma/Dışa Aktarma hakkında daha fazla bilgi [için, Redis için Azure Önbelleğinde İçe Aktarma ve Dışa Aktarma verilerine](cache-how-to-import-export-data.md)bakın.
> 
> 

Kullanılabilir parametrelerin listesini ve açıklamalarını `Export-AzRedisCache`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Export-AzRedisCache -detailed

    NAME
        Export-AzRedisCache

    SYNOPSIS
        Exports data from Azure Cache for Redis to a specified container.


    SYNTAX
        Export-AzRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzRedisCache cmdlet exports data from Azure Cache for Redis to a specified container.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Prefix <String>
            Prefix to use for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -PassThru
            By default Export-AzRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komut, Redis örneği için bir Azure Önbelleğinden Verileri SAS uri tarafından belirtilen kapsayıcıya aktarır.

        PS C:\>Export-AzRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-an-azure-cache-for-redis"></a>Redis için Azure Önbelleğini yeniden başlatmak için
`Reset-AzRedisCache` Cmdlet'i kullanarak Azure Önbelleği redis örneğini yeniden başlatabilirsiniz.

> [!IMPORTANT]
> Yeniden başlatma yalnızca [premium katman](cache-premium-tier-intro.md) önbellekleri için kullanılabilir. Önbelleğiyeniden başlatma hakkında daha fazla bilgi için Önbellek yönetimine bakın [- yeniden başlatın.](cache-administration.md#reboot)
> 
> 

Kullanılabilir parametrelerin listesini ve açıklamalarını `Reset-AzRedisCache`görmek için aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Reset-AzRedisCache -detailed

    NAME
        Reset-AzRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Cache for Redis instance.


    SYNTAX
        Reset-AzRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzRedisCache cmdlet reboots the specified node(s) of an Azure Cache for Redis instance.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.

        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komut, belirtilen önbelleğin her iki düğümlerini yeniden başlatılır.

        PS C:\>Reset-AzRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Sonraki adımlar
Azure ile Windows PowerShell kullanma hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [MSDN'de Redis cmdlet belgeleri için Azure Önbelleği](https://docs.microsoft.com/powershell/module/az.rediscache)
* [Azure Kaynak Yöneticisi Cmdlets](https://go.microsoft.com/fwlink/?LinkID=394765): Azure Kaynak Yöneticisi modülündeki cmdlets'i kullanmayı öğrenin.
* [Azure kaynaklarınızı yönetmek için Kaynak gruplarını kullanma](../azure-resource-manager/templates/deploy-portal.md): Azure portalında kaynak gruplarını nasıl oluşturup yöneteceklerini öğrenin.
* [Azure blogu](https://azure.microsoft.com/blog/): Azure'daki yeni özellikler hakkında bilgi edinin.
* [Windows PowerShell blogu](https://blogs.msdn.com/powershell): Windows PowerShell'deki yeni özellikler hakkında bilgi edinin.
* ["Hey, SenaryoCu Adam!" Blog](https://blogs.technet.com/b/heyscriptingguy/): Windows PowerShell topluluğundan gerçek ipuçları ve püf noktaları alın.

