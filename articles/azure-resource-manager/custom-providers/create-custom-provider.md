---
title: Kaynak sağlayıcısı oluşturma
description: Kaynak sağlayıcısının nasıl oluşturulup özel kaynak türlerinin nasıl dağıtılancaya yapılacağını açıklar.
author: MSEvanhi
ms.topic: tutorial
ms.date: 05/01/2019
ms.author: evanhi
ms.openlocfilehash: 393993a44c860525b9bd9a540ed7afff78e5b93c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75649872"
---
# <a name="quickstart-create-custom-provider-and-deploy-custom-resources"></a>Hızlı başlatma: Özel sağlayıcı oluşturun ve özel kaynakları dağıtın

Bu hızlı başlatmada, kendi kaynak sağlayıcınızı oluşturur ve bu kaynak sağlayıcısı için özel kaynak türlerini dağıtın. Özel sağlayıcılar hakkında daha fazla bilgi için Azure [Özel Sağlayıcıları Önizleme genel görünümüne](overview.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç adımlarını tamamlamak için REST işlemlerini aramanız gerekir. REST [isteklerini göndermenin farklı yolları](/rest/api/azure/)vardır. REST işlemleri için zaten bir aracınız yoksa [ARMClient'ı](https://github.com/projectkudu/ARMClient)yükleyin. Azure Kaynak Yöneticisi API'sini başlatmayı kolaylaştıran açık kaynak kodlu bir komut satırı aracıdır.

## <a name="deploy-custom-provider"></a>Özel sağlayıcı dağıtma

Özel sağlayıcıyı ayarlamak için Azure aboneliğinize bir [örnek şablon](https://github.com/Azure/azure-docs-json-samples/blob/master/custom-providers/customprovider.json) dağıtın.

Şablonu dağıttıktan sonra aboneliğiniz aşağıdaki kaynaklara sahiptir:

* Kaynaklar ve eylemler için işlemler ile İşlev Uygulaması.
* Özel sağlayıcı aracılığıyla oluşturulan kullanıcıları depolamak için Depolama Hesabı.
* Özel kaynak türlerini ve eylemleri tanımlayan Özel Sağlayıcı. İstek göndermek için işlev uygulaması bitiş noktasını kullanır.
* Özel sağlayıcıdan özel kaynak.

PowerShell ile özel sağlayıcı dağıtmak için kullanın:

```azurepowershell-interactive
$rgName = "<resource-group-name>"
$funcName = "<function-app-name>"

New-AzResourceGroup -Name $rgName -Location eastus
New-AzResourceGroupDeployment -ResourceGroupName $rgName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/custom-providers/customprovider.json `
  -funcname $funcName
```

Veya aşağıdaki düğmeyle çözümü dağıtabilirsiniz:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-docs-json-samples%2Fmaster%2Fcustom-providers%2Fcustomprovider.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="view-custom-provider-and-resource"></a>Özel sağlayıcıyı ve kaynağı görüntüleme

Portalda, özel sağlayıcı gizli bir kaynak türüdür. Kaynak sağlayıcısının dağıtıldığını doğrulamak için kaynak grubuna gidin. Gizli türleri **gösterme**seçeneğini seçin.

![Gizli kaynak türlerini göster](./media/create-custom-provider/show-hidden.png)

Dağıttığınız özel kaynak türünü görmek için kaynak türünüzdeki GET işlemini kullanın.

```
GET https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users?api-version=2018-09-01-preview
```

ARMClient ile şunları kullanın:

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

## <a name="call-action"></a>Arama eylemi

Özel sağlayıcınız da **ping**adlı bir eylem vardır. İsteği işleyen kod işlev uygulamasında uygulanır. Ping eylemi bir karşılama ile yanıtlar.

Ping isteği göndermek için, özel sağlayıcınızda POST işlemini kullanın.

```
POST https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/ping?api-version=2018-09-01-preview
```

ARMClient ile şunları kullanın:

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

## <a name="create-resource-type"></a>Kaynak türü oluşturma

Özel kaynak türünü oluşturmak için, kaynağı bir şablonda dağıtabilirsiniz. Bu yaklaşım, bu hızlı başlangıçta dağıttığınız şablonda gösterilir. Ayrıca kaynak türü için bir PUT isteği gönderebilirsiniz.

```
PUT https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<rg-name>/providers/Microsoft.CustomProviders/resourceProviders/<provider-name>/users/<resource-name>?api-version=2018-09-01-preview

{"properties":{"FullName": "Test User", "Location": "Earth"}}
```

ARMClient ile şunları kullanın:

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

Özel sağlayıcılara giriş için Azure [Özel Sağlayıcıları Önizleme genel görünümüne](overview.md)bakın.
