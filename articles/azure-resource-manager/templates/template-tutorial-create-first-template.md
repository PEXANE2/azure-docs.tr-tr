---
title: Öğretici - & dağıtma şablonu oluşturun
description: İlk Azure Kaynak Yöneticisi şablonunuzu oluşturun. Öğreticide, şablon dosyası sözdizimini ve depolama hesabının nasıl dağıtılabildiğini öğrenirsiniz.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: ace76b9a13f44c14e348a0338ca01dd6b3948ce3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369934"
---
# <a name="tutorial-create-and-deploy-your-first-arm-template"></a>Öğretici: İlk ARM şablonunuzu oluşturun ve dağıtın

Bu öğretici, Azure Kaynak Yöneticisi (ARM) şablonlarını sunar. Başlangıç şablonu oluşturup Azure'a nasıl dağıtabileceğinizi gösterir. Şablonun yapısı ve şablonlarla çalışmak için gereksinim duyduğunuz araçlar hakkında bilgi edineceksiniz. Bu öğreticinin tamamlanması yaklaşık **12 dakika** sürer, ancak asıl süre yüklemeniz gereken araçlara bağlı olarak değişir.

Bu öğretici bir serinin ilkidir. Seriboyunca ilerledikçe, ARM şablonunun tüm temel bölümlerini keşfedene kadar başlangıç şablonu adım adım değiştirirsiniz. Bu öğeler çok daha karmaşık şablonlar için yapı taşlarıdır. Serinin sonunda kendi şablonlarınızı oluşturarak emin olduğunuzu ve dağıtımlarınızı şablonlarla otomatikleştirmeye hazır olduğunuzu umuyoruz.

Şablonları kullanmanın yararları ve dağıtımı şablonlarla neden otomatikleştirmeniz gerektiği hakkında bilgi edinmek istiyorsanız, [Azure Kaynak Yöneticisi şablonlarına](overview.md)bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="get-tools"></a>Araçları alın

Şablonlar oluşturmak ve dağıtmak için ihtiyacınız olan araçlara sahip olduğundan emin olarak başlayalım.

### <a name="editor"></a>Düzenleyici

Şablonlar JSON dosyalarıdır. Şablonlar oluşturmak için iyi bir JSON düzenleyicisine ihtiyacınız vardır. Kaynak Yöneticisi Araçları uzantılı Visual Studio Code'u öneririz. Bu araçları yüklemeniz gerekiyorsa, [ARM şablonları oluşturmak için Visual Studio Kodunu Kullan'a](use-vs-code-to-create-template.md)bakın.

### <a name="command-line-deployment"></a>Komut satırı dağıtımı

Ayrıca şablonu dağıtmak için Azure PowerShell veya Azure CLI'ye de ihtiyacınız var. Azure CLI kullanıyorsanız, en son sürüme sahip olmalısınız. Yükleme yönergeleri için bkz:

- [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps)
- [Windows'da Azure CLI'yi yükleme](/cli/azure/install-azure-cli-windows)
- [Azure CLI'yi Linux'a yükleyin](/cli/azure/install-azure-cli-linux)

Azure PowerShell veya Azure CLI'yi yükledikten sonra ilk kez oturum açarak oturum açtın. Yardım için oturum [açma - PowerShell](/powershell/azure/install-az-ps#sign-in) veya [Oturum Aç - Azure CLI](/cli/azure/get-started-with-azure-cli#sign-in)' ya bakın.

Tamam, şablonlar hakkında bilgi edinmeye hazırsın.

## <a name="create-your-first-template"></a>İlk şablonunuzu oluşturma

1. Kaynak Yöneticisi Araçları uzantısı yüklü Görsel Stüdyo Kodunu açın.
1. **Dosya** menüsünden yeni bir dosya oluşturmak için **Yeni Dosya'yı** seçin.
1. **Dosya** menüsünden **Kaydet'i**seçin.
1. Dosyayı **azuredeploy'ı** adlandırın ve **JSON** dosya uzantısını seçin. **Azuredeploy.json**dosyasının tam adı.
1. Dosyayı iş istasyonunuzun merkezine kaydedin. Şablonu dağıtırken bu yolu daha sonra sağlayacağınız için hatırlanması kolay bir yol seçin.
1. Aşağıdaki JSON'u dosyaya kopyalayıp yapıştırın:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": []
    }
    ```

    VS Kodu ortamınız şu şekilde görünür:

    ![Kaynak Yöneticisi şablonu görsel stüdyo kodu ilk şablonu](./media/template-tutorial-create-first-template/resource-manager-visual-studio-code-first-template.png)

    Bu şablon herhangi bir kaynak dağıtmaz. Bir şeylerin yanlış gitme olasılığını en aza indirirken şablon dağıtma adımlarını anlamanız için boş bir şablonla başlıyoruz.

    JSON dosyasışu öğeleri içerir:

    - **$schema**: JSON şema dosyasının konumunu belirtir. Şema dosyası, şablon içinde kullanılabilen özellikleri açıklar. Örneğin, şema **kaynakları** şablon için geçerli özelliklerden biri olarak tanımlar. Şema için tarih 2019-04-01 olduğunu merak etmeyin. Bu şema sürümü günceldir ve en son özelliklerin tümlerini içerir. Şema tarihi değiştirilmemiştir çünkü piyasaya sürülmesinden bu yana herhangi bir kırılma olmamıştır.
    - **contentVersion**: Şablonun sürümünü belirtir (1.0.0.0 gibi). Bu öğe için herhangi bir değer sağlayabilir. Şablonunuzdaönemli değişiklikleri belgelemek için bu değeri kullanın. Şablonu kullanarak kaynakları dağıtırken, bu değer doğru şablonun kullanıldığından emin olmak için kullanılabilir.
    - **kaynaklar**: Dağıtmak veya güncelleştirmek istediğiniz kaynakları içerir. Şu anda boş, ancak daha sonra kaynak eklersiniz.

1. Dosyayı kaydedin.

Tebrikler, ilk şablonunuzu oluşturdunuz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure PowerShell/Azure CLI ile çalışmaya başlamak için Azure kimlik bilgilerinizle oturum açın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
Connect-AzAccount
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az login
```

---
## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Bir şablon dağıttığınızda, kaynakları içeren bir kaynak grubu belirtirsiniz. Dağıtım komutunu çalıştırmadan önce, Azure CLI veya Azure PowerShell ile kaynak grubu oluşturun. Azure PowerShell ve Azure CLI arasında seçim yapmak için aşağıdaki kod bölümündeki sekmeleri seçin. Bu makaledeki CLI örnekleri Bash kabuğu için yazılmıştır.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
New-AzResourceGroup `
  -Name myResourceGroup `
  -Location "Central US"
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az group create \
  --name myResourceGroup \
  --location "Central US"
```

---

## <a name="deploy-template"></a>Şablon dağıtma

Şablonu dağıtmak için Azure CLI veya Azure PowerShell'i kullanın. Oluşturduğunuz kaynak grubunu kullanın. Dağıtım geçmişinde kolayca tanımlayabilmeniz için dağıtıma bir ad verin. Kolaylık sağlamak için, şablon dosyasına giden yolu depolayan bir değişken de oluşturun. Bu değişken, her dağıtyaptığınızda yolu yeniden yazmak zorunda kalmadığınız için dağıtım komutlarını çalıştırmanızı kolaylaştırır.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
$templateFile = "{provide-the-path-to-the-template-file}"
New-AzResourceGroupDeployment `
  -Name blanktemplate `
  -ResourceGroupName myResourceGroup `
  -TemplateFile $templateFile
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bu dağıtım komutunu çalıştırmak için Azure CLI'nin [en son sürümüne](/cli/azure/install-azure-cli) sahip olmalısınız.

```azurecli
templateFile="{provide-the-path-to-the-template-file}"
az deployment group create \
  --name blanktemplate \
  --resource-group myResourceGroup \
  --template-file $templateFile
```

---

Dağıtım komutu sonuçları döndürür. Dağıtımın `ProvisioningState` başarılı olup olmadığını görmek için arayın.

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

![PowerShell dağıtım sağlama durumu](./media/template-tutorial-create-first-template/resource-manager-deployment-provisioningstate.png)

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

![Azure CLI dağıtım sağlama durumu](./media/template-tutorial-create-first-template/azure-cli-provisioning-state.png)

---

## <a name="verify-deployment"></a>Dağıtımı doğrulama

Azure portalındaki kaynak grubunu keşfederek dağıtımı doğrulayabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol menüden **Kaynak gruplarını**seçin.

1. Son yordamda kaynak grubu dağıtımı'nı seçin. Varsayılan ad **myResourceGroup'tur.** Kaynak grubunda hiçbir kaynağın dağıtılmamasını göreceksiniz.

1. Genel görünümün sağ üst kısmında ki bildirimde, dağıtımın durumu görüntülenir. 1 **Başarılı seçin.**

   ![Dağıtım durumunu görüntüleme](./media/template-tutorial-create-first-template/deployment-status.png)

1. Kaynak grubu için dağıtım geçmişini görürsünüz. **Blanktemplate'i**seçin.

   ![Dağıtımı seçin](./media/template-tutorial-create-first-template/select-from-deployment-history.png)

1. Dağıtımın bir özetini görüyorsunuz. Bu durumda, hiçbir kaynak dağıtılmamadığından görülecek çok şey yoktur. Bu serinin ilerleyen saatlerinde, dağıtım geçmişinde özeti gözden geçirmeniz yararlı olabilir. Soldaki bildirimleri, girdileri ve dağıtım sırasında kullanılan şablonu görüntüleyebilirsiniz.

   ![Dağıtım özetini görüntüleme](./media/template-tutorial-create-first-template/view-deployment-summary.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçiyorsanız, kaynak grubunu silmeniz gerekmez.

Şimdi duruyorsanız, kaynak grubunu silmek isteyebilirsiniz.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure'a dağıtmak için basit bir şablon oluşturdunuz. Bir sonraki öğreticide, şablona bir depolama hesabı ekleyecek ve kaynak grubunuza dağıtacaksınız.

> [!div class="nextstepaction"]
> [Kaynak ekleme](template-tutorial-add-resource.md)
