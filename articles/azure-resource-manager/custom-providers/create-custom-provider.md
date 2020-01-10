---
title: Kaynak sağlayıcısı oluştur
description: Bir kaynak sağlayıcısı oluşturmayı ve özel kaynak türlerini dağıtmayı açıklar.
author: MSEvanhi
ms.topic: tutorial
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: 393993a44c860525b9bd9a540ed7afff78e5b93c
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75649872"
---
# <a name="quickstart-create-custom-provider-and-deploy-custom-resources"></a>Hızlı başlangıç: özel sağlayıcı oluşturma ve özel kaynaklar dağıtma

Bu hızlı başlangıçta, kendi kaynak sağlayıcınızı oluşturup bu kaynak sağlayıcısı için özel kaynak türleri dağıtırsınız. Özel sağlayıcılar hakkında daha fazla bilgi için bkz. [Azure özel sağlayıcılar önizlemeye genel bakış](overview.md).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıçtaki adımları tamamlayabilmeniz için REST işlemlerini çağırmanız gerekir. [Rest isteklerinin gönderilmesi için farklı yollar](/rest/api/azure/)vardır. REST işlemlerine yönelik bir aracınız zaten yoksa, [Armclient](https://github.com/projectkudu/ARMClient)'ı yükleyebilirsiniz. Bu, Azure Resource Manager API 'nin çağrılması basitleşerek açık kaynaklı bir komut satırı aracıdır.

## <a name="deploy-custom-provider"></a>Özel sağlayıcı dağıt

Özel sağlayıcıyı ayarlamak için Azure aboneliğinize [örnek bir şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/custom-providers/customprovider.json) dağıtın.

Şablonu dağıttıktan sonra, aboneliğiniz aşağıdaki kaynaklara sahiptir:

* Kaynaklar ve eylemler için işlemlerle İşlev Uygulaması.
* Özel sağlayıcı aracılığıyla oluşturulan kullanıcıları depolamaya yönelik depolama hesabı.
* Özel kaynak türlerini ve eylemlerini tanımlayan özel sağlayıcı. İstek göndermek için App Endpoint işlevini kullanır.
* Özel sağlayıcıdan özel kaynak.

Özel sağlayıcıyı PowerShell ile dağıtmak için şunu kullanın:

```azurepowershell-interactive
$rgName = "<resource-group-name>"
$funcName = "<function-app-name>"

New-AzResourceGroup -Name $rgName -Location eastus
New-AzResourceGroupDeployment -ResourceGroupName $rgName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/custom-providers/customprovider.json `
  -funcname $funcName
```

Ya da çözümü aşağıdaki düğmeyle dağıtabilirsiniz:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-docs-json-samples%2Fmaster%2Fcustom-providers%2Fcustomprovider.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="view-custom-provider-and-resource"></a>Özel sağlayıcıyı ve kaynağı görüntüle

Portalda, özel sağlayıcı gizli bir kaynak türüdür. Kaynak sağlayıcının dağıtıldığını doğrulamak için kaynak grubuna gidin. **Gizli türleri gösterme**seçeneğini belirleyin.

![Gizli kaynak türlerini göster](./media/create-custom-provider/show-hidden.png)

Dağıttığınız özel kaynak türünü görmek için, kaynak türü üzerinde GET işlemini kullanın.

```
GET https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users?api-version=2018-09-01-preview
```

ARMClient ile şunu kullanın:

```powershell
$subID = (Get-AzContext).Subscription.Id
$requestURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users?api-version=2018-09-01-preview"

armclient GET $requestURI
```

Yanıtı alırsınız:

```json
{
  "value": [
    {
      "properties": {
        "provisioningState": "Succeeded",
        "FullName": "Santa Claus",
        "Location": "NorthPole"
      },
      "id": "/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/santa",
      "name": "santa",
      "type": "Microsoft.CustomProviders/resourceProviders/users"
    }
  ]
}
```

## <a name="call-action"></a>Çağrı eylemi

Özel sağlayıcıınızda **ping**adlı bir eylem de vardır. İsteği işleyen kod, işlev uygulamasında uygulanır. Ping eylemi bir selamlama ile yanıt verir.

Bir ping isteği göndermek için özel sağlayıcıınızda POST işlemini kullanın.

```
POST https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/ping?api-version=2018-09-01-preview
```

ARMClient ile şunu kullanın:

```powershell
$pingURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/ping?api-version=2018-09-01-preview"

armclient POST $pingURI
```

Yanıtı alırsınız:

```json
{
  "pingcontent": {
    "source": "<function-name>.azurewebsites.net"
  },
  "message": "hello <function-name>.azurewebsites.net"
}
```

## <a name="create-resource-type"></a>Kaynak türü oluştur

Özel kaynak türünü oluşturmak için, kaynağı bir şablonda dağıtabilirsiniz. Bu yaklaşım, bu hızlı başlangıçta dağıttığınız şablonda gösterilmiştir. Kaynak türü için de bir PUT isteği gönderebilirsiniz.

```
PUT https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/<resource-name>?api-version=2018-09-01-preview

{"properties":{"FullName": "Test User", "Location": "Earth"}}
```

ARMClient ile şunu kullanın:

```powershell
$addURI = "https://management.azure.com/subscriptions/$subID/resourceGroups/$rgName/providers/Microsoft.CustomProviders/resourceProviders/$funcName/users/testuser?api-version=2018-09-01-preview"
$requestBody = "{'properties':{'FullName': 'Test User', 'Location': 'Earth'}}"

armclient PUT $addURI $requestBody
```

Yanıtı alırsınız:

```json
{
  "properties": {
    "provisioningState": "Succeeded",
    "FullName": "Test User",
    "Location": "Earth"
  },
  "id": "/subscriptions/<sub-ID>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/testuser",
  "name": "testuser",
  "type": "Microsoft.CustomProviders/resourceProviders/users"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Özel sağlayıcılara giriş için bkz. [Azure özel sağlayıcılar önizlemeye genel bakış](overview.md).
