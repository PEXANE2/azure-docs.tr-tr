---
title: DNS diğer adı için PowerShell
description: New-Azsqlserverdnsalıas gibi PowerShell cmdlet 'leri, herhangi bir istemci yapılandırmasına dokunmanıza gerek kalmadan yeni istemci bağlantılarını farklı bir Azure SQL veritabanı sunucusuna yönlendirmenizi sağlar.
keywords: DNS SQL veritabanı
ms.custom: seo-lt-2019
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.devlang: PowerShell
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: genemi, amagarwa, maboja, jrasnick, vanto
ms.date: 05/14/2019
ms.openlocfilehash: 9232a99ddd29201e6743c09455d79e9ba22b3b9c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74420409"
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>DNS diğer adı için PowerShell Azure SQL veritabanı

Bu makalede, Azure SQL veritabanı için bir DNS diğer adını nasıl yönetebileceğinizi gösteren bir PowerShell betiği sağlanmaktadır.

> [!NOTE]
> Bu makale, Azure PowerShell az Module veya Azure CLı 'yi kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz.
>
> Az Module ve Azurerd uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Azure PowerShell az Module tanıtımı](/powershell/azure/new-azureps-module-az). Yükleme yönergeleri için bkz. [yükleme Azure PowerShell](/powershell/azure/install-az-ps) veya [Azure CLI 'yi yükleme](/cli/azure/install-azure-cli).

## <a name="dns-alias-in-connection-string"></a>Bağlantı dizesinde DNS diğer adı

Belirli bir Azure SQL veritabanı sunucusuna bağlanmak için, SQL Server Management Studio (SSMS) gibi bir istemci, doğru sunucu adı yerine DNS diğer adı sağlayabilir. Aşağıdaki örnek sunucu dizesinde, *Any-UNIQUE-Alias-name* diğer adı dört düğümlü sunucu dizesindeki ilk noktayla ayrılmış düğümü değiştirir:

   `<yourServer>.database.windows.net`

## <a name="prerequisites"></a>Ön koşullar

Bu makalede verilen demo PowerShell betiğini çalıştırmak istiyorsanız aşağıdaki önkoşullar geçerlidir:

- Azure aboneliği ve hesabı, ücretsiz deneme için bkz. [Azure denemeleri](https://azure.microsoft.com/free/)
- İki Azure SQL veritabanı sunucusu

## <a name="example"></a>Örnek

Aşağıdaki kod örneği, birkaç değişkene değişmez değerler atayarak başlar.

Kodu çalıştırmak için, yer tutucu değerlerini sisteminizdeki gerçek değerlerle eşleşecek şekilde düzenleyin.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Kullanılan cmdlet 'ler şunlardır:

- [New-Azsqlserverdnsalıas](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Azure SQL veritabanı hizmet SISTEMINDE bir DNS diğer adı oluşturur. Diğer ad veritabanı sunucusu 1 ' e başvurur.
- [Get-Azsqlserverdnsalıas](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): SQL DB Server 1 ' e atanan tüm diğer adları alın ve listeleyin.
- [Set-Azsqlserverdnsalıas](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): diğer adın, sunucu 1 ' den sunucu 2 ' ye başvuracak şekilde yapılandırıldığı sunucu adını değiştirir.
- [Remove-Azsqlserverdnsalıas](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): diğer adı kullanarak, diğer adı veritabanı sunucusu 2 ' den kaldırın.

Yüklemek veya yükseltmek için bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps).

Sürümü `Get-Module -ListAvailable Az` bulmak *için\_PowerShell ISE. exe*' de kullanın.

```powershell
$subscriptionName = '<subscriptionName>';
$sqlServerDnsAliasName = '<aliasName>';
$resourceGroupName = '<resourceGroupName>';  
$sqlServerName = '<sqlServerName>';
$resourceGroupName2 = '<resourceGroupNameTwo>'; # can be same or different than $resourceGroupName
$sqlServerName2 = '<sqlServerNameTwo>'; # must be different from $sqlServerName.

# login to Azure
Connect-AzAccount -SubscriptionName $subscriptionName;
$subscriptionId = Get-AzSubscription -SubscriptionName $subscriptionName;

Write-Host 'Assign an alias to server 1...';
New-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName -ServerName $sqlServerName `
    -Name $sqlServerDnsAliasName;

Write-Host 'Get the aliases assigned to server 1...';
Get-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName -ServerName $sqlServerName;

Write-Host 'Move the alias from server 1 to server 2...';
Set-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName2 -TargetServerName $sqlServerName2 `
    -Name $sqlServerDnsAliasName `
    -SourceServerResourceGroup $resourceGroupName -SourceServerName $sqlServerName `
    -SourceServerSubscriptionId $subscriptionId.Id;

Write-Host 'Get the aliases assigned to server 2...';
Get-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName2 -ServerName $sqlServerName2;

Write-Host 'Remove the alias from server 2...';
Remove-AzSqlServerDnsAlias –ResourceGroupName $resourceGroupName2 -ServerName $sqlServerName2 `
    -Name $sqlServerDnsAliasName;
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Kullanılan komutlar şunlardır:

- [az SQL Server DNS-Alias Create](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Azure SQL veritabanı hizmet SISTEMINDE bir DNS diğer adı oluşturur. Diğer ad veritabanı sunucusu 1 ' e başvurur.
- [az SQL Server DNS-Alias Show](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): SQL DB Server 1 ' e atanan tüm diğer adları alın ve listeleyin.
- [az SQL Server DNS-Alias set](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): sunucu 1 ' den sunucu 2 ' ye başvurmak için diğer adın yapılandırıldığı sunucu adını değiştirir.
- [az SQL Server DNS-alias delete](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): diğer adı kullanarak, diğer adı veritabanı sunucusu 2 ' den kaldırın.

Yüklemek veya yükseltmek için bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli).

```azurecli-interactive
$subscriptionName = '<subscriptionName>';
$sqlServerDnsAliasName = '<aliasName>';
$resourceGroupName = '<resourceGroupName>';  
$sqlServerName = '<sqlServerName>';
$resourceGroupName2 = '<resourceGroupNameTwo>'; # can be same or different than $resourceGroupName
$sqlServerName2 = '<sqlServerNameTwo>'; # must be different from $sqlServerName.

# login to Azure
az login -SubscriptionName $subscriptionName;
$subscriptionId = az account list[0].i -SubscriptionName $subscriptionName;

Write-Host 'Assign an alias to server 1...';
az sql server dns-alias create –-resource-group $resourceGroupName --server $sqlServerName `
    --name $sqlServerDnsAliasName;

Write-Host 'Get the aliases assigned to server 1...';
az sql server dns-alias show –-resource-group $resourceGroupName --server $sqlServerName;

Write-Host 'Move the alias from server 1 to server 2...';
az sql server dns-alias set –-resource-group $resourceGroupName2 --server $sqlServerName2 `
    --name $sqlServerDnsAliasName `
    --original-resource-group $resourceGroupName --original-server $sqlServerName `
    --original-subscription-id $subscriptionId.Id;

Write-Host 'Get the aliases assigned to server 2...';
az sql server dns-alias show –-resource-group $resourceGroupName2 --server $sqlServerName2;

Write-Host 'Remove the alias from server 2...';
az sql server dns-alias delete –-resource-group $resourceGroupName2 --server $sqlServerName2 `
    --name $sqlServerDnsAliasName;
```

* * *

## <a name="next-steps"></a>Sonraki adımlar

SQL veritabanı için DNS diğer adı özelliğinin tam açıklaması için bkz. [Azure SQL veritabanı Için DNS diğer adı](dns-alias-overview.md).
