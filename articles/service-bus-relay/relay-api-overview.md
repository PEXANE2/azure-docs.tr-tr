---
title: Azure Röle API'si genel bakış | Microsoft Dokümanlar
description: Bu makale, kullanılabilir Azure Röle API'lerine (.NET Standardı, .NET Framework, Node.js, vb.) genel bir bakış sağlar
services: event-hubs
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: fdaa1d2b-bd80-4e75-abb9-0c3d0773af2d
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2020
ms.author: spelluru
ms.openlocfilehash: 41d9e2026c19c959dc6fe2546b0ef699571ec7cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513975"
---
# <a name="available-relay-apis"></a>Kullanılabilir Röle API'leri

## <a name="runtime-apis"></a>Çalışma Zamanı API'leri

Aşağıdaki tabloda, şu anda kullanılabilir olan tüm Röle çalışma zamanı istemcileri listelenmektedir.

[Ek bilgi](#additional-information) bölümü, her çalışma zamanı kitaplığın durumu hakkında daha fazla bilgi içerir.

| Dil / Platform | Kullanılabilir özellik | İstemci paketi | Depo |
| --- | --- | --- | --- |
| .NET Standard | Karma Bağlantılar | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | WCF Geçişi | [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Yok |
| Node | Karma Bağlantılar | [Websockets:`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[Websockets:`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[HTTP İstekleri:`hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET

.NET ekosisteminin birden çok çalışma süresi vardır, bu nedenle Röle için birden çok .NET kitaplık vardır. .NET Standart kitaplığı .NET Core veya .NET Framework kullanılarak çalıştırılabilirken, .NET Framework kitaplığı yalnızca .NET Framework ortamında çalıştırılabilir. .NET Frameworks hakkında daha fazla bilgi için [çerçeve sürümlerine](/dotnet/articles/standard/frameworks)bakın.

.NET Framework kitaplığı yalnızca WCF programlama modelini destekler ve WCF `net.tcp` taşımasını temel alan özel bir ikili protokole dayanır. Bu protokol ve kitaplık, varolan uygulamalarla geriye dönük uyumluluk için korunur.

.NET Standart kitaplığı, HTTP ve WebSockets'te bulunan Karma Bağlantılar Rölesi için açık protokol tanımına dayanır. Kitaplık, Websockets üzerinde bir akış soyutlama ve HTTP isteklerini yanıtlamak için basit bir istek yanıt API hareketi destekler. [Web API](https://github.com/Azure/azure-relay-dotnet) örneği, Karma Bağlantıların web hizmetleri için ASP.NET Core ile nasıl entegre edilebildiğini gösterir.

#### <a name="nodejs"></a>Node.js

Yukarıdaki tabloda listelenen Karma Bağlantılar modülleri, yerel ağ yığını yerine Azure Röle hizmetini dinleyen alternatif uygulamalarla mevcut Düğüm.js modüllerini değiştirir veya değiştirir.

Modül, `hyco-https` çekirdek Düğüm.js modüllerini `http` değiştirir ve kısmen `https`geçersiz kılar ve bu çekirdek modüllere dayanan birçok mevcut Node.js modülü ve uygulamasıyla uyumlu bir HTTPS dinleyici uygulaması sağlar.

`hyco-ws` Ve `hyco-websocket` modüller, Node.js için popüler `ws` ve `websocket` modülleri değiştirerek, modüllerin ve uygulamaların her iki modüle de bağlı olarak Hibrit Bağlantı Rölesi'nin arkasında çalışmasını sağlayan alternatif dinleyici uygulamaları sağlar.

Bu modüllerle ilgili ayrıntılar [azure-röle düğümü](https://github.com/Azure/azure-relay-node) GitHub deposunda bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Röle hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Geçiş hakkında SSS](relay-faq.md)
