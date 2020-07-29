---
title: 'Hızlı başlangıç: ARM şablonları kullanarak Azure bilişsel hizmetler kaynağı oluşturma | Microsoft Docs'
description: Bilişsel hizmetler kaynağını dağıtmak için Azure Resource Manager şablonu kullanmaya başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: quickstart
ms.date: 07/27/2020
ms.author: aahi
ms.custom: subject-armqs
ms.openlocfilehash: 9ecbd7778480d37fb0a0cf135d3cc5db48bf2add
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323664"
---
# <a name="quickstart-create-a-cognitive-services-resource-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonunu kullanarak bilişsel hizmetler kaynağı oluşturma

Bir Azure Resource Manager şablonu (ARM şablonu) kullanarak bilişsel hizmetler kaynağı oluşturup dağıtmak için bu makaleyi kullanın. Bu çok hizmet kaynağı şunları yapmanızı sağlar:
* Birden çok Azure bilişsel hizmete tek bir anahtar ve uç nokta ile erişin.
* Kullandığınız hizmetlerden faturalandırma yapın.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cognitive-services-universalkey%2Fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services).

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/101-cognitive-services-universalkey/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-cognitive-services-universalkey/azuredeploy.json" highlight="27-41":::

Şablonda bir Azure kaynağı tanımlanmıştır:
* [Microsoft. Biliveservices/accounts](https://docs.microsoft.com/azure/templates/microsoft.cognitiveservices/accounts): bilişsel hizmetler kaynağı oluşturur.

## <a name="deploy-the-template"></a>Şablonu dağıtma

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

1. **Azure 'A dağıt** düğmesine tıklayın.

    [![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cognitive-services-universalkey%2Fazuredeploy.json)

2. Aşağıdaki değerleri girin.
    
    |Değer  |Açıklama  |
    |---------|---------|
    | **Abonelik** | Bir Azure aboneliği seçin. |
    | **Kaynak grubu** | **Yeni oluştur**' u seçin, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam**' a tıklayın. |
    | **Bölge** | Bir bölge seçin.  Örneğin, **Doğu ABD** |
    | **Bilişsel hizmet adı** | Kaynağınız için benzersiz bir adla değiştirin. Dağıtımı doğruladığınızda, sonraki bölümde adı kullanmanız gerekir. |
    | **Konum** | Yukarıda kullanılan bölge ile değiştirin. |
    | **İsteyin** | Kaynağınızın [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/cognitive-services/) . |
    
    :::image type="content" source="media/arm-template/universal-key-portal-template.png" alt-text="Kaynak oluşturma ekranı.":::

3. **Gözden Geçir ve Oluştur**’u ve sonra **Oluştur**’u seçin. Kaynak başarıyla dağıtımı tamamladıktan sonra **Kaynağa Git** düğmesi vurgulanacaktır.

# <a name="azure-cli"></a>[Azure CLI](#tab/CLI)

> [!NOTE]
> `az deployment group`oluşturma için Azure CLı sürüm 2,6 veya üzeri gerekir. Sürüm türünü görüntüleme `az --version` . Daha fazla bilgi için [belgelerine](https://docs.microsoft.com/cli/azure/deployment/group)bakın.

[Yerel makinenizde](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)Azure komut satırı arabirimi 'NI (CLI) kullanarak veya **deneyin** düğmesine sahip bir tarayıcıda aşağıdaki betiği çalıştırın. Yeni bir kaynak grubu için bir ad ve konum (örneğin `centralus` ) girin ve ARM şablonu bunun içinde bilişsel hizmetler kaynağını dağıtmak için kullanılacaktır. Kullandığınız adı unutmayın. Daha sonra dağıtımı doğrulamak için kullanacaksınız.


```azurecli-interactive
read -p "Enter a name for your new resource group:" resourceGroupName &&
read -p "Enter the location (i.e. centralus):" location &&
templateUri="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-cognitive-services-universalkey/azuredeploy.json" &&
az group create --name $resourceGroupName --location "$location" &&
az deployment group create --resource-group $resourceGroupName --template-uri  $templateUri &&
echo "Press [ENTER] to continue ..." &&
read
```

---

[!INCLUDE [Register Azure resource for subscription](./includes/register-resource-subscription.md)]


## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

# <a name="portal"></a>[Portal](#tab/portal)

Dağıtımınız tamamlandığında, yeni kaynağını görmek için **Kaynağa Git** düğmesine tıklayabilirsiniz. Kaynak grubunu da şu şekilde bulabilirsiniz:

1. Sol gezinti menüsünden **kaynak grupları** seçiliyor.
2. Kaynak grubu adı seçiliyor.

# <a name="azure-cli"></a>[Azure CLI](#tab/CLI)

Azure CLı 'yı kullanarak aşağıdaki betiği çalıştırın ve daha önce oluşturduğunuz kaynak grubunun adını girin.

```azurecli-interactive
echo "Enter the resource group where the Cognitive Services resource exists:" &&
read resourceGroupName &&
az cognitiveservices account list -g $resourceGroupName
```

---


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel hizmetler aboneliğini temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, grupta yer alan diğer tüm kaynakları da siler.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve **Kaynak Grupları**'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynağı içeren kaynak grubunu bulun
3. Kaynak grubu listesine sağ tıklayın. **Kaynak grubunu sil**'i seçip onaylayın.

# <a name="azure-cli"></a>[Azure CLI](#tab/CLI)

Azure CLı 'yı kullanarak aşağıdaki betiği çalıştırın ve daha önce oluşturduğunuz kaynak grubunun adını girin. 

```azurecli-interactive
echo "Enter the resource group name, for deletion:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName
```

---

## <a name="next-steps"></a>Sonraki adımlar

* [Azure bilişsel hizmetler 'e yönelik isteklerin kimliğini doğrulama](authentication.md)
* [Azure bilişsel hizmetler nedir?](Welcome.md)
* [Doğal dil desteği](language-support.md)
* [Docker kapsayıcı desteği](cognitive-services-container-support.md)
