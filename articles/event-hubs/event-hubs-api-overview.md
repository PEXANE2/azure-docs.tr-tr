---
title: Azure Etkinlik Hub'ları API'ye genel bakış | Microsoft Dokümanlar
description: Bu makalede, Azure Etkinlik Hub'ları hizmetini kullanmak için kullanılabilir API'lere (çalışma zamanı ve yönetim) genel bir bakış sunulmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162234"
---
# <a name="available-event-hubs-apis"></a>Kullanılabilir Olay Hub'ları API'leri

Bu makalede, Olay Hub'ları kaynaklarını yönetmek için kullanabileceğiniz kullanılabilir API istemcileri kümesi açıklanmaktadır.

## <a name="runtime-apis"></a>Çalışma Zamanı API'leri

Aşağıdaki bölümde, şu anda kullanılabilen tüm Azure Etkinlik Hub'ları çalışma zamanı istemcilerini açıklamaktadır. Bu kitaplıkların bazıları sınırlı yönetim işlevselliği de içermekle birlikte, yönetim işlemlerine adanmış [özel kitaplıklar](#management-apis) da vardır. Bu kitaplıkların temel odak noktası, bir etkinlik hub'ından ileti gönderip almaktır.

Her çalışma zamanı kitaplığıngeçerli durumu hakkında daha fazla bilgi için [ek bilgilere](#additional-information)bakın.

| Dil / Platform | İstemci paketi | EventProcessorHost paketi | Depo |
| --- | --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) | [GitHub](https://github.com/azure/azure-event-hubs-dotnet) |
| .NET Framework | [NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/) | Yok |
| Java | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) | [Maven](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22) | [GitHub](https://github.com/Azure/azure-event-hubs-java) |
| Node | [NPM](https://www.npmjs.com/package/azure-event-hubs) | Yok | [GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs) |
| C | Yok | Yok | [GitHub](https://github.com/Azure/azure-event-hubs-c) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET

.NET ekosisteminin birden çok çalışma alanı vardır, bu nedenle Olay Hub'ları için birden çok .NET kitaplık ları vardır. .NET Standart kitaplığı .NET Core veya .NET Framework kullanılarak çalıştırılabilirken, .NET Framework kitaplığı yalnızca .NET Framework ortamında çalıştırılabilir. .NET Framework sürümleri hakkında daha fazla bilgi için [çerçeve sürümlerine](https://docs.microsoft.com/dotnet/articles/standard/frameworks)bakın.

#### <a name="node"></a>Node

[JavaScript kitaplığı](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs) şu anda önizlemededir ve Microsoft çalışanları ve dış katkıda bulunanlar tarafından bir yan proje olarak korunur. Kaynak kodu da dahil olmak üzere tüm katkılar memnuniyetle karşılanır ve gözden geçirilecektir.

## <a name="management-apis"></a>Yönetim API’leri

Aşağıdaki tabloda şu anda yönetime özel tüm kitaplıklar listelenmektedir. Bu kitaplıkların hiçbiri çalışma zamanı işlemleri içermez ve yalnızca Olay Hub'ları varlıklarını yönetmek amacıyladır.

| Dil / Platform | Yönetim paketi | Depo |
| --- | --- | --- |
| .NET Standard | [NuGet](https://www.nuget.org/packages/Microsoft.Azure.Management.EventHub) | [GitHub](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ResourceManagement/EventHub) |

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay Hub'larına genel bakış](event-hubs-what-is-event-hubs.md)
* [Etkinlik merkezi oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
