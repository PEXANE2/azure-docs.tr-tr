---
title: Azure düğmesine dağıt
description: Azure Kaynak Yöneticisi şablonlarını GitHub deposundan dağıtmak için düğmeyi kullanın.
ms.topic: conceptual
ms.date: 02/07/2020
ms.openlocfilehash: 88436eac970b252d7b0bc7bccee4131e06e9e0cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77109056"
---
# <a name="use-a-deployment-button-to-deploy-templates-from-github-repository"></a>GitHub deposundan şablondağıtmak için dağıtım düğmesini kullanma

Bu makalede, GitHub deposundan şablondağıtmak için **Azure'a Dağıt** düğmesinin nasıl kullanılacağı açıklanmaktadır. Düğmeyi doğrudan GitHub deponuzdaki README.md dosyaya veya depoya atıfta bulunan bir web sayfasına ekleyebilirsiniz.

## <a name="use-common-image"></a>Ortak görüntü kullanma

Düğmeyi web sayfanıza veya deponuza eklemek için aşağıdaki resmi kullanın:

```html
<img src="https://aka.ms/deploytoazurebutton"/>
```

Görüntü şu şekilde görünür:

![Azure düğmesine dağıt](https://aka.ms/deploytoazurebutton)

## <a name="create-url-for-deploying-template"></a>Şablondağıtmak için URL oluşturma

Şablonunuzun URL'sini oluşturmak için, repo'nuzdaki şablonun ham URL'si ile başlayın:

```html
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Sonra, URL kodlayın. Çevrimiçi kodlayıcı kullanabilir veya bir komut çalıştırabilirsiniz. Aşağıdaki PowerShell örneği, url'nin bir değeri nasıl kodlayabildiğini gösterir.

```powershell
[uri]::EscapeDataString($url)
```

Örnek URL kodlandığında aşağıdaki değere sahiptir.

```html
https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

Her bağlantı aynı temel URL ile başlar:

```html
https://portal.azure.com/#create/Microsoft.Template/uri/
```

URL kodlanmış şablon bağlantınızı temel URL'nin sonuna ekleyin.

```html
https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

Bağlantı için tam URL'niz var.

## <a name="create-deploy-to-azure-button"></a>Azure'a Dağıt düğmesi oluştur

Son olarak, bağlantıyı ve görüntüyü bir araya getirin.

GitHub deponuzdaki veya web sayfanızdaki README.md dosyasında Markdown ile düğmeyi eklemek için şunları kullanın:

```markdown
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)
```

HTML için şunları kullanın:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json" target="_blank">
  <img src="https://aka.ms/deploytoazurebutton"/>
</a>
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Çözümün tamamını test etmek için aşağıdaki düğmeyi seçin:

[![Azure'a Dağıt](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)

Portal, parametre değerlerini kolayca sağlamanızı sağlayan bir bölme görüntüler. Parametreler şablondaki varsayılan değerlerle önceden doldurulmuştür.

![Dağıtmak için portalı kullanma](./media/deploy-to-azure-button/portal.png)

## <a name="next-steps"></a>Sonraki adımlar

- Şablonlar hakkında daha fazla bilgi edinmek için [bkz.](template-syntax.md)
