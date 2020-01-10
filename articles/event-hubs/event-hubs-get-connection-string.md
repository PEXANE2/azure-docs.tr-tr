---
title: Azure Event Hubs bağlantı dizesi - alın | Microsoft Docs
description: Bu makalede, istemcilerin Azure Event Hubs'a bağlanmak için kullanabileceği bir bağlantı dizesi alma yönergeleri sağlanır.
services: event-hubs
documentationcenter: na
author: spelluru
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: 744151a1ce8cde630e26c17ccf06569ebd0efb61
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771011"
---
# <a name="get-an-event-hubs-connection-string"></a>Bir Event hubs'ı bağlantı dizesini alma

Event Hubs'ı kullanmak için bir Event Hubs ad alanı oluşturmanız gerekir. Ad alanı, birden fazla olay hub 'ı veya Kafka konu başlığı için kapsam kapsayıcısı olur. Bu ad benzersiz bir veren [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name). Bir ad alanı oluşturduktan sonra Event Hubs ile iletişim kurmak için gerekli bağlantı dizesini edinebilirsiniz.

Azure Event Hubs için bağlantı dizesi içinde gömülü aşağıdaki bileşenlere sahiptir,

* FQDN = oluşturduğunuz EventHubs ad alanının FQDN 'SI (EventHubs ad alanı adını ve ardından servicebus.windows.net) içerir
* SharedAccessKeyName, uygulamanızın SAS anahtarları için seçtiğiniz adı =
* SharedAccessKey oluşturulan anahtar değerini =.

Bağlantı dizesi şablonu şu şekilde görünür
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Örnek bir bağlantı dizesi aşağıdaki gibi görünmelidir `Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

Bu makalede, bağlantı dizesini edinme çeşitli şekillerde sizi yönlendirir.

## <a name="get-connection-string-from-the-portal"></a>Portaldan bağlantı dizesini alma
1. [Azure portalda](https://portal.azure.com) oturum açın. 
2. Sol gezinti menüsünde **tüm hizmetler** ' i seçin. 
3. **Analiz** bölümünde **Event Hubs** ' yi seçin. 
4. Olay Hub 'ları listesinde, Olay Hub 'ınızı seçin.
6. **Event Hubs ad alanı** sayfasında sol menüdeki **paylaşılan erişim ilkeleri** ' ni seçin.

    ![Paylaşılan erişim Ilkeleri menü öğesi](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)
7. İlke listesinde bir **paylaşılan erişim ilkesi** seçin. Varsayılan bir ad: **RootManageSharedAccessPolicy**. Uygun izinlere sahip bir ilke ekleyebilirsiniz (okuma, yazma) ve bu ilkeyi kullanabilirsiniz. 

    ![Event Hubs paylaşılan erişim ilkeleri](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)
8. **Bağlantı dizesi-birincil anahtar** alanının yanındaki **Kopyala** düğmesini seçin. 

    ![Event Hubs-bağlantı dizesi al](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Azure PowerShell ile bağlantı dizesi alınıyor

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıda gösterildiği gibi, belirli bir ilke/kural adı için bağlantı dizesini almak üzere [Get-AzEventHubNamespaceKey](/powershell/module/az.eventhub/get-azeventhubkey) kullanabilirsiniz:

```azurepowershell-interactive
Get-AzEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

## <a name="getting-the-connection-string-with-azure-cli"></a>Azure CLI ile bağlantı dizesi alınıyor
Ad alanı için bağlantı dizesini almak için aşağıdakileri kullanabilirsiniz:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Ya da bir EventHub varlığına yönelik bağlantı dizesini almak için aşağıdakileri kullanabilirsiniz:

```azurecli-interactive
az eventhubs eventhub authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --eventhub-name dummyeventhub --name RootManageSharedAccessKey
```

Event Hubs için Azure CLı komutları hakkında daha fazla bilgi için bkz. [Azure clı Event Hubs](/cli/azure/eventhubs).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
