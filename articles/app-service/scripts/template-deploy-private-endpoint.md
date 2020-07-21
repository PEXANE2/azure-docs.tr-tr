---
title: Bu Azure Resource Manager şablonuyla, Web uygulaması için özel uç nokta dağıtacaksınız.
description: Web uygulamanız için özel uç nokta dağıtmak üzere ARM şablonunu nasıl kullanacağınızı öğrenin
author: ericgre
ms.assetid: 49e460d0-7759-4ceb-b5a4-f1357e4fde56
ms.topic: sample
ms.date: 07/08/2020
ms.author: ericg
ms.service: app-service
ms.workload: web
ms.openlocfilehash: ff2d59ed569037f34e24a69ffafa0df237a3de34
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86535730"
---
# <a name="create-an-app-service-app-and-deploy-private-endpoint-using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak App Service uygulaması oluşturma ve özel uç nokta dağıtma

Bu hızlı başlangıçta, bir Web uygulaması oluşturmak ve özel bir uç nokta ile sunmak için bir Azure Resource Manager şablonu kullanırsınız.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisite"></a>Önkoşul

Etkin aboneliği olan bir Azure hesabınızın olması gerekir. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

Bu şablon, bir Azure Web uygulaması için özel bir uç nokta oluşturur.

### <a name="review-the-template"></a>Şablonu gözden geçirme

:::code language="json" source="~/quickstart-templates/101-private-endpoint-webapp/azuredeploy.json" :::

### <a name="deploy-the-template"></a>Şablonu dağıtma

Azure Resource Manager şablonunu Azure 'a dağıtma:

1. Azure 'da oturum açmak ve şablonu açmak için **Azure 'A dağıt**' ı seçin. Şablon VNet, Web uygulaması, Özel uç nokta ve özel DNS bölgesi oluşturur.

   [Azure’a dağıtın](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-private-endpoint-webapp%2Fazuredeploy.json)

2. Kaynak grubunuzu seçin veya oluşturun.
3. Web uygulamanızın adını, App Service planı, Özel uç noktasını yazın.
5. Hüküm ve koşullar ekstresini okuyun. Kabul ediyorsanız, satın alma > yukarıda belirtilen hüküm ve koşulları kabul ediyorum ' u seçin. Dağıtımın süresi uzun sürebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel uç nokta ile oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, kaynak grubunu silin. Bu, Özel uç noktayı ve ilgili tüm kaynakları kaldırır.

Kaynak grubunu silmek için `Remove-AzResourceGroup` cmdlet 'ini çağırın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name <your resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure App Service Web Apps için ek Azure Resource Manager şablonları [ARM şablonu örneklerinde](../samples-resource-manager-templates.md)bulunabilir.
