---
title: Öğretici-şablondaki kaynaklara etiketler ekleme
description: Azure Resource Manager şablonunuzda dağıttığınız kaynaklara Etiketler ekleyin. Etiketler, kaynakları mantıksal olarak düzenlemenizi sağlar.
author: mumian
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d7853d388b9159328a515818697ba9ddd5b95637
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76773188"
---
# <a name="tutorial-add-tags-in-your-resource-manager-template"></a>Öğretici: Kaynak Yöneticisi şablonunuzda etiketler ekleme

Bu öğreticide, şablonunuzda kaynaklara nasıl etiket ekleneceğini öğreneceksiniz. [Etiketler](../management/tag-resources.md) , kaynaklarınızı mantıksal olarak düzenlemenize yardımcı olur. Etiket değerleri, maliyet raporlarında gösterilir. Bu öğreticinin tamamlandığı **8 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

[Hızlı başlangıç şablonları hakkında öğreticiyi](template-tutorial-quickstart-template.md)tamamlamanızı öneririz, ancak bu gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell ya da Azure CLı ile Visual Studio Code olması gerekir. Daha fazla bilgi için bkz. [şablon araçları](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Şablonu gözden geçir

Önceki şablonunuz bir depolama hesabı, App Service planı ve Web uygulaması dağıttı.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json":::

Bu kaynakları dağıttıktan sonra, maliyetleri izlemeniz ve bir kategoriye ait kaynakları bulmanız gerekebilir. Bu sorunları çözmeye yardımcı olmak için Etiketler ekleyebilirsiniz.

## <a name="add-tags"></a>Etiket ekleme

Kullanımları tanımlamanızı sağlayacak değerler eklemek için kaynakları etiketlerseniz. Örneğin, ortamı ve projeyi listelemek için Etiketler ekleyebilirsiniz. Bir maliyet merkezini veya kaynağa sahip olan takımı tanımlayan etiketler ekleyebilirsiniz. Kuruluşunuz için anlamlı olan tüm değerleri ekleyin.

Aşağıdaki örnek, şablonda yapılan değişiklikleri vurgular. Tüm dosyayı kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json" range="1-118" highlight="46-52,64,78,102":::

## <a name="deploy-template"></a>Şablon dağıtma

Şablonun dağıtılması ve sonuçlara bakmaları zaman alabilir.

Kaynak grubunu oluşturmadıysanız, bkz. [kaynak grubu oluşturma](template-tutorial-create-first-template.md#create-resource-group). Örnek, **TemplateFile** değişkenini, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi şablon dosyası yolu olarak ayarlamış olduğunuzu varsayar.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addtags `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS `
  -webAppName demoapp
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addtags \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Kaynak grubunu Azure portal inceleyerek dağıtımı doğrulayabilirsiniz.

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
1. Sol menüden **kaynak grupları**' nı seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Depolama hesabı kaynağı gibi kaynaklardan birini seçin. Artık etiketlere sahip olduğunu görürsünüz.

   ![Etiketleri göster](./media/template-tutorial-add-tags/show-tags.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiş yapıyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsa, kaynak grubunu silerek dağıttığınız kaynakları temizlemeniz gerekebilir.

1. Azure portalda, sol menüden **Kaynak grubu**’nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **Kaynak grubunu sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kaynaklara Etiketler eklediniz. Sonraki öğreticide, değerleri şablona geçirmeyi kolaylaştırmak için parametre dosyalarını nasıl kullanacağınızı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Parametre dosyası kullan](template-tutorial-use-parameter-file.md)
