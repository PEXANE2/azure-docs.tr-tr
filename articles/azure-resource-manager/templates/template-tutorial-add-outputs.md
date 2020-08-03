---
title: Öğretici-şablona çıktılar ekleme
description: Sözdizimini basitleştirmek için Azure Resource Manager şablonunuza çıktılar ekleyin.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4c90f1ce4bc3f3f5ed91bee86bb1f6f9093876c7
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87497792"
---
# <a name="tutorial-add-outputs-to-your-arm-template"></a>Öğretici: ARM şablonunuza çıktılar ekleme

Bu öğreticide, Azure Resource Manager (ARM) şablonınızdan bir değer döndürmeyi öğrenirsiniz. Dağıtılan bir kaynaktan bir değere ihtiyacınız olduğunda çıktıları kullanırsınız. Bu öğreticinin tamamlana **7 dakika** sürer.

## <a name="prerequisites"></a>Önkoşullar

[Değişkenler hakkında öğreticiyi](template-tutorial-add-variables.md)tamamlamanızı öneririz, ancak bu gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell ya da Azure CLı ile Visual Studio Code olması gerekir. Daha fazla bilgi için bkz. [şablon araçları](template-tutorial-create-first-template.md#get-tools).

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticinin sonunda, şablonunuz aşağıdaki JSON 'a sahipti:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-variable/azuredeploy.json":::

Bir depolama hesabı dağıtır, ancak depolama hesabı hakkında herhangi bir bilgi döndürmez. Daha sonra başvuru için kullanılabilir olmaları için yeni bir kaynaktaki özellikleri yakalamanız gerekebilir.

## <a name="add-outputs"></a>Çıkış Ekle

Şablondan değer döndürmek için çıktıları kullanabilirsiniz. Örneğin, yeni depolama hesabınız için uç noktaları almak faydalı olabilir.

Aşağıdaki örnek, bir çıkış değeri eklemek için şablonunuzda yapılan değişikliği vurgular. Tüm dosyayı kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-outputs/azuredeploy.json" range="1-53" highlight="47-52":::

Eklediğiniz çıktı değeri hakkında dikkat etmeniz için bazı önemli öğeler vardır.

Döndürülen değerin türü **Object**olarak ayarlanır, yanı bir JSON nesnesi döndürür.

Depolama hesabının çalışma zamanı durumunu almak için [başvuru](template-functions-resource.md#reference) işlevini kullanır. Bir kaynağın çalışma zamanı durumunu almak için bir kaynağın adını veya KIMLIĞINI geçirin. Bu durumda, depolama hesabının adını oluşturmak için kullandığınız değişkeni kullanırsınız.

Son olarak, depolama hesabından en son eden **yenyenler** özelliğini döndürür

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtmaya ve döndürülen değere bakmaya hazırsınız.

Kaynak grubunu oluşturmadıysanız, bkz. [kaynak grubu oluşturma](template-tutorial-create-first-template.md#create-resource-group). Örnek, **TemplateFile** değişkenini, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi şablon dosyası yolu olarak ayarlamış olduğunuzu varsayar.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addoutputs `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile `
  -storagePrefix "store" `
  -storageSKU Standard_LRS
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI’nın [en son sürümüne](/cli/azure/install-azure-cli) sahip olmanız gerekir.

```azurecli
az deployment group create \
  --name addoutputs \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

Dağıtım komutunun çıktısında, aşağıdaki örneğe benzer bir nesne görürsünüz ve yalnızca çıkış JSON biçimindedir:

```json
{
    "dfs": "https://storeluktbfkpjjrkm.dfs.core.windows.net/",
    "web": "https://storeluktbfkpjjrkm.z19.web.core.windows.net/",
    "blob": "https://storeluktbfkpjjrkm.blob.core.windows.net/",
    "queue": "https://storeluktbfkpjjrkm.queue.core.windows.net/",
    "table": "https://storeluktbfkpjjrkm.table.core.windows.net/",
    "file": "https://storeluktbfkpjjrkm.file.core.windows.net/"
}
```

> [!NOTE]
> Dağıtım başarısız olursa, hata ayıklama günlüklerini göstermek için dağıtım komutuyla **hata ayıklama** anahtarını kullanın.  **Ayrıntılı** anahtarı, tam hata ayıklama günlüklerini göstermek için de kullanabilirsiniz.

## <a name="review-your-work"></a>Çalışmanızı gözden geçirin

Son altı öğreticilerde bir çok şey yaptınız. Ne yaptığını gözden geçirmeniz biraz zaman atalım. Kolayca sağlanması gereken parametrelere sahip bir şablon oluşturdunuz. Şablon, özelleştirmeye izin verdiğinden ve gerekli değerleri dinamik olarak oluşturduğundan farklı ortamlarda yeniden kullanılabilir. Ayrıca, betiğinizdeki kullanabileceğiniz depolama hesabı hakkındaki bilgileri de döndürür.

Şimdi, kaynak grubuna ve dağıtım geçmişine bakalım.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol menüden **kaynak grupları**' nı seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Yaptığınız adımlara bağlı olarak, kaynak grubunda en az bir ve belki birkaç depolama hesabınız olmalıdır.
1. Ayrıca, geçmişte listelenen birkaç başarılı dağıtıma da sahip olmanız gerekir. Bağlantıyı seçin.

   ![Dağıtımları seçin](./media/template-tutorial-add-outputs/select-deployments.png)

1. Tüm dağıtımlarınızın geçmişini görürsünüz. **Addoutputs**adlı dağıtımı seçin.

   ![Dağıtım geçmişini göster](./media/template-tutorial-add-outputs/show-history.png)

1. Girişleri gözden geçirebilirsiniz.

   ![Girişleri göster](./media/template-tutorial-add-outputs/show-inputs.png)

1. Çıkışları gözden geçirebilirsiniz.

   ![Çıkışları göster](./media/template-tutorial-add-outputs/show-outputs.png)

1. Şablonu gözden geçirebilirsiniz.

   ![Şablonu göster](./media/template-tutorial-add-outputs/show-template.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiş yapıyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsa, kaynak grubunu silerek dağıttığınız kaynakları temizlemeniz gerekebilir.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şablona bir dönüş değeri eklediniz. Sonraki öğreticide, bir şablonu dışarı aktarmayı ve şablonunuzda bu dışarı aktarılmış şablonun parçalarını kullanmayı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Aktarılmış şablonu kullan](template-tutorial-export-template.md)
