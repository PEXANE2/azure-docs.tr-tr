---
title: Azure PowerShell ile Synapse SQL havuzu oluşturma ve sorgulama
description: Azure PowerShell'i kullanarak sunucu düzeyinde güvenlik duvarı kuralına sahip synapse SQL havuzu mantıksal sunucusunu hızla oluşturun.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 4/11/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 45982c0761fecdb456dba5dc4a5d604972b9c3e5
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349307"
---
# <a name="quickstart-create-and-query-a-synapse-sql-pool-with-azure-powershell"></a>Quickstart: Azure PowerShell ile synapse SQL havuzu oluşturma ve sorgulama

Azure PowerShell'i kullanarak Azure Synapse Analytics'te bir Synapse SQL havuzu (veri ambarı) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!IMPORTANT]
> SQL havuzu oluşturmak, faturalandırılabilir yeni bir hizmete neden olabilir.  Daha fazla bilgi için Azure [Synapse Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)bölümüne bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Hangi aboneliği kullandığınızı görmek için [Get-AzSubscription'ı](/powershell/module/az.accounts/get-azsubscription)çalıştırın.

```powershell
Get-AzSubscription
```

Varsayılan dan farklı bir abonelik kullanmanız gerekiyorsa, [Set-AzContext'ı](/powershell/module/az.accounts/set-azcontext)çalıştırın.

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

[Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutunu kullanarak bir [Azure kaynak grubu](../../azure-resource-manager/management/overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```powershell
New-AzResourceGroup -Name $resourcegroupname -Location $location
```

## <a name="create-a-logical-server"></a>Mantıksal sunucu oluşturma

[New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) komutunu kullanarak bir [Azure SQL mantıksal sunucusu](../../sql-database/sql-database-logical-servers.md) oluşturun. Mantıksal sunucu, grup olarak yönetilen bir veritabanı grubu içerir. Aşağıdaki örnek, kaynak grubunuzda rasgele adlandırılmış bir sunucu `ServerAdmin` oluşturur ve `ChangeYourAdminPassword1`bir yönetici kullanıcı adlı ve bir şifre . Bu önceden tanımlı değerleri istediğiniz gibi değiştirin.

```powershell
New-AzSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

[Yeni-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) komutunu kullanarak [Azure SQL sunucu düzeyinde bir güvenlik duvarı kuralı](../../sql-database/sql-database-firewall-configure.md) oluşturun. Sunucu düzeyinde güvenlik duvarı kuralı, SQL Server Management Studio veya SQLCMD yardımcı programı gibi harici bir uygulamanın SQL havuz hizmeti güvenlik duvarı üzerinden SQL havuzuna bağlanmasına olanak tanır. 

Aşağıdaki örnekte, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. Dışarıdan bağlantı kurulabilmesi için IP adresini ortamınız için uygun bir adres olarak değiştirin. Tüm IP adreslerini açmak için başlangıç IP adresi olarak 0.0.0.0’ı, bitiş adresi olaraksa 255.255.255.255’i kullanın.

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL uç noktaları bağlantı noktası 1433 üzerinden iletişim kurar. Bir şirket ağı içinden bağlanmaya çalışıyorsanız, 1433 bağlantı noktası üzerindeki giden trafiğe ağınızın güvenlik duvarı izin vermeyebilir. Bu nedenle, BT departmanınız 1433 bağlantı noktasını açmadıkça Azure SQL sunucunuza bağlanamazsınız.
>


## <a name="create-a-sql-pool"></a>SQL havuzu oluşturma
Aşağıdaki örnekte, daha önce tanımlanmış değişkenleri kullanarak bir SQL havuzu oluşturulur.  Hizmet hedefini, SQL havuzunuz için daha düşük maliyetli bir başlangıç noktası olan DW100c olarak belirtir. 

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

* **İstenenServiceObjectiveName**: İstediğiniz [veri ambarı birimlerinin](what-is-a-data-warehouse-unit-dwu-cdwu.md) miktarı. Bu tutarın artırılması işlem maliyetini artırır. Desteklenen değerlerin listesi için [bellek ve eşzamanlılık sınırlarına](memory-concurrency-limits.md)bakın.
* **DatabaseName**: Oluşturduğunuz SQL havuzunun adı.
* **ServerName**: Oluşturma için kullandığınız sunucunun adı.
* **ResourceGroupName**: Kullandığınız kaynak grubu. Aboneliğinizdeki kullanılabilir kaynak gruplarını bulmak için Get-AzureResource komutunu kullanın.
* **Sürüm**: SQL havuzu oluşturmak için "DataWarehouse" olmalıdır.

İsteğe Bağlı Parametreler şunlardır:

- **CollationName**: Belirtilmezse varsayılan harmanlama SQL_Latin1_General_CP1_CI_AS şeklindedir. Veritabanında harmanlama değiştirilemez.
- **MaxSizeBytes**: Bir veritabanının varsayılan maksimum boyutu 240 TB'dir. Maksimum boyut rowstore verilerini sınırlar. Sütun verileri için sınırsız depolama alanı vardır.

Parametre seçenekleri hakkında daha fazla bilgi için [New-AzSqlDatabase 'a](/powershell/module/az.sql/new-azsqldatabase)bakın.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıç öğreticileri, bu hızlı başlangıcı temel alır. 

> [!TIP]
> Daha sonraki hızlı başlatma öğreticileriyle çalışmaya devam etmeyi planlıyorsanız, bu hızlı başlatmada oluşturulan kaynakları temizlemeyin. Devam etmeyi düşünmüyorsanız, Azure portalındaki bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Sonraki adımlar

Şimdi bir SQL havuzu oluşturdunuz, bir güvenlik duvarı kuralı oluşturdunuz, SQL havuzunuza bağlandınız ve birkaç sorgu çalıştırın. Daha fazla bilgi edinmek [için, SQL havuzu makalesine veri yükle'ye](load-data-from-azure-blob-storage-using-polybase.md) devam edin.
