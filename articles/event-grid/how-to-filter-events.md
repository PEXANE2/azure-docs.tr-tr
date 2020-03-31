---
title: Azure Etkinlik Ağıtı için etkinlikler filtreleme
description: Bu makalede, Olay Izgara aboneliği oluşturulurken olayların (olay türüne, konuya göre, operatörlere ve verilere göre vb.) nasıl filtreuygulanılanın caizdir.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 63a5cdbff79af52d9f96cf410a820c6cfc530066
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454032"
---
# <a name="filter-events-for-event-grid"></a>Olay Izgarası için filtre olayları

Bu makalede, Olay Izgara aboneliği oluşturulurken olaylar nasıl filtrelenir. Olay filtreleme seçenekleri hakkında bilgi edinmek için Olay [Ağı abonelikleri için olay filtrelemeyi anlayın'a](event-filtering.md)bakın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="filter-by-event-type"></a>Olay türüne göre filtreleme

Olay Izgara aboneliği oluştururken, bitiş noktasına hangi [olay türlerinin](event-schema.md) gönderilecek lerini belirtebilirsiniz. Bu bölümdeki örnekler, bir kaynak grubu için olay abonelikleri `Microsoft.Resources.ResourceWriteFailure` oluşturur, ancak gönderilen olayları sınırlar ve `Microsoft.Resources.ResourceWriteSuccess`. Olayları olay türlerine göre filtrelerken daha fazla esnekliğe ihtiyacınız varsa, gelişmiş işleçlere ve veri alanlarına göre filtreleme konusuna bakın.

PowerShell için, `-IncludedEventType` aboneliği oluştururken parametreyi kullanın.

```powershell
$includedEventTypes = "Microsoft.Resources.ResourceWriteFailure", "Microsoft.Resources.ResourceWriteSuccess"

New-AzEventGridSubscription `
  -EventSubscriptionName demoSubToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Endpoint <endpoint-URL> `
  -IncludedEventType $includedEventTypes
```

Azure CLI için `--included-event-types` parametreyi kullanın. Aşağıdaki örnekte, Bir Bash kabuğunda Azure CLI kullanır:

```azurecli
includedEventTypes="Microsoft.Resources.ResourceWriteFailure Microsoft.Resources.ResourceWriteSuccess"

az eventgrid event-subscription create \
  --name demoSubToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --included-event-types $includedEventTypes
```

Kaynak Yöneticisi şablonu için `includedEventTypes` özelliği kullanın.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [
          "Microsoft.Resources.ResourceWriteFailure",
          "Microsoft.Resources.ResourceWriteSuccess"
        ]
      }
    }
  }
]
```

## <a name="filter-by-subject"></a>Konuya göre filtreleme

Olay verilerindeki olayları konuya göre filtreleyebilirsiniz. Konunun başlangıcı veya sonu için eşleşecek bir değer belirtebilirsiniz. Olayları konuya göre filtrelerken daha fazla esnekliğe ihtiyacınız varsa, gelişmiş işleçler ve veri alanlarına göre Filtreleme konusuna bakın.

Aşağıdaki PowerShell örneğinde, konunun başına göre filtreleyen bir olay aboneliği oluşturursunuz. Olayları belirli `-SubjectBeginsWith` bir kaynak için olanolaylarla sınırlamak için parametreyi kullanırsınız. Bir ağ güvenlik grubunun kaynak kimliğini geçersiniz.

```powershell
$resourceId = (Get-AzResource -ResourceName demoSecurityGroup -ResourceGroupName myResourceGroup).ResourceId

New-AzEventGridSubscription `
  -Endpoint <endpoint-URL> `
  -EventSubscriptionName demoSubscriptionToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -SubjectBeginsWith $resourceId
```

Sonraki PowerShell örneği, bir blob depolama için abonelik oluşturur. Olayları, bir konu yla sınırlandıran `.jpg`olaylar.

```powershell
$storageId = (Get-AzStorageAccount -ResourceGroupName myResourceGroup -AccountName $storageName).Id

New-AzEventGridSubscription `
  -EventSubscriptionName demoSubToStorage `
  -Endpoint <endpoint-URL> `
  -ResourceId $storageId `
  -SubjectEndsWith ".jpg"
```

Aşağıdaki Azure CLI örneğinde, konunun başına göre filtreleyen bir olay aboneliği oluşturursunuz. Olayları belirli `--subject-begins-with` bir kaynak için olanolaylarla sınırlamak için parametreyi kullanırsınız. Bir ağ güvenlik grubunun kaynak kimliğini geçersiniz.

```azurecli
resourceId=$(az resource show --name demoSecurityGroup --resource-group myResourceGroup --resource-type Microsoft.Network/networkSecurityGroups --query id --output tsv)

az eventgrid event-subscription create \
  --name demoSubscriptionToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --subject-begins-with $resourceId
```

Sonraki Azure CLI örneği, bir blob depolama alanı için abonelik oluşturur. Olayları, bir konu yla sınırlandıran `.jpg`olaylar.

```azurecli
storageid=$(az storage account show --name $storageName --resource-group myResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name demoSubToStorage \
  --endpoint <endpoint-URL> \
  --subject-ends-with ".jpg"
```

Aşağıdaki Kaynak Yöneticisi şablonu örneğinde, konunun başına göre filtreleyen bir olay aboneliği oluşturursunuz. Belirli bir `subjectBeginsWith` kaynak için olayları sınırlamak için özelliği kullanırsınız. Bir ağ güvenlik grubunun kaynak kimliğini geçersiniz.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "[resourceId('Microsoft.Network/networkSecurityGroups','demoSecurityGroup')]",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

Sonraki Kaynak Yöneticisi şablonu örneği, bir blob depolama alanı için abonelik oluşturur. Olayları, bir konu yla sınırlandıran `.jpg`olaylar.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
    "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/', parameters('eventSubName'))]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectEndsWith": ".jpg",
        "subjectBeginsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

## <a name="filter-by-operators-and-data"></a>Operatörler ve verilere göre filtreleme

Filtrelemede daha fazla esneklik için, olayları filtrelemek için işleçleri ve veri özelliklerini kullanabilirsiniz.

### <a name="subscribe-with-advanced-filters"></a>Gelişmiş filtrelerle abone olun

Gelişmiş filtreleme için kullanabileceğiniz işleçler ve anahtarlar hakkında bilgi edinmek için [Gelişmiş filtreleme'ye](event-filtering.md#advanced-filtering)bakın.

Bu örnekler özel bir konu oluşturur. Özel konuya abone olurlar ve veri nesnesindeki bir değere göre filtrelenirler. Renk özelliği mavi, kırmızı veya yeşil olarak ayarlanmış olaylar aboneliğe gönderilir.

Azure CLI için şunu kullanın:

```azurecli
topicName=<your-topic-name>
endpointURL=<endpoint-URL>

az group create -n gridResourceGroup -l eastus2
az eventgrid topic create --name $topicName -l eastus2 -g gridResourceGroup

topicid=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  -n demoAdvancedSub \
  --advanced-filter data.color stringin blue red green \
  --endpoint $endpointURL \
  --expiration-date "<yyyy-mm-dd>"
```

Abonelik için bir [sona erme tarihi](concepts.md#event-subscription-expiration) belirlendiğine dikkat edin.

PowerShell için şunu kullanın:

```powershell
$topicName = <your-topic-name>
$endpointURL = <endpoint-URL>

New-AzResourceGroup -Name gridResourceGroup -Location eastus2
New-AzEventGridTopic -ResourceGroupName gridResourceGroup -Location eastus2 -Name $topicName

$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name $topicName).Id

$expDate = '<mm/dd/yyyy hh:mm:ss>' | Get-Date
$AdvFilter1=@{operator="StringIn"; key="Data.color"; Values=@('blue', 'red', 'green')}

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint $endpointURL `
  -ExpirationDate $expDate `
  -AdvancedFilter @($AdvFilter1)
```

### <a name="test-filter"></a>Test filtresi

Filtreyi sınamak için, renk alanı yeşile ayarlanmış bir olay gönderin. Yeşil filtredeki değerlerden biri olduğundan, olay bitiş noktasına teslim edilir.

Azure CLI için şunu kullanın:

```azurecli
topicEndpoint=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name $topicName -g gridResourceGroup --query "key1" --output tsv)

event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "green"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```

PowerShell için şunu kullanın:

```powershell
$endpoint = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name $topicName).Endpoint
$keys = Get-AzEventGridTopicKey -ResourceGroupName gridResourceGroup -Name $topicName

$eventID = Get-Random 99999
$eventDate = Get-Date -Format s

$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/cars"
    eventTime= $eventDate
    data= @{
        model="SUV"
        color="green"
    }
    dataVersion="1.0"
}

$body = "["+(ConvertTo-Json $htbody)+"]"

Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Olayın gönderilmediği bir senaryoyu sınamak için renk alanı kümesini sarıya doğru bir olay gönderin. Sarı, abonelikte belirtilen değerlerden biri olmadığından, etkinlik aboneliğinize teslim edilmemiştür.

Azure CLI için şunu kullanın:

```azurecli
event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "yellow"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```
PowerShell için şunu kullanın:

```powershell
$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/cars"
    eventTime= $eventDate
    data= @{
        model="SUV"
        color="yellow"
    }
    dataVersion="1.0"
}

$body = "["+(ConvertTo-Json $htbody)+"]"

Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimlerini izleme hakkında daha fazla bilgi için [Bkz.](monitor-event-delivery.md)
* Kimlik doğrulama anahtarı hakkında daha fazla bilgi için [Olay Izgara güvenliği ve kimlik doğrulama](security-authentication.md)sı'na bakın.
* Azure Olay Ağı aboneliği oluşturma hakkında daha fazla bilgi için [Olay Ağı abonelik şemasına](subscription-creation-schema.md)bakın.
