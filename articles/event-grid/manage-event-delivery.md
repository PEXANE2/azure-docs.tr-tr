---
title: Ölü harf ve yeniden deneme ilkeleri - Azure Etkinlik Ağıt
description: Olay Izgarası için olay teslim seçeneklerini nasıl özelleştirişdirilenaçıklar. Ölü harfli bir hedef ayarlayın ve teslimatı yeniden denemeniz in ne kadar süreceğini belirtin.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/22/2019
ms.author: spelluru
ms.openlocfilehash: caed3c077b4df5da5fd8541b2f7e85ef119604b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72794031"
---
# <a name="dead-letter-and-retry-policies"></a>Ölü harf ve yeniden deneme politikaları

Etkinlik aboneliği oluştururken, etkinlik teslimi için ayarları özelleştirebilirsiniz. Bu makalede, ölü bir harf konumunu nasıl ayarlayabileceğinizi ve yeniden deneme ayarlarını nasıl özelleştirdiğinizi gösterir. Bu özellikler hakkında daha fazla bilgi için [Olay Izgara iletisi teslimi ve yeniden deneyin' e](delivery-and-retry.md)bakın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="set-dead-letter-location"></a>Ölü harf konumunu ayarlama

Ölü bir harf konumu ayarlamak için, bitiş noktasına teslim edilemeyecek olayları tutmak için bir depolama hesabı gerekir. Örnekler, varolan bir depolama hesabının kaynak kimliğini alır. Bunlar, çıkmaz harf bitiş noktası için bu depolama hesabında bir kapsayıcı kullanan bir olay aboneliği oluştururlar.

> [!NOTE]
> - Bu makalede komutları çalıştırmadan önce depolama alanında bir depolama hesabı ve bir blob kapsayıcıoluşturun.
> - Olay Izgara hizmeti bu kapsayıcıda lekeler oluşturur. Blobs adları büyük harftüm harfleri ile Olay Izgara abonelik adı olacaktır. Örneğin, aboneliğin adı My-Blob-Subscription ise, ölü harf blobs isimleri MY-BLOB-SUBSCRIPTION (myblobcontainer/MY-BLOB-SUBSCRIPTION/2019/8/8/5/1111111-1111-1111111111111111111111111111111111111111111111111111.json) olacaktır. Bu davranış, Azure hizmetleri arasındaki servis talebi işleme farklılıklarına karşı korumakiçindir.


### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
containername=testcontainer

topicid=$(az eventgrid topic show --name demoTopic -g gridResourceGroup --query id --output tsv)
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Ölü harfleri kapatmak için, olay aboneliği oluşturmak için komutu yeniden çalıştırın, ancak '' için `deadletter-endpoint`bir değer sağlamayın. Etkinlik aboneliğini silmeniz gerekmez.

> [!NOTE]
> Azure CLI'yi yerel makinenizde kullanıyorsanız, Azure CLI sürüm 2.0.56 veya üzeri kullanın. Azure CLI'nin en son sürümünü yükleme yle ilgili talimatlar [için](/cli/azure/install-azure-cli)bkz.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
$containername = "testcontainer"

$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

Ölü harfleri kapatmak için, olay aboneliği oluşturmak için komutu yeniden çalıştırın, ancak '' için `DeadLetterEndpoint`bir değer sağlamayın. Etkinlik aboneliğini silmeniz gerekmez.

> [!NOTE]
> Azure Poweshell'i yerel makinenizde kullanıyorsanız, Azure PowerShell sürüm 1.1.0 veya daha büyük kullanın. [Azure yüklemelerinden](https://azure.microsoft.com/downloads/)en son Azure PowerShell'i indirin ve yükleyin.

## <a name="set-retry-policy"></a>Yeniden deneme ilkesini ayarlama

Olay Izgara aboneliği oluştururken, Olay Izgara'sının olayı gerçekleştirmeyi ne kadar süre denemesi gerektiğine göre değerleri ayarlayabilirsiniz. Varsayılan olarak, Olay Grid 24 saat (1440 dakika) veya 30 kez çalışır. Olay ağı aboneliğiniz için bu değerlerden birini ayarlayabilirsiniz. Etkinlik zamanının canlı değeri 1'den 1440'a kadar bir arasayı olmalıdır. Maksimum yeniden denemeleriçin değer 1 ile 30 arasında bir arayıcı olmalıdır.

[Yeniden deneme zamanlamasını](delivery-and-retry.md#retry-schedule-and-duration)yapılandıramazsınız.

### <a name="azure-cli"></a>Azure CLI

Etkinlik süresini 1440 dakikadan başka bir değere ayarlamak için şunları kullanın:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

Maksimum yeniden denemelerini 30'dan başka bir değere ayarlamak için şunları kullanın:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Her ikisini `event-ttl` de `max-deliver-attempts`ayarlarsanız ve Olay Grid olay teslimini ne zaman durduracaklarını belirlemek için ilk inin süresini sonvermek için kullanır.

### <a name="powershell"></a>PowerShell

Etkinlik süresini 1440 dakikadan başka bir değere ayarlamak için şunları kullanın:

```azurepowershell-interactive
$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

Maksimum yeniden denemelerini 30'dan başka bir değere ayarlamak için şunları kullanın:

```azurepowershell-interactive
$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -MaxDeliveryAttempt 18
```

Her ikisini `EventTtl` de `MaxDeliveryAttempt`ayarlarsanız ve Olay Grid olay teslimini ne zaman durduracaklarını belirlemek için ilk inin süresini sonvermek için kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Ölü harf olaylarını işlemek için bir Azure İşlevi uygulaması kullanan örnek bir uygulama [için .NET için Azure Olay Izgara Ölü Harf Örnekleri'ne](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/)bakın.
* Olay teslimi ve yeniden denemeleri hakkında bilgi için, [Olay Grid ileti teslimi ve yeniden deneme.](delivery-and-retry.md)
* Event Grid’e giriş için bkz. [Event Grid hakkında](overview.md).
* Olay Ağıt'ı kullanmaya hızla başlamak için [Azure Olay Ağıtı ile özel etkinlikler oluştur ve yönlendir'e](custom-event-quickstart.md)bakın.
