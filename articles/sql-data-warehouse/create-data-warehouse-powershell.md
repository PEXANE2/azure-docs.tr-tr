---
title: Azure PowerShell ile SYNAPSE SQL havuzu oluşturma ve sorgulama
description: Azure PowerShell kullanarak sunucu düzeyinde bir güvenlik duvarı kuralıyla hızlı bir şekilde SYNAPSE SQL havuzu mantıksal sunucusu oluşturun.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: development
ms.date: 4/11/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 3cf55a400c1894794d555e1362f2197aad44a96b
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79130301"
---
# <a name="quickstart-create-and-query-a-synapse-sql-pool-with-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell ile SYNAPSE SQL havuzu oluşturma ve sorgulama

Azure PowerShell kullanarak Azure SYNAPSE Analytics 'te bir Synapse SQL Havuzu (veri ambarı) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!IMPORTANT]
> SQL havuzu oluşturmak, yeni bir faturalanabilir hizmetle sonuçlanabilir.  Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Kullanmakta olduğunuz aboneliği görmek için [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription)komutunu çalıştırın.

```powershell
Get-AzSubscription
```

Varsayılandan farklı bir abonelik kullanmanız gerekiyorsa, [set-AzContext](/powershell/module/az.accounts/set-azcontext)komutunu çalıştırın.

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```


## <a name="create-variables"></a>Değişken oluşturma

Bu hızlı başlangıçtaki betiklerde kullanılacak değişkenleri tanımlayacaksınız.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (don't capitalize)
$servername = "server-$(Get-Random)"
# Set an admin name and password for your database
# The sign-in information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDataWarehouse"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak bir [Azure Kaynak grubu](../azure-resource-manager/management/overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `myResourceGroup` konumunda `westeurope` adlı bir kaynak grubu oluşturur.

```powershell
New-AzResourceGroup -Name $resourcegroupname -Location $location
```

## <a name="create-a-logical-server"></a>Mantıksal sunucu oluşturma

[New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) komutunu kullanarak BIR [Azure SQL mantıksal sunucusu](../sql-database/sql-database-logical-servers.md) oluşturun. Mantıksal sunucu, grup olarak yönetilen bir veritabanı grubu içerir. Aşağıdaki örnek, kaynak grubunuzda `ServerAdmin` adlı Yönetici Kullanıcı ve `ChangeYourAdminPassword1`parolası ile rastgele olarak adlandırılmış bir sunucu oluşturur. Bu önceden tanımlı değerleri istediğiniz gibi değiştirin.

```powershell
New-AzSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

[New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) komutunu kullanarak [Azure SQL Server düzeyinde bir güvenlik duvarı kuralı](../sql-database/sql-database-firewall-configure.md) oluşturun. Sunucu düzeyinde bir güvenlik duvarı kuralı, SQL havuzu hizmeti güvenlik duvarı aracılığıyla SQL Server Management Studio veya SQLCMD yardımcı programı gibi bir SQL havuzuna bağlanmak için bir dış uygulamaya izin verir. 

Aşağıdaki örnekte, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. Dışarıdan bağlantı kurulabilmesi için IP adresini ortamınız için uygun bir adres olarak değiştirin. Tüm IP adreslerini açmak için başlangıç IP adresi olarak 0.0.0.0’ı, bitiş adresi olaraksa 255.255.255.255’i kullanın.

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL uç noktaları 1433 bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı takdirde Azure SQL sunucunuza bağlanamazsınız.
>


## <a name="create-a-sql-pool"></a>SQL havuzu oluşturma
Aşağıdaki örnek, daha önce tanımlanmış değişkenleri kullanarak bir SQL havuzu oluşturur.  Hizmet hedefini, SQL havuzunuz için daha düşük maliyetli bir başlangıç noktası olan DW100c olarak belirtir. 

```Powershell
New-AzSqlDatabase `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -Edition "DataWarehouse" `
    -RequestedServiceObjectiveName "DW100c" `
    -CollationName "SQL_Latin1_General_CP1_CI_AS" `
    -MaxSizeBytes 10995116277760
```

Gerekli Parametreler şunlardır:

* **Requestedserviceobjectivename**: istediğiniz [veri ambarı birimlerinin](what-is-a-data-warehouse-unit-dwu-cdwu.md) miktarı. Bu miktarı artırmak, işlem maliyetini artırır. Desteklenen değerlerin bir listesi için bkz. [bellek ve eşzamanlılık sınırları](memory-concurrency-limits.md).
* **DatabaseName**: oluşturmakta olduğunuz SQL havuzunun adı.
* **ServerName**: oluşturma için kullanmakta olduğunuz sunucunun adı.
* **Resourcegroupname**: kullandığınız kaynak grubu. Aboneliğinizdeki kullanılabilir kaynak gruplarını bulmak için Get-AzureResource komutunu kullanın.
* **Sürüm**: bir SQL havuzu oluşturmak Için "DataWarehouse" olmalıdır.

İsteğe Bağlı Parametreler şunlardır:

- **CollationName**: Belirtilmezse varsayılan harmanlama SQL_Latin1_General_CP1_CI_AS şeklindedir. Harmanlama bir veritabanında değiştirilemez.
- **Maxsizebytes**: bir veritabanının varsayılan en büyük boyutu 240tb 'tır. En büyük boyut, rowstore verilerini sınırlandırır. Sütunlu veriler için sınırsız depolama vardır.

Parametre seçenekleri hakkında daha fazla bilgi için, bkz. [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase).


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıç öğreticileri, bu hızlı başlangıcı temel alır. 

> [!TIP]
> Sonraki hızlı başlangıç öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, Azure portal bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Sonraki adımlar

Artık bir SQL havuzu oluşturdunuz, SQL havuzunuza bağlanmış bir güvenlik duvarı kuralı oluşturdunuz ve birkaç sorgu çalıştırdınız. Daha fazla bilgi edinmek için [VERILERI SQL havuzu 'Na yükleme](load-data-from-azure-blob-storage-using-polybase.md) makalesine ilerleyin.
