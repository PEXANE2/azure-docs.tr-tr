---
title: Öğretici-şablona değişken ekleme
description: Sözdizimini basitleştirmek için Azure Resource Manager şablonunuza değişken ekleyin.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: 398b2741d7bafe4c3fdd9520d00eb7fcb766b876
ms.sourcegitcommit: 1fe5127fb5c3f43761f479078251242ae5688386
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90069620"
---
# <a name="tutorial-add-variables-to-your-arm-template"></a>Öğretici: ARM şablonunuza değişkenler ekleme

Bu öğreticide, Azure Resource Manager (ARM) şablonunuza nasıl değişken ekleneceğini öğreneceksiniz. Değişkenler, bir ifadeyi bir kez yazmanızı ve şablon boyunca yeniden kullanılmasını sağlayarak şablonlarınızı basitleştirir. Bu öğreticinin tamamlana **7 dakika** sürer.

## <a name="prerequisites"></a>Önkoşullar

[İşlevlerle ilgili öğreticiyi](template-tutorial-add-functions.md)tamamlamanızı öneririz, ancak bu gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell ya da Azure CLı ile Visual Studio Code olması gerekir. Daha fazla bilgi için bkz. [şablon araçları](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticinin sonunda, şablonunuz aşağıdaki JSON 'a sahipti:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-location/azuredeploy.json":::

Benzersiz bir ad sağlamanız gerektiğinden, depolama hesabı adı için parametre kullanımı zor. Bu serideki önceki öğreticileri tamamladıysanız, büyük olasılıkla benzersiz bir ad tahmin etmeniz çok yorsunuzdur. Depolama hesabı için benzersiz bir ad oluşturan bir değişken ekleyerek bu sorunu çözebilirsiniz.

## <a name="use-variable"></a>Değişken kullan

Aşağıdaki örnek, şablonunuza benzersiz bir depolama hesabı adı oluşturan bir değişken ekleme değişikliklerini vurgular. Tüm dosyayı kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-variable/azuredeploy.json" range="1-47" highlight="5-9,29-31,36":::

**Unisorgtoraygename**adlı bir değişken içerdiğine dikkat edin. Bu değişken bir dize değeri oluşturmak için dört işlevi kullanır.

[Parameters](template-functions-deployment.md#parameters) işlevini zaten öğreniyoruz, bu yüzden bunu inceleyeceğiz.

Ayrıca, [resourceGroup](template-functions-resource.md#resourcegroup) işlevi hakkında bilgi sahibisiniz. Bu durumda, önceki öğreticide gösterildiği gibi **Location** özelliği yerine **ID** özelliğini alırsınız. **ID** özelliği, abonelik kimliği ve kaynak grubu adı dahil olmak üzere kaynak grubunun tam tanımlayıcısını döndürür.

[Uniquestring](template-functions-string.md#uniquestring) işlevi bir 13 karakterlik karma değeri oluşturur. Döndürülen değer geçirdiğiniz parametrelere göre belirlenir. Bu öğreticide, karma değer girişi olarak kaynak grubu KIMLIĞINI kullanırsınız. Bu, bu şablonu farklı kaynak gruplarına dağıtabileceğiniz ve farklı bir benzersiz dize değeri alabileceğiniz anlamına gelir. Ancak, aynı kaynak grubuna dağıtırsanız aynı değeri alırsınız.

[Concat](template-functions-string.md#concat) işlevi değerleri alır ve bunları birleştirir. Bu değişken için, dizeden ve uniqueString işlevindeki dizeden dize alır ve bunları tek bir dize olarak birleştirir.

**Storageprefix** parametresi, depolama hesaplarını belirlemenize yardımcı olan bir ön ek geçirmenize olanak sağlar. Depolama hesaplarının uzun bir kaynak listesinden dağıtımdan sonra tanımlanmasını kolaylaştıran kendi adlandırma kuralınızın oluşturulmasını sağlayabilirsiniz.

Son olarak, depolama adının bir parametre yerine artık değişkene ayarlı olduğuna dikkat edin.

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtalım. Yalnızca depolama adı için ön ek sağlamanız gerektiğinden, bu şablonu dağıtmak önceki şablonlardan daha kolaydır.

Kaynak grubunu oluşturmadıysanız, bkz. [kaynak grubu oluşturma](template-tutorial-create-first-template.md#create-resource-group). Örnek, **TemplateFile** değişkenini, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi şablon dosyası yolu olarak ayarlamış olduğunuzu varsayar.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addnamevariable `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI’nın [en son sürümüne](/cli/azure/install-azure-cli) sahip olmanız gerekir.

```azurecli
az deployment group create \
  --name addnamevariable \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

> [!NOTE]
> Dağıtım başarısız olursa, oluşturulmakta olan kaynaklarla ilgili bilgi almak için **verbose** anahtarını kullanın. Hata ayıklama hakkında daha fazla bilgi edinmek için **hata ayıklama** anahtarını kullanın.

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Kaynak grubunu Azure portal inceleyerek dağıtımı doğrulayabilirsiniz.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol menüden **kaynak grupları**' nı seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Bir depolama hesabı kaynağının dağıtıldığını görürsünüz. Depolama hesabının adı, **Depo** ve rastgele karakterlerden oluşan bir dizedir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiş yapıyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsa, kaynak grubunu silerek dağıttığınız kaynakları temizlemeniz gerekebilir.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, depolama hesabı için benzersiz bir ad oluşturan bir değişken eklediniz. Sonraki öğreticide, dağıtılan depolama hesabından bir değer döndürürler.

> [!div class="nextstepaction"]
> [Çıkış Ekle](template-tutorial-add-outputs.md)
