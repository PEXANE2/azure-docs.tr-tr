---
title: Yönetilen örnek oluşturma (ARM şablonu & PowerShell)
titleSuffix: Azure SQL Managed Instance
description: Yönetilen bir örnek oluşturmak için bu Azure PowerShell örnek betiği kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: seo-dt-2019
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 03/12/2019
ms.openlocfilehash: 9024759f87d30cddfa2f3b7ea6b965ce03632f59
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220878"
---
# <a name="use-powershell-with-an-azure-resource-manager-template-to-create-a-managed-instance"></a>Yönetilen bir örnek oluşturmak için PowerShell 'i Azure Resource Manager şablonuyla kullanma

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqlmi.md)]

Azure PowerShell kitaplığı ve Azure Resource Manager şablonlarını kullanarak yönetilen bir örnek oluşturabilirsiniz.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure PowerShell 1.4.0 veya üzeri bir sürüm gerekir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız `Connect-AzAccount` Azure ile bağlantı oluşturmak için öğesini çalıştırın.

Azure PowerShell komutlar, önceden tanımlanmış bir Azure Resource Manager şablonu kullanarak dağıtımı başlatabilir. Şablonda aşağıdaki özellikler belirtilebilir:

- Yönetilen örnek adı
- SQL Yönetici Kullanıcı adı ve parolası.
- Örneğin boyutu (çekirdek sayısı ve en fazla depolama boyutu).
- Örneğin yerleştirileceği VNet ve alt ağ.
- Örneğin sunucu düzeyi harmanlaması (Önizleme).

Örnek adı, SQL Yöneticisi Kullanıcı adı, VNet/subnet ve harmanlama daha sonra değiştirilemez. Diğer örnek özellikleri değiştirilebilir.

## <a name="prerequisites"></a>Ön koşullar

Bu örnek, [geçerli bir ağ ortamı oluşturduğunuzu](../virtual-network-subnet-create-arm-template.md) veya yönetilen örneğiniz için [mevcut bir VNET 'i değiştirdiğinizi](../vnet-existing-add-subnet.md) varsayar. Gerekirse, ayrı bir [Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment)kullanarak ağ ortamını hazırlayabilirsiniz. 


Örnek [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment) ve [Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork)cmdlet 'lerini kullanır, bu nedenle aşağıdaki PowerShell modüllerini yüklediğinizden emin olun:

```powershell
Install-Module Az.Network
Install-Module Az.Resources
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu


Aşağıdaki betiği bir. JSON dosyasına kaydedin ve dosya konumuna göz önünde saklayın: 

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
                "name": "GP_Gen5",
                "tier": "GeneralPurpose"
            },
            "properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
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

Aşağıdaki PowerShell betiğini, daha önce kaydettiğiniz. JSON dosyası için doğru dosya yoluyla güncelleştirin ve betikteki nesnelerin adlarını değiştirin:

```powershell
$subscriptionId = "ed827499-xxxx-xxxx-xxxx-xxxxxxxxxx"
Select-AzSubscription -SubscriptionId $subscriptionId

# Managed instance properties
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

# Deploy instance using Azure Resource Manager template:
New-AzResourceGroupDeployment  -Name MyDeployment -ResourceGroupName $resourceGroup  `
                                    -TemplateFile 'C:\...\create-managed-instance.json' `
                                    -instance $name -user $user -pwd $secpasswd -subnetId $subnetId
```

Betik tamamlandıktan sonra, yönetilen örneğe tüm Azure hizmetlerinden ve yapılandırılmış IP adresinden erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

Azure SQL yönetilen [örnek PowerShell betikleri](../../database/powershell-script-content-guide.md)' nde, Azure SQL yönetilen örneği Için ek PowerShell betiği örnekleri bulunabilir.
