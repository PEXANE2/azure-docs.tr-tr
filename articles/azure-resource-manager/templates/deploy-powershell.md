---
title: PowerShell ve şablon ile kaynakları dağıtma
description: Azure 'a kaynak dağıtmak için Azure Resource Manager ve Azure PowerShell kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
ms.topic: conceptual
ms.date: 07/21/2020
ms.openlocfilehash: 64993b526b67430266a8b3e85e3bcc233a3e28a3
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079528"
---
# <a name="deploy-resources-with-arm-templates-and-azure-powershell"></a>ARM şablonları ve Azure PowerShell kaynak dağıtma

Bu makalede, kaynaklarınızı Azure 'a dağıtmak için Azure Resource Manager şablonlarla (ARM şablonları) Azure PowerShell nasıl kullanılacağı açıklanmaktadır. Azure çözümlerinizi dağıtma ve yönetme kavramlarını bilmiyorsanız, bkz. [şablon dağıtımına genel bakış](overview.md).

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınızı bir kaynak grubuna, aboneliğe, yönetim grubuna veya kiracıya hedefleyebilirsiniz. Çoğu durumda, dağıtımı bir kaynak grubuna hedefleyebilirsiniz. Daha büyük bir kapsamda ilke ve rol atamaları uygulamak için abonelik, yönetim grubu veya kiracı dağıtımlarını kullanın. Bir aboneliğe dağıtırken, bir kaynak grubu oluşturabilir ve kaynakları buna dağıtabilirsiniz.

Dağıtımın kapsamına bağlı olarak, farklı komutlar kullanırsınız.

* Bir **kaynak grubuna**dağıtmak Için, [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment)kullanın:

  ```azurepowershell
  New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile <path-to-template>
  ```

* Bir **aboneliğe**dağıtmak Için, New-AzSubscriptionDeployment kullanın:

  ```azurepowershell
  New-AzSubscriptionDeployment -Location <location> -TemplateFile <path-to-template>
  ```

  Abonelik düzeyi dağıtımları hakkında daha fazla bilgi için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](deploy-to-subscription.md).

* Bir **yönetim grubuna**dağıtmak Için, [New-AzManagementGroupDeployment](/powershell/module/az.resources/New-AzManagementGroupDeployment)' ı kullanın.

  ```azurepowershell
  New-AzManagementGroupDeployment -Location <location> -TemplateFile <path-to-template>
  ```

  Yönetim grubu düzeyi dağıtımları hakkında daha fazla bilgi için bkz. [Yönetim grubu düzeyinde kaynak oluşturma](deploy-to-management-group.md).

* Bir **kiracıya**dağıtmak Için, [New-AzTenantDeployment](/powershell/module/az.resources/new-aztenantdeployment)kullanın.

  ```azurepowershell
  New-AzTenantDeployment -Location <location> -TemplateFile <path-to-template>
  ```

  Kiracı düzeyinde dağıtımlar hakkında daha fazla bilgi için bkz. [kiracı düzeyinde kaynak oluşturma](deploy-to-tenant.md).

Bu makaledeki örnekler, kaynak grubu dağıtımlarını kullanır.

## <a name="prerequisites"></a>Önkoşullar

Dağıtılacak bir şablonunuz olması gerekir. Henüz bir hesabınız yoksa Azure hızlı başlangıç şablonları deposundan bir [örnek şablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) indirip kaydedin. Bu makalede kullanılan yerel dosya adı **c:\MyTemplates\azuredeploy.js**.

Şablonları dağıtmak için Azure Cloud Shell kullanmadığınız takdirde, Azure PowerShell yüklemeniz ve Azure 'a bağlanmanız gerekir:

- **Azure PowerShell cmdlet 'lerini yerel bilgisayarınıza yükler.** Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](/powershell/azure/get-started-azureps).
- ** [Connect-Azaccount](/powershell/module/az.accounts/connect-azaccount)kullanarak Azure 'a bağlanın**. Birden çok Azure aboneliğiniz varsa, [set-AzContext](/powershell/module/Az.Accounts/Set-AzContext)' i de çalıştırmanız gerekebilir. Daha fazla bilgi için bkz. [birden çok Azure aboneliği kullanma](/powershell/azure/manage-subscriptions-azureps).

## <a name="deploy-local-template"></a>Yerel şablon dağıt

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır. Kaynak grubunun adı yalnızca alfasayısal karakterler, noktalar, alt çizgiler, kısa çizgiler ve parantezler içerebilir. En fazla 90 karakter olabilir. Nokta ile bitemez.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -Name ExampleDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json
```

Dağıtımın tamamlanması birkaç dakika sürebilir.

## <a name="deployment-name"></a>Dağıtım adı

Yukarıdaki örnekte, dağıtımı adlandırmış olursunuz `ExampleDeployment` . Dağıtım için bir ad sağlamazsanız, şablon dosyasının adı kullanılır. Örneğin, adlı bir şablonu dağıtırsanız `azuredeploy.json` ve dağıtım adı belirtmezseniz, dağıtım adlandırılır `azuredeploy` .

Bir dağıtımı her çalıştırdığınızda, kaynak grubunun dağıtım geçmişine dağıtım adı ile bir giriş eklenir. Başka bir dağıtım çalıştırırsanız ve aynı adı verirseniz, önceki girdi geçerli dağıtımla değiştirilmiştir. Dağıtım geçmişinde benzersiz girişleri sürdürmek istiyorsanız, her dağıtıma benzersiz bir ad verin.

Benzersiz bir ad oluşturmak için rastgele bir sayı atayabilirsiniz.

```azurepowershell-interactive
$suffix = Get-Random -Maximum 1000
$deploymentName = "ExampleDeployment" + $suffix
```

Veya bir tarih değeri ekleyin.

```azurepowershell-interactive
$today=Get-Date -Format "MM-dd-yyyy"
$deploymentName="ExampleDeployment"+"$today"
```

Aynı dağıtım adına sahip aynı kaynak grubuna eşzamanlı dağıtımlar çalıştırırsanız, yalnızca son dağıtım tamamlanmıştır. Tamamlanmayan aynı ada sahip dağıtımlar son dağıtımla değiştirilmiştir. Örneğin, adlı bir depolama hesabı dağıtan adlı bir dağıtım çalıştırırsanız `newStorage` `storage1` ve aynı zamanda adlı bir depolama hesabı dağıtan adlı bir dağıtımı çalıştırırsanız `newStorage` `storage2` , yalnızca bir depolama hesabı dağıtırsınız. Elde edilen depolama hesabının adı `storage2` .

Ancak, adlı bir depolama hesabı dağıtan adlı bir dağıtımı çalıştırırsanız `newStorage` `storage1` ve bu işlemi tamamladıktan hemen sonra adlı bir depolama hesabı dağıtan adlı başka bir dağıtım çalıştırırsanız `newStorage` `storage2` , iki depolama hesabınız olur. Biri olarak adlandırılır `storage1` ve diğeri olarak adlandırılır `storage2` . Ancak, dağıtım geçmişinde yalnızca bir girişiniz vardır.

Her dağıtım için benzersiz bir ad belirttiğinizde, bunları çakışma olmadan eşzamanlı olarak çalıştırabilirsiniz. Adlı bir depolama hesabı dağıtan adlı bir dağıtım çalıştırırsanız `newStorage1` `storage1` ve aynı zamanda adında bir depolama hesabı dağıtan adlı başka bir dağıtımı çalıştırırsanız `newStorage2` `storage2` , dağıtım geçmişinde iki depolama hesabınız ve iki giriş vardır.

Eşzamanlı dağıtımlar ile çakışmalardan kaçınmak ve dağıtım geçmişinde benzersiz girişler sağlamak için her dağıtıma benzersiz bir ad verin.

## <a name="deploy-remote-template"></a>Uzak şablonu dağıtma

ARM şablonlarını yerel makinenizde depolamak yerine, bunları bir dış konumda depolamayı tercih edebilirsiniz. Şablonları bir kaynak denetimi deposunda (GitHub gibi) saklayabilirsiniz. Ya da, bunları kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında saklayabilirsiniz.

Dış şablon dağıtmak için **Templateuri** parametresini kullanın. Örnek şablonu GitHub 'dan dağıtmak için örnekteki URI 'yi kullanın.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Önceki örnekte, şablon için genel olarak erişilebilir bir URI gerekir ve bu, şablonunuz önemli verileri içermemelidir. Gizli veriler (yönetici parolası gibi) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre olarak geçirin. Ancak, şablonunuzun herkese açık bir şekilde erişilebilir olmasını istemiyorsanız, bunu özel bir depolama kapsayıcısında depolayarak koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](secure-template-with-sas-token.md). Öğreticiye gitmek için bkz. [öğretici: ARM şablon dağıtımında Azure Key Vault tümleştirme](template-tutorial-use-key-vault.md).

## <a name="preview-changes"></a>Değişiklikleri Önizle

Şablonunuzu dağıtmadan önce, şablonun ortamınızda yapacağız değişiklikleri önizleyebilirsiniz. Şablonun beklediğiniz değişiklikleri yaptığını doğrulamak için [ne yapılır işlemini](template-deploy-what-if.md) kullanın. Ayrıca, şablonu hatalara yönelik olarak doğrular.

## <a name="deploy-from-azure-cloud-shell"></a>Azure Cloud Shell dağıtma

Şablonunuzu dağıtmak için [Azure Cloud Shell](https://shell.azure.com) kullanabilirsiniz. Dış şablon dağıtmak için şablonun URI 'sini sağlayın. Yerel bir şablon dağıtmak için, önce şablonunuzu Cloud Shell depolama hesabına yüklemeniz gerekir. Dosyaları kabuğa yüklemek için kabuk penceresinden **dosyaları karşıya yükle/indir** menü simgesini seçin.

Cloud Shell açmak için, [https://shell.azure.com](https://shell.azure.com) Aşağıdaki kod bölümünde yer alarak **dene** ' ye gidin veya deneyin ' i seçin:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Kodu kabuğa yapıştırmak için, kabuğun içine sağ tıklayıp **Yapıştır**' ı seçin.

## <a name="pass-parameter-values"></a>Parametre değerlerini geçir

Parametre değerlerini geçirmek için satır içi parametreleri ya da bir parametre dosyasını kullanabilirsiniz.

### <a name="inline-parameters"></a>Satır içi parametreler

Satır içi parametreleri geçirmek için, komutuyla parametrenin adlarını sağlayın `New-AzResourceGroupDeployment` . Örneğin, bir şablona bir dize ve dizi geçirmek için şunu kullanın:

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString "inline string" `
  -exampleArray $arrayParam
```

Ayrıca dosyanın içeriğini alabilir ve bu içeriği satır içi bir parametre olarak sağlayabilirsiniz.

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString $(Get-Content -Path c:\MyTemplates\stringcontent.txt -Raw) `
  -exampleArray $arrayParam
```

Bir dosyadan parametre değeri alma, yapılandırma değerleri sağlamanız gerektiğinde faydalıdır. Örneğin, [bir Linux sanal makinesi için Cloud-init değerleri](../../virtual-machines/linux/using-cloud-init.md)sağlayabilirsiniz.

Nesneler dizisini geçirmeniz gerekiyorsa, PowerShell 'de karma tablolar oluşturun ve bunları bir diziye ekleyin. Dağıtım sırasında bu diziyi bir parametre olarak geçirin.

```powershell
$hash1 = @{ Name = "firstSubnet"; AddressPrefix = "10.0.0.0/24"}
$hash2 = @{ Name = "secondSubnet"; AddressPrefix = "10.0.1.0/24"}
$subnetArray = $hash1, $hash2
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleArray $subnetArray
```

### <a name="parameter-files"></a>Parametre dosyaları

Komut dosyanıza satır içi değer olarak parametre geçirmek yerine, parametre değerlerini içeren bir JSON dosyası kullanmayı daha kolay bulabilirsiniz. Parametre dosyası yerel bir dosya veya erişilebilir bir URI 'ye sahip bir dış dosya olabilir.

Parametre dosyası hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi parametre dosyası oluşturma](parameter-files.md).

Yerel bir parametre dosyasını geçirmek için **Templateparameterfile** parametresini kullanın:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Dış parametre dosyasını geçirmek için **Templateparameteruri** parametresini kullanın:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

## <a name="next-steps"></a>Sonraki adımlar

- Hata aldığınızda başarılı bir dağıtıma geri dönmek için, [başarılı bir dağıtımda hata durumunda geri alma](rollback-on-error.md)konusuna bakın.
- Kaynak grubunda var olan, ancak şablonda tanımlanmamış kaynakların nasıl işleneceğini belirtmek için bkz. [Azure Resource Manager Dağıtım modları](deployment-modes.md).
- Şablonunuzda parametrelerin nasıl tanımlanacağını anlamak için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
- SAS belirteci gerektiren bir şablonu dağıtma hakkında daha fazla bilgi için bkz. [özel şablonu SAS belirteci Ile dağıtma](secure-template-with-sas-token.md).
