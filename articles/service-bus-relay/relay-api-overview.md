---
title: Azure Relay API 'ye Genel Bakış | Microsoft Docs
description: Bu makalede, kullanılabilir Azure Relay API 'Lerine (.NET Standard, .NET Framework, Node. js vb.) ilişkin bir genel bakış sunulmaktadır.
services: service-bus-relay
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
ms.openlocfilehash: ad7226b5c5badfddf9f436a1229a48f729485821
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82983134"
---
# <a name="available-relay-apis"></a>Kullanılabilir geçiş API 'Leri

## <a name="runtime-apis"></a>Çalışma zamanı API 'Leri

Aşağıdaki tabloda, şu anda kullanılabilir olan tüm geçiş çalışma zamanı istemcileri listelenmiştir.

[Ek bilgi](#additional-information) bölümü, her çalışma zamanı kitaplığının durumu hakkında daha fazla bilgi içerir.

| Dil/platform | Kullanılabilir özellik | İstemci paketi | Depo |
| --- | --- | --- | --- |
| .NET Standard | Karma Bağlantılar | [Microsoft.Azure.Relay](https://www.nuget.org/packages/Microsoft.Azure.Relay/) | [GitHub](https://github.com/azure/azure-relay-dotnet) |
| .NET Framework | WCF Geçişi | [WindowsAzure. ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) | Yok |
| Node | Karma Bağlantılar | [WebSockets`hyco-ws`](https://www.npmjs.com/package/hyco-ws)<br/>[WebSockets`hyco-websocket`](https://www.npmjs.com/package/hyco-websocket)<br/>[HTTP Istekleri:`hyco-https`](https://www.npmjs.com/package/hyco-https) | [GitHub](https://github.com/Azure/azure-relay-node) |

### <a name="additional-information"></a>Ek bilgiler

#### <a name="net"></a>.NET

.NET ekosisteminde birden fazla çalışma zamanı var, bu nedenle geçiş için birden çok .NET kitaplığı var. .NET Standard kitaplığı, .NET Core veya .NET Framework kullanılarak çalıştırılabilir, ancak .NET Framework kitaplığı yalnızca bir .NET Framework ortamında çalıştırılabilir. .NET çerçeveleri hakkında daha fazla bilgi için bkz. [Framework sürümleri](/dotnet/articles/standard/frameworks).

.NET Framework kitaplığı yalnızca WCF programlama modelini destekler ve WCF `net.tcp` taşımasını temel alan özel bir ikili protokolü kullanır. Bu protokol ve kitaplık, mevcut uygulamalarla geriye dönük uyumluluk için korunur.

.NET Standard kitaplığı, HTTP ve WebSockets üzerinde derlemelerin Karma Bağlantılar geçişinin açık protokol tanımına dayalıdır. Kitaplık, HTTP isteklerini yanıtlamak için WebSockets üzerinde bir akış soyutlama ve basit bir istek-yanıt API hareketi destekler. [Web API](https://github.com/Azure/azure-relay-dotnet) örneği, karma bağlantılar Web hizmetleri için ASP.NET Core nasıl tümleştirileceğini gösterir.

#### <a name="nodejs"></a>Node.js

Yukarıdaki tabloda listelenen Karma Bağlantılar modülleri, var olan Node. js modüllerini yerel ağ yığını yerine Azure Relay hizmetini dinleyen alternatif uygulamalarla değiştirir veya değiştirir.

`hyco-https` Modül, çekirdek Node. js modüllerini `http` `https`düzeltme ve kısmen geçersiz kılar ve bu temel modülleri kullanan birçok mevcut Node. js MODÜLLERIYLE ve uygulamalarla uyumlu bir HTTPS dinleyicisi uygulaması sağlar.

`hyco-ws` Ve `hyco-websocket` modülleri Node. js için popüler `ws` ve `websocket` modülleri düzeltmiştir, bu iki modüle da bağlı olan modül ve uygulamaların karma bağlantılar geçişinin arkasında çalışmasına olanak tanıyan alternatif dinleyici uygulamaları sağlar.

Bu modüllerle ilgili ayrıntılar [Azure-Relay-node](https://github.com/Azure/azure-relay-node) GitHub deposunda bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Relay hakkında daha fazla bilgi edinmek için şu bağlantıları ziyaret edin:
* [Azure Geçiş nedir?](relay-what-is-it.md)
* [Geçiş hakkında SSS](relay-faq.md)
