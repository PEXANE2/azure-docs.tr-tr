---
title: Azure Event Hubs API 'sine genel bakış | Microsoft Docs
description: Bu makalede, Azure Event Hubs hizmetini kullanmaya yönelik kullanılabilir API 'Lere (çalışma zamanı ve yönetim) genel bir bakış sunulmaktadır.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 3f221a0c-182d-4e39-9f3d-3a3c16c5c6ed
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2018
ms.author: shvija
ms.openlocfilehash: 08e10996f633d35ffbf946b61937a0e9a76f7227
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77162234"
---
# <a name="available-event-hubs-apis"></a>Kullanılabilir Event Hubs API 'Leri

Bu makalede, Event Hubs kaynaklarını yönetmek için kullanabileceğiniz kullanılabilir API istemcileri kümesi açıklanır.

## <a name="runtime-apis"></a>Çalışma zamanı API 'Leri

Aşağıdaki bölümde, mevcut olan tüm Azure Event Hubs çalışma zamanı istemcileri açıklanmaktadır. Bu kitaplıkların bazıları da sınırlı yönetim işlevleri de içerirken, yönetim işlemlerine adanmış [belirli kitaplıklar](#management-apis) da vardır. Bu kitaplıkların temel odağı, bir olay hub 'ından ileti göndermek ve almak için kullanılır.

Her çalışma zamanı kitaplığının geçerli durumu hakkında daha fazla bilgi için bkz. [ek bilgi](#additional-information).

| Dil/platform | İstemci paketi | EventProcessorHost paketi | Depo |
| --- | --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET Framework | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | Yok |
| Java | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Node | [NPM](https://www.npmjs.com/package/azure-event-hubs) | Yok | [GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs) |
| C | Yok | Yok | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET

.NET ekosisteminde birden fazla çalışma zamanı bulunur, bu nedenle Event Hubs için birden çok .NET kitaplığı vardır. .NET Standard kitaplığı, .NET Core veya .NET Framework kullanılarak çalıştırılabilir, ancak .NET Framework kitaplığı yalnızca bir .NET Framework ortamında çalıştırılabilir. .NET Framework sürümleri hakkında daha fazla bilgi için bkz. [Framework sürümleri](https://docs.microsoft.com/dotnet/articles/standard/frameworks).

#### <a name="node"></a>Node

[JavaScript kitaplığı](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs) Şu anda önizleme aşamasındadır ve Microsoft çalışanları ve dış katkıda bulunanlar tarafından yan bir proje olarak korunur. Kaynak kodu dahil tüm katkılar hoş geldiniz ve incelenmeyecektir.

## <a name="management-apis"></a>Yönetim API’leri

Aşağıdaki tabloda, mevcut olan yönetime özgü tüm kitaplıklar listelenmiştir. Bu kitaplıkların hiçbiri çalışma zamanı işlemleri içermez ve Event Hubs varlıklarını yönetmek tek amaç içindir.

| Dil/platform | Yönetim Paketi | Depo |
| --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
