---
title: Öğretici - Hızlı başlatma şablonlarını kullanma
description: Şablon geliştirmenizi tamamlamak için Azure Quickstart şablonlarını nasıl kullanacağınızı öğrenin.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 4b82e02ecc009e587b89d1fd151fd13f75a4bcf8
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80408501"
---
# <a name="tutorial-use-azure-quickstart-templates"></a>Öğretici: Azure Quickstart şablonlarını kullanma

[Azure Quickstart şablonları,](https://azure.microsoft.com/resources/templates/) topluluk tarafından katkıda bulunulan şablonların bir deposudur. Şablon geliştirmenizde örnek şablonları kullanabilirsiniz. Bu öğreticide, bir web sitesi kaynak tanımı bulur ve kendi şablonunuza eklersiniz. Tamamlanması yaklaşık **12 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

[Dışa aktarılan şablonlarla ilgili öğreticiyi](template-tutorial-export-template.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticinin sonunda, şablonunuzun aşağıdaki JSON'u vardı:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/export-template/azuredeploy.json":::

Bu şablon depolama hesaplarını ve uygulama hizmeti planlarını dağıtmak için çalışır, ancak buna bir web sitesi eklemek isteyebilirsiniz. Bir kaynağı dağıtmak için gereken JSON'u hızla keşfetmek için önceden oluşturulmuş şablonları kullanabilirsiniz.

## <a name="find-template"></a>Şablonu bulma

1. [Azure Quickstart şablonlarını](https://azure.microsoft.com/resources/templates/) açma
1. **Arama'da**, **linux web uygulamasını dağıt'a**girin.
1. Başlığı olan birini seçin **Temel bir Linux web uygulaması dağıtın.** Bulmakta sorun yaşıyorsanız, işte [doğrudan bağlantı.](https://azure.microsoft.com/resources/templates/101-webapp-basic-linux/)
1. **GitHub'da Gözat'ı**seçin.
1. **azuredeploy.json'u**seçin.
1. Şablonu gözden geçirin. Özellikle, `Microsoft.Web/sites` kaynak arayın.

    ![Kaynak Yöneticisi şablonu quickstart web sitesi](./media/template-tutorial-quickstart-template/resource-manager-template-quickstart-template-web-site.png)

## <a name="revise-existing-template"></a>Varolan şablonu gözden geçirin

Hızlı başlangıç şablonu ile varolan şablonu birleştirin:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/quickstart-template/azuredeploy.json" range="1-108" highlight="32-45,49,85-100":::

Web uygulaması adının Azure genelinde benzersiz olması gerekir. Yinelenen adlara sahip olmayı önlemek için **webAppPortalName** değişkeni **"webAppPortalName"den güncellendi: "[concat(parameters('webAppName'), '-webapp')]"** ile **"webAppPortalName": "[concat(parameters('webAppName'), uniqueString(resourceGroup().id))]"**

Kaynak tanımını `Microsoft.Web/serverfarms` `Microsoft.Web/sites` tanımdan ayırmak için tanımın sonuna virgül ekleyin.

Bu yeni kaynakta dikkat edilmesi gereken birkaç önemli özellik vardır.

Uygulama hizmet planına ayarlanmış **depends depends** adlı bir öğesi olduğunu fark edeceksiniz. Bu ayar gereklidir, çünkü uygulama hizmet planı web uygulaması oluşturulmadan önce var olmalıdır. **DependsOn** öğesi Kaynak Yöneticisi'ne dağıtım için kaynakları nasıl sipariş edinir söyler.

**serverFarmId** özelliği [resourceId](template-functions-resource.md#resourceid) işlevini kullanır. Bu işlev, bir kaynak için benzersiz tanımlayıcıyı alır. Bu durumda, uygulama hizmet planı için benzersiz tanımlayıcı alır. Web uygulaması belirli bir uygulama hizmet planı ile ilişkilidir.

## <a name="deploy-template"></a>Şablon dağıtma

Bir şablon dağıtmak için Azure CLI veya Azure PowerShell'i kullanın.

Kaynak grubunu oluşturmadıysanız, [bkz.](template-tutorial-create-first-template.md#create-resource-group) Örnek, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi **şablonDosya** değişkenini şablon dosyasına giden yola ayarladığınız varsayar.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addwebapp `
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
  --name addwebapp \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS webAppName=demoapp
```

---

> [!NOTE]
> Dağıtım başarısız olduysa, hata ayıklama günlüklerini göstermek için dağıtım komutuyla **hata ayıklama** anahtarını kullanın.  Tam hata ayıklama günlüklerini göstermek için **ayrıntılı** anahtar da kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silerek dağıttığınız kaynakları temizlemek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Şablon geliştirmeiçin hızlı başlangıç şablonunu kullanmayı öğrendiniz. Bir sonraki öğreticide, kaynaklara etiketler eklersiniz.

> [!div class="nextstepaction"]
> [Etiket ekleme](template-tutorial-add-tags.md)
