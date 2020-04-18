---
title: Şablon oluşturma - Visual Studio Code
description: Resource Manager şablonları üzerinde çalışmak için Visual Studio Code ve Azure Resource Manager araçları eklentisini kullanın.
author: neilpeterson
ms.date: 03/27/2019
ms.topic: quickstart
ms.author: nepeters
ms.openlocfilehash: 4b1ecbf3a1f6083261e87537e20d52e755b77424
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81640742"
---
# <a name="quickstart-create-azure-resource-manager-templates-with-visual-studio-code"></a>Hızlı başlatma: Visual Studio Kodu ile Azure Kaynak Yöneticisi şablonları oluşturma

Visual Studio Code için Azure Kaynak Yöneticisi Araçları dil desteği, kaynak parçacıkları ve kaynak otomatik tamamlama sağlar. Bu araçlar, Azure Kaynak Yöneticisi şablonlarının oluşturulmasına ve doğrulanmasına yardımcı olur. Bu hızlı başlangıçta, uzantıyı sıfırdan bir Azure Kaynak Yöneticisi şablonu oluşturmak için kullanırsınız. Bunu yaparken ARM şablon parçacıkları, doğrulama, tamamlama lar ve parametre dosya desteği gibi uzantı özelliklerini deneyimlersiniz.

Bu hızlı başlangıcı tamamlamak için, [Azure Kaynak Yöneticisi araçları uzantısı](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) yüklü visual studio [koduna](https://code.visualstudio.com/)ihtiyacınız vardır. Ayrıca [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) veya [Azure PowerShell modülüyüklü](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.7.0) ve kimlik doğrulaması gerekir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="create-an-arm-template"></a>ARM şablonu oluşturma

Visual Studio Code ile *azuredeploy.json*adında yeni bir dosya oluşturun ve açın. ARM `arm` şablonu oluşturmak için Azure Kaynak Yöneticisi parçacıklarını başlatan kod düzenleyicisine girin.

Azure `arm!` kaynak grubu dağıtımı için kapsamlı bir şablon oluşturmak için seçin.

![Azure Kaynak Yöneticisi iskelesi gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/1.png)

Bu parçacık, ARM şablonu için temel yapı taşlarını oluşturur.

![Tam iskeleli ARM şablonu gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/2.png)

Visual Studio Code dil modunun *JSON'dan* *Azure Kaynak Yöneticisi Şablonu'na*dönüştüğüne dikkat edin. Uzantı, ARM şablonuna özgü doğrulama, tamamlama ve diğer dil hizmetlerini sağlayan ARM şablonlarına özgü bir dil sunucusu içerir.

![Azure Kaynak Yöneticisi'ni Visual Studio Code dil modu olarak gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/3.png)

## <a name="add-an-azure-resource"></a>Azure kaynağı ekleme

Uzantı, birçok Azure kaynağı için parçacıklar içerir. Bu parçacıklar, şablon dağıtımınıza kolayca kaynak eklemek için kullanılabilir.

İmleci şablon **kaynakları** bloğuna yerleştirin, `storage`yazın ve *kol depolama* parçacığı seçin.

![ARM şablonuna eklenen bir kaynağı gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/4.png)

Bu eylem şablona bir depolama kaynağı ekler.

![ARM şablonundaki Azure Depolama kaynağını gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/5.png)

**Sekme** tuşu, depolama hesabındayapılan yapılandırılabilir özellikleri sekmelemek için kullanılabilir.

![Sekme anahtarının kaynak yapılandırması nda gezinmek için nasıl kullanılabileceğini gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/6.png)

## <a name="completion-and-validation"></a>Tamamlama ve doğrulama

Uzantının en güçlü özelliklerinden biri, Azure şemalarıyla tümleştirilmesidir. Azure şemaları uzantıyı doğrulama ve kaynağa duyarlı tamamlama özellikleriyle sağlar. Doğrulama ve tamamlanma eylemini görmek için depolama hesabını değiştirelim. 

İlk olarak, depolama hesabı türünü '. `megaStorage` Bu eylemin geçerli bir değer `megaStorage` olmadığını belirten bir uyarı ürettiğine dikkat edin.

![Geçersiz depolama yapılandırması gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/7.png)

Tamamlama özelliklerini kullanmak için, `megaStorage`kaldır, imleci çift tırnak içine yerleştirin `ctrl`  +  `space`ve . Bu eylem, geçerli değerlerin bir tamamlanma listesini sunar.

![Uzantılı otomatik tamamlamayı gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/8.png)

## <a name="add-template-parameters"></a>Şablon parametreleri ekleme

Şimdi depolama hesabı adını belirtmek için bir parametre oluşturun ve kullanın.

İmlecinizi parametre bloğuna yerleştirin, bir `par`satır başı ekleyin, yazın ve sonra `arm-param-value` parçacığı seçin. Bu eylem şablona genel bir parametre ekler.

![ARM şablonuna eklenen bir parametreyi gösteren görüntü](./media/quickstart-create-templates-use-visual-studio-code/9.png)

Parametrenin adını `storageAccountName` ve açıklamasını `Storage Account Name`güncelleştirin.

![ARM şablonunda tamamlanan parametreyi gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/10.png)

Azure depolama hesabı adlarının en az 3 karakter uzunluğunda ve en fazla 24 karakteri vardır. Hem `minLength` de `maxLength` parametre ye ekleyin ve uygun değerleri sağlayın.

![ARM şablon parametresine eklenen minLength ve maxLength'ı gösteren görüntü](./media/quickstart-create-templates-use-visual-studio-code/11.png)

Şimdi, depolama kaynağında, parametreyi kullanmak için ad özelliğini güncelleştirin. Bunu yapmak için geçerli adı kaldırın. ARM şablon işlevlerinin bir `[`listesini oluşturan bir çift teklif ve açılış kare ayraç girin. Listeden *parametreleri* seçin. 

![ARM şablon kaynaklarında parametreleri kullanırken otomatik tamamlamayı gösteren görüntü](./media/quickstart-create-templates-use-visual-studio-code/12.png)

Yuvarlak parantez içinde `'` tek bir teklif girerek şablonda tanımlanan tüm parametrelerin bir listesini üretir, bu durumda, *storageAccountName*. Parametreyi seçin.

![ARM şablon uytundaki tamamlanmış parametreyi gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/13.png)

## <a name="create-a-parameter-file"></a>Parametre dosyası oluşturma

ARM şablon parametre dosyası, ortama özgü parametre değerlerini depolamanızı ve bu değerleri dağıtım zamanında grup olarak geçirmenize olanak tanır. Örneğin, bir test ortamına özgü değerlere sahip bir parametre dosyanız ve üretim ortamı için başka bir parametre dosyanız olabilir.

Uzantı, varolan şablonlarınızdan parametre dosyası oluşturmayı kolaylaştırır. Bunu yapmak için, kod düzenleyicisindeki şablona `Select/Create Parameter File`sağ tıklayın ve .

![ARM şablonundan parametre dosyası oluşturmak için sağ tıklatma işlemini gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/14.png)

Parametre `New`  >  `All Parameters` dosyası için bir ad ve konum seçin > seçin.

![ARM şablonundan parametre dosyası oluştururken adı gösteren resim ve dosya iletişim kutusunu kaydetme](./media/quickstart-create-templates-use-visual-studio-code/15.png)

Bu eylem yeni bir parametre dosyası oluşturur ve oluşturulduğu şablonla eşler. Şablon seçilirken Visual Studio Code durum çubuğundaki geçerli şablon/parametre dosya eşlemini görebilir ve değiştirebilirsiniz.

![](./media/quickstart-create-templates-use-visual-studio-code/16.png)

Parametre dosyası şablona eşlenmiş olduğundan, uzantı hem şablonu hem de parametre dosyasını birlikte doğrular. Uygulamada bu doğrulamayı görmek için parametre dosyasındaki `storageAccountName` parametreye iki karakterli bir değer ekleyin ve dosyayı kaydedin.

![Parametre dosyası sorunu nedeniyle geçersiz kılınan şablonu gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/17.png)

ARM şablonuna geri gidin ve değerin parametre ölçütlerini karşılamadığını belirten bir hatanın yükseltildiğini fark edin.

![Geçerli bir ARM şablonu gösteren resim](./media/quickstart-create-templates-use-visual-studio-code/18.png)

Değeri uygun bir şeyle güncelleştirin, dosyayı kaydedin ve şablona geri gidin. Parametredeki hatanın çözüldüğüne dikkat edin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Tümleşik Visual Studio Code `ctrl`  +  ```` ` ```` terminalini anahtar kombinasyonunu kullanarak açın ve şablonu dağıtmak için Azure CLI veya Azure PowerShell modüllerini kullanın.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
az group create --name arm-vscode --location eastus

az deployment group create --resource-group arm-vscode --template-file azuredeploy.json --parameters azuredeploy.parameters.json
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
New-AzResourceGroup -Name arm-vscode -Location eastus

New-AzResourceGroupDeployment -ResourceGroupName arm-vscode -TemplateFile ./azuredeploy.json -TemplateParameterFile ./azuredeploy.parameters.json
```
---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarına artık ihtiyaç duyulmadığında, hızlı başlangıç kaynak grubunu silmek için Azure CLI veya Azure PowerShell modülünü kullanın.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
az group delete --name arm-vscode
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
Remove-AzResourceGroup -Name arm-vscode
```
---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Başlangıç öğretileri](./template-tutorial-create-first-template.md)
