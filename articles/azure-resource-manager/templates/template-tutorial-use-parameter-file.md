---
title: Tutorial - şablonu dağıtmak için parametre dosyasını kullanın
description: Azure Kaynak Yöneticisi şablonunuzu dağıtmak için kullanılacak değerleri içeren parametre dosyalarını kullanın.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 0f7b966d4241716d71779e966de5d408711e4543
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371787"
---
# <a name="tutorial-use-parameter-files-to-deploy-your-arm-template"></a>Öğretici: ARM şablonunuzu dağıtmak için parametre dosyalarını kullanma

Bu eğitimde, dağıtım sırasında geçtiğiniz değerleri depolamak için [parametre dosyalarını](parameter-files.md) nasıl kullanacağınızı öğrenirsiniz. Önceki öğreticilerde, dağıtım komutunuzla satır içinde parametreler kullandınız. Bu yaklaşım, Azure Kaynak Yöneticisi (ARM) şablonunuzu sınamak için işe yaradı, ancak dağıtımları otomatikleştirirken ortamınız için bir dizi değer geçirmek daha kolay olabilir. Parametre dosyaları, belirli bir ortam için parametre değerlerinin paketlemikolaylaştırır. Bu eğitimde, geliştirme ve üretim ortamları için parametre dosyaları oluşturursunuz. Tamamlanması yaklaşık **12 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

Etiketler le ilgili [eğitimi](template-tutorial-add-tags.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Şablonunuzun dağıtım sırasında sağlayabileceğiniz birçok parametresi vardır. Önceki öğreticinin sonunda şablonunuzun aşağıdaki gibi görünmesi

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json":::

Bu şablon iyi çalışır, ancak şimdi şablon için geçtiğiniz parametreleri kolayca yönetmek istiyorsunuz.

## <a name="add-parameter-files"></a>Parametre dosyaları ekleme

Parametre dosyaları, şablonunuza benzer bir yapıya sahip JSON dosyalarıdır. Dosyada, dağıtım sırasında geçmek istediğiniz parametre değerlerini sağlarsınız.

VS Code'da, aşağıdaki içeriği içeren yeni bir dosya oluşturun. Dosyayı **azuredeploy.parameters.dev.json**adıyla kaydet.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.dev.json":::

Bu dosya, geliştirme ortamı için parametre dosyanızdır. Depolama hesabı için Standard_LRS kullandığına, kaynakları **dev** önekiyle adlandırır ve **Çevre** etiketini **Dev**olarak ayarlar.

Yine, aşağıdaki içeriği içeren yeni bir dosya oluşturun. **Azuredeploy.parameters.prod.json**adıyla dosyayı kaydedin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.parameters.prod.json":::

Bu dosya, üretim ortamı için parametre dosyanızdır. Depolama hesabı için Standard_GRS kullandığına, kaynakları **contoso** önekiyle adlandırır ve **Çevre** etiketini **Üretim**olarak ayarlar. Gerçek bir üretim ortamında, ücretsiz dışında bir SKU ile bir uygulama hizmeti kullanmak istiyorum, ama biz bu öğretici için bu SKU kullanmaya devam edeceğiz.

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtmak için Azure CLI veya Azure PowerShell'i kullanın.

Şablonunuzun son testi olarak iki yeni kaynak grubu oluşturalım. Biri dev çevre için, diğeri de üretim ortamı için.

İlk olarak, dev ortamına konuşlandırılacağız.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
$templateFile = "{path-to-the-template-file}"
$parameterFile="{path-to-azuredeploy.parameters.dev.json}"
New-AzResourceGroup `
  -Name myResourceGroupDev `
  -Location "East US"
New-AzResourceGroupDeployment `
  -Name devenvironment `
  -ResourceGroupName myResourceGroupDev `
  -TemplateFile $templateFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
templateFile="{path-to-the-template-file}"
devParameterFile="{path-to-azuredeploy.parameters.dev.json}"
az group create \
  --name myResourceGroupDev \
  --location "East US"
az deployment group create \
  --name devenvironment \
  --resource-group myResourceGroupDev \
  --template-file $templateFile \
  --parameters $devParameterFile
```

---

Şimdi, üretim ortamına konuşlandırılacağız.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
$parameterFile="{path-to-azuredeploy.parameters.prod.json}"
New-AzResourceGroup `
  -Name myResourceGroupProd `
  -Location "West US"
New-AzResourceGroupDeployment `
  -Name prodenvironment `
  -ResourceGroupName myResourceGroupProd `
  -TemplateFile $templateFile `
  -TemplateParameterFile $parameterFile
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
prodParameterFile="{path-to-azuredeploy.parameters.prod.json}"
az group create \
  --name myResourceGroupProd \
  --location "West US"
az deployment group create \
  --name prodenvironment \
  --resource-group myResourceGroupProd \
  --template-file $templateFile \
  --parameters $prodParameterFile
```

---

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Azure portalındaki kaynak gruplarını keşfederek dağıtımı doğrulayabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol menüden **Kaynak gruplarını**seçin.
1. Bu öğreticide dağıttığınız iki yeni kaynak grubunu görürsünüz.
1. Kaynak grubunu seçin ve dağıtılan kaynakları görüntüleyin. Bu ortam için parametre dosyanızda belirttiğiniz değerlerle eşleştiklerine dikkat edin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin. Bu seriyi tamamladıysanız, silmeniz gereken üç kaynak grubu var : myResourceGroup, myResourceGroupDev ve myResourceGroupProd.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, şablonları Azure'a dağıtmaya giriş ini tamamladınız. Geri bildirim bölümünde herhangi bir yorumunuz ve öneriniz varsa bize bildirin. Teşekkür ederiz!

Bir sonraki öğretici seri, şablonları dağıtma hakkında daha fazla ayrıntıya girer.

> [!div class="nextstepaction"]
> [Yerel şablon dağıtma](./deployment-tutorial-local-template.md)
