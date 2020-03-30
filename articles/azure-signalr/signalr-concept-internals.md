---
title: Azure SignalR Service iç işlevleri
description: Azure SignalR Hizmeti dahili bilgileri, mimarisi, bağlantıları ve verilerin nasıl aktarıldığı hakkında bilgi edinin.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 11/13/2019
ms.author: zhshang
ms.openlocfilehash: 62afa5ee6993aa1bb3c7b5926e5320ab1fa510a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74157605"
---
# <a name="azure-signalr-service-internals"></a>Azure SignalR Service iç işlevleri

Azure SignalR Hizmeti, ASP.NET Core SignalR çerçevesinin üzerine inşa edilmiştir. Ayrıca önizleme özelliği olarak SignalR ASP.NET destekler.

> Azure SignalR Hizmeti, ASP.NET SignalR'ı desteklemek için ASP.NET Core çerçevesinin üzerine ASP.NET SignalR veri protokolünü yeniden uygular

Birkaç satır kod değişikliğiyle SignalR Hizmeti ile çalışmak için yerel bir ASP.NET Core SignalR uygulamasını kolayca geçirebilirsiniz.

Aşağıdaki diyagramda, uygulama sunucunuzla Birlikte Sinyal Hizmeti kullandığınızda tipik mimari açıklanmaktadır.

Kendi kendine barındırılan ASP.NET Core SignalR uygulamasından farklar da tartışılmıştır.

![Mimari](./media/signalr-concept-internals/arch.png)

## <a name="server-connections"></a>Sunucu bağlantıları

Kendi kendine barındırılan ASP.NET Core SignalR uygulama sunucusu istemcileri doğrudan dinler ve bağlar.

SignalR Hizmeti ile uygulama sunucusu artık kalıcı istemci bağlantılarını kabul etmiyor, bunun yerine:

1. Bir `negotiate` bitiş noktası, her hub için Azure SignalR Service SDK tarafından ortaya çıkar.
1. Bu bitiş noktası, istemcinin anlaşma isteklerini yanıtlar ve istemcileri SignalR Hizmetine yönlendirir.
1. Sonunda, istemciler SignalR Hizmeti'ne bağlanır.

Daha fazla bilgi için [Bkz. İstemci bağlantıları.](#client-connections)

Uygulama sunucusu başlatıldıktan sonra, 
- Core SignalR ASP.NET için Azure SignalR Service SDK, Hub başına 5 WebSocket bağlantısını SignalR Hizmeti'ne açar. 
- ASP.NET SignalR için Azure SignalR Service SDK, Hub başına 5 WebSocket bağlantısını SignalR Hizmeti'ne ve uygulama başına bir WebSocket bağlantısı açar.

5 WebSocket bağlantıları [yapılandırmada](https://github.com/Azure/azure-signalr/blob/dev/docs/use-signalr-service.md#connectioncount)değiştirilebilen varsayılan değerdir.

İstemcilere gelen ve gelen iletiler bu bağlantılara katlanır.

Bu bağlantılar her zaman SignalR Hizmetine bağlı kalır. Ağ sorunu için bir sunucu bağlantısı kesilirse,
- bu sunucu bağlantısı nın bağlantısını kesmesi ile sunulan tüm istemciler (bu konuda daha fazla bilgi [için, istemci ve sunucu arasında Veri aktarımı](#data-transmit-between-client-and-server)bakın);
- sunucu bağlantısı otomatik olarak yeniden bağlanmaya başlar.

## <a name="client-connections"></a>İstemci bağlantıları

SignalR Hizmetini kullandığınızda, istemciler uygulama sunucusu yerine SignalR Hizmeti'ne bağlanır.
İstemci ve Sinyal Hizmeti arasında kalıcı bağlantılar kurmak için iki adım vardır.

1. İstemci uygulama sunucusuna bir anlaşma isteği gönderir. Azure SignalR Service SDK ile uygulama sunucusu SignalR Hizmeti'nin URL'si ve erişim belirteciyle yeniden yönlendirme yanıtı verir.

- Core SignalRASP.NET için tipik bir yönlendirme yanıtı şu şekilde görünür:
    ```
    {
        "url":"https://test.service.signalr.net/client/?hub=chat&...",
        "accessToken":"<a typical JWT token>"
    }
    ```
- SignalR ASP.NET için tipik bir yönlendirme yanıtı şu şekilde görünür:
    ```
    {
        "ProtocolVersion":"2.0",
        "RedirectUrl":"https://test.service.signalr.net/aspnetclient",
        "AccessToken":"<a typical JWT token>"
    }
    ```

1. Yönlendirme yanıtını aldıktan sonra istemci, SignalR Hizmetine bağlanmak için normal işlemi başlatmak için yeni URL'yi ve erişim belirtecisini kullanır.

Core SignalR'ın taşıma [protokolleri](https://github.com/aspnet/SignalR/blob/release/2.2/specs/TransportProtocols.md)ASP.NET hakkında daha fazla bilgi edinin.

## <a name="data-transmit-between-client-and-server"></a>İstemci ve sunucu arasında veri iletimi

Bir istemci SignalR Hizmetine bağlandığında, hizmet çalışma süresi bu istemciye hizmet verecek bir sunucu bağlantısı bulur
- Bu adım yalnızca bir kez olur ve istemci ve sunucu bağlantıları arasında bire bir eşlemedir.
- Eşleme, istemci veya sunucu bağlantısı kesilene kadar SignalR Hizmeti'nde tutulur.

Bu noktada, uygulama sunucusu yeni istemciden bilgi içeren bir olay alır. Uygulama sunucusunda istemciye mantıksal bir bağlantı oluşturulur. Veri kanalı SignalR Service aracılığıyla istemciden uygulama sunucusuna kurulur.

SignalR hizmeti istemciden eşleştirme uygulama sunucusuna veri aktarır. Ve uygulama sunucusundan gelen veriler eşlenen istemcilere gönderilir.

Gördüğünüz gibi, Azure SinyalR Hizmeti aslında uygulama sunucusu ve istemciler arasında mantıksal bir aktarım katmanıdır. Tüm kalıcı bağlantılar SignalR Hizmeti'ne yüklenir.
Uygulama sunucusunun yalnızca istemci bağlantıları konusunda endişelenmeden hub sınıfındaki iş mantığını işlemesi gerekir.