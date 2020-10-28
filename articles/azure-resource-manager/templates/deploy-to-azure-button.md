---
title: Azure'a Dağıt düğmesi
description: Bir GitHub deposundan Azure Resource Manager şablonları dağıtmak için düğmeyi kullanın.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: 62a0a8b0336d9a7fcf00efb172775b9606bcef98
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92675389"
---
# <a name="use-a-deployment-button-to-deploy-templates-from-github-repository"></a>GitHub deposundan şablon dağıtmak için bir dağıtım düğmesi kullanın

Bu makalede, bir GitHub deposundan şablon dağıtmak için **Azure 'Da dağıtma** düğmesinin nasıl kullanılacağı açıklanır. Düğme doğrudan GitHub deponuzdaki README.md dosyasına eklenebilir. İsterseniz, depoya başvuran bir Web sayfasına düğmeyi de ekleyebilirsiniz.

Dağıtım kapsamı, şablon şemasına göre belirlenir. Daha fazla bilgi için bkz.

* [kaynak grupları](deploy-to-resource-group.md)
* [Aboneliklerin](deploy-to-subscription.md)
* [Yönetim grupları](deploy-to-management-group.md)
* [kiracılar](deploy-to-tenant.md).

## <a name="use-common-image"></a>Ortak görüntü kullan

Web sayfanıza veya depoya düğme eklemek için aşağıdaki görüntüyü kullanın:

```html
<img src="https://aka.ms/deploytoazurebutton"/>
```

Görüntü şöyle görünür:

![Azure'a Dağıt düğmesi](https://aka.ms/deploytoazurebutton)

## <a name="create-url-for-deploying-template"></a>Şablon dağıtmak için URL Oluştur

Şablonunuzun URL 'sini oluşturmak için, deponuzdaki şablonun ham URL 'SI ile başlayın. Ham URL 'yi görmek için **RAW** ' ı seçin.

:::image type="content" source="./media/deploy-to-azure-button/select-raw.png" alt-text="Ham Seç":::

URL biçimi:

```html
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Ardından, URL 'yi bir URL kodlu değere dönüştürün. Çevrimiçi bir kodlayıcı kullanabilir veya bir komut çalıştırabilirsiniz. Aşağıdaki PowerShell örneği, bir değeri nasıl bir değerin kodlayagösterdiğini gösterir.

```powershell
[uri]::EscapeDataString($url)
```

URL kodlandıktan sonra örnek URL aşağıdaki değere sahiptir.

```html
https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

Her bağlantı aynı temel URL ile başlar:

```html
https://portal.azure.com/#create/Microsoft.Template/uri/
```

URL kodlu şablon bağlantısını temel URL 'nin sonuna ekleyin.

```html
https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json
```

Bağlantı için tam URL 'niz vardır.

## <a name="create-deploy-to-azure-button"></a>Azure 'a dağıt düğmesine oluştur

Son olarak, bağlantı ve görüntüyü bir araya getirin.

GitHub deponuzdaki README.md dosyasında veya bir Web sayfasında Marku ile düğme eklemek için şunu kullanın:

```markdown
[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)
```

HTML için şunu kullanın:

```html
<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json" target="_blank">
  <img src="https://aka.ms/deploytoazurebutton"/>
</a>
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Çözümü tam olarak test etmek için aşağıdaki düğmeyi seçin:

[![Azure’a dağıtın](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-storage-account-create%2Fazuredeploy.json)

Portal, kolayca parametre değerleri sağlamanıza olanak tanıyan bir bölme görüntüler. Parametreler şablondaki varsayılan değerlerle önceden doldurulur.

![Dağıtmak için portalı kullanma](./media/deploy-to-azure-button/portal.png)

## <a name="next-steps"></a>Sonraki adımlar

- Şablonlar hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](template-syntax.md).
