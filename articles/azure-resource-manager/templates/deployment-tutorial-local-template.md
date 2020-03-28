---
title: Öğretici - Yerel Azure Kaynak Yöneticisi şablonu dağıtma
description: Yerel bilgisayarınızdan Azure Kaynak Yöneticisi şablonunu nasıl dağıtılayınızı öğrenin
ms.date: 03/13/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: c8e3eb62fa52caeaa63808b6b9ea199bdff5c4da
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80082261"
---
# <a name="tutorial-deploy-a-local-azure-resource-manager-template"></a>Öğretici: Yerel bir Azure Kaynak Yöneticisi şablonu dağıtma

Yerel makinenizden azure kaynak yöneticisi şablonunu nasıl dağıtılayacaklarını öğrenin. Tamamlanması yaklaşık **8 dakika** sürer.

Bu öğretici bir serinin ilkidir. Seri boyunca ilerledikçe, bağlantılı bir şablon oluşturarak şablonu modüle eder, bağlantılı şablonu bir depolama hesabında saklar ve SAS belirteci kullanarak bağlantılı şablonu güvenli hale getirmekve şablonları dağıtmak için devop ardışık bir yapı oluşturmayı öğrenirsiniz. Bu seri şablon dağıtımına odaklanır.  Şablon geliştirmeyi öğrenmek istiyorsanız, [başlangıç eğitimlerine](./template-tutorial-create-first-template.md)bakın.

## <a name="get-tools"></a>Araçları alın

Şablonları dağıtmak için gereken araçlara sahip olduğundan emin olarak başlayalım.

### <a name="command-line-deployment"></a>Komut satırı dağıtımı

Şablonu dağıtmak için Azure PowerShell veya Azure CLI gerekir. Yükleme yönergeleri için bkz:

- [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps)
- [Windows'da Azure CLI'yi yükleme](/cli/azure/install-azure-cli-windows)
- [Azure CLI'yi Linux'a yükleyin](/cli/azure/install-azure-cli-linux)

Azure PowerShell veya Azure CLI'yi yükledikten sonra ilk kez oturum açarak oturum açtın. Yardım için oturum [açma - PowerShell](/powershell/azure/install-az-ps#sign-in) veya [Oturum Aç - Azure CLI](/cli/azure/get-started-with-azure-cli#sign-in)' ya bakın.

### <a name="editor-optional"></a>Editör (İsteğe Bağlı)

Şablonlar JSON dosyalarıdır. Şablonları gözden geçirmek/gözden geçirmek için iyi bir JSON düzenleyicisine ihtiyacınız vardır. Kaynak Yöneticisi Araçları uzantılı Visual Studio Code'u öneririz. Bu araçları yüklemeniz gerekiyorsa, [Azure Kaynak Yöneticisi şablonları oluşturmak için Visual Studio Kodunu Kullan'a](use-vs-code-to-create-template.md)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Bu öğreticide kullanılan şablon, [Quickstart şablonları hakkında öğreticide](template-tutorial-quickstart-template.md)kullanılan şablona benzer. Şablonu oluşturmak istiyorsanız, bu öğretici üzerinden gidebilirsiniz. Ancak bu öğretici yi tamamlamak için gerekli değildir.

Şablon, bir depolama hesabı, uygulama hizmet planı ve web uygulaması dağıtLar.

:::code language="json" source="~/resourcemanager-templates/get-started-deployment/local-template/azuredeploy.json":::

> [!IMPORTANT]
> Depolama hesabı adları uzunluk ve kullanım numaraları ve küçük harfler sadece 3 ve 24 karakter arasında olmalıdır. İsim benzersiz olmalı. Şablonda, depolama hesabı adı "depo" eklenen proje adıdır ve proje adı 3 ile 11 karakter arasında olmalıdır. Bu nedenle proje adı depolama hesabı adı gereksinimlerini karşılamalı ve 11 karakterden az karaktere sahiptir.

Şablonun bir kopyasını .json uzantısı olan azuredeploy.json ile yerel bilgisayarınıza kaydedin. Bu şablonu daha sonra öğreticide dağıtın.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Bir şablon dağıtmak için Azure PowerShell/Azure CLI ile çalışmaya başlamak için Azure kimlik bilgilerinizle oturum açın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
Connect-AzAccount
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az login
```

---

Birden çok Azure aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin:

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
Select-AzSubscription [SubscriptionID/SubscriptionName]
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az account set --subscription [SubscriptionID/SubscriptionName]
```

---

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Bir şablon dağıttığınızda, kaynakları içeren bir kaynak grubu belirtirsiniz. Dağıtım komutunu çalıştırmadan önce, Azure CLI veya Azure PowerShell ile kaynak grubu oluşturun. Azure PowerShell ve Azure CLI arasında seçim yapmak için aşağıdaki kod bölümündeki sekmeleri seçin. Bu makaledeki CLI örnekleri Bash kabuğu için yazılmıştır.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

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

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

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

Azure PowerShell'i kullanarak şablon dağıtma hakkında daha fazla bilgi edinmek için [Kaynak Yöneticisi şablonlarıyla kaynakları dağıt ve Azure PowerShell'e](./deploy-powershell.md)bakın.

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

Azure CLI'yi kullanarak şablon dağıtma hakkında daha fazla bilgi edinmek için kaynak [yöneticisi şablonlarıyla kaynakları dağıt ve Azure CLI'ye](./deploy-cli.md)bakın.

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Yerel bir şablonu nasıl dağıtabileceğinizi öğrendiniz. Bir sonraki öğreticide, şablonu bir ana şablona ve bağlantılı şablona ayırır sınız ve bağlantılı şablonu nasıl depolayıp güvenli hale getirtin.

> [!div class="nextstepaction"]
> [Bağlantılı bir şablon dağıtma](./deployment-tutorial-linked-template.md)
