---
title: Azure Röle Karma Bağlantıları - .NET'te WebSockets
description: Azure Relay Hybrid Connections WebSockets için c# konsolu uygulaması yazın.
services: service-bus-relay
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: d1386900-b942-4abf-acfc-38d2ef826253
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/01/2018
ms.author: spelluru
ms.openlocfilehash: 13612f8ffa343e483165a8dbdd54d1b2b1f5e2cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75355188"
---
# <a name="get-started-with-relay-hybrid-connections-websockets-in-net"></a>.NET'teki Röle Hibrit Bağlantılar WebSockets ile başlayın
[!INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

Bu hızlı başlangıçta, Azure Röle'de Karma Bağlantılar WebSockets'i kullanarak ileti gönderen ve alan .NET gönderen ve alıcı uygulamaları oluşturursunuz. Azure Rölesi hakkında genel bilgi edinmek için Azure [Röle'ye](relay-what-is-it.md)bakın. 

Bu hızlı başlangıçta, aşağıdaki adımları atarsınız:

1. Azure portalını kullanarak Geçiş ad alanı oluşturma.
2. Azure portalını kullanarak o ad alanında karma bağlantı oluşturma.
3. İleti almak için bir sunucu (dinleyici) konsol uygulaması yazma.
4. İleti göndermek için bir istemci (gönderen) konsol uygulaması yazma.
5. Uygulamaları çalıştırın. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* [Visual Studio 2015 veya üzeri](https://www.visualstudio.com). Bu öğreticideki örneklerde Visual Studio 2017 kullanılmaktadır.
* Azure aboneliği. Hesabınız yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="create-a-namespace"></a>Ad alanı oluşturma
[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="create-a-hybrid-connection"></a>Karma bağlantı oluşturma
[!INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="create-a-server-application-listener"></a>Sunucu uygulaması (dinleyici) oluşturma
Geçiş hizmetinden ileti dinleyip almak için Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-dotnet-get-started-server.md)]

## <a name="create-a-client-application-sender"></a>İstemci uygulaması (gönderici) oluşturma
Geçiş hizmetine ileti göndermek Visual Studio kullanarak bir C# konsol uygulaması yazın.

[!INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-dotnet-get-started-client.md)]

## <a name="run-the-applications"></a>Uygulamaları çalıştırma
1. Sunucu uygulamasını çalıştırın.
2. İstemci uygulamasını çalıştırın ve metin girin.
3. Sunucu uygulama konsolunun istemci uygulamasına girilen metni görüntülediğinden emin olun.

    ![running-applications](./media/relay-hybrid-connections-dotnet-get-started/running-applications.png)

Tebrikler, tam bir Karma Bağlantılar uygulaması oluşturduk!

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, ileti göndermek ve almak için WebSockets'i kullanan .NET istemcisi ve sunucu uygulamaları oluşturdunuz. Azure Röle'nin Karma Bağlantılar özelliği, ileti göndermek ve almak için HTTP'yi kullanmayı da destekler. Azure Relay Karma Bağlantıları ile HTTP'yi nasıl kullanacağınızı öğrenmek için [HTTP quickstart'a](relay-hybrid-connections-http-requests-dotnet-get-started.md)bakın.

Bu hızlı başlatmada, istemci ve sunucu uygulamaları oluşturmak için .NET Framework'u kullandınız. Node.js kullanarak istemci ve sunucu uygulamaları yazmayı öğrenmek için, [Node.js WebSockets quickstart](relay-hybrid-connections-node-get-started.md) veya [Node.js HTTP quickstart](relay-hybrid-connections-http-requests-dotnet-get-started.md)bakın.

