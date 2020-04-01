---
title: Öğretici - Şablona kaynak ekleme
description: İlk Azure Kaynak Yöneticisi şablonunuzu oluşturmak için adımları açıklar. Şablon dosyası sözdizimini ve depolama hesabının nasıl dağıtılabildiğini öğrenirsiniz.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: dcdbbb325e6589669abe6cf3d25ac5191e29118b
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411744"
---
# <a name="tutorial-add-a-resource-to-your-arm-template"></a>Öğretici: ARM şablonunuza kaynak ekleme

Önceki [öğreticide,](template-tutorial-create-first-template.md)boş bir şablon oluşturmayı ve dağıtmayı öğrendiniz. Şimdi, gerçek bir kaynak dağıtmaya hazırsınız. Bu öğreticide, bir depolama hesabı eklersiniz. Bu öğretici tamamlamak için yaklaşık **9 dakika** sürer.

## <a name="prerequisites"></a>Ön koşullar

[Şablonlar hakkındaki giriş eğitimini](template-tutorial-create-first-template.md)tamamlamanızı öneririz, ancak gerekli değildir.

Kaynak Yöneticisi Araçları uzantısı ve Azure PowerShell veya Azure CLI ile Visual Studio Kodu'na sahip olmalısınız. Daha fazla bilgi için [şablon araçlarına](template-tutorial-create-first-template.md#get-tools)bakın.

## <a name="add-resource"></a>Kaynak ekleme

Varolan şablona bir depolama hesabı tanımı eklemek için aşağıdaki örnekte vurgulanan JSON'a bakın. Şablonun bölümlerini kopyalamaya çalışmak yerine, tüm dosyayı kopyalayın ve şablonunuzu içeriğiyle değiştirin.

**{benzersiz ad}** (kıvırcık köşeli ayraçlar dahil) ve benzersiz bir depolama hesabı adı değiştirin.

> [!IMPORTANT]
> Depolama hesabı adının Azure’da benzersiz olması gerekir. Adın yalnızca küçük harfleri veya sayıları olmalıdır. En fazla 24 karakter olabilir. **Store1'i** önek olarak kullanarak baş harflerinizi ve bugünün tarihini ekleme gibi bir adlandırma deseni deneyebilirsiniz. Örneğin, kullandığınız ad **store1abc09092019**gibi görünebilir.

:::code language="json" source="~/resourcemanager-templates/get-started-with-templates/add-storage/azuredeploy.json" range="1-19" highlight="5-17":::

Bir depolama hesabı için benzersiz bir adı tahmin etmek kolay değildir ve büyük dağıtımları otomatikleştirmek için iyi çalışmaz. Bu öğretici serinin ilerleyen saatlerinde, benzersiz bir ad oluşturmayı kolaylaştıran şablon özelliklerini kullanırsınız.

## <a name="resource-properties"></a>Kaynak özellikleri

Her kaynak türü için kullanılacak özellikleri nasıl bulacağınızı merak ediyor olabilirsiniz. Dağıtmak istediğiniz kaynak türlerini bulmak için [ARM şablonu başvurularını](/azure/templates/) kullanabilirsiniz.

Dağıttığınız her kaynak en az aşağıdaki üç özellime sahiptir:

- **türü**: Kaynağın türü. Bu değer, kaynak sağlayıcısının ad alanı nın ve kaynak türünün (Microsoft.Storage/storageAccounts gibi) bir leşimidir.
- **apiVersion**: Kaynak oluşturmak için kullanılacak REST API sürümü. Her kaynak sağlayıcısı kendi API sürümlerini yayımlar, bu nedenle bu değer türüne özgüdür.
- **ad**: Kaynağın adı.

Çoğu kaynağın, kaynağın dağıtıldığı bölgeyi belirleyen bir **konum** özelliği de var.

Diğer özellikler kaynak türüne ve API sürümüne göre değişir. API sürümü ile kullanılabilir özellikler arasındaki bağlantıyı anlamak önemlidir, bu nedenle daha fazla ayrıntıya atlayalım.

Bu öğreticide, şablona bir depolama hesabı eklediniz. Bu API sürümünü [storageAccounts 2019-04-01'de](/azure/templates/microsoft.storage/2019-04-01/storageaccounts)görebilirsiniz. Şablonunuzun tüm özelliklerini eklemediğinize dikkat edin. Özelliklerin çoğu isteğe bağlıdır. Microsoft.Storage kaynak sağlayıcısı yeni bir API sürümü yayımlayabilir, ancak dağıttığınız sürümün değişmesi gerekmez. Bu sürümü kullanmaya devam edebilir ve dağıtımınızın sonuçlarının tutarlı olacağını bilebilirsiniz.

[StorageAccounts 2016-05-01](/azure/templates/microsoft.storage/2016-05-01/storageaccounts)gibi eski bir API sürümünü görüntülerseniz, daha küçük bir özellik kümesinin kullanılabildiğini görürsünüz.

Bir kaynak için API sürümünü değiştirmeye karar verirseniz, bu sürümün özelliklerini değerlendirip şablonunuzu uygun şekilde ayarladığınızdan emin olun.

## <a name="deploy-template"></a>Şablon dağıtma

Depolama hesabı oluşturmak için şablonu dağıtabilirsiniz. Dağıtımınıza farklı bir ad verin, böylece tarihte kolayca bulabilirsiniz.

Kaynak grubunu oluşturmadıysanız, [bkz.](template-tutorial-create-first-template.md#create-resource-group) Örnek, [ilk öğreticide](template-tutorial-create-first-template.md#deploy-template)gösterildiği gibi **şablonDosya** değişkenini şablon dosyasına giden yola ayarladığınız varsayar.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroupDeployment `
  -Name addstorage `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
az deployment group create \
  --name addstorage \
  --resource-group myResourceGroup \
  --template-file $templateFile
```

---

> [!NOTE]
> Dağıtım başarısız olduysa, hata ayıklama günlüklerini göstermek için dağıtım komutuyla **hata ayıklama** anahtarını kullanın.  Tam hata ayıklama günlüklerini göstermek için **ayrıntılı** anahtar da kullanabilirsiniz.

Karşılaşabileceğiniz iki olası dağıtım hatası:

- Hata: Kod=Hesap Adı Geçersiz; Message={provide-unique-name} geçerli bir depolama hesabı adı değildir. Depolama hesabı adı uzunluğu 3 ve 24 karakter arasında olmalı ve yalnızca sayılar ve küçük harfler kullanmalıdır.

    **Şablonda, {benzersiz ad sağlama}** yerine benzersiz bir depolama hesabı adı bulundurun.  Bkz. [Kaynak Ekle](#add-resource).

- Hata: Kod=StorageAccountAlreadyTaken; Mesaj=Store1abc09092019 adlı depolama hesabı zaten alınmıştır.

    Şablonda, farklı bir depolama hesabı adı deneyin.

Depolama hesabı oluşturulduğundan, bu dağıtım boş şablon dağıtımınızdan daha uzun sürer. Bu yaklaşık bir dakika sürebilir ama genellikle daha hızlıdır.

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Azure portalındaki kaynak grubunu keşfederek dağıtımı doğrulayabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol menüden **Kaynak gruplarını**seçin.
1. Dağıttığınız kaynak grubunu seçin.
1. Bir depolama hesabının dağıtıldığını görüyorsunuz.
1. Dağıtım etiketinde şu anda şöyle yazdığına dikkat edin: **Dağıtımlar: 2 Başarılı .**

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silerek dağıttığınız kaynakları temizlemek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure depolama hesabı dağıtmak için basit bir şablon oluşturdunuz.  Sonraki öğreticilerde, şablona parametreler, değişkenler, kaynaklar ve çıktılar eklemeyi öğrenirsiniz. Bu özellikler çok daha karmaşık şablonlar için yapı taşlarıdır.

> [!div class="nextstepaction"]
> [Parametre ekleme](template-tutorial-add-parameters.md)