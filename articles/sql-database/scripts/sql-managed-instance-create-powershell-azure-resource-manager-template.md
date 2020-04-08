---
title: Şablon örneği - Azure SQL Veritabanı'nda yönetilen bir örnek oluşturma
description: Azure SQL Veritabanı'nda yönetilen bir örnek oluşturmak için bu Azure PowerShell örnek komut dosyasını kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: seo-dt-2019
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 03/12/2019
ms.openlocfilehash: e5be8c9441be5ca441a5c0f7c4444c2edbdc7a95
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811225"
---
# <a name="use-powershell-with-azure-resource-manager-template-to-create-a-managed-instance-in-azure-sql-database"></a>Azure SQL Veritabanı'nda yönetilen bir örnek oluşturmak için Azure Kaynak Yöneticisi şablonuyla PowerShell'i kullanma

Azure SQL Veritabanı Yönetilen Örneği, Azure PowerShell kitaplığı ve Azure Kaynak Yöneticisi şablonları kullanılarak oluşturulabilir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici AZ PowerShell 1.4.0 veya daha sonra gerektirir. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

Azure PowerShell komutları, önceden tanımlanmış Azure Kaynak Yöneticisi şablonunu kullanarak dağıtımı başlatabilir. Aşağıdaki özellikler şablonda belirtilebilir:

- Örnek adı
- SQL yönetici kullanıcı adı ve şifre.
- Örneğin boyutu (çekirdek sayısı ve maksimum depolama boyutu).
- VNet ve örneğin yerleştirileceği alt ağ.
- Örneğin sunucu düzeyinde harmanlama (Önizleme).

Örnek adı, SQL Administrator kullanıcı adı, VNet/subnet ve harmanlama daha sonra değiştirilemez. Diğer örnek özellikleri değiştirilebilir.

## <a name="prerequisites"></a>Ön koşullar

Bu örnek, Yönetilen Örneğiniz için [geçerli bir ağ ortamı oluşturduğunuzu](../sql-database-managed-instance-create-vnet-subnet.md) veya [varolan VNet'i değiştirdiğinizi](../sql-database-managed-instance-configure-vnet-subnet.md) varsayar. Örnek, cmdlets [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment) ve [Get-AzVirtualNetwork'u](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork) kullanır, böylece aşağıdaki PowerShell modüllerini yüklediğinizden emin olun:

```powershell
Install-Module Az.Network
Install-Module Az.Resources
```

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Aşağıdaki içerik, örneği oluşturmak için kullanılacak bir şablonu temsil eden bir dosyaya yerleştirilmelidir:

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

Varsayım, düzgün yapılandırılmış alt ağına sahip Azure VNet'in zaten var olduğudur. Düzgün yapılandırılmış bir alt ağınız yoksa, bağımsız olarak yürütülebilen veya bu şablona dahil edilebilen ayrı [Azure Kaynak Yönetimi şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) kullanarak ağ ortamını hazırlayın.

Bu dosyanın içeriğini .json dosyası olarak kaydedin, dosya yolunu aşağıdaki PowerShell komut dosyasına koyun ve komut dosyasındaki nesnelerin adlarını değiştirin:

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
