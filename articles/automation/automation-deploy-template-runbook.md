---
title: Azure Otomasyonu runbook 'unda Azure Resource Manager şablonu dağıtma
description: Azure depolama 'da depolanan bir Azure Resource Manager şablonunu runbook 'tan dağıtma
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
keywords: PowerShell, runbook, JSON, Azure Otomasyonu
ms.openlocfilehash: 922a4e8d98405de9e2b8420da4abf0e157011546
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850933"
---
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a>Azure Otomasyonu PowerShell runbook’unda Azure Resource Manager şablonu dağıtma

Azure Kaynak [yönetimi şablonu](../azure-resource-manager/resource-manager-create-first-template.md)kullanarak Azure kaynağı dağıtan bir [Azure Otomasyonu PowerShell runbook 'u](automation-first-runbook-textual-powershell.md) yazabilirsiniz.

Bunu yaptığınızda, Azure kaynaklarının dağıtımını otomatik hale getirebilirsiniz. Kaynak Yöneticisi şablonlarınızı Azure depolama gibi merkezi ve güvenli bir konumda tutabilirsiniz.

Bu makalede, yeni bir Azure depolama hesabı dağıtmak için [Azure Storage](../storage/common/storage-introduction.md) 'da depolanan kaynak yöneticisi şablonunu kullanan bir PowerShell runbook 'u oluşturacağız.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için aşağıdaki öğeler gereklidir:

* Azure aboneliği. Henüz bir hesabınız yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) veya [ücretsiz bir hesap için kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-sec-configure-azure-runas-account.md).  Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Kaynak Yöneticisi şablonunun kaydedileceği [Azure depolama hesabı](../storage/common/storage-create-storage-account.md)
* Azure PowerShell yerel bir makineye yüklendi. Azure PowerShell alma hakkında bilgi için bkz. [Azure PowerShell 'ı yüklemek ve yapılandırmak](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) .

## <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma

Bu örnekte, yeni bir Azure depolama hesabı dağıtan bir Kaynak Yöneticisi şablonu kullanıyoruz.

Bir metin düzenleyicisinde, aşağıdaki metni kopyalayın:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

Dosyayı `TemplateTest.json`olarak yerel olarak kaydedin.

## <a name="save-the-resource-manager-template-in-azure-storage"></a>Kaynak Yöneticisi şablonunu Azure Storage 'a kaydetme

Artık PowerShell 'i kullanarak bir Azure depolama dosya paylaşımında oluşturma ve `TemplateTest.json` dosyasını karşıya yükleme.
Dosya paylaşımının nasıl oluşturulacağı ve Azure portal bir dosyanın nasıl yükleneceği hakkında yönergeler için bkz. [Windows 'Da Azure dosya depolama ile çalışmaya başlama](../storage/files/storage-dotnet-how-to-use-files.md).

Yerel makinenizde PowerShell 'i başlatın ve bir dosya paylaşma oluşturup Kaynak Yöneticisi şablonunu bu dosya paylaşımında karşıya yüklemek için aşağıdaki komutları çalıştırın.

```powershell
# Login to Azure
Connect-AzureRmAccount

# Get the access key for your storage account
$key = Get-AzureRmStorageAccountKey -ResourceGroupName 'MyAzureAccount' -Name 'MyStorageAccount'

# Create an Azure Storage context using the first access key
$context = New-AzureStorageContext -StorageAccountName 'MyStorageAccount' -StorageAccountKey $key[0].value

# Create a file share named 'resource-templates' in your Azure Storage account
$fileShare = New-AzureStorageShare -Name 'resource-templates' -Context $context

# Add the TemplateTest.json file to the new file share
# "TemplatePath" is the path where you saved the TemplateTest.json file
$templateFile = 'C:\TemplatePath'
Set-AzureStorageFileContent -ShareName $fileShare.Name -Context $context -Source $templateFile
```

## <a name="create-the-powershell-runbook-script"></a>PowerShell runbook betiğini oluşturma

Şimdi Azure Storage 'dan `TemplateTest.json` dosyasını alan ve yeni bir Azure depolama hesabı oluşturmak için şablonu dağıtan bir PowerShell betiği oluşturacağız.

Bir metin düzenleyicisinde şu metni yapıştırın:

```powershell
param (
    [Parameter(Mandatory=$true)]
    [string]
    $ResourceGroupName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountName,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageAccountKey,

    [Parameter(Mandatory=$true)]
    [string]
    $StorageFileName
)



# Authenticate to Azure if running from Azure Automation
$ServicePrincipalConnection = Get-AutomationConnection -Name "AzureRunAsConnection"
Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint | Write-Verbose

#Set the parameter values for the Resource Manager template
$Parameters = @{
    "storageAccountType"="Standard_LRS"
    }

# Create a new context
$Context = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

Get-AzureStorageFileContent -ShareName 'resource-templates' -Context $Context -path 'TemplateTest.json' -Destination 'C:\Temp'

$TemplateFile = Join-Path -Path 'C:\Temp' -ChildPath $StorageFileName

# Deploy the storage account
New-AzureRmResourceGroupDeployment -ResourceGroupName $ResourceGroupName -TemplateFile $TemplateFile -TemplateParameterObject $Parameters 
``` 

Dosyayı `DeployTemplate.ps1`olarak yerel olarak kaydedin.

## <a name="import-and-publish-the-runbook-into-your-azure-automation-account"></a>Runbook 'u içeri aktarın ve Azure Otomasyonu hesabınıza yayımlayın

Artık PowerShell 'i kullanarak runbook 'u Azure Otomasyonu hesabınıza içeri aktarıp runbook 'u yayımlamanız gerekir.
Azure portal runbook 'u içeri aktarma ve yayımlama hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'nda runbook 'Ları yönetme](manage-runbooks.md).

Otomasyon hesabınıza bir PowerShell runbook 'u olarak `DeployTemplate.ps1` aktarmak için aşağıdaki PowerShell komutlarını çalıştırın:

```powershell
# MyPath is the path where you saved DeployTemplate.ps1
# MyResourceGroup is the name of the Azure ResourceGroup that contains your Azure Automation account
# MyAutomationAccount is the name of your Automation account
$importParams = @{
    Path = 'C:\MyPath\DeployTemplate.ps1'
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Type = 'PowerShell'
}
Import-AzureRmAutomationRunbook @importParams

# Publish the runbook
$publishParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
}
Publish-AzureRmAutomationRunbook @publishParams
```

## <a name="start-the-runbook"></a>Runbook’u başlatma

Şimdi [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet 'ini çağırarak runbook 'u başlatacağız.

Azure portal runbook 'u başlatma hakkında daha fazla bilgi için bkz. [Azure Otomasyonu 'nda runbook başlatma](automation-starting-a-runbook.md).

PowerShell konsolunda aşağıdaki komutları çalıştırın:

```powershell
# Set up the parameters for the runbook
$runbookParams = @{
    ResourceGroupName = 'MyResourceGroup'
    StorageAccountName = 'MyStorageAccount'
    StorageAccountKey = $key[0].Value # We got this key earlier
    StorageFileName = 'TemplateTest.json' 
}

# Set up parameters for the Start-AzureRmAutomationRunbook cmdlet
$startParams = @{
    ResourceGroupName = 'MyResourceGroup'
    AutomationAccountName = 'MyAutomationAccount'
    Name = 'DeployTemplate'
    Parameters = $runbookParams
}

# Start the runbook
$job = Start-AzureRmAutomationRunbook @startParams
```

Runbook çalışır ve `$job.Status`çalıştırarak durumunu kontrol edebilirsiniz.

Runbook Kaynak Yöneticisi şablonunu alır ve yeni bir Azure depolama hesabı dağıtmak için onu kullanır.
Aşağıdaki komutu çalıştırarak yeni depolama hesabının oluşturulduğunu görebilirsiniz:
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a>Özet

İşte bu kadar! Artık Azure Otomasyonu ve Azure Storage 'ı ve tüm Azure kaynaklarınızı dağıtmak için Kaynak Yöneticisi şablonları kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](../azure-resource-manager/resource-group-overview.md)
* Azure depolama 'yı kullanmaya başlamak için bkz. [Azure depolama 'Ya giriş](../storage/common/storage-introduction.md).
* Diğer yararlı Azure Otomasyonu runbook 'larını bulmak için bkz. [Azure Otomasyonu Için runbook ve modül galerileri](automation-runbook-gallery.md).
* Diğer kullanışlı Kaynak Yöneticisi şablonları bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/)


