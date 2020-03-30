---
title: Öğretici - Azure portalından dışa aktarma şablonu
description: Şablon geliştirmenizi tamamlamak için dışa aktarılan şablonu nasıl kullanacağınızı öğrenin.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 5a0d373fdf75f19c8fc1082593c15c14770f79c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369891"
---
# <a name="tutorial-use-exported-template-from-the-azure-portal"></a>Öğretici: Azure portalından dışa aktarılan şablonu kullanma

Bu öğretici seride, bir Azure depolama hesabı dağıtmak için bir şablon oluşturdunuz. Sonraki iki öğreticide, bir *Uygulama Hizmeti planı* ve bir *web sitesi*eklersiniz. Sıfırdan şablonlar oluşturmak yerine, Azure portalından şablonları nasıl dışa aktarabileceğinizi ve [Azure Quickstart şablonlarından](https://azure.microsoft.com/resources/templates/)örnek şablonları nasıl kullanacağınızı öğrenirsiniz. Bu şablonları kullanımınız için özelleştirebilirsiniz. Bu öğretici, şablonları dışa aktarmaya ve şablonunuzun sonucunu özelleştirmeye odaklanır. Tamamlanması yaklaşık **14 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

Çıktılar la ilgili [eğitimi](template-tutorial-add-outputs.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticinin sonunda, şablonunuzun aşağıdaki JSON'u vardı:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-outputs/azuredeploy.json":::

Bu şablon depolama hesaplarını dağıtmak için iyi çalışır, ancak daha fazla kaynak eklemek isteyebilirsiniz. Bu kaynak için JSON'u hızla almak için varolan bir kaynaktan şablon dışa aktarabilirsiniz.

## <a name="create-app-service-plan"></a>App Service planı oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Kaynak oluştur**’u seçin.
1. **Marketi**Ara'da, **Uygulama Hizmeti planını**girin ve ardından App Service **planını**seçin.  Uygulama Hizmeti **planını seçmeyin (klasik)**
1. **Oluştur'u**seçin.
1. Şunları girin:

    - **Abonelik**: Azure aboneliğinizi seçin.
    - **Kaynak Grubu**: **Yeni Oluştur'u** seçin ve ardından bir ad belirtin. Bu öğretici seride kullandığınızdan farklı bir kaynak grubu adı sağlayın.
    - **Ad**: Uygulama hizmet planı için bir ad girin.
    - **İşletim Sistemi**: **Linux**seçin.
    - **Bölge**: Azure konumu seçin. Örneğin, **Orta ABD**.
    - **Fiyatlandırma katmanı**: maliyetlerden tasarruf etmek için SKU'yu **Temel B1** olarak değiştirin (Dev/Test altında).

    ![Kaynak Yöneticisi şablonu dışa aktarma şablonu portalı](./media/template-tutorial-export-template/resource-manager-template-export.png)
1. **Gözden Geçir'i**seçin ve oluşturun.
1. **Oluştur'u**seçin. Kaynağı oluşturmak birkaç dakika sürer.

## <a name="export-template"></a>Şablonu dışarı aktarma

1. **Kaynağa Git'i**seçin.

    ![Kaynağa git](./media/template-tutorial-export-template/resource-manager-template-export-go-to-resource.png)

1. **Dışa Aktar şablonu'nun**

    ![Kaynak Yöneticisi şablonu dışa aktarma şablonu](./media/template-tutorial-export-template/resource-manager-template-export-template.png)

   Dışa aktarma şablonu özelliği kaynağın geçerli durumunu alır ve onu dağıtmak için bir şablon oluşturur. Şablon dışa aktarma, bir kaynağı dağıtmak için gereken JSON'u hızla almanın yararlı bir yolu olabilir.

1. **Microsoft.Web/serverfarms** tanımını ve parametre tanımını şablonunuza kopyalayın.

    ![Kaynak Yöneticisi şablonu dışa aktarma şablonu dışa aktarma şablonu](./media/template-tutorial-export-template/resource-manager-template-exported-template.png)

> [!IMPORTANT]
> Genellikle, dışa aktarılan şablon şablon oluştururken isteyebileceğinden daha ayrıntılıdır. Örneğin, dışa aktarılan şablondaki SKU nesnesinin beş özelliği vardır. Bu şablon çalışır, ancak sadece **ad** özelliğik kullanabilirsiniz. Dışa aktarılan şablonla başlayabilir ve gereksinimlerinize uymak istediğiniz gibi değiştirebilirsiniz.

## <a name="revise-existing-template"></a>Varolan şablonu gözden geçirin

Dışa aktarılan şablon, gereksinim duyduğunuz JSON'un çoğunu verir, ancak şablonunuzun özellemi gerekir. Şablonunuz la dışa aktarılan şablon arasındaki parametreler ve değişkenler arasındaki farklılıklara özellikle dikkat edin. Açıkçası, dışa aktarma işlemi şablonunuzda tanımladığınız parametreleri ve değişkenleri bilmiyor.

Aşağıdaki örnek, şablonunuzun eklemelerini vurgular. Dışa aktarılan kodun yanı sıra bazı değişiklikleri içerir. İlk olarak, adlandırma kuralınızla eşleşecek şekilde parametrenin adını değiştirir. İkinci olarak, uygulama hizmet planının konumu için konum parametrenizi kullanır. Üçüncü olarak, bu değer kaynak düzeyinde **ad** özelliği ile gereksiz olduğundan **özellikleri** nesnesi içinde **adı** kaldırır.

Dosyanın tamamını kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/export-template/azuredeploy.json" range="1-77" highlight="28-31,50-69":::

## <a name="deploy-template"></a>Şablon dağıtma

Bir şablon dağıtmak için Azure CLI veya Azure PowerShell'i kullanın.

Kaynak grubunu oluşturmadıysanız, [bkz.](template-tutorial-create-first-template.md#create-resource-group) Örnek, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi **şablonDosya** değişkenini şablon dosyasına giden yola ayarladığınız varsayar.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addappserviceplan `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
az deployment group create \
  --name addappserviceplan \
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
1. Kaynak grubu bir depolama hesabı ve Bir Uygulama Hizmeti planı içerir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silerek dağıttığınız kaynakları temizlemek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure portalından şablon uyup nasıl dışa aktarıladığınızı ve şablon geliştirmeniz için dışa aktarılan şablonu nasıl kullanacağınızı öğrendiniz. Şablon geliştirmeyi kolaylaştırmak için Azure Quickstart şablonlarını da kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Azure Quickstart şablonlarını kullanma](template-tutorial-quickstart-template.md)
