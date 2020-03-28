---
title: Öğretici - şablona parametre ekleme
description: Yeniden kullanılabilir hale getirmek için Azure Kaynak Yöneticisi şablonunuza parametreler ekleyin.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: b4f8d5098fc9cf2f91139979cae430594edac5af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369863"
---
# <a name="tutorial-add-parameters-to-your-arm-template"></a>Öğretici: ARM şablonunuza parametreler ekleme

Önceki [öğreticide,](template-tutorial-add-resource.md)şablona bir depolama hesabı eklemeyi ve dağıtmayı öğrendiniz. Bu eğitimde, parametreler ekleyerek Azure Kaynak Yöneticisi (ARM) şablonunu nasıl geliştireceğinizi öğrenirsiniz. Bu öğreticinin tamamlanması yaklaşık **14 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

[Kaynaklarla ilgili eğitimi](template-tutorial-add-resource.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticinin sonunda, şablonunuzun aşağıdaki JSON'u vardı:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-storage/azuredeploy.json":::

Bu şablonda bir sorun olduğunu fark etmiş olabilirsiniz. Depolama hesabı adı sabit kodlanmış. Bu şablonu yalnızca her seferinde aynı depolama hesabını dağıtmak için kullanabilirsiniz. Farklı bir ada sahip bir depolama hesabı dağıtmak için, dağıtımlarınızı otomatikleştirmenin pratik bir yolu olmadığı belli olan yeni bir şablon oluşturmanız gerekir.

## <a name="make-template-reusable"></a>Şablonu yeniden kullanılabilir hale getirme

Şablonunuzu yeniden kullanılabilir hale getirmek için, bir depolama hesabı adına geçmek için kullanabileceğiniz bir parametre ekleyelim. Aşağıdaki örnekte vurgulanan JSON şablonunuzda nelerin değiştiğini gösterir. **storageName** parametresi bir dize olarak tanımlanır. En uzun, çok uzun olan adları önlemek için 24 karakterolarak ayarlanır.

Dosyanın tamamını kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-name/azuredeploy.json" range="1-26" highlight="4-10,15":::

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtalım. Aşağıdaki örnekte, şablonAzure CLI veya PowerShell ile dağıtılır. Depolama hesabı adını dağıtım komutundaki değerlerden biri olarak verdiğinize dikkat edin. Depolama hesabı adı için, önceki öğreticide kullandığınız adı sağlayın.

Kaynak grubunu oluşturmadıysanız, [bkz.](template-tutorial-create-first-template.md#create-resource-group) Örnek, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi **şablonDosya** değişkenini şablon dosyasına giden yola ayarladığınız varsayar.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addnameparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storageName "{your-unique-name}"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
az deployment group create \
  --name addnameparameter \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storageName={your-unique-name}
```

---

## <a name="understand-resource-updates"></a>Kaynak güncelleştirmelerini anlama

Önceki bölümde, daha önce oluşturduğunuz aynı ada sahip bir depolama hesabı dağıttınız. Kaynağın yeniden dağıtımdan nasıl etkilendiğini merak ediyor olabilirsiniz.

Kaynak zaten varsa ve özelliklerde değişiklik algılanmamışsa, hiçbir işlem yapılmaz. Kaynak zaten varsa ve bir özellik değiştiyse, kaynak güncelleştirilir. Kaynak yoksa, oluşturulur.

Güncelleştirmeleri işleme nin bu yolu, şablonunuzun Bir Azure çözümü için ihtiyacınız olan tüm kaynakları içerebileceği anlamına gelir. Şablonu güvenle yeniden dağıtabilir ve kaynakların yalnızca gerektiğinde değiştirildiğini veya oluşturulduğunu bilebilirsiniz. Örneğin, depolama hesabınıza dosya eklediyseniz, bu dosyaları kaybetmeden depolama hesabını yeniden dağıtabilirsiniz.

## <a name="customize-by-environment"></a>Ortama göre özelleştirme

Parametreler, belirli bir ortam için tasarlanmış değerler kullanarak dağıtımı özelleştirmenizi sağlar. Örneğin, geliştirme, test ve üretim için bir ortama dağıtım yapıp yaptığınıza bağlı olarak farklı değerleri geçebilirsiniz.

Önceki şablon her zaman bir Standard_LRS depolama hesabı dağıtılır. Ortama bağlı olarak farklı SNU'lar dağıtmak için esneklik isteyebilirsiniz. Aşağıdaki örnekte SKU için bir parametre eklemek için değişiklikler gösterilmektedir. Dosyanın tamamını kopyalayın ve şablonunuzun üzerine yapıştırın.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-sku/azuredeploy.json" range="1-40" highlight="10-23,32":::

**StorageSKU** parametresi varsayılan değere sahiptir. Bu değer, dağıtım sırasında bir değer belirtilmediğinde kullanılır. Ayrıca izin verilen değerlerin bir listesi vardır. Bu değerler, bir depolama hesabı oluşturmak için gereken değerlerle eşleşir. Şablonunuzun kullanıcılarının çalışmayan SDO'larda geçmesini istemezsiniz.

## <a name="redeploy-template"></a>Şablonu yeniden dağıtma

Tekrar göreve hazırsın. Varsayılan SKU **Standard_LRS**olarak ayarlandığı için, bu parametre için bir değer sağlamanız gerekmez.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addskuparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storageName "{your-unique-name}"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment group create \
  --name addskuparameter \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storageName={your-unique-name}
```

---

Şablonunuzun esnekliğini görmek için yeniden dağıtalım. Bu kez SKU parametresini **Standard_GRS**ayarlayın. Farklı bir depolama hesabı oluşturmak için yeni bir ad geçebilir veya varolan depolama hesabınızı güncelleştirmek için aynı adı kullanabilirsiniz. Her iki seçenek de çalışır.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name usedefaultsku `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storageName "{your-unique-name}" `
  -storageSKU Standard_GRS
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment group create \
  --name usedefaultsku \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storageSKU=Standard_GRS storageName={your-unique-name}
```

---

Son olarak, bir test daha çalıştıralım ve izin verilen değerlerden biri olmayan bir SKU'yu geçtiğinde ne olacağını görelim. Bu durumda, şablonunuzun bir kullanıcısının **temel** SNU'lardan biri olduğunu düşündüğü senaryoyu test ediyoruz.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name testskuparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storageName "{your-unique-name}" `
  -storageSKU basic
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment group create \
  --name testskuparameter \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storageSKU=basic storageName={your-unique-name}
```

---

Komut, hangi değerlere izin verildiğini belirten bir hata iletisiyle hemen başarısız olur. Kaynak Yöneticisi dağıtım başlamadan önce hatayı tanımlar.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silerek dağıttığınız kaynakları temizlemek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Parametreler ekleyerek [ilk öğreticide](template-tutorial-create-first-template.md) oluşturulan şablonu geliştirdiniz. Bir sonraki öğreticide, şablon işlevleri hakkında bilgi edineceksiniz.

> [!div class="nextstepaction"]
> [Şablon işlevleri ekleme](template-tutorial-add-functions.md)