---
title: Özel alan Azure Event Grid şemaya eşleme
description: Bu makalede, olay veriniz Event Grid şemayla eşleşmediği zaman özel şemanızın Azure Event Grid şemasına nasıl dönüştürüleceği açıklanır.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: spelluru
ms.openlocfilehash: e8077068a265d659cf6009eb7762188637c373d6
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721668"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Özel alanları Event Grid şemasına eşleme

Olay verileriniz beklenen [Event Grid şemasıyla](event-schema.md)eşleşmiyorsa, etkinliği abonelere yönlendirmek için Event Grid kullanmaya devam edebilirsiniz. Bu makalede, şemanızı Event Grid şemaya eşleme açıklar.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Önizleme özelliğini yükle

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Özgün olay şeması

Şu biçimde olayları gönderen bir uygulamaya sahip varsayalım:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Bu biçim gerekli şema eşleşmiyor olsa da, Event Grid, alanları şemaya eşleme sağlar. Veya, özgün şemada değerleri alabilir.

## <a name="create-custom-topic-with-mapped-fields"></a>Eşlenen alanlar ile özel konu oluşturma

Bir özel konu oluşturma sırasında özgün olay alanlarından event grid şemaya eşleme belirtin. Eşleme özelleştirmek için kullandığınız üç değer vardır:

* **Giriş şeması** değeri şemanın türünü belirtir. Mevcut seçenekler CloudEvents şeması, özel olay Şeması veya Event Grid şema'dir. Event Grid şema varsayılan değerdir. Özel eşleme şemanızı ve event grid şema arasında oluştururken, özel olay şeması kullanın. Olayları CloudEvents şeması içinde olduğunda Cloudevents şeması kullanma.

* **Mapping default Values** özelliği Event Grid şemasındaki alanlar için varsayılan değerleri belirtir. `subject`, `eventtype`ve `dataversion`için varsayılan değerleri ayarlayabilirsiniz. Genellikle, bu üç alan birine karşılık gelen bir alan özel şemanızı içermediğinde bu parametreyi kullanın. Örneğin, veri sürümünün her zaman **1,0**olarak ayarlandığını belirtebilirsiniz.

* **Mapping Fields** değeri, şemadaki alanları Event Grid şemasına eşler. Değerler boşlukla ayrılmış bir anahtar/değer çiftlerini belirtin. Anahtar adı için event grid alanı adını kullanın. Değeri, alan adı kullanın. `id`, `topic`, `eventtime`, `subject`, `eventtype`ve `dataversion`için anahtar adlarını kullanabilirsiniz.

Azure CLI ile özel bir konu oluşturmak için kullanın:

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

## <a name="subscribe-to-event-grid-topic"></a>Event grid konuya abone olma

Özel konuya abone olurken olayları almak için kullanmak istediğiniz bir şema belirtin. CloudEvents şeması, özel olay Şeması veya Event Grid şema belirt Event Grid şema varsayılan değerdir.

Aşağıdaki örnek, bir event grid konuya abone olur ve Event Grid şemayı kullanır. Azure CLI için şunu kullanın:

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

Aşağıdaki örnek, bir event grid konuya abone olur ve Event Grid şemayı kullanır. PowerShell için şunu kullanın:

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

## <a name="publish-event-to-topic"></a>Konuya olayı Yayımla

Özel konuya bir olay göndermek ve eşlemenin sonucunu görmek artık hazırsınız. [Örnek şemasında](#original-event-schema)bir olay göndermek için aşağıdaki komut dosyası:

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

Şimdi Web kancası uç noktanızı arayın. İki aboneliğin farklı şemalarda olayları teslim.

İlk abonelik olay ızgarası şeması kullanılır. Teslim edilen olay biçimdir:

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

Bu alanlar özel konu başlığından eşlemeleri içerir. **Myeventtypefield** , **EventType**ile eşlendi. **Dataversion** ve **Subject** için varsayılan değerler kullanılır. **Veri** nesnesi, özgün olay şeması alanlarını içerir.

İkinci abonelik giriş olay şeması kullanılır. Teslim edilen olay biçimdir:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": {
    "someDataField1": "SomeDataFieldValue"
  }
}
```

Özgün alanları teslim dikkat edin.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimi ve yeniden denemeler hakkında daha fazla bilgi için [Event Grid ileti teslimi ve yeniden deneyin](delivery-and-retry.md).
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Event Grid kullanmaya hızlıca başlamak için bkz. [özel olayları oluşturma ve Azure Event Grid ile yönlendirme](custom-event-quickstart.md).
