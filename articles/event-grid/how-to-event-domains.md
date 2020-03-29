---
title: Azure Olay Ağıtı ile etkinlik etki alanlarıyla etkinlik yayınlama
description: Azure Etkinlik Ağıt'ında büyük konu kümelerini nasıl yönetip etkinlik etki alanlarını kullanarak etkinlikleri onlara nasıl yayınlayacağımı gösterir.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/22/2019
ms.openlocfilehash: 1d07227249806b7d54523af66817a170c19354ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72786548"
---
# <a name="manage-topics-and-publish-events-using-event-domains"></a>Etkinlik etki alanlarını kullanarak konuları yönetme ve olayları yayımlama

Bu makalede, nasıl gösterilmektedir:

* Olay Izgara etki alanı oluşturma
* Olay ızgara konularına abone olun
* Liste tuşları
* Olayları bir etki alanına yayımlama

Olay etki alanları hakkında bilgi edinmek için Olay [Ağı konularını yönetmek için olay etki alanlarını anlayın'a](event-domains.md)bakın.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Önizleme özelliğini yükleme

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="create-an-event-domain"></a>Etkinlik Etki Alanı Oluşturma

Büyük konu kümelerini yönetmek için bir olay etki alanı oluşturun.

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

```azurecli-interactive
# If you haven't already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid domain create \
  -g <my-resource-group> \
  --name <my-domain-name> \
  -l <location>
```

# <a name="powershell"></a>[Powershell](#tab/powershell)
```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridDomain `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain-name> `
  -Location <location>
```
---

Başarılı oluşturma aşağıdaki değerleri döndürür:

```json
{
  "endpoint": "https://<my-domain-name>.westus2-1.eventgrid.azure.net/api/events",
  "id": "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>",
  "inputSchema": "EventGridSchema",
  "inputSchemaMapping": null,
  "location": "westus2",
  "name": "<my-domain-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "<my-resource-group>",
  "tags": null,
  "type": "Microsoft.EventGrid/domains"
}
```

Etki `endpoint` alanını `id` yönetmek ve olayları yayınlamak için gerekli olan ve gerekli olan bu nedenle dikkat edin.

## <a name="manage-access-to-topics"></a>Konulara erişimi yönetme

Konulara erişimin yönetilmesi [rol ataması](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)yoluyla yapılır. Rol ataması, Azure kaynaklarındaki işlemleri belirli bir kapsamdaki yetkili kullanıcılarla sınırlamak için rol tabanlı erişim denetimini kullanır.

Olay Izgara'nın, belirli kullanıcıların etki alanındaki çeşitli konularda ki erişimini atamak için kullanabileceğiniz iki yerleşik rolü vardır. Bu roller, aboneliklerin oluşturulmasına ve silinmesine izin `EventGrid EventSubscription Reader (Preview)`veren ve yalnızca olay aboneliklerinin listelenmesine izin veren rollerdir. `EventGrid EventSubscription Contributor (Preview)`

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)
Aşağıdaki Azure CLI `alice@contoso.com` komutu, olay aboneliklerini yalnızca konuyla `demotopic1`ilgili olarak oluşturmayı ve silmeyi sınırlar:

```azurecli-interactive
az role assignment create \
  --assignee alice@contoso.com \
  --role "EventGrid EventSubscription Contributor (Preview)" \
  --scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1
```

# <a name="powershell"></a>[Powershell](#tab/powershell)
Aşağıdaki PowerShell komutu, yalnızca konuyla ilgili `alice@contoso.com` `demotopic1`etkinlik abonelikleri oluşturmayı ve silmeyi sınırlar:

```azurepowershell-interactive
New-AzureRmRoleAssignment `
  -SignInName alice@contoso.com `
  -RoleDefinitionName "EventGrid EventSubscription Contributor (Preview)" `
  -Scope /subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1
```
---

Olay Izgara işlemleri için erişimi yönetme hakkında daha fazla bilgi için [Olay Ağı güvenliği ve kimlik doğrulama](./security-authentication.md)sı'na bakın.

## <a name="create-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri oluşturma

Olay Izgara hizmeti, bir etki alanı konusu için olay aboneliği oluşturmak için çağrıyı temel alarak bir etki alanında ilgili konuyu otomatik olarak oluşturur ve yönetir. Etki alanında bir konu oluşturmak için ayrı bir adım yoktur. Benzer şekilde, bir konuiçin son olay aboneliği silindiğinde, konu da silinir.

Bir etki alanında bir konuya abone olmak, başka bir Azure kaynağına abone olmakla aynıdır. Kaynak kaynak kimliği için, etki alanını daha önce oluştururken döndürülen olay etki alanı kimliğini belirtin. Abone olmak istediğiniz konuyu belirtmek için `/topics/<my-topic>` kaynak kaynak kimliğinin sonuna ekleyin. Etki alanında tüm olayları alan bir etki alanı kapsamı olay aboneliği oluşturmak için, herhangi bir konu belirtmeden olay etki alanı kimliğini belirtin.

Genellikle, önceki bölümde erişim izni vermiş olduğunuz kullanıcı aboneliği oluşturur. Bu makaleyi basitleştirmek için aboneliği oluşturursunuz. 

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

```azurecli-interactive
az eventgrid event-subscription create \
  --name <event-subscription> \
  --source-resource-id "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1" \
  --endpoint https://contoso.azurewebsites.net/api/updates
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

```azurepowershell-interactive
New-AzureRmEventGridSubscription `
  -ResourceId "/subscriptions/<sub-id>/resourceGroups/<my-resource-group>/providers/Microsoft.EventGrid/domains/<my-domain-name>/topics/demotopic1" `
  -EventSubscriptionName <event-subscription> `
  -Endpoint https://contoso.azurewebsites.net/api/updates
```

---

Etkinliklerinize abone olmak için bir test bitiş noktasına ihtiyacınız varsa, her zaman gelen olayları görüntüleyen önceden oluşturulmuş bir [web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) dağıtabilirsiniz. Etkinliklerinizi test web sitenize `https://<your-site-name>.azurewebsites.net/api/updates`gönderebilirsiniz.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

Bir konu için ayarlanan izinler Azure Etkin Dizini'nde depolanır ve açıkça silinmesi gerekir. Etkinlik aboneliğini silmek, bir konu yla ilgili yazma erişimi varsa, kullanıcının etkinlik abonelikleri oluşturmak için erişimini iptal etmez.


## <a name="publish-events-to-an-event-grid-domain"></a>Olayları Bir Olay Izgara Etki Alanına yayımlama

Olayları bir etki alanına yayımlamak, [özel bir konuya yayımlamakla](./post-to-custom-topic.md)aynıdır. Ancak, özel konuya yayımlamak yerine, tüm olayları etki alanı bitiş noktasına yayımlarsınız. JSON olay verilerinde, olayların gitmesini istediğiniz konuyu belirtirsiniz. Aşağıdaki olaylar `"id": "1111"` dizisi, konuya `demotopic1` ile olay la `"id": "2222"` sonuçlanırken, olay `demotopic2`konusuna gönderilir:

```json
[{
  "topic": "demotopic1",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "demotopic2",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)
Azure CLI ile etki alanı bitiş noktasını elde etmek için

```azurecli-interactive
az eventgrid domain show \
  -g <my-resource-group> \
  -n <my-domain>
```

Bir etki alanının anahtarlarını almak için şunları kullanın:

```azurecli-interactive
az eventgrid domain key list \
  -g <my-resource-group> \
  -n <my-domain>
```

# <a name="powershell"></a>[Powershell](#tab/powershell)
PowerShell ile etki alanı bitiş noktasını elde etmek için

```azurepowershell-interactive
Get-AzureRmEventGridDomain `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain>
```

Bir etki alanının anahtarlarını almak için şunları kullanın:

```azurepowershell-interactive
Get-AzureRmEventGridDomainKey `
  -ResourceGroupName <my-resource-group> `
  -Name <my-domain>
```
---

Ardından, etkinliklerinizi Olay Izgara etki alanınızda yayınlamak için en sevdiğiniz http POST yöntemini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Olay etki alanlarındaki üst düzey kavramlar ve bunların neden yararlı olduğu hakkında daha fazla bilgi için [Olay Etki Alanları'nın kavramsal genel görünümüne](event-domains.md)bakın.
