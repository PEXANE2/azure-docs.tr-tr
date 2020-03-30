---
title: PowerShell ve şablonile kaynakları dağıtma
description: Kaynakları Azure'a dağıtmak için Azure Kaynak Yöneticisi'ni ve Azure PowerShell'i kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: e595aa8f86a24e59c8e00d24ea8e9dcb0875a8f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153276"
---
# <a name="deploy-resources-with-arm-templates-and-azure-powershell"></a>ARM şablonları ve Azure PowerShell ile kaynakları dağıtma

Kaynaklarınızı Azure'a dağıtmak için Azure Kaynak Yöneticisi (ARM) şablonlarıyla Azure PowerShell'i nasıl kullanacağınızı öğrenin. Azure çözümlerinizi dağıtma ve yönetme kavramları hakkında daha fazla bilgi için [şablon dağıtımına genel bakış](overview.md)bölümüne bakın.

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınızı bir kaynak grubuna, abonelike, yönetim grubuna veya kiracıya hedefleyebilirsiniz. Çoğu durumda, bir kaynak grubuna dağıtım hedefleceksiniz. İlkeleri ve rol ödevlerini daha geniş bir kapsamda uygulamak için abonelik, yönetim grubu veya kiracı dağıtımlarını kullanın. Bir aboneye dağıtılırken, bir kaynak grubu oluşturabilir ve ona kaynak dağıtabilirsiniz.

Dağıtımın kapsamına bağlı olarak, farklı komutlar kullanırsınız.

Bir kaynak **grubuna**dağıtmak için [Yeni-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment)kullanın:

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile <path-to-template>
```

Bir **aboneğe**dağıtmak için Yeni-AzSubscriptionDeployment'ı kullanın:

```azurepowershell
New-AzSubscriptionDeployment -Location <location> -TemplateFile <path-to-template>
```

Abonelik düzeyi dağıtımları hakkında daha fazla bilgi için [bkz.](deploy-to-subscription.md)

Bir yönetim **grubuna**dağıtmak için [Yeni-AzManagementGroupDeployment'ı](/powershell/module/az.resources/New-AzManagementGroupDeployment)kullanın.

```azurepowershell
New-AzManagementGroupDeployment -Location <location> -TemplateFile <path-to-template>
```

Yönetim grubu düzeyindeki dağıtımlar hakkında daha fazla bilgi için [bkz.](deploy-to-management-group.md)

**Kiracıya**dağıtmak için [Yeni-AzTenantDeployment'ı](/powershell/module/az.resources/new-aztenantdeployment)kullanın.

```azurepowershell
New-AzTenantDeployment -Location <location> -TemplateFile <path-to-template>
```

Kiracı düzeyindeki dağıtımlar hakkında daha fazla bilgi için [bkz.](deploy-to-tenant.md)

Bu makaledeki örneklerkaynak grubu dağıtımlarını kullanır.

## <a name="prerequisites"></a>Ön koşullar

Dağıtmak için bir şablona ihtiyacınız var. Zaten bir tane yoksa, Azure Quickstart [şablonları](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) repo'dan bir örnek şablon indirin ve kaydedin. Bu makalede kullanılan yerel dosya adı **c:\MyTemplates\azuredeploy.json**.

Şablonları dağıtmak için Azure Bulut Kabuğu'nu kullanmadığınız sürece, Azure PowerShell'i yüklemeniz ve Azure'a bağlanmanız gerekir:

- **Azure PowerShell cmdlets'i yerel bilgisayarınıza yükleyin.** Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](/powershell/azure/get-started-azureps).
- ** [Connect-AZAccount'ı](/powershell/module/az.accounts/connect-azaccount)kullanarak Azure'a bağlanın.** Birden çok Azure aboneliğiniz varsa, [Set-AzContext'ı](/powershell/module/Az.Accounts/Set-AzContext)da çalıştırmanız gerekebilir. Daha fazla bilgi için [bkz.](/powershell/azure/manage-subscriptions-azureps)

## <a name="deploy-local-template"></a>Yerel şablonu dağıtma

Aşağıdaki örnek bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır. Kaynak grubunun adı yalnızca alfasayısal karakterler, dönemler, alt çizerler, tireler ve parantez içerebilir. 90 karaktere kadar olabilir. Bir dönemde bitmez.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json
```

Dağıtımın tamamlanması birkaç dakika sürebilir.

## <a name="deploy-remote-template"></a>Uzak şablonu dağıtma

ARM şablonlarını yerel makinenizde depolamak yerine, bunları harici bir konumda saklamayı tercih edebilirsiniz. Şablonları kaynak denetim deposunda (GitHub gibi) depolayabilirsiniz. Veya, kuruluşunuzdaki paylaşılan erişim için bunları bir Azure depolama hesabında saklayabilirsiniz.

Harici bir şablon dağıtmak için **TemplateUri** parametresini kullanın. Örnek şablonu GitHub'dan dağıtmak için uri'yi örnekte kullanın.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Önceki örnek, şablonunuz hassas verileri içermemelidir, çünkü çoğu senaryo için çalışır şablon için kamuya açık bir URI gerektirir. Hassas verileri (yönetici parolası gibi) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre olarak geçirin. Ancak, şablonunuzun herkese açık olmasını istemiyorsanız, şablonu özel bir depolama kapsayıcısında depolayarak şablonu koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren bir şablonu dağıtma hakkında bilgi [için](secure-template-with-sas-token.md)bkz. Öğreticiye geçmek için [bkz: Öğretici: Azure Anahtar Kasasını ARM şablon dağıtımına entegre edin.](template-tutorial-use-key-vault.md)

## <a name="deploy-from-azure-cloud-shell"></a>Azure Bulut BulutU Kabuğundan Dağıtma

Şablonunuzu dağıtmak için [Azure Bulut Kabuğu'nu](https://shell.azure.com) kullanabilirsiniz. Harici bir şablon dağıtmak için şablonun URI'sini sağlayın. Yerel bir şablon dağıtmak için öncelikle şablonunuzu Bulut Shell'inizin depolama hesabına yüklemeniz gerekir. Dosyayı kabuğuna yüklemek için, kabuk penceresinden **Dosyaları Yükle/İndir** menüsü simgesini seçin.

Bulut kabuğunu açmak için [https://shell.azure.com](https://shell.azure.com)aşağıdaki kod bölümünden Try-It'e göz atın veya **Try-It'i** seçin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Kodu kabuğun içine yapıştırmak için kabuğun içine sağ tıklayın ve ardından **Yapıştır'ı**seçin.

## <a name="pass-parameter-values"></a>Geçiş parametresi değerleri

Parametre değerlerini geçmek için satır içinde parametreler ilerler veya parametre dosyasını kullanabilirsiniz.

### <a name="inline-parameters"></a>Satır dışı parametreler

Satır dışı parametreleri geçirmek için, `New-AzResourceGroupDeployment` komutu ile parametre adlarını sağlayın. Örneğin, bir dize ve diziyi şablona geçirmek için şunları kullanın:

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString "inline string" `
  -exampleArray $arrayParam
```

Ayrıca dosyanın içeriğini alabilir ve bu içeriği satır satırlı parametre olarak sağlayabilirsiniz.

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString $(Get-Content -Path c:\MyTemplates\stringcontent.txt -Raw) `
  -exampleArray $arrayParam
```

Yapılandırma değerleri sağlamanız gerektiğinde dosyadan parametre değeri almak yararlıdır. Örneğin, [bir Linux sanal makinesi için bulut-init değerleri](../../virtual-machines/linux/using-cloud-init.md)sağlayabilirsiniz.

Bir dizi nesnenin geçmesi gerekiyorsa, PowerShell'de karma tablolar oluşturun ve bunları bir diziye ekleyin. Dağıtım sırasında bu diziyi parametre olarak geçirin.

```powershell
$hash1 = @{ Name = "firstSubnet"; AddressPrefix = "10.0.0.0/24"}
$hash2 = @{ Name = "secondSubnet"; AddressPrefix = "10.0.1.0/24"}
$subnetArray = $hash1, $hash2
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleArray $subnetArray
```

### <a name="parameter-files"></a>Parametre dosyaları

Komut dosyanızdaki satır değerleri olarak parametreleri geçirmek yerine, parametre değerlerini içeren bir JSON dosyasını kullanmayı daha kolay bulabilirsiniz. Parametre dosyası yerel bir dosya veya erişilebilir bir URI ile harici bir dosya olabilir.

Parametre dosyası hakkında daha fazla bilgi için [kaynak yöneticisi parametre dosyası oluştur'a](parameter-files.md)bakın.

Yerel bir parametre dosyasını geçmek için **TemplateParameterFile** parametresini kullanın:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Harici bir parametre dosyasını geçmek için **TemplateParameterUri** parametresini kullanın:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

## <a name="test-template-deployments"></a>Şablon dağıtımlarını test edin

Şablonunuzu ve parametre değerlerinizi gerçekten kaynak dağıtmadan sınamak için [Test-AzResourceGroupDeployment'ı](/powershell/module/az.resources/test-azresourcegroupdeployment)kullanın. 

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json -storageAccountType Standard_GRS
```

Hata algılanmamışsa, komut yanıt olmadan tamamlanır. Bir hata algılanırsa, komut bir hata iletisi döndürür. Örneğin, depolama hesabı SKU için yanlış bir değer geçen, aşağıdaki hatayı döndürür:

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Şablonunuzda sözdizimi hatası varsa, komut şablonu ayrışdıramayacağını belirten bir hata döndürür. İleti, ayrıştma hatasının satır numarasını ve konumunu gösterir.

```powershell
Test-AzResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

## <a name="next-steps"></a>Sonraki adımlar

- Bir hata aldığınızda başarılı bir dağıtıma geri dönmek [için, başarılı dağıtımhatasında Rollback'i](rollback-on-error.md)görün.
- Kaynak grubunda bulunan ancak şablonda tanımlanmayan kaynakların nasıl işleyeceğini belirtmek için Azure [Kaynak Yöneticisi dağıtım modlarına](deployment-modes.md)bakın.
- Şablonunuzdaparametrelerin nasıl tanımlandığını anlamak için [bkz.](template-syntax.md)
- SAS belirteci gerektiren bir şablonu dağıtma hakkında bilgi [için](secure-template-with-sas-token.md)bkz.
