---
title: Öğretici - şablona çıktı ekleme
description: Sözdizimini basitleştirmek için Azure Kaynak Yöneticisi şablonunuza çıktılar ekleyin.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 2ee1a2c7037bde68b7858b57a03c78bd2016ff1c
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80743557"
---
# <a name="tutorial-add-outputs-to-your-arm-template"></a>Öğretici: ARM şablonunuza çıktı ekleme

Bu eğitimde, Azure Kaynak Yöneticisi (ARM) şablonunuzdan bir değeri nasıl döndüreceklerini öğrenirsiniz. Dağıtılmış bir kaynaktan bir değere ihtiyacınız olduğunda çıktıları kullanırsınız. Bu eğitimin tamamlanması **7 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

[Değişkenler hakkındaki eğitimi](template-tutorial-add-variables.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="review-template"></a>Şablonu gözden geçir

Önceki öğreticinin sonunda, şablonunuzun aşağıdaki JSON'u vardı:

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-variable/azuredeploy.json":::

Bir depolama hesabı dağıtıyor, ancak depolama hesabı hakkında herhangi bir bilgi döndürmez. Özellikleri yeni bir kaynaktan yakalamanız gerekebilir, böylece daha sonra başvuru için kullanılabilir olurlar.

## <a name="add-outputs"></a>Çıktı ekleme

Şablondan değerleri döndürmek için çıktıları kullanabilirsiniz. Örneğin, yeni depolama hesabınız için uç noktaları almak yararlı olabilir.

Aşağıdaki örnek, çıktı değeri eklemek için şablonunuzdayapılan değişikliği vurgular. Dosyanın tamamını kopyalayın ve şablonunuzu içeriğiyle değiştirin.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-outputs/azuredeploy.json" range="1-53" highlight="47-52":::

Eklediğiniz çıktı değeri hakkında dikkat edilmesi gereken bazı önemli öğeler vardır.

Döndürülen değer türü **nesne**olarak ayarlanır, bu da json nesnesini döndürdettiği anlamına gelir.

Depolama hesabının çalışma zamanı durumunu almak için [başvuru](template-functions-resource.md#reference) işlevini kullanır. Bir kaynağın çalışma zamanı durumunu almak için, kaynağın adını veya kimliğini geçersiniz. Bu durumda, depolama hesabının adını oluşturmak için kullandığınız değişkeni kullanırsınız.

Son olarak, depolama hesabından **birincil Endpoints** özelliğidöndürür

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtmaya ve döndürülen değere bakmaya hazırsınız.

Kaynak grubunu oluşturmadıysanız, [bkz.](template-tutorial-create-first-template.md#create-resource-group) Örnek, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi **şablonDosya** değişkenini şablon dosyasına giden yola ayarladığınız varsayar.

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

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
az deployment group create \
  --name addoutputs \
  --resource-group myResourceGroup \
  --template-file $templateFile \
  --parameters storagePrefix=store storageSKU=Standard_LRS
```

---

Dağıtım komutu çıktısında, yalnızca çıktı JSON biçimindeyse aşağıdaki örneğe benzer bir nesne görürsünüz:

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
> Dağıtım başarısız olduysa, hata ayıklama günlüklerini göstermek için dağıtım komutuyla **hata ayıklama** anahtarını kullanın.  Tam hata ayıklama günlüklerini göstermek için **ayrıntılı** anahtar da kullanabilirsiniz.

## <a name="review-your-work"></a>Çalışmanızı gözden geçirin

Son altı derste çok şey yaptın. Yaptıklarınızı gözden geçirmek için bir dakikanızı ayıralım. Sağlaması kolay parametreleriçeren bir şablon oluşturdunuz. Şablon, özelleştirmeye izin verdiği ve dinamik olarak gerekli değerleri oluşturduğundan farklı ortamlarda yeniden kullanılabilir. Ayrıca komut dosyanızda kullanabileceğiniz depolama hesabı yla ilgili bilgileri de döndürür.

Şimdi kaynak grubuna ve dağıtım geçmişine bakalım.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol menüden **Kaynak gruplarını**seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Yaptığınız adımlara bağlı olarak, kaynak grubunda en az bir ve belki de birkaç depolama hesabınız olmalıdır.
1. Ayrıca, geçmişte listelenen birkaç başarılı dağıtımları olmalıdır. Bağlantıyı seçin.

   ![Dağıtımları seçin](./media/template-tutorial-add-outputs/select-deployments.png)

1. Tarihteki tüm konuşlandırmalarınızı görüyorsunuz. **Addoutputs**adlı dağıtımı seçin.

   ![Dağıtım geçmişini göster](./media/template-tutorial-add-outputs/show-history.png)

1. Girişleri gözden geçirebilirsiniz.

   ![Girişleri göster](./media/template-tutorial-add-outputs/show-inputs.png)

1. Çıktıları gözden geçirebilirsiniz.

   ![Çıktıları göster](./media/template-tutorial-add-outputs/show-outputs.png)

1. Şablonu gözden geçirebilirsiniz.

   ![Şablonu göster](./media/template-tutorial-add-outputs/show-template.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silerek dağıttığınız kaynakları temizlemek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şablona bir iade değeri eklediniz. Bir sonraki öğreticide, şablonu nasıl dışa aktaracağınız ve bu dışa aktarılan şablonun bölümlerini şablonunuzda nasıl kullanacağınızı öğreneceksiniz.

> [!div class="nextstepaction"]
> [Dışa aktarılan şablonu kullanma](template-tutorial-export-template.md)
