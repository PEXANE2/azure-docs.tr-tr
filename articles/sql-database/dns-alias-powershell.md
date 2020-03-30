---
title: DNS takma adları için PowerShell
description: New-AzSqlServerDNSAlias gibi PowerShell cmdlet'leri, herhangi bir istemci yapılandırmasına dokunmak zorunda kalmadan yeni istemci bağlantılarını farklı bir Azure SQL Veritabanı sunucusuna yönlendirmenizi sağlar.
keywords: dns sql veritabanı
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74420409"
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>Azure SQL Veritabanına DNS Diğer Adları için PowerShell

Bu makalede, Azure SQL Veritabanı için bir DNS diğer adını nasıl yönetebileceğinizi gösteren bir PowerShell komut dosyası sağlanmaktadır.

> [!NOTE]
> Bu makale, Azure PowerShell Az modüllerini veya Azure CLI'yi kullanacak şekilde güncelleştirildi. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz.
>
> Az modülü ve AzureRM uyumluluğu hakkında daha fazla bilgi edinmek için Azure [PowerShell Az modüllerini tanıtma 'ya](/powershell/azure/new-azureps-module-az)bakın. Yükleme yönergeleri için Azure [PowerShell'i Yükle](/powershell/azure/install-az-ps) veya [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın.

## <a name="dns-alias-in-connection-string"></a>Bağlantı dizesinde DNS diğer adı

Belirli bir Azure SQL Veritabanı sunucusunu bağlamak için, SQL Server Management Studio (SSMS) gibi bir istemci gerçek sunucu adı yerine DNS takma ad adını sağlayabilir. Aşağıdaki örnek sunucu dizesinde, diğer ad *herhangi bir benzersiz-diğer ad adı* dört düğüm sunucu dizesinde ilk nokta-delimited düğüm değiştirir:

   `<yourServer>.database.windows.net`

## <a name="prerequisites"></a>Ön koşullar

Bu makalede verilen demo PowerShell komut dosyasını çalıştırmak istiyorsanız, aşağıdaki ön koşullar geçerlidir:

- Ücretsiz deneme için Azure aboneliği ve hesabı [bkz.](https://azure.microsoft.com/free/)
- İki Azure SQL veritabanı sunucusu

## <a name="example"></a>Örnek

Aşağıdaki kod örneği, birkaç değişkene gerçek değerler atayarak başlar.

Kodu çalıştırmak için, sisteminizdeki gerçek değerlerle eşleşecek şekilde yer tutucu değerlerini düzenle.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Kullanılan cmdlets şunlardır:

- [New-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Azure SQL Veritabanı hizmet sisteminde Bir DNS takma adı oluşturur. Takma ad veritabanı sunucusu 1'e başvurur.
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): SQL DB server 1'e atanan tüm takma adları alın ve listeleyin.
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): Diğer adın ifade etmek üzere yapılandırıldığı sunucu adını, sunucu 1'den sunucu 2'ye değiştirir.
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): Diğer adı kullanarak veritabanı sunucusu 2'den diğer adı kaldırın.

Yüklemek veya yükseltmek için bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-az-ps).

Sürümü `Get-Module -ListAvailable Az` bulmak için *powershell\_ise.exe*kullanın.

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

- [az sql server dns-diğer ad oluşturma](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Azure SQL Veritabanı hizmet sisteminde bir DNS diğer adı oluşturur. Takma ad veritabanı sunucusu 1'e başvurur.
- [az sql server dns-diğer ad göster](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): SQL DB server 1'e atanan tüm diğer adları alın ve listeleyin.
- [az sql server dns-diğer ad kümesi](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): Diğer adın ifade etmek üzere yapılandırıldığı sunucu adını, sunucu 1'den server 2'ye değiştirir.
- [az sql server dns-diğer ad silme](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): Diğer adı kullanarak veritabanı sunucusu 2'den diğer adı kaldırın.

Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli)bakın.

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

SQL Veritabanı için DNS diğer ad özelliğinin tam açıklaması [için Azure SQL veritabanı için DNS diğer adı bölümüne](dns-alias-overview.md)bakın.
