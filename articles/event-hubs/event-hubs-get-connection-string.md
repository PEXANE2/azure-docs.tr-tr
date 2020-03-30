---
title: Bağlantı dizesi alın - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, istemcilerin Azure Olay Hub'larına bağlanmak için kullanabilecekleri bir bağlantı dizesi almak için yönergeler sağlanmaktadır.
services: event-hubs
documentationcenter: na
author: spelluru
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: 77a768f907ad989a457ee498f26ad0f6e004f786
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264940"
---
# <a name="get-an-event-hubs-connection-string"></a>Olay Hub'ları bağlantı dizesi alma

Olay Hub'larını kullanmak için bir Olay Hub'ları ad alanı oluşturmanız gerekir. Ad alanı, birden çok etkinlik merkezi veya Kafka konusu için bir kapsam kapsayıcısIdur. Bu ad alanı size benzersiz bir [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)verir. Ad alanı oluşturulduktan sonra, Olay Hub'ları ile iletişim kurmak için gereken bağlantı dizesini elde edebilirsiniz.

Azure Etkinlik Hub'ları için bağlantı dizesi, içinde gömülü aşağıdaki bileşenlere sahiptir,

* FQDN = Oluşturduğunuz EventHubs ad alanının FQDN'si (servicebus.windows.net ardından EventHubs ad alanı adını içerir)
* SharedAccessKeyName = uygulamanızın SAS tuşları için seçtiğiniz ad
* SharedAccessKey = anahtarın oluşturulan değeri.

Bağlantı dize şablonu aşağıdaki gibi görünür
```
Endpoint=sb://<FQDN>/;SharedAccessKeyName=<KeyName>;SharedAccessKey=<KeyValue>
```

Örnek bir bağlantı dizesi,`Endpoint=sb://dummynamespace.servicebus.windows.net/;SharedAccessKeyName=DummyAccessKeyName;SharedAccessKey=5dOntTRytoC24opYThisAsit3is2B+OGY1US/fuL3ly=`

Bu makalede, bağlantı dizesini elde etmenin çeşitli yolları size yol alabilirsiniz.

## <a name="get-connection-string-from-the-portal"></a>Portaldan bağlantı dizesini alma
1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Sol navigasyon menüsündeki **Tüm hizmetleri** seçin. 
3. **Analytics** bölümünde **Etkinlik Hub'larını** seçin. 
4. Olay merkezleri listesinde, etkinlik merkezinizi seçin.
6. Olay **Hub'ları Ad Alanı** sayfasında, sol menüde **Paylaşılan Erişim İlkeleri'ni** seçin.

    ![Paylaşılan Erişim İlkeleri menü öğesi](./media/event-hubs-get-connection-string/event-hubs-get-connection-string1.png)
7. İlkeler listesinde paylaşılan bir **erişim ilkesi** seçin. Varsayılan olanın adı: **RootManageSharedAccessPolicy**. Uygun izinlere (okuma, yazma) sahip bir ilke ekleyebilir ve bu ilkeyi kullanabilirsiniz. 

    ![Olay Hub'ları paylaşılan erişim ilkeleri](./media/event-hubs-get-connection-string/event-hubs-get-connection-string2.png)
8. **Bağlantı dizesi birincil anahtar** alanının yanındaki **kopya** düğmesini seçin. 

    ![Olay Hub'ları - bağlantı dizesi alın](./media/event-hubs-get-connection-string/event-hubs-get-connection-string3.png)

## <a name="getting-the-connection-string-with-azure-powershell"></a>Azure PowerShell ile bağlantı dizesini alma

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıda gösterildiği gibi belirli bir ilke/kural adı için bağlantı dizesini almak için [Get-AzEventHubKey'i](/powershell/module/az.eventhub/get-azeventhubkey) kullanabilirsiniz:

```azurepowershell-interactive
Get-AzEventHubKey -ResourceGroupName dummyresourcegroup -NamespaceName dummynamespace -AuthorizationRuleName RootManageSharedAccessKey
```

## <a name="getting-the-connection-string-with-azure-cli"></a>Azure CLI ile bağlantı dizesini alma
Ad alanı nın bağlantı dizesini almak için aşağıdakileri kullanabilirsiniz:

```azurecli-interactive
az eventhubs namespace authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --name RootManageSharedAccessKey
```

Veya bir EventHub varlığı için bağlantı dizesini almak için aşağıdakileri kullanabilirsiniz:

```azurecli-interactive
az eventhubs eventhub authorization-rule keys list --resource-group dummyresourcegroup --namespace-name dummynamespace --eventhub-name dummyeventhub --name RootManageSharedAccessKey
```

Etkinlik Hub'ları için Azure CLI komutları hakkında daha fazla bilgi için [Etkinlik Hub'ları için Azure CLI'ye](/cli/azure/eventhubs)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay Hub'larına genel bakış](event-hubs-what-is-event-hubs.md)
* [Etkinlik Merkezi Oluşturma](event-hubs-create.md)
