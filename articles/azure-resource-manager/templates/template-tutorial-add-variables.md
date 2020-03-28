---
title: Öğretici - şablona değişken ekleme
description: Sözdizimini basitleştirmek için Azure Kaynak Yöneticisi şablonunuza değişkenler ekleyin.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 7fa6c36b604d0e96b2192ceb6c5585afcade080b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371723"
---
# <a name="tutorial-add-variables-to-your-arm-template"></a>Öğretici: ARM şablonunuza değişken ekleme

Bu eğitimde, Azure Kaynak Yöneticisi (ARM) şablonunuza nasıl bir değişken ekleyeceğinizi öğrenirsiniz. Değişkenler, bir ifadeyi bir kez yazmanızı ve şablon boyunca yeniden kullanmanızı sağlayarak şablonlarınızı basitleştirir. Bu eğitimin tamamlanması **7 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

[İşlevler hakkında öğreticiyi](template-tutorial-add-functions.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticinin sonunda, şablonunuzun aşağıdaki JSON'u vardı:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-location/azuredeploy.json":::

Benzersiz bir ad vermeniz gerekdığından, depolama hesabı adı için parametre kullanımı zordur. Bu serinin önceki öğreticilerini tamamladıysanız, büyük olasılıkla benzersiz bir adı tahmin etmekten yoruldunuz. Depolama hesabı için benzersiz bir ad oluşturan bir değişken ekleyerek bu sorunu çözersiniz.

## <a name="use-variable"></a>Değişkeni kullan

Aşağıdaki örnek, şablonunuza benzersiz bir depolama hesabı adı oluşturan bir değişken eklemek için yapılan değişiklikleri vurgular. Dosyanın tamamını kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-variable/azuredeploy.json" range="1-47" highlight="5-9,29-31,36":::

**UniqueStorageName**adlı bir değişken içerdiğine dikkat edin. Bu değişken, bir dize değeri oluşturmak için dört işlev kullanır.

[Parametrelerin](template-functions-deployment.md#parameters) işlevini zaten biliyorsun, bu yüzden incelemeyeyeceğiz.

[ResourceGroup](template-functions-resource.md#resourcegroup) işlevini de biliyorsundur. Bu durumda, önceki öğreticide gösterildiği gibi **konum** özelliği yerine **kimlik** özelliğini alırsınız. **Kimlik** özelliği, abonelik kimliği ve kaynak grubu adı da dahil olmak üzere kaynak grubunun tam tanımlayıcısını döndürür.

[uniqueString](template-functions-string.md#uniquestring) işlevi 13 karakterlik karma değer oluşturur. Döndürülen değer, geçtiğiniz parametrelere göre belirlenir. Bu öğretici için, karma değer ingirişi olarak kaynak grubu kimliğini kullanırsınız. Bu, bu şablonu farklı kaynak gruplarına dağıtabileceğiniz ve farklı bir benzersiz dize değeri elde edebilirsiniz anlamına gelir. Ancak, aynı kaynak grubuna dağıtırsanız aynı değeri alırsınız.

[Concat](template-functions-string.md#concat) işlevi değerleri alır ve bunları birleştirir. Bu değişken için, string parametre ve string uniqueString işlevinden alır ve bunları tek bir dize halinde birleştirir.

**StoragePrefix** parametresi, depolama hesaplarını belirlemenize yardımcı olan bir önek içinde geçmenizi sağlar. Uzun bir kaynak listesinden dağıtımdan sonra depolama hesaplarını belirlemeyi kolaylaştıran kendi adlandırma kuralınızı oluşturabilirsiniz.

Son olarak, depolama adının artık parametre yerine değişkene ayarlı olduğuna dikkat edin.

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtalım. Depolama adı için sadece öneki sağladığınız için bu şablonu dağıtmak önceki şablonlardan daha kolaydır.

Kaynak grubunu oluşturmadıysanız, [bkz.](template-tutorial-create-first-template.md#create-resource-group) Örnek, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi **şablonDosya** değişkenini şablon dosyasına giden yola ayarladığınız varsayar.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addnamevariable `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
az deployment group create \
  --name addnamevariable \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Azure portalındaki kaynak grubunu keşfederek dağıtımı doğrulayabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol menüden **Kaynak gruplarını**seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Bir depolama hesabı kaynağının dağıtıldığını görüyorsunuz. Depolama hesabının adı **depo** artı rasgele karakterler dizisidir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silerek dağıttığınız kaynakları temizlemek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, depolama hesabı için benzersiz bir ad oluşturan bir değişken eklediniz. Sonraki öğreticide, dağıtılan depolama hesabından bir değer döndürebilirsiniz.

> [!div class="nextstepaction"]
> [Çıktı ekleme](template-tutorial-add-outputs.md)
