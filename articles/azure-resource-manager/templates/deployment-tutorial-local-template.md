---
title: Öğretici-yerel bir Azure Resource Manager şablonu dağıtma
description: Yerel bilgisayarınızdan Azure Resource Manager şablonu dağıtmayı öğrenin
ms.date: 05/20/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: ef52d20b20446e3b8f3b761949bf255b99d358cc
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87499594"
---
# <a name="tutorial-deploy-a-local-azure-resource-manager-template"></a>Öğretici: yerel bir Azure Resource Manager şablonu dağıtma

Yerel makinenizden Azure Resource Manager şablonu dağıtmayı öğrenin. Yaklaşık **8 dakika** sürer.

Bu öğretici bir serinin birincisidir. Seriler aracılığıyla ilerleyerek, bağlantılı bir şablon oluşturarak şablonu modüle getirin, bağlantılı şablonu bir depolama hesabında depolar ve SAS belirtecini kullanarak bağlı şablonu güvenli hale getirin ve şablonları dağıtmak için bir DevOp Işlem hattı oluşturmayı öğreneceksiniz. Bu seri, şablon dağıtımına odaklanır.  Şablon geliştirmeyi öğrenmek isterseniz, [Başlangıç öğreticilerine](./template-tutorial-create-first-template.md)bakın.

## <a name="get-tools"></a>Araçları al

Şablonları dağıtmanız için ihtiyacınız olan araçlara sahip olduğunuzdan başlayalım.

### <a name="command-line-deployment"></a>Komut satırı dağıtımı

Şablonu dağıtmak için Azure PowerShell ya da Azure CLı gerekir. Yükleme yönergeleri için bkz.:

- [Azure PowerShell yüklensin](/powershell/azure/install-az-ps)
- [Windows'da Azure CLI'yi yükleme](/cli/azure/install-azure-cli-windows)
- [Linux 'ta Azure CLı 'yı yükler](/cli/azure/install-azure-cli-linux)

Azure PowerShell veya Azure CLı yükledikten sonra, ilk kez oturum açarak emin olun. Yardım için bkz. [oturum açma-PowerShell](/powershell/azure/install-az-ps#sign-in) veya [Oturum Açma-Azure CLI](/cli/azure/get-started-with-azure-cli#sign-in).

### <a name="editor-optional"></a>Düzenleyici (Isteğe bağlı)

Şablonlar JSON dosyalarıdır. Şablonları gözden geçirmek/düzenlemek için iyi bir JSON düzenleyicisine ihtiyacınız vardır. Kaynak Yöneticisi Araçları uzantısı ile Visual Studio Code önerilir. Bu araçları yüklemeniz gerekiyorsa bkz. [hızlı başlangıç: Visual Studio Code ile Azure Resource Manager şablonları oluşturma](quickstart-create-templates-use-visual-studio-code.md).

## <a name="review-template"></a>Şablonu gözden geçir

Şablon bir depolama hesabı, App Service planı ve Web uygulaması dağıtır. Şablonu oluşturmaya ilgileniyorsanız, [hızlı başlangıç şablonları hakkında öğreticiye](template-tutorial-quickstart-template.md)gidebilirsiniz. Ancak bu öğreticiyi tamamlamak için gerekli değildir.

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/local-template/azuredeploy.json":::

> [!IMPORTANT]
> Depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakam ve küçük harf kullanılmalıdır. Ad benzersiz olmalıdır. Şablonda, depolama hesabı adı "depola" eklenmiş proje adı ve proje adı 3 ila 11 karakter arasında olmalıdır. Bu nedenle, proje adı depolama hesabı adı gereksinimlerini karşılamalıdır ve 11 ' den az karakter içermelidir.

Şablonun bir kopyasını. JSON uzantısıyla yerel bilgisayarınıza kaydedin, örneğin, azuredeploy.js. Bu şablonu öğreticide daha sonra dağıtırsınız.

## <a name="sign-in-to-azure"></a>Azure’da oturum açma

Bir şablon dağıtmak için Azure PowerShell/Azure CLı ile çalışmaya başlamak için Azure kimlik bilgilerinizle oturum açın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Connect-AzAccount
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az login
```

---

Birden çok Azure aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin:

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
Set-AzContext [SubscriptionID/SubscriptionName]
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az account set --subscription [SubscriptionID/SubscriptionName]
```

---

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Bir şablonu dağıtırken, kaynakları içerecek bir kaynak grubu belirtirsiniz. Dağıtım komutunu çalıştırmadan önce, kaynak grubunu Azure CLı veya Azure PowerShell ile oluşturun. Azure PowerShell ve Azure CLı arasında seçim yapmak için aşağıdaki kod bölümündeki sekmeleri seçin. Bu makaledeki CLı örnekleri bash kabuğu için yazılmıştır.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource and resource group names"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup `
  -Name $resourceGroupName `
  -Location "Central US"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
echo "Enter a project name that is used to generate resource and resource group names:"
read projectName
resourceGroupName="${projectName}rg"

az group create \
  --name $resourceGroupName \
  --location "Central US"
```

---

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtmak için bir veya her iki dağıtım seçeneğini kullanın.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name"
$templateFile = Read-Host -Prompt "Enter the template file path and file name"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
  -Name DeployLocalTemplate `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile $templateFile `
  -projectName $projectName `
  -verbose
```

Azure PowerShell kullanarak şablon dağıtma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure PowerShell ile kaynak dağıtma](./deploy-powershell.md).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
echo "Enter the same project name:"
read projectName
echo "Enter the template file path and file name:"
read templateFile
resourceGroupName="${projectName}rg"

az deployment group create \
  --name DeployLocalTemplate \
  --resource-group $resourceGroupName \
  --template-file $templateFile \
  --parameters projectName=$projectName \
  --verbose
```

Azure CLı kullanarak şablon dağıtma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynak dağıtma](./deploy-cli.md).

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Yerel bir şablon dağıtmayı öğrendiniz. Sonraki öğreticide, şablonu bir ana şablona ve bağlı bir şablona ayırdınız ve bağlantılı şablonu nasıl depolayacağınızı ve güvenli hale ayarlayacağınızı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Bağlantılı şablonu dağıtma](./deployment-tutorial-linked-template.md)
