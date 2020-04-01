---
title: Öğretici - şablon işlevleri ekle
description: Değerleri oluşturmak için Azure Kaynak Yöneticisi şablonunuza şablon işlevleri ekleyin.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: e4984b286bf031b66272919a487d09a90f972ce0
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410970"
---
# <a name="tutorial-add-template-functions-to-your-arm-template"></a>Öğretici: ARM şablonunuza şablon işlevleri ekleme

Bu eğitimde, Azure Kaynak Yöneticisi (ARM) şablonunuza [şablon işlevlerini](template-functions.md) nasıl ekleyeceğinizi öğrenirsiniz. Değerleri dinamik olarak oluşturmak için işlevleri kullanırsınız. Bu sistem tarafından sağlanan şablon işlevlerine ek olarak, [kullanıcı tanımlı işlevler](./template-user-defined-functions.md)de oluşturabilirsiniz. Bu eğitimin tamamlanması **7 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

Parametreler le ilgili [eğitimi](template-tutorial-add-parameters.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticinin sonunda, şablonunuzun aşağıdaki JSON'u vardı:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-sku/azuredeploy.json":::

Depolama hesabının konumu **Doğu ABD**için sabit kodlanmış. Ancak, depolama hesabını diğer bölgelere dağıtmanız gerekebilir. Şablonunuzun esneklikten yoksun bir sorunuyla yine karşı karşıyasınız. Konum için bir parametre ekleyebilirsiniz, ancak varsayılan değeri sabit kodlanmış bir değerden daha anlamlı ysa harika olur.

## <a name="use-function"></a>İşlev kullanma

Bu serinin önceki öğreticisini tamamladıysanız, zaten bir işlev kullandınız. **"[parameters('storageName')]"** [eklediğinizde, parametreler](template-functions-deployment.md#parameters) işlevini kullandınız. Köşeli ayraçlar, parantez içindeki sözdiziminin bir [şablon ifadesi](template-expressions.md)olduğunu gösterir. Kaynak Yöneticisi, sözdizimini gerçek bir değer olarak ele almaktansa çözer.

Işlevler, dağıtım sırasında dinamik olarak değer alarak şablonunuza esneklik katar. Bu öğreticide, dağıtım için kullandığınız kaynak grubunun konumunu almak için bir işlev kullanırsınız.

Aşağıdaki **örnekte, konum**adı verilen bir parametre eklemek için yapılan değişiklikler vurgulanır.  Parametre varsayılan değeri [kaynak Grubu](template-functions-resource.md#resourcegroup) işlevini çağırır. Bu işlev, dağıtım için kullanılan kaynak grubu hakkında bilgi içeren bir nesne döndürür. Nesneüzerindeki özelliklerden biri konum özelliğidir. Varsayılan değeri kullandığınızda, depolama hesabı konumu kaynak grubuyla aynı konuma sahiptir. Kaynak grubundaki kaynakların aynı konumu paylaşması gerekemez. Gerektiğinde farklı bir konum da sağlayabilirsiniz.

Dosyanın tamamını kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-location/azuredeploy.json" range="1-44" highlight="24-27,34":::

## <a name="deploy-template"></a>Şablon dağıtma

Önceki öğreticilerde, Doğu ABD'de bir depolama hesabı oluşturdunuz, ancak kaynak grubunuz Orta ABD'de oluşturuldu. Bu öğretici için depolama hesabınız kaynak grubuyla aynı bölgede oluşturulur. Konum için varsayılan değeri kullanın, böylece bu parametre değerini sağlamanız gerekmez. Farklı bir konumda bir depolama hesabı oluşturduğunuz için depolama hesabı için yeni bir ad sağlamanız gerekir. Örneğin, **store2** yerine önek olarak **store2 kullanın1.**

Kaynak grubunu oluşturmadıysanız, [bkz.](template-tutorial-create-first-template.md#create-resource-group) Örnek, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi **şablonDosya** değişkenini şablon dosyasına giden yola ayarladığınız varsayar.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addlocationparameter `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storageName "{new-unique-name}"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
az deployment group create \
  --name addlocationparameter \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storageName={new-unique-name}
```

---

> [!NOTE]
> Dağıtım başarısız olduysa, hata ayıklama günlüklerini göstermek için dağıtım komutuyla **hata ayıklama** anahtarını kullanın.  Tam hata ayıklama günlüklerini göstermek için **ayrıntılı** anahtar da kullanabilirsiniz.

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Azure portalındaki kaynak grubunu keşfederek dağıtımı doğrulayabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol menüden **Kaynak gruplarını**seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Bir depolama hesabı kaynağının dağıtıldığını ve kaynak grubuyla aynı konuma sahip olduğunu görürsünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silerek dağıttığınız kaynakları temizlemek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir parametre için varsayılan değeri tanımlarken bir işlev kullandınız. Bu öğretici seride, işlevleri kullanmaya devam eceksiniz. Serinin sonunda, şablonun her bölümüne işlevler eklersiniz.

> [!div class="nextstepaction"]
> [Değişkenleri ekleme](template-tutorial-add-variables.md)
