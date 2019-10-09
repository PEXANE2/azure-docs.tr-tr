---
title: Öğretici-Azure portal Azure Resource Manager şablonu dışarı aktarma
description: Şablon geliştirmeyi tamamlamaya yönelik olarak, aktarılmış bir şablonu nasıl kullanacağınızı öğrenin.
services: azure-resource-manager
author: mumian
manager: carmonmills
ms.service: azure-resource-manager
ms.date: 10/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 0564763b05dd44eb908ec00f98a97e9d4970099f
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177577"
---
# <a name="tutorial-use-exported-template-from-the-azure-portal"></a>Öğretici: Azure portal dışarıya aktarılmış şablon kullanma

Bu öğretici serisinde, Azure depolama hesabı dağıtmak için bir şablon oluşturdunuz. Sonraki iki öğreticilerde, bir *App Service planı* ve bir *Web sitesi*eklersiniz. Sıfırdan şablon oluşturmak yerine, Azure portal şablonları dışarı aktarmayı ve [Azure hızlı başlangıç şablonlarından](/resources/templates/)örnek şablonları kullanmayı öğreneceksiniz. Bu şablonları kullanım için özelleştirirsiniz. Bu öğretici, şablonları dışarı aktarmaya ve şablonunuzun sonucunu özelleştirmeye odaklanır. Yaklaşık **14 dakika** sürer.

## <a name="prerequisites"></a>Önkoşullar

[Çıktılar hakkında öğreticiyi](template-tutorial-add-outputs.md)tamamlamanızı öneririz, ancak bu gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell ya da Azure CLı ile Visual Studio Code olması gerekir. Daha fazla bilgi için bkz. [şablon araçları](template-tutorial-create-first-template.md#get-tools).

## <a name="review-your-template"></a>Şablonunuzu gözden geçirin

Önceki öğreticinin sonunda, şablonunuz aşağıdaki JSON 'a sahipti:

[!code-json[](~/resourcemanager-templates/get-started-with-templates/add-outputs/azuredeploy.json)]

Bu şablon, depolama hesaplarının dağıtımı için iyi bir sonuç verir, ancak buna daha fazla kaynak eklemek isteyebilirsiniz. Var olan bir kaynaktan bir şablonu dışarı aktarmak için bu kaynağın JSON 'sini hızlıca alabilirsiniz.

## <a name="create-app-service-plan"></a>App Service planı oluştur

1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
1. **Kaynak oluştur**' u seçin.
1. **Markette ara**' te **App Service planı**girin ve **App Service planı**' nı seçin.  **App Service planı seçme (klasik)**
1. **Oluştur**’u seçin.
1. Girmesini

    - **Abonelik**: Azure aboneliğinizi seçin.
    - **Kaynak grubu**: **Yeni oluştur** ' u seçin ve ardından bir ad belirtin. Bu öğretici serisinde kullandığınızdan farklı bir kaynak grubu adı belirtin.
    - **Ad**: App Service planı için bir ad girin.
    - **Işletim sistemi**: **Linux**' u seçin.
    - **Bölge**: bir Azure konumu seçin. Örneğin, **Orta ABD**.
    - **Fiyatlandırma katmanı**: maliyetleri kaydetmek için SKU 'Yu **temel B1** (geliştirme/test altında) olarak değiştirin.

    ![Kaynak Yöneticisi şablonu dışarı aktarma şablonu portalı](./media/template-tutorial-export-template/resource-manager-template-export.png)
1. **Gözden geçir ve Oluştur '** u seçin.
1. **Oluştur**’u seçin. Kaynağı oluşturmak birkaç dakika sürer.

## <a name="export-the-template"></a>Şablonu dışarı aktarma

1. **Kaynağa Git**' i seçin.

    ![Kaynağa git](./media/template-tutorial-export-template/resource-manager-template-export-go-to-resource.png)

1. **Şablonu dışarı aktar**' ı seçin.

    ![Kaynak Yöneticisi şablonu dışarı aktarma şablonu](./media/template-tutorial-export-template/resource-manager-template-export-template.png)

   Şablonu dışarı aktar özelliği, bir kaynağın geçerli durumunu alır ve bunu dağıtmak için bir şablon oluşturur. Bir şablonu dışarı aktarmak, bir kaynağı dağıtmanız için ihtiyacınız olan JSON 'ı hızlı bir şekilde almanın yararlı bir yolu olabilir.

1. **Microsoft. Web/sunucugrupları** tanımını ve parametre tanımını şablonunuza kopyalayın.

    ![Kaynak Yöneticisi şablonu dışarı aktarma şablonu dışarı aktarma şablonu](./media/template-tutorial-export-template/resource-manager-template-exported-template.png)

> [!IMPORTANT]
> Genellikle, içe aktarılmış şablon, bir şablon oluştururken kullanmak isteyebileceğiniz daha ayrıntılıdır. Örneğin, verdiğiniz şablondaki SKU nesnesinin beş özelliği vardır. Bu şablon işe yarar, ancak yalnızca **Name** özelliğini kullanabilirsiniz. Verdiğiniz şablonla başlayabilir ve sonra gereksinimlerinize uyacak şekilde değiştirebilirsiniz.

## <a name="revise-the-existing-template"></a>Mevcut şablonu gözden geçir

İçe aktarılmış şablon size ihtiyacınız olan JSON 'ın çoğunu verir, ancak şablonunuz için özelleştirmeniz gerekir. Şablonlarınız ve aktarılmış şablon arasındaki parametrelerde ve değişkenlerde farklara dikkat edin. Kuşkusuz, dışarı aktarma işlemi şablonunuzda zaten tanımladığınız parametreleri ve değişkenleri bilmez.

Aşağıdaki örnek, şablonlarınızın eklemelerini vurgular. Bu, içe aktarılmış kodu ve bazı değişiklikleri içerir. İlk olarak, parametre adını adlandırma kuralınızın eşleşmesi için değiştirir. İkincisi, App Service planının konumu için konum parametresini kullanır. Üçüncü olarak, bu değer kaynak düzeyinde **Name** özelliği ile Artıklı olduğundan, **Özellikler** nesnesinin içindeki **adı** kaldırır.

Tüm dosyayı kopyalayın ve şablonunuzu içeriğiyle değiştirin.

[!code-json[](~/resourcemanager-templates/get-started-with-templates/export-template/azuredeploy.json?range=1-77&highlight=28-31,50-69)]

## <a name="deploy-the-template"></a>Şablonu dağıtma

Bir şablonu dağıtmak için Azure CLı veya Azure PowerShell kullanın.

Kaynak grubunu oluşturmadıysanız, bkz. [kaynak grubu oluşturma](template-tutorial-create-first-template.md#create-resource-group). Örnek, **TemplateFile** değişkenini, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi şablon dosyası yolu olarak ayarlamış olduğunuzu varsayar.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addappserviceplan `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-clitabazure-cli"></a>[Azure CLı](#tab/azure-cli)

```azurecli
az group deployment create \
  --name addappserviceplan \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

## <a name="verify-deployment"></a>Dağıtımı doğrula

Kaynak grubunu Azure portal inceleyerek dağıtımı doğrulayabilirsiniz.

1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
1. Sol menüden **kaynak grupları**' nı seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Kaynak grubu, bir depolama hesabı ve bir App Service planı içerir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiş yapıyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsa, kaynak grubunu silerek dağıttığınız kaynakları temizlemeniz gerekebilir.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure portal bir şablonu nasıl dışarı aktarıp şablon geliştirmede dışarı aktarılmış şablonu nasıl kullanacağınızı öğrendiniz. Şablon geliştirmeyi basitleştirmek için Azure hızlı başlangıç şablonlarını da kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Azure hızlı başlangıç şablonlarını kullanma](template-tutorial-quickstart-template.md)
