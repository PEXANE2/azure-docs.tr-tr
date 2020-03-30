---
title: "Quickstart: PowerShell - Azure Etkinlik Hub'larını kullanarak etkinlik merkezi oluşturma"
description: Bu hızlı başlangıçta Azure PowerShell'i kullanarak olay hub'ı oluşturma ve ardından .NET Standard SDK'sını kullanarak olayları gönderip alma adımları anlatılmaktadır.
services: event-hubs
author: spelluru
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: quickstart
ms.custom: seodec18
ms.date: 11/05/2019
ms.author: spelluru
ms.openlocfilehash: 89ec1957e75fa45eef6c7939a77e5cc1b3cf7806
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77162149"
---
# <a name="quickstart-create-an-event-hub-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell'i kullanarak olay hub'ı oluşturma

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu hızlı başlangıçta Azure PowerShell'i kullanarak olay hub'ı oluşturacaksınız.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

- Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.][]
- [Visual Studio 2019](https://www.visualstudio.com/vs).
- [.NET Standard SDK'sı](https://www.microsoft.com/net/download/windows), sürüm 2.0 veya üzeri.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel ortamda kullanıyorsanız bu hızlı başlangıcı tamamlamak için PowerShell'in en son sürümüne sahip olmanız gerekir. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure PowerShell’i Yükleme ve Yapılandırma](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu, Azure kaynaklarından oluşan mantıksal bir koleksiyondur ve olay hub'ı oluşturmak için bir kaynak grubuna ihtiyacınız vardır. 

Aşağıdaki örnekte Doğu ABD bölgesinde bir kaynak grubu oluşturulmaktadır. `myResourceGroup` değerini kullanmak istediğiniz kaynak grubu adıyla değiştirin:

```azurepowershell-interactive
New-AzResourceGroup –Name myResourceGroup –Location eastus
```

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Kaynak grubunuzu oluşturduktan sonra bu kaynak grubu içinde bir Event Hubs ad alanı oluşturun. Event Hubs ad alanı, içinde olay hub'ınızı oluşturabileceğiniz benzersiz bir tam etki alanı adı sağlar. `namespace_name` değerini ad alanınız için benzersiz bir adla değiştirin:

```azurepowershell-interactive
New-AzEventHubNamespace -ResourceGroupName myResourceGroup -NamespaceName namespace_name -Location eastus
```

## <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Bir Event Hubs ad alanı oluşturduğunuza göre şimdi bu ad alanının içinde bir olay hub'ı oluşturabilirsiniz:  
İzin verilen `MessageRetentionInDays` süre 1 ile 7 gün arasındadır.

```azurepowershell-interactive
New-AzEventHub -ResourceGroupName myResourceGroup -NamespaceName namespace_name -EventHubName eventhub_name -MessageRetentionInDays 3
```

Tebrikler! Azure PowerShell’i kullanarak bir Event Hubs ad alanı ve bu ad alanının içinde bir olay hub'ı oluşturdunuz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Event Hubs ad alanını oluşturdunuz ve olay hub'ınızdan olay gönderip almak için örnek uygulamaları kullandınız. Bir etkinlik merkezinden etkinlik almak için (veya) etkinlik göndermek için adım adım talimatlar **için, Gönder'e** bakın ve etkinlik eğitimlerini alın: 

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](get-started-java-send-v2.md)
- [Python](get-started-python-send-v2.md)
- [Javascript](get-started-java-send-v2.md)
- [Git](event-hubs-go-get-started-send.md)
- [C (yalnızca gönderme)](event-hubs-c-getstarted-send.md)
- [Apache Storm (yalnızca alma)](event-hubs-storm-getstarted-receive.md)


[ücretsiz bir hesap oluşturmak]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install and Configure Azure PowerShell]: https://docs.microsoft.com/powershell/azure/install-az-ps
[New-AzResourceGroup]: https://docs.microsoft.com/powershell/module/az.resources/new-Azresourcegroup
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png
