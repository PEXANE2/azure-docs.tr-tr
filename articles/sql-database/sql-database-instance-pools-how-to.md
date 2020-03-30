---
title: Örnek havuzları nasıl yapılır kılavuzu (önizleme)
description: Bu makalede, Azure SQL Veritabanı örnek havuzları (önizleme) nasıl oluşturulup yönetileceği açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab
ms.date: 09/05/2019
ms.openlocfilehash: 4a27165d929cc9bc5f18e372f7f108887e466e43
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299371"
---
# <a name="azure-sql-database-instance-pools-preview-how-to-guide"></a>Azure SQL Veritabanı örnek havuzları (önizleme) nasıl yapılır kılavuzu

Bu [makalede, örnek havuzları](sql-database-instance-pools.md)oluşturmak ve yönetmek hakkında ayrıntılar sağlar.

## <a name="instance-pool-operations"></a>Örnek havuz işlemleri

Aşağıdaki tablo, örnek havuzları ve Bunların Azure portalı ve PowerShell'deki kullanılabilirliğiyle ilgili kullanılabilir işlemleri gösterir.

|Komut|Azure portalında|PowerShell|
|:---|:---|:---|
|Örnek havuzu oluşturma|Hayır|Evet|
|Örnek havuzu güncelleştirme (sınırlı sayıda özellik)|Hayır |Evet |
|Örnek havuzu kullanımını ve özelliklerini denetleyin|Hayır|Evet |
|Örnek havuzusilme|Hayır|Evet|
|Örnek havuzu içinde yönetilen örnek oluşturma|Hayır|Evet|
|Yönetilen örnek kaynak kullanımını güncelleştirme|Evet |Evet|
|Yönetilen örnek kullanımını ve özelliklerini denetleme|Evet|Evet|
|Yönetilen örneği havuzdan silme|Evet|Evet|
|Havuza yerleştirilen yönetilen durumda bir veritabanı oluşturma|Evet|Evet|
|Yönetilen örnekten veritabanını silme|Evet|Evet|

Kullanılabilir [PowerShell komutları](https://docs.microsoft.com/powershell/module/az.sql/)

|Cmdlet |Açıklama |
|:---|:---|
|[Yeni-AzSqlInstancePool](/powershell/module/az.sql/new-azsqlinstancepool/) | Azure SQL Veritabanı örnek havuzu oluşturur. |
|[Get-AzSqlInstancePool](/powershell/module/az.sql/get-azsqlinstancepool/) | Azure SQL örnek havuzu hakkındaki bilgileri verir. |
|[Set-AzSqlInstancePool](/powershell/module/az.sql/set-azsqlinstancepool/) | Azure SQL Veritabanı örnek havuzunun özelliklerini ayarlar. |
|[Remove-AzSqlInstancePool](/powershell/module/az.sql/remove-azsqlinstancepool/) | Azure SQL Veritabanı örnek havuzukaldırır. |
|[Get-AzSqlInstancePoolUsage](/powershell/module/az.sql/get-azsqlinstancepoolusage/) | Azure SQL örnek havuzu kullanımı yla ilgili bilgileri verir. |


PowerShell'i kullanmak için [PowerShell Core'un en son sürümünü yükleyin](https://docs.microsoft.com/powershell/scripting/install/installing-powershell#powershell)ve [Azure PowerShell modülünün yüklenmesi yönergelerini](https://docs.microsoft.com/powershell/azure/install-az-ps)izleyin.

Hem havuz içi hem de tek örnek örnekleri örnekleriyle ilgili işlemler için, standart [yönetilen örnek komutlarını](sql-database-managed-instance-create-manage.md#powershell-create-and-manage-managed-instances)kullanın, ancak bu komutları havuzdaki bir örnek için kullanırken *örnek havuzu adı* özelliğinin doldurulması gerekir.

## <a name="how-to-deploy-managed-instances-into-pools"></a>Yönetilen örnekleri havuzlara dağıtma

Bir örneği havuza dağıtma işlemi aşağıdaki iki adımdan oluşur:

1. Bir kerelik örnek havuz dağıtımı. Bu, sürenin [boş bir alt ağda oluşturulan tek](sql-database-managed-instance.md#managed-instance-management-operations)bir örneği dağıtmakla aynı olduğu uzun süren bir işlemdir.

2. Örnek havuzunda yinelenen örnek dağıtımı. Örnek havuz parametresi bu işlemin bir parçası olarak açıkça belirtilmelidir. Bu genellikle 5 dakika kadar süren nispeten hızlı bir işlemdir.

Genel önizlemede, her iki adım da yalnızca PowerShell ve Kaynak Yöneticisi şablonları kullanılarak desteklenir. Azure portalı deneyimi şu anda kullanılamıyor.

Yönetilen bir örnek havuza dağıtıldıktan sonra, fiyatlandırma katmanı sayfasındaki özelliklerini değiştirmek için Azure portalını *kullanabilirsiniz.*


## <a name="create-an-instance-pool"></a>Örnek havuzu oluşturma

Örnek havuzu oluşturmak için:

1. [Alt ağ içeren bir sanal ağ oluşturun.](#create-a-virtual-network-with-a-subnet)
2. [Örnek havuzu oluşturun.](#create-an-instance-pool)


### <a name="create-a-virtual-network-with-a-subnet"></a>Alt ağ ile sanal ağ oluşturma 

Aynı sanal ağ içinde birden çok örnek havuzu yerleştirmek için aşağıdaki makalelere bakın:

- [Azure SQL Veritabanı yönetilen örneği için VNet alt net boyutunu belirleyin.](sql-database-managed-instance-determine-size-vnet-subnet.md)
- [Azure portal şablonu](sql-database-managed-instance-create-vnet-subnet.md) kullanarak yeni sanal ağ ve alt ağ oluşturun veya [varolan bir sanal ağı hazırlamak](sql-database-managed-instance-configure-vnet-subnet.md)için yönergeleri izleyin.
 


### <a name="create-an-instance-pool"></a>Örnek havuzu oluşturma 

Önceki adımları tamamladıktan sonra, bir örnek havuzu oluşturmaya hazırsınız.

Aşağıdaki kısıtlamalar örnek havuzları için geçerlidir:

- Genel Amaçlı ve Gen5 genel önizlemede kullanılabilir.
- Havuz adı yalnızca küçük harf, sayı ve tire içerebilir ve tire ile başedemez.
- AHB (Azure Karma Avantajı) kullanmak istiyorsanız, örnek havuz düzeyinde uygulanır. Havuz oluşturma sırasında lisans türünü ayarlayabilir veya oluşturulduktan sonra istediğiniz zaman güncelleştirebilirsiniz.

> [!IMPORTANT]
> Örnek havuzu dağıtmak, yaklaşık 4,5 saat süren uzun süren bir işlemdir.

Ağ parametrelerini almak için:

```powershell
$virtualNetwork = Get-AzVirtualNetwork -Name "miPoolVirtualNetwork" -ResourceGroupName "myResourceGroup"
$subnet = Get-AzVirtualNetworkSubnetConfig -Name "miPoolSubnet" -VirtualNetwork $virtualNetwork
```

Örnek havuzu oluşturmak için:

```powershell
$instancePool = New-AzSqlInstancePool `
  -ResourceGroupName "myResourceGroup" `
  -Name "mi-pool-name" `
  -SubnetId $subnet.Id `
  -LicenseType "LicenseIncluded" `
  -VCore 80 `
  -Edition "GeneralPurpose" `
  -ComputeGeneration "Gen5" `
  -Location "westeurope"
```

> [!IMPORTANT]
> Örnek havuzudağıtmak uzun süren bir işlem olduğundan, bu makaledeki aşağıdaki adımlardan herhangi birini çalıştırmadan önce tamamlanmasını beklemeniz gerekir.

## <a name="create-a-managed-instance-inside-the-pool"></a>Havuz içinde yönetilen bir örnek oluşturma 

Örnek havuzunun başarılı bir şekilde dağıtılmasından sonra, içinde bir örnek oluşturmanın zamanı geldiğinde.

Yönetilen bir örnek oluşturmak için aşağıdaki komutu uygulayın:

```powershell
$instanceOne = $instancePool | New-AzSqlInstance -Name "mi-pool-name" -VCore 2 -StorageSizeInGB 256
```

Bir örneğin ibi havuz içinde dağıtmak birkaç dakika sürer. İlk örnek oluşturulduktan sonra ek örnekler oluşturulabilir:

```powershell
$instanceTwo = $instancePool | New-AzSqlInstance -Name "mi-pool-name" -VCore 4 -StorageSizeInGB 512
```

## <a name="create-a-database-inside-an-instance"></a>Bir örnek içinde veritabanı oluşturma 

Havuz içinde yönetilen bir örnekte veritabanları oluşturmak ve yönetmek için tek örnek komutlarını kullanın.

Yönetilen bir örnek içinde bir veritabanı oluşturmak için:

```powershell
$poolinstancedb = New-AzSqlInstanceDatabase -Name "mipooldb1" -InstanceName "poolmi-001" -ResourceGroupName "myResourceGroup"
```


## <a name="get-instance-pool-usage"></a>Örnek havuz kullanımı alma 
 
Havuz içindeki örneklerin listesini almak için:

```powershell
$instancePool | Get-AzSqlInstance
```


Havuz kaynağı kullanımını almak için:

```powershell
$instancePool | Get-AzSqlInstancePoolUsage
```


Havuzun ve içindeki örneklerin ayrıntılı kullanım özetini almak için:

```powershell
$instancePool | Get-AzSqlInstancePoolUsage –ExpandChildren
```

Bir örnekte veritabanlarını listelemek için:

```powershell
$databases = Get-AzSqlInstanceDatabase -InstanceName "pool-mi-001" -ResourceGroupName "resource-group-name"
```


> [!NOTE]
> Havuz başına 100 veritabanı sınırı vardır (örnek başına değil).


## <a name="scale-a-managed-instance-inside-a-pool"></a>Yönetilen bir örneği havuz içinde ölçeklendirin 


Yönetilen bir örneği veritabanlarıyla doldurtuktan sonra, depolama veya performansla ilgili örnek sınırlarına ulaşabilirsiniz. Bu durumda, havuz kullanımı aşılanmadıysa, örneğini ölçeklendirebilirsiniz.
Yönetilen bir örneği havuz içinde ölçekleme birkaç dakika süren bir işlemdir. Ölçekleme için ön koşul, örneğin havuz düzeyinde vCores ve depolama kullanılabilir.

vCores ve depolama boyutunu güncellemek için:

```powershell
$instanceOne | Set-AzSqlInstance -VCore 8 -StorageSizeInGB 512 -InstancePoolName "mi-pool-name"
```


Yalnızca depolama boyutunu güncelleştirmek için:

```powershell
$instance | Set-AzSqlInstance -StorageSizeInGB 1024 -InstancePoolName "mi-pool-name"
```



## <a name="connect-to-a-managed-instance-inside-a-pool"></a>Havuz içinde yönetilen bir örne bağlanma

Havuzda yönetilen bir örne bağlanmak için aşağıdaki iki adım gereklidir:

1. [Örnek için ortak bitiş noktasını etkinleştirin.](#enable-the-public-endpoint-for-the-instance)
2. [Ağ güvenlik grubuna (NSG) gelen kural ekleyin.](#add-an-inbound-rule-to-the-network-security-group)

Her iki adım tamamlandıktan sonra, örnek oluşturma sırasında sağlanan ortak bir bitiş noktası adresi, bağlantı noktası ve kimlik bilgilerini kullanarak örne bağlanabilirsiniz. 

### <a name="enable-the-public-endpoint-for-the-instance"></a>Örnek için genel bitiş noktasını etkinleştirme

Bir örnek için ortak bitiş noktasını etkinleştirme, Azure portalı üzerinden veya aşağıdaki PowerShell komutunu kullanarak yapılabilir:


```powershell
$instanceOne | Set-AzSqlInstance -InstancePoolName "pool-mi-001" -PublicDataEndpointEnabled $true
```

Bu parametre örnek oluşturma sırasında da ayarlanabilir.

### <a name="add-an-inbound-rule-to-the-network-security-group"></a>Ağ güvenlik grubuna gelen kural ekleme 

Bu adım Azure portalı üzerinden veya PowerShell komutları kullanılarak yapılabilir ve alt ağ yönetilen örnek için hazırlandıktan sonra her zaman yapılabilir.

Ayrıntılar için bkz. [Ağ güvenlik grubunda genel uç nokta trafiğine izin](sql-database-managed-instance-public-endpoint-configure.md#allow-public-endpoint-traffic-on-the-network-security-group)ver.


## <a name="move-an-existing-single-instance-inside-an-instance-pool"></a>Varolan tek bir örneği örnek havuzunun içinde taşıma 
 
Örnekleri havuza girip çıkarmak genel önizleme sınırlamalarından biridir. Kullanılabilen geçici çözüm, havuz dışındaki bir örnekten havuzda zaten bulunan bir örne veritabanlarının zaman içinde geri yükolmasına dayanır. 

Her iki örnek de aynı abonelikte ve bölgede olmalıdır. Bölgeler arası ve çapraz abonelik geri yüklemesi şu anda desteklenmez.

Bu işlemin bir kesinti süresi var.

Varolan veritabanlarını taşımak için:

1. Geçiş yaptığınız yönetilen örnekteki iş yüklerini duraklatın.
2. Sistem veritabanları oluşturmak ve bunları örnek havuzunun içindeki örnekte yürütmek için komut dosyaları oluşturun.
3. Her veritabanının tek bir örnekten havuzdaki örne zamanında geri yüklemesi yapın.

    ```powershell
    $resourceGroupName = "my resource group name"
    $managedInstanceName = "my managed instance name"
    $databaseName = "my source database name"
    $pointInTime = "2019-08-21T08:51:39.3882806Z"
    $targetDatabase = "name of the new database that will be created"
    $targetResourceGroupName = "resource group of instance pool"
    $targetInstanceName = "pool instance name"
       
    Restore-AzSqlInstanceDatabase -FromPointInTimeBackup `
      -ResourceGroupName $resourceGroupName `
      -InstanceName $managedInstanceName `
      -Name $databaseName `
      -PointInTime $pointInTime `
      -TargetInstanceDatabaseName $targetDatabase `
      -TargetResourceGroupName $targetResourceGroupName `
      -TargetInstanceName $targetInstanceName
    ```

4. Uygulamanızı yeni örneğe yönlendirin ve iş yüklerini devam ettirin.

Birden çok veritabanı varsa, her veritabanı için işlemi yineleyin.


## <a name="next-steps"></a>Sonraki adımlar

- Özellikler ve karşılaştırma listesi için [SQL ortak özelliklerine](sql-database-features.md)bakın.
- VNet yapılandırması hakkında daha fazla bilgi için [yönetilen örnek VNet](sql-database-managed-instance-connectivity-architecture.md)yapılandırması'na bakın.
- Yönetilen bir örnek oluşturan ve bir yedekleme dosyasından veritabanını geri yükleyen hızlı [bir](sql-database-managed-instance-get-started.md)başlangıç için bkz.
- Geçiş için Azure Veritabanı Geçiş Hizmeti'ni (DMS) kullanan bir öğretici için, [Yönetilen Örnek Geçişi'ne bakın.](../dms/tutorial-sql-server-to-managed-instance.md)
- Yerleşik sorun giderme zekasıyla yönetilen örnek veritabanı performansının gelişmiş izlenmesi için Azure [SQL Analytics'i kullanarak Azure SQL Veritabanını Izleyin](../azure-monitor/insights/azure-sql.md)bölümüne bakın.
- Fiyatlandırma bilgileri için, [SQL Veritabanı yönetilen örnek fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/)bakın.
