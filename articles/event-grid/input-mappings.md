---
title: Azure Olay Izgara şemasına özel alanı haritalama
description: Bu makalede, etkinlik verileriniz Olay Izgara şemasıyla eşleşmediğinde özel şemanızı Azure Olay Izgara şemasına nasıl dönüştürdüğünüz açıklanmaktadır.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: e8077068a265d659cf6009eb7762188637c373d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721668"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Özel alanları Event Grid şemasına eşleme

Etkinlik verileriniz beklenen [Olay Izgara şemasıyla](event-schema.md)eşleşmiyorsa, olayı abonelere yönlendirmek için Olay Izgarasını kullanmaya devam edebilirsiniz. Bu makalede, şemanızın Olay Izgara şemasıyla nasıl eşleneniz açıklanmaktadır.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Önizleme özelliğini yükleme

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Orijinal olay şeması

Olayları aşağıdaki biçimde gönderen bir uygulamanız olduğunu varsayalım:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Bu biçim gerekli şemaya uymasa da, Olay Izgara alanlarınızı şema ile eşlebilmenizi sağlar. Veya özgün şemadaki değerleri alabilirsiniz.

## <a name="create-custom-topic-with-mapped-fields"></a>Eşlenen alanlar ile özel konu oluşturma

Özel bir konu oluştururken, özgün etkinliğinizdeki alanların olay ızgara şemasına nasıl eşlenerek eşlenerek yapılacağını belirtin. Eşlecilik özelleştirmek için kullandığınız üç değer vardır:

* **Giriş şeması** değeri şema türünü belirtir. Kullanılabilir seçenekler CloudEvents şeması, özel olay şeması veya Olay Izgara şemasıdır. Varsayılan değer Olay Izgara şemasıdır. Şemanız ve olay ızgara şeması arasında özel eşleme oluştururken, özel olay şeması kullanın. Olaylar CloudEvents şemasında olduğunda, CloudEvents şemasını kullanın.

* **Eşleme varsayılan değerleri** özelliği Olay Izgara şemasındaki alanlar için varsayılan değerleri belirtir. Varsayılan değerleri `subject`, ve `eventtype`. `dataversion` Genellikle, özel şemanız bu üç alandan birine karşılık gelen bir alan içermiyorsa, bu parametreyi kullanırsınız. Örneğin, veri sürümünün her zaman **1.0**olarak ayarlanır belirtebilirsiniz.

* **Eşleme alanları,** şemanızdan olay ızgara şemasına kadar olan alanları değerlendirir. Boşluk ayrılmış anahtar/değer çiftlerinde değerleri belirtirsiniz. Anahtar adı için olay ızgara alanının adını kullanın. Değer için alanınızın adını kullanın. Anahtar `id`adlarını , `topic`, `eventtime` `subject`, `eventtype`, `dataversion`ve .

Azure CLI ile özel bir konu oluşturmak için şunları kullanın:

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema \
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName myResourceGroup `
  -Name demotopic `
  -Location eastus2 `
  -InputSchema CustomEventSchema `
  -InputMappingField @{eventType="myEventTypeField"} `
  -InputMappingDefaultValue @{subject="DefaultSubject"; dataVersion="1.0" }
```

## <a name="subscribe-to-event-grid-topic"></a>Olay ızgarası konusuna abone olun

Özel konuya abone olurken, olayları almak için kullanmak istediğiniz şemayı belirtirsiniz. CloudEvents şeasını, özel olay şemasını veya Olay Izgara şeasını belirtirsiniz. Varsayılan değer Olay Izgara şemasıdır.

Aşağıdaki örnek, bir olay ızgarası konusuna abone dir ve Olay Izgara şemasını kullanır. Azure CLI için şunu kullanın:

```azurecli-interactive
topicid=$(az eventgrid topic show --name demoTopic -g myResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name eventsub1 \
  --event-delivery-schema eventgridschema \
  --endpoint <endpoint_URL>
```

Sonraki örnek, olayın giriş şemasını kullanır:

```azurecli-interactive
az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name eventsub2 \
  --event-delivery-schema custominputschema \
  --endpoint <endpoint_URL>
```

Aşağıdaki örnek, bir olay ızgarası konusuna abone dir ve Olay Izgara şemasını kullanır. PowerShell için şunu kullanın:

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName myResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName eventsub1 `
  -EndpointType webhook `
  -Endpoint <endpoint-url> `
  -DeliverySchema EventGridSchema
```

Sonraki örnek, olayın giriş şemasını kullanır:

```azurepowershell-interactive
New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName eventsub2 `
  -EndpointType webhook `
  -Endpoint <endpoint-url> `
  -DeliverySchema CustomInputSchema
```

## <a name="publish-event-to-topic"></a>Olayı konuya yayımlama

Artık özel konuya bir olay göndermeye ve eşlemenin sonucunu görmeye hazırsınız. [Örnek şemada](#original-event-schema)bir olay göndermek için aşağıdaki komut dosyası:

Azure CLI için şunu kullanın:

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

event='[ { "myEventTypeField":"Created", "resource":"Users/example/Messages/1000", "resourceData":{"someDataField1":"SomeDataFieldValue"} } ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
$endpoint = (Get-AzureRmEventGridTopic -ResourceGroupName myResourceGroup -Name demotopic).Endpoint
$keys = Get-AzureRmEventGridTopicKey -ResourceGroupName myResourceGroup -Name demotopic

$htbody = @{
    myEventTypeField="Created"
    resource="Users/example/Messages/1000"
    resourceData= @{
        someDataField1="SomeDataFieldValue"
    }
}

$body = "["+(ConvertTo-Json $htbody)+"]"
Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Şimdi, WebHook bitiş noktasına bakın. İki abonelik farklı şemalarda etkinlikler sundu.

İlk abonelik olay ızgara şeması kullanılır. Teslim edilen olayın biçimi:

```json
{
  "id": "aa5b8e2a-1235-4032-be8f-5223395b9eae",
  "eventTime": "2018-11-07T23:59:14.7997564Z",
  "eventType": "Created",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.EventGrid/topics/demotopic",
  "subject": "DefaultSubject",
  "data": {
    "myEventTypeField": "Created",
    "resource": "Users/example/Messages/1000",
    "resourceData": {
      "someDataField1": "SomeDataFieldValue"
    }
  }
}
```

Bu alanlar, özel konueşlerini içerir. **myEventTypeField** **EventType**eşlenir. **DataVersion** ve **Subject** için varsayılan değerler kullanılır. **Veri** nesnesi özgün olay şeması alanlarını içerir.

İkinci abonelik giriş olay şeması kullanılır. Teslim edilen olayın biçimi:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": {
    "someDataField1": "SomeDataFieldValue"
  }
}
```

Orijinal alanların teslim edildiğine dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeleri hakkında bilgi için, [Olay Grid ileti teslimi ve yeniden deneme.](delivery-and-retry.md)
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
