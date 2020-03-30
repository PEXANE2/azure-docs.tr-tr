---
title: Azure Kaynak Yöneticisi ile Studio (klasik) çalışma alanını dağıtma
titleSuffix: ML Studio (classic) - Azure
description: Azure Kaynak Yöneticisi şablonu kullanarak Azure Machine Learning Studio (klasik) için çalışma alanı dağıtma
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/05/2018
ms.openlocfilehash: 34333d4fe6e9b34a0c8b56cca8123f4ed93a917a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218108"
---
# <a name="deploy-azure-machine-learning-studio-classic-workspace-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni Kullanarak Azure Machine Learning Studio (klasik) Çalışma Alanı'nı dağıtma

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Kaynak Yöneticisi dağıtım şablonu kullanmak, bir doğrulama ve yeniden deneme mekanizmasıyla birbirine bağlı bileşenleri dağıtmanız için ölçeklenebilir bir yol sunarak zamandan tasarruf etmenizi sağlar. Örneğin, Azure Machine Learning Studio (klasik) Çalışma Alanlarını ayarlamak için önce bir Azure depolama hesabı yapılandırmanız ve ardından çalışma alanınızı dağıtmanız gerekir. Bunu yüzlerce çalışma alanı için el ile yaptığınızı düşünün. Daha kolay bir alternatif, Bir Studio (klasik) Çalışma Alanı ve tüm bağımlılıklarını dağıtmak için bir Azure Kaynak Yöneticisi şablonu kullanmaktır. Bu makale, bu işlem adım adım geçer. Azure Kaynak Yöneticisi'ne büyük bir genel bakış için Azure [Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)bölümüne bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Adım adım: Makine Öğrenimi Çalışma Alanı oluşturun
Bir Azure kaynak grubu oluşturup, ardından kaynak yöneticisi şablonu kullanarak yeni bir Azure depolama hesabı ve yeni bir Azure Machine Learning Studio (klasik) Çalışma Alanı dağıtacağız. Dağıtım tamamlandıktan sonra, oluşturulan çalışma alanları (birincil anahtar, çalışma alanı kimliği ve çalışma alanının URL'si) hakkında önemli bilgiler yazdıracağız.

### <a name="create-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu oluşturma

Machine Learning Workspace, ona bağlı veri kümesini depolamak için bir Azure depolama hesabı gerektirir.
Aşağıdaki şablon, depolama hesabı adını ve çalışma alanı adını oluşturmak için kaynak grubunun adını kullanır.  Çalışma alanını oluştururken depolama hesabı adını özellik olarak da kullanır.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Bu şablonu c:\temp\' altında mlworkspace.json dosyası olarak kaydedin.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Şablona dayalı kaynak grubunu dağıtma

* PowerShell’i açın
* Azure Kaynak Yöneticisi ve Azure Hizmet Yönetimi için modülleri yükleme

```powershell
# Install the Azure Resource Manager modules from the PowerShell Gallery (press "A")
Install-Module Az -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press "A")
Install-Module Azure -Scope CurrentUser
```

   Bu adımlar, kalan adımları tamamlamak için gerekli modülleri karşıdan yükleyin ve yükler. Bu yalnızca PowerShell komutlarını çalıştırdığınız ortamda bir kez yapılması gerekir.

* Azure'da kimlik doğrulaması

```powershell
# Authenticate (enter your credentials in the pop-up window)
Connect-AzAccount
```
Bu adımın her oturum için tekrarlanması gerekir. Kimlik doğrulaması alındıktan sonra, abonelik bilgileriniz görüntülenmelidir.

![Azure Hesabı](./media/deploy-with-resource-manager-template/azuresubscription.png)

Artık Azure'a erişebildiğimize göre, kaynak grubunu oluşturabiliriz.

* Kaynak grubu oluşturma

```powershell
$rg = New-AzResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Kaynak grubunun doğru şekilde sağlanmış olduğundan doğrulayın. **ProvisioningState** "Başarılı" olmalıdır.
Kaynak grubu adı, depolama hesabı adını oluşturmak için şablon tarafından kullanılır. Depolama hesabı adı 3 ile 24 karakter arasında uzunlukta olmalı ve yalnızca sayılar ve küçük harfler kullanmalıdır.

![Kaynak Grubu](./media/deploy-with-resource-manager-template/resourcegroupprovisioning.png)

* Kaynak grubu dağıtımını kullanarak, yeni bir Machine Learning Çalışma Alanı dağıtın.

```powershell
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Dağıtım tamamlandıktan sonra, dağıttığınız çalışma alanının özelliklerine erişmek kolaydır. Örneğin, Birincil Anahtar Belirteci'ne erişebilirsiniz.

```powershell
# Access Azure Machine Learning Studio Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Varolan çalışma alanı belirteçlerini almanın başka bir yolu da Invoke-AzResourceAction komutunu kullanmaktır. Örneğin, tüm çalışma alanlarının birincil ve ikincil belirteçlerini listeleyebilirsiniz.

```powershell
# List the primary and secondary tokens of all workspaces
Get-AzResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |ForEach-Object { Invoke-AzResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}
```
Çalışma alanı sağlandıktan sonra, [Azure Machine Learning Studio için PowerShell Modül'ü (klasik)](https://aka.ms/amlps)kullanarak birçok Azure Machine Learning Studio (klasik) görevi otomatikleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Kaynak Yöneticisi Şablonları yazma](../../azure-resource-manager/templates/template-syntax.md)hakkında daha fazla bilgi edinin.
* [Azure Quickstart Şablonları Deposu'na](https://github.com/Azure/azure-quickstart-templates)göz atın.
* Azure Kaynak [Yöneticisi](https://channel9.msdn.com/Events/Ignite/2015/C9-39)ile ilgili bu videoyu izleyin.
* Kaynak [Yöneticisi şablon başvuru yardımına](https://docs.microsoft.com/azure/templates/microsoft.machinelearning/allversions) bakın

<!--Link references-->
