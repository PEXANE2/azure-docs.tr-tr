---
title: Öğretici - şablondaki kaynaklara etiket ekleme
description: Azure Kaynak Yöneticisi şablonunuzda dağıttığınız kaynaklara etiketler ekleyin. Etiketler, kaynakları mantıksal olarak düzenlemenize izin sağlar.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 748a32d0ea8bfb0f23a99ce99d0aaf051118bc19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369888"
---
# <a name="tutorial-add-tags-in-your-arm-template"></a>Öğretici: ARM şablonunuzda etiket ekleme

Bu eğitimde, Azure Kaynak Yöneticisi (ARM) şablonunuzda kaynaklara nasıl etiket ekleyeceğinizi öğrenirsiniz. [Etiketler](../management/tag-resources.md) kaynaklarınızı mantıksal olarak düzenlemenize yardımcı olur. Etiket değerleri maliyet raporlarında gösteriş. Bu eğitimin tamamlanması **8 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

[Quickstart şablonları hakkındaki eğitimi](template-tutorial-quickstart-template.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki şablonunuzda bir depolama hesabı, Uygulama Hizmeti planı ve web uygulaması dağıtıldı.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json":::

Bu kaynakları dağıttıktan sonra, maliyetleri izlemeniz ve bir kategoriye ait kaynakları bulmanız gerekebilir. Bu sorunları çözmeye yardımcı olmak için etiketler ekleyebilirsiniz.

## <a name="add-tags"></a>Etiket ekleme

Kaynakları, bunların kullanımını belirlemenize yardımcı olacak değerler eklemek için etiketlersiniz. Örneğin, ortamı ve projeyi listeleyen etiketler ekleyebilirsiniz. Maliyet merkezini veya kaynağın sahibi olan ekibi tanımlayan etiketler ekleyebilirsiniz. Kuruluşunuz için anlamlı olan tüm değerleri ekleyin.

Aşağıdaki örnekte şablondaki değişiklikler vurgulanır. Dosyanın tamamını kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-tags/azuredeploy.json" range="1-118" highlight="46-52,64,78,102":::

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtma nın ve sonuçlara bakmanın zamanı.

Kaynak grubunu oluşturmadıysanız, [bkz.](template-tutorial-create-first-template.md#create-resource-group) Örnek, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi **şablonDosya** değişkenini şablon dosyasına giden yola ayarladığınız varsayar.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addtags `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS `
  -webAppName demoapp
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
az deployment group create \
  --name addtags \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Azure portalındaki kaynak grubunu keşfederek dağıtımı doğrulayabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol menüden **Kaynak gruplarını**seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Depolama hesabı kaynağı gibi kaynaklardan birini seçin. Artık etiketleri olduğunu görüyorsunuz.

   ![Etiketleri göster](./media/template-tutorial-add-tags/show-tags.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silerek dağıttığınız kaynakları temizlemek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, kaynaklara etiketler eklediniz. Bir sonraki öğreticide, şablona geçen değerleri basitleştirmek için parametre dosyalarını nasıl kullanacağınızı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Parametre dosyanı kullanma](template-tutorial-use-parameter-file.md)
