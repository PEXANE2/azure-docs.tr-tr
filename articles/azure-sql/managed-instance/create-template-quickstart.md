---
title: 'Azure Resource Manager: Azure SQL yönetilen örneği oluşturma'
description: Azure Resource Manager şablonu kullanarak Azure SQL yönetilen örneği oluşturmayı öğrenin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: subject-armqs
ms.devlang: ''
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 06/22/2020
ms.openlocfilehash: 01c6c37d31d41f88b370face372555536724adde
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256187"
---
# <a name="quickstart-create-an-azure-sql-managed-instance-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak Azure SQL yönetilen örneği oluşturma

Bu hızlı başlangıç, Azure SQL yönetilen örneği ve vNet oluşturmak için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanmaktadır.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Yok.

## <a name="create-an-azure-sql-managed-instance"></a>Azure SQL Yönetilen Örnek oluşturma

[Azure SQL yönetilen örneği](sql-managed-instance-paas-overview.md) , SQL Server veritabanı altyapısı ile neredeyse %100 Özellik eşliği olan akıllı, tam olarak yönetilen, ölçeklenebilir bir bulut veritabanıdır.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-sqlmi-new-vnet/azuredeploy.json":::

Bu kaynaklar şablonda tanımlanmıştır:

- [**Microsoft. SQL/ManagedInstances**](/azure/templates/microsoft.sql/managedinstances)
- [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.Network/virtualNetworks)
- [**Microsoft. Network/routeTables**](/azure/templates/microsoft.Network/routeTables)
- [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.Network/networkSecurityGroups)



Daha fazla şablon örneği, [Azure hızlı başlangıç şablonlarında](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular)bulunabilir.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Azure Cloud Shell açmak için aşağıdaki PowerShell kod bloğundan **deneyin** öğesini seçin.

> [!IMPORTANT]
> Yönetilen bir örneği dağıtmak uzun süren bir işlemdir. Alt ağdaki ilk Örneğin dağıtımı, genellikle mevcut yönetilen örneklerle bir alt ağa dağıtmaya kıyasla çok daha uzun sürer. Ortalama sağlama süreleri için bkz. [SQL yönetilen örnek yönetimi işlemleri](sql-managed-instance-paas-overview.md#management-operations).

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sqlmi-new-vnet/azuredeploy.json"

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri

Read-Host -Prompt "Press [ENTER] to continue ..."
```

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
read -p "Enter a project name that is used for generating resource names:" projectName &&
read -p "Enter the location (i.e. centralus):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sqlmi-new-vnet/azuredeploy.json" &&
resourceGroupName="${projectName}rg" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri  $templateUri &&
echo "Press [ENTER] to continue ..." &&
read
```

* * *

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

[Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) ziyaret edin ve yönetilen Örneğin seçili kaynak grubunuzda olduğunu doğrulayın. Yönetilen bir örnek oluşturmak biraz zaman alabilir, kaynak grubunuzun **genel bakış** sayfasında **dağıtımlar** bağlantısını denetlemeniz gerekebilir.

- Azure sanal makinesinden SQL yönetilen örneği 'ne bağlanmayı gösteren bir hızlı başlangıç için bkz. [Azure sanal makine bağlantısını yapılandırma](connect-vm-instance-configure.md).
- Noktadan siteye bağlantı kullanarak bir şirket içi istemci bilgisayarından SQL yönetilen örneği 'ne bağlanmayı gösteren bir hızlı başlangıç için bkz. [Noktadan siteye bağlantı yapılandırma](point-to-site-p2s-configure.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki adımlara gitmek istiyorsanız yönetilen örneği koruyun, ancak ek [öğreticileri](#next-steps)tamamladıktan sonra yönetilen örneği ve ilgili kaynakları silin. Yönetilen bir örneği sildikten sonra, bkz. [yönetilen bir örneği sildikten sonra bir alt ağı silme](virtual-cluster-delete.md).


Kaynak grubunu silmek için:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

* * *

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure SQL yönetilen örneğine bağlanmak için bir Azure VM yapılandırma](connect-vm-instance-configure.md)
