---
title: PowerShell örneği-Azure SQL veritabanı 'nda yönetilen bir örnek oluşturma | Microsoft Docs
description: Azure SQL veritabanı 'nda yönetilen bir örnek oluşturmak için örnek betik Azure PowerShell
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 03/12/2019
ms.openlocfilehash: 5a60e8efad41c94deeedd545e6e0c1c96ff04e25
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68569736"
---
# <a name="use-powershell-with-azure-resource-manager-template-to-create-a-managed-instance-in-azure-sql-database"></a>Azure SQL veritabanı 'nda yönetilen bir örnek oluşturmak için Azure Resource Manager şablonuyla PowerShell kullanma

Azure SQL veritabanı yönetilen örneği, Azure PowerShell kitaplığı ve Azure Resource Manager şablonları kullanılarak oluşturulabilir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici AZ PowerShell 1.4.0 veya üstünü gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

Azure PowerShell komutlar, önceden tanımlanmış Azure Resource Manager şablonu kullanarak dağıtımı başlatabilir. Şablonda aşağıdaki özellikler belirtilebilir:

- Örnek adı
- SQL Yönetici Kullanıcı adı ve parolası.
- Örneğin boyutu (çekirdek sayısı ve en fazla depolama boyutu).
- Örneğin yerleştirileceği VNet ve alt ağ.
- Örneğin sunucu düzeyi harmanlaması (Önizleme).

Örnek adı, SQL Yöneticisi Kullanıcı adı, VNet/subnet ve harmanlama daha sonra değiştirilemez. Diğer örnek özellikleri değiştirilebilir.

## <a name="prerequisites"></a>Önkoşullar

Bu örnek, [geçerli bir ağ ortamı oluşturduğunuzu](../sql-database-managed-instance-create-vnet-subnet.md) veya yönetilen örneğiniz Için [mevcut VNET](../sql-database-managed-instance-configure-vnet-subnet.md) 'i değiştirdiğinizi varsayar. Örnek, [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment) ve [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork) cmdlet 'lerini kullanır, bu nedenle aşağıdaki PowerShell modüllerini yüklediğinizden emin olun:

```powershell
Install-Module Az.Network
Install-Module Az.Resources
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Aşağıdaki içerik örneği oluşturmak için kullanılacak bir şablonu temsil eden bir dosyaya yerleştirilmelidir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "instance": {
            "type": "string"
        },
        "user": {
            "type": "string"
        },
        "pwd": {
            "type": "securestring"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('instance')]",
            "location": "West Central US",
            "tags": {
                "Description":"GP Instance with custom instance collation - Serbian_Cyrillic_100_CS_AS"
            },
            "sku": {
                "name": "GP_Gen4",
                "tier": "GeneralPurpose"
            },
            "properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen4",
                "collation": "Serbian_Cyrillic_100_CS_AS"
            },
            "type": "Microsoft.Sql/managedInstances",
            "identity": {
                "type": "SystemAssigned"
            },
            "apiVersion": "2015-05-01-preview"
        }
    ]
}
```

Varsayım, doğru yapılandırılmış alt ağa sahip Azure VNet 'in zaten varolduğu bir değer. Doğru yapılandırılmış bir alt ağınız yoksa, bağımsız olarak yürütülebilecek veya bu şablona dahil edilen ayrı [Azure Kaynak yönetilen şablonunu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) kullanarak ağ ortamını hazırlayın.

Bu dosyanın içeriğini. JSON dosyası olarak kaydedin, dosya yolunu aşağıdaki PowerShell betiğine yerleştirin ve betikteki nesnelerin adlarını değiştirin:

```powershell
$subscriptionId = "ed827499-xxxx-xxxx-xxxx-xxxxxxxxxx"
Select-AzSubscription -SubscriptionId $subscriptionId

# Managed Instance properties
$resourceGroup = "rg_mi"
$location = "West Central US"
$name = "managed-instance-name"
$user = "miSqlAdmin"
$secpasswd = ConvertTo-SecureString "<Put some strong password here>" -AsPlainText -Force

# Network configuration
$vNetName = "my_vnet"
$vNetResourceGroup = "rg_mi_vnet"
$subnetName = "ManagedInstances"
$vNet = Get-AzVirtualNetwork -Name $vNetName -ResourceGroupName $vNetResourceGroup
$subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetName -VirtualNetwork $vNet
$subnetId = $subnet.Id

# Deploy Instance using Azure Resource Manager template:
New-AzResourceGroupDeployment  -Name MyDeployment -ResourceGroupName $resourceGroup  `
                                    -TemplateFile 'C:\...\create-managed-instance.json' `
                                    -instance $name -user $user -pwd $secpasswd -subnetId $subnetId
```

Betik başarılı şekilde çalıştırıldıktan sonra, SQL Veritabanı tüm Azure hizmetlerinden ve yapılandırılmış IP adresinden erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Ek SQL Veritabanı PowerShell betiği örnekleri [Azure SQL Veritabanı PowerShell betikleri](../sql-database-powershell-samples.md) içinde bulunabilir.
