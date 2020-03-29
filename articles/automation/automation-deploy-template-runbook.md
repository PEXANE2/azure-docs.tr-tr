---
title: Azure Otomasyonu runbook'una Azure Kaynak Yöneticisi şablonu dağıtma
description: Bir runbook'tan Azure Depolama'da depolanan bir Azure Kaynak Yöneticisi şablonu dağıtma
services: automation
ms.subservice: process-automation
ms.date: 03/16/2018
ms.topic: conceptual
keywords: powershell, runbook, json, azure otomasyon
ms.openlocfilehash: d4adbea42cda54380ad32dce40cfa0d8391ee490
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75366643"
---
# <a name="deploy-an-azure-resource-manager-template-in-an-azure-automation-powershell-runbook"></a>Azure Otomasyonu PowerShell runbook’unda Azure Resource Manager şablonu dağıtma

[Azure Kaynak Yönetimi şablonu](../azure-resource-manager/resource-manager-create-first-template.md)kullanarak bir Azure kaynağını dağıtan bir [Azure Otomasyon PowerShell runbook](automation-first-runbook-textual-powershell.md) yazabilirsiniz.

Bunu yaparak Azure kaynaklarının dağıtımını otomatikleştirebilirsiniz. Kaynak Yöneticisi şablonlarınızı Azure Depolama gibi merkezi ve güvenli bir konumda koruyabilirsiniz.

Bu makalede, yeni bir Azure Depolama hesabı dağıtmak için [Azure Depolama'da](../storage/common/storage-introduction.md) depolanan Kaynak Yöneticisi şablonu kullanan bir PowerShell runbook oluşturduk.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap için kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-sec-configure-azure-runas-account.md).  Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Kaynak Yöneticisi şablonu depolanabilecek [Azure Depolama hesabı](../storage/common/storage-create-storage-account.md)
* Azure Powershell yerel bir makineye yüklendi. Azure PowerShell'i nasıl alacağınız hakkında bilgi almak için [Azure Powershell'i Yükle ve yapılandırma](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) ya da yapılandırma ya da azure powershell'i nasıl edindiğinize ilişkin bilgi için bkz.

## <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma

Bu örnekte, yeni bir Azure Depolama hesabı dağıtan bir Kaynak Yöneticisi şablonu kullanırız.

Metin düzenleyicisinde aşağıdaki metni kopyalayın:

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

Dosyayı yerel olarak `TemplateTest.json`' olarak kaydedin.

## <a name="save-the-resource-manager-template-in-azure-storage"></a>Kaynak Yöneticisi şablonu Azure Depolama'da kaydedin

Şimdi PowerShell'i azure depolama dosya paylaşımı `TemplateTest.json` oluşturmak ve dosyayı yüklemek için kullanıyoruz.
Azure portalında dosya paylaşımı oluşturma ve dosya yükleme hakkında talimatlar [için](../storage/files/storage-dotnet-how-to-use-files.md)bkz.

PowerShell'i yerel makinenizde başlatın ve bir dosya paylaşımı oluşturmak ve Kaynak Yöneticisi şablonunu bu dosya paylaşımına yüklemek için aşağıdaki komutları çalıştırın.

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

## <a name="create-the-powershell-runbook-script"></a>PowerShell runbook komut dosyası oluşturma

Şimdi, dosyayı `TemplateTest.json` Azure Depolama'dan alan ve şablonu dağıtarak yeni bir Azure Depolama hesabı oluşturmak üzere dağıtan bir PowerShell komut dosyası oluşturuyoruz.

Metin düzenleyicisinde aşağıdaki metni yapıştırın:

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

Dosyayı yerel olarak `DeployTemplate.ps1`' olarak kaydedin.

## <a name="import-and-publish-the-runbook-into-your-azure-automation-account"></a>Runbook'u Azure Otomasyon hesabınıza alma ve yayımlama

Şimdi, runbook'u Azure Otomasyon hesabınıza almak ve runbook'u yayınlamak için PowerShell'i kullanıyoruz.
Azure portalında bir runbook'un nasıl içe aktarılabildiğini ve yayımlayacağı hakkında bilgi için Azure [Otomasyonunda runbook'ları Yönet'e](manage-runbooks.md)bakın.

PowerShell `DeployTemplate.ps1` runbook olarak Otomasyon hesabınıza almak için aşağıdaki PowerShell komutlarını çalıştırın:

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

Şimdi [runbook'u Başlat-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet'i arayarak başlatıyoruz.

Azure portalında runbook'u başlatma hakkında bilgi için azure [otomasyonunda runbook başlatma'ya](automation-starting-a-runbook.md)bakın.

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

Runbook çalışır ve çalıştırarak `$job.Status`durumunu kontrol edebilirsiniz.

Runbook, Kaynak Yöneticisi şablonuna alır ve yeni bir Azure Depolama hesabı dağıtmak için kullanır.
Yeni depolama hesabının aşağıdaki komutu çalıştırarak oluşturulduğunu görebilirsiniz:
```powershell
Get-AzureRmStorageAccount
```

## <a name="summary"></a>Özet

İşte bu kadar! Artık tüm Azure kaynaklarınızı dağıtmak için Azure Otomasyonu ve Azure Depolama ve Kaynak Yöneticisi şablonlarını kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Kaynak Yöneticisi şablonları hakkında daha fazla bilgi edinmek için [Azure Kaynak Yöneticisi'ne genel bakış](../azure-resource-manager/management/overview.md)
* Azure Depolama'ya başlamak [için](../storage/common/storage-introduction.md)bkz.
* Diğer kullanışlı Azure Otomasyon runbook'larını bulmak [için Azure Otomasyonu için Runbook ve modül galerilerine](automation-runbook-gallery.md)bakın.
* Diğer kullanışlı Kaynak Yöneticisi şablonlarını bulmak için [Azure Hızlı Başlangıç Şablonlarına](https://azure.microsoft.com/resources/templates/) bakın


