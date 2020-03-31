---
title: 'Azure Kaynak Yöneticisi: Tek bir veritabanı oluşturma'
description: Azure Kaynak Yöneticisi şablonunu kullanarak Azure SQL Veritabanı'nda tek bir veritabanı oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: subject-armqs
ms.devlang: ''
ms.topic: quickstart
author: mumian
ms.author: jgao
ms.reviewer: carlrab
ms.date: 06/28/2019
ms.openlocfilehash: 7c42ff7f42dea049752f9f879abffffd0e7b3902
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79531337"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-resource-manager-template"></a>Hızlı başlatma: Azure Kaynak Yöneticisi şablonu kullanarak Azure SQL Veritabanı'nda tek bir veritabanı oluşturma

Tek bir [veritabanı](sql-database-single-database.md) oluşturmak, Azure SQL Veritabanı'nda veritabanı oluşturmak için en hızlı ve en basit dağıtım seçeneğidir. Bu hızlı başlangıç, Azure Kaynak Yöneticisi şablonu kullanarak tek bir veritabanının nasıl oluşturulabileceğinizi gösterir.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, ücretsiz [bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

None

## <a name="create-a-single-database"></a>Tek bir veritabanı oluşturma

Tek bir veritabanı, iki [satın alma modelinden](sql-database-purchase-models.md)birini kullanarak tanımlanmış bir işlem, bellek, IO ve depolama kaynakları kümesine sahiptir. Tek bir veritabanı oluşturduğunuzda, onu yönetmek ve belirli bir bölgedeki [Azure kaynak grubuna](../azure-resource-manager/management/overview.md) yerleştirmek için bir [SQL Veritabanı sunucusu](sql-database-servers.md) da tanımlarsınız.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-sql-logical-server/)

:::code language="json" source="~/quickstart-templates/101-sql-logical-server/azuredeploy.json" range="1-163" highlight="63-132":::

Bu kaynaklar şablonda tanımlanır:

- [**Microsoft.Sql/sunucular**](/azure/templates/microsoft.sql/servers)
- [**Microsoft.Sql/servers/firewallRules**](/azure/templates/microsoft.sql/servers/firewallrules)
- [**Microsoft.Sql/servers/securityAlertPolicies**](/azure/templates/microsoft.sql/servers/securityalertpolicies)
- [**Microsoft.Sql/servers/vulnerabilityAssessments**](/azure/templates/microsoft.sql/servers/vulnerabilityassessments)
- [**Microsoft.Sql/servers/connectionPolitikalar**](/azure/templates/microsoft.sql/servers/connectionpolicies)
- [**Microsoft.Storage/storageHesapları**](/azure/templates/microsoft.storage/storageaccounts)
- [**Microsoft.Storage/storageAccounts/providers/roleAssignments**](/azure/templates/microsoft.authorization/roleassignments)

[Azure Quickstart Şablonları'nda](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sql&pageNumber=1&sort=Popular)daha fazla Azure SQL veritabanı şablonu örneği bulunabilir.

### <a name="deploy-the-template"></a>Şablonu dağıtma

Azure Bulut Shell'i açmak için aşağıdaki PowerShell kod bloğundan **deneyin'i** seçin.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-sql-logical-server/azuredeploy.json" -administratorLogin $adminUser -administratorLoginPassword $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter an Azure location (i.e. centralus)"
$adminUser = Read-Host -Prompt "Enter the SQL server administrator username"
$adminPassword = Read-Host -Prompt "Enter the SQl server administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"

az group create --location $location --name $resourceGroupName

az group deployment create -g $resourceGroupName --template-uri "D:\GitHub\azure-docs-json-samples\SQLServerAndDatabase\azuredeploy.json" `
    --parameters 'projectName=' + $projectName \
                 'administratorLogin=' + $adminUser \
                 'administratorLoginPassword=' + $adminPassword

Read-Host -Prompt "Press [ENTER] to continue ..."
```

* * *

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Veritabanını sorgulamak için [veritabanını sorgula'ya](./sql-database-single-database-get-started.md#query-the-database)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Sonraki adımlara](#next-steps)gitmek istiyorsanız bu kaynak grubunu, veritabanı sunucusunu ve tek veritabanını saklayın. Sonraki adımlar, veritabanınızı farklı yöntemler kullanarak nasıl bağlayıp sorgulayabileceğinizi gösterir.

Kaynak grubunu silmek için:

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

* * *

## <a name="next-steps"></a>Sonraki adımlar

- Şirket içi veya uzak araçlardan tek bir veritabanına bağlanmak için sunucu düzeyinde bir güvenlik duvarı kuralı oluşturun. Daha fazla bilgi için [bkz.](sql-database-server-level-firewall-rule.md)
- Sunucu düzeyinde bir güvenlik duvarı kuralı oluşturduktan sonra, birkaç farklı araç ve dil kullanarak veritabanınızı [bağlayın](sql-database-connect-query.md) ve sorgulayın.
  - [SQL Server Management Studio kullanarak bağlanma ve sorgulama](sql-database-connect-query-ssms.md)
  - [Azure Data Studio kullanarak bağlanma ve sorgulama](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Azure CLI kullanarak tek bir veritabanı oluşturmak için [Azure CLI örneklerine](sql-database-cli-samples.md)bakın.
- Azure PowerShell'i kullanarak tek bir veritabanı oluşturmak için [Azure PowerShell örneklerine](sql-database-powershell-samples.md)bakın.
- Kaynak Yöneticisi şablonlarını nasıl oluşturabilirsiniz öğrenmek için [bkz.](../azure-resource-manager/templates/template-tutorial-create-first-template.md)
